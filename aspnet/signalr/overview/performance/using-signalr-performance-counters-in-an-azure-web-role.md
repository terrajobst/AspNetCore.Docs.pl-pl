---
uid: signalr/overview/performance/using-signalr-performance-counters-in-an-azure-web-role
title: "Za pomocą liczników wydajności SignalR w roli sieci Web platformy Azure | Dokumentacja firmy Microsoft"
author: guardrex
description: "Jak zainstalować i używać liczniki wydajności SignalR w roli sieci Web platformy Azure."
keywords: Licznik ASP.NET,signalr,Performance, roli sieci web platformy azure
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/11/2017
ms.topic: article
ms.assetid: 2a127d3b-21ed-4cc9-bec0-cdab4e742a25
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/performance/using-signalr-performance-counters-in-an-azure-web-role
msc.type: authoredcontent
ms.openlocfilehash: 0d2717eb318d282e21e9aa8622a205f556e3a4ee
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
# <a name="using-signalr-performance-counters-in-an-azure-web-role"></a>Za pomocą liczników wydajności SignalR w roli sieci Web Azure

Przez [Luke Latham](https://github.com/guardrex)

Liczniki wydajności SignalR są używane do monitorowania wydajności aplikacji w roli sieci Web platformy Azure. Liczniki są przechwytywane przez Microsoft Azure Diagnostics. Zainstaluj liczniki wydajności SignalR na platformie Azure za pomocą *signalr.exe*, tego samego narzędzia użyta do aplikacji autonomicznej lub lokalnie. Ponieważ role Azure są przejściowe, należy skonfigurować aplikację do zainstalowania i zarejestrowania liczniki wydajności SignalR podczas uruchamiania.

## <a name="prerequisites"></a>Wymagania wstępne

* [Visual Studio 2015](https://www.visualstudio.com/vs/visual-studio-express/)
* [Microsoft Azure SDK dla programu Visual Studio 2015 (VS2015)](https://azure.microsoft.com/downloads/) **Uwaga: ponowne uruchomienie komputera po zainstalowaniu zestawu SDK.**
* Subskrypcja Microsoft Azure: w celu uzyskania bezpłatnego konta wersji próbnej platformy Azure, zobacz [Azure bezpłatnej wersji próbnej](https://azure.microsoft.com/free/).

## <a name="creating-an-azure-web-role-application-that-exposes-signalr-performance-counters"></a>Tworzenie aplikacji roli sieci Web platformy Azure, która przedstawia liczniki wydajności SignalR

1. Otwórz program Visual Studio 2015.

2. W programie Visual Studio 2015, wybierz **pliku &gt; nowy &gt; projektu**.

3. W **szablony** okienku **nowy projekt** w obszarze **Visual C#** węzła, wybierz opcję **chmury** węzeł i wybierz **Usługi w chmurze azure** szablonu. Nazwa aplikacji **SignalRPerfCounters** i wybierz **OK**.

   ![Nowych aplikacji w chmurze](using-signalr-performance-counters-in-an-azure-web-role/_static/image1.png)
    
4. W **nową usługę w chmurze Azure Microsoft** okno dialogowe, wybierz opcję **roli sieci Web ASP.NET** i wybierz  **&gt;**  przycisk, aby dodać rolę do projektu. Wybierz **OK**.

   ![Dodaj rolę sieci Web ASP.NET](using-signalr-performance-counters-in-an-azure-web-role/_static/image2.png)
    
5. W **nowej aplikacji sieci Web ASP.NET - WebRole1** okno dialogowe, wybierz opcję **MVC** szablonu, a następnie wybierz **OK**.

   ![Dodawanie interfejsu API sieci Web i MVC](using-signalr-performance-counters-in-an-azure-web-role/_static/image3.png)
    
6. W **Eksploratora rozwiązań**, otwórz *diagnostics.wadcfgx* plików w obszarze **WebRole1**.

   ![Diagnostics.wadcfgx Eksploratora rozwiązań](using-signalr-performance-counters-in-an-azure-web-role/_static/image4.png)
    
7. Zastąp zawartość pliku o następującej konfiguracji i Zapisz plik:

   [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample1.xml)]
    
8. Otwórz **Konsola Menedżera pakietów** z **narzędzia &gt; Menedżera pakietów NuGet**. Wprowadź następujące polecenia, aby zainstalować najnowszą wersję SignalR i SignalR pakietu narzędzi:

   [!code-powershell[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample2.ps1)]
    
9. Konfiguruje aplikację do zainstalowania liczniki wydajności SignalR do wystąpienia roli, podczas uruchamiania lub odtwarzana. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **WebRole1** projekt i wybierz **Dodaj &gt; nowy Folder**. Nazwa nowego folderu *uruchamiania*.

   ![Dodaj Folder początkowy](using-signalr-performance-counters-in-an-azure-web-role/_static/image5.png)
    
10. Kopiuj *signalr.exe* pliku (dodany ze **Microsoft.AspNet.SignalR.Utils** pakietu) z  **&lt;folderu projektu&gt;\SignalRPerfCounters\packages\ Microsoft.AspNet.SignalR.Utils. &lt;wersji&gt;\tools** do *uruchamiania* folder utworzony w poprzednim kroku.

11. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *uruchamiania* i wybierz polecenie **Dodaj &gt; istniejący element**. W wyświetlonym oknie dialogowym wybierz *signalr.exe* i wybierz **Dodaj**.

    ![Dodaj signalr.exe do projektu](using-signalr-performance-counters-in-an-azure-web-role/_static/image6.png)
    
12. Kliknij prawym przyciskiem myszy *uruchamiania* utworzony folder. Wybierz **dodać &gt; nowy element**. Wybierz **ogólne** węzła, wybierz opcję **pliku tekstowego**oraz nazwę nowego elementu *SignalRPerfCounterInstall.cmd*. Ten plik polecenia instaluje liczniki wydajności SignalR do roli sieci web.

    ![Utwórz plik wsadowy instalacji liczników wydajności SignalR](using-signalr-performance-counters-in-an-azure-web-role/_static/image7.png)
     
13. Gdy program Visual Studio tworzy *SignalRPerfCounterInstall.cmd* pliku, zostanie automatycznie otwarty w oknie głównym. Zamień zawartość pliku następujący skrypt, a następnie zapisz i zamknij plik. Ten skrypt jest wykonywany *signalr.exe*, która dodaje do wystąpienia roli liczniki wydajności SignalR.

    [!code-console[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample3.cmd)]
    
14. Wybierz *signalr.exe* w pliku **Eksploratora rozwiązań**. W pliku **właściwości**ustaw **Kopiuj do katalogu wyjściowego** do **zawsze Kopiuj**.

    ![Ustaw Kopiuj do katalogu wyjściowego do skopiowania zawsze](using-signalr-performance-counters-in-an-azure-web-role/_static/image8.png)
    
15. Powtórz poprzedni krok dla *SignalRPerfCounterInstall.cmd* pliku.

    
16. Kliknij prawym przyciskiem myszy *SignalRPerfCounterInstall.cmd* plik i wybierz **Otwórz za pomocą**. W wyświetlonym oknie dialogowym wybierz **edytora plików binarnych** i wybierz **OK**.

    ![Otwórz z edytora plików binarnych](using-signalr-performance-counters-in-an-azure-web-role/_static/image9.png)
    
17. W edytorze binarnym wybierz wszystkie bajty wiodące w pliku i usuń je. Zapisz i zamknij plik.

    ![Usuń bajty wiodące](using-signalr-performance-counters-in-an-azure-web-role/_static/image10.png)
    
18. Otwórz *ServiceDefinition.csdef* i Dodaj zadanie uruchamiania, która wykonuje *SignalrPerfCounterInstall.cmd* pliku podczas uruchamiania usługi:

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample4.xml?highlight=4-7)]
    
19. Otwórz `Views/Shared/_Layout.cshtml` i usunąć skrypt pakiet jQuery na końcu pliku.

    [!code-cshtml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample5.cshtml)]
    
20. Dodaj klienta JavaScript, który wywołuje stale `increment` metody na serwerze. Otwórz `Views/Home/Index.cshtml` i Zastąp zawartość następującym kodem:

    [!code-cshtml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample6.cshtml)]
    
21. Utwórz nowy folder w **WebRole1** projektu o nazwie *koncentratory*. Kliknij prawym przyciskiem myszy *koncentratory* folderu w **Eksploratora rozwiązań**, wybierz pozycję **Web &gt; SignalR**i wybierz **klasy koncentratora SignalR (v2)**. Nazwa nowego centrum *MyHub.cs* i wybierz **Dodaj**.

    ![Dodawanie klasy koncentratora SignalR do folderu koncentratory, w oknie dialogowym Dodawanie nowego elementu](using-signalr-performance-counters-in-an-azure-web-role/_static/image13.png)

22. *MyHub.cs* zostanie automatycznie otwarte w oknie głównym. Zastąp zawartość następującym kodem, a następnie zapisz i zamknij plik:

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample7.cs)]
    
23. *[Crank.exe](signalr-connection-density-testing-with-crank.md)*  to połączenie testowania Narzędzie dostarczone z SignalR codebase. Ponieważ węzłówką wymaga połączenie trwałe, możesz dodać je do lokacji do użytku podczas testowania. Dodaj nowy folder w celu **WebRole1** projektu o nazwie *PersistentConnections*. Kliknij prawym przyciskiem myszy folder, a następnie wybierz **Dodaj &gt; klasy**. Nazwa nowego pliku klasy *MyPersistentConnections.cs* i wybierz **Dodaj**.

24. Zostanie otwarty program Visual Studio *MyPersistentConnections.cs* pliku w oknie głównym. Zastąp zawartość następującym kodem, a następnie zapisz i zamknij plik:

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample8.cs)]
    
25. Przy użyciu `Startup` klas, obiektów SignalR uruchomić po uruchomieniu OWIN. Otwarcia lub utworzenia *Startup.cs* i Zastąp zawartość następującym kodem:

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample9.cs)]
    
    W powyższym kodzie `OwinStartup` atrybut oznacza tej klasy, aby uruchomić OWIN. `Configuration` Metoda uruchamia SignalR.
    
26. Przetestować aplikację w emulatorze Microsoft Azure przez naciśnięcie przycisku **F5**.

    > [!NOTE]
    > Jeśli wystąpią **fileloadexception —** w **MapSignalR**, zmień przekierowania powiązania w *web.config* do następującego:

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample12.xml?highlight=3,7)]
    
27. Poczekaj około jednej minuty. Otwórz okno narzędzia Eksplorator chmury w programie Visual Studio (**widoku &gt; Eksplorator chmury**) i Rozwiń ścieżkę `(Local)\Storage Accounts\(Development)\Tables`. Kliknij dwukrotnie **WADPerformanceCountersTable**. Powinny pojawić się liczników SignalR w danych tabeli. Jeśli nie widzisz tabeli może być konieczne ponowne wprowadzenie poświadczeń usługi Azure Storage. Musisz wybrać **Odśwież** przycisk, aby wyświetlić tabelę w **Eksplorator chmury** lub wybierz **Odśwież** przycisk w oknie Otwórz tabelę, aby wyświetlić dane w tabeli.

    ![Wybranie tabeli liczniki wydajności WAD w programie Visual Studio Cloud Explorer](using-signalr-performance-counters-in-an-azure-web-role/_static/image11.png)

    ![Wyświetlanie liczniki zbierane w tabeli liczniki wydajności WAD](using-signalr-performance-counters-in-an-azure-web-role/_static/image12.png)
    
28. Aby przetestować aplikację w chmurze, należy zaktualizować **ServiceConfiguration.Cloud.cscfg** pliku i ustaw `Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString` do prawidłowego ciągu połączenia konta magazynu Azure.

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample10.xml)]

29. Wdrażanie aplikacji do subskrypcji platformy Azure. Aby uzyskać więcej informacji na temat sposobu wdrażania aplikacji na platformie Azure, zobacz [sposobu tworzenia i wdrażania usługi w chmurze](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).

30. Poczekaj kilka minut. W **Eksplorator chmury**, Znajdź konto magazynu skonfigurowanych powyżej i Znajdź `WADPerformanceCountersTable` tabeli w nim. Powinny pojawić się liczników SignalR w danych tabeli. Jeśli nie widzisz tabeli może być konieczne ponowne wprowadzenie poświadczeń usługi Azure Storage. Musisz wybrać **Odśwież** przycisk, aby wyświetlić tabelę w **Eksplorator chmury** lub wybierz **Odśwież** przycisk w oknie Otwórz tabelę, aby wyświetlić dane w tabeli.

Specjalne podziękowania dla [Richard pole](https://social.msdn.microsoft.com/profile/Martin+Richard) oryginalnej zawartości używanych w tym samouczku.
