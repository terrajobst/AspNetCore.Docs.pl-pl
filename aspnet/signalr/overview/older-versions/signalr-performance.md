---
uid: signalr/overview/older-versions/signalr-performance
title: Wydajność SignalR (SignalR 1.x) | Dokumentacja firmy Microsoft
author: pfletcher
description: Wydajność SignalR
ms.author: riande
ms.date: 07/03/2013
ms.assetid: 9594d644-66b6-4223-acdd-23e29a6e4c46
msc.legacyurl: /signalr/overview/older-versions/signalr-performance
msc.type: authoredcontent
ms.openlocfilehash: 3ac62639617e1ff83761d0a1d45c27303d0b820d
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912764"
---
<a name="signalr-performance-signalr-1x"></a><span data-ttu-id="eee91-103">Wydajność SignalR (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="eee91-103">SignalR Performance (SignalR 1.x)</span></span>
====================
<span data-ttu-id="eee91-104">przez [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="eee91-104">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> <span data-ttu-id="eee91-105">W tym temacie opisano sposób projektowania, miar i zwiększenie wydajności w aplikacji SignalR.</span><span class="sxs-lookup"><span data-stu-id="eee91-105">This topic describes how to design for, measure, and improve performance in a SignalR application.</span></span>


<span data-ttu-id="eee91-106">Ostatnie prezentacji na wydajność SignalR i skalowania, zobacz [skalowanie sieci Web w czasie rzeczywistym z użyciem ASP.NET SignalR](https://channel9.msdn.com/Events/Build/2013/3-502).</span><span class="sxs-lookup"><span data-stu-id="eee91-106">For a recent presentation on SignalR performance and scaling, see [Scaling the Real-time Web with ASP.NET SignalR](https://channel9.msdn.com/Events/Build/2013/3-502).</span></span>

<span data-ttu-id="eee91-107">Ten temat zawiera następujące sekcje:</span><span class="sxs-lookup"><span data-stu-id="eee91-107">This topic contains the following sections:</span></span>

- [<span data-ttu-id="eee91-108">Zagadnienia dotyczące projektowania</span><span class="sxs-lookup"><span data-stu-id="eee91-108">Design considerations</span></span>](#design)
- [<span data-ttu-id="eee91-109">Dostrajanie wydajności serwera SignalR</span><span class="sxs-lookup"><span data-stu-id="eee91-109">Tuning your SignalR server for performance</span></span>](#tuning)
- [<span data-ttu-id="eee91-110">Rozwiązywanie problemów z wydajnością</span><span class="sxs-lookup"><span data-stu-id="eee91-110">Troubleshooting performance issues</span></span>](#troubleshooting)
- [<span data-ttu-id="eee91-111">Korzystanie z liczników wydajności SignalR</span><span class="sxs-lookup"><span data-stu-id="eee91-111">Using SignalR performance counters</span></span>](#perfcounters)
- [<span data-ttu-id="eee91-112">Korzystanie z innych liczników wydajności</span><span class="sxs-lookup"><span data-stu-id="eee91-112">Using other performance counters</span></span>](#othercounters)
- [<span data-ttu-id="eee91-113">Inne zasoby</span><span class="sxs-lookup"><span data-stu-id="eee91-113">Other resources</span></span>](#otherresources)

<a id="design"></a>

## <a name="design-considerations"></a><span data-ttu-id="eee91-114">Zagadnienia dotyczące projektowania</span><span class="sxs-lookup"><span data-stu-id="eee91-114">Design considerations</span></span>

<span data-ttu-id="eee91-115">W tej sekcji opisano wzorców, które można zastosować podczas projektowania aplikacji SignalR, aby upewnić się, że wydajność jest nie utrudniona, generując niepotrzebny ruch sieciowy.</span><span class="sxs-lookup"><span data-stu-id="eee91-115">This section describes patterns that can be implemented during the design of a SignalR application, to ensure that performance is not being hindered by generating unnecessary network traffic.</span></span>

### <a name="throttling-message-frequency"></a><span data-ttu-id="eee91-116">Ograniczanie częstotliwości wiadomości</span><span class="sxs-lookup"><span data-stu-id="eee91-116">Throttling message frequency</span></span>

<span data-ttu-id="eee91-117">Nawet w przypadku aplikacji, która wysyła komunikaty o wysokiej częstotliwości (np. aplikacji gier w języku czasu rzeczywistego) większość aplikacji nie trzeba wysłać więcej niż kilka komunikatów na sekundę.</span><span class="sxs-lookup"><span data-stu-id="eee91-117">Even in an application that sends out messages at a high frequency (such as a realtime gaming application), most applications don't need to send more than a few messages a second.</span></span> <span data-ttu-id="eee91-118">Aby zmniejszyć ilość ruchu sieciowego, który generuje każdy klient, można zaimplementować pętli komunikatów, kolejki i wysyła się komunikaty nie częściej niż stałą stawkę (czyli maksymalna liczba wiadomości będą wysyłane co sekundę w przypadku komunikatów w tym czasie w nterwał do wysłania).</span><span class="sxs-lookup"><span data-stu-id="eee91-118">To reduce the amount of traffic that each client generates, a message loop can be implemented that queues and sends out messages no more frequently than a fixed rate (that is, up to a certain number of messages will be sent every second, if there are messages in that time interval to be sent).</span></span> <span data-ttu-id="eee91-119">Dla przykładowej aplikacji, która ogranicza wiadomości do niektórych współczynnik (od klienta i serwera), zobacz [Realtime o wysokiej częstotliwości za pomocą biblioteki SignalR](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="eee91-119">For a sample application that throttles messages to a certain rate (from both client and server), see [High-Frequency Realtime with SignalR](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).</span></span>

### <a name="reducing-message-size"></a><span data-ttu-id="eee91-120">Zmniejszenie rozmiaru wiadomości</span><span class="sxs-lookup"><span data-stu-id="eee91-120">Reducing message size</span></span>

<span data-ttu-id="eee91-121">Aby zmniejszyć rozmiar komunikatu SignalR, należy zmniejszyć rozmiar serializacji obiektów.</span><span class="sxs-lookup"><span data-stu-id="eee91-121">You can reduce the size of a SignalR message by reducing the size of your serialized objects.</span></span> <span data-ttu-id="eee91-122">W kodzie serwera, jest wysyłana obiekt, który zawiera właściwości, które nie muszą być przekazywane, uniemożliwi te właściwości serializowanego przy użyciu `JsonIgnore` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="eee91-122">In server code, if you're sending an object that contains properties that don't need to be transmitted, prevent those properties from being serialized by using the `JsonIgnore` attribute.</span></span> <span data-ttu-id="eee91-123">Nazwy właściwości są także przechowywane informacje w wiadomości; nazwy właściwości może zostać skrócony przy użyciu `JsonProperty` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="eee91-123">The names of properties are also stored in the message; the names of properties can be shortened using the `JsonProperty` attribute.</span></span> <span data-ttu-id="eee91-124">W poniższym przykładzie kodu pokazano sposób wykluczać właściwość zostały wysłane do klienta oraz skrócić nazwy właściwości:</span><span class="sxs-lookup"><span data-stu-id="eee91-124">The following code sample demonstrates how to exclude a property from being sent to the client, and how to shorten property names:</span></span>

<span data-ttu-id="eee91-125">**Kod serwera platformy .NET, który demonstruje, atrybut JsonIgnore wykluczanie danych zostały wysłane do klienta, a atrybut JsonProperty, aby zmniejszyć rozmiar komunikatu**</span><span class="sxs-lookup"><span data-stu-id="eee91-125">**.NET server code that demonstrates the JsonIgnore attribute to exclude data from being sent to the client, and the JsonProperty attribute to reduce message size**</span></span>

[!code-csharp[Main](signalr-performance/samples/sample1.cs?highlight=5,7,10)]

<span data-ttu-id="eee91-126">Aby zachować czytelność / łatwości utrzymania kodu klienta, nazwy skróconej właściwości mogą być ponownie zmapowany do przyjaznego dla człowieka nazwami po otrzymaniu komunikatu.</span><span class="sxs-lookup"><span data-stu-id="eee91-126">In order to retain readability/ maintainability in the client code, the abbreviated property names can be remapped to human-friendly names after the message is received.</span></span> <span data-ttu-id="eee91-127">W poniższym przykładzie kodu pokazano jeden sposób możliwe ponowne mapowanie skrócone nazwy tymi dłużej, definiując kontraktu komunikatu (mapowanie) i za pomocą `reMap` funkcja do stosowania Umowy do klasy zoptymalizowane wiadomości:</span><span class="sxs-lookup"><span data-stu-id="eee91-127">The following code sample demonstrates one possible way of remapping shortened names to longer ones, by defining a message contract (mapping), and using the `reMap` function to apply the contract to the optimized message class:</span></span>

<span data-ttu-id="eee91-128">**Kod JavaScript po stronie klienta, który ponownie mapuje skrócone nazwy właściwości na nazwy czytelny dla człowieka**</span><span class="sxs-lookup"><span data-stu-id="eee91-128">**Client-side JavaScript code that remaps shortened property names to human-readable names**</span></span>

[!code-javascript[Main](signalr-performance/samples/sample2.js)]

<span data-ttu-id="eee91-129">Nazwy mogą skrócony w wiadomościach od klienta do serwera, jak również za pomocą tej samej metody.</span><span class="sxs-lookup"><span data-stu-id="eee91-129">Names can be shortened in messages from the client to the server as well, using the same method.</span></span>

<span data-ttu-id="eee91-130">Zmniejszenie zużycia pamięci (czyli ilości pamięci używanej dla wiadomości) komunikatu obiektu może również zwiększyć wydajność.</span><span class="sxs-lookup"><span data-stu-id="eee91-130">Reducing the memory footprint (that is, the amount of memory used for the message) of the message object can also improve performance.</span></span> <span data-ttu-id="eee91-131">Przykładowo jeśli pełną gamę `int` nie jest potrzebna, `short` lub `byte` mogą być używane zamiast tego.</span><span class="sxs-lookup"><span data-stu-id="eee91-131">For example, if the full range of an `int` is not needed, a `short` or `byte` can be used instead.</span></span>

<span data-ttu-id="eee91-132">Ponieważ komunikaty są przechowywane w magistrali komunikatów w pamięci serwera, zmniejszenie rozmiaru wiadomości, można także rozwiązać problemy z z pamięci serwera.</span><span class="sxs-lookup"><span data-stu-id="eee91-132">Since messages are stored in the message bus in server memory, reducing the size of messages can also address server memory issues.</span></span>

<a id="tuning"></a>

### <a name="tuning-your-signalr-server-for-performance"></a><span data-ttu-id="eee91-133">Dostrajanie wydajności serwera SignalR</span><span class="sxs-lookup"><span data-stu-id="eee91-133">Tuning your SignalR server for performance</span></span>

<span data-ttu-id="eee91-134">Następujące ustawienia konfiguracji można Dostrajanie Serwer w celu zapewnienia lepszej wydajności w aplikacji SignalR.</span><span class="sxs-lookup"><span data-stu-id="eee91-134">The following configuration settings can be used to tune your server for better performance in a SignalR application.</span></span> <span data-ttu-id="eee91-135">Aby uzyskać ogólne informacje na temat sposobu zwiększenia wydajności w aplikacji ASP.NET, zobacz [poprawę wydajności ASP.NET](https://msdn.microsoft.com/library/ff647787.aspx).</span><span class="sxs-lookup"><span data-stu-id="eee91-135">For general information on how to improve performance in an ASP.NET application, see [Improving ASP.NET Performance](https://msdn.microsoft.com/library/ff647787.aspx).</span></span>

<span data-ttu-id="eee91-136">**Ustawienia konfiguracji SignalR**</span><span class="sxs-lookup"><span data-stu-id="eee91-136">**SignalR configuration settings**</span></span>

- <span data-ttu-id="eee91-137">**DefaultMessageBufferSize**: domyślnie SignalR zachowuje 1000 komunikatów w pamięci na Centrum połączenia.</span><span class="sxs-lookup"><span data-stu-id="eee91-137">**DefaultMessageBufferSize**: By default, SignalR retains 1000 messages in memory per hub per connection.</span></span> <span data-ttu-id="eee91-138">Jeśli używane są duże wiadomości, że może to spowodować problemy z pamięcią, które mogą być złagodzone przez zmniejszenie tej wartości.</span><span class="sxs-lookup"><span data-stu-id="eee91-138">If large messages are being used, this may create memory issues which can be alleviated by reducing this value.</span></span> <span data-ttu-id="eee91-139">To ustawienie można ustawić w `Application_Start` programu obsługi zdarzeń w aplikacji ASP.NET lub `Configuration` metody klasy początkowej OWIN w samodzielnie hostowanej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="eee91-139">This setting can be set in the `Application_Start` event handler in an ASP.NET application, or in the `Configuration` method of an OWIN startup class in a self-hosted application.</span></span> <span data-ttu-id="eee91-140">W poniższym przykładzie pokazano, jak zmniejszyć tę wartość, aby zmniejszyć ilość pamięci zajmowaną przez aplikację, aby zmniejszyć ilość używanej pamięci serwera:</span><span class="sxs-lookup"><span data-stu-id="eee91-140">The following sample demonstrates how to reduce this value in order to reduce the memory footprint of your application in order to reduce the amount of server memory used:</span></span>

    <span data-ttu-id="eee91-141">**Kod serwera .NET w pliku Global.asax dla malejącej domyślny rozmiar buforu komunikatu**</span><span class="sxs-lookup"><span data-stu-id="eee91-141">**.NET server code in Global.asax for decreasing default message buffer size**</span></span>

    [!code-csharp[Main](signalr-performance/samples/sample3.cs)]

<span data-ttu-id="eee91-142">**Ustawienia konfiguracji usług IIS**</span><span class="sxs-lookup"><span data-stu-id="eee91-142">**IIS configuration settings**</span></span>

- <span data-ttu-id="eee91-143">**Maksymalna liczba równoczesnych żądań dla aplikacji**: zwiększenie liczby równoczesnych IIS żądań zwiększy zasoby serwera są dostępne na potrzeby obsługiwania żądań.</span><span class="sxs-lookup"><span data-stu-id="eee91-143">**Max concurrent requests per application**: Increasing the number of concurrent IIS requests will increase server resources available for serving requests.</span></span> <span data-ttu-id="eee91-144">Wartość domyślna to 5000; Aby zwiększyć to ustawienie, wykonaj następujące polecenia w wierszu polecenia z podwyższonym poziomem uprawnień:</span><span class="sxs-lookup"><span data-stu-id="eee91-144">The default value is 5000; to increase this setting, execute the following commands in an elevated command prompt:</span></span>

    [!code-console[Main](signalr-performance/samples/sample4.cmd)]

<span data-ttu-id="eee91-145">**Ustawienia konfiguracji programu ASP.NET**</span><span class="sxs-lookup"><span data-stu-id="eee91-145">**ASP.NET configuration settings**</span></span>

<span data-ttu-id="eee91-146">Ta sekcja zawiera ustawienia konfiguracji, które można ustawić w `aspnet.config` pliku.</span><span class="sxs-lookup"><span data-stu-id="eee91-146">This section includes configuration settings that can be set in the `aspnet.config` file.</span></span> <span data-ttu-id="eee91-147">Ten plik znajduje się w jednej z dwóch lokalizacji, w zależności od platformy:</span><span class="sxs-lookup"><span data-stu-id="eee91-147">This file is found in one of two locations, depending on platform:</span></span>

- `%windir%\Microsoft.NET\Framework\v4.0.30319\aspnet.config`
- `%windir%\Microsoft.NET\Framework64\v4.0.30319\aspnet.config`

<span data-ttu-id="eee91-148">Ustawienia programu ASP.NET, które może poprawić wydajność SignalR są następujące:</span><span class="sxs-lookup"><span data-stu-id="eee91-148">ASP.NET settings that may improve SignalR performance include the following:</span></span>

- <span data-ttu-id="eee91-149">**Maksymalna liczba równoczesnych żądań na procesor CPU,**: zwiększenie tego ustawienia może zlikwidować wąskie gardła wydajności.</span><span class="sxs-lookup"><span data-stu-id="eee91-149">**Maximum concurrent requests per CPU**: Increasing this setting may alleviate performance bottlenecks.</span></span> <span data-ttu-id="eee91-150">Aby zwiększyć to ustawienie, Dodaj następujące ustawienie konfiguracji, aby `aspnet.config` pliku:</span><span class="sxs-lookup"><span data-stu-id="eee91-150">To increase this setting, add the following configuration setting to the `aspnet.config` file:</span></span>

    [!code-xml[Main](signalr-performance/samples/sample5.xml?highlight=4)]
- <span data-ttu-id="eee91-151">**Ograniczenie kolejki żądań**: kiedy łączna liczba połączeń przekroczy `maxConcurrentRequestsPerCPU` ustawienie ASP.NET rozpocznie ograniczanie żądań za pomocą kolejki.</span><span class="sxs-lookup"><span data-stu-id="eee91-151">**Request Queue Limit**: When the total number of connections exceeds the `maxConcurrentRequestsPerCPU` setting, ASP.NET will start throttling requests using a queue.</span></span> <span data-ttu-id="eee91-152">Aby zwiększyć rozmiar kolejki, możesz zwiększyć `requestQueueLimit` ustawienie.</span><span class="sxs-lookup"><span data-stu-id="eee91-152">To increase the size of the queue, you can increase the `requestQueueLimit` setting.</span></span> <span data-ttu-id="eee91-153">Aby to zrobić, Dodaj następujące ustawienie konfiguracji, aby `processModel` w węźle `config/machine.config` (zamiast `aspnet.config`):</span><span class="sxs-lookup"><span data-stu-id="eee91-153">To do this, add the following configuration setting to the `processModel` node in `config/machine.config` (rather than `aspnet.config`):</span></span>

    [!code-xml[Main](signalr-performance/samples/sample6.xml)]

<a id="troubleshooting"></a>

## <a name="troubleshooting-performance-issues"></a><span data-ttu-id="eee91-154">Rozwiązywanie problemów z wydajnością</span><span class="sxs-lookup"><span data-stu-id="eee91-154">Troubleshooting performance issues</span></span>

<span data-ttu-id="eee91-155">W tej sekcji opisano sposoby znajdowania wąskich gardeł wydajności w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="eee91-155">This section describes ways to find performance bottlenecks in your application.</span></span>

### <a name="verifying-that-websocket-is-being-used"></a><span data-ttu-id="eee91-156">Weryfikowanie, używanego protokołu WebSocket</span><span class="sxs-lookup"><span data-stu-id="eee91-156">Verifying that WebSocket is being used</span></span>

<span data-ttu-id="eee91-157">Podczas SignalR można użyć różnych transportu do komunikacji między klientem i serwerem, WebSocket oferuje zalety istotnie poprawiającą wydajność i można używać, jeśli klient i serwer obsługują go.</span><span class="sxs-lookup"><span data-stu-id="eee91-157">While SignalR can use a variety of transports for communication between client and server, WebSocket offers a significant performance advantage, and should be used if the client and server support it.</span></span> <span data-ttu-id="eee91-158">Aby określić, jeśli z klientem i serwerem spełniają wymagania dla protokołu WebSocket, zobacz [transportu i planów awaryjnych](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="eee91-158">To determine if your client and server meet the requirements for WebSocket, see [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span> <span data-ttu-id="eee91-159">Aby ustalić, jakie transportu jest używany w aplikacji, przy użyciu narzędzi deweloperskich w przeglądarce i sprawdź dzienniki, aby zobaczyć, jakie transportu jest używany dla połączenia.</span><span class="sxs-lookup"><span data-stu-id="eee91-159">To determine what transport is being used in your application, you can use the browser developer tools, and examine the logs to see what transport is being used for the connection.</span></span> <span data-ttu-id="eee91-160">Aby uzyskać informacje na temat korzystania z narzędzi programistycznych przeglądarki w programie Internet Explorer i Chrome, zobacz [transportu i planów awaryjnych](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="eee91-160">For information on using the browser development tools in Internet Explorer and Chrome, see [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="perfcounters"></a>

## <a name="using-signalr-performance-counters"></a><span data-ttu-id="eee91-161">Korzystanie z liczników wydajności SignalR</span><span class="sxs-lookup"><span data-stu-id="eee91-161">Using SignalR performance counters</span></span>

<span data-ttu-id="eee91-162">W tej sekcji opisano, jak włączanie i używanie liczników wydajności SignalR, w `Microsoft.AspNet.SignalR.Utils` pakietu.</span><span class="sxs-lookup"><span data-stu-id="eee91-162">This section describes how to enable and use SignalR performance counters, found in the `Microsoft.AspNet.SignalR.Utils` package.</span></span>

### <a name="installing-signalrexe"></a><span data-ttu-id="eee91-163">Instalowanie signalr.exe</span><span class="sxs-lookup"><span data-stu-id="eee91-163">Installing signalr.exe</span></span>

<span data-ttu-id="eee91-164">Liczniki wydajności można dodać do serwera przy użyciu narzędzia o nazwie SignalR.exe.</span><span class="sxs-lookup"><span data-stu-id="eee91-164">Peformance counters can be added to the server using a utility called SignalR.exe.</span></span> <span data-ttu-id="eee91-165">Aby zainstalować to narzędzie, wykonaj następujące kroki:</span><span class="sxs-lookup"><span data-stu-id="eee91-165">To install this utility, follow these steps:</span></span>

1. <span data-ttu-id="eee91-166">W programie Visual Studio, wybierz **narzędzia** > **Menedżera pakietów NuGet** > **Zarządzaj pakietami NuGet dla rozwiązania**</span><span class="sxs-lookup"><span data-stu-id="eee91-166">In Visual Studio, select **Tools** > **NuGet Package Manager** > **Manage NuGet Packages for Solution**</span></span>
2. <span data-ttu-id="eee91-167">Wyszukaj **signalr.utils**i wybierz opcję instalacji.</span><span class="sxs-lookup"><span data-stu-id="eee91-167">Search for **signalr.utils**, and select Install.</span></span>

    ![](signalr-performance/_static/image1.png)
3. <span data-ttu-id="eee91-168">Zaakceptuj Umowę licencyjną, aby zainstalować pakiet.</span><span class="sxs-lookup"><span data-stu-id="eee91-168">Accept the license agreement to install the package.</span></span>
4. <span data-ttu-id="eee91-169">SignalR.exe zostanie zainstalowana tak, aby `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`.</span><span class="sxs-lookup"><span data-stu-id="eee91-169">SignalR.exe will be installed to `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`.</span></span>

### <a name="installing-performance-counters-with-signalrexe"></a><span data-ttu-id="eee91-170">Instalowanie liczników wydajności z SignalR.exe</span><span class="sxs-lookup"><span data-stu-id="eee91-170">Installing performance counters with SignalR.exe</span></span>

<span data-ttu-id="eee91-171">Aby zainstalować liczników wydajności SignalR, uruchom SignalR.exe w wierszu polecenia z podwyższonym poziomem uprawnień, poziomem następujący parametr:</span><span class="sxs-lookup"><span data-stu-id="eee91-171">To install SignalR performance counters, run SignalR.exe in an elevated command prompt with the following parameter:</span></span>

[!code-console[Main](signalr-performance/samples/sample7.cmd)]

<span data-ttu-id="eee91-172">Aby usunąć liczników wydajności SignalR, uruchom SignalR.exe w wierszu polecenia z podwyższonym poziomem uprawnień, poziomem następujący parametr:</span><span class="sxs-lookup"><span data-stu-id="eee91-172">To remove SignalR performance counters, run SignalR.exe in an elevated command prompt with the following parameter:</span></span>

[!code-console[Main](signalr-performance/samples/sample8.cmd)]

### <a name="signalr-performance-counters"></a><span data-ttu-id="eee91-173">Liczniki wydajności SignalR</span><span class="sxs-lookup"><span data-stu-id="eee91-173">SignalR Performance counters</span></span>

<span data-ttu-id="eee91-174">Pakiet narzędzia instaluje następujące liczniki wydajności.</span><span class="sxs-lookup"><span data-stu-id="eee91-174">The utilites package installs the following performance counters.</span></span> <span data-ttu-id="eee91-175">"Całkowita" liczniki pomiaru liczba zdarzeń od czasu ostatniego pula aplikacji lub serwera ponownego uruchomienia.</span><span class="sxs-lookup"><span data-stu-id="eee91-175">The "Total" counters measure the number of events since the last application pool or server restart.</span></span>

<span data-ttu-id="eee91-176">**Metryki połączeń**</span><span class="sxs-lookup"><span data-stu-id="eee91-176">**Connection metrics**</span></span>

<span data-ttu-id="eee91-177">Następujące metryki pomiaru zdarzenia okresu istnienia połączenia, które wystąpiły.</span><span class="sxs-lookup"><span data-stu-id="eee91-177">The following metrics measure the connection lifetime events that occur.</span></span> <span data-ttu-id="eee91-178">Aby uzyskać więcej informacji, zobacz [zrozumienia i obsługa zdarzeń okresu istnienia połączenia](../guide-to-the-api/handling-connection-lifetime-events.md).</span><span class="sxs-lookup"><span data-stu-id="eee91-178">For more information, see [Understanding and Handling Connection Lifetime Events](../guide-to-the-api/handling-connection-lifetime-events.md).</span></span>

- <span data-ttu-id="eee91-179">**Połączeń**</span><span class="sxs-lookup"><span data-stu-id="eee91-179">**Connections Connected**</span></span>
- <span data-ttu-id="eee91-180">**Połączenia zakończone**</span><span class="sxs-lookup"><span data-stu-id="eee91-180">**Connections Reconnected**</span></span>
- <span data-ttu-id="eee91-181">**Disonnected połączeń**</span><span class="sxs-lookup"><span data-stu-id="eee91-181">**Connections Disonnected**</span></span>
- <span data-ttu-id="eee91-182">**Bieżące połączenia**</span><span class="sxs-lookup"><span data-stu-id="eee91-182">**Connections Current**</span></span>

<span data-ttu-id="eee91-183">**Metryki wiadomości**</span><span class="sxs-lookup"><span data-stu-id="eee91-183">**Message metrics**</span></span>

<span data-ttu-id="eee91-184">Następujące metryki pomiaru ruchu komunikat generowany przez SignalR.</span><span class="sxs-lookup"><span data-stu-id="eee91-184">The following metrics measure the message traffic generated by SignalR.</span></span>

- <span data-ttu-id="eee91-185">**Komunikaty o połączenia odebrane łącznie**</span><span class="sxs-lookup"><span data-stu-id="eee91-185">**Connection Messages Received Total**</span></span>
- <span data-ttu-id="eee91-186">**Łączna liczba wysyłane wiadomości połączenia**</span><span class="sxs-lookup"><span data-stu-id="eee91-186">**Connection Messages Sent Total**</span></span>
- <span data-ttu-id="eee91-187">**Połączenie komunikaty odebrane/s**</span><span class="sxs-lookup"><span data-stu-id="eee91-187">**Connection Messages Received/Sec**</span></span>
- <span data-ttu-id="eee91-188">**Połączenie wiadomości wysłane/s**</span><span class="sxs-lookup"><span data-stu-id="eee91-188">**Connection Messages Sent/Sec**</span></span>

<span data-ttu-id="eee91-189">**Komunikatów bus metryki**</span><span class="sxs-lookup"><span data-stu-id="eee91-189">**Message bus metrics**</span></span>

<span data-ttu-id="eee91-190">Następujące metryki pomiaru ruchu za pośrednictwem wewnętrznego magistrali komunikatu SignalR, kolejki, w której są umieszczane wszystkich przychodzących i wychodzących komunikatów SignalR.</span><span class="sxs-lookup"><span data-stu-id="eee91-190">The following metrics measure traffic through the internal SignalR message bus, the queue in which all incoming and outgoing SignalR messages are placed.</span></span> <span data-ttu-id="eee91-191">Komunikat jest **opublikowano** po zakończeniu wysyłania lub emisji.</span><span class="sxs-lookup"><span data-stu-id="eee91-191">A message is **Published** when it is sent or broadcast.</span></span> <span data-ttu-id="eee91-192">A **subskrybenta** w tym kontekście jest subskrypcji na magistrali komunikatu; to powinna być równa liczbie klientów, a także sam serwer.</span><span class="sxs-lookup"><span data-stu-id="eee91-192">A **Subscriber** in this context is a subscription on the message bus; this should equal the number of clients plus the server itself.</span></span> <span data-ttu-id="eee91-193">**Przydzielone procesu roboczego** to składnik, który wysyła dane do aktywnego połączenia; **zajęty, proces roboczy** to taki, który jest aktywnie wysyłania komunikatu.</span><span class="sxs-lookup"><span data-stu-id="eee91-193">An **Allocated Worker** is a component that sends data to active connections; a **Busy Worker** is one that is actively sending a message.</span></span>

- <span data-ttu-id="eee91-194">**Magistrala komunikatów komunikaty odebrane łącznie**</span><span class="sxs-lookup"><span data-stu-id="eee91-194">**Message Bus Messages Received Total**</span></span>
- <span data-ttu-id="eee91-195">**Magistrala komunikatów komunikaty odebrane/s**</span><span class="sxs-lookup"><span data-stu-id="eee91-195">**Message Bus Messages Received/Sec**</span></span>
- <span data-ttu-id="eee91-196">**Komunikatów Bus komunikatów opublikowanych łącznie**</span><span class="sxs-lookup"><span data-stu-id="eee91-196">**Message Bus Messages Published Total**</span></span>
- <span data-ttu-id="eee91-197">**Magistrala komunikatów komunikaty publikowane na sekundę**</span><span class="sxs-lookup"><span data-stu-id="eee91-197">**Message Bus Messages Published/Sec**</span></span>
- <span data-ttu-id="eee91-198">**Bieżąca subskrybentów magistrali komunikatów**</span><span class="sxs-lookup"><span data-stu-id="eee91-198">**Message Bus Subscribers Current**</span></span>
- <span data-ttu-id="eee91-199">**Łączna liczba subskrybentów magistrali komunikatów**</span><span class="sxs-lookup"><span data-stu-id="eee91-199">**Message Bus Subscribers Total**</span></span>
- <span data-ttu-id="eee91-200">**Subskrybentów magistrali komunikatów na sekundę**</span><span class="sxs-lookup"><span data-stu-id="eee91-200">**Message Bus Subscribers/Sec**</span></span>
- <span data-ttu-id="eee91-201">**Magistrala komunikatów przydzielonych pracowników**</span><span class="sxs-lookup"><span data-stu-id="eee91-201">**Message Bus Allocated Workers**</span></span>
- <span data-ttu-id="eee91-202">**Komunikatów Bus zajętych pracowników**</span><span class="sxs-lookup"><span data-stu-id="eee91-202">**Message Bus Busy Workers**</span></span>
- <span data-ttu-id="eee91-203">**Bieżąca tematy magistrali komunikatów**</span><span class="sxs-lookup"><span data-stu-id="eee91-203">**Message Bus Topics Current**</span></span>

<span data-ttu-id="eee91-204">**Błąd metryki**</span><span class="sxs-lookup"><span data-stu-id="eee91-204">**Error metrics**</span></span>

<span data-ttu-id="eee91-205">Następujące metryki pomiaru błędów generowanych przez ruch komunikatów SignalR.</span><span class="sxs-lookup"><span data-stu-id="eee91-205">The following metrics measure errors generated by SignalR message traffic.</span></span> <span data-ttu-id="eee91-206">**Rozpoznawanie Centrum** błędy występują, gdy nie można rozpoznać koncentratora lub metody koncentratora.</span><span class="sxs-lookup"><span data-stu-id="eee91-206">**Hub Resolution** errors occur when a hub or hub method cannot be resolved.</span></span> <span data-ttu-id="eee91-207">**Wywołania koncentratora** błędy są wyjątki generowane podczas wywołania metody koncentratora.</span><span class="sxs-lookup"><span data-stu-id="eee91-207">**Hub Invocation** errors are exceptions thrown while invoking a hub method.</span></span> <span data-ttu-id="eee91-208">**Transport** błędy są błędami połączenia podczas żądania lub odpowiedzi HTTP.</span><span class="sxs-lookup"><span data-stu-id="eee91-208">**Transport** errors are connection errors thrown during an HTTP request or response.</span></span>

- <span data-ttu-id="eee91-209">**Błędów: Suma wszystkich**</span><span class="sxs-lookup"><span data-stu-id="eee91-209">**Errors: All Total**</span></span>
- <span data-ttu-id="eee91-210">**Błędy: All/s**</span><span class="sxs-lookup"><span data-stu-id="eee91-210">**Errors: All/Sec**</span></span>
- <span data-ttu-id="eee91-211">**Błędy: Łączna liczba rozpoznawania koncentratora**</span><span class="sxs-lookup"><span data-stu-id="eee91-211">**Errors: Hub Resolution Total**</span></span>
- <span data-ttu-id="eee91-212">**Błędy: Centrum rozpoznawania/s**</span><span class="sxs-lookup"><span data-stu-id="eee91-212">**Errors: Hub Resolution/Sec**</span></span>
- <span data-ttu-id="eee91-213">**Błędy: Łączna liczba wywołania koncentratora**</span><span class="sxs-lookup"><span data-stu-id="eee91-213">**Errors: Hub Invocation Total**</span></span>
- <span data-ttu-id="eee91-214">**Błędy: Wywołania koncentratora na sekundę**</span><span class="sxs-lookup"><span data-stu-id="eee91-214">**Errors: Hub Invocation/Sec**</span></span>
- <span data-ttu-id="eee91-215">**Błędy: Łączna liczba transportu**</span><span class="sxs-lookup"><span data-stu-id="eee91-215">**Errors: Transport Total**</span></span>
- <span data-ttu-id="eee91-216">**Błędy: Transportu na sekundę**</span><span class="sxs-lookup"><span data-stu-id="eee91-216">**Errors: Transport/Sec**</span></span>

<span data-ttu-id="eee91-217">**Metryki skalowania w poziomie**</span><span class="sxs-lookup"><span data-stu-id="eee91-217">**Scaleout metrics**</span></span>

<span data-ttu-id="eee91-218">Następujące metryki pomiaru ruchu i błędy generowane przez dostawcę skalowania w poziomie.</span><span class="sxs-lookup"><span data-stu-id="eee91-218">The following metrics measure traffic and errors generated by the scaleout provider.</span></span> <span data-ttu-id="eee91-219">A **Stream** w tym kontekście jest jednostki skalowania używanej przez dostawcę skalowania; to jest tabelą, jeśli jest używany program SQL Server, tematu, jeśli jest używana usługa Service Bus i subskrypcji, jeśli jest używany w pamięci podręcznej Redis.</span><span class="sxs-lookup"><span data-stu-id="eee91-219">A **Stream** in this context is a scale unit used by the scaleout provider; this is a table if SQL Server is used, a Topic if Service Bus is used, and a Subscription if Redis is used.</span></span> <span data-ttu-id="eee91-220">Domyślnie jest używany tylko jeden strumień, ale to może być zwiększana za pośrednictwem konfiguracji programu SQL Server i usługi Service Bus.</span><span class="sxs-lookup"><span data-stu-id="eee91-220">By default, only one stream is used, but this can be increased through configuration on SQL Server and Service Bus.</span></span> <span data-ttu-id="eee91-221">A **buforowanie** strumienia to taki, który został wprowadzony w stan; gdy strumień jest w stanie błędu, wszystkie wiadomości wysyłane do systemu backplane zakończy się niepowodzeniem, dopóki nie strumienia nie jest już spowodował błąd.</span><span class="sxs-lookup"><span data-stu-id="eee91-221">A **Buffering** stream is one that has entered a faulted state; when the stream is in the faulted state, all messages sent to the backplane will fail immediately until the stream is no longer faulting.</span></span> <span data-ttu-id="eee91-222">**Długość kolejki wysyłania** jest to liczba komunikatów, które zostały opublikowane, ale nie została jeszcze wysłane.</span><span class="sxs-lookup"><span data-stu-id="eee91-222">The **Send Queue Length** is the number of messages that have been posted but not yet sent.</span></span>

- <span data-ttu-id="eee91-223">**Magistrali komunikatów skalowania w poziomie komunikaty odebrane/s**</span><span class="sxs-lookup"><span data-stu-id="eee91-223">**Scaleout Message Bus Messages Received/Sec**</span></span>
- <span data-ttu-id="eee91-224">**Łączna liczba strumieni skalowania w poziomie**</span><span class="sxs-lookup"><span data-stu-id="eee91-224">**Scaleout Streams Total**</span></span>
- <span data-ttu-id="eee91-225">**Skalowalne strumieni Open**</span><span class="sxs-lookup"><span data-stu-id="eee91-225">**Scaleout Streams Open**</span></span>
- <span data-ttu-id="eee91-226">**Strumienie ze skalowaniem buforowania**</span><span class="sxs-lookup"><span data-stu-id="eee91-226">**Scaleout Streams Buffering**</span></span>
- <span data-ttu-id="eee91-227">**Całkowita liczba błędów skalowania w poziomie**</span><span class="sxs-lookup"><span data-stu-id="eee91-227">**Scaleout Errors Total**</span></span>
- <span data-ttu-id="eee91-228">**Błędów skalowania na sekundę**</span><span class="sxs-lookup"><span data-stu-id="eee91-228">**Scaleout Errors/Sec**</span></span>
- <span data-ttu-id="eee91-229">**Długość kolejki wysyłania skalowania w poziomie**</span><span class="sxs-lookup"><span data-stu-id="eee91-229">**Scaleout Send Queue Length**</span></span>

<span data-ttu-id="eee91-230">Aby uzyskać więcej informacji na temat co te liczniki są pomiaru, zobacz [SignalR — skalowanie w poziomie za pomocą usługi Azure Service Bus](scaleout-with-windows-azure-service-bus.md).</span><span class="sxs-lookup"><span data-stu-id="eee91-230">For more information on what these counters are measuring, see [SignalR Scaleout with Azure Service Bus](scaleout-with-windows-azure-service-bus.md).</span></span>

<a id="othercounters"></a>

## <a name="using-other-performance-counters"></a><span data-ttu-id="eee91-231">Korzystanie z innych liczników wydajności</span><span class="sxs-lookup"><span data-stu-id="eee91-231">Using other performance counters</span></span>

<span data-ttu-id="eee91-232">Następujące liczniki wydajności mogą być też przydatne do monitorowania wydajności aplikacji.</span><span class="sxs-lookup"><span data-stu-id="eee91-232">The following performance counters may also be useful in monitoring your application's performance.</span></span>

<span data-ttu-id="eee91-233">**Pamięć**</span><span class="sxs-lookup"><span data-stu-id="eee91-233">**Memory**</span></span>

- <span data-ttu-id="eee91-234">.NET CLR pamięci liczba bajtów we wszystkich sterty (na potrzeby w3wp)</span><span class="sxs-lookup"><span data-stu-id="eee91-234">.NET CLR Memory# bytes in all Heaps (for w3wp)</span></span>

<span data-ttu-id="eee91-235">**ASP.NET**</span><span class="sxs-lookup"><span data-stu-id="eee91-235">**ASP.NET**</span></span>

- <span data-ttu-id="eee91-236">Bieżąca ASP.NET\Requests</span><span class="sxs-lookup"><span data-stu-id="eee91-236">ASP.NET\Requests Current</span></span>
- <span data-ttu-id="eee91-237">ASP.NET\Queued</span><span class="sxs-lookup"><span data-stu-id="eee91-237">ASP.NET\Queued</span></span>
- <span data-ttu-id="eee91-238">ASP.NET\Rejected</span><span class="sxs-lookup"><span data-stu-id="eee91-238">ASP.NET\Rejected</span></span>

<span data-ttu-id="eee91-239">**CPU**</span><span class="sxs-lookup"><span data-stu-id="eee91-239">**CPU**</span></span>

- <span data-ttu-id="eee91-240">Czas Information\Processor procesora</span><span class="sxs-lookup"><span data-stu-id="eee91-240">Processor Information\Processor Time</span></span>

<span data-ttu-id="eee91-241">**TCP/IP**</span><span class="sxs-lookup"><span data-stu-id="eee91-241">**TCP/IP**</span></span>

- <span data-ttu-id="eee91-242">TCPv6/ustanowionych połączeń</span><span class="sxs-lookup"><span data-stu-id="eee91-242">TCPv6/Connections Established</span></span>
- <span data-ttu-id="eee91-243">TCPv4/ustanowionych połączeń</span><span class="sxs-lookup"><span data-stu-id="eee91-243">TCPv4/Connections Established</span></span>

<span data-ttu-id="eee91-244">**Usługa sieci Web**</span><span class="sxs-lookup"><span data-stu-id="eee91-244">**Web Service**</span></span>

- <span data-ttu-id="eee91-245">Połączenia Service\Current sieci Web</span><span class="sxs-lookup"><span data-stu-id="eee91-245">Web Service\Current Connections</span></span>
- <span data-ttu-id="eee91-246">Połączenia Service\Maximum sieci Web</span><span class="sxs-lookup"><span data-stu-id="eee91-246">Web Service\Maximum Connections</span></span>

<span data-ttu-id="eee91-247">**Wątkowość**</span><span class="sxs-lookup"><span data-stu-id="eee91-247">**Threading**</span></span>

- <span data-ttu-id="eee91-248">.NET CLR LocksAndThreads\# bieżącego logiczne wątków</span><span class="sxs-lookup"><span data-stu-id="eee91-248">.NET CLR LocksAndThreads\# of current logical Threads</span></span>
- <span data-ttu-id="eee91-249">.NET CLR LocksAnd wątków\# bieżącego fizycznych wątków</span><span class="sxs-lookup"><span data-stu-id="eee91-249">.NET CLR LocksAnd Threads\# of current physical Threads</span></span>

<a id="otherresources"></a>

## <a name="other-resources"></a><span data-ttu-id="eee91-250">Inne zasoby</span><span class="sxs-lookup"><span data-stu-id="eee91-250">Other Resources</span></span>

<span data-ttu-id="eee91-251">Aby uzyskać więcej informacji na temat wydajności programu ASP.NET, monitorowania i dostrajania, zobacz następujące tematy:</span><span class="sxs-lookup"><span data-stu-id="eee91-251">For more information on ASP.NET performance monitoring and tuning, see the following topics:</span></span>

- <span data-ttu-id="eee91-252">[Przegląd wydajności platformy ASP.NET](https://msdn.microsoft.com/library/cc668225(v=vs.100).aspx)</span><span class="sxs-lookup"><span data-stu-id="eee91-252">[ASP.NET Performance Overview](https://msdn.microsoft.com/library/cc668225(v=vs.100).aspx)</span></span>
- [<span data-ttu-id="eee91-253">Użycie wątków programu ASP.NET, IIS 7.5, usługi IIS 7.0 i IIS 6.0</span><span class="sxs-lookup"><span data-stu-id="eee91-253">ASP.NET Thread Usage on IIS 7.5, IIS 7.0, and IIS 6.0</span></span>](https://blogs.msdn.com/b/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx)
- [<span data-ttu-id="eee91-254">&lt;applicationPool&gt; — Element (ustawienia internetowe)</span><span class="sxs-lookup"><span data-stu-id="eee91-254">&lt;applicationPool&gt; Element (Web Settings)</span></span>](https://msdn.microsoft.com/library/dd560842.aspx)
