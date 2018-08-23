---
uid: signalr/overview/deployment/tutorial-signalr-self-host
title: 'Samouczek: Host samodzielny SignalR | Dokumentacja firmy Microsoft'
author: pfletcher
description: W tym samouczku pokazano, jak utworzyć samodzielnie hostowany serwer SignalR 2 i jak połączyć się z nim za pomocą klienta języka JavaScript. Wersje oprogramowania używanego w tym samouczku V...
ms.author: riande
ms.date: 06/10/2014
ms.assetid: 400db427-27af-4f2f-abf0-5486d5e024b5
msc.legacyurl: /signalr/overview/deployment/tutorial-signalr-self-host
msc.type: authoredcontent
ms.openlocfilehash: 5d7d485357a6c820f11e0135e2ff9479c1965d96
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756602"
---
<a name="tutorial-signalr-self-host"></a>Samouczek: Host samodzielny SignalR
====================
przez [Patrick Fletcher](https://github.com/pfletcher)

[Pobieranie ukończone projektu](http://code.msdn.microsoft.com/SignalR-Self-Host-Sample-6da0f383)

> W tym samouczku pokazano, jak utworzyć samodzielnie hostowany serwer SignalR 2 i jak połączyć się z nim za pomocą klienta języka JavaScript.
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
> ## <a name="questions-and-comments"></a>Pytania i komentarze
> 
> Jak się podoba w tym samouczku, i co można było ulepszyć proces w komentarzach u dołu strony, wystaw opinię. Jeśli masz pytania, na które nie są bezpośrednio związane z tego samouczka, możesz zamieścić je do [forum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) lub [StackOverflow.com](http://stackoverflow.com/).


## <a name="overview"></a>Omówienie

Serwer biblioteki SignalR zwykle znajduje się w aplikacji ASP.NET w usługach IIS, ale może też być samodzielnie hostowane (na przykład w aplikacji konsoli lub usługa Windows) przy użyciu biblioteki hosta samodzielnego. Ta biblioteka, podobnie jak wszystkie SignalR 2, jest oparta na OWIN ([Open Web Interface for .NET](http://owin.org)). OWIN definiuje abstrakcję między serwerami sieci web platformy .NET i aplikacji sieci web. OWIN oddziela aplikacji sieci web na serwerze, co sprawia, że OWIN jest idealnym rozwiązaniem dla hostingu samodzielnego aplikacji sieci web w swoim własnym procesie, poza usług IIS.

Nie obsługują w usługach IIS przyczyny:

- Środowiska, w której usługi IIS są niedostępne lub pożądane, takich jak istniejącej farmy serwerów bez usługi IIS.
- Zmniejszenie wydajności programu IIS należy unikać.
- Funkcja SignalR jest ma zostać dodany do istniejącego wystąpienia usług aplikacji, która działa w usługę Windows, rola procesu roboczego platformy Azure lub inny proces.

Jeśli to rozwiązanie jest obecnie sporządzana jako hosta samodzielnego ze względu na wydajność, zaleca się również testu, aplikacji hostowanych w usługach IIS, aby określić korzyści w zakresie wydajności.

Ten samouczek zawiera następujące sekcje:

- [Tworzenie serwera](#server)
- [Dostęp do serwera przy użyciu klienta języka JavaScript](#js)

<a id="server"></a>

## <a name="creating-the-server"></a>Tworzenie serwera

W tym samouczku utworzysz serwer, który znajduje się w aplikacji konsoli, ale serwer może być hostowana w dowolny rodzaj procesu, takie jak Windows, usługi lub roli procesu roboczego platformy Azure. Przykładowy kod do obsługi serwera SignalR, w usłudze Windows, zobacz [Self-Hosting SignalR w usłudze Windows](https://code.msdn.microsoft.com/SignalR-self-hosted-in-6ff7e6c3).

1. Otwórz program Visual Studio 2013 z uprawnieniami administratora. Wybierz **pliku**, **nowy projekt**. Wybierz **Windows** w obszarze **Visual C#** w węźle **szablony** okienka, a następnie wybierz **aplikację Konsolową** szablonu. Nazwa nowego projektu "SignalRSelfHost", a następnie kliknij przycisk **OK**.

    ![](tutorial-signalr-self-host/_static/image1.png)
2. Otwórz konsolę Menedżera pakietów biblioteki, wybierając **narzędzia**, **Menedżer pakietów biblioteki**, **Konsola Menedżera pakietów**.
3. W konsoli Menedżera pakietów wpisz następujące polecenie:

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample1.ps1)]

    To polecenie dodaje biblioteki SignalR 2 Self-Host do projektu.
4. W konsoli Menedżera pakietów wpisz następujące polecenie:

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample2.ps1)]

    To polecenie dodaje bibliotekę Microsoft.Owin.Cors do projektu. Ta biblioteka będzie służyć do obsługi między domenami, który jest wymagany dla aplikacji, które hostują SignalR i strony sieci web klienta w różnych domenach. Ponieważ będziesz hostować, SignalR serwera i klienta sieci web na innym, oznacza to, że ten między domenami musi być włączona dla komunikacji między tymi składnikami.
5. Zastąp zawartości Program.cs następującym kodem.

    [!code-csharp[Main](tutorial-signalr-self-host/samples/sample3.cs)]

    Powyższy kod obejmuje trzy klasy:

    - **Program**, w tym **Main** Definiowanie ścieżki podstawowej wykonywania metody. W przypadku tej metody, aplikacji sieci web typu **uruchamiania** została uruchomiona na określony adres URL (`http://localhost:8080`). Jeśli zabezpieczeń jest wymagany w punkcie końcowym, można zaimplementować protokół SSL. Zobacz [porady: Konfigurowanie portu z certyfikatem SSL](https://msdn.microsoft.com/library/ms733791.aspx) Aby uzyskać więcej informacji.
    - **Uruchamianie**, klasa zawierająca konfiguracje dla serwera biblioteki SignalR (konfiguracji tylko w tym samouczku używany jest wywołanie `UseCors`) i wywołanie `MapSignalR`, tworzy trasy dla obiektów Centrum w projekcie.
    - **MyHub**, klasa Centrum SignalR, zapewniające aplikacji dla klientów. Ta klasa zawiera jedną metodę, **wysyłania**, że klienci wywoła wysyłać wiadomość do wszystkich innych połączonych klientów.
6. Skompilować i uruchomić aplikację. Adres, który jest uruchomiony serwer powinien być wyświetlony w oknie konsoli.

    ![](tutorial-signalr-self-host/_static/image2.png)
7. Jeśli wykonanie nie powiedzie się z wyjątkiem `System.Reflection.TargetInvocationException was unhandled`, konieczne będzie ponowne uruchomienie programu Visual Studio z uprawnieniami administratora.
8. Zatrzymaj aplikację przed przejściem do następnej sekcji.

<a id="js"></a>

## <a name="accessing-the-server-with-a-javascript-client"></a>Dostęp do serwera przy użyciu klienta języka JavaScript

W tej sekcji użyjesz tego samego klienta JavaScript z [samouczka Wprowadzenie](../getting-started/tutorial-getting-started-with-signalr.md). Wybierzemy jedną modyfikację do klienta, która umożliwia jawne zdefiniowanie URL koncentratora. Przy użyciu samodzielnie hostowanej aplikacji serwer może nie koniecznie w ten sam adres co adres URL połączenia (z powodu serwery proxy i moduły równoważenia obciążenia), więc adres URL musi być jawnie zdefiniowane.

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy rozwiązanie i wybierz **Dodaj**, **nowy projekt**. Wybierz **Web** , a następnie wybierz węzeł **aplikacji sieci Web ASP.NET** szablonu. Nadaj projektowi nazwę "JavascriptClient", a następnie kliknij przycisk **OK**.

    ![](tutorial-signalr-self-host/_static/image3.png)
2. Wybierz **pusty** szablonu i pozostaw niezaznaczoną pozostałe opcje. Wybierz **Utwórz projekt**.

    ![](tutorial-signalr-self-host/_static/image4.png)
3. W konsoli Menedżera pakietów, wybierz projekt "JavascriptClient" **projekt domyślny** listy rozwijanej, a następnie wykonaj następujące polecenie:

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample4.ps1)]

    To polecenie instaluje biblioteki SignalR i JQuery, które będą potrzebne w kliencie.
4. Kliknij prawym przyciskiem myszy na projekt i wybierz **Dodaj**, **nowy element**. Wybierz **Web** węzłem, a następnie wybierz stronę HTML. Nazwij stronę **Default.html**.

    ![](tutorial-signalr-self-host/_static/image5.png)
5. Zastąp zawartość strony HTML z następującym kodem. Upewnij się, że odwołania do skryptu w tym miejscu są takie same skryptów w folderze skryptów projektu.

    [!code-html[Main](tutorial-signalr-self-host/samples/sample5.html?highlight=31-32)]

    Poniższy kod (wyróżnione w powyższym przykładowym kodzie) jest dodanie zgłaszający klienta używanego w tym samouczku wprowadzenie Stared (oprócz uaktualnienie kodu do SignalR w wersji 2 beta). Ten wiersz kodu jawnie ustawia adres URL podstawowego połączenia dla elementu SignalR na serwerze.

    [!code-javascript[Main](tutorial-signalr-self-host/samples/sample6.js)]
6. Kliknij prawym przyciskiem myszy rozwiązanie, a następnie wybierz pozycję **Ustaw projekty startowe...** . Wybierz **wiele projektów startowych** przycisk radiowy, a następnie ustaw oba projekty **akcji** do **Start**.

    ![](tutorial-signalr-self-host/_static/image6.png)
7. Kliknij prawym przyciskiem myszy na "Default.html", a następnie wybierz pozycję **Ustaw jako stronę startową**.
8. Uruchom aplikację. Zostanie uruchomiony serwer i strony. Może być konieczne ponowne załadowanie strony sieci web (lub wybierz **Kontynuuj** w debugerze) Jeśli strona ładuje się przed rozpoczęciem serwera.
9. W przeglądarce należy podać nazwę użytkownika po wyświetleniu monitu. Skopiuj adres URL strony w innej karcie przeglądarki lub w oknie i podaj inną nazwę użytkownika. Będzie mogła wysyłać wiadomości z okienka jednej przeglądarki do innego, tak jak samouczka Wprowadzenie.
