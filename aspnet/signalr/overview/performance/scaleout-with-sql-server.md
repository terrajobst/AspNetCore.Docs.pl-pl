---
uid: signalr/overview/performance/scaleout-with-sql-server
title: SignalR — skalowanie w poziomie z programem SQL Server | Dokumentacja firmy Microsoft
author: MikeWasson
description: Wersje oprogramowania, używaną w tym temacie dodatku .NET 4.5 SignalR dla programu Visual Studio 2013 w wersji 2, poprzednie wersje w tym temacie informacji o wcześniejszych wersjach...
ms.author: riande
ms.date: 06/10/2014
ms.assetid: 98358b6e-9139-4239-ba3a-2d7dd74dd664
msc.legacyurl: /signalr/overview/performance/scaleout-with-sql-server
msc.type: authoredcontent
ms.openlocfilehash: 7d9acfc8ed2dd692f36528a8a9700d92d86a4203
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/10/2018
ms.locfileid: "48910450"
---
<a name="signalr-scaleout-with-sql-server"></a>SignalR — skalowanie w poziomie z programem SQL Server
====================
przez [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)

> ## <a name="software-versions-used-in-this-topic"></a>Wersje oprogramowania używaną w tym temacie
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - .NET 4.5
> - SignalR w wersji 2
>
>
>
> ## <a name="previous-versions-of-this-topic"></a>Poprzednie wersje tego tematu
>
> Aby uzyskać informacje dotyczące starszych wersji biblioteki SignalR, zobacz [starsze wersje biblioteki SignalR](../older-versions/index.md).
>
> ## <a name="questions-and-comments"></a>Pytania i komentarze
>
> Jak się podoba w tym samouczku, i co można było ulepszyć proces w komentarzach u dołu strony, wystaw opinię. Jeśli masz pytania, na które nie są bezpośrednio związane z tego samouczka, możesz zamieścić je do [forum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) lub [StackOverflow.com](http://stackoverflow.com/).


W tym samouczku użyjesz programu SQL Server, aby dystrybuować komunikaty do aplikacji SignalR, które zostało wdrożone w dwóch osobnych wystąpień usługi IIS. W tym samouczku można również uruchomić na maszynie jeden test, ale aby uzyskać pełny wpływ, należy wdrożyć aplikacji SignalR do co najmniej dwóch serwerów. SQL Server należy również zainstalować na jednym serwerze lub na oddzielnym serwerze dedykowanym. Innym rozwiązaniem jest uruchamianie samouczek przy użyciu maszyn wirtualnych na platformie Azure.

![](scaleout-with-sql-server/_static/image1.png)

## <a name="prerequisites"></a>Wymagania wstępne

Microsoft SQL Server 2005 lub nowszego. Systemu backplane obsługuje wersje komputerach stacjonarnych, jak i serwera programu SQL Server. Nie obsługuje programu SQL Server Compact Edition lub Azure SQL Database. (Jeśli aplikacja jest hostowana na platformie Azure, należy wziąć pod uwagę płyty montażowej usługi Service Bus zamiast.)

## <a name="overview"></a>Omówienie

Przed przejściem do szczegółowe podręcznika, poniżej przedstawiono krótkie podsumowanie będzie wykonywać.

1. Tworzenie nowej pustej bazy danych. Systemu backplane utworzy niezbędne tabele w tej bazie danych.
2. Dodaj te pakiety NuGet do aplikacji:

    - [Microsoft.AspNet.SignalR](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [Microsoft.AspNet.SignalR.SqlServer](http://nuget.org/packages/Microsoft.AspNet.SignalR.SqlServer)
3. Tworzenie aplikacji SignalR.
4. Dodaj następujący kod do pliku Startup.cs w celu skonfigurowania systemu backplane:

    [!code-csharp[Main](scaleout-with-sql-server/samples/sample1.cs)]

   Ten kod konfiguruje systemu backplane z wartościami domyślnymi dla [TableCount](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.sqlscaleoutconfiguration.tablecount(v=vs.118).aspx) i [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx). Aby uzyskać informacje na temat zmiany tych wartości, zobacz [wydajność SignalR: metryki skalowania](signalr-performance.md#scaleout_metrics).

## <a name="configure-the-database"></a>Skonfiguruj bazę danych

Zdecyduj, czy aplikacja użyje uwierzytelniania Windows lub uwierzytelniania programu SQL Server dostęp do bazy danych. Niezależnie od tego upewnij się, że użytkownik bazy danych ma uprawnienia do zalogować, Utwórz schematy i tworzenie tabel.

Utwórz nową bazę danych do płyty montażowej do użycia. Bazy danych można nadać dowolną nazwę. Nie trzeba tworzyć żadnych tabel w bazie danych. Spowoduje to utworzenie niezbędne tabele systemu backplane.

![](scaleout-with-sql-server/_static/image2.png)

## <a name="enable-service-broker"></a>Włącz brokera usług

Zalecane jest, aby włączyć brokera usługi dla bazy danych systemu backplane. Usługa Service Broker zapewnia macierzystą obsługę komunikatów i kolejkowania w SQL Server, która umożliwia bardziej wydajnie otrzymywać aktualizacje systemu backplane. (Jednak systemu backplane również działa bez programu Service Broker.)

Aby sprawdzić, czy programu Service Broker jest włączona, należy zbadać **jest\_brokera\_włączone** kolumny w **sys.databases** widok katalogu.

[!code-sql[Main](scaleout-with-sql-server/samples/sample2.sql)]

![](scaleout-with-sql-server/_static/image3.png)

Aby włączyć programu Service Broker, użyj następującego zapytania SQL:

[!code-sql[Main](scaleout-with-sql-server/samples/sample3.sql)]

> [!NOTE]
> Jeśli wydaje się zakleszczenie, upewnij się, to zapytanie nie istnieją żadne aplikacje połączone z bazą danych.


Po włączeniu śledzenia śledzenia również pokaże czy programu Service Broker jest włączona.

## <a name="create-a-signalr-application"></a>Tworzenie aplikacji SignalR

Tworzenie aplikacji SignalR, wykonując dowolną z następujących samouczków:

- [Wprowadzenie do SignalR w wersji 2.0](../getting-started/tutorial-getting-started-with-signalr.md)
- [Wprowadzenie do SignalR w wersji 2.0 i MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md)

Następnie zmodyfikujemy aplikacji rozmów w celu obsługi skalowania w poziomie z programem SQL Server. Najpierw Dodaj pakiet SignalR.SqlServer NuGet do projektu. W programie Visual Studio z **narzędzia** menu, wybierz opcję **Menedżera pakietów NuGet**, a następnie wybierz **Konsola Menedżera pakietów**. W oknie Konsola Menedżera pakietów wprowadź następujące polecenie:

[!code-powershell[Main](scaleout-with-sql-server/samples/sample4.ps1)]

Następnie otwórz plik Startup.cs. Dodaj następujący kod do **Konfiguruj** metody:

[!code-csharp[Main](scaleout-with-sql-server/samples/sample5.cs)]

## <a name="deploy-and-run-the-application"></a>Wdrażanie i uruchamianie aplikacji

Przygotowanie do wdrożenia aplikacji SignalR wystąpień systemu Windows Server.

Dodaj rolę usług IIS. Obejmują funkcje "Opracowywanie zawartości dla aplikacji", w tym protokołu WebSocket.

![](scaleout-with-sql-server/_static/image4.png)

Również obejmować usługę zarządzania (wymienione w obszarze "Narzędzia do zarządzania").

![](scaleout-with-sql-server/_static/image5.png)

**Zainstaluj narzędzie Web Deploy 3.0.** Podczas uruchamiania Menedżera usług IIS, zostanie wyświetlony monit o zainstalowanie platformy sieci Web firmy Microsoft lub możesz [Pobierz intstaller](https://go.microsoft.com/fwlink/?LinkId=255386). W Instalatorze platformy wyszukiwania dla narzędzia Web Deploy i zainstalować Web Deploy 3.0

![](scaleout-with-sql-server/_static/image6.png)

Sprawdź, czy usługa zarządzania sieci Web jest uruchomiona. Jeśli nie, uruchom usługę. (Jeśli nie widzisz usługi zarządzania siecią Web, na liście usług Windows, upewnij się, że po dodaniu roli usług IIS są zainstalowane usługi zarządzania.)

Wreszcie Otwórz port 8172 dla protokołu TCP. Jest to port, który korzysta z narzędzia Web Deploy.

Teraz wszystko jest gotowe do wdrożenia projektu programu Visual Studio z komputera deweloperskiego z serwerem. W Eksploratorze rozwiązań kliknij rozwiązanie prawym przyciskiem myszy, a następnie kliknij przycisk **Publikuj**.

Aby uzyskać bardziej szczegółową dokumentację na temat wdrażania w Internecie, zobacz [zawartości mapy wdrażania w sieci Web dla programu Visual Studio i platformy ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).

W przypadku wdrożenia aplikacji na dwóch serwerach, można otworzyć każde wystąpienie w osobnym oknie przeglądarki i zobacz ich odbierania komunikatów SignalR w innej. (Oczywiście w środowisku produkcyjnym, dwa serwery będą znajdują się za modułem równoważenia obciążenia.)

![](scaleout-with-sql-server/_static/image7.png)

Po uruchomieniu aplikacji, możesz zobaczyć, że SignalR automatycznie utworzył tabele w bazie danych:

![](scaleout-with-sql-server/_static/image8.png)

SignalR zarządza tabelami. Tak długo, jak Twoja aplikacja zostanie wdrożona, nie usunąć wiersze, zmodyfikować tabeli i tak dalej.
