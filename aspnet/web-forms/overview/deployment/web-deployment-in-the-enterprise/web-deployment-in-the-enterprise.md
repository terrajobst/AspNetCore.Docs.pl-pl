---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/web-deployment-in-the-enterprise
title: "Narzędzie Web Deployment w przedsiębiorstwie | Dokumentacja firmy Microsoft"
author: jrjlee
description: "Ten samouczek przedstawia sposób spełniają partii wyzwania, które będą występować w przypadku zarządzać wdrażaniem aplikacji sieci web skali przedsiębiorstwa devel..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: b8283698-7b82-42a8-8d83-3aeb18ca7fcc
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/web-deployment-in-the-enterprise
msc.type: authoredcontent
ms.openlocfilehash: 6210d01f65bcadf8ae4209e372d5aac68861bd7a
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/15/2018
---
<a name="web-deployment-in-the-enterprise"></a>Wdrażanie sieci Web w przedsiębiorstwie
====================
przez [Lewandowski Jason](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> W tym samouczku opisano sposób spełniają partii wyzwania, które będą występować w przypadku zarządzać wdrażaniem aplikacji sieci web w skali przedsiębiorstwa do środowiska deweloperskie, testu, przemieszczania i produkcji. Samouczek zawiera odwołanie do rozwiązania wraz z kombinacją koncepcyjne i zadań zawartości do przeprowadzenia różnych typowych zadań i procedur.
> 
> Włoska translacji tego samouczka, odwiedź stronę [ http://www.lucamorelli.it ](http://www.lucamorelli.it).


## <a name="enterprise-deployment-challenges"></a>Związane z wdrożeniem Enterprise

Organizacje często wystąpić te problemy, sprawdzając zarządzać wdrażaniem złożonych, rozwiązań skali przedsiębiorstwa:

- Musisz mieć możliwość wdrażanie projektów w wielu środowiskach, takich jak dewelopera lub testowania środowisk, przemieszczania platform i serwerów produkcyjnych. Rozwiązanie musi być wdrażane z różnych ustawień konfiguracji dla każdego środowiska.
- Należy wdrożyć wiele projektów zależnych jednocześnie jako część pojedynczy krok lub zautomatyzowanych procesów kompilacji i wdrożenia.
- Musisz być możliwe do wdrożenia na dysku z zautomatyzowanego procesu. Na przykład chcesz użyć do wdrożenia aplikacji sieci web w środowisku testowym, gdy zaznaczone jest pole Nowy kod procesie ciągłej integracji (CI).
- Musisz być w stanie kontrolować proces wdrażania i ustaw zmienne wdrożenia z zewnętrznego programu Visual Studio, jak deweloperzy prawdopodobnie nie ma ustawienia prawidłowej konfiguracji lub niezbędnych poświadczeń dla każdego środowiska docelowego.
- Należy wdrożyć projekty oparte na schemacie bazy danych i Zachowaj istniejące dane na kolejne wdrożenia.
- Należy wdrożyć bazy danych członkostwa na zasadzie ad hoc bez wdrażania danych konta użytkownika. Ponadto może być konieczne zaktualizowanie schematu bazy danych członkostwa wdrożonej bez utraty danych istniejącego konta użytkownika.
- Należy wykluczyć pewne pliki lub foldery, podczas wdrażania zawartości w różnych środowiskach docelowych.

## <a name="overview-of-approach"></a>Omówienie rozwiązania

Ten samouczek, wraz z innych samouczków w tej serii korzysta z tej metody wysokiego poziomu sprostać wyzwaniom opisane powyżej.

- **Niestandardowe pliki projektu Microsoft kompilacji Engine (MSBuild) umożliwiają sterowanie ogólny proces tworzenia i wdrażania.**
- Umożliwia tworzenie i wdrażanie w ramach jednej, skryptowe operacji każdy projekt w rozwiązaniu.
- Ustawienia charakterystyczne dla środowiska są skonfigurowane przy użyciu plików prostych projektu określonego środowiska. W przeciwieństwie do programu Visual Studio — na podejście przy użyciu konfiguracje rozwiązania i profilów do konfigurowania wdrożeń dla różnych środowisk publikowania, to rozwiązanie umożliwia konfigurowanie i zarządzanie procesem wdrażania z zewnętrznego programu Visual Studio. Oznacza to, nie należy poprawić wiedzę na temat parametrów połączenia, punktów końcowych usługi poświadczenia serwera i inne zmienne wdrożenia dla środowiska docelowego deweloperów.
- Pliki projektu niestandardowy może być wywoływany przez program Team Build jako część przepływu pracy Team Foundation Server (TFS). Dzięki temu można skonfigurować automatycznego wdrażania dla scenariuszy elementu konfiguracji.

**Narzędzie Internet Information Services (IIS) Web Deployment (Web Deploy) umożliwia pakietów i wdrożyć projekty aplikacji sieci web.**

- Narzędzie Web Deploy zapewniają strukturę, która umożliwia pakietu i wdrożenie zawartości aplikacji sieci web na serwerze sieci web usług IIS docelowego, wraz z zależności, ustawienia konfiguracji, ustawienia zabezpieczeń i inne wymagania.
- Cały proces pakowania i wdrażania z można kontrolować, w ramach plików projektu MSBuild niestandardowych. Można również manipulować ustawienia konfiguracji dołączone do sieci web pakietu wdrożeniowego, takie jak parametry połączenia, punkty końcowe usługi i szczegóły docelowym usług IIS.
- Narzędzie Web Deploy, wraz z proces publikowania w sieci Web oferuje wiele punktów rozszerzalności, które pozwalają dostosować wdrożeń. Na przykład jest łatwy do wykluczenia niepotrzebne pliki i foldery z pakietów wdrażania w sieci web.

**Użyj narzędzia VSDBCMD.exe do wdrożenia i zaktualizuj schematów bazy danych.**

- VSDBCMD umożliwia wdrażanie baz danych z pliku schematu bazy danych (.dbschema), który jest generowany podczas kompilowania projektu bazy danych programu Visual Studio. Z kolei bazy danych wdrażania funkcji Web Deploy jest bardziej odpowiednie wdrażanie istniejących baz danych z lokalnego wystąpienia programu SQL Server.
- W przeciwieństwie do funkcji programu Visual Studio do wdrażania projektów bazy danych VSDBCMD umożliwia wdrażanie aktualizacji różnicowych istniejącą bazę danych docelowych. Dzięki temu można zachować istniejących danych podczas uaktualniania schematu bazy danych.
- Polecenia VSDBCMD z można wykonywać w ramach plików projektu MSBuild niestandardowych.

## <a name="content-map"></a>Mapa zawartości

Ten samouczek zawiera tematy, które można podzielić na cztery główne obszary.

Te tematy wprowadzić rozwiązanie odwołania & #x 2014; rozwiązania z menedżerem skontaktuj się z & #x 2014; i opisano, jak pobrać i skonfigurować go na komputerze lokalnym:

- [Rozwiązanie Contact Manager](the-contact-manager-solution.md)
- [Konfigurowanie rozwiązania Contact Manager](setting-up-the-contact-manager-solution.md)

Te tematy wprowadzenie pliki projektu programu MSBuild, opisano, jak można tworzyć i pliki projektu niestandardowe i przeprowadź przez proces wdrażania rozwiązania Menedżera skontaktuj się z:

- [Objaśnienie pliku projektu](understanding-the-project-file.md)
- [Objaśnienie procesu kompilacji](understanding-the-build-process.md)

Te tematy opisują wdrażanie aplikacji sieci web, w tym sposobu działania procesu kompilacji i tworzenia pakietów, jak proces kompilacji integruje się z potoku publikowania w sieci Web, jak zmodyfikować parametrów wdrożenia i sposobu wdrażania pakietów sieci web do miejsca docelowego środowiskach:

- [Kompilowanie i tworzenie pakietów projektów aplikacji internetowych](building-and-packaging-web-application-projects.md)
- [Konfigurowanie parametrów na potrzeby wdrożenia pakietu internetowego](configuring-parameters-for-web-package-deployment.md)
- [Wdrażanie pakietów internetowych](deploying-web-packages.md)

- [Wdrażanie projektów bazy danych](deploying-database-projects.md) zawiera opis różnych technik, można użyć do wdrożenia projektów bazy danych programu Visual Studio, oraz zalety i wady każdego z podejść. [Tworzenie i uruchamianie pliku poleceń wdrażania](creating-and-running-a-deployment-command-file.md) zawiera opis sposobu tworzenia pliku prostego polecenia, który hermetyzuje logiki wdrożenia i umożliwia wdrażanie złożonych rozwiązań jako proces pojedynczy krok.
- Na koniec [ręczne instalowanie pakietów sieci Web](manually-installing-web-packages.md) poprzez wyświetlenie do importowania pakietów sieci web do usług IIS jest zakończenie samouczka.

## <a name="key-technologies"></a>Kluczowe technologie

Tematy w tym samouczku głównie używać tych technologii do zarządzania kompilacji i wdrażania:

- Visual Studio 2010
- MSBuild
- IIS 7.5
- Web Deploy 2.0
- Narzędzie wdrażania VSDBCMD.exe baz danych

## <a name="other-tutorials-in-this-series"></a>Innych samouczków w tej serii

To jest częścią serii samouczków pięć w skali przedsiębiorstwa wdrożenia sieci web. Są to innych samouczków z serii:

- [Wdrażanie aplikacji sieci Web w scenariuszach dla przedsiębiorstw](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md). Ta zawartość wprowadzające udostępnia kontekstowe tła serii samouczka. Opisuje samouczek scenariusza i przedstawia, jak zadania i wskazówki, które opisano w całym serii pasuje do szerszego procesu zarządzania cyklem życia aplikacji (ALM).
- [Konfigurowanie środowisk serwera sieci Web wdrożenia](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md). W tym samouczku opisano sposób konfigurowania serwerów systemu Windows do obsługi różnych scenariuszy wdrażania, w tym wdrażania pakietu sieci web do zdalnego przy użyciu usługi sieci Web wdrożenia Agent (agent zdalnego) lub program obsługi wdrażania w sieci Web i wdrożenia zdalnej bazy danych. Go znajdują się wskazówki dotyczące wybierania metody wdrażania właściwe dla własnego środowiska i przedstawiono sposób używać struktury farmy sieci Web (WFF) w celu replikowania wdrożonych aplikacji sieci web na wszystkich serwerach sieci web w farmie serwerów.
- [Konfigurowanie serwera Team Foundation Server Web Deployment](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). W tym samouczku opisano sposób konfigurowania TFS w celu obsługi różnych scenariuszy wdrażania, łącznie z automatycznego wdrażania w trakcie procesu konfiguracji i ręcznie wyzwalane wdrożeń określonej kompilacji.
- [Zaawansowane wdrażanie w przedsiębiorstwie sieci Web](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). Ten przewodnik opisuje sposób wykonywania różnych bardziej zaawansowanych zadań wdrażania, jak dostosowywanie wdrożenia bazy danych w wielu środowiskach, z wyjątkiem plików i folderów z wdrożenia i pobieranie aplikacji sieci web w trybie offline podczas procesu wdrażania .

>[!div class="step-by-step"]
[Next](the-contact-manager-solution.md)
