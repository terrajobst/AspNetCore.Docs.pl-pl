---
title: ASP.NET Core SignalR produkcji hostowania i skalowania
author: tdykstra
description: Dowiedz się, jak uniknąć wydajności i skalowania problemy w aplikacjach korzystających z biblioteki SignalR platformy ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 11/28/2018
uid: signalr/scale
ms.openlocfilehash: 94791ffb73b58a9026942d632bce59773e3fda5b
ms.sourcegitcommit: e9b99854b0a8021dafabee0db5e1338067f250a9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2018
ms.locfileid: "52453025"
---
# <a name="aspnet-core-signalr-hosting-and-scaling"></a>ASP.NET Core SignalR hostowania i skalowania

Przez [Andrew Stanton pielęgniarki](https://twitter.com/anurse), [Brady'ego Gastera](https://twitter.com/bradygaster), i [Tom Dykstra](https://github.com/tdykstra),

W tym artykule wyjaśniono, hostowanie i skalowanie zagadnienia dotyczące aplikacji o wysokim natężeniu ruchu, korzystających z biblioteki SignalR platformy ASP.NET Core.

## <a name="tcp-connection-resources"></a>Zasoby połączenia TCP

Liczba jednoczesnych połączeń TCP, która może obsłużyć serwer sieci web jest ograniczona. Użyj standardowych klientów HTTP *efemeryczne* połączeń. Te połączenia może zostać zamknięty, gdy klient przechodzi bezczynności i otworzyć ponownie później. Z drugiej strony, jest połączenia SignalR *trwałego*. Połączenia SignalR pozostają otwarte, nawet wtedy, gdy klient wprowadzona bezczynności. W aplikacji o dużym natężeniu ruchu, który obsługuje wielu klientów tych połączeń trwałych może spowodować serwerów trafić ich maksymalna liczba połączeń.

Połączenia trwałe wymaga użycia dodatkowej pamięci, aby śledzić każdego połączenia.

Duże wykorzystanie związane z połączeniem zasobów przez element SignalR może wpływać na inne aplikacje sieci web, które są hostowane na tym samym serwerze. Jeśli SignalR zostanie otwarty i zawiera ostatni dostępnych połączeń TCP, inne aplikacje sieci web na tym samym serwerze również mieć dostępnych do nich żadnych połączeń.

Jeśli serwer jest uruchomiony poza połączeniami, zostaną wyświetlone błędy losowe gniazda, a połączenie zresetowane błędy. Na przykład:

```
An attempt was made to access a socket in a way forbidden by its access permissions...
```

Aby uniknąć użycia zasobów SignalR przyczyną błędów w innych aplikacjach sieci web, należy uruchomić na różnych serwerach niż inne aplikacje sieci web SignalR.

Aby uniknąć użycia zasobów SignalR przyczyną błędów w aplikacji SignalR, skalowanie w poziomie, aby ograniczyć liczbę połączeń serwera musi obsługiwać.

## <a name="scale-out"></a>Skalowanie w poziomie

Aplikację, która używa biblioteki SignalR musi do śledzenia swoich połączeń, które stwarza problemy dla farmy serwerów. Dodawanie serwera i pobiera nowe połączenia, które inne serwery nie wiedzieć o. Na przykład SignalR na każdym serwerze, na poniższym diagramie nie rozpoznaje połączeń na innych serwerach. Gdy SignalR na jednym z serwerów chce wysłać wiadomość do wszystkich klientów, komunikat prowadzi tylko klientów podłączonych do tego serwera.

![Skalowanie SignalR bez płyty montażowej](scale/_static/scale-no-backplane.png)

Opcje dotyczące rozwiązywania tego problemu są [usługi Azure SignalR Service](#azure-signalr-service) i [Redis płyty montażowej](#redis-backplane).

## <a name="azure-signalr-service"></a>Usługi Azure SignalR Service

Usługa Azure SignalR Service to serwer proxy, a nie jako płyty montażowej. Każdorazowo, klient inicjuje połączenie z serwerem, klient zostanie przekierowany do połączenia z usługą. Ten proces jest zilustrowany na poniższym diagramie:

![Nawiązywania połączenia z usługi Azure SignalR Service](scale/_static/azure-signalr-service-one-connection.png)

Wynik jest, że usługa zarządza wszystkich połączeń klienta, a każdy serwer wymaga tylko niewielka liczba stałej połączenia z usługą, jak pokazano na poniższym diagramie:

![Klienci połączeni z usługą serwerów połączonych z usługą](scale/_static/azure-signalr-service-multiple-connections.png)

Takie podejście do skalowania w poziomie ma kilka zalet nad alternatywą płyty montażowej Redis:

* Trwałych sesji, nazywana również [koligacja klienta](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing#step-3---configure-client-affinity), nie jest wymagana, ponieważ klienci są przekierowywani bezpośrednio do usługi Azure SignalR Service podczas nawiązywania połączenia.
* SignalR, którą można skalować aplikacji na podstawie liczby wiadomości wysyłanych, gdy usługa Azure SignalR Service automatycznie skaluje się do obsługi dowolnej liczby połączeń. Na przykład może być tysiące klientów, ale jeśli tylko kilka komunikatów na sekundę są wysyłane, nie będzie już konieczne aplikacji SignalR, skalowanie w poziomie do wielu serwerów, tylko do obsługi połączeń, samodzielnie.
* Aplikacji SignalR nie będzie używać znacznie więcej zasobów połączenia niż aplikacji sieci web bez SignalR.

Z tego względu zalecamy Azure SignalR Service dla wszystkich aplikacji biblioteki SignalR platformy ASP.NET Core hostowanej na platformie Azure, w tym usługi App Service, maszyny wirtualne i kontenery.

Aby uzyskać więcej informacji, zobacz [dokumentacji usługi Azure SignalR Service](/azure/azure-signalr/signalr-overview).

## <a name="redis-backplane"></a>Redis płyty montażowej

[Redis](https://redis.io/) jest parach klucz wartość w pamięci, który obsługuje system obsługi komunikatów za pomocą modelu publikowania/subskrybowania. Płyty montażowej Redis w SignalR używa funkcji publikowania/subskrybowania do przekazywania wiadomości na inne serwery. Gdy klient nawiązuje połączenie, informacje o połączeniu są przekazywane do systemu backplane. Gdy serwer chce, aby wysłać wiadomość do wszystkich klientów, wysyła do systemu backplane. Systemu backplane zna wszystkich podłączonych klientów, które serwery są włączone. Wysyła wiadomość do wszystkich klientów za pośrednictwem ich odpowiednich serwerów. Ten proces jest zilustrowany na poniższym diagramie:

![Redis płyty montażowej, komunikatu wysłanego z jednego serwera do wszystkich klientów](scale/_static/redis-backplane.png)

Płyty montażowej Redis jest zalecanym podejściem skalowalnego w poziomie dla aplikacji hostowanych w ramach swojej własnej infrastruktury. Usługi Azure SignalR Service jest praktyczny dotycząca użycia w środowisku produkcyjnym za pomocą aplikacji w środowisku lokalnym, ze względu na czas oczekiwania na połączenie między centrum danych a centrum danych platformy Azure.

Zalety usługi Azure SignalR Service zanotowanej wcześniej są wady płyty montażowej Redis:

* Trwałych sesji, nazywana również [koligacja klienta](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing#step-3---configure-client-affinity), jest wymagana. Po zainicjowaniu połączenia na serwerze, połączenie musi pozostać na tym serwerze.
* Aplikacji SignalR musisz skalować w poziomie na podstawie liczby klientów, nawet wtedy, gdy kilka komunikaty są wysyłane.
* Aplikacji SignalR zużywa znacznie więcej zasobów połączenia niż aplikacji sieci web bez SignalR.

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji, zobacz następujące zasoby:

* [Dokumentacja usługi Azure SignalR Service](/azure/azure-signalr/signalr-overview)
* [Konfigurowanie montażowa pamięci podręcznej Redis](xref:signalr/redis-backplane)
