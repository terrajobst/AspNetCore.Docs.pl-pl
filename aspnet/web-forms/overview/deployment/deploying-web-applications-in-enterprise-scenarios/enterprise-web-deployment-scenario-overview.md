---
uid: web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview
title: 'Wdrożenia sieci Web w przedsiębiorstwie: Omówienie scenariusza | Dokumentacja firmy Microsoft'
author: jrjlee
description: Ten zestaw samouczki używa przykładowe rozwiązanie z realistyczne poziom złożoności, oraz scenariusz wdrażania fikcyjnej organizacji, aby zapewnić ref...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/03/2012
ms.topic: article
ms.assetid: aa862153-4cd8-4e33-beeb-abf502c6664f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview
msc.type: authoredcontent
ms.openlocfilehash: 20f6e206d6aa4bebb4936246468f5ada0e213236
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30890024"
---
<a name="enterprise-web-deployment-scenario-overview"></a>Wdrożenia sieci Web w przedsiębiorstwie: Omówienie scenariusza
====================
przez [Lewandowski Jason](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Ten zestaw samouczki używa przykładowe rozwiązanie z realistyczne poziom złożoności, oraz scenariusz wdrażania fikcyjnej organizacji do implementacji odwołania i zadania i wskazówki dotyczące typowych kontekstu. W tym temacie opisano samouczek scenariusza i przedstawiono przykładowe rozwiązanie.


## <a name="scenario-description"></a>Opis scenariusza

Fikcyjnej firmy Fabrikam, Inc. tworzy rozwiązanie umożliwiający zdalne zespoły przechowywania i pobierania informacji kontaktowych z interfejsem sieci web.

Procesy zarządzania cyklem życia aplikacji (ALM) w firmie Fabrikam, Inc. wymagają rozwiązania można wdrożyć na trzy środowiska serwera na różnych etapach procesu tworzenia oprogramowania:

- Test lub "piaskownicy" środowiska deweloperskiego.
- Intranetowego środowiska przemieszczania.
- Internetowy środowiska produkcyjnego.

Każdy z tych środowisk ma inną konfigurację i wymagania dotyczące zabezpieczeń, a każdy stanowi związane z wdrożeniem unikatowych.

### <a name="the-fabrikam-inc-server-infrastructure"></a>The Fabrikam, Inc. Serwer infrastruktury

Jest to ogólny opracowywania i wdrażania infrastruktury w firmie Fabrikam, Inc.

![](enterprise-web-deployment-scenario-overview/_static/image1.png)

Stacje robocze developer, infrastruktury kontroli źródła, deweloperów środowiska testowego i środowisko przejściowe, które wszystkie znajdują się w sieci intranet w obrębie domeny Fabrikam.net. W środowisku produkcyjnym znajduje się w sieci obwodowej (znanej także jako strefa DMZ, strefą zdemilitaryzowaną i podsiecią ekranowaną), która jest odizolowana od sieci intranet przez zaporę. Jest to typowy scenariusz wdrażania: zwykle izolowanie serwerów sieci web skierowane do Internetu z wewnętrznego serwera infrastruktury za pośrednictwem zapór lub serwerów bram.

W tym przykładzie:

- Serwer Team Foundation Server (TFS) 2010 z serwerem kompilacji oddzielne zapewnia kontroli źródła i funkcje ciągłej integracji (CI).
- Środowisko testowe developer obejmuje serwer sieci web usług Internet Information usług (IIS) 7.5 i serwer bazy danych programu SQL Server 2008 R2.
- W środowisku produkcyjnym zawiera wiele serwerów sieci web usług IIS 7.5 synchronizowane przez serwer kontrolera Framework kolektywu serwerów sieci Web (WFF) wraz z serwerem bazy danych programu SQL Server 2008 R2. W praktyce serwer bazy danych może użyć klastrowania lub dublowania poprawiające skalowalność i dostępność.
- Środowisko przejściowe zaprojektowano w celu zreplikowania możliwie konfiguracji środowiska produkcyjnego.
- Zasady izolacji zapory i sieci nie zezwalają na direct, automatyczne wdrażanie z sieci intranet w sieci obwodowej.

Konfigurację wszystkich tych środowisk opisano szczegółowo w drugi samouczka [Konfigurowanie środowiska serwera sieci Web wdrożenia](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md).

### <a name="team-roles-for-alm"></a>Role zespołu dla ALM

Ci użytkownicy są zaangażowane w tworzenie, zarządzanie, tworzenie i publikowanie rozwiązania z menedżerem skontaktuj się z pomocą:

- Matt Hink jest deweloperem aplikacji sieci web w firmie Fabrikam, Inc. Jest on częścią zespołu, który opracowany rozwiązania Contact Manager przy użyciu programu Visual Studio 2010. Matt ma pełne uprawnienia administracyjne na serwerach w środowisku testowym developer umożliwia mu skonfiguruj środowisko do swoich potrzeb. Ma on także użytkownikowi dostęp do wystąpienia programu Visual Studio 2010 TFS, gdzie przechowuje on kodu źródłowego rozwiązania kontaktów Menedżerze.
- Tomasz Tomasz jest administratorem serwera dla firmy Fabrikam, Inc. zespół deweloperów. Tomasz ma dostęp administracyjny na serwerze TFS, dzięki czemu on można skonfigurować wszystkie aspekty TFS i Team Build. Tomasz również ma dostęp administracyjny do badania i przemieszczania serwerów sieci web, a także pełni rolę administratora bazy danych (DBA) dla serwerów baz danych w środowisk testowych i przejściowych. Tomasz skonfigurował Team Build na serwerze TFS, aby wykonać te zadania:

    - Tworzenie i Uruchamianie testów jednostek dla aplikacji, gdy użytkownik sprawdza się w pliku do programu TFS. Jest to elementu konfiguracji.
    - Wdrażanie aplikacji Contact Manager do środowiska testowego automatycznie po aplikacji przekazuje testów jednostkowych. W tym publikowania bazy danych na serwery testu w początkowym wdrożeniu i wszelkie aktualizacje bazy danych po początkowym wdrożeniu.
    - Wdrażanie aplikacji Contact Manager do środowiska pomostowego w jednym kroku procesu.
    - Utwórz pakiet sieci Web, który administrator serwera sieci Web i Administrator służy do publikowania aplikacji do środowiska produkcyjnego.
- Andrews Ewa jest odpowiedzialny za wdrażanie aplikacji na serwerach produkcyjnych firmy Fabrikam, Inc. administratora serwera. Użytkownik ma dostęp do odczytu do udziału, w których TFS Team Build przechowują pakietu wdrożeniowego sieci web po tworzenia aplikacji skontaktuj się z Menedżera. Użytkownik ma również dostęp administracyjny do produkcyjnych serwerów sieci web, dzięki czemu użytkownik może wdrożyć aplikację w środowisku produkcyjnym. Ponadto użytkownik działa jako administrator, który wdraża baz danych i aktualizacje bazy danych na serwerze bazy danych w środowisku produkcyjnym.

<a id="_The_Contact_Manager"></a>

### <a name="the-contact-manager-solution"></a>Rozwiązania z menedżerem kontaktu

Skontaktuj się z Menedżera rozwiązanie zezwala użytkownikom na zarejestrowanym, zalogowany dodawać i edytować informacje kontaktowe za pośrednictwem interfejsu sieci web. Rozwiązanie kontaktów Menedżerze składa się z czterech poszczególnych projektów:

![](enterprise-web-deployment-scenario-overview/_static/image2.png)

- **ContactManager.Mvc**. Jest to projekt aplikacji sieci web ASP.NET MVC 3, który reprezentuje punkt wejścia dla rozwiązania. Zapewnia ona niektórych funkcji aplikacji sieci web podstawowych, takich jak zapewniając użytkownikom możliwość można tworzyć i wyświetlać szczegóły dotyczące kontaktu. Aplikacja korzysta z usługi Windows Communication Foundation (WCF) do zarządzania kontaktów i aplikacji usługi bazy danych programu ASP.NET do zarządzania, uwierzytelniania i autoryzacji.
- **ContactManager.Database**. Jest to projekt bazy danych programu Visual Studio 2010. Projekt definiuje schemat bazy danych, dane kontaktowe magazynów.
- **ContactManager.Service**. Jest to projekt usługi sieci web WCF. Ujawnia WCF utworzyć punktu końcowego, który umożliwia obiekty wywołujące do wykonania, pobrać, aktualizowania i usuwania (CRUD) operations Manager skontaktuj się z bazy danych. Usługa zależy od tego, skontaktuj się z Menedżera bazy danych i zestaw ContactManager.Common.dll.
- **ContactManager.Common**. Jest to projektu biblioteki klas. Usługi WCF, zależy od typów zdefiniowanych w tym zestawie.

Pełny przegląd rozwiązania i jej wymagań dotyczących wdrożenia znajduje się w pierwszym samouczku tej serii [Web Deployment w przedsiębiorstwie](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md).

<a id="_Deployment_Tasks"></a>

## <a name="deployment-tasks"></a>Zadania wdrażania

Istnieje kilka różnych zadań związanych z wdrażaniem aplikacji w różnych środowiskach w dużych organizacjach. Są to kluczowe zadania, obejmujące samouczków:

![](enterprise-web-deployment-scenario-overview/_static/image3.png)

Poniżej przedstawiono listę każdego kroku w procesie wdrożenia z punktu widzenia użytkowników opisanego wcześniej w tym dokumencie:

1. Wszyscy członkowie zespołu Przejrzyj rozwiązania menedżera kontaktu w Visual Studio 2010, aby określić wymagania dotyczące wdrażania klucza i problemy.
2. Matt Hink może wdrożyć rozwiązanie kontaktów Menedżerze bezpośrednio ze stacji roboczej developer do deweloperów środowiska testowego, przeprowadzenie wstępnym teście logiki wdrożenia.
3. Matt Hink dodaje aplikację do kontroli źródła w programie TFS.
4. Tomasz Tomasz tworzy różne definicje kompilacji dla rozwiązania Menedżera skontaktuj się z Team Build. Jedna definicja kompilacji używa elementu konfiguracji do wdrożenia rozwiązania do środowiska testowego developer zawsze, gdy użytkownik sprawdza w nowy kod. Inną definicję kompilacji umożliwia użytkownikom wyzwalacza wdrożenia do środowiska pomostowego zgodnie z potrzebami.
5. Za każdym razem, gdy zaewidencjonowaniu nowy kod, Team Build automatycznie tworzy składniki rozwiązania, uruchamia testy jednostkowe i wdraża rozwiązanie na deweloperów środowiska testowego, jeśli kompilacja zakończyła się pomyślnie i przebiegu testów jednostkowych.
6. Gdy użytkownik uruchamia wdrożenia do środowiska pomostowego, rozwiązanie spakowanego i wdrożone w jednym kroku procesu. Ten proces generuje również pakietów ręcznego wdrażania do środowiska produkcyjnego.
7. Ewa Andrews wdraża aplikację w środowisku produkcyjnym przez ręczne zaimportowanie pakietu sieci web utworzony w kroku 6.

### <a name="key-deployment-issues"></a>Problemy z wdrażaniem klucza

Rozwiązanie kontaktów Menedżerze i scenariusz firmy Fabrikam, Inc. Zaznacz różnych najczęściej występujących problemów i wyzwania, które mogą wystąpić podczas wdrażania złożone, rozwiązań w skali przedsiębiorstwa. Na przykład:

- Musisz mieć możliwość wdrażanie projektów w wielu środowiskach, takich jak dewelopera lub testowania środowisk, przemieszczania platform i serwerów produkcyjnych. Rozwiązanie musi być wdrażane z różnych ustawień konfiguracji dla każdego środowiska.
- Należy wdrożyć wiele projektów zależnych jednocześnie jako część pojedynczy krok lub zautomatyzowanych procesów kompilacji i wdrożenia.
- Musisz być możliwe do wdrożenia na dysku z zautomatyzowanego procesu. Na przykład chcesz użyć do wdrożenia aplikacji sieci web w środowisku przemieszczania po zaewidencjonowaniu nowy kod procesu CI.
- Musisz być w stanie kontrolować proces wdrażania i ustaw zmienne wdrożenia z zewnętrznego programu Visual Studio, jak deweloperzy prawdopodobnie nie ma ustawienia prawidłowej konfiguracji lub niezbędnych poświadczeń dla każdego środowiska docelowego.
- Należy wdrożyć projekty oparte na schemacie bazy danych i Zachowaj istniejące dane na kolejne wdrożenia.
- Należy wdrożyć bazy danych członkostwa na zasadzie ad hoc bez wdrażania danych konta użytkownika. Ponadto może być konieczne zaktualizowanie schematu bazy danych członkostwa wdrożonej bez utraty danych istniejącego konta użytkownika.
- Należy wykluczyć pewne pliki lub foldery, podczas wdrażania zawartości w różnych środowiskach docelowych.

Ponadto zarządzanie wdrożenia, kiedy aktualizacje są często i przyrostowych zgłasza się pewne wyzwania dodatkowe. Na przykład:

- Uruchom testy jednostkowe, zawsze dewelopera sprawdza w nowy kod. Chcesz wdrożyć rozwiązanie, jeśli kod pomyślnie przejdzie testy jednostkowe.
- Podczas wdrażania aplikacji sieci web w środowisku tymczasowym czy produkcyjnym, należy przekierowywać użytkowników *aplikacji\_offline.htm* pliku na czas trwania procesu wdrażania.
- Chcesz rejestrować działań wdrożenia. Proces wdrażania należy wysłać powiadomienia e-mail o pomyślnym lub niepomyślnym wdrożeń do wyznaczonych odbiorców.
- W przypadku niepowodzenia automatycznego wdrażania procesu wdrażania należy ponów próbę wykonania bieżącego wdrożenia lub wdrożyć poprzedniego pakietu sieci web zamiast tego.

> [!div class="step-by-step"]
> [Poprzednie](deploying-web-applications-in-enterprise-scenarios.md)
> [dalej](application-lifecycle-management-from-development-to-production.md)
