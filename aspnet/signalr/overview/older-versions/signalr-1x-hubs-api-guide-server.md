---
uid: signalr/overview/older-versions/signalr-1x-hubs-api-guide-server
title: "Podręcznik interfejsu API koncentratorów SignalR platformy ASP.NET — serwera (SignalR 1.x) | Dokumentacja firmy Microsoft"
author: pfletcher
description: "Ten dokument zawiera wprowadzenie do programowania po stronie serwera interfejsu API koncentratorów SignalR platformy ASP.NET SignalR w wersji 1.1, z demonstratin przykłady kodu..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/17/2013
ms.topic: article
ms.assetid: 03e4b9f5-0fea-4d94-959f-014b2762a301
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/signalr-1x-hubs-api-guide-server
msc.type: authoredcontent
ms.openlocfilehash: e594dd1ea4ae027cf0b82574fc5a3eb061b1f2e1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-signalr-hubs-api-guide---server-signalr-1x"></a>Podręcznik interfejsu API koncentratorów SignalR platformy ASP.NET — serwera (SignalR 1.x)
====================
przez [Patrick Fletcher](https://github.com/pfletcher), [Dykstra niestandardowy](https://github.com/tdykstra)

> Ten dokument zawiera wprowadzenie do programowania po stronie serwera interfejsu API koncentratorów SignalR platformy ASP.NET dla biblioteki SignalR w wersji 1.1, z przykładów kodu prezentacja typowe opcje.
> 
> Interfejs API koncentratorów SignalR umożliwia upewnij zdalnych wywołań procedur (RPC) z serwera do połączonych klientów i klientów na serwerze. W kodzie serwera zdefiniuj metody, które mogą być wywoływane przez klientów i wywołania metod, które są uruchamiane na komputerze klienckim. W kodu klienta należy zdefiniować metody, które mogą być wywoływane z serwera i wywołania metody, które są uruchamiane na serwerze. SignalR odpowiada on za wszystkie żmudne procesy klient serwer dla Ciebie.
> 
> SignalR udostępnia również interfejs API niższego poziomu o nazwie połączeń trwałych. Wprowadzenie do SignalR, koncentratorach i połączeń trwałych lub Samouczek przedstawiający sposób tworzenia pełnej aplikacji SignalR, patrz [SignalR — wprowadzenie](index.md).


## <a name="overview"></a>Omówienie

Ten dokument zawiera następujące sekcje:

- [Jak zarejestrować trasy SignalR i skonfigurować opcje SignalR](#route)

    - [Adres URL /signalr](#signalrurl)
    - [Konfigurowanie opcji SignalR](#options)
- [Jak utworzyć i używać klas elementu Hub](#hubclass)

    - [Okres istnienia obiektu Centrum](#transience)
    - [Wielkość liter formatu Centrum nazw klientów języka JavaScript](#hubnames)
    - [Wiele centrów](#multiplehubs)
- [Sposób definiowania metod w klasie koncentratora, której klienci mogą wywoływać](#hubmethods)

    - [Wielkość liter formatu nazw klientów języka JavaScript — metoda](#methodnames)
    - [Kiedy asynchroniczne](#asyncmethods)
    - [Definiowanie przeciążenia](#overloads)
- [Wywoływanie metody z klasy koncentratora klienta](#callfromhub)

    - [Wybieranie których klienci otrzymają RPC](#selectingclients)
    - [Weryfikacja nie kompilacji dla nazw — metoda](#dynamicmethodnames)
    - [Dopasowywanie nazw bez uwzględniania wielkości liter — metoda](#caseinsensitive)
    - [Wykonanie asynchroniczne](#asyncclient)
- [Jak zarządzać członkostwa w grupie z klasy koncentratora](#groupsfromhub)

    - [Asynchroniczne wykonywanie metod Add i Remove](#asyncgroupmethods)
    - [Trwałość członkostwa grupy](#grouppersistence)
    - [Grupy jednego użytkownika](#singleusergroups)
- [Sposób obsługi zdarzeń okres istnienia połączenia w klasie Centrum](#connectionlifetime)

    - [Kiedy są wywoływane OnConnected ondisconnected elementu i onreconnected elementu](#onreconnected)
    - [Stan elementu wywołującego pusta](#nocallerstate)
- [Jak uzyskać informacji o kliencie z właściwości kontekstu](#contextproperty)
- [Sposób przekazywania stan między klientami a klasy koncentratora](#passstate)
- [Sposób obsługi błędów w klasie Centrum](#handleErrors)
- [Jak wywoływać metod klienta i zarządzanie grupami z poza klasą Centrum](#callfromoutsidehub)

    - [Wywoływanie metody klienta](#callingclientsoutsidehub)
    - [Członkostwo w grupie zarządzania](#managinggroupsoutsidehub)
- [Jak włączyć śledzenie](#tracing)
- [Dostosowywanie potoku koncentratory](#hubpipeline)

Dokumentacja na temat programu klientów zobacz następujące zasoby:

- [Podręcznik interfejsu API koncentratorów SignalR — JavaScript klienta](index.md)
- [Podręcznik interfejsu API koncentratorów SignalR - klienta .NET](index.md)

Linki do tematów dokumentacji interfejsu API są wersja platformy .NET 4.5 interfejsu API. Jeśli używasz programu .NET 4, zobacz [wersji .NET 4 tematy interfejsu API](https://msdn.microsoft.com/en-us/library/jj891075(v=vs.100).aspx).

<a id="route"></a>

## <a name="how-to-register-the-signalr-route-and-configure-signalr-options"></a>Jak zarejestrować trasy SignalR i skonfigurować opcje SignalR

Aby zdefiniować trasy, którego klienci będą używać do nawiązania połączenia z koncentratorem, należy wywołać [MapHubs](https://msdn.microsoft.com/en-us/library/system.web.routing.signalrrouteextensions.maphubs(v=vs.111).aspx) metody podczas uruchamiania aplikacji. `MapHubs`jest [— metoda rozszerzenia](https://msdn.microsoft.com/en-us/library/vstudio/bb383977.aspx) dla `System.Web.Routing.RouteCollection` klasy. Poniższy przykład przedstawia sposób definiowania trasy koncentratory SignalR w *Global.asax* pliku.

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample1.cs)]

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample2.cs?highlight=5)]

Jeśli dodajesz SignalR funkcje do aplikacji platformy ASP.NET MVC, upewnij się, że trasy SignalR został dodany przed innych tras. Aby uzyskać więcej informacji, zobacz [samouczek: wprowadzenie do korzystania z SignalR i MVC 4](index.md).

<a id="signalrurl"></a>

### <a name="the-signalr-url"></a>Adres URL /signalr

Domyślnie jest adres URL trasy, którego klienci będą używać do nawiązania połączenia z koncentratorem "/ signalr". (Nie należy mylić ten adres URL z adresu URL "/ signalr/hubs", który jest automatycznie wygenerowanego pliku JavaScript. Aby uzyskać więcej informacji na temat wygenerowany serwer proxy, zobacz [Podręcznik interfejsu API koncentratorów SignalR - JavaScript Client - wygenerowany serwer proxy i jakie operacje dla Ciebie](index.md).)

Może to być nadzwyczajne okoliczności powodujących, że ten podstawowy adres URL nie można używać dla biblioteki SignalR; na przykład istnieje folder projektu o nazwie *signalr* i nie chcesz zmienić nazwę. W takim przypadku można zmienić podstawowy adres URL, jak pokazano w poniższych przykładach (Zastąp "/ signalr" w przykładowym kodzie z żądany adres URL).

**Kod serwera, który określa adres URL**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample3.cs?highlight=1)]

**Kod JavaScript klienta, który określa adres URL (z wygenerowany serwer proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample4.js?highlight=1)]

**Kod JavaScript klienta, który określa adres URL (bez wygenerowany serwer proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample5.js?highlight=1)]

**Kod klienta .NET, który określa adres URL**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample6.cs?highlight=1)]

<a id="options"></a>

### <a name="configuring-signalr-options"></a>Konfigurowanie opcji SignalR

Overloads z `MapHubs` metody pozwalają określić niestandardowy adres URL, moduł rozpoznawania zależności niestandardowych i następujących opcji:

- Włącz połączenia między domenami z klientów przeglądarek.

    Zwykle Jeśli przeglądarka ładuje strony z `http://contoso.com`, połączenia SignalR jest w tej samej domenie, w `http://contoso.com/signalr`. Jeśli strona z `http://contoso.com` nawiązuje połączenie `http://fabrikam.com/signalr`, czyli połączenia między domenami. Ze względów bezpieczeństwa połączeń między domenami są domyślnie wyłączone. Aby uzyskać więcej informacji, zobacz [ASP.NET SignalR koncentratory interfejsu API przewodnik - JavaScript Client - jak nawiązać połączenie między domenami](index.md).
- Włącz szczegółowe komunikaty o błędach.

    Jeśli wystąpią błędy, domyślne zachowanie SignalR ma wysłać do klientów komunikatu powiadomienia bez szczegółowe informacje o co się stało. Wysyłanie szczegółowe informacje o błędzie do klientów nie jest zalecane w środowisku produkcyjnym, ponieważ złośliwi użytkownicy mogą mieć możliwość skorzystaj z informacji w ataków na aplikację. Do rozwiązywania problemów, służy tę opcję, aby włączyć raportowanie błędów więcej informacji.
- Wyłączenie automatycznie generowanych plików proxy JavaScript.

    Domyślnie plik JavaScript z serwerów proxy dla Twojej klas elementu Hub jest generowany w odpowiedzi na adres URL "/ signalr/hubs". Jeśli nie chcesz używać serwery proxy JavaScript, lub jeśli chcesz wygenerować ten plik ręcznie i odwoływać się do fizycznego pliku w klientach, służy tę opcję, aby wyłączyć generowanie serwera proxy. Aby uzyskać więcej informacji, zobacz [Podręcznik interfejsu API koncentratorów SignalR - JavaScript klient - serwer proxy generowany sposobu tworzenia pliku fizycznego dla elementu SignalR](index.md).

Poniższy przykład przedstawia sposób określić te opcje i adres URL połączenia SignalR w wywołaniu `MapHubs` metody. Aby określić niestandardowy adres URL, zastąp "/ signalr" w przykładzie z adresem URL, którego chcesz używać.

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample7.cs)]

<a id="hubclass"></a>

## <a name="how-to-create-and-use-hub-classes"></a>Jak utworzyć i używać klas elementu Hub

Koncentrator, utworzyć klasę, która jest pochodną [Microsoft.Aspnet.Signalr.Hub](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx). W poniższym przykładzie przedstawiono prostą klasę Centrum dla aplikacji czatu.

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample8.cs)]

W tym przykładzie można wywołać połączony klient `NewContosoChatMessage` metody, i tak, aby wszyscy połączeni klienci emitowania odebrane dane.

<a id="transience"></a>

### <a name="hub-object-lifetime"></a>Okres istnienia obiektu Centrum

Nie utworzyć wystąpienia klasy koncentratora lub wywołać metod w kodzie na serwerze; wszystko jest realizowane automatycznie przez potoku koncentratory SignalR. SignalR tworzy nowe wystąpienie klasy koncentratora każdej musi do obsługi operacji koncentratora, takich jak gdy klient nawiąże połączenie, rozłącza lub sprawia, że wywołanie metody do serwera.

Ponieważ wystąpień klasy koncentratora są przejściowe, nie możesz użyć ich do zarządzania stanem z jedną metodę wywołania do następnego. Zawsze serwer odbiera wywołanie metody od klienta, nowe wystąpienie klasy procesów Centrum wiadomości. Aby zachować stan za pośrednictwem wiele połączeń i wywołania metody, przy użyciu innej metody, takie jak bazy danych lub statyczna zmienna na klasy koncentratora lub inną klasę, która nie pochodzi od `Hub`. Jeśli będą się powtarzać dane w pamięci, przy użyciu metody takie jak zmienna statyczna klasy koncentratora, dane zostaną utracone podczas odtwarzania domen aplikacji.

Jeśli chcesz wysyłać do klientów z własnego kodu uruchomioną poza klasą Centrum nie można go przez utworzenie wystąpienia klasy wystąpienie koncentratora, ale możesz zrobić to poprzez odwołanie do obiektu context z SignalR dla klasy koncentratora. Aby uzyskać więcej informacji, zobacz [jak wywoływać metod klienta i zarządzanie grupami z poza klasą Centrum](#callfromoutsidehub) dalszej części tego tematu.

<a id="hubnames"></a>

### <a name="camel-casing-of-hub-names-in-javascript-clients"></a>Wielkość liter formatu Centrum nazw klientów języka JavaScript

Domyślnie klientów języka JavaScript odwoływać się do koncentratorów, za pomocą wersji formatu — z uwzględnieniem wielkości liter nazwy klasy. SignalR automatycznie sprawia, że ta zmiana tak, aby kod JavaScript można są zgodne z konwencjami JavaScript. Poprzedni przykład, czy określone jako `contosoChatHub` w kodzie JavaScript.

**Serwer**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample9.cs?highlight=1)]

**JavaScript klienta przy użyciu wygenerowanego serwera proxy**

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample10.js?highlight=1)]

Jeśli chcesz określić inną nazwę dla klientów do używania, Dodaj `HubName` atrybutu. Jeśli używasz `HubName` atrybutu, nie ulega zmianie nazwę na format camelcase na klientów języka JavaScript.

**Serwer**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample11.cs?highlight=1)]

**JavaScript klienta przy użyciu wygenerowanego serwera proxy**

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample12.js?highlight=1)]

<a id="multiplehubs"></a>

### <a name="multiple-hubs"></a>Wiele centrów

Można zdefiniować wiele klas elementu Hub w aplikacji. Po wykonaniu tej czynności, połączenie jest udostępnione, ale grup są oddzielone:

- Wszyscy klienci będą używać tego samego adresu URL nawiązać połączenia z usługą SignalR ("/ signalr" lub adres URL niestandardowej, jeśli została określona), i czy połączenie jest używany przez wszystkie koncentratory zdefiniowane przez usługę.

    Nie ma żadnej różnicy wydajności dla wielu koncentratorów w porównaniu do definiowania wszystkie funkcje Centrum w jednej klasy.
- Wszystkie koncentratory uzyskać informacje o tej samej żądania HTTP.

    Ponieważ wszystkie koncentratory mają tego samego połączenia, tylko informacje żądania HTTP, które pobiera serwera jest, co polega na oryginalnego żądania HTTP, który nawiązuje połączenia SignalR. Jeśli używasz żądanie połączenia do przekazywania informacji z klienta do serwera, określając ciąg zapytania nie może dostarczyć różne ciągi zapytania do różnych centrów. Wszystkie koncentratory otrzyma tych samych informacji.
- Wygenerowany plik serwery proxy JavaScript będzie zawierać serwerów proxy dla wszystkich koncentratorów w jednym pliku.

    Aby uzyskać informacje o serwery proxy JavaScript, zobacz [Podręcznik interfejsu API koncentratorów SignalR - JavaScript Client - wygenerowany serwer proxy i jakie operacje dla Ciebie](index.md).
- Grupy są definiowane w koncentratorów.

    W SignalR, którą można zdefiniować nazwę grupy w celu emisji podzbiorowi klientów połączonych. Grupy są obsługiwane oddzielnie dla każdej koncentratora. Na przykład grupa o nazwie "Administratorzy" to jeden zestaw klientów z `ContosoChatHub` klasy i tej samej nazwy grupy może odwoływać się do innej grupy klientów dla Twojego `StockTickerHub` klasy.

<a id="hubmethods"></a>

## <a name="how-to-define-methods-in-the-hub-class-that-clients-can-call"></a>Sposób definiowania metod w klasie koncentratora, której klienci mogą wywoływać

Aby aktywować metody koncentratora, który ma zostać wywołane z klienta, należy zadeklarować publiczną metodę, jak pokazano w poniższych przykładach.

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample13.cs?highlight=3)]

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample14.cs?highlight=3)]

Można określić zwracany typ i parametry, łącznie z typami złożonymi i tablic, tak jak w dowolnej metody C#. Wszystkie dane, które zostanie wyświetlony w parametrach lub zwróć się do obiektu wywołującego przesyłane między klientem a serwerem przy użyciu formatu JSON, a SignalR obsługuje automatycznie powiązania złożone obiekty i tablice obiektów.

<a id="methodnames"></a>

### <a name="camel-casing-of-method-names-in-javascript-clients"></a>Wielkość liter formatu nazw klientów języka JavaScript — metoda

Domyślnie klienci JavaScript odwołanie do metod koncentratora za pomocą wersji formatu — z uwzględnieniem wielkości liter w nazwie metody. SignalR automatycznie sprawia, że ta zmiana tak, aby kod JavaScript można są zgodne z konwencjami JavaScript.

**Serwer**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample15.cs?highlight=1)]

**JavaScript klienta przy użyciu wygenerowanego serwera proxy**

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample16.js?highlight=1)]

Jeśli chcesz określić inną nazwę dla klientów do używania, Dodaj `HubMethodName` atrybutu.

**Serwer**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample17.cs?highlight=1)]

**JavaScript klienta przy użyciu wygenerowanego serwera proxy**

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample18.js?highlight=1)]

<a id="asyncmethods"></a>

### <a name="when-to-execute-asynchronously"></a>Kiedy asynchroniczne

Jeśli metoda będzie można długotrwałe lub ma pracę który będzie obejmują oczekujące, takich jak wyszukiwania w bazie danych lub wywołania usługi sieci web należy metody koncentratora asynchroniczne zwracając [zadań](https://msdn.microsoft.com/en-us/library/system.threading.tasks.task.aspx) (zamiast `void` zwracać) lub [ Zadanie&lt;T&gt; ](https://msdn.microsoft.com/en-us/library/dd321424.aspx) obiektu (zamiast `T` zwracany typ). Po powrocie `Task` obiektu z metody SignalR czeka na `Task` aby zakończyć, a następnie wysyła bez otoki wynik do klienta, więc nie ma żadnej różnicy w sposób code wywołania metody w kliencie.

Tworzenie metody koncentratora asynchroniczne pozwala uniknąć blokuje połączenia, gdy używa transportu protokołu WebSocket. Gdy metody koncentratora wykonuje synchronicznie i transport jest protokołu WebSocket, kolejne wywołania metody koncentratora od tego samego klienta są zablokowane, dopiero po zakończeniu metody koncentratora.

W poniższym przykładzie przedstawiono tej samej metody kodowanych, aby były uruchamiane synchronicznie lub asynchronicznie, a następnie kod JavaScript klienta, która działa w przypadku wywoływania danej wersji.

**Synchroniczne**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample19.cs)]

**Asynchroniczne — program ASP.NET 4.5**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample20.cs?highlight=1,7-8)]

**JavaScript klienta przy użyciu wygenerowanego serwera proxy**

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample21.js)]

Aby uzyskać więcej informacji o sposobie używania metod asynchronicznych w programie ASP.NET 4.5, zobacz [przy użyciu metod asynchronicznych w technologii ASP.NET MVC 4](../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md).

<a id="overloads"></a>

### <a name="defining-overloads"></a>Definiowanie przeciążenia

Jeśli chcesz zdefiniować przeciążenia metody, liczba parametrów w każdym przeciążenia musi być różne. W przypadku przeciążenia jest rozróżnianie właśnie, określając różne typy parametrów, klasy koncentratora zostanie skompilowany, ale usługi SignalR spowoduje zgłoszenie wyjątku w czasie wykonywania, gdy klienci spróbują do wywołania, jednego z przeciążeń.

<a id="callfromhub"></a>

## <a name="how-to-call-client-methods-from-the-hub-class"></a>Wywoływanie metody z klasy koncentratora klienta

Aby wywołać metody klienta, z serwera, należy użyć `Clients` właściwość w metodę w klasie koncentratora. W poniższym przykładzie pokazano kod serwera, który wywołuje `addNewMessageToPage` na wszystkich połączonych klientów i kod klienta, który definiuje metodę w kliencie JavaScript.

**Serwer**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample22.cs?highlight=5)]

**JavaScript klienta przy użyciu wygenerowanego serwera proxy**

[!code-html[Main](signalr-1x-hubs-api-guide-server/samples/sample23.html?highlight=1)]

Nie można pobrać wartość zwracaną z metody klienta; Składnia, takich jak `int x = Clients.All.add(1,1)` nie działa.

Można określić typy złożone i tablic parametrów. Poniższy przykład przekazuje typu złożonego do klienta w parametru metody.

**Kod serwera, która wywołuje metodę klienta przy użyciu obiekt złożony**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample24.cs?highlight=3)]

**Kod serwera, który definiuje obiekt złożony**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample25.cs?highlight=1)]

**JavaScript klienta przy użyciu wygenerowanego serwera proxy**

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample26.js?highlight=2-3)]

<a id="selectingclients"></a>

### <a name="selecting-which-clients-will-receive-the-rpc"></a>Wybieranie których klienci otrzymają RPC

Zwraca właściwości klientów [HubConnectionContext](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx) obiekt, który zapewnia kilka opcji określenie, którzy klienci otrzymają RPC:

- Wszyscy połączeni klienci.

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample27.cs)]
- Tylko klienta wywołującego.

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample28.cs)]
- Wszyscy klienci oprócz klienta wywołującego.

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample29.cs)]
- Określonego klienta identyfikowany przez identyfikator połączenia.

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample30.css)]

    W tym przykładzie wywołuje `addContosoChatMessageToPage` na klienta wywołującego i działa tak samo jak przy użyciu `Clients.Caller`.
- Wszyscy połączeni klienci oprócz określonych klientów, identyfikowany przez identyfikator połączenia.

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample31.cs)]
- Wszyscy połączeni klienci w określonej grupie.

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample32.css)]
- Wszyscy połączeni klienci w określonej grupie oprócz określonych klientów, identyfikowany przez identyfikator połączenia.

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample33.cs)]
- Wszyscy połączeni klienci w określonej grupie oprócz klienta wywołującego.

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample34.css)]

<a id="dynamicmethodnames"></a>

### <a name="no-compile-time-validation-for-method-names"></a>Weryfikacja nie kompilacji dla nazw — metoda

Określona nazwa metody jest interpretowany jako obiekt dynamiczny, co oznacza, że nie istnieje IntelliSense lub weryfikacji kompilacji. Wyrażenie jest obliczane w czasie wykonywania. Podczas wywołania metody, SignalR wysyła nazwę metody i wartości parametrów do klienta, a jeśli klient ma metody odpowiadającej nazwie, że metoda jest wywoływana i wartości parametrów są przekazywane do niego. Jeśli odpowiadającej metody znajduje się na komputerze klienckim, nie błąd. Aby uzyskać informacje o formacie dane, które SignalR przesyła do klienta w tle podczas wywoływania metody klienta, zobacz [wprowadzenie do SignalR](index.md).

<a id="caseinsensitive"></a>

### <a name="case-insensitive-method-name-matching"></a>Dopasowywanie nazw bez uwzględniania wielkości liter — metoda

Dopasowania nazwy metody jest rozróżniana wielkość liter. Na przykład `Clients.All.addContosoChatMessageToPage` będą wykonywane na serwerze `AddContosoChatMessageToPage`, `addcontosochatmessagetopage`, lub `addContosoChatMessageToPage` na kliencie.

<a id="asyncclient"></a>

### <a name="asynchronous-execution"></a>Wykonanie asynchroniczne

Asynchronicznie wykonuje metodę, która jest wywoływana. Wszelki kod, który jest dostarczany po wywołaniu metody klientowi wykona natychmiast bez oczekiwania na SignalR na zakończenie przesyłania danych do klientów, chyba że zostanie określony, że kolejnych wierszy kodu powinna czekać na ukończenie — metoda. W poniższych przykładach kodu przedstawiają sposób wykonania dwóch metod klienta po kolei, jeden przy użyciu kod, który działa w programie .NET 4.5, a drugi przy użyciu kod, który działa w .NET 4.

**Przykład .NET 4.5**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample35.cs?highlight=1,3)]

**Przykład .NET 4**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample36.cs?highlight=3-4)]

Jeśli używasz `await` lub `ContinueWith` do poczekaj na zakończenie metodę klienta, przed wykonaniem następnego wiersza kodu, nie oznacza że klienci faktycznie zostanie wyświetlony komunikat, przed wykonaniem następnego wiersza kodu. Tylko klienta wywołania metody "zakończenia" oznacza, że SignalR przeprowadził wszystko, co jest niezbędne do wysyłania wiadomości. Potrzebujesz weryfikacji, że klienci komunikat, masz program mechanizmu samodzielnie. Na przykład można kodu `MessageReceived` metody koncentratora, w `addContosoChatMessageToPage` metody na kliencie może wywołać `MessageReceived` po wykonaniu niezależnie od przypadku pracy należy wykonać na kliencie. W `MessageReceived` koncentratora pozwala wykonywać pracę zależy od klienta faktycznie odbioru i przetwarzania oryginalnego wywołania metody.

### <a name="how-to-use-a-string-variable-as-the-method-name"></a>Jak używać zmiennej ciągu jako nazwy — metoda

Aby wywołać metodę klienta za pomocą zmiennej ciągu jako nazwy metody rzutowania `Clients.All` (lub `Clients.Others`, `Clients.Caller`, itd.) do `IClientProxy` , a następnie wywołać [Invoke (methodName, argumenty...) ](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.hubs.iclientproxy.invoke(v=vs.111).aspx).

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample37.cs)]

<a id="groupsfromhub"></a>

## <a name="how-to-manage-group-membership-from-the-hub-class"></a>Jak zarządzać członkostwa w grupie z klasy koncentratora

Grupy w SignalR udostępnia metody emisji wiadomości do określonego podzbiór połączonych klientów. Grupa może zawierać dowolną liczbę klientów, a klient może być członkiem dowolnej liczby grup.

Aby zarządzać członkostwa w grupie, należy użyć [Dodaj](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) i [Usuń](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) metody udostępniane przez `Groups` właściwość klasy koncentratora. W poniższym przykładzie przedstawiono `Groups.Add` i `Groups.Remove` metody używane w metodach koncentratora, które są wywoływane przez kod klienta, a następnie kod JavaScript klienta, który je wywołuje.

**Serwer**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample38.cs?highlight=5,10)]

**JavaScript klienta przy użyciu wygenerowanego serwera proxy**

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample39.js)]

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample40.js)]

Nie trzeba jawnie tworzyć grupy. Obowiązująca grupy jest tworzony automatycznie przy pierwszym określić jego nazwę w wywołaniu `Groups.Add`, i zostaje usunięte po usunięciu ostatniego połączenia z członkostwa w nim.

Brak żadnego interfejsu API do pobierania listy członkostwa grupy lub grup. SignalR wysyła wiadomości do klientów i grup na podstawie [modelu pub/sub](http://en.wikipedia.org/wiki/Publish/subscribe), i nie przechowuje list grup lub członkostwa w grupach. Dzięki temu można zmaksymalizować skalowalność, ponieważ po dodaniu węzła do farmy sieci web SignalR zachowuje stan ma być propagowane do nowego węzła.

<a id="asyncgroupmethods"></a>

### <a name="asynchronous-execution-of-add-and-remove-methods"></a>Asynchroniczne wykonywanie metod Add i Remove

`Groups.Add` i `Groups.Remove` metod wykonania asynchronicznie. Jeśli chcesz dodać do grupy klienta i natychmiast Wyślij wiadomość do klienta przy użyciu grupy, należy upewnić się, że `Groups.Add` metoda zakończy się najpierw. W poniższych przykładach kodu pokazano, jak to zrobić, za pomocą kodu, który działa w .NET 4.5 i za pomocą kodu, który działa w .NET 4

**Przykład .NET 4.5**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample41.cs?highlight=1,3)]

**Przykład .NET 4**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample42.cs?highlight=3-4)]

<a id="grouppersistence"></a>

### <a name="group-membership-persistence"></a>Trwałość członkostwa grupy

SignalR śledzi połączenia, nie użytkowników, więc jeśli chcesz, aby użytkownik musiał być w tej samej grupie za każdym razem, gdy użytkownik nawiązuje połączenie, należy wywołać `Groups.Add` za każdym razem, gdy użytkownik ustanawia nowego połączenia.

Po do tymczasowej utraty łączności czasami SignalR można przywrócić połączenia automatycznie. W takim przypadku SignalR przywraca tego samego połączenia, bez możliwości tworzenia nowego połączenia, a więc członkostwa grupy klienta zostanie automatycznie przywrócony. Jest to możliwe nawet po tymczasowego podziału jest wynikiem ponownym uruchomieniu serwera lub niepowodzenie, ponieważ stan połączenia dla każdego klienta, w tym członkostwa w grupach, jest zwrotnego do klienta. Jeśli serwer ulegnie awarii, zostało zastąpione przez nowy serwer, zanim upłynie limit czasu połączenia klienta można automatycznie ponownie na nowym serwerze i ponownie zarejestrować się w grupach, które jest członkiem.

Gdy połączenia nie mogą zostać przywrócone automatycznie po utracie łączności, lub po upływie limitu czasu połączenia lub gdy klient odłączy się (na przykład, gdy przeglądarka przechodzi do nowej strony), członkostwa w grupach zostaną utracone. Następnym razem, gdy użytkownik łączy będzie nowego połączenia. Aby zachować członkostwa w grupach, gdy ten sam użytkownik ustanawia nowego połączenia, aplikacja musi śledzić skojarzenia między użytkownikami i grupami, a następnie przywróć członkostwa w grupach za każdym razem użytkownik ustanawia nowego połączenia.

Aby uzyskać więcej informacji na temat połączeń i ponowne łączenie, zobacz [sposobu obsługi zdarzenia okresu istnienia połączenia w klasie Centrum](#connectionlifetime) dalszej części tego tematu.

<a id="singleusergroups"></a>

### <a name="single-user-groups"></a>Grupy jednego użytkownika

Aplikacje używające SignalR zwykle mają do śledzenia skojarzenia między użytkownikami i połączeń, aby dowiedzieć się, użytkownika, który wysłał komunikat i które użytkownicy powinni otrzymywać wiadomości. Grupy są używane w jednym z dwóch typowe wzorce dla operacją.

- Grupy pojedynczego użytkownika.

    Można określić nazwę użytkownika, jako nazwę grupy i Dodaj do grupy bieżący identyfikator połączenia, za każdym razem, gdy użytkownik nawiąże połączenie lub ponownie nawiąże połączenie. Do wysyłania wiadomości do użytkowników, możesz wysłać do grupy. Wadą tej metody jest, że grupa nie udostępniają sposób, aby sprawdzić, czy użytkownik jest w trybie online lub offline.
- Śledzenie skojarzenia między nazwami użytkowników i identyfikatorów połączeń.

    Można przechowywać skojarzenia między każdej nazwy użytkownika i co najmniej jednego połączenia identyfikatorów słownika lub bazy danych i zaktualizować przechowywanych danych za każdym razem, użytkownik połączeniu lub rozłączeniu. Aby wysłać wiadomości do użytkownika, należy określić identyfikatorów połączeń. Wadą tej metody jest, że ma więcej pamięci.

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events-in-the-hub-class"></a>Sposób obsługi zdarzeń okres istnienia połączenia w klasie Centrum

Typowe przyczyny na potrzeby obsługi zdarzeń okres istnienia połączenia są do śledzenia czy użytkownik jest połączony i nie i do śledzenia skojarzenie między nazwy użytkowników i identyfikatorów połączeń. Do uruchamiania własnego kodu, gdy klienci Połącz lub Rozłącz, Zastąp `OnConnected`, `OnDisconnected`, i `OnReconnected` metody wirtualne koncentratora klasy, jak pokazano w poniższym przykładzie.

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample43.cs?highlight=3,14,22)]

<a id="onreconnected"></a>

### <a name="when-onconnected-ondisconnected-and-onreconnected-are-called"></a>Kiedy są wywoływane OnConnected ondisconnected elementu i onreconnected elementu

Zawsze przeglądarką przechodzi do nowej strony, nowe połączenie ma należy ustalić, co oznacza, że zostanie wykonany SignalR `OnDisconnected` metody następuje `OnConnected` metody. Po nawiązaniu nowego połączenia SignalR zawsze tworzy nowy identyfikator połączenia.

`OnReconnected` Metoda jest wywoływana, gdy nastąpiła tymczasowe przerwy w łączności SignalR automatycznie odzyskiwanie, takich jak kabla jest tymczasowo odłączony po ponownym ustanowieniu połączenia, zanim upłynie limit czasu połączenia. `OnDisconnected` Metoda jest wywoływana, gdy klient został odłączony, a SignalR nie można automatycznie ponownie zestawione, takie jak kiedy przeglądarką przechodzi do nowej strony. W związku z tym jest możliwe sekwencji zdarzeń dla danego klienta `OnConnected`, `OnReconnected`, `OnDisconnected`; lub `OnConnected`, `OnDisconnected`. Sekwencja nie będą widzieć `OnConnected`, `OnDisconnected`, `OnReconnected` dla danego połączenia.

`OnDisconnected` — Metoda nie jest wywoływana w niektórych scenariuszach, na przykład jeśli serwer ulegnie uszkodzeniu lub pobiera odtwarzania domen aplikacji. Gdy inny serwer, który jest dostarczany w wierszu lub domena aplikacji zakończeniu jej odtworzenia, niektórzy klienci może istnieć możliwość Połącz się ponownie i wyzwalać `OnReconnected` zdarzeń.

Aby uzyskać więcej informacji, zobacz [zrozumienia i obsługi zdarzeń okres istnienia połączenia w SignalR](index.md).

<a id="nocallerstate"></a>

### <a name="caller-state-not-populated"></a>Stan elementu wywołującego pusta

Metody obsługi zdarzeń okres istnienia połączenia są nazywane z serwera, co oznacza, że należy umieścić w stan `state` obiektu na kliencie nie zostaną wypełnione w `Caller` właściwości na serwerze. Informacje o `state` obiektu i `Caller` właściwości, zobacz [sposób przekazywania stan między klientami a klasy koncentratora](#passstate) dalszej części tego tematu.

<a id="contextproperty"></a>

## <a name="how-to-get-information-about-the-client-from-the-context-property"></a>Jak uzyskać informacji o kliencie z właściwości kontekstu

Aby uzyskać informacje o kliencie, należy użyć `Context` właściwość klasy koncentratora. `Context` Zwraca [HubCallerContext](https://msdn.microsoft.com/en-us/library/jj890883(v=vs.111).aspx) obiektu, który zapewnia dostęp do następujących informacji:

- Identyfikator połączenia klienta wywołującego.

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample44.cs?highlight=1)]

    Identyfikator połączenia jest identyfikatorem GUID, który jest przypisywany przez SignalR (w swoim własnym kodem nie można określić wartości). Istnieje jeden identyfikator połączenia dla każdego połączenia i tego samego połączenia, którego identyfikator jest używany przez wszystkie koncentratory, jeśli masz wiele koncentratorów w aplikacji.
- Dane nagłówka HTTP.

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample45.cs?highlight=1)]

    Można także uzyskać nagłówki HTTP od `Context.Headers`. Przyczyna wiele odwołań do samo jest to, że `Context.Headers` został utworzony, `Context.Request` właściwość została dodana później, a `Context.Headers` został zachowany na potrzeby zgodności z poprzednimi wersjami.
- Zapytanie danych ciągu.

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample46.cs?highlight=1)]

    Można również uzyskać dane ciągu zapytania z `Context.QueryString`.

    Ciąg zapytania, który można pobrać tej właściwości jest ten, który został użyty w żądaniu HTTP, który nawiązaniu połączenia SignalR. Możesz dodać parametrów ciągu zapytania w kliencie, konfigurując połączenia, które stanowi wygodny sposób przekazywania danych o kliencie od klienta do serwera. Poniższy przykład przedstawia sposób dodanie ciągu zapytania w kliencie JavaScript, gdy używasz wygenerowany serwer proxy.

    [!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample47.js?highlight=1)]

    Aby uzyskać więcej informacji na temat ustawiania parametrów ciągu zapytania, zobacz przewodniki interfejsu API dla [JavaScript](index.md) i [.NET](index.md) klientów.

    Metoda transportu używaną dla połączenia danych ciąg zapytania, oraz inne wartości, używana wewnętrznie przez SignalR można znaleźć:

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample48.cs)]

    Wartość `transportMethod` będzie "Websocket", "serverSentEvents", "foreverFrame" lub "longPolling". Należy pamiętać, że jeśli wybierzesz tę wartość `OnConnected` metoda obsługi zdarzeń, w niektórych scenariuszach może wstępnie pobrać wartość transportu, która nie jest metodą końcowego wynegocjowanym transport dla połączenia. W takim przypadku metoda zgłosi wyjątek i zostanie wywołany ponownie później po nawiązaniu metodę końcowego transportu.
- Pliki cookie.

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample49.cs?highlight=1)]

    Można także uzyskać plików cookie z `Context.RequestCookies`.
- Informacje o użytkowniku.

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample50.cs?highlight=1)]
- Obiekt kontekstu HTTP dla żądania:

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample51.cs?highlight=1)]

    Użyj tej metody, zamiast pobierania `HttpContext.Current` uzyskanie `HttpContext` obiektu połączenia SignalR.

<a id="passstate"></a>

## <a name="how-to-pass-state-between-clients-and-the-hub-class"></a>Sposób przekazywania stan między klientami a klasy koncentratora

Serwer proxy klienta udostępnia `state` obiektu, w którym można przechowywać dane, które mają zostać przesłane do serwera przy każdym wywołaniu metody. Na serwerze można uzyskać dostępu do tych danych w `Clients.Caller` właściwości w metodach koncentratora, które są wywoływane przez klientów. `Clients.Caller` Właściwość jest pusta dla metody obsługi zdarzeń okres istnienia połączenia `OnConnected`, `OnDisconnected`, i `OnReconnected`.

Tworzenie lub aktualizowanie danych w `state` obiektu i `Clients.Caller` właściwości działa w obu kierunkach. Można aktualizować wartości na serwerze i są one przekazywane z powrotem do klienta.

Poniższy przykład przedstawia kod klienta JavaScript, który zapisuje stan w celu przesłania go do serwera z każdego wywołania metody.

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample52.js?highlight=1-2)]

Poniższy przykład przedstawia kod odpowiednik w klienta programu .NET.

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample53.cs?highlight=1-2)]

W klasie Centrum mogą uzyskiwać dostęp do tych danych w `Clients.Caller` właściwości. Poniższy przykład przedstawia kod, który pobiera stan określonego w poprzednim przykładzie.

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample54.cs?highlight=3-4)]

> [!NOTE]
> Ten mechanizm utrwalanie stanu nie jest przeznaczony dla dużych ilości danych, ponieważ wszystko, co należy umieścić w `state` lub `Clients.Caller` właściwość jest zwrotnego z każdego wywołania metody. Jest to przydatne w przypadku mniejszych elementy, takie jak nazwy użytkowników lub liczników.


<a id="handleErrors"></a>

## <a name="how-to-handle-errors-in-the-hub-class"></a>Sposób obsługi błędów w klasie Centrum

Do obsługi błędów, które występują w Centrum metody klasy, użyj jednej lub obu z następujących metod:

- Zawijanie kodu metody w bloków try-catch i dziennika obiekt wyjątku. Wyjątek na potrzeby debugowania można wysłać do klienta, ale zabezpieczeń powodów wysyłanie szczegółowych informacji do klientów w środowisku produkcyjnym nie jest zalecane.
- Utwórz moduł potoku koncentratory, który obsługuje [OnIncomingError](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.hubs.hubpipelinemodule.onincomingerror(v=vs.111).aspx) metody. W poniższym przykładzie przedstawiono modułu potoku, który rejestruje błędy, następuje kod w pliku Global.asax, który injects modułu z potokiem koncentratorów.

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample55.cs)]

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample56.cs?highlight=3)]

Aby uzyskać więcej informacji na temat modułów potoku koncentratora, zobacz [sposób dostosowywania procesu koncentratory](#hubpipeline) dalszej części tego tematu.

<a id="tracing"></a>

## <a name="how-to-enable-tracing"></a>Jak włączyć śledzenie

Aby włączyć śledzenie po stronie serwera, Dodaj system.diagnostics element do pliku Web.config, jak pokazano w poniższym przykładzie:

[!code-html[Main](signalr-1x-hubs-api-guide-server/samples/sample57.html?highlight=17-72)]

Po uruchomieniu aplikacji w programie Visual Studio można wyświetlić dzienniki w **dane wyjściowe** okna.

<a id="callfromoutsidehub"></a>

## <a name="how-to-call-client-methods-and-manage-groups-from-outside-the-hub-class"></a>Jak wywoływać metod klienta i zarządzanie grupami z poza klasą Centrum

Aby wywołać klienta metody z innej klasy niż klasa koncentratora, pobrać odwołanie do obiektu context z SignalR dla koncentratora i używać, aby wywoływać metod na kliencie lub Zarządzanie grupami.

Poniższy przykład `StockTicker` klasy pobiera obiekt kontekstu, zapisze go w wystąpieniu klasy przechowuje wystąpienia klasy statycznej właściwości i używa kontekstu z pojedyncze wystąpienie klasy do wywołania `updateStockPrice` metody na komputerach klienckich, które są podłączone do koncentratora o nazwie `StockTickerHub`.

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample58.cs?highlight=8,24)]

Jeśli musisz używać wielu godzin kontekstu w obiekcie długotrwałe, Pobierz odwołanie jednokrotnie i zapisz go zamiast pobierania go ponownie za każdym razem. Uzyskanie kontekstu raz zapewnia SignalR i wysyła wiadomości do klientów w tej samej kolejności, w której metody koncentratora upewnij klienta wywołania metody. Samouczek, który pokazuje, jak używać kontekstu SignalR dla koncentratora, zobacz [serwera emisji z ASP.NET SignalR](index.md).

<a id="callingclientsoutsidehub"></a>

### <a name="calling-client-methods"></a>Wywoływanie metody klienta

Można określić, którzy klienci otrzymają RPC, ale ma mniej opcji niż pod numerem klasy koncentratora. Przyczyną tego błędu jest to, czy kontekst nie jest skojarzony z określonego wywołania klienta, więc wszystkie metody wymagające wiedzy bieżący identyfikator połączenia, takich jak `Clients.Others`, lub `Clients.Caller`, lub `Clients.OthersInGroup`, nie są dostępne. Dostępne są następujące opcje:

- Wszyscy połączeni klienci.

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample59.cs)]
- Określonego klienta identyfikowany przez identyfikator połączenia.

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample60.css)]
- Wszyscy połączeni klienci oprócz określonych klientów, identyfikowany przez identyfikator połączenia.

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample61.cs)]
- Wszyscy połączeni klienci w określonej grupie.

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample62.css)]
- Wszystkich połączonych klientów należących do określonej grupy, z wyjątkiem określonych klientach, identyfikowany przez identyfikator połączenia.

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample63.cs)]

Jeśli dzwonisz w klasie z systemem innym niż Centrum metody w klasie koncentratora, można przekazywać w bieżący identyfikator połączenia i użyj jej z `Clients.Client`, `Clients.AllExcept`, lub `Clients.Group` do symulowania `Clients.Caller`, `Clients.Others`, lub `Clients.OthersInGroup`. W poniższym przykładzie `MoveShapeHub` klasy przekazuje identyfikator połączenia, aby `Broadcaster` klasy, aby `Broadcaster` klasy można symulować `Clients.Others`.

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample64.cs?highlight=12,36)]

<a id="managinggroupsoutsidehub"></a>

### <a name="managing-group-membership"></a>Członkostwo w grupie zarządzania

Do zarządzania grupami mają te same opcje tak jak w klasie koncentratora.

- Dodawanie klienta do grupy

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample65.cs)]
- Usunięcie klienta z grupy

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample66.css)]

<a id="hubpipeline"></a>

## <a name="how-to-customize-the-hubs-pipeline"></a>Dostosowywanie potoku koncentratory

SignalR pozwala na iniekcję własny kod do potoku koncentratora. W poniższym przykładzie przedstawiono niestandardowego modułu potoku koncentratora zaloguje się każdego przychodzącego wywołania metody otrzymała od klienta i wychodzących wywołanie metody wywoływane na komputerze klienckim:

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample67.cs)]

Poniższy kod w *Global.asax* pliku rejestruje modułu do działania w potoku koncentratora:

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample68.cs?highlight=3)]

Istnieje wiele różnych metod, które można zastąpić. Aby uzyskać pełną listę, zobacz [metody HubPipelineModule](https://msdn.microsoft.com/en-us/library/jj918633(v=vs.111).aspx).
