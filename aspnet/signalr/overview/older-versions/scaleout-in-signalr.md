---
uid: signalr/overview/older-versions/scaleout-in-signalr
title: Wprowadzenie do skalowania w SignalR 1.x | Dokumentacja firmy Microsoft
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/29/2013
ms.topic: article
ms.assetid: 3fd9f11c-799b-4001-bd60-1e70cfc61c19
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/scaleout-in-signalr
msc.type: authoredcontent
ms.openlocfilehash: e6230d4d65adb8c9a064545ad761898ca53562bf
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="introduction-to-scaleout-in-signalr-1x"></a>Wprowadzenie do skalowania w SignalR 1.x
====================
przez [Wasson Jan](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)

Ogólnie rzecz biorąc, istnieją dwa sposoby można skalować aplikacji sieci web: *skalowanie w górę* i *skalowanie*.

- Skalowanie w górę oznacza, że za pomocą większy serwer (lub większych maszyn wirtualnych) z więcej pamięci RAM, procesory itp.
- Skalowanie w poziomie oznacza dodanie większej liczby serwerów do obsługi obciążenia.

Problem z skalowaniu jest szybkie osiągnęła limit rozmiaru maszyny. Ponadto należy do skalowania w poziomie. Jednak gdy skalowanie w poziomie klientów można pobrać kierowane do różnych serwerów. Klient, który jest podłączony do jednego serwera nie będą otrzymywać wiadomości wysyłane z innego serwera.

![](scaleout-in-signalr/_static/image1.png)

Jedno rozwiązanie ma przesyłać dalej komunikatów między serwerami przy użycia składnik o nazwie *płyty montażowej*. W środowisku IDE włączone każdego wystąpienia aplikacji wysyła komunikaty do systemu backplane i systemu backplane przekazuje je do wystąpienia aplikacji. (W electronics, płyty montażowej to grupa równoległych łączników. Analogicznie montażowa SignalR łączy wiele serwerów.)

![](scaleout-in-signalr/_static/image2.png)

Biblioteka SignalR udostępnia obecnie trzy montażowych:

- **Usługa Azure Service Bus**. Usługa Service Bus jest infrastruktury obsługi wiadomości, umożliwiający składników do wysyłania wiadomości w sposób luźno powiązanych.
- **Redis**. Redis jest magazyn kluczy i wartości w pamięci. Redis obsługuje wzorzec publikowania/subskrypcji ("pub/sub") do wysyłania wiadomości.
- **Program SQL Server**. Środowiska IDE programu SQL Server zapisuje komunikaty do tabel SQL. Systemu backplane używa brokera usług dla wydajność obsługi wiadomości. Jednak działa także jeśli Service Broker jest wyłączona.

W przypadku wdrożenia aplikacji na platformie Azure, należy rozważyć użycie usługi Azure Service Bus systemu backplane. Jeśli są wdrażane w farmie serwerów, należy wziąć pod uwagę program SQL Server lub montażowych Redis.

Samouczki krok po kroku dla każdej płyty montażowej można znaleźć w następujących tematach:

- [Skalowania SignalR z usługi Azure Service Bus](scaleout-with-windows-azure-service-bus.md)
- [Skalowania SignalR z pamięci podręcznej Redis](scaleout-with-redis.md)
- [Skalowania SignalR z programem SQL Server](scaleout-with-sql-server.md)

## <a name="implementation"></a>Implementacja

W bibliotece SignalR każdy komunikat jest wysyłany za pośrednictwem magistrali komunikatów. Implementuje magistrali komunikatów [IMessageBus](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.messaging.imessagebus(v=vs.100).aspx) interfejsu, która dostarcza abstrakcji publikowania/subskrypcji. Montażowych pracy przez zastąpienie domyślnie **IMessageBus** z magistralą przeznaczone dla tego systemu backplane. Na przykład jest magistrali komunikatu Redis [RedisMessageBus](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.redis.redismessagebus(v=vs.100).aspx), i używa pamięci podręcznej Redis [pub/sub](http://redis.io/topics/pubsub) mechanizm do wysyłania i odbierania wiadomości.

Każde wystąpienie serwera łączy do systemu backplane za pośrednictwem magistrali. Po wysłaniu komunikatu trafia do systemu backplane i wysyła go do każdego serwera systemu backplane. Gdy serwer pobiera wiadomość z systemu backplane, umieszcza ją w lokalnej pamięci podręcznej. Serwer następnie dostarcza klientom z lokalnej pamięci podręcznej.

Dla każdego połączenia klienta jest śledzony postęp klienta podczas odczytu strumienia komunikatów przy użyciu kursora. (Kursora reprezentuje pozycją w strumieniu komunikat). Jeśli klient zakończy połączenie i następnie ponownie nawiązuje połączenie, zapyta magistrali jakiekolwiek komunikaty, które dotarły po wartości kursora klienta. To samo się stanie, jeśli połączenie wykorzystuje [długiego sondowania](../getting-started/introduction-to-signalr.md#transports). Po zakończeniu żądania długiej sondy klienta zostanie otwarte nowe połączenie i prosi o wiadomości, które dotarły po kursora.

Kursor działa mechanizm nawet wtedy, gdy klient jest kierowany do innego serwera na Połącz się ponownie. Systemu backplane zna wszystkie serwery i nie ma znaczenia, który serwer, klient nawiąże połączenie.

## <a name="limitations"></a>Ograniczenia

Przepustowość maksymalna wiadomości za pomocą środowiska IDE, jest mniejsze niż jest w przypadku klientów komunikować się bezpośrednio do węzła pojedynczego serwera. Wynika to z systemu backplane przekazuje każdej wiadomości do każdego węzła, więc systemu backplane może stać się wąskiego gardła. Czy to ograniczenie dotyczy problem, zależy od aplikacji. Na przykład poniżej przedstawiono kilka typowych scenariuszy SignalR:

- [Emisja serwera](tutorial-server-broadcast-with-aspnet-signalr.md) (np. giełdowych): montażowych działa dobrze w tym scenariuszu, ponieważ serwer określa szybkość, z jaką komunikaty.
- [Klient do klienta](tutorial-getting-started-with-signalr.md) (np. rozmowę): W tym scenariuszu systemu backplane może być wąskiego gardła, jeśli liczba komunikatów skaluje o liczbie klientów; oznacza to, jeśli liczba komunikatów rozwoju przyłączyć się proporcjonalnie jako większej liczby klientów.
- [Wysoka częstotliwość w czasie rzeczywistym](tutorial-high-frequency-realtime-with-signalr.md) (np. w czasie rzeczywistym gry): środowisku IDE nie jest zalecane dla tego scenariusza.

## <a name="enabling-tracing-for-signalr-scaleout"></a>Włączanie śledzenia dla skalowania SignalR

Aby włączyć śledzenie dla montażowych, Dodaj następujące sekcje do pliku web.config w katalogu głównym **konfiguracji** elementu:

[!code-html[Main](scaleout-in-signalr/samples/sample1.html)]
