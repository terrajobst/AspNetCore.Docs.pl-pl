---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr
title: 'Samouczek: Wprowadzenie do SignalR 2 | Dokumentacja firmy Microsoft'
author: pfletcher
description: Ten samouczek pokazuje, jak utworzyć aplikację do obsługi rozmów w czasie rzeczywistym przy użyciu SignalR. Będzie dodać SignalR do pustych aplikacji sieci web ASP.NET i utworzyć pa HTML...
ms.author: aspnetcontent
ms.date: 06/10/2014
ms.assetid: a8b3b778-f009-4369-85c7-e90f9878d8b4
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 798838af099cceb12652b7c6c66633a03a73e538
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37841845"
---
<a name="tutorial-getting-started-with-signalr-2"></a>Samouczek: Wprowadzenie do SignalR 2
====================
przez [Patrick Fletcher](https://github.com/pfletcher)

[Pobieranie ukończone projektu](http://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9)

> Ten samouczek pokazuje, jak utworzyć aplikację do obsługi rozmów w czasie rzeczywistym przy użyciu SignalR. Będzie dodać SignalR do pustych aplikacji sieci web ASP.NET i Utwórz stronę HTML do wysyłania i wyświetla komunikaty. 
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używanego w tym samouczku
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
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

W tym samouczku przedstawiono rozwoju SignalR, pokazując, jak utworzyć aplikację proste rozmowy opartej na przeglądarce. Będzie dodać biblioteki SignalR do pustą aplikację sieci web platformy ASP.NET, Utwórz klasę hub wysyłanie komunikatów do klientów i Utwórz stronę HTML, który umożliwia użytkownikom wysyłanie i odbieranie wiadomości rozmowy. Z podobnego samouczka dotyczącego pokazującym, jak utworzyć aplikację do obsługi rozmów w MVC 5 przy użyciu widoku MVC, zobacz [rozpoczęcie korzystania z SignalR 2 i MVC 5](tutorial-getting-started-with-signalr-and-mvc.md).

> [!NOTE]
> Ten samouczek przedstawia sposób tworzenia aplikacji SignalR w wersji 2. Aby uzyskać szczegółowe informacje na temat zmian między SignalR 1.x i 2, zobacz [projektów uaktualnianie SignalR 1.x](../releases/upgrading-signalr-1x-projects-to-20.md) i [Visual Studio 2013 Release Notes](../../../visual-studio/overview/2013/release-notes.md#TOC13).

SignalR to biblioteki .NET typu open source do tworzenia aplikacji internetowych, które wymagają interakcji z użytkownikiem na żywo lub aktualizacji danych w czasie rzeczywistym. Przykłady obejmują społecznościowe aplikacje, gry wielodostępnym, biznesowych współpracę i w nowościach, pogody lub finansowych aktualizacji aplikacji. Są one często nazywane aplikacji w czasie rzeczywistym.

SignalR upraszcza proces tworzenia aplikacji w czasie rzeczywistym. Obejmuje ona bibliotekę serwera programu ASP.NET oraz bibliotekę kliencką JavaScript, aby ułatwić zarządzanie połączeniami klient serwer i wypychanie aktualizacji zawartości dla klientów. Biblioteki SignalR można dodać do istniejącej aplikacji ASP.NET do uzyskania funkcji w czasie rzeczywistym.

Samouczek przedstawia następujące zadania deweloperskie SignalR:

- Dodawanie biblioteki SignalR do aplikacji sieci web ASP.NET.
- Tworzenie klasy koncentratora, aby wypchnąć zawartości do klientów.
- Tworzenie klasy początkowej OWIN do konfigurowania aplikacji.
- Przy użyciu biblioteki jQuery SignalR na stronie sieci web umożliwiają przesyłanie komunikatów oraz wyświetlania aktualizacji z koncentratora.

Poniższy zrzut ekranu przedstawia aplikacji rozmów w przeglądarce. Każdy nowy użytkownik może publikować komentarze i zobacz komentarze dodane po użytkownik nie przyłączy czatu.

![Wystąpienia rozmowy](tutorial-getting-started-with-signalr/_static/image1.png)

Sekcje:

- [Konfigurowanie projektu](#setup)
- [Uruchamianie aplikacji przykładowej](#run)
- [Poszukaj w kodzie](#code)
- [Następne kroki](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a>Konfigurowanie projektu

W tej sekcji pokazano, jak utworzyć pustą aplikację sieci web platformy ASP.NET, za pomocą programu Visual Studio 2013 i SignalR w wersji 2 Dodaj SignalR i tworzenie aplikacji czatu.

Wymagania wstępne:

- Visual Studio 2013. Jeśli nie masz programu Visual Studio, zobacz [ASP.NET pliki do pobrania](https://www.asp.net/downloads) można pobrać bezpłatny program Visual Studio 2013 Express narzędzia programistyczne.

Poniższe kroki Użyj programu Visual Studio 2013, aby utworzyć pustą aplikację sieci Web platformy ASP.NET i dodać biblioteki SignalR:

1. W programie Visual Studio należy utworzyć aplikację sieci Web platformy ASP.NET.

    ![Tworzenie sieci web](tutorial-getting-started-with-signalr/_static/image2.png)
2. W **nowy projekt ASP.NET** okna, pozostaw **pusty** zaznaczone, a następnie kliknij przycisk **Tworzenie projektu**.

    ![Tworzenie pustego sieci web](tutorial-getting-started-with-signalr/_static/image3.png)
3. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt, wybierz **Dodaj | Klasa Centrum SignalR (v2)**. Nazwa klasy **ChatHub.cs** i dodaj go do projektu. Spowoduje to utworzenie **ChatHub** klasy i dodaje do projektu zestawu plików skryptów i odwołania do zestawów, obsługujące bibliotekę SignalR.

    > [!NOTE]
    > Biblioteki SignalR można dodać do projektu, otwierając **narzędzia | Menedżer pakietów biblioteki | Konsola Menedżera pakietów** i uruchamiając polecenie:

    `install-package Microsoft.AspNet.SignalR`

    Jeśli używasz konsoli można dodać SignalR, należy utworzyć klasa Centrum SignalR w osobnym kroku po dodaniu SignalR.

    > [!NOTE]
    > Jeśli używasz programu Visual Studio 2012, **klasa Centrum SignalR (v2)** szablonu nie będą dostępne. Możesz dodać zwykły **klasy** o nazwie `ChatHub` zamiast tego.
4. W **Eksploratora rozwiązań**, rozwiń węzeł skryptów. Biblioteki skryptów, jQuery i SignalR są widoczne w projekcie.
5. Zastąp kod w nowej **ChatHub** klasy z następującym kodem.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample1.cs)]
6. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt, a następnie kliknij przycisk **Dodaj | Klasa początkowa OWIN**. Nadaj nowej klasie `Startup` i kliknij przycisk OK.

    > [!NOTE]
    > Jeśli używasz programu Visual Studio 2012, **klasy początkowej OWIN** szablonu nie będą dostępne. Możesz dodać zwykły **klasy** o nazwie `Startup` zamiast tego.
7. Zmień zawartość elementu nową klasę uruchamiania do następujących.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample2.cs)]
8. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt, a następnie kliknij przycisk **Dodaj | Strona HTML**. Nadaj nowej stronie `index.html`.
    >[!NOTE]
    >Czasami trzeba zmienić numery wersji dla odwołań do biblioteki JQuery i SignalR
9. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy właśnie utworzony strony HTML i kliknij przycisk **Ustaw jako strona startowa**.
10. Zastąp kod domyślnej strony HTML z następującym kodem.

    > [!NOTE]
    > Może być zainstalowana nowsza wersja skryptów SignalR przez Menedżera pakietów. Sprawdź, czy odwołania do skryptu poniżej odpowiadają wersji plików skrypt w projekcie (będą różne, jeśli dodano SignalR za pomocą narzędzia NuGet, zamiast dodawać koncentrator.)

    [!code-html[Main](tutorial-getting-started-with-signalr/samples/sample3.html)]
11. **Zapisz wszystko** dla projektu.

<a id="run"></a>

## <a name="run-the-sample"></a>Uruchamianie aplikacji przykładowej

1. Naciśnij klawisz F5, aby uruchomić projekt w trybie debugowania. Strona HTML ładuje się w wystąpieniu przeglądarki i wyświetla monit o nazwę użytkownika.

    ![Wprowadź nazwę użytkownika](tutorial-getting-started-with-signalr/_static/image4.png)
2. Wprowadź nazwę użytkownika.
3. Skopiuj adres URL w wierszu adresu przeglądarki i użyj go, aby otworzyć dwóch większej liczby wystąpień przeglądarki. W każdym wystąpieniu przeglądarki wprowadź unikatową nazwę użytkownika.
4. W każdym wystąpieniu przeglądarki Dodaj komentarz, a następnie kliknij przycisk **wysyłania**. Komentarze powinien być wyświetlany we wszystkich wystąpieniach przeglądarki.

    > [!NOTE]
    > Tej aplikacji rozmów prostego nie utrzymuje kontekst dyskusji na serwerze. Centrum emituje komentarzy do wszystkich bieżących użytkowników. Użytkownicy, którzy dołączają do czatu później zostanie wyświetlony komunikat o dodane od czasu dołączenia.

    Poniższy zrzut ekranu przedstawia aplikacji rozmów w trzech wystąpień przeglądarki, z których wszystkie są aktualizowane w jedno wystąpienie jest wysyłana wiadomość:

    ![Rozmowa przeglądarki](tutorial-getting-started-with-signalr/_static/image5.png)
5. W **Eksploratora rozwiązań**, zbadaj **dokumenty skryptów** węzła dla działającej aplikacji. Brak pliku skryptu o nazwie **koncentratory** generujący biblioteki SignalR dynamicznie w czasie wykonywania. Ten plik zarządza komunikacji między jQuery skryptu i kod po stronie serwera.

    ![](tutorial-getting-started-with-signalr/_static/image6.png)

<a id="code"></a>

## <a name="examine-the-code"></a>Poszukaj w kodzie

Aplikacji rozmów SignalR pokazuje dwa podstawowe zadania rozwoju SignalR: tworzenie koncentratora jako obiekt główny koordynacji na serwerze i za pomocą biblioteki jQuery SignalR do wysyłania i odbierania komunikatów.

### <a name="signalr-hubs"></a>Koncentratory SignalR

W przykładowym kodzie **ChatHub** klasa pochodzi od **Microsoft.AspNet.SignalR.Hub** klasy. Wyprowadzanie z **Centrum** klasy jest to wygodny sposób, aby skompilować aplikację SignalR. Można utworzyć metody publiczne na klasy koncentratora, a następnie uzyskać dostęp do tych metod, wywołując je ze skryptów na stronie sieci web.

W kodzie Rozmowa klienci wywołują **ChatHub.Send** metodę, aby wysyłać nowy komunikat. Centrum z kolei wysyła wiadomość do wszystkich klientów, wywołując **Clients.All.broadcastMessage**.

**Wysyłania** metoda pokazuje kilka koncepcji w Centrum:

- Tak, aby je wywoływać klientów do deklarowania metod publicznych w koncentratorze.
- Użyj **Microsoft.AspNet.SignalR.Hub.Clients** właściwość dynamiczna ma dostęp do wszystkich klientów podłączone do tego koncentratora.
- Wywoływanie funkcji na komputerze klienckim (takich jak `broadcastMessage` funkcji) aby zaktualizować klientów.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample4.cs)]

### <a name="signalr-and-jquery"></a>SignalR i jQuery

Strony HTML w przykładzie kodu pokazano, jak użyć biblioteki jQuery SignalR do komunikowania się z Centrum SignalR. Serwer proxy, aby odwoływać się do Centrum deklarowania funkcji, która serwera można wywołać w celu wypychania zawartości do klientów i od połączenia, aby wysyłać komunikaty do Centrum deklaruje podstawowych zadań w kodzie.

Poniższy kod deklaruje odwołanie do serwera proxy koncentratora.

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample5.js)]

> [!NOTE]
> Odwołanie do klasy serwera i jej elementów członkowskich w JavaScript znajduje się w camelcase. Przykładowy kod odwołuje się do języka C# **ChatHub** klasy w języku JavaScript jako **chatHub**.


Poniższy kod jest na tym, jak utworzyć funkcję wywołania zwrotnego w skrypcie. Klasy koncentratora na serwerze wywołuje tę funkcję, aby wypchnąć aktualizacji zawartości dla każdego klienta. Dwa wiersze, czy kodowanie HTML zawartości przed wyświetleniem go są opcjonalne i Pokaż prostego sposobu zapobiegania uruchomienie skryptu.

[!code-html[Main](tutorial-getting-started-with-signalr/samples/sample6.html)]

Poniższy kod przedstawia sposób nawiązać połączenie z koncentratorem. Kod uruchamia połączenie i przekazuje go po funkcji do obsługi zdarzenia click na **wysyłania** przycisk na stronie HTML.

> [!NOTE]
> Takie podejście zapewnia, że połączenie zostało nawiązane przed wykonaniem programu obsługi zdarzeń.


[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample7.js)]

<a id="next"></a>

## <a name="next-steps"></a>Następne kroki

Wiesz, że SignalR to architektura służąca do tworzenia aplikacji sieci web w czasie rzeczywistym. Pokazaliśmy również kilka zadań programistycznych SignalR: jak dodać SignalR do aplikacji ASP.NET, sposób tworzenia klasy koncentratora oraz jak wysyłać i odbierać komunikaty z Centrum.

Aby uzyskać wskazówki dotyczące sposobu wdrażania przykładowej aplikacji SignalR na platformie Azure, zobacz [przy użyciu SignalR z usługą Web Apps w usłudze Azure App Service](../deployment/using-signalr-with-azure-web-sites.md). Aby uzyskać szczegółowe informacje o sposobie wdrażania projektu sieci web programu Visual Studio z witryny sieci Web do Windows Azure, zobacz [tworzenie aplikacji sieci web platformy ASP.NET w usłudze Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).

Aby dowiedzieć się bardziej zaawansowanych pojęciach zmiany SignalR, odwiedź następującą witrynę dla SignalR kod źródłowy i zasobów:

- [Projekt SignalR](http://signalr.net)
- [SignalR Github i przykłady](https://github.com/SignalR/SignalR)
- [Witryny typu Wiki biblioteki SignalR](https://github.com/SignalR/SignalR/wiki)
