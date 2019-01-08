---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr
title: 'Samouczek: Rozmowy w czasie rzeczywistym przy użyciu SignalR 2 | Dokumentacja firmy Microsoft'
author: pfletcher
description: Ten samouczek pokazuje, jak utworzyć aplikację do obsługi rozmów w czasie rzeczywistym przy użyciu SignalR. Możesz dodać SignalR na pustą aplikację sieci web platformy ASP.NET.
ms.author: riande
ms.date: 01/02/2019
ms.assetid: a8b3b778-f009-4369-85c7-e90f9878d8b4
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr
msc.type: authoredcontent
ms.topic: tutorial
ms.openlocfilehash: aa015abc47bb2450e04e167c0404aaa1d119ba2c
ms.sourcegitcommit: 97d7a00bd39c83a8f6bccb9daa44130a509f75ce
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/08/2019
ms.locfileid: "54098627"
---
# <a name="tutorial-real-time-chat-with-signalr-2"></a>Samouczek: Rozmowy w czasie rzeczywistym przy użyciu SignalR 2

Ten samouczek pokazuje, jak utworzyć aplikację do obsługi rozmów w czasie rzeczywistym przy użyciu SignalR. Dodaj SignalR do pustych aplikacji sieci web ASP.NET i Utwórz stronę HTML do wysyłania i wyświetla komunikaty.

W ramach tego samouczka możesz:

> [!div class="checklist"]
> * Konfigurowanie projektu
> * Uruchamianie aplikacji przykładowej
> * Poszukaj w kodzie

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

## <a name="prerequisites"></a>Wymagania wstępne

* [Program Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) z **ASP.NET i tworzenie aplikacji internetowych** obciążenia.

## <a name="set-up-the-project"></a>Konfigurowanie projektu

W tej sekcji pokazano, jak utworzyć pustą aplikację sieci web platformy ASP.NET, za pomocą programu Visual Studio 2017 i SignalR 2 Dodaj SignalR i tworzenie aplikacji czatu.

1. W programie Visual Studio należy utworzyć aplikację sieci Web platformy ASP.NET.

    ![Tworzenie sieci web](tutorial-getting-started-with-signalr/_static/image2.png)

1. W **nowy projekt ASP.NET - SignalRChat** okna, pozostaw **pusty** zaznaczone, a następnie wybierz **OK**.

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj** > **nowy element**.

1. W **Dodaj nowy element - SignalRChat**, wybierz opcję **zainstalowane** > **Visual C#**   >  **Web**  >  **SignalR** , a następnie wybierz **klasa Centrum SignalR (v2)**.

1. Nazwa klasy *ChatHub* i dodaj go do projektu.

    Spowoduje to utworzenie *ChatHub.cs* plik i dodaje zestaw plików skryptów i odwołania do zestawów, obsługujące bibliotekę SignalR do projektu.

1. Zastąp kod w nowej *ChatHub.cs* plik klasy przy użyciu tego kodu:

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample1.cs)]

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj** > **nowy element**.

1. W **Dodaj nowy element - SignalRChat** wybierz **zainstalowane** > **Visual C#**   >  **Web** a Wybierz **klasy początkowej OWIN**.

1. Nazwa klasy *uruchamiania* i dodaj go do projektu.

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj** > **strony HTML**.

1. Nadaj nowej stronie *indeksu* i wybierz **OK**.

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy strony HTML został utworzony i wybierz **Ustaw jako strona startowa**.

1. Zastąp kod domyślnej strony HTML przy użyciu tego kodu:

    [!code-html[Main](tutorial-getting-started-with-signalr/samples/sample3.html)]

1. W **Eksploratora rozwiązań**, rozwiń węzeł **skrypty**.

    Biblioteki skryptów, jQuery i SignalR są widoczne w projekcie.

    > [!IMPORTANT]
    > Menedżer pakietów może zainstalowano późniejszą wersję skrypty SignalR.

1. Upewnij się, że odwołania do skryptów w blok kodu odpowiadają wersji plików skrypt w projekcie.

    Odwołania do skryptu z oryginalnego bloku kodu:
    ```html
    <!--Script references. -->
    <!--Reference the jQuery library. -->
    <script src="Scripts/jquery-3.1.1.min.js" ></script>
    <!--Reference the SignalR library. -->
    <script src="Scripts/jquery.signalR-2.2.1.min.js"></script>
    ```

1. Jeśli nie są zgodne, zaktualizuj *.html* pliku.

1. Na pasku menu wybierz **pliku** > **Zapisz wszystko**.

## <a name="run-the-sample"></a>Uruchamianie aplikacji przykładowej

1. Na pasku narzędzi, Włącz **debugowanie skryptu** a następnie wybierz przycisk Odtwórz, aby uruchomić przykład w trybie debugowania.

    ![Wprowadź nazwę użytkownika](tutorial-getting-started-with-signalr/_static/image3.png)

1. Po otwarciu przeglądarki, wprowadź nazwę swojej rozmowy tożsamości.

1. Skopiuj adres URL w przeglądarce otwórz dwóch innych przeglądarek i wklejać adresy URL paski adresu.

1. W każdej przeglądarce wprowadź unikatową nazwę.

1. Teraz Dodaj komentarz i wybierz pozycję **wysyłania**. Powtórz, w innych przeglądarkach. Komentarze są wyświetlane w czasie rzeczywistym.

    > [!NOTE]
    > Tej aplikacji rozmów prostego nie utrzymuje kontekst dyskusji na serwerze. Centrum emituje komentarzy do wszystkich bieżących użytkowników. Użytkownicy, którzy dołączają do czatu później zostanie wyświetlony komunikat o dodane od czasu dołączenia.

    Zobacz, jak działa aplikacji rozmów w trzech różnych przeglądarkach. Tom Anand i Susan wysyłania wiadomości, wszystkie przeglądarki aktualizacji w czasie rzeczywistym:

    ![Wszystkie trzy przeglądarki wyświetlenia tych samych Historia](tutorial-getting-started-with-signalr/_static/image4.png)

1. W **Eksploratora rozwiązań**, zbadaj **dokumenty skryptów** węzła dla działającej aplikacji. Brak pliku skryptu o nazwie *koncentratory* generujący biblioteki SignalR w czasie wykonywania. Ten plik zarządza komunikacji między jQuery skryptu i kod po stronie serwera.

    ![wygenerowany automatycznie koncentratory skryptu w węźle dokumentów skryptu](tutorial-getting-started-with-signalr/_static/image5.png)

## <a name="examine-the-code"></a>Poszukaj w kodzie

Aplikacja SignalRChat pokazuje dwa podstawowe zadania rozwoju SignalR. Prezentuje sposób Utwórz koncentrator. Serwer używa koncentratora jako obiekt główny koordynacji. Centrum używa biblioteki jQuery SignalR do wysyłania i odbierania komunikatów.

### <a name="signalr-hubs-in-the-chathubcs"></a>Koncentratory SignalR w ChatHub.cs

W powyższym przykładowym kodzie `ChatHub` klasa pochodzi od `Microsoft.AspNet.SignalR.Hub` klasy. Wyprowadzanie z `Hub` klasy jest to wygodny sposób, aby skompilować aplikację SignalR. Można utworzyć metody publiczne na klasie Centrum i następnie użyć tych metod, wywołując je ze skryptów na stronie sieci web.

W kodzie Rozmowa klienci wywołują `ChatHub.Send` metodę, aby wysyłać nowy komunikat. Centrum wiadomość jest wysyłana do wszystkich klientów, wywołując `Clients.All.broadcastMessage`.

`Send` Metoda pokazuje kilka koncepcji w Centrum:

* Tak, aby je wywoływać klientów do deklarowania metod publicznych w koncentratorze.

* Użyj `Microsoft.AspNet.SignalR.Hub.Clients` właściwości dynamicznych do komunikowania się ze wszystkimi klientami podłączone do tego koncentratora.

* Wywołania funkcji na komputerze klienckim (takich jak `broadcastMessage` funkcji) aby zaktualizować klientów.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample4.cs)]

### <a name="signalr-and-jquery-in-the-indexhtml"></a>SignalR i jQuery w index.html

*Index.html* strony w przykładzie kodu pokazano, jak użyć biblioteki jQuery SignalR do komunikowania się z Centrum SignalR. Kod wykonuje wiele istotnych zadań. Go deklaruje serwer proxy, aby odwoływać się do koncentratora, deklaruje funkcję, serwer można wywołać w celu wypychania zawartości do klientów i rozpoczyna połączenie do wysłania wiadomości do Centrum.

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample5.js)]

> [!NOTE]
> W języku JavaScript, musi być camelCase odwołanie do klasy serwera i jej elementów członkowskich. Odwołania do próbki kodu C# *ChatHub* klasy w języku JavaScript jako `chatHub`.

W tym bloku kodu utworzysz funkcję wywołania zwrotnego w skrypcie.

[!code-html[Main](tutorial-getting-started-with-signalr/samples/sample6.html)]

Klasy koncentratora na serwerze wywołuje tę funkcję, aby wypchnąć aktualizacji zawartości dla każdego klienta. Dwa wiersze, że — kodowanie HTML zawartości przed wyświetleniem go są opcjonalne i Pokaż dobrym sposobem, aby uniemożliwić uruchomienie skryptu.

Ten kod otwiera połączenie z koncentratorem.

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample7.js)]

> [!NOTE]
> Takie podejście zapewnia, że kod nawiąże połączenie, przed wykonaniem programu obsługi zdarzeń.

Kod uruchamia połączenie i przekazuje go po funkcji do obsługi zdarzenia click na **wysyłania** przycisk na stronie HTML.

## <a name="additional-resources"></a>Dodatkowe zasoby

Aby uzyskać więcej informacji na temat biblioteki SignalR zobacz następujące zasoby:

* [Projekt SignalR](http://signalr.net)

* [SignalR Github i przykłady](https://github.com/SignalR/SignalR)

* [Witryny typu Wiki biblioteki SignalR](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a>Następne kroki

W tym samouczku możesz:

> [!div class="checklist"]
> * Konfigurowanie projektu
> * Uruchomiono próbki
> * Zbadanie kodu

Przejdź do następnego artykułu, aby dowiedzieć się, jak używać biblioteki SignalR i MVC 5.
> [!div class="nextstepaction"]
> [SignalR 2 i MVC 5](tutorial-getting-started-with-signalr-and-mvc.md)