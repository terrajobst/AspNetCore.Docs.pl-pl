---
uid: web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview
title: 'Wdrażania sieci Web w przedsiębiorstwie: Omówienie scenariusza | Dokumentacja firmy Microsoft'
author: jrjlee
description: Tego zestawu samouczków używa przykładowe rozwiązanie przy użyciu realistycznej stopień złożoności, wraz z scenariusz wdrażania fikcyjnej organizacji, aby zapewnić odwołanie...
ms.author: riande
ms.date: 05/03/2012
ms.assetid: aa862153-4cd8-4e33-beeb-abf502c6664f
msc.legacyurl: /web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview
msc.type: authoredcontent
ms.openlocfilehash: ec5b62f3991fa256bc8efe7abe9b953d61d1a515
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752075"
---
<a name="enterprise-web-deployment-scenario-overview"></a>Wdrażanie sieci Web w przedsiębiorstwie: Omówienie scenariusza
====================
przez [Jason Lee](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Tego zestawu samouczków używa przykładowe rozwiązanie z realistyczne stopień złożoności, wraz z fikcyjnego wdrożenia zrealizować scenariusz przedsiębiorstwa, zapewniają implementację referencyjną i wydawanie zadania i wskazówki dotyczące typowych kontekstu. W tym temacie opisano scenariusz samouczka i wprowadza przykładowe rozwiązanie.


## <a name="scenario-description"></a>Opis scenariusza

Fikcyjnej firmy Fabrikam, Inc. jest utworzenie rozwiązania, które umożliwia zdalne zespoły sprzedaży, przechowywać i pobierać dane kontaktowe z interfejsem sieci web.

Procesy zarządzania cyklem życia aplikacji (ALM) w firmie Fabrikam, Inc. wymagają, jakie rozwiązanie będzie można wdrożyć trzy środowiska serwera na różnych etapach procesu tworzenia oprogramowania:

- Test lub "piaskownicy" środowiska deweloperskiego.
- Oparte na sieci intranet środowiska przejściowego.
- Środowisko produkcyjne dostępnego z Internetu.

Każdy z tych środowisk ma inną konfigurację i wymagań dotyczących zabezpieczeń, a każda stwarza wyzwania związane z wdrożeniem unikatowy.

### <a name="the-fabrikam-inc-server-infrastructure"></a>The Fabrikam, Inc. Serwer infrastruktury

Jest to wysokiego poziomu procesu projektowania i wdrażania infrastruktury w firmie Fabrikam, Inc.

![](enterprise-web-deployment-scenario-overview/_static/image1.png)

Stacje robocze deweloperów, infrastruktura kontroli źródła, środowisku testowym dla deweloperów i środowisko przejściowe, który znajduje się w sieci intranet w domenie Fabrikam.net. W środowisku produkcyjnym znajduje się w sieci obwodowej (znany także jako DMZ, strefa zdemilitaryzowana i podsieć ekranowana), która jest odizolowana od sieci intranet przez zaporę. Jest to typowy scenariusz wdrażania: zazwyczaj izolowanie serwerów sieci web dostępnym z Internetu z wewnętrznego serwera infrastruktury przy użyciu zapór lub serwerów bram.

W tym przykładzie:

- Serwer Team Foundation Server (TFS) 2010 z serwerem kompilacji oddzielne zapewnia kontroli źródła i funkcji ciągłej integracji (CI).
- Środowisko testowe dla deweloperów zawiera Internet Information Services (IIS) 7.5 serwera sieci web i serwera bazy danych programu SQL Server 2008 R2.
- W środowisku produkcyjnym obejmuje wiele serwerów sieci web usług IIS 7.5 synchronizowane przez serwer kontrolera w ramach farmy sieci Web (WFF), wraz z serwerem bazy danych programu SQL Server 2008 R2. W praktyce serwer bazy danych może używać klastrowania lub dublowania w celu poprawy skalowalności i dostępności.
- Środowisko przejściowe jest przeznaczony do możliwie najdokładniej replikować konfigurację środowiska produkcyjnego.
- Zasady izolacji zapory i sieci nie zezwalają na direct, automatyczne wdrażanie z sieci intranet w sieci obwodowej.

Konfigurację wszystkich tych środowisk jest opisany bardziej szczegółowo w drugim samouczku [Konfigurowanie środowisk serwera wdrażania sieci Web](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md).

### <a name="team-roles-for-alm"></a>Role dla ALM

Ci użytkownicy są zaangażowane w tworzenie, zarządzanie, tworzenie i publikowanie rozwiązania Contact Manager:

- Matt Hink jest programistą aplikacji sieci web w firmie Fabrikam, Inc. Jest on częścią zespołu, która opracowała rozwiązanie Contact Manager przy użyciu programu Visual Studio 2010. Matt ma pełne uprawnienia administracyjne na serwerach w środowisku testowym dla deweloperów, co pozwala mu skonfiguruj środowisko do swoich potrzeb. Ma on także użytkownikom dostęp do wystąpienia programu Visual Studio 2010 TFS, gdzie przechowuje on kod źródłowy rozwiązania Contact Manager.
- Tomasz Tomasz jest administratorem serwera dla zespołu deweloperów firmy Fabrikam, Inc. Rob ma dostęp administratora na serwerze TFS, więc on można skonfigurować z wszystkimi aspektami programu TFS i kompilacji zespołowej. Rob również ma dostęp administracyjny do testowego i przejściowego serwerów sieci web i działa jako administrator bazy danych (DBA) dotyczące serwerów bazy danych w środowisk testowych i przejściowych. Tomasz skonfigurował Team Build na serwerze TFS, aby wykonać te zadania:

    - Skompiluj i uruchom testy jednostkowe na aplikacji, zawsze wtedy, gdy użytkownik ewidencjonuje plik do programu TFS. Jest to nazywane ciągłej integracji.
    - Wdrażanie aplikacji Contact Manager do środowiska testowego automatycznie, gdy aplikacja przekazuje testów jednostkowych. W tym publikowania bazy danych na serwerach testów na początkowego wdrożenia wraz z wszelkimi aktualizacjami bazy danych po początkowym wdrożeniu.
    - Wdrażanie aplikacji Contact Manager w środowisku przejściowym w procesie pojedynczy krok.
    - Utwórz pakiet sieci Web, który administrator serwera sieci Web oraz administratorem baz danych można użyć do publikowania aplikacji do środowiska produkcyjnego.
- Lisa Andrews jest odpowiedzialny za wdrażanie aplikacji na serwerach produkcyjnych firmy Fabrikam, Inc. administrator serwera. Ma ona dostęp do odczytu do udziału, w którym programu TFS Team Build przechowuje pakiet wdrażania sieci web, gdy zbudował aplikacji Contact Manager. Również ma ona dostęp administracyjny do produkcyjnych serwerów sieci web, dzięki czemu użytkownik może wdrożyć aplikację do środowiska produkcyjnego. Ponadto działa ona jako administrator bazy danych, który służy do wdrażania baz danych i aktualizowanie bazy danych na serwerze bazy danych w środowisku produkcyjnym.

<a id="_The_Contact_Manager"></a>

### <a name="the-contact-manager-solution"></a>Rozwiązanie Contact Manager

Rozwiązanie Contact Manager jest przeznaczony do powiadomić użytkowników zarejestrowanych, zalogowany, dodawać i edytować informacje kontaktowe za pośrednictwem interfejsu sieci web. Rozwiązanie Contact Manager składa się z czterech poszczególnych projektów:

![](enterprise-web-deployment-scenario-overview/_static/image2.png)

- **ContactManager.Mvc**. Jest to projektu aplikacji sieci web wzorca ASP.NET MVC 3, który reprezentuje punkt wejścia dla rozwiązania. Oferuje ona niektóre funkcje aplikacji web podstawowych, takich jak zapewniając użytkownikom możliwość tworzenia i wyświetlania szczegółów dotyczących kontaktu ds. Aplikacja korzysta z usługi Windows Communication Foundation (WCF) do zarządzania kontaktami i aplikacji usługi bazy danych programu ASP.NET, aby zarządzać uwierzytelnianiem i autoryzacją.
- **ContactManager.Database**. Jest to projekt bazy danych programu Visual Studio 2010. Projekt definiuje schemat bazy danych, dane kontaktowe magazynów.
- **ContactManager.Service**. Jest to projekt usługi sieci web WCF. Udostępnia usługi WCF, utworzyć punkt końcowy, który umożliwia obiektom wywołującym do wykonania, pobieranie, aktualizowanie i usuwanie operacji (CRUD) w bazie danych Contact Manager. Usługa zależy od tego, Contact Manager bazy danych i zestaw ContactManager.Common.dll.
- **ContactManager.Common**. Jest to projekt biblioteki klas. Usługa WCF opiera się na typy zdefiniowane w tym zestawie.

Pełny przegląd rozwiązania i jego wymagania dotyczące wdrażania znajduje się w pierwszym samouczku tej serii [wdrażanie w Internecie w przedsiębiorstwie](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md).

<a id="_Deployment_Tasks"></a>

## <a name="deployment-tasks"></a>Zadania związane z wdrażaniem

Istnieje kilka zadań distinct związanych z wdrażaniem aplikacji w różnych środowiskach w dużych organizacjach. Oto kluczowe zadania, które obejmują samouczków:

![](enterprise-web-deployment-scenario-overview/_static/image3.png)

Poniżej przedstawiono listę każdego kroku w procesie wdrożenia z punktu widzenia użytkowników opisane wcześniej w tym dokumencie:

1. Wszyscy członkowie zespołu Przejrzyj rozwiązanie Contact Manager w Visual Studio 2010, aby określić wymagania dotyczące wdrażania klucza i problemów.
2. Matt Hink mogą wdrażać rozwiązania Contact Manager bezpośrednio z poziomu stacji roboczej dewelopera do środowiska testowego dla deweloperów, do przeprowadzenia testu wstępnego, logiki wdrożenia.
3. Matt Hink dodaje aplikację do kontroli źródła w programie TFS.
4. Tomasz Rob tworzy różnych definicji kompilacji dla rozwiązania Contact Manager w programie Team Build. Jedną definicję kompilacji używa ciągłej integracji, aby wdrożyć to rozwiązanie do środowiska testowego dla deweloperów w każdym przypadku, gdy użytkownik zaewidencjonuje nowy kod. Inną definicję kompilacji pozwala użytkownikom Wyzwalaj wdrożenia do środowiska pomostowego, zgodnie z potrzebami.
5. Za każdym razem, gdy użytkownik zaewidencjonuje nowy kod, Team Build automatycznie tworzy składników rozwiązania, przeprowadza testy jednostkowe i wdraża to rozwiązanie do środowiska testowego dla deweloperów, jeśli kompilacja zakończyła się pomyślnie i przebieg testów jednostkowych.
6. Po użytkownik wyzwala wdrożenia do środowiska pomostowego, rozwiązanie jest w pakiecie i wdrożone w ramach jednego kroku procesu. Ten proces generuje również pakiet ręcznego wdrażania do środowiska produkcyjnego.
7. Lisa Andrews wdrażania aplikacji w środowisku produkcyjnym przy ręcznym importowaniu definicji pakietu sieci web utworzonej w kroku 6.

### <a name="key-deployment-issues"></a>Problemy z wdrażaniem klucza

Rozwiązanie Contact Manager i scenariusz firmy Fabrikam, Inc. wyróżnienia różnych najczęściej występujących problemów i wyzwania, które mogą wystąpić podczas wdrażania złożonych, rozwiązania w skali przedsiębiorstwa. Na przykład:

- Musisz mieć możliwość wdrażania projektów w wielu środowiskach, takich jak deweloper lub testowym, przemieszczania platform i obsługi serwerów produkcyjnych. Rozwiązanie musi zostać wdrożone za pomocą różnych ustawień konfiguracji dla każdego środowiska.
- Należy wdrożyć wiele projektów zależnych jednocześnie w ramach pojedynczy krok i automatycznych procesu kompilowania i wdrażania.
- Musisz być w stanie do wdrożenia na dysku z zautomatyzowanego procesu. Na przykład chcesz korzystać z procesu ciągłej integracji do wdrożenia aplikacji sieci web w środowisku przejściowym, gdy nowy kod jest zaewidencjonowany.
- Musisz być w stanie kontrolować proces wdrażania i Ustawianie zmiennych wdrożenia z poza programem Visual Studio, jak deweloperzy prawdopodobnie nie ma ustawienia prawidłowej konfiguracji lub niezbędnych poświadczeń dla każdego środowiska docelowego.
- Należy wdrożyć projekty oparte na schemacie bazy danych i zachować istniejące dane na kolejne wdrożenia.
- Należy wdrożyć baz danych członkostwa na zasadzie ad hoc, bez konieczności wdrażania danych konta użytkownika. Również może być konieczne zaktualizowanie schematu członkostwa wdrożonej bazy danych bez utraty danych istniejącego konta użytkownika.
- Należy wykluczyć pewne pliki lub foldery, podczas wdrażania zawartości w różnych środowiskach docelowych.

Ponadto Zarządzanie wdrożeniem, kiedy aktualizacje są często i przyrostowe zgłasza się niektóre wyzwania, dodatkowe. Na przykład:

- Uruchamianie testów jednostkowych, za każdym razem, gdy programista zaewidencjonuje nowy kod. Chcesz wdrożyć rozwiązanie, jeśli kod przekazuje testów jednostkowych.
- Podczas wdrażania aplikacji sieci web w środowisku tymczasowym lub produkcyjnym chcesz przekierować użytkowników do *aplikacji\_offline.htm* pliku na czas trwania procesu wdrażania.
- Chcesz rejestrować działań wdrożenia. Proces wdrażania należy wysłać pocztą e-mail powiadomienia dotyczące wdrożenia z powodzeniem lub niepowodzeniem do wyznaczonych odbiorców.
- W przypadku niepowodzenia automatycznego wdrażania procesu wdrażania należy ponów próbę wykonania bieżącego wdrożenia lub zamiast tego wdrożenia poprzedniej pakietu sieci web.

> [!div class="step-by-step"]
> [Poprzednie](deploying-web-applications-in-enterprise-scenarios.md)
> [dalej](application-lifecycle-management-from-development-to-production.md)
