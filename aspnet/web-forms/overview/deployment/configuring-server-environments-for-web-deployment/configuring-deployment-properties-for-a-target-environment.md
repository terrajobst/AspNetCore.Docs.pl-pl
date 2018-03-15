---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment
title: "Konfigurowanie właściwości wdrożenia dla środowiska docelowego | Dokumentacja firmy Microsoft"
author: jrjlee
description: "W tym temacie opisano sposób konfigurowania właściwości specyficzne dla środowiska, aby wdrożyć przykładowe rozwiązanie kontaktów Menedżerze do danego środowiska docelowego..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: b5b86e03-b8ed-46e6-90fa-e1da88ef34e9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment
msc.type: authoredcontent
ms.openlocfilehash: f27b8376b332ff21185be0fd5c00ced7d40a20bd
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/15/2018
---
<a name="configuring-deployment-properties-for-a-target-environment"></a>Konfigurowanie właściwości wdrożenia dla środowiska docelowego
====================
przez [Lewandowski Jason](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> W tym temacie opisano sposób konfigurowania właściwości specyficzne dla środowiska, aby wdrożyć przykładowe rozwiązanie kontaktów Menedżerze do danego środowiska docelowego.


Ten temat jest częścią serii samouczków na podstawie tych wymagań związanych z przedsiębiorstwa wdrażaniem fikcyjnej firmy o nazwie firmy Fabrikam, Inc. Przykładowe rozwiązanie & #x 2014; korzysta z tego samouczka serii [kontaktów Menedżerze](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) #x 2014; & rozwiązania do reprezentowania aplikacji sieci web z realistyczne poziom złożoności, w tym aplikacji ASP.NET MVC 3, systemu Windows Usługi Communication Foundation (WCF), a projekt bazy danych.

Istotą te samouczki metody wdrażania opiera się na podejście pliku projektu podziału opisane w [opis procesu kompilacji](../web-deployment-in-the-enterprise/understanding-the-build-process.md), w którym jest kontrolowany przez proces kompilacji projektu dwa pliki & #x 2014; jeden zawierający Tworzenie instrukcji, które mają zastosowanie do każdego środowiska docelowego i dysk zawierający ustawienia kompilacji i wdrożenia określonego środowiska. W czasie kompilacji pliku projektu określonego środowiska jest scalany pliku projektu niezależny od środowiska pełny zestaw instrukcji kompilacji.

## <a name="process-overview"></a>Omówienie procesu

Plik projektu, który ma być używany do tworzenia i wdrażania rozwiązania kontaktów Menedżerze jest podzielony na dwa pliki fizyczne:

- Jeden zawierający uniwersalnego kompilacji, ustawienia i instrukcje ( *Publish.proj* pliku).
- Jeden zawierający określonego środowiska ustawienia kompilacji (*Env Dev.proj*, *Env Stage.proj*i tak dalej).

W czasie kompilacji pliku odpowiedni projekt określonego środowiska jest scalany uniwersalnego *Publish.proj* pliku pełny zestaw instrukcji kompilacji. Można skonfigurować wdrożenie do określonego miejsca docelowego środowisk przez utworzenie lub dostosowywanie plików projektu określonego środowiska przy użyciu ustawień, które opisano scenariusz wdrażania.

Wiele z tych wartości są określane na podstawie konfiguracji środowiska docelowego & #x 2014 r., w szczególności, czy Twoje docelowego serwera sieci web jest skonfigurowany do używania usługi sieci Web wdrożenia Agent (agent zdalnego) lub program obsługi wdrażania sieci Web. Aby uzyskać więcej informacji na temat tych metod oraz wskazówki dotyczące wybierania podejście dla własnego środowiska, zobacz [Wybieranie podejście prawo do wdrożenia w sieci Web](choosing-the-right-approach-to-web-deployment.md).

[Scenariusza menedżera kontaktu](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md) wymaga dwóch plików projektu określonego środowiska:

- Wdrożenia w środowisku testowym developer (*Env Dev.proj*). Środowisko testowe dewelopera jest skonfigurowany do akceptowania zdalnych wdrożeń za pomocą zdalnego agenta, zgodnie z opisem w [scenariusz: Konfigurowanie środowisku testu sieci Web wdrożenia](scenario-configuring-a-test-environment-for-web-deployment.md). Ten plik należy podać agenta zdalnego adres punktu końcowego, a także ustawienia specyficzne dla lokalizacji, takie jak parametry połączenia i punktów końcowych usługi.
- Wdrożenia w środowisku przemieszczania (*Env Stage.proj*). Środowisko tymczasowe jest skonfigurowany do akceptowania zdalnych wdrożeń za pomocą obsługi wdrażania w sieci Web, zgodnie z opisem w [scenariusz: Konfigurowanie środowisku przemieszczania Web Deployment](scenario-configuring-a-staging-environment-for-web-deployment.md). Ten plik należy podać adres punktu końcowego program obsługi wdrażania w sieci Web, a także ustawienia specyficzne dla lokalizacji, takie jak parametry połączenia i punkty końcowe usługi.

Jest ważne, należy pamiętać, że ustawienia skonfigurowane w pliku projektu określonego środowiska nie mają wpływu na zawartość sieci web pakiet sam & #x 2014; zamiast kontrolowania sposobu wdrażania pakietu i jakie wartości parametrów są dostępne podczas pakiet jest wyodrębniana. W przypadku importowania pakietu sieci web w środowisku produkcyjnym ręcznie, zgodnie z opisem w [scenariusz: Konfigurowanie środowiska produkcyjnego Web Deployment](scenario-configuring-a-production-environment-for-web-deployment.md) i [ręczne instalowanie pakietów sieci Web](../web-deployment-in-the-enterprise/manually-installing-web-packages.md), więc nie ma znaczenia, jakie ustawienia używane w pliku projektu określonego środowiska podczas generowania pakietu. Internet Information Services (IIS) Manager wyświetli monit o dowolnej wartości sparametryzowane przyjmują, takich jak parametry połączenia i punktów końcowych usługi, po zaimportowaniu pakietu.

Aby wdrożyć rozwiązanie kontaktów Menedżerze środowiska docelowego, można dostosować ten plik lub użyj go jako szablon i Utwórz własny plik.

**Aby skonfigurować ustawienia określonego środowiska wdrażania rozwiązania Menedżera skontaktuj się z**

1. Otwórz rozwiązanie ContactManager WCF w programie Visual Studio 2010.
2. W **Eksploratora rozwiązań** okna, rozwiń węzeł **publikowania** folder, rozwiń węzeł **EnvConfig** folder, a następnie kliknij dwukrotnie plik **Env-Dev.proj**.

    ![](configuring-deployment-properties-for-a-target-environment/_static/image1.png)
3. Zastąp wartości właściwości w *Env Dev.proj* pliku przy użyciu prawidłowych wartości dla środowiska testowego.

    > [!NOTE]
    > Tabela, z tą procedurą zapewnia więcej informacji na każdej z tych właściwości.
4. Zapisz swoją pracę, a następnie Zamknij *Env Dev.proj* pliku.

## <a name="choosing-the-right-deployment-properties"></a>Wybieranie właściwości wdrożenia w prawo

Poniższa tabela zawiera opis przeznaczenia każdej właściwości w przykładowym pliku projektu określonego środowiska *Env Dev.proj*i zawiera pewne wskazówki dotyczące wartości, należy podać.

| Nazwa właściwości | Szczegóły |
| --- | --- |
| **MSDeployComputerName** nazwę docelowej sieci web usługi lub serwera punktu końcowego. | W przypadku instalowania usługi agenta zdalnego na docelowym serwerze sieci web, można określić nazwy komputera docelowego (na przykład **TESTWEB1** lub **TESTWEB1.fabrikam.net**), albo określić zdalne Agent endpoint (na przykład `http://TESTWEB1/MSDEPLOYAGENTSERVICE`). Wdrożenie działa tak samo w każdym przypadku. Jeśli wdrażasz do obsługi wdrażania sieci Web na serwerze docelowym, należy określić punkt końcowy usługi i uwzględnić nazwę witryny sieci Web usług IIS jako parametr ciągu zapytania (na przykład `https://STAGEWEB1:8172/MSDeploy.axd?site=DemoSite`). |
| **MSDeployAuth** metodę, która narzędzia Web Deploy powinna być używana do uwierzytelniania na komputerze zdalnym. | To powinien być ustawiony na **NTLM** lub **podstawowe**. Zazwyczaj użyjesz **NTLM** w przypadku instalowania usługi zdalnego agenta i **podstawowe** Jeśli wdrażasz do obsługi wdrażania w sieci Web. Jeśli używane jest uwierzytelnianie podstawowe, należy określić nazwę użytkownika i hasło, które ma być Personifikowane narzędzia wdrażania usług IIS sieci Web (Web Deploy) w celu wdrożenia. W tym przykładzie, te wartości są realizowane za pośrednictwem **MSDeployUsername** i **MSDeployPassword** właściwości. Jeśli używane jest uwierzytelnianie NTLM, można pominąć te właściwości lub pozostaw je puste. |
| **MSDeployUsername** Jeśli używane jest uwierzytelnianie podstawowe, narzędzie Web Deploy będzie używała tego konta na komputerze zdalnym. | To powinno mieć postać *domeny*\*username * (na przykład **FABRIKAM\matt**). Ta wartość jest używana tylko w przypadku, jeśli określisz uwierzytelnianie podstawowe. Jeśli używane jest uwierzytelnianie NTLM, można pominąć właściwości. Jeśli wartość zostanie podana, zostaną zignorowane. |
| **MSDeployPassword** Jeśli używane jest uwierzytelnianie podstawowe, narzędzie Web Deploy użyje tego hasła na komputerze zdalnym. | Jest to hasło dla konta użytkownika określonego w **MSDeployUsername** właściwości. Ta wartość jest używana tylko w przypadku, jeśli określisz uwierzytelnianie podstawowe. Jeśli używane jest uwierzytelnianie NTLM, można pominąć właściwości. Jeśli wartość zostanie podana, zostaną zignorowane. |
| **ContactManagerIisPath** ścieżki usług IIS, na którym chcesz wdrożyć aplikacji MVC kontaktów Menedżerze. | Powinna to być ścieżka wyświetlaną w Menedżerze usług IIS w formularzu [*nazwa witryny sieci Web usług IIS*] / [*web ** Nazwa aplikacji*]. Należy pamiętać, że witryna sieci Web IIS musi istnieć przed wdrożeniem aplikacji. Na przykład jeśli po utworzeniu witryny sieci Web usług IIS o nazwie DemoSite, można określić ścieżki IIS dla aplikacji MVC jako DemoSite/ContactManager. |
| **ContactManagerServiceIisPath** ścieżki usług IIS, na którym chcesz wdrożyć usługę WCF kontaktów Menedżerze. | Na przykład, jeśli po utworzeniu witryny sieci Web usług IIS o nazwie DemoSite, można określić ścieżki IIS dla usługi WCF, ponieważ **DemoSite/ContactManagerService**. |
| **ContactManagerTargetUrl** adres URL, pod którym usługa WCF jest osiągalna. | Będzie to mieć postać [*adres URL witryny sieci Web IIS*] / [*Nazwa aplikacji usługi*] / [*punktu końcowego usługi*]. Na przykład, jeśli po utworzeniu witryny sieci Web usług IIS na porcie 85 adres URL będzie mieć postać `http://localhost:85/ContactManagerService/ContactService.svc`. Należy pamiętać, że aplikację MVC oraz usługi WCF są wdrażane do tego samego serwera. W związku z tym ten adres URL jest dostępny tylko na komputerze, na którym jest zainstalowany. W związku z tym lepiej jest użyć localhost lub adres IP, a nie nazwa komputera lub nagłówek hosta, adres URL. Jeśli używasz nazwę komputera lub nagłówek hosta, [wyboru sprzężenia zwrotnego](https://go.microsoft.com/?linkid=9805131) funkcji zabezpieczeń w usługach IIS może blokować adresy URL i zwraca **HTTP 401.1 — Dostęp nieautoryzowany** błędu. |
| **CmDatabaseConnectionString** ciąg połączenia dla serwera bazy danych. | Parametry połączenia określa zarówno poświadczeń używanych VSDBCMD Aby skontaktować się z serwerem bazy danych i utworzyć bazę danych i poświadczeń używanych pulę aplikacji serwera sieci web do kontaktowania się z serwerem bazy danych i interakcji z bazą danych. Zasadniczo dostępne są dwie opcje w tym miejscu. Można określić **Integrated Security = true**, w takim przypadku jest stosowane zintegrowane uwierzytelnianie systemu Windows: **źródła danych = TESTDB1; Integrated Security = true** w takim przypadku baza danych zostanie utworzona z użyciem poświadczenia użytkownika, który uruchamia VSDBCMD pliku wykonywalnego i aplikacji będą uzyskiwać dostęp do bazy danych przy użyciu tożsamości konto komputera serwera sieci web. Alternatywnie można określić nazwę użytkownika i hasło konta programu SQL Server. W takim przypadku poświadczenia serwera SQL są używane zarówno przez VSDBCMD utworzyć bazę danych, jak i w puli aplikacji do interakcji z bazą danych: **źródła danych = TESTDB1; Nazwa użytkownika = ASqlUser; Hasło = Pa$ $w0rd** wskazówki w tym temacie założono użyjesz zintegrowane uwierzytelnianie systemu Windows. |
| **CmTargetDatabase** nazwę bazy danych zostaną utworzone na serwerze bazy danych. | Wartość podana tutaj jest dodawana do polecenia VSDBCMD jako parametr. Służy również do kompilacji pełne parametry połączenia używanego przez pulę aplikacji na serwerze sieci web do interakcji z bazą danych. |
  

Poniższe przykłady pokazują, jak można skonfigurować te właściwości dla scenariuszy wdrażania.

### <a name="example-1x2014deployment-to-the-remote-agent-service"></a>Przykład 1 & #x 2014; wdrożenia z usługą agenta zdalnego

W tym przykładzie:

- Jest wdrażany z usługą agenta zdalnego na TESTWEB1.
- W przypadku poinstruowanie, narzędzie Web Deploy do użycia uwierzytelniania NTLM. Narzędzie Web Deploy zostanie uruchomiony przy użyciu poświadczeń, którego użyto do wywołania aparat kompilacji firmy Microsoft (MSBuild).
- Aby wdrożyć używasz zintegrowanego uwierzytelniania **ContactManager** TESTDB1 bazy danych. Bazy danych zostanie wdrożony przy użyciu poświadczeń używanych do wywołania programu MSBuild.


[!code-xml[Main](configuring-deployment-properties-for-a-target-environment/samples/sample1.xml)]


### <a name="example-2x2014deployment-to-the-web-deploy-handler-endpoint"></a>Przykład 2 & #x 2014; wdrażania w sieci Web wdrażanie obsługi punktu końcowego

W tym przykładzie:

- Jest wdrażany z punktem końcowym usługi sieci Web obsługi wdrażania na STAGEWEB1.
- W przypadku poinstruowanie, narzędzie Web Deploy na uwierzytelnianie podstawowe.
- W przypadku określania, czy narzędzie Web Deploy ma być Personifikowane FABRIKAM\stagingdeployer konto na komputerze zdalnym.
- Używasz uwierzytelniania programu SQL Server, aby wdrożyć **ContactManager** STAGEDB1 bazy danych.


[!code-xml[Main](configuring-deployment-properties-for-a-target-environment/samples/sample2.xml)]


## <a name="conclusion"></a>Wniosek

W tym momencie pliki programu project pełni są skonfigurowane do tworzenia i wdrażania rozwiązania z menedżerem skontaktuj się z co najmniej jednego środowiska docelowego.

Aby używać tych plików projektu jako część procesu wdrażania krok pojedynczej i powtarzalnej, musisz wykonać *Publish.proj* plików przy użyciu programu MSBuild i podaj lokalizację pliku projektu określonego środowiska jako parametr. Można to zrobić na różne sposoby:

- Omówienie programu MSBuild i wprowadzenie do plików projektów niestandardowych, zobacz [opis pliku projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md).
- Informacje na temat sposobu sformułować polecenie MSBuild, które wykonuje plików projektu niestandardowych, zobacz [wdrażanie pakietów sieci Web](../web-deployment-in-the-enterprise/deploying-web-packages.md).
- Aby uzyskać informacje o tym, jak wdrożyć polecenia programu MSBuild w pliku poleceń dla kroku pojedynczej i powtarzalnej wdrożeń, zobacz [tworzenie i uruchamianie pliku poleceń wdrażania](../web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file.md).
- Aby uzyskać informacje na temat sposobu wykonywania plików niestandardowe projektu z poziomu kompilacji zespołowej, zobacz [Tworzenie definicji kompilacji tego wdrożenia obsługuje](../configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment.md).

>[!div class="step-by-step"]
[Poprzednie](creating-a-server-farm-with-the-web-farm-framework.md)
