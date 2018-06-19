---
uid: signalr/overview/older-versions/troubleshooting
title: Rozwiązywanie problemów z SignalR (SignalR 1.x) | Dokumentacja firmy Microsoft
author: pfletcher
description: W tym artykule opisano często występujące problemy z opracowywanie aplikacji SignalR.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/05/2013
ms.topic: article
ms.assetid: 347210ba-c452-4feb-886f-b51d89f58971
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/troubleshooting
msc.type: authoredcontent
ms.openlocfilehash: eb739f806eb57d09008fa0bf04b5a40721dab73d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
ms.locfileid: "26566051"
---
<a name="signalr-troubleshooting-signalr-1x"></a>Rozwiązywanie problemów z SignalR (SignalR 1.x)
====================
przez [Patrick Fletcher](https://github.com/pfletcher)

> W tym dokumencie opisano typowe problemy dotyczące rozwiązywania problemów z SignalR.


Ten dokument zawiera następujące sekcje.

- [Wywoływanie metody między klientem i serwerem dyskretnie kończy się niepowodzeniem](#connection)
- [Inne problemy z połączeniem](#other)
- [Błędy kompilacji i po stronie serwera](#server)
- [Problemy z usługą Visual Studio](#vs)
- [Problemy z internetowych usług informacyjnych](#iis)
- [Problemy z platformy Azure](#azure)

<a id="connection"></a>

## <a name="calling-methods-between-the-client-and-server-silently-fails"></a>Wywoływanie metody między klientem i serwerem dyskretnie kończy się niepowodzeniem

W tej sekcji opisano możliwe przyczyny wywołania metody między klientem i serwerem niepowodzenie bez komunikat o błędzie łatwy do rozpoznania. W aplikacji SignalR serwer nie ma informacji o metod, które implementuje w kliencie; gdy serwer wywołuje metodę klienta, dane nazwy i parametru metody są wysyłane do klienta, a metoda jest wykonywana tylko wtedy, gdy istnieje w formacie, który określony serwer. Jeśli odpowiadającej metody znajduje się na kliencie, nic się nie dzieje i żaden komunikat o błędzie nie jest zgłaszany na serwerze.

W celu dalszego badania nie pobierania wywoływać metody klienta, można włączyć rejestrowanie przed wywołaniem metody start koncentratora, aby zobaczyć, jakie wywołania pochodzą z serwera. Aby włączyć rejestrowanie w aplikacji JavaScript, zobacz [włączania rejestrowania po stronie klienta (wersja klienta JavaScript)](../guide-to-the-api/hubs-api-guide-javascript-client.md#logging). Aby włączyć rejestrowanie w aplikacji klienckiej .NET, zobacz [włączania rejestrowania po stronie klienta (wersja klienta .NET)](../guide-to-the-api/hubs-api-guide-net-client.md#logging).

### <a name="misspelled-method-incorrect-method-signature-or-incorrect-hub-name"></a>Błędnie — metoda, podpis metody nieprawidłowe lub nazwa nieprawidłowa koncentratora

Jeśli w nazwie lub podpisie metody wywoływanego jest dokładnie zgodny odpowiedniej metody na kliencie, wywołanie zakończy się niepowodzeniem. Sprawdź, czy nazwa metody wywoływane przez serwer zgodna Nazwa metody na kliencie. Ponadto SignalR tworzy serwera proxy koncentratora, za pomocą metody formatu — z uwzględnieniem wielkości liter, jest odpowiednie w języku JavaScript, więc wywołana metoda `SendMessage` na serwerze może być wywoływana `sendMessage` na serwerze proxy klienta. Jeśli używasz `HubName` atrybutu w kodzie po stronie serwera, sprawdź, czy nazwa używana zgodna z nazwą pozwala utworzyć koncentratora na kliencie. Jeśli nie używasz `HubName` atrybutu, sprawdzić, czy nazwa koncentratora na kliencie JavaScript jest formatu — z uwzględnieniem wielkości liter, takie jak chatHub zamiast ChatHub.

### <a name="duplicate-method-name-on-client"></a>Zduplikowana nazwa metody na kliencie

Sprawdź, czy nie zduplikowaną metodę na kliencie, który różni się tylko wielkością liter. Jeśli aplikacja kliencka ma metodę o nazwie `sendMessage`, sprawdź, czy nie ma również metody o nazwie `SendMessage` również.

### <a name="missing-json-parser-on-the-client"></a>Brak analizator JSON na kliencie

SignalR wymaga analizatora składni JSON do serializacji połączeń między serwerem a klientem. Jeśli klient nie ma wbudowanej analizatora składni JSON (takich jak program Internet Explorer 7), należy uwzględnić w aplikacji. Możesz pobrać analizatora składni JSON [tutaj](http://nuget.org/packages/json2).

### <a name="mixing-hub-and-persistentconnection-syntax"></a>Mieszanie koncentratora i klasy PersistentConnection składni

Biblioteka SignalR używa dwóch modeli komunikacji: koncentratorów i PersistentConnections. Składnia wywoływania tych dwóch komunikacji modeli różni się w kodzie klienta. Jeśli koncentrator został dodany w kodzie serwera, Sprawdź cały kod klienta jest używana składnia prawidłowego koncentratora.

**Kod klienta JavaScript, który tworzy klasy PersistentConnection w kliencie JavaScript**

[!code-javascript[Main](troubleshooting/samples/sample1.js)]

**Kod JavaScript klienta, która tworzy Proxy koncentratora na kliencie Javascript**

[!code-javascript[Main](troubleshooting/samples/sample2.js)]

**C# serwera kod, który mapuje trasę do klasy PersistentConnection**

[!code-csharp[Main](troubleshooting/samples/sample3.cs)]

**C# serwera kod, który mapuje trasy z koncentratorem lub do wielu koncentratorów, jeśli masz wiele aplikacji**

[!code-csharp[Main](troubleshooting/samples/sample4.cs)]

### <a name="connection-started-before-subscriptions-are-added"></a>Połączenie zostało uruchomione przed subskrypcje są dodawane

Jeśli połączenie koncentratora jest uruchomiona, zanim metod, które mogą być wywoływane z serwera zostaną dodane do serwera proxy, nie można odebrać wiadomości. Następujący kod JavaScript nie zostanie prawidłowo uruchomić Centrum:

**Niepoprawny kod klienta JavaScript nie zezwala na komunikaty koncentratory do odebrania**

[!code-javascript[Main](troubleshooting/samples/sample5.js)]

Zamiast tego dodać subskrypcji metoda przed wywołaniem metody Start:

**Kod klienta JavaScript poprawnie dodające subskrypcji z koncentratorem**

[!code-javascript[Main](troubleshooting/samples/sample6.js)]

### <a name="missing-method-name-on-the-hub-proxy"></a>Brak nazwy metody dla serwera proxy koncentratora

Upewnij się, że metoda, zdefiniowanego na serwerze jest subskrybentem na kliencie. Nawet jeśli serwer definiuje metodę, nadal należy dodać do serwera proxy klienta. Metody można dodać do serwera proxy klienta w następujący sposób (należy pamiętać, że metoda jest dodawana do `client` elementu członkowskiego koncentratora, a nie bezpośrednio koncentratora):

**Kod JavaScript klienta, który dodaje metody do serwera proxy koncentratora**

[!code-javascript[Main](troubleshooting/samples/sample7.js)]

### <a name="hub-or-hub-methods-not-declared-as-public"></a>Centrum i metod koncentratorów nie jest zadeklarowany jako publiczny

Aby były widoczne na kliencie, implementacji koncentratora i metody musi być zadeklarowany jako `public`.

### <a name="accessing-hub-from-a-different-application"></a>Uzyskiwanie dostępu do Centrum z innej aplikacji

Koncentratory SignalR można uzyskać tylko za pośrednictwem aplikacji, które implementują klientów SignalR. SignalR nie może współpracować z innymi bibliotekami komunikacji (np. SOAP lub WCF usługi sieci web.) Jeśli nie jest dostępny dla danej platformy docelowej nie klienta SignalR, nie masz dostępu do serwera punktu końcowego bezpośrednio.

### <a name="manually-serializing-data"></a>Ręcznie serializacja danych

SignalR będą automatycznie używać JSON do serializacji metodę parametry tam nie ma potrzeby zrobić samodzielnie.

### <a name="remote-hub-method-not-executed-on-client-in-ondisconnected-function"></a>Nie wykonano na kliencie w funkcji ondisconnected elementu zdalnym metody koncentratora

To zachowanie jest celowe. Gdy `OnDisconnected` jest wywoływana, koncentratora został już wprowadzony `Disconnected` stanu, który nie zezwala na dalsze można wywołać metody koncentratora.

**C# serwera kod, który prawidłowo wykonuje kod w zdarzeniu ondisconnected elementu**

[!code-csharp[Main](troubleshooting/samples/sample8.cs)]

### <a name="connection-limit-reached"></a>Osiągnięto limit połączeń

Korzystając z pełną wersję programu IIS w systemie operacyjnym klienta, takich jak Windows 7, nakłada się limit 10 połączeń. Podczas korzystania z systemu operacyjnego klienta, użyj programu IIS Express w celu uniknięcia tego limitu.

### <a name="cross-domain-connection-not-set-up-properly"></a>Połączenie między domenami nie zostało prawidłowo skonfigurowane

Jeśli połączenie między domenami (połączenie dla którego SignalR adres URL nie jest w tej samej domenie co stronę hostingu) nie jest skonfigurowane poprawnie, połączenie może zakończyć się niepowodzeniem bez komunikatu o błędzie. Aby uzyskać informacje na temat włączania komunikacji między domenami, zobacz [jak nawiązać połączenie między domenami](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain).

### <a name="connection-using-ntlm-active-directory-not-working-in-net-client"></a>Połączenia przy użyciu protokołu NTLM (Active Directory), nie będzie działać w klienta .NET

Połączenie w aplikacji klienta .NET, która używa zabezpieczeń domeny może zakończyć się niepowodzeniem, jeśli połączenie nie jest prawidłowo skonfigurowany. Aby użyć SignalR w środowisku domeny, należy ustawić właściwość wymagane połączenia w następujący sposób:

**C# klienta kod, który implementuje poświadczeń dla połączenia**

[!code-csharp[Main](troubleshooting/samples/sample9.cs)]

<a id="other"></a>

## <a name="other-connection-issues"></a>Inne problemy z połączeniem

W tej sekcji opisano przyczyny i rozwiązania symptomy lub komunikaty o błędach, jakie występują podczas połączenia.

### <a name="start-must-be-called-before-data-can-be-sent-error"></a>Błąd "Start musi zostać wywołana przed wysłaniem danych"

Ten błąd występuje często, jeśli kod odwołuje się do obiektów SignalR przed rozpoczęciem połączenia. Wireup dla programów obsługi i podobnych czy będzie wywołania metody, zdefiniowanego na serwerze należy dodać po zakończeniu połączenia. Należy pamiętać, że wywołanie `Start` jest asynchroniczne, więc zakończeniu kod po wywołaniu mogą być wykonywane przed. Dodaj programy obsługi, połączenie zostanie całkowicie rozpoczęta najlepiej do funkcji wywołania zwrotnego, która jest przekazywana jako parametr do metody uruchamiania:

**Kod klienta JavaScript poprawnie dodające procedury obsługi zdarzeń, które odwołują się obiekty SignalR**

[!code-javascript[Main](troubleshooting/samples/sample10.js?highlight=1)]

Ten błąd będzie także wystąpić, jeśli połączenie zostanie zatrzymana, gdy SignalR obiekty są nadal odwołuje.

### <a name="301-moved-permanently-or-302-moved-temporarily-error"></a>"301 Przeniesiono trwale" lub "302 został tymczasowo przeniesiony" błędu

Ten błąd może wystąpić, jeśli projekt zawiera folder o nazwie SignalR, który będzie zakłócać proxy tworzone automatycznie. Aby uniknąć tego błędu, nie należy używać folder o nazwie `SignalR` w aplikacji, lub Włącz automatyczną serwera proxy generowania off. Zobacz [Proxy generowany i jakie operacje dla Ciebie](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy) więcej szczegółów.

### <a name="403-forbidden-error-in-net-or-silverlight-client"></a>Błąd "403 zabroniony" w kliencie .NET lub Silverlight

Ten błąd może wystąpić w środowiskach międzydomenowego, gdzie komunikacji między domenami nie jest prawidłowo włączone. Aby uzyskać informacje na temat włączania komunikacji między domenami, zobacz [jak nawiązać połączenie między domenami](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain). Aby nawiązać połączenie między domenami w kliencie programu Silverlight, zobacz [międzydomenowego połączenia od klientów programu Silverlight](../guide-to-the-api/hubs-api-guide-net-client.md#slcrossdomain).

### <a name="404-not-found-error"></a>Błąd "nie można odnaleźć 404"

Istnieje kilka przyczyn tego problemu. Sprawdź wszystkie z następujących czynności:

- **Odwołanie do adresu serwera proxy koncentratora nieprawidłowo sformatowana:** ten błąd często jest widoczny, gdy odwołanie do wygenerowanego Centrum adres serwera proxy nie jest prawidłowo sformatowany. Upewnij się, że adres centrum odniesienia poprawnie. Zobacz [jak odwołuje się dynamicznie generowanym obiektu pośredniczącego](../guide-to-the-api/hubs-api-guide-javascript-client.md#dynamicproxy) szczegółowe informacje.
- **Dodawanie tras do aplikacji przed dodaniem trasę koncentratora:** Jeśli aplikacja korzysta z innych tras, sprawdź, czy trasa pierwszy dodawane wywołania `MapHubs`.

### <a name="500-internal-server-error"></a>"500 Wewnętrzny błąd serwera"

Jest to bardzo ogólny błąd, który może występować wiele różnych przyczyn. Szczegóły błędu powinny być wyświetlane w dzienniku zdarzeń serwera lub znajduje się za pomocą debugowania serwera. Bardziej szczegółowe informacje o błędzie można uzyskać poprzez włączenie szczegółowe błędy na serwerze. Aby uzyskać więcej informacji, zobacz [sposób obsługi błędów w klasie Centrum](../guide-to-the-api/hubs-api-guide-server.md#handleErrors).

### <a name="typeerror-lthubtypegt-is-undefined-error"></a>"TypeError: &lt;hubType&gt; zdefiniowano" Błąd

Spowoduje to błąd wywołania `MapHubs` nie zostało utworzone prawidłowo. Zobacz [jak zarejestrować trasy SignalR i skonfigurować opcje SignalR](../guide-to-the-api/hubs-api-guide-server.md#route) Aby uzyskać więcej informacji.

### <a name="jsonserializationexception-was-unhandled-by-user-code"></a>JsonSerializationException został obsłużony przez kod użytkownika

Sprawdź parametry, których wysyłanie do metody nie dołączaj do serializacji typów (na przykład dojścia do plików lub połączenia z bazą danych). Jeśli musisz użyć elementów członkowskich w obiekcie po stronie serwera, który nie ma do wysłania do klienta (lub zabezpieczeń ze względu na serializacji), użyj `JSONIgnore` atrybutu.

### <a name="protocol-error-unknown-transport-error"></a>"Błąd protokołu: nieznany transport" Błąd

Ten błąd może wystąpić, jeśli klient nie obsługuje transportów, które korzysta z SignalR. Zobacz [transportu i przejścia](../getting-started/introduction-to-signalr.md#transports) informacje, na którym przeglądarki może być używany z SignalR.

### <a name="javascript-hub-proxy-generation-has-been-disabled"></a>"Generowanie serwera proxy elementu Hub w języku JavaScript zostało wyłączone."

Ten błąd wystąpi, jeśli `DisableJavaScriptProxies` ustawiono podczas także w tym odwołania na dynamicznie generowanym serwer proxy na `signalr/hubs`. Aby uzyskać więcej informacji na temat tworzenia serwer proxy ręcznie, zobacz [wygenerowany serwer proxy i jakie operacje dla Ciebie](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy).

### <a name="the-connection-id-is-in-the-incorrect-format-or-the-user-identity-cannot-change-during-an-active-signalr-connection-error"></a>"Identyfikator połączenia, który ma niepoprawny format" lub "tożsamości użytkownika nie można zmieniać podczas aktywnego połączenia SignalR" Błąd

Ten błąd może wystąpić, jeśli jest używane uwierzytelnianie, a klient wylogowaniu przed połączenie zostanie zatrzymana. Rozwiązaniem jest zatrzymanie połączenia SignalR, przed zalogowaniem klienta.

### <a name="uncaught-error-signalr-jquery-not-found-please-ensure-jquery-is-referenced-before-the-signalrjs-file-error"></a>"Nieprzechwyconego błędu: SignalR: nie można odnaleźć jQuery. Upewnij się, że jQuery odwołuje się do pliku SignalR.js"Błąd

Klient programu SignalR JavaScript wymaga jQuery do uruchomienia. Sprawdź, czy odwołania do jQuery jest poprawna, czy ścieżki jest prawidłowa i czy odniesienie do jQuery jest przed odwołanie do biblioteki SignalR.

### <a name="uncaught-typeerror-cannot-read-property-ltpropertygt-of-undefined-error"></a>"Nieprzechwyconego TypeError: nie można odczytać właściwości"&lt;właściwości&gt;"undefined" Błąd

Ten błąd wynika z nie posiadają jQuery lub serwera proxy koncentratory odpowiednie odwołania. Sprawdź, czy odwołania do jQuery i koncentratory serwera proxy jest poprawna, czy ścieżki jest prawidłowa i czy odniesienie do jQuery jest przed odwołanie do serwera proxy koncentratorów. Domyślne odwołanie do serwera proxy koncentratory powinna wyglądać następująco:

**Kod HTML po stronie klienta, która poprawnie odwołuje się do serwera proxy koncentratory**

[!code-html[Main](troubleshooting/samples/sample11.html)]

### <a name="runtimebinderexception-was-unhandled-by-user-code-error"></a>Błąd "RuntimeBinderException został obsłużony przez kod użytkownika"

Ten błąd może wystąpić, gdy niepoprawne przeciążenia `Hub.On` jest używany. Jeśli metoda ma wartość zwracaną, zwracany typ musi być określona jako parametr typu ogólnego:

**Metody zdefiniowane na kliencie (bez wygenerowany serwer proxy)**

[!code-html[Main](troubleshooting/samples/sample12.html?highlight=1)]

### <a name="connection-id-is-inconsistent-or-connection-breaks-between-page-loads"></a>Identyfikator połączenia jest niespójna lub przerywa połączenia między elementami strony

To zachowanie jest celowe. Ponieważ obiekt koncentratora znajduje się w obiekcie strony, koncentratora zostanie zniszczony po odświeżeniu strony. Aplikacji wielostronicowych musi zachować skojarzenia między użytkownikami a identyfikatorów połączeń, dzięki czemu będą one spójne ładunków strona. Identyfikatorów połączeń mogą być przechowywane na serwerze w jednej `ConcurrentDictionary` obiektu lub bazy danych.

### <a name="value-cannot-be-null-error"></a>Błąd "Wartości nie może mieć wartości null"

Po stronie serwera metody z opcjonalnymi parametrami nie są obecnie obsługiwane; opcjonalny parametr zostanie pominięty, metoda zakończy się niepowodzeniem. Aby uzyskać więcej informacji, zobacz [następujące parametry opcjonalne](https://github.com/SignalR/SignalR/issues/324).

### <a name="firefox-cant-establish-a-connection-to-the-server-at-ltaddressgt-error-in-firebug"></a>"Firefox nie może nawiązać połączenie z serwerem przy &lt;adres&gt;" Wystąpił błąd podczas Firebug

Ten komunikat o błędzie może być widoczny w Firebug w przypadku awarii negocjacji protokołu WebSocket transportu i zamiast niego jest używana innego transportu. To zachowanie jest celowe.

### <a name="the-remote-certificate-is-invalid-according-to-the-validation-procedure-error-in-net-client-application"></a>Błąd "jest nieprawidłowa przy uwzględnieniu procedurę sprawdzania poprawności certyfikatu zdalnego" w aplikacji klienta .NET

Jeśli serwer wymaga certyfikatów klienta, a następnie dodać x509certificate do połączenia przed dokonaniem żądania. Dodawanie certyfikatu do połączenia za pomocą `Connection.AddClientCertificate`.

### <a name="connection-drops-after-authentication-times-out"></a>Porzuca połączenia po upływie limitu czasu uwierzytelniania

To zachowanie jest celowe. Nie można zmodyfikować poświadczenia uwierzytelniania, gdy połączenie jest aktywne; Aby odświeżyć poświadczenia, połączenie musi zatrzymana i uruchomiona ponownie.

### <a name="onconnected-gets-called-twice-when-using-jquery-mobile"></a>OnConnected jest wywoływana dwukrotnie przy użyciu jQuery Mobile

jQuery Mobile `initializePage` funkcja wymusza skryptów na każdej stronie, które mają być wykonane ponownie, co powoduje utworzenie drugiego połączenia. Rozwiązania tego problemu obejmują:

- Zawiera odniesienie do przed plik JavaScript, jQuery Mobile.
- Wyłącz `initializePage` funkcja przez ustawienie `$.mobile.autoInitializePage = false`.
- Poczekaj na stronie, aby zakończyć inicjowanie przed rozpoczęciem połączenia.

### <a name="messages-are-delayed-in-silverlight-applications-using-server-sent-events"></a>Komunikaty są opóźnione w aplikacji Silverlight przy użyciu zdarzenia wysyłane serwera

Przy użyciu serwera wysyłane zdarzenia na Silverlight opóźnienia wiadomości. Aby wymusić długi sondowania zamiast tego należy użyć, należy użyć następującego po rozpoczęciu połączenia:

[!code-css[Main](troubleshooting/samples/sample13.css)]

### <a name="permission-denied-using-forever-frame-protocol"></a>Nieskończona "Odmowa uprawnienia" za pomocą ramki protokołu

Jest to znany problem opisano [tutaj](https://github.com/SignalR/SignalR/issues/1963). Objawem tego mogą być widoczne za pomocą najnowszej biblioteki JQuery; Obejście polega na starszą wersję aplikacji JQuery 1.8.2.

<a id="server"></a>

## <a name="compilation-and-server-side-errors"></a>Błędy kompilacji i po stronie serwera

 Poniższa sekcja zawiera możliwych rozwiązań kompilatora i błędy podczas wykonywania po stronie serwera. 

### <a name="reference-to-hub-instance-is-null"></a>Odwołanie do wystąpienia Centrum ma wartość null

Ponieważ wystąpienie koncentratora jest tworzony dla każdego połączenia, nie można utworzyć wystąpienia koncentratora w kodzie użytkownika. Aby wywołać metod w koncentratorze z poza samego koncentratora, zobacz [jak wywoływać metod klienta i zarządzanie grupami z poza klasą Centrum](../guide-to-the-api/hubs-api-guide-server.md#callfromoutsidehub) dotyczące sposobu uzyskania odwołania do kontekst koncentratora.

### <a name="httpcontextcurrentsession-is-null"></a>HTTPContext.Current.Session ma wartość null

To zachowanie jest celowe. SignalR nie obsługuje stanu sesji ASP.NET, ponieważ włączenie stanu sesji spowoduje przerwanie dupleksowy do obsługi komunikatów.

### <a name="no-suitable-method-to-override"></a>Nie odpowiedniej metody do przesłonięcia

Ten błąd może być wyświetlana, jeśli używasz kodu ze starszą dokumentacją lub blogów. Sprawdź, czy użytkownik nie odwołują się do nazwy metody, które zostały zmienione lub przestarzałe (takie jak `OnConnectedAsync`).

### <a name="hostcontextextensionswebsocketserverurl-is-null"></a>HostContextExtensions.WebSocketServerUrl ma wartość null

To zachowanie jest celowe. Ten element członkowski jest przestarzały i nie powinna być używana.

### <a name="a-route-named-signalrhubs-is-already-in-the-route-collection-error"></a>Trasa o nazwie "signalr.hubs" błąd "jest już w kolekcji tras"

Ten błąd będzie widoczny, gdy `MapHubs` jest wywoływana dwukrotnie przez aplikację. Niektóre przykładowe aplikacje wywołania `MapHubs` bezpośrednio w pliku globalnego aplikacji; innymi wywoływania klasy otoki. Upewnij się, że aplikacja nie wykonać obie czynności.

<a id="vs"></a>

## <a name="visual-studio-issues"></a>Problemy z usługą Visual Studio

W tej sekcji opisano problemy w programie Visual Studio.

### <a name="script-documents-node-does-not-appear-in-solution-explorer"></a>W Eksploratorze rozwiązań nie ma węzła dokumentów skryptu

Niektóre z naszymi samouczkami kierujące do węzła "Dokumentach skryptów" w Eksploratorze rozwiązań podczas debugowania. Ten węzeł jest generowany przez debuger JavaScript i zostanie wyświetlony tylko podczas debugowania klientów w przeglądarkach w programie Internet Explorer; węzeł nie będą wyświetlane, jeśli są używane Chrome lub Firefox. Debuger JavaScript również nie będzie działać, jeśli jest uruchomiony inny debuger klienta, takich jak debugera Silverlight.

### <a name="signalr-does-not-work-on-visual-studio-2008-or-earlier"></a>SignalR nie działa w programie Visual Studio 2008 lub starszym

To zachowanie jest celowe. SignalR wymaga programu .NET Framework 4 lub nowszy; wymaga to opracowanych aplikacji SignalR w Visual Studio 2010 lub nowszej.

<a id="iis"></a>

## <a name="iis-issues"></a>Problemy z usług IIS

Ta sekcja zawiera problemy z internetowych usług informacyjnych.

### <a name="web-site-crashes-after-maphubs-call"></a>Witryna sieci Web ulegnie awarii po MapHubs wywołania

Ten problem został rozwiązany w najnowszej wersji biblioteki signalr. Sprawdź, że używasz najnowszej wersji biblioteki signalr, aktualizując instalacji przy użyciu narzędzia NuGet.

<a id="azure"></a>

## <a name="azure-issues"></a>Problemy z platformy Azure

Ta sekcja zawiera problemy w systemie Microsoft Azure.

### <a name="messages-are-not-received-through-the-azure-backplane-after-altering-topic-names"></a>Komunikaty nie są odbierane za pośrednictwem usługi Azure płyty montażowej po zmieniania nazwy tematów

Tematy używane przez usługi Azure płyty montażowej nie są przeznaczone do można konfigurować użytkownika.
