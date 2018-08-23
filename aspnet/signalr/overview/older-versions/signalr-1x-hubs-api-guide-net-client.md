---
uid: signalr/overview/older-versions/signalr-1x-hubs-api-guide-net-client
title: Podręcznik interfejsu API centrów SignalR platformy ASP.NET — klient modelu .NET (SignalR 1.x) | Dokumentacja firmy Microsoft
author: pfletcher
description: Ten dokument zawiera wprowadzenie do korzystania z interfejsu API centrów dla elementu SignalR w wersji 2 w przypadku klientów programu .NET, takich jak Windows Store (WinRT), WPF, Silverlight i wad...
ms.author: riande
ms.date: 04/17/2013
ms.assetid: c334adc3-d6dc-44f3-9f06-f7634475aad3
msc.legacyurl: /signalr/overview/older-versions/signalr-1x-hubs-api-guide-net-client
msc.type: authoredcontent
ms.openlocfilehash: 5889429645ea1c682ea43c4b17afb3745318e32d
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41757344"
---
<a name="aspnet-signalr-hubs-api-guide---net-client-signalr-1x"></a>Podręcznik interfejsu API centrów SignalR platformy ASP.NET — klient modelu .NET (SignalR 1.x)
====================
przez [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)

> Ten dokument zawiera wprowadzenie do korzystania z interfejsu API centrów dla elementu SignalR w wersji 2 w przypadku klientów programu .NET, takich jak Windows Store (WinRT), WPF, Silverlight oraz aplikacji konsoli.
> 
> Interfejsu API centrów SignalR umożliwia zdalne wywołania procedur (RPC) z serwera do połączonych klientów i od klientów z serwerem. W kodzie serwera należy zdefiniować metody, które mogą być wywoływane przez klientów, a wywołanie metody, które są uruchamiane na komputerze klienckim. W kodzie klienta definiowania metod, które mogą być wywoływane z serwera, a wywołanie metody, które są uruchamiane na serwerze. SignalR zajmuje się wszystkie nadmiar klient serwer dla Ciebie.
> 
> SignalR oferuje również interfejs API niższego poziomu o nazwie połączeń trwałych. Wprowadzenie do SignalR, centra i połączenia trwałego lub samouczek, w którym przedstawiono sposób tworzenia kompletnej aplikacji SignalR, patrz [SignalR — wprowadzenie](../getting-started/index.md).


## <a name="overview"></a>Omówienie

Ten dokument zawiera następujące sekcje:

- [Instalacja klienta](#clientsetup)
- [Jak nawiązać połączenie](#establishconnection)

    - [Połączenia między domenami od klientów programu Silverlight](#slcrossdomain)
- [Jak skonfigurować połączenie](#configureconnection)

    - [Jak ustawić maksymalną liczbę równoczesnych połączeń klientów WPF](#maxconnections)
    - [Jak określić parametry ciągu zapytania](#querystring)
    - [Jak określić metodę transportu](#transport)
    - [Jak określić nagłówki HTTP](#httpheaders)
    - [Jak określić certyfikaty klienta](#clientcertificate)
- [Jak utworzyć serwer proxy koncentratora](#proxy)
- [Sposób definiowania metody na kliencie, który można wywołać serwera](#callclient)

    - [Metody bez parametrów](#clientmethodswithoutparms)
    - [Metody z parametrami, określając typy parametrów](#clientmethodswithparmtypes)
    - [Metody z parametrami, określając obiektów dynamicznych parametrów](#clientmethodswithdynamparms)
    - [Jak usunąć program obsługi](#removehandler)
- [Jak wywołać metody serwera z poziomu klienta](#callserver)
- [Jak obsługiwać zdarzenia okresu istnienia połączenia](#connectionlifetime)
- [Sposób obsługi błędów](#handleerrors)
- [Włączanie rejestrowania po stronie klienta](#logging)
- [WPF, Silverlight oraz przykłady kodu aplikacji konsoli dla metod klienta, które można wywołać serwera](#wpfsl)

Przykładowych projektach klientów platformy .NET na ten temat można znaleźć w następujących zasobach:

- [gustavo armenta / SignalR-Samples](https://github.com/gustavo-armenta/SignalR-Samples) w witrynie GitHub.com (przykłady aplikacji konsoli WinRT, Silverlight,).
- [DamianEdwards / SignalR MoveShapeDemo / MoveShape.Desktop](https://github.com/DamianEdwards/SignalR-MoveShapeDemo/tree/master/MoveShape/MoveShape.Desktop) w witrynie GitHub.com (np. WPF).
- [SignalR / Microsoft.AspNet.SignalR.Client.Samples](https://github.com/SignalR/SignalR/tree/master/samples/Microsoft.AspNet.SignalR.Client.Samples) w witrynie GitHub.com (np. aplikację konsoli).

Dokumentacja na temat programu server lub klientów języka JavaScript, zobacz następujące zasoby:

- [Podręcznik interfejsu API centrów SignalR — serwer](../guide-to-the-api/hubs-api-guide-server.md)
- [Podręcznik interfejsu API centrów SignalR — klient JavaScript](../guide-to-the-api/hubs-api-guide-javascript-client.md)

Łącza do tematów, dokumentacja interfejsu API są .NET 4.5 wersję interfejsu API. Jeśli używasz programu .NET 4, zobacz [tematy interfejsu API w wersji .NET 4](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).

<a id="clientsetup"></a>

## <a name="client-setup"></a>Instalacja klienta

Zainstaluj [Microsoft.AspNet.SignalR.Client](http://nuget.org/packages/Microsoft.AspNet.SignalR.Client) pakietu NuGet (nie [Microsoft.AspNet.SignalR](http://nuget.org/packages/microsoft.aspnet.signalr) pakietu). Ten pakiet obsługuje WinRT, Silverlight, WPF, aplikacji konsoli i klienci Windows Phone, zarówno dla platformy .NET 4 i .NET 4.5.

Jeśli wersji biblioteki SignalR, że masz na komputerze klienckim jest inna niż wersja, która ma na serwerze, SignalR jest zazwyczaj możliwość dostosowania różnicy. Na przykład po wydaniu SignalR w wersji 2.0 i można zainstalować na serwerze, serwer obsługuje klientów, którzy mają 1.1.x zainstalowane, a także klientów, którzy mają zainstalowany w wersji 2.0. Jeśli różnica między wersją na serwerze i wersji na komputerze klienckim jest zbyt duża, SignalR zgłasza `InvalidOperationException` wyjątku, gdy klient próbuje nawiązać połączenie. Komunikat o błędzie "`You are using a version of the client that isn't compatible with the server. Client version X.X, server version X.X`".

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a>Jak nawiązać połączenie

Zanim usługa może nawiązać połączenie, należy utworzyć `HubConnection` obiektu i tworzyć serwera proxy. Aby nawiązać połączenie, należy wywołać `Start` metody `HubConnection` obiektu.

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample1.cs?highlight=1,4)]

> [!NOTE]
> W przypadku klientów JavaScript należy zarejestrować co najmniej jeden program obsługi zdarzeń przed wywołaniem `Start` metodę, aby nawiązać połączenie. To nie jest wymagane dla klientów programu .NET. Dla klientów JavaScript wygenerowany serwer proxy automatycznie tworzy kod serwery proxy do wszystkich centrów znajdujące się na serwerze i rejestrowania procedury obsługi jest wskazuje, które koncentratorów klienta zamierza użyć. Ale dla klienta programu .NET utworzyć serwery proxy Centrum ręcznie, więc SignalR przyjęto założenie, że będziesz używać dowolne utworzonego serwera proxy dla Centrum.


Przykładowy kod używa domyślnej "/ signalr" adres URL, aby nawiązać połączenie z usługą SignalR. Aby uzyskać informacje o sposobie określania innego podstawowego adresu URL, zobacz [Podręcznik interfejsu API centrów SignalR platformy ASP.NET - Server - /signalr URL](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).

`Start` Metoda jest wykonywana asynchronicznie. Aby upewnić się, że kolejne wiersze kodu nie wykonany dopóki, po nawiązaniu połączenia, należy użyć `await` w metodzie asynchronicznej, ASP.NET 4.5 lub `.Wait()` w metodzie synchronicznej. Nie używaj `.Wait()` w kliencie WinRT.

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample2.cs?highlight=1)]

[!code-css[Main](signalr-1x-hubs-api-guide-net-client/samples/sample3.css?highlight=1)]

`HubConnection` Klasa jest metodą o bezpiecznych wątkach.

<a id="slcrossdomain"></a>

### <a name="cross-domain-connections-from-silverlight-clients"></a>Połączenia między domenami od klientów programu Silverlight

Aby uzyskać informacje o sposobie włączania połączeń między domenami od klientów programu Silverlight, zobacz [wprowadzania Service Available Across Domain Boundaries](https://msdn.microsoft.com/library/cc197955(v=vs.95).aspx).

<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a>Jak skonfigurować połączenie

Przed nawiązaniem połączenia, można określić dowolną z następujących opcji:

- Limit połączeń współbieżnych.
- Parametry ciągu zapytania.
- Metoda transportu.
- Nagłówki HTTP.
- Certyfikaty klienta.

<a id="maxconnections"></a>

### <a name="how-to-set-the-maximum-number-of-concurrent-connections-in-wpf-clients"></a>Jak ustawić maksymalną liczbę równoczesnych połączeń klientów WPF

W klientach programu WPF trzeba zwiększyć maksymalną liczbę równoczesnych połączeń od jego wartość domyślną 2. Zalecana wartość wynosi 10.

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample4.cs?highlight=4)]

Aby uzyskać więcej informacji, zobacz [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx).

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a>Jak określić parametry ciągu zapytania

Jeśli chcesz wysyłać dane do serwera, gdy klient nawiąże połączenie, można dodać parametry ciągu zapytania do obiektu połączenia. Poniższy przykład pokazuje, jak ustawić parametr ciągu zapytania w kodzie klienta.

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample5.cs)]

Poniższy przykład pokazuje, jak odczytywać parametr ciągu zapytania w kodzie serwera.

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample6.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a>Jak określić metodę transportu

Częścią procesu nawiązywania połączenia klienta SignalR zwykle negocjuje z serwerem, aby określić najlepsze transportu, która jest obsługiwana przez serwer i klienta. Jeśli znasz już transport, którego chcesz użyć, można pominąć ten proces negocjacji. Aby określić metodę transportu, należy przekazać w obiekcie transportu do metody uruchamiania. Poniższy przykład pokazuje, jak określić metodę transportu w kodzie klienta.

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample7.cs?highlight=4)]

[Microsoft.AspNet.SignalR.Client.Transports](https://msdn.microsoft.com/library/jj918090(v=vs.111).aspx) przestrzeń nazw zawiera następujące klasy, używanych do określania transportu.

- [LongPollingTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.longpollingtransport(v=vs.111).aspx)
- [ServerSentEventsTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.serversenteventstransport(v=vs.111).aspx)
- [WebSocketTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.websockettransport(v=vs.111).aspx) (dostępne tylko wtedy, gdy serwera i klienta użyj .NET 4.5).
- [AutoTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.autotransport(v=vs.111).aspx) (automatycznie wybiera najlepsze transportu, która jest obsługiwana zarówno przez klienta i serwera. Jest to domyślny transport. Te informacje w celu przekazywania `Start` metoda ma taki sam skutek jak nie przechodzi coś.)

ForeverFrame transport jest niedostępna na tej liście, ponieważ jest używany tylko przez przeglądarki.

Aby uzyskać informacje o sposobie Sprawdź metodą transportu w kodzie serwera, zobacz [Podręcznik interfejsu API centrów SignalR platformy ASP.NET - Server - sposób uzyskiwania informacji na temat klienta z właściwości kontekstu](../guide-to-the-api/hubs-api-guide-server.md#contextproperty). Aby uzyskać więcej informacji dotyczących transportu i planów awaryjnych, zobacz [wprowadzenie do SignalR - transportu i planów awaryjnych](../getting-started/introduction-to-signalr.md#transports).

<a id="httpheaders"></a>

### <a name="how-to-specify-http-headers"></a>Jak określić nagłówki HTTP

Aby ustawić nagłówki HTTP, użyj `Headers` właściwości w obiekcie połączenia. Poniższy przykład pokazuje, jak dodać nagłówek HTTP.

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample8.cs?highlight=2)]

<a id="clientcertificate"></a>

### <a name="how-to-specify-client-certificates"></a>Jak określić certyfikaty klienta

Aby dodać certyfikaty klienta, należy użyć `AddClientCertificate` metody na obiekt połączenia.

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample9.cs?highlight=2)]

<a id="proxy"></a>

## <a name="how-to-create-the-hub-proxy"></a>Jak utworzyć serwer proxy koncentratora

Aby zdefiniować metody dla klienta, który może wywołać koncentrator, z serwera i wywoływanie metod koncentratora na serwerze, należy utworzyć serwer proxy dla koncentratora przez wywołanie metody `CreateHubProxy` w obiekcie połączenia. Ciąg przekazanej do `CreateHubProxy` jest nazwą klasy koncentratora lub nazwa określona przez `HubName` atrybutu, jeśli użyto jednego na serwerze. Dopasowywaniu nazwy nie jest rozróżniana wielkość liter.

**Klasy koncentratora na serwerze**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample10.cs?highlight=1)]

**Utwórz serwer proxy klienta dla klasy koncentratora**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample11.cs?highlight=2)]

Jeśli dekoracji klasy koncentratora z `HubName` atrybutu, należy użyć tej nazwy.

**Klasy koncentratora na serwerze**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample12.cs)]

**Utwórz serwer proxy klienta dla klasy koncentratora**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample13.cs?highlight=2)]

Obiekt serwera proxy jest metodą o bezpiecznych wątkach. W rzeczywistości, jeśli wywołasz `HubConnection.CreateHubProxy` wiele razy z takimi samymi `hubName`, możesz uzyskać takie same buforowane `IHubProxy` obiektu.

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a>Sposób definiowania metody na kliencie, który można wywołać serwera

Aby zdefiniować metodę, która może wywołać serwera, należy użyć serwera proxy `On` metodę, aby zarejestrować program obsługi zdarzeń.

Dopasowywaniu nazwy metody nie jest rozróżniana wielkość liter. Na przykład `Clients.All.UpdateStockPrice` będą wykonywane na serwerze `updateStockPrice`, `updatestockprice`, lub `UpdateStockPrice` na komputerze klienckim.

Innego klienta platformy mają różne wymagania dotyczące sposobu pisania kodu metody, aby zaktualizować interfejs użytkownika. Przykłady zamieszczone dotyczą klientów WinRT (Windows Store .NET). WPF, Silverlight oraz przykłady aplikacji konsoli znajdują się w [osobnej sekcji w dalszej części tego tematu](#wpfsl).

<a id="clientmethodswithoutparms"></a>

### <a name="methods-without-parameters"></a>Metody bez parametrów

Jeśli metoda jest obsługa nie ma parametrów, użyj przeciążenia nieogólnego `On` metody:

**Kod serwera podczas wywoływania metody klienta bez parametrów**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample14.cs?highlight=5)]

**Kod klienta WinRT metody wywoływane z serwera bez parametrów ([zapoznaj się z przykładami WPF i Silverlight w dalszej części tego tematu](#wpfsl))**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample15.cs)]

<a id="clientmethodswithparmtypes"></a>

### <a name="methods-with-parameters-specifying-the-parameter-types"></a>Metody z parametrami, określając typy parametrów

Jeśli metoda jest obsługa ma parametry, należy określić typy parametrów jako typów ogólnego `On` metody. Istnieją przeciążenia ogólne `On` metody umożliwiają określenie parametrów do 8 (4 w systemie Windows Phone 7). W poniższym przykładzie jeden parametr jest wysyłane do `UpdateStockPrice` metody.

**Kod serwera podczas wywoływania metody klienta z parametrem**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample16.cs?highlight=3)]

**Klasy zapasów użytym dla parametru**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample17.cs)]

**Kod klienta WinRT dla metody wywoływane z serwera za pomocą parametru ([zapoznaj się z przykładami WPF i Silverlight w dalszej części tego tematu](#wpfsl))**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample18.cs?highlight=1,5)]

<a id="clientmethodswithdynamparms"></a>

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a>Metody z parametrami, określając obiektów dynamicznych parametrów

Jako alternatywę do określania parametrów jako typów ogólnego `On` metody, parametry można określić jako obiektów dynamicznych:

**Kod serwera podczas wywoływania metody klienta z parametrem**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample19.cs?highlight=3)]

**Klasy zapasów użytym dla parametru**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample20.cs)]

**Kod klienta WinRT dla metody wywoływane z serwera z parametrem przy użyciu obiekt dynamiczny parametr ([zapoznaj się z przykładami WPF i Silverlight w dalszej części tego tematu](#wpfsl))**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample21.cs?highlight=1,5)]

<a id="removehandler"></a>

### <a name="how-to-remove-a-handler"></a>Jak usunąć program obsługi

Aby usunąć program obsługi, należy wywołać jej `Dispose` metody.

**Kod klienta dla metodę o nazwie z serwera**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample22.cs?highlight=1)]

**Kod klienta, aby usunąć program obsługi**

[!code-css[Main](signalr-1x-hubs-api-guide-net-client/samples/sample23.css?highlight=1)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a>Jak wywołać metody serwera z poziomu klienta

Aby wywołać metodę na serwerze, należy użyć `Invoke` metody serwera proxy koncentratora.

Jeśli metoda nie zwraca żadnej wartości, użyj przeciążenia nieogólnego `Invoke` metody.

**Kod serwera dla metody, która nie zwraca żadnej wartości**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample24.cs?highlight=3)]

**Kod klienta, wywołanie metody, która nie zwraca żadnej wartości**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample25.cs?highlight=1)]

Jeśli metoda nie zwraca wartości, należy określić typ zwracany jako ogólny typ `Invoke` metody.

**Kod serwera dla metody, która nie zwraca wartości i przyjmuje parametr typu złożonego**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample26.cs?highlight=1)]

**Klasy zapasów użytym dla parametru i zwracają wartość**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample27.cs)]

**Kod klienta, wywołanie metody, która nie zwraca wartości i przyjmuje parametr typu złożonego w metodzie asynchronicznej ASP.NET 4.5**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample28.cs?highlight=1-2)]

**Kod klienta, wywołanie metody, która nie zwraca wartości i przyjmuje parametr typu złożonego, metoda synchroniczna**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample29.cs?highlight=1-2)]

`Invoke` Metoda wykonuje asynchronicznie i zwraca `Task` obiektu. Jeśli nie określisz `await` lub `.Wait()`, następnego wiersza kodu zostaną wykonane, zanim metodę, która wywołuje zakończenia.

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a>Jak obsługiwać zdarzenia okresu istnienia połączenia

Biblioteka SignalR udostępnia następujące połączenia zdarzenia okresu istnienia, które może obsłużyć:

- `Received`: Zgłaszane w przypadku nieodebrania żadnych danych w połączeniu. Udostępnia odebrane dane.
- `ConnectionSlow`: Wywoływane, gdy klient wykrywa wolno lub często porzucanie połączenia.
- `Reconnecting`: Wywoływane, gdy transportu źródłowego rozpoczyna ponowne nawiązywanie połączenia.
- `Reconnected`: Wywoływane, gdy nawiązał transportu źródłowego.
- `StateChanged`: Wywoływane, gdy zmieni się stan połączenia. Zawiera stan stary i nowy stan. Aby uzyskać informacje na temat połączenia wartości stanu zobacz [wyliczenie Element ConnectionState](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connectionstate(v=vs.111).aspx).
- `Closed`: Wywoływane, gdy połączenie zostało rozłączone.

Na przykład, jeśli chcesz wyświetlić komunikaty ostrzegawcze dotyczące błędów, które nie są one krytyczny, ale powodują sporadyczne problemy z połączeniem, takie jak powolność lub zbyt częstej porzucenie połączenia, obsługi `ConnectionSlow` zdarzeń.

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample30.cs)]

Aby uzyskać więcej informacji, zobacz [zrozumienia i obsługa zdarzeń okresu istnienia połączenia w SignalR](../guide-to-the-api/handling-connection-lifetime-events.md).

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a>Sposób obsługi błędów

Jeśli nie włączysz jawnie szczegółowe komunikaty o błędach na serwerze, obiekt wyjątku, który zwraca SignalR, po wystąpieniu błędu zawiera minimalne informacje o tym błędzie. Na przykład, jeśli wywołanie `newContosoChatMessage` zawiera komunikat o błędzie w obiekcie błąd zakończy się niepowodzeniem, "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" Wysyłanie szczegółowe komunikaty o błędach do klientów w środowisku produkcyjnym jest niezalecane ze względów bezpieczeństwa, ale jeśli chcesz włączyć szczegółowe komunikaty o błędach dla rozwiązywania problemów, użyj poniższego kodu, na serwerze.

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample31.cs?highlight=2)]

<a id="handleerrors"></a>

Do obsługi błędów, które wywołuje SignalR, można dodać program obsługi `Error` zdarzenie na obiekt połączenia.

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample32.cs)]

Do obsługi błędów z wywołań metod, zawijania kodu w bloku try-catch.

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample33.cs)]

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a>Włączanie rejestrowania po stronie klienta

Aby włączyć rejestrowanie klienta, należy ustawić `TraceLevel` i `TraceWriter` właściwości w obiekcie połączenia.

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample34.cs?highlight=2-3)]

<a id="wpfsl"></a>

## <a name="wpf-silverlight-and-console-application-code-samples-for-client-methods-that-the-server-can-call"></a>WPF, Silverlight oraz przykłady kodu aplikacji konsoli dla metod klienta, które można wywołać serwera

Przykłady kodu, w przedstawionej wcześniej do definiowania metody klientów, które serwer może wywoływać dotyczą klientów WinRT. Poniższe przykłady pokazują równoważny kod dla WPF, Silverlight oraz klientów aplikacji konsoli.

### <a name="methods-without-parameters"></a>Metody bez parametrów

**WPF kodu klienta dla metodę o nazwie z serwera bez parametrów**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample35.cs?highlight=1)]

**Kod klienta Silverlight dla metodę o nazwie z serwera bez parametrów**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample36.cs?highlight=1)]

**Kod klienta aplikacji konsolowej, metoda jest wywoływana z serwera bez parametrów**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample37.cs?highlight=1)]

### <a name="methods-with-parameters-specifying-the-parameter-types"></a>Metody z parametrami, określając typy parametrów

**WPF kodu klienta dla metodę o nazwie z serwera z parametrem**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample38.cs?highlight=1,4)]

**Kod klienta Silverlight dla metodę o nazwie z serwera z parametrem**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample39.cs?highlight=1,5)]

**Kod klienta aplikacji konsolowej, metoda jest wywoływana z serwera za pomocą parametru**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample40.cs?highlight=1-2)]

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a>Metody z parametrami, określając obiektów dynamicznych parametrów

**WPF kodu klienta dla metodę o nazwie z serwera z parametrem przy użyciu obiekt dynamiczny parametr**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample41.cs?highlight=1,4)]

**Kod klienta Silverlight dla metodę o nazwie z serwera z parametrem przy użyciu obiekt dynamiczny parametr**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample42.cs?highlight=1,5)]

**Kod klienta aplikacji konsoli dla metody wywoływane z serwera z parametrem przy użyciu obiekt dynamiczny parametr**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample43.cs?highlight=1-2)]
