---
uid: signalr/overview/older-versions/signalr-performance
title: SignalR wydajności (SignalR 1.x) | Dokumentacja firmy Microsoft
author: pfletcher
description: SignalR wydajności
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/03/2013
ms.topic: article
ms.assetid: 9594d644-66b6-4223-acdd-23e29a6e4c46
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/signalr-performance
msc.type: authoredcontent
ms.openlocfilehash: bad742af28d6c36bb1b66207c2ba09d140332449
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
ms.locfileid: "28037155"
---
<a name="signalr-performance-signalr-1x"></a><span data-ttu-id="7ae99-103">SignalR wydajności (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="7ae99-103">SignalR Performance (SignalR 1.x)</span></span>
====================
<span data-ttu-id="7ae99-104">przez [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="7ae99-104">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> <span data-ttu-id="7ae99-105">W tym temacie opisano, jak projektować pod kątem pomiaru i zwiększyć wydajność w aplikacji SignalR.</span><span class="sxs-lookup"><span data-stu-id="7ae99-105">This topic describes how to design for, measure, and improve performance in a SignalR application.</span></span>


<span data-ttu-id="7ae99-106">Ostatnie prezentacji na SignalR wydajności i skalowania, zobacz [skalowanie sieci Web w czasie rzeczywistym za pomocą programu ASP.NET SignalR](https://channel9.msdn.com/Events/Build/2013/3-502).</span><span class="sxs-lookup"><span data-stu-id="7ae99-106">For a recent presentation on SignalR performance and scaling, see [Scaling the Real-time Web with ASP.NET SignalR](https://channel9.msdn.com/Events/Build/2013/3-502).</span></span>

<span data-ttu-id="7ae99-107">Ten temat zawiera następujące sekcje:</span><span class="sxs-lookup"><span data-stu-id="7ae99-107">This topic contains the following sections:</span></span>

- [<span data-ttu-id="7ae99-108">Zagadnienia dotyczące projektowania</span><span class="sxs-lookup"><span data-stu-id="7ae99-108">Design considerations</span></span>](#design)
- [<span data-ttu-id="7ae99-109">Dostrajanie wydajności serwera SignalR</span><span class="sxs-lookup"><span data-stu-id="7ae99-109">Tuning your SignalR server for performance</span></span>](#tuning)
- [<span data-ttu-id="7ae99-110">Rozwiązywanie problemów z wydajnością</span><span class="sxs-lookup"><span data-stu-id="7ae99-110">Troubleshooting performance issues</span></span>](#troubleshooting)
- [<span data-ttu-id="7ae99-111">Za pomocą liczników wydajności SignalR</span><span class="sxs-lookup"><span data-stu-id="7ae99-111">Using SignalR performance counters</span></span>](#perfcounters)
- [<span data-ttu-id="7ae99-112">Korzystanie z innych liczników wydajności</span><span class="sxs-lookup"><span data-stu-id="7ae99-112">Using other performance counters</span></span>](#othercounters)
- [<span data-ttu-id="7ae99-113">Inne zasoby</span><span class="sxs-lookup"><span data-stu-id="7ae99-113">Other resources</span></span>](#otherresources)

<a id="design"></a>

## <a name="design-considerations"></a><span data-ttu-id="7ae99-114">Zagadnienia dotyczące projektowania</span><span class="sxs-lookup"><span data-stu-id="7ae99-114">Design considerations</span></span>

<span data-ttu-id="7ae99-115">W tej sekcji opisano wzorców, które można zastosować podczas projektowania aplikacji SignalR, aby upewnić się, że wydajność jest nie utrudniona, generując ruchu sieciowego niepotrzebne.</span><span class="sxs-lookup"><span data-stu-id="7ae99-115">This section describes patterns that can be implemented during the design of a SignalR application, to ensure that performance is not being hindered by generating unnecessary network traffic.</span></span>

### <a name="throttling-message-frequency"></a><span data-ttu-id="7ae99-116">Ograniczanie częstotliwość komunikatów</span><span class="sxs-lookup"><span data-stu-id="7ae99-116">Throttling message frequency</span></span>

<span data-ttu-id="7ae99-117">Nawet w przypadku aplikacji, która wysyła komunikaty wysoka częstotliwość (na przykład w czasie rzeczywistym gry aplikacji) większość aplikacji, nie trzeba więcej niż kilka wiadomości na sekundę.</span><span class="sxs-lookup"><span data-stu-id="7ae99-117">Even in an application that sends out messages at a high frequency (such as a realtime gaming application), most applications don't need to send more than a few messages a second.</span></span> <span data-ttu-id="7ae99-118">Aby zmniejszyć ilość ruchu sieciowego, który generuje każdego klienta, można zaimplementować pętli komunikatów wiadomości nie częściej niż stały szybkość kolejek i wysyła się (to znaczy do określonej liczby wiadomości zostaną wysłane co sekundę w przypadku komunikatów w tym czasie w terval do wysłania).</span><span class="sxs-lookup"><span data-stu-id="7ae99-118">To reduce the amount of traffic that each client generates, a message loop can be implemented that queues and sends out messages no more frequently than a fixed rate (that is, up to a certain number of messages will be sent every second, if there are messages in that time interval to be sent).</span></span> <span data-ttu-id="7ae99-119">Przykładową aplikację, która ogranicza wiadomości do niektórych wartości (od klienta i serwera), zobacz [czasu rzeczywistego wysokiej częstotliwości z SignalR](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="7ae99-119">For a sample application that throttles messages to a certain rate (from both client and server), see [High-Frequency Realtime with SignalR](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).</span></span>

### <a name="reducing-message-size"></a><span data-ttu-id="7ae99-120">Zmniejszenie rozmiaru wiadomości</span><span class="sxs-lookup"><span data-stu-id="7ae99-120">Reducing message size</span></span>

<span data-ttu-id="7ae99-121">Rozmiar obiektów serializowanych, można zmniejszyć rozmiar komunikatu SignalR.</span><span class="sxs-lookup"><span data-stu-id="7ae99-121">You can reduce the size of a SignalR message by reducing the size of your serialized objects.</span></span> <span data-ttu-id="7ae99-122">W kodzie serwera wysyłasz obiekt, który zawiera właściwości, które nie muszą być przekazywane, uniemożliwi te właściwości poddany serializacji przy użyciu `JsonIgnore` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="7ae99-122">In server code, if you're sending an object that contains properties that don't need to be transmitted, prevent those properties from being serialized by using the `JsonIgnore` attribute.</span></span> <span data-ttu-id="7ae99-123">Nazwy właściwości są także przechowywane w komunikacie; nazwy właściwości może zostać skrócony przy użyciu `JsonProperty` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="7ae99-123">The names of properties are also stored in the message; the names of properties can be shortened using the `JsonProperty` attribute.</span></span> <span data-ttu-id="7ae99-124">W poniższym przykładzie kodu pokazano wykluczać właściwość zostały wysłane do klienta oraz skrócić nazwy właściwości:</span><span class="sxs-lookup"><span data-stu-id="7ae99-124">The following code sample demonstrates how to exclude a property from being sent to the client, and how to shorten property names:</span></span>

<span data-ttu-id="7ae99-125">**Kod serwera .NET, który demonstruje atrybutu JsonIgnore wykluczanie danych zostały wysłane do klienta, a atrybut JsonProperty, aby zmniejszyć rozmiar wiadomości**</span><span class="sxs-lookup"><span data-stu-id="7ae99-125">**.NET server code that demonstrates the JsonIgnore attribute to exclude data from being sent to the client, and the JsonProperty attribute to reduce message size**</span></span>

[!code-csharp[Main](signalr-performance/samples/sample1.cs?highlight=5,7,10)]

<span data-ttu-id="7ae99-126">Aby zachować czytelność / utrzymanie kodu klienta, nazwy skróconej właściwości mogą być ponownie zmapowany do personelu przyjaznych nazw po odebraniu komunikatu.</span><span class="sxs-lookup"><span data-stu-id="7ae99-126">In order to retain readability/ maintainability in the client code, the abbreviated property names can be remapped to human-friendly names after the message is received.</span></span> <span data-ttu-id="7ae99-127">W poniższym przykładzie kodu pokazano możliwe sposób ponownego mapowania skrócone nazwy do tych dłużej, definiując kontraktu komunikatu (mapowanie) i przy użyciu `reMap` funkcja dotyczyć kontrakt klasy zoptymalizowane wiadomości:</span><span class="sxs-lookup"><span data-stu-id="7ae99-127">The following code sample demonstrates one possible way of remapping shortened names to longer ones, by defining a message contract (mapping), and using the `reMap` function to apply the contract to the optimized message class:</span></span>

<span data-ttu-id="7ae99-128">**Kod JavaScript po stronie klienta, który ponownie mapuje skrócony nazw właściwości do nazwy zrozumiałą dla użytkownika**</span><span class="sxs-lookup"><span data-stu-id="7ae99-128">**Client-side JavaScript code that remaps shortened property names to human-readable names**</span></span>

[!code-javascript[Main](signalr-performance/samples/sample2.js)]

<span data-ttu-id="7ae99-129">Nazwy mogą skrócony w wiadomościach z klienta do serwera, jak również za pomocą tej samej metody.</span><span class="sxs-lookup"><span data-stu-id="7ae99-129">Names can be shortened in messages from the client to the server as well, using the same method.</span></span>

<span data-ttu-id="7ae99-130">Zmniejszenie zużycia pamięci (to znaczy ilość pamięci użytej wiadomości) komunikatu obiektu mogą także podnieść wydajność.</span><span class="sxs-lookup"><span data-stu-id="7ae99-130">Reducing the memory footprint (that is, the amount of memory used for the message) of the message object can also improve performance.</span></span> <span data-ttu-id="7ae99-131">Na przykład jeśli pełny zakres `int` nie jest potrzebny, `short` lub `byte` może być użyty.</span><span class="sxs-lookup"><span data-stu-id="7ae99-131">For example, if the full range of an `int` is not needed, a `short` or `byte` can be used instead.</span></span>

<span data-ttu-id="7ae99-132">Ponieważ komunikaty są przechowywane w magistrali komunikatów w pamięci serwera, zmniejszenie jego rozmiar wiadomości, można także rozwiązać problemy pamięci serwera.</span><span class="sxs-lookup"><span data-stu-id="7ae99-132">Since messages are stored in the message bus in server memory, reducing the size of messages can also address server memory issues.</span></span>

<a id="tuning"></a>

### <a name="tuning-your-signalr-server-for-performance"></a><span data-ttu-id="7ae99-133">Dostrajanie wydajności serwera SignalR</span><span class="sxs-lookup"><span data-stu-id="7ae99-133">Tuning your SignalR server for performance</span></span>

<span data-ttu-id="7ae99-134">Następujące ustawienia konfiguracji może służyć do dostrojenia serwera w celu poprawy wydajności w aplikacji SignalR.</span><span class="sxs-lookup"><span data-stu-id="7ae99-134">The following configuration settings can be used to tune your server for better performance in a SignalR application.</span></span> <span data-ttu-id="7ae99-135">Aby uzyskać ogólne informacje na temat sposobu zwiększenia wydajności w aplikacji ASP.NET, zobacz [poprawę wydajności ASP.NET](https://msdn.microsoft.com/library/ff647787.aspx).</span><span class="sxs-lookup"><span data-stu-id="7ae99-135">For general information on how to improve performance in an ASP.NET application, see [Improving ASP.NET Performance](https://msdn.microsoft.com/library/ff647787.aspx).</span></span>

<span data-ttu-id="7ae99-136">**Ustawienia konfiguracji SignalR**</span><span class="sxs-lookup"><span data-stu-id="7ae99-136">**SignalR configuration settings**</span></span>

- <span data-ttu-id="7ae99-137">**DefaultMessageBufferSize**: domyślnie SignalR zachowuje 1000 komunikatów w pamięci dla koncentratora dla każdego połączenia.</span><span class="sxs-lookup"><span data-stu-id="7ae99-137">**DefaultMessageBufferSize**: By default, SignalR retains 1000 messages in memory per hub per connection.</span></span> <span data-ttu-id="7ae99-138">Jeśli są używane duże wiadomości, może to spowodować problemy z pamięcią, które mogą być złagodzone przez zmniejszenie tej wartości.</span><span class="sxs-lookup"><span data-stu-id="7ae99-138">If large messages are being used, this may create memory issues which can be alleviated by reducing this value.</span></span> <span data-ttu-id="7ae99-139">To ustawienie można ustawić w `Application_Start` obsługi zdarzeń w aplikacji ASP.NET lub `Configuration` metody klasy początkowej OWIN w siebie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7ae99-139">This setting can be set in the `Application_Start` event handler in an ASP.NET application, or in the `Configuration` method of an OWIN startup class in a self-hosted application.</span></span> <span data-ttu-id="7ae99-140">W poniższym przykładzie pokazano sposób Zmniejsz tę wartość, aby zmniejszyć ilość pamięci zajmowaną przez aplikację, aby zmniejszyć ilość używanej pamięci serwera:</span><span class="sxs-lookup"><span data-stu-id="7ae99-140">The following sample demonstrates how to reduce this value in order to reduce the memory footprint of your application in order to reduce the amount of server memory used:</span></span>

    <span data-ttu-id="7ae99-141">**.NET server kod w pliku Global.asax zmniejszenie domyślny rozmiar buforu komunikatu**</span><span class="sxs-lookup"><span data-stu-id="7ae99-141">**.NET server code in Global.asax for decreasing default message buffer size**</span></span>

    [!code-csharp[Main](signalr-performance/samples/sample3.cs)]

<span data-ttu-id="7ae99-142">**Ustawienia konfiguracji usług IIS**</span><span class="sxs-lookup"><span data-stu-id="7ae99-142">**IIS configuration settings**</span></span>

- <span data-ttu-id="7ae99-143">**Maksymalna liczba jednoczesnych żądań na aplikację**: zwiększenie liczby równoczesnych usługi IIS żądań spowoduje zwiększenie zasobów serwera dostępne dla obsługi żądań.</span><span class="sxs-lookup"><span data-stu-id="7ae99-143">**Max concurrent requests per application**: Increasing the number of concurrent IIS requests will increase server resources available for serving requests.</span></span> <span data-ttu-id="7ae99-144">Wartość domyślna to 5000; Aby zwiększyć to ustawienie, uruchom następujące polecenia w wierszu polecenia z pełnymi uprawnieniami:</span><span class="sxs-lookup"><span data-stu-id="7ae99-144">The default value is 5000; to increase this setting, execute the following commands in an elevated command prompt:</span></span>

    [!code-console[Main](signalr-performance/samples/sample4.cmd)]

<span data-ttu-id="7ae99-145">**Ustawienia konfiguracji programu ASP.NET**</span><span class="sxs-lookup"><span data-stu-id="7ae99-145">**ASP.NET configuration settings**</span></span>

<span data-ttu-id="7ae99-146">Ta sekcja zawiera ustawienia konfiguracji, które można ustawić w `aspnet.config` pliku.</span><span class="sxs-lookup"><span data-stu-id="7ae99-146">This section includes configuration settings that can be set in the `aspnet.config` file.</span></span> <span data-ttu-id="7ae99-147">Ten plik znajduje się w dwóch miejscach, w zależności od platformy:</span><span class="sxs-lookup"><span data-stu-id="7ae99-147">This file is found in one of two locations, depending on platform:</span></span>

- `%windir%\Microsoft.NET\Framework\v4.0.30319\aspnet.config`
- `%windir%\Microsoft.NET\Framework64\v4.0.30319\aspnet.config`

<span data-ttu-id="7ae99-148">Ustawienia programu ASP.NET, które może poprawić wydajność SignalR są następujące:</span><span class="sxs-lookup"><span data-stu-id="7ae99-148">ASP.NET settings that may improve SignalR performance include the following:</span></span>

- <span data-ttu-id="7ae99-149">**Maksymalna liczba równoczesnych żądań dla każdego procesora CPU**: zwiększenie tego ustawienia może zlikwidować wąskie gardła wydajności.</span><span class="sxs-lookup"><span data-stu-id="7ae99-149">**Maximum concurrent requests per CPU**: Increasing this setting may alleviate performance bottlenecks.</span></span> <span data-ttu-id="7ae99-150">Aby zwiększyć to ustawienie, Dodaj następujące ustawienie konfiguracji do `aspnet.config` pliku:</span><span class="sxs-lookup"><span data-stu-id="7ae99-150">To increase this setting, add the following configuration setting to the `aspnet.config` file:</span></span>

    [!code-xml[Main](signalr-performance/samples/sample5.xml?highlight=4)]
- <span data-ttu-id="7ae99-151">**Ograniczenie kolejki żądań**: gdy przekracza całkowitą liczbę połączeń `maxConcurrentRequestsPerCPU` ustawienie ASP.NET rozpocznie ograniczania przy użyciu kolejki żądań.</span><span class="sxs-lookup"><span data-stu-id="7ae99-151">**Request Queue Limit**: When the total number of connections exceeds the `maxConcurrentRequestsPerCPU` setting, ASP.NET will start throttling requests using a queue.</span></span> <span data-ttu-id="7ae99-152">Aby zwiększyć rozmiar kolejki, można zwiększyć `requestQueueLimit` ustawienie.</span><span class="sxs-lookup"><span data-stu-id="7ae99-152">To increase the size of the queue, you can increase the `requestQueueLimit` setting.</span></span> <span data-ttu-id="7ae99-153">Aby to zrobić, Dodaj następujące ustawienie konfiguracji, aby `processModel` w węźle `config/machine.config` (zamiast `aspnet.config`):</span><span class="sxs-lookup"><span data-stu-id="7ae99-153">To do this, add the following configuration setting to the `processModel` node in `config/machine.config` (rather than `aspnet.config`):</span></span>

    [!code-xml[Main](signalr-performance/samples/sample6.xml)]

<a id="troubleshooting"></a>

## <a name="troubleshooting-performance-issues"></a><span data-ttu-id="7ae99-154">Rozwiązywanie problemów z wydajnością</span><span class="sxs-lookup"><span data-stu-id="7ae99-154">Troubleshooting performance issues</span></span>

<span data-ttu-id="7ae99-155">W tej sekcji opisano sposoby znajdowania wąskich gardeł zmniejszających wydajność aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7ae99-155">This section describes ways to find performance bottlenecks in your application.</span></span>

### <a name="verifying-that-websocket-is-being-used"></a><span data-ttu-id="7ae99-156">Weryfikowanie, że używany jest protokół WebSocket</span><span class="sxs-lookup"><span data-stu-id="7ae99-156">Verifying that WebSocket is being used</span></span>

<span data-ttu-id="7ae99-157">Podczas SignalR można używać różnych transportu do komunikacji między klientem a serwerem, WebSocket zapewnia korzyści znaczących wydajności i powinien być używany, jeśli klient i serwer obsługuje.</span><span class="sxs-lookup"><span data-stu-id="7ae99-157">While SignalR can use a variety of transports for communication between client and server, WebSocket offers a significant performance advantage, and should be used if the client and server support it.</span></span> <span data-ttu-id="7ae99-158">Aby określić, jeśli Twoje klienta i serwera spełniają wymagania protokołu WebSocket, zobacz [transportu i przejścia](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="7ae99-158">To determine if your client and server meet the requirements for WebSocket, see [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span> <span data-ttu-id="7ae99-159">Aby ustalić, jakie transportu jest używany w aplikacji, można użyć narzędzi deweloperskich przeglądarki i sprawdź dzienniki, aby zobaczyć, jakie transportu jest używany przez połączenie.</span><span class="sxs-lookup"><span data-stu-id="7ae99-159">To determine what transport is being used in your application, you can use the browser developer tools, and examine the logs to see what transport is being used for the connection.</span></span> <span data-ttu-id="7ae99-160">Aby uzyskać informacje na temat używania narzędzia programistyczne przeglądarki w programie Internet Explorer i Chrome, zobacz [transportu i przejścia](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="7ae99-160">For information on using the browser development tools in Internet Explorer and Chrome, see [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="perfcounters"></a>

## <a name="using-signalr-performance-counters"></a><span data-ttu-id="7ae99-161">Za pomocą liczników wydajności SignalR</span><span class="sxs-lookup"><span data-stu-id="7ae99-161">Using SignalR performance counters</span></span>

<span data-ttu-id="7ae99-162">W tej sekcji opisano, jak włączanie i używanie liczniki wydajności SignalR, w `Microsoft.AspNet.SignalR.Utils` pakietu.</span><span class="sxs-lookup"><span data-stu-id="7ae99-162">This section describes how to enable and use SignalR performance counters, found in the `Microsoft.AspNet.SignalR.Utils` package.</span></span>

### <a name="installing-signalrexe"></a><span data-ttu-id="7ae99-163">Instalowanie signalr.exe</span><span class="sxs-lookup"><span data-stu-id="7ae99-163">Installing signalr.exe</span></span>

<span data-ttu-id="7ae99-164">Na serwerze przy użyciu narzędzia, nazywany SignalR.exe można dodać liczniki analizy.</span><span class="sxs-lookup"><span data-stu-id="7ae99-164">Peformance counters can be added to the server using a utility called SignalR.exe.</span></span> <span data-ttu-id="7ae99-165">Aby zainstalować to narzędzie, wykonaj następujące kroki:</span><span class="sxs-lookup"><span data-stu-id="7ae99-165">To install this utility, follow these steps:</span></span>

1. <span data-ttu-id="7ae99-166">W aplikacji Visual Studio, wybierz **narzędzia**, **Menedżer pakietów biblioteki**, **Zarządzaj pakietami NuGet rozwiązania...**</span><span class="sxs-lookup"><span data-stu-id="7ae99-166">In your Visual Studio application, select **Tools**, **Library Package Manager**, **Manage NuGet Packages for Solution...**</span></span>
2. <span data-ttu-id="7ae99-167">Wyszukaj **signalr.utils**i wybierz opcję instalacji.</span><span class="sxs-lookup"><span data-stu-id="7ae99-167">Search for **signalr.utils**, and select Install.</span></span>

    ![](signalr-performance/_static/image1.png)
3. <span data-ttu-id="7ae99-168">Zaakceptuj Umowę licencyjną, aby zainstalować pakiet.</span><span class="sxs-lookup"><span data-stu-id="7ae99-168">Accept the license agreement to install the package.</span></span>
4. <span data-ttu-id="7ae99-169">Zostanie zainstalowana SignalR.exe `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`.</span><span class="sxs-lookup"><span data-stu-id="7ae99-169">SignalR.exe will be installed to `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`.</span></span>

### <a name="installing-performance-counters-with-signalrexe"></a><span data-ttu-id="7ae99-170">Instalowanie liczników wydajności z SignalR.exe</span><span class="sxs-lookup"><span data-stu-id="7ae99-170">Installing performance counters with SignalR.exe</span></span>

<span data-ttu-id="7ae99-171">Aby zainstalować liczniki wydajności SignalR, uruchom SignalR.exe w wierszu polecenia z podwyższonym poziomem uprawnień, z następującym parametrem:</span><span class="sxs-lookup"><span data-stu-id="7ae99-171">To install SignalR performance counters, run SignalR.exe in an elevated command prompt with the following parameter:</span></span>

[!code-console[Main](signalr-performance/samples/sample7.cmd)]

<span data-ttu-id="7ae99-172">Aby usunąć liczniki wydajności SignalR, uruchom SignalR.exe w wierszu polecenia z podwyższonym poziomem uprawnień, z następującym parametrem:</span><span class="sxs-lookup"><span data-stu-id="7ae99-172">To remove SignalR performance counters, run SignalR.exe in an elevated command prompt with the following parameter:</span></span>

[!code-console[Main](signalr-performance/samples/sample8.cmd)]

### <a name="signalr-performance-counters"></a><span data-ttu-id="7ae99-173">Liczniki wydajności SignalR</span><span class="sxs-lookup"><span data-stu-id="7ae99-173">SignalR Performance counters</span></span>

<span data-ttu-id="7ae99-174">Pakiet utilites instaluje następujące liczniki wydajności.</span><span class="sxs-lookup"><span data-stu-id="7ae99-174">The utilites package installs the following performance counters.</span></span> <span data-ttu-id="7ae99-175">"Całkowita" liczniki mierzenia liczby zdarzeń od czasu ostatniego puli aplikacji lub serwera ponownego uruchomienia.</span><span class="sxs-lookup"><span data-stu-id="7ae99-175">The "Total" counters measure the number of events since the last application pool or server restart.</span></span>

<span data-ttu-id="7ae99-176">**Metryki połączenia**</span><span class="sxs-lookup"><span data-stu-id="7ae99-176">**Connection metrics**</span></span>

<span data-ttu-id="7ae99-177">Następujące metryki pomiaru połączenia okres istnienia zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="7ae99-177">The following metrics measure the connection lifetime events that occur.</span></span> <span data-ttu-id="7ae99-178">Aby uzyskać więcej informacji, zobacz [zrozumienia i obsługi zdarzeń okres istnienia połączenia](../guide-to-the-api/handling-connection-lifetime-events.md).</span><span class="sxs-lookup"><span data-stu-id="7ae99-178">For more information, see [Understanding and Handling Connection Lifetime Events](../guide-to-the-api/handling-connection-lifetime-events.md).</span></span>

- <span data-ttu-id="7ae99-179">**Połączeń**</span><span class="sxs-lookup"><span data-stu-id="7ae99-179">**Connections Connected**</span></span>
- <span data-ttu-id="7ae99-180">**Ponowne łączenie zakończone połączeń**</span><span class="sxs-lookup"><span data-stu-id="7ae99-180">**Connections Reconnected**</span></span>
- <span data-ttu-id="7ae99-181">**Disonnected połączenia**</span><span class="sxs-lookup"><span data-stu-id="7ae99-181">**Connections Disonnected**</span></span>
- <span data-ttu-id="7ae99-182">**Bieżące połączenia**</span><span class="sxs-lookup"><span data-stu-id="7ae99-182">**Connections Current**</span></span>

<span data-ttu-id="7ae99-183">**Metryki wiadomości**</span><span class="sxs-lookup"><span data-stu-id="7ae99-183">**Message metrics**</span></span>

<span data-ttu-id="7ae99-184">Następujące metryki pomiaru ruch wiadomości generowany przez SignalR.</span><span class="sxs-lookup"><span data-stu-id="7ae99-184">The following metrics measure the message traffic generated by SignalR.</span></span>

- <span data-ttu-id="7ae99-185">**Odebrane komunikaty połączenia danych całkowitych**</span><span class="sxs-lookup"><span data-stu-id="7ae99-185">**Connection Messages Received Total**</span></span>
- <span data-ttu-id="7ae99-186">**Całkowita liczba wysyłane komunikaty połączenia**</span><span class="sxs-lookup"><span data-stu-id="7ae99-186">**Connection Messages Sent Total**</span></span>
- <span data-ttu-id="7ae99-187">**Komunikaty połączeń odebrane/s**</span><span class="sxs-lookup"><span data-stu-id="7ae99-187">**Connection Messages Received/Sec**</span></span>
- <span data-ttu-id="7ae99-188">**Połączenie komunikaty wysłane/s**</span><span class="sxs-lookup"><span data-stu-id="7ae99-188">**Connection Messages Sent/Sec**</span></span>

<span data-ttu-id="7ae99-189">**Metryki magistrali komunikatów**</span><span class="sxs-lookup"><span data-stu-id="7ae99-189">**Message bus metrics**</span></span>

<span data-ttu-id="7ae99-190">Następujące metryki pomiaru ruchu za pośrednictwem wewnętrznego magistrali komunikatu SignalR, kolejki, w którym znajdują się wszystkie komunikaty przychodzące i wychodzące SignalR.</span><span class="sxs-lookup"><span data-stu-id="7ae99-190">The following metrics measure traffic through the internal SignalR message bus, the queue in which all incoming and outgoing SignalR messages are placed.</span></span> <span data-ttu-id="7ae99-191">Komunikat jest **opublikowano** po jest wysyłana lub emisji.</span><span class="sxs-lookup"><span data-stu-id="7ae99-191">A message is **Published** when it is sent or broadcast.</span></span> <span data-ttu-id="7ae99-192">A **subskrybenta** w tym kontekście jest subskrypcji na magistrali komunikatu; ta powinna być równa liczbę klientów oraz sam serwer.</span><span class="sxs-lookup"><span data-stu-id="7ae99-192">A **Subscriber** in this context is a subscription on the message bus; this should equal the number of clients plus the server itself.</span></span> <span data-ttu-id="7ae99-193">**Przydzielone procesu roboczego** jest składnikiem, który wysyła dane do aktywnych połączeń; **zajęty procesu roboczego** to taki, który jest aktywnie wysłanie wiadomości.</span><span class="sxs-lookup"><span data-stu-id="7ae99-193">An **Allocated Worker** is a component that sends data to active connections; a **Busy Worker** is one that is actively sending a message.</span></span>

- <span data-ttu-id="7ae99-194">**Całkowita liczba odebranych komunikatów magistrali komunikatów**</span><span class="sxs-lookup"><span data-stu-id="7ae99-194">**Message Bus Messages Received Total**</span></span>
- <span data-ttu-id="7ae99-195">**Magistrala komunikatów wiadomości odebrane/s**</span><span class="sxs-lookup"><span data-stu-id="7ae99-195">**Message Bus Messages Received/Sec**</span></span>
- <span data-ttu-id="7ae99-196">**Komunikatów magistrali komunikatów publikowanych danych całkowitych**</span><span class="sxs-lookup"><span data-stu-id="7ae99-196">**Message Bus Messages Published Total**</span></span>
- <span data-ttu-id="7ae99-197">**Magistrala komunikatu komunikaty opublikowane na sekundę**</span><span class="sxs-lookup"><span data-stu-id="7ae99-197">**Message Bus Messages Published/Sec**</span></span>
- <span data-ttu-id="7ae99-198">**Bieżąca subskrybentów magistrali komunikatów**</span><span class="sxs-lookup"><span data-stu-id="7ae99-198">**Message Bus Subscribers Current**</span></span>
- <span data-ttu-id="7ae99-199">**Łączna liczba subskrybentów magistrali komunikatów**</span><span class="sxs-lookup"><span data-stu-id="7ae99-199">**Message Bus Subscribers Total**</span></span>
- <span data-ttu-id="7ae99-200">**Subskrybentów magistrali komunikatów na sekundę**</span><span class="sxs-lookup"><span data-stu-id="7ae99-200">**Message Bus Subscribers/Sec**</span></span>
- <span data-ttu-id="7ae99-201">**Magistrala komunikatów przydzielonych pracowników**</span><span class="sxs-lookup"><span data-stu-id="7ae99-201">**Message Bus Allocated Workers**</span></span>
- <span data-ttu-id="7ae99-202">**Zajętych pracowników magistrali komunikatów**</span><span class="sxs-lookup"><span data-stu-id="7ae99-202">**Message Bus Busy Workers**</span></span>
- <span data-ttu-id="7ae99-203">**Bieżąca tematy magistrali komunikatów**</span><span class="sxs-lookup"><span data-stu-id="7ae99-203">**Message Bus Topics Current**</span></span>

<span data-ttu-id="7ae99-204">**Błąd metryk**</span><span class="sxs-lookup"><span data-stu-id="7ae99-204">**Error metrics**</span></span>

<span data-ttu-id="7ae99-205">Następujące metryki pomiaru błędy generowane przez ruch komunikatów SignalR.</span><span class="sxs-lookup"><span data-stu-id="7ae99-205">The following metrics measure errors generated by SignalR message traffic.</span></span> <span data-ttu-id="7ae99-206">**Rozpoznawania koncentratora** błędy występują, gdy nie można ustalić koncentratora lub metody koncentratora.</span><span class="sxs-lookup"><span data-stu-id="7ae99-206">**Hub Resolution** errors occur when a hub or hub method cannot be resolved.</span></span> <span data-ttu-id="7ae99-207">**Wywołania koncentratora** błędy są wyjątki zgłaszane podczas wywoływania metody koncentratora.</span><span class="sxs-lookup"><span data-stu-id="7ae99-207">**Hub Invocation** errors are exceptions thrown while invoking a hub method.</span></span> <span data-ttu-id="7ae99-208">**Transport** wyjątek podczas żądania lub odpowiedzi HTTP błędów połączeń są błędy.</span><span class="sxs-lookup"><span data-stu-id="7ae99-208">**Transport** errors are connection errors thrown during an HTTP request or response.</span></span>

- <span data-ttu-id="7ae99-209">**Błędów: Suma wszystkich**</span><span class="sxs-lookup"><span data-stu-id="7ae99-209">**Errors: All Total**</span></span>
- <span data-ttu-id="7ae99-210">**Błędy: All/s**</span><span class="sxs-lookup"><span data-stu-id="7ae99-210">**Errors: All/Sec**</span></span>
- <span data-ttu-id="7ae99-211">**Błędy: Całkowita liczba rozpoznawania koncentratora**</span><span class="sxs-lookup"><span data-stu-id="7ae99-211">**Errors: Hub Resolution Total**</span></span>
- <span data-ttu-id="7ae99-212">**Błędy: Rozpoznawania koncentratora na sekundę**</span><span class="sxs-lookup"><span data-stu-id="7ae99-212">**Errors: Hub Resolution/Sec**</span></span>
- <span data-ttu-id="7ae99-213">**Błędy: Całkowita liczba wywołania koncentratora**</span><span class="sxs-lookup"><span data-stu-id="7ae99-213">**Errors: Hub Invocation Total**</span></span>
- <span data-ttu-id="7ae99-214">**Błędy: Wywołania koncentratora na sekundę**</span><span class="sxs-lookup"><span data-stu-id="7ae99-214">**Errors: Hub Invocation/Sec**</span></span>
- <span data-ttu-id="7ae99-215">**Błędy: Całkowita liczba transportu**</span><span class="sxs-lookup"><span data-stu-id="7ae99-215">**Errors: Transport Total**</span></span>
- <span data-ttu-id="7ae99-216">**Błędy: Transportu na sekundę**</span><span class="sxs-lookup"><span data-stu-id="7ae99-216">**Errors: Transport/Sec**</span></span>

<span data-ttu-id="7ae99-217">**Metryki skalowania**</span><span class="sxs-lookup"><span data-stu-id="7ae99-217">**Scaleout metrics**</span></span>

<span data-ttu-id="7ae99-218">Następujące metryki pomiaru ruchu i błędów wygenerowanych przez dostawcę skalowania w poziomie.</span><span class="sxs-lookup"><span data-stu-id="7ae99-218">The following metrics measure traffic and errors generated by the scaleout provider.</span></span> <span data-ttu-id="7ae99-219">A **strumienia** w tym kontekście jest jednostką skalowania jest używany przez dostawcę skalowania, co jest tabeli, jeśli jest używany program SQL Server, tematu, jeśli jest używana usługa Service Bus i subskrypcji, użycie pamięci podręcznej Redis.</span><span class="sxs-lookup"><span data-stu-id="7ae99-219">A **Stream** in this context is a scale unit used by the scaleout provider; this is a table if SQL Server is used, a Topic if Service Bus is used, and a Subscription if Redis is used.</span></span> <span data-ttu-id="7ae99-220">Domyślnie jest używane tylko jednego strumienia, ale to można zwiększyć za pomocą konfiguracji na serwerze SQL i usługi Service Bus.</span><span class="sxs-lookup"><span data-stu-id="7ae99-220">By default, only one stream is used, but this can be increased through configuration on SQL Server and Service Bus.</span></span> <span data-ttu-id="7ae99-221">A **buforowanie** strumienia, który przeszedł w stan; gdy strumień jest w stanie błędnej, wszystkie komunikaty wysyłane na płytę montażową zakończy się niepowodzeniem aż do strumienia nie spowodował błąd.</span><span class="sxs-lookup"><span data-stu-id="7ae99-221">A **Buffering** stream is one that has entered a faulted state; when the stream is in the faulted state, all messages sent to the backplane will fail immediately until the stream is no longer faulting.</span></span> <span data-ttu-id="7ae99-222">**Długość kolejki wysyłania** jest liczba komunikatów, które zostały przesłane, ale nie zostały jeszcze wysłane.</span><span class="sxs-lookup"><span data-stu-id="7ae99-222">The **Send Queue Length** is the number of messages that have been posted but not yet sent.</span></span>

- <span data-ttu-id="7ae99-223">**Magistrali komunikatów skalowania wiadomości odebrane/s**</span><span class="sxs-lookup"><span data-stu-id="7ae99-223">**Scaleout Message Bus Messages Received/Sec**</span></span>
- <span data-ttu-id="7ae99-224">**Całkowita liczba strumieni skalowania w poziomie**</span><span class="sxs-lookup"><span data-stu-id="7ae99-224">**Scaleout Streams Total**</span></span>
- <span data-ttu-id="7ae99-225">**Otwórz strumieni skalowania w poziomie**</span><span class="sxs-lookup"><span data-stu-id="7ae99-225">**Scaleout Streams Open**</span></span>
- <span data-ttu-id="7ae99-226">**Strumienie skalowania buforowania**</span><span class="sxs-lookup"><span data-stu-id="7ae99-226">**Scaleout Streams Buffering**</span></span>
- <span data-ttu-id="7ae99-227">**Całkowita liczba błędów skalowania w poziomie**</span><span class="sxs-lookup"><span data-stu-id="7ae99-227">**Scaleout Errors Total**</span></span>
- <span data-ttu-id="7ae99-228">**Błędów skalowania na sekundę**</span><span class="sxs-lookup"><span data-stu-id="7ae99-228">**Scaleout Errors/Sec**</span></span>
- <span data-ttu-id="7ae99-229">**Długość kolejki wysyłania skalowania w poziomie**</span><span class="sxs-lookup"><span data-stu-id="7ae99-229">**Scaleout Send Queue Length**</span></span>

<span data-ttu-id="7ae99-230">Aby uzyskać więcej informacji na jakie te liczniki są pomiaru, zobacz [skalowania SignalR z usługi Azure Service Bus](scaleout-with-windows-azure-service-bus.md).</span><span class="sxs-lookup"><span data-stu-id="7ae99-230">For more information on what these counters are measuring, see [SignalR Scaleout with Azure Service Bus](scaleout-with-windows-azure-service-bus.md).</span></span>

<a id="othercounters"></a>

## <a name="using-other-performance-counters"></a><span data-ttu-id="7ae99-231">Korzystanie z innych liczników wydajności</span><span class="sxs-lookup"><span data-stu-id="7ae99-231">Using other performance counters</span></span>

<span data-ttu-id="7ae99-232">Następujące liczniki wydajności mogą być również przydatne w ramach monitorowania wydajności aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7ae99-232">The following performance counters may also be useful in monitoring your application's performance.</span></span>

<span data-ttu-id="7ae99-233">**Pamięci**</span><span class="sxs-lookup"><span data-stu-id="7ae99-233">**Memory**</span></span>

- <span data-ttu-id="7ae99-234">.NET CLR pamięci bajtów we wszystkich sterty (dla w3wp)</span><span class="sxs-lookup"><span data-stu-id="7ae99-234">.NET CLR Memory# bytes in all Heaps (for w3wp)</span></span>

<span data-ttu-id="7ae99-235">**ASP.NET**</span><span class="sxs-lookup"><span data-stu-id="7ae99-235">**ASP.NET**</span></span>

- <span data-ttu-id="7ae99-236">Bieżąca ASP.NET\Requests</span><span class="sxs-lookup"><span data-stu-id="7ae99-236">ASP.NET\Requests Current</span></span>
- <span data-ttu-id="7ae99-237">ASP.NET\Queued</span><span class="sxs-lookup"><span data-stu-id="7ae99-237">ASP.NET\Queued</span></span>
- <span data-ttu-id="7ae99-238">ASP.NET\Rejected</span><span class="sxs-lookup"><span data-stu-id="7ae99-238">ASP.NET\Rejected</span></span>

<span data-ttu-id="7ae99-239">**CPU**</span><span class="sxs-lookup"><span data-stu-id="7ae99-239">**CPU**</span></span>

- <span data-ttu-id="7ae99-240">Czas procesora Information\Processor</span><span class="sxs-lookup"><span data-stu-id="7ae99-240">Processor Information\Processor Time</span></span>

<span data-ttu-id="7ae99-241">**TCP/IP**</span><span class="sxs-lookup"><span data-stu-id="7ae99-241">**TCP/IP**</span></span>

- <span data-ttu-id="7ae99-242">TCPv6/połączeń</span><span class="sxs-lookup"><span data-stu-id="7ae99-242">TCPv6/Connections Established</span></span>
- <span data-ttu-id="7ae99-243">TCPv4/połączeń</span><span class="sxs-lookup"><span data-stu-id="7ae99-243">TCPv4/Connections Established</span></span>

<span data-ttu-id="7ae99-244">**Usługi sieci Web**</span><span class="sxs-lookup"><span data-stu-id="7ae99-244">**Web Service**</span></span>

- <span data-ttu-id="7ae99-245">Połączenia Service\Current sieci Web</span><span class="sxs-lookup"><span data-stu-id="7ae99-245">Web Service\Current Connections</span></span>
- <span data-ttu-id="7ae99-246">Połączenia Service\Maximum sieci Web</span><span class="sxs-lookup"><span data-stu-id="7ae99-246">Web Service\Maximum Connections</span></span>

<span data-ttu-id="7ae99-247">**Wątkowość**</span><span class="sxs-lookup"><span data-stu-id="7ae99-247">**Threading**</span></span>

- <span data-ttu-id="7ae99-248">.NET CLR LocksAndThreads\# aktualna liczba wątków logicznych</span><span class="sxs-lookup"><span data-stu-id="7ae99-248">.NET CLR LocksAndThreads\# of current logical Threads</span></span>
- <span data-ttu-id="7ae99-249">.NET CLR LocksAnd wątków\# aktualna liczba wątków fizycznych</span><span class="sxs-lookup"><span data-stu-id="7ae99-249">.NET CLR LocksAnd Threads\# of current physical Threads</span></span>

<a id="otherresources"></a>

## <a name="other-resources"></a><span data-ttu-id="7ae99-250">Inne zasoby</span><span class="sxs-lookup"><span data-stu-id="7ae99-250">Other Resources</span></span>

<span data-ttu-id="7ae99-251">Aby uzyskać więcej informacji na temat wydajności programu ASP.NET, monitorowania i dostrajania, zobacz następujące tematy:</span><span class="sxs-lookup"><span data-stu-id="7ae99-251">For more information on ASP.NET performance monitoring and tuning, see the following topics:</span></span>

- <span data-ttu-id="7ae99-252">[Omówienie wydajności programu ASP.NET](https://msdn.microsoft.com/library/cc668225(v=vs.100).aspx)</span><span class="sxs-lookup"><span data-stu-id="7ae99-252">[ASP.NET Performance Overview](https://msdn.microsoft.com/library/cc668225(v=vs.100).aspx)</span></span>
- [<span data-ttu-id="7ae99-253">Użycie wątku ASP.NET usług IIS 7.5, usługi IIS 7.0 i IIS 6.0</span><span class="sxs-lookup"><span data-stu-id="7ae99-253">ASP.NET Thread Usage on IIS 7.5, IIS 7.0, and IIS 6.0</span></span>](https://blogs.msdn.com/b/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx)
- [<span data-ttu-id="7ae99-254">&lt;applicationPool&gt; elementu (ustawienia sieci Web)</span><span class="sxs-lookup"><span data-stu-id="7ae99-254">&lt;applicationPool&gt; Element (Web Settings)</span></span>](https://msdn.microsoft.com/library/dd560842.aspx)
