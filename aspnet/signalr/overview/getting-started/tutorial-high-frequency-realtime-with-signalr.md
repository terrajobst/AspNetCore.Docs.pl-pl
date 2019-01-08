---
uid: signalr/overview/getting-started/tutorial-high-frequency-realtime-with-signalr
title: 'Samouczek: Tworzenie aplikacji w czasie rzeczywistym z wysoką częstotliwością przy użyciu SignalR 2 | Dokumentacja firmy Microsoft'
author: pfletcher
description: W tym samouczku przedstawiono sposób tworzenia aplikacji sieci web, która używa biblioteki SignalR platformy ASP.NET w celu zapewnienia wysokiej częstotliwości funkcje obsługi komunikatów.
ms.author: riande
ms.date: 01/02/2019
ms.assetid: 9f969dda-78ea-4329-b1e3-e51c02210a2b
msc.legacyurl: /signalr/overview/getting-started/tutorial-high-frequency-realtime-with-signalr
msc.type: authoredcontent
ms.topic: tutorial
ms.openlocfilehash: 85503db0b41be6f87136627667d6dd71f0d4f609
ms.sourcegitcommit: 97d7a00bd39c83a8f6bccb9daa44130a509f75ce
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/08/2019
ms.locfileid: "54098593"
---
# <a name="tutorial-create-high-frequency-real-time-app-with-signalr-2"></a>Samouczek: Tworzenie aplikacji w czasie rzeczywistym z wysoką częstotliwością przy użyciu SignalR 2

W tym samouczku przedstawiono sposób tworzenia aplikacji sieci web korzystającą z signalr2 na platformie ASP.NET w celu zapewnienia funkcji obsługi wiadomości o wysokiej częstotliwości. W tym przypadku "komunikaty o wysokiej częstotliwości" oznacza, że serwer wysyła aktualizacje według stałej stawki. Możesz wysyłać maksymalnie 10 komunikatów na sekundę.

Aplikacji, którą utworzysz Wyświetla kształtu, w którym można przeciągać użytkowników. Serwer aktualizacji położenie kształtu we wszystkich przeglądarkach połączonych, aby dopasować położenie przeciąganego kształtu przy użyciu Przekroczono limit czasu aktualizacji.

Pojęciami opisanymi w tym samouczku mają aplikacji w czasie rzeczywistym gry i inne aplikacje symulacji.

W ramach tego samouczka możesz:

> [!div class="checklist"]
> * Konfigurowanie projektu
> * Tworzenie podstawowej aplikacji
> * Mapowanie do koncentratora, po uruchomieniu aplikacji
> * Dodaj klienta
> * Uruchamianie aplikacji
> * Dodaj pętlę klienta
> * Dodaj pętlę serwera
> * Dodaj płynne animacje

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

## <a name="prerequisites"></a>Wymagania wstępne

* [Program Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) z **ASP.NET i tworzenie aplikacji internetowych** obciążenia.

## <a name="set-up-the-project"></a>Konfigurowanie projektu

W tej sekcji utworzysz projekt programu Visual Studio 2017.

W tej sekcji pokazano, jak utworzyć pustą aplikację sieci Web platformy ASP.NET i dodać biblioteki SignalR i jQuery.UI przy użyciu programu Visual Studio 2017.

1. W programie Visual Studio należy utworzyć aplikację sieci Web platformy ASP.NET.

    ![Tworzenie sieci web](tutorial-high-frequency-realtime-with-signalr/_static/image1.png)

1. W **nowej aplikacji sieci Web ASP.NET - MoveShapeDemo** okna, pozostaw **pusty** zaznaczone, a następnie wybierz **OK**.

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj** > **nowy element**.

1. W **Dodaj nowy element - MoveShapeDemo**, wybierz opcję **zainstalowane** > **Visual C#**   >  **Web**  >  **SignalR** , a następnie wybierz **klasa Centrum SignalR (v2)**.

1. Nazwa klasy *MoveShapeHub* i dodaj go do projektu.

    Spowoduje to utworzenie *MoveShapeHub.cs* pliku klasy. Jednocześnie dodaje zestaw plików skryptów i odwołania do zestawów, obsługujące bibliotekę SignalR do projektu.

1. Wybierz **narzędzia** > **Menedżera pakietów NuGet** > **Konsola Menedżera pakietów**.

1. W **Konsola Menedżera pakietów**, uruchom następujące polecenie:

    ```console
    Install-Package jQuery.UI.Combined
    ```

    Polecenie instaluje biblioteki interfejsu użytkownika jQuery. Umożliwia ona animować kształt.

1. W **Eksploratora rozwiązań**, rozwiń węzeł skryptów.

    ![Odwołania do biblioteki skryptu](tutorial-high-frequency-realtime-with-signalr/_static/image2.png)

    Biblioteki skryptów, jQuery, jQueryUI i SignalR są widoczne w projekcie.

## <a name="create-the-base-application"></a>Tworzenie podstawowej aplikacji

W tej sekcji utworzysz aplikację przeglądarki. Aplikacja wysyła położenie kształtu do serwera podczas każdego zdarzenie przesunięcia kursora myszy. Serwer wysyła te informacje do wszystkich innych połączonych klientów w czasie rzeczywistym. Dowiesz się więcej na temat tej aplikacji w kolejnych sekcjach.

1. Otwórz *MoveShapeHub.cs* pliku.

1. Zastąp kod w *MoveShapeHub.cs* pliku przy użyciu tego kodu:

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample1.cs)]

1. Zapisz plik.

`MoveShapeHub` Klasa jest implementacją Centrum SignalR. Podobnie jak w [wprowadzenie do SignalR](tutorial-getting-started-with-signalr.md) samouczku Centrum ma metodę, która bezpośrednio wywoływać klientów. W tym przypadku klient przesyła obiektu za pomocą nowego X i Y koordynuje kształtu do serwera. Tych współrzędnych Pobierz wysłano do wszystkich innych połączonych klientów. SignalR serializuje automatycznie tego obiektu przy użyciu formatu JSON.

Wysyła aplikacji `ShapeModel` obiektu do klienta. Zawiera elementy członkowskie do przechowywania położenie kształtu. Wersja obiektu na serwerze ma również członkiem, aby śledzić przechowywanych danych których klienta. Ten obiekt zapobiega wysłaniem dane klienta z powrotem do samego serwera. Używa tego elementu członkowskiego `JsonIgnore` atrybutu, aby uniemożliwić aplikacji serializacji danych i wysłanie go do klienta.

## <a name="map-to-the-hub-when-app-starts"></a>Mapowanie do koncentratora, po uruchomieniu aplikacji

Następnie należy skonfigurować mapowanie do Centrum podczas uruchamiania aplikacji. W SignalR 2 Dodawanie klasy początkowej OWIN tworzy mapowanie.

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj** > **nowy element**.

1. W **Dodaj nowy element - MoveShapeDemo** wybierz **zainstalowane** > **Visual C#**   >  **Web** a Wybierz **klasy początkowej OWIN**.

1. Nazwa klasy *uruchamiania* i wybierz **OK**.

1. Zastąp kod domyślne *Startup.cs* pliku przy użyciu tego kodu:

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample2.cs)]

Wywołuje klasę uruchamiania OWIN `MapSignalR` gdy aplikacja wykonuje `Configuration` metody. Aplikacja dodaje Klasa początkowa OWIN dla przetwarzania przy użyciu `OwinStartup` atrybutu zestawu.

## <a name="add-the-client"></a>Dodaj klienta

Dodaj stronę HTML dla klienta.

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj** > **strony HTML**.

1. Nazwij stronę **domyślne** i wybierz **OK**.

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *Default.html* i wybierz **Ustaw jako strona startowa**.

1. Zastąp kod domyślne *Default.html* pliku przy użyciu tego kodu:

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample3.html?highlight=14-16)]

1. W **Eksploratora rozwiązań**, rozwiń węzeł **skrypty**.

    Biblioteki skryptów, jQuery i SignalR są widoczne w projekcie.

    > [!IMPORTANT]
    > Menedżer pakietów instaluje późniejszą wersję skrypty SignalR.

1. Aktualizuj odwołania do skryptu w bloku kodu, aby odpowiadać wersji plików skrypt w projekcie.

Ten kod HTML i JavaScript tworzy czerwonego `div` o nazwie `shape`. Włącza zachowanie przeciągania kształtu, za pomocą biblioteki jQuery i używa `drag` zdarzenia do wysłania położenie kształtu do serwera.

## <a name="run-the-app"></a>Uruchamianie aplikacji

Aplikację można uruchomić na se'e pracy. Podczas przeciągania kształtu wokół okna przeglądarki kształt zbyt przenosi w innych przeglądarkach.

1. Na pasku narzędzi, Włącz **debugowanie skryptu** a następnie wybierz przycisk Odtwórz, aby uruchomić aplikację w trybie debugowania.

    ![Zrzut ekranu przedstawiający użytkownika, włączając tryb debugowania i wybierając polecenie play.](tutorial-high-frequency-realtime-with-signalr/_static/image3.png)

    Zostanie otwarte okno przeglądarki z czerwonym kształtu w prawym górnym rogu.

1. Skopiuj adres URL strony.

1. Otwórz innej przeglądarki i wklej adres URL na pasku adresu.

1. Przeciągnij kształt w jednym z okna przeglądarki. Następuje kształt w oknie przeglądarki.

Podczas gdy aplikacji funkcji przy użyciu tej metody, nie jest zalecany model programowania. Nie ma żadnego górnego limitu liczby wiadomości wysyłanych wprowadzenie. W rezultacie klientami a serwerem Pobierz przeciążeniu z wiadomościami i spadku wydajności. Ponadto aplikacja wyświetla rozłączonych animacji na komputerze klienckim. Ta animacja jerky wynika kształt natychmiast przechodzi przez każdą z tych metod. Zaleca kształt bezproblemową do każdej nowej lokalizacji. Następnie dowiesz się, jak rozwiązać te problemy.

## <a name="add-the-client-loop"></a>Dodaj pętlę klienta

Wysyłanie położenie kształtu na każdym zdarzenie przesunięcia kursora myszy tworzy niepotrzebne ilości ruchu sieciowego. Aplikacja musi ograniczania wiadomości od klienta.

Użyj javascript `setInterval` funkcję, aby skonfigurować pętlę, która wysyła do serwera według stałej stawki ustalanej nowe informacje pozycji. Ta pętla jest reprezentacją podstawowe "pętli gry". Jest wielokrotnie funkcji obsługujące funkcje gier.

1. Zastąp kod klienta w *Default.html* pliku przy użyciu tego kodu:

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample4.html?highlight=14-16)]

    > [!IMPORTANT]
    > Musisz ponownie Zamień odwołania do skryptu. Muszą one być zgodne wersje skrypty w projekcie.

    Ten nowy kod dodaje `updateServerModel` funkcji. Pobiera ona wywoływana z częstotliwością, stały. Funkcja wysyła umieszczanie danych do serwera przy każdym `moved` flagi oznacza, że są nowe dane pozycji do wysłania.

1. Wybierz przycisk Odtwórz, aby uruchomić aplikację

1. Skopiuj adres URL strony.

1. Otwórz innej przeglądarki i wklej adres URL na pasku adresu.

1. Przeciągnij kształt w jednym z okna przeglądarki. Następuje kształt w oknie przeglądarki.

Ponieważ aplikacja ogranicza liczbę wiadomości, które są wysyłane do serwera, animacja nie będzie wyświetlane tak dobre, czy na początku.

## <a name="add-the-server-loop"></a>Dodaj pętlę serwera

W bieżącej aplikacji komunikatów wysyłanych z serwera do klienta wysyłana tak często, jak ich otrzymaniu. Jak widać na komputerze klienckim, ten ruch sieciowy przedstawia podobny problem.

Aplikacja może wysyłać komunikaty częściej niż zajdzie taka potrzeba. Połączenie może być propagowane w wyniku. Ta sekcja opisuje sposób aktualizacji server, aby dodać czasomierz, co ogranicza współczynnik wiadomości wychodzących.

1. Zastąp zawartość `MoveShapeHub.cs` przy użyciu tego kodu:

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample5.cs)]

1. Wybierz przycisk Odtwórz, aby uruchomić aplikację.

1. Skopiuj adres URL strony.

1. Otwórz innej przeglądarki i wklej adres URL na pasku adresu.

1. Przeciągnij kształt w jednym z okna przeglądarki.

Ten kod rozwija klienta, aby dodać `Broadcaster` klasy. Nowa klasa ogranicza wiadomości wychodzących za pomocą `Timer` klasy z programu .NET framework.

Zaleca się dowiedzieć się, że sam Centrum przejściowe. Jest tworzony za każdym razem, gdy jest to konieczne. Aby aplikacja tworzy `Broadcaster` jako pojedynczą. Używa inicjowania z opóźnieniem mają być odroczone `Broadcaster`jego tworzenia, dopóki nie jest to konieczne. Pozwala to zagwarantować, że aplikacja tworzy pierwsze wystąpienie koncentratora całkowicie przed rozpoczęciem czasomierza.

Wywołania na klientach `UpdateShape` funkcji są przenoszone poza Centrum `UpdateModel` metody. Nie jest już nazywa się natychmiast, gdy aplikacja odbiera wiadomości przychodzących. Zamiast tego aplikacja wysyła komunikaty do klientów w wysokości 25 wywołania na sekundę. Ten proces odbywa się przez `_broadcastLoop` czasomierza z poziomu `Broadcaster` klasy.

Na koniec, a nie bezpośrednio, wywołanie metody klienta z Centrum `Broadcaster` klasa musi uzyskać odwołanie do aktualnie działających `_hubContext` koncentratora. Pobiera odwołanie z `GlobalHost`.

## <a name="add-smooth-animation"></a>Dodaj płynne animacje

Aplikacja jest prawie gotowy, ale firma Microsoft może spowodować, że jeden więcej poprawy jakości obsługi. Aplikacja spowoduje przesunięcie kształtu na komputerze klienckim w odpowiedzi na komunikaty serwera. Zamiast ustawiać położenie kształtu do nowej lokalizacji określonej przez serwer, użyj biblioteki interfejsu użytkownika JQuery `animate` funkcji. To płynnie przeniesienie kształt od jego bieżących i nowych pozycji.

1. Aktualizacja klienta programu `updateShape` method in Class metoda *Default.html* pliku, aby wyglądał jak wyróżniony kod:

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample6.html?highlight=33-40)]

1. Wybierz przycisk Odtwórz, aby uruchomić aplikację.

1. Skopiuj adres URL strony.

1. Otwórz innej przeglądarki i wklej adres URL na pasku adresu.

1. Przeciągnij kształt w jednym z okna przeglądarki.

Przemieszczanie kształtu w innym oknie zostanie wyświetlona mniej jerky. Aplikacja argumentu ruchu za pośrednictwem czasu, a nie raz ustawiany na wiadomości przychodzącej.

Ten kod przenosi kształt z poprzedniej lokalizacji na nową. Serwer udostępnia położenie kształtu w miarę upływu interwał animacji. W tym przypadku to 100 milisekund. Aplikacja czyści wszystkie poprzednie animacje systemem kształtu, przed rozpoczęciem nowej animacji.

## <a name="additional-resources"></a>Dodatkowe zasoby

Komunikat przedstawia informacje na temat przyjęła przydatne do tworzenia gier online i innych symulacje, takie jak [ShootR gry utworzone przy użyciu SignalR](https://shootr.azurewebsites.net/).

Aby uzyskać więcej informacji na temat biblioteki SignalR zobacz następujące zasoby:

* [Projekt SignalR](http://signalr.net)

* [SignalR GitHub i przykłady](https://github.com/SignalR/SignalR)

* [Witryny typu Wiki biblioteki SignalR](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a>Następne kroki

W ramach tego samouczka możesz:

> [!div class="checklist"]
> * Konfigurowanie projektu
> * Podstawowa aplikacja
> * Mapowany do koncentratora, po uruchomieniu aplikacji
> * Dodaje klienta
> * Uruchomienia aplikacji
> * Dodano pętlę klienta
> * Dodano pętlę serwera
> * Dodano płynne animacje

Przejdź do następnego artykułu, aby dowiedzieć się, jak utworzyć aplikację sieci web korzystającą z signalr2 na platformie ASP.NET w celu zapewnienia funkcji emisji serwera.
> [!div class="nextstepaction"]
> [SignalR 2 i emisji serwera](tutorial-server-broadcast-with-signalr.md)