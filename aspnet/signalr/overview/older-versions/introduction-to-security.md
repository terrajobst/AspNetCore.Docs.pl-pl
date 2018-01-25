---
uid: signalr/overview/older-versions/introduction-to-security
title: "Wprowadzenie do zabezpieczeń SignalR (SignalR 1.x) | Dokumentacja firmy Microsoft"
author: pfletcher
description: "W tym artykule opisano problemy z zabezpieczeniami, które należy wziąć pod uwagę podczas opracowywania aplikacji SignalR."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 715a4059-d307-4631-abbb-c789c95d6eb4
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/introduction-to-security
msc.type: authoredcontent
ms.openlocfilehash: ebc83098b73902fa3f7a90a38dafc43b413e75fe
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="introduction-to-signalr-security-signalr-1x"></a>Wprowadzenie do zabezpieczeń SignalR (SignalR 1.x)
====================
przez [Patrick Fletcher](https://github.com/pfletcher), [FitzMacken niestandardowy](https://github.com/tfitzmac)

> W tym artykule opisano problemy z zabezpieczeniami, które należy wziąć pod uwagę podczas opracowywania aplikacji SignalR.


## <a name="overview"></a>Omówienie

Ten dokument zawiera następujące sekcje:

- [Pojęcia dotyczące zabezpieczeń SignalR](#concepts)

    - [Uwierzytelnianie i autoryzacja](#authentication)
    - [Token połączenia](#connectiontoken)
    - [Jeśli ponowne łączenie, ponowne przyłączanie grup](#rejoingroup)
- [Jak SignalR uniemożliwia sfałszowaniem żądań Cross-Site](#csrf)
- [Zalecenia dotyczące zabezpieczeń SignalR](#recommendations)

    - [Secure Socket warstwy protokołu (SSL)](#ssl)
    - [Nie należy używać grup jako mechanizm zabezpieczeń](#groupsecurity)
    - [Bezpiecznie obsługi danych wejściowych od klientów](#input)
    - [Uzgadnianie zmianę stanu użytkownika z aktywnego połączenia.](#reconcile)
    - [Automatycznie generowanych plików proxy JavaScript](#autogen)
    - [Wyjątki](#exceptions)

<a id="concepts"></a>

## <a name="signalr-security-concepts"></a>Pojęcia dotyczące zabezpieczeń SignalR

<a id="authentication"></a>

### <a name="authentication-and-authorization"></a>Uwierzytelnianie i autoryzacja

Celem jest zintegrowana z istniejącej struktury uwierzytelniania dla aplikacji SignalR. Nie zawiera żadnych funkcji do uwierzytelniania użytkowników. Zamiast tego można uwierzytelniać użytkowników, jako normalny w aplikacji, a następnie pracować z wynikami uwierzytelniania w kodzie SignalR. Na przykład mogą uwierzytelnianie użytkowników za pomocą uwierzytelniania formularzy ASP.NET, a następnie w Centrum, wymusić użytkowników, którzy lub role mają uprawnienia do wywołania metody. W Centrum należy przekazać informacje uwierzytelniania, takie jak nazwa użytkownika lub określa, czy użytkownik należy do roli, do klienta.

Biblioteka SignalR udostępnia [autoryzacji](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) atrybutu, aby określić, którzy użytkownicy mają dostęp do Centrum lub metody. Zastosuj atrybut autoryzacji do koncentratora lub konkretnej metody koncentratora. Bez atrybutu autoryzacji wszystkich metod publicznych koncentratora są dostępne dla klienta, który jest podłączony do koncentratora. Aby uzyskać więcej informacji na temat koncentratorów, zobacz [uwierzytelniania i autoryzacji dla koncentratorów SignalR](../security/hub-authorization.md).

`Authorize` Atrybut jest używany tylko z koncentratorami. Aby wymusić reguł autoryzacji, korzystając z `PersistentConnection` konieczne jest przesłonięcie `AuthorizeRequest` metody. Aby uzyskać więcej informacji na temat połączeń trwałych, zobacz [uwierzytelniania i autoryzacji dla połączenia trwałego SignalR](../security/persistent-connection-authorization.md).

<a id="connectiontoken"></a>

### <a name="connection-token"></a>Token połączenia

SignalR zmniejsza ryzyko wykonywania poleceń złośliwego weryfikując tożsamość nadawcy. Token połączenie, zawierający identyfikator połączenia i nazwę użytkownika dla uwierzytelnionych użytkowników są przekazywane między klientem i serwerem dla każdego żądania. Identyfikator połączenia jest unikatowy identyfikator, który jest losowo generowany przez serwer po utworzeniu nowego połączenia i jest utrwalona w czasie trwania połączenia. Nazwa użytkownika jest zapewniana przez mechanizm uwierzytelniania dla aplikacji sieci web. Token połączenia jest chroniony za pomocą szyfrowania i podpis cyfrowy.

![](introduction-to-security/_static/image2.png)

Dla każdego żądania serwer sprawdza poprawność zawartości tokenu, aby upewnić się, czy żądanie pochodzi z określonego użytkownika. Nazwa użytkownika musi odpowiadać identyfikator połączenia. Weryfikując zarówno identyfikator połączenia, jak i nazwę użytkownika, SignalR zapobiega złośliwemu użytkownikowi łatwe personifikowanie innego użytkownika. Jeśli serwer nie może sprawdzić poprawności tokenu połączenia, żądanie kończy się niepowodzeniem.

![](introduction-to-security/_static/image4.png)

Ponieważ identyfikator połączenia, który jest częścią procesu weryfikacji, nie należy ujawnić identyfikator połączenia jednego użytkownika do innych użytkowników lub przechowywać wartość po stronie klienta, takich jak w pliku cookie.

<a id="rejoingroup"></a>

### <a name="rejoining-groups-when-reconnecting"></a>Jeśli ponowne łączenie, ponowne przyłączanie grup

Domyślnie aplikacji SignalR zostanie automatycznie ponownie przypisać użytkownika do odpowiednich grup przy ponownym łączeniu z tymczasowego przerw w działaniu, np. gdy połączenie jest porzucona i nawiązał ponownie, zanim upłynie limit czasu połączenia. Przy ponownym łączeniu, klient przekazuje token grupy, który zawiera identyfikator połączenia i przypisanych grup. Token grupy jest cyfrowo podpisane i zaszyfrowane. Klient przechowuje ten sam identyfikator połączenia po ponowne nawiązanie połączenia; w związku z tym przekazany z klienta ponownie połączony identyfikator połączenia musi być zgodna poprzedniej identyfikator połączenia używany przez klienta. Przekazanie żądania dołączenia nieautoryzowanym grupom przy ponownym łączeniu do weryfikacji uniemożliwia złośliwy użytkownik.

Jest jednak należy pamiętać, token grupy nie wygaśnie. Jeśli użytkownik należał do grupy w przeszłości, ale został wykluczeni z tej grupy, użytkownik może być mógł naśladować token grupy, która zawiera zabronione grupę. Trzeba bezpiecznie zarządzać użytkowników, którzy należą do grupy, które należy do przechowywania danych na serwerze, takich jak w bazie danych. Następnie należy dodać logikę do aplikacji, która sprawdza na serwerze, czy użytkownik należy do grupy. Na przykład kontrolować członkostwo w grupie, zobacz [Praca z grupami](../guide-to-the-api/working-with-groups.md).

Automatyczne ponowne przyłączanie grup tylko wtedy, gdy po przerwaniu tymczasowego ponownie nawiązał połączenie. Jeśli użytkownik rozłączy się przez przejście na inną aplikację lub ponownym uruchomieniem aplikacji, aplikacja musi obsługiwać jak dodać tego użytkownika do grupy poprawne. Aby uzyskać więcej informacji, zobacz [Praca z grupami](../guide-to-the-api/working-with-groups.md).

<a id="csrf"></a>

## <a name="how-signalr-prevents-cross-site-request-forgery"></a>Jak SignalR uniemożliwia sfałszowaniem żądań Cross-Site

Sfałszowaniem żądania między witrynami (CSRF) jest atak, gdzie niebezpiecznej witryny wysyła żądanie do narażone lokacji, w którym użytkownik jest aktualnie zalogowany. SignalR zapobiega CSRF, wykonując bardzo prawdopodobne dla złośliwa witryna można utworzyć prawidłowe żądania aplikacji SignalR.

### <a name="description-of-csrf-attack"></a>Opis elementu ataku CSRF

Oto przykład ataku CSRF:

1. Uwierzytelnianie przy użyciu formularzy logowania użytkownika do www.example.com.
2. Serwer uwierzytelnia użytkownika. Odpowiedź z serwera zawiera pliku cookie uwierzytelniania.
3. Bez rejestrowania, użytkownik odwiedzi złośliwą witrynę sieci web. Ta witryna złośliwego zawiera poniższy formularz HTML: 

    [!code-html[Main](introduction-to-security/samples/sample1.html)]

 Należy zauważyć, że akcja formularza zapisuje do lokacji narażony, nie niebezpiecznej witryny. Jest to część CSRF "cross-site".
4. Użytkownik klika przycisk Prześlij. Przeglądarka zawiera pliku cookie uwierzytelniania z żądaniem.
5. Żądanie działa na serwerze example.com z kontekstem uwierzytelniania użytkownika i mogą wykonywać wszystkie uwierzytelniony użytkownik może wykonywać.

Mimo że w tym przykładzie wymaga od użytkownika kliknij przycisk formularz, strony złośliwych może równie łatwo uruchomić skrypt, który wysyła żądanie AJAX do aplikacji SignalR. Ponadto przy użyciu protokołu SSL nie atak CSRF, ponieważ złośliwa witryna można wysyłać żądania "https://".

Zazwyczaj CSRF ataki są możliwe przed witryn sieci web, które używają plików cookie do uwierzytelniania, ponieważ przeglądarki wysyłać wszystkich odpowiednich plików cookie do docelowej witryny sieci web. Jednak ataków CSRF nie są ograniczone do wykorzystania plików cookie. Na przykład uwierzytelnianie podstawowe i szyfrowane również są zagrożone. Po zalogowaniu się użytkownika za pomocą uwierzytelnianie podstawowe lub szyfrowane, przeglądarka automatycznie wysyła poświadczenia zakończenia sesji.

### <a name="csrf-mitigations-taken-by-signalr"></a>Środki zaradcze CSRF wykonywaną przez SignalR

SignalR wykonuje następujące czynności, aby uniemożliwić tworzenie prawidłowych żądań kierowanych do aplikacji SignalR niebezpiecznej witryny. Czynności te są pobierane domyślnie i nie wymagają żadnych czynności w kodzie.

- **Wyłącz żądania obejmujące różne domeny**  
 Domyślnie między domeny żądania są wyłączone w aplikacji SignalR, aby uniemożliwić użytkownikom wywołaniem punktu końcowego SignalR z domeny zewnętrznej. Każde żądanie pochodzące z domeny zewnętrznej automatycznie jest uznawane za nieprawidłowe i jest zablokowana. Zaleca się zachowanie domyślne zachowanie; w przeciwnym razie złośliwa witryna można wymuszać użytkowników wysyłać polecenia do witryny. Jeśli musisz użyć żądania obejmujące różne domeny, zobacz [jak nawiązać połączenie między domenami](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain) .
- **Przekaż token połączenia w ciągu zapytania, nie pliku cookie.**  
 SignalR przekazuje token połączenia jako wartość ciągu kwerendy zamiast jako plik cookie. Token połączenia nie są przechowywane jako plik cookie, token połączenia nie jest przypadkowo przekazany do przeglądarki po napotkaniu złośliwego kodu. Ponadto token połączenia nie jest trwały poza bieżącego połączenia. W związku z tym złośliwy użytkownik nie może wysłać żądanie poświadczeniami uwierzytelniania przez innego użytkownika.
- **Sprawdź token połączenia**  
 Zgodnie z opisem w [token połączenia](#connectiontoken) sekcji serwer zna identyfikator połączenia, z którym jest skojarzony z każdym uwierzytelnionego użytkownika. Serwer nie przetwarza wszystkie żądania z identyfikator połączenia, który nie jest zgodna z nazwą użytkownika. Jest mało prawdopodobne, złośliwy użytkownik może odgadnąć prawidłowe żądania, ponieważ złośliwy użytkownik musi znać nazwy użytkownika i bieżący identyfikator generowany losowo połączenia. Ten identyfikator połączenia staje się nieprawidłowy, natychmiast po zakończeniu połączenia. Użytkownicy anonimowi nie powinny mieć dostępu do informacji poufnych.

<a id="recommendations"></a>

## <a name="signalr-security-recommendations"></a>Zalecenia dotyczące zabezpieczeń SignalR

<a id="ssl"></a>

### <a name="secure-socket-layers-ssl-protocol"></a>Secure Socket warstwy protokołu (SSL)

Protokół SSL używa szyfrowania do zabezpieczenia transportu danych między klientem i serwerem. Jeśli aplikacja SignalR przesyła poufnych informacji między klientem a serwerem, użyj protokołu SSL dla transportu. Aby uzyskać więcej informacji o konfigurowaniu protokołu SSL, zobacz [sposobu konfigurowania protokołu SSL w usługach IIS 7](https://www.iis.net/learn/manage/configuring-security/how-to-set-up-ssl-on-iis).

<a id="groupsecurity"></a>

### <a name="do-not-use-groups-as-a-security-mechanism"></a>Nie należy używać grup jako mechanizm zabezpieczeń

Grupy to wygodny sposób zbierania powiązanych użytkowników, ale nie są one bezpieczne mechanizm ograniczania dostępu do poufnych informacji. Dotyczy to zwłaszcza użytkowników mogą automatycznie ponownie dołączyć grupy podczas ponownego połączenia. Zamiast tego należy rozważyć dodanie użytkownicy o odpowiednich uprawnieniach do roli i ograniczenie dostępu do metody koncentratora, aby tylko członkowie tej roli. Przykład ograniczanie dostępu na podstawie roli, zobacz [uwierzytelniania i autoryzacji dla koncentratorów SignalR](../security/hub-authorization.md). Na przykład kontroli dostępu użytkownika do grupy przy ponownym łączeniu zobacz [Praca z grupami](../guide-to-the-api/working-with-groups.md).

<a id="input"></a>

### <a name="safely-handling-input-from-clients"></a>Bezpiecznie obsługi danych wejściowych od klientów

Aby upewnić się, że złośliwy użytkownik nie będzie przesyłał skryptu do innych użytkowników musi być zakodowany wszystkich danych wejściowych od klientów, przewidziany do emisji do innych klientów. Najlepiej jest kodowanie wiadomości na odbieranie klientów, a nie serwera, ponieważ aplikacja SignalR może mieć wielu różnych klientów. W związku z tym kodowanie HTML działa dla klienta sieci web, ale nie dla innych klientów. Na przykład metodę klienta sieci web do wyświetlania komunikatu rozmów czy bezpiecznie obsłużyć nazwy użytkownika i wiadomości przez wywołanie metody `html()` funkcji.

[!code-html[Main](introduction-to-security/samples/sample2.html?highlight=3-4)]

<a id="reconcile"></a>

### <a name="reconciling-a-change-in-user-status-with-an-active-connection"></a>Uzgadnianie zmianę stanu użytkownika z aktywnego połączenia.

Zmiana stanu uwierzytelniania użytkownika podczas aktywnego połączenia istnieje, zostanie wyświetlony błąd informujący, "tożsamości użytkownika nie można zmienić podczas aktywnego połączenia SignalR". W takim przypadku aplikacji należy ponownie połączyć się z serwerem do upewnij się, że połączenia identyfikator i nazwa użytkownika są skoordynowany sposób. Na przykład jeśli aplikacja zezwala użytkownikowi na Wyloguj się, gdy istnieje aktywnego połączenia, nazwa użytkownika dla połączenia nie będzie już zgodny nazwę, która jest przekazywany w dla następnego żądania. Będzie chcesz przerwać połączenie przed wylogowaniem użytkownika, a następnie uruchom go ponownie.

Jest jednak należy pamiętać, że większość aplikacji nie trzeba ręcznie zatrzymaj i uruchom połączenie. Jeśli aplikacja przekierowuje użytkowników na osobnej stronie po zalogowaniu, takie jak domyślne zachowanie w aplikacji formularzy sieci Web lub aplikacji MVC lub Odświeża bieżącą stronę po wylogowaniu, jest automatycznie rozłączany aktywnego połączenia, a nie wymaga dodatkowych działań.

Poniższy przykład pokazuje, jak do zatrzymywania i uruchamiania połączenia, gdy stan użytkownika został zmieniony.

[!code-html[Main](introduction-to-security/samples/sample3.html)]

Lub może zmienić stan uwierzytelniania użytkownika, wygaśniecie korzysta z uwierzytelniania formularzy, jeśli nie ma żadnych działań, aby zapewnić prawidłowy plik cookie uwierzytelniania. W takim przypadku użytkownik będzie logować i nazwa użytkownika nie będzie już zgodny z nazwą użytkownika w token połączenia. Aby rozwiązać ten problem, należy dodać niektóre skrypt, który okresowo żąda zasobu na serwerze sieci web, aby zapewnić prawidłowy plik cookie uwierzytelniania. Poniższy przykład pokazuje, jak żądanie zasobu co 30 minut.

[!code-javascript[Main](introduction-to-security/samples/sample4.js)]

<a id="autogen"></a>

### <a name="automatically-generated-javascript-proxy-files"></a>Automatycznie generowanych plików proxy JavaScript

Jeśli nie chcesz wszystkich koncentratorów i metod uwzględnić w pliku proxy JavaScript dla każdego użytkownika, można wyłączyć automatyczne generowanie pliku. Możesz wybrać tę opcję, jeśli masz wiele koncentratorów i metod, ale nie chcesz, aby każdy użytkownik pod uwagę wszystkie metody. Wyłącz automatyczne generowanie przez ustawienie **EnableJavaScriptProxies** do **false**.

[!code-csharp[Main](introduction-to-security/samples/sample5.cs)]

Aby uzyskać więcej informacji na temat plików proxy JavaScript, zobacz [wygenerowany serwer proxy i jakie operacje dla Ciebie](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy). <a id="exceptions"></a>

### <a name="exceptions"></a>Wyjątki

Należy unikać przekazywanie obiekty wyjątków do klientów, ponieważ obiektów może spowodować ujawnienie poufnych informacji do klientów. Zamiast tego należy wywołać metodę na kliencie, który wyświetla odpowiedni komunikat.

[!code-csharp[Main](introduction-to-security/samples/sample6.cs)]
