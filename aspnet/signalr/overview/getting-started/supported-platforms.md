---
uid: signalr/overview/getting-started/supported-platforms
title: Obsługiwane platformy | Dokumentacja firmy Microsoft
author: pfletcher
description: W tym artykule opisano, jakie klientów i serwerów, które są obsługiwane przez SignalR.
ms.author: aspnetcontent
ms.date: 04/18/2018
ms.assetid: eac31beb-0f46-4afa-9def-e80904dea4f0
msc.legacyurl: /signalr/overview/getting-started/supported-platforms
msc.type: authoredcontent
ms.openlocfilehash: 5d77db71c5c6b0c297756921b5b7cb79add03998
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37805814"
---
<a name="supported-platforms"></a>Obsługiwane platformy
====================
przez [Patrick Fletcher](https://github.com/pfletcher)

> W tym artykule opisano, jakie klientów i serwerów, które są obsługiwane przez SignalR. 
> 
> ## <a name="questions-and-comments"></a>Pytania i komentarze
> 
> Jak się podoba w tym samouczku, i co można było ulepszyć proces w komentarzach u dołu strony, wystaw opinię. Jeśli masz pytania, na które nie są bezpośrednio związane z tego samouczka, możesz zamieścić je do [forum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) lub [StackOverflow.com](http://stackoverflow.com/).


SignalR jest obsługiwany w ramach różnych serwera konfiguracji i klienta. Ponadto każda opcja transportu ma ustalony zbiór wymogów dotyczących własnych; Jeśli wymagania systemowe dla transportu nie są dostępne, SignalR będą bezpiecznie tryb failover do innego transportu. Aby uzyskać więcej informacji na temat transportów, które obsługuje SignalR, zobacz [transportu i planów awaryjnych](introduction-to-signalr.md#transports).

## <a name="server-system-requirements"></a>Wymagania systemowe serwera

Składnik serwera SignalR może być hostowana na wielu różnych konfiguracji serwera. W tej sekcji opisano obsługiwane wersje systemów operacyjnych, .NET framework, Internet Information Server i inne składniki.

### <a name="supported-server-operating-systems"></a>Obsługiwane systemy operacyjne serwera

Składnik serwera SignalR mogą być hostowane w następujących systemach operacyjnych serwera lub klienta. Pamiętaj, że dla elementu SignalR do używają funkcji WebSockets, wymagane jest system Windows Server 2012, Windows Server 2016 lub Windows 8 (WebSocket może być używane w Windows Azure Web Sites, tak długo, jak ustawiono witryny wersji dla programu .NET framework 4.5 i gniazda sieci Web jest włączona w tej lokacji Strona konfiguracji).

- Windows Server 2016
- Windows Server 2012
- Windows Server 2008 r2
- Windows 10
- Windows 8
- Windows 7
- System Microsoft Azure

### <a name="supported-server-net-framework-version"></a>Obsługiwany serwer z .NET Framework w wersji

SignalR 2 jest obsługiwany tylko w programie .NET Framework 4.5. Zobacz [zalecane aktualizacje](#updates) dotyczącej aktualizacji, które zwiększają niezawodność, zgodności, stabilności i wydajności.

### <a name="supported-server-iis-versions"></a>Obsługiwany serwer z wersjami usług IIS

Kiedy SignalR znajduje się w usługach IIS, obsługiwane są następujące wersje. Należy pamiętać, że jeśli jest używany system operacyjny klienta, takie jak do tworzenia aplikacji (system Windows 8 lub Windows 7), pełnych wersji programu IIS lub Cassini nie stosuje się, ponieważ będzie istniało ograniczenie do 10 równoczesnych połączeń nałożone, które będzie można połączyć się z bardzo szybko od połączenia są przejściowy, często nawiązane ponownie i są nie usunięte natychmiast po nie jest już używana. Usługi IIS Express, należy używać w systemach operacyjnych klienta.

Należy również zauważyć, czy dla elementu SignalR do użycia protokołu WebSocket, IIS 8 lub usług IIS Express 8 musi być używany, serwer musi używać systemu Windows 8, Windows Server 2012 lub nowszy, i WebSocket wymaga włączenia w usługach IIS. Aby uzyskać informacje o sposobie włączania protokołu WebSocket w usługach IIS, zobacz [obsługi protokołu WebSocket 8.0 IIS](https://www.iis.net/learn/get-started/whats-new-in-iis-8/iis-80-websocket-protocol-support).

- Usługi IIS 8 lub 8 usług IIS Express.
- Usługi IIS 7 i 7.5. Obsługa [adresy URL bez rozszerzeń](https://support.microsoft.com/kb/980368) jest wymagana.
- Usługi IIS muszą działać w trybie zintegrowanym; trybu klasycznego nie jest obsługiwana. Maksymalnie 30 sekund opóźnienia w dostarczaniu wiadomości mogą się pojawić, jeśli usługi IIS zostanie uruchomiony w trybie klasycznym przy użyciu transportu Server-Sent zdarzenia.
- Aplikacji macierzystej musi działać w trybie pełnego zaufania.

## <a name="client-system-requirements"></a>Wymagania dotyczące systemu klienta

SignalR może służyć w wielu różnych platformach klienckich. W tej sekcji opisano wymagania systemowe dotyczące korzystania z SignalR w przeglądarkach sieci web, aplikacje pulpitu Windows, aplikacji Silverlight i urządzeń przenośnych.

### <a name="web-browsers"></a>Przeglądarki sieci Web

SignalR mogą być używane w różnych przeglądarek sieci web, ale zazwyczaj najnowsza dwie wersje są obsługiwane.

Aplikacje, które używają SignalR w przeglądarkach, należy użyć wersji jQuery 1.6.4 lub nowsze wersje główne (na przykład 1.7.2 1.8.2 lub 1.9.1).

SignalR może służyć w następujących przeglądarkach:

- Wersje programu Microsoft Internet Explorer 8, 9, 10 i 11. Nowoczesny, klasycznych i mobilnych wersje są obsługiwane.
- Przeglądarki Mozilla Firefox: bieżącej wersji - 1, Windows i Mac wersji.
- Google Chrome: wersja bieżąca - 1, Windows i Mac wersji.
- Przeglądarki Safari: Bieżąca wersja - 1, wersje systemów Mac i iOS.
- Opera: Bieżąca wersja - 1, tylko Windows.
- Przeglądarki systemu android

Oprócz wymaga niektórych przeglądarkach, transportów różnych, które korzysta z biblioteki SignalR mają swoje własne wymagania. Następujące transportów są objęte następujące konfiguracje:

<a id="browser"></a>

**Wymagania dotyczące transportu przeglądarki sieci Web**

| Transportu | Internet Explorer | Chrome (Windows lub z systemem iOS) | Firefox | Safari (OS x lub z systemem iOS) | Android |
| --- | --- | --- | --- | --- | --- |
| Funkcja WebSockets | 10+ | Bieżąca wartość-1 | Bieżąca wartość-1 | Bieżąca wartość-1 | Brak |
| Zdarzenia wysłanego przez serwer | Brak | Bieżąca wartość-1 | Bieżąca wartość-1 | Bieżąca wartość-1 | Brak |
| ForeverFrame | 8+ | Brak | Brak | Brak | 4.1 |
| Długiego sondowania | 8+ | Bieżąca wartość-1 | Bieżąca wartość-1 | Bieżąca wartość-1 | 4.1 |

\*: 6 + wymagane do pełnej funkcjonalności.

#### <a name="unsupported-browsers"></a>Nieobsługiwane przeglądarki

Podczas gdy SignalR *może* działać bez poważne problemy w starszych wersjach przeglądarki, firma Microsoft aktywnie test nie SignalR w ich, jak i ogólnie nie rozwiązują błędy, które mogą się pojawić w nich.

### <a name="windows-desktop-and-silverlight-applications"></a>Windows Desktop i aplikacji Silverlight

Oprócz korzystania z przeglądarki sieci web, SignalR mogą być hostowane w klienta Windows autonomicznego lub w aplikacji Silverlight. Aplikacje pulpitu Windows i Silverlight SignalR mają następujące wymagania systemowe.

- Są obsługiwane aplikacje przy użyciu platformy .NET 4, Windows XP z dodatkiem SP3 lub nowszy.
- Aplikacje przy użyciu programu .NET Framework 4.5 są obsługiwane w systemie Windows Vista lub nowszym.

Oprócz systemu operacyjnego i wymagania dotyczące programu .NET framework dostępne dla SignalR transportów mają własne wymagania. Następujące transportów są objęte następujące konfiguracje:

**Wymagania transportu programu Silverlight i Windows Desktop**

| Transportu | Aplikacja platformy .NET | Silverlight |
| --- | --- | --- |
| Gniazda sieci Web | Windows 8 + i .NET 4.5 + | Brak |
| Nieskończona ramki | Brak | Brak |
| Zdarzenia wysłanego przez serwer | .NET 4+ | 5+ |
| Długiego sondowania | .NET 4+ | 5+ |

<a id="android"></a>

### <a name="windows-store-and-windows-phone-applications"></a>Windows Store i aplikacji Windows Phone

SignalR może służyć w aplikacjach Windows Store i aplikacji systemu Windows Phone 8. Następujące transportów są objęte następujące konfiguracje:

**Windows Store i Windows Phone transportu wymagań**

| Transportu | Windows Store/ .NET | Windows Store / JavaScript | Windows Phone / IE | Windows Phone / platformy .NET |
| --- | --- | --- | --- | --- |
| Funkcja WebSockets | Brak | Win8+ | 8+ | Brak |
| Nieskończona ramki | Brak | Win8+ | 7.5+ | Brak |
| Zdarzenia wysłanego przez serwer | Win8+ | Brak | Brak | 8+ |
| Długiego sondowania | Win8+ | Win8+ | 7.5+ | 8+ |

<a id="updates"></a>

## <a name="recommended-updates"></a>Zalecane aktualizacje

Następujące aktualizacje są zalecane dla serwerów z SignalR:

- Dostępna jest aktualizacja dla programu .NET Framework 4.5 [tutaj](https://support.microsoft.com/kb/2750149).
- Firma Microsoft udostępni okresowo poprawek QFE dla platformy ASP.NET. Te powinny być stosowane jako dostępne.
