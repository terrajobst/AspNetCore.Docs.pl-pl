---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment
title: Konfigurowanie serwera Team Foundation Server Web Deployment | Dokumentacja firmy Microsoft
author: jrjlee
description: "Ten samouczek przedstawia sposób konfigurowania Team Foundation Server (TFS) 2010 do tworzenia rozwiązań i wdrażania zawartości sieci web do różnych środowiskach docelowych. To..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: ff55233a-e795-4007-a4fc-861fe1bb590b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 72f60841a1381380c0ea6167077420f960180dc7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="configuring-team-foundation-server-for-web-deployment"></a>Konfigurowanie serwera Team Foundation Server Web Deployment
====================
przez [Lewandowski Jason](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Ten samouczek przedstawia sposób konfigurowania Team Foundation Server (TFS) 2010 do tworzenia rozwiązań i wdrażania zawartości sieci web do różnych środowiskach docelowych. W tym scenariuszy ciągłej integracji (CI), gdzie wdrażania zawartości automatycznie każdorazowego dewelopera dokona zmian. Może również obejmować ręczne wyzwalacza scenariuszy, w którym administrator może wykonać aby wyzwolić wdrożenie określonej kompilacji w środowisku przemieszczania po kompilacji została zweryfikowana i zweryfikowane w środowisku testowym. Tematy w tym samouczku przeprowadzi Cię przez proces całą konfigurację, w tym:
> 
> - Sposób tworzenia nowego projektu zespołowego w programie TFS.
> - Jak dodać zawartości do kontroli źródła.
> - Jak skonfigurować serwer kompilacji obsługuje elementu konfiguracji i wdrażania.
> - Jak utworzyć definicję kompilacji, która zawiera logikę wdrożenia.
> - Jak skonfigurować uprawnienia dla automatycznego wdrażania.
> 
> Włoska translacji tego samouczka, odwiedź stronę [http://www.lucamorelli.it](http://www.lucamorelli.it).


Ten samouczek zakłada, że zainstalowano TFS 2010 i utworzyć kolekcję projektów zespołowych w trakcie procesu konfiguracji początkowej. [Team Foundation podręczniku instalacji programu Visual Studio 2010](https://go.microsoft.com/?linkid=9805132) zawiera kompleksowe wskazówki na temat tych zadań.

## <a name="context"></a>Kontekst

To jest częścią serii samouczków oparte na wymaganiach dotyczących wdrożenia enterprise fikcyjnej firmy o nazwie firmy Fabrikam, Inc. Przykładowe rozwiązanie & #x 2014; korzysta z tego samouczka serii [kontaktów Menedżerze](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) #x 2014; & rozwiązania do reprezentowania aplikacji sieci web z realistyczne poziom złożoności, w tym aplikacji ASP.NET MVC 3, systemu Windows Usługi Communication Foundation (WCF), a projekt bazy danych.

Istotą te samouczki metody wdrażania opiera się na podejście pliku projektu podziału opisane w [opis procesu kompilacji](../web-deployment-in-the-enterprise/understanding-the-build-process.md), w którym jest kontrolowany przez proces kompilacji projektu dwa pliki & #x 2014; jeden zawierający Tworzenie instrukcji, które mają zastosowanie do każdego środowiska docelowego i dysk zawierający ustawienia kompilacji i wdrożenia określonego środowiska. W czasie kompilacji pliku projektu określonego środowiska jest scalany pliku projektu niezależny od środowiska pełny zestaw instrukcji kompilacji.

## <a name="scenario-overview"></a>Omówienie scenariusza

Scenariusz wysokiego poziomu te samouczki jest opisany w [wdrożenia sieci Web w przedsiębiorstwie: omówienie scenariusza](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md). Firma Microsoft zaleca, przejrzyj ten temat przed rozpoczęciem tego samouczka.

## <a name="how-to-use-this-tutorial"></a>Jak używać tego samouczka

Jeśli po raz pierwszy zostały wykonane zadania opisane w tym samouczku, lub jeśli chcesz wykonać przykłady za pomocą rozwiązania próbki, powinny działać z samouczka tematami w kolejności. Alternatywnie można użyć tematy jako wskazówki do wykonywania określonych zadań. W tym samouczku omówiono następujące zagadnienia:

- [Tworzenie nowego projektu zespołowego w programie TFS](creating-a-team-project-in-tfs.md). Projekt zespołowy jest jednostka core kontroli źródła, zarządzanie procesem i kompilacji w programie TFS. Musisz utworzyć projektu zespołowego, zanim będzie można dodać zawartości do kontroli źródła lub Utwórz definicje kompilacji.
- [Dodawanie zawartości do kontroli źródła](adding-content-to-source-control.md). Po utworzeniu projektu zespołowego, można uruchomić Dodawanie zawartości do kontroli źródła. Należy dodać swoje projekty i rozwiązania, oraz wszelkie zależności zewnętrzne, aby rozpocząć konfigurowanie kompilacji.
- [Konfigurowanie TFS kompilacji serwera sieci Web wdrożenia](configuring-a-tfs-build-server-for-web-deployment.md). Jeśli chcesz utworzyć zawartości projektu zespołowego, musisz skonfigurować serwer kompilacji. W większości przypadków powinien to być na osobnym komputerze z programem TFS instalacji. Aby skonfigurować serwer kompilacji, należy zainstalować i skonfigurować usługę kompilacji TFS, zainstaluj program Visual Studio 2010, tworzenie kontrolerów kompilacji i agentów kompilacji, zainstalować wszystkie produkty lub składniki, które kodu wymaga, aby pomyślnie kompilacji i zainstaluj Internetowe usługi informacyjne (IIS w) sieci Web narzędzia Deployment (Web Deploy).
- [Tworzenie definicji kompilacji, który obsługuje wdrażanie](creating-a-build-definition-that-supports-deployment.md). Przed uruchomieniem usługi kolejkowania wiadomości lub wyzwalania kompilacji w programie TFS, musisz utworzyć co najmniej jednej kompilacji definicji dla projektu zespołowego. Definicja kompilacji definiuje każdego aspektu kompilacji, łącznie z czynności, które powinny być uwzględnione w kompilacji, jakie zdarzenia powinny wywoływać kompilacji i gdzie Team Build wysłać dane wyjściowe kompilacji. Można skonfigurować definicję kompilacji do uruchamiania niestandardowych plików projektu Microsoft kompilacji Engine (MSBuild), który pozwala umieścić w kompilacjach zautomatyzowanych logiki wdrożenia.
- [Wdrażanie określonej kompilacji](deploying-a-specific-build.md). W wiele scenariuszy należy wdrożyć określonej kompilacji zamiast ostatniej kompilacji w środowisku docelowym. W takim przypadku można skonfigurować definicję kompilacji, która wdraża zawartość z listy określonego folderu.
- [Wdrożenie kompilacji konfigurowania uprawnień dla zespołu](configuring-permissions-for-team-build-deployment.md). W przypadku usługi kompilacji do wdrażania zawartości w ramach procesu kompilacji zautomatyzowane, musisz różne uprawnienia dla konta usługi kompilacji na docelowej serwerów sieci web i serwery baz danych.

## <a name="key-technologies"></a>Kluczowe technologie

Ten samouczek koncentruje się na temat korzystania z tych produktów i technologii do obsługi automatycznych kompilowanie i wdrażanie w sieci web:

- Visual Studio Team Foundation Server 2010
- Team Build i MSBuild
- Web Deploy

Samouczek dotyka również przy użyciu systemu Windows Server 2008 R2, IIS 7.5, SQL Server 2008 R2, programu ASP.NET 4.0 i ASP.NET MVC 3.

## <a name="other-tutorials-in-this-series"></a>Innych samouczków w tej serii

To jest częścią serii samouczków pięć w skali przedsiębiorstwa wdrożenia sieci web. Są to innych samouczków z serii:

- [Wdrażanie aplikacji sieci Web w scenariuszach dla przedsiębiorstw](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md). Ta zawartość wprowadzające udostępnia kontekstowe tła serii samouczka. Opisuje samouczek scenariusza i przedstawia, jak zadania i wskazówki, które opisano w całym serii pasuje do szerszego procesu zarządzania cyklem życia aplikacji (ALM).
- [Narzędzie Web Deployment w przedsiębiorstwie](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Ten samouczek zawiera koncepcyjnej wprowadzenie do plików projektów MSBuild, Web potok publikowania (WPP), narzędzie Web Deploy i innych technologii pokrewnych. Wyjaśniono sposób korzystania tych narzędzi wspólnie do zarządzania procesami złożone wdrożenia.
- [Konfigurowanie środowisk serwera sieci Web wdrożenia](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md). W tym samouczku opisano sposób konfigurowania serwerów systemu Windows do obsługi różnych scenariuszy wdrażania, w tym wdrażania pakietu sieci web do zdalnego przy użyciu usługi sieci Web wdrożenia Agent (agent zdalnego) lub program obsługi wdrażania w sieci Web i wdrożenia zdalnej bazy danych. Go znajdują się wskazówki dotyczące wybierania metody wdrażania właściwe dla własnego środowiska i przedstawiono sposób używać struktury farmy sieci Web (WFF) w celu replikowania wdrożonych aplikacji sieci web na wszystkich serwerach sieci web w farmie serwerów.
- [Zaawansowane wdrażanie w przedsiębiorstwie sieci Web](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). Ten przewodnik opisuje sposób wykonywania różnych bardziej zaawansowanych zadań wdrażania, jak dostosowywanie wdrożenia bazy danych w wielu środowiskach, z wyjątkiem plików i folderów z wdrożenia i pobieranie aplikacji sieci web w trybie offline podczas procesu wdrażania .

>[!div class="step-by-step"]
[Dalej](creating-a-team-project-in-tfs.md)
