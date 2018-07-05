---
uid: web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios
title: Wdrażanie aplikacji sieci Web w scenariuszach dla przedsiębiorstw przy użyciu programu Visual Studio 2010 | Dokumentacja firmy Microsoft
author: jrjlee
description: Tego zestawu samouczków opisano narzędzia i techniki, które służy do wdrażania aplikacji sieci web w różnych scenariuszach dla przedsiębiorstw. Wyjaśniono, jak najlepiej wykorzystać...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/03/2012
ms.topic: article
ms.assetid: 48cfe378-d62a-48c6-a4db-6be3cead6898
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios
msc.type: authoredcontent
ms.openlocfilehash: 8412000e150f59911bb38f0147b1a487bef60c18
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37376841"
---
<a name="deploying-web-applications-in-enterprise-scenarios-using-visual-studio-2010"></a>Wdrażanie aplikacji sieci Web w scenariuszach dla przedsiębiorstw przy użyciu programu Visual Studio 2010
====================
przez [Jason Lee](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Tego zestawu samouczków opisano narzędzia i techniki, które służy do wdrażania aplikacji sieci web w różnych scenariuszach dla przedsiębiorstw. Wyjaśniono, jak najlepiej korzystać z technologii, takich jak Visual Studio 2010, aparatu Microsoft Build Engine (MSBuild), Internet Information Services (IIS) 7.5, narzędzie wdrażania sieci Web usług IIS (Web Deploy), Framework kolektywu serwerów sieci Web (WFF) i narzędzi, takich jak VSDBCMD.exe do Uprość i zarządzanie procesem wdrażania. Zawiera omówienia pojęć i wskazówki zadań, który pomoże Ci:
> 
> - Przejrzyj i ustanowić wymagania dotyczące wdrażania dla aplikacji sieci web w skali przedsiębiorstwa.
> - Konfigurowanie testów, przemieszczania i produkcji środowiska serwera sieci web do obsługi wdrażania w Internecie.
> - Skonfiguruj procesów ciągłej integracji (CI) Team Foundation Server (TFS) do obsługi wdrażania w Internecie automatycznych.
> - Wdrażanie aplikacji sieci web skali korporacyjnej dla środowisk innym serwerem przy użyciu różnych wymagań i ograniczeń.
> - Wdrożyć zmiany w aplikacji sieci web, które są uruchomione w środowiskach inny serwer.
> 
> > [!NOTE]
> > Gdy w tych samouczkach opisano użycie programu TFS jako serwer ciągłej integracji, na dowolnym serwerze CI łatwo dostosowuje się się ze wskazówkami. Nie musisz szczegółową wiedzę na TFS, aby zrozumieć i korzystać z samouczków.
> 
> 
> Włoska tłumaczenia w tych samouczkach, odwiedź stronę [ http://www.lucamorelli.it ](http://www.lucamorelli.it).


## <a name="about-the-authors"></a>O autorach

JASON jest główną technolog z [wzorca zawartości](http://www.contentmaster.com/) gdzie on pracował nad z produktów firmy Microsoft i technologie, szczególnie programu SharePoint i programie ASP.NET: od kilku lat. JASON posiada tytuł doktora w obliczeniowej i jest obecnie MCPD MCTS certyfikat. Może odczytywać firmy Jason blog techniczny w [www.jrjlee.com](http://www.jrjlee.com/).

Benjamin Curry jest główną technolog z [wzorca zawartości](http://www.contentmaster.com/) który zapisał oficjalne dokumenty, dokumentacja zestawu SDK, prezentacje programu PowerPoint i szkolenia prowadzone przez instruktorów i w trybie online podczas kariery zawodowej. Oryginalny element członkowski zespół dokumentacji platformy ASP.NET, pracował on za pomocą technologii sieci web firmy Microsoft dla ponad dziesięciu lat.

## <a name="target-audience"></a>Docelowi odbiorcy

Jest tym zestawie samouczków dla deweloperów aplikacji sieci web platformy ASP.NET i architektom rozwiązań, którzy korzystają z programu Visual Studio 2010 do tworzenia aplikacji sieci web w skali przedsiębiorstwa. Aby uzyskać maksymalne korzyści z zawartości, powinien być komfortowo, jednocześnie przy użyciu programu Visual Studio 2010 i masz podstawowe znajomość TFS, razem z świadomości technologii platformy sieci web firmy Microsoft, takich jak ASP.NET MVC 3, Windows Communication Foundation (WCF), usługi IIS, SQL Serwer i projektów bazy danych programu Visual Studio. Jednak nie trzeba znać narzędzia do wdrażania i technologii, lub musi wiedzieć, jak skonfigurować systemy ciągłej integracji.

## <a name="requirements"></a>Wymagania

Postępuj zgodnie z instrukcji opisanych w przewodnikach i wykonywać zadania, które opisują te samouczki, będą potrzebne do zainstalowania tego oprogramowania na komputerze deweloperskim:

- Visual Studio 2010 Premium lub Ultimate Edition z dodatkiem Service Pack 1
- .NET framework 4.0
- .NET framework 3.5 z dodatkiem Service Pack 1
- ASP.NET MVC 3.0
- Usługi IIS 7.5 Express
- SQL Server Express 2008 R2

Przeprowadzenie wdrożenia opisane w tych przewodników, musisz mieć dostęp do środowiska wdrażania aplikacji sieci Web przykładowej. Aby uzyskać najlepsze wyniki tych środowisk powinny odzwierciedlać wzorca wdrażania przedsiębiorstwa w Twojej organizacji. Następnie można zmodyfikować wskazówki podane w tej dokumentacji, aby odzwierciedlić środowisk wdrażania i wymagań swojej organizacji.

## <a name="series-contents"></a>Zawartość serii

W tej sekcji wprowadzające składa się z dwóch dodatkowe tematy. Te są przeznaczone do zapewnienia niektóre szerszym kontekście samouczków, które należy wykonać:

- [Wdrażanie sieci Web w przedsiębiorstwie: Omówienie scenariusza](enterprise-web-deployment-scenario-overview.md). W tym temacie opisano scenariusz, który stanowi podstawę dla każdego z samouczków w tej serii. Scenariusz koncentruje się na wymagania dotyczące zarządzania cyklem życia aplikacji (ALM) fikcyjnej firmy o nazwie firmy Fabrikam, Inc., ponieważ rozwija aplikację sieci web w skali przedsiębiorstwa.
- [Zarządzanie cyklem życia aplikacji: Od projektowania do produkcji](application-lifecycle-management-from-development-to-production.md). Ten temat zawiera omówienie wysokiego poziomu, end-to-end procesu wdrażania. Przykład ilustruje sposób Fabrikam, Inc. przechodzi skali korporacyjnej aplikację sieci web ASP.NET za pomocą środowisk testowania, przejściowego i produkcji jako część procesu ciągłego rozwoju.

Seria zawiera cztery zestawy samouczka. Każdy koncentruje się na różnych aspektów wdrażania w Internecie:

- [Narzędzie Web Deployment w przedsiębiorstwie](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Ten samouczek zawiera ogólne wprowadzenie do plików projektu MSBuild, potoku publikowania w sieci Web, narzędzie Web Deploy i inne powiązane technologie. Jej wyjaśnia, jak można używać tych narzędzi wspólnie do zarządzania procesami złożonego wdrożenia.
- [Konfigurowanie środowisk serwera na potrzeby wdrażania w Internecie](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md). W tym samouczku opisano sposób konfigurowania serwerów Windows do obsługi różnych scenariuszy wdrażania, w tym wdrażania pakietu sieci web do zdalnego przy użyciu usługi agenta wdrażania sieci Web ("agent zdalny") lub program obsługi wdrażania w sieci Web i wdrożenia zdalnej bazy danych. Zwraca uwagę na wybór metody wdrożenia odpowiednie dla Twojego środowiska i opisano w nim sposób używania WFF replikowanie wdrożonych aplikacji sieci web na wszystkich serwerach sieci web w farmie serwerów.
- [Konfigurowanie serwera Team Foundation Server na potrzeby wdrażania w Internecie](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). W tym samouczku opisano sposób konfigurowania programu TFS do obsługi różnych scenariuszy wdrażania, w tym zautomatyzowane wdrożenia w ramach procesu ciągłej integracji i ręcznie wyzwolony wdrożeń z konkretnymi kompilacjami.
- [Zaawansowane wdrażanie w Internecie Enterprise](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). W tym samouczku opisano sposób wykonywania różnych bardziej zaawansowanych zadań wdrażania, takich jak dostosowywanie wdrożeń bazy danych w wielu środowiskach, wykluczanie plików i folderów z wdrożenia i przełączania w tryb offline w aplikacjach sieci web podczas procesu wdrażania .

## <a name="where-to-start"></a>Gdzie zacząć

Tego zestawu samouczków używa przykładowe rozwiązanie z realistyczne stopień złożoności, wraz z fikcyjnego wdrożenia zrealizować scenariusz przedsiębiorstwa, zapewniają implementację referencyjną i wydawanie zadania i wskazówki dotyczące typowych kontekstu. Następny temat [wdrażania sieci Web w przedsiębiorstwie: omówienie scenariusza](enterprise-web-deployment-scenario-overview.md), zawiera wprowadzenie do scenariusza i przykładowe rozwiązanie. W tym miejscu możesz pracować za pomocą samouczków i tematów, które najlepiej odpowiadają potrzebom użytkownika.

> [!div class="step-by-step"]
> [Next](enterprise-web-deployment-scenario-overview.md)
