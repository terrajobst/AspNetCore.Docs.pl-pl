---
title: ASP.NET Core i skalowanie produkcji sygnalizującego
author: bradygaster
description: Dowiedz się, jak uniknąć problemów z wydajnością i skalowaniem w aplikacjach korzystających z sygnałów ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/28/2018
uid: signalr/scale
ms.openlocfilehash: 26b02cffdd472fc21dc4aee7052a0ba939b82c0f
ms.sourcegitcommit: 79eeb17604b536e8f34641d1e6b697fb9a2ee21f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/24/2019
ms.locfileid: "71211733"
---
# <a name="aspnet-core-signalr-hosting-and-scaling"></a>ASP.NET Core hosting i skalowanie sygnałów

Autorzy [Andrew Stanton-pielęgniarki](https://twitter.com/anurse), [Brady Gastera](https://twitter.com/bradygaster)i [Tomasz Dykstra](https://github.com/tdykstra),

W tym artykule opisano zagadnienia dotyczące hostingu i skalowania dla aplikacji o dużym natężeniu ruchu, które używają usługi ASP.NET Core Signaler.

## <a name="sticky-sessions"></a>Sesje programu Sticky

Sygnalizujący wymaga, aby wszystkie żądania HTTP dotyczące określonego połączenia były obsługiwane przez ten sam proces serwera. Gdy program sygnalizujący jest uruchomiony w farmie serwerów (na wielu serwerach), należy użyć "sesji programu Sticky". "Sesje programu Sticky Notes" są również nazywane koligacją sesji przez niektóre moduły równoważenia obciążenia. Azure App Service używa [routingu żądań aplikacji](https://docs.microsoft.com/iis/extensions/planning-for-arr/application-request-routing-version-2-overview) (ARR) do przesyłania żądań. Włączenie ustawienia "koligacja ARR" w Azure App Service spowoduje włączenie "sesji programu Sticky Notes". Jedyną sytuacją, w której nie są wymagane sesje programu Sticky, są:

1. W przypadku hostowania na jednym serwerze w ramach jednego procesu.
1. W przypadku korzystania z usługi Azure Signal Service.
1. Gdy wszyscy klienci są skonfigurowani do korzystania **tylko** z obiektów WebSockets, **a** [ustawienie SkipNegotiation](xref:signalr/configuration#configure-additional-options) jest włączone w konfiguracji klienta.

We wszystkich innych przypadkach (w tym gdy jest używany plan Redis), środowisko serwera musi być skonfigurowane dla sesji programu Sticky Notes.

Aby uzyskać wskazówki dotyczące konfigurowania Azure App Service dla sygnalizacji, <xref:signalr/publish-to-azure-web-app>Zobacz.

## <a name="tcp-connection-resources"></a>Zasoby połączenia TCP

Liczba współbieżnych połączeń TCP, które może obsługiwać serwer sieci Web, jest ograniczona. Klienci standardowi HTTP korzystają z połączeń *tymczasowych* . Te połączenia można zamknąć, gdy klient przechodzi w stan bezczynności i zostanie otwarty ponownie później. Z drugiej strony połączenie sygnalizujące ma charakter *trwały*. Połączenia sygnalizujące pozostają otwarte nawet wtedy, gdy klient przejdzie w stan bezczynności. W aplikacji o dużym natężeniu ruchu, która obsługuje wielu klientów, te trwałe połączenia mogą spowodować, że serwery osiągnął maksymalną liczbę połączeń.

Połączenia trwałe zużywają także dodatkową pamięć, aby śledzić każde połączenie.

Duże wykorzystanie zasobów związanych z połączeniami przez sygnalizującego może mieć wpływ na inne aplikacje sieci Web, które są hostowane na tym samym serwerze. Gdy sygnalizujący otwiera i przechowuje ostatnie dostępne połączenia TCP, inne aplikacje sieci Web na tym samym serwerze również nie będą miały dostępnych dodatkowych połączeń.

Jeśli na serwerze wykorzystano połączenia, zobaczysz losowe błędy gniazda i błędy resetowania połączenia. Na przykład:

```
An attempt was made to access a socket in a way forbidden by its access permissions...
```

Aby zapobiec wykorzystaniu przez inne aplikacje sieci Web błędów przez zasoby sygnalizujące, uruchom program sygnalizujący na różnych serwerach niż inne aplikacje sieci Web.

Aby zachować użycie zasobów przez sygnalizujące z powodu błędów w aplikacji sygnalizującej, Skaluj w poziomie, aby ograniczyć liczbę połączeń, które serwer musi obsłużyć.

## <a name="scale-out"></a>Skalowanie w poziomie

Aplikacja, która korzysta z sygnalizującego, musi śledzić wszystkie połączenia, które tworzą problemy dla farmy serwerów. Dodaj serwer i pobiera nowe połączenia, o których nie wie inne serwery. Na przykład program sygnalizujący na każdym serwerze na poniższym diagramie nie rozpoznaje połączeń na innych serwerach. Gdy sygnał na jednym z serwerów chce wysłać komunikat do wszystkich klientów, komunikat zostanie przekierowany do klientów podłączonych do tego serwera.

![Skalowanie sygnału bez planu](scale/_static/scale-no-backplane.png)

Opcjami rozwiązywania tego problemu jest [usługa Azure Signal Service](#azure-signalr-service) i [Redis plan](#redis-backplane).

## <a name="azure-signalr-service"></a>Usługa Azure SignalR Service

Usługa Azure Signal Service to serwer proxy, a nie plan. Za każdym razem, gdy klient inicjuje połączenie z serwerem, klient zostaje przekierowany do programu w celu nawiązania połączenia z usługą. Ten proces przedstawiono na poniższym diagramie:

![Nawiązywanie połączenia z usługą Azure sygnalizującej](scale/_static/azure-signalr-service-one-connection.png)

Wynika to z tego, że usługa zarządza wszystkimi połączeniami klientów, natomiast każdy serwer potrzebuje tylko małej stałej liczby połączeń z usługą, jak pokazano na poniższym diagramie:

![Klienci połączeni z usługą, serwery połączone z usługą](scale/_static/azure-signalr-service-multiple-connections.png)

Takie podejście do skalowania w poziomie ma kilka korzyści w porównaniu z Redisą dla planu wieloplanowego:

* Sesje programu Sticky Notes, nazywane również [koligacją klienta](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing#step-3---configure-client-affinity), nie są wymagane, ponieważ klienci są natychmiast przekierowywani do usługi Azure sygnalizującej, gdy łączą się.
* Aplikacja sygnalizująca może być skalowana w poziomie na podstawie liczby wysłanych komunikatów, a usługa Azure sygnalizująca automatycznie skaluje się do obsługi dowolnej liczby połączeń. Na przykład mogą istnieć tysiące klientów, ale jeśli zostanie wysłanych tylko kilka komunikatów na sekundę, aplikacja sygnalizująca nie będzie musiała skalować się do wielu serwerów, aby obsługiwać same połączenia.
* Aplikacja sygnalizująca nie będzie używać znacznie więcej zasobów połączenia niż aplikacja internetowa bez sygnalizującego.

Z tego względu zalecamy korzystanie z usługi Azure sygnalizującej dla wszystkich ASP.NET Core aplikacji sygnalizujących hostowanych na platformie Azure, w tym App Service, maszyn wirtualnych i kontenerów.

Aby uzyskać więcej informacji, zobacz [dokumentację usługi Azure Signal Service](/azure/azure-signalr/signalr-overview).

## <a name="redis-backplane"></a>Płyta montażowa Redis

[Redis](https://redis.io/) to magazyn kluczy w pamięci, który obsługuje system obsługi komunikatów z modelem publikowania/subskrybowania. Funkcja Rediser programu sygnalizującego używa funkcji pub/sub do przesyłania dalej komunikatów do innych serwerów. Gdy klient nawiązuje połączenie, informacje o połączeniu są przesyłane do planu. Gdy serwer chce wysłać komunikat do wszystkich klientów, wysyła do planu. W planie dla wszystkich podłączonych klientów i serwerów, na których się znajdują. Wysyła komunikat do wszystkich klientów za pośrednictwem odpowiednich serwerów. Ten proces przedstawiono na poniższym diagramie:

![Redis, wiadomości wysyłane z jednego serwera do wszystkich klientów](scale/_static/redis-backplane.png)

Plan Redis to zalecane podejście skalowalne w poziomie dla aplikacji hostowanych we własnej infrastrukturze. Usługa Azure Signal Service nie jest praktyczną opcją do użycia w środowisku produkcyjnym z aplikacjami lokalnymi ze względu na opóźnienie połączenia między centrum danych a centrum danych platformy Azure.

Zalety usługi Azure Signal Service zanotowane wcześniej są wadą dla Redisego planu IT:

* Są wymagane sesje programu Sticky Notes, nazywane również [koligacją klienta](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing#step-3---configure-client-affinity). Po zainicjowaniu połączenia na serwerze połączenie musi pozostać na tym serwerze.
* Aplikacja sygnalizująca musi być skalowana w poziomie na podstawie liczby klientów, nawet jeśli jest wysyłanych kilka komunikatów.
* Aplikacja sygnalizująca używa znacznie większej liczby zasobów połączenia niż aplikacja internetowa bez sygnalizującego.

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji, zobacz następujące zasoby:

* [Dokumentacja usługi Azure sygnalizującego](/azure/azure-signalr/signalr-overview)
* [Konfigurowanie planu Redis](xref:signalr/redis-backplane)
