---
uid: signalr/overview/getting-started/introduction-to-signalr
title: Wprowadzenie do SignalR | Dokumentacja firmy Microsoft
author: pfletcher
description: "W tym artykule opisano, co to jest SignalR i niektóre rozwiązania, który został zaprojektowany do utworzenia."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 0fab5e35-8c1f-43d4-8635-b8aba8766a71
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/getting-started/introduction-to-signalr
msc.type: authoredcontent
ms.openlocfilehash: 27150d314b6861f1098e6ef4a7de94e7b371a78e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="introduction-to-signalr"></a>Wprowadzenie do SignalR
====================
przez [Patrick Fletcher](https://github.com/pfletcher)

> W tym artykule opisano, co to jest SignalR i niektóre rozwiązania, który został zaprojektowany do utworzenia. 
> 
> ## <a name="questions-and-comments"></a>Pytania i komentarze
> 
> Wystaw opinię na jak zbędne tego samouczka i jakie firma Microsoft może poprawić w komentarze u dołu strony. Jeśli masz pytania, które nie są bezpośrednio związane z tego samouczka możesz zamieścić je do [forum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) lub [StackOverflow.com](http://stackoverflow.com/).


## <a name="what-is-signalr"></a>Co to jest SignalR?

Biblioteka SignalR platformy ASP.NET to biblioteka dla deweloperów platformy ASP.NET, które upraszcza proces dodawania funkcji sieci web w czasie rzeczywistym do aplikacji. Funkcje sieci web w czasie rzeczywistym jest możliwość server przekazywanie zawartości przez kod do połączonych klientów natychmiast po jej udostępnieniu, zamiast serwera, poczekaj, aż klientowi żądanie nowych danych.

SignalR można dodać dowolny rodzaj funkcji "w czasie rzeczywistym" sieci web do aplikacji ASP.NET. Podczas rozmowy jest często używana jako przykład, możesz zrobić wiele więcej. Wszystkie razem, kiedy użytkownik odświeża strony sieci web, aby zobaczyć nowe dane, lub implementuje stronę [długiego sondowania](http://en.wikipedia.org/wiki/Push_technology#Long_polling) można pobrać nowe dane, jest kandydatem do przy użyciu SignalR. Przykłady obejmują pulpitów nawigacyjnych i monitorowania aplikacji, współpracy aplikacji (takich jak jednoczesne edytowanie dokumentów), zadań, aktualizacje postępu i formularzy w czasie rzeczywistym.

SignalR umożliwia również całkowicie nowych typów aplikacji sieci web, które wymagają wysokiej częstotliwości aktualizacji z serwera, na przykład w czasie rzeczywistym gier. Na dużą przykład tego, zobacz [ShootR gier.](http://shootr.signalr.net/)

Biblioteka SignalR udostępnia prosty interfejs API do tworzenia klienta serwera zdalnych wywołań procedur (RPC) które wywołują funkcje JavaScript w kliencie w przeglądarkach (i innych platform klienta), z kodu .NET po stronie serwera. Biblioteka SignalR zawiera też interfejs API umożliwiający zarządzanie połączeniami (na przykład nawiązywanie i zakańczanie zdarzeń) i grupowanie połączeń.

![Wywoływanie metody z SignalR](introduction-to-signalr/_static/image1.png)

SignalR automatycznie obsługuje zarządzanie połączeniami i umożliwia komunikatów rozgłaszanych do wszystkich klientów podłączonych równocześnie, takich jak pokoju rozmów. Można również wysyłać wiadomości do określonych klientów. Połączenie między klientem a serwerem jest trwała, w przeciwieństwie do klasycznego połączenia HTTP, nawiązane ponownie dla każdego komunikatu.

SignalR obsługuje funkcje "serwera wypychania", w którym kod serwera, można umieszczać kod klienta w przeglądarce, za pomocą zdalnego wywołania procedury (RPC), zamiast wspólnego modelu żądań i odpowiedzi w sieci web dzisiaj.

Aplikacji SignalR można skalować w poziomie do tysięcy klientów przy użyciu usługi Service Bus, programu SQL Server lub [Redis](http://redis.io).

SignalR to open source, dostępny za pośrednictwem [GitHub](https://github.com/signalr).

## <a name="signalr-and-websocket"></a>SignalR i protokołu WebSocket

SignalR używa nowego transportu protokołu WebSocket, jeśli jest on dostępny i powraca do starszych transporty w miarę potrzeby. Podczas pisania można pewnością aplikacji bezpośrednio za pomocą protokołu WebSocket, używając SignalR oznacza, że wiele dodatkowe funkcje, które należy zaimplementować będzie już zostały wykonane dla Ciebie. Co najważniejsze oznacza to, że można kodu aplikacji, aby móc korzystać z protokołu WebSocket bez konieczności martwić tworzenia ścieżki kodu oddzielne starszych klientów. SignalR również osłony można z martwiąc się o aktualizacje protokołu WebSocket, ponieważ SignalR nadal będzie zostać zaktualizowany do obsługi zmian w podstawowym transportu aplikacji spójny interfejs między wersjami protokołu WebSocket.

Na pewno może tworzyć rozwiązania za pomocą protokołu WebSocket wyłącznie, SignalR zawiera wszystkie funkcje, należy zapisać użytkownika, takie jak powrotu do innego transportu i modyfikowania aplikacji w celu aktualizacji do implementacji protokołu WebSocket.

<a id="transports"></a>

## <a name="transports-and-fallbacks"></a>Transporty i przejścia

SignalR to Abstrakcja przez niektóre transportów, które są wymagane do pracy w czasie rzeczywistym między klientem i serwerem. Połączenia SignalR rozpoczyna się jako HTTP, a następnie jest podwyższany do połączenia obiektu WebSocket, jeśli jest dostępna. Protokół WebSocket jest idealny transportu dla biblioteki SignalR, ponieważ sprawia, że najbardziej efektywne wykorzystanie pamięci serwera, ma ona uzyskać najmniejsze opóźnienia i ma większość funkcji (na przykład pełnego dupleksu komunikacji między klientem i serwerem), ale ma także najbardziej rygorystyczne wymagania dotyczące: WebSocket wymaga serwera z systemem Windows Server 2012 lub Windows 8 i .NET Framework 4.5. Jeśli te wymagania nie są spełnione, SignalR spróbuje Użyj innego transportu, aby wprowadzić jego połączenia.

### <a name="html-5-transports"></a>Transporty HTML 5

Transporty te są zależne od obsługę [HTML 5](http://en.wikipedia.org/wiki/HTML5). Jeśli przeglądarka klienta nie obsługuje standardu HTML 5, będą używane starsze transportów.

- **Protokół WebSocket** (Jeśli serwer i przeglądarki wskazują one obsługi protokołu Websocket). Protokół WebSocket jest tylko transport, który określa wartość true, trwałe, dwukierunkowe połączenie między klientem i serwerem. Jednak WebSocket ma wymagania najbardziej rygorystyczne; jest w pełni obsługiwana tylko w najnowszej wersji programu Microsoft Internet Explorer i Google Chrome, Mozilla Firefox i zawiera tylko częściowe wdrożenie w innych przeglądarkach, takie jak Opera i Safari.
- **Zdarzenia wysyłane serwera**, znanej również jako źródła zdarzeń (Jeśli przeglądarka obsługuje wysyłane zdarzeń serwera, który jest zasadniczo wszystkie przeglądarki, z wyjątkiem programu Internet Explorer).

### <a name="comet-transports"></a>Transporty kometa

Następujące transportu są oparte na [kometa](http://en.wikipedia.org/wiki/Comet_(programming)) modelu aplikacji sieci web, w którym przeglądarki lub inne klienta obsługuje żądania HTTP przechowywane przez długi, którym serwer w szczególności wypychanie danych do klienta bez klienta programu żąda go.

- **Nieskończona ramki** (dla programu Internet Explorer tylko). Nieskończona ramki tworzy ukryte IFrame, dzięki czemu żądanie do punktu końcowego na serwerze, który nie zostanie zakończone. Serwer następnie stale wysyła skryptu do klienta, który natychmiast zostanie wykonany, zapewniając połączenia jednokierunkowe w czasie rzeczywistym z serwera do klienta. Połączenie z klienta do serwera używa oddzielnego połączenia z serwera do klienta połączenia i jak standardowe żądania HTTP, nowe połączenie jest tworzony dla każdego elementu danych, który ma być wysyłane.
- **Długie sondowania AJAX**. Sondowanie długo nie tworzy trwałe połączenie, ale zamiast tego odpytuje serwer z żądaniem pozostanie otwarty, dopóki serwer odpowiada, w którym połączenie zostanie zamknięte i natychmiast zażądano nowego połączenia. Może to powodować pewnego opóźnienia przesyłania podczas resetuje połączenie.

Aby uzyskać więcej informacji na jakie transportu są obsługiwane w jakich konfiguracjach, zobacz [obsługiwane platformy](supported-platforms.md).

### <a name="transport-selection-process"></a>Procesu wyboru transportu

Na poniższej liście przedstawiono kroki, które używa SignalR, aby zdecydować, które transportu do użycia.

1. Jeśli zainstalowano przeglądarkę Internet Explorer 8 lub wcześniej, używany jest długa sondowania.
2. Jeśli skonfigurowano JSONP (oznacza to, `jsonp` parametr ma wartość `true` po uruchomieniu połączenia), długi sondowania jest używany.
3. Jeśli między domenami jest nawiązywane połączenie (jeśli SignalR punktu końcowego nie jest w tej samej domenie co hostingu strony), następnie protokołu WebSocket zostaną użyte, jeśli są spełnione poniższe kryteria:

    - Klient obsługuje CORS (Cross-Origin Resource Sharing). Aby uzyskać szczegółowe informacje, na których klienci obsługi mechanizmu CORS, zobacz [CORS w caniuse.com](http://www.caniuse.com/CORS).
    - Klient obsługuje protokół WebSocket
    - Serwer obsługuje protokół WebSocket

    Jeśli którekolwiek z tych kryteriów nie są spełnione, będą używane długie sondowania. Aby uzyskać więcej informacji dotyczących połączeń między domenami, zobacz [jak nawiązać połączenie między domenami](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain).
4. Jeśli nie skonfigurowano JSONP i nie jest połączeniem między domenami, protokołu WebSocket zostaną użyte, jeśli klient i serwer jego obsługi.
5. Jeśli klient lub serwer nie obsługują protokołu WebSocket, zdarzenia wysyłane serwera jest używany, jeśli jest dostępna.
6. Jeśli zdarzenia wysyłane serwera nie jest dostępna, nastąpiła nieskończona ramki.
7. Długie sondowania jest używany w przypadku niepowodzenia nieskończona ramki.

<a id="MonitoringTransports"></a>
### <a name="monitoring-transports"></a>Transporty monitorowania

Można określić transportu używa aplikacji przez włączenie rejestrowania w Centrum i otwieranie okna konsoli w przeglądarce.

Aby włączyć rejestrowanie zdarzeń w Centrum w przeglądarce, Dodaj następujące polecenie do aplikacji klienta:

`$.connection.hub.logging = true;`

- W programie Internet Explorer Otwórz narzędzia deweloperskie, naciskając klawisz F12 i kliknij kartę konsoli.

    ![Konsola programu Microsoft Internet Explorer](introduction-to-signalr/_static/image2.png)
- W przeglądarce Chrome Otwórz konsolę, naciskając klawisze Ctrl + Shift + J.

    ![Konsoli programu Google Chrome](introduction-to-signalr/_static/image3.png)

Otwórz konsolę i włączone rejestrowanie będziesz mieć możliwość Zobacz transport, który jest używany przez SignalR.

![Konsoli w programie Internet Explorer przedstawiający protokołu WebSocket transportu](introduction-to-signalr/_static/image4.png)

### <a name="specifying-a-transport"></a>Określanie transportu

Transportu do negocjowania musi upłynąć trochę czasu i klient/serwer zasobów. Jeśli funkcje klienta są znane, transport można określić po uruchomieniu połączenia klienta. Poniższy fragment kodu przedstawia uruchamianie połączenie przy użyciu transportu sondowania długi Ajax, jak mają być używane, jeśli nosiła ona, że klient nie obsługuje żadnego innego protokołu:

`connection.start({ transport: 'longPolling' });`

Jeśli klient próby transportów określonych w kolejności, można określić rezerwowy order. Poniższy fragment kodu przedstawia WebSocket w trakcie i awarii, który, przejście bezpośrednio do sondowania długo.

`connection.start({ transport: ['webSockets','longPolling'] });`

Stałe typu string służący do określania transportu są zdefiniowane w następujący sposób:

- `webSockets`
- `foreverFrame`
- `serverSentEvents`
- `longPolling`

## <a name="connections-and-hubs"></a>Połączeniami i koncentratorami

Interfejs API SignalR zawiera dwa modele komunikacji między klientami a serwerami: stałe połączeniami i koncentratorami.

Połączenie reprezentuje proste punktu końcowego do wysyłania wiadomości jednego adresata, grupowanych lub emisji. Umożliwia trwałe połączenie interfejsu API (reprezentowane przez klasę klasy PersistentConnection w kodu platformy .NET), deweloper bezpośredni dostęp do protokołu komunikacyjnego niskiego poziomu, który ujawnia SignalR. Przy użyciu modelu komunikacji połączenia jest znane deweloperom, którzy użyli API opartego na połączeniach, takie jak Windows Communication Foundation.

Koncentrator jest bardziej ogólne potoku wbudowane w system API połączenia, który pozwala klienta i serwera, bezpośrednie wywoływanie metod na siebie. SignalR obsługuje wysyłki poza granicami maszyny tak, jakby przez magic, co pozwala klientom wywoływać metod na serwerze jako łatwo jako metody lokalne i na odwrót. Przy użyciu modelu komunikacji koncentratory jest znane deweloperom, którzy użyli zdalnego wywoływania interfejsów API, takich jak .NET Remoting. Przy użyciu koncentratora umożliwia również przekazywanie jednoznacznie parametrów do metod, włączanie wiązania modelu.

### <a name="architecture-diagram"></a>diagram architektury

Na poniższym diagramie przedstawiono relację między koncentratorów, połączeń trwałych i podstawowych technologii używanych dla transportu.

![Diagram architektury SignalR przedstawiający interfejsów API transportu i klientów](introduction-to-signalr/_static/image5.png)

### <a name="how-hubs-work"></a>Jak działają centrów

Gdy kod po stronie serwera wywołuje metodę dla klienta, pakiet są wysyłane przez aktywny transport, który zawiera nazwy i parametry metody do wywołania (gdy obiekt jest wysyłany jako parametr metody, jego jest zserializowanym przy użyciu JSON). Następnie klient dopasowuje Nazwa metody do metody zdefiniowane w kodzie po stronie klienta. Jeśli istnieje dopasowanie, metody klientów będą wykonywane przy użyciu danych parametru zdeserializowany.

Wywołanie metody można monitorować za pomocą takich narzędzi jak [Fiddler.](http://fiddler2.com/) Na poniższej ilustracji przedstawiono wywołania metody wysłanych z serwera SignalR klient przeglądarki sieci web w okienku Dzienniki narzędzia Fiddler. Wywołanie metody wysyłanych z koncentratora o nazwie `MoveShapeHub`, i jest wywoływana metoda jest wywoływana `updateShape`.

![Widok pokazujący ruchu SignalR dziennika Fiddler](introduction-to-signalr/_static/image6.png)

W tym przykładzie nazwy Centrum jest identyfikowany przy `H` parametru; metody nazwa jest oznaczone symbolem `M` określono parametr, a dane są wysyłane do metody z `A` parametru. Aplikacji, która wygenerowała ten komunikat jest tworzony w [wysokiej częstotliwości w czasie rzeczywistym](tutorial-high-frequency-realtime-with-signalr.md) samouczka.

### <a name="choosing-a-communication-model"></a>Wybieranie modelu komunikacji

Większość aplikacji należy używać interfejsu API koncentratorów. Interfejs API połączeń mogła zostać użyta w następujących okolicznościach:

- Format rzeczywiste wysłano wiadomość musi być określony.
- Deweloper preferuje do pracy z modelem obsługi wiadomości i wysyłania, a nie modelu zdalnego wywoływania.
- Istniejących aplikacji, która używa modelu obsługi wiadomości jest są przenoszone do użycia SignalR.
