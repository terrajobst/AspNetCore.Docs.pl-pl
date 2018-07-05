---
uid: signalr/overview/older-versions/tutorial-high-frequency-realtime-with-signalr
title: Realtime wysoką częstotliwością przy użyciu SignalR 1.x | Dokumentacja firmy Microsoft
author: pfletcher
description: W tym samouczku przedstawiono sposób tworzenia aplikacji sieci web, która używa biblioteki SignalR platformy ASP.NET w celu zapewnienia wysokiej częstotliwości funkcje obsługi komunikatów. O wysokiej częstotliwości komunikatów w...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/16/2013
ms.topic: article
ms.assetid: ad2a5da5-2e79-40ea-bc84-028d327f5982
ms.technology: dotnet-signalr
msc.legacyurl: /signalr/overview/older-versions/tutorial-high-frequency-realtime-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: d9a5539d79d0b6396282af8036f2c6417f44ce23
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37401137"
---
<a name="high-frequency-realtime-with-signalr-1x"></a>Realtime wysoką częstotliwością przy użyciu SignalR 1.x
====================
przez [Patrick Fletcher](https://github.com/pfletcher)

> W tym samouczku przedstawiono sposób tworzenia aplikacji sieci web, która używa biblioteki SignalR platformy ASP.NET w celu zapewnienia wysokiej częstotliwości funkcje obsługi komunikatów. Komunikaty o wysokiej częstotliwości w takim przypadku oznacza, że aktualizacje, które są wysyłane według stałej stawki ustalanej; w przypadku tej aplikacji, maksymalnie 10 komunikatów na sekundę.
> 
> Aplikacji, które zostaną utworzone w tym samouczku Wyświetla kształtu, w którym można przeciągać użytkowników. Aby dopasować położenie przeciąganego kształtu przy użyciu Przekroczono limit czasu aktualizacji zostaną zaktualizowane położenie kształtu w innych przeglądarkach połączonych.
> 
> Pojęciami opisanymi w tym samouczku mają aplikacji w czasie rzeczywistym gry i inne aplikacje symulacji.
> 
> Zapraszamy są komentarze dotyczące tego samouczka. Jeśli masz pytania, na które nie są bezpośrednio związane z tego samouczka, możesz zamieścić je do [forum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) lub [StackOverflow.com](http://stackoverflow.com).


## <a name="overview"></a>Omówienie

W tym samouczku pokazano, jak utworzyć aplikację, która współużytkuje stanu obiektu z innych przeglądarek w czasie rzeczywistym. MoveShape nosi nazwę aplikacji, którą utworzymy. MoveShape strony wyświetli element tag Div języka HTML, który użytkownik może przeciągać; gdy użytkownik przeciągnie Div, jego nowego położenia będą wysyłane do serwera, który zostanie wyświetlona informacja o innych klientów podłączonych do zaktualizowania położenie kształtu do dopasowania.

![W oknie aplikacji](tutorial-high-frequency-realtime-with-signalr/_static/image1.png)

Aplikacja utworzona w ramach tego samouczka opiera się na demonstrację przez Damianem Edwardsem. Film wideo zawierający tej wersji demonstracyjnej są widoczne [tutaj](https://channel9.msdn.com/Series/Building-Web-Apps-with-ASP-NET-Jump-Start/Building-Web-Apps-with-ASPNET-Jump-Start-08-Real-time-Communication-with-SignalR).

Samouczek rozpocznie się poprzez zademonstrowanie sposobu wysyłania komunikatów SignalR z każdego zdarzenia, który jest uruchamiany jako kształt zostanie przeciągnięty. Każdy klient połączonych następnie zaktualizuje położenie lokalnej wersji kształt każdorazowo, gdy wiadomość zostaje odebrana.

Gdy aplikacja będzie działać, przy użyciu tej metody, nie jest zalecany model programowania, ponieważ byłoby żadnego górnego limitu liczby wprowadzenie wysłanych komunikatów, dzięki czemu klientów i serwera można uzyskać przeciążeniu przy użyciu komunikatów i może zmniejszyć wydajność . Animacja wyświetlanych na komputerze klienckim będą również rozłączonych jako kształt będzie natychmiast przeniesionych przez każdą z tych metod zamiast przenoszenia klucz do każdej nowej lokalizacji. Kolejnych sekcjach tego samouczka przedstawiono sposób tworzenia funkcji czasomierza, która ogranicza maksymalną szybkość jaką komunikaty są wysyłane przez klienta lub serwera oraz sposób na przesunięcie kształtu płynnie między lokalizacjami. Można pobrać z ostateczną wersją aplikacji utworzonych w tym samouczku [galerii kodów](https://code.msdn.microsoft.com/SignalR-MoveShape-demo-3366dac6).

Ten samouczek zawiera następujące sekcje:

- [Wymagania wstępne](#prerequisites)
- [Tworzenie projektu](#createtheproject)
- [Dodaj pakiety JQuery.UI NuGet i biblioteki SignalR platformy ASP.NET](#nugetpackages)
- [Tworzenie podstawowej aplikacji](#baseapp)
- [Dodaj pętlę klienta](#clientloop)
- [Dodaj pętlę serwera](#serverloop)
- [Dodaj płynne animacje na komputerze klienckim](#animation)
- [Trzeba wykonywać dalszych czynności](#furthersteps)

<a id="prerequisites"></a>

## <a name="prerequisites"></a>Wymagania wstępne

Ten samouczek wymaga programu Visual Studio 2012 lub Visual Studio 2010. Jeśli używany jest program Visual Studio 2010, że projekt będzie używać .NET Framework 4, a nie .NET Framework 4.5.

Jeśli używasz programu Visual Studio 2012, zaleca się zainstalowanie [aktualizacji platformy ASP.NET i Web Tools 2012.2](https://go.microsoft.com/fwlink/?LinkId=282650). Ta aktualizacja zawiera nowe funkcje, takie jak ulepszenia publikowania, nowe funkcje i nowych szablonów.

Jeśli masz program Visual Studio 2010, upewnij się, że [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) jest zainstalowany.

<a id="createtheproject"></a>

## <a name="create-the-project"></a>Utwórz projekt

W tej sekcji utworzymy projektu w programie Visual Studio.

1. Z **pliku** kliknij menu **nowy projekt**.
2. W **nowy projekt** okna dialogowego rozwiń **C#** w obszarze **szablony** i wybierz **Web**.
3. Wybierz **pusta aplikacja sieci Web platformy ASP.NET** szablonu, nazwę projektu *MoveShapeDemo*i kliknij przycisk **OK**.

    ![Tworzenie nowego projektu](tutorial-high-frequency-realtime-with-signalr/_static/image2.png)

<a id="nugetpackages"></a>

## <a name="add-the-signalr-and-jqueryui-nuget-packages"></a>Dodaj pakiety JQuery.UI NuGet i SignalR

Funkcje biblioteki SignalR można dodać do projektu, instalując pakiet NuGet. W tym samouczku zostanie również na użytek pakietu JQuery.UI umożliwiając kształt na przeciąganie i animowany.

1. Kliknij przycisk **narzędzia | Menedżer pakietów biblioteki | Konsola Menedżera pakietów**.
2. Wprowadź następujące polecenie w Menedżera pakietów.

    [!code-powershell[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample1.ps1)]

    Pakiet biblioteki SignalR instaluje kilka innych pakietów NuGet jako zależności. Po zakończeniu instalacji znajdują się wszystkie składniki serwera i klienta wymaganych do korzystania z biblioteki SignalR w aplikacji ASP.NET.
3. Wprowadź następujące polecenie w konsoli Menedżera pakietów, aby zainstalować pakiety JQuery i JQuery.UI.

    [!code-powershell[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample2.ps1)]

<a id="baseapp"></a>

## <a name="create-the-base-application"></a>Tworzenie podstawowej aplikacji

W tej sekcji utworzymy aplikację przeglądarki, która wysyła położenie kształtu do serwera podczas każdego zdarzenie przesunięcia kursora myszy. Serwer następnie wysyła te informacje do wszystkich innych połączonych klientów po otrzymaniu. Firma Microsoft będzie rozwiń w tej aplikacji w kolejnych sekcjach.

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy nad projektem i wybierz **Dodaj**, **klasy...** . Nazwa klasy **MoveShapeHub** i kliknij przycisk **Dodaj**.
2. Zastąp kod w nowej **MoveShapeHub** klasy z następującym kodem.

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample3.cs)]

    `MoveShapeHub` Powyżej klasa jest implementacją Centrum SignalR. Podobnie jak w [wprowadzenie do SignalR](index.md) samouczku Centrum ma metodę, która klientów będzie wywoływać bezpośrednio. W takim przypadku klient wyśle obiekt zawierający nową współrzędne X i Y kształtu do serwera, który następnie pobiera wysłano do wszystkich innych połączonych klientów. SignalR zostanie automatycznie serializuj tego obiektu przy użyciu formatu JSON.

    Obiekt, który zostanie wysłany do klienta (`ShapeModel`) zawiera elementy członkowskie do przechowywania położenie kształtu. Wersja obiektu na serwerze zawiera także członkiem, aby śledzić przechowywanych danych których klienta, tak, aby ich własne dane nie zostaną wysłane danego klienta. Używa tego elementu członkowskiego `JsonIgnore` atrybutu, aby zachować serializowane i wysłane do klienta.
3. Następnie skonfigurujemy Centrum podczas uruchamiania aplikacji. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt, a następnie kliknij przycisk **Dodaj | Globalna klasa aplikacji**. Zaakceptuj domyślną nazwę *Global* i kliknij przycisk **OK**.

    ![Dodaj globalna klasa aplikacji](tutorial-high-frequency-realtime-with-signalr/_static/image3.png)
4. Dodaj następujący kod `using` instrukcji znajdującej się po podane **przy użyciu** instrukcji w klasie Global.asax.cs.

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample4.cs)]
5. Dodaj następujący wiersz kodu w `Application_Start` metody klasy globalny można zarejestrować trasy domyślnej dla elementu SignalR.

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample5.cs)]

    Plik global.asax powinien wyglądać następująco:

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample6.cs)]
6. Następnie dodamy klienta. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt, a następnie kliknij przycisk **Dodaj | Nowy element**. W **Dodaj nowy element** okno dialogowe, wybierz opcję **strony Html**. Nadaj nazwę odpowiednie strony (takich jak **Default.html**) i kliknij przycisk **Dodaj**.
7. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy właśnie utworzona strona i kliknij przycisk **Ustaw jako strona startowa**.
8. Zastąp kod domyślną stronę HTML za pomocą poniższej wstawki kodu.

    > [!NOTE]
    > Upewnij się, że skrypt odwołuje się poniżej dopasowania pakietów dodane do projektu w folderze skryptów. W programie Visual Studio 2010 wersja JQuery i dodane do projektu biblioteki SignalR mogą być niezgodne poniższych numerów wersji.

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample7.html)]

    Powyższy kod HTML i JavaScript tworzy Div czerwony, o nazwie kształtu, włącza zachowanie przeciągania kształtu, za pomocą biblioteki jQuery i użyciu kształtu `drag` zdarzenia do wysłania położenie kształtu do serwera.
9. Uruchom aplikację, naciskając klawisz F5. Skopiuj adres URL strony i wklej go w drugim oknie przeglądarki. Przeciągnij kształt w jednym z okna przeglądarki; Przeniesienie kształt w oknie przeglądarki.

    ![W oknie aplikacji](tutorial-high-frequency-realtime-with-signalr/_static/image4.png)

<a id="clientloop"></a>

## <a name="add-the-client-loop"></a>Dodaj pętlę klienta

Ponieważ wysyłanie położenie kształtu na zdarzenie przesunięcia kursora myszy, co spowoduje utworzenie niepotrzebnych ilości ruchu sieciowego, wiadomości z klienta konieczne ograniczona. Użyjemy javascript `setInterval` funkcję, aby skonfigurować pętlę, która wysyła do serwera według stałej stawki ustalanej nowe informacje pozycji. Ta pętla jest bardzo proste reprezentacja "gier pętlę", wielokrotnie wywołana funkcja, która napędza wszystkich funkcji grę lub innych symulacji.

1. Zaktualizuj kod klienta, strony HTML, aby dopasować poniższy fragment kodu.

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample8.html)]

    Powyższe aktualizacji dodano m.in. `updateServerModel` funkcji, która jest wywoływana z częstotliwością, stały. Ta funkcja wysyła umieszczanie danych do serwera przy każdym `moved` flagi oznacza, że są nowe dane pozycji do wysłania.
2. Uruchom aplikację, naciskając klawisz F5. Skopiuj adres URL strony i wklej go w drugim oknie przeglądarki. Przeciągnij kształt w jednym z okna przeglądarki; Przeniesienie kształt w oknie przeglądarki. Ponieważ zostanie ograniczona liczba wiadomości, które są wysyłane do serwera, animacja nie będzie wyświetlane tak dobre, jak w poprzedniej sekcji.

    ![W oknie aplikacji](tutorial-high-frequency-realtime-with-signalr/_static/image5.png)

<a id="serverloop"></a>

## <a name="add-the-server-loop"></a>Dodaj pętlę serwera

W bieżącej aplikacji komunikatów wysyłanych z serwera do klienta wysyłana tak często, jak zostały odebrane. Przedstawia podobny problem, ponieważ zostało zaobserwowane na kliencie; częściej niż jest to konieczne, a połączenie może stać się propagowane w rezultacie można wysyłać wiadomości. W tej sekcji opisano, jak można zaktualizować serwera do zaimplementowania czasomierz, co ogranicza współczynnik wiadomości wychodzących.

1. Zastąp zawartość `MoveShapeHub.cs` następującym fragmentem kodu.

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample9.cs)]

    Powyższy kod rozwija klienta, aby dodać `Broadcaster` klasy, która ogranicza wychodzącej wiadomości przy użyciu `Timer` klasy z programu .NET framework.

    Ponieważ Centrum, sama jest przejściowy (jest tworzony za każdym razem, gdy jest to konieczne), `Broadcaster` zostanie utworzony jako pojedynczą. Inicjalizacja z opóźnieniem (wprowadzona w .NET 4) umożliwia odroczenie jej tworzenia, dopóki nie jest to potrzebne, zapewniając czy pierwsze wystąpienie koncentratora został całkowicie utworzony przed rozpoczęciem czasomierza.

    Wywołania na klientach `UpdateShape` funkcji są przenoszone poza Centrum `UpdateModel` metodę, tak że nie jest już jest wywoływana przy każdym przychodzące komunikaty są odbierane. Zamiast tego wysłania wiadomości do klientów w wysokości 25 wywołania na sekundę, zarządza `_broadcastLoop` czasomierza z poziomu `Broadcaster` klasy.

    Na koniec, a nie bezpośrednio, wywołanie metody klienta z Centrum `Broadcaster` klasa musi uzyskać odwołanie do aktualnie operacyjnych Centrum (`_hubContext`) przy użyciu `GlobalHost`.
2. Uruchom aplikację, naciskając klawisz F5. Skopiuj adres URL strony i wklej go w drugim oknie przeglądarki. Przeciągnij kształt w jednym z okna przeglądarki; Przeniesienie kształt w oknie przeglądarki. Nie będzie widoczna różnica w przeglądarce z poprzedniej sekcji, ale zostanie ograniczona liczba wiadomości, które są wysyłane do klienta.

    ![W oknie aplikacji](tutorial-high-frequency-realtime-with-signalr/_static/image6.png)

<a id="animation"></a>

## <a name="add-smooth-animation-on-the-client"></a>Dodaj płynne animacje na komputerze klienckim

Aplikacja jest niemal ukończone, ale firma Microsoft może spowodować, że jeden więcej poprawę, ruch kształt na komputerze klienckim, ponieważ są one przenoszone w odpowiedzi na komunikaty serwera. Zamiast ustawić położenie kształtu do nowej lokalizacji określonej przez serwer, użyjemy biblioteki interfejsu użytkownika JQuery `animate` funkcję, aby przesunąć kształt płynnie od jego bieżących i nowych pozycji.

1. Aktualizacja klienta programu `updateShape` metody do wyszukiwania, takie jak wyróżniony kod poniżej:

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample10.html?highlight=35-42)]

    Powyższy kod przenosi kształt z poprzedniej lokalizacji nowe podany przez serwer w ciągu interwału animacji (w tym przypadku 100 milisekund). Wszelkie poprzednie animacji uruchomionych na kształt jest czyszczona przed uruchomieniem nowej animacji.
2. Uruchom aplikację, naciskając klawisz F5. Skopiuj adres URL strony i wklej go w drugim oknie przeglądarki. Przeciągnij kształt w jednym z okna przeglądarki; Przeniesienie kształt w oknie przeglądarki. Przemieszczanie kształtu w innym oknie powinna zostać wyświetlona mniej jerky, zgodnie z jego ruchu jest interpolowane z upływem czasu, a nie raz ustawiany na wiadomości przychodzącej.

    ![W oknie aplikacji](tutorial-high-frequency-realtime-with-signalr/_static/image7.png)

<a id="furthersteps"></a>

## <a name="further-steps"></a>Trzeba wykonywać dalszych czynności

W tym samouczku wyjaśniono sposób programowania aplikacji SignalR, która wysyła komunikaty o wysokiej częstotliwości między klientami a serwerami. Ten model komunikacji jest przydatne w przypadku tworzenia gier online i innych symulacje, takie jak [ShootR gry utworzone przy użyciu SignalR](http://shootr.signalr.net).

Kompletna aplikacja utworzona w ramach tego samouczka można pobrać z [galerii kodów](https://code.msdn.microsoft.com/SignalR-MoveShape-demo-3366dac6).

Aby dowiedzieć się więcej na temat pojęć programowania SignalR, odwiedź następujące witryny dla SignalR kod źródłowy i zasoby:

- [Projekt SignalR](http://signalr.net)
- [SignalR Github i przykłady](https://github.com/SignalR/SignalR)
- [Witryny typu Wiki biblioteki SignalR](https://github.com/SignalR/SignalR/wiki)
