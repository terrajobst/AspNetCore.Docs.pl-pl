---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment
title: Wybieranie właściwego podejścia do wdrażania w Internecie | Dokumentacja firmy Microsoft
author: jrjlee
description: Podczas pracy z Internet Information Services (IIS) Narzędzie Web Deployment (Web Deploy) 2.0 lub nowszej, istnieją trzy główne metody można użyć, aby uzyskać...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 787a53fd-9901-4a11-9d58-61e0509cda45
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: f1c3a09d9e960a53a25e0dd0b953224204a9ba7b
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37367748"
---
<a name="choosing-the-right-approach-to-web-deployment"></a>Wybieranie właściwego podejścia do wdrażania w Internecie
====================
przez [Jason Lee](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Podczas pracy z Internet Information Services (IIS) Narzędzie Web Deployment (Web Deploy) 2.0 lub nowszej, istnieją trzy główne metody umożliwia pobieranie aplikacji sieci web spakowane w taki sposób, na serwerze sieci web. Można:
> 
> - Wdrażanie aplikacji z lokalizacji zdalnej, określając jako docelowe *Usługa agenta wdrażania sieci Web* (znany także jako "agent zdalny") na serwerze docelowym.
> - Wdrażanie aplikacji z lokalizacji zdalnej za pomocą sieci Web wdrażanie na żądanie (znany także jako "temp agent").
> - Wdrażanie aplikacji z lokalizacji zdalnej, określając jako docelowe *obsługi wdrażania sieci Web usług IIS* na serwerze docelowym.
> - Wdróż aplikację ręcznie kopiując pakiet sieci web na serwerze docelowym, i zaimportowaniu ich za pomocą Menedżera usług IIS.
> 
> Sposób konfigurowania serwerów sieci web docelowego będzie zależeć od których podejścia do wdrożenia, którego chcesz użyć. W tym temacie pomogą w podjęciu decyzji, które podejścia do wdrażania jest odpowiedni dla Ciebie.


W poniższej tabeli przedstawiono główne zalety i wady każdej metody wdrożenia wraz z scenariusze, które najczęściej własnych każde podejście.

| Podejście | Zalety | Wady | Typowe scenariusze |
| --- | --- | --- | --- |
| Zdalny Agent | Jest łatwy w konfiguracji. Nadaje się do regularnych aktualizacji zawartości i aplikacji sieci web. | Użytkownik musi być administratorem na serwerze docelowym. użytkownik nie może podać alternatywne poświadczenia. | Środowisk deweloperskich. Środowiska testowe. |
| Agent tymczasowego | Nie ma potrzeby instalowania narzędzia Web Deploy na komputerze docelowym. Najnowszą wersję narzędzia Web Deploy automatycznie jest używany. | Użytkownik musi być administratorem na serwerze docelowym. Użytkownik nie może podać alternatywne poświadczenia. | Środowisk deweloperskich. Środowiska testowe. |
| Program obsługi narzędzia Web Deploy | Użytkownicy niebędący administratorami mogą wdrożyć zawartość. Nadaje się do regularnych aktualizacji zawartości i aplikacji sieci web. | Jest znacznie bardziej złożone, aby skonfigurować. | Środowiska przejściowe. Intranet środowisk produkcyjnych. Obsługiwanych środowiskach. |
| Wdrożenie w trybie offline | Jest bardzo łatwe do skonfigurowania. Nadaje się do środowiska izolowanego. | Administrator serwera, należy ręcznie skopiować i zaimportować pakiet za każdym razem, gdy. | Połączone z Internetem środowisk produkcyjnych. Środowiska izolowane sieci. |
  

## <a name="using-the-remote-agent"></a>Za pomocą agenta zdalnego

Po zainstalowaniu narzędzia Web Deploy na serwerze docelowym przy użyciu ustawień domyślnych, Usługa agenta wdrażania sieci Web ("agent zdalny") jest zainstalowany i automatycznie pracę. Domyślnie zdalny agent uwidacznia punkt końcowy HTTP pod tym adresem:


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample1.cmd)]


> [!NOTE]
> Możesz zastąpić [*serwera*] nazwą komputera serwera sieci web, adres IP serwera sieci web lub nazwy hosta, jest rozpoznawana jako serwer sieci web.


Administratorzy serwera można wdrożyć pakietów sieci web z lokalizacji zdalnej, np. maszyny dewelopera lub serwer kompilacji, określając adres tego punktu końcowego. Na przykład załóżmy, że Matt Hink w firmie Fabrikam, Inc. opracowała projekt aplikacji sieci web ContactManager.Mvc na jego komputerze dewelopera. Proces kompilacji generuje pakiet sieci web, łącznie z *. pliku deploy.cmd* pliku, który zawiera polecenia, narzędzie Web Deploy wymagany do zainstalowania pakietu. Jeśli Matt jest kontem administratora na serwerze, na serwerze TESTWEB1, umożliwia wdrożenie aplikacji sieci web na serwerze sieci web test, uruchamiając poniższe polecenie na jego komputerze dewelopera:


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample2.cmd)]


W rzeczywistości plik wykonywalny narzędzia Web Deploy może wywnioskować adres punktu końcowego zdalnego agenta, jeśli podasz nazwę komputera, więc Matt wystarczy wpisać to:


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample3.cmd)]


> [!NOTE]
> Aby uzyskać więcej informacji na temat składni wiersza polecenia narzędzia Web Deploy i *. pliku deploy.cmd* plików, zobacz [porady: Instalowanie wdrażania pakietu przy użyciu pliku deploy.cmd pliku](https://msdn.microsoft.com/library/ff356104.aspx).


Agent zdalny zapewnia prosty sposób wdrożyć zawartość z lokalizacji zdalnej, a takie podejście może pracować również wdrażanie jednym kliknięciem i automatycznych. Jednak użytkownik uruchamiający polecenie wdrożenia musi być również administratora domeny lub członkiem lokalnej grupy administratorów na serwerze docelowym. Ponadto agent zdalny nie obsługuje uwierzytelnianie podstawowe, dlatego nie można przekazać alternatywne poświadczenia w wierszu polecenia.

Agent zdalnego udostępnia przydatne do wdrożenia w rozwoju lub scenariuszy testowych, w którym nie jest niczym niezwykłym deweloperom kontrolują administrator o pełnych uprawnieniach w środowisku testowym serwera i zazwyczaj odbudować i bardzo ponownego wdrażania aplikacji często. Jednak to podejście jest zazwyczaj mniej dopuszczalny dla środowisk przejściowych lub produkcyjnych.

Aby uzyskać przykład end-to-end scenariusza, który korzysta z metody agenta zdalnego, zobacz [scenariusz: Konfigurowanie środowiska testowego na potrzeby wdrażania w Internecie](scenario-configuring-a-test-environment-for-web-deployment.md).

## <a name="using-the-temp-agent"></a>Przy użyciu tymczasowego agenta

Podejście tymczasowego agenta do wdrożenia jest podobne do metody agenta zdalnego. Jednak w przeciwieństwie do podejście agenta zdalnego nie trzeba zainstalować narzędzie Web Deploy na serwerze docelowym. Zamiast tego podczas wykonywania wdrożenia narzędzia Web Deploy zainstaluje tymczasowej wersji Usługa agenta wdrażania sieci web na serwerze docelowym i będzie on używany do wdrażania zawartości w usługach IIS. Po zakończeniu wdrożenia zostaną usunięte wszystkie pliki tymczasowe.

Jeśli chcesz użyć ustawienie dostawcy tymczasowego agenta, należy dodać **/g** flagi do polecenia wdrożenia:


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample4.cmd)]


> [!NOTE]
> Nie można użyć tymczasowego agenta. Jeśli usługa agenta wdrażania sieci web jest zainstalowana na komputerze docelowym, nawet wtedy, gdy usługa nie jest uruchomiona.


Zaletą tego podejścia jest to, że nie jest wymagane do obsługi instalacji programu Web Deploy na serwerach docelowych z usługi. Ponadto nie musisz upewnić się, że komputery źródłowy i docelowy są uruchomione na tę samą wersję narzędzia Web Deploy. Jednak to podejście odczuwa te same ograniczenia podmiotu zabezpieczeń jako podejście agenta zdalnego, to znaczy, musi być kontem lokalnego administratora na serwerze docelowym w celu wdrażania zawartości i obsługiwane jest tylko uwierzytelnianie NTLM. Podejście tymczasowego agenta wymaga również znacznie większej liczby czynności konfiguracyjnych środowiska docelowego.

Aby uzyskać więcej informacji na temat korzystania z tymczasowego agenta, zobacz [jak: Instalowanie wdrażania pakietu przy użyciu pliku deploy.cmd pliku](https://msdn.microsoft.com/library/ff356104.aspx) i [sieci Web wdrażanie na żądanie](https://technet.microsoft.com/library/ee517345(WS.10).aspx).

## <a name="using-the-web-deploy-handler"></a>Korzystanie z sieci Web wdrażanie obsługi

Dla serwera IIS 7 lub nowszy, narzędzie Web Deploy oferuje podejście alternatywne wdrożenia do obsługi wdrażania w sieci Web usług IIS. Program obsługi wdrażania sieci Web jest ściśle zintegrowana z usługą zarządzania sieci Web usług IIS (WMSvc), który został zaprojektowany, aby umożliwić użytkownikom zarządzanie witryn sieci Web usług IIS z lokalizacji zdalnych.

Domyślnie zdalny agent uwidacznia punkt końcowy HTTP pod tym adresem:


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample5.cmd)]


> [!NOTE]
> Możesz zastąpić [*serwera*] nazwą komputera serwera sieci web, adres IP serwera sieci web lub nazwy hosta, jest rozpoznawana jako serwer sieci web.


Dużą zaletą programu obsługi wdrażania sieci Web za pośrednictwem agenta zdalnego i tymczasowego agenta jest skonfigurowanie usług IIS, aby umożliwić użytkownikom niebędącym administratorami na wdrażanie aplikacji i zawartości do określonych witryn internetowych usług IIS. Program obsługi wdrażania sieci Web obsługuje również uwierzytelnianie podstawowe, dzięki czemu możesz podać alternatywne poświadczenia jako parametry w poleceniach narzędzia Web Deploy. Główną wadą jest początkowo znacznie trudniejsze do instalowania i konfigurowania obsługi wdrażania sieci Web.

W przypadku użytkowników niebędących administratorami usługi zarządzania siecią Web (WMSvc) zezwala tylko użytkownik nawiązał połączenie usług IIS przy użyciu połączenia na poziomie witryny, a nie połączenie poziomu serwera. Aby uzyskać dostęp do określonej lokacji, może zawierać ciągu zapytania specyficzne dla lokacji adres punktu końcowego:


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample6.cmd)]


Na przykład załóżmy, że proces kompilacji jest skonfigurowana do automatycznego wdrożenia aplikacji sieci web w środowisku przejściowym po każdej udanej kompilacji. Jeśli używasz podejścia agenta zdalnego, będziesz potrzebować upewnij administratora na serwerach docelowych z Twojej tożsamości procesu kompilacji. Z kolei przy użyciu podejścia program obsługi wdrażania w sieci Web można nadać użytkownik niebędący administratorem&#x2014;**FABRIKAM\stagingdeployer** w tym przypadku&#x2014;zapewniają te uprawnienia do określonych usług IIS witryna sieci Web tylko i procesem kompilacji poświadczenia, aby wdrożyć pakiet sieci web.


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample7.cmd)]


> [!NOTE]
> Aby uzyskać więcej informacji na temat narzędzia Web Deploy operacji wiersza polecenia i składnię, zobacz [sieci Web wdrażanie Command Line Reference](https://technet.microsoft.com/library/dd568991(v=ws.10).aspx). Aby uzyskać więcej informacji na temat korzystania z *. pliku deploy.cmd* plików, zobacz [porady: Instalowanie wdrażania pakietu przy użyciu pliku deploy.cmd pliku](https://msdn.microsoft.com/library/ff356104.aspx).


Program obsługi wdrażania sieci Web udostępnia przydatne wdrożenie przejściowe środowisk, obsługiwanych środowiskach i opartych na sieci intranet środowisk produkcyjnych, gdzie dostęp zdalny do serwera jest dostępne, ale nie są poświadczenia administratora.

Na przykład end-to-end scenariusza, który korzysta z metody obsługi wdrażania sieci Web, zobacz [scenariusz: Konfigurowanie środowiska przejściowego na potrzeby wdrażania w Internecie](scenario-configuring-a-staging-environment-for-web-deployment.md).

## <a name="using-offline-deployment"></a>Przy użyciu wdrożenia w trybie Offline

W niektórych przypadkach nie jest możliwe lub praktyczne wdrożyć aplikacje i zawartość w witrynie sieci Web usług IIS z lokalizacji zdalnej. Na przykład komputery źródłowy i docelowy mogą znajdować się w sieci izolowanej lub segmentów sieci lub zasad zapory nie może zezwolić na zdalny dostęp.

W takich przypadkach możesz nadal używać pakowania i publikowania Web Deploy; Możesz po prostu nie można ich używać z lokalizacji zdalnej. Zamiast tego administrator na serwerze docelowym należy skopiować pakiet sieci web na serwerze i zaimportuj go za pomocą Menedżera usług IIS.

![](choosing-the-right-approach-to-web-deployment/_static/image1.png)

Metody wdrożenia w trybie offline jest zazwyczaj przydatne w środowiskach produkcyjnych dostępnego z Internetu, w której serwery w sieci obwodowej mogą mieć ograniczony łączności z komputerami w sieci wewnętrznej.

Aby uzyskać przykład end-to-end scenariusza, który używa metody wdrożenia w trybie offline, zobacz [scenariusz: Konfigurowanie środowiska produkcyjnego na potrzeby wdrażania w Internecie](scenario-configuring-a-production-environment-for-web-deployment.md).

## <a name="further-reading"></a>Dalsze informacje

Aby uzyskać więcej informacji na temat narzędzia Web Deploy operacji wiersza polecenia i składnię, zobacz [sieci Web wdrażanie Command Line Reference](https://technet.microsoft.com/library/dd568991(v=ws.10).aspx). Aby uzyskać więcej informacji na temat korzystania z *. pliku deploy.cmd* plików, zobacz [porady: Instalowanie wdrażania pakietu przy użyciu pliku deploy.cmd pliku](https://msdn.microsoft.com/library/ff356104.aspx).

Aby uzyskać bardziej ogólne wskazówki na różne sposoby, w którym można wdrożyć w sieci web pakiety z komputera zdalnego, zobacz [za pomocą sieci Web wdrażanie zdalnie](https://technet.microsoft.com/library/ee461175(WS.10).aspx). Aby uzyskać więcej informacji na temat korzystania z sieci Web wdrażanie na żądanie, zobacz [sieci Web wdrażanie na żądanie](https://technet.microsoft.com/library/ee517345(WS.10).aspx).

> [!div class="step-by-step"]
> [Poprzednie](configuring-server-environments-for-web-deployment.md)
> [dalej](scenario-configuring-a-test-environment-for-web-deployment.md)
