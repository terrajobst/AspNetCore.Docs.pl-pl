---
uid: signalr/overview/performance/scaleout-in-signalr
title: Wprowadzenie do skalowania w poziomie w SignalR | Dokumentacja firmy Microsoft
author: MikeWasson
description: Wersje oprogramowania, używaną w tym temacie dodatku .NET 4.5 SignalR dla programu Visual Studio 2013 w wersji 2, poprzednie wersje w tym temacie informacji o wcześniejszych wersjach...
ms.author: aspnetcontent
ms.date: 06/10/2014
ms.assetid: 7e781fc1-1c1f-45a8-bc1d-338e96dbe9c9
msc.legacyurl: /signalr/overview/performance/scaleout-in-signalr
msc.type: authoredcontent
ms.openlocfilehash: 3215d61c04222632b3fae1079184e5cbf03708e8
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37807700"
---
<a name="introduction-to-scaleout-in-signalr"></a>Wprowadzenie do skalowania w poziomie w SignalR
====================
przez [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)

> ## <a name="software-versions-used-in-this-topic"></a>Wersje oprogramowania używaną w tym temacie
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR w wersji 2
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a>Poprzednie wersje tego tematu
> 
> Aby uzyskać informacje dotyczące starszych wersji biblioteki SignalR, zobacz [starsze wersje biblioteki SignalR](../older-versions/index.md).
> 
> ## <a name="questions-and-comments"></a>Pytania i komentarze
> 
> Jak się podoba w tym samouczku, i co można było ulepszyć proces w komentarzach u dołu strony, wystaw opinię. Jeśli masz pytania, na które nie są bezpośrednio związane z tego samouczka, możesz zamieścić je do [forum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) lub [StackOverflow.com](http://stackoverflow.com/).


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
- **Program SQL Server**. Płyty montażowej programu SQL Server zapisuje komunikaty do tabel SQL. Systemu backplane używa brokera usług dla komunikatów wydajne. Jednak działa Jeśli programu Service Broker nie jest włączona.

W przypadku wdrożenia aplikacji na platformie Azure, należy wziąć pod uwagę przy użyciu usługi Redis płyty montażowej [usługi Azure Redis Cache](https://azure.microsoft.com/services/cache/). Jeśli są wdrażane w farmie serwerów, należy wziąć pod uwagę programu SQL Server lub montażowych pamięci podręcznej Redis.

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

- [Emisja serwera](../getting-started/tutorial-server-broadcast-with-signalr.md) (np. giełdowej): montażowych zadziałać dla tego scenariusza, ponieważ serwer kontroluje szybkość, z jaką komunikaty są wysyłane.
- [Klient do klienta](../getting-started/tutorial-getting-started-with-signalr.md) (np. chat): W tym scenariuszu systemu backplane może być "wąskie gardło", jeśli liczba komunikatów jest skalowana o liczbie klientów; oznacza to, jeśli liczba komunikatów rośnie przyłączyć proporcjonalnie, ponieważ coraz więcej klientów.
- [O wysokiej częstotliwości w czasie rzeczywistym](../getting-started/tutorial-high-frequency-realtime-with-signalr.md) (np. w czasie rzeczywistym gry): płyty montażowej nie jest zalecane w przypadku tego scenariusza.

## <a name="enabling-tracing-for-signalr-scaleout"></a>Włączanie śledzenia SignalR — skalowanie w poziomie

Aby włączyć śledzenie montażowych, należy dodać następujące sekcje do pliku web.config w katalogu głównym **konfiguracji** elementu:

[!code-html[Main](scaleout-in-signalr/samples/sample1.html)]
