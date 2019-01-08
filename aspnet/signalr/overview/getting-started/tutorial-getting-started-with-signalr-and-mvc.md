---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
title: 'Samouczek: Rozmowy w czasie rzeczywistym z SignalR 2 i MVC 5 | Dokumentacja firmy Microsoft'
author: pfletcher
description: W tym samouczku pokazano, jak używać signalr2 na platformie ASP.NET do tworzenia aplikacji rozmowy w czasie rzeczywistym. Biblioteki SignalR można dodać do aplikacji MVC 5.
ms.author: riande
ms.date: 01/02/2019
ms.assetid: 80bfe5fb-bdfc-41fe-ac43-2132e5d69fac
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
msc.type: authoredcontent
ms.topic: tutorial
ms.openlocfilehash: eb4b7e1403f4070d65702b756bf98c5294c7fb17
ms.sourcegitcommit: 97d7a00bd39c83a8f6bccb9daa44130a509f75ce
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/08/2019
ms.locfileid: "54098607"
---
# <a name="tutorial-real-time-chat-with-signalr-2-and-mvc-5"></a>Samouczek: Rozmowy w czasie rzeczywistym z SignalR 2 i MVC 5

W tym samouczku pokazano, jak używać signalr2 na platformie ASP.NET do tworzenia aplikacji rozmowy w czasie rzeczywistym. Dodawanie SignalR do aplikacji MVC 5 i tworzenie widoku czatu do wysyłania i wyświetla komunikaty.

W ramach tego samouczka możesz:

> [!div class="checklist"]
> * Konfigurowanie projektu
> * Uruchamianie aplikacji przykładowej
> * Poszukaj w kodzie

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

## <a name="prerequisites"></a>Wymagania wstępne

* [Program Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) z **ASP.NET i tworzenie aplikacji internetowych** obciążenia.

## <a name="set-up-the-project"></a>Konfigurowanie projektu

W tej sekcji pokazano, jak używać programu Visual Studio 2017 i signalr2 do tworzenia pustej aplikacji ASP.NET MVC 5, dodawanie biblioteki SignalR i tworzenie aplikacji czatu.

1. W programie Visual Studio tworzenie aplikacji C# ASP.NET przeznaczonych .NET Framework 4.5, nadaj jej nazwę SignalRChat, a następnie kliknij przycisk OK.

    ![Tworzenie sieci web](tutorial-getting-started-with-signalr-and-mvc/_static/image1.png)

1. W **nowej aplikacji sieci Web ASP.NET - SignalRMvcChat**, wybierz opcję **MVC** , a następnie wybierz **Zmień uwierzytelnianie**.

1. W **Zmień uwierzytelnianie**, wybierz opcję **bez uwierzytelniania** i kliknij przycisk **OK**.

    ![Wybierz pozycję bez uwierzytelniania](tutorial-getting-started-with-signalr-and-mvc/_static/image2.png)

1. W **nowej aplikacji sieci Web ASP.NET - SignalRMvcChat**, wybierz opcję **OK**.

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj** > **nowy element**.

1. W **Dodaj nowy element - SignalRChat**, wybierz opcję **zainstalowane** > **Visual C#**   >  **Web**  >  **SignalR** , a następnie wybierz **klasa Centrum SignalR (v2)**.

1. Nazwa klasy *ChatHub* i dodaj go do projektu.

    Spowoduje to utworzenie *ChatHub.cs* plik i dodaje zestaw plików skryptów i odwołania do zestawów, obsługujące bibliotekę SignalR do projektu.

1. Zastąp kod w nowej *ChatHub.cs* plik klasy przy użyciu tego kodu:

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample1.cs)]

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj** > **klasy**.

1. Nadaj nowej klasie *uruchamiania* i dodaj go do projektu.

1. Zastąp kod w *Startup.cs* plik klasy przy użyciu tego kodu:

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample2.cs)]

1. W **Eksploratora rozwiązań**, wybierz opcję **kontrolerów** > **HomeController.cs**.

1. Dodaj tę metodę w celu *HomeController.cs*.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample3.cs)]

    Ta metoda zwraca **Chat** widok, który zostanie utworzony w późniejszym kroku.

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **widoków** > **Home**i wybierz **Dodaj**  >    **Widok**.

1. W **Dodaj widok**, Nazwij nowy widok **Chat** i wybierz **Dodaj**.

1. Zastąp zawartość **Chat.cshtml** przy użyciu tego kodu:

    [!code-cshtml[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample4.cshtml)]

1. W **Eksploratora rozwiązań**, rozwiń węzeł **skrypty**.

    Biblioteki skryptów, jQuery i SignalR są widoczne w projekcie.

    > [!IMPORTANT]
    > Menedżer pakietów może zainstalowano późniejszą wersję skrypty SignalR.

1. Upewnij się, że odwołania do skryptów w blok kodu odpowiadają wersji plików skrypt w projekcie.

    Odwołania do skryptu z oryginalnego bloku kodu:

    ```cshtml
    <!--Script references. -->
    <!--The jQuery library is required and is referenced by default in _Layout.cshtml. -->
    <!--Reference the SignalR library. -->
    <script src="~/Scripts/jquery.signalR-2.1.0.min.js"></script>
    ```

1. Jeśli nie są zgodne, zaktualizuj *.cshtml* pliku.

1. Na pasku menu wybierz **pliku** > **Zapisz wszystko**.

## <a name="run-the-sample"></a>Uruchamianie aplikacji przykładowej

1. Na pasku narzędzi, Włącz **debugowanie skryptu** a następnie wybierz przycisk Odtwórz, aby uruchomić przykład w trybie debugowania.

    ![Wprowadź nazwę użytkownika](tutorial-getting-started-with-signalr-and-mvc/_static/image3.png)

1. Po otwarciu przeglądarki, wprowadź nazwę swojej rozmowy tożsamości.

1. Skopiuj adres URL w przeglądarce otwórz dwóch innych przeglądarek i wklejać adresy URL paski adresu.

1. W każdej przeglądarce wprowadź unikatową nazwę.

1. Teraz Dodaj komentarz i wybierz pozycję **wysyłania**. Powtórz, w innych przeglądarkach. Komentarze są wyświetlane w czasie rzeczywistym.

    > [!NOTE]
    > Tej aplikacji rozmów prostego nie utrzymuje kontekst dyskusji na serwerze. Centrum emituje komentarzy do wszystkich bieżących użytkowników. Użytkownicy, którzy dołączają do czatu później zostanie wyświetlony komunikat o dodane od czasu dołączenia.

    Zobacz, jak działa aplikacji rozmów w trzech różnych przeglądarkach. Tom Anand i Susan wysyłania wiadomości, wszystkie przeglądarki aktualizacji w czasie rzeczywistym:

    ![Wszystkie trzy przeglądarki wyświetlenia tych samych Historia](tutorial-getting-started-with-signalr-and-mvc/_static/image4.png)

1. W **Eksploratora rozwiązań**, zbadaj **dokumenty skryptów** węzła dla działającej aplikacji. Brak pliku skryptu o nazwie *koncentratory* generujący biblioteki SignalR w czasie wykonywania. Ten plik zarządza komunikacji między jQuery skryptu i kod po stronie serwera.

    ![wygenerowany automatycznie koncentratory skryptu w węźle dokumentów skryptu](tutorial-getting-started-with-signalr-and-mvc/_static/image5.png)

## <a name="examine-the-code"></a>Poszukaj w kodzie

Aplikacji rozmów SignalR pokazuje dwa podstawowe zadania rozwoju SignalR. Prezentuje sposób Utwórz koncentrator. Serwer używa koncentratora jako obiekt główny koordynacji. Centrum używa biblioteki jQuery SignalR do wysyłania i odbierania komunikatów.

### <a name="signalr-hubs-in-the-chathubcs"></a>Koncentratory SignalR w ChatHub.cs

W przykładowym kodzie `ChatHub` klasa pochodzi od `Microsoft.AspNet.SignalR.Hub` klasy. Wyprowadzanie z `Hub` klasy jest to wygodny sposób, aby skompilować aplikację SignalR. Można utworzyć metody publiczne na klasy koncentratora, a następnie uzyskać dostęp do tych metod, wywołując je ze skryptów na stronie sieci web.

W kodzie Rozmowa klienci wywołują `ChatHub.Send` metodę, aby wysyłać nowy komunikat. Centrum z kolei wysyła wiadomość do wszystkich klientów, wywołując `Clients.All.addNewMessageToPage`.

`Send` Metoda pokazuje kilka koncepcji w Centrum:

* Tak, aby je wywoływać klientów do deklarowania metod publicznych w koncentratorze.

* Użyj `Microsoft.AspNet.SignalR.Hub.Clients` właściwości dynamicznych do komunikowania się ze wszystkimi klientami podłączone do tego koncentratora.

* Wywołania funkcji na komputerze klienckim (takich jak `addNewMessageToPage` funkcji) aby zaktualizować klientów.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample5.cs)]

### <a name="signalr-and-jquery-chatcshtml"></a>SignalR i jQuery Chat.cshtml

*Chat.cshtml* plik widoku w przykładzie kodu pokazano, jak użyć biblioteki jQuery SignalR do komunikowania się z Centrum SignalR.  Kod wykonuje wiele istotnych zadań. To tworzy odwołanie do serwera proxy automatycznie wygenerowana dla koncentratora, deklaruje funkcję, serwer można wywołać w celu wypychania zawartości do klientów i rozpoczyna połączenie do wysłania wiadomości do Centrum.

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample6.js)]

> [!NOTE]
> Odwołanie do klasy serwera i jej elementów członkowskich w JavaScript, znajduje się w camelCase. Odwołania do próbki kodu C# `ChatHub` klasy w języku JavaScript jako `chatHub`.

W tym bloku kodu utworzysz funkcję wywołania zwrotnego w skrypcie.

[!code-html[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample7.html)]

Klasy koncentratora na serwerze wywołuje tę funkcję, aby wypchnąć aktualizacji zawartości dla każdego klienta. Opcjonalne wywołanie `htmlEncode` funkcja przedstawiono sposób HTML zakodować zawartość komunikatu przed wyświetleniem go na stronie. Jest to sposób, aby uniemożliwić uruchomienie skryptu.

Ten kod otwiera połączenie z koncentratorem.

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample8.js)]

> [!NOTE]
> Takie podejście zapewnia ustanowić połączenie przed wykonaniem programu obsługi zdarzeń.

Kod uruchamia połączenie i przekazuje go po funkcji do obsługi zdarzenia click na **wysyłania** przycisk na stronie rozmowy.

## <a name="additional-resources"></a>Dodatkowe zasoby

Aby uzyskać więcej informacji na temat biblioteki SignalR zobacz następujące zasoby:

* [Projekt SignalR](http://signalr.net)

* [SignalR GitHub i przykłady](https://github.com/SignalR/SignalR)

* [Witryny typu Wiki biblioteki SignalR](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a>Następne kroki

W ramach tego samouczka możesz:

> [!div class="checklist"]
> * Konfigurowanie projektu
> * Uruchomiono próbki
> * Zbadanie kodu

Przejdź do następnego artykułu, aby dowiedzieć się, jak utworzyć aplikację sieci web korzystającą z signalr2 na platformie ASP.NET w celu zapewnienia funkcji obsługi wiadomości o wysokiej częstotliwości.
> [!div class="nextstepaction"]
> [Aplikacja sieci Web z obsługą komunikatów o wysokiej częstotliwości](tutorial-high-frequency-realtime-with-signalr.md)