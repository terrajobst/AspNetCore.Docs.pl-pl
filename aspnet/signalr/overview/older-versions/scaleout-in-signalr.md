---
uid: signalr/overview/older-versions/scaleout-in-signalr
title: Wprowadzenie do skalowania w poziomie w SignalR 1.x | Dokumentacja firmy Microsoft
author: MikeWasson
description: ''
ms.author: riande
ms.date: 04/29/2013
ms.assetid: 3fd9f11c-799b-4001-bd60-1e70cfc61c19
msc.legacyurl: /signalr/overview/older-versions/scaleout-in-signalr
msc.type: authoredcontent
ms.openlocfilehash: fffa424ea4b62a54b9df48aaa409541ab5d1608f
ms.sourcegitcommit: 74e3be25ea37b5fc8b4b433b0b872547b4b99186
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53287601"
---
<a name="introduction-to-scaleout-in-signalr-1x"></a>Wprowadzenie do skalowania w poziomie w usłudze SignalR 1.x
====================
przez [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

Ogólnie rzecz biorąc, istnieją dwa sposoby skalowania aplikacji sieci web: *skalowanie w górę* i *skalowanie w poziomie*.

- Skalowanie w górę oznacza, że przy większej serwerem (lub większych maszyn wirtualnych) więcej pamięci RAM, procesory itp.
- Skalowanie w poziomie oznacza, że dodawanie kolejnych serwerów do obsługi obciążenia.

Problem ze skalowaniem w jest szybko osiągnięty limit na rozmiar maszyny. Poza tym musisz skalować w poziomie. Jednak skalowanie w poziomie polega klientów można uzyskać skierować na różnych serwerach. Klient, który jest podłączony do jednego serwera nie będą otrzymywać wiadomości wysłanych z innego serwera.

![](scaleout-in-signalr/_static/image1.png)

Jedno rozwiązanie jest przesyłanie komunikatów między serwerami z użyciem składnik o nazwie *płyty montażowej*. Z płyty montażowej włączone każde wystąpienie aplikacji wysyła komunikaty do systemu backplane. Ponadto systemu backplane przekazuje je do innych wystąpień aplikacji. (W electronics, montażowa jest grupą łączników równoległych. Analogicznie montażowa SignalR nawiązuje połączenie z wieloma serwerami.)

![](scaleout-in-signalr/_static/image2.png)

Biblioteka SignalR udostępnia obecnie trzy montażowych:

- **Usługi Azure Service Bus**. Service Bus to infrastruktura obsługi komunikatów, umożliwiający składników do wysyłania wiadomości w swobodną.
- **Redis**. Redis jest przechowywanie par klucz wartość w pamięci. Usługa redis obsługuje wzorzec publikowania/subskrybowania ("pub/sub") do wysyłania wiadomości.
- **SQL Server**. Płyty montażowej programu SQL Server zapisuje komunikaty do tabel SQL. Systemu backplane używa brokera usług dla komunikatów wydajne. Jednak działa Jeśli programu Service Broker nie jest włączona.

W przypadku wdrożenia aplikacji na platformie Azure, należy wziąć pod uwagę przy użyciu usługi Azure Service Bus systemu backplane. Jeśli są wdrażane w farmie serwerów, należy wziąć pod uwagę programu SQL Server lub montażowych pamięci podręcznej Redis.

Samouczki krok po kroku dla każdej płyty montażowej można znaleźć w następujących tematach:

- [SignalR — skalowanie w poziomie z użyciem usługi Azure Service Bus](scaleout-with-windows-azure-service-bus.md)
- [SignalR — skalowanie w poziomie z użyciem pamięci podręcznej Redis](scaleout-with-redis.md)
- [SignalR — skalowanie w poziomie z użyciem programu SQL Server](scaleout-with-sql-server.md)

## <a name="implementation"></a>Implementacja

W SignalR każdy komunikat jest wysyłany za pośrednictwem magistrali komunikatów. Implementuje w magistrali komunikatów [IMessageBus](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.imessagebus(v=vs.100).aspx) interfejs, który udostępnia abstrakcji publikowania/subskrybowania. Montażowych pracy, zastępując domyślne **IMessageBus** z magistralą przeznaczone dla tego systemu backplane. Na przykład magistralę komunikatu dla pamięci podręcznej Redis jest [RedisMessageBus](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.redis.redismessagebus(v=vs.100).aspx), i wykorzystuje usługi Redis [publikowania/subskrybowania](http://redis.io/topics/pubsub) mechanizm do wysyłania i odbierania komunikatów.

Każde wystąpienie serwera łączy do systemu backplane za pośrednictwem magistrali. Po wysłaniu komunikatu przejdzie do systemu backplane, a następnie wysyła je do każdego serwera systemu backplane. Gdy serwer otrzymuje komunikat z systemu backplane, umieszcza ją w swojej lokalnej pamięci podręcznej. Serwer następnie dostarcza komunikaty do klientów z lokalnej pamięci podręcznej.

Dla każdego połączenia klienta postępu klienta podczas odczytu strumienia komunikatów są śledzone za pomocą kursora. (Kursor reprezentuje pozycji w strumieniu wiadomości). Jeśli klient rozłącza i następnie ponownie nawiązuje połączenie, prosi magistrali dla komunikatów, które odebrano po wartości kursora klienta. Tak samo się dzieje, gdy połączenie używa [długiego sondowania](../getting-started/introduction-to-signalr.md#transports). Po zakończeniu żądania długiej, klient zostanie otwarte nowe połączenie i prosi o wiadomości, które odebrano od kursora.

Kursor działa mechanizm nawet wtedy, gdy klient jest kierowany do innego serwera, na ponownie nawiąż połączenie. Systemu backplane zna wszystkie serwery i nie ma znaczenia, na który serwer, klient nawiąże połączenie.

## <a name="limitations"></a>Ograniczenia

Korzystając z płyty montażowej, przepływność komunikatów maksymalna jest niższa niż to, gdy klienci komunikować się bezpośrednio do węzła pojedynczego serwera. Wynika to z systemu backplane przekazuje każdy komunikat do każdego węzła, dzięki czemu systemu backplane może stać się wąskim gardłem. Czy to ograniczenie dotyczy problem, zależy od aplikacji. Na przykład poniżej przedstawiono kilka typowych scenariuszy SignalR:

- [Emisja serwera](tutorial-server-broadcast-with-aspnet-signalr.md) (np. giełdowej): Montażowych działa dobrze sprawdza się w tym scenariuszu, ponieważ serwer kontroluje szybkość, z jaką komunikaty są wysyłane.
- [Klient — klient](tutorial-getting-started-with-signalr.md) (np. chat): W tym scenariuszu systemu backplane może być "wąskie gardło", jeśli liczba komunikatów jest skalowana o liczbie klientów. oznacza to jeśli liczba komunikatów rośnie dołączyć proporcjonalnie, ponieważ coraz więcej klientów.
- [O wysokiej częstotliwości w czasie rzeczywistym](tutorial-high-frequency-realtime-with-signalr.md) (np. w czasie rzeczywistym gry): W tym scenariuszu nie zaleca się systemu backplane.

## <a name="enabling-tracing-for-signalr-scaleout"></a>Włączanie śledzenia SignalR — skalowanie w poziomie

Aby włączyć śledzenie montażowych, należy dodać następujące sekcje do pliku web.config w katalogu głównym **konfiguracji** elementu:

[!code-html[Main](scaleout-in-signalr/samples/sample1.html)]
