---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr
title: 'Samouczek: Wprowadzenie do korzystania z SignalR 2 | Dokumentacja firmy Microsoft'
author: pfletcher
description: Ten samouczek pokazuje, jak utworzyć aplikację do obsługi rozmów w czasie rzeczywistym przy użyciu SignalR. Spowoduje dodanie SignalR do pustą aplikację sieci web ASP.NET i utworzyć pa HTML...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: a8b3b778-f009-4369-85c7-e90f9878d8b4
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 8be851f5a2b1cca39f5f8f284ff1c002c486d7e8
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
ms.locfileid: "28036807"
---
<a name="tutorial-getting-started-with-signalr-2"></a>Samouczek: Wprowadzenie do korzystania z SignalR 2
====================
przez [Patrick Fletcher](https://github.com/pfletcher)

[Pobieranie ukończone projektu](http://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9)

> Ten samouczek pokazuje, jak utworzyć aplikację do obsługi rozmów w czasie rzeczywistym przy użyciu SignalR. Spowoduje dodanie SignalR do pustą aplikację sieci web ASP.NET i Utwórz stronę HTML do wysyłania i wyświetlenie komunikatów. 
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Używane w samouczku wersje oprogramowania
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

W tym samouczku wprowadzenie programowanie SignalR przez przedstawiający sposób tworzenia aplikacji proste rozmów bazujące na przeglądarce. Zostanie dodać biblioteki SignalR do pusta aplikacja sieci web platformy ASP.NET, Utwórz klasę koncentratora do wysyłania wiadomości do klientów i utworzyć strona HTML, która umożliwia użytkownikom wysyłanie i odbieranie wiadomości. Podobne samouczek przedstawia sposób tworzenia aplikacji rozmów w MVC 5 widoku MVC, zobacz [wprowadzenie SignalR 2 i MVC 5](tutorial-getting-started-with-signalr-and-mvc.md).

> [!NOTE]
> Ten samouczek przedstawia sposób tworzenia aplikacji SignalR w wersji 2. Aby uzyskać szczegółowe informacje o zmianach między SignalR 1.x i 2, zobacz [SignalR Uaktualnianie projektów 1.x](../releases/upgrading-signalr-1x-projects-to-20.md) i [programu Visual Studio 2013 informacje o wersji](../../../visual-studio/overview/2013/release-notes.md#TOC13).

SignalR to open source biblioteki .NET do tworzenia aplikacji sieci web, które wymagają interakcji z użytkownikiem na żywo lub aktualizacji danych w czasie rzeczywistym. Przykładami społecznościowych aplikacji, wielodostępnym gry, business współpracy i grup dyskusyjnych, pogody lub finansowych aktualizacji aplikacji. Są one często nazywane aplikacji w czasie rzeczywistym.

SignalR upraszcza proces tworzenia aplikacji w czasie rzeczywistym. Zawiera bibliotekę serwera programu ASP.NET i biblioteki klienckiej JavaScript, aby ułatwić zarządzanie połączeniami klient serwer i wypychanie aktualizacji zawartości do klientów. Biblioteka SignalR można dodać do istniejącej aplikacji ASP.NET do uzyskania funkcjonalność w czasie rzeczywistym.

Samouczek przedstawia następujące SignalR zadań związanych z projektowaniem:

- Dodawanie biblioteki SignalR do aplikacji sieci web ASP.NET.
- Tworzenie klasy koncentratora do dystrybuowania zawartości do klientów.
- Tworzenie klasy początkowej OWIN do skonfigurowania aplikacji.
- Za pomocą biblioteki jQuery SignalR na stronie sieci web można wysyłać wiadomości i wyświetlać aktualizacje z koncentratora.

Poniższy zrzut ekranu przedstawia aplikacji czatu działającego w przeglądarce. Każdy nowy użytkownik można publikować komentarze i wyświetlić komentarze, dodawane po użytkownik nie przyłączy rozmowę.

![Rozmowa wystąpień](tutorial-getting-started-with-signalr/_static/image1.png)

Sekcje:

- [Konfigurowanie projektu](#setup)
- [Uruchom próbki](#run)
- [Sprawdź kod](#code)
- [Następne kroki](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a>Konfigurowanie projektu

W tej sekcji przedstawiono sposób Użyj programu Visual Studio 2013 i SignalR w wersji 2, aby utworzyć pustą aplikację sieci web ASP.NET, Dodaj SignalR i tworzenie aplikacji czatu.

Wymagania wstępne:

- Visual Studio 2013. Jeśli nie masz programu Visual Studio, zobacz [pobiera ASP.NET](https://www.asp.net/downloads) uzyskać bezpłatne narzędzie Visual Studio 2013 Express programowanie.

Następujące kroki Użyj programu Visual Studio 2013, aby utworzyć pustą aplikację sieci Web ASP.NET i dodać biblioteki SignalR:

1. W programie Visual Studio Utwórz aplikację sieci Web platformy ASP.NET.

    ![Tworzenie sieci web](tutorial-getting-started-with-signalr/_static/image2.png)
2. W **nowy projekt ASP.NET** okna, pozostaw **pusty** zaznaczone, a następnie kliknij przycisk **tworzenia projektu**.

    ![Utwórz pusty sieci web](tutorial-getting-started-with-signalr/_static/image3.png)
3. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt, wybierz **Dodaj | Klasy koncentratora SignalR (v2)**. Nazwa klasy **ChatHub.cs** i dodaj go do projektu. Spowoduje to utworzenie **ChatHub** i dodaje do projektu zestawu plików skryptów i odwołania do zestawów, które obsługują SignalR.

    > [!NOTE]
    > Można również dodać SignalR na projekt, otwierając **narzędzia | Menedżer pakietów biblioteki | Konsola Menedżera pakietów** i uruchamiając polecenie:

    `install-package Microsoft.AspNet.SignalR`

    Jeśli używasz konsoli można dodać SignalR, należy utworzyć klasę koncentratora SignalR w osobnym kroku po dodaniu SignalR.

    > [!NOTE]
    > Jeśli używasz programu Visual Studio 2012, **klasy koncentratora SignalR (v2)** szablonu nie będą dostępne. Możesz dodać zwykły **klasy** o nazwie `ChatHub` zamiast tego.
4. W **Eksploratora rozwiązań**, rozwiń węzeł skryptów. Biblioteki skryptów, jQuery i SignalR są widoczne w projekcie.
5. Zastąp kod w nowej **ChatHub** klasy następującym kodem.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample1.cs)]
6. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt, a następnie kliknij przycisk **Dodaj | Klasy początkowej OWIN**. Nazwa nowej klasy `Startup` i kliknij przycisk OK.

    > [!NOTE]
    > Jeśli używasz programu Visual Studio 2012, **klasy początkowej OWIN** szablonu nie będą dostępne. Możesz dodać zwykły **klasy** o nazwie `Startup` zamiast tego.
7. Zmiany zawartości nowa klasa uruchamiania do następującego.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample2.cs)]
8. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt, a następnie kliknij przycisk **Dodaj | Strona HTML**. Nazwa nowej strony `index.html`.
    >[!NOTE]
    >Może być konieczna zmiana numery wersji dla odwołania do biblioteki JQuery i SignalR
9. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy właśnie utworzony strony HTML i kliknij przycisk **Ustaw jako stronę startową**.
10. Zastąp następujący kod w kodzie domyślnym na stronie HTML.

    > [!NOTE]
    > Może być zainstalowana nowsza wersja skryptów SignalR przez Menedżera pakietów. Sprawdź, czy poniżej odwołań do skryptów odpowiadają wersji plików skryptów w projekcie (będą różne, jeśli dodano SignalR przy użyciu narzędzia NuGet, a nie dodaje koncentratora.)

    [!code-html[Main](tutorial-getting-started-with-signalr/samples/sample3.html)]
11. **Zapisz wszystkie** dla projektu.

<a id="run"></a>

## <a name="run-the-sample"></a>Uruchom próbki

1. Naciśnij klawisz F5, aby uruchomić projekt w trybie debugowania. Ładuje strony HTML w wystąpieniu przeglądarki i wyświetla monit o nazwę użytkownika.

    ![Wprowadź nazwę użytkownika](tutorial-getting-started-with-signalr/_static/image4.png)
2. Wprowadź nazwę użytkownika.
3. Skopiuj adres URL w wierszu adresu przeglądarki i używać go otworzyć dwa kolejne wystąpienia przeglądarki. W każdym wystąpieniu przeglądarki wprowadź unikatową nazwę użytkownika.
4. W każdym wystąpieniu przeglądarki dodać komentarz, a następnie kliknij przycisk **wysyłania**. Komentarze powinien być wyświetlany we wszystkich wystąpieniach przeglądarki.

    > [!NOTE]
    > Tej aplikacji rozmów proste kontekstu dyskusji na serwerze nie są zachowywane. Koncentrator emituje komentarze do wszystkich bieżących użytkowników. Użytkownicy, którzy później dołączyć rozmowę zobaczą wiadomości dodane od czasu dołączenia.

    Poniższy zrzut ekranu przedstawia rozmów aplikacja była uruchomiona w trzech przypadkach przeglądarki, które są aktualizowane w jedno wystąpienie wysyła komunikat:

    ![Rozmowa przeglądarki](tutorial-getting-started-with-signalr/_static/image5.png)
5. W **Eksploratora rozwiązań**, sprawdź **dokumentów skryptu** węzła dla działającej aplikacji. Brak pliku skryptu o nazwie **koncentratory** generujący biblioteki SignalR dynamicznie w czasie wykonywania. Ten plik zarządza komunikacji między skryptu jQuery i kod po stronie serwera.

    ![](tutorial-getting-started-with-signalr/_static/image6.png)

<a id="code"></a>

## <a name="examine-the-code"></a>Sprawdź kod

SignalR aplikacji czatu pokazano dwa podstawowe SignalR zadań związanych z projektowaniem: tworzenie koncentratora jako obiekt główny koordynacji na serwerze i używanie biblioteki jQuery SignalR do wysyłania i odbierania wiadomości.

### <a name="signalr-hubs"></a>Koncentratory SignalR

W przykładowym kodzie **ChatHub** pochodną klasy **Microsoft.AspNet.SignalR.Hub** klasy. Wyprowadzanie z **Centrum** klasy jest to wygodny sposób utworzyć aplikację SignalR. Można utworzyć metody publiczne na klasy koncentratora, a następnie uzyskać dostępu do tych metod, wywołując ze skryptów na stronie sieci web.

W kodzie rozmów wywołać klientów **ChatHub.Send** metody do wysłania nowej wiadomości. Koncentrator z kolei wysyła wiadomość do wszystkich klientów przez wywołanie metody **Clients.All.broadcastMessage**.

**Wysyłania** metody przedstawiono kilka pojęć Centrum:

- Metody publiczne należy zadeklarować w koncentratorze tak, aby klienci mogą połączeń telefonicznych z nimi.
- Użyj **Microsoft.AspNet.SignalR.Hub.Clients** właściwość dynamiczna ma dostęp do wszystkich klientów podłączone do tego koncentratora.
- Wywoływanie funkcji na kliencie (takich jak `broadcastMessage` funkcji) aby zaktualizować klientów.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample4.cs)]

### <a name="signalr-and-jquery"></a>SignalR i jQuery

Strony HTML w przykładowy kod przedstawia sposób użycia biblioteki jQuery SignalR do komunikowania się przy użyciu koncentratora SignalR. Podstawowe zadania w kodzie są deklarowanie serwer proxy, aby odwołać koncentratora, deklarowanie funkcję serwera można wywołać w celu wypychania zawartości do klientów, a następnie uruchomić połączenie do wysłania wiadomości do koncentratora.

Poniższy kod deklaruje odwołanie do serwera proxy koncentratora.

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample5.js)]

> [!NOTE]
> W języku JavaScript odwołanie do klasy serwera i jej elementów członkowskich jest w camelcase. Przykładowy kod odwołuje się do języka C# **ChatHub** klasy w języku JavaScript jako **chatHub**.


Następujący kod jest, jak utworzyć funkcję wywołania zwrotnego w skrypcie. Klasy koncentratora na serwerze wywołanie tej funkcji, aby wysyłać aktualizacje zawartości do każdego klienta. Dwa wiersze, czy kodowanie HTML zawartości przed wyświetleniem go są opcjonalne i Pokaż prosty sposób, aby uniemożliwić uruchomienie skryptu.

[!code-html[Main](tutorial-getting-started-with-signalr/samples/sample6.html)]

Poniższy kod przedstawia sposób nawiązać połączenie z koncentratorem. Kod rozpoczyna połączenie, a następnie przekazuje on funkcję obsługi zdarzenia kliknij na **wysyłania** przycisku na stronie HTML.

> [!NOTE]
> Takie podejście zapewnia, że połączenie zostanie nawiązane, przed wykonaniem programu obsługi zdarzeń.


[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample7.js)]

<a id="next"></a>

## <a name="next-steps"></a>Następne kroki

Wiesz, że SignalR to struktura służąca do tworzenia aplikacji sieci web w czasie rzeczywistym. Przedstawiono również kilka zadań związanych z projektowaniem SignalR: jak dodać do aplikacji ASP.NET SignalR, Tworzenie klasy koncentratora oraz sposobu wysyłania i odbierania wiadomości z koncentratora.

Aby uzyskać wskazówki dotyczące sposobu wdrażania przykładowej aplikacji SignalR na platformie Azure, zobacz [SignalR korzystanie z aplikacji sieci Web w usłudze Azure App Service](../deployment/using-signalr-with-azure-web-sites.md). Aby uzyskać szczegółowe informacje o sposobie wdrażania projektu sieci web programu Visual Studio do witryny sieci Web systemu Windows Azure, zobacz [tworzenie aplikacji sieci web platformy ASP.NET w usłudze Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).

Aby uzyskać bardziej zaawansowane pojęcia rozwój SignalR, odwiedź następującą witrynę SignalR kod źródłowy i zasobów:

- [Projekt SignalR](http://signalr.net)
- [SignalR Github i przykłady](https://github.com/SignalR/SignalR)
- [Witryna typu Wiki biblioteki SignalR](https://github.com/SignalR/SignalR/wiki)
