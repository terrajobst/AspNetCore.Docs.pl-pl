---
uid: signalr/overview/older-versions/tutorial-getting-started-with-signalr
title: 'Samouczek: Wprowadzenie do korzystania z SignalR 1.x | Dokumentacja firmy Microsoft'
author: pfletcher
description: "Umożliwia tworzenie aplikacji czatu w czasie rzeczywistym na stronie HTML ASP.NET SignalR."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: fdc3599a-5217-44c1-951f-0eec9812dce7
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/tutorial-getting-started-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: c61be6f7a64c000c8d9489f35eea520fd0bb32dd
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="tutorial-getting-started-with-signalr-1x"></a>Samouczek: Wprowadzenie do korzystania z SignalR 1.x
====================
przez [Patrick Fletcher](https://github.com/pfletcher), [Teebken Timowi](https://github.com/timlt)

> Ten samouczek pokazuje, jak używać SignalR do tworzenia aplikacji rozmów w czasie rzeczywistym. Spowoduje dodanie SignalR do pustą aplikację sieci web ASP.NET i Utwórz stronę HTML do wysyłania i wyświetlenie komunikatów.


## <a name="overview"></a>Omówienie

W tym samouczku wprowadzenie programowanie SignalR przez przedstawiający sposób tworzenia aplikacji proste rozmów bazujące na przeglądarce. Zostanie dodać biblioteki SignalR do pusta aplikacja sieci web platformy ASP.NET, Utwórz klasę koncentratora do wysyłania wiadomości do klientów i utworzyć strona HTML, która umożliwia użytkownikom wysyłanie i odbieranie wiadomości. Podobne samouczek przedstawia sposób tworzenia aplikacji rozmów w MVC 4 przy użyciu widoku MVC, zobacz [wprowadzenie SignalR i MVC 4](index.md).

> [!NOTE]
> Ten samouczek używa (1.x) wersji biblioteki signalr. Aby uzyskać szczegółowe informacje o zmianach między SignalR 1.x i 2.0, zobacz [SignalR Uaktualnianie projektów 1.x](../releases/upgrading-signalr-1x-projects-to-20.md).

SignalR to open source biblioteki .NET do tworzenia aplikacji sieci web, które wymagają interakcji z użytkownikiem na żywo lub aktualizacji danych w czasie rzeczywistym. Przykładami społecznościowych aplikacji, wielodostępnym gry, business współpracy i grup dyskusyjnych, pogody lub finansowych aktualizacji aplikacji. Są one często nazywane aplikacji w czasie rzeczywistym.

SignalR upraszcza proces tworzenia aplikacji w czasie rzeczywistym. Zawiera bibliotekę serwera programu ASP.NET i biblioteki klienckiej JavaScript, aby ułatwić zarządzanie połączeniami klient serwer i wypychanie aktualizacji zawartości do klientów. Biblioteka SignalR można dodać do istniejącej aplikacji ASP.NET do uzyskania funkcjonalność w czasie rzeczywistym.

Samouczek przedstawia następujące SignalR zadań związanych z projektowaniem:

- Dodawanie biblioteki SignalR do aplikacji sieci web ASP.NET.
- Tworzenie klasy koncentratora do dystrybuowania zawartości do klientów.
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

W tej sekcji pokazano, jak utworzyć pustą aplikację sieci web ASP.NET, Dodaj SignalR i tworzenie aplikacji czatu.

Wymagania wstępne:

- Visual Studio 2010 z dodatkiem SP1 lub 2012. Jeśli nie masz programu Visual Studio, zobacz [pobiera ASP.NET](https://www.asp.net/downloads) uzyskać bezpłatne narzędzie Visual Studio 2012 Express programowanie.
- [Microsoft ASP.NET i sieć Web narzędzi 2012.2](https://go.microsoft.com/fwlink/?LinkId=279941). Dla programu Visual Studio 2012 ten Instalator dodaje nowe funkcje platformy ASP.NET w tym szablony SignalR dla programu Visual Studio. Dla programu Visual Studio 2010 z dodatkiem SP1 Instalator nie jest dostępny, ale można Ukończ samouczek, instalując pakiet SignalR NuGet, zgodnie z opisem w ramach kroków konfiguracji.

Następujące kroki Użyj programu Visual Studio 2012, aby utworzyć pustą aplikację sieci Web ASP.NET i dodać biblioteki SignalR:

1. W programie Visual Studio Utwórz pustą aplikację sieci Web ASP.NET.

    ![Utwórz pusty sieci web](tutorial-getting-started-with-signalr/_static/image2.png)
2. Otwórz **Konsola Menedżera pakietów** wybierając **narzędzia | Menedżer pakietów biblioteki | Konsola Menedżera pakietów**. Wprowadź następujące polecenie w oknie konsoli:

    `Install-Package Microsoft.AspNet.SignalR -Version 1.1.3`

    To polecenie powoduje zainstalowanie najnowszej wersji biblioteki signalr 1.x.
3. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt, wybierz **Dodaj | Klasa**. Nazwa nowej klasy **ChatHub**.
4. W **Eksploratora rozwiązań** rozwiń węzeł skryptów. Biblioteki skryptów, jQuery i SignalR są widoczne w projekcie.

    ![Odwołania do biblioteki](tutorial-getting-started-with-signalr/_static/image3.png)
5. Zastąp kod w **ChatHub** klasy następującym kodem.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample1.cs)]
6. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt, a następnie kliknij przycisk **Dodaj | Nowy element**. W **Dodaj nowy element** okno dialogowe, wybierz opcję **globalnej klasy aplikacji** i kliknij przycisk **Dodaj**.

    ![Dodaj globalne](tutorial-getting-started-with-signalr/_static/image4.png)
7. Dodaj następujące `using` instrukcje po dostarczonych `using` instrukcje w klasie Global.asax.cs.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample2.cs)]
8. Dodaj następujący wiersz kodu w `Application_Start` metody klasy globalny można zarejestrować trasy domyślnej koncentratorów SignalR.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample3.cs)]
9. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt, a następnie kliknij przycisk **Dodaj | Nowy element**. W **Dodaj nowy element** okno dialogowe, wybierz strony Html i kliknij przycisk **Dodaj**.
10. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy właśnie utworzony strony HTML i kliknij przycisk **Ustaw jako stronę startową**.
11. Zastąp następujący kod w kodzie domyślnym na stronie HTML.

    [!code-html[Main](tutorial-getting-started-with-signalr/samples/sample4.html)]
12. **Zapisz wszystkie** dla projektu.

<a id="run"></a>

## <a name="run-the-sample"></a>Uruchom próbki

1. Naciśnij klawisz F5, aby uruchomić projekt w trybie debugowania. Ładuje strony HTML w wystąpieniu przeglądarki i wyświetla monit o nazwę użytkownika.

    ![Wprowadź nazwę użytkownika](tutorial-getting-started-with-signalr/_static/image5.png)
2. Wprowadź nazwę użytkownika.
3. Skopiuj adres URL w wierszu adresu przeglądarki i używać go otworzyć dwa kolejne wystąpienia przeglądarki. W każdym wystąpieniu przeglądarki wprowadź unikatową nazwę użytkownika.
4. W każdym wystąpieniu przeglądarki dodać komentarz, a następnie kliknij przycisk **wysyłania**. Komentarze powinien być wyświetlany we wszystkich wystąpieniach przeglądarki.

    > [!NOTE]
    > Tej aplikacji rozmów proste kontekstu dyskusji na serwerze nie są zachowywane. Koncentrator emituje komentarze do wszystkich bieżących użytkowników. Użytkownicy, którzy później dołączyć rozmowę zobaczą wiadomości dodane od czasu dołączenia.

    Poniższy zrzut ekranu przedstawia rozmów aplikacja była uruchomiona w trzech przypadkach przeglądarki, które są aktualizowane w jedno wystąpienie wysyła komunikat:

    ![Rozmowa przeglądarki](tutorial-getting-started-with-signalr/_static/image6.png)
5. W **Eksploratora rozwiązań**, sprawdź **dokumentów skryptu** węzła dla działającej aplikacji. Brak pliku skryptu o nazwie **koncentratory** generujący biblioteki SignalR dynamicznie w czasie wykonywania. Ten plik zarządza komunikacji między skryptu jQuery i kod po stronie serwera.

    ![Wygenerowany Centrum skryptów](tutorial-getting-started-with-signalr/_static/image7.png)

<a id="code"></a>

## <a name="examine-the-code"></a>Sprawdź kod

SignalR aplikacji czatu pokazano dwa podstawowe SignalR zadań związanych z projektowaniem: tworzenie koncentratora jako obiekt główny koordynacji na serwerze i używanie biblioteki jQuery SignalR do wysyłania i odbierania wiadomości.

### <a name="signalr-hubs"></a>Koncentratory SignalR

W przykładowym kodzie **ChatHub** pochodną klasy **Microsoft.AspNet.SignalR.Hub** klasy. Wyprowadzanie z **Centrum** klasy jest to wygodny sposób utworzyć aplikację SignalR. Można utworzyć metody publiczne na klasy koncentratora, a następnie te metody dostępu wywołując ze skryptów jQuery na stronie sieci web.

W kodzie rozmów wywołać klientów **ChatHub.Send** metody do wysłania nowej wiadomości. Koncentrator z kolei wysyła wiadomość do wszystkich klientów przez wywołanie metody **Clients.All.broadcastMessage**.

**Wysyłania** metody przedstawiono kilka pojęć Centrum:

- Metody publiczne należy zadeklarować w koncentratorze tak, aby klienci mogą połączeń telefonicznych z nimi.
- Użyj **Microsoft.AspNet.SignalR.Hub.Clients** właściwość dynamiczna ma dostęp do wszystkich klientów podłączone do tego koncentratora.
- Wywoływanie funkcji jQuery na kliencie (takich jak `broadcastMessage` funkcji) aby zaktualizować klientów.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a>SignalR i jQuery

Strony HTML w przykładowy kod przedstawia sposób użycia biblioteki jQuery SignalR do komunikowania się przy użyciu koncentratora SignalR. Podstawowe zadania w kodzie są deklarowanie serwer proxy, aby odwołać koncentratora, deklarowanie funkcję serwera można wywołać w celu wypychania zawartości do klientów, a następnie uruchomić połączenie do wysłania wiadomości do koncentratora.

Poniższy kod deklaruje serwer proxy koncentratora.

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample6.js)]

> [!NOTE]
> Odwołanie do klasy serwera i jej elementów członkowskich w jQuery znajduje camelcase. Przykładowy kod odwołuje się do języka C# **ChatHub** klasy w jQuery jako **chatHub**.


Następujący kod jest, jak utworzyć funkcję wywołania zwrotnego w skrypcie. Klasy koncentratora na serwerze wywołanie tej funkcji, aby wysyłać aktualizacje zawartości do każdego klienta. Dwa wiersze, czy kodowanie HTML zawartości przed wyświetleniem go są opcjonalne i Pokaż prosty sposób, aby uniemożliwić uruchomienie skryptu.

[!code-html[Main](tutorial-getting-started-with-signalr/samples/sample7.html)]

Poniższy kod przedstawia sposób nawiązać połączenie z koncentratorem. Kod rozpoczyna połączenie, a następnie przekazuje on funkcję obsługi zdarzenia kliknij na **wysyłania** przycisku na stronie HTML.

> [!NOTE]
> Takie podejście temu, że połączenie zostanie nawiązane, przed wykonaniem programu obsługi zdarzeń.


[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a>Następne kroki

Wiesz, że SignalR to struktura służąca do tworzenia aplikacji sieci web w czasie rzeczywistym. Przedstawiono również kilka zadań związanych z projektowaniem SignalR: jak dodać do aplikacji ASP.NET SignalR, Tworzenie klasy koncentratora oraz sposobu wysyłania i odbierania wiadomości z koncentratora.

Można udostępnić przykładowej aplikacji w tym samouczku lub innych aplikacji SignalR za pośrednictwem Internetu przez wdrożenie ich do dostawcy hostingu. Firma Microsoft oferuje usługi hostingu sieci web wolnego maksymalnie 10 witryn sieci web w bezpłatny [systemu Windows Azure, konto próbne](https://www.windowsazure.com/en-us/pricing/free-trial/?WT.mc_id=A443DD604). Aby uzyskać wskazówki dotyczące sposobu wdrażania przykładowej aplikacji SignalR, zobacz [publikowania SignalR pobieranie rozpoczęto próbki jako witryny sieci Web systemu Windows Azure](https://blogs.msdn.com/b/timlee/archive/2013/02/27/deploy-the-signalr-getting-started-sample-as-a-windows-azure-web-site.aspx). Aby uzyskać szczegółowe informacje o sposobie wdrażania projektu sieci web programu Visual Studio do witryny sieci Web systemu Windows Azure, zobacz [wdrażanie aplikacji ASP.NET do witryny sieci Web systemu Windows Azure](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet). (Uwaga: transportu protokołu WebSocket nie jest obecnie obsługiwana dla witryn sieci Web systemu Windows Azure. Protokół WebSocket podczas transportu nie jest dostępna, SignalR używa innych dostępnych transportów zgodnie z opisem w sekcji transportów [wprowadzenie do tematu SignalR](index.md).)

Aby uzyskać bardziej zaawansowane pojęcia rozwój SignalR, odwiedź następującą witrynę SignalR kod źródłowy i zasobów:

- [Projekt SignalR](http://signalr.net)
- [SignalR Github i przykłady](https://github.com/SignalR/SignalR)
- [Witryna typu Wiki biblioteki SignalR](https://github.com/SignalR/SignalR/wiki)
