---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-test-environment-for-web-deployment
title: "Scenariusz: Konfigurowanie środowiska testowego na potrzeby wdrażania w sieci Web | Dokumentacja firmy Microsoft"
author: jrjlee
description: "W tym temacie opisano scenariusz wdrażania web typowa dla deweloperów lub środowisk testowania i opisano zadania, które należy wykonać, aby skonfigurować si..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 44a22ac7-1fc7-4174-b946-c6129fb6a19b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-test-environment-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 23e317c6e0b6daf2d7937b73738e5cb6fa32cde2
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="scenario-configuring-a-test-environment-for-web-deployment"></a>Scenariusz: Konfigurowanie środowiska testowego na potrzeby wdrażania w sieci Web
====================
przez [Lewandowski Jason](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> W tym temacie opisano scenariusz wdrażania web typowa dla deweloperów lub środowisk testowania i opisano zadania, które należy wykonać, aby skonfigurować środowisko podobne.


Gdy deweloperzy pracują w aplikacjach sieci web, ich często otrzymuje dostęp do środowiska serwera, które mają zostać przetestowane zmiany ich aplikacji w ustawieniu realistyczne mogą przy użyciu. Tego rodzaju środowisko rozwoju lub testowania zwykle ma następujące cechy:

- Środowisko składa się z jednym serwerze sieci web a serwerem pojedynczej bazy danych.
- Deweloperzy zazwyczaj są uprawnienia administratora na serwerach, w celu umożliwienia im skonfiguruj środowisko do wymagań aplikacji.
- Zmiany do aplikacji są wdrażane na podstawie często, dlatego środowisko musi obsługiwać pojedynczy krok lub automatycznego wdrażania.

Na przykład w naszym [samouczka scenariusza](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md), Matt Hink jest Deweloper pracujący u firmy Fabrikam, Inc. Matt działa w ramach rozwiązania kontaktów Menedżerze i regularnie konieczne wdrażanie zmiany do środowiska testowego. Matt jest administratorem na serwerze sieci web testów i na serwerze bazy danych testu. Początkowo Matt musi mieć możliwość wdrażania rozwiązania bezpośrednio do środowiska testowego.

![](scenario-configuring-a-test-environment-for-web-deployment/_static/image1.png)

Jako pracy realizowany i deweloperów więcej przyłączyć zespołu menedżera skontaktuj się z rozwiązania jest skonfigurowany dla ciągłej integracji (CI) w Team Foundation Server (TFS). Zawsze, gdy projektant sprawdza w zawartości, Team Build powinien Skompiluj rozwiązanie, uruchom wszystkie testy jednostkowe i automatyczne wdrażanie rozwiązania do środowiska testowego.

![](scenario-configuring-a-test-environment-for-web-deployment/_static/image2.png)

## <a name="solution-overview"></a>Omówienie rozwiązania

Środowisko testowe musi obsługiwać pojedynczy krok lub zautomatyzowane wdrażanie na komputerze zdalnym, dlatego należy wybrać dwa podejścia głównego. Można:

- Skonfiguruj serwer testu sieci web do obsługi wdrożenia przy użyciu usługi sieci Web wdrożenia agenta ("agent zdalnego").
- Skonfiguruj serwer sieci web testów do obsługi wdrożenia przy użyciu procedury obsługi narzędzia Web Deploy.

> [!NOTE]
> Można także użyć [sieci Web wdrażanie na żądanie](https://technet.microsoft.com/library/ee517345(WS.10).aspx) ("agent tymczasowego"). To jest podobna do metody zdalnego agenta pod względem wymagań i ograniczeń.


W takim przypadku deweloperzy mają uprawnienia administratora na serwerze docelowym, a środowisko testowe nie podlega ograniczenia ograniczeniami zabezpieczeń, wybór logicznej jest skonfigurowanie testu serwera sieci web do obsługi wdrożenia przy użyciu agenta zdalnego. To jest mniej złożona i wymaga mniej początkowej konfiguracji niż podejście program obsługi wdrażania w sieci Web. Należy także skonfigurować serwer bazy danych do obsługi dostępu zdalnego i wdrażania.

Te tematy zawierają wszystkie informacje potrzebne do wykonania tych zadań:

- [Konfigurowanie serwera sieci Web narzędzia Web Deploy publikowania (agenta zdalnego)](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md). W tym temacie opisano sposób tworzenia serwera sieci web, który obsługuje narzędzia Web Deploy publikowania, stosując metodę agenta zdalnego od czystą kompilacji systemu Windows Server 2008 R2.
- [Konfigurowanie serwera bazy danych dla narzędzia Web Deploy publikowanie](configuring-a-database-server-for-web-deploy-publishing.md). W tym temacie opisano sposób konfigurowania serwera bazy danych do obsługi dostępu zdalnego i wdrażania, zaczynając od domyślnej instalacji programu SQL Server 2008 R2.

## <a name="further-reading"></a>Dalsze informacje

Aby uzyskać wskazówki dotyczące konfigurowania typowe środowisko przejściowe, zobacz [scenariusz: Konfigurowanie środowisku przemieszczania Web Deployment](scenario-configuring-a-staging-environment-for-web-deployment.md). Aby uzyskać wskazówki dotyczące konfigurowania środowiska produkcji, zobacz [scenariusz: Konfigurowanie środowiska produkcyjnego Web Deployment](scenario-configuring-a-production-environment-for-web-deployment.md).

>[!div class="step-by-step"]
[Poprzednie](choosing-the-right-approach-to-web-deployment.md)
[dalej](scenario-configuring-a-staging-environment-for-web-deployment.md)
