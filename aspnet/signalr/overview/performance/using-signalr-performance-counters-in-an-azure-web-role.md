---
uid: signalr/overview/performance/using-signalr-performance-counters-in-an-azure-web-role
title: Korzystanie z liczników wydajności SignalR w roli sieci Web platformy Azure | Dokumentacja firmy Microsoft
author: guardrex
description: Jak zainstalować i korzystać z liczników wydajności SignalR w roli sieci Web platformy Azure.
keywords: Licznik ASP.NET,signalr,Performance, roli sieci web platformy azure
ms.author: riande
ms.date: 02/11/2017
ms.assetid: 2a127d3b-21ed-4cc9-bec0-cdab4e742a25
msc.legacyurl: /signalr/overview/performance/using-signalr-performance-counters-in-an-azure-web-role
msc.type: authoredcontent
ms.openlocfilehash: acc9836535466a801f1f46ec18d05937e2e42af2
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754738"
---
# <a name="using-signalr-performance-counters-in-an-azure-web-role"></a>Korzystanie z liczników wydajności SignalR w roli sieci Web platformy Azure

Przez [Luke Latham](https://github.com/guardrex)

Liczniki wydajności SignalR są używane do monitorowania wydajności aplikacji w roli sieci Web platformy Azure. Liczniki są przechwytywane przez Microsoft Azure Diagnostics. Instalowanie liczników wydajności SignalR na platformie Azure dzięki *signalr.exe*, tego samego narzędzia, które są używane dla aplikacji autonomicznych lub w środowisku lokalnym. Ponieważ przejściowy ról platformy Azure, możesz skonfigurować aplikację tak, aby zainstalować i zarejestrować liczników wydajności SignalR podczas uruchamiania.

## <a name="prerequisites"></a>Wymagania wstępne

* [Visual Studio 2015](https://www.visualstudio.com/vs/visual-studio-express/)
* [Zestaw Microsoft Azure SDK dla programu Visual Studio 2015 (VS2015)](https://azure.microsoft.com/downloads/) **Uwaga: ponowne uruchomienie komputera po zainstalowaniu zestawu SDK.**
* Subskrypcja Microsoft Azure: aby Załóż bezpłatne konto wersji próbnej platformy Azure, zobacz [bezpłatna wersja próbna platformy Azure](https://azure.microsoft.com/free/).

## <a name="creating-an-azure-web-role-application-that-exposes-signalr-performance-counters"></a>Tworzenie aplikacji w roli sieci Web platformy Azure, który udostępnia liczników wydajności SignalR

1. Otwórz program Visual Studio 2015.

2. W programie Visual Studio 2015 wybierz **pliku** > **New** > **projektu**.

3. W **szablony** okienku **nowy projekt** okna w obszarze **Visual C#** węzeł **chmury** węzeł i wybierz pozycję **Usługa w chmurze** szablonu. Określanie nazwy aplikacji **SignalRPerfCounters** i wybierz **OK**.

   ![Nowa aplikacja w chmurze](using-signalr-performance-counters-in-an-azure-web-role/_static/image1.png)
    
4. W **nową usługę w chmurze Azure Microsoft** okno dialogowe, wybierz opcję **Role sieci Web ASP.NET** i wybierz > przycisk, aby dodać rolę do projektu. Wybierz **OK**.

   ![Dodaj rolę sieci Web platformy ASP.NET](using-signalr-performance-counters-in-an-azure-web-role/_static/image2.png)
    
5. W **nowej aplikacji sieci Web ASP.NET - WebRole1** okno dialogowe, wybierz opcję **MVC** szablonu, a następnie wybierz **OK**.

   ![Dodawanie MVC i Web API](using-signalr-performance-counters-in-an-azure-web-role/_static/image3.png)
    
6. W **Eksploratora rozwiązań**, otwórz *diagnostics.wadcfgx* plik **WebRole1**.

   ![Diagnostics.wadcfgx Eksploratora rozwiązań](using-signalr-performance-counters-in-an-azure-web-role/_static/image4.png)
    
7. Zastąp zawartość pliku o następującej konfiguracji, a następnie zapisz plik:

   [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample1.xml)]
    
8. Otwórz **Konsola Menedżera pakietów** z **narzędzia** > **Menedżera pakietów NuGet**. Wprowadź następujące polecenia, aby zainstalować najnowszą wersję biblioteki SignalR i pakietu Narzędzia SignalR:

   [!code-powershell[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample2.ps1)]
    
9. Skonfigurować aplikację można zainstalować liczników wydajności SignalR w wystąpieniu roli, podczas uruchamiania lub odtwarzania. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **WebRole1** projektu, a następnie wybierz **Dodaj** > **nowy Folder**. Nadaj nazwę nowego folderu *uruchamiania*.

   ![Dodaj Folder początkowy](using-signalr-performance-counters-in-an-azure-web-role/_static/image5.png)
    
10. Kopiuj *signalr.exe* pliku (dodane za pomocą **Microsoft.AspNet.SignalR.Utils** pakietu) z \<folderu projektu > / SignalRPerfCounters/packages/Microsoft.AspNet.SignalR.Utils.\< w wersji > / narzędzi do *uruchamiania* folderu utworzonego w poprzednim kroku.

11. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *uruchamiania* i wybierz polecenie **Dodaj** > **istniejący element**. W wyświetlonym oknie dialogowym wybierz *signalr.exe* i wybierz **Dodaj**.

    ![Dodaj signalr.exe do projektu](using-signalr-performance-counters-in-an-azure-web-role/_static/image6.png)
    
12. Kliknij prawym przyciskiem myszy *uruchamiania* utworzony folder. Wybierz **Dodaj** > **nowy element**. Wybierz **ogólne** węzeł **plik tekstowy**i nadaj nazwę nowego elementu *SignalRPerfCounterInstall.cmd*. Ten plik polecenia zainstaluje liczników wydajności SignalR w roli sieci web.

    ![Utwórz plik wsadowy instalacji liczników wydajności SignalR](using-signalr-performance-counters-in-an-azure-web-role/_static/image7.png)
     
13. Gdy program Visual Studio tworzy *SignalRPerfCounterInstall.cmd* pliku, zostanie automatycznie otwarty w głównym oknie. Zastąp zawartość pliku następującym skryptem, a następnie zapisz i zamknij plik. Ten skrypt jest wykonywany *signalr.exe*, która dodaje liczników wydajności SignalR do wystąpienia roli.

    [!code-console[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample3.cmd)]
    
14. Wybierz *signalr.exe* w pliku **Eksploratora rozwiązań**. W pliku **właściwości**ustaw **Kopiuj do katalogu wyjściowego** do **zawsze Kopiuj**.

    ![Ustaw Kopiuj do katalogu wyjściowego, aby zawsze Kopiuj](using-signalr-performance-counters-in-an-azure-web-role/_static/image8.png)
    
15. Powtórz poprzedni krok dla *SignalRPerfCounterInstall.cmd* pliku.

    
16. Kliknij prawym przyciskiem myszy *SignalRPerfCounterInstall.cmd* plik i wybierz **Otwórz za pomocą**. W wyświetlonym oknie dialogowym wybierz **Edytor plików binarnych** i wybierz **OK**.

    ![Otwórz za pomocą edytora pliku binarnego](using-signalr-performance-counters-in-an-azure-web-role/_static/image9.png)
    
17. W edytorze binarnym zaznacz wszystkie wiodące bajty w pliku i usuń je. Zapisz i zamknij plik.

    ![Usuń bajty wiodące](using-signalr-performance-counters-in-an-azure-web-role/_static/image10.png)
    
18. Otwórz *ServiceDefinition.csdef* i Dodaj zadanie uruchamiania, który jest wykonywany *SignalrPerfCounterInstall.cmd* pliku podczas uruchamiania usługi:

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample4.xml?highlight=4-7)]
    
19. Otwórz `Views/Shared/_Layout.cshtml` i Usuń skrypt pakietu jQuery od końca pliku.

    [!code-cshtml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample5.cshtml)]
    
20. Dodaj klienta JavaScript, która stale wywołuje `increment` metody na serwerze. Otwórz `Views/Home/Index.cshtml` i zastąp jego zawartość następującym kodem:

    [!code-cshtml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample6.cshtml)]
    
21. Utwórz nowy folder w **WebRole1** projektu o nazwie *koncentratory*. Kliknij prawym przyciskiem myszy *koncentratory* folderu w **Eksploratora rozwiązań**, wybierz opcję **Web** > **SignalR**i wybierz  **Klasa Centrum SignalR (v2)**. Nazwa nowego centrum *MyHub.cs* i wybierz **Dodaj**.

    ![Dodawanie klasy koncentratora SignalR do folderu koncentratory, w oknie dialogowym Dodaj nowy element](using-signalr-performance-counters-in-an-azure-web-role/_static/image13.png)

22. *MyHub.cs* zostanie automatycznie otwarte w głównym oknie. Zastąp zawartość następującym kodem, a następnie zapisz i zamknij plik:

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample7.cs)]
    
23. *[Crank.exe](signalr-connection-density-testing-with-crank.md)*  zagęszczenie połączenia testowania narzędzie dołączonym SignalR codebase. Ponieważ węzłówką wymaga trwałego połączenia, możesz dodać je do witryny do użycia podczas testowania. Dodaj nowy folder w celu **WebRole1** projekt o nazwie *PersistentConnections*. Kliknij prawym przyciskiem myszy ten folder, a następnie wybierz pozycję **Dodaj** > **klasy**. Nadaj nowemu plikowi klasy *MyPersistentConnections.cs* i wybierz **Dodaj**.

24. Zostanie otwarty program Visual Studio *MyPersistentConnections.cs* pliku w oknie głównym. Zastąp zawartość następującym kodem, a następnie zapisz i zamknij plik:

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample8.cs)]
    
25. Za pomocą `Startup` klasy obiektów SignalR rozpoczyna się w momencie uruchamiania OWIN. Otwórz lub Utwórz *Startup.cs* i zastąp jego zawartość następującym kodem:

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample9.cs)]
    
    W powyższym kodzie `OwinStartup` atrybut oznacza tej klasy, aby uruchomić OWIN. `Configuration` Metoda uruchamia SignalR.
    
26. Przetestuj aplikację w emulatorze Microsoft Azure, naciskając klawisz **F5**.

    > [!NOTE]
    > Jeśli napotkasz **fileloadexception —** na **MapSignalR**, zmień przekierowania powiązań w *web.config* do następującego:

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample12.xml?highlight=3,7)]
    
27. Poczekaj około jednej minuty. Otwórz okno narzędzia programu Cloud Explorer w programie Visual Studio (**widoku** > **programu Cloud Explorer**) i Rozwiń ścieżkę `(Local)/Storage Accounts/(Development)/Tables`. Kliknij dwukrotnie **WADPerformanceCountersTable**. Powinien zostać wyświetlony liczników SignalR w danych tabeli. Jeśli nie widzisz tabeli może być konieczne ponownie wprowadzić swoje poświadczenia usługi Azure Storage. Musisz wybrać **Odśwież** przycisk, aby wyświetlić tabelę w **programu Cloud Explorer** lub wybierz **Odśwież** przycisk w oknie Otwórz tabelę, aby wyświetlić dane w tabeli.

    ![Wybieranie tabeli WAD liczników wydajności w programie Cloud Explorer programu Visual Studio](using-signalr-performance-counters-in-an-azure-web-role/_static/image11.png)

    ![Pokazywanie liczników zbieranych w tabeli liczniki wydajności funkcji WAD](using-signalr-performance-counters-in-an-azure-web-role/_static/image12.png)
    
28. Aby przetestować aplikację w chmurze, należy zaktualizować **ServiceConfiguration.Cloud.cscfg** plik i ustaw `Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString` na prawidłowe parametry połączenia konta usługi Azure Storage.

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample10.xml)]

29. Wdrażanie aplikacji do subskrypcji platformy Azure. Aby uzyskać więcej informacji na temat sposobu wdrażania aplikacji na platformie Azure, zobacz [jak utworzyć i wdrożyć usługę w chmurze](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).

30. Poczekaj kilka minut. W **programu Cloud Explorer**Znajdź konto magazynu skonfigurowanych powyżej i Znajdź `WADPerformanceCountersTable` tabeli w nim. Powinien zostać wyświetlony liczników SignalR w danych tabeli. Jeśli nie widzisz tabeli może być konieczne ponownie wprowadzić swoje poświadczenia usługi Azure Storage. Musisz wybrać **Odśwież** przycisk, aby wyświetlić tabelę w **programu Cloud Explorer** lub wybierz **Odśwież** przycisk w oknie Otwórz tabelę, aby wyświetlić dane w tabeli.

Specjalne podziękowania dla [Martin Richard](https://social.msdn.microsoft.com/profile/Martin+Richard) dla oryginalnej zawartości, które są używane w tym samouczku.
