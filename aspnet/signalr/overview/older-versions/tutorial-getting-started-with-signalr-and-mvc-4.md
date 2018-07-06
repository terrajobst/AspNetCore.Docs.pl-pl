---
uid: signalr/overview/older-versions/tutorial-getting-started-with-signalr-and-mvc-4
title: 'Samouczek: Wprowadzenie do SignalR 1.x i MVC 4 | Dokumentacja firmy Microsoft'
author: pfletcher
description: Użyj biblioteki SignalR platformy ASP.NET i ASP.NET MVC 4, aby utworzyć aplikację do rozmów w czasie rzeczywistym.
ms.author: aspnetcontent
ms.date: 03/29/2013
ms.assetid: eeef9f73-6de3-49f9-b50b-9af22108f2ce
msc.legacyurl: /signalr/overview/older-versions/tutorial-getting-started-with-signalr-and-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: 2d9f983a859f2920154d2021bb313ffa7300198e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37823478"
---
<a name="tutorial-getting-started-with-signalr-1x-and-mvc-4"></a>Samouczek: Wprowadzenie do SignalR 1.x i MVC 4
====================
przez [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)

> W tym samouczku pokazano, jak używać biblioteki SignalR platformy ASP.NET do tworzenia aplikacji rozmowy w czasie rzeczywistym. Będzie dodać SignalR do aplikacji MVC 4 i utworzyć widok czatu do wysyłania i wyświetla komunikaty.


## <a name="overview"></a>Omówienie

Ten samouczek zawiera wprowadzenie do tworzenia aplikacji sieci web w czasie rzeczywistym przy użyciu biblioteki SignalR platformy ASP.NET i ASP.NET MVC 4. W tym samouczku użyto tego samego kodu aplikacji rozmów jako [samouczka Wprowadzenie do SignalR](tutorial-getting-started-with-signalr.md), ale pokazuje, jak dodać go do aplikacji MVC 4, na podstawie szablonu Internet.

W tym temacie dowiesz się, następujące zadania deweloperskie SignalR:

- Dodawanie biblioteki SignalR do aplikacji MVC 4.
- Tworzenie klasy koncentratora, aby wypchnąć zawartości do klientów.
- Przy użyciu biblioteki jQuery SignalR na stronie sieci web umożliwiają przesyłanie komunikatów oraz wyświetlania aktualizacji z koncentratora.

Poniższy zrzut ekranu przedstawia zakończonych rozmów uruchomieniu aplikacji w przeglądarce.

![Wystąpienia rozmowy](tutorial-getting-started-with-signalr-and-mvc-4/_static/image2.png)

Sekcje:

- [Konfigurowanie projektu](#setup)
- [Uruchamianie aplikacji przykładowej](#run)
- [Poszukaj w kodzie](#code)
- [Następne kroki](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a>Konfigurowanie projektu

Wymagania wstępne:

- Visual Studio 2010 SP1, program Visual Studio 2012 lub Visual Studio 2012 Express. Jeśli nie masz programu Visual Studio, zobacz [ASP.NET pliki do pobrania](https://www.asp.net/downloads) można pobrać bezpłatny program Visual Studio 2012 Express narzędzia programistyczne.
- Dla programu Visual Studio 2010, należy zainstalować [platformy ASP.NET MVC 4](https://www.microsoft.com/download/details.aspx?id=30683).

W tej sekcji przedstawiono sposób tworzenia aplikacji ASP.NET MVC 4, dodawanie biblioteki SignalR i tworzenie aplikacji czatu.

1. 1. W programie Visual Studio tworzenie aplikacji ASP.NET MVC 4, nadaj jej nazwę SignalRChat, a następnie kliknij przycisk OK.

        > [!NOTE]
        > W programie VS 2010, wybrać **.NET Framework 4** z rozwijanej listy formantów wersji Framework. Wersje programu .NET Framework 4 i 4.5 działa kod SignalR.

        ![Tworzenie sieci web mvc](tutorial-getting-started-with-signalr-and-mvc-4/_static/image3.png)
      2. Wybierz szablon aplikacji internetowych, wyczyść opcję **Tworzenie projektu testu jednostkowego**i kliknij przycisk OK.

         ![Tworzenie witryny internetowej mvc](tutorial-getting-started-with-signalr-and-mvc-4/_static/image4.png)
      3. Otwórz **narzędzia | Menedżer pakietów biblioteki | Konsola Menedżera pakietów** i uruchom następujące polecenie. Ten krok powoduje dodanie do projektu, zbiór plików skryptów i odwołania do zestawów, które umożliwiają funkcji SignalR.

         `install-package Microsoft.AspNet.SignalR -Version 1.1.3`
      4. W **Eksploratora rozwiązań** rozwiń folder skryptów. Należy pamiętać, że biblioteki skryptów dla elementu SignalR zostały dodane do projektu.

         ![Odwołań do biblioteki](tutorial-getting-started-with-signalr-and-mvc-4/_static/image6.png)
      5. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt, wybierz **Dodaj | Nowy Folder**, i Dodaj nowy folder o nazwie **koncentratory**.
      6. Kliknij prawym przyciskiem myszy **Hubs** folderu, kliknij przycisk **Dodaj | Klasa**i Utwórz nową klasę C# o nazwie **ChatHub.cs**. Ta klasa będzie używany jako koncentrator serwera SignalR, która wysyła komunikaty do wszystkich klientów.

> [!NOTE]
> Jeśli używasz programu Visual Studio 2012 i zainstalowano [aktualizacji platformy ASP.NET i Web Tools 2012.2](../../../visual-studio/overview/2012/aspnet-and-web-tools-20122-release-notes-rtw.md#_Installation), nowy szablon elementu SignalR można użyć do utworzenia klasy koncentratora. Aby to zrobić, kliknij prawym przyciskiem myszy **Hubs** folderu, kliknij przycisk **Dodaj | Nowy element**, wybierz opcję **klasa Centrum SignalR (v1)** i nazwę klasy **ChatHub.cs**.


1. Zastąp kod w **ChatHub** klasy z następującym kodem.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample1.cs)]
2. Otwórz **Global.asax** pliku projektu i dodaj wywołanie do metody `RouteTable.Routes.MapHubs();` jako pierwszy wiersz kodu w `Application_Start` metody. Ten kod rejestruje domyślną trasę dla centrów SignalR i musi zostać wywołana przed zarejestrowaniem jakichkolwiek innych tras. Gotowy `Application_Start` metoda wygląda podobnie jak w poniższym przykładzie.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample2.cs)]
3. Edytuj `HomeController` klasy znalezione w **Controllers/HomeController.cs** i dodaj następującą metodę do klasy. Ta metoda zwraca **Chat** widok, który spowoduje utworzenie w późniejszym kroku.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample3.cs)]
4. Kliknij prawym przyciskiem myszy w obrębie `Chat` metoda właśnie utworzony i kliknij przycisk **Dodaj widok** do utworzenia nowego pliku widoku.
5. W **Dodaj widok** okna dialogowego, upewnij się, że zaznaczone jest pole wyboru do **Użyj układ strony wzorcowej** (Usuń zaznaczenie pola wyboru), a następnie kliknij przycisk **Dodaj**.

    ![Dodawanie widoku](tutorial-getting-started-with-signalr-and-mvc-4/_static/image8.png)
6. Edytuj nowy plik widoku o nazwie **Chat.cshtml**. Po &lt;h2&gt; tag, wklej następujący kod &lt;div&gt; sekcji i `@section scripts` blok kodu do strony. Ten skrypt umożliwia strony Aby wysyłać wiadomości rozmów i wyświetlenie komunikatów z serwera. Kompletny kod dla widoku rozmowy pojawia się w następującym fragmencie kodu.

    > [!IMPORTANT]
    > Po dodaniu SignalR i inne biblioteki skryptu do projektu programu Visual Studio, Menedżera pakietów może zainstalować wersje skrypty, które są nowsze niż wersje przedstawione w tym temacie. Upewnij się, że odwołania do skryptu w kodzie są takie same wersje bibliotek skryptów zainstalowana w twoim projekcie.

    [!code-cshtml[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample4.cshtml)]
7. **Zapisz wszystko** dla projektu.

<a id="run"></a>

## <a name="run-the-sample"></a>Uruchamianie aplikacji przykładowej

1. Naciśnij klawisz F5, aby uruchomić projekt w trybie debugowania.
2. W wierszu adresu przeglądarki, dołącz **/home/rozmowy** adres URL domyślnej strony dla projektu. Rozmowa strona ładuje się w wystąpieniu przeglądarki i wyświetla monit o nazwę użytkownika.

    ![Wprowadź nazwę użytkownika](tutorial-getting-started-with-signalr-and-mvc-4/_static/image9.png)
3. Wprowadź nazwę użytkownika.
4. Skopiuj adres URL w wierszu adresu przeglądarki i użyj go, aby otworzyć dwóch większej liczby wystąpień przeglądarki. W każdym wystąpieniu przeglądarki wprowadź unikatową nazwę użytkownika.
5. W każdym wystąpieniu przeglądarki Dodaj komentarz, a następnie kliknij przycisk **wysyłania**. Komentarze powinien być wyświetlany we wszystkich wystąpieniach przeglądarki.

    > [!NOTE]
    > Tej aplikacji rozmów prostego nie utrzymuje kontekst dyskusji na serwerze. Centrum emituje komentarzy do wszystkich bieżących użytkowników. Użytkownicy, którzy dołączają do czatu później zostanie wyświetlony komunikat o dodane od czasu dołączenia.
6. Poniższy zrzut ekranu przedstawia aplikacji rozmów w przeglądarce.

    ![Rozmowa przeglądarki](tutorial-getting-started-with-signalr-and-mvc-4/_static/image11.png)
7. W **Eksploratora rozwiązań**, zbadaj **dokumenty skryptów** węzła dla działającej aplikacji. Ten węzeł jest widoczny w trybie debugowania, jeśli używasz programu Internet Explorer jako przeglądarki. Brak pliku skryptu o nazwie **koncentratory** generujący biblioteki SignalR dynamicznie w czasie wykonywania. Ten plik zarządza komunikacji między jQuery skryptu i kod po stronie serwera. Jeśli używasz przeglądarki innej niż Internet Explorer, można także przejść do dynamicznego **hubs** plików, przechodząc do niego bezpośrednio, na przykład http://mywebsite/signalr/hubs.

    ![Wygenerowany Centrum skryptów](tutorial-getting-started-with-signalr-and-mvc-4/_static/image13.png)

<a id="code"></a>

## <a name="examine-the-code"></a>Poszukaj w kodzie

Aplikacji rozmów SignalR pokazuje dwa podstawowe zadania rozwoju SignalR: tworzenie koncentratora jako obiekt główny koordynacji na serwerze i za pomocą biblioteki jQuery SignalR do wysyłania i odbierania komunikatów.

### <a name="signalr-hubs"></a>Koncentratory SignalR

W przykładowym kodzie **ChatHub** klasa pochodzi od **Microsoft.AspNet.SignalR.Hub** klasy. Wyprowadzanie z **Centrum** klasy jest to wygodny sposób, aby skompilować aplikację SignalR. Można utworzyć metody publiczne na klasy koncentratora, a następnie uzyskać dostęp do tych metod, wywołując je z jQuery skrypty na stronie sieci web.

W kodzie Rozmowa klienci wywołują **ChatHub.Send** metodę, aby wysyłać nowy komunikat. Centrum z kolei wysyła wiadomość do wszystkich klientów, wywołując **Clients.All.addNewMessageToPage**.

**Wysyłania** metoda pokazuje kilka koncepcji w Centrum:

- Tak, aby je wywoływać klientów do deklarowania metod publicznych w koncentratorze.
- Użyj **Microsoft.AspNet.SignalR.Hub.Clients** właściwość dostęp do wszystkich klientów podłączone do tego koncentratora.
- Wywołaj funkcję jQuery na kliencie (takich jak `addNewMessageToPage` funkcji) aby zaktualizować klientów.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a>SignalR i jQuery

**Chat.cshtml** plik widoku w przykładzie kodu pokazano, jak użyć biblioteki jQuery SignalR do komunikowania się z Centrum SignalR. Podstawowe zadania w kodzie do utworzenia odwołania do automatycznego generowania serwera proxy koncentratora, deklarowania funkcji, która serwera można wywołać w celu wypychania zawartości do klientów i od połączenia do wysyłania komunikatów do Centrum.

Poniższy kod deklaruje serwera proxy koncentratora.

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample6.js)]

> [!NOTE]
> Odwołanie do klasy serwera i jej elementów członkowskich w jQuery znajduje się w camelcase. Przykładowy kod odwołuje się do języka C# **ChatHub** klasy w jQuery jako **chatHub**. Jeśli chcesz, aby odwołać się do `ChatHub` klasy jQuery za pomocą konwencjonalnych Pascal wielkość liter jak w języku C#, Edytuj plik klasy ChatHub.cs. Dodaj `using` instrukcję, aby odwoływać się do `Microsoft.AspNet.SignalR.Hubs` przestrzeni nazw. Następnie dodaj `HubName` atrybutu `ChatHub` klasy, na przykład `[HubName("ChatHub")]`. Na koniec zaktualizuj użytkownikowi jQuery do `ChatHub` klasy.


Poniższy kod przedstawia sposób tworzenia funkcji wywołania zwrotnego w skrypcie. Klasy koncentratora na serwerze wywołuje tę funkcję, aby wypchnąć aktualizacji zawartości dla każdego klienta. Opcjonalne wywołanie `htmlEncode` funkcja przedstawiono sposób HTML zakodować zawartość komunikatu przed wyświetleniem go na stronie, aby uniemożliwić uruchomienie skryptu.

[!code-html[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample7.html)]

Poniższy kod przedstawia sposób nawiązać połączenie z koncentratorem. Kod uruchamia połączenie i przekazuje go po funkcji do obsługi zdarzenia click na **wysyłania** przycisk na stronie rozmowy.

> [!NOTE]
> Takie podejście zapewnia, że połączenie zostało nawiązane przed wykonaniem programu obsługi zdarzeń.


[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a>Następne kroki

Wiesz, że SignalR to architektura służąca do tworzenia aplikacji sieci web w czasie rzeczywistym. Pokazaliśmy również kilka zadań programistycznych SignalR: jak dodać SignalR do aplikacji ASP.NET, sposób tworzenia klasy koncentratora oraz jak wysyłać i odbierać komunikaty z Centrum.

Aby dowiedzieć się bardziej zaawansowanych pojęciach zmiany SignalR, odwiedź następującą witrynę dla SignalR kod źródłowy i zasobów:

- [Projekt SignalR](http://signalr.net)
- [SignalR Github i przykłady](https://github.com/SignalR/SignalR)
- [Witryny typu Wiki biblioteki SignalR](https://github.com/SignalR/SignalR/wiki)
