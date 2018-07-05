---
uid: signalr/overview/older-versions/handling-connection-lifetime-events
title: Opis i obsługa zdarzeń okresu istnienia połączenia w SignalR 1.x | Dokumentacja firmy Microsoft
author: pfletcher
description: W tym artykule opisano korzystanie ze zdarzeń udostępnianych przez interfejs API koncentratorów.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/05/2013
ms.topic: article
ms.assetid: e608e263-264d-448b-b0eb-6eeb77713b22
ms.technology: dotnet-signalr
msc.legacyurl: /signalr/overview/older-versions/handling-connection-lifetime-events
msc.type: authoredcontent
ms.openlocfilehash: 91303a9a1a507f3211f070194e7cce4ddf395e73
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37366431"
---
<a name="understanding-and-handling-connection-lifetime-events-in-signalr-1x"></a>Opis i obsługa zdarzeń okresu istnienia połączenia w SignalR 1.x
====================
przez [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)

> Ten artykuł zawiera omówienie SignalR połączenie, ponowne nawiązanie połączenia i rozłączenia zdarzenia, które może obsłużyć i ustawienia limitu czasu i utrzymywania aktywności, które można skonfigurować.
> 
> Tego artykułu przyjęto założenie, że masz już pewną wiedzę na temat zdarzenia okresu istnienia połączenia i SignalR. Wprowadzenie do SignalR, patrz [SignalR — Przegląd — wprowadzenie do](index.md). Wyświetla zdarzenia okresu istnienia połączenia na ten temat można znaleźć w następujących zasobach:
> 
> - [Jak obsługiwać zdarzenia okresu istnienia połączenia w klasie Centrum](index.md)
> - [Jak obsługiwać zdarzenia okresu istnienia połączenia w klientów języka JavaScript](index.md)
> - [Jak obsługiwać zdarzenia okresu istnienia połączenia w przypadku klientów programu .NET](index.md)


## <a name="overview"></a>Omówienie

Ten artykuł zawiera następujące sekcje:

- [Terminologia okres istnienia połączenia i scenariusze](#terminology)

    - [Połączenia SignalR, połączenia transportu i fizycznych połączeń](#signalrvstransport)
    - [Scenariusze rozłączenia transportu](#transportdisconnect)
    - [Scenariusze rozłączenia klienta](#clientdisconnect)
    - [Scenariusze odłączenia serwera](#serverdisconnect)
- [Ustawienia limitu czasu i utrzymywania aktywności](#timeoutkeepalive)

    - [ConnectionTimeout](#connectiontimeout)
    - [DisconnectTimeout](#disconnecttimeout)
    - [KeepAlive](#keepalive)
    - [Jak zmienić ustawienia limitu czasu i utrzymywania aktywności](#changetimeout)
- [Jak powiadamiać użytkownika o odłączenia](#notifydisconnect)
- [Jak ponownie w sposób ciągły](#continuousreconnect)
- [Jak odłączyć klienta w kodzie serwera](#disconnectclientfromserver)

Łącza do tematów, dokumentacja interfejsu API są .NET 4.5 wersję interfejsu API. Jeśli używasz programu .NET 4, zobacz [tematy interfejsu API w wersji .NET 4](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).

<a id="terminology"></a>

## <a name="connection-lifetime-terminology-and-scenarios"></a>Terminologia okres istnienia połączenia i scenariusze

`OnReconnected` Programu obsługi zdarzeń w Centrum SignalR można wykonać bezpośrednio po `OnConnected` , ale nie po `OnDisconnected` dla danego klienta. Przyczyna może mieć ponowne połączenie bez rozłączeniu zakłada, że kilka sposobów, w których wyraz "connection" jest używany w SignalR.

<a id="signalrvstransport"></a>

### <a name="signalr-connections-transport-connections-and-physical-connections"></a>Połączenia SignalR, połączenia transportu i fizycznych połączeń

Ten artykuł będzie odróżnić *połączeniami SignalR*, *transportu połączenia*, i *połączeń fizycznych*:

- **Połączenia SignalR** odwołuje się do relacji logicznych między klientem i adres URL serwera, obsługiwany przez interfejs API SignalR, unikatowo identyfikowana przez identyfikator połączenia. Dane dotyczące tej relacji jest obsługiwany przez SignalR, jest używany do ustanawiania połączenia transportu. Końce relacji i SignalR usuwa danych, gdy klient wywołuje `Stop` metody lub limit czasu zostanie osiągnięty podczas SignalR próby ponownego nawiązania połączenia transportu utracone.
- **Połączenie będzie transportowane** odwołuje się do relacji logicznych między klientem a serwerem, obsługiwane przez jedną z czterech transportu interfejsów API: WebSockets, zdarzenia wysłanego przez serwer, nieskończoność ramki lub długie sondowania. SignalR używa transportu interfejsu API można utworzyć połączenia transportowego i interfejsów API transportu zależy od istnienia połączenia sieci fizycznej, można utworzyć połączenia transportowego. Połączenie transportu kończy się, gdy SignalR kończy go lub transportu API wykryje, że fizyczne połączenie jest uszkodzone.
- **Fizyczne połączenie** odwołuje się do łącza sieci fizycznej — przewodów, sygnały sieci bezprzewodowej, routery, itp. — ułatwiają komunikację między komputerem klienckim a serwerem. Fizyczne połączenie musi być obecna, aby możliwe było nawiązanie połączenia transportu i można ustanowić połączenia transportu w celu ustanowienia połączenia SignalR. Jednak istotne fizycznego połączenia nie zawsze natychmiast kończy się połączenie transportu lub połączenia SignalR, jak opisano w dalszej części tego tematu.

Na poniższym diagramie połączenia SignalR jest reprezentowany przez koncentratory interfejsu API i PersistentConnection API SignalR warstwy połączenie transportu jest reprezentowany przez warstwę transportu i fizyczne połączenie jest reprezentowane przez linie między serwerem a klientami.

![Diagram architektury usługi SignalR](handling-connection-lifetime-events/_static/image1.png)

Gdy wywołujesz `Start` metody w kliencie SignalR, udostępniasz kod klienta SignalR wszystkie informacje, ile potrzebuje, aby ustanowić fizyczne połączenie z serwerem. Kod klienta SignalR używa tych informacji do przesyłania żądania HTTP i ustanowić połączenie fizycznej za pomocą jednej z metod transportu cztery. Jeśli połączenie transportu nie powiedzie się lub serwer nie powiedzie się, połączenia SignalR nie działa natychmiast od razu, ponieważ klient nadal zawiera informacje potrzebne do automatycznego ponownego nawiązywania nowego połączenia transportu do tego samego adresu URL SignalR. W tym scenariuszu nie interwencji z aplikacji użytkownika jest używana, a gdy kod klienta SignalR ustanawia nowego połączenia transportu, nie rozpoczyna nowe połączenia SignalR. Ciągłość działania połączenia SignalR znajduje odzwierciedlenie w rzeczywistości, identyfikator połączenia, który jest tworzony podczas wywoływania `Start` metody nie zmienia się.

`OnReconnected` Program obsługi zdarzeń w Centrum wykonuje podczas transportu ponownym nawiązaniu połączenia automatycznie po została utracona. `OnDisconnected` Program obsługi zdarzeń uruchamia się na końcu połączenia SignalR. Połączenia SignalR można zakończyć w dowolnym z następujących sposobów:

- Jeśli klient wywołuje `Stop` metody, komunikat o zatrzymaniu są wysyłane do serwera i klienta i serwera zakończenia połączenia SignalR natychmiast.
- Po utraty łączności między klientem i serwerem klient próbuje połączyć, a serwer oczekuje na klienta w celu ponownego nawiązania połączenia. Jeśli prób ponownego połączenia nie zakończy się pomyślnie, a kończy się w określonym przedziale czasu rozłączenia, klienta i serwera zakończenia połączenia SignalR. Zatrzymuje się próby nawiązania ponownego połączenia klienta i serwera usuwa jego reprezentację połączenia SignalR.
- Jeśli klient zostanie zatrzymana, bez szansę, aby wywołać `Stop` metody dla klienta w celu ponownego nawiązania połączenia, czeka serwer, a następnie kończy połączenia SignalR, po upływie limitu czasu rozłączenia.
- Jeśli zatrzymuje serwera, uruchamianie, klient próbuje ponownie połączyć (ponownie utworzyć połączenia transportowego), a następnie kończy połączenia SignalR, po upływie limitu czasu rozłączenia.

Gdy nie ma żadnych problemów połączenia i aplikacji użytkownika kończy się połączenia SignalR, wywołując `Stop` metody, połączenia SignalR i połączenie transportu rozpocząć i zakończyć w tym samym czasie. Poniższe sekcje bardziej szczegółowo opisano inne scenariusze.

<a id="transportdisconnect"></a>

### <a name="transport-disconnection-scenarios"></a>Scenariusze rozłączenia transportu

Fizyczne połączenia może być wolne lub mogą wystąpić zakłócenia w łączności. W zależności od takich czynników, takich jak długość przerwania mogą być opuszczane połączenie transportu. SignalR próbuje ponownie ustanowić połączenie transportu. Czasami interfejs API połączenia transportu wykrywa czas przestoju i odrzuca połączenie transportu i SignalR dowiaduje się natychmiast, połączenie zostanie przerwane. W innych scenariuszach interfejs API połączenia transportu ani SignalR uświadamia sobie, natychmiastowe połączenie zostało utracone. Dla wszystkich transportów na z wyjątkiem długiego sondowania klienta SignalR używa funkcji o nazwie *keepalive* pod kątem utraty łączności transportu interfejsu API nie jest w stanie wykryć. Aby uzyskać informacji na temat długo sondowania połączeń, zobacz [ustawienia limitu czasu i utrzymywania aktywności](#timeoutkeepalive) w dalszej części tego tematu.

Gdy połączenie jest nieaktywny, okresowo serwer wysyła pakiet keepalive do klienta. Począwszy od daty, który jest zapisywany w tym artykule domyślna częstotliwość to co 10 sekund. Przez nasłuchiwanie dla tych pakietów, klientów można określić, czy występuje problem z połączeniem. Jeśli nie zostanie odebrany pakiet keepalive, gdy oczekiwano, po pewnym czasie klient przyjmie założenie, że nie istnieją problemy z połączeniem takich jak powolność lub przerw w działaniu. Jeśli keepalive nadal nie zostanie odebrany po dłuższy czas, klient przyjmie założenie, że połączenie zostało porzucone, a jego rozpoczęciem próby nawiązania ponownego połączenia.

Na poniższym diagramie przedstawiono zdarzeń klienta i serwera, które są wywoływane w typowym scenariuszu, gdy występują problemy z fizycznego połączenia, które natychmiast nie są rozpoznawane przez transportu interfejsu API. Diagram ma zastosowanie do następujących okolicznościach:

- Transport jest WebSockets, nieskończoność ramkę lub zdarzenia wysłanego przez serwer.
- Istnieją różne okresy przerwy w połączeniu sieci fizycznej.
- Transport interfejsu API nie zostaną powiadomieni o przerwami w działaniu, dzięki czemu SignalR opiera się na funkcjach keepalive w celu jego wykrycia.

![Odłączenia transportu](handling-connection-lifetime-events/_static/image2.png)

Jeśli klient przechodzi do ponownego połączenia trybu, ale nie można ustanowić połączenia transportu, w ramach limit czasu rozłączenia, serwer kończy połączenia SignalR. Jeśli tak się stanie, serwer wykonuje Centrum `OnDisconnected` metody i kolejki rozłączenia komunikat do wysłania do klienta, w przypadku, gdy klient zarządza połączyć się później. Jeśli klient ponowne, odbierze polecenie rozłączenia i wywołania `Stop` metody. W tym scenariuszu `OnReconnected` nie jest wykonywany, gdy klient ponownie nawiąże połączenie, i `OnDisconnected` nie jest wykonywany, gdy klient wywołuje `Stop`. Na poniższym diagramie przedstawiono w tym scenariuszu.

![Zakłócenia transportu — limit czasu serwera](handling-connection-lifetime-events/_static/image3.png)

Zdarzenia okresu istnienia połączenia SignalR, które mogą być wywoływane na komputerze klienckim, są następujące:

- `ConnectionSlow` Zdarzenie klienta.

    Wywoływane, gdy wstępnie zdefiniowane część keepalive limit czasu minęło od czasu ostatniego komunikatu lub keepalive ping zostało przesłane. Domyślny okres ostrzeżenie limitu czasu utrzymywania aktywności jest 2/3 limitu czasu utrzymywania aktywności. Limit czasu utrzymywania aktywności jest 20 sekund, więc ostrzeżenie pojawia się na około 13 sekundach.

    Domyślnie serwer wysyła pakiety ping keepalive co 10 sekund, a klient sprawdza, czy pakiety utrzymywania aktywności o co 2 sekundy (1/3 różnicy między wartością limitu czasu keepalive keepalive wartość Ostrzeżenie limitu czasu).

    Jeśli interfejs API transportu uzyska informacje o rozłączeniu, SignalR może informowany o odłączenie przed przekazuje okres ostrzeżenie limitu czasu utrzymywania aktywności. W takim przypadku `ConnectionSlow` zdarzenie nie będzie wywoływane i SignalR musieli przejść bezpośrednio do `Reconnecting` zdarzeń.
- `Reconnecting` Zdarzenie klienta.

    Wywoływane, gdy (transportu interfejsu API wykryje, czy połączenie zostało utracone lub (b) keepalive limitu czasu minęło od czasu ostatniego komunikatu lub keepalive ping zostało przesłane. Kod klienta SignalR podejmuje próbę ponownego połączenia. To zdarzenie może obsłużyć, jeśli chcesz, aby aplikacja wykonania określonego działania, gdy transport połączenie zostanie przerwane. Domyślny okres limitu czasu utrzymywania aktywności jest obecnie 20 sekund.

    Jeśli kod klienta próbuje wywołać metodę koncentratora, gdy trwa ponowne nawiązywanie połączenia trybu SignalR, SignalR spróbuj wysłać polecenie. W większości przypadków, takie próby zakończą się niepowodzeniem, ale w niektórych sytuacjach może się powieść. Serwer wysłał zdarzenia, nieskończoność ramki i długi transport sondowania SignalR używa dwa kanały komunikacyjne: jeden, używanego przez klienta do wysyłania komunikatów i jeden, używaną do odbierania komunikatów. Kanał używany do odbierania jest stale otwarte i jest ten, który jest zamykane, gdy fizyczne połączenie zostanie przerwane. Kanał, używane na potrzeby wysyłania pozostaje dostępna, więc jeśli zostanie przywrócona fizycznej łączności, wywołanie metody z klienta do serwera może zakończyć się pomyślnie przed kanału receive zostanie ponownie nawiązane. Wartość zwracana nie będą odbierane, dopóki SignalR ponownie zostanie otwarty kanał używany do odbierania.
- `Reconnected` Zdarzenie klienta.

    Wywoływane po ustanowieniu połączenia transportu. `OnReconnected` Wykonuje program obsługi zdarzeń w Centrum.
- `Closed` Zdarzenie klienta (`disconnected` zdarzeń w języku JavaScript).

    Wywoływane, gdy rozłączenia upłynięcia limitu czasu podczas, gdy kod klienta SignalR jest próby nawiązania ponownego połączenia po utracie połączenia transportu. Odłącz domyślny limit czasu wynosi 30 sekund. (To zdarzenie jest również zgłaszane w przypadku połączenia zakończy się, ponieważ `Stop` metoda jest wywoływana.)

Transport połączenia przerw w działaniu, które nie są wykrywane przez transportu interfejsu API i nie zwlekaj odbiór keepalive ping z serwera przez czas dłuższy niż limit czasu ostrzeżenia keepalive nie spowodować dowolnego połączenia, zdarzenia okresu istnienia zgłoszenie.

Niektóre środowiska sieciowe celowo Zamknij bezczynnych połączeń, a inną funkcję pakiety utrzymywania aktywności jest aby temu zapobiec, dzięki czemu będzie tych sieciach wiedzieć, że połączenia SignalR jest używany. W skrajnych przypadkach domyślnej częstotliwości równej keepalive ping może nie być wystarczające, aby zapobiec zamkniętego połączenia. W takim przypadku można skonfigurować keepalive ping do wysłania częściej. Aby uzyskać więcej informacji, zobacz [ustawienia limitu czasu i utrzymywania aktywności](#timeoutkeepalive) w dalszej części tego tematu.

> [!NOTE] 
> 
> [!IMPORTANT]
> Kolejność zdarzeń opisane w tym miejscu nie jest gwarantowana. SignalR sprawia, że każda próba zgłaszać zdarzenia okresu istnienia połączenia w sposób przewidywalny zgodnie z tym schemacie, ale istnieją wielu wariantów zdarzenia sieci i wiele sposobów, w których podstawowych struktur komunikacji, takich jak transportu interfejsy API z je obsłużyć. Na przykład `Reconnected` zdarzeń nie może zostać wywołane, gdy klient ponownie nawiąże połączenie, lub `OnConnected` obsługi na serwerze może być uruchomiony, gdy próba nawiązania połączenia zakończy się niepowodzeniem. W tym temacie opisano tylko efekty, zwykle produkcji przez niektórych typowych okolicznościach.


<a id="clientdisconnect"></a>

### <a name="client-disconnection-scenarios"></a>Scenariusze rozłączenia klienta

W kliencie przeglądarki kod klienta SignalR, który obsługuje połączenia SignalR jest uruchamiany w kontekście JavaScript, strony sieci web. Który ma Dlaczego musi się kończyć, kiedy przejść z jednego połączenia SignalR strony do innej, a także że użytkownika dlaczego masz wiele połączeń z wielu identyfikatorów połączeń. Jeśli łączysz się z wielu okna przeglądarki lub karty. Gdy użytkownik zamyka okna lub karty przeglądarki, lub przechodzi do nowej strony lub odświeża stronę, połączenia SignalR natychmiast zakończy się, ponieważ kod klienta SignalR zdarzenie przeglądarka obsługuje i wywołuje `Stop` metody. W tych scenariuszach lub dowolnej platformie klienta, gdy Twoja aplikacja wywołuje `Stop` metody `OnDisconnected` programu obsługi zdarzeń jest wykonywana bezpośrednio na serwerze, i zgłasza klienta `Closed` zdarzeń (zdarzenie jest nazywane `disconnected` w JavaScript).

Jeśli aplikacja kliencka lub komputera, na którym jest uruchomiona na awarie lub przechodzi w stan uśpienia (na przykład, gdy użytkownik zamknie komputera przenośnego), serwer nie jest informowany o co się stało. Chodzi o serwer żądał, utrata klienta może wynikać ze przerwanie łączności i klienta może być próby nawiązania ponownego połączenia. Dlatego w tych scenariuszach, czeka serwer, aby zaoferować klientowi produkt szansę, aby połączyć ponownie a `OnDisconnected` nie jest wykonywane do momentu rozłączenia upłynięcia limitu czasu (około 30 sekund domyślnie). Na poniższym diagramie przedstawiono w tym scenariuszu.

![Niepowodzenie komputera klienta](handling-connection-lifetime-events/_static/image4.png)

<a id="serverdisconnect"></a>

### <a name="server-disconnection-scenarios"></a>Scenariusze odłączenia serwera

Gdy serwer przejdzie do trybu offline — ponowne uruchomienie, zakończy się niepowodzeniem, domena aplikacji odtwarzanie itd. — wynik może być podobny do utracono połączenie lub transportu interfejsu API i SignalR może od razu wiedzieć, że serwer został usunięty, a SignalR może zacząć próby nawiązania ponownego połączenia bez wywoływanie `ConnectionSlow` zdarzeń. Jeśli klient przechodzi do ponownego połączenia trybu i odzyskuje serwer lub ponownego uruchamiania lub nowy serwer w tryb online przed upływem limitu czasu rozłączenia, klient nawiąże połączenie do przywróconej lub nowego serwera. W takim przypadku nadal połączenia SignalR na komputerze klienckim i `Reconnected` zdarzenie jest wywoływane. Na pierwszym serwerze `OnDisconnected` nigdy nie jest wykonywane i na nowym serwerze `OnReconnected` jest wykonywany, mimo że `OnConnected` nigdy nie została wykonana dla tego klienta na tym serwerze przed. (Efekt jest taki sam, gdy klient ponownie nawiąże połączenie z tym samym serwerem po ponownym uruchomieniu lub aplikacji domeny, ponieważ po ponownym uruchomieniu serwera go ma Brak pamięci działania poprzednie połączenie.) Poniższy diagram przyjęto założenie, że transportu interfejsu API uzyska informacje o utracie połączenia natychmiast, więc `ConnectionSlow` zdarzenie jest zgłaszane w nie.

![Awaria serwera i ponowne nawiązanie połączenia](handling-connection-lifetime-events/_static/image5.png)

Jeśli serwer nie są dostępne przed upływem limitu czasu rozłączenia, kończy się połączenia SignalR. W tym scenariuszu `Closed` zdarzeń (`disconnected` w klientów języka JavaScript) jest uruchamiany na komputerze klienckim, ale `OnDisconnected` nigdy nie jest wywoływana na serwerze. Poniższy diagram przyjęto założenie, że transportu interfejsu API nie zostaną powiadomieni o utracono połączenie, zostanie on wykryty przez funkcje keepalive SignalR i `ConnectionSlow` zdarzenie jest wywoływane.

![Awaria serwera i limitu czasu](handling-connection-lifetime-events/_static/image6.png)

<a id="timeoutkeepalive"></a>

## <a name="timeout-and-keepalive-settings"></a>Ustawienia limitu czasu i utrzymywania aktywności

Wartość domyślna `ConnectionTimeout`, `DisconnectTimeout`, i `KeepAlive` wartości są odpowiednie dla większości scenariuszy, ale można ją zmienić, jeśli w środowisku zawierającym specjalnymi potrzebami. Na przykład jeśli środowisko sieciowe zamyka połączenia, które są w stanie bezczynności przez 5 sekund, może być konieczne Zmniejsz wartość utrzymywania aktywności.

<a id="connectiontimeout"></a>

### <a name="connectiontimeout"></a>ConnectionTimeout

To ustawienie oznacza ilość czasu, aby pozostawić połączenie transportu, Otwórz i oczekuje na odpowiedź przed jego zamknięciem i otwarciem nowego połączenia. Wartość domyślna wynosi 110 sekund.

To ustawienie dotyczy tylko kiedy keepalive funkcja jest wyłączona, co zwykle ma zastosowanie tylko długiego transportu sondowania. Na poniższym diagramie przedstawiono efekt tego ustawienia, na czas sondowania połączenie transportu.

![Czas połączenia transportu sondowania](handling-connection-lifetime-events/_static/image7.png)

<a id="disconnecttimeout"></a>

### <a name="disconnecttimeout"></a>DisconnectTimeout

To ustawienie oznacza ilość czasu połączenia transportu są tracone przed zgłoszeniem `Disconnected` zdarzeń. Wartość domyślna to 30 sekund. Po ustawieniu `DisconnectTimeout`, `KeepAlive` jest automatycznie ustawiona na 1/3 `DisconnectTimeout` wartość.

<a id="keepalive"></a>

### <a name="keepalive"></a>KeepAlive

To ustawienie oznacza ilość czasu oczekiwania przed wysłaniem pakietu keepalive bezczynnego połączenia. Wartość domyślna to 10 sekund. Ta wartość nie może być więcej niż 1/3 `DisconnectTimeout` wartość.

Jeśli chcesz ustawić zarówno `DisconnectTimeout` i `KeepAlive`ustaw `KeepAlive` po `DisconnectTimeout`. W przeciwnym razie swoje `KeepAlive` ustawienia zostaną zastąpione podczas `DisconnectTimeout` automatycznie ustawia `KeepAlive` 1/3 wartości limitu czasu.

Jeśli chcesz wyłączyć funkcję keepalive `KeepAlive` na wartość null. Funkcje Keepalive jest automatycznie wyłączana dla długiego transportu sondowania.

<a id="changetimeout"></a>

### <a name="how-to-change-timeout-and-keepalive-settings"></a>Jak zmienić ustawienia limitu czasu i utrzymywania aktywności

Aby zmienić wartości domyślne dla tych ustawień, należy ustawić je w `Application_Start` w swojej *Global.asax* pliku, jak pokazano w poniższym przykładzie. Wartości podane w przykładowym kodzie są takie same jak wartości domyślne.

[!code-csharp[Main](handling-connection-lifetime-events/samples/sample1.cs)]

<a id="notifydisconnect"></a>

## <a name="how-to-notify-the-user-about-disconnections"></a>Jak powiadamiać użytkownika o odłączenia

W niektórych aplikacjach możesz chcieć wyświetlić komunikat dla użytkownika, gdy występują problemy z łącznością. Masz kilka opcji, jak i kiedy mają to robić. Poniższe przykłady kodu są dla klienta kodu JavaScript przy użyciu wygenerowany serwer proxy.

- Obsługa `connectionSlow` zdarzenie, aby wyświetlić komunikat, jak SignalR to znane problemy z połączeniem, zanim usługa zostanie wprowadzona do ponownego połączenia trybu.

    [!code-javascript[Main](handling-connection-lifetime-events/samples/sample2.js)]
- Obsługa `reconnecting` zdarzenie, aby wyświetlić komunikat, gdy SignalR zna rozłączeniu i przechodzi do ponownego połączenia trybu.

    [!code-javascript[Main](handling-connection-lifetime-events/samples/sample3.js)]
- Obsługa `disconnected` zdarzenie, aby wyświetlić komunikat informujący o tym, gdy próba ponownego połączenia został przekroczony. W tym scenariuszu, jedynym sposobem, aby ponownie ustanowić połączenia z serwerem ponownie jest ponowne uruchomienie połączenia SignalR, wywołując `Start` metody, która utworzy nowy identyfikator połączenia. Poniższy przykład kodu używa flagi, aby zapewnić, generowaniu powiadomienia tylko po upływie ponownie nawiązującego połączenie limitu czasu, nie po normalne zakończenia połączenia SignalR, spowodowane przez wywołanie metody `Stop` metody.

    [!code-javascript[Main](handling-connection-lifetime-events/samples/sample4.js)]

<a id="continuousreconnect"></a>

## <a name="how-to-continuously-reconnect"></a>Jak ponownie w sposób ciągły

W niektórych aplikacjach można automatycznie ponownie ustanowić połączenia, po zostało utracone i ponowić próbę połączenia został przekroczony. Aby to zrobić, wywołując `Start` metody z Twojej `Closed` programu obsługi zdarzeń (`disconnected` programu obsługi zdarzeń na komputerach klienckich JavaScript). Należy poczekać czasie przed wywołaniem `Start` aby zapobiec temu zbyt często kiedy serwer lub fizycznego połączenia są niedostępne. Poniższy przykładowy kod dotyczy klienta języka JavaScript przy użyciu wygenerowany serwer proxy.

[!code-javascript[Main](handling-connection-lifetime-events/samples/sample5.js)]

Potencjalny problem pod uwagę w przypadku klientów mobilnych to, że prób ciągłe ponowne nawiązanie połączenia, gdy serwer lub fizyczne połączenie jest niedostępny, może spowodować zużycie baterii niepotrzebne.

<a id="disconnectclientfromserver"></a>

## <a name="how-to-disconnect-a-client-in-server-code"></a>Jak odłączyć klienta w kodzie serwera

SignalR wersji 1.1.1 nie ma wbudowanego serwera interfejsu API dla rozłączanie klientów. Istnieją [plany, aby dodać tę funkcję w przyszłości](https://github.com/SignalR/SignalR/issues/2101). W bieżącej wersji biblioteki SignalR Najprostszym sposobem, aby odłączyć klienta z serwera jest zaimplementować metodę rozłączenia na kliencie i wywoływanie tej metody z serwera. Poniższy przykładowy kod przedstawia metodę rozłączenia dla klienta kodu JavaScript przy użyciu wygenerowany serwer proxy.

[!code-javascript[Main](handling-connection-lifetime-events/samples/sample6.js)]

> [!WARNING]
> Zabezpieczenia — ta metoda rozłączanie klientów ani proponowane wbudowanego interfejsu API pozwala sprostać scenariusza zaatakowanych przez hakerów klientów z systemem złośliwego kodu, ponieważ klientów można się ponownie lub może spowodować usunięcie kodu zaatakowanych przez hakerów `stopClient` metody lub zmian Poznaj możliwości. Właściwym miejscem do implementowania stanowych ochrony (DOS) typu "odmowa usługi" jest nie w ramach lub warstwa serwera, ale raczej w infrastrukturze frontonu.
