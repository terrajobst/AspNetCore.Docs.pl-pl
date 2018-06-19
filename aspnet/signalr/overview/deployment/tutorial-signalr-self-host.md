---
uid: signalr/overview/deployment/tutorial-signalr-self-host
title: 'Samouczek: SignalR hosta samodzielnego | Dokumentacja firmy Microsoft'
author: pfletcher
description: Ten samouczek pokazuje, jak utworzyć własny hostowanego serwera SignalR 2 oraz sposób nawiązania połączenia z klientem JavaScript. Używane w samouczku V wersje oprogramowania...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 400db427-27af-4f2f-abf0-5486d5e024b5
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/deployment/tutorial-signalr-self-host
msc.type: authoredcontent
ms.openlocfilehash: d38e6fbc3407e4beca6942bbdefcaa8258ebc5ad
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
ms.locfileid: "28036781"
---
<a name="tutorial-signalr-self-host"></a>Samouczek: SignalR Host samodzielny
====================
przez [Patrick Fletcher](https://github.com/pfletcher)

[Pobieranie ukończone projektu](http://code.msdn.microsoft.com/SignalR-Self-Host-Sample-6da0f383)

> Ten samouczek pokazuje, jak utworzyć własny hostowanego serwera SignalR 2 oraz sposób nawiązania połączenia z klientem JavaScript.
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
> ## <a name="questions-and-comments"></a>Pytania i komentarze
> 
> Wystaw opinię na jak zbędne tego samouczka i jakie firma Microsoft może poprawić w komentarze u dołu strony. Jeśli masz pytania, które nie są bezpośrednio związane z tego samouczka możesz zamieścić je do [forum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) lub [StackOverflow.com](http://stackoverflow.com/).


## <a name="overview"></a>Omówienie

Serwer SignalR zazwyczaj znajduje się w aplikacji ASP.NET w usługach IIS, ale może również być hostowanie Samoobsługowe (na przykład w aplikacji konsoli lub usługi systemu Windows) za pomocą biblioteki hosta samodzielnego. Ta biblioteka, podobnie jak wszystkie SignalR 2, w oparciu OWIN ([interfejsu Open Web dla platformy .NET](http://owin.org)). OWIN definiuje abstrakcję między serwerami sieci web .NET i aplikacji sieci web. OWIN oddziela aplikacji sieci web na serwerze, co sprawia, że OWIN idealne rozwiązanie w przypadku samodzielnej obsługi aplikacji sieci web w własnego procesu poza usług IIS.

Nie zawiera w usługach IIS przyczyny:

- Środowiska, w którym usługi IIS nie jest dostępny lub pożądane, takich jak istniejącej farmy serwerów bez usług IIS.
- Zmniejszenie wydajności programu IIS trzeba unikać.
- Funkcja SignalR jest mają zostać dodane do exising aplikację, która działa w usługi systemu Windows, rola procesu roboczego platformy Azure lub inny proces.

Jeśli rozwiązanie jest tworzone jako hosta samodzielnego ze względu na wydajność, zaleca się również testu z aplikacją hostowanych w usługach IIS w celu określenia poprawiać wydajność.

Ten samouczek zawiera następujące sekcje:

- [Tworzenie serwera](#server)
- [Dostęp do serwera z klientem JavaScript](#js)

<a id="server"></a>

## <a name="creating-the-server"></a>Tworzenie serwera

W tym samouczku utworzysz serwer, który znajduje się w aplikacji konsoli, ale serwer może być obsługiwana przez dowolny rodzaj procesu, taką jak usługa systemu Windows lub roli procesu roboczego platformy Azure. Przykładowy kod do obsługi serwera SignalR, w usłudze systemu Windows, temacie [Self-Hosting SignalR w usłudze Windows](https://code.msdn.microsoft.com/SignalR-self-hosted-in-6ff7e6c3).

1. Otwórz program Visual Studio 2013 z uprawnieniami administratora. Wybierz **pliku**, **nowy projekt**. Wybierz **Windows** w obszarze **Visual C#** w węźle **szablony** okienka, a następnie wybierz **aplikacji konsoli** szablonu. Nazwa nowego projektu "SignalRSelfHost", a następnie kliknij przycisk **OK**.

    ![](tutorial-signalr-self-host/_static/image1.png)
2. Otwórz konsolę Menedżera pakietów biblioteki wybierając **narzędzia**, **Menedżer pakietów biblioteki**, **Konsola Menedżera pakietów**.
3. W konsoli Menedżera pakietów wprowadź następujące polecenie:

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample1.ps1)]

    To polecenie dodaje biblioteki SignalR 2 Self-Host do projektu.
4. W konsoli Menedżera pakietów wprowadź następujące polecenie:

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample2.ps1)]

    To polecenie dodaje biblioteki Microsoft.Owin.Cors do projektu. Ta biblioteka będzie służyć do obsługi między domenami, które jest wymagane dla aplikacji, które hosta SignalR i klient strony sieci web w różnych domenach. Ponieważ będzie można obsługiwać SignalR serwera i klienta sieci web na inne porty, oznacza to, że tej domeny — musi być włączona dla komunikacji między tymi składnikami.
5. Zastąp zawartość pliku Program.cs następującym kodem.

    [!code-csharp[Main](tutorial-signalr-self-host/samples/sample3.cs)]

    Powyższy kod obejmuje trzy klasy:

    - **Program**, takie jak **Main** metody Definiowanie ścieżki podstawowej wykonywania. W przypadku tej metody, aplikacji sieci web typu **uruchamiania** została uruchomiona z określonym adresem URL (`http://localhost:8080`). Jeśli zabezpieczeń jest wymagane dla punktu końcowego, można stosować protokół SSL. Zobacz [porady: Konfigurowanie portu z certyfikatem SSL](https://msdn.microsoft.com/library/ms733791.aspx) Aby uzyskać więcej informacji.
    - **Uruchamianie**, klasa zawierająca konfiguracje serwera SignalR (tylko konfiguracja, w tym samouczku używana jest wywołanie `UseCors`) i wywołania w celu `MapSignalR`, co powoduje trasy dla obiektów Centrum w projekcie.
    - **MyHub**, klasy koncentratora SignalR udostępniającą aplikacji do klientów. Ta klasa zawiera tylko jedną metodę **wysyłania**, że klienci wywoła wysyłać wiadomości do wszystkich innych połączonych klientów.
6. Kompilowanie i uruchamianie aplikacji. Adres, który jest uruchomiony serwer powinien być wyświetlony w oknie konsoli.

    ![](tutorial-signalr-self-host/_static/image2.png)
7. Jeśli wykonanie zakończy się niepowodzeniem z wyjątkiem `System.Reflection.TargetInvocationException was unhandled`, konieczne będzie ponowne uruchomienie programu Visual Studio z uprawnieniami administratora.
8. Zatrzymaj aplikację przed kontynuowaniem do następnej sekcji.

<a id="js"></a>

## <a name="accessing-the-server-with-a-javascript-client"></a>Dostęp do serwera z klientem JavaScript

W tej sekcji użyjesz tego samego klienta JavaScript z [Wprowadzenie — samouczek](../getting-started/tutorial-getting-started-with-signalr.md). Tylko wybierzemy modyfikację jednego klienta, który jest jawnie określenie adresu URL koncentratora. Z własnym hostowanej aplikacji serwer może nie koniecznie na ten sam adres URL połączenia (ze względu na odwrotne serwery proxy i moduły równoważenia obciążenia), więc adres URL musi być jawnie zdefiniowane w.

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy w ramach rozwiązania i wybierz **Dodaj**, **nowy projekt**. Wybierz **Web** , a następnie wybierz węzeł **aplikacji sieci Web ASP.NET** szablonu. Nazwij projekt "JavascriptClient", a następnie kliknij przycisk **OK**.

    ![](tutorial-signalr-self-host/_static/image3.png)
2. Wybierz **pusty** szablonu i pozostaw niezaznaczone pozostałe opcje. Wybierz **Utwórz projekt**.

    ![](tutorial-signalr-self-host/_static/image4.png)
3. W konsoli Menedżera pakietów, wybierz projekt "JavascriptClient" **domyślny projekt** listy rozwijanej, a następnie wykonaj następujące polecenie:

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample4.ps1)]

    To polecenie powoduje zainstalowanie biblioteki SignalR i JQuery, które będą potrzebne w kliencie.
4. Kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj**, **nowy element**. Wybierz **Web** węzeł, a następnie wybierz stronę HTML. Nazwa strony **Default.html**.

    ![](tutorial-signalr-self-host/_static/image5.png)
5. Zastąp zawartość strony HTML z następującym kodem. Sprawdź, czy odwołań do skryptów tutaj odpowiadają skryptów w folderze skryptów projektu.

    [!code-html[Main](tutorial-signalr-self-host/samples/sample5.html?highlight=31-32)]

    Poniższy kod (wyróżniony w powyższym przykładowym kodzie) jest wprowadzonych do klienta używany w samouczku pobierania Stared (oprócz uaktualniania kodu do wersji beta programu SignalR w wersji 2). Ten wiersz kodu jawnie ustawia adres URL połączenia podstawowej dla biblioteki SignalR na serwerze.

    [!code-javascript[Main](tutorial-signalr-self-host/samples/sample6.js)]
6. Kliknij prawym przyciskiem myszy w ramach rozwiązania, a następnie wybierz **Ustaw projekty startowe...** . Wybierz **wiele projektów startowych** przycisk radiowy i ustaw oba projekty **akcji** do **Start**.

    ![](tutorial-signalr-self-host/_static/image6.png)
7. Kliknij prawym przyciskiem "Default.html" i wybierz **Ustaw jako stronę startową**.
8. Uruchom aplikację. Spowoduje uruchomienie serwera i strony. Może być konieczne ponowne załadowanie tej strony sieci web (lub wybierz **Kontynuuj** w debugerze) Jeśli strony ładuje zanim serwer jest uruchomiony.
9. W przeglądarce należy podać nazwę użytkownika, po wyświetleniu monitu. Skopiuj adres URL strony do innej karty przeglądarki lub okna i podaj inną nazwę użytkownika. Można wysyłać z okienka w przeglądarce jednego do drugiego, tak jak w samouczku wprowadzenie.
