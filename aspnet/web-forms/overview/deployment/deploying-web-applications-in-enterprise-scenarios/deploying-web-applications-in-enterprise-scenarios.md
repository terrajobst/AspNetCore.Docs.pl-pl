---
uid: web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios
title: Wdrażanie aplikacji sieci Web w scenariuszach dla przedsiębiorstw przy użyciu programu Visual Studio 2010 | Dokumentacja firmy Microsoft
author: jrjlee
description: Ten zestaw samouczków opisano narzędzia i metod, które służy do wdrażania aplikacji sieci web w różnych scenariuszach dla przedsiębiorstwa. Wyjaśniono, jak najlepiej wykorzystać...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/03/2012
ms.topic: article
ms.assetid: 48cfe378-d62a-48c6-a4db-6be3cead6898
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios
msc.type: authoredcontent
ms.openlocfilehash: 921b1ccd8a1f2109a51f3f75149588422fefb91d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30890232"
---
<a name="deploying-web-applications-in-enterprise-scenarios-using-visual-studio-2010"></a>Wdrażanie aplikacji sieci Web w scenariuszach dla przedsiębiorstw przy użyciu programu Visual Studio 2010
====================
przez [Lewandowski Jason](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Ten zestaw samouczków opisano narzędzia i metod, które służy do wdrażania aplikacji sieci web w różnych scenariuszach dla przedsiębiorstwa. Wyjaśniono, jak najlepiej korzystać z technologii, takich jak Visual Studio 2010, aparat kompilacji firmy Microsoft (MSBuild), Internet informacji Services (IIS) 7.5, narzędzia wdrażania usług IIS sieci Web (Web Deploy), Framework kolektywu serwerów sieci Web (WFF) i narzędzi, takich jak VSDBCMD.exe do uproszczenie i zarządzanie procesem wdrażania. Zawiera omówienie pojęć i wskazówki zorientowane na zadania, który pomoże Ci:
> 
> - Przejrzyj i ustanowienia wymagań związanych z wdrażaniem aplikacji sieci web skali przedsiębiorstwa.
> - Skonfiguruj test, tymczasowych i produkcyjnych środowiska serwera sieci web do obsługi wdrożenia sieci web.
> - Skonfiguruj procesów ciągłej integracji (CI) Team Foundation Server (TFS) do obsługi wdrażania automatycznego sieci web.
> - Wdrażanie aplikacji sieci web skali przedsiębiorstwa do innego serwera środowisk przy użyciu różnych wymagań i ograniczeń.
> - Wdróż zmian w aplikacji sieci web, które działają w środowiskach inny serwer.
> 
> > [!NOTE]
> > Gdy te samouczki opisano użycie TFS jako serwer elementu konfiguracji, wskazówki łatwo jest dostosowane do dowolnego serwera CI. Nie trzeba szczegółowej znajomości TFS, aby poznać i korzystać z samouczków.
> 
> 
> Włoska translacji tego samouczka, odwiedź stronę [ http://www.lucamorelli.it ](http://www.lucamorelli.it).


## <a name="about-the-authors"></a>O autorów

JASON jest główną technologist z [wzorca zawartości](http://www.contentmaster.com/) gdzie on pracuje z produktów firmy Microsoft i technologie, szczególnie programu SharePoint i ASP.NET, przez kilka lat. JASON zawiera Praca w przypadku komputerów i jest obecnie MCPD i MCTS certyfikowane. Możesz przeczytać Jason w blogu techniczne w [www.jrjlee.com](http://www.jrjlee.com/).

Curry Benjaminowi jest główną technologist z [wzorca zawartości](http://www.contentmaster.com/) który został zapisany oficjalnych dokumentów, dokumentacji zestawu SDK prezentacji programu PowerPoint i prowadzone i online szkoleń podczas jego zawodowych. Oryginalnego elementu członkowskiego zespołu programu ASP.NET, on współpracuje z technologii sieci web firmy Microsoft za pośrednictwem dekadę.

## <a name="target-audience"></a>Odbiorcy docelowi

Jest to zestaw samouczków dla deweloperów aplikacji sieci web ASP.NET i architekci rozwiązań, korzystających z programu Visual Studio 2010 do tworzenia aplikacji sieci web w skali przedsiębiorstwa. Aby uzyskać najbardziej wartości z zawartości, należy wiedzieć, jak za pomocą programu Visual Studio 2010 i mieć podstawowe znajomość TFS, wraz z pogłębianie wiedzy na temat technologii platformy sieci web firmy Microsoft, takich jak ASP.NET MVC 3, Windows Communication Foundation (WCF), usługi IIS, SQL Serwer i projektów bazy danych programu Visual Studio. Jednak nie trzeba znać technologie i narzędzia wdrażania lub trzeba wiedzieć, jak skonfigurować elementu konfiguracji systemów.

## <a name="requirements"></a>Wymagania

Wykonaj wskazówki i wykonywać zadania, które opisują te samouczki, musisz zainstalować to oprogramowanie na komputerze deweloperskim:

- Visual Studio 2010 Premium lub Ultimate Edition z dodatkiem Service Pack 1
- .NET Framework 4.0
- .NET framework 3.5 z dodatkiem Service Pack 1
- ASP.NET MVC 3.0
- Usługi IIS 7.5 Express
- SQL Server Express 2008 R2

Do wykonania opisanych w całym te wskazówki dotyczące kroków wdrażania, musisz mieć dostęp do środowiska wdrażania aplikacji sieci Web przykładowej. Aby uzyskać najlepsze wyniki tych środowisk powinien odzwierciedlać wzorca wdrażania przedsiębiorstwa w organizacji. Następnie można zmodyfikować wskazówki zawarte w tej dokumentacji, aby uwzględnić w środowiskach wdrożenia oraz wymagań organizacji.

## <a name="series-contents"></a>Zawartość serii

W tej sekcji wprowadzające składa się z dwóch tematy dodatkowe. Te zaprojektowano niektórych szerszym kontekstem samouczki, które należy wykonać:

- [Wdrożenia sieci Web w przedsiębiorstwie: Omówienie scenariusza](enterprise-web-deployment-scenario-overview.md). W tym temacie opisano scenariusz, który stanowi podstawę dla każdego z samouczków w tej serii. Scenariusz koncentruje się na wymagania dotyczące zarządzania cyklem życia aplikacji (ALM) fikcyjnej firmy o nazwie firmy Fabrikam, Inc. miarę jej rozwoju aplikacji sieci web w skali przedsiębiorstwa.
- [Zarządzanie cyklem życia aplikacji: Od projektowania do produkcji](application-lifecycle-management-from-development-to-production.md). Ten temat zawiera omówienie wysokiego poziomu, end-to-end procesu wdrażania. Go przedstawiono, jak firma Fabrikam przenosi skali przedsiębiorstwa aplikacja sieci web ASP.NET za pośrednictwem środowiska testowego, tymczasowych i produkcyjnych w ramach procesu projektowania ciągłe.

Seria zawiera cztery zestawy samouczka. Każdy koncentruje się na różnych aspektów wdrożenia sieci web:

- [Narzędzie Web Deployment w przedsiębiorstwie](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Ten samouczek zawiera koncepcyjnej wprowadzenie do plików projektu MSBuild, potoku publikowania w sieci Web narzędzia Web Deploy i innych technologii pokrewnych. Wyjaśniono sposób korzystania tych narzędzi wspólnie do zarządzania procesami złożone wdrożenia.
- [Konfigurowanie środowisk serwera sieci Web wdrożenia](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md). W tym samouczku opisano sposób konfigurowania serwerów systemu Windows do obsługi różnych scenariuszy wdrażania, w tym wdrażania pakietu sieci web do zdalnego przy użyciu usługi sieci Web wdrożenia agenta ("agent zdalnego") lub program obsługi wdrażania w sieci Web i wdrożenia zdalnej bazy danych. Go znajdują się wskazówki dotyczące wybierania metody wdrażania właściwe dla własnego środowiska i opisuje sposób użycia WFF replikowanie wdrożonych aplikacji sieci web na wszystkich serwerach sieci web w farmie serwerów.
- [Konfigurowanie serwera Team Foundation Server Web Deployment](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). W tym samouczku opisano sposób konfigurowania TFS w celu obsługi różnych scenariuszy wdrażania, łącznie z automatycznego wdrażania w trakcie procesu konfiguracji i ręcznie wyzwalane wdrożeń określonej kompilacji.
- [Zaawansowane wdrażanie w przedsiębiorstwie sieci Web](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). Ten przewodnik opisuje sposób wykonywania różnych bardziej zaawansowanych zadań wdrażania, jak dostosowywanie wdrożenia bazy danych w wielu środowiskach, z wyjątkiem plików i folderów z wdrożenia i pobieranie aplikacji sieci web w trybie offline podczas procesu wdrażania .

## <a name="where-to-start"></a>Jak zacząć

Ten zestaw samouczki używa przykładowe rozwiązanie z realistyczne poziom złożoności, oraz scenariusz wdrażania fikcyjnej organizacji do implementacji odwołania i zadania i wskazówki dotyczące typowych kontekstu. Następnym temacie [wdrożenia sieci Web w przedsiębiorstwie: omówienie scenariusza](enterprise-web-deployment-scenario-overview.md), wprowadza scenariusza i przykładowe rozwiązanie. Z tego miejsca pracy za pośrednictwem samouczki i tematy, które najlepiej odpowiadają Twoim potrzebom.

> [!div class="step-by-step"]
> [Next](enterprise-web-deployment-scenario-overview.md)
