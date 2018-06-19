---
uid: signalr/overview/older-versions/tutorial-getting-started-with-signalr-and-mvc-4
title: 'Samouczek: Wprowadzenie do korzystania z SignalR 1.x a MVC 4 | Dokumentacja firmy Microsoft'
author: pfletcher
description: Do tworzenia aplikacji rozmów w czasie rzeczywistym, należy użyć SignalR platformy ASP.NET i ASP.NET MVC 4.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/29/2013
ms.topic: article
ms.assetid: eeef9f73-6de3-49f9-b50b-9af22108f2ce
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/tutorial-getting-started-with-signalr-and-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: 1ae330be5caf00c3cac7451f326398c0958538af
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30873722"
---
<a name="tutorial-getting-started-with-signalr-1x-and-mvc-4"></a>Samouczek: Wprowadzenie do korzystania z SignalR 1.x a MVC 4
====================
przez [Patrick Fletcher](https://github.com/pfletcher), [Teebken Timowi](https://github.com/timlt)

> Ten samouczek pokazuje, jak używać ASP.NET SignalR do tworzenia aplikacji rozmów w czasie rzeczywistym. Spowoduje dodanie SignalR do aplikacji MVC 4 i utworzyć widok rozmów do wysyłania i wyświetlenie komunikatów.


## <a name="overview"></a>Omówienie

Ten samouczek stanowi wprowadzenie do programowania aplikacji sieci web w czasie rzeczywistym z biblioteki SignalR platformy ASP.NET i ASP.NET MVC 4. W samouczku ten sam kod aplikacji rozmów jako [SignalR Wprowadzenie — samouczek](tutorial-getting-started-with-signalr.md), ale pokazano, jak dodać go do aplikacji MVC 4, na podstawie szablonu Internet.

W tym temacie przedstawiono następujące zadania programowanie SignalR:

- Dodawanie biblioteki SignalR do aplikacji MVC 4.
- Tworzenie klasy koncentratora do dystrybuowania zawartości do klientów.
- Za pomocą biblioteki jQuery SignalR na stronie sieci web można wysyłać wiadomości i wyświetlać aktualizacje z koncentratora.

Poniższy zrzut ekranu przedstawia aplikacji rozmów ukończone działającego w przeglądarce.

![Rozmowa wystąpień](tutorial-getting-started-with-signalr-and-mvc-4/_static/image2.png)

Sekcje:

- [Konfigurowanie projektu](#setup)
- [Uruchom próbki](#run)
- [Sprawdź kod](#code)
- [Następne kroki](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a>Konfigurowanie projektu

Wymagania wstępne:

- Visual Studio 2010 z dodatkiem SP1, program Visual Studio 2012 lub Visual Studio 2012 Express. Jeśli nie masz programu Visual Studio, zobacz [pobiera ASP.NET](https://www.asp.net/downloads) uzyskać bezpłatne narzędzie Visual Studio 2012 Express programowanie.
- Dla programu Visual Studio 2010, należy zainstalować [ASP.NET MVC 4](https://www.microsoft.com/download/details.aspx?id=30683).

W tej sekcji przedstawiono sposób tworzenia aplikacji ASP.NET MVC 4, Dodaj bibliotekę SignalR i tworzenie aplikacji czatu.

1. 1. W programie Visual Studio tworzenie aplikacji ASP.NET MVC 4, nadaj jej nazwę SignalRChat, a następnie kliknij przycisk OK.

        > [!NOTE]
        > VS 2010 wybierz **.NET Framework 4** za pomocą kontrolki rozwijanej wersji Framework. SignalR kod działa w wersji systemu .NET Framework 4 i 4.5.

        ![Tworzenie sieci web mvc](tutorial-getting-started-with-signalr-and-mvc-4/_static/image3.png)
      2. Wybierz szablon aplikacji internetowych, usuń zaznaczenie opcji do **Utwórz jednostkowy projekt testowy**i kliknij przycisk OK.

         ![Tworzenie witryny internetowej mvc](tutorial-getting-started-with-signalr-and-mvc-4/_static/image4.png)
      3. Otwórz **narzędzia | Menedżer pakietów biblioteki | Konsola Menedżera pakietów** i uruchom następujące polecenie. Ten krok powoduje dodanie do projektu zestawu plików skryptów i włączyć funkcję SignalR odwołań do zestawu.

         `install-package Microsoft.AspNet.SignalR -Version 1.1.3`
      4. W **Eksploratora rozwiązań** rozwiń folder skryptów. Należy pamiętać, że skrypt biblioteki SignalR zostały dodane do projektu.

         ![Odwołania do biblioteki](tutorial-getting-started-with-signalr-and-mvc-4/_static/image6.png)
      5. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt, wybierz **Dodaj | Nowy Folder**, i Dodaj nowy folder o nazwie **koncentratory**.
      6. Kliknij prawym przyciskiem myszy **koncentratory** folderu, kliknij przycisk **Dodaj | Klasa**i Utwórz nową klasę C# o nazwie **ChatHub.cs**. Ta klasa będzie używany jako SignalR koncentratora serwera, który wysyła komunikaty do wszystkich klientów.

> [!NOTE]
> Jeśli używasz programu Visual Studio 2012 i zainstalowano [ASP.NET i 2012.2 narzędzia sieci Web aktualizacji](../../../visual-studio/overview/2012/aspnet-and-web-tools-20122-release-notes-rtw.md#_Installation), nowy szablon elementu SignalR umożliwia tworzenie klasy koncentratora. W tym celu kliknij prawym przyciskiem myszy **koncentratory** folderu, kliknij przycisk **Dodaj | Nowy element**, wybierz pozycję **klasy koncentratora SignalR (wersja 1)**, a nazwa klasy **ChatHub.cs**.


1. Zastąp kod w **ChatHub** klasy następującym kodem.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample1.cs)]
2. Otwórz **Global.asax** pliku projektu, a następnie dodaj wywołanie do metody `RouteTable.Routes.MapHubs();` w pierwszym wierszu kodu w `Application_Start` metody. Ten kod rejestruje domyślną trasę dla koncentratorów SignalR i musi zostać wywołana przed zarejestrowaniem innych tras. Wypełniony `Application_Start` metody wygląda jak w następującym przykładzie.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample2.cs)]
3. Edytuj `HomeController` znaleziono klasy w **Controllers/HomeController.cs** i dodaj następującą metodę do klasy. Ta metoda zwraca **rozmowę** widoku, który zostanie utworzony w kolejnym kroku.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample3.cs)]
4. Kliknij prawym przyciskiem myszy w obrębie `Chat` metody możesz po prostu utworzyć i kliknij **Dodaj widok** do utworzenia nowego pliku widoku.
5. W **Dodaj widok** okna dialogowego, upewnij się, że jest zaznaczone pole wyboru do **Użyj układ strony wzorcowej** (Usuń zaznaczenie pola wyboru), a następnie kliknij przycisk **Dodaj**.

    ![Dodawanie widoku](tutorial-getting-started-with-signalr-and-mvc-4/_static/image8.png)
6. Edytuj plik widok o nazwie **Chat.cshtml**. Po &lt;h2&gt; tagów, wklej następujący &lt;div&gt; sekcji i `@section scripts` blok kodu do strony. Ten skrypt umożliwia strony do wysyłania wiadomości rozmów i wyświetlenie komunikatów z serwera. Kompletny kod dla widoku rozmów pojawia się w następującym fragmencie kodu.

    > [!IMPORTANT]
    > Po dodaniu SignalR i innych bibliotek skryptu do projektu programu Visual Studio Package Manager może zainstalować wersje skryptów, które są nowsze niż wersje przedstawiono w tym temacie. Upewnij się, że odwołań do skryptów w kodzie zgodne z wersjami bibliotek skryptu zainstalowanych w projekcie.

    [!code-cshtml[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample4.cshtml)]
7. **Zapisz wszystkie** dla projektu.

<a id="run"></a>

## <a name="run-the-sample"></a>Uruchom próbki

1. Naciśnij klawisz F5, aby uruchomić projekt w trybie debugowania.
2. W wierszu adresu przeglądarki, dołącz **/home/rozmów** adres URL domyślnej strony dla projektu. Ładuje strony rozmów w wystąpieniu przeglądarki i wyświetla monit o nazwę użytkownika.

    ![Wprowadź nazwę użytkownika](tutorial-getting-started-with-signalr-and-mvc-4/_static/image9.png)
3. Wprowadź nazwę użytkownika.
4. Skopiuj adres URL w wierszu adresu przeglądarki i używać go otworzyć dwa kolejne wystąpienia przeglądarki. W każdym wystąpieniu przeglądarki wprowadź unikatową nazwę użytkownika.
5. W każdym wystąpieniu przeglądarki dodać komentarz, a następnie kliknij przycisk **wysyłania**. Komentarze powinien być wyświetlany we wszystkich wystąpieniach przeglądarki.

    > [!NOTE]
    > Tej aplikacji rozmów proste kontekstu dyskusji na serwerze nie są zachowywane. Koncentrator emituje komentarze do wszystkich bieżących użytkowników. Użytkownicy, którzy później dołączyć rozmowę zobaczą wiadomości dodane od czasu dołączenia.
6. Poniższy zrzut ekranu przedstawia aplikacji czatu działającego w przeglądarce.

    ![Rozmowa przeglądarki](tutorial-getting-started-with-signalr-and-mvc-4/_static/image11.png)
7. W **Eksploratora rozwiązań**, sprawdź **dokumentów skryptu** węzła dla działającej aplikacji. Ten węzeł jest widoczny w trybie debugowania, jeśli używasz programu Internet Explorer jako przeglądarki. Brak pliku skryptu o nazwie **koncentratory** generujący biblioteki SignalR dynamicznie w czasie wykonywania. Ten plik zarządza komunikacji między skryptu jQuery i kod po stronie serwera. Jeśli używasz przeglądarki innej niż program Internet Explorer, można także przejść do dynamicznej **koncentratory** plików, przechodząc do niego bezpośrednio, na przykład http://mywebsite/signalr/hubs.

    ![Wygenerowany Centrum skryptów](tutorial-getting-started-with-signalr-and-mvc-4/_static/image13.png)

<a id="code"></a>

## <a name="examine-the-code"></a>Sprawdź kod

SignalR aplikacji czatu pokazano dwa podstawowe SignalR zadań związanych z projektowaniem: tworzenie koncentratora jako obiekt główny koordynacji na serwerze i używanie biblioteki jQuery SignalR do wysyłania i odbierania wiadomości.

### <a name="signalr-hubs"></a>Koncentratory SignalR

W przykładowym kodzie **ChatHub** pochodną klasy **Microsoft.AspNet.SignalR.Hub** klasy. Wyprowadzanie z **Centrum** klasy jest to wygodny sposób utworzyć aplikację SignalR. Można utworzyć metody publiczne na klasy koncentratora, a następnie te metody dostępu wywołując ze skryptów jQuery na stronie sieci web.

W kodzie rozmów wywołać klientów **ChatHub.Send** metody do wysłania nowej wiadomości. Koncentrator z kolei wysyła wiadomość do wszystkich klientów przez wywołanie metody **Clients.All.addNewMessageToPage**.

**Wysyłania** metody przedstawiono kilka pojęć Centrum:

- Metody publiczne należy zadeklarować w koncentratorze tak, aby klienci mogą połączeń telefonicznych z nimi.
- Użyj **Microsoft.AspNet.SignalR.Hub.Clients** właściwość, aby dostęp do wszystkich klientów podłączone do tego koncentratora.
- Wywoływanie funkcji jQuery na kliencie (takich jak `addNewMessageToPage` funkcji) aby zaktualizować klientów.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a>SignalR i jQuery

**Chat.cshtml** widoku plik przykładowy kod przedstawia sposób użycia biblioteki jQuery SignalR do komunikowania się przy użyciu koncentratora SignalR. Podstawowe zadania w kodzie utworzenie odwołania do automatycznie generowanej serwera proxy dla koncentratora, deklarowanie funkcję serwera można wywołać w celu wypychania zawartości do klientów, a następnie uruchomić połączenie do wysyłania komunikatów do koncentratora.

Poniższy kod deklaruje serwer proxy koncentratora.

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample6.js)]

> [!NOTE]
> Odwołanie do klasy serwera i jej elementów członkowskich w jQuery znajduje camelcase. Przykładowy kod odwołuje się do języka C# **ChatHub** klasy w jQuery jako **chatHub**. Jeśli chcesz odwołać `ChatHub` klasy jQuery z konwencjonalnej Pascal wielkość liter jak w języku C#, Edytuj plik klasy ChatHub.cs. Dodaj `using` instrukcji, aby odwołać `Microsoft.AspNet.SignalR.Hubs` przestrzeni nazw. Następnie dodaj `HubName` atrybutu `ChatHub` klasy, na przykład `[HubName("ChatHub")]`. Na koniec zaktualizuj jQuery odwołania do `ChatHub` klasy.


Poniższy kod przedstawia sposób tworzenia funkcję wywołania zwrotnego w skrypcie. Klasy koncentratora na serwerze wywołanie tej funkcji, aby wysyłać aktualizacje zawartości do każdego klienta. Opcjonalnie można wywołać `htmlEncode` funkcja pokazuje sposób HTML kodowanie zawartości komunikatu, przed wyświetleniem go na stronie, aby uniemożliwić uruchomienie skryptu.

[!code-html[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample7.html)]

Poniższy kod przedstawia sposób nawiązać połączenie z koncentratorem. Kod rozpoczyna połączenie, a następnie przekazuje on funkcję obsługi zdarzenia kliknij na **wysyłania** przycisku na stronie rozmów.

> [!NOTE]
> Takie podejście zapewnia, że połączenie zostanie nawiązane, przed wykonaniem programu obsługi zdarzeń.


[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a>Następne kroki

Wiesz, że SignalR to struktura służąca do tworzenia aplikacji sieci web w czasie rzeczywistym. Przedstawiono również kilka zadań związanych z projektowaniem SignalR: jak dodać do aplikacji ASP.NET SignalR, Tworzenie klasy koncentratora oraz sposobu wysyłania i odbierania wiadomości z koncentratora.

Aby uzyskać bardziej zaawansowane pojęcia rozwój SignalR, odwiedź następującą witrynę SignalR kod źródłowy i zasobów:

- [Projekt SignalR](http://signalr.net)
- [SignalR Github i przykłady](https://github.com/SignalR/SignalR)
- [Witryna typu Wiki biblioteki SignalR](https://github.com/SignalR/SignalR/wiki)
