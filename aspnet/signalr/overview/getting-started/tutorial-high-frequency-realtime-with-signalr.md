---
uid: signalr/overview/getting-started/tutorial-high-frequency-realtime-with-signalr
title: 'Samouczek: Wysokiej częstotliwości z czasu rzeczywistego z SignalR 2 | Dokumentacja firmy Microsoft'
author: pfletcher
description: W tym samouczku przedstawiono sposób tworzenia aplikacji sieci web, który używa biblioteki SignalR platformy ASP.NET w celu zapewnienia wysokiej częstotliwości z obsługą wiadomości. Wysokiej częstotliwości wiadomości w...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 9f969dda-78ea-4329-b1e3-e51c02210a2b
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/getting-started/tutorial-high-frequency-realtime-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: ab051b2ab85d1aac1e7179f342f22f470b1d1cc7
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="tutorial-high-frequency-realtime-with-signalr-2"></a>Samouczek: Wysokiej częstotliwości z czasu rzeczywistego z SignalR 2
====================
przez [Patrick Fletcher](https://github.com/pfletcher)

[Pobieranie ukończone projektu](http://code.msdn.microsoft.com/SignalR-20-MoveShape-Demo-6285b83a)

> W tym samouczku przedstawiono sposób tworzenia aplikacji sieci web, który używa programu ASP.NET SignalR 2 umożliwiają korzystanie z funkcji obsługi komunikatów o dużej częstotliwości. W takim przypadku wiadomości o dużej częstotliwości oznacza aktualizacje, które są wysyłane według stałej stawki ustalanej; w przypadku tej aplikacji, maksymalnie 10 komunikatów na sekundę.
> 
> Aplikacji, które zostaną utworzone w tym samouczku Wyświetla kształtu, który można przeciągnąć użytkowników. Położenie kształtu w innych przeglądarkach połączonych zostaną zaktualizowane do dopasowania pozycja przeciąganego kształtu przy użyciu czasu aktualizacji.
> 
> Pojęciami opisanymi w tym samouczku zostały aplikacji w czasie rzeczywistym gry i innych aplikacji symulacji.
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

Ten samouczek przedstawia sposób tworzenia aplikacji, który współużytkuje stan obiektu z innych przeglądarek w czasie rzeczywistym. Aplikacja, którą utworzymy nosi nazwę MoveShape. Na stronie MoveShape wyświetli element HTML Div, który użytkownik może przeciągać; gdy użytkownik przeciąga Div, jego nowego położenia będą wysyłane do serwera, który poinformuje wszystkich pozostałych klientów podłączonych do zaktualizowania pozycji kształtu do dopasowania.

![W oknie aplikacji](tutorial-high-frequency-realtime-with-signalr/_static/image1.png)

Aplikacji utworzonej w tym samouczku opiera się na pokaz przez Dyszkiewicz Damianowi. Film wideo zawierający ten pokaz są widoczne [tutaj](https://channel9.msdn.com/Series/Building-Web-Apps-with-ASP-NET-Jump-Start/Building-Web-Apps-with-ASPNET-Jump-Start-08-Real-time-Communication-with-SignalR).

Samouczek zostanie uruchomiony przez pokazująca, jak wysyłać SignalR z każdym Zdarzenie uruchamiane jako kształt zostanie przeciągnięty. Każdy połączony klient następnie zaktualizuje pozycja lokalnej wersji kształtu zawsze, gdy wiadomość zostanie odebrana.

Gdy aplikacja będzie działać, za pomocą tej metody, nie jest zalecane model programowania, ponieważ nie byłoby górny limit liczby wiadomości wysłane pierwsze, klientów i serwera można uzyskać przeciążony komunikatów, i może zmniejszyć wydajność . Również będą wyświetlane animacji na kliencie, odłączony, jak kształt będzie można przenieść natychmiast przez każdą z tych metod, a nie przenoszenie klucz do każdej nowej lokalizacji. Kolejnych sekcjach samouczka przedstawiono sposób tworzenia funkcji czasomierza, która ogranicza maksymalna szybkość z jaką komunikaty są wysyłane przez klienta lub serwera i jak przenieść kształt sprawnie między lokalizacjami. Z ostateczną wersją aplikacji utworzonej w tym samouczku można pobrać z [galerii kodu](https://code.msdn.microsoft.com/SignalR-20-MoveShape-Demo-6285b83a).

Ten samouczek zawiera następujące sekcje:

- [Wymagania wstępne](#prerequisites)
- [Tworzenie projektu i dodawanie pakietu SignalR i JQuery.UI NuGet](#createtheproject2013)
- [Tworzenie podstawowej aplikacji](#baseapp)
- [Uruchamianie koncentratora, podczas uruchamiania aplikacji](#startup2013)
- [Dodaj pętlę klienta](#clientloop)
- [Dodaj pętlę serwera](#serverloop)
- [Dodaj smooth animacji na kliencie](#animation)
- [Dodatkowe kroki](#furthersteps)

<a id="prerequisites"></a>

## <a name="prerequisites"></a>Wymagania wstępne

Ten samouczek wymaga programu Visual Studio 2013.

<a id="createtheproject2013"></a>

## <a name="create-the-project-and-add-the-signalr-and-jqueryui-nuget-package"></a>Tworzenie projektu i dodawanie pakietu SignalR i JQuery.UI NuGet

W tej sekcji utworzymy projektu programu Visual Studio 2013.

Następujące kroki Użyj programu Visual Studio 2013, aby utworzyć pustą aplikację sieci Web ASP.NET i dodać biblioteki SignalR i jQuery.UI:

1. W programie Visual Studio Utwórz aplikację sieci Web platformy ASP.NET.

    ![Tworzenie sieci web](tutorial-high-frequency-realtime-with-signalr/_static/image2.png)
2. W **nowy projekt ASP.NET** okna, pozostaw **pusty** zaznaczone, a następnie kliknij przycisk **tworzenia projektu**.

    ![Utwórz pusty sieci web](tutorial-high-frequency-realtime-with-signalr/_static/image3.png)
3. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt, wybierz **Dodaj | Klasy koncentratora SignalR (v2)**. Nazwa klasy **MoveShapeHub.cs** i dodaj go do projektu. Spowoduje to utworzenie **MoveShapeHub** i dodaje do projektu zestawu plików skryptów i odwołania do zestawów, które obsługują SignalR.

    > [!NOTE]
    > SignalR można dodać do projektu, klikając **narzędzia | Menedżer pakietów biblioteki | Konsola Menedżera pakietów** i uruchamiając polecenie:

    `install-package Microsoft.AspNet.SignalR`. 

    Jeśli używasz konsoli można dodać SignalR, należy utworzyć klasę koncentratora SignalR w osobnym kroku po dodaniu SignalR.
4. Kliknij przycisk **narzędzia | Menedżer pakietów biblioteki | Konsola Menedżera pakietów**. W oknie Menedżera pakietów uruchom następujące polecenie:

    `Install-Package jQuery.UI.Combined`

    Spowoduje to zainstalowanie biblioteki interfejsu użytkownika jQuery, który zostanie użyty do animowania kształtu.
5. W **Eksploratora rozwiązań** rozwiń węzeł skryptów. Skrypt biblioteki SignalR, jQuery i jQueryUI są widoczne w projekcie.

    ![Odwołania do skryptu biblioteki](tutorial-high-frequency-realtime-with-signalr/_static/image4.png)

<a id="baseapp"></a>

## <a name="create-the-base-application"></a>Tworzenie podstawowej aplikacji

W tej sekcji utworzymy aplikacji przeglądarki, która wysyła lokalizacji kształtu do serwera podczas każdego zdarzenie przesunięcia kursora myszy. Serwer wysyła następnie te informacje do wszystkich innych połączonych klientów odbierane. Firma Microsoft będzie rozwiń w tej aplikacji w kolejnych sekcjach.

1. Jeśli jeszcze nie utworzono klasy MoveShapeHub.cs w **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj**, **klasy...** . Nazwa klasy **MoveShapeHub** i kliknij przycisk **Dodaj**.
2. Zastąp kod w nowej **MoveShapeHub** klasy następującym kodem.

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample1.cs)]

    `MoveShapeHub` Powyżej klasa jest implementacją koncentratora SignalR. Podobnie jak w [wprowadzenie SignalR](tutorial-getting-started-with-signalr.md) samouczka koncentratora ma metodę, która klientów będzie wywoływać bezpośrednio. W takim przypadku klient wyśle obiekt zawierający nowe współrzędne X i Y kształtu do serwera, który następnie pobiera wysłano do wszystkich innych połączonych klientów. SignalR zostanie automatycznie serializuj ten obiekt, za pomocą formatu JSON.

    Obiekt, który zostanie wysłany do klienta (`ShapeModel`) zawiera elementy członkowskie do przechowywania położenie kształtu. Wersja obiektu na serwerze zawiera także członka do śledzenia danych klienta, które są przechowywane, tak, aby własne dane nie zostaną wysłane danego klienta. Ten element członkowski używa `JsonIgnore` atrybutu być serializowane i wysłane do klienta.

<a id="startup2013"></a>
## <a name="starting-the-hub-when-the-application-starts"></a>Uruchamianie koncentratora, podczas uruchamiania aplikacji

1. Następnie będzie skonfigurowanie mapowania do koncentratora podczas uruchamiania aplikacji. W wersji SignalR 2 odbywa się przez dodanie klasę początkową OWIN wywoła `MapSignalR` gdy klasa początkowa `Configuration` metoda jest wykonywana po uruchomieniu OWIN. Klasa jest dodawany do uruchamiania w OWIN przetworzyć przy użyciu `OwinStartup` atrybutu zestawu.

    W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt, a następnie kliknij przycisk **Dodaj | Klasy początkowej OWIN**. Nazwa klasy *uruchamiania* i kliknij przycisk **OK**.
2. Zmień poniżej zawartość pliku Startup.cs:

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample2.cs)]

<a id="client"></a>
## <a name="adding-the-client"></a>Dodawanie klienta

1. Następnie dodamy klienta. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt, a następnie kliknij przycisk **Dodaj | Nowy element**. W **Dodaj nowy element** okno dialogowe, wybierz opcję **strony Html**. Nazwa strony **Default.html** i kliknij przycisk **Dodaj**.
2. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy właśnie utworzony strony i kliknij przycisk **Ustaw jako stronę startową**.
3. Zamień poniższy fragment kodu w kodzie domyślnym na stronie HTML.

    > [!NOTE]
    > Sprawdź, czy skrypt odwołuje się poniżej dopasowania pakietów dodane do projektu w folderze skryptów.

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample3.html)]

    Powyższy kod HTML i JavaScript tworzy red Div o nazwie kształtu, włącza zachowanie przeciągania kształtu, za pomocą biblioteki jQuery i używa kształtu `drag` zdarzenia do wysłania kształtów do serwera.
4. Uruchom aplikację, naciskając klawisz F5. Skopiuj adres URL strony i wklej go do drugiego okna przeglądarki. Przeciągnij kształt w jednym z okna przeglądarki; należy przenieść kształt w oknie przeglądarki.

    ![W oknie aplikacji](tutorial-high-frequency-realtime-with-signalr/_static/image5.png)

<a id="clientloop"></a>

## <a name="add-the-client-loop"></a>Dodaj pętlę klienta

Ponieważ wysyłanie lokalizacji kształtu na zdarzenie przesunięcia kursora myszy, co spowoduje utworzenie niepotrzebnych ilość ruchu sieciowego, wiadomości z klienta konieczne jest ograniczany. Użyjemy skryptu javascript `setInterval` funkcji, aby skonfigurować pętli, które wysyła do serwera według stałej stawki ustalanej nowych informacji o położeniu. Pętla jest bardzo proste reprezentację "gier pętlę", wielokrotnie wywołana funkcja, którego wszystkie funkcje gry lub inne symulacji.

1. Zaktualizuj kod klienta na stronie HTML do dopasowania poniższy fragment kodu.

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample4.html)]

    Powyżej aktualizacja dodaje `updateServerModel` funkcja, która jest wywoływana w ustalonej częstotliwości. Ta funkcja wysyła umieszczanie danych do serwera przy każdym `moved` flaga wskazuje, czy jest nowe dane pozycji do wysyłania.
2. Uruchom aplikację, naciskając klawisz F5. Skopiuj adres URL strony i wklej go do drugiego okna przeglądarki. Przeciągnij kształt w jednym z okna przeglądarki; należy przenieść kształt w oknie przeglądarki. Ponieważ liczba wiadomości, które jest wysyłana do serwera będzie ograniczony, animacji nie będą wyświetlane jako smooth, jak w poprzedniej sekcji.

    ![W oknie aplikacji](tutorial-high-frequency-realtime-with-signalr/_static/image6.png)

<a id="serverloop"></a>

## <a name="add-the-server-loop"></a>Dodaj pętlę serwera

W bieżącej aplikacji wiadomości wysłanych z serwera do klienta Przejdź często są odbierane. Przedstawia podobny problem, jak wspomniano na kliencie; częściej niż jest to konieczne, a połączenie może stać się rozpływową w związku z tym można wysłać wiadomości. Ta sekcja opisuje sposób aktualizacji serwera do zaimplementowania czasomierza, która ogranicza to liczba komunikatów wychodzących.

1. Zastąp zawartość `MoveShapeHub.cs` z poniższy fragment kodu.

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample5.cs)]

    Powyższy kod rozszerza klienta, aby dodać `Broadcaster` klasy, która ogranicza wychodzącej wiadomości przy użyciu `Timer` klasy z programu .NET framework.

    Ponieważ koncentratora sam jest przejściowy (go jest tworzony za każdym razem, gdy jest to potrzebne), `Broadcaster` zostanie utworzona jako pojedynczą. Inicjalizacja z opóźnieniem (zostanie wprowadzony w .NET 4) jest używany na odraczanie jego tworzenia, dopóki nie jest to potrzebne, zapewnienie, że pierwsze wystąpienie koncentratora całkowicie został utworzony przed uruchomieniem czasomierza.

    Wywołania na klientach `UpdateShape` funkcji jest następnie przeniesiona poza koncentratora `UpdateModel` metodę, tak aby już nie jest wywoływana, natychmiast, gdy są odbierane wiadomości przychodzących. Zamiast tego będą wysyłane wiadomości do klientów z szybkością 25 wywołania na sekundę, zarządza `_broadcastLoop` czasomierza z poziomu `Broadcaster` klasy.

    Ponadto zamiast bezpośrednio, wywołanie metody klienta z Centrum `Broadcaster` należy uzyskać odwołania do aktualnie operacyjnych Centrum klasę (`_hubContext`) przy użyciu `GlobalHost`.
2. Uruchom aplikację, naciskając klawisz F5. Skopiuj adres URL strony i wklej go do drugiego okna przeglądarki. Przeciągnij kształt w jednym z okna przeglądarki; należy przenieść kształt w oknie przeglądarki. Nie będą widoczne różnica w przeglądarce z poprzedniej sekcji, ale liczba wiadomości, które jest wysyłana do klienta będzie ograniczony.

    ![W oknie aplikacji](tutorial-high-frequency-realtime-with-signalr/_static/image7.png)

<a id="animation"></a>

## <a name="add-smooth-animation-on-the-client"></a>Dodaj smooth animacji na kliencie

Aplikacja jest niemal ukończone, ale firma Microsoft może wprowadzać co więcej poprawy jakości, w ruchu kształtu na kliencie przy przenoszeniu w odpowiedzi na komunikaty serwera. Zamiast ustawienie pozycji kształtu do nowej lokalizacji określonej przez serwer, użyjemy biblioteki interfejsu użytkownika JQuery `animate` funkcji, aby przesunąć kształt sprawnie między jego bieżące oraz nowe położenie.

1. Aktualizacja klienta programu `updateShape` metody, aby wyglądały jak wyróżniony kod poniżej:

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample6.html?highlight=33-40)]

    Powyższy kod przenosi kształt z poprzedniej lokalizacji do nowego podane przez serwer w czasie trwania animacji interwał (w tym przypadku 100 milisekund). Wszelkie poprzednie animacji uruchomionych na kształt jest wyczyszczone przed uruchomieniem nowego animacji.
2. Uruchom aplikację, naciskając klawisz F5. Skopiuj adres URL strony i wklej go do drugiego okna przeglądarki. Przeciągnij kształt w jednym z okna przeglądarki; należy przenieść kształt w oknie przeglądarki. Przenoszenie kształtu w innym oknie powinna pojawić się mniej jerky, przesunięcie jest interpolowane z upływem czasu, a nie ustawiany raz na komunikat przychodzący.

    ![W oknie aplikacji](tutorial-high-frequency-realtime-with-signalr/_static/image8.png)

<a id="furthersteps"></a>

## <a name="further-steps"></a>Dodatkowe kroki

W tym samouczku kiedy znasz już jak do programowania aplikacji SignalR, który wysyła komunikaty o dużej częstotliwości między klientami a serwerami. Ten model komunikacji jest przydatne w przypadku takich jak tworzenie gry online i innych symulacje [gry ShootR utworzone za pomocą SignalR](http://shootr.signalr.net).

Kompletna aplikacja utworzonej w tym samouczku można pobrać z [galerii kodu](https://code.msdn.microsoft.com/SignalR-20-MoveShape-Demo-6285b83a).

Aby dowiedzieć się więcej o koncepcjach programowanie SignalR, odwiedź następujące witryny dla SignalR kod źródłowy i zasoby:

- [Projekt SignalR](http://signalr.net)
- [SignalR Github i przykłady](https://github.com/SignalR/SignalR)
- [Witryna typu Wiki biblioteki SignalR](https://github.com/SignalR/SignalR/wiki)

Aby uzyskać wskazówki dotyczące sposobu wdrażania aplikacji SignalR na platformie Azure, zobacz [SignalR korzystanie z aplikacji sieci Web w usłudze Azure App Service](../deployment/using-signalr-with-azure-web-sites.md). Aby uzyskać szczegółowe informacje o sposobie wdrażania projektu sieci web programu Visual Studio do witryny sieci Web systemu Windows Azure, zobacz [tworzenie aplikacji sieci web platformy ASP.NET w usłudze Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).
