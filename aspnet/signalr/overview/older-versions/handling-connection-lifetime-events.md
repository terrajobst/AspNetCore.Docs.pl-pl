---
uid: signalr/overview/older-versions/handling-connection-lifetime-events
title: "Opis i obsługa zdarzeń okres istnienia połączenia SignalR 1.x | Dokumentacja firmy Microsoft"
author: pfletcher
description: "W tym artykule opisano sposób użycia udostępniany przez interfejs API centrów zdarzeń."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/05/2013
ms.topic: article
ms.assetid: e608e263-264d-448b-b0eb-6eeb77713b22
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/handling-connection-lifetime-events
msc.type: authoredcontent
ms.openlocfilehash: db29c3382895ef4d7efc3a686fa558189c8788de
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="understanding-and-handling-connection-lifetime-events-in-signalr-1x"></a>Opis i obsługa zdarzeń okres istnienia połączenia SignalR 1.x
====================
przez [Patrick Fletcher](https://github.com/pfletcher), [Dykstra niestandardowy](https://github.com/tdykstra)

> Ten artykuł zawiera omówienie SignalR połączenia, ponowne nawiązanie połączenia i rozłączenia zdarzenia, które może obsłużyć i ustawienia limitu czasu i utrzymywania aktywności, które można skonfigurować.
> 
> Artykuł przyjęto założenie, że masz już pewna znajomość zdarzenia okresu istnienia połączenia i SignalR. Aby obejrzeć wprowadzenie do SignalR, zobacz [SignalR — Przegląd — wprowadzenie](index.md). Dla listy zdarzeń okres istnienia połączenia zobacz następujące zasoby:
> 
> - [Sposób obsługi zdarzeń okres istnienia połączenia w klasie Centrum](index.md)
> - [Sposób obsługi zdarzeń okres istnienia połączenia klientów JavaScript](index.md)
> - [Sposób obsługi zdarzeń okres istnienia połączenia klientów platformy .NET](index.md)


## <a name="overview"></a>Omówienie

Ten artykuł zawiera następujące sekcje:

- [Terminologia okres istnienia połączenia i scenariusze](#terminology)

    - [Połączenia SignalR, połączenia transportu i fizycznego połączenia](#signalrvstransport)
    - [Scenariusze rozłączenia transportu](#transportdisconnect)
    - [Scenariusze rozłączenia klienta](#clientdisconnect)
    - [Scenariusze serwera rozłączenia](#serverdisconnect)
- [Ustawienia limitu czasu i utrzymywania aktywności](#timeoutkeepalive)

    - [ConnectionTimeout](#connectiontimeout)
    - [Wartości DisconnectTimeout](#disconnecttimeout)
    - [Utrzymywania aktywności](#keepalive)
    - [Jak zmienić ustawienia limitu czasu i utrzymywania aktywności](#changetimeout)
- [Jak użytkownicy są powiadamiani o rozłączeń](#notifydisconnect)
- [Jak połączyć się ponownie w sposób ciągły](#continuousreconnect)
- [Jak można rozłączyć klienta w kodzie serwera](#disconnectclientfromserver)

Linki do tematów dokumentacji interfejsu API są wersja platformy .NET 4.5 interfejsu API. Jeśli używasz programu .NET 4, zobacz [wersji .NET 4 tematy interfejsu API](https://msdn.microsoft.com/en-us/library/jj891075(v=vs.100).aspx).

<a id="terminology"></a>

## <a name="connection-lifetime-terminology-and-scenarios"></a>Terminologia okres istnienia połączenia i scenariusze

`OnReconnected` Obsługi zdarzeń w Centrum SignalR można wykonywać bezpośrednio po `OnConnected` , ale nie po `OnDisconnected` dla danego klienta. Przyczyną może być ponowne połączenie bez zakończenia połączenia jest czy istnieje kilka sposobów, w których używany jest program word "połączenia" w bibliotece SignalR.

<a id="signalrvstransport"></a>

### <a name="signalr-connections-transport-connections-and-physical-connections"></a>Połączenia SignalR, połączenia transportu i fizycznego połączenia

W tym artykule będzie odróżnienie *połączeniami SignalR*, *transportu połączenia*, i *połączeń fizycznych*:

- **Połączenia SignalR** odwołuje się do relacji logicznych między klientem a adresu URL serwera obsługiwane przez interfejs API SignalR i unikatowo identyfikowana przez identyfikator połączenia. Dane dotyczące tej relacji jest obsługiwany przez SignalR, jest używany do ustanawiania połączenia transportowego. Końców relacji i SignalR usuwa dane, gdy klient wywołuje `Stop` metody lub limit czasu zostanie osiągnięty podczas SignalR próbuje ponownie ustanowić połączenia transportu utracone.
- **Połączenia transportu** odwołuje się do relacji logicznych między klientem a serwerem, obsługiwane przez jedną z czterech transportu interfejsów API: Websocket, zdarzenia wysłanego przez serwer, zawsze ramki lub długie sondowania. SignalR używa transportu interfejsu API w celu utworzenia połączenia transportu i interfejsu API transportu zależy od istnienia połączenia sieci fizycznej można utworzyć połączenia transportowego. Połączenie transportu kończy się po SignalR kończy je lub gdy transportu interfejsu API wykryje, że fizyczne połączenie zostanie przerwane.
- **Połączenie fizyczne** odwołuje się do łącza sieci fizycznej--przewodów sygnały sieci bezprzewodowej, routery, itp.--komunikację między komputerem klienckim a na serwerze. Fizycznego połączenia musi być obecny w celu nawiązania połączenia transportu i można ustanowić połączenia transportu w celu nawiązania połączenia SignalR. Jednak fundamentalne fizycznego połączenia nie zawsze natychmiast zakończyć połączenie transportu lub połączenia SignalR, jak opisano w dalszej części tego tematu.

Na poniższym diagramie połączenia SignalR jest reprezentowany przez interfejs API koncentratorów i klasy PersistentConnection SignalR interfejsu API warstwy, połączenie transportu jest reprezentowany przez warstwę transportu i fizycznego połączenia jest reprezentowana przez linie między serwerem i klientów.

![Diagram architektury SignalR](handling-connection-lifetime-events/_static/image1.png)

Podczas wywoływania `Start` metody klienta SignalR, czy podajesz kod klienta SignalR wszystkich informacji wymaganych w celu ustalenia fizycznego połączenia z serwerem. Kod klienta SignalR używa tych informacji do przesyłania żądania HTTP i nawiązywać połączenie fizyczne za pomocą jednej z metod transportu cztery. Jeśli połączenie transportu nie powiedzie się lub serwer nie powiedzie się, połączenia SignalR nie zniknie natychmiast, ponieważ klient nadal zawiera informacje potrzebne do przywrócenia automatycznie nowe połączenie transportu do tego samego adresu URL SignalR. W tym scenariuszu uczestniczy interwencji z aplikacji użytkownika, a gdy kod klienta SignalR ustanawia nowego połączenia transportu, nie uruchamia nowego połączenia SignalR. Ciągłość połączenia SignalR znajduje odzwierciedlenie w fakcie który identyfikator połączenia, który jest tworzony podczas wywoływania `Start` metody, nie ulega zmianie.

`OnReconnected` Obsługi zdarzeń w koncentratorze wykonuje się, gdy po za utracone jest automatycznie ponownie nawiązać połączenie transportu. `OnDisconnected` Program obsługi zdarzeń jest wykonywana na końcu połączenia SignalR. Połączenia SignalR można zakończyć w dowolnym z następujących sposobów:

- Jeśli klient wywołuje `Stop` metody, komunikat o zatrzymaniu są wysyłane do serwera i klienta i serwera zakończenia połączenia SignalR natychmiast.
- Po łączności między klientem a serwerem zostaną utracone, klient próbuje połączyć się ponownie, a serwer oczekiwanie klienta do ponownego połączenia. Jeśli limit czasu rozłączenia kończy się podejmuje próbę ponownego połączenia nie zakończy się pomyślnie, klienta i serwera zakończenia połączenia SignalR. Klient przestaje próba ponownego połączenia, a serwer usuwa jej reprezentację połączenia SignalR.
- Jeśli klient zatrzymane, bez konieczności możliwość wywołania `Stop` oczekiwania przez serwer metody dla klienta ponownie połączyć i kończy się po upływie limitu czasu rozłączenia połączenia SignalR.
- Jeśli zatrzymuje serwera, uruchamianie, klient próbuje ponownie (ponownie utworzyć połączenie transportu), a następnie kończy się po upływie limitu czasu rozłączenia połączenia SignalR.

Gdy nie ma żadnych problemów połączenia i aplikacji użytkownika kończy się połączenia SignalR, wywołując `Stop` metody, połączenia SignalR i połączenie transportu begin i końcu o tym samym czasie. W poniższych sekcjach opisano szczegółowo w innych scenariuszach.

<a id="transportdisconnect"></a>

### <a name="transport-disconnection-scenarios"></a>Scenariusze rozłączenia transportu

Połączeń fizycznych może być wolne lub może być przerw w łączności. W zależności od czynników, takich jak długość przerwania mogą być opuszczane połączenie transportu. SignalR próbuje ponownie ustanowić połączenie transportu. Czasami połączenia transportowego interfejsu API wykrywa czas przestoju i porzuca połączenie transportu i SignalR znajduje natychmiast że połączenie zostało utracone. W innych scenariuszach połączenia transportowego interfejsu API ani SignalR uzyska natychmiast połączenie zostało utracone. Dla wszystkich transportów z wyjątkiem długiego sondowania, klient SignalR używa funkcji o nazwie *keepalive* do sprawdzenia utraty połączenia, który transportu interfejsu API nie jest w stanie wykryć. Informacje o długie sondowania połączeń, zobacz [ustawienia limitu czasu i utrzymywania aktywności](#timeoutkeepalive) dalszej części tego tematu.

Gdy połączenie jest nieaktywny, okresowo serwer wysyła pakiet keepalive do klienta. Informacje w dniu, który jest zapisywany w tym artykule domyślna częstotliwość wynosi co 10 sekund. Przez nasłuchiwanie dla tych pakietów, klientów można stwierdzić, czy istnieje problem z połączeniem. Jeśli pakiet keepalive nie otrzymano oczekiwanej chwili, po pewnym czasie klient przyjmie założenie, że istnieją problemy z połączeniem takich jak powolność lub przerw w działaniu. Jeśli wartości keepalive nadal nie zostanie odebrane po dłużej klienta założono, że połączenie zostało przerwane i zaczyna się próba ponownego połączenia.

Na poniższym diagramie przedstawiono zdarzeń klienta i serwera, które są wywoływane w typowy scenariusz, gdy występują problemy z fizycznego połączenia, które nie są natychmiast rozpoznawane przez transport interfejsu API. Diagram dotyczy następujących okolicznościach:

- Transport jest Websocket, zawsze ramki lub zdarzenia wysłanego przez serwer.
- Istnieją różne okresy przerwy w połączeniu sieci fizycznej.
- Transport interfejsu API nie zostaną powiadomieni o przerwami w działaniu, więc SignalR zależy od funkcji keepalive do wykrycia.

![Rozłączeń transportu](handling-connection-lifetime-events/_static/image2.png)

Jeśli klient przechodzi do trybu połączyć się ponownie, ale nie można ustanowić połączenia transportowego przed upływem limitu czasu rozłączenia, serwer przerywa połączenia SignalR. W takim przypadku serwer wykonuje koncentratora `OnDisconnected` — metoda i kolejek Rozłącz komunikat do wysłania do klienta, w przypadku, gdy klient zarządza połączyć się później. Jeśli klient ponowne, odbierze polecenie rozłączenia i wywołania `Stop` metody. W tym scenariuszu `OnReconnected` nie jest wykonywany, gdy klient ponownie nawiąże połączenie, i `OnDisconnected` nie została wykonana, kiedy klient wywołuje `Stop`. Na poniższym diagramie przedstawiono w tym scenariuszu.

![Zakłócenia transportu — limit czasu serwera](handling-connection-lifetime-events/_static/image3.png)

Zdarzenia okresu istnienia połączenia SignalR, które mogą być wywoływane na kliencie są następujące:

- `ConnectionSlow`Zdarzenie klienta.

    Wywoływane, gdy wstępnie zdefiniowane część keepalive limit czasu upłynął od ostatniego komunikatu lub odebrano keepalive ping. Domyślny okres ostrzeżenie limitu czasu utrzymywania aktywności to 2/3 limitu czasu utrzymywania aktywności. Limit czasu utrzymywania aktywności jest 20 sekund, więc to ostrzeżenie występuje w około 13 sekund.

    Domyślnie serwer wysyła pakiety ping keepalive co 10 sekund, a klient sprawdza, czy polecenia ping keepalive o co 2 sekund (jedna trzecia różnicy między wartość limitu czasu utrzymywania aktywności i wartość keepalive ostrzeżenie limitu czasu).

    Jeśli transportu interfejsu API uzyska informacje o rozłączeniu, SignalR może mieć świadomość rozłączenia przed przekazuje keepalive ostrzeżenie limit czasu. W takim przypadku `ConnectionSlow` zdarzenie nie będzie wywoływane i SignalR przejdzie bezpośrednio do `Reconnecting` zdarzeń.
- `Reconnecting`Zdarzenie klienta.

    Wywoływane, gdy (interfejsu API transportu wykryje, że połączenie zostanie przerwane, (b) keepalive limit czasu upłynął od ostatniego komunikatu lub odebrano keepalive ping. Kod klienta SignalR rozpoczyna się próba ponownego połączenia. To zdarzenie może obsłużyć, jeśli chcesz, aby aplikacja wykonania określonych czynności podczas transportu połączenie zostanie przerwane. Domyślny okres limitu czasu utrzymywania aktywności jest obecnie 20 sekund.

    Jeśli kod klienta podejmie próbę wywołania metody koncentratora SignalR jest w trybie połączyć się ponownie, SignalR spróbuj wysłać polecenie. W większości przypadków takie próby zakończą się niepowodzeniem, ale w niektórych przypadkach może się powieść. Serwer wysłał zdarzenia, zawsze ramki, i długi transport sondowania SignalR używa dwa kanały komunikacyjne, który klient używa do wysyłania wiadomości i jedną, która jest używana do odbierania wiadomości. Kanał używany do odbierania jest stale otwarte i to konto, z którego jest zamknięty, gdy fizyczne połączenie zostanie przerwane. Kanał używany do wysyłania pozostaje dostępna, więc po przywróceniu łączności fizycznych, wywołanie metody z klienta do serwera może zakończyć się pomyślnie przed ponownym nawiązaniu kanału receive. Zwracana wartość nie będą odbierane, dopóki SignalR ponownie otworzy kanał używany do odbierania.
- `Reconnected`Zdarzenie klienta.

    Uruchamiany po ustanowieniu połączenia transportu. `OnReconnected` Wykonuje program obsługi zdarzeń w Centrum.
- `Closed`Zdarzenie klienta (`disconnected` zdarzeń w języku JavaScript).

    Wywoływane, gdy limit czasu rozłączenia wygasa, gdy kod klienta SignalR próbuje połączyć się ponownie po utracie połączenia transportowego. Odłącz domyślny limit czasu wynosi 30 sekund. (To zdarzenie jest również wywoływane po zakończeniu połączenia, ponieważ `Stop` metoda jest wywoływana.)

Przerw połączenia transportu, które nie są wykrywane przez transport interfejsu API i nie opóźnienie odbiór keepalive ping z serwera przez czas dłuższy niż limit czasu ostrzeżenia keepalive może nie spowodować dowolnego połączenia, okres istnienia zdarzeń do wywołania.

W niektórych środowiskach sieci celowo Zamknij bezczynności połączenia, a inną funkcję pakiety utrzymywania aktywności jest aby temu zapobiec przez umożliwienie tych sieci wiedzieć, że połączenia SignalR jest używany. W ekstremalnych przypadkach z domyślną częstotliwością wynoszącą polecenia ping keepalive nie może być za mało, aby uniemożliwić połączenia zamknięte. W takim przypadku można skonfigurować keepalive ping do wysłania częściej. Aby uzyskać więcej informacji, zobacz [ustawienia limitu czasu i utrzymywania aktywności](#timeoutkeepalive) dalszej części tego tematu.

> [!NOTE] 
> 
> [!IMPORTANT]
> Kolejność zdarzeń opisane w tym miejscu nie jest gwarantowana. SignalR sprawia, że każda próba wywołania połączenia okres istnienia zdarzeń w sposób przewidywalny zgodnie z tego systemu, ale istnieje wiele zmian zdarzenia sieci i wiele sposobów, w których podstawowej struktury komunikacji transportu interfejsów API obsługi. Na przykład `Reconnected` zdarzeń nie może zostać wywołane, gdy klient ponownie nawiąże połączenie, lub `OnConnected` obsługi na serwerze może działać podczas próby nawiązania połączenia zakończy się niepowodzeniem. W tym temacie opisano tylko efekty zwykle produkcji niektórych typowych okoliczności.


<a id="clientdisconnect"></a>

### <a name="client-disconnection-scenarios"></a>Scenariusze rozłączenia klienta

W przeglądarce klienta SignalR kod klienta, który obsługuje połączenia SignalR działa w kontekście JavaScript strony sieci web. Który ma Dlaczego połączenia SignalR musi kończyć się po przejściu z jednej strony do innego i że jego dlaczego masz wiele połączeń z wielu identyfikatorów połączeń. Jeśli łączysz się z wielu okien przeglądarki lub kart. Gdy użytkownik zamyka okna lub karty przeglądarki, lub przechodzi do nowej strony lub odświeża stronę, połączenia SignalR natychmiast zakończy się, ponieważ zdarzenie przeglądarka obsługuje kod klienta SignalR dla wywołań i `Stop` metody. W tych scenariuszach lub dowolnej platformie klienta, gdy wywołuje aplikację `Stop` metody `OnDisconnected` program obsługi zdarzeń jest wykonywana bezpośrednio na serwerze i kliencie zgłasza `Closed` zdarzeń (zdarzenie jest nazywane `disconnected` w JavaScript).

Jeśli aplikacja kliencka lub komputera, na którym jest uruchomiona na ulegnie awarii lub przechodzi w stan uśpienia (na przykład gdy użytkownik zamyka komputer przenośny), serwer nie otrzymuje informacji o co się stało. Ile serwer żądał, utraty klienta może być z powodu przerw w łączności i klient próbuje ponownie. W związku z tym w tych scenariuszach czeka na umożliwieniu klient ponownie połączyć, serwer i `OnDisconnected` nie wykonuj do rozłączenia limit czasu wygaśnięcia (domyślnie około 30 sekund). Na poniższym diagramie przedstawiono w tym scenariuszu.

![Niepowodzenie komputera klienta](handling-connection-lifetime-events/_static/image4.png)

<a id="serverdisconnect"></a>

### <a name="server-disconnection-scenarios"></a>Scenariusze serwera rozłączenia

Gdy serwer przejdzie do trybu offline — ponowne uruchomienie, nie powiedzie się, domena aplikacji odtwarza, itp.--wynik może być podobna do podłączania utracone lub transportu interfejsu API i SignalR mogą od razu wiedzieć, że serwer został usunięty, i SignalR może rozpocząć próba ponownego połączenia bez wywoływanie `ConnectionSlow` zdarzeń. Jeśli klient przechodzi do trybu połączyć się ponownie i odzyskuje serwer lub ponownego uruchomienia lub serwer w tryb online przed upływem limitu czasu rozłączenia, klient nawiąże połączenie do przywróconej lub nowego serwera. W takim przypadku nadal połączenia SignalR na kliencie i `Reconnected` zdarzenia. Na pierwszym serwerze `OnDisconnected` nigdy nie jest wykonywane i na nowym serwerze `OnReconnected` jest wykonywany, mimo że `OnConnected` nigdy nie zostało wykonane dla tego klienta na tym serwerze przed. (Efekt jest taki sam, jeśli klient połączy się ponownie do tego samego serwera po odtworzenia domeny aplikacji lub ponowne uruchomienie komputera, ponieważ po ponownym uruchomieniu serwera go ma Brak pamięci działania poprzednie połączenie.) Poniższy diagram przyjęto założenie, że transportu interfejsu API uzyska informacje o utracie połączenia od razu, więc `ConnectionSlow` nie zdarzenia.

![Błąd serwera i ponownym](handling-connection-lifetime-events/_static/image5.png)

Jeśli serwer stał się dostępny przed upływem limitu czasu rozłączenia, kończy się połączenia SignalR. W tym scenariuszu `Closed` zdarzenia (`disconnected` w klientach JavaScript) jest uruchamiany na kliencie, ale `OnDisconnected` nigdy nie jest wywoływana na serwerze. Poniższy diagram przyjęto założenie, że transportu interfejsu API nie zostaną powiadomieni o utracono połączenie, zostanie on wykryty przez SignalR keepalive funkcjonalność i `ConnectionSlow` zdarzenia.

![Błąd serwera i limit czasu](handling-connection-lifetime-events/_static/image6.png)

<a id="timeoutkeepalive"></a>

## <a name="timeout-and-keepalive-settings"></a>Ustawienia limitu czasu i utrzymywania aktywności

Wartość domyślna `ConnectionTimeout`, `DisconnectTimeout`, i `KeepAlive` wartości są odpowiednie dla większości scenariuszy, ale można ją zmienić, jeśli środowiska pod kątem specjalnymi potrzebami. Na przykład środowiska sieciowego zamknięcie połączenia, które są w stanie bezczynności przez 5 sekund, może być konieczne zmniejszenie wartości keepalive.

<a id="connectiontimeout"></a>

### <a name="connectiontimeout"></a>ConnectionTimeout

To ustawienie oznacza ilość czasu, aby pozostawić połączenia transportowego otwarte i Oczekiwanie na odpowiedź przed jego zamknięciem i otwarciem nowego połączenia. Wartość domyślna to 110 sekund.

To ustawienie jest stosowane tylko podczas utrzymywania aktywności funkcji jest wyłączone, co zwykle ma zastosowanie tylko długiego transportu sondowania. Poniższy diagram ilustruje efekt tego ustawienia na długi sondowania połączenie transportu.

![Czas połączenia transportu sondowania](handling-connection-lifetime-events/_static/image7.png)

<a id="disconnecttimeout"></a>

### <a name="disconnecttimeout"></a>Wartości DisconnectTimeout

To ustawienie reprezentuje czas oczekiwania po połączenie transportu są tracone przed zgłoszeniem `Disconnected` zdarzeń. Wartość domyślna to 30 sekund. Podczas ustawiania `DisconnectTimeout`, `KeepAlive` automatycznie ma ustawioną wartość 1/3 `DisconnectTimeout` wartość.

<a id="keepalive"></a>

### <a name="keepalive"></a>Utrzymywania aktywności

To ustawienie oznacza ilość czasu oczekiwania przed wysłaniem pakietów keepalive bezczynnego połączenia. Wartość domyślna to 10 sekund. Ta wartość nie może być więcej niż 1/3 `DisconnectTimeout` wartość.

Jeśli chcesz ustawić obie `DisconnectTimeout` i `KeepAlive`ustaw `KeepAlive` po `DisconnectTimeout`. W przeciwnym razie Twojej `KeepAlive` ustawienia zostaną zastąpione podczas `DisconnectTimeout` automatycznie ustawia `KeepAlive` 1/3 wartości limitu czasu.

Jeśli chcesz wyłączyć funkcje keepalive, ustaw `KeepAlive` na wartość null. Funkcje Keepalive jest automatycznie wyłączana dla długiego transportu sondowania.

<a id="changetimeout"></a>

### <a name="how-to-change-timeout-and-keepalive-settings"></a>Jak zmienić ustawienia limitu czasu i utrzymywania aktywności

Aby zmienić wartości domyślne dla tych ustawień, należy ustawić je w `Application_Start` w Twojej *Global.asax* plików, jak pokazano w poniższym przykładzie. Wartości wyświetlane w przykładowym kodzie są takie same jak wartości domyślne.

[!code-csharp[Main](handling-connection-lifetime-events/samples/sample1.cs)]

<a id="notifydisconnect"></a>

## <a name="how-to-notify-the-user-about-disconnections"></a>Jak użytkownicy są powiadamiani o rozłączeń

W niektórych aplikacjach może być wyświetlany komunikat do użytkownika, gdy istnieją problemy z łącznością. Masz kilka opcji, jak i kiedy mają to robić. Poniższe przykłady kodu są przeznaczone dla klienta kodu JavaScript przy użyciu wygenerowanego serwera proxy.

- Obsługa `connectionSlow` zdarzeń, aby wyświetlić komunikat, jak SignalR jest znane problemy z połączeniem, przed jego przechodzi w tryb połączyć się ponownie.

    [!code-javascript[Main](handling-connection-lifetime-events/samples/sample2.js)]
- Obsługa `reconnecting` zdarzenia wyświetlony komunikat SignalR zna rozłączeniu i przechodzi do trybu połączyć się ponownie.

    [!code-javascript[Main](handling-connection-lifetime-events/samples/sample3.js)]
- Obsługa `disconnected` zdarzeń do wyświetlania komunikatu, gdy próba ponownego połączenia został przekroczony. W tym scenariuszu, jedynym sposobem, aby ponownie ustanowić połączenia z serwerem ponownie uruchomi się ponownie połączenia SignalR, wywołując `Start` metodę, która utworzy nowy identyfikator połączenia. Poniższy przykładowy kod używa flagę, aby upewnić się, że wystawiać powiadomienia tylko po upływie limitu czasu ponownie nawiązującego połączenie, nie po normalne zakończenia połączenia SignalR spowodowane przez wywołanie metody `Stop` metody.

    [!code-javascript[Main](handling-connection-lifetime-events/samples/sample4.js)]

<a id="continuousreconnect"></a>

## <a name="how-to-continuously-reconnect"></a>Jak połączyć się ponownie w sposób ciągły

W niektórych aplikacjach można automatycznie ponownie ustanowić połączenia, po zostało utracone, a próba ponownego połączenia został przekroczony. Aby to zrobić, należy wywołać `Start` metody z Twojej `Closed` obsługi zdarzeń (`disconnected` obsługi zdarzeń na klientach JavaScript). Należy poczekać pewien czas przed wywołaniem `Start` w celu uniknięcia w ten sposób zbyt często podczas serwera lub połączenie fizyczne są niedostępne. Poniższy przykład kodu dotyczy klienta kodu JavaScript przy użyciu wygenerowanego serwera proxy.

[!code-javascript[Main](handling-connection-lifetime-events/samples/sample5.js)]

Potencjalny problem, należy pamiętać o w klientów mobilnych jest, że prób ciągłe ponowne nawiązanie połączenia, gdy serwer lub połączenie fizyczne nie jest dostępna, może spowodować zużycie baterii niepotrzebne.

<a id="disconnectclientfromserver"></a>

## <a name="how-to-disconnect-a-client-in-server-code"></a>Jak można rozłączyć klienta w kodzie serwera

SignalR wersji 1.1.1 nie ma wbudowanego serwera interfejsu API dla rozłączanie klientów. Brak [plany dodaniem tej funkcji w przyszłości](https://github.com/SignalR/SignalR/issues/2101). W bieżącej wersji biblioteki SignalR Najprostszym sposobem, aby rozłączyć się z serwerem klienta jest zaimplementować metodę rozłączenia na kliencie, a następnie wywołać tej metody z serwera. Poniższy przykładowy kod przedstawia metodę rozłączenia dla klienta kodu JavaScript przy użyciu wygenerowanego serwera proxy.

[!code-javascript[Main](handling-connection-lifetime-events/samples/sample6.js)]

> [!WARNING]
> Zabezpieczenia — tej metody dla rozłączanie klientów ani proponowanych wbudowanego interfejsu API będzie dotyczyć scenariusza zaatakowanych przez hakerów klientów, którzy są uruchomione złośliwego kodu, ponieważ klientów można się ponownie połączyć lub zaatakowanych przez hakerów kodu może spowodować usunięcie `stopClient` metody lub zmian jaką pełni funkcję. Odpowiednie miejsce do implementowania stanowe typu "odmowa usługi" (DOS) ochrony jest nie znajduje się w ramach lub warstwa serwera, ale raczej w infrastrukturze frontonu.
