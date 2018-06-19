---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
title: 'Samouczek: Wprowadzenie do korzystania z SignalR 2 i MVC 5 | Dokumentacja firmy Microsoft'
author: pfletcher
description: Ten samouczek pokazuje, jak używać ASP.NET SignalR 2 do tworzenia aplikacji rozmów w czasie rzeczywistym. Spowoduje dodanie SignalR do aplikacji MVC 5 i Utwórz widok rozmów...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 80bfe5fb-bdfc-41fe-ac43-2132e5d69fac
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
msc.type: authoredcontent
ms.openlocfilehash: ca0471114da7363c5df9d459308708e7ab4f8b84
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
ms.locfileid: "28033850"
---
<a name="tutorial-getting-started-with-signalr-2-and-mvc-5"></a>Samouczek: Wprowadzenie do korzystania z SignalR 2 i MVC 5
====================
przez [Patrick Fletcher](https://github.com/pfletcher), [Teebken Timowi](https://github.com/timlt)

[Pobieranie ukończone projektu](http://code.msdn.microsoft.com/Getting-Started-with-c366b2f3)

> Ten samouczek pokazuje, jak używać ASP.NET SignalR 2 do tworzenia aplikacji rozmów w czasie rzeczywistym. Spowoduje dodanie SignalR do aplikacji MVC 5 i utworzyć widok rozmów do wysyłania i wyświetlenie komunikatów. 
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Używane w samouczku wersje oprogramowania
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - MVC 5
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

Ten samouczek stanowi wprowadzenie do programowania aplikacji sieci web w czasie rzeczywistym z 2 SignalR platformy ASP.NET i ASP.NET MVC 5. W samouczku ten sam kod aplikacji rozmów jako [SignalR Wprowadzenie — samouczek](tutorial-getting-started-with-signalr.md), ale pokazano, jak dodać go do aplikacji MVC 5.

W tym temacie przedstawiono następujące zadania programowanie SignalR:

- Dodawanie biblioteki SignalR do aplikacji MVC 5.
- Tworzenie klasy uruchamiania OWIN do dystrybuowania zawartości do klientów i koncentratora.
- Za pomocą biblioteki jQuery SignalR na stronie sieci web można wysyłać wiadomości i wyświetlać aktualizacje z koncentratora.

Poniższy zrzut ekranu przedstawia aplikacji rozmów ukończone działającego w przeglądarce.

![Rozmowa wystąpień](tutorial-getting-started-with-signalr-and-mvc/_static/image1.png)

Sekcje:

- [Konfigurowanie projektu](#setup)
- [Uruchom próbki](#run)
- [Sprawdź kod](#code)
- [Następne kroki](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a>Konfigurowanie projektu

Wymagania wstępne:

- Visual Studio 2013. Jeśli nie masz programu Visual Studio, zobacz [pobiera ASP.NET](https://www.asp.net/downloads) uzyskać bezpłatne narzędzie Visual Studio 2013 Express programowanie.

W tej sekcji przedstawiono sposób tworzenia aplikacji ASP.NET MVC 5, Dodaj bibliotekę SignalR i tworzenie aplikacji czatu.

1. W programie Visual Studio tworzenie aplikacji C# ASP.NET, którego element docelowy .NET Framework 4.5, nadaj jej nazwę SignalRChat i kliknij przycisk OK.

    ![Tworzenie sieci web](tutorial-getting-started-with-signalr-and-mvc/_static/image2.png)
2. W `New ASP.NET Project` okna dialogowego, a następnie wybierz **MVC**i kliknij przycisk **Zmień uwierzytelnianie**.

    ![Tworzenie sieci web](tutorial-getting-started-with-signalr-and-mvc/_static/image3.png)
3. Wybierz **bez uwierzytelniania** w **Zmień uwierzytelnianie** okna dialogowego, a następnie kliknij przycisk **OK**.

    ![Wybierz bez uwierzytelniania](tutorial-getting-started-with-signalr-and-mvc/_static/image4.png)

    > [!NOTE]
    > W przypadku wybrania dostawcy uwierzytelniania innej aplikacji, `Startup.cs` klasy zostaną utworzone automatycznie; nie należy tworzyć własne `Startup.cs` klasy w kroku 10 poniżej.
4. Kliknij przycisk **OK** w **nowy projekt ASP.NET** okna dialogowego.
5. Otwórz **narzędzia | Menedżer pakietów biblioteki | Konsola Menedżera pakietów** i uruchom następujące polecenie. Ten krok powoduje dodanie do projektu zestawu plików skryptów i włączyć funkcję SignalR odwołań do zestawu.

    `install-package Microsoft.AspNet.SignalR`
6. W **Eksploratora rozwiązań**, rozwiń folder skryptów. Należy pamiętać, że skrypt biblioteki SignalR zostały dodane do projektu.

    ![Folder skryptów](tutorial-getting-started-with-signalr-and-mvc/_static/image5.png)
7. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt, wybierz **Dodaj | Nowy Folder**, i Dodaj nowy folder o nazwie **koncentratory**.
8. Kliknij prawym przyciskiem myszy **koncentratory** folderu, kliknij przycisk **Dodaj | Nowy element**, wybierz pozycję **Visual C# | Sieci Web | SignalR** w węźle **zainstalowana** okienku wybierz **klasy koncentratora SignalR (v2)** w środkowym okienku i Utwórz nowe Centrum o nazwie **ChatHub.cs**. Ta klasa będzie używany jako SignalR koncentratora serwera, który wysyła komunikaty do wszystkich klientów.

    ![Utwórz nowe Centrum](tutorial-getting-started-with-signalr-and-mvc/_static/image6.png)
9. Zastąp kod w **ChatHub** klasy następującym kodem.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample1.cs)]
10. Utwórz nową klasę o nazwie pliku Startup.cs. Zmienić zawartość pliku do następującego.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample2.cs)]
11. Edytuj `HomeController` znaleziono klasy w **Controllers/HomeController.cs** i dodaj następującą metodę do klasy. Ta metoda zwraca **rozmowę** widoku, który zostanie utworzony w kolejnym kroku.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample3.cs)]
12. Kliknij prawym przyciskiem myszy **widoków domowych** folder, a następnie wybierz **Dodaj... | Widok**.
13. W **Dodaj widok** okna dialogowego, nazwę nowego widoku **rozmowę**.

    ![Dodawanie widoku](tutorial-getting-started-with-signalr-and-mvc/_static/image7.png)
14. Zastąp zawartość **Chat.cshtml** następującym kodem.

    > [!IMPORTANT]
    > Po dodaniu SignalR i innych bibliotek skryptu do projektu programu Visual Studio Menedżera pakietów mogą zainstalować wersję pliku skryptu SignalR, która jest nowsza niż wersja wyświetlane w tym temacie. Upewnij się, że odwołanie do skryptu w kodzie zgodna z wersją biblioteki skryptu zainstalowane w projekcie.

    [!code-cshtml[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample4.cshtml)]
15. **Zapisz wszystkie** dla projektu.

<a id="run"></a>

## <a name="run-the-sample"></a>Uruchom próbki

1. Naciśnij klawisz F5, aby uruchomić projekt w trybie debugowania.
2. W wierszu adresu przeglądarki, dołącz **/home/rozmów** adres URL domyślnej strony dla projektu. Ładuje strony rozmów w wystąpieniu przeglądarki i wyświetla monit o nazwę użytkownika.

    ![Wprowadź nazwę użytkownika](tutorial-getting-started-with-signalr-and-mvc/_static/image8.png)
3. Wprowadź nazwę użytkownika.
4. Skopiuj adres URL w wierszu adresu przeglądarki i używać go otworzyć dwa kolejne wystąpienia przeglądarki. W każdym wystąpieniu przeglądarki wprowadź unikatową nazwę użytkownika.
5. W każdym wystąpieniu przeglądarki dodać komentarz, a następnie kliknij przycisk **wysyłania**. Komentarze powinien być wyświetlany we wszystkich wystąpieniach przeglądarki.

    > [!NOTE]
    > Tej aplikacji rozmów proste kontekstu dyskusji na serwerze nie są zachowywane. Koncentrator emituje komentarze do wszystkich bieżących użytkowników. Użytkownicy, którzy później dołączyć rozmowę zobaczą wiadomości dodane od czasu dołączenia.
6. Poniższy zrzut ekranu przedstawia aplikacji czatu działającego w przeglądarce.

    ![Rozmowa przeglądarki](tutorial-getting-started-with-signalr-and-mvc/_static/image9.png)
7. W **Eksploratora rozwiązań**, sprawdź **dokumentów skryptu** węzła dla działającej aplikacji. Ten węzeł jest widoczny w trybie debugowania, jeśli używasz programu Internet Explorer jako przeglądarki. Brak pliku skryptu o nazwie **koncentratory** generujący biblioteki SignalR dynamicznie w czasie wykonywania. Ten plik zarządza komunikacji między skryptu jQuery i kod po stronie serwera. Jeśli używasz przeglądarki innej niż program Internet Explorer, można także przejść do dynamicznej **koncentratory** plików, przechodząc do niego bezpośrednio, na przykład http://mywebsite/signalr/hubs.

<a id="code"></a>

## <a name="examine-the-code"></a>Sprawdź kod

SignalR aplikacji czatu pokazano dwa podstawowe SignalR zadań związanych z projektowaniem: tworzenie koncentratora jako obiekt główny koordynacji na serwerze i używanie biblioteki jQuery SignalR do wysyłania i odbierania wiadomości.

### <a name="signalr-hubs"></a>Koncentratory SignalR

W przykładowym kodzie **ChatHub** pochodną klasy **Microsoft.AspNet.SignalR.Hub** klasy. Wyprowadzanie z **Centrum** klasy jest to wygodny sposób utworzyć aplikację SignalR. Można utworzyć metody publiczne na klasy koncentratora, a następnie uzyskać dostępu do tych metod, wywołując ze skryptów na stronie sieci web.

W kodzie rozmów wywołać klientów **ChatHub.Send** metody do wysłania nowej wiadomości. Koncentrator z kolei wysyła wiadomość do wszystkich klientów przez wywołanie metody **Clients.All.addNewMessageToPage**.

**Wysyłania** metody przedstawiono kilka pojęć Centrum:

- Metody publiczne należy zadeklarować w koncentratorze tak, aby klienci mogą połączeń telefonicznych z nimi.
- Użyj **Microsoft.AspNet.SignalR.Hub.Clients** właściwość, aby dostęp do wszystkich klientów podłączone do tego koncentratora.
- Wywoływanie funkcji na kliencie (takich jak `addNewMessageToPage` funkcji) aby zaktualizować klientów.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a>SignalR i jQuery

**Chat.cshtml** widoku plik przykładowy kod przedstawia sposób użycia biblioteki jQuery SignalR do komunikowania się przy użyciu koncentratora SignalR. Podstawowe zadania w kodzie utworzenie odwołania do automatycznie generowanej serwera proxy dla koncentratora, deklarowanie funkcję serwera można wywołać w celu wypychania zawartości do klientów, a następnie uruchomić połączenie do wysyłania komunikatów do koncentratora.

Poniższy kod deklaruje odwołanie do serwera proxy koncentratora.

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample6.js)]

> [!NOTE]
> W języku JavaScript odwołanie do klasy serwera i jej elementów członkowskich jest w camelcase. Przykładowy kod odwołuje się do języka C# **ChatHub** klasy w języku JavaScript jako **chatHub**. Jeśli chcesz odwołać `ChatHub` klasy jQuery z konwencjonalnej Pascal wielkość liter jak w języku C#, Edytuj plik klasy ChatHub.cs. Dodaj `using` instrukcji, aby odwołać `Microsoft.AspNet.SignalR.Hubs` przestrzeni nazw. Następnie dodaj `HubName` atrybutu `ChatHub` klasy, na przykład `[HubName("ChatHub")]`. Na koniec zaktualizuj jQuery odwołania do `ChatHub` klasy.


Poniższy kod przedstawia sposób tworzenia funkcję wywołania zwrotnego w skrypcie. Klasy koncentratora na serwerze wywołanie tej funkcji, aby wysyłać aktualizacje zawartości do każdego klienta. Opcjonalnie można wywołać `htmlEncode` funkcja pokazuje sposób HTML kodowanie zawartości komunikatu, przed wyświetleniem go na stronie, aby uniemożliwić uruchomienie skryptu.

[!code-html[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample7.html)]

Poniższy kod przedstawia sposób nawiązać połączenie z koncentratorem. Kod rozpoczyna połączenie, a następnie przekazuje on funkcję obsługi zdarzenia kliknij na **wysyłania** przycisku na stronie rozmów.

> [!NOTE]
> Takie podejście zapewnia, że połączenie zostanie nawiązane, przed wykonaniem programu obsługi zdarzeń.


[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a>Następne kroki

Wiesz, że SignalR to struktura służąca do tworzenia aplikacji sieci web w czasie rzeczywistym. Przedstawiono również kilka zadań związanych z projektowaniem SignalR: jak dodać do aplikacji ASP.NET SignalR, Tworzenie klasy koncentratora oraz sposobu wysyłania i odbierania wiadomości z koncentratora.

Aby uzyskać wskazówki dotyczące sposobu wdrażania przykładowej aplikacji SignalR na platformie Azure, zobacz [SignalR korzystanie z aplikacji sieci Web w usłudze Azure App Service](../deployment/using-signalr-with-azure-web-sites.md). Aby uzyskać szczegółowe informacje o sposobie wdrażania projektu sieci web programu Visual Studio do witryny sieci Web systemu Windows Azure, zobacz [tworzenie aplikacji sieci web platformy ASP.NET w usłudze Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).

Aby uzyskać bardziej zaawansowane pojęcia rozwój SignalR, odwiedź następującą witrynę SignalR kod źródłowy i zasobów:

- [Projekt SignalR](http://signalr.net)
- [SignalR Github i przykłady](https://github.com/SignalR/SignalR)
- [Witryna typu Wiki biblioteki SignalR](https://github.com/SignalR/SignalR/wiki)
