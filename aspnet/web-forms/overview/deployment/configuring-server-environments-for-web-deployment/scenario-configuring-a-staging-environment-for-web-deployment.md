---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-staging-environment-for-web-deployment
title: 'Scenariusz: Konfigurowanie środowiska przemieszczania na potrzeby wdrażania w sieci Web | Dokumentacja firmy Microsoft'
author: jrjlee
description: W tym temacie opisano scenariusz wdrażania typowych sieci web w środowisku przemieszczania i opisano zadania, które należy wykonać, aby skonfigurować podobne env...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 5a8e49b7-5317-4125-b107-7e2466b47bb3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-staging-environment-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 3864559b0599091beeacb87e90e80a51285039df
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30892325"
---
<a name="scenario-configuring-a-staging-environment-for-web-deployment"></a>Scenariusz: Konfigurowanie środowiska przemieszczania na potrzeby wdrażania w sieci Web
====================
przez [Lewandowski Jason](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> W tym temacie opisano scenariusz wdrażania typowych sieci web w środowisku przemieszczania i opisano zadania, które należy wykonać, aby skonfigurować środowisko podobne.


Wiele organizacji, użyj środowisk przemieszczania, aby przejrzeć aktualizacje do witryn sieci Web lub aplikacji sieci web. Osób w organizacji daje możliwość Eksploruj i Przejrzyj nowe funkcje lub zawartości przed lokacji "live", czy też innymi słowy jest wdrażana w środowisku produkcyjnym. Środowisko przejściowe zaprojektowano w celu replikowania środowiska produkcyjnego możliwie, aby realistyczne podglądu. Tego rodzaju środowisko przejściowe zwykle ma następujące cechy:

- Środowisko składa się z wielu serwerów sieci web z równoważeniem obciążenia i co najmniej jeden serwer bazy danych, często z klastra trybu failover i dublowanie bazy danych.
- Aplikacje mogą wdrażać ręcznie przez zespół deweloperów lub automatycznie przez serwer Team Build.
- Użytkownicy lub kontom proces wdrażania aplikacji są prawdopodobnie nie będzie mieć uprawnienia administratora na serwerach tymczasowej.
- Zmiany do aplikacji są wdrażane na podstawie często, dlatego środowisko musi obsługiwać pojedynczy krok lub automatycznego wdrażania.

> [!NOTE]
> Skalowanie w poziomie wdrożenie bazy danych na wielu serwerach wykracza poza zakres tego samouczka. Aby uzyskać więcej informacji na ten obszar, zapoznaj się [programu SQL Server — książki Online](https://technet.microsoft.com/library/ms130214.aspx).


Na przykład w naszym [samouczka scenariusza](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md), rozwiązanie Contact Manager zarządza Team Foundation Server (TFS). Administratora TFS, Tomasz Tomasz utworzył definicję kompilacji, która umożliwia deweloperom wyzwolić wdrożenie do środowiska pomostowego zgodnie z potrzebami.

![](scenario-configuring-a-staging-environment-for-web-deployment/_static/image1.png)

Należy pamiętać, że w większości przypadków, nie zawsze chcesz wdrożyć ostatniej kompilacji do środowiska pomostowego. Zamiast tego możesz znacznie bardziej prawdopodobne, że mają zostać wdrożone określonej kompilacji, który już został poddany weryfikacji i weryfikacja w środowisku testowym.

## <a name="solution-overview"></a>Omówienie rozwiązania

W tym scenariuszu można wywnioskować tych faktów z analizy wymagania dotyczące wdrażania:

- Konto użytkownika lub inny proces, który wykonuje wdrożenia nie będzie mieć uprawnienia administratora na serwerach przemieszczania, więc przemieszczania serwerów sieci web musi obsługiwać wdrożenia bez uprawnień administratora. Tak należy skonfigurować przemieszczania serwerów sieci web do używania obsługi wdrażania w sieci Web, a nie zdalnego agenta.
- Środowisko przejściowe zawiera wiele serwerów sieci web, ale zachodzi potrzeba obsługi jednym kliknięciem i automatycznego wdrażania, dlatego należy utworzyć farmę serwerów za pomocą struktury farmy sieci Web (WFF). W ten sposób można wdrożyć aplikację w jednej sieci web serwera (serwera podstawowego), a WFF zreplikuje wdrożenia na wszystkich innych serwerach sieci web w środowisku przemieszczania.
- Użytkownik lub konto procesu, który wykonuje wdrożenie musi mieć uprawnienia do tworzenia baz danych. Tak, musisz dodać konto do **dbcreator** roli serwera na serwerze bazy danych oprócz konfigurowania serwera bazy danych do obsługi dostępu zdalnego i wdrażania.

Te tematy zawierają wszystkie informacje potrzebne do wykonania tych zadań:

- [Tworzenie farmy serwerów z struktura farmy sieci Web](creating-a-server-farm-with-the-web-farm-framework.md). W tym temacie opisano sposób tworzenia i konfigurowania farmy serwerów przy użyciu WFF, dzięki czemu produktów platformy sieci web i składników, ustawienia konfiguracji i witryn sieci Web i aplikacji, które są replikowane między wiele serwerów z równoważeniem obciążenia sieci web.
- [Konfigurowanie serwera sieci Web do publikowania narzędzia Web Deploy (Web Deploy obsługi)](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md). W tym temacie opisano sposób tworzenia serwera sieci web, który obsługuje narzędzia Web Deploy publikowania, stosując metodę agenta zdalnego od czystą kompilacji systemu Windows Server 2008 R2.
- [Konfigurowanie serwera bazy danych dla narzędzia Web Deploy publikowanie](configuring-a-database-server-for-web-deploy-publishing.md). W tym temacie opisano sposób konfigurowania serwera bazy danych do obsługi dostępu zdalnego i wdrażania, zaczynając od domyślnej instalacji programu SQL Server 2008 R2.

## <a name="further-reading"></a>Dalsze informacje

Wskazówki dotyczące konfigurowania środowiska testowego typowe developer, zobacz [scenariusz: Konfigurowanie środowisku testu sieci Web wdrożenia](scenario-configuring-a-test-environment-for-web-deployment.md). Aby uzyskać wskazówki dotyczące konfigurowania środowiska produkcji, zobacz [scenariusz: Konfigurowanie środowiska produkcyjnego Web Deployment](scenario-configuring-a-production-environment-for-web-deployment.md).

> [!div class="step-by-step"]
> [Poprzednie](scenario-configuring-a-test-environment-for-web-deployment.md)
> [dalej](scenario-configuring-a-production-environment-for-web-deployment.md)
