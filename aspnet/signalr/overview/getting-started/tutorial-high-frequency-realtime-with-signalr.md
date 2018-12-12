---
uid: signalr/overview/getting-started/tutorial-high-frequency-realtime-with-signalr
title: 'Samouczek: Wysoka częstotliwość Realtime z SignalR 2 | Dokumentacja firmy Microsoft'
author: pfletcher
description: W tym samouczku przedstawiono sposób tworzenia aplikacji sieci web, która używa biblioteki SignalR platformy ASP.NET w celu zapewnienia wysokiej częstotliwości funkcje obsługi komunikatów. O wysokiej częstotliwości komunikatów w...
ms.author: riande
ms.date: 06/10/2014
ms.assetid: 9f969dda-78ea-4329-b1e3-e51c02210a2b
msc.legacyurl: /signalr/overview/getting-started/tutorial-high-frequency-realtime-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 04ce650509268ee63daafe24bc8dcc9725aea16b
ms.sourcegitcommit: 74e3be25ea37b5fc8b4b433b0b872547b4b99186
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53287734"
---
<a name="tutorial-high-frequency-realtime-with-signalr-2"></a>Samouczek: Wysoka częstotliwość Realtime z SignalR 2
====================
przez [Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

[Pobierz ukończony projekt](http://code.msdn.microsoft.com/SignalR-20-MoveShape-Demo-6285b83a)

> W tym samouczku przedstawiono sposób tworzenia aplikacji sieci web korzystającą z signalr2 na platformie ASP.NET w celu zapewnienia funkcji obsługi wiadomości o wysokiej częstotliwości. Komunikaty o wysokiej częstotliwości w takim przypadku oznacza, że aktualizacje, które są wysyłane według stałej stawki ustalanej; w przypadku tej aplikacji, maksymalnie 10 komunikatów na sekundę.
>
> Aplikacji, które zostaną utworzone w tym samouczku Wyświetla kształtu, w którym można przeciągać użytkowników. Aby dopasować położenie przeciąganego kształtu przy użyciu Przekroczono limit czasu aktualizacji zostaną zaktualizowane położenie kształtu w innych przeglądarkach połączonych.
>
> Pojęciami opisanymi w tym samouczku mają aplikacji w czasie rzeczywistym gry i inne aplikacje symulacji.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używanego w tym samouczku
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
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

W tym samouczku pokazano, jak utworzyć aplikację, która współużytkuje stanu obiektu z innych przeglądarek w czasie rzeczywistym. MoveShape nosi nazwę aplikacji, którą utworzymy. MoveShape strony wyświetli element tag Div języka HTML, który użytkownik może przeciągać; gdy użytkownik przeciągnie Div, jego nowego położenia będą wysyłane do serwera, który zostanie wyświetlona informacja o innych klientów podłączonych do zaktualizowania położenie kształtu do dopasowania.

![W oknie aplikacji](tutorial-high-frequency-realtime-with-signalr/_static/image1.png)

Aplikacja utworzona w ramach tego samouczka opiera się na demonstrację przez Damianem Edwardsem. Film wideo zawierający tej wersji demonstracyjnej są widoczne [tutaj](https://channel9.msdn.com/Series/Building-Web-Apps-with-ASP-NET-Jump-Start/Building-Web-Apps-with-ASPNET-Jump-Start-08-Real-time-Communication-with-SignalR).

Samouczek rozpocznie się poprzez zademonstrowanie sposobu wysyłania komunikatów SignalR z każdego zdarzenia, który jest uruchamiany jako kształt zostanie przeciągnięty. Każdy klient połączonych następnie zaktualizuje położenie lokalnej wersji kształt każdorazowo, gdy wiadomość zostaje odebrana.

Gdy aplikacja będzie działać, przy użyciu tej metody, nie jest zalecany model programowania, ponieważ byłoby żadnego górnego limitu liczby wprowadzenie wysłanych komunikatów, dzięki czemu klientów i serwera można uzyskać przeciążeniu przy użyciu komunikatów i może zmniejszyć wydajność . Animacja wyświetlanych na komputerze klienckim będą również rozłączonych jako kształt będzie natychmiast przeniesionych przez każdą z tych metod zamiast przenoszenia klucz do każdej nowej lokalizacji. Kolejnych sekcjach tego samouczka przedstawiono sposób tworzenia funkcji czasomierza, która ogranicza maksymalną szybkość jaką komunikaty są wysyłane przez klienta lub serwera oraz sposób na przesunięcie kształtu płynnie między lokalizacjami. Można pobrać z ostateczną wersją aplikacji utworzonych w tym samouczku [galerii kodów](https://code.msdn.microsoft.com/SignalR-20-MoveShape-Demo-6285b83a).

Ten samouczek zawiera następujące sekcje:

- [Wymagania wstępne](#prerequisites)
- [Tworzenie projektu i dodawanie pakietu JQuery.UI NuGet i SignalR](#createtheproject2013)
- [Tworzenie podstawowej aplikacji](#baseapp)
- [Uruchamianie koncentratora, podczas uruchamiania aplikacji](#startup2013)
- [Dodaj pętlę klienta](#clientloop)
- [Dodaj pętlę serwera](#serverloop)
- [Dodaj płynne animacje na komputerze klienckim](#animation)
- [Trzeba wykonywać dalszych czynności](#furthersteps)

<a id="prerequisites"></a>

## <a name="prerequisites"></a>Wymagania wstępne

Ten samouczek wymaga programu Visual Studio 2013.

<a id="createtheproject2013"></a>

## <a name="create-the-project-and-add-the-signalr-and-jqueryui-nuget-package"></a>Tworzenie projektu i dodawanie pakietu JQuery.UI NuGet i SignalR

W tej sekcji utworzymy projektu w programie Visual Studio 2013.

Poniższe kroki Użyj programu Visual Studio 2013, aby utworzyć pustą aplikację sieci Web platformy ASP.NET i dodać biblioteki SignalR i jQuery.UI:

1. W programie Visual Studio tworzenie aplikacji sieci Web ASP.NET.

    ![Tworzenie sieci web](tutorial-high-frequency-realtime-with-signalr/_static/image2.png)
2. W **nowy projekt ASP.NET** okna, pozostaw **pusty** zaznaczone, a następnie kliknij przycisk **Tworzenie projektu**.

    ![Tworzenie pustego sieci web](tutorial-high-frequency-realtime-with-signalr/_static/image3.png)
3. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt, wybierz **Dodaj | Klasa Centrum SignalR (v2)**. Nazwa klasy **MoveShapeHub.cs** i dodaj go do projektu. Spowoduje to utworzenie **MoveShapeHub** klasy i dodaje do projektu zestawu plików skryptów i odwołania do zestawów, obsługujące bibliotekę SignalR.

    > [!NOTE]
    > Biblioteki SignalR można dodać do projektu, klikając **Narzędzia > Menedżer pakietów NuGet > Konsola Menedżera pakietów** i uruchamiając polecenie:

    `install-package Microsoft.AspNet.SignalR`.

    Jeśli używasz konsoli można dodać SignalR, należy utworzyć klasa Centrum SignalR w osobnym kroku po dodaniu SignalR.
4. Kliknij przycisk **Narzędzia > Menedżer pakietów NuGet > Konsola Menedżera pakietów**. W oknie Menedżera pakietów wpisz następujące polecenie:

    `Install-Package jQuery.UI.Combined`

    Spowoduje to zainstalowanie biblioteki interfejsu użytkownika jQuery, którego będziesz używać, aby animować kształt.
5. W **Eksploratora rozwiązań** rozwiń węzeł skryptów. Biblioteki skryptów, jQuery, jQueryUI i SignalR są widoczne w projekcie.

    ![Odwołania do biblioteki skryptu](tutorial-high-frequency-realtime-with-signalr/_static/image4.png)

<a id="baseapp"></a>

## <a name="create-the-base-application"></a>Tworzenie podstawowej aplikacji

W tej sekcji utworzymy aplikację przeglądarki, która wysyła położenie kształtu do serwera podczas każdego zdarzenie przesunięcia kursora myszy. Serwer następnie wysyła te informacje do wszystkich innych połączonych klientów po otrzymaniu. Firma Microsoft będzie rozwiń w tej aplikacji w kolejnych sekcjach.

1. Jeśli nie zostało jeszcze utworzone klasy MoveShapeHub.cs w **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy nad projektem i wybierz **Dodaj**, **klasy...** . Nazwa klasy **MoveShapeHub** i kliknij przycisk **Dodaj**.
2. Zastąp kod w nowej **MoveShapeHub** klasy z następującym kodem.

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample1.cs)]

    `MoveShapeHub` Powyżej klasa jest implementacją Centrum SignalR. Podobnie jak w [wprowadzenie do SignalR](tutorial-getting-started-with-signalr.md) samouczku Centrum ma metodę, która klientów będzie wywoływać bezpośrednio. W takim przypadku klient wyśle obiekt zawierający nową współrzędne X i Y kształtu do serwera, który następnie pobiera wysłano do wszystkich innych połączonych klientów. SignalR zostanie automatycznie serializuj tego obiektu przy użyciu formatu JSON.

    Obiekt, który zostanie wysłany do klienta (`ShapeModel`) zawiera elementy członkowskie do przechowywania położenie kształtu. Wersja obiektu na serwerze zawiera także członkiem, aby śledzić przechowywanych danych których klienta, tak, aby ich własne dane nie zostaną wysłane danego klienta. Używa tego elementu członkowskiego `JsonIgnore` atrybutu, aby zachować serializowane i wysłane do klienta.

<a id="startup2013"></a>
## <a name="starting-the-hub-when-the-application-starts"></a>Uruchamianie koncentratora, podczas uruchamiania aplikacji

1. Następnie skonfigurujemy mapowanie do Centrum podczas uruchamiania aplikacji. W SignalR 2, jest to realizowane przez dodawanie klasy początkowej OWIN, co spowoduje wywołanie `MapSignalR` gdy klasa startowa `Configuration` metoda jest wykonywana po uruchomieniu OWIN. Klasa jest dodawany do początkowa OWIN w przetwarzać, przy użyciu `OwinStartup` atrybutu zestawu.

    W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt, a następnie kliknij przycisk **Dodaj | Klasa początkowa OWIN**. Nazwa klasy *uruchamiania* i kliknij przycisk **OK**.
2. Zmień zawartość pliku Startup.cs następujące czynności:

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample2.cs)]

<a id="client"></a>
## <a name="adding-the-client"></a>Dodawanie klienta

1. Następnie dodamy klienta. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt, a następnie kliknij przycisk **Dodaj | Nowy element**. W **Dodaj nowy element** okno dialogowe, wybierz opcję **strony Html**. Nazwij stronę **Default.html** i kliknij przycisk **Dodaj**.
2. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy właśnie utworzona strona i kliknij przycisk **Ustaw jako strona startowa**.
3. Zastąp kod domyślną stronę HTML za pomocą poniższej wstawki kodu.

    > [!NOTE]
    > Upewnij się, że skrypt odwołuje się poniżej dopasowania pakietów dodane do projektu w folderze skryptów.

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample3.html)]

    Powyższy kod HTML i JavaScript tworzy Div czerwony, o nazwie kształtu, włącza zachowanie przeciągania kształtu, za pomocą biblioteki jQuery i użyciu kształtu `drag` zdarzenia do wysłania położenie kształtu do serwera.
4. Uruchom aplikację, naciskając klawisz F5. Skopiuj adres URL strony i wklej go w drugim oknie przeglądarki. Przeciągnij kształt w jednym z okna przeglądarki; Przeniesienie kształt w oknie przeglądarki.

    ![W oknie aplikacji](tutorial-high-frequency-realtime-with-signalr/_static/image5.png)

<a id="clientloop"></a>

## <a name="add-the-client-loop"></a>Dodaj pętlę klienta

Ponieważ wysyłanie położenie kształtu na zdarzenie przesunięcia kursora myszy, co spowoduje utworzenie niepotrzebnych ilości ruchu sieciowego, wiadomości z klienta konieczne ograniczona. Użyjemy javascript `setInterval` funkcję, aby skonfigurować pętlę, która wysyła do serwera według stałej stawki ustalanej nowe informacje pozycji. Ta pętla jest bardzo proste reprezentacja "gier pętlę", wielokrotnie wywołana funkcja, która napędza wszystkich funkcji grę lub innych symulacji.

1. Zaktualizuj kod klienta, strony HTML, aby dopasować poniższy fragment kodu.

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample4.html)]

    Powyższe aktualizacji dodano m.in. `updateServerModel` funkcji, która jest wywoływana z częstotliwością, stały. Ta funkcja wysyła umieszczanie danych do serwera przy każdym `moved` flagi oznacza, że są nowe dane pozycji do wysłania.
2. Uruchom aplikację, naciskając klawisz F5. Skopiuj adres URL strony i wklej go w drugim oknie przeglądarki. Przeciągnij kształt w jednym z okna przeglądarki; Przeniesienie kształt w oknie przeglądarki. Ponieważ zostanie ograniczona liczba wiadomości, które są wysyłane do serwera, animacja nie będzie wyświetlane tak dobre, jak w poprzedniej sekcji.

    ![W oknie aplikacji](tutorial-high-frequency-realtime-with-signalr/_static/image6.png)

<a id="serverloop"></a>

## <a name="add-the-server-loop"></a>Dodaj pętlę serwera

W bieżącej aplikacji komunikatów wysyłanych z serwera do klienta wysyłana tak często, jak zostały odebrane. Przedstawia podobny problem, ponieważ zostało zaobserwowane na kliencie; częściej niż jest to konieczne, a połączenie może stać się propagowane w rezultacie można wysyłać wiadomości. W tej sekcji opisano, jak można zaktualizować serwera do zaimplementowania czasomierz, co ogranicza współczynnik wiadomości wychodzących.

1. Zastąp zawartość `MoveShapeHub.cs` następującym fragmentem kodu.

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample5.cs)]

    Powyższy kod rozwija klienta, aby dodać `Broadcaster` klasy, która ogranicza wychodzącej wiadomości przy użyciu `Timer` klasy z programu .NET framework.

    Ponieważ Centrum, sama jest przejściowy (jest tworzony za każdym razem, gdy jest to konieczne), `Broadcaster` zostanie utworzony jako pojedynczą. Inicjalizacja z opóźnieniem (wprowadzona w .NET 4) umożliwia odroczenie jej tworzenia, dopóki nie jest to potrzebne, zapewniając czy pierwsze wystąpienie koncentratora został całkowicie utworzony przed rozpoczęciem czasomierza.

    Wywołania na klientach `UpdateShape` funkcji są przenoszone poza Centrum `UpdateModel` metodę, tak że nie jest już jest wywoływana przy każdym przychodzące komunikaty są odbierane. Zamiast tego wysłania wiadomości do klientów w wysokości 25 wywołania na sekundę, zarządza `_broadcastLoop` czasomierza z poziomu `Broadcaster` klasy.

    Na koniec, a nie bezpośrednio, wywołanie metody klienta z Centrum `Broadcaster` klasa musi uzyskać odwołanie do aktualnie operacyjnych Centrum (`_hubContext`) przy użyciu `GlobalHost`.
2. Uruchom aplikację, naciskając klawisz F5. Skopiuj adres URL strony i wklej go w drugim oknie przeglądarki. Przeciągnij kształt w jednym z okna przeglądarki; Przeniesienie kształt w oknie przeglądarki. Nie będzie widoczna różnica w przeglądarce z poprzedniej sekcji, ale zostanie ograniczona liczba wiadomości, które są wysyłane do klienta.

    ![W oknie aplikacji](tutorial-high-frequency-realtime-with-signalr/_static/image7.png)

<a id="animation"></a>

## <a name="add-smooth-animation-on-the-client"></a>Dodaj płynne animacje na komputerze klienckim

Aplikacja jest niemal ukończone, ale firma Microsoft może spowodować, że jeden więcej poprawę, ruch kształt na komputerze klienckim, ponieważ są one przenoszone w odpowiedzi na komunikaty serwera. Zamiast ustawić położenie kształtu do nowej lokalizacji określonej przez serwer, użyjemy biblioteki interfejsu użytkownika JQuery `animate` funkcję, aby przesunąć kształt płynnie od jego bieżących i nowych pozycji.

1. Aktualizacja klienta programu `updateShape` metody do wyszukiwania, takie jak wyróżniony kod poniżej:

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample6.html?highlight=33-40)]

    Powyższy kod przenosi kształt z poprzedniej lokalizacji nowe podany przez serwer w ciągu interwału animacji (w tym przypadku 100 milisekund). Wszelkie poprzednie animacji uruchomionych na kształt jest czyszczona przed uruchomieniem nowej animacji.
2. Uruchom aplikację, naciskając klawisz F5. Skopiuj adres URL strony i wklej go w drugim oknie przeglądarki. Przeciągnij kształt w jednym z okna przeglądarki; Przeniesienie kształt w oknie przeglądarki. Przemieszczanie kształtu w innym oknie powinna zostać wyświetlona mniej jerky, zgodnie z jego ruchu jest interpolowane z upływem czasu, a nie raz ustawiany na wiadomości przychodzącej.

    ![W oknie aplikacji](tutorial-high-frequency-realtime-with-signalr/_static/image8.png)

<a id="furthersteps"></a>

## <a name="further-steps"></a>Trzeba wykonywać dalszych czynności

W tym samouczku wyjaśniono sposób programowania aplikacji SignalR, która wysyła komunikaty o wysokiej częstotliwości między klientami a serwerami. Ten model komunikacji jest przydatne w przypadku tworzenia gier online i innych symulacje, takie jak [ShootR gry utworzone przy użyciu SignalR](https://shootr.azurewebsites.net/).

Kompletna aplikacja utworzona w ramach tego samouczka można pobrać z [galerii kodów](https://code.msdn.microsoft.com/SignalR-20-MoveShape-Demo-6285b83a).

Aby dowiedzieć się więcej na temat pojęć programowania SignalR, odwiedź następujące witryny dla SignalR kod źródłowy i zasoby:

- [Projekt SignalR](http://signalr.net)
- [SignalR Github i przykłady](https://github.com/SignalR/SignalR)
- [Witryny typu Wiki biblioteki SignalR](https://github.com/SignalR/SignalR/wiki)

Aby uzyskać wskazówki dotyczące sposobu wdrażania aplikacji SignalR na platformie Azure, zobacz [przy użyciu SignalR z usługą Web Apps w usłudze Azure App Service](../deployment/using-signalr-with-azure-web-sites.md). Aby uzyskać szczegółowe informacje o sposobie wdrażania projektu sieci web programu Visual Studio z witryny sieci Web do Windows Azure, zobacz [tworzenie aplikacji sieci web platformy ASP.NET w usłudze Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).
