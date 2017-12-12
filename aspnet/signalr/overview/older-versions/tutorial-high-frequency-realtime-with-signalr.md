---
uid: signalr/overview/older-versions/tutorial-high-frequency-realtime-with-signalr
title: "Czas rzeczywisty wysokiej częstotliwości z SignalR 1.x | Dokumentacja firmy Microsoft"
author: pfletcher
description: "W tym samouczku przedstawiono sposób tworzenia aplikacji sieci web, który używa biblioteki SignalR platformy ASP.NET w celu zapewnienia wysokiej częstotliwości z obsługą wiadomości. Wysokiej częstotliwości wiadomości w..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/16/2013
ms.topic: article
ms.assetid: ad2a5da5-2e79-40ea-bc84-028d327f5982
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/tutorial-high-frequency-realtime-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 0c680a7d8b911b2734647948b683d5ff6e47aec4
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="high-frequency-realtime-with-signalr-1x"></a>Czas rzeczywisty wysokiej częstotliwości z SignalR 1.x
====================
przez [Patrick Fletcher](https://github.com/pfletcher)

> W tym samouczku przedstawiono sposób tworzenia aplikacji sieci web, który używa biblioteki SignalR platformy ASP.NET w celu zapewnienia wysokiej częstotliwości z obsługą wiadomości. W takim przypadku wiadomości o dużej częstotliwości oznacza aktualizacje, które są wysyłane według stałej stawki ustalanej; w przypadku tej aplikacji, maksymalnie 10 komunikatów na sekundę.
> 
> Aplikacji, które zostaną utworzone w tym samouczku Wyświetla kształtu, który można przeciągnąć użytkowników. Położenie kształtu w innych przeglądarkach połączonych zostaną zaktualizowane do dopasowania pozycja przeciąganego kształtu przy użyciu czasu aktualizacji.
> 
> Pojęciami opisanymi w tym samouczku zostały aplikacji w czasie rzeczywistym gry i innych aplikacji symulacji.
> 
> Komentarze w tym samouczku są powitalnej. Jeśli masz pytania, które nie są bezpośrednio związane z tego samouczka możesz zamieścić je do [forum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) lub [StackOverflow.com](http://stackoverflow.com).


## <a name="overview"></a>Omówienie

Ten samouczek przedstawia sposób tworzenia aplikacji, który współużytkuje stan obiektu z innych przeglądarek w czasie rzeczywistym. Aplikacja, którą utworzymy nosi nazwę MoveShape. Na stronie MoveShape wyświetli element HTML Div, który użytkownik może przeciągać; gdy użytkownik przeciąga Div, jego nowego położenia będą wysyłane do serwera, który poinformuje wszystkich pozostałych klientów podłączonych do zaktualizowania pozycji kształtu do dopasowania.

![W oknie aplikacji](tutorial-high-frequency-realtime-with-signalr/_static/image1.png)

Aplikacji utworzonej w tym samouczku opiera się na pokaz przez Dyszkiewicz Damianowi. Film wideo zawierający ten pokaz są widoczne [tutaj](https://channel9.msdn.com/Series/Building-Web-Apps-with-ASP-NET-Jump-Start/Building-Web-Apps-with-ASPNET-Jump-Start-08-Real-time-Communication-with-SignalR).

Samouczek zostanie uruchomiony przez pokazująca, jak wysyłać SignalR z każdym Zdarzenie uruchamiane jako kształt zostanie przeciągnięty. Każdy połączony klient następnie zaktualizuje pozycja lokalnej wersji kształtu zawsze, gdy wiadomość zostanie odebrana.

Gdy aplikacja będzie działać, za pomocą tej metody, nie jest zalecane model programowania, ponieważ nie byłoby górny limit liczby wiadomości wysłane pierwsze, klientów i serwera można uzyskać przeciążony komunikatów, i może zmniejszyć wydajność . Również będą wyświetlane animacji na kliencie, odłączony, jak kształt będzie można przenieść natychmiast przez każdą z tych metod, a nie przenoszenie klucz do każdej nowej lokalizacji. Kolejnych sekcjach samouczka przedstawiono sposób tworzenia funkcji czasomierza, która ogranicza maksymalna szybkość z jaką komunikaty są wysyłane przez klienta lub serwera i jak przenieść kształt sprawnie między lokalizacjami. Z ostateczną wersją aplikacji utworzonej w tym samouczku można pobrać z [galerii kodu](https://code.msdn.microsoft.com/SignalR-MoveShape-demo-3366dac6).

Ten samouczek zawiera następujące sekcje:

- [Wymagania wstępne](#prerequisites)
- [Tworzenie projektu](#createtheproject)
- [Dodawanie pakietów ASP.NET SignalR i JQuery.UI NuGet](#nugetpackages)
- [Tworzenie podstawowej aplikacji](#baseapp)
- [Dodaj pętlę klienta](#clientloop)
- [Dodaj pętlę serwera](#serverloop)
- [Dodaj smooth animacji na kliencie](#animation)
- [Dodatkowe kroki](#furthersteps)

<a id="prerequisites"></a>

## <a name="prerequisites"></a>Wymagania wstępne

Ten samouczek wymaga programu Visual Studio 2012 lub Visual Studio 2010. Jeśli używany jest program Visual Studio 2010, projekt zostanie użyty, .NET Framework 4, a nie programu .NET Framework 4.5.

Jeśli używasz programu Visual Studio 2012, zaleca się zainstalowanie [ASP.NET i 2012.2 narzędzia sieci Web aktualizacji](https://go.microsoft.com/fwlink/?LinkId=282650). Ta aktualizacja zawiera nowe funkcje, takie jak ulepszenia publikowania, nowe funkcje i nowych szablonów.

Jeśli masz program Visual Studio 2010, upewnij się, że [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) jest zainstalowany.

<a id="createtheproject"></a>

## <a name="create-the-project"></a>Utwórz projekt

W tej sekcji utworzymy projektu programu Visual Studio.

1. Z **pliku** kliknij menu **nowy projekt**.
2. W **nowy projekt** okna dialogowego rozwiń **C#** w obszarze **szablony** i wybierz **Web**.
3. Wybierz **pusta aplikacja sieci Web ASP.NET** szablonu, nazwy projektu *MoveShapeDemo*i kliknij przycisk **OK**.

    ![Tworzenie nowego projektu](tutorial-high-frequency-realtime-with-signalr/_static/image2.png)

<a id="nugetpackages"></a>

## <a name="add-the-signalr-and-jqueryui-nuget-packages"></a>Dodaj SignalR i pakiety JQuery.UI NuGet

Funkcje SignalR można dodać do projektu, instalując pakiet NuGet. W tym samouczku będą też używać pakietu JQuery.UI umożliwiające kształt na przeciąganie i animowany.

1. Kliknij przycisk **narzędzia | Menedżer pakietów biblioteki | Konsola Menedżera pakietów**.
2. Wprowadź następujące polecenie w Menedżera pakietów.

    [!code-powershell[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample1.ps1)]

    Pakiet SignalR instaluje liczbę pozostałych pakietów NuGet jako zależności. Po zakończeniu instalacji znajdują się wszystkie składniki serwera i klienta wymagane do użycia w aplikacji ASP.NET SignalR.
3. Wprowadź następujące polecenie w konsoli Menedżera pakietów, aby zainstalować pakiety JQuery i JQuery.UI.

    [!code-powershell[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample2.ps1)]

<a id="baseapp"></a>

## <a name="create-the-base-application"></a>Tworzenie podstawowej aplikacji

W tej sekcji utworzymy aplikacji przeglądarki, która wysyła lokalizacji kształtu do serwera podczas każdego zdarzenie przesunięcia kursora myszy. Serwer wysyła następnie te informacje do wszystkich innych połączonych klientów odbierane. Firma Microsoft będzie rozwiń w tej aplikacji w kolejnych sekcjach.

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj**, **klasy...** . Nazwa klasy **MoveShapeHub** i kliknij przycisk **Dodaj**.
2. Zastąp kod w nowej **MoveShapeHub** klasy następującym kodem.

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample3.cs)]

    `MoveShapeHub` Powyżej klasa jest implementacją koncentratora SignalR. Podobnie jak w [wprowadzenie SignalR](index.md) samouczka koncentratora ma metodę, która klientów będzie wywoływać bezpośrednio. W takim przypadku klient wyśle obiekt zawierający nowe współrzędne X i Y kształtu do serwera, który następnie pobiera wysłano do wszystkich innych połączonych klientów. SignalR zostanie automatycznie serializuj ten obiekt, za pomocą formatu JSON.

    Obiekt, który zostanie wysłany do klienta (`ShapeModel`) zawiera elementy członkowskie do przechowywania położenie kształtu. Wersja obiektu na serwerze zawiera także członka do śledzenia danych klienta, które są przechowywane, tak, aby własne dane nie zostaną wysłane danego klienta. Ten element członkowski używa `JsonIgnore` atrybutu być serializowane i wysłane do klienta.
3. Dalej skonfigurujemy Twoje koncentratora podczas uruchamiania aplikacji. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt, a następnie kliknij przycisk **Dodaj | Klasy globalne aplikacji**. Zaakceptuj domyślną nazwę *Global* i kliknij przycisk **OK**.

    ![Dodaj klasę globalnego aplikacji](tutorial-high-frequency-realtime-with-signalr/_static/image3.png)
4. Dodaj następujące `using` instrukcji po dostarczonych **przy użyciu** instrukcje w klasie Global.asax.cs.

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample4.cs)]
5. Dodaj następujący wiersz kodu w `Application_Start` metody klasy globalny można zarejestrować trasy domyślnej dla elementu SignalR.

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample5.cs)]

    Plik global.asax powinna wyglądać następująco:

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample6.cs)]
6. Następnie dodamy klienta. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt, a następnie kliknij przycisk **Dodaj | Nowy element**. W **Dodaj nowy element** okno dialogowe, wybierz opcję **strony Html**. Zapewniają odpowiednią nazwę strony (takie jak **Default.html**) i kliknij przycisk **Dodaj**.
7. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy właśnie utworzony strony i kliknij przycisk **Ustaw jako stronę startową**.
8. Zamień poniższy fragment kodu w kodzie domyślnym na stronie HTML.

    > [!NOTE]
    > Sprawdź, czy skrypt odwołuje się poniżej dopasowania pakietów dodane do projektu w folderze skryptów. W programie Visual Studio 2010 wersja, JQuery i SignalR dodane do projektu nie zgadzają poniżej numerów wersji.

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample7.html)]

    Powyższy kod HTML i JavaScript tworzy red Div o nazwie kształtu, włącza zachowanie przeciągania kształtu, za pomocą biblioteki jQuery i używa kształtu `drag` zdarzenia do wysłania kształtów do serwera.
9. Uruchom aplikację, naciskając klawisz F5. Skopiuj adres URL strony i wklej go do drugiego okna przeglądarki. Przeciągnij kształt w jednym z okna przeglądarki; należy przenieść kształt w oknie przeglądarki.

    ![W oknie aplikacji](tutorial-high-frequency-realtime-with-signalr/_static/image4.png)

<a id="clientloop"></a>

## <a name="add-the-client-loop"></a>Dodaj pętlę klienta

Ponieważ wysyłanie lokalizacji kształtu na zdarzenie przesunięcia kursora myszy, co spowoduje utworzenie niepotrzebnych ilość ruchu sieciowego, wiadomości z klienta konieczne jest ograniczany. Użyjemy skryptu javascript `setInterval` funkcji, aby skonfigurować pętli, które wysyła do serwera według stałej stawki ustalanej nowych informacji o położeniu. Pętla jest bardzo proste reprezentację "gier pętlę", wielokrotnie wywołana funkcja, którego wszystkie funkcje gry lub inne symulacji.

1. Zaktualizuj kod klienta na stronie HTML do dopasowania poniższy fragment kodu.

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample8.html)]

    Powyżej aktualizacja dodaje `updateServerModel` funkcja, która jest wywoływana w ustalonej częstotliwości. Ta funkcja wysyła umieszczanie danych do serwera przy każdym `moved` flaga wskazuje, czy jest nowe dane pozycji do wysyłania.
2. Uruchom aplikację, naciskając klawisz F5. Skopiuj adres URL strony i wklej go do drugiego okna przeglądarki. Przeciągnij kształt w jednym z okna przeglądarki; należy przenieść kształt w oknie przeglądarki. Ponieważ liczba wiadomości, które jest wysyłana do serwera będzie ograniczony, animacji nie będą wyświetlane jako smooth, jak w poprzedniej sekcji.

    ![W oknie aplikacji](tutorial-high-frequency-realtime-with-signalr/_static/image5.png)

<a id="serverloop"></a>

## <a name="add-the-server-loop"></a>Dodaj pętlę serwera

W bieżącej aplikacji wiadomości wysłanych z serwera do klienta Przejdź często są odbierane. Przedstawia podobny problem, jak wspomniano na kliencie; częściej niż jest to konieczne, a połączenie może stać się rozpływową w związku z tym można wysłać wiadomości. Ta sekcja opisuje sposób aktualizacji serwera do zaimplementowania czasomierza, która ogranicza to liczba komunikatów wychodzących.

1. Zastąp zawartość `MoveShapeHub.cs` z poniższy fragment kodu.

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample9.cs)]

    Powyższy kod rozszerza klienta, aby dodać `Broadcaster` klasy, która ogranicza wychodzącej wiadomości przy użyciu `Timer` klasy z programu .NET framework.

    Ponieważ koncentratora sam jest przejściowy (go jest tworzony za każdym razem, gdy jest to potrzebne), `Broadcaster` zostanie utworzona jako pojedynczą. Inicjalizacja z opóźnieniem (zostanie wprowadzony w .NET 4) jest używany na odraczanie jego tworzenia, dopóki nie jest to potrzebne, zapewnienie, że pierwsze wystąpienie koncentratora całkowicie został utworzony przed uruchomieniem czasomierza.

    Wywołania na klientach `UpdateShape` funkcji jest następnie przeniesiona poza koncentratora `UpdateModel` metodę, tak aby już nie jest wywoływana, natychmiast, gdy są odbierane wiadomości przychodzących. Zamiast tego będą wysyłane wiadomości do klientów z szybkością 25 wywołania na sekundę, zarządza `_broadcastLoop` czasomierza z poziomu `Broadcaster` klasy.

    Ponadto zamiast bezpośrednio, wywołanie metody klienta z Centrum `Broadcaster` należy uzyskać odwołania do aktualnie operacyjnych Centrum klasę (`_hubContext`) przy użyciu `GlobalHost`.
2. Uruchom aplikację, naciskając klawisz F5. Skopiuj adres URL strony i wklej go do drugiego okna przeglądarki. Przeciągnij kształt w jednym z okna przeglądarki; należy przenieść kształt w oknie przeglądarki. Nie będą widoczne różnica w przeglądarce z poprzedniej sekcji, ale liczba wiadomości, które jest wysyłana do klienta będzie ograniczony.

    ![W oknie aplikacji](tutorial-high-frequency-realtime-with-signalr/_static/image6.png)

<a id="animation"></a>

## <a name="add-smooth-animation-on-the-client"></a>Dodaj smooth animacji na kliencie

Aplikacja jest niemal ukończone, ale firma Microsoft może wprowadzać co więcej poprawy jakości, w ruchu kształtu na kliencie przy przenoszeniu w odpowiedzi na komunikaty serwera. Zamiast ustawienie pozycji kształtu do nowej lokalizacji określonej przez serwer, użyjemy biblioteki interfejsu użytkownika JQuery `animate` funkcji, aby przesunąć kształt sprawnie między jego bieżące oraz nowe położenie.

1. Aktualizacja klienta programu `updateShape` metody, aby wyglądały jak wyróżniony kod poniżej:

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample10.html?highlight=35-42)]

    Powyższy kod przenosi kształt z poprzedniej lokalizacji do nowego podane przez serwer w czasie trwania animacji interwał (w tym przypadku 100 milisekund). Wszelkie poprzednie animacji uruchomionych na kształt jest wyczyszczone przed uruchomieniem nowego animacji.
2. Uruchom aplikację, naciskając klawisz F5. Skopiuj adres URL strony i wklej go do drugiego okna przeglądarki. Przeciągnij kształt w jednym z okna przeglądarki; należy przenieść kształt w oknie przeglądarki. Przenoszenie kształtu w innym oknie powinna pojawić się mniej jerky, przesunięcie jest interpolowane z upływem czasu, a nie ustawiany raz na komunikat przychodzący.

    ![W oknie aplikacji](tutorial-high-frequency-realtime-with-signalr/_static/image7.png)

<a id="furthersteps"></a>

## <a name="further-steps"></a>Dodatkowe kroki

W tym samouczku kiedy znasz już jak do programowania aplikacji SignalR, który wysyła komunikaty o dużej częstotliwości między klientami a serwerami. Ten model komunikacji jest przydatne w przypadku takich jak tworzenie gry online i innych symulacje [gry ShootR utworzone za pomocą SignalR](http://shootr.signalr.net).

Kompletna aplikacja utworzonej w tym samouczku można pobrać z [galerii kodu](https://code.msdn.microsoft.com/SignalR-MoveShape-demo-3366dac6).

Aby dowiedzieć się więcej o koncepcjach programowanie SignalR, odwiedź następujące witryny dla SignalR kod źródłowy i zasoby:

- [Projekt SignalR](http://signalr.net)
- [SignalR Github i przykłady](https://github.com/SignalR/SignalR)
- [Witryna typu Wiki biblioteki SignalR](https://github.com/SignalR/SignalR/wiki)
