---
uid: signalr/overview/older-versions/introduction-to-security
title: Wprowadzenie do zabezpieczeń SignalR (SignalR 1.x) | Dokumentacja firmy Microsoft
author: pfletcher
description: W tym artykule opisano problemy z zabezpieczeniami, które należy wziąć pod uwagę podczas opracowywania aplikacji SignalR.
ms.author: riande
ms.date: 10/17/2013
ms.assetid: 715a4059-d307-4631-abbb-c789c95d6eb4
msc.legacyurl: /signalr/overview/older-versions/introduction-to-security
msc.type: authoredcontent
ms.openlocfilehash: 662ce8d5581d16709ea79de39e078bfa36b02b5c
ms.sourcegitcommit: 74e3be25ea37b5fc8b4b433b0b872547b4b99186
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53287653"
---
<a name="introduction-to-signalr-security-signalr-1x"></a>Wprowadzenie do zabezpieczeń usługi SignalR (SignalR 1.x)
====================
przez [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> W tym artykule opisano problemy z zabezpieczeniami, które należy wziąć pod uwagę podczas opracowywania aplikacji SignalR.


## <a name="overview"></a>Omówienie

Ten dokument zawiera następujące sekcje:

- [Pojęcia dotyczące zabezpieczeń SignalR](#concepts)

    - [Uwierzytelnianie i autoryzacja](#authentication)
    - [Token połączenia](#connectiontoken)
    - [Ponowne przyłączanie grup, gdy ponowne łączenie](#rejoingroup)
- [Jak SignalR zapobiega Cross-Site Request Forgery](#csrf)
- [Zalecenia dotyczące zabezpieczeń SignalR](#recommendations)

    - [Bezpieczny protokół gniazda warstwy SSL)](#ssl)
    - [Nie należy używać grup jako mechanizmu zabezpieczeń](#groupsecurity)
    - [Bezpiecznie Obsługa danych wejściowych od klientów](#input)
    - [Uzgadnianie zmian stanu użytkownika z aktywnym połączeniem](#reconcile)
    - [Automatycznie generowanych plików serwera proxy JavaScript](#autogen)
    - [Wyjątki](#exceptions)

<a id="concepts"></a>

## <a name="signalr-security-concepts"></a>Pojęcia dotyczące zabezpieczeń SignalR

<a id="authentication"></a>

### <a name="authentication-and-authorization"></a>Uwierzytelnianie i autoryzacja

SignalR została zaprojektowana do zintegrowane z istniejącą strukturę uwierzytelniania dla aplikacji. Nie zapewnia żadnych funkcji do uwierzytelniania użytkowników. Zamiast tego można uwierzytelniać użytkowników, jak normalnie w aplikacji, a następnie pracować z wynikami uwierzytelniania w kodzie SignalR. Na przykład mogą uwierzytelniać użytkowników za pomocą uwierzytelniania formularzy programu ASP.NET, a następnie w Centrum, wymuszanie użytkowników, którzy lub role są uprawnione do wywołania metody. W Centrum można również przekazać informacje o uwierzytelnianiu, takie jak nazwa użytkownika lub tego, czy użytkownik należy do roli, do klienta.

Biblioteka SignalR udostępnia [Autoryzuj](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) atrybutu, aby określić, którzy użytkownicy mają dostęp do koncentratora lub metody. Zastosuj atrybut autoryzacji do koncentratora lub konkretnej metody koncentratora. Bez atrybutu autoryzacji wszystkich metod publicznych w Centrum są dostępne dla klienta, który jest podłączony do koncentratora. Aby uzyskać więcej informacji na temat koncentratorów, zobacz [uwierzytelnianie i autoryzacja dla centrów SignalR](../security/hub-authorization.md).

`Authorize` Atrybut jest używany tylko z koncentratorami. Aby wymusić reguł autoryzacji, korzystając z `PersistentConnection` konieczne jest przesłonięcie `AuthorizeRequest` metody. Aby uzyskać więcej informacji na temat połączeń trwałych zobacz [uwierzytelnianie i autoryzacja połączeń trwałych SignalR](../security/persistent-connection-authorization.md).

<a id="connectiontoken"></a>

### <a name="connection-token"></a>Token połączenia

SignalR zmniejsza ryzyko wykonywania poleceń złośliwego przez weryfikację tożsamości nadawcy. Token połączenie, zawierający identyfikator połączenia i nazwę użytkownika dla uwierzytelnionych użytkowników, są przekazywane między klientem i serwerem dla każdego żądania. Identyfikator połączenia, który jest unikatowy identyfikator, który jest generowany losowo przez serwer, gdy nowe połączenie, zostanie utworzona i jest utrwalony na czas trwania połączenia. Nazwa użytkownika jest zapewniana przez mechanizm uwierzytelniania dla aplikacji sieci web. Token połączenia jest chroniony za pomocą szyfrowania i podpisu cyfrowego.

![](introduction-to-security/_static/image2.png)

Dla każdego żądania serwer sprawdza poprawność zawartości tokenu, aby upewnić się, że żądanie pochodzi z określonego użytkownika. Nazwa użytkownika musi odpowiadać identyfikator połączenia. Weryfikując zarówno identyfikator połączenia, jak i nazwę użytkownika, SignalR zapobiega złośliwemu użytkownikowi łatwo personifikacja innego użytkownika. Jeśli serwer nie może sprawdzić poprawności tokenu połączenia, żądanie kończy się niepowodzeniem.

![](introduction-to-security/_static/image4.png)

Ponieważ identyfikator połączenia, który jest częścią procesu weryfikacji, nie należy ujawnić identyfikatora połączenia jednego użytkownika do innych użytkowników lub przechowywania wartości na kliencie, takie jak w pliku cookie.

<a id="rejoingroup"></a>

### <a name="rejoining-groups-when-reconnecting"></a>Ponowne przyłączanie grup, gdy ponowne łączenie

Domyślnie aplikacji SignalR zostanie automatycznie ponownie przypisać użytkownika do odpowiednich grup przy ponownym łączeniu zakłóceniach tymczasowej, np. podczas połączenia jest usunięty i ponownie nawiązane przed upływem limitu czasu połączenia. Przy ponownym łączeniu, klient przekazuje token grupy, który zawiera identyfikator połączenia i przypisanych grup. Token grup jest cyfrowo podpisane i szyfrowane. Klient zachowuje ten sam identyfikator połączenia po ponowne nawiązanie połączenia; w związku z tym przekazany z klienta ponownie połączony identyfikator połączenia musi być zgodna poprzedniego identyfikator połączenia używany przez klienta. Ta weryfikacja uniemożliwia przekazanie żądania dołączenia do grupy nieautoryzowanych przy ponownym łączeniu złośliwy użytkownik.

Jest jednak należy pamiętać, grupy token nie wygaśnie. Jeśli użytkownik należał do grupy w przeszłości, ale został zablokowany w tej grupie, ten użytkownik może mieć do naśladowania token grupy, która zawiera zabronione grupę. Jeśli musisz bezpiecznie zarządzać użytkowników, którzy należą do grupy, które należy do przechowywania danych na serwerze, takie jak w bazie danych. Następnie należy dodać logikę do swojej aplikacji, która sprawdza na serwerze, czy użytkownik należy do grupy. Na przykład sprawdzania, czy członkostwo w grupie, zobacz [Praca z grupami](../guide-to-the-api/working-with-groups.md).

Automatyczne ponowne przyłączanie grup tylko wtedy, gdy połączenie jest zakończone po przerwaniu tymczasowe. Jeśli użytkownik rozłączy się przez przechodzenia poza aplikacji lub ponownego uruchomienia aplikacji, aplikacja musi obsługiwać jak dodać tego użytkownika do grupy poprawne. Aby uzyskać więcej informacji, zobacz [Praca z grupami](../guide-to-the-api/working-with-groups.md).

<a id="csrf"></a>

## <a name="how-signalr-prevents-cross-site-request-forgery"></a>Jak SignalR zapobiega Cross-Site Request Forgery

Cross-Site fałszowaniu żądań Międzywitrynowych to atak, w którym złośliwych witryn wysyła żądanie do lokacji narażony, gdzie użytkownik jest aktualnie zalogowany. SignalR zapobiega CSRF, dzięki czemu bardzo mało prawdopodobne do złośliwych witryn utworzyć prawidłowy żądanie aplikacji SignalR.

### <a name="description-of-csrf-attack"></a>Opis CSRF ataku

Oto przykład ataku typu CSRF:

1. Użytkownik loguje się do `www.example.com`, za pomocą uwierzytelniania formularzy.
2. Serwer uwierzytelnia użytkownika. Odpowiedź z serwera zawiera pliku cookie uwierzytelniania.
3. Bez wylogowanie, użytkownik odwiedzi złośliwą witrynę sieci web. Ta witryna złośliwego zawiera poniższy formularz HTML: 

    [!code-html[Main](introduction-to-security/samples/sample1.html)]

   Należy zauważyć, że akcji formularza publikuje lokacji narażony, nie do złośliwych witryn. Jest to część "cross-site" CSRF.
4. Użytkownik klika przycisk Prześlij. Przeglądarka zawiera pliku cookie uwierzytelniania z żądaniem.
5. Żądanie jest uruchomiony na serwerze example.com z kontekstu uwierzytelniania użytkownika i mogą wykonywać wszystkie uwierzytelniony użytkownik może wykonywać.

Mimo że w tym przykładzie wymaga od użytkownika, kliknij przycisk formularz, złośliwy strony można tak samo jak w prosty sposób uruchamiaj skryptu, który wysyła żądanie AJAX do aplikacji SignalR. Ponadto przy użyciu protokołu SSL nie atak CSRF, ponieważ złośliwa witryna może wysłać żądanie "https://".

Zazwyczaj ataków CSRF możliwe są względem witryny sieci web, które używają plików cookie uwierzytelniania, ponieważ przeglądarek wysyłać wszystkie odpowiednie pliki cookie do docelowej witryny sieci web. Jednak ataków CSRF nie są ograniczone do wykorzystania plików cookie. Na przykład uwierzytelnianie podstawowe i szyfrowane są również narażone. Po użytkownik zaloguje się za pomocą uwierzytelniania podstawowe lub szyfrowane, przeglądarka automatycznie wysyła poświadczenia zakończenia sesji.

### <a name="csrf-mitigations-taken-by-signalr"></a>Środki zaradcze CSRF podjęte przez SignalR

SignalR wykonuje następujące czynności, aby uniemożliwić złośliwym tworzenie prawidłowych żądań do aplikacji SignalR. Te kroki są podejmowane domyślnie i nie wymagają żadnych działań w kodzie.

- **Wyłącz żądania obejmujące różne domeny**  
 Domyślnie między domeny żądania są wyłączone w aplikacji SignalR, aby uniemożliwić użytkownikom wywoływanie punktu końcowego SignalR z domeny zewnętrznej. Każde żądanie, które pochodzą z domeny zewnętrznej zostaje automatycznie uznana za nieprawidłową i jest zablokowany. Zaleca się utrzymywanie to domyślne zachowanie; w przeciwnym razie złośliwych witryn można nakłonienia użytkowników do wysyłania poleceń do swojej witryny. Jeśli musisz użyć żądania obejmujące różne domeny, zobacz [jak nawiązać połączenie między domenami](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain) .
- **Przekaż token połączenia w ciągu zapytania, nie pliku cookie.**  
 SignalR przekazuje token połączenia jako wartość ciągu zapytania, a nie jako plik cookie. Token połączenia nie są przechowywane jako plik cookie, token połączenia nie jest nieodwracalnie przekazany do przeglądarki po napotkaniu złośliwego kodu. Ponadto token połączenia nie jest trwały poza bieżącym połączeniu. W związku z tym złośliwy użytkownik nie może zgłosić wniosek w ramach poświadczeń uwierzytelniania innego użytkownika.
- **Weryfikuj token połączenia**  
 Zgodnie z opisem w [token połączenia](#connectiontoken) sekcji serwera zna identyfikator połączenia, z którym jest skojarzony z każdego uwierzytelnionego użytkownika. Serwer nie przetwarza każde żądanie pochodzące od identyfikator połączenia, który nie jest zgodna z nazwą użytkownika. Jest mało prawdopodobne, złośliwy użytkownik może odgadnięcia prawidłowemu żądaniu, ponieważ złośliwy użytkownik musi wiedzieć, nazwę użytkownika i bieżący identyfikator generowany losowo połączenia. Ten identyfikator połączenia staje się nieprawidłowy, tak szybko, jak połączenie zostanie zakończona. Użytkownicy anonimowi nie powinna mieć dostępu do żadnych poufnych informacji.

<a id="recommendations"></a>

## <a name="signalr-security-recommendations"></a>Zalecenia dotyczące zabezpieczeń SignalR

<a id="ssl"></a>

### <a name="secure-socket-layers-ssl-protocol"></a>Bezpieczny protokół gniazda warstwy SSL)

Protokół SSL używa szyfrowania do zabezpieczenia transportu danych między klientem i serwerem. Jeśli aplikacja SignalR przesyła poufnych informacji między klientem i serwerem, należy używać protokołu SSL dla transportu. Aby uzyskać więcej informacji na temat konfigurowania protokołu SSL, zobacz [sposób konfigurowania protokołu SSL w usługach IIS 7](https://www.iis.net/learn/manage/configuring-security/how-to-set-up-ssl-on-iis).

<a id="groupsecurity"></a>

### <a name="do-not-use-groups-as-a-security-mechanism"></a>Nie należy używać grup jako mechanizmu zabezpieczeń

Grupy to wygodny sposób zbierania powiązanych użytkowników, ale nie są one bezpieczne mechanizm ograniczania dostępu do poufnych informacji. Jest to szczególnie istotne w przypadku, gdy użytkownicy automatycznie ponownie dołączyć do grup podczas ponownego połączenia. Zamiast tego należy rozważyć dodanie uprzywilejowanych użytkowników do roli i ograniczenie dostępu do metody koncentratora do tylko członków tej roli. Aby uzyskać przykład ograniczanie dostępu na podstawie roli, zobacz [uwierzytelnianie i autoryzacja dla centrów SignalR](../security/hub-authorization.md). Na przykład sprawdzania dostępu użytkowników do grup przy ponownym łączeniu zobacz [Praca z grupami](../guide-to-the-api/working-with-groups.md).

<a id="input"></a>

### <a name="safely-handling-input-from-clients"></a>Bezpiecznie Obsługa danych wejściowych od klientów

Wszystkie dane wejściowe od klientów, które jest przeznaczone do emisji do innych klientów muszą być kodowane, aby upewnić się, że złośliwy użytkownik nie będzie przesyłał skryptu do innych użytkowników. Najlepiej jest kodowanie wiadomości na odbieranie klientów, a nie na serwerze, ponieważ aplikacja SignalR może mieć wiele różnych typów klientów. W związku z tym kodowanie HTML działa dla klienta sieci web, ale nie dla innych typów klientów. Na przykład metoda klienta sieci web do wyświetlenia wiadomości rozmowy będzie bezpiecznie obsługiwać nazwy użytkownika i komunikat przez wywołanie metody `html()` funkcji.

[!code-html[Main](introduction-to-security/samples/sample2.html?highlight=3-4)]

<a id="reconcile"></a>

### <a name="reconciling-a-change-in-user-status-with-an-active-connection"></a>Uzgadnianie zmian stanu użytkownika z aktywnym połączeniem

Jeśli stan uwierzytelniania użytkownika zmieni się, gdy istnieje aktywne połączenie, użytkownik otrzyma komunikat o błędzie informujący, że "tożsamość użytkownika nie można zmienić podczas aktywnego połączenia SignalR". W takim przypadku aplikacja powinna ponownym połączeniu się z serwera, aby upewnić się, że połączenia identyfikator i nazwa użytkownika są koordynowane. Na przykład jeśli aplikacja umożliwia użytkownikowi Wyloguj się, gdy istnieje aktywne połączenie, nazwa użytkownika dla połączenia nie będzie już zgodny nazwę, która jest przekazywana do następnego żądania. Będzie chcesz przerwać połączenie przed wylogowaniem użytkownika, a następnie uruchom go ponownie.

Jest jednak należy pamiętać, że większość aplikacji nie trzeba ręcznie uruchamiać i zatrzymywać połączenia. Jeśli aplikacja przekierowuje użytkowników do oddzielnej strony po zalogowaniu, takie jak domyślne zachowanie w aplikacji formularzy sieci Web lub aplikacji MVC lub Odświeża bieżącą stronę po wylogowaniu, aktywne połączenie jest automatycznie rozłączany i nie wymaga dodatkowych działań.

Poniższy przykład pokazuje, jak zatrzymać i uruchomić połączenia, gdy stan użytkownika został zmieniony.

[!code-html[Main](introduction-to-security/samples/sample3.html)]

Lub stanu uwierzytelniania użytkownika mogą ulec zmianie, jeśli Twoja witryna wymaga wygaśniecie za pomocą uwierzytelniania formularzy i nie ma żadnych działań, aby zachować pliku cookie uwierzytelniania jest prawidłowy. W takiej sytuacji użytkownik zostanie wylogowany, a nazwa użytkownika nie będzie już zgodny nazwę użytkownika w tokenie połączenia. Aby rozwiązać ten problem, dodając kilka skrypt, który okresowo żąda zasobu na serwerze sieci web, aby zachować pliku cookie uwierzytelniania jest prawidłowy. Poniższy przykład przedstawia sposób zażądać zasobu co 30 minut.

[!code-javascript[Main](introduction-to-security/samples/sample4.js)]

<a id="autogen"></a>

### <a name="automatically-generated-javascript-proxy-files"></a>Automatycznie generowanych plików serwera proxy JavaScript

Jeśli nie mają zawierać wszystkich koncentratorów i metod w pliku serwera proxy JavaScript dla każdego użytkownika, możesz wyłączyć automatyczne generowanie pliku. Możesz wybrać tę opcję, jeśli masz wiele koncentratorów i metod, ale nie ma pod uwagę wszystkie metody każdego użytkownika. Wyłącz automatyczne generowanie, ustawiając **EnableJavaScriptProxies** do **false**.

[!code-csharp[Main](introduction-to-security/samples/sample5.cs)]

Aby uzyskać więcej informacji na temat plików serwera proxy JavaScript zobacz [wygenerowany serwer proxy i przeznaczenie dla Ciebie](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy). <a id="exceptions"></a>

### <a name="exceptions"></a>Wyjątki

Należy unikać przekazywania obiektów wyjątków do klientów, ponieważ obiekty może spowodować ujawnienie poufnych informacji do klientów. Zamiast tego należy wywołać metodę na komputerze klienckim, który wyświetla komunikat o błędzie istotne.

[!code-csharp[Main](introduction-to-security/samples/sample6.cs)]
