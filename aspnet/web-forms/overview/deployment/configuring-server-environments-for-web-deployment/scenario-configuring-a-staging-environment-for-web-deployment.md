---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-staging-environment-for-web-deployment
title: 'Scenariusz: Konfigurowanie środowiska przejściowego na potrzeby wdrażania w Internecie | Dokumentacja firmy Microsoft'
author: jrjlee
description: W tym temacie opisano scenariusz wdrażania web typowe środowisko przejściowe i opisano zadania, które należy wykonać, aby skonfigurować podobnie środowisko...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 5a8e49b7-5317-4125-b107-7e2466b47bb3
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-staging-environment-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: fda4f056eebb77df3e93c63bdd4fabb071c0a0a9
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37365230"
---
<a name="scenario-configuring-a-staging-environment-for-web-deployment"></a>Scenariusz: Konfigurowanie środowiska przejściowego na potrzeby wdrażania w Internecie
====================
przez [Jason Lee](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> W tym temacie opisano scenariusz wdrażania web typowe środowisko przejściowe i opisano zadania, które należy wykonać, aby można było skonfigurować podobnie środowisko.


Wiele organizacji użyj tymczasowym, aby przejrzeć aktualizacje aplikacji sieci web lub witryn sieci Web. Osoby w organizacji daje szansę, aby eksplorować i przegląd nowych funkcji lub zawartości przed witryna "miejsce na żywo" lub innymi słowy, jest wdrażana w środowisku produkcyjnym. Środowisko przejściowe jest przeznaczony do replikacji w środowisku produkcyjnym możliwie najlepszy sposób, aby zapewnić realistyczne (wersja zapoznawcza). Tego rodzaju środowisko przejściowe zazwyczaj ma następujące cechy:

- Środowisko składa się z wielu serwerów sieci web z równoważeniem obciążenia i jeden lub więcej serwerów bazy danych, często z klastrami trybu failover i dublowanie bazy danych.
- Aplikacje można wdrażać ręcznie przez zespół deweloperów lub automatycznie przez serwer kompilacji zespołowej.
- Użytkownicy lub kontom proces wdrażania aplikacji są prawdopodobnie nie będzie mieć uprawnienia administratora na serwerach przemieszczania.
- Zmiany w aplikacji są wdrażane na podstawie często, dlatego środowisko musi obsługiwać pojedynczy krok lub automatycznego wdrażania.

> [!NOTE]
> Skalowanie w poziomie wdrożenie bazy danych na wielu serwerach wykracza poza zakres tego samouczka. Aby uzyskać więcej informacji na temat tego obszaru, zajrzyj [programu SQL Server — książki Online](https://technet.microsoft.com/library/ms130214.aspx).


Na przykład w naszym [scenariusz samouczka](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md), Team Foundation Server (TFS) zarządza rozwiązania Contact Manager. Administratora TFS, Tomasz Rob utworzył definicję kompilacji, która pozwala deweloperom na wyzwalanie wdrożenia w środowisku przejściowym, zgodnie z potrzebami.

![](scenario-configuring-a-staging-environment-for-web-deployment/_static/image1.png)

Należy pamiętać, że w większości przypadków nie będzie niekoniecznie chcesz wdrożenie najnowszej kompilacji w środowisku przejściowym. Zamiast tego możesz o wiele bardziej prawdopodobne mają zostać wdrożone konkretnej kompilacji, który został już poddany sprawdzania poprawności i weryfikacji w środowisku testowym.

## <a name="solution-overview"></a>Omówienie rozwiązania

W tym scenariuszu można wywnioskować te faktów z analizy wymagania dotyczące wdrażania:

- Konto użytkownika lub autoryzowany proces, który wykonuje wdrożenia nie będzie mieć uprawnienia administratora na serwerze przejściowym, więc przemieszczania serwerów sieci web musi obsługiwać wdrożenia bez uprawnień administratora. W efekcie należy konfigurowanie serwerów sieci web przemieszczania, służące do obsługi wdrażania sieci Web, a nie agenta zdalnego.
- Środowisko przejściowe zawiera wiele serwerów sieci web, ale należy go obsługuje wdrażanie jednym kliknięciem lub zautomatyzowane, więc należy utworzyć farmę serwerów za pomocą struktury farmy sieci Web (WFF). W ten sposób można wdrożyć aplikację w jednej sieci web serwera (serwera podstawowego) i WFF będą replikowane wdrożenia na wszystkich pozostałych serwerach sieci web w środowisku przejściowym.
- Użytkownika lub konto procesu, który wykonuje wdrożenie musi mieć uprawnienia do tworzenia baz danych. Jako takie, musisz dodać konto do **dbcreator** roli serwera na serwerze bazy danych, po skonfigurowaniu serwera bazy danych do obsługi dostępu zdalnego i wdrażania.

Te tematy zawierają wszystkie informacje potrzebne do wykonania tych zadań:

- [Tworzenie farmy serwerów za pomocą rozwiązania Web Farm Framework](creating-a-server-farm-with-the-web-farm-framework.md). W tym temacie opisano sposób tworzenia i konfigurowania farmy serwerów za pomocą WFF, tak aby produktów platformy sieci web i składników, ustawienia konfiguracji i witryn sieci Web i aplikacji, które są replikowane między wieloma serwerami sieci web z równoważeniem obciążenia.
- [Konfigurowanie serwera sieci Web dla publikowania Web Deploy (Web Deploy obsługi)](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md). W tym temacie opisano sposób tworzenia serwera sieci web, który obsługuje narzędzie Web Deploy publikowania, przy użyciu metody agenta zdalnego od czysta kompilacja systemu Windows Server 2008 R2.
- [Konfigurowanie serwera bazy danych dla publikowania Web Deploy](configuring-a-database-server-for-web-deploy-publishing.md). W tym temacie opisano sposób konfigurowania serwera bazy danych do obsługi dostępu zdalnego i wdrażania, zaczynając od domyślnej instalacji programu SQL Server 2008 R2.

## <a name="further-reading"></a>Dalsze informacje

Aby uzyskać wskazówki na temat konfigurowania środowiska testowego typowe dla deweloperów, zobacz [scenariusz: Konfigurowanie środowiska testowania na potrzeby wdrażania w Internecie](scenario-configuring-a-test-environment-for-web-deployment.md). Aby uzyskać wskazówki na temat konfigurowania środowiska produkcji typowych, zobacz [scenariusz: Konfigurowanie środowiska produkcyjnego na potrzeby wdrażania w Internecie](scenario-configuring-a-production-environment-for-web-deployment.md).

> [!div class="step-by-step"]
> [Poprzednie](scenario-configuring-a-test-environment-for-web-deployment.md)
> [dalej](scenario-configuring-a-production-environment-for-web-deployment.md)
