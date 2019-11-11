---
title: ASP.NET Core SignalR hosting i skalowanie produkcji
author: bradygaster
description: Dowiedz się, jak uniknąć problemów z wydajnością i skalowaniem w aplikacjach korzystających z ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/28/2018
uid: signalr/scale
no-loc:
- SignalR
ms.openlocfilehash: a215ce489746a7585cf4e72d4f04e0eac588b44c
ms.sourcegitcommit: a7bbe3890befead19440075b05b9674351f98872
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2019
ms.locfileid: "73905725"
---
# <a name="aspnet-core-opno-locsignalr-hosting-and-scaling"></a>ASP.NET Core SignalR hosting i skalowanie

Autorzy [Andrew Stanton-pielęgniarki](https://twitter.com/anurse), [Brady Gastera](https://twitter.com/bradygaster)i [Tomasz Dykstra](https://github.com/tdykstra),

W tym artykule wyjaśniono zagadnienia dotyczące hostingu i skalowania dla aplikacji o dużym natężeniu ruchu, które używają SignalRASP.NET Core.

## <a name="sticky-sessions"></a>Sesje programu Sticky

SignalR wymaga, aby wszystkie żądania HTTP dotyczące określonego połączenia były obsługiwane przez ten sam proces serwera. Gdy SignalR jest uruchomiona w farmie serwerów (na wielu serwerach), należy użyć "sesji programu Sticky Notes". "Sesje programu Sticky Notes" są również nazywane koligacją sesji przez niektóre moduły równoważenia obciążenia. Azure App Service używa [routingu żądań aplikacji](https://docs.microsoft.com/iis/extensions/planning-for-arr/application-request-routing-version-2-overview) (ARR) do przesyłania żądań. Włączenie ustawienia "koligacja ARR" w Azure App Service spowoduje włączenie "sesji programu Sticky Notes". Jedyną sytuacją, w której nie są wymagane sesje programu Sticky, są:

1. W przypadku hostowania na jednym serwerze w ramach jednego procesu.
1. W przypadku korzystania z usługi Azure SignalR.
1. Gdy wszyscy klienci są skonfigurowani do korzystania **tylko** z obiektów WebSockets, **a** [ustawienie SkipNegotiation](xref:signalr/configuration#configure-additional-options) jest włączone w konfiguracji klienta.

We wszystkich innych przypadkach (w tym gdy jest używany plan Redis), środowisko serwera musi być skonfigurowane dla sesji programu Sticky Notes.

Aby uzyskać wskazówki dotyczące konfigurowania Azure App Service dla SignalR, zobacz <xref:signalr/publish-to-azure-web-app>.

## <a name="tcp-connection-resources"></a>Zasoby połączenia TCP

Liczba współbieżnych połączeń TCP, które może obsługiwać serwer sieci Web, jest ograniczona. Klienci standardowi HTTP korzystają z połączeń *tymczasowych* . Te połączenia można zamknąć, gdy klient przechodzi w stan bezczynności i zostanie otwarty ponownie później. Z drugiej strony połączenie SignalR jest *trwałe*. połączenia SignalR pozostają otwarte nawet wtedy, gdy klient przejdzie w stan bezczynności. W aplikacji o dużym natężeniu ruchu, która obsługuje wielu klientów, te trwałe połączenia mogą spowodować, że serwery osiągnął maksymalną liczbę połączeń.

Połączenia trwałe zużywają także dodatkową pamięć, aby śledzić każde połączenie.

Duże wykorzystanie zasobów związanych z połączeniami przez SignalR może mieć wpływ na inne aplikacje sieci Web, które są hostowane na tym samym serwerze. Gdy SignalR otwiera i utrzymuje ostatnie dostępne połączenia TCP, inne aplikacje sieci Web na tym samym serwerze również nie będą miały dostępnych połączeń.

Jeśli na serwerze wykorzystano połączenia, zobaczysz losowe błędy gniazda i błędy resetowania połączenia. Na przykład:

```
An attempt was made to access a socket in a way forbidden by its access permissions...
```

Aby zapobiec wykorzystaniu przez SignalR zasobów błędów w innych aplikacjach sieci Web, uruchom SignalR na różnych serwerach niż inne aplikacje sieci Web.

Aby zapobiec wykorzystaniu przez SignalR zasobów błędów w aplikacji SignalR, Przeskaluj w poziomie, aby ograniczyć liczbę połączeń, które serwer musi obsłużyć.

## <a name="scale-out"></a>Skalowanie w poziomie

Aplikacja, która używa SignalR musi śledzić wszystkie połączenia, które tworzą problemy dla farmy serwerów. Dodaj serwer i pobiera nowe połączenia, o których nie wie inne serwery. Na przykład SignalR na każdym serwerze na poniższym diagramie jest nieświadomy połączeń na innych serwerach. Gdy SignalR na jednym z serwerów chce wysłać komunikat do wszystkich klientów, komunikat zostanie przekierowany do klientów podłączonych do tego serwera.

![Skalowanie [! OP. NO-LOC (sygnalizujący)] bez planu](scale/_static/scale-no-backplane.png)

Opcjami rozwiązywania tego problemu jest [usługa Azure SignalR](#azure-signalr-service) i [Redis plan](#redis-backplane).

## <a name="azure-opno-locsignalr-service"></a>Usługa SignalR platformy Azure

Usługa Azure SignalR to serwer proxy, a nie plan. Za każdym razem, gdy klient inicjuje połączenie z serwerem, klient zostaje przekierowany do programu w celu nawiązania połączenia z usługą. Ten proces przedstawiono na poniższym diagramie:

![Nawiązywanie połączenia z platformą Azure [! OP. Nie-LOC (sygnalizujący)] usługa](scale/_static/azure-signalr-service-one-connection.png)

Wynika to z tego, że usługa zarządza wszystkimi połączeniami klientów, natomiast każdy serwer potrzebuje tylko małej stałej liczby połączeń z usługą, jak pokazano na poniższym diagramie:

![Klienci połączeni z usługą, serwery połączone z usługą](scale/_static/azure-signalr-service-multiple-connections.png)

Takie podejście do skalowania w poziomie ma kilka korzyści w porównaniu z Redisą dla planu wieloplanowego:

* Sesje programu Sticky Notes, nazywane również [koligacją klienta](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing#step-3---configure-client-affinity), nie są wymagane, ponieważ klienci są natychmiast przekierowywani do usługi Azure SignalR, gdy łączą się.
* Aplikacja SignalR może skalować w poziomie na podstawie liczby wysłanych komunikatów, podczas gdy usługa SignalR platformy Azure automatycznie skaluje się do obsługi dowolnej liczby połączeń. Na przykład mogą istnieć tysiące klientów, ale jeśli są wysyłane tylko kilka komunikatów na sekundę, aplikacja SignalR nie będzie musiała skalować w poziomie do wielu serwerów, aby obsługiwać same połączenia.
* Aplikacja SignalR nie będzie używać znacznie większej liczby zasobów połączenia niż aplikacja internetowa bez SignalR.

Z tego względu zalecamy korzystanie z usługi Azure SignalR dla wszystkich ASP.NET Core SignalR aplikacji hostowanych na platformie Azure, w tym App Service, maszyn wirtualnych i kontenerów.

Aby uzyskać więcej informacji, zobacz [dokumentację usługi Azure SignalR](/azure/azure-signalr/signalr-overview).

## <a name="redis-backplane"></a>Płyta montażowa Redis

[Redis](https://redis.io/) to magazyn kluczy w pamięci, który obsługuje system obsługi komunikatów z modelem publikowania/subskrybowania. Funkcja Redis w SignalR ramach planu wieloplanowego używa funkcji publikowania/podusługi do przesyłania dalej komunikatów do innych serwerów. Gdy klient nawiązuje połączenie, informacje o połączeniu są przesyłane do planu. Gdy serwer chce wysłać komunikat do wszystkich klientów, wysyła do planu. W planie dla wszystkich podłączonych klientów i serwerów, na których się znajdują. Wysyła komunikat do wszystkich klientów za pośrednictwem odpowiednich serwerów. Ten proces przedstawiono na poniższym diagramie:

![Redis, wiadomości wysyłane z jednego serwera do wszystkich klientów](scale/_static/redis-backplane.png)

Plan Redis to zalecane podejście skalowalne w poziomie dla aplikacji hostowanych we własnej infrastrukturze. Usługa Azure SignalR nie jest praktyczną opcją do użycia w środowisku produkcyjnym z aplikacjami lokalnymi ze względu na opóźnienie połączenia między centrum danych a centrum danych platformy Azure.

Zanotowane wcześniej zalety usługi Azure SignalR są niekorzystne dla planu Redis:

* Są wymagane sesje programu Sticky Notes, nazywane również [koligacją klienta](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing#step-3---configure-client-affinity). Po zainicjowaniu połączenia na serwerze połączenie musi pozostać na tym serwerze.
* Aplikacja SignalR musi być skalowana w poziomie na podstawie liczby klientów, nawet jeśli jest wysyłanych kilka komunikatów.
* Aplikacja SignalR używa znacznie większej liczby zasobów połączenia niż aplikacja internetowa bez SignalR.

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji, zobacz następujące zasoby:

* [Dokumentacja usługi Azure SignalR](/azure/azure-signalr/signalr-overview)
* [Konfigurowanie planu Redis](xref:signalr/redis-backplane)
