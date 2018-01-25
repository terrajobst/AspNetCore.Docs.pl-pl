---
uid: signalr/overview/getting-started/tutorial-server-broadcast-with-signalr
title: 'Samouczek: Serwer emisji z SignalR 2 | Dokumentacja firmy Microsoft'
author: tdykstra
description: "W tym samouczku przedstawiono sposób tworzenia aplikacji sieci web, który używa 2 SignalR platformy ASP.NET w celu zapewnienia funkcji emisji serwera. Emisja serwera oznacza, że commun..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/13/2014
ms.topic: article
ms.assetid: 1568247f-60b5-4eca-96e0-e661fbb2b273
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/getting-started/tutorial-server-broadcast-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 98a7ce4991d58181177cf56976888e9fd1526987
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="tutorial-server-broadcast-with-signalr-2"></a>Samouczek: Serwer emisji z SignalR 2
====================
przez [Dykstra Tomasz](https://github.com/tdykstra), [FitzMacken niestandardowy](https://github.com/tfitzmac)

> W tym samouczku przedstawiono sposób tworzenia aplikacji sieci web, który używa 2 SignalR platformy ASP.NET w celu zapewnienia funkcji emisji serwera. Emisja serwera oznacza, że komunikacji klientów są inicjowane przez serwer. Ten scenariusz wymaga różne podejścia programowania niż peer-to-peer scenariuszy, takich jak aplikacje rozmów, w których komunikacji klientów są inicjowane przez jedną lub więcej klientów.
> 
> Aplikacji, które zostaną utworzone w tym samouczku symuluje giełdowych typowy scenariusz emisji funkcje serwera.
> 
> W tym temacie pierwotnie zapisał Patrick Fletcher.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Używane w samouczku wersje oprogramowania
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
> Aby używać programu Visual Studio 2012 z tego samouczka, wykonaj następujące czynności:
> 
> - Aktualizacja Twojego [Menedżera pakietów](http://docs.nuget.org/docs/start-here/installing-nuget) do najnowszej wersji.
> - Zainstaluj [sieci Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx).
> - Instalator platformy sieci Web, wyszukiwanie i instalowanie **ASP.NET i 2013.1 narzędzia sieci Web dla programu Visual Studio 2012**. Spowoduje to zainstalowanie szablony programu Visual Studio dla biblioteki SignalR klas takich jak **Centrum**.
> - Niektóre szablony (takich jak **klasy początkowej OWIN**) nie są dostępne; w tym przypadku powinien użyć pliku klasy.
> 
> 
> ## <a name="tutorial-versions"></a>Samouczek wersji
> 
> Aby uzyskać informacje dotyczące starszych wersji biblioteki signalr, zobacz [starsze wersje biblioteki SignalR](../older-versions/index.md).
> 
> ## <a name="questions-and-comments"></a>Pytania i komentarze
> 
> Wystaw opinię na jak zbędne tego samouczka i jakie firma Microsoft może poprawić w komentarze u dołu strony. Jeśli masz pytania, które nie są bezpośrednio związane z tego samouczka możesz zamieścić je do [forum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) lub [StackOverflow.com](http://stackoverflow.com/).


## <a name="overview"></a>Omówienie

W tym samouczku będzie utworzyć aplikację giełdowych reprezentatywne aplikacji w czasie rzeczywistym, w których chcesz okresowo "push", lub emisji, aby wszyscy połączeni klienci powiadomienia z serwera. W pierwszej części tego samouczka utworzysz uproszczonej wersji tej aplikacji od początku. W pozostałej części tego samouczka zostanie instalowanie pakietu NuGet, który zawiera dodatkowe funkcje oraz przejrzeć kod dla tych funkcji.

Aplikacja, która będzie kompilacji w pierwszej części tego samouczka Wyświetla siatkę z danych podstawowych.

![Wersja początkowa StockTicker](tutorial-server-broadcast-with-signalr/_static/image1.png)

Okresowo serwer losowo aktualizuje giełdowych i wypychanie aktualizacji do wszystkich połączonych klientów. W przeglądarce liczby lub symbole w **zmienić** i  **%**  kolumn dynamicznej zmiany w odpowiedzi na powiadomienia z serwera. Po otwarciu przeglądarki dodatkowych dla tego samego adresu URL, wszystkie one Pokaż tych samych danych i zmian w danych jednocześnie.

Ten samouczek zawiera następujące sekcje:

- [Wymagania wstępne](#prerequisites)
- [Tworzenie projektu](#createproject)
- [Ustaw kod serwera](#server)
- [Ustaw kod klienta](#client)
- [Testowanie aplikacji](#test)
- [Włączanie rejestrowania](#enablelogging)
- [Instalowanie i przejrzeć pełny przykład StockTicker](#fullsample)
- [Następne kroki](#nextsteps)

[Microsoft.AspNet.SignalR.Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) pakiet NuGet instaluje przykładowej symulowane aplikacji giełdowych w projektu programu Visual Studio.

> [!NOTE]
> Jeśli nie chcesz kroków tworzenia aplikacji, można zainstalować pakiet SignalR.Sample w nowym projekcie pusta aplikacja sieci Web platformy ASP.NET. Jeśli zainstalowano pakiet NuGet bez wykonywania czynności w tym samouczku **musi postępuj zgodnie z instrukcjami plik readme.txt**. Aby uruchomić pakiet, należy dodać klasę początkową OWIN, który wywołuje metodę ConfigureSignalR w zainstalowanym pakietem. Zostanie wyświetlony błąd, jeśli nie dodasz klasy początkowej OWIN.


<a id="prerequisites"></a>

## <a name="prerequisites"></a>Wymagania wstępne

Przed rozpoczęciem upewnij się, że masz program Visual Studio 2013 zainstalowany na tym komputerze. Jeśli nie masz programu Visual Studio, zobacz [pobiera ASP.NET](https://www.asp.net/downloads) uzyskać bezpłatne Visual Studio 2013 Express.

<a id="createproject"></a>

## <a name="create-the-project"></a>Utwórz projekt

1. Z **pliku** menu, kliknij przycisk **nowy projekt**.
2. W **nowy projekt** okna dialogowego rozwiń **C#** w obszarze **szablony** i wybierz **Web**.
3. Wybierz **pusta aplikacja sieci Web ASP.NET** szablonu, nazwy projektu *SignalR.StockTicker*i kliknij przycisk **OK**.

    ![Okno dialogowe Nowy projekt](tutorial-server-broadcast-with-signalr/_static/image2.png)
4. W **ASP.NET nowe** okno projektu, pozostaw **pusty** zaznaczone, a następnie kliknij przycisk **tworzenia projektu**.

    ![Okno dialogowe Nowy projekt ASP](tutorial-server-broadcast-with-signalr/_static/image3.png)

<a id="server"></a>

## <a name="set-up-the-server-code"></a>Ustaw kod serwera

W tej sekcji służy do konfigurowania kodu, który jest uruchamiany na serwerze.

### <a name="create-the-stock-class"></a>Tworzenie klasy zasobów

Rozpocznij poprzez utworzenie klasy modelu zasobu, który ma być używany do przechowywania i transmitowania informacji na temat zasobu.

1. Utwórz nowy plik klasy w folderze projektu, nadaj jej nazwę *Stock.cs*, a następnie Zastąp kod szablonu z następującym kodem:

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample1.cs)]

    Dwie właściwości, które zostaną ustawione podczas tworzenia zasobów są symbolu (na przykład MSFT dla firmy Microsoft) i cenę. Inne właściwości zależą od tego, jak i kiedy ustawić cenę. Ustaw cen, po raz pierwszy wartość pobiera propagowane do DayOpen. Kolejne czasu ustawić cen, zmiany i wartości właściwości PercentChange są obliczane w oparciu różnica między ceny i DayOpen.

### <a name="create-the-stockticker-and-stocktickerhub-classes"></a>Tworzenie klasy StockTicker i StockTickerHub

Za pomocą interfejsu API koncentratora SignalR będzie obsługiwać interakcji z serwera do klienta. Klasa StockTickerHub, która jest pochodną klasy koncentratora SignalR będzie obsługiwać odbieranie połączeń i wywołania metody od klientów. Należy również Obsługa danych giełdowych i uruchom obiekt czasomierza okresowo wyzwalanie aktualizacji cen, niezależnie od połączeń klientów. Nie można umieścić te funkcje w klasie Centrum, ponieważ przejściowej wystąpień koncentratorów. Wystąpienie klasy koncentratora jest tworzona dla każdej operacji koncentratora, takie jak połączenia i połączeń z klienta do serwera. Dlatego mechanizm, który przechowuje dane zapasów, aktualizacji ceny i emituje aktualizacji cen musi działać w oddzielnych klasy, która będzie nazwa StockTicker.

![Emisja z StockTicker](tutorial-server-broadcast-with-signalr/_static/image5.png)

Mają tylko jedno wystąpienie klasy StockTicker, aby uruchomić na serwerze, więc musisz skonfigurować odwołanie z każdego wystąpienia StockTickerHub na pojedyncze wystąpienie StockTicker. Klasa StockTicker ma mieć możliwość emisji do klientów, ponieważ ma standardowych danych i wyzwala aktualizacje, ale StockTicker nie jest klasą koncentratora. W związku z tym klasy StockTicker musi pobrać odwołanie do obiektu kontekstu połączenia koncentratora SignalR. Następnie można użyć obiektu context połączenia SignalR do emisji do klientów.

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt i kliknij przycisk **Dodaj | Klasy koncentratora SignalR (v2)**.
2. Nazwa nowego centrum *StockTickerHub.cs*, a następnie kliknij przycisk **Dodaj**. Pakiety SignalR NuGet zostaną dodane do projektu.
3. Zastąp kod szablonu z następującym kodem:

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample2.cs)]

    [Centrum](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx) klasa jest używana do definiowania metod klientów można wywołać na serwerze. Definiujesz jednej metody: `GetAllStocks()`. Gdy klient początkowo łączy się z serwerem, wywoła tę metodę, aby uzyskać listę wszystkich zasobów z ich bieżącej ceny. Metody można synchronicznie wykonać i zwracać `IEnumerable<Stock>` ponieważ zwraca dane z pamięci. Jeśli metoda musiały uzyskać danych przy wykonywaniu czegoś, co wymagałoby oczekiwania, takich jak wyszukiwania w bazie danych lub wywołania usługi sieci web, należy określić `Task<IEnumerable<Stock>>` jako wartości zwracane, aby włączyć przetwarzanie asynchroniczne. Aby uzyskać więcej informacji, zobacz [ASP.NET SignalR koncentratory interfejsu API przewodnik - Server - kiedy asynchroniczne](../guide-to-the-api/hubs-api-guide-server.md#asyncmethods).

    Atrybut HubName Określa, jak koncentratora zostanie dodane odwołanie w kodzie JavaScript na kliencie. Domyślna nazwa na kliencie, jeśli nie możesz użyć tego atrybutu to wersja formatu — z uwzględnieniem wielkości liter nazwy klasy, która w tym przypadku będzie stockTickerHub.

    Jak można zauważyć później podczas tworzenia klasy StockTicker, pojedyncze wystąpienie tej klasy jest tworzony w jego właściwości statycznej wystąpienia. Pojedyncze wystąpienie StockTicker pozostaje w pamięci bez względu na liczbę klientów Połącz lub Rozłącz, czy to wystąpienie jest metoda GetAllStocks używa do zwracania bieżącej standardowych informacji.
4. Utwórz nowy plik klasy w folderze projektu, nadaj jej nazwę *StockTicker.cs*, a następnie Zastąp kod szablonu z następującym kodem:

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample3.cs)]

    Ponieważ wiele wątków będzie działać to samo wystąpienie elementu StockTicker kodu, klasa StockTicker musi być threadsafe.

    ### <a name="storing-the-singleton-instance-in-a-static-field"></a>Przechowywanie pojedyncze wystąpienie w polu statycznym

    Kod inicjuje statycznych \_pole wystąpienia, aby utworzyć kopię zapasową właściwości wystąpienia przy użyciu wystąpienia klasy, a to jest tylko wystąpienia klasy, które można utworzyć, ponieważ Konstruktor jest oznaczony jako prywatny. [Inicjalizacja z opóźnieniem](https://msdn.microsoft.com/library/dd997286.aspx) służy do \_pola wystąpienia nie ze względu na wydajność, ale aby upewnić się, że tworzenie wystąpienia jest threadsafe.

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample4.cs)]

    Zawsze, gdy klient nawiąże połączenie z serwerem, nowe wystąpienie klasy StockTickerHub działa w oddzielnym wątku pobiera pojedyncze wystąpienie StockTicker z StockTicker.Instance właściwości statycznej, jak przedstawiono wcześniej w klasie StockTickerHub.

    ### <a name="storing-stock-data-in-a-concurrentdictionary"></a>Przechowywanie danych giełdowych w obiekt ConcurrentDictionary

    Konstruktor inicjuje \_kolekcji zasobów z niektórymi przykładowych danych podstawowych i GetAllStocks zwraca zasobów. Jak przedstawiono wcześniej, to kolekcja zasobów z kolei jest zwracany przez StockTickerHub.GetAllStocks, która to metoda serwera w klasie koncentratora, której klienci mogą wywoływać.

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample5.cs)]

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample6.cs)]

    Kolekcja zasobów została zdefiniowana jako [obiekt ConcurrentDictionary](https://msdn.microsoft.com/library/dd287191.aspx) typu dla bezpieczeństwa wątków. Alternatywnie, można użyć [słownika](https://msdn.microsoft.com/library/xfhwa508.aspx) obiektu i jawnie zablokować słownik po wprowadzeniu zmian do niego.

    Ta przykładowa aplikacja jest OK do przechowywania danych aplikacji w pamięci i utratę danych, jeśli wystąpienie StockTicker zostanie usunięty. W rzeczywistej aplikacji będzie działać z magazynem danych wewnętrznych, takich jak bazy danych.

    ### <a name="periodically-updating-stock-prices"></a>Okresowo aktualizacji cen akcji

    Obiekt czasomierza, który okresowo wywołuje metody, które aktualizują giełdowych wyrywkowo uruchamiania konstruktora.

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample7.cs)]

    UpdateStockPrices jest wywoływana przez czasomierz, który przekazuje wartość null w parametrze state. Przed zaktualizowaniem ceny, blokada jest pobierany podczas \_updateStockPricesLock obiektu. Kod sprawdza, czy inny wątek już aktualizuje ceny, a następnie wywołuje TryUpdateStockPrice dla każdej akcji na liście. Metoda TryUpdateStockPrice decyduje o tym, czy chcesz zmienić giełdowy i ile je zmienić. Zmiana giełdowy BroadcastStockPrice jest wywoływana emisji zmiany giełdowy wszyscy połączeni klienci.

    \_UpdatingStockPrices flaga jest oznaczony jako [volatile](https://msdn.microsoft.com/library/x13ttww7.aspx) w celu zapewnienia threadsafe do niego dostęp.

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample8.cs)]

    W rzeczywistej aplikacji metoda TryUpdateStockPrice spowodowałoby wywołanie usługi sieci web w celu wyszukania ceny; w tym kodzie użyto do zmiany losowo generator liczb losowych.

    ### <a name="getting-the-signalr-context-so-that-the-stockticker-class-can-broadcast-to-clients"></a>Uzyskanie kontekstu SignalR, dzięki czemu klasy StockTicker można emisji do klientów

    Ponieważ zmiany ceny tutaj pochodzą z obiektu StockTicker, to obiekt, który musi wywołać metodę updateStockPrice na wszystkich podłączonych klientów. W klasie Centrum masz interfejs API do wywoływania metody klienta, ale StockTicker nie pochodzi od klasy koncentratora i nie ma odwołania do dowolnego obiektu koncentratora. W związku z tym w celu emisji do połączonych klientów, klasa StockTicker musi pobrać wystąpienia kontekstu SignalR dla klasy StockTickerHub i używać go do wywołania metody na klientach.

    Ten kod pobiera odwołanie do kontekstu SignalR, podczas tworzenia pojedyncze wystąpienie klasy przebiegi, które odwołują się do konstruktora, i konstruktora umieszczenie go we właściwości klientów.

    Istnieją dwie przyczyny, dlaczego chcesz pobrać kontekstu tylko raz: uzyskanie kontekstu jest kosztowna operacja, i pobierania ich raz zapewnia zachowywanie zamierzonej kolejności komunikatów wysyłanych do klientów.

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample9.cs)]

    Pobieranie właściwości klientów kontekstu i umieść go we właściwości StockTickerClient umożliwia pisanie kodu do wywołania metody klienta, który wygląda tak samo jak w klasie koncentratora. Na przykład aby emisji do wszystkich klientów należy napisać Clients.All.updateStockPrice(stock).

    Metoda updateStockPrice wywołujesz BroadcastStockPrice nie istnieje jeszcze; można będzie dodać później podczas pisania kodu, który działa na kliencie. Ponieważ Clients.All jest dynamiczny, co oznacza, że wyrażenie, które zostanie obliczone w czasie wykonywania mogą odwoływać się do updateStockPrice tutaj. Gdy wykonuje wywołanie tej metody, SignalR wysyła nazwę metody i wartość parametru do klienta, a jeśli klient ma metodę o nazwie updateStockPrice, zostanie wywołana metoda i wartość parametru zostaną przekazane do niej.

    Clients.All oznacza wysłać do wszystkich klientów. SignalR udostępnia inne opcje, aby określić klientów lub grupy klientów do wysyłania do. Aby uzyskać więcej informacji, zobacz [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx).

### <a name="register-the-signalr-route"></a>Zarejestruj trasy SignalR

Serwer musi znać adres URL, który można przechwytywać i bezpośrednio do SignalR. Czy, który zostanie dodany i klasy początkowej OWIN.

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt, a następnie kliknij przycisk **Dodaj | Klasy początkowej OWIN**. Nazwa klasy **Startup.cs**.
2. Zastąp kod w **Startup.cs** następującym kodem.

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample10.cs)]

Ukończono konfigurowanie kod serwera. W następnej sekcji należy skonfigurować klienta.

<a id="client"></a>

## <a name="set-up-the-client-code"></a>Ustaw kod klienta

1. Utwórz nowy plik HTML w folderze projektu i nadaj mu nazwę *StockTicker.html*.
2. Zastąp kod szablonu z następującym kodem.

    [!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample11.html)]

    Kod HTML tworzy tabelę z kolumnami 5, wiersz nagłówka i wierszem danych z jednej komórce, które obejmuje wszystkie kolumny 5. Wiersz danych wyświetla "Trwa ładowanie..." i będą wyświetlane tylko na chwilę podczas uruchamiania aplikacji. Kod JavaScript spowoduje usunięcie tego wiersza i Dodaj w jego miejscu wierszy z podstawowe dane pobrane z serwera.

    Tagi skryptu Określ plik skryptu jQuery, plik skryptu SignalR core pliku skryptu proxy SignalR i StockTicker plik skryptu, który zostanie utworzony później. SignalR pliku skryptu proxy, który określa adres URL "/ signalr/hubs", jest generowane dynamicznie i określa metody serwera proxy dla metod w klasie koncentratora, w tym przypadku StockTickerHub.GetAllStocks. Jeśli wolisz, możesz wygenerować plik JavaScript ręcznie przy użyciu [narzędzia SignalR](http://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) i Wyłącz tworzenie dynamicznych plików w wywołaniu metody MapHubs.
3. > [!IMPORTANT]
 > Upewnij się, że plik JavaScript odwołania w *StockTicker.html* są poprawne. Oznacza to, upewnij się, że wersja jQuery w Twojej tagu skryptu (1.10.2 w przykładzie) jest taka sama jak wersja jQuery do projektu *skryptów* folderu i upewnij się, że wersja SignalR w Twojej tag skryptu jest taka sama jak SignalR Wersja do projektu *skryptów* folderu. Zmiana nazw plików w tagach skryptów, jeśli to konieczne.
4. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *StockTicker.html*, a następnie kliknij przycisk **Ustaw jako stronę startową**.
5. Utwórz nowy plik JavaScript w folderze projektu i nadaj mu nazwę *StockTicker.js*...
6. Zastąp kod szablonu z następującym kodem:

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample12.js)]

    $.connection odwołuje się do serwerów proxy SignalR. Ten kod pobiera odwołanie do serwera proxy dla klasy StockTickerHub i umieszczenie go w zmiennej znacznika. Nazwa serwera proxy jest nazwę, która została ustawiona przez atrybut [HubName]:

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample13.js)]

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample14.cs)]

    Po zdefiniowaniu zmiennych i funkcji ostatniego wiersza kodu w pliku inicjuje połączenia SignalR przez wywołanie funkcji start SignalR. Funkcja start wykonuje asynchronicznie i zwraca [jQuery opóźnieniem obiektu](http://api.jquery.com/category/deferred-object/), co oznacza można wywołać funkcję gotowe, aby określić funkcji do wywołania po ukończeniu operacji asynchronicznej.

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample15.js)]

    Funkcja inicjowania wywołuje funkcję getAllStocks na serwerze i wykorzystuje serwer zwraca można zaktualizować tabeli podstawowe informacje. Należy zauważyć, że domyślnie, należy użyć camel wielkość liter na kliencie, mimo że nazwa metody jest pascal — z uwzględnieniem wielkości liter na serwerze. Reguła wielkości liter formatu ma zastosowanie tylko do metod, a nie obiekty. Na przykład można odwoływać się do zasobu. Symbol i zapasów. Cena, nie stock.symbol lub stock.price.

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample16.js)]

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample17.cs)]

    Jeśli chcesz używać pascal wielkości liter na kliencie lub jeśli chcesz użyć nazwy innej metody, możesz można dekoracji metody koncentratora z atrybutem HubMethodName tak samo dekorowane się klasy koncentratora z atrybutem HubName.

    W przypadku metody init HTML dla wiersza tabeli jest tworzony dla każdego obiektu podstawowego otrzymany z serwera przez formatStock wywołanie do właściwości formatu standardowych obiektu i ograniczyć przez wywołanie (która jest zdefiniowana w górnej części *StockTicker.js*) należy zastąpić symbole zastępcze w zmiennej rowTemplate wartości właściwości standardowych obiektów. Wynikowa HTML następnie jest dołączany do podstawowego tabeli.

    Należy wywołać init, przekazując go jako funkcji wywołania zwrotnego, która jest wykonywana po zakończeniu działania funkcji rozpoczęcia asynchronicznego. Jeśli wywołujesz init jako oddzielną instrukcję języka JavaScript po wywołaniu metody uruchamiania funkcji nie powiedzie się, ponieważ będzie wykonaj natychmiast bez oczekiwania na funkcję start na zakończenie nawiązywania połączenia. W takim przypadku funkcja init może spróbować wywołanie funkcji getAllStocks, zanim zostanie nawiązane połączenie z serwerem.

    Zmiany cen danego zasobu, serwer wywołuje updateStockPrice na podłączonych klientów. Funkcja jest dodawany do właściwości klienta serwera proxy stockTicker Aby udostępnić go do wywołania z serwera.

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample18.js)]

    Funkcja updateStockPrice formatuje obiektem magazynowym otrzymany z serwera do wiersza tabeli tak samo jak funkcja init. Jednak dodanie wiersza do tabeli, a nie wyszukuje giełdowych bieżącego wiersza w tabeli i tego wiersza są zastępowane nową.

<a id="test"></a>

## <a name="test-the-application"></a>Testowanie aplikacji

1. Naciśnij klawisz F5, aby uruchomić aplikację w trybie debugowania.

    Tabela standardowych początkowo wyświetlane "Trwa ładowanie..." wiersza, następnie z niewielkim opóźnieniem wyświetlania standardowych danych początkowych, a następnie uruchom cen akcji można zmienić.

    ![Podczas ładowania](tutorial-server-broadcast-with-signalr/_static/image6.png)

    ![Początkowa tabeli standardowych](tutorial-server-broadcast-with-signalr/_static/image7.png)

    ![Tabela standardowych odbieranie zmian z serwera](tutorial-server-broadcast-with-signalr/_static/image8.png)
2. Skopiuj adres URL z pola adresu przeglądarki i wklej go do co najmniej jednego nowego okna przeglądarki.

    Początkowa wyświetlania standardowych jest taka sama jak pierwszy przeglądarki, a jednocześnie wprowadzenia zmiany.
3. Zamknij wszystkie przeglądarki i Otwórz w przeglądarce nowe, a następnie przejdź do tego samego adresu URL.

    Uruchom na serwerze, aby standardowych tabeli pokazuje, że nadal można zmienić zasobów nadal StockTicker pojedynczego obiektu. (Nie widzisz początkowej tabeli o wartości zero, zmień wartości).
4. Zamknij przeglądarkę.

<a id="enablelogging"></a>

## <a name="enable-logging"></a>Włączanie rejestrowania

SignalR ma funkcję wbudowanych rejestrowanie można włączyć na kliencie do pomocy w rozwiązywaniu problemów. W tej sekcji możesz włączyć rejestrowanie i przykłady, które pokazują, jak dzienniki informujące, które z następujących metod transportu SignalR używa:

- [Protokół WebSockets](http://en.wikipedia.org/wiki/WebSocket), obsługiwanych przez usługi IIS 8 i bieżącej przeglądarki.
- [Serwer wysłał zdarzenia](http://en.wikipedia.org/wiki/Server-sent_events)obsługiwany przez przeglądarki innego niż program Internet Explorer.
- [Nieskończona ramki](http://en.wikipedia.org/wiki/Comet_(programming)#Hidden_iframe), obsługiwanych przez program Internet Explorer.
- [Długie sondowania AJAX](http://en.wikipedia.org/wiki/Comet_(programming)#Ajax_with_long_polling)obsługiwany przez wszystkie przeglądarki.

Dla danego połączenia SignalR wybiera najlepszą metodę transportu, który obsługuje zarówno na serwerze, jak i klienta.

1. Otwórz *StockTicker.js* i Dodaj wiersz kodu, aby włączyć rejestrowanie bezpośrednio przed kod, który inicjuje połączenie z końcem pliku:

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample19.js)]
2. Naciśnij klawisz F5, aby uruchomić projekt.
3. Otwórz okno narzędzia deweloperów w przeglądarce, a następnie wybierz konsoli można znaleźć w dziennikach. Może być konieczne Odśwież stronę, aby wyświetlić dzienniki negocjowania Metoda transportu dla nowego połączenia Signalr.

    Jeśli korzystasz z programu Internet Explorer 10 w systemie Windows 8 (IIS 8), Metoda transportu jest Websocket.

    ![IE 10 IIS 8 Console](tutorial-server-broadcast-with-signalr/_static/image9.png)

    Jeśli korzystasz z programu Internet Explorer 10 w systemie Windows 7 (usług IIS 7.5), Metoda transportu jest iframe.

    ![IE 10 Console, IIS 7.5](tutorial-server-broadcast-with-signalr/_static/image10.png)

    W programie Firefox Zainstaluj dodatek Firebug można pobrać okna konsoli. Jeśli używasz przeglądarki Firefox 19 w systemie Windows 8 (IIS 8), Metoda transportu jest Websocket.

    ![Firefox 19 IIS 8 Websockets](tutorial-server-broadcast-with-signalr/_static/image11.png)

    Jeśli używasz przeglądarki Firefox 19 w systemie Windows 7 (usług IIS 7.5), Metoda transportu jest zdarzenia wysłanego przez serwer.

    ![Konsoli usług IIS 7.5 Firefox 19](tutorial-server-broadcast-with-signalr/_static/image12.png)

<a id="fullsample"></a>

## <a name="install-and-review-the-full-stockticker-sample"></a>Instalowanie i przejrzeć pełny przykład StockTicker

Aplikacji StockTicker, który jest instalowany przez [Microsoft.AspNet.SignalR.Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) pakietu NuGet zawiera więcej funkcji niż uproszczonej wersji, nowo utworzoną od podstaw. W tej części samouczka zainstaluj pakiet NuGet i Przejrzyj nowe funkcje i kod, który implementuje je. Po zainstalowaniu pakietu bez wykonywania wcześniejszych krokach tego samouczka, należy dodać klasę początkową OWIN do projektu. Ten krok jest szczegółowo opisane w pliku pakietu NuGet.

### <a name="install-the-signalrsample-nuget-package"></a>Zainstaluj pakiet SignalR.Sample NuGet

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt i kliknij przycisk **Zarządzaj pakietami NuGet**.
2. W **Zarządzaj pakietami NuGet** okno dialogowe, kliknij przycisk **Online**, wprowadź *SignalR.Sample* w **wyszukiwania Online** , a następnie kliknij przycisk  **Zainstaluj** w **SignalR.Sample** pakietu.

    ![Zainstaluj pakiet SignalR.Sample](tutorial-server-broadcast-with-signalr/_static/image13.png)
3. W **Eksploratora rozwiązań**, rozwiń węzeł *SignalR.Sample* folderu, który został utworzony przez zainstalowanie pakietu SignalR.Sample.
4. W *SignalR.Sample* folderu, kliknij prawym przyciskiem myszy *StockTicker.html*, a następnie kliknij przycisk **Ustaw jako stronę startową**.

    > [!NOTE]
    > Instalowania SignalR.Sample NuGet pakietu może zmienić wersję jQuery, który ma w Twojej *skryptów* folderu. Nowy *StockTicker.html* pliku, który instaluje pakiet w *SignalR.Sample* folderu zostaną zsynchronizowane z wersją jQuery, która instaluje pakiet, ale jeśli chcesz uruchomić oryginalny *StockTicker.html* ponownie, należy najpierw zaktualizować odwołania jQuery w tagu skryptu.

### <a name="run-the-application"></a>Uruchamianie aplikacji

1. Naciśnij klawisz F5, aby uruchomić aplikację.

    Oprócz siatki, który był wyświetlany poprzednio aplikacji pełnej giełdowych wyświetlane okno przewijania w poziomie z wyświetla te same dane zapasów. Po uruchomieniu aplikacji po raz pierwszy "rynku" jest "zamknięte" i statyczne siatki i okno znacznika, w którym nie jest przewijania.

    ![StockTicker ekranu start](tutorial-server-broadcast-with-signalr/_static/image14.png)

    Po kliknięciu **otwartego rynku**, **Live znacznika giełdowych** pole zaczyna być przewijane w poziomie, a serwer jest uruchamiany okresowo emisji giełdowy zmiany na podstawie losowej. Po każdej aktualizacji cen standardowych zmiany zarówno **Live tabeli giełdowych** siatki i **Live znacznika giełdowych** pola są aktualizowane. Podczas zmiany ceny danego zasobu jest dodatnia, zasobów jest wyświetlana z zielonym tłem, i po zmianie ma wartość ujemną, zasobów jest wyświetlany czerwony tła.

    ![Otwarcie rynku aplikacji StockTicker](tutorial-server-broadcast-with-signalr/_static/image15.png)

    **Zamknij rynku** przycisk zatrzymuje zmiany i zatrzymuje znacznika przewijanie i **zresetować** przycisku powoduje przywrócenie wszystkich standardowych danych stanu początkowego przed uruchomieniem zmiany ceny. Jeśli więcej okien przeglądarki i przejść do tego samego adresu URL, można zobaczyć te same dane, aktualizowana dynamicznie w tym samym czasie w każdej przeglądarki. Po kliknięciu jednego z przycisków wszystkie przeglądarki odpowiedź tak samo w tym samym czasie.

### <a name="live-stock-ticker-display"></a>Wyświetlanie znacznika zasobu na żywo

**Live znacznika giełdowych** wyświetlana jest Lista nieuporządkowana w element div, który jest sformatowany w jednym wierszu za style CSS. Znacznika zostanie zainicjowana i zaktualizować ten sam sposób jak tabeli:, zastępując symbole zastępcze w &lt;li&gt; ciąg szablonu i dynamiczne dodawanie &lt;li&gt; elementy &lt;ul&gt; element. Przewijanie odbywa się za pomocą funkcji Animacja jQuery różnicującej lewej margines nieuporządkowaną listę w div.

Giełdowych HTML:

[!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample20.html)]

Giełdowych CSS:

[!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample21.html)]

Kod jQuery, który umożliwia przewijanie:

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample22.js)]

### <a name="additional-methods-on-the-server-that-the-client-can-call"></a>Dodatkowe metody na serwerze, który można wywołać klienta

Klasa StockTickerHub definiuje cztery dodatkowe metody, które klient może wywoływać:

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample23.cs)]

OpenMarket, CloseMarket i resetowania są wywoływane w odpowiedzi do przycisków w górnej części strony. Pokazują one wzorzec jednego klienta, które wyzwalają zmianę stanu, który jest natychmiast propagowane do wszystkich klientów. Każda z tych metod wywołuje metodę w klasie StockTicker tego efekty stanu rynku zmienić i następnie emituje nowy stan.

W klasie StockTicker stan rynku jest obsługiwana przez właściwości MarketState, która zwraca wartość wyliczenia MarketState:

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample24.cs)]

Każdej z metod, które spowodują zmianę stanu rynku zrobić wewnątrz bloku blokady, ponieważ klasa StockTicker musi być threadsafe:

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample25.cs)]

Aby upewnić się, że ten kod jest threadsafe, \_marketState pola, aby utworzyć kopię zapasową właściwości MarketState jest oznaczony jako trwałe,

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample26.cs)]

Metody BroadcastMarketStateChange i BroadcastMarketReset są podobne do już wyświetlane, metoda BroadcastStockPrice, z wyjątkiem wywołują różnych metod zdefiniowany po stronie klienta:

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample27.cs)]

### <a name="additional-functions-on-the-client-that-the-server-can-call"></a>Dodatkowe funkcje na kliencie, który można wywołać serwera

Funkcja updateStockPrice teraz obsługuje zarówno siatki i wyświetlanie znacznika i stosuje jQuery.Color flash czerwony i zielony kolorów.

Nowe funkcje w *SignalR.StockTicker.js* Włączanie i wyłączanie przycisków oparte na rynku stanu i zatrzymać lub uruchomić znacznika okna przewijanie w poziomie. Ponieważ wiele funkcji są dodawane do ticker.client, [jQuery rozszerzanie funkcji](http://api.jquery.com/jQuery.extend/) służy do dodawania ich.

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample28.js)]

### <a name="additional-client-setup-after-establishing-the-connection"></a>Instalacja klienta dodatkowe po ustanowieniu połączenia

Po nawiązaniu przez klienta połączenie, ma wykonania dodatkowych czynności, aby zrobić: Sprawdzanie rynku open lub closed, aby wywołać odpowiednie marketOpened lub funkcja marketClosed i Dołącz wywołania metody serwera do przycisków.

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample29.js)]

Metody serwera nie przewodowe do przycisków dopiero po nawiązaniu połączenia, dzięki czemu kod nie może próbować wywołać metody serwera, zanim staną się dostępne.

<a id="nextsteps"></a>

## <a name="next-steps"></a>Następne kroki

W tym samouczku znasz jak do programowania aplikacji SignalR, który wysyła wiadomości z serwera do wszystkich połączonych klientów, zarówno w regularnych odstępach czasu, jak i w odpowiedzi na powiadomienia za pomocą dowolnego klienta. Wzorzec do zarządzania stanem serwera za pomocą wielowątkowych pojedyncze wystąpienie można również również w scenariuszach gry online przeznaczonych dla wielu graczy. Na przykład zobacz [gry ShootR, która jest oparta na SignalR](https://github.com/NTaylorMullen/ShootR).

Samouczki zawierających scenariusze komunikacji peer-to-peer, zobacz [wprowadzenie SignalR](introduction-to-signalr.md) i [aktualizacji w czasie rzeczywistym z SignalR](tutorial-high-frequency-realtime-with-signalr.md).

Aby uzyskać bardziej zaawansowane pojęcia dotyczące programowania SignalR, odwiedź następującą witrynę dla SignalR kod źródłowy i zasoby:

- [Biblioteka SignalR platformy ASP.NET](../../index.md)
- [Projekt SignalR](http://signalr.net/)
- [SignalR Github i przykłady](https://github.com/SignalR/SignalR)
- [Witryna typu Wiki biblioteki SignalR](https://github.com/SignalR/SignalR/wiki)

Aby uzyskać wskazówki dotyczące sposobu wdrażania aplikacji SignalR na platformie Azure, zobacz [SignalR korzystanie z aplikacji sieci Web w usłudze Azure App Service](../deployment/using-signalr-with-azure-web-sites.md). Aby uzyskać szczegółowe informacje o sposobie wdrażania projektu sieci web programu Visual Studio do witryny sieci Web systemu Windows Azure, zobacz [tworzenie aplikacji sieci web platformy ASP.NET w usłudze Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).
