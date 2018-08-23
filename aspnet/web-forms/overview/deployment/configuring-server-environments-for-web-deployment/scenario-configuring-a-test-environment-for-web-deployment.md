---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-test-environment-for-web-deployment
title: 'Scenariusz: Konfigurowanie środowiska testowego na potrzeby wdrażania w Internecie | Dokumentacja firmy Microsoft'
author: jrjlee
description: W tym temacie opisano scenariusz wdrażania web typowe dla dewelopera lub środowisk testowych i opisano zadania, które należy wykonać, aby skonfigurować zdarzenia serwisowego...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 44a22ac7-1fc7-4174-b946-c6129fb6a19b
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-test-environment-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 3c6c8850e77f4a3cbb71db4353487c8a5e9c097d
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753939"
---
<a name="scenario-configuring-a-test-environment-for-web-deployment"></a>Scenariusz: Konfigurowanie środowiska testowego na potrzeby wdrażania w Internecie
====================
przez [Jason Lee](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> W tym temacie opisano scenariusz wdrażania web typowe dla dewelopera lub środowisk testowych i opisano zadania, które należy wykonać, aby można było skonfigurować podobnie środowisko.


Gdy deweloperzy pracują w aplikacjach sieci web, często mają one dostęp do środowiska serwera, który może służyć do testowania zmian w aplikacjach w ustawieniu realistyczne. Tego rodzaju środowiska deweloperskie lub testowe zazwyczaj ma następujące cechy:

- Środowisko składa się z jednym serwerze sieci web a serwerem pojedynczej bazy danych.
- Deweloperzy zazwyczaj mieć uprawnienia administratora na serwerach, aby umożliwić im skonfiguruj środowisko do wymagań aplikacji.
- Zmiany w aplikacji są wdrażane na podstawie często, dlatego środowisko musi obsługiwać pojedynczy krok lub automatycznego wdrażania.

Na przykład w naszym [scenariusz samouczka](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md), Matt Hink jest programistą w firmie Fabrikam, Inc. Matt pracuje nad rozwiązania Contact Manager i regularnie musi wdrożyć zmiany w środowisku testowym. Matt jest administratorem na serwerze sieci web test i serwera bazy danych testów. Początkowo Matt musi mieć możliwość wdrażania rozwiązania do środowiska testowego bezpośrednio.

![](scenario-configuring-a-test-environment-for-web-deployment/_static/image1.png)

Jako praca postępów i liczby deweloperów, Dołącz do zespołu, Contact Manager rozwiązania jest skonfigurowany w celu zapewnienia ciągłej integracji (CI) w Team Foundation Server (TFS). Zawsze, gdy programista zaewidencjonuje zawartości, Team Build powinien Skompiluj rozwiązanie, testy jednostkowe i automatycznie wdrażać rozwiązania do środowiska testowego.

![](scenario-configuring-a-test-environment-for-web-deployment/_static/image2.png)

## <a name="solution-overview"></a>Omówienie rozwiązania

Środowisko testowe musi obsługiwać pojedynczy krok lub automatycznego wdrażania z komputera zdalnego, dzięki czemu masz do wyboru dwie główne metody. Można:

- Skonfiguruj serwer sieci web test do obsługi wdrożenia przy użyciu usługi agenta wdrażania sieci Web ("agent zdalny").
- Skonfiguruj serwer sieci web test do obsługi wdrożenia przy użyciu procedury obsługi narzędzia Web Deploy.

> [!NOTE]
> Można także użyć [sieci Web wdrażanie na żądanie](https://technet.microsoft.com/library/ee517345(WS.10).aspx) ("temp agent"). Jest to podobne do metody zdalnego agenta pod kątem wymagań i ograniczeń.


W tym przypadku deweloperzy mają uprawnienia administratora na serwerach docelowych, a środowisko testowe nie podlega ograniczenia ścisłymi zasadami kontrolnymi zabezpieczeń, więc logicznym wyborem jest skonfigurowanie serwera sieci web test do obsługi wdrożenia przy użyciu agenta zdalnego. Jest to mniej skomplikowane i wymaga mniej początkowej konfiguracji niż podejście program obsługi wdrażania w sieci Web. Należy również skonfigurować serwer bazy danych do obsługi dostępu zdalnego i wdrażania.

Te tematy zawierają wszystkie informacje potrzebne do wykonania tych zadań:

- [Konfigurowanie serwera sieci Web dla usługi Web Deploy (Agent zdalny) publikowanie](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md). W tym temacie opisano sposób tworzenia serwera sieci web, który obsługuje narzędzie Web Deploy publikowania, przy użyciu metody agenta zdalnego od czysta kompilacja systemu Windows Server 2008 R2.
- [Konfigurowanie serwera bazy danych dla publikowania Web Deploy](configuring-a-database-server-for-web-deploy-publishing.md). W tym temacie opisano sposób konfigurowania serwera bazy danych do obsługi dostępu zdalnego i wdrażania, zaczynając od domyślnej instalacji programu SQL Server 2008 R2.

## <a name="further-reading"></a>Dalsze informacje

Aby uzyskać wskazówki na temat konfigurowania typowe środowisko przejściowe, zobacz [scenariusz: Konfigurowanie środowiska przejściowego na potrzeby wdrażania w Internecie](scenario-configuring-a-staging-environment-for-web-deployment.md). Aby uzyskać wskazówki na temat konfigurowania środowiska produkcji typowych, zobacz [scenariusz: Konfigurowanie środowiska produkcyjnego na potrzeby wdrażania w Internecie](scenario-configuring-a-production-environment-for-web-deployment.md).

> [!div class="step-by-step"]
> [Poprzednie](choosing-the-right-approach-to-web-deployment.md)
> [dalej](scenario-configuring-a-staging-environment-for-web-deployment.md)
