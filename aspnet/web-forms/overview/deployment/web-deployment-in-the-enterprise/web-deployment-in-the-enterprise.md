---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/web-deployment-in-the-enterprise
title: Narzędzie Web Deployment w przedsiębiorstwie | Dokumentacja firmy Microsoft
author: jrjlee
description: Ten przewodnik opisuje sposób spełnić wiele wyzwań, które będą występować w przypadku zarządzać wdrażaniem aplikacji sieci web skali korporacyjnej devel...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: b8283698-7b82-42a8-8d83-3aeb18ca7fcc
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/web-deployment-in-the-enterprise
msc.type: authoredcontent
ms.openlocfilehash: 07c1ea9728c0130b860c0e0a64eb0751245ff840
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37371145"
---
<a name="web-deployment-in-the-enterprise"></a>Wdrażanie w Internecie w przedsiębiorstwie
====================
przez [Jason Lee](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> W tym samouczku opisano sposób spełnia wiele wyzwań, które będą występować w przypadku zarządzać wdrażaniem aplikacji sieci web w skali korporacyjnej dla środowisk programowania, testowania, przemieszczania i produkcji. Samouczek obejmuje rozwiązanie odniesienia wraz z kombinacji koncepcyjne i zadań zawartości przeprowadzenie Cię przez proces różnych typowych zadań i procedur.
> 
> Włoska tłumaczenia w tych samouczkach, odwiedź stronę [ http://www.lucamorelli.it ](http://www.lucamorelli.it).


## <a name="enterprise-deployment-challenges"></a>Wyzwania związane z wdrożeniem w przedsiębiorstwie

Organizacje często napotykasz te wyzwania, sprawdzając zarządzać wdrażaniem złożonych, rozwiązań w skali przedsiębiorstwa:

- Musisz mieć możliwość wdrażania projektów w wielu środowiskach, takich jak deweloper lub testowym, przemieszczania platform i obsługi serwerów produkcyjnych. Rozwiązanie musi zostać wdrożone za pomocą różnych ustawień konfiguracji dla każdego środowiska.
- Należy wdrożyć wiele projektów zależnych jednocześnie w ramach pojedynczy krok i automatycznych procesu kompilowania i wdrażania.
- Musisz być w stanie do wdrożenia na dysku z zautomatyzowanego procesu. Na przykład chcesz korzystać z procesu ciągłej integracji (CI) do wdrożenia aplikacji sieci web w środowisku testowym, gdy nowy kod jest zaewidencjonowany.
- Musisz być w stanie kontrolować proces wdrażania i Ustawianie zmiennych wdrożenia z poza programem Visual Studio, jak deweloperzy prawdopodobnie nie ma ustawienia prawidłowej konfiguracji lub niezbędnych poświadczeń dla każdego środowiska docelowego.
- Należy wdrożyć projekty oparte na schemacie bazy danych i zachować istniejące dane na kolejne wdrożenia.
- Należy wdrożyć baz danych członkostwa na zasadzie ad hoc, bez konieczności wdrażania danych konta użytkownika. Również może być konieczne zaktualizowanie schematu członkostwa wdrożonej bazy danych bez utraty danych istniejącego konta użytkownika.
- Należy wykluczyć pewne pliki lub foldery, podczas wdrażania zawartości w różnych środowiskach docelowych.

## <a name="overview-of-approach"></a>Omówienie podejścia

Tego samouczka, razem z innymi samouczki w tej serii używa tego podejścia wysokiego poziomu, aby sprostać wyzwaniom opisanych powyżej.

- **Użyj niestandardowych plików projektu aparatu Microsoft Build Engine (MSBuild), aby kontrolować całego procesu kompilowania i wdrażania.**
- Umożliwia tworzenie i wdrażanie każdego projektu w rozwiązaniu w ramach jednej operacji za pomocą skryptów.
- Ustawienia specyficzne dla środowiska są skonfigurowane przy użyciu prostego projektu specyficznymi dla środowiska pliki. W przeciwieństwie do programu Visual Studio skoncentrowane na testowaniu i podejście przy użyciu konfiguracji rozwiązania i publikowanie profilów w skonfigurowaniu wdrożeń w różnych środowiskach, to podejście umożliwia konfigurowanie i zarządzanie procesem wdrażania z poza programem Visual Studio. Oznacza to, nie potrzebujesz Rozwijaj wiedzę na temat parametrów połączenia, punktów końcowych usługi, poświadczenia serwera i inne zmienne wdrożenia dla środowiska docelowego deweloperów.
- Pliki projektu niestandardowego może być wywoływany przez program Team Build jako część przepływu pracy Team Foundation Server (TFS). Dzięki temu można skonfigurować automatycznego wdrażania w scenariuszach ciągłej integracji.

**Narzędzie wdrażania sieci Web usług Internet Information Services (IIS), (Web Deploy) umożliwia tworzenie pakietów i wdrażanie projektów aplikacji sieci web.**

- Narzędzie Web Deploy udostępnia strukturę, która umożliwia tworzenie pakietów i wdrażanie zawartości aplikacji sieci web na serwerze sieci web docelowego programu IIS wraz z zależnościami, ustawienia konfiguracji, ustawienia zabezpieczeń i inne wymagania.
- Możesz kontrolować cały proces pakowania i wdrażania z w obrębie niestandardowych plików projektu MSBuild. Możesz również manipulować ustawienia konfiguracji, dołączone do pakietu wdrażania sieci web, takich jak parametry połączenia, punkty końcowe usługi i szczegóły dotyczące przeznaczenia usług IIS.
- Narzędzie Web Deploy, wraz z potoku publikowania w sieci Web, oferuje wiele punkty rozszerzeń, które umożliwiają dostosowanie wdrożenia. Na przykład to proste mają być wykluczone niepotrzebne pliki i foldery z pakietów wdrażania sieci web.

**Narzędzie VSDBCMD.exe umożliwia wdrażanie i aktualizowanie schematy bazy danych.**

- VSDBCMD umożliwia wdrażanie baz danych z pliku schematu bazy danych (.dbschema), który jest generowany, gdy tworzysz projekt bazy danych programu Visual Studio. Z kolei funkcje wdrażania bazy danych, które są zawarte w narzędzia Web Deploy jest bardziej odpowiednie wdrażanie istniejących baz danych z lokalnego wystąpienia programu SQL Server.
- W przeciwieństwie do funkcji programu Visual Studio do wdrażania projektów bazy danych VSDBCMD umożliwia wdrażanie aktualizacji różnicowych do istniejącej docelowej bazy danych. Dzięki temu można zachować wszystkie istniejące dane podczas uaktualniania schemat bazy danych.
- Można wykonać polecenia VSDBCMD z w obrębie niestandardowych plików projektu MSBuild.

## <a name="content-map"></a>Mapa zawartości

Ten samouczek zawiera tematy, które można podzielić na cztery główne obszary.

Te tematy wprowadzenie rozwiązania odwołanie&#x2014;rozwiązania Contact Manager&#x2014;oraz opisano, jak ją pobrać i skonfigurować go na komputerze lokalnym:

- [Rozwiązanie Contact Manager](the-contact-manager-solution.md)
- [Konfigurowanie rozwiązania Contact Manager](setting-up-the-contact-manager-solution.md)

Te tematy wprowadzają pliki projektów programu MSBuild, opisano, jak można utworzyć i używać plików projektów niestandardowych i opisano proces wdrażania rozwiązania Contact Manager:

- [Objaśnienie pliku projektu](understanding-the-project-file.md)
- [Objaśnienie procesu kompilacji](understanding-the-build-process.md)

W tych tematach opisano wdrażanie aplikacji sieci web, w tym sposobu działania procesu kompilacji i pakowanie, jak proces kompilacji integruje się z potoku publikowania w sieci Web, jak zmodyfikować parametrów wdrożenia i sposób wdrażania pakietów sieci web do miejsca docelowego środowiska:

- [Kompilowanie i tworzenie pakietów projektów aplikacji internetowych](building-and-packaging-web-application-projects.md)
- [Konfigurowanie parametrów na potrzeby wdrożenia pakietu internetowego](configuring-parameters-for-web-package-deployment.md)
- [Wdrażanie pakietów internetowych](deploying-web-packages.md)

- [Wdrażanie projektów baz danych](deploying-database-projects.md) w tym artykule opisano różne techniki, które umożliwia wdrażanie projektów baz danych programu Visual Studio, wraz z zalety i wady każdej metody. [Tworzenie i uruchamianie pliku poleceń wdrażania](creating-and-running-a-deployment-command-file.md) zawiera opis sposobu tworzenia pliku prostego polecenia, który hermetyzuje logikę wdrożenia i pozwala na wdrażanie złożonych rozwiązań jako pojedynczy krok procesu.
- Na koniec [ręczne instalowanie pakietów internetowych](manually-installing-web-packages.md) koniec samouczka, pokazując, należy zaimportować pakiety sieci web do usług IIS.

## <a name="key-technologies"></a>Kluczowe technologie

Tematy w tym samouczku przede wszystkim przy użyciu tych technologii do zarządzania, kompilowania i wdrażania:

- Visual Studio 2010
- MSBuild
- IIS 7.5
- Web Deploy 2.0
- Narzędzie wdrażania bazy danych VSDBCMD.exe

## <a name="other-tutorials-in-this-series"></a>Inne samouczki w tej serii

To jest częścią serii samouczków pięć w skali przedsiębiorstwa wdrażanie w Internecie. Poniżej przedstawiono inne samouczki z serii:

- [Wdrażanie aplikacji sieci Web w scenariuszach dla przedsiębiorstw](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md). Ta zawartość wprowadzające zapewnia kontekstowe tła serii samouczków. Scenariusz samouczka opisano w nim, a następnie pokazano, jak zadania i wskazówki, które opisano w całej serii mieści się w szerszym procesu zarządzania cyklem życia aplikacji (ALM).
- [Konfigurowanie środowisk serwera na potrzeby wdrażania w Internecie](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md). W tym samouczku opisano sposób konfigurowania serwerów Windows do obsługi różnych scenariuszy wdrażania, w tym wdrażania pakietu sieci web do zdalnego za pomocą usługi sieci Web Deployment Agent (agent zdalny) lub program obsługi wdrażania w sieci Web oraz zdalnej bazy danych wdrażania. Zwraca uwagę na wybór metody wdrożenia odpowiednie dla Twojego środowiska i opisano w nim sposób używania struktury farmy sieci Web (WFF) do replikacji wdrożonych aplikacji sieci web na wszystkich serwerach sieci web w farmie serwerów.
- [Konfigurowanie serwera Team Foundation Server na potrzeby wdrażania w Internecie](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). W tym samouczku opisano sposób konfigurowania programu TFS do obsługi różnych scenariuszy wdrażania, w tym zautomatyzowane wdrożenia w ramach procesu ciągłej integracji i ręcznie wyzwolony wdrożeń z konkretnymi kompilacjami.
- [Zaawansowane wdrażanie w Internecie Enterprise](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). W tym samouczku opisano sposób wykonywania różnych bardziej zaawansowanych zadań wdrażania, takich jak dostosowywanie wdrożeń bazy danych w wielu środowiskach, wykluczanie plików i folderów z wdrożenia i przełączania w tryb offline w aplikacjach sieci web podczas procesu wdrażania .

> [!div class="step-by-step"]
> [Next](the-contact-manager-solution.md)
