---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-web-packages
title: Wdrażanie pakietów internetowych | Dokumentacja firmy Microsoft
author: jrjlee
description: W tym temacie opisano, jak opublikować pakiety wdrażania sieci web na serwerze zdalnym za pomocą usług Internet Information Services (IIS) Narzędzie Web Deployment (Web...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: a5c5eed2-8683-40a5-a2e1-35c9f8d17c29
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-web-packages
msc.type: authoredcontent
ms.openlocfilehash: d7a17a2830ab044f6f7446edc18c394d97b09fa0
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755048"
---
<a name="deploying-web-packages"></a>Wdrażanie pakietów internetowych
====================
przez [Jason Lee](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> W tym temacie opisano, jak opublikować pakiety wdrażania sieci web na serwerze zdalnym za pomocą narzędzia wdrażania usług Internet Information Services (IIS) w sieci Web (Web Deploy) w wersji 2.0.
> 
> Istnieją dwa główne sposoby, w których można wdrożyć pakiet sieci web na serwerze zdalnym:
> 
> - Można użyć narzędzia wiersza polecenia MSDeploy.exe bezpośrednio.
> - Możesz uruchomić *.deploy.cmd [Nazwa projektu]* plik, który generuje procesu kompilacji.
> 
> Efekt jest taki sam niezależnie od tego podejścia, którego używasz. Zasadniczo wszystkie *. pliku deploy.cmd* plik jest uruchomienie MSDeploy.exe z niektórych wstępnie zdefiniowanych wartości, dzięki czemu nie trzeba podać jak najwięcej informacji, aby wdrożyć pakiet. Upraszcza to proces wdrażania. Z drugiej strony bezpośrednio za pomocą MSDeploy.exe zapewnia znacznie większą elastyczność nad dokładnie sposób wdrażania pakietu.
> 
> Które rozwiązanie, możesz użyć będzie zależeć od wielu czynników, łącznie z jaką kontrolę wymagane przez proces wdrażania i tego, czy zostaną objęci usługa zdalnego agenta narzędzia Web Deploy lub program obsługi wdrażania sieci Web. W tym temacie wyjaśniono, jak używać każde podejście i umożliwia określenie, kiedy każde podejście jest odpowiednie.
> 
> Zadania i wskazówki, w tym temacie przyjęto założenie, że:
> 
> - Możesz już skompilowania i spakowania aplikację sieci web, zgodnie z opisem w [budowanie i projektów aplikacji sieci Web pakietu](building-and-packaging-web-application-projects.md).
> - Zmodyfikowano *SetParameters.xml* plik, aby podać wartości parametrów odpowiednie dla danego środowiska docelowego, zgodnie z opisem w [konfigurowania parametrów wdrożenia pakietu internetowego](configuring-parameters-for-web-package-deployment.md).


Uruchamianie [*Nazwa projektu*]*. pliku deploy.cmd* plik jest najprostszym sposobem wdrażania pakietu sieci web. W szczególności, za pomocą *. pliku deploy.cmd* pliku oferuje następujące korzyści za pośrednictwem bezpośrednio za pomocą MSDeploy.exe:

- Nie potrzebujesz określić lokalizację pakietu wdrażania sieci web&#x2014; *. pliku deploy.cmd* pliku wie już, gdzie jest.
- Nie musisz określić lokalizację *SetParameters.xml* pliku&#x2014; *. pliku deploy.cmd* pliku wie już, gdzie jest.
- Określ dostawców MSDeploy źródłowym i docelowym nie ma potrzeby&#x2014; *. pliku deploy.cmd* pliku już zna wartości, które mają być używane.
- Nie należy określić ustawienia działania MSDeploy&#x2014; *. pliku deploy.cmd* pliku najczęściej wymaganych wartości do polecenia MSDeploy.exe automatycznie dodaje.

Przed użyciem *. pliku deploy.cmd* plik, aby wdrożyć pakiet sieci web, należy upewnić się, że:

- *. Pliku deploy.cmd* pliku [*Nazwa projektu*]. *SetParameters.xml* plików i pakiet sieci web ([*Nazwa projektu*]. *ZIP*) znajdują się w tym samym folderze.
- Narzędzie Web Deploy (MSDeploy.exe) jest zainstalowany na komputerze z uruchomionym *. pliku deploy.cmd* pliku.

*. Pliku deploy.cmd* pliku obsługuje różne opcje wiersza polecenia. Po uruchomieniu pliku w wierszu polecenia, jest to podstawowa składnia:


[!code-console[Main](deploying-web-packages/samples/sample1.cmd)]


Należy określić **/t** flagi lub **/Y** flagi, aby wskazać, czy chcesz przeprowadzić wdrożenie na żywo lub próbnej odpowiednio (nie używaj obie flagi w tym samym poleceniu). Ta tabela zawiera wyjaśnienie przeznaczenia każdego z tych flag.

| Flaga | Opis |
| --- | --- |
| **/T** | Wywołuje MSDeploy.exe z **whatif** flagę wskazującą uruchomienia próbnego. Zamiast wdrażania pakietu, tworzy raport co się stanie, jeśli pakiet został wdrożony. |
| **/Y** | Wywołuje MSDeploy.exe bez **whatif** flagi. To wdrożenie pakietu na komputerze lokalnym lub wybrany serwer docelowy. |
| **/M** | Określa serwer docelowy nazwę lub adres URL usługi. Aby uzyskać więcej informacji na podstawie wartości podanych tutaj zobaczyć **zagadnienia dotyczące punktu końcowego** w tym temacie. Jeżeli pominięto **/M** flagi, pakiet zostanie wdrożony na komputerze lokalnym. |
| **/A** | Określa typ uwierzytelniania, która MSDeploy.exe powinna być używana do wykonywania wdrożenia. Możliwe wartości to **NTLM** i **podstawowe**. Jeżeli pominięto **/A** flagi, typ uwierzytelniania, który domyślnie przyjmuje wartość **NTLM** dla wdrożenia usługi zdalnego agenta narzędzia Web Deploy a do **podstawowe** wdrożenia do narzędzia Web Deploy Program obsługi. |
| **/U** | Określa nazwę użytkownika. Dotyczy to tylko wtedy, gdy używasz uwierzytelniania podstawowego. |
| **/P** | Określa hasło. Dotyczy to tylko wtedy, gdy używasz uwierzytelniania podstawowego. |
| **/L** | Wskazuje, że pakiet powinny być wdrażane z lokalnym wystąpieniem usług IIS Express. |
| **/G** | Określa, że pakiet jest wdrażany za pomocą [ustawienie dostawcy tempAgent](https://technet.microsoft.com/library/ee517345(WS.10).aspx). Jeżeli pominięto **/G** flagi, przyjmowana jest wartość domyślna **false**. |

> [!NOTE]
> Za każdym razem, gdy proces kompilacji tworzy pakiet sieci web, również tworzy plik o nazwie *[Nazwa projektu] .deploy-readme.txt* objaśniający te opcje wdrażania.


Oprócz tych flag można określić ustawienia działania narzędzia Web Deploy jako dodatkowe *. pliku deploy.cmd* parametrów. Dodatkowe ustawienia, które określisz po prostu są przekazywane do źródłowej polecenie MSDeploy.exe. Aby uzyskać więcej informacji na temat tych ustawień, zobacz [ustawienia operacji wdrażania w sieci Web](https://technet.microsoft.com/library/dd569089(WS.10).aspx).

Załóżmy, że mają zostać wdrożone ContactManager.Mvc projektu aplikacji sieci web w środowisku testowym, uruchamiając *. pliku deploy.cmd* pliku. Środowiska testowego jest skonfigurowany do korzystania z usługi zdalnego agenta narzędzia Web Deploy, zgodnie z opisem w [Konfigurowanie serwera sieci Web dla wdrożenia publikowania w sieci Web (Agent zdalny)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent.md). Aby wdrożyć aplikację sieci web, należy wykonać poniższe czynności.

**Aby wdrożyć aplikację sieci web przy użyciu. pliku deploy.cmd pliku**

1. Tworzenie i pakowanie projektu aplikacji sieci web, zgodnie z opisem w [budowanie i projektów aplikacji sieci Web pakietu](building-and-packaging-web-application-projects.md).
2. Modyfikowanie *ContactManager.Mvc.SetParameters.xml* plik będzie zawierał wartości odpowiedni parametr dla danego środowiska testowego zgodnie z opisem w [konfigurowania parametrów wdrożenia pakietu internetowego](configuring-parameters-for-web-package-deployment.md).
3. Otwórz okno wiersza polecenia i przejdź do lokalizacji *ContactManager.Mvc.deploy.cmd* pliku.
4. Wpisz następujące polecenie i naciśnij klawisz Enter:

    [!code-console[Main](deploying-web-packages/samples/sample2.cmd)]

W tym przykładzie:

- **/Y** flaga wskazuje, że chcesz pakiet zostanie rzeczywiście wdrożony, a nie podczas korzystania z wersji próbnej Uruchom.
- **/M** flaga wskazuje, że ma zostać wdrożony pakiet funkcji na serwerze o nazwie TESTWEB1. Z tej wartości MSDeploy.exe podejmie próbę wdrożyć pakiet usługi zdalnego agenta narzędzia Web Deploy na http://TESTWEB1/MSDeployAgentService.
- **/A** flaga wskazuje, że chcesz użyć uwierzytelniania NTLM. W efekcie nie musisz określić nazwę użytkownika i hasło.

Aby zilustrować, jak przy użyciu *. pliku deploy.cmd* pliku upraszcza proces wdrażania, Przyjrzyj się polecenie MSDeploy.exe, który pobiera wygenerowany i wykonać po uruchomieniu *ContactManager.Mvc.deploy.cmd* przy użyciu opcji wymienionych powyżej.


[!code-console[Main](deploying-web-packages/samples/sample3.cmd)]


Aby uzyskać więcej informacji na temat korzystania z *. pliku deploy.cmd* plik, aby wdrożyć pakiet sieci web, zobacz [porady: Instalowanie wdrażania pakietu przy użyciu pliku deploy.cmd pliku](https://msdn.microsoft.com/library/ff356104.aspx).

## <a name="using-msdeployexe"></a>Za pomocą MSDeploy.exe

Mimo że używanie *. pliku deploy.cmd* pliku ogólnie upraszcza proces wdrażania, istnieje kilka sytuacji, gdy jest ona lepiej jest używać MSDeploy.exe bezpośrednio. Na przykład:

- Jeśli chcesz wdrożyć do obsługi wdrażania sieci Web jako użytkownik bez uprawnień administratora, nie możesz użyć *. pliku deploy.cmd* pliku. Jest to spowodowane błędem w narzędziu Web Deploy 2.0, zgodnie z opisem w obszarze **zagadnienia dotyczące punktu końcowego**.
- Jeśli chcesz ręcznie przełączać się między różnymi *SetParameters.xml* plików w różnych lokalizacjach, warto użyć MSDeploy.exe bezpośrednio.
- Jeśli chcesz przesłonić kilka MSDeploy.exe argumenty wiersza polecenia, można użyć MSDeploy.exe bezpośrednio.

Gdy używasz MSDeploy.exe, należy podać trzy kluczowe informacje:

- A **— źródło** parametrem, który wskazuje, skąd pochodzą dane.
- A **— dest** parametrem, który wskazuje, gdzie dane.
- A **— zlecenie** parametrem, który wskazuje [operacji](https://technet.microsoft.com/library/dd568989(WS.10).aspx) chcesz wykonać.

MSDeploy.exe opiera się na [narzędzia Web Deploy dostawców](https://technet.microsoft.com/library/dd569040(WS.10).aspx) do przetwarzania danych źródłowych i docelowych. Narzędzie Web Deploy obejmuje wiele dostawców, które reprezentują zakres źródeł danych i aplikacji może pracować&#x2014;na przykład istnieją dostawców dla baz danych programu SQL Server, serwery sieci web usług IIS, certyfikaty i globalnej zestawów (GAC) w pamięci podręcznej, różne różnymi plikami konfiguracji, a wiele innych typów danych. Zarówno **— źródło** parametru i **— dest** parametr musi określać dostawcy, w postaci **— źródło**: [*providerName*] = [*lokalizacji*]. Podczas wdrażania pakietu sieci web w witrynie sieci Web usług IIS, należy użyć tych wartości:

- **— Źródło** dostawcy jest zawsze [pakietu](https://technet.microsoft.com/library/dd569019(WS.10).aspx). Na przykład:

    [!code-console[Main](deploying-web-packages/samples/sample4.cmd)]
- **— Dest** dostawcy jest zawsze [automatycznie](https://technet.microsoft.com/library/dd569016(WS.10).aspx). Na przykład:

    [!code-console[Main](deploying-web-packages/samples/sample5.cmd)]
- **— Zlecenie** jest zawsze **synchronizacji**.

    [!code-console[Main](deploying-web-packages/samples/sample6.cmd)]

Ponadto musisz określić różnych innych [ustawienia specyficzne dla dostawcy](https://technet.microsoft.com/library/dd569001(WS.10).aspx) i ogólne [ustawienia działania](https://technet.microsoft.com/library/dd569089(WS.10).aspx). Na przykład załóżmy, że chcesz wdrożyć aplikację sieci web ContactManager.Mvc w środowisku przejściowym. Wdrożenie będą ukierunkowane na program obsługi wdrażania sieci Web i muszą korzystać z uwierzytelniania podstawowego. Aby wdrożyć aplikację sieci web, należy wykonać poniższe czynności.

**Aby wdrożyć aplikację sieci web przy użyciu MSDeploy.exe**

1. Tworzenie i pakowanie projektu aplikacji sieci web, zgodnie z opisem w [budowanie i projektów aplikacji sieci Web pakietu](building-and-packaging-web-application-projects.md).
2. Modyfikowanie *ContactManager.Mvc.SetParameters.xml* plik będzie zawierał wartości odpowiedni parametr w środowisku przejściowym, zgodnie z opisem w [konfigurowania parametrów wdrożenia pakietu internetowego](configuring-parameters-for-web-package-deployment.md).
3. Otwórz okno wiersza polecenia i przejdź do lokalizacji programu MSDeploy.exe. Jest to zazwyczaj na %PROGRAMFILES%\IIS\Microsoft V2\msdeploy.exe wdrażania sieci Web.
4. Wpisz następujące polecenie i naciśnij klawisz Enter (Zignoruj podziały wiersza):

    [!code-console[Main](deploying-web-packages/samples/sample7.cmd)]

W tym przykładzie:

- **— Źródło** parametr określa **pakietu** dostawcy i wskazuje lokalizację pakietu sieci web.
- **— Dest** parametr określa **automatycznie** dostawcy. **ComputerName** ustawienie udostępnia adres URL usługi sieci Web wdrażanie obsługi na serwerze docelowym. **Authtype** ustawienie wskazuje, że chcesz użyć uwierzytelniania podstawowego, a w związku z tym należy podać **username** i **hasło**. Na koniec **includeAcls = "False"** ustawienie wskazuje, że nie chcesz skopiować listy kontroli dostępu (ACL) plików w aplikacji sieci web źródłowego na serwer docelowy.
- **— Zlecenie: synchronizacja** argument wskazuje, czy chcesz replikować zawartość źródłową na serwerze docelowym.
- **— DisableLink** argumenty wskazują, że nie chcesz replikować pul aplikacji, katalog wirtualny konfiguracji lub certyfikatów Secure Sockets Layer (SSL) na serwerze docelowym. Aby uzyskać więcej informacji, zobacz [rozszerzeń łączy wdrażania w sieci Web](https://technet.microsoft.com/library/dd569028(WS.10).aspx).
- **— SetParamFile** parametr określa lokalizację *SetParameters.xml* pliku.
- **— AllowUntrusted** przełącznika wskazuje, że narzędzie Web Deploy należy akceptować certyfikaty SSL, które nie zostały wystawione przez zaufany urząd certyfikacji. Jeśli wdrażasz do obsługi wdrażania sieci Web, a certyfikat z podpisem własnym został użyty do bezpiecznego adresu URL usługi, należy dołączyć ten przełącznik.

## <a name="automating-web-package-deployment"></a>Automatyzowanie wdrażania pakietu sieci Web

W wielu scenariuszach dla przedsiębiorstw będzie można wdrożyć pakietów sieci web jako część większych pojedynczy krok lub automatycznego wdrażania. Niezależnie od tego, czy istnieje możliwość wdrażania pakietów usługi sieci web, uruchamiając *. pliku deploy.cmd* pliku lub bezpośrednio za pomocą MSDeploy.exe, można zdefiniować parametry poleceń i wywoływać je z obiektem docelowym aparatu Microsoft Build Engine (MSBuild) plik projektu.

W przykładowym rozwiązaniu Contact Manager, zapoznaj się z **PublishWebPackages** obiektów docelowych w systemie *Publish.proj* pliku. Ten element docelowy jest wykonywany jednokrotnie dla każdego *. pliku deploy.cmd* identyfikowane za pomocą listy elementów o nazwie pliku **PublishPackages**. Obiekt docelowy używa właściwości i metadanych elementu do utworzenia pełny zestaw wartości argumentu dla każdego *. pliku deploy.cmd* pliku, a następnie używa **Exec** zadania do uruchomienia polecenia.


[!code-xml[Main](deploying-web-packages/samples/sample8.xml)]


> [!NOTE]
> Omówienie szerszej model pliku projektu w przykładowym rozwiązaniu i zapoznać się z wprowadzeniem do projektu niestandardowych plików ogólnie rzecz biorąc, zobacz [objaśnienie pliku projektu](understanding-the-project-file.md) i [objaśnienie procesu kompilacji](understanding-the-build-process.md).


## <a name="endpoint-considerations"></a>Zagadnienia dotyczące punktu końcowego

Niezależnie od tego, czy wdrożyć pakiet usługi sieci web, uruchamiając *. pliku deploy.cmd* pliku lub bezpośrednio za pomocą MSDeploy.exe, należy określić nazwę komputera lub punkt końcowy usługi dla danego wdrożenia.

Jeśli serwer sieci web docelowy jest skonfigurowany do wdrożenia przy użyciu usługi zdalnego agenta narzędzia Web Deploy, można określić docelowy adres URL usługi jako lokalizacji docelowej.


[!code-console[Main](deploying-web-packages/samples/sample9.cmd)]


Alternatywnie można określić nazwę serwera, tylko jako lokalizacji docelowej i narzędzia Web Deploy wywnioskuje adres URL usługi agenta zdalnego.


[!code-console[Main](deploying-web-packages/samples/sample10.cmd)]


Jeśli serwer sieci web docelowy został skonfigurowany do wdrożenia przy użyciu obsługi wdrażania sieci Web, należy określić adres punktu końcowego usługi zarządzania siecią Web usług IIS (WMSvc) jako lokalizacji docelowej. Domyślnie ta ma postać:


[!code-console[Main](deploying-web-packages/samples/sample11.cmd)]


Można wskazać dowolne z tych punktów końcowych przy użyciu *. pliku deploy.cmd* pliku lub bezpośrednio MSDeploy.exe. Jednakże jeśli chcesz wdrożyć program obsługi wdrażania sieci Web jako użytkownik bez uprawnień administratora, zgodnie z opisem w [Konfigurowanie serwera sieci Web dla wdrożenia publikowania w sieci Web (Web wdrażanie obsługi)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md), należy dodać ciąg zapytania do adresu punktu końcowego usługi.


[!code-console[Main](deploying-web-packages/samples/sample12.cmd)]


Jest to spowodowane użytkownik niebędący administratorem nie ma dostępu na poziomie serwera w usługach IIS; użytkownik ma dostęp tylko do określonej witryny sieci Web usług IIS. W czasie pisania, ze względu na usterkę w sieci Web potok publikowania (WPP), nie można uruchomić *. pliku deploy.cmd* plików przy użyciu adresu punktu końcowego, który zawiera ciąg zapytania. W tym scenariuszu należy wdrożyć pakiet usługi sieci web bezpośrednio za pomocą MSDeploy.exe.

> [!NOTE]
> Aby uzyskać więcej informacji na temat usługi zdalnego agenta narzędzia Web Deploy i obsługi wdrażania sieci Web, zobacz [Wybieranie podejścia prawo do wdrażania w Internecie](../configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment.md). Aby uzyskać wskazówki dotyczące sposobu konfigurowania plików projektu specyficznymi dla środowiska do wdrożenia z tymi punktami końcowymi, zobacz [Konfigurowanie właściwości wdrożenia dla środowiska docelowego](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).


## <a name="authentication-considerations"></a>Zagadnienia dotyczące uwierzytelniania

Niezależnie od tego, czy wdrożyć pakiet usługi sieci web, uruchamiając *. pliku deploy.cmd* pliku lub bezpośrednio za pomocą MSDeploy.exe, należy określić typ uwierzytelniania. Narzędzie Web Deploy akceptuje dwóch wartości: **NTLM** lub **podstawowe**. Jeśli określisz opcję Uwierzytelnianie podstawowe, należy podać nazwę użytkownika i hasło. Istnieją różne czynniki, które musisz wiedzieć, kiedy wybierz typ uwierzytelniania:

- Jeśli wdrażasz usługi zdalnego agenta narzędzia Web Deploy, należy użyć uwierzytelniania NTLM. Usługa agenta zdalnego nie akceptuje podstawowe poświadczenia uwierzytelniania.
- Jeśli wdrażasz do obsługi wdrażania sieci Web, można użyć protokołu NTLM lub uwierzytelniania podstawowego. Ustawieniem domyślnym jest uwierzytelnianie podstawowe. Mimo że uwierzytelnianie podstawowe, zależy od nazwy użytkownika i hasła przesyłanych w postaci zwykłego tekstu, poświadczenia są chronione jako program obsługi wdrażania sieci Web zawsze używa szyfrowania protokołu SSL.
- Jeśli pakiet sieci web zawiera bazę danych, a serwer sieci web a serwerem bazy danych są osobne maszyny, nie będzie mógł wdrożyć bazę danych przy użyciu uwierzytelniania NTLM, ze względu na [ograniczenia uwierzytelniania NTLM "podwójny przeskok"](https://go.microsoft.com/?linkid=9805120). Musisz podać poświadczenia uwierzytelniania podstawowego do narzędzia Web Deploy lub Użyj poświadczeń programu SQL Server w ciągu połączenia wdrożenia. Ten problem jest opisany bardziej szczegółowo w [wdrażanie baz danych członkostwa w środowiskach przedsiębiorstw](../advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments.md).

## <a name="conclusion"></a>Wniosek

W tym temacie opisano sposób wdrażania pakietu sieci web albo uruchamiając *. pliku deploy.cmd* pliku lub bezpośrednio za pomocą MSDeploy.exe. Wyjaśniono jej, gdy każde podejście może być odpowiednie, a on opisany sposób zdefiniować parametry i uruchom polecenie wdrożenia jako część większego procesu kompilacji pojedynczy krok i automatycznych.

## <a name="further-reading"></a>Dalsze informacje

Aby uzyskać instrukcje dotyczące sposobu tworzenia i sparametryzuj pakiet wdrażania sieci web, zobacz [budowanie i projektów aplikacji sieci Web pakietu](building-and-packaging-web-application-projects.md) i [konfigurowania parametrów wdrożenia pakietu internetowego](configuring-parameters-for-web-package-deployment.md). Aby uzyskać wskazówki dotyczące sposobu tworzenia i wdrażania pakietów sieci web z wystąpienia programu Team Foundation Server (TFS), zobacz [Konfigurowanie serwera Team Foundation Server dla automatycznego wdrażania w Internecie](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). Aby uzyskać informacje na temat sposobu dostosowywania i rozwiązywanie problemów z procesu wdrażania, zobacz [wykluczanie plików i folderów z wdrożenia](../advanced-enterprise-web-deployment/excluding-files-and-folders-from-deployment.md).

> [!div class="step-by-step"]
> [Poprzednie](configuring-parameters-for-web-package-deployment.md)
> [dalej](deploying-database-projects.md)
