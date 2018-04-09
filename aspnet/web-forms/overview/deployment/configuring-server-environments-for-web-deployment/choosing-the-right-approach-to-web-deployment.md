---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment
title: Wybieranie podejście do wdrożenia sieci Web | Dokumentacja firmy Microsoft
author: jrjlee
description: Podczas pracy z usług Internet Information Services (IIS) Narzędzie Web Deployment (Web Deploy) 2.0 lub nowszej, istnieją trzy główne metody można użyć do pobrania...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 787a53fd-9901-4a11-9d58-61e0509cda45
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 2d690744687af93a69743dc6ce6c853629f61f5d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="choosing-the-right-approach-to-web-deployment"></a>Wybieranie podejście do wdrożenia sieci Web
====================
przez [Lewandowski Jason](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Podczas pracy z usług Internet Information Services (IIS) Narzędzie Web Deployment (Web Deploy) 2.0 lub nowszej, istnieją trzy główne metody można użyć do pobrania aplikacji sieci web spakowanych na serwerze sieci web. Można:
> 
> - Wdrażanie aplikacji z lokalizacji zdalnej, wybierając *Usługa agenta sieci Web wdrożenia* (znanej także jako "zdalnego agenta") na serwerze docelowym.
> - Wdrożenie aplikacji z lokalizacji zdalnej przy użyciu sieci Web wdrażanie na żądanie (znanej także jako "tymczasowego agenta").
> - Wdrażanie aplikacji z lokalizacji zdalnej, wybierając *obsługi wdrażania sieci Web usług IIS* na serwerze docelowym.
> - Wdróż aplikację, ręcznie kopiując pakietu sieci web na serwerze docelowym i importowania go za pomocą Menedżera usług IIS.
> 
> Sposób konfigurowania serwerów sieci web docelowym będzie zależeć od rozwiązania do wdrożenia, którego chcesz użyć. W tym temacie mogą pomóc zdecydować, które rozwiązanie do wdrożenia jest odpowiednie dla Ciebie.


W poniższej tabeli zamieszczono główne zalety i wady każdej metody wdrożenia, oraz scenariusze, które najczęściej własnych każdego z podejść.

| Podejście | Zalety | Wady | Typowe scenariusze |
| --- | --- | --- | --- |
| Agent zdalnego | Jest łatwy w konfiguracji. Nadaje się do regularnych aktualizacji aplikacji sieci web i zawartości. | Użytkownik musi być administratorem na serwerze docelowym. Użytkownik nie może podać alternatywne poświadczenia. | Środowisk deweloperskich. Przetestuj środowisk. |
| Tymczasowego agenta | Nie istnieje potrzeba do zainstalowania narzędzia Web Deploy na komputerze docelowym. Automatycznie jest używana najnowsza wersja narzędzia Web Deploy. | Użytkownik musi być administratorem na serwerze docelowym. Użytkownik nie może podać alternatywne poświadczenia. | Środowisk deweloperskich. Przetestuj środowisk. |
| Obsługa narzędzia Web Deploy | Użytkownicy niebędący administratorami można wdrożyć zawartość. Nadaje się do regularnych aktualizacji aplikacji sieci web i zawartości. | Jest znacznie bardziej złożone, aby skonfigurować. | Środowiska przejściowe. Intranet środowisk produkcyjnych. Środowiskach hostowanych. |
| Wdrożenia w trybie offline | Jest bardzo łatwo skonfigurować. Jest ona odpowiednia dla środowiska izolowanego. | Administrator serwera należy ręcznie skopiować i zaimportować pakiet zawsze. | Internetowy środowisk produkcyjnych. W środowiskach sieci izolowanej. |
  

## <a name="using-the-remote-agent"></a>Za pomocą zdalnego agenta

Po zainstalowaniu narzędzia Web Deploy przy użyciu domyślnych ustawień na serwerze docelowym, Usługa agenta wdrażania w sieci Web ("agent zdalnego") automatycznie zainstalowano i uruchomiono. Domyślnie agenta zdalnego udostępnia punkt końcowy HTTP pod tym adresem:


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample1.cmd)]


> [!NOTE]
> Można zastąpić [*serwera*] nazwą komputera serwera sieci web, adres IP serwera sieci web lub nazwy hosta która rozwiązuje problem z serwerem sieci web.


Administratorzy serwera można wdrożyć pakietów sieci web z lokalizacji zdalnej, takich jak komputerze dewelopera lub serwer kompilacji, określając adres tego punktu końcowego. Na przykład załóżmy, że Matt Hink w firmie Fabrikam, Inc. są wbudowane ContactManager.Mvc projektu aplikacji sieci web na jego komputerze dewelopera. Proces kompilacji generuje pakietu sieci web, wraz z *. pliku deploy.cmd* pliku, który zawiera polecenia narzędzia Web Deploy wymagane do zainstalowania pakietu. Jeśli Matt jest administratorem serwera TESTWEB1, uruchamiając poniższe polecenie na jego komputerze dewelopera umożliwia wdrożenie aplikacji sieci web na serwerze sieci web testów:


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample2.cmd)]


Faktycznie pliku wykonywalnego narzędzia Web Deploy można wywnioskować adres punktu końcowego zdalnego agenta, jeśli podasz nazwę komputera, więc Matt musi tylko wpisz to:


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample3.cmd)]


> [!NOTE]
> Aby uzyskać więcej informacji na temat składni wiersza polecenia narzędzia Web Deploy i *. pliku deploy.cmd* plików, zobacz [porady: Instalowanie wdrożenia pakietu za pomocą pliku deploy.cmd pliku](https://msdn.microsoft.com/library/ff356104.aspx).


Agenta zdalnego oferuje łatwe wdrażanie zawartości z lokalizacji zdalnej, a takie podejście może współpracować z jednym kliknięciem i automatycznego wdrażania. Jednak użytkownik, który uruchamia polecenie wdrożenia również musi być administratorem domeny lub członek lokalnej grupy administratorów na serwerze docelowym. Ponadto agenta zdalnego nie obsługuje uwierzytelnianie podstawowe, więc nie można przekazać alternatywne poświadczenia w wierszu polecenia.

Agent zdalnego udostępnia przydatne podejście do wdrożenia w rozwoju lub scenariusze testowania, w którym nie jest rzadko deweloperom kontrolę administrator o pełnych uprawnieniach w środowisku testowym serwera, i aplikacje są zazwyczaj odbudować i wdrożone bardzo często. Jednak ta metoda jest zazwyczaj mniej zaakceptować dla środowisk przemieszczania i produkcji.

Na przykład end-to-end scenariusza, który korzysta z podejścia agenta zdalnego, zobacz [scenariusz: Konfigurowanie środowiska testowego na potrzeby wdrażania w sieci Web](scenario-configuring-a-test-environment-for-web-deployment.md).

## <a name="using-the-temp-agent"></a>Przy użyciu tymczasowego agenta

Podejście tymczasowego agenta do wdrożenia jest podobna do metody agenta zdalnego. Jednak w przeciwieństwie do podejścia agenta zdalnego nie trzeba zainstalować narzędzie Web Deploy na docelowym serwerze sieci web. Zamiast tego podczas wykonywania wdrożenia narzędzia Web Deploy zainstaluje tymczasowego wersja usługi sieci web wdrożenia agenta na serwerze docelowym i użyj go do wdrożenia zawartości w usługach IIS. Po zakończeniu wdrożenia zostaną usunięte wszystkie pliki tymczasowe.

Jeśli chcesz użyć ustawienie dostawcy tymczasowego agenta, należy dodać **/g** flagi polecenia wdrażania:


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample4.cmd)]


> [!NOTE]
> Nie można użyć tymczasowego agenta Jeśli usługę sieci web deployment agent jest zainstalowany na komputerze docelowym, nawet jeśli usługa nie jest uruchomiona.


Zaletą tej metody jest nie potrzebnych do obsługi instalacji narzędzia Web Deploy na serwerach sieci docelowej. Ponadto nie należy się upewnić, czy komputer źródłowy i docelowy są uruchomiona ta sama wersja programu Web Deploy. Jednak takie podejście odczuwa te same ograniczenia główną jako rozwiązanie agenta zdalnego mianowicie musi być administratorem lokalnym na serwerze docelowym w celu wdrażania zawartości, czy obsługiwane jest tylko uwierzytelnianie NTLM. Podejście tymczasowego agenta wymaga również znacznie więcej początkowej konfiguracji środowiska docelowego.

Aby uzyskać więcej informacji na temat używania tymczasowego agenta, zobacz [porady: Instalowanie wdrożenia pakietu za pomocą pliku deploy.cmd pliku](https://msdn.microsoft.com/library/ff356104.aspx) i [sieci Web wdrażanie na żądanie](https://technet.microsoft.com/library/ee517345(WS.10).aspx).

## <a name="using-the-web-deploy-handler"></a>Korzystanie z sieci Web wdrażanie programu obsługi

Dla serwera IIS 7 i nowszych wersjach narzędzia Web Deploy oferuje podejścia alternatywnego wdrożenia za pośrednictwem obsługi wdrażania w sieci Web usług IIS. Obsługa wdrażania w sieci Web jest ściśle zintegrowana z usług sieci Web IIS zarządzania (WMSvc), który umożliwia użytkownikom zarządzanie witryn sieci Web usług IIS z lokalizacji zdalnych.

Domyślnie agenta zdalnego udostępnia punkt końcowy HTTP pod tym adresem:


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample5.cmd)]


> [!NOTE]
> Można zastąpić [*serwera*] nazwą komputera serwera sieci web, adres IP serwera sieci web lub nazwy hosta która rozwiązuje problem z serwerem sieci web.


Duży zaletą obsługi wdrażania sieci Web za pośrednictwem zdalnego agenta i tymczasowego agenta jest skonfigurowanie usług IIS, aby umożliwić użytkownikom niebędącym administratorami wdrażanie aplikacji i zawartości do określonych witryn sieci Web usług IIS. Obsługa wdrażania w sieci Web obsługuje również uwierzytelnianie podstawowe, więc jako parametrów można podać alternatywne poświadczenia w poleceniach narzędzia Web Deploy. Główną wadą jest początkowo znacznie bardziej skomplikowane do instalowania i konfigurowania obsługi wdrażania w sieci Web.

W przypadku użytkowników niebędących administratorami usługi zarządzania siecią Web (WMSvc) zezwala tylko użytkownik mógł się połączyć usług IIS przy użyciu połączenia poziom witryny, a nie połączenie poziomu serwera. Aby uzyskać dostęp do określonej lokacji, może zawierać ciągu zapytania specyficzne dla lokacji w adres punktu końcowego:


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample6.cmd)]


Na przykład załóżmy, że proces kompilacji jest skonfigurowany do automatycznego wdrożenia aplikacji sieci web w środowisku przemieszczania po każdym pomyślnej kompilacji. Jeśli używana jest metoda agenta zdalnego, będzie potrzebny do wyznaczenia tożsamość procesu kompilacji administratora na serwerach sieci docelowej. Z kolei, stosując metodę program obsługi wdrażania w sieci Web można udzielać użytkownik bez uprawnień administratora&#x2014;**FABRIKAM\stagingdeployer** w takim przypadku&#x2014;zapewniają te uprawnienia do określonych usług IIS witryna sieci Web tylko i procesu kompilacji poświadczenia, aby wdrożyć pakiet sieci web.


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample7.cmd)]


> [!NOTE]
> Aby uzyskać więcej informacji dotyczących narzędzia Web Deploy operacji wiersza polecenia i składnię, zobacz [odwołania wiersza polecenia wdrażania w sieci Web](https://technet.microsoft.com/library/dd568991(v=ws.10).aspx). Aby uzyskać więcej informacji na temat używania *. pliku deploy.cmd* plików, zobacz [porady: Instalowanie wdrożenia pakietu za pomocą pliku deploy.cmd pliku](https://msdn.microsoft.com/library/ff356104.aspx).


Obsługa wdrażania w sieci Web udostępnia przydatne podejście do wdrożenia w przejściowym środowiskach, środowiskach hostowanych i środowisk produkcyjnych opartych na sieci intranet, gdzie zdalny dostęp do serwera jest dostępna, ale nie są poświadczenia administratora.

Na przykład end-to-end scenariusza, który korzysta z podejścia program obsługi wdrażania w sieci Web, zobacz [scenariusz: Konfigurowanie środowiska przemieszczania na potrzeby wdrażania w sieci Web](scenario-configuring-a-staging-environment-for-web-deployment.md).

## <a name="using-offline-deployment"></a>Przy użyciu wdrożenia w trybie Offline

W niektórych przypadkach nie jest możliwe lub praktyczne wdrażanie aplikacji i zawartości witryny sieci Web usług IIS z lokalizacji zdalnej. Na przykład komputer źródłowy i docelowy może być w sieciach izolowanych albo segmentach sieci, lub zasad zapory nie zezwala na dostęp zdalny.

W scenariuszach takich można nadal używać opakowywanie i publikowania Web Deploy; po prostu nie można używać ich z lokalizacji zdalnej. Zamiast tego administratora na serwerze docelowym należy skopiować pakiet sieci web na serwerze i zaimportuj go za pomocą Menedżera usług IIS.

![](choosing-the-right-approach-to-web-deployment/_static/image1.png)

Metody wdrożenia w trybie offline jest zazwyczaj przydatne w środowiskach produkcyjnych internetowy, gdzie serwery w sieci obwodowej mogą mieć ograniczony łączności z komputerami w sieci wewnętrznej.

Na przykład end-to-end scenariusza, który używa metody wdrożenia w trybie offline, zobacz [scenariusz: Konfigurowanie środowiska produkcyjnego Web Deployment](scenario-configuring-a-production-environment-for-web-deployment.md).

## <a name="further-reading"></a>Dalsze informacje

Aby uzyskać więcej informacji dotyczących narzędzia Web Deploy operacji wiersza polecenia i składnię, zobacz [odwołania wiersza polecenia wdrażania w sieci Web](https://technet.microsoft.com/library/dd568991(v=ws.10).aspx). Aby uzyskać więcej informacji na temat używania *. pliku deploy.cmd* plików, zobacz [porady: Instalowanie wdrożenia pakietu za pomocą pliku deploy.cmd pliku](https://msdn.microsoft.com/library/ff356104.aspx).

Aby uzyskać bardziej ogólne wskazówki na różne sposoby, w którym można wdrożyć pakietów sieci web z komputera zdalnego, zobacz [przy użyciu sieci Web wdrażanie zdalnie](https://technet.microsoft.com/library/ee461175(WS.10).aspx). Aby uzyskać więcej informacji na temat używania sieci Web wdrażanie na żądanie, zobacz [sieci Web wdrażanie na żądanie](https://technet.microsoft.com/library/ee517345(WS.10).aspx).

> [!div class="step-by-step"]
> [Poprzednie](configuring-server-environments-for-web-deployment.md)
> [dalej](scenario-configuring-a-test-environment-for-web-deployment.md)
