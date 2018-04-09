---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment
title: Konfigurowanie środowisk serwera sieci Web wdrożenia | Dokumentacja firmy Microsoft
author: jrjlee
description: W tym samouczku opisano, jak skonfigurować server w środowiskach z obsługą jednym kliknięciem lub zautomatyzowane, wdrożenia witryny sieci Web i publikowania w różnych scen różnych...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 0bf0959b-4ca8-45de-bd13-b15347543b5a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: ff6118be618a170ac76d66a9de24a7b5cc2d840a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="configuring-server-environments-for-web-deployment"></a>Konfigurowanie środowisk serwera sieci Web wdrożenia
====================
przez [Lewandowski Jason](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Ten samouczek przedstawia sposób konfigurowania środowiska serwera do obsługi jednym kliknięciem lub zautomatyzowane, wdrożenia witryny sieci Web i publikowania w różnych scenariuszach różnych. Samouczek zawiera tematy, aby zademonstrować ukończenia różnych zadań, takich jak konfigurowanie serwera sieci web do obsługi określonych podejścia do wdrażania i konfigurowania farmy serwerów Framework kolektywu serwerów sieci Web (WFF) wraz z omówienie oparta na scenariuszu, które zapewniają wskazówki na trasie wyższego poziomu.
> 
> Scenariusz wdrażania firmy Fabrikam, Inc. opisane w samouczku [wdrożenia sieci Web w przedsiębiorstwie: omówienie scenariusza](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md) jako punkt odniesienia, przykłady i infrastruktury sieci.
> 
> Włoska translacji tego samouczka, odwiedź stronę [ http://www.lucamorelli.it ](http://www.lucamorelli.it).


W tym samouczku omówiono następujące zagadnienia:

- [Wybieranie właściwego podejścia do wdrażania w Internecie](choosing-the-right-approach-to-web-deployment.md)
- [Scenariusz: Konfigurowanie środowiska testowego na potrzeby wdrażania w Internecie](scenario-configuring-a-test-environment-for-web-deployment.md)
- [Scenariusz: Konfigurowanie środowiska przejściowego na potrzeby wdrażania w Internecie](scenario-configuring-a-staging-environment-for-web-deployment.md)
- [Scenariusz: Konfigurowanie środowiska produkcyjnego na potrzeby wdrażania w Internecie](scenario-configuring-a-production-environment-for-web-deployment.md)
- [Konfigurowanie serwera sieci Web dla usługi publikowania Web Deploy (agent zdalny)](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)
- [Konfigurowanie serwera sieci Web dla usługi publikowania Web Deploy (program obsługi narzędzia Web Deploy)](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md)
- [Konfigurowanie serwera sieci Web dla usługi publikowania Web Deploy (wdrożenie w trybie offline)](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)
- [Konfigurowanie serwera bazy danych dla usługi publikowania Web Deploy](configuring-a-database-server-for-web-deploy-publishing.md)
- [Tworzenie farmy serwerów za pomocą rozwiązania Web Farm Framework](creating-a-server-farm-with-the-web-farm-framework.md)
- [Konfigurowanie właściwości wdrożenia dla środowiska docelowego](configuring-deployment-properties-for-a-target-environment.md)

Pierwszym temacie [Wybieranie podejście prawo do wdrożenia w sieci Web](choosing-the-right-approach-to-web-deployment.md), zawiera opis głównych podejścia, można użyć do publikowania aplikacji sieci web za pomocą narzędzia wdrażania Internet Information Services (IIS) w sieci Web (Web Deploy) 2.0. Identyfikuje również scenariusze, które mapują na każde podejście. W tym miejscu każdego tematu scenariusz zawiera ogólne omówienie zadań, które należy wykonać i identyfikuje tematy, które będą potrzebne do pracy za pośrednictwem ułatwiające wykonywanie tych zadań.

Jeśli używasz podejście pliku projektu podziału opisane w [opis procesu kompilacji](../web-deployment-in-the-enterprise/understanding-the-build-process.md) do tworzenia i wdrażania rozwiązania, temat końcowego [Konfigurowanie właściwości wdrożenia dla środowiska docelowego](configuring-deployment-properties-for-a-target-environment.md), zawiera opis sposobu konfigurowania plików projektu określonego środowiska do wdrożenia na docelowym różnych środowiskach.

## <a name="key-technologies"></a>Kluczowe technologie

Ten samouczek koncentruje się na temat korzystania z tych produktów i technologii do obsługi wdrożenia sieci web:

- IIS 7.5
- Narzędzie Web Deploy 2.x
- WFF 2.x
- Usługi zarządzania usługami IIS sieci Web (WMSvc)

Samouczek dotyka również przy użyciu systemu Windows Server 2008 R2, SQL Server 2008 R2, programu ASP.NET 4.0 i ASP.NET MVC 3.

## <a name="other-tutorials-in-this-series"></a>Innych samouczków w tej serii

To jest częścią serii samouczków pięć w skali przedsiębiorstwa wdrożenia sieci web. Są to innych samouczków z serii:

- [Wdrażanie aplikacji sieci Web w scenariuszach dla przedsiębiorstw](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md). Ta zawartość wprowadzające udostępnia kontekstowe tła serii samouczka. Opisuje samouczek scenariusza i przedstawia, jak zadania i wskazówki, które opisano w całym serii pasuje do szerszego procesu zarządzania cyklem życia aplikacji (ALM).
- [Narzędzie Web Deployment w przedsiębiorstwie](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Ten samouczek zawiera koncepcyjnej wprowadzenie do plików projektu Microsoft kompilacji Engine (MSBuild), potoku publikowania w sieci Web narzędzia Web Deploy i innych technologii pokrewnych. Wyjaśniono sposób korzystania tych narzędzi wspólnie do zarządzania procesami złożone wdrożenia.
- [Konfigurowanie serwera Team Foundation Server Web Deployment](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). W tym samouczku opisano sposób konfigurowania Team Foundation Server (TFS) do obsługi różnych scenariuszy wdrażania, łącznie z automatycznego wdrażania w ramach procesu ciągłej integracji (CI) i ręcznie wyzwalane wdrożeń określonej kompilacji.
- [Zaawansowane wdrażanie w przedsiębiorstwie sieci Web](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). Ten przewodnik opisuje sposób wykonywania różnych bardziej zaawansowanych zadań wdrażania, jak dostosowywanie wdrożenia bazy danych w wielu środowiskach, z wyjątkiem plików i folderów z wdrożenia i pobieranie aplikacji sieci web w trybie offline podczas procesu wdrażania .

> [!div class="step-by-step"]
> [Next](choosing-the-right-approach-to-web-deployment.md)
