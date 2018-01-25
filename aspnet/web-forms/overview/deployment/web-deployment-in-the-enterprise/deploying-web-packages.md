---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-web-packages
title: "Wdrażanie pakietów sieci Web | Dokumentacja firmy Microsoft"
author: jrjlee
description: "W tym temacie opisano, jak opublikować pakiety wdrażania w sieci web na serwerze zdalnym za pomocą usług Internet Information Services (IIS) Narzędzie Web Deployment (Web..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: a5c5eed2-8683-40a5-a2e1-35c9f8d17c29
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-web-packages
msc.type: authoredcontent
ms.openlocfilehash: cd2bfa07262155b68ac4605fc7e9748d276d3193
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="deploying-web-packages"></a>Wdrażanie pakietów sieci Web
====================
przez [Lewandowski Jason](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> W tym temacie opisano, jak opublikować pakiety wdrażania w sieci web na serwerze zdalnym za pomocą narzędzia wdrażania Internet Information Services (IIS) w sieci Web (Web Deploy) 2.0.
> 
> Istnieją dwa sposoby, w których można wdrożyć pakiet sieci web na serwerze zdalnym:
> 
> - Można użyć narzędzia wiersza polecenia programu MSDeploy.exe bezpośrednio.
> - Można uruchomić *.deploy.cmd [Nazwa projektu]* plik, który generuje procesu kompilacji.
> 
> W rezultacie jest taka sama niezależnie od tego, które rozwiązanie używasz. Zasadniczo wszystkie *. pliku deploy.cmd* plik nie jest uruchomienie MSDeploy.exe z niektórych wartości ustalonej, dzięki czemu nie trzeba podać te informacje w celu wdrożenia pakietu. Upraszcza proces wdrażania. Z drugiej strony bezpośrednio za pomocą MSDeploy.exe zapewnia znacznie większą elastycznością za pośrednictwem dokładnie sposób wdrażania pakietu.
> 
> Rozwiązania używasz zależy od wielu czynników, w tym poziom kontroli wymagają nad procesem wdrażania i czy docelowych usługi sieci Web wdrażanie agenta zdalnego lub obsługa wdrażania w sieci Web. W tym temacie wyjaśniono, jak używać każdego z podejść i identyfikuje podczas każdego z podejść jest odpowiedni.
> 
> Zadania i wskazówki, w tym temacie założono, że:
> 
> - Został utworzony i spakowanej aplikacji sieci web, zgodnie z opisem w [budynku i projekty aplikacji sieci Web pakowania](building-and-packaging-web-application-projects.md).
> - Zmodyfikowano *SetParameters.xml* plik, aby podać wartości odpowiednich parametrów środowiska docelowego zgodnie z opisem w [konfigurowania parametrów wdrażania pakietu sieci Web](configuring-parameters-for-web-package-deployment.md).


Uruchomiona [*Nazwa projektu*]*. pliku deploy.cmd* plik jest najprostszym sposobem, aby wdrożyć pakiet sieci web. W szczególności za pomocą *. pliku deploy.cmd* plików oferuje tych zalet w porównaniu z bezpośrednio za pomocą MSDeploy.exe:

- Nie musisz określić lokalizację pakietu wdrożeniowego sieci web & #x 2014; *. pliku deploy.cmd* pliku zna już, gdzie jest.
- Nie musisz określić lokalizację *SetParameters.xml* plik & #x 2014; *. pliku deploy.cmd* pliku zna już, gdzie jest.
- Nie trzeba określić źródłowego i docelowego MSDeploy dostawców & #x 2014; *. pliku deploy.cmd* pliku zna już wartości, które mają być używane.
- Nie należy określić ustawienia działania MSDeploy & #x 2014; *. pliku deploy.cmd* file dodaje najczęściej wymagane wartości polecenie MSDeploy.exe automatycznie.

Przed użyciem *. pliku deploy.cmd* plik, aby wdrożyć pakiet sieci web, należy upewnić się, że:

- *. Pliku deploy.cmd* pliku [*Nazwa projektu*]. *SetParameters.xml* plików i pakietu sieci web ([*Nazwa projektu*]. *ZIP*) znajdują się w tym samym folderze.
- Narzędzie Web Deploy (MSDeploy.exe) jest zainstalowany na komputerze z uruchomionym programem *. pliku deploy.cmd* pliku.

*. Pliku deploy.cmd* plik obsługuje różne opcje wiersza polecenia. Po uruchomieniu pliku z wiersza polecenia jest to podstawowa składnia:


[!code-console[Main](deploying-web-packages/samples/sample1.cmd)]


Należy określić **/T** flagi lub **/Y** flagę, aby wskazać, czy chcesz wykonać odpowiednio próbnej lub wdrażania na żywo (nie używaj flagi zarówno w tym samym poleceniu). W następującej tabeli opisano przeznaczenie każdej z tych flag.

| Flaga | Opis |
| --- | --- |
| **/T** | Wywołuje MSDeploy.exe z **-whatif** flagę wskazującą, próbnej. Zamiast wdrażania pakietu, tworzy raport na temat tego, co się stanie, jeśli pakiet został wdrożony. |
| **/Y** | Wywołuje MSDeploy.exe bez **-whatif** flagi. To wdrożenie pakietu na komputerze lokalnym lub wybrany serwer docelowy. |
| **/M** | Wskazuje serwer docelowy nazwa lub adres URL usługi. Aby uzyskać więcej informacji o wartościach podanych tutaj zobacz **zagadnienia dotyczące punktu końcowego** w tym temacie. W przypadku pominięcia **/M** flagi, pakiet zostanie wdrożony na komputerze lokalnym. |
| **/A** | Określa typ uwierzytelniania, która powinna być używana do wykonywania wdrożenia MSDeploy.exe. Możliwe wartości to **NTLM** i **podstawowe**. W przypadku pominięcia **/A** flagę domyślnie typ uwierzytelniania **NTLM** wdrożenia do usługi sieci Web wdrażanie agenta zdalnego oraz na **podstawowe** wdrożenia do narzędzia Web Deploy Program obsługi. |
| **/U** | Określa nazwę użytkownika. Dotyczy to tylko wtedy, gdy używasz uwierzytelniania podstawowego. |
| **/P** | Określa hasło. Dotyczy to tylko wtedy, gdy używasz uwierzytelniania podstawowego. |
| **/L** | Wskazuje, czy powinny zostać wdrożone pakiet do lokalnego wystąpienia usług IIS Express. |
| **/G** | Określa, że pakiet jest wdrażany przy użyciu [ustawienie dostawcy tempAgent](https://technet.microsoft.com/library/ee517345(WS.10).aspx). W przypadku pominięcia **/G** flagi, domyślnie przyjmowana jest wartość **false**. |

> [!NOTE]
> Za każdym razem, gdy proces kompilacji tworzy pakiet sieci web, również tworzy plik o nazwie *[Nazwa projektu] .deploy-readme.txt* objaśniający te opcje wdrażania.


Oprócz tych flag, można określić ustawienia działania narzędzia Web Deploy jako dodatkowe *. pliku deploy.cmd* parametrów. Dodatkowe ustawienia, które określisz po prostu są przekazywane do podstawowej polecenie MSDeploy.exe. Aby uzyskać więcej informacji na temat tych ustawień, zobacz [ustawienia operację wdrażania w sieci Web](https://technet.microsoft.com/library/dd569089(WS.10).aspx).

Załóżmy, że chcesz wdrożyć projekt aplikacji sieci web ContactManager.Mvc do środowiska testowego, uruchamiając *. pliku deploy.cmd* pliku. Środowiska testowego jest skonfigurowany do korzystania z usługi sieci Web wdrażanie agenta zdalnego, zgodnie z opisem w [Konfiguracja serwera sieci Web dla wdrożenia publikowania w sieci Web (agenta zdalnego)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent.md). Aby wdrożyć aplikację sieci web, należy wykonać poniższe czynności.

**Aby wdrożyć aplikację sieci web przy użyciu. plikiem deploy.cmd pliku**

1. Tworzenie i pakiet projektu aplikacji sieci web, zgodnie z opisem w [budynku i projekty aplikacji sieci Web pakowania](building-and-packaging-web-application-projects.md).
2. Modyfikowanie *ContactManager.Mvc.SetParameters.xml* plik zawiera wartości odpowiedni parametr dla danego środowiska testowego zgodnie z opisem w [konfigurowania parametrów wdrażania pakietu sieci Web](configuring-parameters-for-web-package-deployment.md).
3. Otwórz okno wiersza polecenia i przejdź do lokalizacji *ContactManager.Mvc.deploy.cmd* pliku.
4. Wpisz następujące polecenie, a następnie naciśnij klawisz Enter:

    [!code-console[Main](deploying-web-packages/samples/sample2.cmd)]

W tym przykładzie:

- **/Y** flaga wskazuje, że chcesz rzeczywiście wdrożony pakiet, a nie podczas korzystania z wersji próbnej Uruchom.
- **/M** flaga wskazuje, że chcesz wdrożyć pakiet na serwerze o nazwie TESTWEB1. Od tej wartości MSDeploy.exe podejmie próbę wdrożyć pakiet z usługą sieci Web wdrażanie agenta zdalnego w http://TESTWEB1/MSDeployAgentService.
- **/A** flaga wskazuje, czy chcesz korzystać z uwierzytelniania NTLM. Tak nie trzeba określić nazwę użytkownika i hasło.

Aby zilustrować jak polecenie *. pliku deploy.cmd* pliku upraszcza proces wdrażania, Przyjrzyjmy się polecenie MSDeploy.exe, które pobiera wygenerowany i wykonywane po uruchomieniu *ContactManager.Mvc.deploy.cmd* przy użyciu opcji wymienionych powyżej.


[!code-console[Main](deploying-web-packages/samples/sample3.cmd)]


Aby uzyskać więcej informacji na temat używania *. pliku deploy.cmd* plik, aby wdrożyć pakiet sieci web, zobacz [porady: Zainstaluj wdrażania pakietu przy użyciu pliku deploy.cmd pliku](https://msdn.microsoft.com/library/ff356104.aspx).

## <a name="using-msdeployexe"></a>Przy użyciu MSDeploy.exe

Mimo że używanie *. pliku deploy.cmd* pliku zazwyczaj upraszcza proces wdrażania, istnieje kilka sytuacji, gdy nie jest bezpośrednio użyć MSDeploy.exe. Na przykład:

- Jeśli chcesz wdrożyć do obsługi wdrażania sieci Web jako użytkownik bez uprawnień administratora, nie można użyć *. pliku deploy.cmd* pliku. Jest to spowodowane błędem w narzędziu Web Deploy 2.0, zgodnie z opisem w **zagadnienia dotyczące punktu końcowego**.
- Jeśli chcesz ręcznie przełączać się między różnymi *SetParameters.xml* plików w różnych lokalizacjach, użytkownik może chcieć użyć MSDeploy.exe bezpośrednio.
- Jeśli chcesz przesłonić kilka MSDeploy.exe argumenty wiersza polecenia, można użyć bezpośrednio MSDeploy.exe.

Gdy używasz MSDeploy.exe, należy podać trzy kluczowe informacje:

- A **— źródło** parametrem, który wskazuje, gdzie pochodzi z danych.
- A **— dest** parametrem, który wskazuje, gdzie ma danych.
- A **— zlecenie** parametrem, który wskazuje [operacji](https://technet.microsoft.com/library/dd568989(WS.10).aspx) chcesz wykonać.

Zależy od MSDeploy.exe [narzędzia Web Deploy dostawców](https://technet.microsoft.com/library/dd569040(WS.10).aspx) do przetwarzania danych źródłowych i docelowych. Narzędzie Web Deploy obejmuje wiele dostawców reprezentujących zakres źródeł danych i aplikacji może współpracować z & #x 2014; na przykład istnieją dostawców dla baz danych programu SQL Server, serwery sieci web usług IIS, certyfikaty, zestawów (GAC) w pamięci podręcznej GAC, różnych pliki konfiguracji różnych i wiele innych typów danych. Zarówno **— źródło** parametru i **— dest** parametr musi określać dostawcy, w formularzu **— źródło**: [*providerName*] = [*lokalizacji*]. Podczas wdrażania pakietu sieci web do witryny sieci Web usług IIS, należy użyć tych wartości:

- **— Źródło** dostawcy jest zawsze [pakietu](https://technet.microsoft.com/library/dd569019(WS.10).aspx). Na przykład:

    [!code-console[Main](deploying-web-packages/samples/sample4.cmd)]
- **— Dest** dostawcy jest zawsze [automatycznie](https://technet.microsoft.com/library/dd569016(WS.10).aspx). Na przykład:

    [!code-console[Main](deploying-web-packages/samples/sample5.cmd)]
- **— Zlecenie** jest zawsze **synchronizacji**.

    [!code-console[Main](deploying-web-packages/samples/sample6.cmd)]

Ponadto należy określić różne inne [ustawień specyficznych dla dostawcy](https://technet.microsoft.com/library/dd569001(WS.10).aspx) i ogólne [ustawienia działania](https://technet.microsoft.com/library/dd569089(WS.10).aspx). Na przykład załóżmy, że chcesz wdrożyć aplikację sieci web ContactManager.Mvc w środowisku przemieszczania. Wdrożenia będzie obowiązywać obsługi wdrażania sieci Web i użyć uwierzytelniania podstawowego. Aby wdrożyć aplikację sieci web, należy wykonać poniższe czynności.

**Aby wdrożyć aplikację sieci web przy użyciu MSDeploy.exe**

1. Tworzenie i pakiet projektu aplikacji sieci web, zgodnie z opisem w [budynku i projekty aplikacji sieci Web pakowania](building-and-packaging-web-application-projects.md).
2. Modyfikowanie *ContactManager.Mvc.SetParameters.xml* plik zawiera wartości odpowiedni parametr dla danego środowiska przemieszczania, zgodnie z opisem w [konfigurowania parametrów wdrażania pakietu sieci Web](configuring-parameters-for-web-package-deployment.md).
3. Otwórz okno wiersza polecenia i przejdź do lokalizacji MSDeploy.exe. Jest to zazwyczaj na %PROGRAMFILES%\IIS\Microsoft V2\msdeploy.exe wdrażania w sieci Web.
4. Wpisz następujące polecenie i naciśnij klawisz Enter (pominąć podziały):

    [!code-console[Main](deploying-web-packages/samples/sample7.cmd)]

W tym przykładzie:

- **— Źródło** określa parametr **pakietu** dostawcy i wskazuje lokalizację pakietu sieci web.
- **— Dest** określa parametr **automatycznie** dostawcy. **ComputerName** ustawienie zawiera adres URL usługi sieci Web obsługi wdrażania na serwerze docelowym. **Typ** ustawienie wskazuje, że chcesz użyć uwierzytelniania podstawowego i jako taki musisz podać **username** i **hasło**. Na koniec **includeAcls = "False"** ustawienie wskazuje, że nie chcesz kopiować listy kontroli dostępu (ACL) plików w aplikacji sieci web źródłowego na serwer docelowy.
- **— Zlecenie: synchronizacja** argument wskazuje, że chcesz replikować zawartość źródłową na serwerze docelowym.
- **— DisableLink** argumenty wskazują, że nie chcesz replikować pule aplikacji, katalogu wirtualnego konfiguracji lub certyfikatów Secure Sockets Layer (SSL) na serwerze docelowym. Aby uzyskać więcej informacji, zobacz [rozszerzeń łączy wdrażania w sieci Web](https://technet.microsoft.com/library/dd569028(WS.10).aspx).
- **— SetParamFile** parametru zapewnia lokalizację *SetParameters.xml* pliku.
- **— AllowUntrusted** przełącznika wskazuje, że narzędzia Web Deploy należy zaakceptować certyfikatów SSL, które nie zostały wystawione przez zaufany urząd certyfikacji. Jeśli wdrażasz do obsługi wdrażania sieci Web i certyfikatu z podpisem własnym był używany do zabezpieczania adres URL usługi, należy dołączyć ten przełącznik.

## <a name="automating-web-package-deployment"></a>Automatyzacja wdrażania pakietu sieci Web

W wielu scenariuszach dla przedsiębiorstwa należy wdrożyć w ramach większych pojedynczy krok lub zautomatyzowanego wdrażania pakietów sieci web. Niezależnie od tego, czy użytkownik chce wdrożyć pakietów sieci web, uruchamiając *. pliku deploy.cmd* plików lub przy użyciu MSDeploy.exe bezpośrednio, parametryzacja poleceń i skontaktuj się z obiektem docelowym aparat kompilacji firmy Microsoft (MSBuild) plik projektu.

W rozwiązaniu próbki menedżera kontaktu, Przyjrzyjmy się **PublishWebPackages** obiektów docelowych w *Publish.proj* pliku. Ten element docelowy jest uruchamiane jeden raz dla każdego *. pliku deploy.cmd* identyfikowana na podstawie listy elementów o nazwie pliku **PublishPackages**. Obiekt docelowy używa właściwości i elementu metadanych do zbudowania pełny zestaw wartości argumentu dla każdego *. pliku deploy.cmd* pliku, a następnie używa **Exec** zadań, aby uruchomić to polecenie.


[!code-xml[Main](deploying-web-packages/samples/sample8.xml)]


> [!NOTE]
> Szersze omówienie modelu pliku projektu w przykładowe rozwiązanie i wprowadzenie do plików projektów niestandardowych w ogólności, zobacz [opis pliku projektu](understanding-the-project-file.md) i [opis procesu kompilacji](understanding-the-build-process.md).


## <a name="endpoint-considerations"></a>Zagadnienia dotyczące punktu końcowego

Niezależnie od tego, czy wdrażania pakietu sieci web, uruchamiając *. pliku deploy.cmd* plików lub przy użyciu MSDeploy.exe bezpośrednio, należy określić nazwę komputera lub punkt końcowy usługi dla danego wdrożenia.

Jeśli serwer sieci web docelowy został skonfigurowany dla wdrożenia przy użyciu usługi sieci Web wdrażanie agenta zdalnego, należy określić docelowy adres URL usługi jako lokalizacja docelowa.


[!code-console[Main](deploying-web-packages/samples/sample9.cmd)]


Alternatywnie można określić tylko nazwa serwera jako lokalizacja docelowa i Web Deploy wywnioskuje adres URL usługi agenta zdalnego.


[!code-console[Main](deploying-web-packages/samples/sample10.cmd)]


Jeśli serwer sieci web docelowy został skonfigurowany dla wdrożenia przy użyciu procedury obsługi wdrażania w sieci Web, należy określić adres punktu końcowego usługi IIS sieci Web zarządzania (WMSvc) jako lokalizacja docelowa. Domyślnie to ma postać:


[!code-console[Main](deploying-web-packages/samples/sample11.cmd)]


Możesz zastosować dowolną z tych punktów końcowych przy użyciu *. pliku deploy.cmd* pliku lub bezpośrednio MSDeploy.exe. Jednak jeśli chcesz wdrożyć program obsługi wdrażania sieci Web jako użytkownik bez uprawnień administratora, zgodnie z opisem w [Konfiguracja serwera sieci Web dla wdrożenia publikowania w sieci Web (Obsługa wdrażania w sieci Web)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md), należy dodać ciąg zapytania do adresu punktu końcowego usługi.


[!code-console[Main](deploying-web-packages/samples/sample12.cmd)]


To dlatego użytkownik bez uprawnień administratora nie ma dostępu na poziomie serwera w usługach IIS; użytkownik ma dostęp tylko do określonej witryny sieci Web usług IIS. W tym czasie zapisu z powodu błędu w sieci Web potok publikowania (WPP), nie można uruchomić *. pliku deploy.cmd* plików przy użyciu adresu punktu końcowego, który zawiera ciąg zapytania. W tym scenariuszu należy wdrożyć pakiet sieci web przy użyciu MSDeploy.exe bezpośrednio.

> [!NOTE]
> Aby uzyskać więcej informacji dotyczących usługi sieci Web wdrażanie agenta zdalnego i program obsługi wdrażania sieci Web, zobacz [Wybieranie podejście prawo do wdrożenia w sieci Web](../configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment.md). Aby uzyskać wskazówki dotyczące sposobu konfigurowania plików projektu określonego środowiska do wdrożenia z tymi punktami końcowymi, zobacz [konfigurowania właściwości wdrożenia dla środowiska docelowego](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).


## <a name="authentication-considerations"></a>Zagadnienia dotyczące uwierzytelniania

Niezależnie od tego, czy wdrażania pakietu sieci web, uruchamiając *. pliku deploy.cmd* plików lub przy użyciu MSDeploy.exe bezpośrednio, należy określić typ uwierzytelniania. Narzędzie Web Deploy akceptuje dwa możliwe wartości: **NTLM** lub **podstawowe**. Jeśli określisz uwierzytelnianie podstawowe, należy podać nazwę użytkownika i hasło. Istnieją różne czynniki, które należy znać po wybraniu typu uwierzytelniania:

- W przypadku instalowania usługi sieci Web wdrażanie agenta zdalnego, należy użyć uwierzytelniania NTLM. Usługa agenta zdalnego nie akceptuje podstawowe poświadczenia uwierzytelniania.
- Jeśli wdrażasz do obsługi wdrażania w sieci Web, można użyć protokołu NTLM lub uwierzytelniania podstawowego. Ustawieniem domyślnym jest uwierzytelnianie podstawowe. Mimo że podstawowe uwierzytelnianie opiera się na nazwy użytkownika i hasła przesyłanych w postaci zwykłego tekstu, poświadczenia są chronione jako program obsługi wdrażania sieci Web zawsze używa szyfrowania SSL.
- Jeśli pakiet sieci web zawiera bazę danych, a serwer sieci web i serwera bazy danych są osobne maszyny, nie będzie mógł wdrożyć bazę danych przy użyciu uwierzytelniania NTLM, ze względu na [ograniczenia uwierzytelniania NTLM "podwójnym przeskokiem"](https://go.microsoft.com/?linkid=9805120). Należy podać poświadczenia uwierzytelniania podstawowego do narzędzia Web Deploy albo użyj poświadczeń programu SQL Server w ciągu połączenia wdrożenia. Ten problem jest opisany bardziej szczegółowo w [wdrażanie baz danych członkostwa w środowiskach przedsiębiorstw](../advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments.md).

## <a name="conclusion"></a>Wniosek

W tym temacie opisano sposób wdrażania pakietu sieci web albo uruchamiając *. pliku deploy.cmd* plików lub przy użyciu MSDeploy.exe bezpośrednio. Wyjaśniono go, gdy każde podejście może być odpowiednie, a on opisany sposób parametryzacja i uruchom polecenie wdrożenia jako część większego procesu kompilacji pojedynczy krok lub automatyczne.

## <a name="further-reading"></a>Dalsze informacje

Aby uzyskać wskazówki dotyczące sposobu tworzenia i parametryzacja pakietu wdrożeniowego sieci web, zobacz [budynku i projekty aplikacji sieci Web pakowania](building-and-packaging-web-application-projects.md) i [konfigurowania parametrów wdrażania pakietu sieci Web](configuring-parameters-for-web-package-deployment.md). Aby uzyskać wskazówki dotyczące sposobu tworzenia i wdrażania pakietów sieci web z wystąpienia programu Team Foundation Server (TFS), zobacz [Konfigurowanie Team Foundation Server dla automatycznego wdrażania Web](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). Aby uzyskać informacje na temat sposobu dostosowywania i rozwiązywanie problemów z procesu wdrażania, zobacz [z wyjątkiem plików i folderów z wdrożenia](../advanced-enterprise-web-deployment/excluding-files-and-folders-from-deployment.md).

>[!div class="step-by-step"]
[Poprzednie](configuring-parameters-for-web-package-deployment.md)
[dalej](deploying-database-projects.md)
