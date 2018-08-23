---
uid: signalr/overview/getting-started/tutorial-server-broadcast-with-signalr
title: 'Samouczek: Emisje serwera z SignalR 2 | Dokumentacja firmy Microsoft'
author: tdykstra
description: W tym samouczku przedstawiono sposób tworzenia aplikacji sieci web korzystającą z signalr2 na platformie ASP.NET w celu zapewnienia funkcji emisji serwera. Server emisji oznacza że commun...
ms.author: riande
ms.date: 10/13/2014
ms.assetid: 1568247f-60b5-4eca-96e0-e661fbb2b273
msc.legacyurl: /signalr/overview/getting-started/tutorial-server-broadcast-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: c2248e68b3c9411687ab6410f12ec85488fe0738
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755909"
---
<a name="tutorial-server-broadcast-with-signalr-2"></a>Samouczek: Emisje serwera z SignalR 2
====================
przez [Tom Dykstra](https://github.com/tdykstra), [Tom FitzMacken](https://github.com/tfitzmac)

> W tym samouczku przedstawiono sposób tworzenia aplikacji sieci web korzystającą z signalr2 na platformie ASP.NET w celu zapewnienia funkcji emisji serwera. Emisja Server oznacza, że inicjowane komunikacji z klientami przez serwer. Ten scenariusz wymaga innego podejścia programowania niż peer-to-peer scenariuszy, takich jak aplikacje rozmowy, w których komunikacji z klientami są inicjowane przez jeden lub więcej klientów.
> 
> Aplikacja, którą utworzysz w tym samouczku symuluje giełdowej typowy scenariusz emisji funkcje serwera.
> 
> W tym temacie pierwotnie został napisany przez Patrick Fletcher.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używanego w tym samouczku
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR w wersji 2
>   
> 
> 
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a>Z tego samouczka przy użyciu programu Visual Studio 2012
> 
> 
> Aby użyć programu Visual Studio 2012 za pomocą tego samouczka, wykonaj następujące czynności:
> 
> - Aktualizacja usługi [Menedżera pakietów](http://docs.nuget.org/docs/start-here/installing-nuget) do najnowszej wersji.
> - Zainstaluj [Instalator platformy sieci Web](https://www.microsoft.com/web/downloads/platform.aspx).
> - Instalator platformy sieci Web, wyszukiwanie i instalowanie **platformy ASP.NET i Web Tools 2013.1 dla programu Visual Studio 2012**. Szablony programu Visual Studio dla klas SignalR spowoduje to zainstalowanie takich jak **Centrum**.
> - Niektóre szablony (takie jak **klasy początkowej OWIN**) nie są dostępne; w tym przypadku użyj pliku klasy.
> 
> 
> ## <a name="tutorial-versions"></a>Samouczek wersji
> 
> Aby uzyskać informacje dotyczące starszych wersji biblioteki SignalR, zobacz [starsze wersje biblioteki SignalR](../older-versions/index.md).
> 
> ## <a name="questions-and-comments"></a>Pytania i komentarze
> 
> Jak się podoba w tym samouczku, i co można było ulepszyć proces w komentarzach u dołu strony, wystaw opinię. Jeśli masz pytania, na które nie są bezpośrednio związane z tego samouczka, możesz zamieścić je do [forum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) lub [StackOverflow.com](http://stackoverflow.com/).


## <a name="overview"></a>Omówienie

W tym samouczku będziesz tworzenie aplikacji giełdowych, który jest reprezentatywny dla aplikacji w czasie rzeczywistym, w których chcesz okresowo "push" lub rozgłaszać powiadomienia z serwera do wszystkich połączonych klientów. W pierwszej części tego samouczka utworzysz uproszczoną wersję tej aplikacji od podstaw. W dalszej części tego samouczka zainstalujesz pakiet NuGet, który zawiera dodatkowe funkcje i Przegląd kodu dla tych funkcji.

Aplikację, którą utworzysz w pierwszej części tego samouczka Wyświetla siatkę z danych podstawowych.

![Wersja początkowa StockTicker](tutorial-server-broadcast-with-signalr/_static/image1.png)

Okresowo serwer losowo cen akcji aktualizacji i wypychania aktualizacji do wszystkich połączonych klientów. W przeglądarce cyfry i symbole w **zmienić** i **%** kolumn zmieniać dynamicznie w odpowiedzi na powiadomienia z serwera. Jeśli otworzysz dodatkowe przeglądarki pod kątem tego samego adresu URL, wszystkie one pokazywane te same dane i te same zmiany w danych jednocześnie.

Ten samouczek zawiera następujące sekcje:

- [Wymagania wstępne](#prerequisites)
- [Tworzenie projektu](#createproject)
- [Konfigurowanie kodu serwera](#server)
- [Ustaw kod klienta](#client)
- [Testowanie aplikacji](#test)
- [Włączanie rejestrowania](#enablelogging)
- [Instalowanie i przejrzeć pełny przykład StockTicker](#fullsample)
- [Następne kroki](#nextsteps)

[Microsoft.AspNet.SignalR.Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) pakiet NuGet instaluje aplikację giełdowej przykładowe symulowane w projekcie programu Visual Studio.

> [!NOTE]
> Jeśli nie chcesz pracować, kolejne kroki tworzenia aplikacji, można zainstalować pakietu SignalR.Sample w nowym projekcie pusta aplikacja sieci Web platformy ASP.NET. Po zainstalowaniu pakietu NuGet bez wykonywania czynności w tym samouczku **musi postępuj zgodnie z instrukcjami z pliku readme.txt**. Aby uruchomić pakiet, należy dodać klasę początkową OWIN, które wywołuje metodę ConfigureSignalR w zainstalowanym pakietem. Zostanie wyświetlony błąd, jeśli nie dodasz klasy początkowej OWIN.


<a id="prerequisites"></a>

## <a name="prerequisites"></a>Wymagania wstępne

Przed rozpoczęciem upewnij się, że masz zainstalowany na komputerze programu Visual Studio 2013. Jeśli nie masz programu Visual Studio, zobacz [ASP.NET pliki do pobrania](https://www.asp.net/downloads) można pobrać bezpłatny program Visual Studio 2013 Express.

<a id="createproject"></a>

## <a name="create-the-project"></a>Utwórz projekt

1. Z **pliku** menu, kliknij przycisk **nowy projekt**.
2. W **nowy projekt** okna dialogowego rozwiń **C#** w obszarze **szablony** i wybierz **Web**.
3. Wybierz **pusta aplikacja sieci Web platformy ASP.NET** szablonu, nazwę projektu *SignalR.StockTicker*i kliknij przycisk **OK**.

    ![Okno dialogowe Nowy projekt](tutorial-server-broadcast-with-signalr/_static/image2.png)
4. W **nowy ASP.NET** okno projektu, pozostaw **pusty** zaznaczone, a następnie kliknij przycisk **Tworzenie projektu**.

    ![Okno dialogowe Nowy projekt ASP](tutorial-server-broadcast-with-signalr/_static/image3.png)

<a id="server"></a>

## <a name="set-up-the-server-code"></a>Konfigurowanie kodu serwera

W tej sekcji służy do konfigurowania kodu, który działa na serwerze.

### <a name="create-the-stock-class"></a>Tworzenie klasy zasobów

Należy rozpocząć od tworzenia klasy modelu zasobu, które będzie używane do przechowywania i przesyłania informacji dotyczących danego zasobu.

1. Utwórz nowy plik klasy w folderze projektu, nadaj jej nazwę *Stock.cs*, a następnie Zastąp kod szablonu poniższym kodem:

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample1.cs)]

    Dwie właściwości, które zostaną ustawione podczas tworzenia zasobów są Symbol (na przykład MSFT dla firmy Microsoft) i cenę. Inne właściwości zależą od tego, jak i kiedy ustawić cenę. Po raz pierwszy możesz ustawić cenę, wartość pobiera propagowane do DayOpen. Kolejnych okresów, kiedy ustawić cenę, zmiany i PercentChange wartości właściwości są obliczane na podstawie różnicy między ceną i DayOpen.

### <a name="create-the-stockticker-and-stocktickerhub-classes"></a>Tworzenie klasy StockTicker i StockTickerHub

Za pomocą interfejsu API Centrum SignalR będzie obsługiwać interakcji z serwera do klienta. Klasa StockTickerHub, która jest pochodną klasy koncentratora SignalR będzie obsługiwać odbieranie wywołań metod i połączenia od klientów. Należy również do obsługi danych giełdowych i uruchom obiekt czasomierza, aby okresowo wyzwolenia aktualizacji cen, niezależnie od połączeń klientów. Nie można umieścić te funkcje w klasie koncentratora, ponieważ wystąpienia Centrum są przejściowy. Wystąpienie klasy koncentratora jest tworzona dla każdej operacji w Centrum, takie jak połączenia i wywołania od klienta do serwera. Dlatego mechanizm, który przechowuje dane zapasów, aktualizacji cen i emituje aktualizacji cen musi działać w osobnej klasy będzie nazwa StockTicker.

![Emisja z StockTicker](tutorial-server-broadcast-with-signalr/_static/image5.png)

Ma tylko jedno wystąpienie klasy StockTicker do uruchomienia na serwerze, więc musisz skonfigurować odwołanie z każdego wystąpienia StockTickerHub na pojedyncze wystąpienie StockTicker. Klasa StockTicker musi być w stanie wysyłać do klientów, ponieważ ma danych podstawowych i wyzwala aktualizacje, ale StockTicker nie jest klasą Centrum. W związku z tym klasa StockTicker musi uzyskać odwołanie do obiektu kontekstu połączenia koncentratora SignalR. Można następnie użyć obiektu context połączenia SignalR do emisji przeznaczonych dla klientów.

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt i kliknij przycisk **Dodaj | Klasa Centrum SignalR (v2)**.
2. Nazwa nowego centrum *StockTickerHub.cs*, a następnie kliknij przycisk **Dodaj**. Pakiety NuGet biblioteki SignalR zostaną dodane do projektu.
3. Zastąp kod szablonu poniższym kodem:

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample2.cs)]

    [Centrum](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx) klasa jest używana do definiowania metod klientów można wywołać na serwerze. Definiujesz jednej metody: `GetAllStocks()`. Gdy klient początkowo łączy się z serwerem, wywoła tę metodę, aby uzyskać listę wszystkich zasobów z ich bieżącym ceny. Metoda mogą być uruchamiane synchronicznie i zwracać `IEnumerable<Stock>` ponieważ zwraca dane z pamięci. Jeśli metoda musiały uzyskać danych, wykonując coś, co wymagałoby oczekiwania, takich jak wyszukiwania w bazie danych lub wywołanie usługi sieci web należy określić `Task<IEnumerable<Stock>>` jako wartości zwracanej, aby umożliwić przetwarzanie asynchroniczne. Aby uzyskać więcej informacji, zobacz [ASP.NET SignalR Podręcznik interfejsu API centrów — serwer — kiedy są wykonywane asynchronicznie](../guide-to-the-api/hubs-api-guide-server.md#asyncmethods).

    Atrybut HubName Określa, jak koncentratora będzie odwoływać się do kodu JavaScript na komputerze klienckim. Domyślna nazwa na kliencie, jeśli nie korzystasz z tego atrybutu jest nazwy klasy, która w tym przypadku wyniesie stockTickerHub wersją formacie camelcase.

    Jak zobaczysz w później, podczas tworzenia klasy StockTicker, pojedyncze wystąpienie tej klasy jest tworzony w jego właściwość statyczna wystąpienia. Czy pojedyncze wystąpienie StockTicker pozostaje w pamięci, niezależnie od tego, ilu klientów łączyć i rozłączać, a to wystąpienie jest metoda GetAllStocks używa do zwracania bieżącej podstawowych informacji.
4. Utwórz nowy plik klasy w folderze projektu, nadaj jej nazwę *StockTicker.cs*, a następnie Zastąp kod szablonu poniższym kodem:

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample3.cs)]

    Ponieważ wiele wątków będzie uruchamiana to samo wystąpienie elementu kodu StockTicker, klasa StockTicker ma być threadsafe.

    ### <a name="storing-the-singleton-instance-in-a-static-field"></a>Przechowywanie pojedyncze wystąpienie, w polu statycznym

    Ten kod inicjalizuje statycznej \_pole wystąpienia, które tworzy kopię Właściwość wystąpienia przy użyciu wystąpienia klasy i to jest tylko wystąpienia klasy, które mogą być tworzone, ponieważ Konstruktor jest oznaczony jako prywatny. [Inicjalizacja z opóźnieniem](https://msdn.microsoft.com/library/dd997286.aspx) służy do \_pole wystąpienia, nie ze względu na wydajność, ale aby upewnij się, że utworzenie wystąpienia threadsafe.

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample4.cs)]

    Każdorazowo, gdy klient nawiąże połączenie z serwerem, nowe wystąpienie klasy StockTickerHub działające w oddzielnym wątku pobiera pojedyncze wystąpienie StockTicker z StockTicker.Instance właściwości statycznej, jak przedstawiono wcześniej w klasie StockTickerHub.

    ### <a name="storing-stock-data-in-a-concurrentdictionary"></a>Przechowywanie danych giełdowych w ConcurrentDictionary

    Konstruktor inicjuje \_kolekcja zasobów, mając trochę przykładowych danych giełdowych i GetAllStocks zwraca zasobów. Jak wcześniej, to zbiór zasobów z kolei jest zwracany przez StockTickerHub.GetAllStocks, czyli metody serwera w klasy koncentratora, który można wywoływać klientów.

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample5.cs)]

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample6.cs)]

    Kolekcja zasobów jest zdefiniowana jako [ConcurrentDictionary](https://msdn.microsoft.com/library/dd287191.aspx) typu pod kątem bezpieczeństwa wątków. Alternatywnie, można użyć [słownika](https://msdn.microsoft.com/library/xfhwa508.aspx) obiektu i jawnie zablokować słownika, po wprowadzeniu zmian do niego.

    Dla tej przykładowej aplikacji jest dobrze do przechowywania danych aplikacji w pamięci i utratę danych, po usunięciu wystąpienia StockTicker. W rzeczywistej aplikacji będzie działać z magazynem danych zaplecza, takich jak bazy danych.

    ### <a name="periodically-updating-stock-prices"></a>Okresowo aktualizowanie cen akcji

    Konstruktor uruchamiania obiektu czasomierza, który okresowo wywołuje metody, które aktualizują cen akcji na podstawie losowej.

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample7.cs)]

    UpdateStockPrices jest wywoływana przez czasomierz, który przekazuje wartość null w parametrze state. Przed zaktualizowaniem ceny, blokady nałożonej na \_updateStockPricesLock obiektu. Kod sprawdza, jeśli inny wątek już trwa aktualizowanie cen, a następnie wywołuje TryUpdateStockPrice dla każdej akcji, na liście. Metoda TryUpdateStockPrice Określa, czy należy zmienić najniższej ceny i ilości go zmienić. Zmiana najniższej ceny akcji BroadcastStockPrice nosi nazwę emisji zmian cen akcji w wszyscy połączeni klienci.

    \_UpdatingStockPrices flaga jest oznaczony jako [volatile](https://msdn.microsoft.com/library/x13ttww7.aspx) aby upewnić się, że dostęp do niego jest threadsafe.

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample8.cs)]

    W rzeczywistej aplikacji metoda TryUpdateStockPrice będzie wywołać usługę sieci web, aby wyszukać ceny; w tym kodzie użyto generator liczb losowych do zmiany losowo.

    ### <a name="getting-the-signalr-context-so-that-the-stockticker-class-can-broadcast-to-clients"></a>Wprowadzenie kontekstu SignalR, tak aby klasy StockTicker można rozgłaszać do klientów

    Ponieważ zmiany cen poniżej pochodzą w obiekcie StockTicker, to obiekt, który musi wywołać metodę updateStockPrice na wszystkich połączonych klientów. W klasie Centrum masz interfejs API do wywoływania metody klientów, ale StockTicker nie pochodzi od klasy koncentratora i nie ma odwołania do dowolnego obiektu koncentratora. W związku z tym w celu emisji do połączonych klientów, klasa StockTicker musi pobrać wystąpienia kontekstu SignalR dla klasy StockTickerHub i używać go do wywoływania metod na komputerach klienckich.

    Ten kod pobiera odwołanie do kontekstu SignalR, podczas tworzenia wystąpienia klasy pojedyncze, przebiegi, które odwołują się do konstruktora, i Konstruktor umieszcza go we właściwości klientów.

    Istnieją dwie przyczyny, dlaczego chcesz uzyskać kontekst tylko raz: Pobieranie kontekstu jest kosztowną operacją, i pobierania ich raz zapewnia, że jest zachowywany zalecanej kolejności komunikatów wysyłanych do klientów.

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample9.cs)]

    Pobieranie właściwości klientów w kontekście i umieszczenie go we właściwości StockTickerClient umożliwia pisanie kodu w celu wywołania metod klienta, który wygląda tak samo jak w klasie Centrum. Na przykład aby rozgłaszać do wszystkich klientów, można napisać Clients.All.updateStockPrice(stock).

    Metoda updateStockPrice, którą wywołujesz BroadcastStockPrice nie istnieje jeszcze; należy dodać go później podczas pisania kodu, który jest uruchamiany na kliencie. Ponieważ Clients.All jest dynamiczny, co oznacza, że będzie można obliczyć wyrażenia w czasie wykonywania, można znaleźć tutaj updateStockPrice. Po wykonaniu wywołanie tej metody, SignalR wyśle nazwy metody i wartość parametru do klienta, a jeśli klient ma metodę o nazwie updateStockPrice, ta metoda zostanie wywołana, i wartość tego parametru, które zostaną przekazane do niej.

    Clients.All oznacza wysyłać do wszystkich klientów. SignalR udostępnia innych opcji, aby określić, które klientów lub grup klientów, aby wysyłać. Aby uzyskać więcej informacji, zobacz [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx).

### <a name="register-the-signalr-route"></a>Zarejestruj trasy SignalR

Serwer musi znać adres URL, który można przechwycić i kierują je bezpośrednio z SignalR. Aby to zrobić, Dodaj klasę początkową OWIN:

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt, a następnie kliknij przycisk **Dodaj | Klasa początkowa OWIN**. Nazwa klasy **Startup.cs**.
2. Zastąp kod w **Startup.cs** następującym kodem.

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample10.cs)]

Ukończono konfigurowanie kod serwera. W następnej sekcji należy skonfigurować klienta.

<a id="client"></a>

## <a name="set-up-the-client-code"></a>Ustaw kod klienta

1. Utwórz nowy plik HTML w folderze projektu i nadaj mu nazwę *StockTicker.html*.
2. Zastąp kod szablonu poniższym kodem.

    [!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample11.html)]

    Kod HTML tworzy tabelę z kolumnami 5, wiersz nagłówka i wiersz danych z pojedynczą komórkę, która obejmuje wszystkie kolumny 5. Wiersz danych zawiera "Trwa ładowanie..." i będą wyświetlane tylko chwilowo, podczas uruchamiania aplikacji. Kod JavaScript spowoduje usunięcie tego wiersza i dodać w jej miejscu wiersze z danych giełdowych pobrany z serwera.

    Tagi skryptu Określ plik skryptu jQuery, pliku skryptu core SignalR, SignalR pliku skryptu serwera proxy i StockTicker pliku skryptu, który utworzysz w dalszej części. Plik skryptu SignalR serwery proxy, który określa adres URL "/ signalr/centra", jest generowana dynamicznie i definiuje metody serwera proxy dla metody w klasie koncentratora, w tym przypadku StockTickerHub.GetAllStocks. Jeśli wolisz, możesz wygenerować ten plik JavaScript ręcznie przy użyciu [narzędzia SignalR](http://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) i Wyłącz tworzenie dynamicznych plików w wywołaniu metody MapHubs.
3. > [!IMPORTANT]
   > Upewnij się, że plik JavaScript odwołuje się w *StockTicker.html* są poprawne. Oznacza to, upewnij się, że tag skryptu (1.10.2 w przykładzie), w wersji jQuery jest taka sama jak wersja jQuery w swoim projekcie *skrypty* folderu i upewnij się, że wersji biblioteki SignalR tag skryptu jest taka sama jak SignalR Wersja w swoim projekcie *skrypty* folderu. Jeśli to konieczne, należy zmienić nazwy plików w tagów skryptu.
4. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *StockTicker.html*, a następnie kliknij przycisk **Ustaw jako strona startowa**.
5. Utwórz nowy plik JavaScript w folderze projektu i nadaj mu nazwę *StockTicker.js*...
6. Zastąp kod szablonu poniższym kodem:

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample12.js)]

    $.connection odnosi się do serwerów proxy SignalR. Ten kod pobiera odwołanie do serwera proxy dla klasy StockTickerHub i umieszcza go w zmiennej znacznika. Nazwa serwera proxy jest nazwa, która została ustawiona przez atrybut [HubName]:

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample13.js)]

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample14.cs)]

    Po zdefiniowaniu wszystkich zmiennych i funkcji ostatni wiersz kodu w pliku inicjuje połączenia SignalR, wywołując funkcję start SignalR. Funkcja start jest wykonywana asynchronicznie i zwraca [obiektu opóźnionych jQuery](http://api.jquery.com/category/deferred-object/), co oznacza może być wywołanie funkcji gotowe, aby określić funkcję do wywołania po zakończeniu operacji asynchronicznej...

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample15.js)]

    Init — funkcja wywołuje funkcję getAllStocks na serwerze i używa tych informacji, serwer zwraca można zaktualizować podstawowego tabeli. Należy zauważyć, że domyślnie, trzeba użyć pisane wielkość liter na komputerze klienckim, mimo że w nazwie metody istotna jest pascal — z uwzględnieniem wielkości liter na serwerze. Reguła wielkości liter pisane dotyczy tylko metody, a nie obiektów. Na przykład to odwołanie do zasobu. Symbol i zasobów. Ceny, nie stock.symbol lub stock.price.

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample16.js)]

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample17.cs)]

    Jeśli chcesz Użyj pascal wielkość liter w wyrazie na kliencie lub jeśli chcesz użyć nazwy całkowicie inną metodę można można dekoracji metody koncentratora z atrybutem HubMethodName taki sam sposób dekorowane się klasy koncentratora z atrybutem HubName.

    W przypadku metody init kod HTML dla wiersza tabeli jest tworzony dla każdego obiektu podstawowego otrzymany z serwera przez wywołującego formatStock format właściwości obiektu podstawowego i ograniczyć przez wywołanie (który jest zdefiniowany w górnej części *StockTicker.js*) aby zastąpić symbole zastępcze w zmiennej rowTemplate wartości właściwości obiektu podstawowego. Wynikowy kod HTML, następnie jest dołączany do podstawowego tabeli.

    Init jest wywoływana przez przekazanie jej jako funkcja wywołania zwrotnego, który jest wykonywany po zakończeniu funkcji uruchamiania asynchronicznego. Jako osobne instrukcja kodu JavaScript po wywołaniu start o nazwie init, funkcja zakończy się niepowodzeniem, ponieważ będzie wykonywane natychmiast bez oczekiwania na funkcję start, aby zakończyć ustanawiania połączenia. W takim przypadku funkcja init próbowała Wywołaj funkcję getAllStocks, zanim zostanie nawiązane połączenie z serwerem.

    Po zmianie cen zasobów serwera wywołuje updateStockPrice na połączonych klientów. Funkcja jest dodawany do właściwości klienta serwera proxy stockTicker Aby udostępnić go do wywołania z serwera.

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample18.js)]

    Funkcja updateStockPrice formatuje obiektem magazynowym otrzymany z serwera, wiersze tabeli, tak jak w funkcji init. Jednak zamiast dołączać wiersz do tabeli, wyszukuje stock bieżącego wiersza w tabeli i zamienia nowego wiersza.

<a id="test"></a>

## <a name="test-the-application"></a>Testowanie aplikacji

1. Naciśnij klawisz F5, aby uruchomić aplikację w trybie debugowania.

    Podstawowe tabeli początkowo wyświetlane są "Trwa ładowanie..." wiersza, następnie po krótkiej chwili początkowej danych podstawowych jest wyświetlane i zacznij cen akcji można zmienić.

    ![Trwa ładowanie](tutorial-server-broadcast-with-signalr/_static/image6.png)

    ![Początkowa Tabela podstawowych](tutorial-server-broadcast-with-signalr/_static/image7.png)

    ![Tabela podstawowych odebranie zmian z serwera](tutorial-server-broadcast-with-signalr/_static/image8.png)
2. Skopiuj adres URL z paska adresu przeglądarki i wklej go do przynajmniej jednego nowego okna przeglądarki.

    Początkowe wyświetlanie podstawowych jest taki sam jak pierwszy przeglądarki i jednocześnie wprowadzenia zmiany.
3. Zamknij wszystkie przeglądarki i Otwórz w nowym oknie przeglądarki, a następnie przejdź do tego samego adresu URL.

    Uruchom na serwerze, aby podstawowe tabeli pokazuje, że nadal można zmienić zasobów nadal StockTicker pojedynczego obiektu. (Nie widzisz początkowa tabela o wartości zero, zmień ilustracji).
4. Zamknij przeglądarkę.

<a id="enablelogging"></a>

## <a name="enable-logging"></a>Włączanie rejestrowania

SignalR ma funkcję wbudowane funkcje rejestrowania, który można włączyć na kliencie, aby ułatwić rozwiązywanie problemów. W tej sekcji, Włącz rejestrowanie i zapoznaj się z przykładami, które pokazują, jak dzienniki informujące, który z poniższych metod transportu używa SignalR:

- [Gniazda Websocket](http://en.wikipedia.org/wiki/WebSocket)obsługiwany przez usługi IIS 8 i bieżącej przeglądarki.
- [Serwer wysłał zdarzenia](http://en.wikipedia.org/wiki/Server-sent_events), obsługiwane przez przeglądarki innej niż Internet Explorer.
- [Nieskończona ramki](http://en.wikipedia.org/wiki/Comet_(programming)#Hidden_iframe)obsługiwany przez program Internet Explorer.
- [AJAX długim sondowaniem](http://en.wikipedia.org/wiki/Comet_(programming)#Ajax_with_long_polling)obsługiwany przez wszystkie przeglądarki.

Dla dowolnego danego połączenia SignalR wybiera najlepszą metodą transportu, który obsługuje zarówno na serwerze, jak i klienta.

1. Otwórz *StockTicker.js* i Dodaj wiersz kodu, aby włączyć rejestrowanie bezpośrednio przed kod, który inicjuje połączenie z końcem pliku:

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample19.js)]
2. Naciśnij klawisz F5, aby uruchomić projekt.
3. Otwórz okno narzędzi programistycznych w przeglądarce, a następnie wybierz konsolę, aby wyświetlić dzienniki. Trzeba będzie odświeżyć stronę, aby wyświetlić dzienniki negocjowania metodę transportu do nowego połączenia Signalr.

    Jeśli używasz programu Internet Explorer 10 w systemie Windows 8 (IIS 8), metodą transportu jest WebSockets.

    ![Konsolę programu IE 10 IIS 8](tutorial-server-broadcast-with-signalr/_static/image9.png)

    Jeśli korzystasz z programu Internet Explorer 10, Windows 7 (usługi IIS 7.5), metodą transportu jest elementu iframe.

    ![IE 10 Console, IIS 7.5](tutorial-server-broadcast-with-signalr/_static/image10.png)

    W przeglądarce Firefox Zainstaluj dodatek Firebug można pobrać z okna konsoli. Jeśli używasz 19 przeglądarki Firefox w systemie Windows 8 (IIS 8), metodą transportu jest WebSockets.

    ![Firefox 19 IIS 8 Websockets](tutorial-server-broadcast-with-signalr/_static/image11.png)

    Jeśli używasz przeglądarki Firefox 19 Windows 7 (usługi IIS 7.5), metodą transportu jest zdarzenia wysłanego przez serwer.

    ![Konsolę programu Firefox 19 IIS 7.5](tutorial-server-broadcast-with-signalr/_static/image12.png)

<a id="fullsample"></a>

## <a name="install-and-review-the-full-stockticker-sample"></a>Instalowanie i przejrzeć pełny przykład StockTicker

Aplikacja StockTicker, który jest instalowany przez [Microsoft.AspNet.SignalR.Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) pakietu NuGet zawiera więcej funkcji niż uproszczonej wersji, który został utworzony od zera. W tej części samouczka zainstaluj pakiet NuGet i Przejrzyj nowe funkcje i kod, który implementuje je. Po zainstalowaniu pakietu bez przeprowadzania wcześniejsze kroki tego samouczka, należy dodać klasę początkową OWIN do projektu. Ten krok jest szczegółowo plik readme.txt na potrzeby pakietu NuGet.

### <a name="install-the-signalrsample-nuget-package"></a>Zainstaluj pakiet SignalR.Sample NuGet

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt i kliknij przycisk **Zarządzaj pakietami NuGet**.
2. W **Zarządzaj pakietami NuGet** okno dialogowe, kliknij przycisk **Online**, wprowadź *SignalR.Sample* w **Wyszukaj Online** , a następnie kliknij przycisk  **Zainstaluj** w **SignalR.Sample** pakietu.

    ![Zainstaluj pakiet SignalR.Sample](tutorial-server-broadcast-with-signalr/_static/image13.png)
3. W **Eksploratora rozwiązań**, rozwiń węzeł *SignalR.Sample* folder, który został utworzony, instalując pakiet SignalR.Sample.
4. W *SignalR.Sample* folderu, kliknij prawym przyciskiem myszy *StockTicker.html*, a następnie kliknij przycisk **Ustaw jako stronę startową**.

    > [!NOTE]
    > Instalowanie SignalR.Sample NuGet pakietu może zmienić wersję jQuery, które mają w swojej *skrypty* folderu. Nowy *StockTicker.html* pliku, który pakiet instaluje w *SignalR.Sample* folder będzie zsynchronizowany z wersją jQuery instalująca pakiet, ale jeśli chcesz uruchomić oryginalny *StockTicker.html* ponownie plik, trzeba najpierw zaktualizuj odwołanie jQuery w tagu.

### <a name="run-the-application"></a>Uruchamianie aplikacji

1. Naciśnij klawisz F5, aby uruchomić aplikację.

    Oprócz siatki, który był wyświetlany poprzednio aplikacja pełnej giełdowej przedstawiono przewijana poziomo okno, które wyświetla te same dane zapasów. Po uruchomieniu aplikacji po raz pierwszy, "na rynku" to "zamknięte", a zobaczysz statyczne siatki i okna znacznika, który nie jest przewijanie.

    ![StockTicker ekranu start](tutorial-server-broadcast-with-signalr/_static/image14.png)

    Po kliknięciu **wolnym rynku**, **Live znacznika Stock** pole zaczyna być przewijane w poziomie, a serwer jest uruchamiany okresowo wysyłać zmian cen akcji na podstawie losowej. Każdym cena akcji zmiany zarówno **Live tabeli Stock** siatki i **Live znacznika Stock** pola są aktualizowane. Gdy zmian cen zasobów jest dodatnia, cena akcji jest wyświetlany z zielonym tłem i po zmianie jest ujemna, zasobów jest wyświetlana z czerwonym tłem.

    ![Otwórz na rynku aplikacji StockTicker](tutorial-server-broadcast-with-signalr/_static/image15.png)

    **Zamknij rynku** przycisk przestaje zmiany i zatrzymuje znacznika przewijanie i **resetowania** przycisku powoduje zresetowanie wszystkich danych giełdowych do stanu początkowego przed rozpoczęciem cenami. Jeśli otworzysz więcej okna przeglądarki i przejdź do tego samego adresu URL, zobaczysz te same dane, które są aktualizowane dynamicznie w tym samym czasie w każdej przeglądarce. Po kliknięciu jednego z przycisków wszystkie przeglądarki odpowiadać taki sam sposób, w tym samym czasie.

### <a name="live-stock-ticker-display"></a>Na żywo wyświetlanie znacznika zapasów

**Live znacznika Stock** nieuporządkowaną listę w elemencie div., który jest sformatowany w jednej linii za style CSS są wyświetlane. Znacznika zostanie zainicjowana i zaktualizować ten sam sposób jak tabeli:, zastępując symbole zastępcze w wywołaniach &lt;li&gt; ciąg szablonu i dynamiczne dodawanie &lt;li&gt; elementów &lt;ul&gt; element. Przewijania odbywa się za pomocą funkcji animowanie jQuery będzie się różnić w lewej margines nieuporządkowaną listę w obrębie div.

Giełdowej HTML:

[!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample20.html)]

Giełdowej CSS:

[!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample21.html)]

Przewiń kodu jQuery, który sprawia, że:

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample22.js)]

### <a name="additional-methods-on-the-server-that-the-client-can-call"></a>Dodatkowe metody na serwerze, który klient może wywołać

Klasa StockTickerHub definiuje cztery dodatkowe metody, które klient może wywoływać:

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample23.cs)]

OpenMarket, CloseMarket i resetowania są wywoływane w odpowiedzi na przycisków w górnej części strony. Pokazują one wzorca jednego klienta, wyzwalając zmianę stanu, który jest natychmiast propagowane do wszystkich klientów. Każda z tych metod wywołuje metodę w klasie StockTicker tego efekty stanu rynku zmiany, a następnie emituje nowy stan.

W klasie StockTicker stanu rynku jest obsługiwana przez właściwości MarketState, która zwraca wartość wyliczenia MarketState:

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample24.cs)]

Każdej z metod, które zmieniają stan rynku zrobić wewnątrz bloku blokady ponieważ klasa StockTicker musi być threadsafe:

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample25.cs)]

Aby upewnić się, że ten kod jest threadsafe, \_marketState zawierającym kopie właściwość MarketState jest oznaczony jako nietrwałe,

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample26.cs)]

Metody BroadcastMarketStateChange i BroadcastMarketReset są podobne do metody BroadcastStockPrice, która już działa, z wyjątkiem wywołują różnych metod, zdefiniowanych na komputerze klienckim:

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample27.cs)]

### <a name="additional-functions-on-the-client-that-the-server-can-call"></a>Dodatkowe funkcje, na komputerze klienckim, który można wywoływać serwera

Funkcja updateStockPrice obsługuje teraz zarówno w siatce, jak i wyświetlanie znacznika i używa jQuery.Color do flash czerwonego i zielonego kolorów.

Nowe funkcje w *SignalR.StockTicker.js* włączyć i wyłączyć przyciski oparte na rynek stanu i zatrzymać lub uruchomić znacznika okna poziome paski przewijania. Ponieważ wiele funkcji są dodawane do ticker.client, [jQuery rozszerzanie funkcji](http://api.jquery.com/jQuery.extend/) służy do dodawania do nich.

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample28.js)]

### <a name="additional-client-setup-after-establishing-the-connection"></a>Instalacja klienta dodatkowe po ustanowieniu połączenia

Po nawiązaniu połączenia przez klienta ma pewne dodatkowe zadania do wykonania: Sprawdź, czy rynku jest otwarte lub zamknięte w celu wywołania odpowiedniej marketOpened lub marketClosed i dołączyć wywołania metody serwera do przycisków.

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample29.js)]

Metody serwera są nie powiązaną przycisków do momentu po nawiązaniu połączenia, dzięki czemu kod nie mogą próbować wywołać metody serwera, zanim staną się dostępne.

<a id="nextsteps"></a>

## <a name="next-steps"></a>Następne kroki

W tym samouczku wyjaśniono sposób programowania aplikacji SignalR, który emituje komunikaty z serwera do wszyscy połączeni klienci, zarówno w regularnych odstępach czasu i w odpowiedzi na powiadomienia za pomocą dowolnego klienta. Wzorzec przy użyciu wielowątkowych pojedyncze wystąpienie, aby zachować stan serwera można także również w online scenariuszach grę wielu graczy. Aby uzyskać przykład, zobacz [gry ShootR, który jest oparty na SignalR](https://github.com/NTaylorMullen/ShootR).

Samouczki, które pokazują scenariuszy komunikacji między peer-to-peer, zobacz [wprowadzenie do SignalR](introduction-to-signalr.md) i [aktualizacji w czasie rzeczywistym, przy użyciu SignalR](tutorial-high-frequency-realtime-with-signalr.md).

Aby dowiedzieć się więcej zaawansowanych koncepcji programowania SignalR, odwiedź następującą witrynę dla SignalR kod źródłowy i zasobów:

- [Biblioteki SignalR platformy ASP.NET](../../index.md)
- [Projekt SignalR](http://signalr.net/)
- [SignalR Github i przykłady](https://github.com/SignalR/SignalR)
- [Witryny typu Wiki biblioteki SignalR](https://github.com/SignalR/SignalR/wiki)

Aby uzyskać wskazówki dotyczące sposobu wdrażania aplikacji SignalR na platformie Azure, zobacz [przy użyciu SignalR z usługą Web Apps w usłudze Azure App Service](../deployment/using-signalr-with-azure-web-sites.md). Aby uzyskać szczegółowe informacje o sposobie wdrażania projektu sieci web programu Visual Studio z witryny sieci Web do Windows Azure, zobacz [tworzenie aplikacji sieci web platformy ASP.NET w usłudze Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).
