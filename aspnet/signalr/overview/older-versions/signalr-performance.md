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
<a name="signalr-performance-signalr-1x"></a>Wydajność SignalR (SignalR 1.x)
====================
przez [Patrick Fletcher](https://github.com/pfletcher)

> W tym temacie opisano sposób projektowania, miar i zwiększenie wydajności w aplikacji SignalR.


Ostatnie prezentacji na wydajność SignalR i skalowania, zobacz [skalowanie sieci Web w czasie rzeczywistym z użyciem ASP.NET SignalR](https://channel9.msdn.com/Events/Build/2013/3-502).

Ten temat zawiera następujące sekcje:

- [Zagadnienia dotyczące projektowania](#design)
- [Dostrajanie wydajności serwera SignalR](#tuning)
- [Rozwiązywanie problemów z wydajnością](#troubleshooting)
- [Korzystanie z liczników wydajności SignalR](#perfcounters)
- [Korzystanie z innych liczników wydajności](#othercounters)
- [Inne zasoby](#otherresources)

<a id="design"></a>

## <a name="design-considerations"></a>Zagadnienia dotyczące projektowania

W tej sekcji opisano wzorców, które można zastosować podczas projektowania aplikacji SignalR, aby upewnić się, że wydajność jest nie utrudniona, generując niepotrzebny ruch sieciowy.

### <a name="throttling-message-frequency"></a>Ograniczanie częstotliwości wiadomości

Nawet w przypadku aplikacji, która wysyła komunikaty o wysokiej częstotliwości (np. aplikacji gier w języku czasu rzeczywistego) większość aplikacji nie trzeba wysłać więcej niż kilka komunikatów na sekundę. Aby zmniejszyć ilość ruchu sieciowego, który generuje każdy klient, można zaimplementować pętli komunikatów, kolejki i wysyła się komunikaty nie częściej niż stałą stawkę (czyli maksymalna liczba wiadomości będą wysyłane co sekundę w przypadku komunikatów w tym czasie w nterwał do wysłania). Dla przykładowej aplikacji, która ogranicza wiadomości do niektórych współczynnik (od klienta i serwera), zobacz [Realtime o wysokiej częstotliwości za pomocą biblioteki SignalR](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).

### <a name="reducing-message-size"></a>Zmniejszenie rozmiaru wiadomości

Aby zmniejszyć rozmiar komunikatu SignalR, należy zmniejszyć rozmiar serializacji obiektów. W kodzie serwera, jest wysyłana obiekt, który zawiera właściwości, które nie muszą być przekazywane, uniemożliwi te właściwości serializowanego przy użyciu `JsonIgnore` atrybutu. Nazwy właściwości są także przechowywane informacje w wiadomości; nazwy właściwości może zostać skrócony przy użyciu `JsonProperty` atrybutu. W poniższym przykładzie kodu pokazano sposób wykluczać właściwość zostały wysłane do klienta oraz skrócić nazwy właściwości:

**Kod serwera platformy .NET, który demonstruje, atrybut JsonIgnore wykluczanie danych zostały wysłane do klienta, a atrybut JsonProperty, aby zmniejszyć rozmiar komunikatu**

[!code-csharp[Main](signalr-performance/samples/sample1.cs?highlight=5,7,10)]

Aby zachować czytelność / łatwości utrzymania kodu klienta, nazwy skróconej właściwości mogą być ponownie zmapowany do przyjaznego dla człowieka nazwami po otrzymaniu komunikatu. W poniższym przykładzie kodu pokazano jeden sposób możliwe ponowne mapowanie skrócone nazwy tymi dłużej, definiując kontraktu komunikatu (mapowanie) i za pomocą `reMap` funkcja do stosowania Umowy do klasy zoptymalizowane wiadomości:

**Kod JavaScript po stronie klienta, który ponownie mapuje skrócone nazwy właściwości na nazwy czytelny dla człowieka**

[!code-javascript[Main](signalr-performance/samples/sample2.js)]

Nazwy mogą skrócony w wiadomościach od klienta do serwera, jak również za pomocą tej samej metody.

Zmniejszenie zużycia pamięci (czyli ilości pamięci używanej dla wiadomości) komunikatu obiektu może również zwiększyć wydajność. Przykładowo jeśli pełną gamę `int` nie jest potrzebna, `short` lub `byte` mogą być używane zamiast tego.

Ponieważ komunikaty są przechowywane w magistrali komunikatów w pamięci serwera, zmniejszenie rozmiaru wiadomości, można także rozwiązać problemy z z pamięci serwera.

<a id="tuning"></a>

### <a name="tuning-your-signalr-server-for-performance"></a>Dostrajanie wydajności serwera SignalR

Następujące ustawienia konfiguracji można Dostrajanie Serwer w celu zapewnienia lepszej wydajności w aplikacji SignalR. Aby uzyskać ogólne informacje na temat sposobu zwiększenia wydajności w aplikacji ASP.NET, zobacz [poprawę wydajności ASP.NET](https://msdn.microsoft.com/library/ff647787.aspx).

**Ustawienia konfiguracji SignalR**

- **DefaultMessageBufferSize**: domyślnie SignalR zachowuje 1000 komunikatów w pamięci na Centrum połączenia. Jeśli używane są duże wiadomości, że może to spowodować problemy z pamięcią, które mogą być złagodzone przez zmniejszenie tej wartości. To ustawienie można ustawić w `Application_Start` programu obsługi zdarzeń w aplikacji ASP.NET lub `Configuration` metody klasy początkowej OWIN w samodzielnie hostowanej aplikacji. W poniższym przykładzie pokazano, jak zmniejszyć tę wartość, aby zmniejszyć ilość pamięci zajmowaną przez aplikację, aby zmniejszyć ilość używanej pamięci serwera:

    **Kod serwera .NET w pliku Global.asax dla malejącej domyślny rozmiar buforu komunikatu**

    [!code-csharp[Main](signalr-performance/samples/sample3.cs)]

**Ustawienia konfiguracji usług IIS**

- **Maksymalna liczba równoczesnych żądań dla aplikacji**: zwiększenie liczby równoczesnych IIS żądań zwiększy zasoby serwera są dostępne na potrzeby obsługiwania żądań. Wartość domyślna to 5000; Aby zwiększyć to ustawienie, wykonaj następujące polecenia w wierszu polecenia z podwyższonym poziomem uprawnień:

    [!code-console[Main](signalr-performance/samples/sample4.cmd)]

**Ustawienia konfiguracji programu ASP.NET**

Ta sekcja zawiera ustawienia konfiguracji, które można ustawić w `aspnet.config` pliku. Ten plik znajduje się w jednej z dwóch lokalizacji, w zależności od platformy:

- `%windir%\Microsoft.NET\Framework\v4.0.30319\aspnet.config`
- `%windir%\Microsoft.NET\Framework64\v4.0.30319\aspnet.config`

Ustawienia programu ASP.NET, które może poprawić wydajność SignalR są następujące:

- **Maksymalna liczba równoczesnych żądań na procesor CPU,**: zwiększenie tego ustawienia może zlikwidować wąskie gardła wydajności. Aby zwiększyć to ustawienie, Dodaj następujące ustawienie konfiguracji, aby `aspnet.config` pliku:

    [!code-xml[Main](signalr-performance/samples/sample5.xml?highlight=4)]
- **Ograniczenie kolejki żądań**: kiedy łączna liczba połączeń przekroczy `maxConcurrentRequestsPerCPU` ustawienie ASP.NET rozpocznie ograniczanie żądań za pomocą kolejki. Aby zwiększyć rozmiar kolejki, możesz zwiększyć `requestQueueLimit` ustawienie. Aby to zrobić, Dodaj następujące ustawienie konfiguracji, aby `processModel` w węźle `config/machine.config` (zamiast `aspnet.config`):

    [!code-xml[Main](signalr-performance/samples/sample6.xml)]

<a id="troubleshooting"></a>

## <a name="troubleshooting-performance-issues"></a>Rozwiązywanie problemów z wydajnością

W tej sekcji opisano sposoby znajdowania wąskich gardeł wydajności w aplikacji.

### <a name="verifying-that-websocket-is-being-used"></a>Weryfikowanie, używanego protokołu WebSocket

Podczas SignalR można użyć różnych transportu do komunikacji między klientem i serwerem, WebSocket oferuje zalety istotnie poprawiającą wydajność i można używać, jeśli klient i serwer obsługują go. Aby określić, jeśli z klientem i serwerem spełniają wymagania dla protokołu WebSocket, zobacz [transportu i planów awaryjnych](../getting-started/introduction-to-signalr.md#transports). Aby ustalić, jakie transportu jest używany w aplikacji, przy użyciu narzędzi deweloperskich w przeglądarce i sprawdź dzienniki, aby zobaczyć, jakie transportu jest używany dla połączenia. Aby uzyskać informacje na temat korzystania z narzędzi programistycznych przeglądarki w programie Internet Explorer i Chrome, zobacz [transportu i planów awaryjnych](../getting-started/introduction-to-signalr.md#transports).

<a id="perfcounters"></a>

## <a name="using-signalr-performance-counters"></a>Korzystanie z liczników wydajności SignalR

W tej sekcji opisano, jak włączanie i używanie liczników wydajności SignalR, w `Microsoft.AspNet.SignalR.Utils` pakietu.

### <a name="installing-signalrexe"></a>Instalowanie signalr.exe

Liczniki wydajności można dodać do serwera przy użyciu narzędzia o nazwie SignalR.exe. Aby zainstalować to narzędzie, wykonaj następujące kroki:

1. W programie Visual Studio, wybierz **narzędzia** > **Menedżera pakietów NuGet** > **Zarządzaj pakietami NuGet dla rozwiązania**
2. Wyszukaj **signalr.utils**i wybierz opcję instalacji.

    ![](signalr-performance/_static/image1.png)
3. Zaakceptuj Umowę licencyjną, aby zainstalować pakiet.
4. SignalR.exe zostanie zainstalowana tak, aby `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`.

### <a name="installing-performance-counters-with-signalrexe"></a>Instalowanie liczników wydajności z SignalR.exe

Aby zainstalować liczników wydajności SignalR, uruchom SignalR.exe w wierszu polecenia z podwyższonym poziomem uprawnień, poziomem następujący parametr:

[!code-console[Main](signalr-performance/samples/sample7.cmd)]

Aby usunąć liczników wydajności SignalR, uruchom SignalR.exe w wierszu polecenia z podwyższonym poziomem uprawnień, poziomem następujący parametr:

[!code-console[Main](signalr-performance/samples/sample8.cmd)]

### <a name="signalr-performance-counters"></a>Liczniki wydajności SignalR

Pakiet narzędzia instaluje następujące liczniki wydajności. "Całkowita" liczniki pomiaru liczba zdarzeń od czasu ostatniego pula aplikacji lub serwera ponownego uruchomienia.

**Metryki połączeń**

Następujące metryki pomiaru zdarzenia okresu istnienia połączenia, które wystąpiły. Aby uzyskać więcej informacji, zobacz [zrozumienia i obsługa zdarzeń okresu istnienia połączenia](../guide-to-the-api/handling-connection-lifetime-events.md).

- **Połączeń**
- **Połączenia zakończone**
- **Disonnected połączeń**
- **Bieżące połączenia**

**Metryki wiadomości**

Następujące metryki pomiaru ruchu komunikat generowany przez SignalR.

- **Komunikaty o połączenia odebrane łącznie**
- **Łączna liczba wysyłane wiadomości połączenia**
- **Połączenie komunikaty odebrane/s**
- **Połączenie wiadomości wysłane/s**

**Komunikatów bus metryki**

Następujące metryki pomiaru ruchu za pośrednictwem wewnętrznego magistrali komunikatu SignalR, kolejki, w której są umieszczane wszystkich przychodzących i wychodzących komunikatów SignalR. Komunikat jest **opublikowano** po zakończeniu wysyłania lub emisji. A **subskrybenta** w tym kontekście jest subskrypcji na magistrali komunikatu; to powinna być równa liczbie klientów, a także sam serwer. **Przydzielone procesu roboczego** to składnik, który wysyła dane do aktywnego połączenia; **zajęty, proces roboczy** to taki, który jest aktywnie wysyłania komunikatu.

- **Magistrala komunikatów komunikaty odebrane łącznie**
- **Magistrala komunikatów komunikaty odebrane/s**
- **Komunikatów Bus komunikatów opublikowanych łącznie**
- **Magistrala komunikatów komunikaty publikowane na sekundę**
- **Bieżąca subskrybentów magistrali komunikatów**
- **Łączna liczba subskrybentów magistrali komunikatów**
- **Subskrybentów magistrali komunikatów na sekundę**
- **Magistrala komunikatów przydzielonych pracowników**
- **Komunikatów Bus zajętych pracowników**
- **Bieżąca tematy magistrali komunikatów**

**Błąd metryki**

Następujące metryki pomiaru błędów generowanych przez ruch komunikatów SignalR. **Rozpoznawanie Centrum** błędy występują, gdy nie można rozpoznać koncentratora lub metody koncentratora. **Wywołania koncentratora** błędy są wyjątki generowane podczas wywołania metody koncentratora. **Transport** błędy są błędami połączenia podczas żądania lub odpowiedzi HTTP.

- **Błędów: Suma wszystkich**
- **Błędy: All/s**
- **Błędy: Łączna liczba rozpoznawania koncentratora**
- **Błędy: Centrum rozpoznawania/s**
- **Błędy: Łączna liczba wywołania koncentratora**
- **Błędy: Wywołania koncentratora na sekundę**
- **Błędy: Łączna liczba transportu**
- **Błędy: Transportu na sekundę**

**Metryki skalowania w poziomie**

Następujące metryki pomiaru ruchu i błędy generowane przez dostawcę skalowania w poziomie. A **Stream** w tym kontekście jest jednostki skalowania używanej przez dostawcę skalowania; to jest tabelą, jeśli jest używany program SQL Server, tematu, jeśli jest używana usługa Service Bus i subskrypcji, jeśli jest używany w pamięci podręcznej Redis. Domyślnie jest używany tylko jeden strumień, ale to może być zwiększana za pośrednictwem konfiguracji programu SQL Server i usługi Service Bus. A **buforowanie** strumienia to taki, który został wprowadzony w stan; gdy strumień jest w stanie błędu, wszystkie wiadomości wysyłane do systemu backplane zakończy się niepowodzeniem, dopóki nie strumienia nie jest już spowodował błąd. **Długość kolejki wysyłania** jest to liczba komunikatów, które zostały opublikowane, ale nie została jeszcze wysłane.

- **Magistrali komunikatów skalowania w poziomie komunikaty odebrane/s**
- **Łączna liczba strumieni skalowania w poziomie**
- **Skalowalne strumieni Open**
- **Strumienie ze skalowaniem buforowania**
- **Całkowita liczba błędów skalowania w poziomie**
- **Błędów skalowania na sekundę**
- **Długość kolejki wysyłania skalowania w poziomie**

Aby uzyskać więcej informacji na temat co te liczniki są pomiaru, zobacz [SignalR — skalowanie w poziomie za pomocą usługi Azure Service Bus](scaleout-with-windows-azure-service-bus.md).

<a id="othercounters"></a>

## <a name="using-other-performance-counters"></a>Korzystanie z innych liczników wydajności

Następujące liczniki wydajności mogą być też przydatne do monitorowania wydajności aplikacji.

**Pamięć**

- .NET CLR pamięci liczba bajtów we wszystkich sterty (na potrzeby w3wp)

**ASP.NET**

- Bieżąca ASP.NET\Requests
- ASP.NET\Queued
- ASP.NET\Rejected

**CPU**

- Czas Information\Processor procesora

**TCP/IP**

- TCPv6/ustanowionych połączeń
- TCPv4/ustanowionych połączeń

**Usługa sieci Web**

- Połączenia Service\Current sieci Web
- Połączenia Service\Maximum sieci Web

**Wątkowość**

- .NET CLR LocksAndThreads\# bieżącego logiczne wątków
- .NET CLR LocksAnd wątków\# bieżącego fizycznych wątków

<a id="otherresources"></a>

## <a name="other-resources"></a>Inne zasoby

Aby uzyskać więcej informacji na temat wydajności programu ASP.NET, monitorowania i dostrajania, zobacz następujące tematy:

- [Przegląd wydajności platformy ASP.NET](https://msdn.microsoft.com/library/cc668225(v=vs.100).aspx)
- [Użycie wątków programu ASP.NET, IIS 7.5, usługi IIS 7.0 i IIS 6.0](https://blogs.msdn.com/b/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx)
- [&lt;applicationPool&gt; — Element (ustawienia internetowe)](https://msdn.microsoft.com/library/dd560842.aspx)
