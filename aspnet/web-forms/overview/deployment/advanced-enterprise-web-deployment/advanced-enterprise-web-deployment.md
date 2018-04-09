---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/advanced-enterprise-web-deployment
title: Zaawansowane wdrożenia sieci Web w przedsiębiorstwie | Dokumentacja firmy Microsoft
author: jrjlee
description: Ten samouczek przedstawia sposób wykonywania różnych zadań, które są wymagane lub pożądane wiele scenariuszy wdrażania w przedsiębiorstwie. Dla Włoska translati...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 7dcaba80-f2ec-4db3-ad98-daadc3afdb49
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/advanced-enterprise-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 892e494b6fde994c4d04952382e4d618d73cad5c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="advanced-enterprise-web-deployment"></a>Wdrażanie sieci Web Zaawansowane Enterprise
====================
przez [Lewandowski Jason](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Ten samouczek przedstawia sposób wykonywania różnych zadań, które są wymagane lub pożądane wiele scenariuszy wdrażania w przedsiębiorstwie.
> 
> Włoska translacji tego samouczka, odwiedź stronę [ http://www.lucamorelli.it ](http://www.lucamorelli.it).


To jest częścią serii samouczków na podstawie tych wymagań związanych z przedsiębiorstwa wdrażaniem fikcyjnej firmy o nazwie firmy Fabrikam, Inc. Przykładowe rozwiązanie korzysta z tego samouczka serii&#x2014; [kontaktów Menedżerze](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) rozwiązania&#x2014;do reprezentowania aplikacji sieci web z realistyczne poziom złożoności, w tym aplikacji ASP.NET MVC 3, Windows Communication Usługa Foundation (WCF), a projekt bazy danych.

Istotą te samouczki metody wdrażania opiera się na podejście pliku projektu podziału opisane w [opis procesu kompilacji](../web-deployment-in-the-enterprise/understanding-the-build-process.md), w którym jest kontrolowany przez proces kompilacji dwa pliki projektu&#x2014;jeden zawierający Tworzenie instrukcji, które mają zastosowanie do każdego środowiska docelowego i dysk zawierający ustawienia kompilacji i wdrożenia określonego środowiska. W czasie kompilacji pliku projektu określonego środowiska jest scalany pliku projektu niezależny od środowiska pełny zestaw instrukcji kompilacji.

## <a name="scenario-overview"></a>Omówienie scenariusza

Scenariusz wysokiego poziomu te samouczki jest opisany w [wdrożenia sieci Web w przedsiębiorstwie: omówienie scenariusza](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md). Firma Microsoft zaleca, przejrzyj ten temat przed rozpoczęciem tego samouczka.

## <a name="how-to-use-this-tutorial"></a>Jak używać tego samouczka

- Każdy z tematów w tym samouczku jest niezależna i dotyczy konkretnego żądania lub problem występujący w scenariuszach wdrażania w przedsiębiorstwie. Nie musisz skorzystać z tych tematów w określonej kolejności. Jednak w tym samouczku omówiono niektóre zaawansowane zadania. Tak, należy zapoznać się z pojęciami i techniki który [Web Deployment w przedsiębiorstwie](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md) samouczek obejmuje w celu uzyskania największej korzyści z tej zawartości.
- W tym samouczku omówiono następujące zagadnienia:
- [Wykonywanie wdrożenia "Co w przypadku"](performing-a-what-if-deployment.md). W wiele scenariuszy należy do określenia wpływu proponowanych wdrożenia w środowisku docelowym lub istniejącą zawartość przed wprowadzeniem faktycznie wszelkie zmiany. W tym temacie opisano, jak uruchomić wdrożenia "co w przypadku" do generowania skryptów aktualizacji bazy danych i plików dziennika, tak, jakby zawartość w środowisku docelowym wdrożyli bez wprowadzania zmian. Analizowanie tych zasobów może pomóc dodatkowe potencjalne problemy klienta z wyprzedzeniem wdrożenia na żywo.
- [Dostosowywanie wdrożenia bazy danych w wielu środowiskach](customizing-database-deployments-for-multiple-environments.md). Podczas wdrażania projektu bazy danych do wielu miejsc docelowych, często należy dostosować właściwości wdrożenia dla każdego środowiska docelowego. Na przykład w środowisku testowym będzie zazwyczaj ponownego tworzenia bazy danych przy każdym wdrożeniu w środowisku tymczasowym czy produkcyjnym może być znacznie bardziej prawdopodobne zachować dane aktualizacje przyrostowe. W tym temacie opisano, jak można zastosować te zmiany właściwości w logice wdrożenia przez utworzenie pliku konfiguracji (.sqldeployment) określonego środowiska wdrażania dla każdego środowiska docelowego.
- Wdrażanie członkostwo roli bazy danych do środowiska testowego. Podczas ponownego tworzenia bazy danych przy każdym wdrożeniu&#x2014;na przykład w ramach ciągłej integracji (CI) tworzenia i wdrażania w środowisku testowym&#x2014;zwykle należy skonfigurować członkostwo roli bazy danych zawsze. Na przykład zazwyczaj należy udzielić uprawnień do tożsamości puli aplikacji, które są skojarzone z aplikacją sieci web. W tym temacie opisano, jak można zautomatyzować ten proces przez dodanie skryptu SQL po wdrożeniu do logiki wdrożenia.
- [Wdrożenie bazy danych członkostwa w środowiskach przedsiębiorstw](deploying-membership-databases-to-enterprise-environments.md). Bazy danych członkostwa ASP.NET mają różne właściwości, które skomplikować procesu wdrażania. Na przykład wdrożenie tylko schematu pozostawi bazy danych w działającym stanie stanu. W większości przypadków zaleca się utworzenie bazy danych członkostwa bezpośrednio w każdym środowisku docelowym. Jednak jeśli trzeba wdrożyć bazę danych członkostwa, w tym temacie opisano niektóre z metod używanych w celu spełnienia wyzwania związane.
- [Wykluczanie plików i folderów z wdrożenia](excluding-files-and-folders-from-deployment.md). W niektórych scenariuszach należy dostosować zawartość pakietu sieci web do określonego miejsca docelowego środowisk. Na przykład możesz chcieć obejmują pełne wersje bibliotek JavaScript podczas wdrażania w środowisku testowym obsługuje debugowania po stronie klienta, ale zminimalizowany wersje bibliotek podczas wdrażania w środowisku tymczasowym czy produkcyjnym. W tym temacie opisano, jak można wykluczyć określone pliki i foldery z proces tworzenia pakietu.
- [Wdrażanie pobierania aplikacji sieci Web w trybie Offline z sieci Web](taking-web-applications-offline-with-web-deploy.md). Podczas wdrażania rozwiązania w środowisku tymczasowym czy produkcyjnym, często należy wykonać w czasie trwania procesu wdrażania aplikacji sieci web w trybie offline. W tym temacie opisano sposób dodawania *aplikacji\_offline.htm* plików do aplikacji sieci web w chwili rozpoczęcia procesu wdrażania i usunąć go na końcu. Gdy *aplikacji\_offline.htm* plik znajduje się w miejscu, wszyscy użytkownicy, którzy przejdź do aplikacji sieci web są automatycznie przekierowywane do *aplikacji\_offline.htm* pliku.
- [Uruchamianie skryptów programu PowerShell systemu Windows z MSBuild](running-windows-powershell-scripts-from-msbuild-project-files.md). Wiele scenariuszy wdrażania wymaga bardziej złożonej akcje po wdrożeniu, takich jak dodawanie źródła zdarzeń niestandardowych w rejestrze lub konfigurowania replikacji między wystąpieniami programu SQL Server. Te akcje są często wykonywane za pomocą skryptów środowiska Windows PowerShell. W tym temacie opisano sposób uruchamiania skryptów programu Windows PowerShell z pliku projektu Microsoft kompilacji Engine (MSBuild) jako część procesu kompilacji i wdrożenia.
- [Rozwiązywanie problemów z proces tworzenia pakietu](troubleshooting-the-packaging-process.md). Web potok publikowania (WPP) Definiuje właściwości programu MSBuild o nazwie **EnablePackageProcessLoggingAndAssert** można wygenerować szczegółowe informacje na temat procesu tworzenia pakietów dla projektów aplikacji sieci web. W tym temacie opisano, co oznacza właściwości i jak z niego korzystać.

## <a name="key-technologies"></a>Kluczowe technologie

Ten samouczek koncentruje się na temat korzystania z tych produktów i technologii do obsługi automatycznych kompilowanie i wdrażanie w sieci web:

- Program Visual Studio 2010 i Team Foundation Server (TFS) 2010
- MSBuild i kompilacji Team TFS
- Internetowe usługi informacyjne (IIS) 7.5
- Narzędzia wdrażania Web usług IIS (Web Deploy) 2.1
- Narzędzie wdrażania VSDBCMD.exe baz danych

## <a name="other-tutorials-in-this-series"></a>Innych samouczków w tej serii

To jest częścią serii samouczków pięć w skali przedsiębiorstwa wdrożenia sieci web. Są to innych samouczków z serii:

- [Wdrażanie aplikacji sieci Web w scenariuszach dla przedsiębiorstw](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md). Ta zawartość wprowadzające udostępnia kontekstowe tła serii samouczka. Opisuje samouczek scenariusza i przedstawia, jak zadania i wskazówki, które opisano w całym serii pasuje do szerszego procesu zarządzania cyklem życia aplikacji (ALM).
- [Narzędzie Web Deployment w przedsiębiorstwie](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Ten samouczek zawiera koncepcyjnej wprowadzenie do plików projektu MSBuild, WPP narzędzia Web Deploy i innych technologii pokrewnych. Wyjaśniono sposób korzystania tych narzędzi wspólnie do zarządzania procesami złożone wdrożenia.
- [Konfigurowanie środowisk serwera sieci Web wdrożenia](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md). W tym samouczku opisano sposób konfigurowania serwerów systemu Windows do obsługi różnych scenariuszy wdrażania, w tym wdrażania pakietu sieci web do zdalnego przy użyciu usługi sieci Web wdrożenia Agent (agent zdalnego) lub program obsługi wdrażania w sieci Web i wdrożenia zdalnej bazy danych. Go znajdują się wskazówki dotyczące wybierania metody wdrażania właściwe dla własnego środowiska i przedstawiono sposób używać struktury farmy sieci Web (WFF) w celu replikowania wdrożonych aplikacji sieci web na wszystkich serwerach sieci web w farmie serwerów.
- [Konfigurowanie serwera Team Foundation Server Web Deployment](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). W tym samouczku opisano sposób konfigurowania TFS w celu obsługi różnych scenariuszy wdrażania, łącznie z automatycznego wdrażania w trakcie procesu konfiguracji i ręcznie wyzwalane wdrożeń określonej kompilacji.

> [!div class="step-by-step"]
> [Next](performing-a-what-if-deployment.md)
