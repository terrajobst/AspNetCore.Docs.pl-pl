---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
title: 'Samouczek: Wprowadzenie do SignalR 2 i MVC 5 | Dokumentacja firmy Microsoft'
author: pfletcher
description: W tym samouczku pokazano, jak używać signalr2 na platformie ASP.NET do tworzenia aplikacji rozmowy w czasie rzeczywistym. Będzie dodać SignalR do aplikacji MVC 5 i Utwórz widok rozmowy...
ms.author: riande
ms.date: 06/10/2014
ms.assetid: 80bfe5fb-bdfc-41fe-ac43-2132e5d69fac
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
msc.type: authoredcontent
ms.openlocfilehash: 568f82daa67f33736c2bf7a45a3e1339f265c487
ms.sourcegitcommit: 74e3be25ea37b5fc8b4b433b0b872547b4b99186
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53287523"
---
<a name="tutorial-getting-started-with-signalr-2-and-mvc-5"></a>Samouczek: Wprowadzenie do SignalR 2 i MVC 5
====================
przez [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

[Pobierz ukończony projekt](http://code.msdn.microsoft.com/Getting-Started-with-c366b2f3)

> W tym samouczku pokazano, jak używać signalr2 na platformie ASP.NET do tworzenia aplikacji rozmowy w czasie rzeczywistym. Będzie dodać SignalR do aplikacji MVC 5 i Utwórz widok czatu do wysyłania i wyświetla komunikaty.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używanego w tym samouczku
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - .NET 4.5
> - MVC 5
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

W tym samouczku przedstawiono tworzenie aplikacji sieci web w czasie rzeczywistym z signalr2 na platformie ASP.NET i ASP.NET MVC 5. W tym samouczku użyto tego samego kodu aplikacji rozmów jako [samouczka Wprowadzenie do SignalR](tutorial-getting-started-with-signalr.md), ale pokazuje, jak dodać go do aplikacji MVC 5.

W tym temacie dowiesz się, następujące zadania deweloperskie SignalR:

- Dodawanie biblioteki SignalR do aplikacji MVC 5.
- Utworzenie Centrum i klas uruchamiania OWIN do wypychania zawartości do klientów.
- Przy użyciu biblioteki jQuery SignalR na stronie sieci web umożliwiają przesyłanie komunikatów oraz wyświetlania aktualizacji z koncentratora.

Poniższy zrzut ekranu przedstawia zakończonych rozmów uruchomieniu aplikacji w przeglądarce.

![Wystąpienia rozmowy](tutorial-getting-started-with-signalr-and-mvc/_static/image1.png)

Sekcje:

- [Konfigurowanie projektu](#setup)
- [Uruchamianie aplikacji przykładowej](#run)
- [Poszukaj w kodzie](#code)
- [Następne kroki](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a>Konfigurowanie projektu

Wymagania wstępne:

- Visual Studio 2013. Jeśli nie masz programu Visual Studio, zobacz [ASP.NET pliki do pobrania](https://www.asp.net/downloads) można pobrać bezpłatny program Visual Studio 2013 Express narzędzia programistyczne.

W tej sekcji przedstawiono sposób tworzenia aplikacji ASP.NET MVC 5, dodawanie biblioteki SignalR i tworzenie aplikacji czatu.

1. W programie Visual Studio tworzenie aplikacji C# ASP.NET przeznaczonych .NET Framework 4.5, nadaj jej nazwę SignalRChat, a następnie kliknij przycisk OK.

    ![Tworzenie sieci web](tutorial-getting-started-with-signalr-and-mvc/_static/image2.png)
2. W `New ASP.NET Project` okna dialogowego, a następnie wybierz **MVC**i kliknij przycisk **Zmień uwierzytelnianie**.

    ![Tworzenie sieci web](tutorial-getting-started-with-signalr-and-mvc/_static/image3.png)
3. Wybierz **bez uwierzytelniania** w **Zmień uwierzytelnianie** okna dialogowego, a następnie kliknij przycisk **OK**.

    ![Wybierz pozycję bez uwierzytelniania](tutorial-getting-started-with-signalr-and-mvc/_static/image4.png)

    > [!NOTE]
    > Jeśli wybierzesz dostawcy uwierzytelniania inny dla aplikacji, `Startup.cs` klasy, które zostaną utworzone dla Ciebie; nie należy tworzyć własne `Startup.cs` klasy w kroku 10 poniżej.
4. Kliknij przycisk **OK** w **nowy projekt ASP.NET** okna dialogowego.
5. Otwórz **Narzędzia > Menedżer pakietów NuGet > Konsola Menedżera pakietów** i uruchom następujące polecenie. Ten krok powoduje dodanie do projektu, zbiór plików skryptów i odwołania do zestawów, które umożliwiają funkcji SignalR.

    `install-package Microsoft.AspNet.SignalR`
6. W **Eksploratora rozwiązań**, rozwiń folder skryptów. Należy pamiętać, że biblioteki skryptów dla elementu SignalR zostały dodane do projektu.

    ![Folder skryptów](tutorial-getting-started-with-signalr-and-mvc/_static/image5.png)
7. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt, wybierz **Dodaj | Nowy Folder**, i Dodaj nowy folder o nazwie **koncentratory**.
8. Kliknij prawym przyciskiem myszy **Hubs** folderu, kliknij przycisk **Dodaj | Nowy element**, wybierz opcję **Visual C# | W sieci Web | SignalR** w węźle **zainstalowane** okienku wybierz **klasa Centrum SignalR (v2)** w środkowym okienku i Utwórz nowe Centrum o nazwie **ChatHub.cs**. Ta klasa będzie używany jako koncentrator serwera SignalR, która wysyła komunikaty do wszystkich klientów.

    ![Utwórz nowe Centrum](tutorial-getting-started-with-signalr-and-mvc/_static/image6.png)
9. Zastąp kod w **ChatHub** klasy z następującym kodem.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample1.cs)]
10. Utwórz nową klasę o nazwie pliku Startup.cs. Zmień zawartość pliku do następujących.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample2.cs)]
11. Edytuj `HomeController` klasy znalezione w **Controllers/HomeController.cs** i dodaj następującą metodę do klasy. Ta metoda zwraca **Chat** widok, który spowoduje utworzenie w późniejszym kroku.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample3.cs)]
12. Kliknij prawym przyciskiem myszy **widoków domowych** folder, a następnie wybierz **Dodaj... | Widok**.
13. W **Dodaj widok** okno dialogowe, nazwę nowego widoku **Chat**.

    ![Dodawanie widoku](tutorial-getting-started-with-signalr-and-mvc/_static/image7.png)
14. Zastąp zawartość **Chat.cshtml** następującym kodem.

    > [!IMPORTANT]
    > Po dodaniu SignalR i inne biblioteki skryptu do projektu programu Visual Studio, Menedżera pakietów może zainstalować wersję pliku skryptu SignalR, która jest nowsza niż wersja przedstawione w tym temacie. Upewnij się, że odwołanie do skryptu w kodzie zgodny z wersją biblioteki skryptów zainstalowana w twoim projekcie.

    [!code-cshtml[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample4.cshtml)]
15. **Zapisz wszystko** dla projektu.

<a id="run"></a>

## <a name="run-the-sample"></a>Uruchamianie aplikacji przykładowej

1. Naciśnij klawisz F5, aby uruchomić projekt w trybie debugowania.
2. W wierszu adresu przeglądarki, dołącz **/home/rozmowy** adres URL domyślnej strony dla projektu. Rozmowa strona ładuje się w wystąpieniu przeglądarki i wyświetla monit o nazwę użytkownika.

    ![Wprowadź nazwę użytkownika](tutorial-getting-started-with-signalr-and-mvc/_static/image8.png)
3. Wprowadź nazwę użytkownika.
4. Skopiuj adres URL w wierszu adresu przeglądarki i użyj go, aby otworzyć dwóch większej liczby wystąpień przeglądarki. W każdym wystąpieniu przeglądarki wprowadź unikatową nazwę użytkownika.
5. W każdym wystąpieniu przeglądarki Dodaj komentarz, a następnie kliknij przycisk **wysyłania**. Komentarze powinien być wyświetlany we wszystkich wystąpieniach przeglądarki.

    > [!NOTE]
    > Tej aplikacji rozmów prostego nie utrzymuje kontekst dyskusji na serwerze. Centrum emituje komentarzy do wszystkich bieżących użytkowników. Użytkownicy, którzy dołączają do czatu później zostanie wyświetlony komunikat o dodane od czasu dołączenia.
6. Poniższy zrzut ekranu przedstawia aplikacji rozmów w przeglądarce.

    ![Rozmowa przeglądarki](tutorial-getting-started-with-signalr-and-mvc/_static/image9.png)
7. W **Eksploratora rozwiązań**, zbadaj **dokumenty skryptów** węzła dla działającej aplikacji. Ten węzeł jest widoczny w trybie debugowania, jeśli używasz programu Internet Explorer jako przeglądarki. Brak pliku skryptu o nazwie **koncentratory** generujący biblioteki SignalR dynamicznie w czasie wykonywania. Ten plik zarządza komunikacji między jQuery skryptu i kod po stronie serwera. Jeśli używasz przeglądarki innej niż Internet Explorer, można także przejść do dynamicznego **hubs** plików, przechodząc do niego bezpośrednio, na przykład http://mywebsite/signalr/hubs.

<a id="code"></a>

## <a name="examine-the-code"></a>Poszukaj w kodzie

Aplikacji rozmów SignalR pokazuje dwa podstawowe zadania rozwoju SignalR: tworzenie koncentratora jako obiekt główny koordynacji na serwerze i za pomocą biblioteki jQuery SignalR do wysyłania i odbierania komunikatów.

### <a name="signalr-hubs"></a>Koncentratory SignalR

W przykładowym kodzie **ChatHub** klasa pochodzi od **Microsoft.AspNet.SignalR.Hub** klasy. Wyprowadzanie z **Centrum** klasy jest to wygodny sposób, aby skompilować aplikację SignalR. Można utworzyć metody publiczne na klasy koncentratora, a następnie uzyskać dostęp do tych metod, wywołując je ze skryptów na stronie sieci web.

W kodzie Rozmowa klienci wywołują **ChatHub.Send** metodę, aby wysyłać nowy komunikat. Centrum z kolei wysyła wiadomość do wszystkich klientów, wywołując **Clients.All.addNewMessageToPage**.

**Wysyłania** metoda pokazuje kilka koncepcji w Centrum:

- Tak, aby je wywoływać klientów do deklarowania metod publicznych w koncentratorze.
- Użyj **Microsoft.AspNet.SignalR.Hub.Clients** właściwość dostęp do wszystkich klientów podłączone do tego koncentratora.
- Wywoływanie funkcji na komputerze klienckim (takich jak `addNewMessageToPage` funkcji) aby zaktualizować klientów.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a>SignalR i jQuery

**Chat.cshtml** plik widoku w przykładzie kodu pokazano, jak użyć biblioteki jQuery SignalR do komunikowania się z Centrum SignalR. Podstawowe zadania w kodzie do utworzenia odwołania do automatycznego generowania serwera proxy koncentratora, deklarowania funkcji, która serwera można wywołać w celu wypychania zawartości do klientów i od połączenia do wysyłania komunikatów do Centrum.

Poniższy kod deklaruje odwołanie do serwera proxy koncentratora.

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample6.js)]

> [!NOTE]
> Odwołanie do klasy serwera i jej elementów członkowskich w JavaScript znajduje się w camelcase. Przykładowy kod odwołuje się do języka C# **ChatHub** klasy w języku JavaScript jako **chatHub**. Jeśli chcesz, aby odwołać się do `ChatHub` klasy jQuery za pomocą konwencjonalnych Pascal wielkość liter jak w języku C#, Edytuj plik klasy ChatHub.cs. Dodaj `using` instrukcję, aby odwoływać się do `Microsoft.AspNet.SignalR.Hubs` przestrzeni nazw. Następnie dodaj `HubName` atrybutu `ChatHub` klasy, na przykład `[HubName("ChatHub")]`. Na koniec zaktualizuj użytkownikowi jQuery do `ChatHub` klasy.


Poniższy kod przedstawia sposób tworzenia funkcji wywołania zwrotnego w skrypcie. Klasy koncentratora na serwerze wywołuje tę funkcję, aby wypchnąć aktualizacji zawartości dla każdego klienta. Opcjonalne wywołanie `htmlEncode` funkcja przedstawiono sposób HTML zakodować zawartość komunikatu przed wyświetleniem go na stronie, aby uniemożliwić uruchomienie skryptu.

[!code-html[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample7.html)]

Poniższy kod przedstawia sposób nawiązać połączenie z koncentratorem. Kod uruchamia połączenie i przekazuje go po funkcji do obsługi zdarzenia click na **wysyłania** przycisk na stronie rozmowy.

> [!NOTE]
> Takie podejście zapewnia, że połączenie zostało nawiązane przed wykonaniem programu obsługi zdarzeń.


[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a>Następne kroki

Wiesz, że SignalR to architektura służąca do tworzenia aplikacji sieci web w czasie rzeczywistym. Pokazaliśmy również kilka zadań programistycznych SignalR: jak dodać SignalR do aplikacji ASP.NET, sposób tworzenia klasy koncentratora oraz jak wysyłać i odbierać komunikaty z Centrum.

Aby uzyskać wskazówki dotyczące sposobu wdrażania przykładowej aplikacji SignalR na platformie Azure, zobacz [przy użyciu SignalR z usługą Web Apps w usłudze Azure App Service](../deployment/using-signalr-with-azure-web-sites.md). Aby uzyskać szczegółowe informacje o sposobie wdrażania projektu sieci web programu Visual Studio z witryny sieci Web do Windows Azure, zobacz [tworzenie aplikacji sieci web platformy ASP.NET w usłudze Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).

Aby dowiedzieć się bardziej zaawansowanych pojęciach zmiany SignalR, odwiedź następującą witrynę dla SignalR kod źródłowy i zasobów:

- [Projekt SignalR](http://signalr.net)
- [SignalR Github i przykłady](https://github.com/SignalR/SignalR)
- [Witryny typu Wiki biblioteki SignalR](https://github.com/SignalR/SignalR/wiki)
