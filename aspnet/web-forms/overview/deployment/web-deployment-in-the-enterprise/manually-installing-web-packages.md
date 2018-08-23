---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/manually-installing-web-packages
title: Ręczne instalowanie pakietów internetowych | Dokumentacja firmy Microsoft
author: jrjlee
description: W tym temacie opisano, jak ręcznie zaimportować pakiet wdrażania sieci web do programu Internet Information Services (IIS). Tworzenie tematu i instalacja sieci Web pakietu...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: f11d22a7-5d32-4ad0-8a9b-276460a61c06
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/manually-installing-web-packages
msc.type: authoredcontent
ms.openlocfilehash: ca85db5cfb30bc06d6d3b94001a3668088461b87
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754683"
---
<a name="manually-installing-web-packages"></a>Ręczne instalowanie pakietów internetowych
====================
przez [Jason Lee](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> W tym temacie opisano, jak ręcznie zaimportować pakiet wdrażania sieci web do programu Internet Information Services (IIS).
> 
> Temat [budowanie i projektów aplikacji sieci Web pakietu](building-and-packaging-web-application-projects.md) opisane w jaki sposób usługi IIS narzędzie Web Deployment (Web Deploy), w połączeniu z aparatu Microsoft Build Engine (MSBuild) i sieci Web Publishing potoku (WPP), umożliwia pakiet usługi Projekty aplikacji sieci Web do postaci pliku zip jednego. Ten plik, powszechnie znane jako pakiet wdrażania sieci web (lub po prostu pakietu wdrożeniowego) zawiera wszystkie informacje zawartość i konfigurację, usług IIS wymaga, aby ponownie utworzyć aplikację sieci web na serwerze sieci web.
> 
> Po utworzeniu pakietu wdrożeniowego sieci web, możesz opublikować go na serwerze usług IIS na różne sposoby. Wiele scenariuszy warto korzystać z zalet punktów integracji między MSBuild, WPP i narzędzia Web Deploy na tworzenie i instalowanie pakietów internetowych zdalnie w ramach automatycznych lub pojedynczy krok procesu kompilowania i wdrażania. Ten proces jest opisany w [wdrażanie pakietów internetowych](deploying-web-packages.md). Jednak nie jest to zawsze możliwe. Załóżmy, że chcesz przeprowadzić wdrożenie aplikacji sieci web w środowisku produkcyjnym dostępnego z Internetu. Ze względów bezpieczeństwa środowiska produkcyjnego wynosi bardzo szanse na się za zaporą w podsieci, który jest oddzielony od serwera kompilacji, w sieci obwodowej (znany także jako DMZ, strefa zdemilitaryzowana i podsieć ekranowana). W wielu przypadkach w środowisku produkcyjnym będzie w oddzielnej domenie lub w sieci izolowanej fizycznie.
> 
> W tych scenariuszach jedynym rozwiązaniem może być portu pakiet sieci web na serwerze docelowym i ręcznie zaimportuj go do usług IIS. Chociaż to podejście wyklucza automatycznego wdrażania, jest nadal bardzo skuteczne technika publikowania aplikacji sieci web&#x2014;po prostu skopiuj plik zip pojedynczy serwer sieci web i proces importu za pomocą kreatora.


Ten temat jest częścią serii samouczków na podstawie wymagania dotyczące wdrażania enterprise fikcyjnej firmy o nazwie firmy Fabrikam, Inc. Przykładowe rozwiązanie korzysta z tej serii samouczków&#x2014; [rozwiązania Contact Manager](the-contact-manager-solution.md)&#x2014;do reprezentowania aplikacji sieci web przy użyciu realistycznej stopień złożoności, łącznie z aplikacją ASP.NET MVC 3 komunikacji Windows Usługa Foundation (WCF), a projekt bazy danych.

## <a name="task-overview"></a>Omówienie zadań

Należy wykonać te zadania wysokiego poziomu, aby zaimportować pakiet wdrażania sieci web do usług IIS:

- Utwórz pakiet wdrażania sieci web przy użyciu programu MSBuild wiersza polecenia, Team Build lub Visual Studio 2010.
- Skopiuj pakiet sieci web do docelowego serwera sieci web.
- Aby zainstalować pakiet sieci web i podaj wartości dla zmiennych, takich jak parametry połączenia i punktów końcowych usługi, należy użyć Kreator importu pakietu aplikacji w Menedżerze usług IIS.

W tym temacie pokazują sposób wykonania tych procedur. Zadania i wskazówki, w tym temacie założono, że znasz już pojęcia dotyczące pakietów sieci web narzędzia Web Deploy oraz potok WPP. Aby uzyskać więcej informacji, zobacz [budowanie i projektów aplikacji sieci Web pakietu](building-and-packaging-web-application-projects.md).

> [!NOTE]
> W tym temacie najlepiej sprawdza się w połączeniu z [Konfigurowanie serwera sieci Web dla wdrożenia publikowania w sieci Web (wdrożenie w trybie Offline)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md), która wyjaśnia, jak zainstalować wymagane składniki i przygotować to witryna sieci Web usług IIS do importowania pakietu.


## <a name="create-a-web-deployment-package"></a>Utwórz pakiet wdrażania sieci Web

Pierwszym zadaniem jest utworzenie pakietu wdrożeniowego sieci web dla projektu aplikacji sieci web, którą chcesz wdrożyć. Pakiety sieci web można tworzyć różne sposoby.

**Podejście 1: Tworzenie pakietu jako część procesu kompilacji z programem Visual Studio**

Można skonfigurować projektu aplikacji sieci web, aby utworzyć pakiet wdrażania sieci web po każdej kompilacji za pomocą **pakowaniu/publikowaniu Web** kartę na stronach właściwości projektu. Ten proces jest opisany w [budowanie i projektów aplikacji sieci Web pakietu](building-and-packaging-web-application-projects.md).

**Podejście 2: Tworzenie pakietu jako część procesu kompilacji za pomocą narzędzia MSBuild**

Tworzenie projektu aplikacji sieci web przy użyciu programu MSBuild bezpośrednio albo za pomocą niestandardowego pliku projektu MSBuild w wierszu polecenia można utworzyć pakiet wdrażania sieci web jako część procesu kompilacji, umieszczając **DeployOnBuild = true** i **DeployTarget = pakiet** właściwości w swojej dyspozycji. Ten proces jest opisany w [objaśnienie procesu kompilacji](understanding-the-build-process.md).

**Podejście 3: Utwórz pakiet na żądanie w programie Visual Studio**

Pakiet wdrażania sieci web dla projektu aplikacji sieci web można utworzyć w dowolnym momencie w programie Visual Studio 2010. Aby to zrobić, w **Eksploratora rozwiązań** , kliknij prawym przyciskiem myszy projekt aplikacji sieci web, a następnie kliknij przycisk **kompilacji pakietu wdrożeniowego**.

![](manually-installing-web-packages/_static/image1.png)

**Podejście 4: Tworzenie pakietu na żądanie z poziomu wiersza polecenia**

Można utworzyć pakiet wdrażania sieci web z wiersza polecenia za pomocą wywołania **pakietu** obiektu docelowego projektu aplikacji sieci web, korzystając z programu MSBuild. Polecenie powinien wyglądać następująco:


[!code-console[Main](manually-installing-web-packages/samples/sample1.cmd)]


Zależności od tego podejścia, możesz użyć, efekt jest taki sam. Potok WPP tworzy pakiet wdrażania sieci web w postaci pliku zip, wraz z różnych zasobów pomocniczych, w folderze danych wyjściowych dla projektu aplikacji sieci web.

![](manually-installing-web-packages/_static/image2.png)

Jeśli zamierzasz ręcznie zaimportować pakiet, potrzebujesz tylko pliku zip. Skopiuj ten plik do docelowego serwera sieci web i rozpocząć proces importowania.

## <a name="import-a-web-package-into-iis"></a>Importowanie pakietu sieci Web do usług IIS

Aby zaimportować pakiet wdrażania sieci web z lokalnego systemu plików do witryny sieci Web usług IIS, można użyć następnej procedury. Przed wykonaniem tej procedury upewnij się, że masz:

- Skopiować pakiet wdrażania sieci web na serwerze sieci web.
- Skonfigurowany serwer sieci web usług IIS do hostowania aplikacji.

Aby uzyskać więcej informacji na temat konfigurowania serwera sieci web usług IIS do obsługi pakietów wdrażania sieci web, zobacz [Konfigurowanie serwera sieci Web dla wdrożenia publikowania w sieci Web (wdrożenie w trybie Offline)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md).

**Aby zaimportować pakiet wdrażania sieci web, za pomocą Menedżera usług IIS**

1. W Menedżerze usług IIS w **połączeń** okienku kliknij prawym przyciskiem myszy witryny sieci Web usług IIS, wskaż opcję **Wdróż**, a następnie kliknij przycisk **Importuj aplikację**.

    ![](manually-installing-web-packages/_static/image3.png)
2. W Kreatorze importowania pakietów aplikacji na **wybierz pakiet** strony, przejdź do lokalizacji pakietu wdrażania sieci web, a następnie kliknij przycisk **dalej**.
3. Na **wybierz zawartość pakietu** strony, zawartość, która nie wymaga, a następnie kliknij przycisk Wyczyść **dalej**.

    ![](manually-installing-web-packages/_static/image4.png)

    > [!NOTE]
    > W wielu przypadkach może nie chcieć zaimportować wszystkie elementy, dołączonej do pakietu wdrożeniowego sieci web. Na przykład nie może być Zezwalaj na narzędzia Web Deploy w celu zastąpienia skojarzonej bazy danych.  
    > **Udzielić uprawnień** wpisów, ustaw uprawnienia w systemie plików docelowego, aby upewnić się, że tożsamość puli aplikacji dostęp do folderu fizycznego, która przechowuje zawartości witryny sieci Web. Ponadto użytkownika uwierzytelnianie anonimowe jest przyznane uprawnienia do odczytu folderze umożliwi aplikacji obsługi plików typu protokołu dynamicznej konfiguracji hosta (MIME, Multipurpose Internet Mail Extensions). Jeśli wolisz, możesz usunąć te wpisy i ręcznie skonfigurować uprawnienia.
4. Na **wprowadzanie informacji o pakiecie aplikacji** podaj żądane informacje.

    ![](manually-installing-web-packages/_static/image5.png)
5. Podczas tworzenia pakietu sieci web, potok WPP analizuje plik konfiguracyjny aplikacji i wykrywa żadnych zmiennych, takich jak parametry połączenia i punktów końcowych usługi. W takim przypadku:

    1. **Ścieżka aplikacji** jest ścieżką usług IIS, w którym chcesz zainstalować aplikację. To ustawienie jest wspólne dla wszystkich pakiety wdrożeniowe, które tworzy potok WPP.
    2. **Adres punktu końcowego usługi ContactService** adres, który aplikacja powinna używać do komunikowania się z wdrożonej usługi WCF. To ustawienie odnosi się do zapisu w *web.config* pliku.
    3. Pierwszy **parametry połączenia** ma parametry połączenia, którego narzędzie Web Deploy należy użyć do wdrożenia bazy danych skojarzonych z aplikacją (w tym przypadku członkostwa bazy danych programu ASP.NET). To ustawienie odpowiada ustawieniu na **Pakuj/Publikuj SQL** kartę w programie Visual Studio.
    4. Drugi **parametry połączenia** ma parametry połączenia, które faktycznie użyje aplikacji do komunikowania się z bazą danych, gdy jest uruchomiona. Odpowiada to wpis ciągu połączenia w *web.config* pliku.

        > [!NOTE]
        > Aby uzyskać więcej informacji na temat pochodzenie tych parametrów, zobacz [konfigurowania parametrów wdrożenia pakietu internetowego](configuring-parameters-for-web-package-deployment.md).
6. Kliknij przycisk **Dalej**.
7. Jeśli nie jest to udało Ci się wdrożyć aplikację do tej witryny sieci Web po raz pierwszy monit będzie określić, czy Usuń całą istniejącą zawartość przed instalacją. Wybierz opcję, która jest odpowiednia dla wymagań, a następnie kliknij przycisk **dalej**.

    ![](manually-installing-web-packages/_static/image6.png)
8. Gdy usługi IIS zakończy instalowanie pakietu, kliknij przycisk **Zakończ**.

    ![](manually-installing-web-packages/_static/image7.png)

W tym momencie pomyślnie opublikowana aplikacja sieci web usług IIS.

## <a name="conclusion"></a>Wniosek

W tym temacie opisano, jak zaimportować pakiet wdrażania sieci web do witryny sieci Web usług IIS za pomocą Menedżera usług IIS. Takie podejście do publikowania aplikacji sieci web jest odpowiednia w przypadku, gdy ograniczenia zabezpieczeń lub infrastruktury wprowadza zdalnego wdrażania, jest niemożliwe lub niepożądane.

## <a name="further-reading"></a>Dalsze informacje

Aby uzyskać wskazówki dotyczące sposobu konfigurowania serwera sieci web usług IIS do obsługi ręcznym importowaniu definicji pakietu sieci web, zobacz [Konfigurowanie serwera sieci Web dla wdrożenia publikowania w sieci Web (wdrożenie w trybie Offline)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md). Aby uzyskać bardziej ogólne wskazówki na temat wdrażania pakietów sieci web, zobacz [wskazówki: Wdrażanie aplikacji internetowej projektu korzystającej pakiet wdrażania sieci Web (część 1 z 4)](https://msdn.microsoft.com/library/dd483479.aspx).

> [!div class="step-by-step"]
> [Poprzednie](creating-and-running-a-deployment-command-file.md)
