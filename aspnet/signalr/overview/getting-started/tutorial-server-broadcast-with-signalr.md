---
uid: signalr/overview/getting-started/tutorial-server-broadcast-with-signalr
title: 'Samouczek: Emisje serwera z użyciem SignalR 2 | Dokumentacja firmy Microsoft'
author: tdykstra
description: W tym samouczku przedstawiono sposób tworzenia aplikacji sieci web korzystającą z signalr2 na platformie ASP.NET w celu zapewnienia funkcji emisji serwera.
ms.author: bradyg
ms.date: 01/02/2019
ms.topic: tutorial
ms.assetid: 1568247f-60b5-4eca-96e0-e661fbb2b273
msc.legacyurl: /signalr/overview/getting-started/tutorial-server-broadcast-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: a243c78c7d552f1c82a88c6083871fcd16538618
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2019
ms.locfileid: "54837432"
---
# <a name="tutorial-server-broadcast-with-signalr-2"></a>Samouczek: Serwer emisji z SignalR 2

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

W tym samouczku przedstawiono sposób tworzenia aplikacji sieci web korzystającą z signalr2 na platformie ASP.NET w celu zapewnienia funkcji emisji serwera. Emisji serwera oznacza, że serwer jest uruchamiany komunikacji klientów.

Aplikacja, którą utworzysz w tym samouczku symuluje giełdowej typowy scenariusz emisji funkcje serwera. Okresowo serwer losowo aktualizuje giełdowych i aktualizacji można rozgłaszać do wszystkich połączonych klientów. W przeglądarce, cyfry i symbole w **zmienić** i **%** kolumn zmieniać dynamicznie w odpowiedzi na powiadomienia z serwera. Jeśli otworzysz dodatkowe przeglądarki pod kątem tego samego adresu URL, wszystkie one pokazywane te same dane i te same zmiany w danych jednocześnie.

![Tworzenie sieci web](tutorial-server-broadcast-with-signalr/_static/image1.png)

W ramach tego samouczka możesz:

> [!div class="checklist"]
> * Utwórz projekt
> * Konfigurowanie kodu serwera
> * Badanie kodu serwera
> * Ustaw kod klienta
> * Badanie kodu klienta
> * Testowanie aplikacji
> * Włącz rejestrowanie

> [!IMPORTANT]
> Jeśli nie chcesz pracować, kolejne kroki tworzenia aplikacji, można zainstalować pakietu SignalR.Sample w nowym projekcie pusta aplikacja sieci Web platformy ASP.NET. Po zainstalowaniu pakietu NuGet bez wykonywania czynności w ramach tego samouczka należy postępuj zgodnie z instrukcjami w *readme.txt* pliku. Aby uruchomić pakiet, należy dodać początkowa OWIN klasy która wywołuje metodę `ConfigureSignalR` metody w zainstalowanym pakietem. Zostanie wyświetlony błąd, jeśli nie dodasz klasy początkowej OWIN. Zobacz [zainstalować przykład StockTicker](#install-the-stockticker-sample) dalszej części tego artykułu.


## <a name="prerequisites"></a>Wymagania wstępne

 * [Program Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) z **ASP.NET i tworzenie aplikacji internetowych** obciążenia.

## <a name="create-the-project"></a>Utwórz projekt

W tej sekcji pokazano, jak utworzyć pustą aplikację sieci Web ASP.NET za pomocą programu Visual Studio 2017.

1. W programie Visual Studio należy utworzyć aplikację sieci Web platformy ASP.NET.

    ![Tworzenie sieci web](tutorial-server-broadcast-with-signalr/_static/image2.png)

1. W **nowej aplikacji sieci Web ASP.NET - SignalR.StockTicker** okna, pozostaw **pusty** zaznaczone, a następnie wybierz **OK**.

## <a name="set-up-the-server-code"></a>Konfigurowanie kodu serwera

W tej sekcji służy do konfigurowania kodu, który działa na serwerze.

### <a name="create-the-stock-class"></a>Tworzenie klasy zasobów

Rozpocznij od utworzenia *Stock* modelu klasy, które będzie używane do przechowywania i przesyłania informacji dotyczących danego zasobu.

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj** > **klasy**.

1. Nazwa klasy *Stock* i dodaj go do projektu.

1. Zastąp kod w *Stock.cs* pliku przy użyciu tego kodu:

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample1.cs)]

    Dwie właściwości, które zostaną ustawione podczas tworzenia zasobów `Symbol` (na przykład MSFT dla firmy Microsoft) i `Price`. Inne właściwości zależą od tego, jak i kiedy ustawisz `Price`. Możesz ustawić po raz pierwszy `Price`, wartość pobiera propagowane do `DayOpen`. Po tym, gdy ustawisz `Price`, aplikacja oblicza `Change` i `PercentChange` wartości właściwości na podstawie różnicy między `Price` i `DayOpen`.

### <a name="create-the-stocktickerhub-and-stockticker-classes"></a>Tworzenie klasy StockTickerHub i StockTicker

Za pomocą interfejsu API Centrum SignalR będzie obsługiwać interakcji z serwera do klienta. A `StockTickerHub` klasę pochodzącą od `SignalRHub` klasy będzie obsługiwać odbieranie wywołań metod i połączenia od klientów. Należy również utrzymania danych giełdowych i uruchom `Timer` obiektu. `Timer` Obiektu okresowo spowoduje wyzwolenie aktualizacji cen niezależne od połączeń klientów. Nie można umieścić te funkcje w `Hub` klasy, ponieważ przejściowy koncentratorów. Aplikacja tworzy `Hub` wystąpienia klasy dla każdego zadania w Centrum, takich jak połączenia i wywołania od klienta do serwera. Dlatego mechanizm, który przechowuje dane zapasów, aktualizacji cen i emituje aktualizacji cen musi działać w osobnej klasy. Będzie nazwa klasy `StockTicker`.

![Emisja z StockTicker](tutorial-server-broadcast-with-signalr/_static/image3.png)

Chcesz tylko jedno wystąpienie `StockTicker` klasy są uruchamiane na serwerze, więc musisz skonfigurować odwołanie z każdej `StockTickerHub` wystąpienia do wzorca singleton `StockTicker` wystąpienia. `StockTicker` Klasa ma wysyłać do klientów, ponieważ ma danych podstawowych i wyzwala aktualizacje, ale `StockTicker` nie jest `Hub` klasy. `StockTicker` Klasy musi uzyskać odwołanie do obiektu kontekstu połączenia koncentratora SignalR. Można następnie użyć obiektu context połączenia SignalR do emisji przeznaczonych dla klientów.

#### <a name="create-stocktickerhubcs"></a>Create StockTickerHub.cs

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj** > **nowy element**.

1. W **Dodaj nowy element - SignalR.StockTicker**, wybierz opcję **zainstalowane** > **Visual C#**   >  **Web**  >  **SignalR** , a następnie wybierz **klasa Centrum SignalR (v2)**.

1. Nazwa klasy *StockTickerHub* i dodaj go do projektu.

    Spowoduje to utworzenie *StockTickerHub.cs* pliku klasy. Jednocześnie dodaje zestaw pliki skryptów i odwołania do zestawu, który obsługuje SignalR do projektu.

1. Zastąp kod w *StockTickerHub.cs* pliku przy użyciu tego kodu:

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample2.cs)]

1. Zapisz plik.

Ta aplikacja używa [Centrum](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx) klasy do definiowania metod klientów można wywołać na serwerze. Możesz zdefiniować jedną z metod: `GetAllStocks()`. Gdy klient początkowo łączy się z serwerem, wywoła tę metodę, aby uzyskać listę wszystkich zasobów z ich bieżącym ceny. Metoda mogą być uruchamiane synchronicznie i zwracać `IEnumerable<Stock>` ponieważ zwraca dane z pamięci.

Jeśli metoda musiały uzyskać danych, wykonując coś, co wymagałoby oczekiwania, takich jak wyszukiwania w bazie danych lub wywołanie usługi sieci web należy określić `Task<IEnumerable<Stock>>` jako wartości zwracanej, aby umożliwić przetwarzanie asynchroniczne. Aby uzyskać więcej informacji, zobacz [ASP.NET SignalR Podręcznik interfejsu API centrów — serwer — kiedy są wykonywane asynchronicznie](../guide-to-the-api/hubs-api-guide-server.md#asyncmethods).

`HubName` Atrybut określa, jak aplikacja będzie odwoływać się w Centrum w kodzie JavaScript na komputerze klienckim. Domyślna nazwa na kliencie, jeśli nie korzystasz z tego atrybutu to camelCase wersję nazwy klasy, która w tym przypadku wyniesie `stockTickerHub`.

Jak zobaczysz później podczas tworzenia `StockTicker` klasy, aplikacja tworzy pojedyncze wystąpienie tej klasy w jego statyczny `Instance` właściwości. To wystąpienie singleton `StockTicker` znajduje się w pamięci, niezależnie od tego, ilu klientów łączyć i rozłączać. To wystąpienie jest co `GetAllStocks()` metoda używa do zwracania bieżących informacji podstawowych.

#### <a name="create-stocktickercs"></a>Create StockTicker.cs

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj** > **klasy**.

1. Nazwa klasy *StockTicker* i dodaj go do projektu.

1. Zastąp kod w *StockTicker.cs* pliku przy użyciu tego kodu:

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample3.cs)]

Ponieważ wszystkie wątki będą uruchomione to samo wystąpienie elementu StockTicker kodu, klasa StockTicker musi być metodą o bezpiecznych wątkach.

### <a name="examine-the-server-code"></a>Badanie kodu serwera

Kod serwera podczas badania, pomoże Ci zrozumieć, jak działa aplikacja.

#### <a name="storing-the-singleton-instance-in-a-static-field"></a>Przechowywanie pojedyncze wystąpienie, w polu statycznym

Ten kod inicjalizuje statycznej `_instance` pola, która będzie tworzyć kopię `Instance` właściwości przy użyciu wystąpienia klasy. Ponieważ Konstruktor jest prywatny, jest tylko wystąpienia klasy, które można utworzyć aplikacji. Ta aplikacja używa [inicjowania z opóźnieniem](/dotnet/framework/performance/lazy-initialization) dla `_instance` pola. Nie jest ze względu na wydajność. Jest upewnij się, że tworzenie wystąpienia jest bezpieczna dla wątków.

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample4.cs)]

Każdorazowo, klient nawiąże połączenie z serwerem, nowe wystąpienie klasy StockTickerHub działające w oddzielnym wątku pobiera StockTicker pojedyncze wystąpienie z `StockTicker.Instance` właściwość statyczna jak pokazano wcześniej w `StockTickerHub` klasy.

#### <a name="storing-stock-data-in-a-concurrentdictionary"></a>Przechowywanie danych giełdowych w ConcurrentDictionary

Konstruktor inicjuje `_stocks` kolekcji z pewnymi przykładowymi danymi zapasów i `GetAllStocks` zwraca zasobów. Jak wcześniej, to zbiór zasobów jest zwracany przez `StockTickerHub.GetAllStocks`, czyli metody serwera w `Hub` klasę, która może wywołać klientów.

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample5.cs)]

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample6.cs)]

Kolekcja zasobów jest zdefiniowana jako [ConcurrentDictionary](https://msdn.microsoft.com/library/dd287191.aspx) typu pod kątem bezpieczeństwa wątków. Alternatywnie, można użyć [słownika](https://msdn.microsoft.com/library/xfhwa508.aspx) obiektu i jawnie zablokować słownika, po wprowadzeniu zmian do niego.

Ta przykładowa aplikacja OK do przechowywania danych aplikacji w pamięci i jest do utraty danych, gdy aplikacja usuwa `StockTicker` wystąpienia. W rzeczywistej aplikacji będzie działać z magazynem danych zaplecza, takich jak bazy danych.

#### <a name="periodically-updating-stock-prices"></a>Okresowo aktualizowanie cen akcji

Konstruktor jest uruchamiany `Timer` obiekt, który okresowo wywołuje metody, które aktualizują cen akcji na podstawie losowej.

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample7.cs)]

 `Timer` wywołania `UpdateStockPrices`, które przechodzą w wartości null w parametrze state. Przed zaktualizowaniem ceny, aplikacja przejmuje blokadę `_updateStockPricesLock` obiektu. Sprawdza kod, jeśli inny wątek już trwa aktualizowanie cen, a następnie wywołuje `TryUpdateStockPrice` na każdej akcji, na liście. `TryUpdateStockPrice` Metoda określa, czy należy zmienić najniższej ceny i ilości go zmienić. Jeśli zmieni się cena akcji w aplikacji jest nazywana `BroadcastStockPrice` do emisji przeznaczonych jest zmiana najniższej ceny akcji dla wszystkich połączonych klientów.

`_updatingStockPrices` Flagi wyznaczony [volatile](https://msdn.microsoft.com/library/x13ttww7.aspx) aby upewnić się, jest metodą o bezpiecznych wątkach.

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample8.cs)]

W rzeczywistej aplikacji `TryUpdateStockPrice` metoda będzie wywołać usługę sieci web, aby wyszukać cena. W tym kodzie aplikacja używa generator liczb losowych, aby wprowadzić zmiany w losowo.

#### <a name="getting-the-signalr-context-so-that-the-stockticker-class-can-broadcast-to-clients"></a>Wprowadzenie kontekstu SignalR, tak aby klasy StockTicker można rozgłaszać do klientów

Ponieważ zmiany cen poniżej pochodzą z `StockTicker` obiektu, jest to obiekt, który musi wywołać `updateStockPrice` metody na wszystkich połączonych klientów. W `Hub` klasy, masz interfejs API do wywoływania metody klientów, ale `StockTicker` nie pochodzi od `Hub` klasy, a nie ma odniesienia do dowolnych `Hub` obiektu. Do emisji przeznaczonych do połączonych klientów `StockTicker` klasa ma wystąpienia kontekstu SignalR dla `StockTickerHub` klasy i używać go do wywoływania metod na komputerach klienckich.

Ten kod pobiera odwołanie do kontekstu SignalR, podczas tworzenia wystąpienia klasy pojedyncze, przebiegi, które odwołują się do konstruktora, i umieszcza go w Konstruktorze `Clients` właściwości.

Istnieją dwie przyczyny, dlaczego chcesz uzyskać kontekst tylko raz: Pobieranie kontekstu jest kosztowne zadania oraz pobierania ich po sprawia, że się, że aplikacja zachowuje zalecanej kolejności wiadomości wysyłane do klientów.

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample9.cs)]

Wprowadzenie `Clients` właściwość kontekstu i umieszczenie go w `StockTickerClient` właściwość umożliwia pisanie kodu w celu wywołania metody klienta, który wygląda tak samo jak w `Hub` klasy. Na przykład napisać rozgłaszać do wszystkich klientów `Clients.All.updateStockPrice(stock)`.

`updateStockPrice` Metodę, która jest wywoływana w `BroadcastStockPrice` jeszcze nie istnieje. Należy dodać go później podczas pisania kodu, który jest uruchamiany na kliencie. Możesz zapoznać się z `updateStockPrice` tutaj ponieważ `Clients.All` jest dynamiczną, która oznacza, że aplikacja będzie oceniać wyrażenia w czasie wykonywania. Po wykonaniu wywołanie tej metody, SignalR wyśle nazwy metody i wartość parametru do klienta, a jeśli klient ma metodę o nazwie `updateStockPrice`, aplikacja wywoła tę metodę i przekaż wartość parametru do niego.

`Clients.All` oznacza, że wysyłać do wszystkich klientów. SignalR udostępnia innych opcji, aby określić, które klientów lub grup klientów, aby wysyłać. Aby uzyskać więcej informacji, zobacz [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx).

### <a name="register-the-signalr-route"></a>Zarejestruj trasy SignalR

Serwer musi znać adres URL, który można przechwycić i kierują je bezpośrednio z SignalR. Aby to zrobić, Dodaj klasę początkową OWIN:

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj** > **nowy element**.

1. W **Dodaj nowy element - SignalR.StockTicker** wybierz **zainstalowane** > **Visual C#**   >  **Web** i następnie wybierz pozycję **klasy początkowej OWIN**.

1. Nazwa klasy *uruchamiania* i wybierz **OK**.

1. Zastąp kod domyślne *Startup.cs* pliku przy użyciu tego kodu:

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample10.cs)]

Konfigurowanie kodu serwera zostało zakończone. W następnej sekcji należy skonfigurować klienta.

## <a name="set-up-the-client-code"></a>Ustaw kod klienta

W tej sekcji służy do konfigurowania kod, który jest uruchamiany na kliencie.

### <a name="create-the-html-page-and-javascript-file"></a>Utwórz stronę HTML i plik JavaScript

Strony HTML będą wyświetlane dane i plik JavaScript będzie organizowania danych.

#### <a name="create-stocktickerhtml"></a>Create StockTicker.html

Najpierw należy dodać klienta HTML.

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj** > **strony HTML**.

1. Nadaj plikowi nazwę *StockTicker* i wybierz **OK**.

1. Zastąp kod domyślne *StockTicker.html* pliku przy użyciu tego kodu:

    [!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample11.html?highlight=40-43)]

    Kod HTML tworzy tabelę z pięciu kolumn, wiersz nagłówka i wiersz danych z pojedynczą komórkę, która obejmuje wszystkie kolumny pięć. Wiersz danych zawiera "Trwa ładowanie..." chwilowo, po uruchomieniu aplikacji. Kod JavaScript spowoduje usunięcie tego wiersza i dodać w jej miejscu wiersze z danych giełdowych pobrany z serwera.

    Określ tagi skryptu:

    * Plik skryptu jQuery.

    * Plik skryptu core SignalR.

    * Plik skryptu proxy SignalR.

    * Plik skryptu StockTicker, którą utworzysz później.

    Aplikacja dynamicznie generuje plik skryptu proxy SignalR. Określa adres URL "/ signalr/centra" i definiuje metody serwera proxy dla metody na klasy koncentratora, w tym przypadku `StockTickerHub.GetAllStocks`. Jeśli wolisz, możesz wygenerować ten plik JavaScript ręcznie przy użyciu [narzędzia SignalR](http://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/). Nie należy zapominać wyłączyć tworzenie dynamicznych plików w `MapHubs` wywołania metody.

1. W **Eksploratora rozwiązań**, rozwiń węzeł **skrypty**.

    Biblioteki skryptów, jQuery i SignalR są widoczne w projekcie.

    > [!IMPORTANT]
    > Menedżer pakietów zainstaluje późniejszą wersję skrypty SignalR.

1. Aktualizuj odwołania do skryptu w bloku kodu, aby odpowiadać wersji plików skrypt w projekcie.

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *StockTicker.html*, a następnie wybierz pozycję **Ustaw jako strona startowa**.

#### <a name="create-stocktickerjs"></a>Create StockTicker.js

Teraz można utworzyć pliku JavaScript.

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj** > **plik JavaScript**.

1. Nadaj plikowi nazwę *StockTicker* i wybierz **OK**.

1. Dodaj następujący kod do *StockTicker.js* pliku:

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample12.js)]

### <a name="examine-the-client-code"></a>Badanie kodu klienta

Jeśli możesz zbadać kod klienta, pomoże Ci dowiedzieć się, jak kod klienta współdziała z kod serwera, aby aplikacja pracuje.

#### <a name="starting-the-connection"></a>Zainicjowanie połączenia

`$.connection` odnosi się do serwerów proxy SignalR. Ten kod pobiera odwołanie do serwera proxy dla `StockTickerHub` klasy i umieszcza go w `ticker` zmiennej. Nazwa serwera proxy jest nazwa, która została ustawiona przez `HubName` atrybutu:

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample13.js)]

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample14.cs)]

Po zdefiniowaniu wszystkich zmiennych i funkcji, ostatni wiersz kodu w pliku inicjuje połączenia SignalR, wywołując SignalR `start` funkcji. `start` Funkcja wykonuje asynchronicznie i zwraca [jQuery opóźnionych obiektu](http://api.jquery.com/category/deferred-object/). Można wywołać funkcję gotowe do określenia funkcji do wywołania po jej zakończeniu akcję asynchroniczną.

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample15.js)]

#### <a name="getting-all-the-stocks"></a>Wprowadzenie do zasobów

`init` Wywołaniach funkcji `getAllStocks` funkcji na serwerze i używa tych informacji, serwer zwraca można zaktualizować podstawowego tabeli. Należy zauważyć, że domyślnie, trzeba użyć camelCasing na kliencie, nawet jeśli nazwa metody jest pascal — z uwzględnieniem wielkości liter na serwerze. Reguła camelCasing dotyczy tylko metody, a nie obiektów. Na przykład, możesz odwołać się do `stock.Symbol` i `stock.Price`, a nie `stock.symbol` lub `stock.price`.

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample16.js)]

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample17.cs)]

W `init` metody, aplikacja tworzy HTML dla wiersza tabeli, dla każdego obiektu podstawowego otrzymany z serwera, wywołując `formatStock` format właściwości `stock` obiektu, a następnie przez wywołanie `supplant` zastąpić symbole zastępcze w wywołaniach `rowTemplate` zmiennej za pomocą `stock` wartości właściwości obiektu. Wynikowy kod HTML, następnie jest dołączany do podstawowego tabeli.

> [!NOTE]
> Należy wywołać `init` przez przekazanie jej jako `callback` funkcji, który jest wykonywany po asynchroniczną `start` funkcji zostanie zakończone. Jeśli wywołujesz `init` jako osobne instrukcja kodu JavaScript po wywołaniu `start`, funkcja może zakończyć się niepowodzeniem, ponieważ aplikacja może działać natychmiast bez oczekiwania na funkcję start, aby zakończyć ustanawiania połączenia. W takim przypadku `init` próbowała funkcja do wywołania `getAllStocks` działać, zanim aplikacja nawiązuje połączenie z serwerem.

#### <a name="getting-updated-stock-prices"></a>Pobieranie zaktualizowanego cen akcji

Po zmianie cen zasobów serwera wywoływanych przez nią `updateStockPrice` na połączonych klientów. Aplikacja dodaje funkcję do właściwości klienta `stockTicker` serwera proxy, aby udostępnić ją do wywołań z serwera.

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample18.js)]

`updateStockPrice` Formaty funkcja obiektem magazynowym otrzymany z serwera do tabeli Wiersz taki sam sposób jak w `init` funkcji. Zamiast dołączać wiersz do tabeli, wyszukuje stock bieżącego wiersza w tabeli i zamienia nowego wiersza.

## <a name="test-the-application"></a>Testowanie aplikacji

Można przetestować aplikację, aby upewnić się, że działa. Zostaną wyświetlone wszystkie okna przeglądarki, Wyświetl tabeli podstawowe na żywo z cen akcji zmienne.

1. Na pasku narzędzi, Włącz **debugowanie skryptu** a następnie wybierz przycisk Odtwórz, aby uruchomić aplikację w trybie debugowania.

    ![Zrzut ekranu przedstawiający użytkownika, włączając tryb debugowania i wybierając polecenie play.](tutorial-server-broadcast-with-signalr/_static/image4.png)

    Zostanie otwarte okno przeglądarki, wyświetlanie **Live tabeli Stock**. Tabela podstawowych początkowo jest widocznych wiersza "Trwa ładowanie...", następnie po pewnym czasie aplikacja będzie wyświetlana początkowej danych podstawowych i zacznij cen akcji można zmienić.

1. Skopiuj adres URL w przeglądarce otwórz dwóch innych przeglądarek i wklejać adresy URL paski adresu.

    Początkowe wyświetlanie podstawowych jest taki sam jak pierwszy przeglądarki i jednocześnie wprowadzenia zmiany.

1. Zamknij wszystkie przeglądarki, Otwórz w nowym oknie przeglądarki i przejdź do tego samego adresu URL.

    Nadal StockTicker pojedynczego obiektu do uruchomienia na serwerze. **Live tabeli Stock** pokazuje, które nadal zasobów można zmienić. Nie widzisz początkowa tabela o wartości zero, zmień wartości.

1. Zamknij przeglądarkę.

## <a name="enable-logging"></a>Włącz rejestrowanie

SignalR ma funkcję wbudowane funkcje rejestrowania, który można włączyć na kliencie, aby ułatwić rozwiązywanie problemów. W tej sekcji, Włącz rejestrowanie i zapoznaj się z przykładami, które pokazują, jak dzienniki informujące, który z poniższych metod transportu używa SignalR:

* [Gniazda Websocket](http://en.wikipedia.org/wiki/WebSocket)obsługiwany przez usługi IIS 8 i bieżącej przeglądarki.

* [Serwer wysłał zdarzenia](http://en.wikipedia.org/wiki/Server-sent_events), obsługiwane przez przeglądarki innej niż Internet Explorer.

* [Nieskończona ramki](http://en.wikipedia.org/wiki/Comet_(programming)#Hidden_iframe)obsługiwany przez program Internet Explorer.

* [AJAX długim sondowaniem](http://en.wikipedia.org/wiki/Comet_(programming)#Ajax_with_long_polling)obsługiwany przez wszystkie przeglądarki.

Dla dowolnego danego połączenia SignalR wybiera najlepszą metodą transportu, który obsługuje zarówno na serwerze, jak i klienta.

1. Open *StockTicker.js*.

1. Dodaj ten wyróżniony wiersz kodu, aby włączyć rejestrowanie bezpośrednio przed kod, który inicjuje połączenie z końcem pliku:

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample19.js?highlight=2)]

1. Naciśnij klawisz **F5** Aby uruchomić projekt.

1. Otwórz okno narzędzi programistycznych w przeglądarce, a następnie wybierz konsolę, aby wyświetlić dzienniki. Trzeba będzie odświeżyć stronę, aby wyświetlić dzienniki negocjowania metodę transportu do nowego połączenia SignalR.

    * Jeśli korzystasz z programu Internet Explorer 10 w systemie Windows 8 (IIS 8), jest metodą transportu **WebSockets**.

    * Jeśli korzystasz z programu Internet Explorer 10, Windows 7 (usługi IIS 7.5), jest metodą transportu **iframe**.

    * Jeśli korzystasz z 19 przeglądarki Firefox w systemie Windows 8 (IIS 8), jest metodą transportu **WebSockets**.

        > [!TIP]
        > W przeglądarce Firefox Zainstaluj dodatek Firebug można pobrać z okna konsoli.

    * Jeśli korzystasz z Firefox 19, Windows 7 (usługi IIS 7.5), jest metodą transportu **serwer wysłał** zdarzenia.

## <a name="install-the-stockticker-sample"></a>Instalowanie przykładowej StockTicker

[Microsoft.AspNet.SignalR.Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) instaluje aplikację StockTicker. Pakiet NuGet zawiera więcej funkcji niż uproszczonej wersji, który został utworzony od zera. W tej części samouczka zainstaluj pakiet NuGet i Przejrzyj nowe funkcje i kod, który implementuje je.

> [!IMPORTANT]
> Po zainstalowaniu pakietu bez przeprowadzania wcześniejsze kroki tego samouczka, należy dodać klasę początkową OWIN do projektu. Ten plik readme.txt dla pakietu NuGet wyjaśnia, w tym kroku.

### <a name="install-the-signalrsample-nuget-package"></a>Zainstaluj pakiet SignalR.Sample NuGet

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt i wybierz **Zarządzaj pakietami NuGet**.

1. W **Menedżera pakietów NuGet: SignalR.StockTicker**, wybierz opcję **Przeglądaj**.

1. Z **źródła pakietu**, wybierz opcję **nuget.org**.

1. Wprowadź *SignalR.Sample* w polu wyszukiwania i wybierz pozycję **Microsoft.AspNet.SignalR.Sample** > **zainstalować**.

1. W **Eksploratora rozwiązań**, rozwiń węzeł *SignalR.Sample* folderu.

    Instalowanie pakietu SignalR.Sample utworzony folder i jego zawartość.

1. W *SignalR.Sample* folderu, kliknij prawym przyciskiem myszy *StockTicker.html*, a następnie wybierz pozycję **Ustaw jako stronę startową**.

    > [!NOTE]
    > Instalowanie SignalR.Sample NuGet pakietu może zmienić wersję jQuery, które mają w swojej *skrypty* folderu. Nowy *StockTicker.html* pliku, który pakiet instaluje w *SignalR.Sample* folder będzie zsynchronizowany z wersją jQuery instalująca pakiet, ale jeśli chcesz uruchomić oryginalny *StockTicker.html* ponownie plik, trzeba najpierw zaktualizuj odwołanie jQuery w tagu.

### <a name="run-the-application"></a>Uruchamianie aplikacji

 Tabelę, która działa w pierwszej aplikacji ma użytecznych funkcji. Aplikacja pełnej giełdowej przedstawiono nowe funkcje: okna przewijania w poziomie, pokazujący danych giełdowych i zasobów, które zmieniają kolor, jak długo będą rosnąć i dzielą się.

1. Naciśnij klawisz **F5** do uruchomienia aplikacji.

     Po uruchomieniu aplikacji po raz pierwszy "na rynku" to "zamknięte", a zobaczysz tabelę statyczną i okna znacznika, który nie jest przewijanie.

1. Wybierz **wolnym rynku**.

    ![Zrzut ekranu przedstawiający znacznika na żywo.](tutorial-server-broadcast-with-signalr/_static/image5.png)

    * **Live znacznika Stock** pole zaczyna być przewijane w poziomie, a serwer jest uruchamiany okresowo wysyłać zmian cen akcji na podstawie losowej.

    * Każdorazowo cena akcji zmiany, aplikacja aktualizuje zarówno **Live tabeli Stock** i **Live znacznika Stock**.

    * Gdy zmian cen zasobów jest dodatnia, aplikacja będzie wyświetlana cena akcji z zielonym tłem.

    * Gdy ta zmiana jest ujemna, stock czerwonym tle wyświetlany przez aplikację.

1. Wybierz **Zamknij rynku**.

    * Tabela aktualizacji zatrzymania.

    * Znacznika zatrzymuje przewijania.

1. Wybierz **resetowania**.

    * Wszystkie dane podstawowe jest resetowany.

    * Aplikacja przywraca stan początkowy przed wprowadzeniem zmian cen pracę.

1. Skopiuj adres URL w przeglądarce otwórz dwóch innych przeglądarek i wklejać adresy URL paski adresu.

1. Możesz zobaczyć te same dane, które są aktualizowane dynamicznie w tym samym czasie w każdej przeglądarce.

1. Po wybraniu dowolnej kontrolki wszystkie przeglądarki odpowiedzieć ten sam sposób, w tym samym czasie.

### <a name="live-stock-ticker-display"></a>Na żywo wyświetlanie znacznika zapasów

**Live znacznika Stock** wyświetlana jest Lista nieuporządkowana w `<div>` element sformatowane w jednej linii za style CSS. Inicjuje i aktualizuje znacznika taki sam sposób jak tabela aplikacji:, zastępując symbole zastępcze w wywołaniach `<li>` ciąg szablonu i dynamiczne dodawanie `<li>` elementów `<ul>` elementu. Aplikacja zawiera przewijanie przy użyciu jQuery `animate` funkcji różnią się w lewej margines nieuporządkowaną listę w obrębie `<div>`.

#### <a name="signalrsample-stocktickerhtml"></a>SignalR.Sample StockTicker.html

Giełdowej kodu HTML:

[!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample20.html)]

#### <a name="signalrsample-stocktickercss"></a>SignalR.Sample StockTicker.css

Giełdowej kod CSS:

[!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample21.html)]

#### <a name="signalrsample-signalrstocktickerjs"></a>SignalR.Sample SignalR.StockTicker.js

Przewiń kodu jQuery, który sprawia, że:

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample22.js)]

### <a name="additional-methods-on-the-server-that-the-client-can-call"></a>Dodatkowe metody na serwerze, który klient może wywołać

Aby dodać elastyczność do aplikacji, istnieją dodatkowe metody, które aplikacja może wywołać.

#### <a name="signalrsample-stocktickerhubcs"></a>SignalR.Sample StockTickerHub.cs

`StockTickerHub` Klasa definiuje cztery dodatkowe metody, które klient może wywoływać:

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample23.cs)]

Wywołania aplikacji `OpenMarket`, `CloseMarket`, i `Reset` w odpowiedzi na przycisków w górnej części strony. Pokazują one wzorca jednego klienta, wyzwalając zmianę stanu natychmiast propagowane do wszystkich klientów. Każda z tych metod wywołuje metodę `StockTicker` klasę, która powoduje, że zmiana stanu na rynku, a następnie emituje nowy stan.

#### <a name="signalrsample-stocktickercs"></a>SignalR.Sample StockTicker.cs

W `StockTicker` klasy, aplikacja przechowuje stan rynku dzięki `MarketState` właściwość, która zwraca `MarketState` wartość wyliczenia:

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample24.cs)]

Każdej z metod, które zmieniają stan rynku zrobić wewnątrz bloku blokady ponieważ `StockTicker` klasy musi być metodą o bezpiecznych wątkach:

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample25.cs)]

Aby upewnić się, że ten kod jest bezpieczna dla wątków, `_marketState` pola, która będzie tworzyć kopię `MarketState` wyznaczona właściwość `volatile`:

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample26.cs)]

`BroadcastMarketStateChange` i `BroadcastMarketReset` metody są podobne do metody BroadcastStockPrice, która już działa, z wyjątkiem wywołują różnych metod, zdefiniowanych na komputerze klienckim:

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample27.cs)]

### <a name="additional-functions-on-the-client-that-the-server-can-call"></a>Dodatkowe funkcje, na komputerze klienckim, który można wywoływać serwera

`updateStockPrice` Funkcja obsługuje teraz zarówno w tabeli, jak i wyświetlanie znacznika, przy czym `jQuery.Color` do flash czerwonego i zielonego kolorów.

Nowe funkcje w *SignalR.StockTicker.js* Włączanie i wyłączanie przycisków na podstawie stanu rynku. Mogą również zatrzymać lub rozpocząć **Live znacznika Stock** przewijanie w poziomie. Ponieważ wiele funkcji są dodawane do `ticker.client`, ta aplikacja używa [jQuery rozszerzanie funkcji](http://api.jquery.com/jQuery.extend/) je dodać.

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample28.js)]

### <a name="additional-client-setup-after-establishing-the-connection"></a>Instalacja klienta dodatkowe po ustanowieniu połączenia

Po nawiązaniu połączenia przez klienta ma pewne dodatkowe zadania do wykonania:

* Dowiedz się, czy rynku jest otwarte lub zamknięte do wywołania odpowiednie `marketOpened` lub `marketClosed` funkcji.

* Dołącz wywołania metody serwera do przycisków.

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample29.js)]

Metody serwera nie są powiązaną przycisków do momentu, po Aplikacja nawiązuje połączenie. To dlatego kod nie można wywołać metody serwera, zanim staną się dostępne.

## <a name="additional-resources"></a>Dodatkowe zasoby

W tym samouczku wyjaśniono sposób programowania aplikacji SignalR, który emituje komunikaty z serwera do wszystkich połączonych klientów. Teraz może emitować wiadomości w regularnych odstępach czasu i odpowiedzi na powiadomienia za pomocą dowolnego klienta. Pojęcie wielowątkowych pojedyncze wystąpienie służy do zarządzania stanem serwera w trybie online scenariuszach gier wielu graczy. Aby uzyskać przykład, zobacz [ShootR gry oparte na SignalR](https://github.com/NTaylorMullen/ShootR).

Samouczki, które pokazują scenariuszy komunikacji między peer-to-peer, zobacz [wprowadzenie do SignalR](introduction-to-signalr.md) i [aktualizacji w czasie rzeczywistym, przy użyciu SignalR](tutorial-high-frequency-realtime-with-signalr.md).

Aby uzyskać więcej informacji na temat biblioteki SignalR zobacz następujące zasoby:

* [ASP.NET SignalR](../../index.md)
* [Projekt SignalR](http://signalr.net/)
* [SignalR GitHub i przykłady](https://github.com/SignalR/SignalR)
* [Witryny typu Wiki biblioteki SignalR](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a>Następne kroki

W ramach tego samouczka możesz:

> [!div class="checklist"]
> * Tworzenia projektu
> * Konfigurowanie kodu serwera
> * Zbadanie kodu serwera
> * Ustaw kod klienta
> * Zbadanie kodu klienta
> * Testowanie aplikacji
> * Rejestrowanie włączone

Przejdź do następnego artykułu, aby dowiedzieć się, jak utworzyć aplikację sieci web w czasie rzeczywistym, korzystającą z signalr2 na platformie ASP.NET.
> [!div class="nextstepaction"]
> [Tworzenie aplikacji sieci web w czasie rzeczywistym przy użyciu biblioteki SignalR](real-time-web-applications-with-signalr.md)