---
uid: web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/application-lifecycle-management-from-development-to-production
title: 'Zarządzanie cyklem życia aplikacji: Od projektowania do produkcji | Dokumentacja firmy Microsoft'
author: jrjlee
description: W tym temacie przedstawiono sposób fikcyjnej firmy zarządzania wdrożenia aplikacji sieci web ASP.NET za pomocą środowisk testowania, przejściowego i produkcji jako par...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: f97a1145-6470-4bca-8f15-ccfb25fb903c
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/application-lifecycle-management-from-development-to-production
msc.type: authoredcontent
ms.openlocfilehash: fdd51d2b6836c7fed04132f7c05bbede772d21e0
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37362813"
---
<a name="application-lifecycle-management-from-development-to-production"></a>Zarządzanie cyklem życia aplikacji: Od projektowania do produkcji
====================
przez [Jason Lee](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> W tym temacie przedstawiono sposób fikcyjnej firmy zarządzania wdrożenia aplikacji sieci web ASP.NET za pomocą środowisk testowania, przejściowego i produkcji jako część procesu ciągłego rozwoju. W temacie podano linki do dalszych informacji i wskazówki na temat sposobu wykonywania określonych zadań.
> 
> Temat ma na celu zapewnienie ogólny przegląd dla [serii samouczków](deploying-web-applications-in-enterprise-scenarios.md) na wdrażanie w Internecie w przedsiębiorstwie. Nie martw się, jeśli nie znasz niektóre pojęcia opisane w tym miejscu&#x2014;samouczków, które należy wykonać zawierają szczegółowe informacje o wszystkich tych zadań i technik.
> 
> > [!NOTE]
> > Zapewnienia Forthe prostotę, w tym temacie nie omówiono w nim aktualizowanie bazy danych jako część procesu wdrażania. Wprowadzanie aktualizacji przyrostowych funkcje bazy danych jest wymagane wiele scenariuszy wdrażania w przedsiębiorstwie i wskazówki można znaleźć w sposób, w tym celu w dalszej części tej serii samouczków. Aby uzyskać więcej informacji, zobacz [wdrażanie projektów baz danych](../web-deployment-in-the-enterprise/deploying-database-projects.md).


## <a name="overview"></a>Omówienie

Przedstawione poniżej proces wdrażania opiera się na scenariusz wdrażania firmy Fabrikam, Inc., które są opisane w [wdrażania sieci Web w przedsiębiorstwie: omówienie scenariusza](enterprise-web-deployment-scenario-overview.md). Omówienie scenariusza należy przeczytać przed zapoznają się w tym temacie. Zasadniczo scenariusz sprawdza, jak organizacja zarządza wdrożenia aplikacji sieci web jest złożone, [rozwiązania Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md), za pośrednictwem różnych faz w typowym środowisku przedsiębiorstwa.

Na wysokim poziomie rozwiązania Contact Manager przechodzi przez te etapy jako część procesu projektowania i proces wdrażania przez:

1. Sprawdza Deweloper jakiś kod w Team Foundation Server (TFS) 2010.
2. TFS jest tworzony kod i uruchamia wszystkie testy jednostkowe, skojarzone z projektem zespołowym.
3. TFS wdraża to rozwiązanie do środowiska testowego.
4. Zespół deweloperów sprawdza i weryfikuje rozwiązania w środowisku testowym.
5. Administrator środowiska przejściowego wykonuje wdrożenia "what if" środowiska tymczasowego w celu ustalenia, czy wdrożenie spowoduje, że wszelkie problemy.
6. Administrator środowiska przejściowego wykonuje na żywo wdrożenia w środowisku przejściowym.
7. Rozwiązanie podlega testów w środowisku przejściowym odbiorczych użytkowników.
8. Pakiety wdrożeniowe sieci web są importowane ręcznie do środowiska produkcyjnego.

Te etapy częścią cyklu rozwoju ciągłe.

![](application-lifecycle-management-from-development-to-production/_static/image1.png)

W praktyce proces jest nieco bardziej skomplikowane niż to, jak zostanie wyświetlony, gdy spojrzymy na każdym etapie bardziej szczegółowo. Fabrikam, Inc. używa innego podejścia do wdrażania dla każdego środowiska docelowego.

![](application-lifecycle-management-from-development-to-production/_static/image2.png)

Pozostała część tego tematu sprawdza, czy te etapy klucza tego cyklu wdrażania:

- **Wymagania wstępne**: jak należy skonfigurować infrastrukturę serwera, zanim przeniesiesz logikę wdrożenie w miejscu.
- **Początkowe tworzenie i wdrażanie**: należy wykonać przed wdrożeniem rozwiązania po raz pierwszy.
- **Wdrożenie do testowania**: jak pakować i wdrażać zawartość w środowisku testowym automatycznie, gdy programista zaewidencjonuje nowy kod.
- **Wdrożenie przejściowe**: Wdrażanie określonej kompilacji, aby środowisko przejściowe i sposobu wykonywania "what if" wdrożeń, aby upewnić się, że wdrożenie nie powoduje żadnych problemów.
- **Wdrożenia w środowisku produkcyjnym**: importowanie pakietów sieci web w środowisku produkcyjnym, gdy infrastruktury sieci uniemożliwia zdalnego wdrażania.

## <a name="prerequisites"></a>Wymagania wstępne

Pierwsze zadanie w każdym scenariuszu wdrożenia jest upewnij się, że infrastruktury serwera spełnia wymagania wdrożenia narzędzi i technik. W tym przypadku firmy Fabrikam, Inc. skonfigurował swoją infrastrukturę serwera następująco:

- TFS jest skonfigurowany do zawierać kolekcji projektów zespołowych, kontrolery kompilacji i agentów kompilacji. Zobacz [Konfigurowanie serwera Team Foundation Server dla automatycznego wdrażania w Internecie](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md) Aby uzyskać więcej informacji.
- Środowisko testowe jest skonfigurowany do akceptowania zdalnych wdrożeń przy użyciu usługi agenta wdrażania sieci Web ("agent zdalny"), zgodnie z opisem w [scenariusz: Konfigurowanie środowiska testowania na potrzeby wdrażania w Internecie](../configuring-server-environments-for-web-deployment/scenario-configuring-a-test-environment-for-web-deployment.md) i [ Konfigurowanie serwera sieci Web dla usługi Web Deploy (Agent zdalny) publikowanie](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent.md).
- Środowisko przejściowe jest skonfigurowany do akceptowania zdalnych wdrożeń przy użyciu punktu końcowego obsługi wdrażania sieci Web, zgodnie z opisem w [scenariusz: Konfigurowanie środowiska przejściowego na potrzeby wdrażania w Internecie](../configuring-server-environments-for-web-deployment/scenario-configuring-a-staging-environment-for-web-deployment.md) i [skonfigurować serwer sieci Web publikowania w sieci Web Deploy (Web Deploy obsługi)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md).
- W środowisku produkcyjnym jest skonfigurowane i umożliwiają administratorowi ręcznie zaimportować pakiety wdrażania sieci web w Internet Information Services (IIS), zgodnie z opisem w [scenariusz: Konfigurowanie środowiska produkcyjnego na potrzeby wdrażania w Internecie](../configuring-server-environments-for-web-deployment/scenario-configuring-a-production-environment-for-web-deployment.md) i [skonfigurować serwer sieci Web dla usługi Web Deploy (wdrożenie w trybie Offline) publikowanie](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md).

## <a name="initial-development-and-deployment"></a>Początkowego projektowania i wdrażania

Zanim zespół deweloperów Fabrikam, Inc., można wdrożyć rozwiązanie Contact Manager po raz pierwszy, musi wykonać następujące zadania:

- Tworzenie nowego projektu zespołowego w programie TFS.
- Tworzenie plików projektów aparatu Microsoft Build Engine (MSBuild), które zawierają logikę wdrożenia.
- Tworzenie definicji kompilacji serwera TFS, które mogą powodować procesów wdrażania.

### <a name="create-a-new-team-project"></a>Tworzenie nowego projektu zespołowego

- Administratora TFS, Tomasz Rob powoduje utworzenie nowego projektu zespołowego dla aplikacji, zgodnie z opisem w [tworzenia projektu zespołowego w programie TFS](../configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs.md). Następnie główny programista Matt Hink tworzy szkielet rozwiązanie. ADAM sprawdza, czy jego pliki do nowego projektu zespołowego w programie TFS, zgodnie z opisem w [dodawania zawartości do kontroli źródła](../configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control.md).

### <a name="create-the-deployment-logic"></a>Tworzenie logiki wdrożenia

Matt Hink tworzy różnych niestandardowych plików projektu MSBuild przy użyciu podejścia pliku projektu Podziel opisanego w [objaśnienie pliku projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md). Matt tworzy:

- Plik projektu o nazwie *Publish.proj* , które jest uruchamiane w procesie wdrażania. Ten plik zawiera docelowych elementów MSBuild kompilować projekty w rozwiązaniu, tworzenie pakietów w sieci web i wdrożenia pakietów dla środowiska serwera docelowego.
- Pliki projektu specyficznego dla środowiska o nazwie *Env Dev.proj* i *Env Stage.proj*. Zawierają one ustawienia, które są specyficzne dla środowiska testowego i środowisko przejściowe, takie jak parametry połączenia, punkty końcowe usługi i szczegółowe informacje o usługę zdalną, która, otrzymają pakiet sieci web. Aby uzyskać wskazówki na temat wybierania odpowiednich ustawień dla określonego miejsca docelowego środowiska, zobacz [Konfigurowanie właściwości wdrożenia dla środowiska docelowego](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).

Aby uruchomić wdrożenie, użytkownik wykonuje *Publish.proj* plików przy użyciu programu MSBuild lub kompilacji zespołowej i określa lokalizację pliku projektu specyficznymi dla środowiska (*Env Dev.proj* lub *Env Stage.proj*) jako argument wiersza polecenia. *Publish.proj* pliku następnie importuje plik projektu specyficznego dla środowiska, aby utworzyć kompletny zestaw instrukcji dla każdego środowiska docelowego publikowania.

> [!NOTE]
> Sposób, w jaki działają te pliki niestandardowego projektu jest niezależna od mechanizmu, którego używasz do wywoływania MSBuild. Na przykład, można użyć wiersza polecenia MSBuild bezpośrednio, zgodnie z opisem w [objaśnienie pliku projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md). Możesz uruchamiać pliki projektu z pliku polecenia, zgodnie z opisem w [tworzenie i uruchamianie pliku poleceń wdrażania](../web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file.md). Alternatywnie można uruchomić pliki projektu z definicji kompilacji w programie TFS, zgodnie z opisem w [tworząc definicję kompilacji tego wdrożenia obsługuje](../configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment.md).  
> W każdym przypadku efekt jest taki sam&#x2014;MSBuild wykonuje pliku scalonego projektu i wdraża rozwiązanie na środowisku docelowym. Zapewnia dużą elastyczność w jak wyzwolić proces publikowania.


Gdy ma on utworzone pliki niestandardowego projektu, Matt dodaje je do folderu rozwiązania i ewidencjonuje je w kontroli źródła.

### <a name="create-build-definitions"></a>Utwórz definicje kompilacji

Jako zadanie przygotowania końcowego Matt i Rob współpracują ze sobą do tworzenia trzech definicji kompilacji dla nowego projektu zespołowego:

- **DeployToTest**. Kompiluje rozwiązanie Contact Manager i wdraża ją do środowiska testowego, za każdym razem, gdy występuje, ewidencjonowania.
- **DeployToStaging**. Wdraża zasobów z określonej poprzedniej kompilacji do środowiska pomostowego, gdy deweloper kolejkuje kompilację.
- **DeployToStaging-WhatIf**. Wykonuje "what if" wdrożenia w środowisku przejściowym, gdy deweloper kolejkuje kompilację.

W kolejnych sekcjach więcej szczegółowych informacji dotyczących każdej z tych definicji kompilacji.

## <a name="deployment-to-test"></a>Wdrożenie do testowania

Zespół deweloperów w firmie Fabrikam, Inc. zachowuje środowisk testowych, związanych z różnymi rodzajami oprogramowania testowania działań, takich jak weryfikacji i sprawdzania poprawności, użyteczność testowania, testy zgodności i ad hoc, czy poznawczego testowania.

Zespół projektowy, który utworzył definicję kompilacji w programie TFS o nazwie **DeployToTest**. Ta definicja kompilacji używa wyzwalacza ciągłej integracji, co oznacza, że proces kompilacji jest uruchamiany za każdym razem, gdy członek zespołu deweloperów firmy Fabrikam, Inc. wykona ewidencjonowania. Gdy kompilacja zostaje wyzwolona, definicji kompilacji wykonują następujące czynności:

- Skompiluj rozwiązanie ContactManager.sln. Tworzy to z kolei każdy projekt w ramach rozwiązania.
- Uruchom wszystkie testy jednostkowe w strukturze folderu rozwiązania (jeśli jest to rozwiązanie zostanie skompilowane pomyślnie).
- Uruchamianie plików niestandardowego projektu, które sterują procesem wdrażania (Jeśli rozwiązanie pomyślnie skompilowana i przeszedł wszystkie testy jednostki).

Wynik końcowy to, czy rozwiązanie pomyślnie skompilowana, przekazuje testów jednostkowych pakietów sieci web i inne zasoby dotyczące wdrażania są wdrażane do środowiska testowego.

![](application-lifecycle-management-from-development-to-production/_static/image3.png)

### <a name="how-does-the-deployment-process-work"></a>Jak działa w procesie wdrażania?

**DeployToTest** tworzenia definicji dostarcza te argumenty MSBuild:


[!code-console[Main](application-lifecycle-management-from-development-to-production/samples/sample1.cmd)]


**DeployOnBuild = true** i **DeployTarget = pakiet** właściwości są używane w przypadku kompilacji zespołowej kompilacji projektów w rozwiązaniu. Gdy projekt jest projektem aplikacji sieci web, te właściwości poinstruować program MSBuild, aby utworzyć pakiet wdrożeniowy sieci web dla projektu. **TargetEnvPropsFile** właściwość zawiera informacje dla *Publish.proj* gdzie można znaleźć pliku projektu specyficznymi dla środowiska, aby zaimportować plik.

> [!NOTE]
> Szczegółowy przewodnik na temat tworzenia definicji kompilacji, tak, zobacz [tworząc definicję kompilacji tego wdrożenia obsługuje](../configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment.md).


*Publish.proj* plik zawiera elementy docelowe, które kompilują każdy projekt w rozwiązaniu. Jednak obejmuje również logikę warunkową, pomija te Kompiluj elementów docelowych, gdy wykonujesz pliku w programie Team Build. Dzięki temu można wykorzystać funkcje dodatkowe kompilacji, pozwalającą na tworzenie zespołu, takie jak możliwość uruchamiania testów jednostkowych. Jeśli kompilacja rozwiązania lub jednostki testy kończyć się niepowodzeniem, *Publish.proj* pliku nie zostaną wykonane, a aplikacja nie zostanie wdrożony.

Logikę warunkową odbywa się poprzez ocenę **BuildingInTeamBuild** właściwości. Jest to właściwość MSBuild, automatycznie zostaje ustawiony poziom **true** podczas używania Team Build do tworzenia projektów.

## <a name="deployment-to-staging"></a>Wdrażanie w środowisku przejściowym

Podczas kompilacji spełnia wszystkie wymagania zespołu deweloperów w środowisku testowym, zespół może chcesz wdrożyć tę samą kompilację w środowisku przejściowym. Środowiska przejściowe zwykle są skonfigurowane do dopasowania właściwości "na żywo" środowisku co dokładnie, jak to możliwe, na przykład w specyfikacjach serwera, systemy operacyjne i oprogramowanie oraz konfiguracji sieci lub produkcji. Środowiska przejściowe są często używane do testowania obciążenia, testy odbiorcze użytkowników i przeglądy wewnętrzne szersze. Kompilacje są wdrażane w środowisku przejściowym bezpośrednio z serwera kompilacji.

![](application-lifecycle-management-from-development-to-production/_static/image4.png)

Definicje kompilacji, używany do wdrożenia rozwiązania w środowisku przejściowym **DeployToStaging-WhatIf** i **DeployToStaging**, udostępnianie następującą charakterystykę:

- Nie faktycznie tworzą niczego. Gdy Rob wdraża to rozwiązanie do środowiska pomostowego, chce wdrożyć konkretne, istniejące kompilacji, która już została zweryfikowana i sprawdzone w środowisku testowym. Definicje kompilacji, wystarczy na uruchamianie plików niestandardowego projektu, które sterują procesem wdrażania.
- Kiedy Rob wyzwala kompilację, on używa parametrów kompilacji, aby określić kompilację, która zawiera zasoby, które chce wdrożyć z serwera kompilacji.
- Definicje kompilacji nie są wyzwalane automatycznie. Rob ręcznie kolejkuje kompilację, gdy chce wdrożyć rozwiązanie w środowisku przejściowym.

Oto ogólny proces wdrażania w środowisku przejściowym:

1. Administrator środowiska przejściowego Tomasz Rob umieszcza w kolejce kompilacji za pomocą **DeployToStaging-WhatIf** definicji kompilacji. Rob używa parametrów definicji kompilacji, aby określić kompilację, która chce wdrożyć.
2. **DeployToStaging-WhatIf** kompilacja definicji przebiegów plików niestandardowego projektu w trybie "what if". Spowoduje to wygenerowanie plików dziennika, tak, jakby Rob działał na żywo wdrożenia, ale faktycznie nie wprowadza żadnych zmian w środowisku docelowym.
3. Rob monitoruje pliki dziennika w celu potwierdzenia skutków wdrożenia w środowisku przejściowym. W szczególności Rob chce sprawdzić, co zostanie dodany, co zostanie zaktualizowana i co zostanie usunięte.
4. Jeśli Rob jest spełnione, wdrożenie nie wprowadzić żadnych niepożądanych zmian do istniejących zasobów lub danych, i umieszcza w kolejce kompilacji za pomocą **DeployToStaging** definicji kompilacji.
5. **DeployToStaging** pliki niestandardowego projektu w definicji uruchomienia kompilacji. Zasoby związane z wdrażaniem tych opublikować na podstawowym serwerze sieci web w środowisku przejściowym.
6. Kontroler Framework kolektywu serwerów sieci Web (WFF) synchronizuje serwerów sieci web w środowisku przejściowym. Dzięki temu aplikacja dostępna na wszystkich serwerach sieci web w farmie serwerów.

### <a name="how-does-the-deployment-process-work"></a>Jak działa w procesie wdrażania?

**DeployToStaging** tworzenia definicji dostarcza te argumenty MSBuild:


[!code-console[Main](application-lifecycle-management-from-development-to-production/samples/sample2.cmd)]


**TargetEnvPropsFile** właściwość zawiera informacje dla *Publish.proj* gdzie można znaleźć pliku projektu specyficznymi dla środowiska, aby zaimportować plik. **OutputRoot** właściwości przesłonięcia wbudowanej wartości i wskazuje lokalizację folderu kompilacji, która zawiera zasoby, którą chcesz wdrożyć. Gdy Rob kolejkuje kompilację, używając **parametry** kartę, aby zapewnić zaktualizowaną wartość dla **OutputRoot** właściwości.

![](application-lifecycle-management-from-development-to-production/_static/image5.png)

> [!NOTE]
> Aby uzyskać więcej informacji na temat tworzenia definicji kompilacji, tak, zobacz [wdrażanie określonej kompilacji](../configuring-team-foundation-server-for-web-deployment/deploying-a-specific-build.md).


**DeployToStaging-WhatIf** definicji kompilacji zawiera tę samą logikę wdrożenia jako **DeployToStaging** definicji kompilacji. Jednak zawiera dodatkowy argument **WhatIf = true**:


[!code-console[Main](application-lifecycle-management-from-development-to-production/samples/sample3.cmd)]


W ramach *Publish.proj* pliku **WhatIf** właściwość wskazuje, że wszystkie zasoby dotyczące wdrażania powinny być publikowane w trybie "what if". Innymi słowy pliki dziennika są generowane, tak, jakby wyprzedzeniem zdecydowali wdrożenia, ale nic nie zostanie faktycznie zmienione w środowisku docelowym. Dzięki temu można ocenić wpływ proponowanej wdrożenia&#x2014;w szczególności, co będzie poproś o dodanie Cię, jakie zostaną zaktualizowani i jakie będą usuwane&#x2014;przed faktycznie wprowadzone zmiany.

> [!NOTE]
> Aby uzyskać więcej informacji na temat konfigurowania wdrożenia "what if", zobacz [wykonywania wdrożenia "What If"](../advanced-enterprise-web-deployment/performing-a-what-if-deployment.md).


Po wdrożeniu aplikacji na podstawowym serwerze sieci web w środowisku przejściowym, WFF automatyczną synchronizację aplikacji na wszystkich serwerach w farmie serwerów.

> [!NOTE]
> Aby uzyskać więcej informacji na temat konfigurowania WFF do synchronizowania serwerów sieci web, zobacz [utworzyć farmę serwerów za pomocą rozwiązania Web Farm Framework](../configuring-server-environments-for-web-deployment/creating-a-server-farm-with-the-web-farm-framework.md).


## <a name="deployment-to-production"></a>Wdrożenia w środowisku produkcyjnym

Po zatwierdzeniu kompilacji w środowisku przejściowym zespół firmy Fabrikam, Inc. opublikowanie aplikacji do środowiska produkcyjnego. W środowisku produkcyjnym jest, gdzie aplikacja przejdzie "na żywo" i osiągnie jego docelowymi odbiorcami użytkowników końcowych.

W środowisku produkcyjnym znajduje się w sieci obwodowej dostępnego z Internetu. To jest odizolowana od sieci wewnętrznej, który zawiera serwer kompilacji. Administrator środowiska produkcyjnego, Lisa Andrews, należy ręcznie skopiować pakiety wdrażania sieci web z serwera kompilacji i zaimportować je do usług IIS na serwerze sieci web produkcji podstawowej.

![](application-lifecycle-management-from-development-to-production/_static/image6.png)

Oto ogólny proces wdrażania w środowisku produkcyjnym:

1. Zespołu deweloperów z informacją o tym Lisa czy kompilacja jest gotowa do wdrożenia w środowisku produkcyjnym. Zespół z informacją o tym Lisa lokalizacji pakiety wdrażania sieci web w ramach folderu docelowego na serwerze kompilacji.
2. Lisa zbiera pakietów sieci web z serwera kompilacji i kopiuje je na podstawowym serwerze sieci web w środowisku produkcyjnym.
3. Lisa używa Menedżera usług IIS, aby zaimportować i opublikować pakietów sieci web na podstawowym serwerze sieci web.
4. Kontroler WFF synchronizuje serwerów sieci web w środowisku produkcyjnym. Dzięki temu aplikacja dostępna na wszystkich serwerach sieci web w farmie serwerów.

### <a name="how-does-the-deployment-process-work"></a>Jak działa w procesie wdrażania?

Menedżer usług IIS zawiera Kreatora importowania aplikacji pakietu, który ułatwia publikowanie pakietów w sieci web w witrynie sieci Web usług IIS. Aby uzyskać wskazówki na temat sposobu wykonania tej procedury, zobacz [ręczne instalowanie pakietów internetowych](../web-deployment-in-the-enterprise/manually-installing-web-packages.md).

## <a name="conclusion"></a>Wniosek

W tym temacie podano ilustrację cykl życia wdrożenia dla aplikacji sieci web typowe skali przedsiębiorstwa.

Ten temat jest częścią serii samouczków, które zawiera wskazówek na temat różnych aspektów wdrażania aplikacji sieci web. W praktyce dostępnych jest wiele dodatkowych zadań i uwagi na każdym etapie procesu wdrażania, a nie jest możliwe je pokryć wszystko w jednym wskazówki. Aby uzyskać więcej informacji zapoznaj się z tych samouczków:

- [Narzędzie Web Deployment w przedsiębiorstwie](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Ten samouczek zawiera obszerne wprowadzenie do funkcji techniki wdrażania sieci web przy użyciu programu MSBuild i Narzędzie wdrażania sieci Web usług IIS (Web Deploy).
- [Konfigurowanie środowisk serwera na potrzeby wdrażania w Internecie](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md). Ten samouczek zawiera wskazówki dotyczące sposobu konfigurowania środowisk serwera Windows do obsługi różnych scenariuszy wdrażania.
- [Konfigurowanie serwera Team Foundation Server dla automatycznego wdrażania w Internecie](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). Ten samouczek zawiera wskazówki na temat wdrażania logiki do procesów kompilacji TFS.
- [Zaawansowane wdrażanie w Internecie Enterprise](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). Ten samouczek zawiera wskazówki na temat sposobu spełniają niektóre wyzwania związane z wdrożeniem bardziej złożone, organizacje.

> [!div class="step-by-step"]
> [Poprzednie](enterprise-web-deployment-scenario-overview.md)
