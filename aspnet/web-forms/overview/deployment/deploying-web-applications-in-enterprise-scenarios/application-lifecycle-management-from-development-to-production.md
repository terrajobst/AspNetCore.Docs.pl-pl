---
uid: web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/application-lifecycle-management-from-development-to-production
title: 'Zarządzanie cyklem życia aplikacji: Od projektowania do produkcji | Dokumentacja firmy Microsoft'
author: jrjlee
description: W tym temacie przedstawiono, jak fikcyjnej firmy zarządza wdrożenia aplikacji sieci web ASP.NET za pośrednictwem środowiska testowego, tymczasowych i produkcyjnych jako par...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: f97a1145-6470-4bca-8f15-ccfb25fb903c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/application-lifecycle-management-from-development-to-production
msc.type: authoredcontent
ms.openlocfilehash: 8beeffb374df09c6695a1845199d30006ddcc1b7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30887290"
---
<a name="application-lifecycle-management-from-development-to-production"></a>Zarządzanie cyklem życia aplikacji: Od projektowania do produkcji
====================
przez [Lewandowski Jason](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> W tym temacie przedstawiono, jak fikcyjnej firmy zarządza wdrożenia aplikacji sieci web ASP.NET za pośrednictwem środowiska testowego, tymczasowych i produkcyjnych w ramach procesu projektowania ciągłe. W temacie podano linki dodatkowe informacje i wskazówki dotyczące wykonywania określonych zadań.
> 
> Temat jest przeznaczony do stanowią ogólne omówienie dla [serii samouczków](deploying-web-applications-in-enterprise-scenarios.md) w sieci web wdrożenia w przedsiębiorstwie. Nie martw się, jeśli nie masz doświadczenia w obsłudze niektórych pojęcia opisane tutaj&#x2014;samouczki, które należy wykonać zawierają szczegółowe informacje na wszystkich tych zadań i technik.
> 
> > [!NOTE]
> > Zapewnienia Forthe prostotę, w tym temacie nie omówiono w nim aktualizowania bazy danych jako część procesu wdrażania. Jednak wprowadzanie aktualizacji przyrostowych funkcje bazy danych jest wymagany wiele scenariuszy wdrażania w przedsiębiorstwie i wskazówki można znaleźć w sposób, w tym celu w dalszej części tego samouczka serii. Aby uzyskać więcej informacji, zobacz [wdrażania projektów bazy danych](../web-deployment-in-the-enterprise/deploying-database-projects.md).


## <a name="overview"></a>Omówienie

Proces wdrażania przedstawione w tym miejscu jest oparta na scenariusz wdrażania firmy Fabrikam, Inc. opisany w [wdrożenia sieci Web w przedsiębiorstwie: omówienie scenariusza](enterprise-web-deployment-scenario-overview.md). Omówienie scenariusza należy zapoznać się przed badanie w tym temacie. Zasadniczo scenariusz sprawdza jak organizacja zarządza wdrożenia aplikacji sieci web rozsądnych złożonych, [rozwiązania kontaktów Menedżerze](../web-deployment-in-the-enterprise/the-contact-manager-solution.md), za pośrednictwem różnych faz w środowisku typowego przedsiębiorstwa.

Na wysokim poziomie rozwiązanie kontaktów Menedżerze przechodzi przez te etapy jako część procesu wdrażania i rozwoju:

1. Projektant sprawdza kodu w Team Foundation Server (TFS) 2010.
2. TFS kompilacji kodu i uruchamia wszystkie testy jednostkowe skojarzone z projektem zespołowym.
3. TFS wdraża rozwiązanie do środowiska testowego.
4. Zespół deweloperów sprawdza i weryfikuje rozwiązania w środowisku testowym.
5. Administrator środowiska przemieszczania wykonuje wdrożenia "co w przypadku" do środowiska pomostowego, do ustalenia, czy wdrożenie spowoduje, że wszystkie problemy.
6. Administrator środowiska przemieszczania wykonuje na żywo wdrożenia do środowiska pomostowego.
7. Rozwiązanie podlega akceptacji użytkownika testowanie w środowisku przemieszczania.
8. Pakiety wdrożeniowe sieci web są importowane ręcznie do środowiska produkcyjnego.

Etapy te stanowią część cyklu programowanie ciągłe.

![](application-lifecycle-management-from-development-to-production/_static/image1.png)

W praktyce proces jest nieco bardziej skomplikowane niż to, jak można zauważyć Jeśli przyjrzymy się na każdym z etapów bardziej szczegółowo. Firma Fabrikam używa innego podejścia do wdrożenia dla każdego środowiska docelowego.

![](application-lifecycle-management-from-development-to-production/_static/image2.png)

Pozostała część tego tematu sprawdza etapy te klucza tego cyklu wdrażania:

- **Wymagania wstępne**: jak konieczne będzie skonfigurowanie infrastruktury serwera, przed wprowadzeniem logiki wdrożenia w miejscu.
- **Początkowa opracowywania i wdrażania**: co należy zrobić przed wdrożeniem rozwiązania po raz pierwszy.
- **Wdrożenie w celu testowania**: jak pakiet i wdróż zawartość do środowiska testowego automatycznie po dewelopera zaewidencjonowaniu nowy kod.
- **Wdrożenie w celu tymczasowego**: Wdrażanie określonych kompilacje do środowiska pomostowego i wykonać "co w przypadku" wdrożeń, aby upewnić się, że wdrożenie nie powoduje żadnych problemów.
- **Wdrożenia do produkcji**: sposób importu pakietów sieci web w środowisku produkcyjnym, gdy infrastruktury sieci uniemożliwia zdalnego wdrażania.

## <a name="prerequisites"></a>Wymagania wstępne

Pierwszym zadaniem w jakimkolwiek scenariuszu wdrażania jest upewnij się, że infrastruktury serwera spełnia wymagania wdrożenia narzędzi i technik. W takim przypadku firmy Fabrikam, Inc. skonfigurował jego infrastruktury serwera, jak to:

- TFS jest skonfigurowany do uwzględnienia kolekcji projektów zespołowych, kontrolerów kompilacji, a agentów kompilacji. Zobacz [Konfigurowanie Team Foundation Server dla automatycznego wdrażania Web](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md) Aby uzyskać więcej informacji.
- Środowisko testowe jest skonfigurowany do akceptowania zdalnych wdrożeń przy użyciu usługi sieci Web wdrożenia agenta ("zdalnego agenta"), zgodnie z opisem w [scenariusz: Konfigurowanie środowisku testu sieci Web wdrożenia](../configuring-server-environments-for-web-deployment/scenario-configuring-a-test-environment-for-web-deployment.md) i [ Konfigurowanie serwera sieci Web narzędzia Web Deploy publikowania (agenta zdalnego)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent.md).
- Środowisko tymczasowe jest skonfigurowany do akceptowania zdalnych wdrożeń przy użyciu punktu końcowego: program obsługi wdrażania w sieci Web, zgodnie z opisem w [scenariusz: Konfigurowanie środowisku przemieszczania Web Deployment](../configuring-server-environments-for-web-deployment/scenario-configuring-a-staging-environment-for-web-deployment.md) i [skonfigurować serwer sieci Web dla sieci Web wdrażanie publikowania (Web Deploy obsługi)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md).
- W środowisku produkcyjnym jest skonfigurowane i umożliwiają administratorowi ręcznie zaimportować pakiety wdrażania w sieci web do Internet Information Services (IIS), zgodnie z opisem w [scenariusz: Konfigurowanie środowiska produkcyjnego Web Deployment](../configuring-server-environments-for-web-deployment/scenario-configuring-a-production-environment-for-web-deployment.md) i [skonfigurować serwer sieci Web narzędzia Web Deploy publikowania (wdrożenie w trybie Offline)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md).

## <a name="initial-development-and-deployment"></a>Początkowego projektowania i wdrażania

Zanim zespół deweloperów firmy Fabrikam, Inc. można wdrożyć rozwiązanie kontaktów Menedżerze po raz pierwszy, należy wykonać te zadania:

- Tworzenie nowego projektu zespołowego w programie TFS.
- Utwórz pliki projektu Microsoft kompilacji Engine (MSBuild), które zawierają logiki wdrożenia.
- Utwórz definicje kompilacji TFS, które mogą powodować procesów wdrażania.

### <a name="create-a-new-team-project"></a>Tworzenie nowego projektu zespołowego

- Administratora TFS, Tomasz Tomasz powoduje utworzenie nowego projektu zespołowego dla aplikacji, zgodnie z opisem w [Tworzenie nowego projektu zespołowego w programie TFS](../configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs.md). Następnie główny programista Matt Hink tworzy szkielet rozwiązanie. ADAM sprawdza jego pliki do nowego projektu zespołowego w programie TFS, zgodnie z opisem w [Dodawanie zawartości do kontroli źródła](../configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control.md).

### <a name="create-the-deployment-logic"></a>Utwórz logiki wdrożenia

Matt Hink tworzy różne niestandardowe pliki projektu MSBuild przy użyciu podejście pliku projektu podziału opisane w [opis pliku projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md). Matt tworzy:

- Plik projektu o nazwie *Publish.proj* , na którym działają procesu wdrażania. Ten plik zawiera docelowych elementów MSBuild kompilacji projektów w rozwiązaniu, tworzenie pakietów sieci web i wdrożenia pakietów do środowiska serwera docelowego.
- Pliki projektu określonego środowiska o nazwie *Env Dev.proj* i *Env Stage.proj*. Zawierają one ustawienia, które są specyficzne dla środowiska testowego i środowisko przejściowe, takie jak parametry połączenia, punkty końcowe usługi i szczegóły usługi zdalnej, która odbierze pakietu sieci web. Aby uzyskać wskazówki dotyczące wybierania odpowiednich ustawień dla środowisk określonych docelowych, zobacz [konfigurowania właściwości wdrożenia dla środowiska docelowego](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).

Aby uruchomić wdrożenie, użytkownik wykonuje *Publish.proj* plików przy użyciu programu MSBuild lub Team Build i określa lokalizację pliku odpowiedni projekt określonego środowiska (*Env Dev.proj* lub *Env Stage.proj*) jako argumentu wiersza polecenia. *Publish.proj* pliku następnie importuje plik projektu określonego środowiska, aby utworzyć kompletny zestaw publikowania instrukcje dla każdego środowiska docelowego.

> [!NOTE]
> Sposób pracy te pliki projektu niestandardowy jest niezależna od mechanizmu używanego do wywoływania MSBuild. Na przykład można wiersza polecenia programu MSBuild bezpośrednio, zgodnie z opisem w [opis pliku projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md). Możesz uruchamiać pliki projektu z pliku poleceń, zgodnie z opisem w [tworzenie i uruchamianie pliku poleceń wdrażania](../web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file.md). Alternatywnie można uruchomić pliki projektu z definicji kompilacji w programie TFS, zgodnie z opisem w [Tworzenie definicji kompilacji tego wdrożenia obsługuje](../configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment.md).  
> W każdym przypadku wynik końcowy jest taki sam&#x2014;MSBuild wykonuje plik projektu scalone i wdraża rozwiązanie na środowisku docelowym. Zapewnia dużą elastyczność w sposób wyzwalania procesu publikowania.


Po on utworzone pliki projektu niestandardowych, Matt dodaje je do folderu rozwiązania i sprawdza ich do kontroli źródła.

### <a name="create-build-definitions"></a>Tworzenie definicji kompilacji

Jako zadanie przygotowanie końcowego Matt i Tomasz współdziałają ze sobą do tworzenia trzech definicji kompilacji dla nowego projektu zespołowego:

- **DeployToTest**. Buduje rozwiązanie kontaktów Menedżerze i wdraża ją do środowiska testowego za każdym razem, gdy ewidencjonowania występuje.
- **DeployToStaging**. Wdraża zasobów z określonego ostatniej kompilacji do środowiska pomostowego, gdy projektant umieszcza w kolejce kompilacji.
- **DeployToStaging-WhatIf**. Wykonuje wdrożenia "co w przypadku" do środowiska pomostowego, gdy projektant umieszcza w kolejce kompilacji.

W kolejnych sekcjach zawierają więcej szczegółów każdego z tych definicje kompilacji.

## <a name="deployment-to-test"></a>Wdrożenie do testu

Zespół deweloperów w firmie Fabrikam, Inc. obsługuje środowisk testowych przeprowadzenie różnych testowania czynności, takich jak weryfikacji i sprawdzania poprawności, testowych użytecznością testowania zgodności i ad hoc lub poznawcze testowanie oprogramowania.

Zespół deweloperów utworzył definicję kompilacji w programie TFS o nazwie **DeployToTest**. Ta definicja kompilacji używa wyzwalacz ciągłej integracji, co oznacza, że proces kompilacji jest uruchamiany za każdym razem, gdy członek zespołu rozwoju firmy Fabrikam, Inc. wykonuje ewidencjonowania. Po wyzwoleniu kompilacji zostanie definicji kompilacji:

- Skompiluj rozwiązanie ContactManager.sln. Tworzy to z kolei każdy projekt w ramach rozwiązania.
- Uruchom wszystkie testy jednostkowe w strukturze folderu rozwiązania (Jeśli rozwiązanie kompilacje pomyślnie).
- Uruchom plików projektów niestandardowych, które kontrolować proces wdrażania (Jeśli rozwiązanie pomyślnie kompilacje i przekazuje wszystkie testy jednostkowe).

W rezultacie jest, że jeśli rozwiązanie pomyślnie kompilacje i przekazuje testów jednostkowych, pakietów sieci web i innych zasobów wdrożenia są wdrażane do środowiska testowego.

![](application-lifecycle-management-from-development-to-production/_static/image3.png)

### <a name="how-does-the-deployment-process-work"></a>Jak działa proces wdrożenia?

**DeployToTest** kompilacji dostaw definicji tych argumentów dla programu MSBuild:


[!code-console[Main](application-lifecycle-management-from-development-to-production/samples/sample1.cmd)]


**DeployOnBuild = true** i **DeployTarget = pakiet** właściwości są używane, gdy Team Build kompiluje projekty w rozwiązaniu. Te właściwości, gdy projekt jest projektem aplikacji sieci web, poinstruuj MSBuild, aby utworzyć pakiet wdrożeniowy sieci web dla projektu. **TargetEnvPropsFile** informuje właściwości *Publish.proj* pliku, gdzie można znaleźć pliku projektu określonego środowiska do zaimportowania.

> [!NOTE]
> Aby uzyskać szczegółowe wskazówki dotyczące sposobu tworzenia definicję kompilacji w następujący sposób, zobacz [Tworzenie definicji kompilacji tego wdrożenia obsługuje](../configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment.md).


*Publish.proj* plik zawiera elementy docelowe, które kompilują każdego projektu w rozwiązaniu. Jednak obejmuje również logikę warunkową czy pomija te kompilacji Jeśli plik jest wykonywany w Team Build. Dzięki temu można wykorzystać funkcje dodatkowe kompilacji Team Build oferuje, takie jak możliwość uruchamiania testów jednostkowych. Jeśli kompilacji rozwiązania lub jednostki testów kończyć się niepowodzeniem, *Publish.proj* pliku nie zostanie wykonany i nie można wdrożyć aplikacji.

Logikę warunkową odbywa się przy ocenie **BuildingInTeamBuild** właściwości. Jest to właściwość MSBuild, która jest automatycznie ustawiana **true** korzystając Team Build do tworzenia projektów.

## <a name="deployment-to-staging"></a>Wdrożenie w celu tymczasowego

Podczas kompilacji spełnia wszystkie wymagania zespół deweloperów w środowisku testowym, zespół może mają zostać wdrożone na tej samej kompilacji w środowisku przemieszczania. Środowiska przejściowe zwykle są skonfigurowane do dopasowania właściwości środowisko produkcyjne lub środowisko "na żywo" jako dokładnie, jak to możliwe, na przykład pod względem specyfikacje serwera, systemów operacyjnych i oprogramowania i konfiguracji sieci. Środowiska przejściowe są często używane dla testów obciążenia, testów akceptacyjnych przez użytkowników i szerszych wewnętrzne przeglądy. Kompilacje są wdrażane w środowisku przemieszczania bezpośrednio z poziomu serwera kompilacji.

![](application-lifecycle-management-from-development-to-production/_static/image4.png)

Definicje kompilacji, używany do wdrażania rozwiązania do środowiska pomostowego **DeployToStaging-WhatIf** i **DeployToStaging**, te cechy:

- Ich nie zbudowaniem żadnych czynności. Gdy Tomasz wdraża rozwiązanie do środowiska pomostowego, chce wdrożyć określone, istniejące kompilacji, która już została zweryfikowana i zweryfikowane w środowisku testowym. Definicje kompilacji, wystarczy na uruchamianie plików niestandardowych projektu, które kontrolują procesu wdrażania.
- Kiedy Tomasz wyzwala kompilacji, używa parametrów kompilacji do określenia, które kompilacji zawiera zasoby, które chce wdrożyć z serwera kompilacji.
- Definicje kompilacji nie są automatycznie wyzwalane. Tomasz ręcznie kolejki kompilację, gdy chce wdrożyć rozwiązanie do środowiska pomostowego.

Oto ogólny proces wdrażania do środowiska pomostowego:

1. Administrator środowiska przemieszczania Tomasz Tomasz, umieszcza w kolejce kompilacji za pomocą **DeployToStaging-WhatIf** definicji kompilacji. Tomasz użyto parametrów definicji kompilacji do określenia kompilacji, które chce wdrożyć.
2. **DeployToStaging-WhatIf** kompilacja definicji uruchamia plików projektu niestandardowych w trybie "co w przypadku". Generuje pliki dziennika, tak jakby Tomasz wykonywała wdrożenia na żywo, ale faktycznie nie wprowadza żadnych zmian w środowisku docelowym.
3. Tomasz przegląda pliki dziennika w celu potwierdzenia skutków wdrożenia w środowisku przemieszczania. W szczególności Tomasz chce, aby sprawdzić, co zostanie dodany, co zostanie zaktualizowana i jakie zostaną usunięte.
4. Jeśli Tomasz jest spełnione, że wdrożenie nie zmiany niepożądanych do istniejących zasobów lub danych, on kolejki przy użyciu kompilacji **DeployToStaging** definicji kompilacji.
5. **DeployToStaging** kompilacja definicji uruchamia plików projektu niestandardowych. Zasoby związane z wdrażaniem tych opublikować na podstawowym serwerze sieci web w środowisku przemieszczania.
6. Kontroler Framework kolektywu serwerów sieci Web (WFF) synchronizuje serwerów sieci web w środowisku przemieszczania. Dzięki temu aplikacja dostępne na wszystkich serwerach sieci web w farmie serwerów.

### <a name="how-does-the-deployment-process-work"></a>Jak działa proces wdrożenia?

**DeployToStaging** kompilacji dostaw definicji tych argumentów dla programu MSBuild:


[!code-console[Main](application-lifecycle-management-from-development-to-production/samples/sample2.cmd)]


**TargetEnvPropsFile** informuje właściwości *Publish.proj* pliku, gdzie można znaleźć pliku projektu określonego środowiska do zaimportowania. **OutputRoot** właściwość przesłonięcia wbudowanej wartości i określa lokalizację folderu kompilacji, który zawiera zasoby, którą chcesz wdrożyć. W przypadku Tomasz umieszcza w kolejce kompilacji, korzysta on **parametry** kartę, aby zapewnić zaktualizowanej wartości do **OutputRoot** właściwości.

![](application-lifecycle-management-from-development-to-production/_static/image5.png)

> [!NOTE]
> Aby uzyskać więcej informacji na temat tworzenia definicji kompilacji, jak to, zobacz [wdrożyć określonej kompilacji](../configuring-team-foundation-server-for-web-deployment/deploying-a-specific-build.md).


**DeployToStaging-WhatIf** definicji kompilacji zawiera tej samej logiki wdrożenia, **DeployToStaging** definicji kompilacji. Jednak zawiera dodatkowy argument **WhatIf = true**:


[!code-console[Main](application-lifecycle-management-from-development-to-production/samples/sample3.cmd)]


W ramach *Publish.proj* pliku **WhatIf** właściwość wskazuje, że wszystkie zasoby wdrożenia powinien zostać opublikowany w trybie "co w przypadku". Innymi słowy pliki dziennika są generowane, tak jakby wdrożenie ma wyprzedzeniem usunięty, ale nic nie jest rzeczywiste zmiany w środowisku docelowym. Pozwala to ocenić wpływ wdrożenia proponowanych&#x2014;w szczególności, co spowoduje zostaną dodane, co będzie aktualizowany i co zostanie usunięty&#x2014;przed wprowadzeniem faktycznie żadnych zmian.

> [!NOTE]
> Aby uzyskać więcej informacji na temat konfigurowania "co w przypadku" wdrożenia, zobacz [wykonywania wdrożenia "Co w przypadku"](../advanced-enterprise-web-deployment/performing-a-what-if-deployment.md).


Po wdrożeniu aplikacji na podstawowym serwerze sieci web w środowisku przemieszczania, WFF automatycznie zsynchronizuj aplikację na wszystkich serwerach w farmie serwerów.

> [!NOTE]
> Aby uzyskać więcej informacji na temat konfigurowania WFF do synchronizowania serwerów sieci web, zobacz [utworzyć farmę serwerów z struktura farmy sieci Web](../configuring-server-environments-for-web-deployment/creating-a-server-farm-with-the-web-farm-framework.md).


## <a name="deployment-to-production"></a>Wdrożenia do produkcji

Po zatwierdzeniu kompilacji w środowisku przemieszczania team firmy Fabrikam, Inc. można opublikować aplikacji do środowiska produkcyjnego. Środowiska produkcyjnego jest, gdzie aplikacja przechodzi "na żywo" i jego docelowi użytkownicy końcowi osiągnie.

Środowiska produkcyjnego znajduje się w sieci obwodowej internetowy. To jest odizolowana od sieci wewnętrznej, która zawiera serwer kompilacji. Administrator środowiska produkcyjnego, Andrews Ewa ręcznie należy skopiować pakiety wdrożeniowe sieci web z poziomu serwera kompilacji i zaimportuj je do usług IIS na serwerze sieci web produkcji podstawowej.

![](application-lifecycle-management-from-development-to-production/_static/image6.png)

Oto ogólny proces wdrażania w środowisku produkcyjnym:

1. Zespół deweloperów z informacją o tym Ewa czy kompilacja jest gotowa do wdrożenia w środowisku produkcyjnym. Zespół z informacją o tym Ewa lokalizacji pakiety wdrożeniowe sieci web w ramach folderu przechowywania na serwerze kompilacji.
2. Ewa zbiera pakietów sieci web z poziomu serwera kompilacji i kopiuje je na podstawowym serwerze sieci web w środowisku produkcyjnym.
3. Ewa używa Menedżera usług IIS, aby zaimportować i publikowania pakietów sieci web na podstawowym serwerze sieci web.
4. Kontroler WFF synchronizuje serwerów sieci web w środowisku produkcyjnym. Dzięki temu aplikacja dostępne na wszystkich serwerach sieci web w farmie serwerów.

### <a name="how-does-the-deployment-process-work"></a>Jak działa proces wdrożenia?

Menedżer usług IIS zawiera importu Kreatora pakietu aplikacji, który ułatwia publikowanie pakietów sieci web do witryny sieci Web usług IIS. Aby uzyskać wskazówki na temat sposobu wykonania tej procedury, zobacz [ręczne instalowanie pakietów sieci Web](../web-deployment-in-the-enterprise/manually-installing-web-packages.md).

## <a name="conclusion"></a>Wniosek

W tym temacie podano ilustrację cykl życia wdrożenia dla aplikacji sieci web typowe skali przedsiębiorstwa.

Ten temat jest częścią serii samouczków, które zawierają wskazówki dotyczące różnych aspektów wdrożenia aplikacji sieci web. W praktyce istnieje wiele dodatkowych zadań i uwagi na każdym etapie procesu wdrażania, a nie jest możliwe pokrywał się je w jednej wskazówki. Aby uzyskać więcej informacji zapoznaj się te samouczki:

- [Narzędzie Web Deployment w przedsiębiorstwie](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Ten samouczek zawiera obszerne techniki wdrażania sieci web przy użyciu programu MSBuild i narzędzia wdrażania usług IIS sieci Web (Web Deploy).
- [Konfigurowanie środowisk serwera sieci Web wdrożenia](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md). Ten samouczek zawiera wskazówki dotyczące sposobu konfigurowania systemu Windows server w środowiskach do obsługi różnych scenariuszy wdrażania.
- [Konfigurowania serwera Team Foundation Server dla automatycznego wdrażania Web](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). Ten samouczek zawiera wskazówki dotyczące sposobu integracji logiki wdrożenia do procesów kompilacji TFS.
- [Zaawansowane wdrażanie w przedsiębiorstwie sieci Web](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). Ten samouczek zawiera wskazówki dotyczące sposobu spełniają niektóre wyzwania bardziej złożone wdrożenia napotykane przez organizacje.

> [!div class="step-by-step"]
> [Poprzednie](enterprise-web-deployment-scenario-overview.md)
