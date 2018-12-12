---
uid: signalr/overview/performance/signalr-performance
title: Wydajność SignalR | Dokumentacja firmy Microsoft
author: pfletcher
description: Wydajność usługi SignalR
ms.author: riande
ms.date: 06/10/2014
ms.assetid: 3751f5e7-59db-4be0-a290-50abc24e5c84
msc.legacyurl: /signalr/overview/performance/signalr-performance
msc.type: authoredcontent
ms.openlocfilehash: 9346f0ff9720361f07afe196f59305f0f38ffe8a
ms.sourcegitcommit: 74e3be25ea37b5fc8b4b433b0b872547b4b99186
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53287789"
---
<a name="signalr-performance"></a><span data-ttu-id="5371f-103">Wydajność usługi SignalR</span><span class="sxs-lookup"><span data-stu-id="5371f-103">SignalR Performance</span></span>
====================
<span data-ttu-id="5371f-104">przez [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="5371f-104">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="5371f-105">W tym temacie opisano sposób projektowania, miar i zwiększenie wydajności w aplikacji SignalR.</span><span class="sxs-lookup"><span data-stu-id="5371f-105">This topic describes how to design for, measure, and improve performance in a SignalR application.</span></span>
>
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="5371f-106">Wersje oprogramowania używaną w tym temacie</span><span class="sxs-lookup"><span data-stu-id="5371f-106">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="5371f-107">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="5371f-107">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="5371f-108">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="5371f-108">.NET 4.5</span></span>
> - <span data-ttu-id="5371f-109">SignalR w wersji 2</span><span class="sxs-lookup"><span data-stu-id="5371f-109">SignalR version 2</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="5371f-110">Poprzednie wersje tego tematu</span><span class="sxs-lookup"><span data-stu-id="5371f-110">Previous versions of this topic</span></span>
>
> <span data-ttu-id="5371f-111">Aby uzyskać informacje dotyczące starszych wersji biblioteki SignalR, zobacz [starsze wersje biblioteki SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="5371f-111">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="5371f-112">Pytania i komentarze</span><span class="sxs-lookup"><span data-stu-id="5371f-112">Questions and comments</span></span>
>
> <span data-ttu-id="5371f-113">Jak się podoba w tym samouczku, i co można było ulepszyć proces w komentarzach u dołu strony, wystaw opinię.</span><span class="sxs-lookup"><span data-stu-id="5371f-113">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="5371f-114">Jeśli masz pytania, na które nie są bezpośrednio związane z tego samouczka, możesz zamieścić je do [forum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) lub [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="5371f-114">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<span data-ttu-id="5371f-115">Ostatnie prezentacji na wydajność SignalR i skalowania, zobacz [skalowanie sieci Web w czasie rzeczywistym z użyciem ASP.NET SignalR](https://channel9.msdn.com/Events/Build/2013/3-502).</span><span class="sxs-lookup"><span data-stu-id="5371f-115">For a recent presentation on SignalR performance and scaling, see [Scaling the Real-time Web with ASP.NET SignalR](https://channel9.msdn.com/Events/Build/2013/3-502).</span></span>

<span data-ttu-id="5371f-116">Ten temat zawiera następujące sekcje:</span><span class="sxs-lookup"><span data-stu-id="5371f-116">This topic contains the following sections:</span></span>

- [<span data-ttu-id="5371f-117">Zagadnienia dotyczące projektowania</span><span class="sxs-lookup"><span data-stu-id="5371f-117">Design considerations</span></span>](#design)
- [<span data-ttu-id="5371f-118">Dostrajanie wydajności serwera SignalR</span><span class="sxs-lookup"><span data-stu-id="5371f-118">Tuning your SignalR server for performance</span></span>](#tuning)
- [<span data-ttu-id="5371f-119">Rozwiązywanie problemów z wydajnością</span><span class="sxs-lookup"><span data-stu-id="5371f-119">Troubleshooting performance issues</span></span>](#troubleshooting)
- [<span data-ttu-id="5371f-120">Korzystanie z liczników wydajności SignalR</span><span class="sxs-lookup"><span data-stu-id="5371f-120">Using SignalR performance counters</span></span>](#perfcounters)
- [<span data-ttu-id="5371f-121">Korzystanie z innych liczników wydajności</span><span class="sxs-lookup"><span data-stu-id="5371f-121">Using other performance counters</span></span>](#othercounters)
- [<span data-ttu-id="5371f-122">Inne zasoby</span><span class="sxs-lookup"><span data-stu-id="5371f-122">Other resources</span></span>](#otherresources)

<a id="design"></a>

## <a name="design-considerations"></a><span data-ttu-id="5371f-123">Zagadnienia dotyczące projektowania</span><span class="sxs-lookup"><span data-stu-id="5371f-123">Design considerations</span></span>

<span data-ttu-id="5371f-124">W tej sekcji opisano wzorców, które można zastosować podczas projektowania aplikacji SignalR, aby upewnić się, że wydajność jest nie utrudniona, generując niepotrzebny ruch sieciowy.</span><span class="sxs-lookup"><span data-stu-id="5371f-124">This section describes patterns that can be implemented during the design of a SignalR application, to ensure that performance is not being hindered by generating unnecessary network traffic.</span></span>

### <a name="throttling-message-frequency"></a><span data-ttu-id="5371f-125">Ograniczanie częstotliwości wiadomości</span><span class="sxs-lookup"><span data-stu-id="5371f-125">Throttling message frequency</span></span>

<span data-ttu-id="5371f-126">Nawet w przypadku aplikacji, która wysyła komunikaty o wysokiej częstotliwości (np. aplikacji gier w języku czasu rzeczywistego) większość aplikacji nie trzeba wysłać więcej niż kilka komunikatów na sekundę.</span><span class="sxs-lookup"><span data-stu-id="5371f-126">Even in an application that sends out messages at a high frequency (such as a realtime gaming application), most applications don't need to send more than a few messages a second.</span></span> <span data-ttu-id="5371f-127">Aby zmniejszyć ilość ruchu sieciowego, który generuje każdy klient, można zaimplementować pętli komunikatów, kolejki i wysyła się komunikaty nie częściej niż stałą stawkę (czyli maksymalna liczba wiadomości będą wysyłane co sekundę w przypadku komunikatów w tym czasie w nterwał do wysłania).</span><span class="sxs-lookup"><span data-stu-id="5371f-127">To reduce the amount of traffic that each client generates, a message loop can be implemented that queues and sends out messages no more frequently than a fixed rate (that is, up to a certain number of messages will be sent every second, if there are messages in that time interval to be sent).</span></span> <span data-ttu-id="5371f-128">Dla przykładowej aplikacji, która ogranicza wiadomości do niektórych współczynnik (od klienta i serwera), zobacz [Realtime o wysokiej częstotliwości za pomocą biblioteki SignalR](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="5371f-128">For a sample application that throttles messages to a certain rate (from both client and server), see [High-Frequency Realtime with SignalR](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).</span></span>

### <a name="reducing-message-size"></a><span data-ttu-id="5371f-129">Zmniejszenie rozmiaru wiadomości</span><span class="sxs-lookup"><span data-stu-id="5371f-129">Reducing message size</span></span>

<span data-ttu-id="5371f-130">Aby zmniejszyć rozmiar komunikatu SignalR, należy zmniejszyć rozmiar serializacji obiektów.</span><span class="sxs-lookup"><span data-stu-id="5371f-130">You can reduce the size of a SignalR message by reducing the size of your serialized objects.</span></span> <span data-ttu-id="5371f-131">W kodzie serwera, jest wysyłana obiekt, który zawiera właściwości, które nie muszą być przekazywane, uniemożliwi te właściwości serializowanego przy użyciu `JsonIgnore` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="5371f-131">In server code, if you're sending an object that contains properties that don't need to be transmitted, prevent those properties from being serialized by using the `JsonIgnore` attribute.</span></span> <span data-ttu-id="5371f-132">Nazwy właściwości są także przechowywane informacje w wiadomości; nazwy właściwości może zostać skrócony przy użyciu `JsonProperty` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="5371f-132">The names of properties are also stored in the message; the names of properties can be shortened using the `JsonProperty` attribute.</span></span> <span data-ttu-id="5371f-133">W poniższym przykładzie kodu pokazano sposób wykluczać właściwość zostały wysłane do klienta oraz skrócić nazwy właściwości:</span><span class="sxs-lookup"><span data-stu-id="5371f-133">The following code sample demonstrates how to exclude a property from being sent to the client, and how to shorten property names:</span></span>

<span data-ttu-id="5371f-134">**Kod serwera platformy .NET, który demonstruje, atrybut JsonIgnore wykluczanie danych zostały wysłane do klienta, a atrybut JsonProperty, aby zmniejszyć rozmiar komunikatu**</span><span class="sxs-lookup"><span data-stu-id="5371f-134">**.NET server code that demonstrates the JsonIgnore attribute to exclude data from being sent to the client, and the JsonProperty attribute to reduce message size**</span></span>

[!code-csharp[Main](signalr-performance/samples/sample1.cs?highlight=5,7,10)]

<span data-ttu-id="5371f-135">Aby zachować czytelność / łatwości utrzymania kodu klienta, nazwy skróconej właściwości mogą być ponownie zmapowany do przyjaznego dla człowieka nazwami po otrzymaniu komunikatu.</span><span class="sxs-lookup"><span data-stu-id="5371f-135">In order to retain readability/ maintainability in the client code, the abbreviated property names can be remapped to human-friendly names after the message is received.</span></span> <span data-ttu-id="5371f-136">W poniższym przykładzie kodu pokazano jeden sposób możliwe ponowne mapowanie skrócone nazwy tymi dłużej, definiując kontraktu komunikatu (mapowanie) i za pomocą `reMap` funkcja do stosowania Umowy do klasy zoptymalizowane wiadomości:</span><span class="sxs-lookup"><span data-stu-id="5371f-136">The following code sample demonstrates one possible way of remapping shortened names to longer ones, by defining a message contract (mapping), and using the `reMap` function to apply the contract to the optimized message class:</span></span>

<span data-ttu-id="5371f-137">**Kod JavaScript po stronie klienta, który ponownie mapuje skrócone nazwy właściwości na nazwy czytelny dla człowieka**</span><span class="sxs-lookup"><span data-stu-id="5371f-137">**Client-side JavaScript code that remaps shortened property names to human-readable names**</span></span>

[!code-javascript[Main](signalr-performance/samples/sample2.js)]

<span data-ttu-id="5371f-138">Nazwy mogą skrócony w wiadomościach od klienta do serwera, jak również za pomocą tej samej metody.</span><span class="sxs-lookup"><span data-stu-id="5371f-138">Names can be shortened in messages from the client to the server as well, using the same method.</span></span>

<span data-ttu-id="5371f-139">Zmniejszenie zużycia pamięci (czyli ilości pamięci używanej dla wiadomości) komunikatu obiektu może również zwiększyć wydajność.</span><span class="sxs-lookup"><span data-stu-id="5371f-139">Reducing the memory footprint (that is, the amount of memory used for the message) of the message object can also improve performance.</span></span> <span data-ttu-id="5371f-140">Przykładowo jeśli pełną gamę `int` nie jest potrzebna, `short` lub `byte` mogą być używane zamiast tego.</span><span class="sxs-lookup"><span data-stu-id="5371f-140">For example, if the full range of an `int` is not needed, a `short` or `byte` can be used instead.</span></span>

<span data-ttu-id="5371f-141">Ponieważ komunikaty są przechowywane w magistrali komunikatów w pamięci serwera, zmniejszenie rozmiaru wiadomości, można także rozwiązać problemy z z pamięci serwera.</span><span class="sxs-lookup"><span data-stu-id="5371f-141">Since messages are stored in the message bus in server memory, reducing the size of messages can also address server memory issues.</span></span>

<a id="tuning"></a>

### <a name="tuning-your-signalr-server-for-performance"></a><span data-ttu-id="5371f-142">Dostrajanie wydajności serwera SignalR</span><span class="sxs-lookup"><span data-stu-id="5371f-142">Tuning your SignalR server for performance</span></span>

<span data-ttu-id="5371f-143">Następujące ustawienia konfiguracji można Dostrajanie Serwer w celu zapewnienia lepszej wydajności w aplikacji SignalR.</span><span class="sxs-lookup"><span data-stu-id="5371f-143">The following configuration settings can be used to tune your server for better performance in a SignalR application.</span></span> <span data-ttu-id="5371f-144">Aby uzyskać ogólne informacje na temat sposobu zwiększenia wydajności w aplikacji ASP.NET, zobacz [poprawę wydajności ASP.NET](https://msdn.microsoft.com/library/ff647787.aspx).</span><span class="sxs-lookup"><span data-stu-id="5371f-144">For general information on how to improve performance in an ASP.NET application, see [Improving ASP.NET Performance](https://msdn.microsoft.com/library/ff647787.aspx).</span></span>

<span data-ttu-id="5371f-145">**Ustawienia konfiguracji SignalR**</span><span class="sxs-lookup"><span data-stu-id="5371f-145">**SignalR configuration settings**</span></span>

- <span data-ttu-id="5371f-146">**DefaultMessageBufferSize**: Domyślnie SignalR zachowuje 1000 komunikatów w pamięci na Centrum połączenia.</span><span class="sxs-lookup"><span data-stu-id="5371f-146">**DefaultMessageBufferSize**: By default, SignalR retains 1000 messages in memory per hub per connection.</span></span> <span data-ttu-id="5371f-147">Jeśli używane są duże wiadomości, że może to spowodować problemy z pamięcią, które mogą być złagodzone przez zmniejszenie tej wartości.</span><span class="sxs-lookup"><span data-stu-id="5371f-147">If large messages are being used, this may create memory issues which can be alleviated by reducing this value.</span></span> <span data-ttu-id="5371f-148">To ustawienie można ustawić w `Application_Start` programu obsługi zdarzeń w aplikacji ASP.NET lub `Configuration` metody klasy początkowej OWIN w samodzielnie hostowanej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="5371f-148">This setting can be set in the `Application_Start` event handler in an ASP.NET application, or in the `Configuration` method of an OWIN startup class in a self-hosted application.</span></span> <span data-ttu-id="5371f-149">W poniższym przykładzie pokazano, jak zmniejszyć tę wartość, aby zmniejszyć ilość pamięci zajmowaną przez aplikację, aby zmniejszyć ilość używanej pamięci serwera:</span><span class="sxs-lookup"><span data-stu-id="5371f-149">The following sample demonstrates how to reduce this value in order to reduce the memory footprint of your application in order to reduce the amount of server memory used:</span></span>

    <span data-ttu-id="5371f-150">**Kod serwera .NET w pliku Startup.cs dla malejącej domyślny rozmiar buforu komunikatu**</span><span class="sxs-lookup"><span data-stu-id="5371f-150">**.NET server code in Startup.cs for decreasing default message buffer size**</span></span>

    [!code-csharp[Main](signalr-performance/samples/sample3.cs)]

<span data-ttu-id="5371f-151">**Ustawienia konfiguracji usług IIS**</span><span class="sxs-lookup"><span data-stu-id="5371f-151">**IIS configuration settings**</span></span>

- <span data-ttu-id="5371f-152">**Maksymalna liczba równoczesnych żądań dla aplikacji**: Zwiększenie liczby równoczesnych IIS żądań zwiększy zasoby serwera są dostępne na potrzeby obsługiwania żądań.</span><span class="sxs-lookup"><span data-stu-id="5371f-152">**Max concurrent requests per application**: Increasing the number of concurrent IIS requests will increase server resources available for serving requests.</span></span> <span data-ttu-id="5371f-153">Wartość domyślna to 5000; Aby zwiększyć to ustawienie, wykonaj następujące polecenia w wierszu polecenia z podwyższonym poziomem uprawnień:</span><span class="sxs-lookup"><span data-stu-id="5371f-153">The default value is 5000; to increase this setting, execute the following commands in an elevated command prompt:</span></span>

    [!code-console[Main](signalr-performance/samples/sample4.cmd)]
- <span data-ttu-id="5371f-154">**ApplicationPool QueueLength**: Jest to maksymalna liczba żądań, sterownik Http.sys umieszcza w kolejce dla puli aplikacji.</span><span class="sxs-lookup"><span data-stu-id="5371f-154">**ApplicationPool QueueLength**: This is the maximum number of requests that Http.sys queues for the application pool.</span></span> <span data-ttu-id="5371f-155">Gdy kolejka jest zapełniona, nowe żądania otrzymują odpowiedź 503 "Usługa niedostępna".</span><span class="sxs-lookup"><span data-stu-id="5371f-155">When the queue is full, new requests receive a 503 "Service Unavailable" response.</span></span> <span data-ttu-id="5371f-156">Wartość domyślna to 1000.</span><span class="sxs-lookup"><span data-stu-id="5371f-156">The default value is 1000.</span></span>

    <span data-ttu-id="5371f-157">Skracanie długość kolejki dla procesu roboczego puli aplikacji, hostowanie aplikacji będzie zaoszczędzenia zasobów pamięci.</span><span class="sxs-lookup"><span data-stu-id="5371f-157">Shortening the queue length for the worker process in the application pool hosting your application will conserve memory resources.</span></span> <span data-ttu-id="5371f-158">Aby uzyskać więcej informacji, zobacz [dostrajania, konfigurowanie pul aplikacji i zarządzanie nimi](https://technet.microsoft.com/library/cc745955.aspx).</span><span class="sxs-lookup"><span data-stu-id="5371f-158">For more information, see [Managing, Tuning, and Configuring Application Pools](https://technet.microsoft.com/library/cc745955.aspx).</span></span>

<span data-ttu-id="5371f-159">**Ustawienia konfiguracji programu ASP.NET**</span><span class="sxs-lookup"><span data-stu-id="5371f-159">**ASP.NET configuration settings**</span></span>

<span data-ttu-id="5371f-160">Ta sekcja zawiera ustawienia konfiguracji, które można ustawić w `aspnet.config` pliku.</span><span class="sxs-lookup"><span data-stu-id="5371f-160">This section includes configuration settings that can be set in the `aspnet.config` file.</span></span> <span data-ttu-id="5371f-161">Ten plik znajduje się w jednej z dwóch lokalizacji, w zależności od platformy:</span><span class="sxs-lookup"><span data-stu-id="5371f-161">This file is found in one of two locations, depending on platform:</span></span>

- `%windir%\Microsoft.NET\Framework\v4.0.30319\aspnet.config`
- `%windir%\Microsoft.NET\Framework64\v4.0.30319\aspnet.config`

<span data-ttu-id="5371f-162">Ustawienia programu ASP.NET, które może poprawić wydajność SignalR są następujące:</span><span class="sxs-lookup"><span data-stu-id="5371f-162">ASP.NET settings that may improve SignalR performance include the following:</span></span>

- <span data-ttu-id="5371f-163">**Maksymalna liczba równoczesnych żądań na procesor CPU,**: Zwiększa to ustawienie może zlikwidować wąskie gardła wydajności.</span><span class="sxs-lookup"><span data-stu-id="5371f-163">**Maximum concurrent requests per CPU**: Increasing this setting may alleviate performance bottlenecks.</span></span> <span data-ttu-id="5371f-164">Aby zwiększyć to ustawienie, Dodaj następujące ustawienie konfiguracji, aby `aspnet.config` pliku:</span><span class="sxs-lookup"><span data-stu-id="5371f-164">To increase this setting, add the following configuration setting to the `aspnet.config` file:</span></span>

    [!code-xml[Main](signalr-performance/samples/sample5.xml?highlight=4)]
- <span data-ttu-id="5371f-165">**Ograniczenie kolejki żądań**: Jeśli łączna liczba połączeń przekroczy `maxConcurrentRequestsPerCPU` ustawienie ASP.NET rozpocznie ograniczanie żądań za pomocą kolejki.</span><span class="sxs-lookup"><span data-stu-id="5371f-165">**Request Queue Limit**: When the total number of connections exceeds the `maxConcurrentRequestsPerCPU` setting, ASP.NET will start throttling requests using a queue.</span></span> <span data-ttu-id="5371f-166">Aby zwiększyć rozmiar kolejki, możesz zwiększyć `requestQueueLimit` ustawienie.</span><span class="sxs-lookup"><span data-stu-id="5371f-166">To increase the size of the queue, you can increase the `requestQueueLimit` setting.</span></span> <span data-ttu-id="5371f-167">Aby to zrobić, Dodaj następujące ustawienie konfiguracji, aby `processModel` w węźle `config/machine.config` (zamiast `aspnet.config`):</span><span class="sxs-lookup"><span data-stu-id="5371f-167">To do this, add the following configuration setting to the `processModel` node in `config/machine.config` (rather than `aspnet.config`):</span></span>

    [!code-xml[Main](signalr-performance/samples/sample6.xml)]

<a id="troubleshooting"></a>

## <a name="troubleshooting-performance-issues"></a><span data-ttu-id="5371f-168">Rozwiązywanie problemów z wydajnością</span><span class="sxs-lookup"><span data-stu-id="5371f-168">Troubleshooting performance issues</span></span>

<span data-ttu-id="5371f-169">W tej sekcji opisano sposoby znajdowania wąskich gardeł wydajności w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="5371f-169">This section describes ways to find performance bottlenecks in your application.</span></span>

### <a name="verifying-that-websocket-is-being-used"></a><span data-ttu-id="5371f-170">Weryfikowanie, używanego protokołu WebSocket</span><span class="sxs-lookup"><span data-stu-id="5371f-170">Verifying that WebSocket is being used</span></span>

<span data-ttu-id="5371f-171">Podczas SignalR można użyć różnych transportu do komunikacji między klientem i serwerem, WebSocket oferuje zalety istotnie poprawiającą wydajność i można używać, jeśli klient i serwer obsługują go.</span><span class="sxs-lookup"><span data-stu-id="5371f-171">While SignalR can use a variety of transports for communication between client and server, WebSocket offers a significant performance advantage, and should be used if the client and server support it.</span></span> <span data-ttu-id="5371f-172">Aby określić, jeśli z klientem i serwerem spełniają wymagania dla protokołu WebSocket, zobacz [transportu i planów awaryjnych](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="5371f-172">To determine if your client and server meet the requirements for WebSocket, see [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span> <span data-ttu-id="5371f-173">Aby ustalić, jakie transportu jest używany w aplikacji, przy użyciu narzędzi deweloperskich w przeglądarce i sprawdź dzienniki, aby zobaczyć, jakie transportu jest używany dla połączenia.</span><span class="sxs-lookup"><span data-stu-id="5371f-173">To determine what transport is being used in your application, you can use the browser developer tools, and examine the logs to see what transport is being used for the connection.</span></span> <span data-ttu-id="5371f-174">Aby uzyskać informacje na temat korzystania z narzędzi programistycznych przeglądarki w programie Internet Explorer i Chrome, zobacz [transportu i planów awaryjnych](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="5371f-174">For information on using the browser development tools in Internet Explorer and Chrome, see [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="perfcounters"></a>

## <a name="using-signalr-performance-counters"></a><span data-ttu-id="5371f-175">Korzystanie z liczników wydajności SignalR</span><span class="sxs-lookup"><span data-stu-id="5371f-175">Using SignalR performance counters</span></span>

<span data-ttu-id="5371f-176">W tej sekcji opisano, jak włączanie i używanie liczników wydajności SignalR, w `Microsoft.AspNet.SignalR.Utils` pakietu.</span><span class="sxs-lookup"><span data-stu-id="5371f-176">This section describes how to enable and use SignalR performance counters, found in the `Microsoft.AspNet.SignalR.Utils` package.</span></span>

### <a name="installing-signalrexe"></a><span data-ttu-id="5371f-177">Instalowanie signalr.exe</span><span class="sxs-lookup"><span data-stu-id="5371f-177">Installing signalr.exe</span></span>

<span data-ttu-id="5371f-178">Liczniki wydajności można dodać do serwera przy użyciu narzędzia o nazwie SignalR.exe.</span><span class="sxs-lookup"><span data-stu-id="5371f-178">Performance counters can be added to the server using a utility called SignalR.exe.</span></span> <span data-ttu-id="5371f-179">Aby zainstalować to narzędzie, wykonaj następujące kroki:</span><span class="sxs-lookup"><span data-stu-id="5371f-179">To install this utility, follow these steps:</span></span>

1. <span data-ttu-id="5371f-180">W programie Visual Studio, wybierz **narzędzia** > **Menedżera pakietów NuGet** > **Zarządzaj pakietami NuGet dla rozwiązania**</span><span class="sxs-lookup"><span data-stu-id="5371f-180">In Visual Studio, select **Tools** > **NuGet Package Manager** > **Manage NuGet Packages for Solution**</span></span>
2. <span data-ttu-id="5371f-181">Wyszukaj **signalr.utils**i wybierz opcję instalacji.</span><span class="sxs-lookup"><span data-stu-id="5371f-181">Search for **signalr.utils**, and select Install.</span></span>

    ![](signalr-performance/_static/image1.png)
3. <span data-ttu-id="5371f-182">Zaakceptuj Umowę licencyjną, aby zainstalować pakiet.</span><span class="sxs-lookup"><span data-stu-id="5371f-182">Accept the license agreement to install the package.</span></span>
4. <span data-ttu-id="5371f-183">SignalR.exe zostanie zainstalowana tak, aby `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`.</span><span class="sxs-lookup"><span data-stu-id="5371f-183">SignalR.exe will be installed to `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`.</span></span>

### <a name="installing-performance-counters-with-signalrexe"></a><span data-ttu-id="5371f-184">Instalowanie liczników wydajności z SignalR.exe</span><span class="sxs-lookup"><span data-stu-id="5371f-184">Installing performance counters with SignalR.exe</span></span>

<span data-ttu-id="5371f-185">Aby zainstalować liczników wydajności SignalR, uruchom SignalR.exe w wierszu polecenia z podwyższonym poziomem uprawnień, poziomem następujący parametr:</span><span class="sxs-lookup"><span data-stu-id="5371f-185">To install SignalR performance counters, run SignalR.exe in an elevated command prompt with the following parameter:</span></span>

[!code-console[Main](signalr-performance/samples/sample7.cmd)]

<span data-ttu-id="5371f-186">Aby usunąć liczników wydajności SignalR, uruchom SignalR.exe w wierszu polecenia z podwyższonym poziomem uprawnień, poziomem następujący parametr:</span><span class="sxs-lookup"><span data-stu-id="5371f-186">To remove SignalR performance counters, run SignalR.exe in an elevated command prompt with the following parameter:</span></span>

[!code-console[Main](signalr-performance/samples/sample8.cmd)]

### <a name="signalr-performance-counters"></a><span data-ttu-id="5371f-187">Liczniki wydajności SignalR</span><span class="sxs-lookup"><span data-stu-id="5371f-187">SignalR Performance counters</span></span>

<span data-ttu-id="5371f-188">Pakiet narzędzi instaluje następujące liczniki wydajności.</span><span class="sxs-lookup"><span data-stu-id="5371f-188">The utilities package installs the following performance counters.</span></span> <span data-ttu-id="5371f-189">"Całkowita" liczniki pomiaru liczba zdarzeń od czasu ostatniego pula aplikacji lub serwera ponownego uruchomienia.</span><span class="sxs-lookup"><span data-stu-id="5371f-189">The "Total" counters measure the number of events since the last application pool or server restart.</span></span>

<span data-ttu-id="5371f-190">**Metryki połączeń**</span><span class="sxs-lookup"><span data-stu-id="5371f-190">**Connection metrics**</span></span>

<span data-ttu-id="5371f-191">Następujące metryki pomiaru zdarzenia okresu istnienia połączenia, które wystąpiły.</span><span class="sxs-lookup"><span data-stu-id="5371f-191">The following metrics measure the connection lifetime events that occur.</span></span> <span data-ttu-id="5371f-192">Aby uzyskać więcej informacji, zobacz [zrozumienia i obsługa zdarzeń okresu istnienia połączenia](../guide-to-the-api/handling-connection-lifetime-events.md).</span><span class="sxs-lookup"><span data-stu-id="5371f-192">For more information, see [Understanding and Handling Connection Lifetime Events](../guide-to-the-api/handling-connection-lifetime-events.md).</span></span>

- <span data-ttu-id="5371f-193">**Połączeń**</span><span class="sxs-lookup"><span data-stu-id="5371f-193">**Connections Connected**</span></span>
- <span data-ttu-id="5371f-194">**Połączenia zakończone**</span><span class="sxs-lookup"><span data-stu-id="5371f-194">**Connections Reconnected**</span></span>
- <span data-ttu-id="5371f-195">**Połączenia rozłączony**</span><span class="sxs-lookup"><span data-stu-id="5371f-195">**Connections Disconnected**</span></span>
- <span data-ttu-id="5371f-196">**Bieżące połączenia**</span><span class="sxs-lookup"><span data-stu-id="5371f-196">**Connections Current**</span></span>

<span data-ttu-id="5371f-197">**Metryki wiadomości**</span><span class="sxs-lookup"><span data-stu-id="5371f-197">**Message metrics**</span></span>

<span data-ttu-id="5371f-198">Następujące metryki pomiaru ruchu komunikat generowany przez SignalR.</span><span class="sxs-lookup"><span data-stu-id="5371f-198">The following metrics measure the message traffic generated by SignalR.</span></span>

- <span data-ttu-id="5371f-199">**Komunikaty o połączenia odebrane łącznie**</span><span class="sxs-lookup"><span data-stu-id="5371f-199">**Connection Messages Received Total**</span></span>
- <span data-ttu-id="5371f-200">**Łączna liczba wysyłane wiadomości połączenia**</span><span class="sxs-lookup"><span data-stu-id="5371f-200">**Connection Messages Sent Total**</span></span>
- <span data-ttu-id="5371f-201">**Połączenie komunikaty odebrane/s**</span><span class="sxs-lookup"><span data-stu-id="5371f-201">**Connection Messages Received/Sec**</span></span>
- <span data-ttu-id="5371f-202">**Połączenie wiadomości wysłane/s**</span><span class="sxs-lookup"><span data-stu-id="5371f-202">**Connection Messages Sent/Sec**</span></span>

<span data-ttu-id="5371f-203">**Komunikatów bus metryki**</span><span class="sxs-lookup"><span data-stu-id="5371f-203">**Message bus metrics**</span></span>

<span data-ttu-id="5371f-204">Następujące metryki pomiaru ruchu za pośrednictwem wewnętrznego magistrali komunikatu SignalR, kolejki, w której są umieszczane wszystkich przychodzących i wychodzących komunikatów SignalR.</span><span class="sxs-lookup"><span data-stu-id="5371f-204">The following metrics measure traffic through the internal SignalR message bus, the queue in which all incoming and outgoing SignalR messages are placed.</span></span> <span data-ttu-id="5371f-205">Komunikat jest **opublikowano** po zakończeniu wysyłania lub emisji.</span><span class="sxs-lookup"><span data-stu-id="5371f-205">A message is **Published** when it is sent or broadcast.</span></span> <span data-ttu-id="5371f-206">A **subskrybenta** w tym kontekście jest subskrypcji na magistrali komunikatu; to powinna być równa liczbie klientów, a także sam serwer.</span><span class="sxs-lookup"><span data-stu-id="5371f-206">A **Subscriber** in this context is a subscription on the message bus; this should equal the number of clients plus the server itself.</span></span> <span data-ttu-id="5371f-207">**Przydzielone procesu roboczego** to składnik, który wysyła dane do aktywnego połączenia; **zajęty, proces roboczy** to taki, który jest aktywnie wysyłania komunikatu.</span><span class="sxs-lookup"><span data-stu-id="5371f-207">An **Allocated Worker** is a component that sends data to active connections; a **Busy Worker** is one that is actively sending a message.</span></span>

- <span data-ttu-id="5371f-208">**Magistrala komunikatów komunikaty odebrane łącznie**</span><span class="sxs-lookup"><span data-stu-id="5371f-208">**Message Bus Messages Received Total**</span></span>
- <span data-ttu-id="5371f-209">**Magistrala komunikatów komunikaty odebrane/s**</span><span class="sxs-lookup"><span data-stu-id="5371f-209">**Message Bus Messages Received/Sec**</span></span>
- <span data-ttu-id="5371f-210">**Komunikatów Bus komunikatów opublikowanych łącznie**</span><span class="sxs-lookup"><span data-stu-id="5371f-210">**Message Bus Messages Published Total**</span></span>
- <span data-ttu-id="5371f-211">**Magistrala komunikatów komunikaty publikowane na sekundę**</span><span class="sxs-lookup"><span data-stu-id="5371f-211">**Message Bus Messages Published/Sec**</span></span>
- <span data-ttu-id="5371f-212">**Bieżąca subskrybentów magistrali komunikatów**</span><span class="sxs-lookup"><span data-stu-id="5371f-212">**Message Bus Subscribers Current**</span></span>
- <span data-ttu-id="5371f-213">**Łączna liczba subskrybentów magistrali komunikatów**</span><span class="sxs-lookup"><span data-stu-id="5371f-213">**Message Bus Subscribers Total**</span></span>
- <span data-ttu-id="5371f-214">**Subskrybentów magistrali komunikatów na sekundę**</span><span class="sxs-lookup"><span data-stu-id="5371f-214">**Message Bus Subscribers/Sec**</span></span>
- <span data-ttu-id="5371f-215">**Magistrala komunikatów przydzielonych pracowników**</span><span class="sxs-lookup"><span data-stu-id="5371f-215">**Message Bus Allocated Workers**</span></span>
- <span data-ttu-id="5371f-216">**Komunikatów Bus zajętych pracowników**</span><span class="sxs-lookup"><span data-stu-id="5371f-216">**Message Bus Busy Workers**</span></span>
- <span data-ttu-id="5371f-217">**Bieżąca tematy magistrali komunikatów**</span><span class="sxs-lookup"><span data-stu-id="5371f-217">**Message Bus Topics Current**</span></span>

<span data-ttu-id="5371f-218">**Błąd metryki**</span><span class="sxs-lookup"><span data-stu-id="5371f-218">**Error metrics**</span></span>

<span data-ttu-id="5371f-219">Następujące metryki pomiaru błędów generowanych przez ruch komunikatów SignalR.</span><span class="sxs-lookup"><span data-stu-id="5371f-219">The following metrics measure errors generated by SignalR message traffic.</span></span> <span data-ttu-id="5371f-220">**Rozpoznawanie Centrum** błędy występują, gdy nie można rozpoznać koncentratora lub metody koncentratora.</span><span class="sxs-lookup"><span data-stu-id="5371f-220">**Hub Resolution** errors occur when a hub or hub method cannot be resolved.</span></span> <span data-ttu-id="5371f-221">**Wywołania koncentratora** błędy są wyjątki generowane podczas wywołania metody koncentratora.</span><span class="sxs-lookup"><span data-stu-id="5371f-221">**Hub Invocation** errors are exceptions thrown while invoking a hub method.</span></span> <span data-ttu-id="5371f-222">**Transport** błędy są błędami połączenia podczas żądania lub odpowiedzi HTTP.</span><span class="sxs-lookup"><span data-stu-id="5371f-222">**Transport** errors are connection errors thrown during an HTTP request or response.</span></span>

- <span data-ttu-id="5371f-223">**Błędy: Suma wszystkich**</span><span class="sxs-lookup"><span data-stu-id="5371f-223">**Errors: All Total**</span></span>
- <span data-ttu-id="5371f-224">**Błędy: Wszystkie na sekundę**</span><span class="sxs-lookup"><span data-stu-id="5371f-224">**Errors: All/Sec**</span></span>
- <span data-ttu-id="5371f-225">**Błędy: Łączna liczba rozpoznawania koncentratora**</span><span class="sxs-lookup"><span data-stu-id="5371f-225">**Errors: Hub Resolution Total**</span></span>
- <span data-ttu-id="5371f-226">**Błędy: Rozpoznawania koncentratora na sekundę**</span><span class="sxs-lookup"><span data-stu-id="5371f-226">**Errors: Hub Resolution/Sec**</span></span>
- <span data-ttu-id="5371f-227">**Błędy: Łączna liczba wywołania koncentratora**</span><span class="sxs-lookup"><span data-stu-id="5371f-227">**Errors: Hub Invocation Total**</span></span>
- <span data-ttu-id="5371f-228">**Błędy: Wywołania koncentratora na sekundę**</span><span class="sxs-lookup"><span data-stu-id="5371f-228">**Errors: Hub Invocation/Sec**</span></span>
- <span data-ttu-id="5371f-229">**Błędy: Łączna liczba transportu**</span><span class="sxs-lookup"><span data-stu-id="5371f-229">**Errors: Transport Total**</span></span>
- <span data-ttu-id="5371f-230">**Błędy: Transportu na sekundę**</span><span class="sxs-lookup"><span data-stu-id="5371f-230">**Errors: Transport/Sec**</span></span>

<a id="scaleout_metrics"></a>

<span data-ttu-id="5371f-231">**Metryki skalowania w poziomie**</span><span class="sxs-lookup"><span data-stu-id="5371f-231">**Scaleout metrics**</span></span>

<span data-ttu-id="5371f-232">Następujące metryki pomiaru ruchu i błędy generowane przez dostawcę skalowania w poziomie.</span><span class="sxs-lookup"><span data-stu-id="5371f-232">The following metrics measure traffic and errors generated by the scaleout provider.</span></span> <span data-ttu-id="5371f-233">A **Stream** w tym kontekście jest jednostki skalowania używanej przez dostawcę skalowania; to jest tabelą, jeśli jest używany program SQL Server, tematu, jeśli jest używana usługa Service Bus i subskrypcji, jeśli jest używany w pamięci podręcznej Redis.</span><span class="sxs-lookup"><span data-stu-id="5371f-233">A **Stream** in this context is a scale unit used by the scaleout provider; this is a table if SQL Server is used, a Topic if Service Bus is used, and a Subscription if Redis is used.</span></span> <span data-ttu-id="5371f-234">Zapewnia każdego strumienia, uporządkowany operacji odczytu i zapisu; jednemu strumieniowi jest potencjalnych wąskich gardeł skali, dzięki czemu można zwiększyć liczbę strumieni z myślą o zmniejszeniu tego wąskiego gardła.</span><span class="sxs-lookup"><span data-stu-id="5371f-234">Each stream ensures ordered read and write operations; a single stream is a potential scale bottleneck, so the number of streams can be increased to help reduce that bottleneck.</span></span> <span data-ttu-id="5371f-235">Użycie wielu strumieni SignalR będzie automatycznie dystrybuować komunikaty (fragmencie) te strumienie w sposób, który gwarantuje, że komunikaty wysyłane z dowolnego danego połączenia są w kolejności.</span><span class="sxs-lookup"><span data-stu-id="5371f-235">If multiple streams are used, SignalR will automatically distribute (shard) messages across these streams in a way that ensures messages sent from any given connection are in order.</span></span>

<span data-ttu-id="5371f-236">[MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx) kontroluje długość kolejki wysyłania skalowania, które są obsługiwane przez SignalR.</span><span class="sxs-lookup"><span data-stu-id="5371f-236">The [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx) setting controls the length of the scaleout send queue maintained by SignalR.</span></span> <span data-ttu-id="5371f-237">Zostanie ustawiona na wartość większą niż 0, zostaną umieszczone wszystkie komunikaty w kolejce wysyłania do wysłania pojedynczo do skonfigurowanego płyty montażowej obsługi komunikatów.</span><span class="sxs-lookup"><span data-stu-id="5371f-237">Setting it to a value greater than 0 will place all messages in a send queue to be sent one at a time to the configured messaging backplane.</span></span> <span data-ttu-id="5371f-238">Rozmiar kolejki przekroczy skonfigurowany długość, kolejne wywołania do wysyłania będą działać od razu z [InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception(v=vs.118).aspx) do momentu liczba wiadomości w kolejce jest mniejsza niż wartość ustawienia ponownie.</span><span class="sxs-lookup"><span data-stu-id="5371f-238">If the size of the queue goes above the configured length, subsequent calls to send will fail immediately with an [InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception(v=vs.118).aspx) until the number of messages in the queue is less than the setting again.</span></span> <span data-ttu-id="5371f-239">Kolejkowanie jest domyślnie wyłączona, ponieważ implementowane montażowych zazwyczaj mają własne usługi kolejkowania lub Sterowanie przepływem w miejscu.</span><span class="sxs-lookup"><span data-stu-id="5371f-239">Queueing is disabled by default because the implemented backplanes generally have their own queuing or flow-control in place.</span></span> <span data-ttu-id="5371f-240">W przypadku programu SQL Server efektywnego buforowania połączeń ogranicza liczbę wysyła dzieje się w dowolnym momencie.</span><span class="sxs-lookup"><span data-stu-id="5371f-240">In the case of SQL Server, connection pooling effectively limits the number of sends going on at any one time.</span></span>

<span data-ttu-id="5371f-241">Domyślnie tylko jeden strumień jest używany do programu SQL Server i usługi Redis, pięć strumienie są używane dla usługi Service Bus i Kolejkowanie jest wyłączona, ale te ustawienia można zmienić, modyfikując konfigurację programu SQL Server i usługi Service Bus:</span><span class="sxs-lookup"><span data-stu-id="5371f-241">By default, only one stream is used for SQL Server and Redis, five streams are used for Service Bus, and queueing is disabled, but these settings can be changed through configuration on SQL Server and Service Bus:</span></span>

<span data-ttu-id="5371f-242">**Kod serwera platformy .NET do konfigurowania długość kolejki i liczba tabeli płyty montażowej programu SQL Server**</span><span class="sxs-lookup"><span data-stu-id="5371f-242">**.NET Server Code for configuring table count and queue length for SQL Server backplane**</span></span>

[!code-csharp[Main](signalr-performance/samples/sample9.cs)]

<span data-ttu-id="5371f-243">**Konfigurowanie długość kolejki i liczba tematu usługi Service Bus płyty montażowej kod serwera .NET**</span><span class="sxs-lookup"><span data-stu-id="5371f-243">**.NET Server Code for configuring topic count and queue length for Service Bus backplane**</span></span>

[!code-csharp[Main](signalr-performance/samples/sample10.cs)]

<span data-ttu-id="5371f-244">A **buforowanie** strumienia to taki, który został wprowadzony w stan; gdy strumień jest w stanie błędu, wszystkie wiadomości wysyłane do systemu backplane zakończy się niepowodzeniem, dopóki nie strumienia nie jest już spowodował błąd.</span><span class="sxs-lookup"><span data-stu-id="5371f-244">A **Buffering** stream is one that has entered a faulted state; when the stream is in the faulted state, all messages sent to the backplane will fail immediately until the stream is no longer faulting.</span></span> <span data-ttu-id="5371f-245">**Długość kolejki wysyłania** jest to liczba komunikatów, które zostały opublikowane, ale nie została jeszcze wysłane.</span><span class="sxs-lookup"><span data-stu-id="5371f-245">The **Send Queue Length** is the number of messages that have been posted but not yet sent.</span></span>

- <span data-ttu-id="5371f-246">**Magistrali komunikatów skalowania w poziomie komunikaty odebrane/s**</span><span class="sxs-lookup"><span data-stu-id="5371f-246">**Scaleout Message Bus Messages Received/Sec**</span></span>
- <span data-ttu-id="5371f-247">**Łączna liczba strumieni skalowania w poziomie**</span><span class="sxs-lookup"><span data-stu-id="5371f-247">**Scaleout Streams Total**</span></span>
- <span data-ttu-id="5371f-248">**Skalowalne strumieni Open**</span><span class="sxs-lookup"><span data-stu-id="5371f-248">**Scaleout Streams Open**</span></span>
- <span data-ttu-id="5371f-249">**Strumienie ze skalowaniem buforowania**</span><span class="sxs-lookup"><span data-stu-id="5371f-249">**Scaleout Streams Buffering**</span></span>
- <span data-ttu-id="5371f-250">**Całkowita liczba błędów skalowania w poziomie**</span><span class="sxs-lookup"><span data-stu-id="5371f-250">**Scaleout Errors Total**</span></span>
- <span data-ttu-id="5371f-251">**Błędów skalowania na sekundę**</span><span class="sxs-lookup"><span data-stu-id="5371f-251">**Scaleout Errors/Sec**</span></span>
- <span data-ttu-id="5371f-252">**Długość kolejki wysyłania skalowania w poziomie**</span><span class="sxs-lookup"><span data-stu-id="5371f-252">**Scaleout Send Queue Length**</span></span>

<span data-ttu-id="5371f-253">Aby uzyskać więcej informacji na temat co te liczniki są pomiaru, zobacz [SignalR — skalowanie w poziomie za pomocą usługi Azure Service Bus](scaleout-with-windows-azure-service-bus.md).</span><span class="sxs-lookup"><span data-stu-id="5371f-253">For more information on what these counters are measuring, see [SignalR Scaleout with Azure Service Bus](scaleout-with-windows-azure-service-bus.md).</span></span>

<a id="othercounters"></a>

## <a name="using-other-performance-counters"></a><span data-ttu-id="5371f-254">Korzystanie z innych liczników wydajności</span><span class="sxs-lookup"><span data-stu-id="5371f-254">Using other performance counters</span></span>

<span data-ttu-id="5371f-255">Następujące liczniki wydajności mogą być też przydatne do monitorowania wydajności aplikacji.</span><span class="sxs-lookup"><span data-stu-id="5371f-255">The following performance counters may also be useful in monitoring your application's performance.</span></span>

<span data-ttu-id="5371f-256">**Pamięć**</span><span class="sxs-lookup"><span data-stu-id="5371f-256">**Memory**</span></span>

- <span data-ttu-id="5371f-257">Pamięć .NET CLR\\liczba bajtów we wszystkich sterty (na potrzeby w3wp)</span><span class="sxs-lookup"><span data-stu-id="5371f-257">.NET CLR Memory\\# bytes in all Heaps (for w3wp)</span></span>

<span data-ttu-id="5371f-258">**ASP.NET**</span><span class="sxs-lookup"><span data-stu-id="5371f-258">**ASP.NET**</span></span>

- <span data-ttu-id="5371f-259">Bieżąca ASP.NET\Requests</span><span class="sxs-lookup"><span data-stu-id="5371f-259">ASP.NET\Requests Current</span></span>
- <span data-ttu-id="5371f-260">ASP.NET\Queued</span><span class="sxs-lookup"><span data-stu-id="5371f-260">ASP.NET\Queued</span></span>
- <span data-ttu-id="5371f-261">ASP.NET\Rejected</span><span class="sxs-lookup"><span data-stu-id="5371f-261">ASP.NET\Rejected</span></span>

<span data-ttu-id="5371f-262">**CPU**</span><span class="sxs-lookup"><span data-stu-id="5371f-262">**CPU**</span></span>

- <span data-ttu-id="5371f-263">Czas Information\Processor procesora</span><span class="sxs-lookup"><span data-stu-id="5371f-263">Processor Information\Processor Time</span></span>

<span data-ttu-id="5371f-264">**TCP/IP**</span><span class="sxs-lookup"><span data-stu-id="5371f-264">**TCP/IP**</span></span>

- <span data-ttu-id="5371f-265">TCPv6/ustanowionych połączeń</span><span class="sxs-lookup"><span data-stu-id="5371f-265">TCPv6/Connections Established</span></span>
- <span data-ttu-id="5371f-266">TCPv4/ustanowionych połączeń</span><span class="sxs-lookup"><span data-stu-id="5371f-266">TCPv4/Connections Established</span></span>

<span data-ttu-id="5371f-267">**Usługa sieci Web**</span><span class="sxs-lookup"><span data-stu-id="5371f-267">**Web Service**</span></span>

- <span data-ttu-id="5371f-268">Połączenia Service\Current sieci Web</span><span class="sxs-lookup"><span data-stu-id="5371f-268">Web Service\Current Connections</span></span>
- <span data-ttu-id="5371f-269">Połączenia Service\Maximum sieci Web</span><span class="sxs-lookup"><span data-stu-id="5371f-269">Web Service\Maximum Connections</span></span>

<span data-ttu-id="5371f-270">**Wątkowość**</span><span class="sxs-lookup"><span data-stu-id="5371f-270">**Threading**</span></span>

- <span data-ttu-id="5371f-271">.NET CLR blokad i wątków\\liczba bieżących wątków logiczne</span><span class="sxs-lookup"><span data-stu-id="5371f-271">.NET CLR Locks And Threads\\# of current logical Threads</span></span>
- <span data-ttu-id="5371f-272">.NET CLR blokad i wątków\\liczba bieżących wątków fizycznych</span><span class="sxs-lookup"><span data-stu-id="5371f-272">.NET CLR Locks And Threads\\# of current physical Threads</span></span>

<a id="otherresources"></a>

## <a name="other-resources"></a><span data-ttu-id="5371f-273">Inne zasoby</span><span class="sxs-lookup"><span data-stu-id="5371f-273">Other Resources</span></span>

<span data-ttu-id="5371f-274">Aby uzyskać więcej informacji na temat wydajności programu ASP.NET, monitorowania i dostrajania, zobacz następujące tematy:</span><span class="sxs-lookup"><span data-stu-id="5371f-274">For more information on ASP.NET performance monitoring and tuning, see the following topics:</span></span>

- <span data-ttu-id="5371f-275">[Przegląd wydajności platformy ASP.NET](https://msdn.microsoft.com/library/cc668225(v=vs.100).aspx)</span><span class="sxs-lookup"><span data-stu-id="5371f-275">[ASP.NET Performance Overview](https://msdn.microsoft.com/library/cc668225(v=vs.100).aspx)</span></span>
- [<span data-ttu-id="5371f-276">Użycie wątków programu ASP.NET, IIS 7.5, usługi IIS 7.0 i IIS 6.0</span><span class="sxs-lookup"><span data-stu-id="5371f-276">ASP.NET Thread Usage on IIS 7.5, IIS 7.0, and IIS 6.0</span></span>](https://blogs.msdn.com/b/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx)
- [<span data-ttu-id="5371f-277">&lt;applicationPool&gt; — Element (ustawienia internetowe)</span><span class="sxs-lookup"><span data-stu-id="5371f-277">&lt;applicationPool&gt; Element (Web Settings)</span></span>](https://msdn.microsoft.com/library/dd560842.aspx)
