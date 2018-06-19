---
uid: signalr/overview/guide-to-the-api/hubs-api-guide-net-client
title: Podręcznik interfejsu API koncentratorów SignalR platformy ASP.NET — klienta .NET (C#) | Dokumentacja firmy Microsoft
author: pfletcher
description: Ten dokument zawiera wprowadzenie do korzystania z interfejsu API koncentratorów dla biblioteki SignalR w wersji 2 w klientów platformy .NET, takich jak Sklep Windows (WinRT), WPF, Silverlight i wad...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 6d02d9f7-94e5-4140-9f51-5a6040f274f6
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/guide-to-the-api/hubs-api-guide-net-client
msc.type: authoredcontent
ms.openlocfilehash: c52a02291e18b1dd8a9d95b33fe466d17aae835f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
ms.locfileid: "28043941"
---
<a name="aspnet-signalr-hubs-api-guide---net-client-c"></a>Podręcznik interfejsu API koncentratorów SignalR platformy ASP.NET — klienta .NET (C#)
====================
przez [Patrick Fletcher](https://github.com/pfletcher), [Dykstra niestandardowy](https://github.com/tdykstra)

> Ten dokument zawiera wprowadzenie do korzystania z interfejsu API koncentratorów dla biblioteki SignalR w wersji 2 w klientów platformy .NET, takie jak magazynu systemu Windows (WinRT), WPF i Silverlight oraz aplikacji konsoli.
> 
> Interfejs API koncentratorów SignalR umożliwia upewnij zdalnych wywołań procedur (RPC) z serwera do połączonych klientów i klientów na serwerze. W kodzie serwera zdefiniuj metody, które mogą być wywoływane przez klientów i wywołania metod, które są uruchamiane na komputerze klienckim. W kodu klienta należy zdefiniować metody, które mogą być wywoływane z serwera i wywołania metody, które są uruchamiane na serwerze. SignalR odpowiada on za wszystkie żmudne procesy klient serwer dla Ciebie.
> 
> SignalR udostępnia również interfejs API niższego poziomu o nazwie połączeń trwałych. Wprowadzenie do SignalR, koncentratorach i połączeń trwałych lub Samouczek przedstawiający sposób tworzenia pełnej aplikacji SignalR, patrz [SignalR — wprowadzenie](../getting-started/index.md).
> 
> ## <a name="software-versions-used-in-this-topic"></a>Wersje oprogramowania używane w tym temacie
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR w wersji 2
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a>Poprzednie wersje tego tematu
> 
> Aby uzyskać informacje dotyczące starszych wersji biblioteki signalr, zobacz [starsze wersje biblioteki SignalR](../older-versions/index.md).
> 
> ## <a name="questions-and-comments"></a>Pytania i komentarze
> 
> Wystaw opinię na jak zbędne tego samouczka i jakie firma Microsoft może poprawić w komentarze u dołu strony. Jeśli masz pytania, które nie są bezpośrednio związane z tego samouczka możesz zamieścić je do [forum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) lub [StackOverflow.com](http://stackoverflow.com/).


## <a name="overview"></a>Omówienie

Ten dokument zawiera następujące sekcje:

- [Instalacja klienta](#clientsetup)
- [Jak nawiązać połączenie](#establishconnection)

    - [Między domenami połączenia od klientów programu Silverlight](#slcrossdomain)
- [Jak skonfigurować połączenia](#configureconnection)

    - [Jak ustawić maksymalną liczbę równoczesnych połączeń klientów WPF](#maxconnections)
    - [Jak określać parametrów ciągu zapytania](#querystring)
    - [Jak określić metodę transportu](#transport)
    - [Jak określić nagłówków HTTP](#httpheaders)
    - [Jak określić certyfikaty klienta](#clientcertificate)
- [Tworzenie serwera proxy koncentratora](#proxy)
- [Sposób definiowania metod na komputerze klienckim, który można wywołać serwera](#callclient)

    - [Metody bez parametrów](#clientmethodswithoutparms)
    - [Metody z parametrami, określając typy parametrów](#clientmethodswithparmtypes)
    - [Metody z parametrami, określając obiekty dynamiczne parametrów](#clientmethodswithdynamparms)
    - [Jak usunąć program obsługi](#removehandler)
- [Sposób wywołania metody serwera z klienta](#callserver)
- [Sposób obsługi zdarzeń okres istnienia połączenia](#connectionlifetime)
- [Sposób obsługi błędów](#handleerrors)
- [Jak włączyć rejestrowanie klienta](#logging)
- [WPF, Silverlight i przykładów kodu aplikacji konsoli dla metod klienta, które może wywoływać serwera](#wpfsl)

Przykładowych projektach klienta .NET na ten temat można znaleźć w następujących zasobach:

- [Gustaw armenta / SignalR próbek](https://github.com/gustavo-armenta/SignalR-Samples) w witrynie GitHub.com (przykłady aplikacji konsoli WinRT, Silverlight,).
- [DamianEdwards / SignalR MoveShapeDemo / MoveShape.Desktop](https://github.com/DamianEdwards/SignalR-MoveShapeDemo/tree/master/MoveShape/MoveShape.Desktop) w witrynie GitHub.com (przykład WPF).
- [SignalR / Microsoft.AspNet.SignalR.Client.Samples](https://github.com/SignalR/SignalR/tree/master/samples/Microsoft.AspNet.SignalR.Client.Samples) w witrynie GitHub.com (przykład aplikacji konsoli).

Dokumentacja na temat programu server lub klientów języka JavaScript, zobacz następujące zasoby:

- [Podręcznik interfejsu API koncentratorów SignalR — serwer](hubs-api-guide-server.md)
- [Podręcznik interfejsu API koncentratorów SignalR — JavaScript klienta](hubs-api-guide-javascript-client.md)

Linki do tematów dokumentacji interfejsu API są wersja platformy .NET 4.5 interfejsu API. Jeśli używasz programu .NET 4, zobacz [wersji .NET 4 tematy interfejsu API](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).

<a id="clientsetup"></a>

## <a name="client-setup"></a>Instalacja klienta

Zainstaluj [Microsoft.AspNet.SignalR.Client](http://nuget.org/packages/Microsoft.AspNet.SignalR.Client) pakietu NuGet (nie [Microsoft.AspNet.SignalR](http://nuget.org/packages/microsoft.aspnet.signalr) pakietu). Ten pakiet obsługuje WinRT, Silverlight, WPF, aplikacji konsoli i klienci Windows Phone dla platformy .NET 4.5 i .NET 4.

Wersja SignalR zainstalowanego na komputerze klienckim jest inna niż wersja zainstalowanego na serwerze, SignalR jest często możliwość dostosowania różnicy. Na przykład serwer z systemem w wersji 2 SignalR będzie obsługiwać klientów, którzy mają zainstalowane 1.1.x, a także klientów, którzy mają w wersji 2 zainstalowane. Jeśli różnica między wersją na serwerze i wersja na komputerze klienckim jest zbyt duża lub jeśli klient jest nowsza niż na serwerze, zgłasza SignalR `InvalidOperationException` wyjątku, gdy klient próbuje nawiązać połączenie. Komunikat o błędzie "`You are using a version of the client that isn't compatible with the server. Client version X.X, server version X.X`".

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a>Jak nawiązać połączenie

Przed może nawiązać połączenie, należy utworzyć `HubConnection` obiektu i utworzyć serwer proxy. Aby nawiązać połączenie, należy wywołać `Start` metoda `HubConnection` obiektu.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample1.cs?highlight=1,4)]

> [!NOTE]
> W przypadku klientów JavaScript należy zarejestrować co najmniej jeden program obsługi zdarzeń przed wywołaniem `Start` metody do nawiązania połączenia. To nie jest wymagane w przypadku klientów platformy .NET. Dla klientów języka JavaScript, kod wygenerowany serwer proxy po automatycznie tworzy serwery proxy dla wszystkich koncentratorów, które istnieją na serwerze, a rejestrowanie obsługi jest sposób wskazuje które koncentratorów klienta zamierza użyć. Ale dla klienta programu .NET utworzyć proxy koncentratora ręcznie, więc SignalR przyjęto założenie, że będziesz używać dowolnego Centrum utworzonego na serwerze proxy dla.


Przykładowy kod używa domyślnej "/ signalr" adres URL do łączenia się z usługą SignalR. Aby uzyskać informacje o sposobie określania innego podstawowego adresu URL, zobacz [ASP.NET SignalR koncentratory interfejsu API przewodnik - Server - /signalr URL](hubs-api-guide-server.md#signalrurl).

`Start` Metoda wykonuje asynchronicznie. Aby upewnić się, że kolejnych wierszy kodu nie są wykonywane dopiero po nawiązaniu połączenia, należy użyć `await` w ASP.NET 4.5 metody asynchronicznej lub `.Wait()` w metodzie synchronicznej. Nie używaj `.Wait()` w kliencie WinRT.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample2.cs?highlight=1)]

[!code-css[Main](hubs-api-guide-net-client/samples/sample3.css?highlight=1)]

<a id="slcrossdomain"></a>

### <a name="cross-domain-connections-from-silverlight-clients"></a>Między domenami połączenia od klientów programu Silverlight

Aby uzyskać informacje o sposobie włączania połączeń między domenami z klientów programu Silverlight, zobacz [wprowadzania Service Available Across Domain Boundaries](https://msdn.microsoft.com/library/cc197955(v=vs.95).aspx).

<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a>Jak skonfigurować połączenia

Przed nawiązaniem połączenia można określić dowolną z następujących opcji:

- Limit równoczesnych połączeń.
- Parametry ciągu zapytania.
- Metoda transportu.
- Nagłówki HTTP.
- Certyfikaty klienta.

<a id="maxconnections"></a>

### <a name="how-to-set-the-maximum-number-of-concurrent-connections-in-wpf-clients"></a>Jak ustawić maksymalną liczbę równoczesnych połączeń klientów WPF

W klientach WPF może być konieczne zwiększyć maksymalną liczbę równoczesnych połączeń od wartość domyślną 2. Zalecana wartość to 10.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample4.cs?highlight=4)]

Aby uzyskać więcej informacji, zobacz [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx).

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a>Jak określać parametrów ciągu zapytania

Jeśli chcesz wysyłać dane do serwera, gdy klient nawiąże połączenie, możesz dodać parametrów ciągu zapytania do obiektu połączenia. Poniższy przykład pokazuje, jak ustawić parametr ciągu zapytania w kodzie klienta.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample5.cs)]

Poniższy przykład pokazuje, jak można odczytać parametr ciągu zapytania w kodzie serwera.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample6.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a>Jak określić metodę transportu

W ramach procesu łączenia klienta SignalR zwykle negocjuje z serwerem w celu ustalenia najlepsze transport, który jest obsługiwany przez zarówno serwera i klienta. Jeśli znasz już transport, który ma być używany, można pominąć ten proces negocjacji. Aby określić metodę transportu, należy przekazać w obiekcie transportu do metody rozpoczęcia. Poniższy przykład pokazuje, jak określić metodę transportu w kodu klienta.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample7.cs?highlight=4)]

[Microsoft.AspNet.SignalR.Client.Transports](https://msdn.microsoft.com/library/jj918090(v=vs.111).aspx) przestrzeń nazw zawiera następujące klasy używanych do określania transportu.

- [LongPollingTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.longpollingtransport(v=vs.111).aspx)
- [ServerSentEventsTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.serversenteventstransport(v=vs.111).aspx)
- [WebSocketTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.websockettransport(v=vs.111).aspx) (dostępne tylko wtedy, gdy zarówno serwera, jak i klienta korzystają z platformy .NET 4.5.)
- [AutoTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.autotransport(v=vs.111).aspx) (automatycznie wybiera najlepsze transport, który jest obsługiwana zarówno przez klienta i serwera. Jest to domyślny transport. Przekazywanie, to aby `Start` metoda ma ten sam efekt co w niczego nie przekazywany.)

ForeverFrame transport nie jest uwzględniony na liście, ponieważ jest używany tylko przez przeglądarki.

Aby dowiedzieć się, jak sprawdzić Metoda transportu w kodzie serwera, zobacz [ASP.NET SignalR koncentratory interfejsu API przewodnik - Server - metody uzyskiwania informacji o kliencie z właściwości kontekstu](hubs-api-guide-server.md#contextproperty). Aby uzyskać więcej informacji na temat transportów i przejścia, zobacz [wprowadzenie do biblioteki SignalR — transportu i przejścia](../getting-started/introduction-to-signalr.md#transports).

<a id="httpheaders"></a>

### <a name="how-to-specify-http-headers"></a>Jak określić nagłówków HTTP

Aby ustawić nagłówków HTTP, użyj `Headers` właściwości obiektu połączenia. Poniższy przykład przedstawia sposób dodawania nagłówka HTTP.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample8.cs?highlight=2)]

<a id="clientcertificate"></a>

### <a name="how-to-specify-client-certificates"></a>Jak określić certyfikaty klienta

Aby dodać certyfikaty klienta, użyj `AddClientCertificate` metody dla obiekt połączenia.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample9.cs?highlight=2)]

<a id="proxy"></a>

## <a name="how-to-create-the-hub-proxy"></a>Tworzenie serwera proxy koncentratora

Aby zdefiniować metody na komputerze klienckim koncentratora można wywołać z serwera, a do wywołania metody koncentratora na serwerze, należy utworzyć serwer proxy dla koncentratora przez wywołanie metody `CreateHubProxy` na obiekt połączenia. Ciąg przekazywane w celu `CreateHubProxy` to nazwa klasy koncentratora lub nazwa określona przez `HubName` atrybut, jeśli użyto jednego serwera. Dopasowywaniu nazwy nie jest rozróżniana wielkość liter.

**Klasy koncentratora na serwerze**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample10.cs?highlight=1)]

**Utwórz serwer proxy klienta dla koncentratora klasy**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample11.cs?highlight=2)]

Jeśli dekoracji klasy koncentratora z `HubName` atrybutu, użyj tej nazwy.

**Klasy koncentratora na serwerze**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample12.cs)]

**Utwórz serwer proxy klienta dla koncentratora klasy**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample13.cs?highlight=2)]

Jeśli należy wywołać `HubConnection.CreateHubProxy` wiele razy z tym samym `hubName`, możesz uzyskać je w pamięci podręcznej `IHubProxy` obiektu.

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a>Sposób definiowania metod na komputerze klienckim, który można wywołać serwera

Aby określić metodę można wywołać serwera, Użyj serwera proxy `On` metodę, aby zarejestrować program obsługi zdarzeń.

Dopasowania nazwy metody jest rozróżniana wielkość liter. Na przykład `Clients.All.UpdateStockPrice` będą wykonywane na serwerze `updateStockPrice`, `updatestockprice`, lub `UpdateStockPrice` na kliencie.

Platformy innego klienta mają różne wymagania dotyczące sposobu pisania kodu metody można zaktualizować interfejsu użytkownika. Przykłady pokazano dotyczą klientów WinRT (.NET Sklepu Windows). WPF, Silverlight i przykłady aplikacji konsoli znajdują się w [osobnej sekcji w dalszej części tego tematu](#wpfsl).

<a id="clientmethodswithoutparms"></a>

### <a name="methods-without-parameters"></a>Metody bez parametrów

Jeśli metoda w przypadku obsługi nie ma parametrów, użyj przeciążenia nierodzajową `On` metody:

**Wywołanie metody klienta bez parametrów kodu serwera**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample14.cs?highlight=5)]

**Kod klienta WinRT metody wywoływane z serwera bez parametrów ([zawiera przykłady WPF i Silverlight w dalszej części tego tematu](#wpfsl))**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample15.cs)]

<a id="clientmethodswithparmtypes"></a>

### <a name="methods-with-parameters-specifying-the-parameter-types"></a>Metody z parametrami, określając typy parametrów

Jeśli metoda w przypadku obsługi ma parametry, określ typy parametrów jako typy ogólne z `On` metody. Brak przeciążeń ogólnego `On` sposób można określić maksymalnie 8 parametrów (4 w systemie Windows Phone 7). W poniższym przykładzie jeden parametr jest wysyłane do `UpdateStockPrice` metody.

**Wywołanie metody klienta z parametrem kodu serwera**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample16.cs?highlight=3)]

**Klasy zapasów użyte w parametrze**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample17.cs)]

**Kod klienta WinRT metody wywoływane z serwera z parametrem ([zawiera przykłady WPF i Silverlight w dalszej części tego tematu](#wpfsl))**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample18.cs?highlight=1,5)]

<a id="clientmethodswithdynamparms"></a>

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a>Metody z parametrami, określając obiekty dynamiczne parametrów

Jako alternatywę do określania parametrów jako typów ogólnych z `On` metody, można określić parametrów jako obiekty dynamiczne:

**Wywołanie metody klienta z parametrem kodu serwera**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample19.cs?highlight=3)]

**Klasy zapasów użyte w parametrze**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample20.cs)]

**Kod klienta WinRT metody o nazwie z serwera z parametrem dla parametru za pomocą obiektu dynamicznego ([zawiera przykłady WPF i Silverlight w dalszej części tego tematu](#wpfsl))**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample21.cs?highlight=1,5)]

<a id="removehandler"></a>

### <a name="how-to-remove-a-handler"></a>Jak usunąć program obsługi

Aby usunąć program obsługi, należy wywołać jej `Dispose` metody.

**Kodu klienta dla metody o nazwie z serwera**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample22.cs?highlight=1)]

**Kod klienta, aby usunąć program obsługi**

[!code-css[Main](hubs-api-guide-net-client/samples/sample23.css?highlight=1)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a>Sposób wywołania metody serwera z klienta

Aby wywołać metodę na serwerze, należy użyć `Invoke` metody dla serwera proxy koncentratora.

Jeśli metoda serwera nie ma zwracanych wartości, użyj przeciążenia nierodzajową `Invoke` metody.

**Kod serwera dla metody, która nie ma wartości zwracane**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample24.cs?highlight=3)]

**Kod klienta podczas wywoływania metody, która nie ma wartości zwracane**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample25.cs?highlight=1)]

Jeśli metoda serwera ma wartość zwracaną, określ typ zwracany jako typ ogólny `Invoke` metody.

**Kod serwera dla metody, która ma wartość zwracaną i przyjmuje parametr typu złożonego**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample26.cs?highlight=1)]

**Klasa zasobu użyte w parametrze i zwracać wartość**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample27.cs)]

**Kod klienta podczas wywoływania metody, która ma wartość zwracaną i przyjmuje parametr typu złożonego, to metoda asynchroniczna ASP.NET 4.5**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample28.cs?highlight=1-2)]

**Wywoływanie metody, która ma wartość zwracaną i przyjmuje parametr typu złożonego, metoda synchroniczna kodu klienta**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample29.cs?highlight=1-2)]

`Invoke` Metoda wykonuje asynchronicznie i zwraca `Task` obiektu. Jeśli nie określisz `await` lub `.Wait()`, następnym wierszu kodu zostanie wykonany przed metody, które można wywołać zakończono wykonywanie.

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a>Sposób obsługi zdarzeń okres istnienia połączenia

Biblioteka SignalR udostępnia następujące połączenie okres istnienia zdarzeń, które może obsłużyć:

- `Received`: Zgłaszane w przypadku nieodebrania żadnych danych w połączeniu. Udostępnia odebranych danych.
- `ConnectionSlow`: Wywoływane, gdy klient wykryje wolne lub często porzucanie połączenie.
- `Reconnecting`: Wywoływane, gdy transportu źródłowego rozpoczyna ponowne nawiązywanie połączenia.
- `Reconnected`: Wywoływane, gdy podłączył transportu źródłowego.
- `StateChanged`: Wywoływane po zmianie stanu połączenia. Zawiera stan stary i nowy stan. Aby uzyskać informacje o połączeniu zobacz wartości stanu [wyliczenie Element ConnectionState](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connectionstate(v=vs.111).aspx).
- `Closed`: Wywoływane, gdy połączenie zostało rozłączone.

Na przykład, jeśli chcesz wyświetlić komunikaty ostrzegawcze błędów, które nie są krytyczne, ale powodują sporadyczne problemy z połączeniem, takie jak powolność lub zbyt częstej porzucenie połączenia, obsługi `ConnectionSlow` zdarzeń.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample30.cs)]

Aby uzyskać więcej informacji, zobacz [zrozumienia i obsługi zdarzeń okres istnienia połączenia w SignalR](handling-connection-lifetime-events.md).

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a>Sposób obsługi błędów

Jeśli nie włączysz jawnie szczegółowe komunikaty o błędach na serwerze, obiekt wyjątku, który zwraca SignalR po wystąpieniu błędu zawiera minimalne informacje o błędzie. Na przykład, jeśli wywołanie `newContosoChatMessage` zawiera komunikat o błędzie w obiekcie błąd kończy się niepowodzeniem, "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" Wysyłanie szczegółowe komunikaty o błędach do klientów w środowisku produkcyjnym nie jest zalecana ze względów bezpieczeństwa, ale jeśli chcesz włączyć szczegółowe komunikaty o błędach dla rozwiązywania problemów, należy użyć poniższego kodu na serwerze.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample31.cs?highlight=2)]

<a id="handleerrors"></a>

Do obsługi błędów, które wywołuje SignalR, można dodać obsługi dla `Error` zdarzenia dla obiekt połączenia.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample32.cs)]

Do obsługi błędów z wywołań metody opisywanego, ujmij kod w bloku try-catch.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample33.cs)]

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a>Jak włączyć rejestrowanie klienta

Aby włączyć rejestrowanie klienta, należy ustawić `TraceLevel` i `TraceWriter` właściwości obiektu połączenia.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample34.cs?highlight=2-3)]

<a id="wpfsl"></a>

## <a name="wpf-silverlight-and-console-application-code-samples-for-client-methods-that-the-server-can-call"></a>WPF, Silverlight i przykładów kodu aplikacji konsoli dla metod klienta, które może wywoływać serwera

Przykłady kodu pokazano wcześniej do definiowania metod klienta, które można wywołać serwera dotyczą klientów WinRT. Poniższe przykłady Pokaż równoważne kod WPF, Silverlight i klientów aplikacji konsoli.

### <a name="methods-without-parameters"></a>Metody bez parametrów

**WPF kodu klienta dla metody o nazwie z serwera bez parametrów**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample35.cs?highlight=1)]

**Kod klienta Silverlight dla metody wywoływane z serwera bez parametrów**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample36.cs?highlight=1)]

**Kod klienta aplikacji konsoli metody wywoływane z serwera bez parametrów**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample37.cs?highlight=1)]

### <a name="methods-with-parameters-specifying-the-parameter-types"></a>Metody z parametrami, określając typy parametrów

**WPF kodu klienta dla metody o nazwie z serwera z parametrem**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample38.cs?highlight=1,4)]

**Kod klienta Silverlight dla metody o nazwie z serwera z parametrem**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample39.cs?highlight=1,5)]

**Kod klienta aplikacji konsoli metody wywoływane z serwera z parametrem**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample40.cs?highlight=1-2)]

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a>Metody z parametrami, określając obiekty dynamiczne parametrów

**WPF kodu klienta dla metody o nazwie z serwera z parametrem przy użyciu obiekt dynamiczny dla parametru**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample41.cs?highlight=1,4)]

**Kod klienta Silverlight dla metody o nazwie z serwera z parametrem przy użyciu obiekt dynamiczny dla parametru**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample42.cs?highlight=1,5)]

**Kod klienta aplikacji konsoli metody o nazwie z serwera z parametrem przy użyciu obiekt dynamiczny dla parametru**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample43.cs?highlight=1-2)]
