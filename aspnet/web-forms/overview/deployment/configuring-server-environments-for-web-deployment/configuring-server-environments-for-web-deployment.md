---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment
title: Konfigurowanie środowisk serwera na potrzeby wdrażania w Internecie | Dokumentacja firmy Microsoft
author: jrjlee
description: Ten samouczek przedstawia sposób konfigurowania środowisk serwera na potrzeby pomocy technicznej, uruchamiane jednym kliknięciem i automatycznych, wdrożenia witryny sieci Web i publikowania w różnych scen różnych...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 0bf0959b-4ca8-45de-bd13-b15347543b5a
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 4f6433f0b8a9ad3b3634c9bcd8d95015eebaa865
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755877"
---
<a name="configuring-server-environments-for-web-deployment"></a>Konfigurowanie środowisk serwera na potrzeby wdrażania w Internecie
====================
przez [Jason Lee](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Ten samouczek przedstawia sposób konfigurowania środowisk serwera na potrzeby obsługi jednym kliknięciem i automatycznych, wdrożenia witryny sieci Web i publikowania w różnych scenariuszach różnych. Samouczek zawiera tematy, które przeprowadzą Cię przez wykonywania różnych zadań, takich jak konfigurowanie serwera sieci web do obsługi określonych podejścia do wdrażania i konfigurowania farmy serwerów Framework kolektywu serwerów sieci Web (WFF) wraz z omówienia oparte na scenariuszach, które zapewniają wyższego poziomu wskazówki end-to-end.
> 
> W tym samouczku obejmuje scenariusz wdrażania firmy Fabrikam, Inc., które są opisane w [wdrażania sieci Web w przedsiębiorstwie: omówienie scenariusza](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md) jako punkt odniesienia, przykłady i infrastruktury sieci.
> 
> Włoska tłumaczenia w tych samouczkach, odwiedź stronę [ http://www.lucamorelli.it ](http://www.lucamorelli.it).


Ten samouczek obejmuje następujące tematy:

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

Pierwszy temat [Wybieranie podejścia prawo do wdrażania w Internecie](choosing-the-right-approach-to-web-deployment.md), w tym artykule opisano główne metody, można użyć do publikowania aplikacji sieci web za pomocą narzędzia wdrażania usług Internet Information Services (IIS) w sieci Web (Web Deploy) w wersji 2.0. Określa on scenariusze, które mapowania do danej metody. W tym miejscu każdego tematu scenariusz zawiera ogólne omówienie tych zadań, które należy wykonać i identyfikuje tematy, które będą potrzebne do pracy za pośrednictwem w celu ułatwienia realizacji tych zadań.

Jeśli używasz podziału podejście pliku projektu, opisane w [objaśnienie procesu kompilacji](../web-deployment-in-the-enterprise/understanding-the-build-process.md) do tworzenia i wdrażania rozwiązania, ostatnim temacie [Konfigurowanie właściwości wdrożenia dla środowiska docelowego](configuring-deployment-properties-for-a-target-environment.md), w tym artykule opisano sposób konfigurowania plików projektów specyficznych dla środowiska do wdrożenia w środowiskach różnych miejsc docelowych.

## <a name="key-technologies"></a>Kluczowe technologie

Ten samouczek koncentruje się na temat korzystania z tych produktów i technologii umożliwiających wdrażanie w Internecie:

- IIS 7.5
- Narzędzie Web Deploy 2.x
- WFF 2.x
- Usługa zarządzania usługami IIS sieci Web (WMSvc)

Samouczek styka się również przy użyciu systemu Windows Server 2008 R2, SQL Server 2008 R2, programu ASP.NET 4.0 i ASP.NET MVC 3.

## <a name="other-tutorials-in-this-series"></a>Inne samouczki w tej serii

To jest częścią serii samouczków pięć w skali przedsiębiorstwa wdrażanie w Internecie. Poniżej przedstawiono inne samouczki z serii:

- [Wdrażanie aplikacji sieci Web w scenariuszach dla przedsiębiorstw](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md). Ta zawartość wprowadzające zapewnia kontekstowe tła serii samouczków. Scenariusz samouczka opisano w nim, a następnie pokazano, jak zadania i wskazówki, które opisano w całej serii mieści się w szerszym procesu zarządzania cyklem życia aplikacji (ALM).
- [Narzędzie Web Deployment w przedsiębiorstwie](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Ten samouczek zawiera ogólne wprowadzenie do plików projektów aparatu Microsoft Build Engine (MSBuild), potok publikowania w sieci Web, narzędzie Web Deploy i inne powiązane technologie. Jej wyjaśnia, jak można używać tych narzędzi wspólnie do zarządzania procesami złożonego wdrożenia.
- [Konfigurowanie serwera Team Foundation Server na potrzeby wdrażania w Internecie](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). W tym samouczku opisano, jak skonfigurować Team Foundation Server (TFS) do obsługi różnych scenariuszy wdrażania, w tym zautomatyzowane wdrożenia w ramach procesu ciągłej integracji (CI) i ręcznie wyzwolony wdrożeń z konkretnymi kompilacjami.
- [Zaawansowane wdrażanie w Internecie Enterprise](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). W tym samouczku opisano sposób wykonywania różnych bardziej zaawansowanych zadań wdrażania, takich jak dostosowywanie wdrożeń bazy danych w wielu środowiskach, wykluczanie plików i folderów z wdrożenia i przełączania w tryb offline w aplikacjach sieci web podczas procesu wdrażania .

> [!div class="step-by-step"]
> [Next](choosing-the-right-approach-to-web-deployment.md)
