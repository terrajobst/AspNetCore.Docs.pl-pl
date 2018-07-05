---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/advanced-enterprise-web-deployment
title: Zaawansowane wdrażanie w Internecie Enterprise | Dokumentacja firmy Microsoft
author: jrjlee
description: Ten samouczek przedstawia sposób wykonywania różnych zadań, które są wymagane lub pożądane wiele scenariuszy wdrażania w przedsiębiorstwie. Dla włoskiego translati...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 7dcaba80-f2ec-4db3-ad98-daadc3afdb49
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/advanced-enterprise-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 7b4546079d15649ff2371ced6746a29a90bdfc85
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37401876"
---
<a name="advanced-enterprise-web-deployment"></a>Wdrażanie w Internecie zaawansowane przedsiębiorstwa
====================
przez [Jason Lee](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Ten samouczek przedstawia sposób wykonywania różnych zadań, które są wymagane lub pożądane wiele scenariuszy wdrażania w przedsiębiorstwie.
> 
> Włoska tłumaczenia w tych samouczkach, odwiedź stronę [ http://www.lucamorelli.it ](http://www.lucamorelli.it).


To jest częścią serii samouczków na podstawie wymagania dotyczące wdrażania enterprise fikcyjnej firmy o nazwie firmy Fabrikam, Inc. Przykładowe rozwiązanie korzysta z tej serii samouczków&#x2014; [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) rozwiązania&#x2014;do reprezentowania aplikacji sieci web przy użyciu realistycznej stopień złożoności, łącznie z aplikacją ASP.NET MVC 3 komunikacji Windows Usługa Foundation (WCF), a projekt bazy danych.

Metody wdrażania w ramach tego samouczka opiera się na podejście pliku projektu Podziel opisane w [objaśnienie procesu kompilacji](../web-deployment-in-the-enterprise/understanding-the-build-process.md), w którym proces kompilacji jest kontrolowana przez dwa pliki projektu&#x2014;jeden zawierający Tworzenie instrukcji, które mają zastosowanie do każdego środowiska docelowego i jeden zawierający ustawienia specyficzne dla środowiska kompilacji i wdrażania. W czasie kompilacji pliku projektu specyficznymi dla środowiska jest scalana w pliku projektu niezależnego od środowiska w celu utworzenia kompletny zestaw instrukcji kompilacji.

## <a name="scenario-overview"></a>Omówienie scenariusza

Scenariusz wysokiego poziomu w tych samouczkach opisano w [wdrażania sieci Web w przedsiębiorstwie: omówienie scenariusza](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md). Firma Microsoft zaleca, przejrzyj ten temat przed rozpoczęciem tego samouczka.

## <a name="how-to-use-this-tutorial"></a>Jak korzystać z tego samouczka

- Każdy z tematów, w tym samouczku jest niezależna i dotyczy danego żądania lub problem występujący w scenariuszach wdrażania w przedsiębiorstwie. Nie musisz trzymać się kolejności tych tematów w określonej kolejności. Jednak w tym samouczku omówiono niektóre zaawansowane zadania. Jako takie, należy zapoznać się z pojęciami i technik, [wdrażanie w Internecie w przedsiębiorstwie](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md) samouczek obejmuje w celu uzyskania najwięcej korzyści z tej zawartości.
- Ten samouczek obejmuje następujące tematy:
- [Wykonywanie wdrożenia "What If"](performing-a-what-if-deployment.md). W wielu scenariuszach należy określić wpływ proponowana wdrożenia w środowisku docelowym lub dowolnym istniejącą zawartość, przed faktycznie wprowadzone zmiany. W tym temacie opisano, jak można uruchomić wdrożenia "what if" można wygenerować skryptów aktualizacji bazy danych i plików dziennika, tak, jakby zawartość w środowisku docelowym wdrożyli bez wprowadzania zmian. Analizowanie te zasoby mogą ułatwić wykrywanie wszelkich potencjalnych problemów przed jej wprowadzeniem wdrożenia na żywo.
- [Dostosowywanie wdrożeń bazy danych dla wielu środowisk](customizing-database-deployments-for-multiple-environments.md). Podczas wdrażania projektu bazy danych do wielu miejsc docelowych, często warto dostosować właściwości wdrożenia dla każdego środowiska docelowego. Na przykład w środowiskach testowych będzie najczęściej ponownego tworzenia bazy danych przy każdym wdrożeniu w środowiskach przejściowych lub produkcyjnych może być znacznie częściej wprowadzić aktualizacje przyrostowe, aby zachować swoje dane. W tym temacie opisano, jak można zastosować te zmiany właściwości w logice wdrożenia, tworząc plik konfiguracji (.sqldeployment) specyficznymi dla środowiska wdrażania dla każdego środowiska docelowego.
- Wdrażanie członkostw ról bazy danych do środowisk testowych. Podczas ponownego tworzenia bazy danych przy każdym wdrożeniu&#x2014;na przykład w ramach ciągłej integracji (CI) tworzenie i wdrażanie w środowisku testowym&#x2014;zwykle należy skonfigurować członkostw roli bazy danych za każdym razem, gdy. Na przykład zazwyczaj należy przyznać uprawnienia do tożsamości puli aplikacji, które są skojarzone z aplikacją sieci web. W tym temacie opisano, jak zautomatyzować ten proces, dodając skrypt SQL po wdrożeniu do logiki wdrożenia.
- [Wdrażanie baz danych członkostwa w środowiskach przedsiębiorstw](deploying-membership-databases-to-enterprise-environments.md). Baz danych członkostwa ASP.NET mają różne cechy, które mogą skomplikować procesu wdrażania. Na przykład wdrożenie tylko schematy spowoduje pozostawienie bazy danych w działającym stanie. W większości przypadków najlepiej utworzyć bazę danych członkostwa bezpośrednio w każdym środowisku docelowym. Jednakże jeśli musisz wdrożyć bazę danych członkostwa, w tym temacie opisano niektóre z metod, których można użyć, aby sprostać wyzwaniom nieprzerwaną pracę.
- [Wykluczanie plików i folderów z wdrożenia](excluding-files-and-folders-from-deployment.md). W niektórych przypadkach należy dostosować zawartość sieci web pakietu do określonego miejsca docelowego środowiska. Na przykład możesz chcieć obejmują pełne wersje bibliotek JavaScript, w przypadku wdrażania w środowisku testowym, obsługuje debugowanie po stronie klienta, ale użyj zminimalizowany wersje bibliotek, podczas wdrażania w środowisku tymczasowym czy produkcyjnym. W tym temacie opisano, jak można wykluczyć określone pliki i foldery z procesu tworzenia pakietu.
- [Tworzenie aplikacji sieci Web w trybie Offline za pomocą sieci Web wdrażanie](taking-web-applications-offline-with-web-deploy.md). Podczas wdrażania rozwiązania w środowisku tymczasowym czy produkcyjnym często warto wykonać aplikacji sieci web w trybie offline na czas trwania procesu wdrażania. W tym temacie opisano sposób dodawania *aplikacji\_offline.htm* plików do aplikacji sieci web na początku procesu wdrażania i usunąć go na końcu. Gdy *aplikacji\_offline.htm* plik znajduje się w miejscu, użytkownicy, którzy przejdź do aplikacji sieci web są automatycznie przekierowywane do *aplikacji\_offline.htm* pliku.
- [Uruchamianie skryptów programu Windows PowerShell z programu MSBuild](running-windows-powershell-scripts-from-msbuild-project-files.md). Wiele scenariuszy wdrażania wymaga bardziej złożone akcje po wdrożeniu, takich jak dodawanie niestandardowych źródłach zdarzeń w rejestrze lub konfigurowania replikacji między wystąpieniami programu SQL Server. Te akcje są często realizowane za pośrednictwem skryptów programu Windows PowerShell. W tym temacie opisano sposób uruchamiania skryptów programu Windows PowerShell z pliku projektu aparatu Microsoft Build Engine (MSBuild) jako część procesu kompilacji i wdrażania.
- [Rozwiązywanie problemów z procesem tworzenia pakietów](troubleshooting-the-packaging-process.md). Web potok publikowania (WPP) definiuje właściwość narzędzia MSBuild o nazwie **EnablePackageProcessLoggingAndAssert** służącego do generowania szczegółowe informacje na temat procesu tworzenia pakietów dla projektów aplikacji sieci web. W tym temacie opisano, co robi właściwości i jak z niej korzystać.

## <a name="key-technologies"></a>Kluczowe technologie

Ten samouczek koncentruje się na temat korzystania z tych produktów i technologii umożliwiających automatyczne kompilowanie i wdrażanie w Internecie:

- Program Visual Studio 2010 i Team Foundation Server (TFS) 2010
- MSBuild i TFS Team Build
- Internetowe usługi informacyjne (IIS) 7.5
- Narzędzia do wdrażania sieci Web IIS (Web Deploy) 2.1
- Narzędzie wdrażania bazy danych VSDBCMD.exe

## <a name="other-tutorials-in-this-series"></a>Inne samouczki w tej serii

To jest częścią serii samouczków pięć w skali przedsiębiorstwa wdrażanie w Internecie. Poniżej przedstawiono inne samouczki w tej serii:

- [Wdrażanie aplikacji sieci Web w scenariuszach dla przedsiębiorstw](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md). Ta zawartość wprowadzające zapewnia kontekstowe tła serii samouczków. Scenariusz samouczka opisano w nim, a następnie pokazano, jak zadania i wskazówki, które opisano w całej serii mieści się w szerszym procesu zarządzania cyklem życia aplikacji (ALM).
- [Narzędzie Web Deployment w przedsiębiorstwie](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Ten samouczek zawiera ogólne wprowadzenie do plików projektu MSBuild, potok WPP, narzędzie Web Deploy i inne powiązane technologie. Jej wyjaśnia, jak można używać tych narzędzi wspólnie do zarządzania procesami złożonego wdrożenia.
- [Konfigurowanie środowisk serwera na potrzeby wdrażania w Internecie](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md). W tym samouczku opisano sposób konfigurowania serwerów Windows do obsługi różnych scenariuszy wdrażania, w tym wdrażania pakietu sieci web do zdalnego za pomocą usługi sieci Web Deployment Agent (agent zdalny) lub program obsługi wdrażania w sieci Web oraz zdalnej bazy danych wdrażania. Zwraca uwagę na wybór metody wdrożenia odpowiednie dla Twojego środowiska i opisano w nim sposób używania struktury farmy sieci Web (WFF) do replikacji wdrożonych aplikacji sieci web na wszystkich serwerach sieci web w farmie serwerów.
- [Konfigurowanie serwera Team Foundation Server na potrzeby wdrażania w Internecie](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). W tym samouczku opisano sposób konfigurowania programu TFS do obsługi różnych scenariuszy wdrażania, w tym zautomatyzowane wdrożenia w ramach procesu ciągłej integracji i ręcznie wyzwolony wdrożeń z konkretnymi kompilacjami.

> [!div class="step-by-step"]
> [Next](performing-a-what-if-deployment.md)
