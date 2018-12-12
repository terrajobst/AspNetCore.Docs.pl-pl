---
uid: signalr/overview/testing-and-debugging/troubleshooting
title: Rozwiązywanie problemów z SignalR | Dokumentacja firmy Microsoft
author: pfletcher
description: W tym artykule opisano typowe problemy z programowaniem aplikacji SignalR.
ms.author: riande
ms.date: 06/10/2014
ms.assetid: 4b559e6c-4fb0-4a04-9812-45cf08ae5779
msc.legacyurl: /signalr/overview/testing-and-debugging/troubleshooting
msc.type: authoredcontent
ms.openlocfilehash: e41061f0310c021b10dc6667a5c3297788213b0a
ms.sourcegitcommit: 74e3be25ea37b5fc8b4b433b0b872547b4b99186
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53287958"
---
<a name="signalr-troubleshooting"></a>Rozwiązywanie problemów z usługą SignalR
====================
przez [Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> W tym dokumencie opisano typowe problemy dotyczące rozwiązywania problemów z SignalR.
>
> ## <a name="software-versions-used-in-this-topic"></a>Wersje oprogramowania używaną w tym temacie
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
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


Ten dokument zawiera następujące sekcje.

- [Podczas wywoływania metody między klientem i serwerem dyskretnie kończy się niepowodzeniem](#connection)
- [Konfigurowanie websockets usług IIS na polecenie ping/pong wykrywania martwy klienta](#pong)
- [Inne problemy z połączeniem](#other)
- [Błędy kompilacji i po stronie serwera](#server)
- [Problemy z programu Visual Studio](#vs)
- [Problemy z internetowych usług informacyjnych](#iis)
- [Problemy z Microsoft Azure](#azure)

<a id="connection"></a>

## <a name="calling-methods-between-the-client-and-server-silently-fails"></a>Podczas wywoływania metody między klientem i serwerem dyskretnie kończy się niepowodzeniem

W tej sekcji opisano możliwe przyczyny między klientem i serwerem niepowodzenie bez komunikatów zrozumiałego błędu w wywołaniu metody. W przypadku aplikacji SignalR serwera nie ma informacji o metodach, które implementuje klienta; gdy serwer wywołuje metodę klienta, dane nazwy i parametru metody są wysyłane do klienta, a metoda jest wykonywana tylko wtedy, gdy istnieje w formacie, który określony serwer. Jeśli żadna metoda dopasowania zostanie znaleziony na komputerze klienckim, nic się nie dzieje, i bez komunikatu o błędzie jest wywoływane na serwerze.

Aby badać nie wprowadzenie wywoływanych metod klienta, można włączyć rejestrowanie przed wywołaniem metody start koncentratora, aby zobaczyć, jakie wywołuje pochodzą z serwera. Aby włączyć rejestrowanie w aplikacji w języku JavaScript, zobacz [włączania rejestrowania po stronie klienta (wersja klienta JavaScript)](../guide-to-the-api/hubs-api-guide-javascript-client.md#logging). Aby włączyć rejestrowanie w aplikacji klienckiej platformy .NET, zobacz [włączania rejestrowania po stronie klienta (w wersji klienta platformy .NET)](../guide-to-the-api/hubs-api-guide-net-client.md#logging).

### <a name="misspelled-method-incorrect-method-signature-or-incorrect-hub-name"></a>Błędnie napisane metody, podpis metody niepoprawne lub nazwa nieprawidłowa Centrum

Nazwa lub podpis metody o nazwie nie jest dokładnie dopasowana odpowiedniej metody na kliencie, wywołanie zakończy się niepowodzeniem. Sprawdź, czy nazwa metody wywoływane przez serwer odpowiada nazwę metody na kliencie. Ponadto SignalR tworzy serwer proxy koncentratora, za pomocą metod formacie camelcase, jest odpowiednie w języku JavaScript, więc wywołana metoda `SendMessage` na serwerze może być wywoływana `sendMessage` na serwerze proxy klienta. Jeśli używasz `HubName` atrybutu w kodzie po stronie serwera, upewnij się, że nazwa używana odpowiada nazwa służąca do tworzenia koncentratora na kliencie. Jeśli nie używasz `HubName` atrybutu, sprawdź, czy nazwa koncentratora na kliencie JavaScript — w formacie camelcase, takich jak chatHub zamiast ChatHub.

### <a name="duplicate-method-name-on-client"></a>Zduplikowana nazwa metody na kliencie

Sprawdź, czy ma zduplikowaną metodę na komputerze klienckim, który różni się tylko wielkością liter. Jeśli Twoja aplikacja kliencka ma metodę o nazwie `sendMessage`, sprawdź, czy nie ma również metodę o nazwie `SendMessage` również.

### <a name="missing-json-parser-on-the-client"></a>Brak analizatora JSON na komputerze klienckim

SignalR wymaga analizatora składni JSON do serializacji wywołań między serwerem a klientem. Jeśli Twój klient nie ma wbudowanych analizatora składni JSON (np. programu Internet Explorer 7), należy podać jeden w aplikacji. Możesz pobrać analizatora składni JSON [tutaj](http://nuget.org/packages/json2).

### <a name="mixing-hub-and-persistentconnection-syntax"></a>Łączenie Centrum i PersistentConnection składni

SignalR używa dwóch modeli komunikacji: Centra i PersistentConnections. Składnia wywoływania modele te dwa komunikacji różni się w kodzie klienta. Jeśli koncentrator zostały dodane w kodzie serwera, należy sprawdzić, cały kod klienta jest używana składnia odpowiedniego koncentratora.

**Kod klienta JavaScript, który tworzy PersistentConnection klient JavaScript**

[!code-javascript[Main](troubleshooting/samples/sample1.js)]

**Kod klienta JavaScript, który tworzy serwer Proxy koncentratora klienta Javascript**

[!code-javascript[Main](troubleshooting/samples/sample2.js)]

**C# serwera kod, który mapuje trasę PersistentConnection**

[!code-csharp[Main](troubleshooting/samples/sample3.cs)]

**C# serwera kod, który mapuje trasę koncentratora lub wielu centrów, jeśli masz wiele aplikacji**

[!code-css[Main](troubleshooting/samples/sample4.css)]

### <a name="connection-started-before-subscriptions-are-added"></a>Połączenia pracy przed subskrypcje są dodawane

Jeśli połączenie koncentratora jest uruchomiona, zanim metod, które mogą być wywoływane z serwera zostaną dodane do serwera proxy, nie można odebrać wiadomości. Następujący kod JavaScript nie będą prawidłowo uruchomić Centrum:

**Nieprawidłowy kod klienta JavaScript nie pozwoli na Hubs komunikaty do odbioru**

[!code-javascript[Main](troubleshooting/samples/sample5.js)]

Zamiast tego Dodaj subskrypcje metoda przed wywołaniem rozpoczęcia:

**Kod klienta JavaScript, który poprawnie dodaje subskrypcji do koncentratora**

[!code-javascript[Main](troubleshooting/samples/sample6.js)]

### <a name="missing-method-name-on-the-hub-proxy"></a>Brak nazwy metody dla serwera proxy koncentratora

Upewnij się, że metoda, zdefiniowana na serwerze jest subskrybowana na komputerze klienckim. Mimo, że serwer definiuje metodę, nadal należy dodać do serwera proxy klienta. Metody można dodać do serwera proxy klienta w następujący sposób (należy pamiętać, że metoda jest dodawana do `client` członkiem Centrum, a nie bezpośrednio Centrum):

**Kod klienta JavaScript, który dodaje metody do serwera proxy koncentratora**

[!code-javascript[Main](troubleshooting/samples/sample7.js)]

### <a name="hub-or-hub-methods-not-declared-as-public"></a>Koncentrator lub metod koncentratora, które nie jest zadeklarowany jako publiczny

Mają być wyświetlane na komputerze klienckim, implementacji koncentratora i metody musi być zadeklarowany jako `public`.

### <a name="accessing-hub-from-a-different-application"></a>Uzyskiwanie dostępu do Centrum z innej aplikacji

Koncentratory SignalR zostać oceniony jedynie przez aplikacje, które implementują klientów SignalR. SignalR nie może współpracować z innymi bibliotekami komunikacji (na przykład protokołu SOAP usług WCF i sieci web.) Jeśli nie klienta SignalR jest dostępna dla danej platformy docelowej, nie masz dostępu do punktu końcowego serwera bezpośrednio.

### <a name="manually-serializing-data"></a>Ręcznie serializacja danych

SignalR będzie automatycznie używać JSON do serializacji metodę parametry tam nie ma potrzeby zrobić to samodzielnie.

### <a name="remote-hub-method-not-executed-on-client-in-ondisconnected-function"></a>Zdalne metody koncentratora, które nie zostały wykonane na komputerze klienckim w funkcji ondisconnected elementu

To zachowanie jest celowe. Gdy `OnDisconnected` jest wywoływana, koncentrator został już wprowadzone `Disconnected` stanu, który nie zezwala na dalsze można wywołać metody koncentratora.

**C# serwera kod, który prawidłowo wykonuje kod w zdarzeniu ondisconnected elementu**

[!code-csharp[Main](troubleshooting/samples/sample8.cs)]

### <a name="ondisconnect-not-firing-at-consistent-times"></a>Nie uruchomiono w czasie spójny OnDisconnect

To zachowanie jest celowe. Gdy użytkownik próbuje opuścić stronę z aktywnego połączenia SignalR, klienta SignalR następnie wprowadzi próba optymalnych informować serwera, czy połączenie klienta zostaną zatrzymane. Jeśli klienta SignalR optymalnych próba nie powiedzie się uzyskać dostęp do serwera, serwer będzie dysponować połączenia po można skonfigurować `DisconnectTimeout` później, w tym czasie `OnDisconnected` zdarzenia będą uruchamiane. Jeśli klienta SignalR optymalnych próba zakończy się pomyślnie, `OnDisconnected` zdarzenia będą uruchamiane natychmiast.

Aby uzyskać informacje na temat ustawień dotyczących `DisconnectTimeout` ustawienie, zobacz [Obsługa zdarzeń okresu istnienia połączenia: DisconnectTimeout](../guide-to-the-api/handling-connection-lifetime-events.md#disconnecttimeout).

### <a name="connection-limit-reached"></a>Osiągnięto limit połączeń

Korzystając z pełnej wersji programu IIS w systemie operacyjnym klienta, takich jak Windows 7, nakłada się limit 10 połączeń. Korzystając z system operacyjny klienta, użyj usług IIS Express w celu uniknięcia tego limitu.

### <a name="cross-domain-connection-not-set-up-properly"></a>Połączenia między domenami nie są poprawnie ustawione

Jeśli połączenie między domenami (dla której SignalR adres URL nie jest w tej samej domenie co strona macierzysta połączenia) nie jest skonfigurowany poprawnie, połączenie może zakończyć się niepowodzeniem bez komunikatu o błędzie. Aby uzyskać informacje na temat włączania komunikacji między domenami, zobacz [jak nawiązać połączenie między domenami](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain).

### <a name="connection-using-ntlm-active-directory-not-working-in-net-client"></a>Połączenie przy użyciu metod NTLM (Active Directory) nie działa w klienta platformy .NET

Połączenia w aplikacji klienckiej platformy .NET, która używa zabezpieczenia domeny może zakończyć się niepowodzeniem, jeśli połączenie nie jest prawidłowo skonfigurowany. Aby używanie biblioteki SignalR w środowisku domeny, należy ustawić właściwość wymagane połączenie w następujący sposób:

**C# kod klienta, który implementuje poświadczenia połączenia**

[!code-csharp[Main](troubleshooting/samples/sample9.cs)]

<a id="pong"></a>

## <a name="configuring-iis-websockets-to-pingpong-to-detect-a-dead-client"></a>Konfigurowanie websockets usług IIS na polecenie ping/pong wykrywania martwy klienta

Serwery biblioteki SignalR nie wiadomo, jeśli klient jest dead, czy nie, i opierają się na powiadomienie z bazowego websocket błędów połączenia, oznacza to, `OnClose` wywołania zwrotnego. Jedno rozwiązanie tego problemu jest skonfigurować websockets usług IIS, aby wykonać polecenie ping/pong dla Ciebie. Daje to gwarancję, że połączenie zostanie zamknięte, jeśli nieoczekiwanie przerywa. Aby uzyskać więcej informacji, zobacz [ten wpis w witrynie stackoverflow](http://stackoverflow.com/questions/19502755/websocket-clients-state-not-changing-on-network-loss).

<a id="other"></a>

## <a name="other-connection-issues"></a>Inne problemy z połączeniem

W tej sekcji opisano przyczyny i rozwiązania dla symptomy lub komunikaty o błędach, które występują podczas połączenia.

### <a name="start-must-be-called-before-data-can-be-sent-error"></a>Błąd "Start musi zostać wywołana przed wysłaniem danych"

Ten błąd występuje często, jeśli kod odwołuje się do obiektów SignalR przed rozpoczęciem połączenia. Wireup dla programów obsługi i podobne że zostanie wywołania metod, zdefiniowanych na serwerze należy dodać po zakończeniu połączenie. Należy pamiętać, że wywołanie `Start` jest asynchroniczna, więc kończy się kod po wywołaniu mogą być wykonywane przed. Najlepszym sposobem dodania obsługi po connection rozpoczyna się całkowicie jest umieść je w funkcji wywołania zwrotnego, który jest przekazywany jako parametr do metody uruchamiania:

**Kod klienta JavaScript, który dodaje poprawnie procedury obsługi zdarzeń, które odwołują się obiekty SignalR**

[!code-javascript[Main](troubleshooting/samples/sample10.js?highlight=1)]

Ten błąd będzie widoczny także w przypadku, gdy połączenie zatrzymuje się podczas SignalR obiekty są nadal odwołuje.

### <a name="301-moved-permanently-or-302-moved-temporarily-error"></a>Błąd "302 został tymczasowo przeniesiony" lub "301 przeniesiony trwale"

Ten błąd może zobaczyć, jeśli projekt zawiera folder o nazwie SignalR, który będzie zakłócać proxy tworzone automatycznie. Aby uniknąć tego błędu, nie należy używać w folderze o nazwie `SignalR` w aplikacji lub Wyłącz automatyczną serwera proxy generation wyłączone. Zobacz [serwera Proxy, które są generowane i robi dla Ciebie](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy) Aby uzyskać więcej informacji.

### <a name="403-forbidden-error-in-net-or-silverlight-client"></a>"403 Dostęp zabroniony" Błąd w kliencie .NET lub Silverlight

Ten błąd może wystąpić w środowiskach między domenami gdzie komunikacji między domenami nie jest prawidłowo włączone. Aby uzyskać informacje na temat włączania komunikacji między domenami, zobacz [jak nawiązać połączenie między domenami](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain). Aby nawiązać połączenie między domenami w klienta Silverlight, zobacz [połączenia między domenami od klientów programu Silverlight](../guide-to-the-api/hubs-api-guide-net-client.md#slcrossdomain).

### <a name="404-not-found-error"></a>Błąd "404 Nie znaleziono"

Istnieje kilka przyczyn tego problemu. Sprawdź wszystkie z następujących czynności:

- **Nieprawidłowy format odwołanie adresu serwera proxy Centrum:** Ten błąd jest częsta, jeśli odwołanie do Centrum wygenerowany adres serwera proxy nie jest poprawnie sformatowany. Sprawdź, czy adres centrum odniesienia prawidłowo. Zobacz [jak odwoływać się do serwera proxy w dynamicznie generowanym](../guide-to-the-api/hubs-api-guide-javascript-client.md#dynamicproxy) Aby uzyskać szczegółowe informacje.
- **Dodawanie tras do aplikacji, przed dodaniem trasę koncentratora:** Jeśli aplikacja korzysta z innych tras, sprawdź, czy pierwsza trasa dodano wywołanie `MapSignalR`.
- **Za pomocą usług IIS 7 i 7.5 bez aktualizacji dla adresów URL bez rozszerzenia:** Za pomocą usług IIS 7 i 7.5 wymaga aktualizacji dla adresów URL bez rozszerzenia, dzięki czemu serwer może zapewniać dostęp do definicji Centrum w `/signalr/hubs`. Aktualizację można znaleźć [tutaj](https://support.microsoft.com/kb/980368).
- **Pamięć podręczna programu IIS nieaktualne lub uszkodzone:** Aby sprawdzić, czy zawartość pamięci podręcznej nie jest nieaktualna, wprowadź następujące polecenie w oknie programu PowerShell, aby wyczyścić pamięć podręczną:

    [!code-powershell[Main](troubleshooting/samples/sample11.ps1)]

### <a name="500-internal-server-error"></a>"500 Wewnętrzny błąd serwera"

Jest to bardzo ogólny błąd, który może mieć różnych przyczyn. Szczegółowe informacje o błędzie powinien zostać wyświetlony w dzienniku zdarzeń serwera lub znajduje się za pośrednictwem serwera debugowania. Bardziej szczegółowe informacje o błędzie można uzyskać, włączając szczegółowe błędy na serwerze. Aby uzyskać więcej informacji, zobacz [sposób obsługi błędów w klasie Centrum](../guide-to-the-api/hubs-api-guide-server.md#handleErrors).

Ten błąd występuje również powszechnie, jeśli zapora lub serwer proxy nie jest prawidłowo skonfigurowany, powodując nagłówki żądania ponownego napisania. To rozwiązanie jest upewnij się, że port 80 jest włączona na zapory lub serwera proxy.

### <a name="unexpected-response-code-500"></a>"Nieoczekiwany kod odpowiedzi: 500"

Ten błąd może wystąpić, jeśli wersja .NET framework co używany w aplikacji nie jest zgodna wersja określona w pliku Web.Config. To rozwiązanie jest Sprawdź, czy .NET 4.5 jest używany zarówno w ustawieniach aplikacji, jak i w pliku Web.Config.

### <a name="typeerror-lthubtypegt-is-undefined-error"></a>"TypeError: &lt;hubType&gt; zdefiniowano" Błąd

Spowoduje to błąd, jeśli wywołanie `MapSignalR` nie jest wykonywana prawidłowo. Zobacz [jak się zarejestrować oprogramowanie pośredniczące SignalR i skonfiguruj opcje SignalR](../guide-to-the-api/hubs-api-guide-server.md#route) Aby uzyskać więcej informacji.

### <a name="jsonserializationexception-was-unhandled-by-user-code"></a>JsonSerializationException nie został obsłużony przez kod użytkownika

Upewnij się, że parametry, z których możesz wysłać do metody nie zawierają nieprzeznaczone do typów (takich jak dojścia do plików lub połączenia z bazą danych). Jeśli musisz używać elementów członkowskich w obiekcie po stronie serwera, który nie ma do wysłania do klienta, (zarówno dla zabezpieczeń i powodów serializacji), użyj `JSONIgnore` atrybutu.

### <a name="protocol-error-unknown-transport-error"></a>"Błąd protokołu: Błąd transportu nieznany"

Ten błąd może wystąpić, jeśli klient nie obsługuje transportów, które korzysta z biblioteki SignalR. Zobacz [transportu i planów awaryjnych](../getting-started/introduction-to-signalr.md#transports) informacji, na którym przeglądarki mogą być używane z SignalR.

### <a name="javascript-hub-proxy-generation-has-been-disabled"></a>"Generowania serwera proxy koncentratora języka JavaScript została wyłączona."

Ten błąd wystąpi, jeśli `DisableJavaScriptProxies` ustawiono podczas dotyczy to również odwołanie na dynamicznie generowanym serwer proxy na `signalr/hubs`. Aby uzyskać więcej informacji na temat ręcznego tworzenia serwera proxy, zobacz [wygenerowany serwer proxy i przeznaczenie dla Ciebie](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy).

### <a name="the-connection-id-is-in-the-incorrect-format-or-the-user-identity-cannot-change-during-an-active-signalr-connection-error"></a>"Identyfikator połączenia, który jest w niepoprawnym formacie" lub "tożsamość użytkownika nie można zmienić podczas aktywnego połączenia SignalR" Błąd

Ten błąd może być widoczny, gdy jest używane uwierzytelnianie, a klient jest wylogowanie, zanim połączenie zostanie zatrzymana. Rozwiązaniem jest zatrzymanie połączenia SignalR przed wylogowanie klienta.

### <a name="uncaught-error-signalr-jquery-not-found-please-ensure-jquery-is-referenced-before-the-signalrjs-file-error"></a>"Nieprzechwycony błąd: SignalR: jQuery nie można odnaleźć. Upewnij się, że jQuery odwołuje się do pliku SignalR.js"Błąd

Klient SignalR JavaScript wymaga jQuery do uruchomienia. Sprawdź, czy której można się odwołać do technologii jQuery jest poprawna, czy ścieżki jest prawidłowa i czy odwołanie do technologii jQuery jest przed odwołanie do biblioteki SignalR.

### <a name="uncaught-typeerror-cannot-read-property-ltpropertygt-of-undefined-error"></a>"Nieprzechwycony TypeError: Nie można odczytać właściwości "&lt;właściwość&gt;" undefined "Błąd

Ten błąd wynika z nie posiadają jQuery lub serwer proxy koncentratory odpowiednie odwołania. Sprawdź, czy odwołania do jQuery i koncentratory serwer proxy jest prawidłowy, czy ścieżki jest prawidłowa i czy odwołanie do technologii jQuery jest przed odwołanie do serwera proxy koncentratorów. Domyślne odwołanie do serwera proxy koncentratory powinien wyglądać następująco:

**Kod po stronie klienta HTML, która poprawnie odwołuje się do serwera proxy koncentratory**

[!code-html[Main](troubleshooting/samples/sample12.html)]

### <a name="runtimebinderexception-was-unhandled-by-user-code-error"></a>Błąd "RuntimeBinderException nie został obsłużony przez kod użytkownika"

Ten błąd może wystąpić, gdy niepoprawne przeciążenia `Hub.On` jest używany. Jeśli metoda nie zwraca wartości, zwracanym typem musi być określona jako parametr typu ogólnego:

**Metody zdefiniowane na komputerze klienckim (bez wygenerowany serwer proxy)**

[!code-html[Main](troubleshooting/samples/sample13.html?highlight=1)]

### <a name="connection-id-is-inconsistent-or-connection-breaks-between-page-loads"></a>Identyfikator połączenia jest niezgodny lub zerwania połączenia między ładowań strony

To zachowanie jest celowe. Od czasu w obiekcie strony znajduje się obiekt koncentratora, koncentratora jest niszczony, kiedy odświeżeniu strony. Aplikacja wielostronicowych musi zachować skojarzenia między użytkownikami i identyfikatorów połączeń, tak aby były one wspólne dla ładowań strony. Identyfikatorów połączeń mogą być przechowywane na serwerze albo `ConcurrentDictionary` obiektu lub bazy danych.

### <a name="value-cannot-be-null-error"></a>Błąd "Wartość nie może mieć wartości null"

Metod po stronie serwera z opcjonalnymi parametrami nie są obecnie obsługiwane; opcjonalny parametr zostanie pominięty, metoda zakończy się niepowodzeniem. Aby uzyskać więcej informacji, zobacz [następujące parametry opcjonalne](https://github.com/SignalR/SignalR/issues/324).

### <a name="firefox-cant-establish-a-connection-to-the-server-at-ltaddressgt-error-in-firebug"></a>"Firefox nie może nawiązać połączenia z serwerem w &lt;adres&gt;" Błąd Firebug

Ten komunikat o błędzie są widoczne w Firebug, jeśli negocjacji protokołu WebSocket transport zakończy się niepowodzeniem, a innego transportu jest używana zamiast tego. To zachowanie jest celowe.

### <a name="the-remote-certificate-is-invalid-according-to-the-validation-procedure-error-in-net-client-application"></a>Błąd "jest nieprawidłowy według procedury weryfikacji certyfikatu zdalnego" w aplikacji klienckiej platformy .NET

Jeśli serwer wymaga certyfikatów klienta, a następnie dodać x509certificate do połączenia przed wykonaniem żądania. Dodaj certyfikat do połączenia przy użyciu `Connection.AddClientCertificate`.

### <a name="connection-drops-after-authentication-times-out"></a>Połączenie spada po upłynie limit czasu uwierzytelniania

To zachowanie jest celowe. Poświadczenia uwierzytelniania nie może być modyfikowany, gdy połączenie jest aktywne; Aby odświeżyć poświadczenia, połączenie musi być zatrzymana i uruchomiona ponownie.

### <a name="onconnected-gets-called-twice-when-using-jquery-mobile"></a>Onconnected elementu jest wywoływana dwa razy, gdy przy użyciu technologii jQuery Mobile

jQuery Mobile `initializePage` funkcja wymusza skrypty na każdej stronie, aby ponownie uruchomił, co tworzy drugie połączenie. Rozwiązania tego problemu obejmują:

- Zawierać odwołanie do technologii jQuery Mobile przed pliku JavaScript.
- Wyłącz `initializePage` funkcji, ustawiając `$.mobile.autoInitializePage = false`.
- Poczekaj, aż strony Aby zakończyć inicjowanie przed rozpoczęciem połączenia.

### <a name="messages-are-delayed-in-silverlight-applications-using-server-sent-events"></a>Komunikaty są opóźnione w aplikacji Silverlight, przy użyciu serwera wysyłane zdarzenia

Wiadomości są opóźnione, gdy za pomocą serwera wysyłane zdarzenia na dodatku Silverlight. Aby wymusić długim sondowaniem ma być używany w zamian, należy użyć następującego po zainicjowanie połączenia:

[!code-css[Main](troubleshooting/samples/sample14.css)]

### <a name="permission-denied-using-forever-frame-protocol"></a>Nieskończoność "Odmowa uprawnień" za pomocą ramki protokołu

Jest to znany problem opisany [tutaj](https://github.com/SignalR/SignalR/issues/1963). Mogą być widoczne symptomami przy użyciu najnowszej biblioteki JQuery; Obejście polega na starszą wersję aplikacji do technologii JQuery 1.8.2.

### <a name="invalidoperationexception-not-a-valid-web-socket-request"></a>"InvalidOperationException: Nie prawidłowe żądanie gniazda sieci web.

Ten błąd może wystąpić, jeśli jest używany protokół WebSocket, ale serwer proxy sieci modyfikuje nagłówki żądania. Rozwiązanie jest skonfigurować serwer proxy, aby umożliwić WebSocket na porcie 80.

### <a name="exception-ltmethod-namegt-method-could-not-be-resolved-when-client-calls-method-on-server"></a>"Wyjątków: &lt;nazwę metody&gt; nie można rozpoznać metody" Kiedy klient wywołuje metodę na serwerze

Ten błąd może wynikać z używanie typów danych, których nie można odnaleźć w ładunku JSON, takich jak tablica. Obejście polega na typ danych, który jest wykrywane przez usługę JSON, takich jak IList. Aby uzyskać więcej informacji, zobacz [klienta platformy .NET nie można wywołać metod koncentratorów, za pomocą tablicy parametrów](https://github.com/SignalR/SignalR/issues/2672).

<a id="server"></a>

## <a name="compilation-and-server-side-errors"></a>Błędy kompilacji i po stronie serwera

 Poniższa sekcja zawiera możliwe rozwiązania kompilatora oraz błędów środowiska uruchomieniowego po stronie serwera.

### <a name="reference-to-hub-instance-is-null"></a>Odwołanie do wystąpienia Centrum ma wartość null

Ponieważ wystąpienie koncentratora jest tworzony dla każdego połączenia, nie można utworzyć wystąpienie koncentratora w kodzie użytkownika. Wywoływanie metod koncentratora z poza Centrum, samego, zobacz [jak wywołać metody klienta grup i zarządzanie nimi z poza klasy koncentratora](../guide-to-the-api/hubs-api-guide-server.md#callfromoutsidehub) dotyczące sposobu uzyskania odwołania do kontekst koncentratora.

### <a name="httpcontextcurrentsession-is-null"></a>HTTPContext.Current.Session ma wartość null

To zachowanie jest celowe. SignalR nie obsługuje stanu sesji programu ASP.NET, ponieważ Włączanie stanu sesji zaburzyłaby dwukierunkowego komunikatów.

### <a name="no-suitable-method-to-override"></a>Nie odpowiedniej metody do przesłonięcia

Może zostać wyświetlony ten błąd, jeśli używasz kodu z ze starszą dokumentacją lub blogów. Sprawdź, czy użytkownik nie odwołują się do nazw metod, które zostały zmienione lub przestarzałe (takich jak `OnConnectedAsync`).

### <a name="hostcontextextensionswebsocketserverurl-is-null"></a>HostContextExtensions.WebSocketServerUrl ma wartość null

To zachowanie jest celowe. Ta składowa jest przestarzała i nie powinna być używana.

### <a name="a-route-named-signalrhubs-is-already-in-the-route-collection-error"></a>Trasę o nazwie "signalr.hubs" błąd "jest już w kolekcji trasę"

Ten błąd będzie widoczny, gdy `MapSignalR` jest wywoływana dwa razy przez aplikację. Niektóre wywołania aplikacji przykład `MapSignalR` bezpośrednio w klasie uruchamiania; inne wykonać wywołanie klasy otoki. Upewnij się, że aplikacja nie wykonać obie czynności.

### <a name="websocket-is-not-used"></a>WebSocket nie jest używany.

Jeśli upewnieniu się, że serwerem a klientami spełniają wymagania protokołu WebSocket (wymienione w [obsługiwane platformy](../getting-started/supported-platforms.md) dokumentu), musisz włączyć WebSocket na serwerze. Instrukcje w ten sposób można znaleźć [tutaj](https://www.iis.net/learn/get-started/whats-new-in-iis-8/iis-80-websocket-protocol-support).

### <a name="connection-is-undefined"></a>$.connection jest niezdefiniowana

Ten błąd wskazuje, że skrypty na stronie nie są ładowane prawidłowo lub serwera proxy koncentratora nie jest dostępny lub jest niepoprawnie dostępny. Sprawdź, czy odwołania do skryptu na stronie odpowiadają skrypty ładowane w projekcie i że /signalr/hubs będą dostępne w przeglądarce, gdy serwer jest uruchomiony.

### <a name="one-or-more-types-required-to-compile-a-dynamic-expression-cannot-be-found"></a>Nie można odnaleźć co najmniej jednego typu wymaganego do skompilowania wyrażenia dynamicznego

Ten błąd wskazuje, że `Microsoft.CSharp` Brak biblioteki. Dodaj ją w **zestawów -&gt;Framework** kartę.

### <a name="caller-state-cannot-be-accessed-from-clientscaller-in-visual-basic-or-in-a-strongly-typed-hub-conversion-from-type-taskof-object-to-type-string-is-not-valid-error"></a>Stan elementu wywołującego, nie są dostępne z Clients.Caller w języku Visual Basic lub w Centrum silnie typizowaną; Błąd "Konwersja z typu"Task (Object o)"na typ"String"jest nieprawidłowa"

Aby uzyskać dostęp stan elementu wywołującego w języku Visual Basic lub w silnie typizowany Centrum, użyj `Clients.CallerState` własności (zostanie wprowadzony w SignalR 2.1) zamiast `Clients.Caller`.

<a id="vs"></a>

## <a name="visual-studio-issues"></a>Problemy z programu Visual Studio

W tej sekcji opisano problemy napotkane w programie Visual Studio.

### <a name="script-documents-node-does-not-appear-in-solution-explorer"></a>Nie ma węzła dokumenty skryptów w Eksploratorze rozwiązań

Niektóre z naszych samouczków przekierowanie do węzła "Dokumenty skryptów" w Eksploratorze rozwiązań podczas debugowania. Ten węzeł jest generowany przez debuger JavaScript, a zostanie wyświetlony tylko podczas debugowania na komputerach klienckich w programie Internet Explorer; węzeł nie będą wyświetlane, jeśli używane są przeglądarki Chrome lub Firefox. Debuger JavaScript również nie będzie działać, jeśli jest uruchomiony inny debuger klienta, takich jak Silverlight debugger.

### <a name="signalr-does-not-work-on-visual-studio-2008-or-earlier"></a>SignalR nie działa w programie Visual Studio 2008 lub starszej

To zachowanie jest celowe. SignalR wymaga programu .NET Framework 4 lub nowszy; wymaga to, że aplikacji SignalR być opracowane w programie Visual Studio 2010 lub nowszej. Składnik serwera signalr wymaga programu .NET Framework 4.5.

<a id="iis"></a>

## <a name="iis-issues"></a>Problemy dotyczące usług IIS

Ta sekcja zawiera problemy związane z internetowych usług informacyjnych.

### <a name="signalr-works-on-visual-studio-development-server-but-not-in-iis"></a>SignalR działa na serwerze deweloperskim programu Visual Studio, ale nie w usługach IIS

SignalR jest obsługiwane w usługach IIS 7.0 i 7.5, ale Obsługa adresów URL bez rozszerzenia, które muszą zostać dodane. Aby dodać obsługę adresów URL bez rozszerzenia, zobacz [https://support.microsoft.com/kb/980368](https://support.microsoft.com/kb/980368)

SignalR wymaga programu ASP.NET, należy zainstalować na serwerze (ASP.NET nie jest zainstalowany na serwerze IIS domyślnie). Aby zainstalować program ASP.NET, zobacz [ASP.NET pliki do pobrania](https://www.asp.net/downloads).

<a id="azure"></a>

## <a name="microsoft-azure-issues"></a>Problemy z Microsoft Azure

Ta sekcja zawiera problemów z platformą Microsoft Azure.

### <a name="fileloadexception-when-hosting-signalr-in-an-azure-worker-role"></a>Fileloadexception — w przypadku hostowania SignalR w roli procesu roboczego platformy Azure

Hosting SignalR w roli procesu roboczego platformy Azure może spowodować wyjątek "nie można załadować pliku lub zestawu ' Microsoft.Owin, Version = 2.0.0.0". Jest to znany problem z NuGet; Przekierowania powiązań nie są automatycznie dodawane w projektach roli procesu roboczego platformy Azure. Aby rozwiązać ten problem, można ręcznie dodać przekierowania powiązań. Dodaj następujące wiersze do `app.config` plików dla projektu roli proces roboczy.

[!code-xml[Main](troubleshooting/samples/sample15.xml)]

### <a name="messages-are-not-received-through-the-azure-backplane-after-altering-topic-names"></a>Komunikaty nie są odbierane za pośrednictwem platformy Azure płyty montażowej po zmieniania nazwy tematów

Tematy używana przez Azure płyty montażowej są przechowywane wewnętrznie; nie są przeznaczone do można skonfigurować dla użytkownika.
