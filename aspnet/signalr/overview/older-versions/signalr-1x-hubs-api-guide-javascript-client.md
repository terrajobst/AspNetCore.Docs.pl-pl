---
uid: signalr/overview/older-versions/signalr-1x-hubs-api-guide-javascript-client
title: Przewodnik interfejsu API koncentratorów SignalR 1.x — klienta JavaScript | Dokumentacja firmy Microsoft
author: pfletcher
description: Ten dokument zawiera wprowadzenie do korzystania z interfejsu API koncentratorów dla biblioteki SignalR w wersji 1.1 języka JavaScript klientów, takie jak przeglądarki i Sklepu Windows (WinJS) applic...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/17/2013
ms.topic: article
ms.assetid: dcd4593b-1118-418a-af71-d12ff33fb36d
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/signalr-1x-hubs-api-guide-javascript-client
msc.type: authoredcontent
ms.openlocfilehash: f92470b2022f343cfd6d822abb255dc19947b4d1
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="signalr-1x-hubs-api-guide---javascript-client"></a>Podręcznik interfejsu API SignalR 1.x koncentratory — JavaScript klienta
====================
przez [Patrick Fletcher](https://github.com/pfletcher), [Dykstra niestandardowy](https://github.com/tdykstra)

> Ten dokument zawiera wprowadzenie do korzystania z interfejsu API koncentratorów dla biblioteki SignalR w wersji 1.1 języka JavaScript klientów, takie jak przeglądarki i aplikacje ze Sklepu Windows (WinJS).
> 
> Interfejs API koncentratorów SignalR umożliwia upewnij zdalnych wywołań procedur (RPC) z serwera do połączonych klientów i klientów na serwerze. W kodzie serwera zdefiniuj metody, które mogą być wywoływane przez klientów i wywołania metod, które są uruchamiane na komputerze klienckim. W kodu klienta należy zdefiniować metody, które mogą być wywoływane z serwera i wywołania metody, które są uruchamiane na serwerze. SignalR odpowiada on za wszystkie żmudne procesy klient serwer dla Ciebie.
> 
> SignalR udostępnia również interfejs API niższego poziomu o nazwie połączeń trwałych. Wprowadzenie do SignalR, koncentratorach i połączeń trwałych lub Samouczek przedstawiający sposób tworzenia pełnej aplikacji SignalR, patrz [SignalR — wprowadzenie](../getting-started/index.md).


## <a name="overview"></a>Omówienie

Ten dokument zawiera następujące sekcje:

- [Wygenerowany serwer proxy i jakie operacje dla Ciebie](#genproxy)

    - [Kiedy należy używać wygenerowany serwer proxy](#cantusegenproxy)
- [Instalacja klienta](#clientsetup)

    - [Jak odwołanie dynamicznie generowanym serwera proxy](#dynamicproxy)
    - [Jak utworzyć plik fizyczny dla elementu SignalR wygenerowany serwer proxy](#manualproxy)
- [Jak nawiązać połączenie](#establishconnection)

    - [$. connection.hub jest taki sam obiekt, utworzenie tego $.hubConnection()](#connequivalence)
    - [Wykonanie asynchroniczne, metody rozpoczęcia](#asyncstart)
- [Jak nawiązać połączenie między domenami](#crossdomain)
- [Jak skonfigurować połączenia](#configureconnection)

    - [Jak określać parametrów ciągu zapytania](#querystring)
    - [Jak określić metodę transportu](#transport)
- [Jak uzyskać serwer proxy dla klasy Centrum](#getproxy)
- [Sposób definiowania metod na komputerze klienckim, który można wywołać serwera](#callclient)
- [Sposób wywołania metody serwera z klienta](#callserver)
- [Sposób obsługi zdarzeń okres istnienia połączenia](#connectionlifetime)
- [Sposób obsługi błędów](#handleerrors)
- [Jak włączyć rejestrowanie klienta](#logging)

Dokumentacja na temat programu server lub klientów platformy .NET, zobacz następujące zasoby:

- [Podręcznik interfejsu API koncentratorów SignalR — serwer](../guide-to-the-api/hubs-api-guide-server.md)
- [Podręcznik interfejsu API koncentratorów SignalR - klienta .NET](../guide-to-the-api/hubs-api-guide-net-client.md)

Linki do tematów dokumentacji interfejsu API są wersja platformy .NET 4.5 interfejsu API. Jeśli używasz programu .NET 4, zobacz [wersji .NET 4 tematy interfejsu API](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).

<a id="genproxy"></a>

## <a name="the-generated-proxy-and-what-it-does-for-you"></a>Wygenerowany serwer proxy i jakie operacje dla Ciebie

Można program klienta JavaScript do komunikowania się z usługą SignalR z lub bez serwera proxy, który generuje SignalR dla Ciebie. Serwer proxy wykonuje automatycznie jest upraszczają składnię kodu przy użyciu połączenia metod zapisywania, które wymaga serwera, a wywoływać metod na serwerze.

Podczas pisania kodu do wywołania metody serwera proxy wygenerowanego pozwala na użycie składnię, która wygląda, jakby były wykonywania funkcji lokalnej: można napisać `serverMethod(arg1, arg2)` zamiast `invoke('serverMethod', arg1, arg2)`. Składnia wygenerowany serwer proxy również umożliwia natychmiastowe i zrozumiały błąd po stronie klienta, jeśli wpisujesz nazwę metody serwera. A jeśli ręcznie utworzyć plik, który definiuje serwerów proxy, można także uzyskać obsługę funkcji IntelliSense do pisania kodu, który wywołuje metody serwera.

Na przykład załóżmy, że masz następujące klasy koncentratora na serwerze:

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample1.cs?highlight=1,3,5)]

W poniższych przykładach kodu pokazano, co JavaScript kod wygląda podobnie do wywoływania `NewContosoChatMessage` metody na serwerze i odbieranie wywołań `addContosoChatMessageToPage` metody z serwera.

**Przy użyciu wygenerowanego serwera proxy**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample2.js?highlight=1-2,8)]

**Bez wygenerowany serwer proxy**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample3.js?highlight=2-3,9)]

<a id="cantusegenproxy"></a>

### <a name="when-to-use-the-generated-proxy"></a>Kiedy należy używać wygenerowany serwer proxy

Jeśli chcesz zarejestrować wielu obsług zdarzeń dla metody klienta, która wymaga serwera, nie możesz użyć wygenerowanego serwera proxy. W przeciwnym razie można wybrać opcję użycia wygenerowany serwer proxy lub nie jest oparty na preferencje kodowania. Jeśli wybierzesz nie go użyć, nie trzeba odwoływać "koncentratory/signalr" adres URL w `script` element kodu klienta.

<a id="clientsetup"></a>

## <a name="client-setup"></a>Instalacja klienta

Klient JavaScript wymaga odwołania do jQuery i plik JavaScript SignalR core. Wersja jQuery musi być 1.6.4 lub nowsze wersje główne 1.7.2, 1.8.2 lub 1.9.1. Jeśli zdecydujesz się używać wygenerowany serwer proxy, należy również odwołanie do serwera proxy SignalR wygenerowany plik JavaScript. W poniższym przykładzie pokazano, jak odwołania może wyglądać na stronie HTML, który używa wygenerowany serwer proxy.

[!code-html[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample4.html)]

Te odwołania muszą być zawarte w następującej kolejności: jQuery imię, nazwisko SignalR core po i serwery proxy SignalR.

<a id="dynamicproxy"></a>

### <a name="how-to-reference-the-dynamically-generated-proxy"></a>Jak odwołanie dynamicznie generowanym serwera proxy

W powyższym przykładzie odwołanie do serwera proxy generowany SignalR jest dynamicznie wygenerowanego kodu JavaScript, nie do pliku fizycznego. SignalR tworzy kod JavaScript dla serwera proxy na bieżąco i udostępnia ono do klienta w odpowiedzi na adres URL "/ signalr/hubs". Jeśli określono inny podstawowy adres URL dla połączenia SignalR na serwerze w sieci `MapHubs` metody, adres URL pliku dynamicznie generowanym serwera proxy jest adres URL niestandardowego z "/ koncentratory" dołączone do niego.

> [!NOTE]
> Dla klientów JavaScript systemu Windows 8 (w Sklepie Windows) należy użyć pliku fizycznego serwera proxy zamiast dynamicznie generowanym. Aby uzyskać więcej informacji, zobacz [proxy generowany sposobu tworzenia pliku fizycznego dla elementu SignalR](#manualproxy) dalszej części tego tematu.


W widoku programu ASP.NET MVC 4 Razor używana tylda do odwoływania się do katalogu głównego aplikacji w odwołaniu do pliku z serwera proxy:

[!code-html[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample5.html)]

Aby uzyskać więcej informacji o używaniu SignalR w MVC 4, zobacz [wprowadzenie SignalR i MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md).

W widoku programu ASP.NET MVC 3 Razor, użyj `Url.Content` Twojego serwera proxy dla odwołania do pliku:

[!code-cshtml[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample6.cshtml)]

W aplikacji formularzy sieci Web ASP.NET, użyj `ResolveClientUrl` Twojego proxy odwołanie do pliku lub zarejestruj go za pośrednictwem ScriptManager przy użyciu aplikacji głównej ścieżki względnej (rozpoczynający się tyldą):

[!code-aspx[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample7.aspx)]

Jako ogólną regułę należy używać tej samej metody do określania adresu URL "/ signalr/hubs" używanego w przypadku plików CSS i JavaScript. Jeśli określisz adresu URL bez użycia tyldy w niektórych scenariuszach aplikacja będzie działać poprawnie podczas testowania w programie Visual Studio za pomocą usług IIS Express, ale zakończy się niepowodzeniem z błędem 404, podczas wdrażania usługi IIS. Aby uzyskać więcej informacji, zobacz **rozpoznawania odwołania do zasobów na poziomie głównym** w [serwerów sieci Web w programie Visual Studio dla projektów sieci Web ASP.NET](https://msdn.microsoft.com/library/58wxa9w5.aspx) w witrynie MSDN.

Podczas uruchamiania projektu sieci web w programie Visual Studio 2012 w trybie debugowania, a jeśli używasz programu Internet Explorer jako przeglądarki widać pliku serwera proxy w **Eksploratora rozwiązań** w obszarze **dokumentów skryptu**, jak pokazano w poniższej ilustracji.

![Plik wygenerowany serwer proxy JavaScript w Eksploratorze rozwiązań](signalr-1x-hubs-api-guide-javascript-client/_static/image1.png)

Aby wyświetlić zawartość pliku, kliknij dwukrotnie **koncentratory**. Jeśli nie używasz programu Visual Studio 2012 i przeglądarki Internet Explorer lub nie są w trybie debugowania, można także uzyskać zawartość pliku, przechodząc do adresu URL "/ signalR/hubs". Na przykład, jeśli witryna jest hostowana na `http://localhost:56699`, przejdź do `http://localhost:56699/SignalR/hubs` w przeglądarce.

<a id="manualproxy"></a>

### <a name="how-to-create-a-physical-file-for-the-signalr-generated-proxy"></a>Jak utworzyć plik fizyczny dla elementu SignalR wygenerowany serwer proxy

Alternatywą dla dynamicznie generowanym serwera proxy można utworzyć pliku fizycznego, który zawiera kod serwera proxy i się odwoływać. Można to zrobić, aby kontrolować buforowanie lub tworzenie pakietów zachowanie lub pobrać IntelliSense, gdy są kodowania wywołania metody serwera.

Aby utworzyć plik serwera proxy, wykonaj następujące czynności:

1. Zainstaluj [Microsoft.AspNet.SignalR.Utils](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) pakietu NuGet.
2. Otwórz wiersz polecenia i przejdź do *narzędzia* folder zawierający plik SignalR.exe. Folder narzędzia znajduje się w następującej lokalizacji:

    `[your solution folder]\packages\Microsoft.AspNet.SignalR.Utils.1.0.1\tools`
3. Wprowadź następujące polecenie:

    `signalr ghp /path:[path to the .dll that contains your Hub class]`

    Ścieżka do Twojej *.dll* jest zwykle *bin* folderu w folderze projektu.

    To polecenie tworzy plik o nazwie *server.js* w tym samym folderze co *signalr.exe*.
4. Umieść *server.js* w odpowiedni folder w projekcie, zmień jego nazwę na potrzeby aplikacji, a następnie dodaj odwołanie do niego zamiast odwołania "koncentratory/signalr".

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a>Jak nawiązać połączenie

Przed może nawiązać połączenie, należy utworzyć obiektu połączenia, Utwórz serwer proxy i zarejestruj procedury obsługi zdarzeń dla metod, które mogą być wywoływane z serwera. Po skonfigurowaniu serwera proxy i programów obsługi nawiązania połączenia przez wywołanie metody `start` metody.

Jeśli używasz wygenerowany serwer proxy, nie trzeba utworzyć obiektu połączenia w swoim własnym kodem, ponieważ kod wygenerowany serwer proxy jest dla Ciebie.

<a id="nogenconnection"></a>

**Nawiązania z nim połączenia (wygenerowany serwer proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample8.js?highlight=5)]

**Nawiąż połączenie (bez wygenerowany serwer proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample9.js?highlight=1,6)]

Przykładowy kod używa domyślnej "/ signalr" adres URL do łączenia się z usługą SignalR. Aby uzyskać informacje o sposobie określania innego podstawowego adresu URL, zobacz [ASP.NET SignalR koncentratory interfejsu API przewodnik - Server - /signalr URL](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).

> [!NOTE]
> Zwykle Zarejestruj obsługę zdarzeń przed wywołaniem `start` metody do nawiązania połączenia. Jeśli chcesz zarejestrować niektóre programy obsługi zdarzeń po ustanowieniu połączenia, możesz to zrobić, ale musisz zarejestrować co najmniej jeden z zawierający program(y) obsługi zdarzeń przed wywołaniem `start` metody. Jedną z przyczyn to jest aplikacja może być wiele koncentratorów, że nie ma uruchamiać `OnConnected` zdarzenia w każdym koncentratora, jeśli mają tylko jeden z nich umożliwia. Po nawiązaniu połączenia jest obecność metody klienta dla serwera proxy koncentratora, co informuje SignalR do wyzwolenia `OnConnected` zdarzeń. Jeśli nie zarejestrujesz żadnych programów obsługi zdarzeń przed wywołaniem `start` metody, można do wywołania metody koncentratora, ale koncentratora `OnConnected` nie można wywołać metody i żadnych metod klienta zostanie wywołany z serwera.


<a id="connequivalence"></a>

### <a name="connectionhub-is-the-same-object-that-hubconnection-creates"></a>$. connection.hub jest taki sam obiekt, utworzenie tego $.hubConnection()

Jak widać w przykładach, gdy używasz wygenerowany serwer proxy `$.connection.hub` odwołuje się do obiektu połączenia. Jest to ten sam obiekt, który można pobrać przez wywołanie metody `$.hubConnection()` gdy nie używasz wygenerowany serwer proxy. Kod wygenerowany serwer proxy tworzy połączenie dla Ciebie, wykonując następującą instrukcję.

![Tworzenie połączenia w pliku wygenerowany serwer proxy](signalr-1x-hubs-api-guide-javascript-client/_static/image3.png)

Podczas korzystania z wygenerowany serwer proxy, można wykonywać wszystkie z `$.connection.hub` czy można wykonać za pomocą obiektu połączenia, gdy nie używasz wygenerowany serwer proxy.

<a id="asyncstart"></a>

### <a name="asynchronous-execution-of-the-start-method"></a>Wykonanie asynchroniczne, metody rozpoczęcia

`start` Metoda wykonuje asynchronicznie. Zwraca [obiektu opóźnieniem jQuery](http://api.jquery.com/category/deferred-object/), co oznacza, że można dodawać funkcje wywołania zwrotnego wywoływania metod, takich jak `pipe`, `done`, i `fail`. Jeśli masz kod, który będzie wykonywany po nawiązaniu połączenia, takie jak wywołanie metody server, umieść ten kod w funkcji wywołania zwrotnego lub wywołać ją z funkcji wywołania zwrotnego. `.done` Metody wywołania zwrotnego jest wykonywana po nawiązaniu połączenia i po żadnego kodu, który istnieje Twojej `OnConnected` metoda obsługi zdarzeń na serwerze ukończeniem wykonywania.

Jeśli umieścisz instrukcji "Połączone" z poprzedniego przykładu jako kolejny wiersz kodu po `start` wywołanie metody (nie w `.done` wywołania zwrotnego), `console.log` wiersza zostanie wykonany przed połączenie zostanie nawiązane, jak pokazano w następującym przykład:

![Niewłaściwy sposób pisania kodu, który jest uruchamiany po nawiązaniu połączenia](signalr-1x-hubs-api-guide-javascript-client/_static/image5.png)

<a id="crossdomain"></a>

## <a name="how-to-establish-a-cross-domain-connection"></a>Jak nawiązać połączenie między domenami

Zwykle Jeśli przeglądarka ładuje strony z `http://contoso.com`, połączenia SignalR jest w tej samej domenie, w `http://contoso.com/signalr`. Jeśli strona z `http://contoso.com` nawiązuje połączenie `http://fabrikam.com/signalr`, czyli połączenia między domenami. Ze względów bezpieczeństwa połączeń między domenami są domyślnie wyłączone. Nawiązanie połączenia między domenami, upewnij się, że połączenia między domenami są włączone na serwerze, a następnie określ adres URL połączenia podczas tworzenia obiektu połączenia. SignalR będzie używać odpowiednich technologii dla połączeń między domenami, takich jak [JSONP](http://en.wikipedia.org/wiki/JSONP) lub [CORS](http://en.wikipedia.org/wiki/Cross-origin_resource_sharing).

Na serwerze, należy włączyć połączenia między domenami, wybierając odpowiednią opcję podczas wywoływania `MapHubs` metody.

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample10.cs?highlight=2)]

Na komputerze klienckim Podaj adres URL, podczas tworzenia obiektu połączenia (bez wygenerowany serwer proxy) lub przed wywołaniem metody start (z wygenerowany serwer proxy).

**Kod klienta, który określa połączenia między domenami (z wygenerowany serwer proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample11.js?highlight=1)]

**Kod klienta, który definiuje połączenie między domenami (bez wygenerowany serwer proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample12.js?highlight=1)]

Jeśli używasz `$.hubConnection` konstruktora, nie trzeba uwzględnić `signalr` w adresie URL, ponieważ jest automatycznie dodawany (chyba że zostanie `useDefaultUrl` jako `false`).

Można utworzyć wiele połączeń z różnych punktów końcowych.

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample13.js)]

> [!NOTE] 
> 
> - Nie należy ustawiać `jQuery.support.cors` na wartość true w kodzie.
> 
>     ![Nie jest ustawiana na wartość true jQuery.support.cors](signalr-1x-hubs-api-guide-javascript-client/_static/image7.png)
> 
>     SignalR obsługuje używanie JSONP lub CORS. Ustawienie `jQuery.support.cors` na wartość PRAWDA powoduje wyłączenie JSONP, ponieważ powoduje ona SignalR do wniosku, przeglądarka obsługuje mechanizm CORS.
> - Jeśli łączysz się z adresem URL localhost, Internet Explorer 10 nie uważają, że połączenie między domenami, aplikacja będzie działać lokalnie z programu Internet Explorer 10, nawet jeśli nie zostały włączone połączenia między domenami na serwerze.
> - Aby uzyskać informacji o korzystaniu z programu Internet Explorer 9 połączeń między domenami, zobacz [tego wątku StackOverflow](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work).
> - Aby uzyskać informacji o korzystaniu z programu Chrome połączeń między domenami, zobacz [tego wątku StackOverflow](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome).
> - Przykładowy kod używa domyślnej "/ signalr" adres URL do łączenia się z usługą SignalR. Aby uzyskać informacje o sposobie określania innego podstawowego adresu URL, zobacz [ASP.NET SignalR koncentratory interfejsu API przewodnik - Server - /signalr URL](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).


<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a>Jak skonfigurować połączenia

Przed nawiązaniem połączenia, można określić parametrów ciągu zapytania lub określić metodę transportu.

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a>Jak określać parametrów ciągu zapytania

Jeśli chcesz wysyłać dane do serwera, gdy klient nawiąże połączenie, możesz dodać parametrów ciągu zapytania do obiektu połączenia. Poniższe przykłady przedstawiają sposób ustawić parametr ciągu zapytania w kodzie klienta.

**Ustaw wartość ciągu zapytania przed wywołaniem metody start (z wygenerowany serwer proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample14.js?highlight=1)]

**Ustaw wartość ciągu zapytania przed wywołaniem metody start (bez wygenerowany serwer proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample15.js?highlight=2)]

Poniższy przykład pokazuje, jak można odczytać parametr ciągu zapytania w kodzie serwera.

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample16.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a>Jak określić metodę transportu

W ramach procesu łączenia klienta SignalR zwykle negocjuje z serwerem w celu ustalenia najlepsze transport, który jest obsługiwany przez zarówno serwera i klienta. Jeśli znasz już transport, którego chcesz użyć, można pominąć ten proces negocjacji, określając Metoda transportu podczas wywoływania `start` metody.

**Kod klienta, który określa metodę transportu (z wygenerowany serwer proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample17.js?highlight=1)]

**Kod klienta, który określa metodę transportu (bez wygenerowany serwer proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample18.js?highlight=2)]

Alternatywnie można określić wielu metod transportu w kolejności, w której ma zostać SignalR ich:

**Kod klienta, który określa schemat rezerwowy niestandardowych transportu (o wygenerowany serwer proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample19.js?highlight=1)]

**Kod klienta, który określa rezerwowy schemat transportu niestandardowe (bez wygenerowany serwer proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample20.js?highlight=2)]

Następujące wartości można użyć do określenia metody transportu:

- "webSockets"
- "foreverFrame"
- "serverSentEvents"
- "longPolling"

Poniższe przykłady przedstawiają sposób dowiedzieć się, którego metoda transportu jest używany przez połączenie.

**Kod klienta, który zawiera metodę transportu używany przez połączenie (z wygenerowany serwer proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample21.js?highlight=2)]

**Kod klienta, który zawiera metodę transportu używany przez połączenie (bez wygenerowany serwer proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample22.js?highlight=3)]

Aby dowiedzieć się, jak sprawdzić Metoda transportu w kodzie serwera, zobacz [ASP.NET SignalR koncentratory interfejsu API przewodnik - Server - metody uzyskiwania informacji o kliencie z właściwości kontekstu](../guide-to-the-api/hubs-api-guide-server.md#contextproperty). Aby uzyskać więcej informacji na temat transportów i przejścia, zobacz [wprowadzenie do biblioteki SignalR — transportu i przejścia](../getting-started/introduction-to-signalr.md#transports).

<a id="getproxy"></a>

## <a name="how-to-get-a-proxy-for-a-hub-class"></a>Jak uzyskać serwer proxy dla klasy Centrum

Każdy obiekt połączenia utworzonego hermetyzuje informacje dotyczące połączenia do usługi SignalR, która zawiera co najmniej jednej klasy koncentratora. Aby komunikować się z klasy koncentratora, należy użyć obiekt serwera proxy samodzielnie utworzony (Jeśli nie używasz wygenerowany serwer proxy) lub która zostanie wygenerowany.

Na komputerze klienckim nazwa serwera proxy jest wersja formatu — z uwzględnieniem wielkości liter nazwy klasy koncentratora. SignalR automatycznie sprawia, że ta zmiana tak, aby kod JavaScript można są zgodne z konwencjami JavaScript.

**Klasy koncentratora na serwerze**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample23.cs?highlight=1)]

**Pobierz odwołanie do wygenerowanego klienta serwera proxy dla Centrum**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample24.js?highlight=1)]

**Utwórz serwer proxy klienta dla klasy koncentratora (bez wygenerowany serwer proxy)**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample25.cs?highlight=1)]

Jeśli dekoracji klasy koncentratora z `HubName` atrybutu, użyj dokładną nazwę bez Zmienianie wielkości liter.

**Klasy koncentratora na serwerze z atrybutem HubName**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample26.cs?highlight=1)]

**Pobierz odwołanie do wygenerowanego klienta serwera proxy dla Centrum**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample27.js?highlight=1)]

**Utwórz serwer proxy klienta dla klasy koncentratora (bez wygenerowany serwer proxy)**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample28.cs?highlight=1)]

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a>Sposób definiowania metod na komputerze klienckim, który można wywołać serwera

Aby zdefiniować metody, która serwera można wywołać z koncentratorem, Dodaj program obsługi zdarzeń do serwera proxy koncentratora za pomocą `client` właściwości wygenerowanego serwera proxy lub wywołanie `on` metodę, jeśli nie używasz wygenerowany serwer proxy. Parametry mogą być obiektów złożonych.

Dodawanie obsługi zdarzeń przed wywołaniem `start` metody do nawiązania połączenia. (Jeśli chcesz dodać obsługę zdarzeń po wywołaniu `start` metody, zobacz sekcję Uwaga w [sposobu ustanawiania połączenia](#establishconnection) we wcześniejszej części tego dokumentu, a należy użyć składni wyświetlany w celu zdefiniowania metody bez użycia wygenerowany serwer proxy.)

Dopasowania nazwy metody jest rozróżniana wielkość liter. Na przykład `Clients.All.addContosoChatMessageToPage` będą wykonywane na serwerze `AddContosoChatMessageToPage`, `addContosoChatMessageToPage`, lub `addcontosochatmessagetopage` na kliencie.

**Zdefiniuj metodę na kliencie (z wygenerowany serwer proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample29.js?highlight=2)]

**Innym sposobem zdefiniować metody na kliencie (z wygenerowany serwer proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample30.js?highlight=1-2)]

**Zdefiniuj metodę na kliencie (bez wygenerowany serwer proxy lub podczas dodawania po wywołaniu metody rozpoczęcia)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample31.js?highlight=3)]

**Kod serwera, który wywołuje metodę klienta**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample32.cs?highlight=5)]

Poniższe przykłady obiekt złożony jako parametr metody.

**Zdefiniuj metodę na kliencie, który pobiera obiekt złożony (z wygenerowany serwer proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample33.js?highlight=2-3)]

**Zdefiniuj metodę na kliencie, który pobiera obiekt złożony (bez wygenerowany serwer proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample34.js?highlight=3-4)]

**Kod serwera, który definiuje obiekt złożony**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample35.cs?highlight=1)]

**Kod serwera, który wywołuje metodę klienta przy użyciu obiekt złożony**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample36.cs?highlight=3)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a>Sposób wywołania metody serwera z klienta

Aby wywołać metodę serwera od klienta, należy użyć `server` właściwości wygenerowany serwer proxy lub `invoke` metody dla serwera proxy koncentratora, jeśli nie używasz wygenerowany serwer proxy. Parametrów lub wartości zwracanej można obiektów złożonych.

Przekaż w wersji camelcase nazwę metody koncentratora. SignalR automatycznie sprawia, że ta zmiana tak, aby kod JavaScript można są zgodne z konwencjami JavaScript.

Poniższe przykłady przedstawiają sposób wywołania metody serwera, który nie ma wartości zwracanej i wywołać metodę serwera, który ma wartość zwracaną.

**Metoda serwera nie atrybutem HubMethodName**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample37.cs?highlight=3)]

**Parametr przekazany kod serwera, który definiuje obiekt złożony**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample38.cs)]

**Kod klienta, który wywołuje metodę serwer (z wygenerowany serwer proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample39.js?highlight=1)]

**Kod klienta, który wywołuje metodę serwera (bez wygenerowany serwer proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample40.js?highlight=1)]

Jeśli dekorowane metody koncentratora z `HubMethodName` atrybutu, użyj tej nazwy bez Zmienianie wielkości liter.

**Metoda serwera** z atrybutem HubMethodName

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample41.cs?highlight=3)]

**Kod klienta, który wywołuje metodę serwer (z wygenerowany serwer proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample42.js?highlight=1)]

**Kod klienta, który wywołuje metodę serwera (bez wygenerowany serwer proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample43.js?highlight=1)]

Powyższych przykładach pokazano, jak wywołać metodę serwera, który nie ma wartości zwracanych. Poniższe przykłady przedstawiają sposób wywołania metody serwera, która ma wartość zwracaną.

**Kod serwera dla metody, która zawiera wartości zwracanej**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample44.cs?highlight=3)]

**Klasy zapasów, używany do** zwracają wartość

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample45.cs?highlight=1)]

**Kod klienta, który wywołuje metodę serwer (z wygenerowany serwer proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample46.js?highlight=2,4-5)]

**Kod klienta, który wywołuje metodę serwera (bez wygenerowany serwer proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample47.js?highlight=2,4-5)]

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a>Sposób obsługi zdarzeń okres istnienia połączenia

Biblioteka SignalR udostępnia następujące połączenie okres istnienia zdarzeń, które może obsłużyć:

- `starting`: Wywoływane przed wszystkie dane są wysyłane za pośrednictwem połączenia.
- `received`: Zgłaszane w przypadku nieodebrania żadnych danych w połączeniu. Udostępnia odebranych danych.
- `connectionSlow`: Wywoływane, gdy klient wykryje wolne lub często porzucanie połączenie.
- `reconnecting`: Wywoływane, gdy transportu źródłowego rozpoczyna ponowne nawiązywanie połączenia.
- `reconnected`: Wywoływane, gdy podłączył transportu źródłowego.
- `stateChanged`: Wywoływane po zmianie stanu połączenia. Zawiera stan stary i nowy stan (łączenie, połączony, Reconnecting lub Rozłączono).
- `disconnected`: Wywoływane, gdy połączenie zostało rozłączone.

Na przykład, jeśli mają być wyświetlane ostrzeżenia, gdy istnieją problemy z połączeniem, które mogą powodować zauważalne opóźnienia, obsługi `connectionSlow` zdarzeń.

**Obsłuż zdarzenie connectionSlow (z wygenerowany serwer proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample48.js?highlight=1)]

**Obsłuż zdarzenie connectionSlow (bez wygenerowany serwer proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample49.js?highlight=2)]

Aby uzyskać więcej informacji, zobacz [zrozumienia i obsługi zdarzeń okres istnienia połączenia w SignalR](index.md).

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a>Sposób obsługi błędów

Udostępnia klienta SignalR JavaScript `error` zdarzeń, które można dodać program obsługi. Aby dodać obsługę błędów, które są wynikiem wywołania metody serwera umożliwia także metody kończyć się niepowodzeniem.

Jeśli nie włączysz jawnie szczegółowe komunikaty o błędach na serwerze, obiekt wyjątku, który zwraca SignalR po wystąpieniu błędu zawiera minimalne informacje o błędzie. Na przykład, jeśli wywołanie `newContosoChatMessage` zawiera komunikat o błędzie w obiekcie błąd kończy się niepowodzeniem, "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" Wysyłanie szczegółowe komunikaty o błędach do klientów w środowisku produkcyjnym nie jest zalecana ze względów bezpieczeństwa, ale jeśli chcesz włączyć szczegółowe komunikaty o błędach dla rozwiązywania problemów, należy użyć poniższego kodu na serwerze.

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample50.cs?highlight=2)]

Poniższy przykład przedstawia sposób dodawania obsługi dla zdarzenia błędu.

**Dodaj program obsługi błędów (z wygenerowany serwer proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample51.js?highlight=1)]

**Dodaj program obsługi błędów (bez wygenerowany serwer proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample52.js?highlight=2)]

Poniższy przykład przedstawia sposób obsługi błędu z wywołania metody.

**Dojście błąd z wywołania metody (z wygenerowany serwer proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample53.js?highlight=2)]

**Dojście błąd z wywołania metody (bez wygenerowany serwer proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample54.js?highlight=2)]

W przypadku niepowodzenia wywołania metody `error` również zdarzenia, więc kodu w `error` obsługi metody i w `.fail` metody wywołania zwrotnego jest wykonywany.

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a>Jak włączyć rejestrowanie klienta

Aby włączyć rejestrowanie klienta połączenia, ustaw `logging` właściwości w obiekcie połączenia przed wywołaniem `start` metody do nawiązania połączenia.

**Włącz rejestrowanie (z wygenerowany serwer proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample55.js?highlight=1)]

**Włącz rejestrowanie (bez wygenerowany serwer proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample56.js?highlight=2)]

Aby wyświetlić dzienniki, Otwórz narzędzia dla deweloperów w przeglądarce i przejdź do karty konsoli. Samouczek, który zawiera instrukcje krok po kroku i ekran zrzuty, które pokazują, jak to zrobić, zobacz [serwera emisji z ASP.NET Signalr - Włącz rejestrowanie](index.md).
