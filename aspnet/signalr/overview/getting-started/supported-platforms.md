---
uid: signalr/overview/getting-started/supported-platforms
title: "Obsługiwane platformy | Dokumentacja firmy Microsoft"
author: pfletcher
description: "W tym artykule opisano, jakie klientów i serwerów obsługiwanych przez SignalR."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: eac31beb-0f46-4afa-9def-e80904dea4f0
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/getting-started/supported-platforms
msc.type: authoredcontent
ms.openlocfilehash: 7f41017a2a8c058c01fe6f89a2503eb5fa77048e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="supported-platforms"></a>Obsługiwane platformy
====================
przez [Patrick Fletcher](https://github.com/pfletcher)

> W tym artykule opisano, jakie klientów i serwerów obsługiwanych przez SignalR. 
> 
> ## <a name="questions-and-comments"></a>Pytania i komentarze
> 
> Wystaw opinię na jak zbędne tego samouczka i jakie firma Microsoft może poprawić w komentarze u dołu strony. Jeśli masz pytania, które nie są bezpośrednio związane z tego samouczka możesz zamieścić je do [forum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) lub [StackOverflow.com](http://stackoverflow.com/).


SignalR jest obsługiwany w ramach różnych serwera i konfiguracje klienta. Ponadto każda opcja transportu ma zestaw wymagań własnych; Jeśli wymagania systemowe dla transportu nie są dostępne, SignalR będą bezpiecznie trybu failover do innych transportów. Aby uzyskać więcej informacji na transportów, które obsługuje SignalR, zobacz [transportu i przejścia](introduction-to-signalr.md#transports).

## <a name="server-system-requirements"></a>Wymagania dotyczące systemu serwera

Składnik serwera SignalR mogą być hostowane na różnych konfiguracji serwera. W tej sekcji opisano obsługiwane wersje systemów operacyjnych, .NET framework, Internet Information Server i inne składniki.

### <a name="supported-server-operating-systems"></a>Systemy operacyjne obsługiwane serwera

Składnik serwera SignalR może być obsługiwany w następujących systemach operacyjnych klienta lub serwera. Należy pamiętać, że dla biblioteki SignalR do używania protokołu WebSockets, Windows Server 2012 lub Windows 8 wymagana (WebSocket może być używane w witrynach sieci Web systemu Windows Azure tak długo, jak ustawiono witryny .NET framework w wersji 4.5 i gniazda sieci Web jest włączona na stronie konfiguracji witryny).

- Windows Server 2012
- Windows Server 2008 r2
- Windows 8
- Windows 7
- System Microsoft Azure

### <a name="supported-server-net-framework-version"></a>.NET Framework w wersji obsługiwanej serwera

SignalR 2 jest obsługiwany tylko w programie .NET Framework 4.5. Zobacz [zalecane aktualizacje](#updates) sekcji aktualizacje, które zwiększają niezawodności, kompatybilności, stabilności i wydajności.

### <a name="supported-server-iis-versions"></a>Obsługiwany serwer z wersjami usług IIS

Kiedy SignalR znajduje się w usługach IIS, obsługiwane są następujące wersje. Pamiętaj, że jeśli jest używany system operacyjny klienta, takich jak rozwoju (Windows 8 lub Windows 7), pełnej wersji programu IIS lub Cassini powinien nie można użyć, ponieważ będzie limitu 10 równoczesnych połączeń nałożone, które będzie można połączyć się z bardzo szybko od połączenia są przejściowy, często nawiązał ponownie i czy nie usuwane natychmiast po nie jest już używana. Usługi IIS Express powinien być używany w systemach operacyjnych klienta.

Należy również zauważyć, że dla biblioteki SignalR do używania protokołu WebSocket, można używać usług IIS 8 lub usług IIS Express 8, Windows 8, Windows Server 2012 lub nowszym, musi być używane przez serwer i protokołu WebSocket musi być włączona w usługach IIS. Aby uzyskać informacje o sposobie włączania protokołu WebSocket w usługach IIS, zobacz [Obsługa protokołu WebSocket 8.0 IIS](https://www.iis.net/learn/get-started/whats-new-in-iis-8/iis-80-websocket-protocol-support).

- Usługi IIS 8 lub 8 usług IIS Express.
- Usługi IIS 7 i 7.5. Obsługa [adresy URL bez rozszerzeń](https://support.microsoft.com/kb/980368) jest wymagana.
- Usługi IIS musi być uruchomiony w trybie zintegrowanym; trybu klasycznego nie jest obsługiwana. Opóźnienia komunikat do 30 sekund mogą wystąpić, jeśli usługi IIS jest uruchamiana w trybie klasycznym, za pomocą transportu Server-Sent zdarzenia.
- Aplikacja macierzysta musi działać w trybie pełnego zaufania.

## <a name="client-system-requirements"></a>Wymagania dotyczące systemu klienta

SignalR można na wiele platform klient. W tej sekcji opisano wymagania systemowe dotyczące korzystania z SignalR w przeglądarkach sieci web, aplikacji pulpitu systemu Windows aplikacji Silverlight i urządzeń przenośnych.

### <a name="web-browsers"></a>Przeglądarki sieci Web

SignalR mogą być używane w różnych przeglądarek sieci web, ale zazwyczaj są obsługiwane tylko dwóch najnowszej wersji.

Aplikacje używające SignalR w przeglądarkach muszą używać wersji jQuery 1.6.4 lub nowsze wersje główne (na przykład 1.7.2 1.8.2 lub 1.9.1).

SignalR mogą być używane w następujących przeglądarkach:

- Wersje programu Microsoft Internet Explorer 8, 9, 10 lub 11. Nowoczesny, pulpitu i przenośnych wersje są obsługiwane.
- Mozilla Firefox: wersja bieżąca - 1, Windows i Mac wersji.
- Google Chrome: wersja bieżąca - 1, Windows i Mac wersji.
- Safari: Bieżąca wersja - 1, wersje zarówno Mac i z systemem iOS.
- Opera: Bieżąca wersja - 1, tylko w systemie Windows.
- Przeglądarki systemu android

Oprócz wymagające niektóre przeglądarki, różnych transportów używane przez SignalR mają wymagania we własnym. Następujące transportu są obsługiwane w następujących konfiguracji:

<a id="browser"></a>

**Wymagania dotyczące transportu przeglądarki sieci Web**

| Transportu | Internet Explorer | Chrome (Windows lub z systemem iOS) | Firefox | Safari (OS x lub iOS) | Android |
| --- | --- | --- | --- | --- | --- |
| Protokół WebSockets | 10+ | Bieżąca wartość-1 | Bieżąca wartość-1 | Bieżąca wartość-1 | Brak |
| Zdarzenia wysłanego przez serwer | Brak | Bieżąca wartość-1 | Bieżąca wartość-1 | Bieżąca wartość-1 | Brak |
| ForeverFrame | 8+ | Brak | Brak | Brak | 4.1 |
| Długiego sondowania | 8+ | Bieżąca wartość-1 | Bieżąca wartość-1 | Bieżąca wartość-1 | 4.1 |

\*: 6 + wymagane do zapewnienia pełnej funkcjonalności.

#### <a name="unsupported-browsers"></a>Nieobsługiwane przeglądarki

Podczas SignalR *może* uruchamiane bez poważne problemy w starszych wersjach przeglądarki, firma Microsoft nie aktywnie Testuj SignalR w nich, jak i zazwyczaj nie rozwiązują usterki, które mogą być wyświetlane.

### <a name="windows-desktop-and-silverlight-applications"></a>Pulpitu systemu Windows i aplikacji Silverlight

Oprócz działającego w przeglądarce sieci web, SignalR może być hostowana w autonomicznej klienta systemu Windows lub aplikacji Silverlight. Aplikacje pulpitu systemu Windows i Silverlight SignalR mają następujące wymagania systemowe.

- Aplikacje przy użyciu platformy .NET 4 są obsługiwane w systemie Windows XP z dodatkiem SP3 lub nowszym.
- Aplikacje przy użyciu programu .NET Framework 4.5 są obsługiwane w systemie Windows Vista lub nowszego.

Oprócz systemu operacyjnego i wymagania dotyczące programu .NET framework transportów dostępne dla SignalR ma własnych wymagań. Następujące transportu są obsługiwane w następujących konfiguracji:

**Wymagania dotyczące transportu Silverlight i pulpitu systemu Windows**

| Transportu | Aplikacja .NET | Silverlight |
| --- | --- | --- |
| Gniazda sieci Web | Windows 8 i .NET 4.5 + | Brak |
| Nieskończona ramki | Brak | Brak |
| Zdarzenia wysłanego przez serwer | .NET 4 | 5+ |
| Długiego sondowania | .NET 4 | 5+ |

<a id="android"></a>

### <a name="windows-store-and-windows-phone-applications"></a>Sklep Windows i Windows Phone aplikacje

SignalR można używać w aplikacji ze Sklepu Windows i aplikacji Windows Phone 8. Następujące transportu są obsługiwane w następujących konfiguracji:

**Sklep Windows i Windows Phone transportu wymagania**

| Transportu | Sklep Windows / .NET | Sklep Windows / JavaScript | Windows Phone / IE | Windows Phone / .NET |
| --- | --- | --- | --- | --- |
| Protokół WebSockets | Brak | Windows 8 + | 8+ | Brak |
| Nieskończona ramki | Brak | Windows 8 + | 7.5+ | Brak |
| Zdarzenia wysłanego przez serwer | Windows 8 + | Brak | Brak | 8+ |
| Długiego sondowania | Windows 8 + | Windows 8 + | 7.5+ | 8+ |

<a id="updates"></a>

## <a name="recommended-updates"></a>Zalecane aktualizacje

W przypadku serwerów SignalR zaleca się następujące aktualizacje:

- Dostępna jest aktualizacja dla programu .NET Framework 4.5 [tutaj](https://support.microsoft.com/kb/2750149).
- Firma Microsoft będzie okresowo udostępniać poprawek QFE dla platformy ASP.NET. Te powinny być stosowane jako dostępne.
