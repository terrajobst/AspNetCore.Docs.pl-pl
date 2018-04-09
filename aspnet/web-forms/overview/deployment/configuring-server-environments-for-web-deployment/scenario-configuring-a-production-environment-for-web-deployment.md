---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-production-environment-for-web-deployment
title: 'Scenariusz: Konfigurowanie środowiska produkcyjnego wdrożenia sieci Web | Dokumentacja firmy Microsoft'
author: jrjlee
description: W tym temacie opisano scenariusz wdrażania typowych sieci web w środowisku produkcyjnym i opisano zadania, które należy wykonać, aby skonfigurować podobne...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 2e861511-450e-4752-a61e-4a01933f9b6e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-production-environment-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 4de5b1f20f3adcb53765c7cb9765c0d90a80e677
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="scenario-configuring-a-production-environment-for-web-deployment"></a>Scenariusz: Konfigurowanie środowiska produkcyjnego wdrożenia sieci Web
====================
przez [Lewandowski Jason](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> W tym temacie opisano scenariusz wdrażania typowych sieci web w środowisku produkcyjnym i opisano zadania, które należy wykonać, aby skonfigurować środowisko podobne.


W środowisku produkcyjnym jest ostatecznym miejscem docelowym dla aplikacji sieci web lub witryny sieci Web. W tym punkcie aplikacji została testy, został wdrożony w środowisku przemieszczania i jest gotowe "na żywo." Właściwości środowisku produkcyjnym można powszechnie różny w zależności od charakteru i celu zawartości sieci web, wielkości organizacji, docelowych odbiorców i wiele innych czynników. W scenariuszu skali przedsiębiorstwa środowisku produkcyjnym może mieć następujące właściwości:

- Środowisko składa się z wielu serwerów sieci web z równoważeniem obciążenia i co najmniej jeden serwer bazy danych, często z klastra trybu failover i dublowanie bazy danych.
- Jeśli środowisko jest połączonych z Internetem, prawdopodobnie rozdzielony z sieci wewnętrznej. Być może jest ona w innej podsieci w sieci obwodowej, może być w innej domenie i może znajdować się na infrastrukturę sieci zupełnie innego.
- Deweloperzy i kont procesu serwera kompilacji są wysoce nieprawdopodobne uprawnień administratora na serwerach produkcyjnych.
- Zmiany do aplikacji są wdrażane na podstawie rzadziej niż testu lub przemieszczania wdrożeń.

> [!NOTE]
> Skalowanie w poziomie wdrożenie bazy danych na wielu serwerach wykracza poza zakres tego samouczka. Aby uzyskać więcej informacji na ten obszar, zapoznaj się [programu SQL Server — książki Online](https://technet.microsoft.com/library/ms130214.aspx).


Na przykład w naszym [samouczka scenariusza](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md), serwerem Team Build zawiera definicje kompilacji, które pozwalają zbudować rozwiązanie do kontaktów Menedżerze i wdrożyć ją w środowisku przemieszczania w jednym kroku. Gdy aplikacja jest gotowa do wdrożenia w środowisku produkcyjnym ze względu na ograniczenia narzucone przez wymagania dotyczące zabezpieczeń i infrastruktury sieci administratora środowiska produkcyjnego należy ręcznie skopiować pakiet sieci web na serwerze sieci web w środowisku produkcyjnym i zaimportować go za pośrednictwem Menedżera usług Internet Information Services (IIS).

![](scenario-configuring-a-production-environment-for-web-deployment/_static/image1.png)

## <a name="solution-overview"></a>Omówienie rozwiązania

W tym scenariuszu można wywnioskować tych faktów z analizy wymagania dotyczące wdrażania:

- Z powodu ograniczeń zabezpieczeń i konfiguracji sieci nie można skonfigurować środowisku produkcyjnym w celu obsługi wdrożenia jednym kliknięciem lub automatyczne. Wdrożenia w trybie offline jest tylko działało podejście, w tym scenariuszu.
- W środowisku produkcyjnym zawiera wiele serwerów sieci web, dzięki czemu można używać struktury farmy sieci Web (WFF), aby utworzyć farmę serwerów. W ten sposób, administrator musi się tylko do zaimportowania aplikacji na serwerze sieci web co (podstawowy serwer) i WFF zreplikuje wdrożenia na wszystkich innych serwerach sieci web w środowisku produkcyjnym.

Te tematy zawierają wszystkie informacje potrzebne do wykonania tych zadań:

- [Tworzenie farmy serwerów z struktura farmy sieci Web](configuring-a-database-server-for-web-deploy-publishing.md). W tym temacie opisano sposób tworzenia i konfigurowania farmy serwerów przy użyciu WFF, dzięki czemu produktów platformy sieci web i składników, ustawienia konfiguracji i witryn sieci Web i aplikacji, które są replikowane między wiele serwerów z równoważeniem obciążenia sieci web.
- [Konfigurowanie serwera sieci Web narzędzia Web Deploy publikowania (wdrożenie w trybie Offline)](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md). W tym temacie opisano sposób tworzenia serwera sieci web umożliwiające administratorom importowanie i wdrażanie pakietów sieci web ręcznie, zaczynając od czystą kompilacji systemu Windows Server 2008 R2.
- [Konfigurowanie serwera bazy danych dla narzędzia Web Deploy publikowanie](configuring-a-database-server-for-web-deploy-publishing.md). W tym temacie opisano sposób konfigurowania serwera bazy danych do obsługi dostępu zdalnego i wdrażania, zaczynając od domyślnej instalacji programu SQL Server 2008 R2.

## <a name="further-reading"></a>Dalsze informacje

Wskazówki dotyczące konfigurowania środowiska testowego typowe developer, zobacz [scenariusz: Konfigurowanie środowisku testu sieci Web wdrożenia](scenario-configuring-a-test-environment-for-web-deployment.md). Aby uzyskać wskazówki dotyczące konfigurowania typowe środowisko przejściowe, zobacz [scenariusz: Konfigurowanie środowisku przemieszczania Web Deployment](scenario-configuring-a-staging-environment-for-web-deployment.md).

> [!div class="step-by-step"]
> [Poprzednie](scenario-configuring-a-staging-environment-for-web-deployment.md)
> [dalej](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)
