---
uid: signalr/overview/older-versions/tutorial-getting-started-with-signalr
title: 'Samouczek: Wprowadzenie do SignalR 1.x | Dokumentacja firmy Microsoft'
author: pfletcher
description: Użyj biblioteki SignalR platformy ASP.NET, aby utworzyć aplikację do rozmów w czasie rzeczywistym na stronie HTML.
ms.author: riande
ms.date: 02/18/2013
ms.assetid: fdc3599a-5217-44c1-951f-0eec9812dce7
msc.legacyurl: /signalr/overview/older-versions/tutorial-getting-started-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 2223675ab2ec40a7e25229bf34b2f0ffddc31fed
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752707"
---
<a name="tutorial-getting-started-with-signalr-1x"></a>Samouczek: Wprowadzenie do SignalR 1.x
====================
przez [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)

> Ten samouczek pokazuje, jak utworzyć aplikację do obsługi rozmów w czasie rzeczywistym przy użyciu SignalR. Będzie dodać SignalR do pustych aplikacji sieci web ASP.NET i Utwórz stronę HTML do wysyłania i wyświetla komunikaty.


## <a name="overview"></a>Omówienie

W tym samouczku przedstawiono rozwoju SignalR, pokazując, jak utworzyć aplikację proste rozmowy opartej na przeglądarce. Będzie dodać biblioteki SignalR do pustą aplikację sieci web platformy ASP.NET, Utwórz klasę hub wysyłanie komunikatów do klientów i Utwórz stronę HTML, który umożliwia użytkownikom wysyłanie i odbieranie wiadomości rozmowy. Z podobnego samouczka dotyczącego pokazującym, jak utworzyć aplikację do obsługi rozmów w MVC 4 przy użyciu widoku MVC, zobacz [wprowadzenie do SignalR i MVC 4](index.md).

> [!NOTE]
> W tym samouczku korzysta z wersji release (1.x) SignalR. Aby uzyskać szczegółowe informacje na temat zmian między SignalR 1.x i 2.0, zobacz [projektów uaktualnianie SignalR 1.x](../releases/upgrading-signalr-1x-projects-to-20.md).

SignalR to biblioteki .NET typu open source do tworzenia aplikacji internetowych, które wymagają interakcji z użytkownikiem na żywo lub aktualizacji danych w czasie rzeczywistym. Przykłady obejmują społecznościowe aplikacje, gry wielodostępnym, biznesowych współpracę i w nowościach, pogody lub finansowych aktualizacji aplikacji. Są one często nazywane aplikacji w czasie rzeczywistym.

SignalR upraszcza proces tworzenia aplikacji w czasie rzeczywistym. Obejmuje ona bibliotekę serwera programu ASP.NET oraz bibliotekę kliencką JavaScript, aby ułatwić zarządzanie połączeniami klient serwer i wypychanie aktualizacji zawartości dla klientów. Biblioteki SignalR można dodać do istniejącej aplikacji ASP.NET do uzyskania funkcji w czasie rzeczywistym.

Samouczek przedstawia następujące zadania deweloperskie SignalR:

- Dodawanie biblioteki SignalR do aplikacji sieci web ASP.NET.
- Tworzenie klasy koncentratora, aby wypchnąć zawartości do klientów.
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

W tej sekcji pokazano, jak utworzyć pustą aplikację sieci web platformy ASP.NET, Dodaj SignalR i tworzenie aplikacji czatu.

Wymagania wstępne:

- Visual Studio 2010 z dodatkiem SP1 lub 2012. Jeśli nie masz programu Visual Studio, zobacz [ASP.NET pliki do pobrania](https://www.asp.net/downloads) można pobrać bezpłatny program Visual Studio 2012 Express narzędzia programistyczne.
- [Microsoft ASP.NET and Web Tools 2012.2](https://go.microsoft.com/fwlink/?LinkId=279941). Dla programu Visual Studio 2012 Instalator dodaje nowe funkcje platformy ASP.NET, w tym szablony SignalR do programu Visual Studio. Dla programu Visual Studio 2010 z dodatkiem SP1 Instalator nie jest dostępny, ale zakończysz pracę z samouczkiem, instalując pakiet NuGet biblioteki SignalR, zgodnie z opisem w ramach kroków konfiguracji.

Poniższe kroki Użyj programu Visual Studio 2012, aby utworzyć pustą aplikację sieci Web platformy ASP.NET i dodać biblioteki SignalR:

1. W programie Visual Studio Utwórz pustą aplikację sieci Web platformy ASP.NET.

    ![Tworzenie pustego sieci web](tutorial-getting-started-with-signalr/_static/image2.png)
2. Otwórz **Konsola Menedżera pakietów** , wybierając **narzędzia | Menedżer pakietów biblioteki | Konsola Menedżera pakietów**. Wprowadź następujące polecenie w oknie konsoli:

    `Install-Package Microsoft.AspNet.SignalR -Version 1.1.3`

    To polecenie powoduje zainstalowanie najnowszej wersji biblioteki SignalR 1.x.
3. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt, wybierz **Dodaj | Klasa**. Nadaj nowej klasie **ChatHub**.
4. W **Eksploratora rozwiązań** rozwiń węzeł skryptów. Biblioteki skryptów, jQuery i SignalR są widoczne w projekcie.

    ![Odwołań do biblioteki](tutorial-getting-started-with-signalr/_static/image3.png)
5. Zastąp kod w **ChatHub** klasy z następującym kodem.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample1.cs)]
6. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt, a następnie kliknij przycisk **Dodaj | Nowy element**. W **Dodaj nowy element** okno dialogowe, wybierz opcję **globalna klasa aplikacji** i kliknij przycisk **Dodaj**.

    ![Dodaj globalne](tutorial-getting-started-with-signalr/_static/image4.png)
7. Dodaj następujący kod `using` instrukcji po podane `using` instrukcji w klasie Global.asax.cs.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample2.cs)]
8. Dodaj następujący wiersz kodu w `Application_Start` metody klasy globalny można zarejestrować domyślną trasę dla centrów SignalR.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample3.cs)]
9. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt, a następnie kliknij przycisk **Dodaj | Nowy element**. W **Dodaj nowy element** okno dialogowe, wybierz stronę Html i kliknij przycisk **Dodaj**.
10. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy właśnie utworzony strony HTML i kliknij przycisk **Ustaw jako strona startowa**.
11. Zastąp kod domyślnej strony HTML z następującym kodem.

    [!code-html[Main](tutorial-getting-started-with-signalr/samples/sample4.html)]
12. **Zapisz wszystko** dla projektu.

<a id="run"></a>

## <a name="run-the-sample"></a>Uruchamianie aplikacji przykładowej

1. Naciśnij klawisz F5, aby uruchomić projekt w trybie debugowania. Strona HTML ładuje się w wystąpieniu przeglądarki i wyświetla monit o nazwę użytkownika.

    ![Wprowadź nazwę użytkownika](tutorial-getting-started-with-signalr/_static/image5.png)
2. Wprowadź nazwę użytkownika.
3. Skopiuj adres URL w wierszu adresu przeglądarki i użyj go, aby otworzyć dwóch większej liczby wystąpień przeglądarki. W każdym wystąpieniu przeglądarki wprowadź unikatową nazwę użytkownika.
4. W każdym wystąpieniu przeglądarki Dodaj komentarz, a następnie kliknij przycisk **wysyłania**. Komentarze powinien być wyświetlany we wszystkich wystąpieniach przeglądarki.

    > [!NOTE]
    > Tej aplikacji rozmów prostego nie utrzymuje kontekst dyskusji na serwerze. Centrum emituje komentarzy do wszystkich bieżących użytkowników. Użytkownicy, którzy dołączają do czatu później zostanie wyświetlony komunikat o dodane od czasu dołączenia.

    Poniższy zrzut ekranu przedstawia aplikacji rozmów w trzech wystąpień przeglądarki, z których wszystkie są aktualizowane w jedno wystąpienie jest wysyłana wiadomość:

    ![Rozmowa przeglądarki](tutorial-getting-started-with-signalr/_static/image6.png)
5. W **Eksploratora rozwiązań**, zbadaj **dokumenty skryptów** węzła dla działającej aplikacji. Brak pliku skryptu o nazwie **koncentratory** generujący biblioteki SignalR dynamicznie w czasie wykonywania. Ten plik zarządza komunikacji między jQuery skryptu i kod po stronie serwera.

    ![Wygenerowany Centrum skryptów](tutorial-getting-started-with-signalr/_static/image7.png)

<a id="code"></a>

## <a name="examine-the-code"></a>Poszukaj w kodzie

Aplikacji rozmów SignalR pokazuje dwa podstawowe zadania rozwoju SignalR: tworzenie koncentratora jako obiekt główny koordynacji na serwerze i za pomocą biblioteki jQuery SignalR do wysyłania i odbierania komunikatów.

### <a name="signalr-hubs"></a>Koncentratory SignalR

W przykładowym kodzie **ChatHub** klasa pochodzi od **Microsoft.AspNet.SignalR.Hub** klasy. Wyprowadzanie z **Centrum** klasy jest to wygodny sposób, aby skompilować aplikację SignalR. Można utworzyć metody publiczne na klasy koncentratora, a następnie uzyskać dostęp do tych metod, wywołując je z jQuery skrypty na stronie sieci web.

W kodzie Rozmowa klienci wywołują **ChatHub.Send** metodę, aby wysyłać nowy komunikat. Centrum z kolei wysyła wiadomość do wszystkich klientów, wywołując **Clients.All.broadcastMessage**.

**Wysyłania** metoda pokazuje kilka koncepcji w Centrum:

- Tak, aby je wywoływać klientów do deklarowania metod publicznych w koncentratorze.
- Użyj **Microsoft.AspNet.SignalR.Hub.Clients** właściwość dynamiczna ma dostęp do wszystkich klientów podłączone do tego koncentratora.
- Wywołaj funkcję jQuery na kliencie (takich jak `broadcastMessage` funkcji) aby zaktualizować klientów.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a>SignalR i jQuery

Strony HTML w przykładzie kodu pokazano, jak użyć biblioteki jQuery SignalR do komunikowania się z Centrum SignalR. Serwer proxy, aby odwoływać się do Centrum deklarowania funkcji, która serwera można wywołać w celu wypychania zawartości do klientów i od połączenia, aby wysyłać komunikaty do Centrum deklaruje podstawowych zadań w kodzie.

Poniższy kod deklaruje serwera proxy koncentratora.

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample6.js)]

> [!NOTE]
> Odwołanie do klasy serwera i jej elementów członkowskich w jQuery znajduje się w camelcase. Przykładowy kod odwołuje się do języka C# **ChatHub** klasy w jQuery jako **chatHub**.


Poniższy kod jest na tym, jak utworzyć funkcję wywołania zwrotnego w skrypcie. Klasy koncentratora na serwerze wywołuje tę funkcję, aby wypchnąć aktualizacji zawartości dla każdego klienta. Dwa wiersze, czy kodowanie HTML zawartości przed wyświetleniem go są opcjonalne i Pokaż prostego sposobu zapobiegania uruchomienie skryptu.

[!code-html[Main](tutorial-getting-started-with-signalr/samples/sample7.html)]

Poniższy kod przedstawia sposób nawiązać połączenie z koncentratorem. Kod uruchamia połączenie i przekazuje go po funkcji do obsługi zdarzenia click na **wysyłania** przycisk na stronie HTML.

> [!NOTE]
> To podejście ubezpieczycielom, że połączenie zostało nawiązane przed wykonaniem programu obsługi zdarzeń.


[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a>Następne kroki

Wiesz, że SignalR to architektura służąca do tworzenia aplikacji sieci web w czasie rzeczywistym. Pokazaliśmy również kilka zadań programistycznych SignalR: jak dodać SignalR do aplikacji ASP.NET, sposób tworzenia klasy koncentratora oraz jak wysyłać i odbierać komunikaty z Centrum.

Możesz udostępnić przykładowej aplikacji w tym samouczku lub innych aplikacji SignalR w Internecie, wdrażając je do dostawcy usług hostingowych. Firma Microsoft oferuje bezpłatny internetowy hostowanie do 10 witryn sieci web w ramach bezpłatnego [konta wersji próbnej usługi Windows Azure](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604). Aby uzyskać wskazówki dotyczące sposobu wdrażania przykładowej aplikacji SignalR, zobacz [publikowania SignalR wprowadzenie do przykładu jako witryny sieci Web Azure Windows](https://blogs.msdn.com/b/timlee/archive/2013/02/27/deploy-the-signalr-getting-started-sample-as-a-windows-azure-web-site.aspx). Aby uzyskać szczegółowe informacje o sposobie wdrażania projektu sieci web programu Visual Studio z witryny sieci Web do Windows Azure, zobacz [wdrażanie aplikacji ASP.NET do witryny sieci Web Windows Azure](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet). (Uwaga: transportu WebSocket nie jest obecnie obsługiwane dla witryn sieci Web Windows Azure. WebSocket podczas transportu nie jest dostępna, SignalR używa innych dostępnych transportu, zgodnie z opisem w sekcji transportów [wprowadzenie do SignalR tematu](index.md).)

Aby dowiedzieć się bardziej zaawansowanych pojęciach zmiany SignalR, odwiedź następującą witrynę dla SignalR kod źródłowy i zasobów:

- [Projekt SignalR](http://signalr.net)
- [SignalR Github i przykłady](https://github.com/SignalR/SignalR)
- [Witryny typu Wiki biblioteki SignalR](https://github.com/SignalR/SignalR/wiki)
