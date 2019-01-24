---
uid: signalr/overview/older-versions/signalr-1x-hubs-api-guide-javascript-client
title: Podręcznik interfejsu API centrów SignalR 1.x — klient JavaScript | Dokumentacja firmy Microsoft
author: bradygaster
description: Ten dokument zawiera wprowadzenie do korzystania z interfejsu API centrów dla elementu SignalR w wersji 1.1 w klientów języka JavaScript, takie jak przeglądarki i Windows Store (WinJS) applic...
ms.author: bradyg
ms.date: 04/17/2013
ms.assetid: dcd4593b-1118-418a-af71-d12ff33fb36d
msc.legacyurl: /signalr/overview/older-versions/signalr-1x-hubs-api-guide-javascript-client
msc.type: authoredcontent
ms.openlocfilehash: eb40648ca06adcceaa613ba86abfcf7459369c7e
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2019
ms.locfileid: "54836756"
---
<a name="signalr-1x-hubs-api-guide---javascript-client"></a>Podręcznik interfejsu API centrów usługi SignalR 1.x — klient JavaScript
====================
przez [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Ten dokument zawiera wprowadzenie do korzystania z interfejsu API centrów dla elementu SignalR w wersji 1.1 w klientów języka JavaScript, takie jak przeglądarki i aplikacje Windows Store (WinJS).
> 
> Interfejsu API centrów SignalR umożliwia zdalne wywołania procedur (RPC) z serwera do połączonych klientów i od klientów z serwerem. W kodzie serwera należy zdefiniować metody, które mogą być wywoływane przez klientów, a wywołanie metody, które są uruchamiane na komputerze klienckim. W kodzie klienta definiowania metod, które mogą być wywoływane z serwera, a wywołanie metody, które są uruchamiane na serwerze. SignalR zajmuje się wszystkie nadmiar klient serwer dla Ciebie.
> 
> SignalR oferuje również interfejs API niższego poziomu o nazwie połączeń trwałych. Wprowadzenie do SignalR, centra i połączenia trwałego lub samouczek, w którym przedstawiono sposób tworzenia kompletnej aplikacji SignalR, patrz [SignalR — wprowadzenie](../getting-started/index.md).


## <a name="overview"></a>Omówienie

Ten dokument zawiera następujące sekcje:

- [Wygenerowany serwer proxy i przeznaczenie dla Ciebie](#genproxy)

    - [Kiedy należy używać wygenerowany serwer proxy](#cantusegenproxy)
- [Instalacja klienta](#clientsetup)

    - [Jak odwoływać się dynamicznie generowanym serwera proxy](#dynamicproxy)
    - [Jak utworzyć plik fizyczny dla elementu SignalR wygenerowany serwer proxy](#manualproxy)
- [Jak nawiązać połączenie](#establishconnection)

    - [$. connection.hub jest taki sam obiekt, tworzy tego $.hubConnection()](#connequivalence)
    - [Wykonanie asynchroniczne metody start](#asyncstart)
- [Jak nawiązać połączenie między domenami](#crossdomain)
- [Jak skonfigurować połączenie](#configureconnection)

    - [Jak określić parametry ciągu zapytania](#querystring)
    - [Jak określić metodę transportu](#transport)
- [Jak uzyskać serwera proxy dla klasy koncentratora](#getproxy)
- [Sposób definiowania metody na kliencie, który można wywołać serwera](#callclient)
- [Jak wywołać metody serwera z poziomu klienta](#callserver)
- [Jak obsługiwać zdarzenia okresu istnienia połączenia](#connectionlifetime)
- [Sposób obsługi błędów](#handleerrors)
- [Włączanie rejestrowania po stronie klienta](#logging)

Dokumentacja na temat programu server lub klientów programu .NET, zobacz następujące zasoby:

- [Podręcznik interfejsu API centrów SignalR — serwer](../guide-to-the-api/hubs-api-guide-server.md)
- [Podręcznik interfejsu API centrów SignalR — klient modelu .NET](../guide-to-the-api/hubs-api-guide-net-client.md)

Łącza do tematów, dokumentacja interfejsu API są .NET 4.5 wersję interfejsu API. Jeśli używasz programu .NET 4, zobacz [tematy interfejsu API w wersji .NET 4](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).

<a id="genproxy"></a>

## <a name="the-generated-proxy-and-what-it-does-for-you"></a>Wygenerowany serwer proxy i przeznaczenie dla Ciebie

Można programować klienta JavaScript do komunikowania się z usługą SignalR, z lub bez serwera proxy, który generuje SignalR. Serwer proxy jest dla Ciebie to uprościć jego składnię kodu używana do łączenia metod zapisywania, które wymaga serwera, i wywoływać metody na serwerze.

Podczas pisania kodu w celu wywołania metody serwera wygenerowany serwer proxy umożliwia użycie składni, która wygląda tak, jakby były wykonywania funkcji lokalnej: możesz napisać `serverMethod(arg1, arg2)` zamiast `invoke('serverMethod', arg1, arg2)`. Składnia wygenerowany serwer proxy umożliwia również natychmiastowe i zrozumiałym błąd po stronie klienta, jeśli wpisujesz nazwę metody serwera. A jeśli ręcznie utworzyć plik, który definiuje serwery proxy, możesz również uzyskać obsługę technologii IntelliSense do pisania kodu, który wywołuje metody serwera.

Na przykład załóżmy, że masz następujące klasy koncentratora na serwerze:

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample1.cs?highlight=1,3,5)]

W poniższych przykładach kodu pokazano, co JavaScript kod wygląda do wywoływania `NewContosoChatMessage` metody na serwerze i odbieranie wywołań `addContosoChatMessageToPage` metody z serwera.

**Za pomocą wygenerowanego serwera proxy**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample2.js?highlight=1-2,8)]

**Bez wygenerowany serwer proxy**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample3.js?highlight=2-3,9)]

<a id="cantusegenproxy"></a>

### <a name="when-to-use-the-generated-proxy"></a>Kiedy należy używać wygenerowany serwer proxy

Jeśli chcesz zarejestrować wiele procedur obsługi zdarzeń dla metod klienta, który wywołuje serwer, nie możesz użyć wygenerowanego serwera proxy. W przeciwnym razie można używać wygenerowany serwer proxy lub nie na podstawie kodowania preferencji użytkownika. Jeśli nie chcesz go użyć, nie musisz odwoływać się do adresu URL "koncentratory/signalr" w `script` elementu w kodzie klienta.

<a id="clientsetup"></a>

## <a name="client-setup"></a>Instalacja klienta

Klient JavaScript wymaga odwołania do technologii jQuery i plik JavaScript core SignalR. Wersja jQuery musi być 1.6.4 lub nowsze wersje główne, takich jak 1.7.2, 1.8.2 lub 1.9.1. Jeśli zdecydujesz się używać wygenerowany serwer proxy, należy również odwołanie do serwera proxy SignalR wygenerowany plik JavaScript. Poniższy przykład pokazuje, jak odwołania może wyglądać na stronie HTML, który używa wygenerowany serwer proxy.

[!code-html[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample4.html)]

Te odwołania muszą być zawarte w następującej kolejności: jQuery imię i nazwisko core SignalR po tym i serwery proxy SignalR.

<a id="dynamicproxy"></a>

### <a name="how-to-reference-the-dynamically-generated-proxy"></a>Jak odwoływać się dynamicznie generowanym serwera proxy

W powyższym przykładzie odwołanie do serwera proxy SignalR, generowany jest dynamicznie generowanego kodu JavaScript, nie do pliku fizycznego. SignalR tworzy kod JavaScript dla serwera proxy na bieżąco i służy ona do klienta w odpowiedzi na adres URL "/ signalr/centra". Jeśli określono inny podstawowy adres URL dla połączenia SignalR na serwerze w Twojej `MapHubs` metody, adres URL pliku dynamicznie generowanym serwera proxy jest adres URL niestandardowego za pomocą "/ koncentratory" dołączone do niego.

> [!NOTE]
> Dla klientów systemu Windows 8 (Windows Store) JavaScript należy użyć pliku fizycznego serwera proxy zamiast generowanych dynamicznie. Aby uzyskać więcej informacji, zobacz [sposobu tworzenia pliku fizycznego dla elementu SignalR wygenerowany serwer proxy](#manualproxy) w dalszej części tego tematu.


W widoku Razor programu ASP.NET MVC 4 należy użyć tylda do odwoływania się do katalogu głównego aplikacji, w której można się odwołać plik serwera proxy:

[!code-html[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample5.html)]

Aby uzyskać więcej informacji na temat korzystania z SignalR w MVC 4, zobacz [wprowadzenie do SignalR i MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md).

W widoku Razor programu ASP.NET MVC 3, należy użyć `Url.Content` usługi serwera proxy dla odwołania do pliku:

[!code-cshtml[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample6.cshtml)]

W aplikacji formularzy sieci Web ASP.NET, za pomocą `ResolveClientUrl` swoje serwery proxy odwołanie do pliku lub zarejestrować go za pośrednictwem Menedżera skryptów przy użyciu ścieżki względnej katalogu głównego aplikacji (rozpoczynający się tyldą):

[!code-aspx[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample7.aspx)]

Zgodnie z ogólną zasadą należy użyć tej samej metody dla określania adresu URL "/ signalr/centra", której użyjesz dla plików CSS i JavaScript. Jeśli określisz adresu URL bez użycia tyldy w niektórych scenariuszach aplikacja będzie działać poprawnie podczas testowania w programie Visual Studio za pomocą usług IIS Express, ale zakończy się niepowodzeniem z powodu błędu 404, podczas wdrażania usługi IIS. Aby uzyskać więcej informacji, zobacz **rozpoznawania odwołań do zasobów na poziomie głównym** w [serwerów sieci Web w programie Visual Studio dla projektów sieci Web platformy ASP.NET](https://msdn.microsoft.com/library/58wxa9w5.aspx) w witrynie MSDN.

Po uruchomieniu projektu sieci web w programie Visual Studio 2012 w trybie debugowania, jeśli używasz programu Internet Explorer jako przeglądarki widać plik serwera proxy w **Eksploratora rozwiązań** w obszarze **dokumenty skryptów**, jak pokazano na na poniższej ilustracji.

![Plik wygenerowany serwer proxy JavaScript w Eksploratorze rozwiązań](signalr-1x-hubs-api-guide-javascript-client/_static/image1.png)

Aby wyświetlić zawartość pliku, kliknij dwukrotnie **koncentratory**. Jeśli nie używasz programu Visual Studio 2012 i program Internet Explorer lub jeśli nie jesteś w trybie debugowania, możesz także uzyskać zawartość pliku, przechodząc do adresu URL "/ signalR/centra". Na przykład, jeśli witryna jest hostowana na `http://localhost:56699`, przejdź do `http://localhost:56699/SignalR/hubs` w przeglądarce.

<a id="manualproxy"></a>

### <a name="how-to-create-a-physical-file-for-the-signalr-generated-proxy"></a>Jak utworzyć plik fizyczny dla elementu SignalR wygenerowany serwer proxy

Jako alternatywy dla dynamicznie generowanych serwera proxy można utworzyć pliku fizycznego, który ma kod serwera proxy i odwoływać się do tego pliku. Można w tym kontrolę nad buforowania i tworzenia pakietów zachowanie lub korzystania z funkcji IntelliSense podczas kodowania wywołania metody serwera.

Aby utworzyć plik serwera proxy, wykonaj następujące czynności:

1. Zainstaluj [Microsoft.AspNet.SignalR.Utils](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) pakietu NuGet.
2. Otwórz wiersz polecenia i przejdź do *narzędzia* folder zawierający plik SignalR.exe. Folder narzędzia znajduje się w następującej lokalizacji:

    `[your solution folder]\packages\Microsoft.AspNet.SignalR.Utils.1.0.1\tools`
3. Wprowadź następujące polecenie:

    `signalr ghp /path:[path to the .dll that contains your Hub class]`

    Ścieżka do Twojej *.dll* jest zazwyczaj *bin* folderu w folderze projektu.

    To polecenie tworzy plik o nazwie *server.js* w tym samym folderze co *signalr.exe*.
4. Umieść *server.js* plików w folderze właściwe w projekcie, zmienić jego nazwę odpowiednio do aplikacji i Dodaj odwołanie do niego zamiast odwołania "koncentratory/signalr".

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a>Jak nawiązać połączenie

Przed może nawiązać połączenie, należy utworzyć obiekt połączenia, utworzyć serwer proxy i zarejestruj procedury obsługi zdarzeń dla metod, które mogą być wywoływane z serwera. Po skonfigurowaniu serwera proxy i programów obsługi nawiązania połączenia przez wywołanie metody `start` metody.

Jeśli używasz wygenerowany serwer proxy, nie trzeba utworzyć obiekt połączenia we własnym kodzie, ponieważ kod wygenerowany serwer proxy zrobi to za Ciebie.

<a id="nogenconnection"></a>

**Nawiąż połączenie (z wygenerowany serwer proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample8.js?highlight=5)]

**Nawiąż połączenie (bez wygenerowany serwer proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample9.js?highlight=1,6)]

Przykładowy kod używa domyślnej "/ signalr" adres URL, aby nawiązać połączenie z usługą SignalR. Aby uzyskać informacje o sposobie określania innego podstawowego adresu URL, zobacz [Podręcznik interfejsu API centrów SignalR platformy ASP.NET - Server - /signalr URL](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).

> [!NOTE]
> Zwykle rejestrowanie procedur obsługi zdarzeń przed wywołaniem `start` metodę, aby nawiązać połączenie. Jeśli chcesz zarejestrować niektóre procedury obsługi zdarzeń po ustanowieniu połączenia, możesz to zrobić, ale musisz się zarejestrować, co najmniej jeden z Twojej handler(s) zdarzeń przed wywołaniem `start` metody. Jeden przyczyną jest to, że może istnieć wiele centrów w aplikacji, ale nie chcesz wyzwolić `OnConnected` zdarzenia dla każdego Centrum, jeśli tylko będą korzystać z jednym z nich. Po nawiązaniu połączenia jest obecność metodę klienta dla serwera proxy koncentratora, co informuje SignalR do wyzwolenia `OnConnected` zdarzeń. Jeżeli nie zarejestrujesz dowolnej procedury obsługi zdarzeń przed wywołaniem `start` metody, można do wywołania metody koncentratora, ale Centrum `OnConnected` nie można wywołać metody i żadnych metod klienta zostanie wywołana z serwera.


<a id="connequivalence"></a>

### <a name="connectionhub-is-the-same-object-that-hubconnection-creates"></a>$. connection.hub jest taki sam obiekt, tworzy tego $.hubConnection()

Jak widać w przykładach, gdy używasz wygenerowany serwer proxy `$.connection.hub` odwołuje się do obiektu połączenia. Jest to ten sam obiekt, który można uzyskać przez wywołanie metody `$.hubConnection()` gdy nie używasz wygenerowany serwer proxy. Kod wygenerowany serwer proxy tworzy połączenie, wykonując następującą instrukcję:

![Tworzenie połączenia w pliku wygenerowanego serwera proxy](signalr-1x-hubs-api-guide-javascript-client/_static/image3.png)

Wygenerowany serwer proxy jest używany, można wykonać z `$.connection.hub` , można wykonać za pomocą obiektu połączenia, gdy nie używasz wygenerowany serwer proxy.

<a id="asyncstart"></a>

### <a name="asynchronous-execution-of-the-start-method"></a>Wykonanie asynchroniczne metody start

`start` Metoda jest wykonywana asynchronicznie. Zwraca [obiektu opóźnionych jQuery](http://api.jquery.com/category/deferred-object/), co oznacza, że można dodać funkcji wywołania zwrotnego wywoływania metod, takich jak `pipe`, `done`, i `fail`. Jeśli masz kod, który ma zostać wykonany po nawiązaniu połączenia, takie jak wywołanie metody serwera, umieść ten kod w funkcji wywołania zwrotnego lub wywołać go z funkcji wywołania zwrotnego. `.done` Metody wywołania zwrotnego jest wykonywane po ustanowieniu połączenia, a po dowolny kod, który ma swojej `OnConnected` zakończy się wykonywanie metody obsługi zdarzeń na serwerze.

Jeśli umieścisz instrukcję "Teraz connected", z poprzedniego przykładu jako następnego wiersza kodu po `start` wywołania metody (nie w `.done` wywołania zwrotnego), `console.log` wiersza zostaną wykonane, zanim połączenie zostanie nawiązane, jak pokazano w następującym przykład:

![Problem ze sposobem pisania kodu, który jest uruchamiany po nawiązaniu połączenia](signalr-1x-hubs-api-guide-javascript-client/_static/image5.png)

<a id="crossdomain"></a>

## <a name="how-to-establish-a-cross-domain-connection"></a>Jak nawiązać połączenie między domenami

Zazwyczaj, gdy przeglądarka ładuje stronę z `http://contoso.com`, połączenia SignalR jest w tej samej domenie, w `http://contoso.com/signalr`. Jeśli strona z `http://contoso.com` nawiązuje połączenie `http://fabrikam.com/signalr`, oznacza to połączenie między domenami. Ze względów bezpieczeństwa połączeń między domenami są domyślnie wyłączone. Aby nawiązać połączenie między domenami, upewnij się, że połączenia między domenami są włączone na serwerze, a adres URL połączenia można określić podczas tworzenia obiektu połączenia. SignalR użyje odpowiedniej technologii dla połączeń między domenami, takich jak [JSONP](http://en.wikipedia.org/wiki/JSONP) lub [CORS](http://en.wikipedia.org/wiki/Cross-origin_resource_sharing).

Na serwerze, należy włączyć połączenia między domenami, wybierając tę opcję, gdy wywołujesz `MapHubs` metody.

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample10.cs?highlight=2)]

Na komputerze klienckim należy określić adres URL, podczas tworzenia obiektu połączenia (bez wygenerowany serwer proxy), lub przed wywołaniem metody start (z wygenerowany serwer proxy).

**Kod klienta, który określa połączenie między domenami (z wygenerowany serwer proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample11.js?highlight=1)]

**Kod klienta, który określa połączenie między domenami (bez wygenerowany serwer proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample12.js?highlight=1)]

Kiedy używasz `$.hubConnection` konstruktora, nie trzeba uwzględnić `signalr` w adresie URL, ponieważ jest automatycznie dodawany (chyba że określisz `useDefaultUrl` jako `false`).

Możesz utworzyć wiele połączeń do różnych punktów końcowych.

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample13.js)]

> [!NOTE] 
> 
> - Nie należy ustawiać `jQuery.support.cors` na wartość true w kodzie.
> 
>     ![Nie jest ustawiana na wartość true jQuery.support.cors](signalr-1x-hubs-api-guide-javascript-client/_static/image7.png)
> 
>     SignalR obsługuje stosowanie JSONP lub mechanizmu CORS. Ustawienie `jQuery.support.cors` do wartości true powoduje wyłączenie JSONP, ponieważ powoduje ona SignalR założył, przeglądarka obsługuje mechanizm CORS.
> - Podczas łączenia z adresem URL localhost, programu Internet Explorer 10 nie należy wziąć pod uwagę jej połączenie między domenami, dzięki czemu aplikacja będzie działać lokalnie za pomocą programu Internet Explorer 10, nawet jeśli nie zostały włączone połączenia między domenami na serwerze.
> - Aby dowiedzieć się, jak za pomocą połączeń między domenami, za pomocą programu Internet Explorer 9, zobacz [wątek w witrynie StackOverflow](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work).
> - Aby dowiedzieć się, jak za pomocą przeglądarki Chrome przy użyciu połączenia między domenami, zobacz [wątek w witrynie StackOverflow](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome).
> - Przykładowy kod używa domyślnej "/ signalr" adres URL, aby nawiązać połączenie z usługą SignalR. Aby uzyskać informacje o sposobie określania innego podstawowego adresu URL, zobacz [Podręcznik interfejsu API centrów SignalR platformy ASP.NET - Server - /signalr URL](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).


<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a>Jak skonfigurować połączenie

Przed nawiązaniem połączenia, można określić parametry ciągu zapytania lub Określ metodę transportu.

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a>Jak określić parametry ciągu zapytania

Jeśli chcesz wysyłać dane do serwera, gdy klient nawiąże połączenie, można dodać parametry ciągu zapytania do obiektu połączenia. Poniższe przykłady pokazują, jak można ustawić jako parametr ciągu zapytania w kodzie klienta.

**Ustaw wartość ciągu zapytania przed wywołaniem metody start (z wygenerowany serwer proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample14.js?highlight=1)]

**Ustaw wartość ciągu zapytania przed wywołaniem metody start (bez wygenerowany serwer proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample15.js?highlight=2)]

Poniższy przykład pokazuje, jak odczytywać parametr ciągu zapytania w kodzie serwera.

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample16.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a>Jak określić metodę transportu

Częścią procesu nawiązywania połączenia klienta SignalR zwykle negocjuje z serwerem, aby określić najlepsze transportu, która jest obsługiwana przez serwer i klienta. Jeśli znasz już transport, którego chcesz użyć, można pominąć ten proces negocjacji, określając metodą transportu podczas wywoływania `start` metody.

**Kod klienta, który określa metodę transportu (z wygenerowany serwer proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample17.js?highlight=1)]

**Kod klienta, który określa metodę transportu (bez wygenerowany serwer proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample18.js?highlight=2)]

Alternatywnie można określić wielu metod transportu, w kolejności, w której chcesz SignalR, aby wypróbować je:

**Kod klienta, który określa rezerwowy schematu transport w niestandardowych (z wygenerowany serwer proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample19.js?highlight=1)]

**Kod klienta, który określa rezerwowy schemat transportu niestandardowe (bez wygenerowany serwer proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample20.js?highlight=2)]

Do określania metodą transportu, można użyć następujących wartości:

- "webSockets"
- "foreverFrame"
- "serverSentEvents"
- "longPolling"

Poniższe przykłady pokazują, jak można dowiedzieć się, jakiej metody transportu jest używany przez połączenie.

**Kod klienta, który zawiera metodę transportu, używane przez połączenia (z wygenerowany serwer proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample21.js?highlight=2)]

**Kod klienta, który zawiera metodę transportu, używany przez połączenie (bez wygenerowany serwer proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample22.js?highlight=3)]

Aby uzyskać informacje o sposobie Sprawdź metodą transportu w kodzie serwera, zobacz [Podręcznik interfejsu API centrów SignalR platformy ASP.NET - Server - sposób uzyskiwania informacji na temat klienta z właściwości kontekstu](../guide-to-the-api/hubs-api-guide-server.md#contextproperty). Aby uzyskać więcej informacji dotyczących transportu i planów awaryjnych, zobacz [wprowadzenie do SignalR - transportu i planów awaryjnych](../getting-started/introduction-to-signalr.md#transports).

<a id="getproxy"></a>

## <a name="how-to-get-a-proxy-for-a-hub-class"></a>Jak uzyskać serwera proxy dla klasy koncentratora

Każdy obiekt połączenia, którą tworzysz hermetyzuje informacje dotyczące połączenia SignalR service, który zawiera co najmniej jedną klasę koncentratora. Aby komunikować się z klasy koncentratora, należy użyć obiektu serwera proxy utworzonych samodzielnie (Jeśli nie używasz wygenerowany serwer proxy) lub która zostanie wygenerowany.

Na komputerze klienckim nazwę serwera proxy jest wersja formacie camelcase nazwy klasy koncentratora. SignalR automatycznie sprawia, że ta zmiana tak, aby kod JavaScript można jest zgodna z konwencjami języka JavaScript.

**Klasy koncentratora na serwerze**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample23.cs?highlight=1)]

**Pobierz odwołanie do serwera proxy wygenerowanego klienta dla koncentratora**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample24.js?highlight=1)]

**Utwórz serwer proxy klienta dla klasy koncentratora (bez wygenerowany serwer proxy)**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample25.cs?highlight=1)]

Jeśli dekoracji klasy koncentratora z `HubName` atrybutu, należy użyć dokładnej nazwy bez wprowadzania zmian w przypadku.

**Klasy koncentratora na serwerze z atrybutem HubName**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample26.cs?highlight=1)]

**Pobierz odwołanie do serwera proxy wygenerowanego klienta dla koncentratora**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample27.js?highlight=1)]

**Utwórz serwer proxy klienta dla klasy koncentratora (bez wygenerowany serwer proxy)**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample28.cs?highlight=1)]

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a>Sposób definiowania metody na kliencie, który można wywołać serwera

Aby zdefiniować metodę, która serwera można wywołać z koncentratorem, Dodaj program obsługi zdarzeń do serwera proxy koncentratora za pomocą `client` właściwość wygenerowany serwer proxy lub wywołanie `on` metody, jeśli nie używasz wygenerowany serwer proxy. Parametry mogą być złożone obiekty.

Dodawanie obsługi zdarzeń, przed wywołaniem `start` metodę, aby nawiązać połączenie. (Jeśli chcesz dodać procedury obsługi zdarzeń po wywołaniu `start` metody, zobacz uwagi w [jak nawiązać połączenie](#establishconnection) we wcześniejszej części tego dokumentu, a następnie użyj składni przedstawionej w celu zdefiniowania metody bez użycia wygenerowany serwer proxy.)

Dopasowywaniu nazwy metody nie jest rozróżniana wielkość liter. Na przykład `Clients.All.addContosoChatMessageToPage` będą wykonywane na serwerze `AddContosoChatMessageToPage`, `addContosoChatMessageToPage`, lub `addcontosochatmessagetopage` na komputerze klienckim.

**Zdefiniuj metodę na komputerze klienckim (z wygenerowany serwer proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample29.js?highlight=2)]

**Alternatywny sposób definiowania metody na komputerze klienckim (z wygenerowany serwer proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample30.js?highlight=1-2)]

**Zdefiniuj metodę na kliencie (bez wygenerowany serwer proxy lub podczas dodawania po wywołaniu metody start)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample31.js?highlight=3)]

**Kod serwera, który wywołuje metodę klienta**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample32.cs?highlight=5)]

Poniższe przykłady obiektu złożonego jako parametr metody.

**Zdefiniuj metodę na komputerze klienckim, który przyjmuje obiekt złożony (z wygenerowany serwer proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample33.js?highlight=2-3)]

**Zdefiniuj metodę na komputerze klienckim, który przyjmuje obiekt złożony (bez wygenerowany serwer proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample34.js?highlight=3-4)]

**Kod serwera, który definiuje obiekt złożony**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample35.cs?highlight=1)]

**Kod serwera, który wywołuje metodę klienta przy użyciu obiektu złożonego**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample36.cs?highlight=3)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a>Jak wywołać metody serwera z poziomu klienta

Aby wywołać metody serwera od klienta, należy użyć `server` właściwość wygenerowany serwer proxy lub `invoke` metody serwera proxy koncentratora, jeśli nie używasz wygenerowany serwer proxy. Wartości zwracane lub parametry mogą być złożone obiekty.

Przekaż camelcase wersję nazwę metody koncentratora. SignalR automatycznie sprawia, że ta zmiana tak, aby kod JavaScript można jest zgodna z konwencjami języka JavaScript.

W poniższych przykładach pokazano, jak wywoływać metody serwera, który nie ma wartości zwracanej i sposób wywoływania metody serwera, który ma wartość zwracaną.

**Metoda serwera nie atrybutem HubMethodName**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample37.cs?highlight=3)]

**Kod serwera, który definiuje obiektu złożonego przekazanego do parametru**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample38.cs)]

**Kod klienta, który wywołuje metodę serwera (za pomocą wygenerowany serwer proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample39.js?highlight=1)]

**Kod klienta, który wywołuje metodę serwera (bez wygenerowany serwer proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample40.js?highlight=1)]

Jeśli dekorowane metody koncentratora, za pomocą `HubMethodName` atrybutu, należy użyć tej nazwy bez wprowadzania zmian w przypadku.

**Metoda serwera** atrybutem HubMethodName

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample41.cs?highlight=3)]

**Kod klienta, który wywołuje metodę serwera (za pomocą wygenerowany serwer proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample42.js?highlight=1)]

**Kod klienta, który wywołuje metodę serwera (bez wygenerowany serwer proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample43.js?highlight=1)]

Sposób wywoływania metody serwera, która nie zwraca żadnej wartości można znaleźć w poprzednich przykładach. Następujące przykłady przedstawiają sposób wywoływania metody serwera, która nie zwraca wartości.

**Kod serwera dla metody, która nie zwraca wartości**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample44.cs?highlight=3)]

**Klasy zapasów, używany do** zwracają wartość

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample45.cs?highlight=1)]

**Kod klienta, który wywołuje metodę serwera (za pomocą wygenerowany serwer proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample46.js?highlight=2,4-5)]

**Kod klienta, który wywołuje metodę serwera (bez wygenerowany serwer proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample47.js?highlight=2,4-5)]

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a>Jak obsługiwać zdarzenia okresu istnienia połączenia

Biblioteka SignalR udostępnia następujące połączenia zdarzenia okresu istnienia, które może obsłużyć:

- `starting`: Wywoływane przed wszystkie dane są wysyłane za pośrednictwem połączenia.
- `received`: Zgłaszane w przypadku nieodebrania żadnych danych w połączeniu. Udostępnia odebrane dane.
- `connectionSlow`: Wywoływane, gdy klient wykrywa wolno lub często porzucanie połączenia.
- `reconnecting`: Wywoływane, gdy transportu źródłowego rozpoczyna ponowne nawiązywanie połączenia.
- `reconnected`: Wywoływane, gdy nawiązał transportu źródłowego.
- `stateChanged`: Wywoływane, gdy zmieni się stan połączenia. Zawiera stan stary i nowy stan (łączenie, połączony, ponowne nawiązywanie połączenia i Rozłączono).
- `disconnected`: Wywoływane, gdy połączenie zostało rozłączone.

Na przykład, jeśli chcesz wyświetlić komunikaty ostrzegawcze, gdy występują problemy z połączeniem, które mogłyby spowodować zauważalnego opóźnienia, obsługi `connectionSlow` zdarzeń.

**Obsługuj zdarzenie connectionSlow (z wygenerowany serwer proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample48.js?highlight=1)]

**Obsługuj zdarzenie connectionSlow (bez wygenerowany serwer proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample49.js?highlight=2)]

Aby uzyskać więcej informacji, zobacz [zrozumienia i obsługa zdarzeń okresu istnienia połączenia w SignalR](index.md).

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a>Sposób obsługi błędów

Udostępnia klienta SignalR JavaScript `error` zdarzenia, które można dodać program obsługi. Metoda niepowodzenie umożliwia również dodawanie obsługi błędów, które wynikają z wywołania metody serwera.

Jeśli nie włączysz jawnie szczegółowe komunikaty o błędach na serwerze, obiekt wyjątku, który zwraca SignalR, po wystąpieniu błędu zawiera minimalne informacje o tym błędzie. Na przykład, jeśli wywołanie `newContosoChatMessage` zawiera komunikat o błędzie w obiekcie błąd zakończy się niepowodzeniem, "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" Wysyłanie szczegółowe komunikaty o błędach do klientów w środowisku produkcyjnym jest niezalecane ze względów bezpieczeństwa, ale jeśli chcesz włączyć szczegółowe komunikaty o błędach dla rozwiązywania problemów, użyj poniższego kodu, na serwerze.

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample50.cs?highlight=2)]

Poniższy przykład pokazuje, jak dodać program obsługi zdarzenia błędu.

**Dodaj procedurę obsługi błędów (z wygenerowany serwer proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample51.js?highlight=1)]

**Dodaj procedurę obsługi błędów (bez wygenerowany serwer proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample52.js?highlight=2)]

Poniższy przykład pokazuje, jak obsłużyć błąd z wywołania metody.

**Obsłużyć błąd w wywołaniu metody (z wygenerowany serwer proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample53.js?highlight=2)]

**Obsłużyć błąd w wywołaniu metody (bez wygenerowany serwer proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample54.js?highlight=2)]

W przypadku niepowodzenia wywołania metody `error` zdarzenie jest również zgłaszane, więc swój kod w `error` obsługę metody i w `.fail` metody wywołania zwrotnego jest wykonywany.

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a>Włączanie rejestrowania po stronie klienta

Aby włączyć połączenia logowania po stronie klienta, należy ustawić `logging` właściwości w obiekcie połączenia przed wywołaniem `start` metodę, aby nawiązać połączenie.

**Włączanie rejestrowania (z wygenerowany serwer proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample55.js?highlight=1)]

**Włączanie rejestrowania (bez wygenerowany serwer proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample56.js?highlight=2)]

Aby wyświetlić dzienniki, Otwórz narzędzia deweloperskie przeglądarki i przejdź do karty konsoli. Aby uzyskać samouczek, który zawiera instrukcje krok po kroku i ekran zrzuty, które pokazują, jak to zrobić, zobacz [emisje serwera z użyciem ASP.NET Signalr - Włącz rejestrowanie](index.md).
