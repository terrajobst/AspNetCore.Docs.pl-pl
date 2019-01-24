---
uid: signalr/overview/getting-started/introduction-to-signalr
title: Wprowadzenie do SignalR | Dokumentacja firmy Microsoft
author: bradygaster
description: W tym artykule opisano, co to jest SignalR i niektóre z rozwiązań, który został zaprojektowany do utworzenia.
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: 0fab5e35-8c1f-43d4-8635-b8aba8766a71
msc.legacyurl: /signalr/overview/getting-started/introduction-to-signalr
msc.type: authoredcontent
ms.openlocfilehash: 7b9dae3e5d79319a9fefee41f4525a59f950746a
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2019
ms.locfileid: "54837770"
---
<a name="introduction-to-signalr"></a>Wprowadzenie do usługi SignalR
====================

przez [Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]


> W tym artykule opisano, co to jest SignalR i niektóre z rozwiązań, który został zaprojektowany do utworzenia. 
> 
> ## <a name="questions-and-comments"></a>Pytania i komentarze
> 
> Jak się podoba w tym samouczku, i co można było ulepszyć proces w komentarzach u dołu strony, wystaw opinię. Jeśli masz pytania, na które nie są bezpośrednio związane z tego samouczka, możesz zamieścić je do [forum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) lub [StackOverflow.com](https://stackoverflow.com/questions/tagged/signalr).

## <a name="what-is-signalr"></a>Co to jest SignalR?

Biblioteki SignalR platformy ASP.NET to biblioteka dla deweloperów platformy ASP.NET, która upraszcza proces dodawania funkcji internetowych w czasie rzeczywistym do aplikacji. Funkcje sieci web w czasie rzeczywistym jest możliwość server przekazywanie zawartości przez kod do połączonych klientów natychmiast po jej udostępnieniu, zamiast serwera oczekiwania dla klientów dane nowego żądania.

Biblioteki SignalR można dodawać dowolny rodzaj funkcji "w czasie rzeczywistym" w sieci web do aplikacji platformy ASP.NET. Podczas rozmowy jest często używana jako przykład, możesz tworzyć wiele więcej. Ilekroć użytkownika odświeża stronę sieci web, aby zobaczyć nowe dane lub strona implementuje [długiego sondowania](http://en.wikipedia.org/wiki/Push_technology#Long_polling) można pobrać nowe dane, jest kandydatem do przy użyciu biblioteki SignalR. Przykłady obejmują pulpity nawigacyjne i monitorowania aplikacji, aplikacji współpracy (na przykład jednoczesne edytowanie dokumentów), zadań, aktualizacje postępu i formularzy w czasie rzeczywistym.

SignalR umożliwia również całkowicie nowych typów aplikacji sieci web, które wymagają wysokiej częstotliwości aktualizacji z serwera, na przykład w czasie rzeczywistym gry.

Biblioteka SignalR udostępnia prosty interfejs API do tworzenia klienta serwera zdalnych wywołań procedur (RPC), które wywołują funkcje języka JavaScript w kliencie przeglądarki (i innych platform klienta) od kodu .NET po stronie serwera. Biblioteka SignalR zawiera też interfejs API umożliwiający zarządzanie połączeniami (na przykład nawiązywać połączenia i zdarzenia rozłączenia) i grupowanie połączeń.

![Wywoływanie metod przy użyciu SignalR](introduction-to-signalr/_static/image1.png)

SignalR automatycznie obsługuje zarządzanie połączeniami i umożliwia emisji komunikaty, aby wszyscy połączeni klienci równocześnie, takich jak pokoju rozmów. Można również wysyłać wiadomości do określonych klientów. Połączenie między klientem i serwerem jest trwała, w odróżnieniu od klasycznego połączenia HTTP, które zostanie ponownie nawiązane dla każdego komunikatu.

SignalR obsługuje funkcje "serwera wypychania", w którym kod serwera, można umieszczać kodu klienta w przeglądarce, za pomocą zdalnego wywołania procedury (RPC), a nie wspólnego modelu odpowiedź na żądanie w sieci web już dziś.

Aplikacji SignalR można skalować do tysięcy klientów przy użyciu usługi Service Bus, SQL Server lub [Redis](http://redis.io).

SignalR to open source, dostępne za pośrednictwem [GitHub](https://github.com/signalr).

## <a name="signalr-and-websocket"></a>SignalR i WebSocket

SignalR używa nowy transport WebSocket, gdzie są dostępne i nastąpi powrót do starszej transportu, gdy jest to konieczne. Gdy bez obaw można zapisać aplikacji bezpośrednio za pomocą protokołu WebSocket, przy użyciu SignalR oznacza wiele dodatkowe funkcje, które trzeba do zaimplementowania zostało już przeprowadzone dla Ciebie. Co najważniejsze oznacza to, czy tworzyć kod aplikacji w taki sposób, aby móc korzystać z protokołu WebSocket, bez konieczności martwienia się o Tworzenie ścieżki osobnego kodu dla starszych klientów. SignalR również ochronnym możesz z konieczności martwienia się o aktualizacjach WebSocket, ponieważ SignalR zostanie zaktualizowany i będzie obsługiwać zmiany w podstawowej transportu, zapewniając spójny interfejs aplikacji wersje protokołu WebSocket.

<a id="transports"></a>

## <a name="transports-and-fallbacks"></a>Transporty i planów awaryjnych

SignalR to Abstrakcja za pośrednictwem niektórych transportu, które są wymagane do pracy w czasie rzeczywistym między klientem i serwerem. Połączenia SignalR startuje jako protokołu HTTP, a następnie zostanie podwyższony do połączeń protokołu WebSocket, jeśli jest ona dostępna. Protokół WebSocket jest idealny transport dla elementu SignalR, ponieważ ona sprawia, że najbardziej efektywne wykorzystanie pamięci serwera, ma najniższe opóźnienie i ma większość podstawowych funkcji (na przykład w trybie pełnego dupleksu komunikacji między klientem i serwerem), ale ma on także najbardziej rygorystyczne wymagania: WebSocket wymaga serwera z systemem Windows Server 2012 lub Windows 8 i .NET Framework 4.5. Jeśli te wymagania nie są spełnione, SignalR spróbuje użyć innego transportu się jego połączenia.

### <a name="html-5-transports"></a>HTML 5 transports

Transporty te zależą od pomocy technicznej dla [HTML 5](http://en.wikipedia.org/wiki/HTML5). Jeśli przeglądarka klienta nie obsługuje standardu HTML 5, starsze transportów będą używane.

- **WebSocket** (Jeśli zarówno serwer, jak i przeglądarka wskazują, obsługują one Websocket). Protokół WebSocket jest tylko transportu, który ustanawia wartość true, trwały, dwukierunkowe połączenie między klientem i serwerem. Jednak WebSocket ma najbardziej rygorystyczne wymagania; jest w pełni obsługiwana tylko w najnowszych wersjach programu Microsoft Internet Explorer, Google Chrome i Mozilla Firefox, a tylko został wdrożony częściowo w innych przeglądarkach, takie jak Opera i Safari.
- **Zdarzenia wysyłane serwera**, znanego również jako źródła zdarzeń (Jeśli przeglądarka obsługuje serwer wysyłane zdarzenia, jest zasadniczo wszystkie przeglądarki, z wyjątkiem programu Internet Explorer).

### <a name="comet-transports"></a>Transporty comet

Następujące transportów opierają się na [Comet](http://en.wikipedia.org/wiki/Comet_(programming)) modelu aplikacji sieci web, w którym przeglądarki lub innych klientów obsługuje żądania HTTP przechowywane przez długi, używane przez serwer do specjalnie wypychanie danych do klienta bez użycia klienta programu żądanie.

- **Nieskończona ramki** (dla programu Internet Explorer tylko). Nieskończona ramki tworzy ukrytego elementu IFrame, który kieruje żądanie do punktu końcowego na serwerze, który nie zostanie zakończone. Serwer następnie stale wysyła skryptu do klienta, które jest wykonywane od razu, zapewniając połączenia jednokierunkowe w czasie rzeczywistym z serwera do klienta. Połączenie od klienta do serwera używa oddzielnego połączenia z serwera do klienta połączenia i jak standardowe żądania HTTP, nowe połączenie jest tworzony dla każdego z nich dane, które muszą być wysyłane.
- **AJAX długim sondowaniem**. Długim sondowaniem nie tworzy trwałe połączenie, ale zamiast tego sonduje serwer z żądaniem, który pozostanie otwarty, dopóki serwer odpowiada, w tym momencie połączenie zostaje zamknięte, a następnie natychmiast zażądano nowego połączenia. Może to powodować pewne opóźnienie, podczas resetuje połączenie.

Aby uzyskać więcej informacji na jakie transportów są wspierane w ramach której konfiguracji, zobacz [platformy obsługiwane przez](supported-platforms.md).

### <a name="transport-selection-process"></a>Proces wyboru transportu

Na poniższej liście przedstawiono kroki, które korzysta z biblioteki SignalR podjęcie decyzji, które transportu do użycia.

1. Jeśli zainstalowano przeglądarkę Internet Explorer 8 lub starszym, długie sondowania jest używany.
2. Jeśli skonfigurowano JSONP (oznacza to, `jsonp` parametr ma wartość `true` po uruchomieniu połączenia), długie sondowania jest używany.
3. Jeśli między domenami jest nawiązywane połączenie (to znaczy, jeśli punkt końcowy SignalR nie jest w tej samej domenie co strona macierzysta), następnie WebSocket zostaną użyte, jeśli są spełnione poniższe kryteria:

   - Klient obsługuje mechanizm CORS (Cross-Origin Resource Sharing). Aby uzyskać szczegółowe informacje, na których klienci obsługują mechanizmu CORS, zobacz [mechanizmu CORS w caniuse.com](http://www.caniuse.com/CORS).
   - Klient obsługuje WebSocket
   - Serwer obsługuje WebSocket

     Jeśli dowolne z poniższych kryteriów nie są spełnione, długie sondowania będą używane. Aby uzyskać więcej informacji na temat połączeń między domenami, zobacz [jak nawiązać połączenie między domenami](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain).
4. Jeśli nie skonfigurowano JSONP, połączenie nie jest między domenami WebSocket będą używane, jeżeli klient i serwer obsługuje.
5. Jeśli klient lub serwer nie obsługują protokołu WebSocket, zdarzenia wysyłane serwera jest używana, jeśli jest ona dostępna.
6. Jeśli zdarzenia wysyłane serwera nie jest dostępny, zostanie podjęta nieskończona ramki.
7. W przypadku niepowodzenia nieskończona ramki długie sondowania jest używany.

<a id="MonitoringTransports"></a>
### <a name="monitoring-transports"></a>Monitorowanie transportu

Można określić transportu aplikacja wykorzystuje, należy włączyć rejestrowanie w Centrum i otwierając okno konsoli w przeglądarce.

Aby włączyć rejestrowanie dla Twojego Centrum zdarzeń w przeglądarce, Dodaj następujące polecenie, aby Twoja aplikacja kliencka:

`$.connection.hub.logging = true;`

- W programie Internet Explorer Otwórz narzędzia deweloperskie, naciskając klawisz F12, a następnie kliknij kartę konsoli.

    ![Konsola programu Microsoft Internet Explorer](introduction-to-signalr/_static/image2.png)
- W przeglądarce Chrome Otwórz konsolę, naciskając klawisze Ctrl + Shift + J.

    ![Console in Google Chrome](introduction-to-signalr/_static/image3.png)

Otwórz konsolę i Rejestrowanie włączone będzie można zobaczyć, które transportu jest używany przez SignalR.

![Konsoli w programie Internet Explorer, przedstawiający WebSocket transportu](introduction-to-signalr/_static/image4.png)

### <a name="specifying-a-transport"></a>Określanie transportu

Negocjowanie transportu trwa pewien czas i klient/serwer zasobów. Jeśli są znane możliwości klienta, transport można określić po uruchomieniu połączenia klienta. Poniższy fragment kodu przedstawia uruchamianie połączenie za pomocą transportu sondowania długie Ajax zostałyby użyte, jeśli jest znane, klient obsługuje inne protokołu:

`connection.start({ transport: 'longPolling' });`

Jeśli klient próby transportów określonych w kolejności, można określić rezerwowego kolejności. Poniższy fragment kodu pokazuje, w trakcie WebSocket i kończy się niepowodzeniem, który, przechodząc bezpośrednio do sondowania długie.

`connection.start({ transport: ['webSockets','longPolling'] });`

Stałe typu string do określania transportów są zdefiniowane w następujący sposób:

- `webSockets`
- `foreverFrame`
- `serverSentEvents`
- `longPolling`

## <a name="connections-and-hubs"></a>Połączeniami i koncentratorami

Interfejs API SignalR zawiera dwa modele do komunikacji między klientami a serwerami: Trwałe połączeniami i koncentratorami.

Połączenie reprezentuje proste punktu końcowego do wysyłania wiadomości jednego adresata, pogrupowanych lub emisji. Udostępnia trwałe połączenie interfejsu API (reprezentowane przez klasę PersistentConnection w kodzie .NET), deweloper bezpośredni dostęp do protokołu komunikacyjnego niskiego poziomu, który udostępnia SignalR. Przy użyciu modelu komunikacji połączeń nie będą niczym nowym dla deweloperów, którzy korzystali z opartego na połączeniach interfejsów API, takich jak Windows Communication Foundation.

Koncentrator jest bardziej ogólny potoku utworzonych na podstawie interfejsu API połączenia, który umożliwia klienta i serwera, bezpośrednie wywoływanie metod na siebie nawzajem. SignalR obsługuje wysyłanie granice maszyny tak, jakby przez magic, umożliwiając klientom wywoływać metody na serwerze, jak łatwo jako metody lokalne i na odwrót. Przy użyciu modelu komunikacji Hubs będą niczym nowym dla deweloperów, którzy użyli zdalnego wywoływania interfejsów API, takich jak wywołaniem funkcji zdalnych .NET. Za pomocą Centrum umożliwia również silnie typizowane parametry są przekazywane do metody, umożliwiając wiązania modelu.

### <a name="architecture-diagram"></a>Diagram architektury

Na poniższym diagramie przedstawiono relację między koncentratorów, połączeń trwałych i podstawowych technologii używanych dla transportu.

![Diagram architektury SignalR, przedstawiający interfejsów API, transportu i klientów](introduction-to-signalr/_static/image5.png)

### <a name="how-hubs-work"></a>Jak działają koncentratory

Gdy kod po stronie serwera wywołuje metody na kliencie, pakiet jest wysyłany na aktywny transport, który zawiera nazwę i parametry metody do wywołania (gdy obiekt jest wysyłany jako parametru metody, jest serializowany przy użyciu formatu JSON). Klient następnie dopasowuje Nazwa metody do metody zdefiniowane w kodzie po stronie klienta. Jeśli istnieje dopasowanie, metoda klienta będą wykonywane przy użyciu danych zdeserializowany parametru.

Wywołania metody które można monitorować za pomocą narzędzi, takich jak [programu Fiddler.](http://fiddler2.com/) Na poniższej ilustracji przedstawiono wywołanie metody wysyłane z serwera biblioteki SignalR do klienta przeglądarki internetowej, w okienku dzienniki programu Fiddler. Wywołania metody które są wysyłane z Centrum o nazwie `MoveShapeHub`, i jest wywoływana metoda jest wywoływana `updateShape`.

![Widok dziennika programu Fiddler przedstawiający ruchu SignalR](introduction-to-signalr/_static/image6.png)

W tym przykładzie nazwy Centrum jest identyfikowany za pomocą `H` parametru; metoda nazwa jest identyfikowany za pomocą `M` parametrów i danych wysyłanych do metody jest identyfikowany za pomocą `A` parametru. Aplikacja, która wygenerowała ten komunikat zostanie utworzona w [o wysokiej częstotliwości w czasie rzeczywistym](tutorial-high-frequency-realtime-with-signalr.md) samouczka.

### <a name="choosing-a-communication-model"></a>Wybieranie modelu komunikacji

Większość aplikacji należy używać interfejsu API centrów. Połączenia interfejsu API można używać w następujących okolicznościach:

- Format rzeczywiste wiadomością wysłaną musi być określony.
- Preferuje projektanta do pracy z modelu obsługi komunikatów i wysyłania, a nie w modelu zdalnego wywoływania.
- Istniejącą aplikację, która używa modelu obsługi komunikatów są są przenoszone do korzystania z biblioteki SignalR.
