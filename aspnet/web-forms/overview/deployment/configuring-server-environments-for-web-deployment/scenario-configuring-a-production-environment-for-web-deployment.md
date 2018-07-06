---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-production-environment-for-web-deployment
title: 'Scenariusz: Konfigurowanie środowiska produkcyjnego na potrzeby wdrażania w Internecie | Dokumentacja firmy Microsoft'
author: jrjlee
description: W tym temacie opisano scenariusz wdrażania typowej sieci web w środowisku produkcyjnym i opisano zadania, które należy wykonać, aby skonfigurować podobny...
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: 2e861511-450e-4752-a61e-4a01933f9b6e
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-production-environment-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: dc16b2fadec8051f78d13ffeb97abf2d9e6930f5
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37806716"
---
<a name="scenario-configuring-a-production-environment-for-web-deployment"></a>Scenariusz: Konfigurowanie środowiska produkcyjnego na potrzeby wdrażania w Internecie
====================
przez [Jason Lee](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> W tym temacie opisano scenariusz wdrażania typowej sieci web w środowisku produkcyjnym oraz opisano zadania, które należy wykonać, aby można było skonfigurować podobnie środowisko.


W środowisku produkcyjnym jest ostatecznym miejscem przeznaczenia dla aplikacji sieci web lub witryny sieci Web. Na tym etapie aplikacja przeszła testy, został wdrożony w środowisku przejściowym i jest gotowa do "aktywowania." Cechy w środowisku produkcyjnym mogą się znacznie zmieniać zgodnie z natury i celem zawartości sieci web, rozmiar Twojej organizacji, docelowych odbiorców i wiele innych czynników. W scenariuszu skali przedsiębiorstwa w środowisku produkcyjnym może mieć następującą charakterystykę:

- Środowisko składa się z wielu serwerów sieci web z równoważeniem obciążenia i jeden lub więcej serwerów bazy danych, często z klastrami trybu failover i dublowanie bazy danych.
- Jeśli środowisko jest połączone z Internetem, prawdopodobnie rozdzielony z sieci wewnętrznej. Może być w innej podsieci w sieci obwodowej, może być w innej domenie, a może znajdować się na infrastrukturę sieci zupełnie innego.
- Deweloperzy i kont proces serwera kompilacji są bardzo mało prawdopodobne mieć uprawnienia administratora na serwerach produkcyjnych.
- Do aplikacji są wdrażane na podstawie rzadziej niż testowych i przejściowych wdrożeń.

> [!NOTE]
> Skalowanie w poziomie wdrożenie bazy danych na wielu serwerach wykracza poza zakres tego samouczka. Aby uzyskać więcej informacji na temat tego obszaru, zajrzyj [programu SQL Server — książki Online](https://technet.microsoft.com/library/ms130214.aspx).


Na przykład w naszym [scenariusz samouczka](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md), serwer kompilacji zespołowej zawiera definicje kompilacji, które pozwalają użytkownikom tworzenia rozwiązania Contact Manager oraz wdrażania jej w środowisku przejściowym w jednym kroku. Gdy aplikacja jest gotowa do wdrożenia w środowisku produkcyjnym ze względu na ograniczenia nałożone przez wymagania dotyczące zabezpieczeń i infrastruktury sieci, administrator środowiska produkcyjnego, należy ręcznie skopiować pakiet sieci web na serwerze sieci web w środowisku produkcyjnym i zaimportować go za pomocą Menedżera usług Internet Information Services (IIS).

![](scenario-configuring-a-production-environment-for-web-deployment/_static/image1.png)

## <a name="solution-overview"></a>Omówienie rozwiązania

W tym scenariuszu można wywnioskować te faktów z analizy wymagania dotyczące wdrażania:

- Ze względu na ograniczenia zabezpieczeń i konfiguracji sieci nie można skonfigurować obsługują wdrażanie jednym kliknięciem i automatycznych w środowisku produkcyjnym. Wdrożenie w trybie offline jest jedynym realnym podejście, w tym scenariuszu.
- W środowisku produkcyjnym obejmuje wiele serwerów sieci web, aby można było używać struktury farmy sieci Web (WFF), aby utworzyć farmę serwerów. Użycie tej metody, administrator musi się tylko do zaimportowania aplikację na jedną witrynę sieci web serwera (serwera podstawowego) i WFF będą replikowane wdrożenia na wszystkich pozostałych serwerach sieci web w środowisku produkcyjnym.

Te tematy zawierają wszystkie informacje potrzebne do wykonania tych zadań:

- [Tworzenie farmy serwerów za pomocą rozwiązania Web Farm Framework](configuring-a-database-server-for-web-deploy-publishing.md). W tym temacie opisano sposób tworzenia i konfigurowania farmy serwerów za pomocą WFF, tak aby produktów platformy sieci web i składników, ustawienia konfiguracji i witryn sieci Web i aplikacji, które są replikowane między wieloma serwerami sieci web z równoważeniem obciążenia.
- [Konfigurowanie serwera sieci Web dla usługi Web Deploy (wdrożenie w trybie Offline) publikowanie](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md). W tym temacie opisano sposób tworzenia serwera sieci web umożliwiający administratorom importowanie i wdrażanie pakietów internetowych ręcznie, zaczynając od czysta kompilacja systemu Windows Server 2008 R2.
- [Konfigurowanie serwera bazy danych dla publikowania Web Deploy](configuring-a-database-server-for-web-deploy-publishing.md). W tym temacie opisano sposób konfigurowania serwera bazy danych do obsługi dostępu zdalnego i wdrażania, zaczynając od domyślnej instalacji programu SQL Server 2008 R2.

## <a name="further-reading"></a>Dalsze informacje

Aby uzyskać wskazówki na temat konfigurowania środowiska testowego typowe dla deweloperów, zobacz [scenariusz: Konfigurowanie środowiska testowania na potrzeby wdrażania w Internecie](scenario-configuring-a-test-environment-for-web-deployment.md). Aby uzyskać wskazówki na temat konfigurowania typowe środowisko przejściowe, zobacz [scenariusz: Konfigurowanie środowiska przejściowego na potrzeby wdrażania w Internecie](scenario-configuring-a-staging-environment-for-web-deployment.md).

> [!div class="step-by-step"]
> [Poprzednie](scenario-configuring-a-staging-environment-for-web-deployment.md)
> [dalej](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)
