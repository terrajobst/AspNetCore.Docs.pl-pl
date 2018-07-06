---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment
title: Konfigurowanie serwera Team Foundation Server dla wdrażania w sieci Web | Dokumentacja firmy Microsoft
author: jrjlee
description: Ten samouczek przedstawia sposób konfigurowania Team Foundation Server (TFS) 2010 do tworzenia rozwiązań i wdrażania zawartości sieci web w różnych środowiskach docelowych. To...
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: ff55233a-e795-4007-a4fc-861fe1bb590b
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 0155f8dc4ca05a91ed8921f83aa6fa1b0b59c1a5
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37829690"
---
<a name="configuring-team-foundation-server-for-web-deployment"></a>Konfigurowanie serwera Team Foundation Server dla wdrażania w sieci Web
====================
przez [Jason Lee](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Ten samouczek przedstawia sposób konfigurowania Team Foundation Server (TFS) 2010 do tworzenia rozwiązań i wdrażania zawartości sieci web w różnych środowiskach docelowych. Dotyczy to również scenariuszach ciągłej integracji (CI), gdzie wdrażania zawartości automatycznie za każdym razem, gdy deweloper wprowadza zmianę. Może również obejmować wyzwalacza ręcznego scenariuszy, w którym administrator może wykonać wyzwalacza wdrażanie określonej kompilacji w środowisku przejściowym, gdy kompilacja zostanie zweryfikowane i sprawdzone w środowisku testowym. Tematy w tym samouczku przeprowadzi Cię przez proces całą konfigurację, w tym:
> 
> - Sposób tworzenia nowego projektu zespołowego w programie TFS.
> - Jak dodawać zawartość do kontroli źródła.
> - Jak skonfigurować serwer kompilacji do obsługi ciągłej integracji i wdrażania.
> - Jak utworzyć definicję kompilacji, który zawiera logikę wdrożenia.
> - Jak skonfigurować uprawnienia dla automatycznego wdrażania.
> 
> Włoska tłumaczenia w tych samouczkach, odwiedź stronę [ http://www.lucamorelli.it ](http://www.lucamorelli.it).


Ten samouczek zakłada, że zainstalowano TFS 2010 i utworzyć kolekcję projektów zespołowych w ramach procesu konfiguracji początkowej. [Team Foundation podręczniku instalacji programu Visual Studio 2010](https://go.microsoft.com/?linkid=9805132) zapewnia kompleksowe wskazówki na temat tych zadań.

## <a name="context"></a>Kontekst

To jest częścią serii samouczków, w oparciu o wymagania dotyczące wdrażania enterprise fikcyjnej firmy o nazwie firmy Fabrikam, Inc. Przykładowe rozwiązanie korzysta z tej serii samouczków&#x2014; [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) rozwiązania&#x2014;do reprezentowania aplikacji sieci web przy użyciu realistycznej stopień złożoności, łącznie z aplikacją ASP.NET MVC 3 komunikacji Windows Usługa Foundation (WCF), a projekt bazy danych.

Metody wdrażania w ramach tego samouczka opiera się na podejście pliku projektu Podziel opisane w [objaśnienie procesu kompilacji](../web-deployment-in-the-enterprise/understanding-the-build-process.md), w którym proces kompilacji jest kontrolowana przez dwa pliki projektu&#x2014;jeden zawierający Tworzenie instrukcji, które mają zastosowanie do każdego środowiska docelowego i jeden zawierający ustawienia specyficzne dla środowiska kompilacji i wdrażania. W czasie kompilacji pliku projektu specyficznymi dla środowiska jest scalana w pliku projektu niezależnego od środowiska w celu utworzenia kompletny zestaw instrukcji kompilacji.

## <a name="scenario-overview"></a>Omówienie scenariusza

Scenariusz wysokiego poziomu w tych samouczkach opisano w [wdrażania sieci Web w przedsiębiorstwie: omówienie scenariusza](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md). Firma Microsoft zaleca, przejrzyj ten temat przed rozpoczęciem tego samouczka.

## <a name="how-to-use-this-tutorial"></a>Jak korzystać z tego samouczka

Jeśli po raz pierwszy zostały wykonane zadania opisane w tym samouczku, lub jeśli chcesz pracować z przykładami, za pomocą przykładowe rozwiązanie możesz powinni skorzystać z samouczka tematów w kolejności. Alternatywnie można użyć poszczególnych tematów jako wskazówkę do wykonywania określonych zadań. Ten samouczek obejmuje następujące tematy:

- [Tworzenie projektu zespołowego w TFS](creating-a-team-project-in-tfs.md). Projekt zespołowy jest jednostką core kontroli źródła, zarządzanie procesem i kompilacji w programie TFS. Należy utworzyć projekt zespołowy, aby można było dodać zawartość do kontroli źródła lub Utwórz definicje kompilacji.
- [Dodawanie zawartości do kontroli źródła](adding-content-to-source-control.md). Po utworzeniu projektu zespołowego, możesz rozpocząć dodawanie zawartości do kontroli źródła. Musisz dodać swoje projekty i rozwiązania, wraz z wszelkich zależności zewnętrznych, przed rozpoczęciem konfigurowania kompilacji.
- [Konfigurowanie TFS serwer kompilacji dla wdrażania w Internecie](configuring-a-tfs-build-server-for-web-deployment.md). Jeśli chcesz skompilować zawartości projektu zespołowego, należy skonfigurować serwer kompilacji. W większości przypadków powinno to być na osobnym komputerze z instalacji programu TFS. Aby skonfigurować serwer kompilacji, należy zainstalować i skonfigurować usługę kompilacji serwera TFS, zainstaluj program Visual Studio 2010, tworzenie kontrolerów kompilacji i agentów kompilacji, zainstalowanie produktów lub składniki, których potrzebuje Twój kod, aby można było pomyślnie przeprowadzać kompilacje i zainstaluj Internetowe usługi informacyjne (IIS w) sieci Web narzędzia Deployment (Web Deploy).
- [Tworzenie definicji kompilacji, który obsługuje wdrażanie](creating-a-build-definition-that-supports-deployment.md). Przed rozpoczęciem kolejkowania lub wyzwolenie kompilacji w programie TFS, musisz utworzyć co najmniej jedną definicję kompilacji dla projektu zespołowego. Definicja kompilacji definiuje każdy aspekt kompilacji, w tym rzeczy, które powinny być uwzględnione w kompilacji, jakie zdarzenia powinny wywoływać kompilacji i gdzie Team Build wysłać dane wyjściowe kompilacji. Można skonfigurować definicję kompilacji do uruchamiania niestandardowych plików projektu aparatu Microsoft Build Engine (MSBuild), który pozwala umieścić logiki wdrożenia w kompilacjach zautomatyzowanych.
- [Wdrażanie określonej kompilacji](deploying-a-specific-build.md). Wiele scenariuszy warto wdrożyć konkretnej kompilacji, zamiast najnowszej kompilacji do środowiska docelowego. W takim przypadku można skonfigurować definicję kompilacji, która wdraża zawartość z folderu wrzucania określone.
- [Konfigurowanie uprawnień dla zespołu wdrożenia kompilacji](configuring-permissions-for-team-build-deployment.md). W przypadku usługi kompilacji do wdrażania zawartości w ramach zautomatyzowanego procesu kompilacji, należy przyznać różne uprawnienia do konta usługi kompilacji na serwerach docelowych z sieci web i serwery baz danych.

## <a name="key-technologies"></a>Kluczowe technologie

Ten samouczek koncentruje się na temat korzystania z tych produktów i technologii umożliwiających automatyczne kompilowanie i wdrażanie w Internecie:

- Visual Studio Team Foundation Server 2010
- Kompilacja zespołowa i MSBuild
- Web Deploy

Samouczek styka się również przy użyciu systemu Windows Server 2008 R2, IIS 7.5, SQL Server 2008 R2, programu ASP.NET 4.0 i ASP.NET MVC 3.

## <a name="other-tutorials-in-this-series"></a>Inne samouczki w tej serii

To jest częścią serii samouczków pięć w skali przedsiębiorstwa wdrażanie w Internecie. Poniżej przedstawiono inne samouczki z serii:

- [Wdrażanie aplikacji sieci Web w scenariuszach dla przedsiębiorstw](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md). Ta zawartość wprowadzające zapewnia kontekstowe tła serii samouczków. Scenariusz samouczka opisano w nim, a następnie pokazano, jak zadania i wskazówki, które opisano w całej serii mieści się w szerszym procesu zarządzania cyklem życia aplikacji (ALM).
- [Narzędzie Web Deployment w przedsiębiorstwie](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Ten samouczek zawiera ogólne wprowadzenie do plików projektu MSBuild, sieci Web potok publikowania (WPP), narzędzie Web Deploy i inne powiązane technologie. Jej wyjaśnia, jak można używać tych narzędzi wspólnie do zarządzania procesami złożonego wdrożenia.
- [Konfigurowanie środowisk serwera na potrzeby wdrażania w Internecie](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md). W tym samouczku opisano sposób konfigurowania serwerów Windows do obsługi różnych scenariuszy wdrażania, w tym wdrażania pakietu sieci web do zdalnego za pomocą usługi sieci Web Deployment Agent (agent zdalny) lub program obsługi wdrażania w sieci Web oraz zdalnej bazy danych wdrażania. Zwraca uwagę na wybór metody wdrożenia odpowiednie dla Twojego środowiska i opisano w nim sposób używania struktury farmy sieci Web (WFF) do replikacji wdrożonych aplikacji sieci web na wszystkich serwerach sieci web w farmie serwerów.
- [Zaawansowane wdrażanie w Internecie Enterprise](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). W tym samouczku opisano sposób wykonywania różnych bardziej zaawansowanych zadań wdrażania, takich jak dostosowywanie wdrożeń bazy danych w wielu środowiskach, wykluczanie plików i folderów z wdrożenia i przełączania w tryb offline w aplikacjach sieci web podczas procesu wdrażania .

> [!div class="step-by-step"]
> [Next](creating-a-team-project-in-tfs.md)
