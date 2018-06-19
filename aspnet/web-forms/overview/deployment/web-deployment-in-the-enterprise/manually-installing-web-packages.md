---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/manually-installing-web-packages
title: Ręczne instalowanie pakietów sieci Web | Dokumentacja firmy Microsoft
author: jrjlee
description: W tym temacie opisano, jak ręcznie zaimportować pakiet wdrożeniowy sieci web do programu Internet Information Services (IIS). Tworzenie tematu i pakowania instalacja sieci Web...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: f11d22a7-5d32-4ad0-8a9b-276460a61c06
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/manually-installing-web-packages
msc.type: authoredcontent
ms.openlocfilehash: 4a28ea92b22e4928e41a39a8a91b62bfa4f08175
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30890219"
---
<a name="manually-installing-web-packages"></a>Ręczne instalowanie pakietów sieci Web
====================
przez [Lewandowski Jason](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> W tym temacie opisano, jak ręcznie zaimportować pakiet wdrożeniowy sieci web do programu Internet Information Services (IIS).
> 
> Temat [budynku i projekty aplikacji sieci Web pakowania](building-and-packaging-web-application-projects.md) opisano w jaki sposób usługi IIS narzędzie Web Deployment (Web Deploy), w połączeniu z aparat kompilacji firmy Microsoft (MSBuild) i sieci Web publikowania potoku (WPP), umożliwia pakietu sieci Projekty aplikacji sieci Web do pliku zip pojedynczego. Ten plik, powszechnie znane jako pakiet wdrożeniowy sieci web (lub po prostu pakiet wdrożeniowy) zawiera wszystkie informacje zawartość i konfigurację czy usługi IIS wymaga, aby ponownie utworzyć aplikację sieci web na serwerze sieci web.
> 
> Po utworzeniu pakietu wdrożeniowego sieci web można opublikować na serwerze usług IIS na różne sposoby. W wiele scenariuszy należy skorzystać z punkty integracji między MSBuild, WPP i Web Deploy do tworzenia i instalowania pakietów sieci web zdalnie w trakcie procesu automatyczna lub pojedynczy krok kompilacji i wdrożenia. Ten proces jest opisany w [wdrażanie pakietów sieci Web](deploying-web-packages.md). Jednak nie jest to zawsze możliwe. Załóżmy, że chcesz wdrożyć aplikację sieci web do środowiska produkcyjnego skierowane do Internetu. Ze względów bezpieczeństwa środowiska produkcyjnego, jest na bardzo najmniej mogą znajdować się za zaporą w podsieci, która jest niezależna od serwera kompilacji w sieci obwodowej (znanej także jako strefa DMZ, strefą zdemilitaryzowaną i podsiecią ekranowaną). W wielu przypadkach w środowisku produkcyjnym będzie w osobnej domenie lub w sieci izolowanej fizycznie.
> 
> W tych scenariuszach może być jedyną opcją portu pakiet sieci web na serwerze docelowym i ręcznie zaimportuj go do usług IIS. Mimo że takie podejście wyklucza automatycznego wdrażania, jest nadal bardzo wydajny technika publikowania aplikacji sieci web&#x2014;po prostu skopiuj plik zip jednego do serwera sieci web i jest używany Kreator prowadzące przez proces importowania.


Ten temat jest częścią serii samouczków na podstawie tych wymagań związanych z przedsiębiorstwa wdrażaniem fikcyjnej firmy o nazwie firmy Fabrikam, Inc. Przykładowe rozwiązanie korzysta z tego samouczka serii&#x2014; [rozwiązania kontaktów Menedżerze](the-contact-manager-solution.md)&#x2014;do reprezentowania aplikacji sieci web z realistyczne poziom złożoności, w tym aplikacji ASP.NET MVC 3, Windows Communication Usługa Foundation (WCF), a projekt bazy danych.

## <a name="task-overview"></a>Omówienie zadań

Musisz wykonać te zadania wysokiego poziomu, można zaimportować pakietu wdrożeniowego sieci web do usług IIS:

- Utwórz pakiet wdrożeniowy sieci web przy użyciu programu MSBuild wiersza polecenia, Team Build lub Visual Studio 2010.
- Skopiuj pakiet sieci web do docelowego serwera sieci web.
- Aby zainstalować pakiet sieci web i podaj wartości zmiennych, takich jak parametry połączenia i punktów końcowych usługi, należy użyć Kreatora importowania aplikacji pakietu w Menedżerze usług IIS.

W tym temacie opisano sposób wykonywania tych procedur. Zadania i wskazówki, w tym temacie założono, że znasz już pojęcia dotyczące pakietów sieci web narzędzia Web Deploy i WPP. Aby uzyskać więcej informacji, zobacz [budynku i projekty aplikacji sieci Web pakowania](building-and-packaging-web-application-projects.md).

> [!NOTE]
> W tym temacie najlepiej jest używany w połączeniu z [Konfiguracja serwera sieci Web dla wdrożenia publikowania w sieci Web (wdrożenie w trybie Offline)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md), który wyjaśnia, jak zainstalować wymagane składniki i przygotować witryny sieci Web usług IIS do zaimportowania pakietu.


## <a name="create-a-web-deployment-package"></a>Utwórz pakiet wdrożeniowy sieci Web

Pierwszym zadaniem jest utworzyć pakiet wdrożeniowy sieci web dla projektu aplikacji sieci web, którą chcesz wdrożyć. Pakiety sieci web można tworzyć wiele sposobów.

**Podejście 1: Tworzenie pakietu jako część procesu kompilacji z programem Visual Studio**

Można skonfigurować projektu aplikacji sieci web, aby utworzyć pakiet wdrożeniowy sieci web po każdej kompilacji za pomocą **pakowaniu/publikowaniu Web** kartę na stronach właściwości projektu. Ten proces jest opisany w [budynku i projekty aplikacji sieci Web pakowania](building-and-packaging-web-application-projects.md).

**Podejście 2: Tworzenie pakietu jako część procesu kompilacji przy użyciu programu MSBuild**

Skompilowanie projektu aplikacji sieci web przy użyciu programu MSBuild bezpośrednio, za pomocą niestandardowego pliku projektu MSBuild lub z wiersza polecenia, można utworzyć pakiet wdrożeniowy sieci web jako część procesu kompilacji, umieszczając w niej **DeployOnBuild = true** i **DeployTarget = pakiet** właściwości polecenia. Ten proces jest opisany w [opis procesu kompilacji](understanding-the-build-process.md).

**Podejście 3: Tworzenie pakietu na żądanie w programie Visual Studio**

W programie Visual Studio 2010 w dowolnym momencie można utworzyć pakietu wdrożeniowego sieci web dla projektu aplikacji sieci web. Aby to zrobić, w **Eksploratora rozwiązań** , kliknij prawym przyciskiem myszy projekt aplikacji sieci web, a następnie kliknij przycisk **kompilacji pakietu wdrożeniowego**.

![](manually-installing-web-packages/_static/image1.png)

**Podejście 4: Tworzenie pakietu na żądanie z poziomu wiersza polecenia**

Można utworzyć pakiet wdrożeniowy sieci web z wiersza polecenia, wywołując **pakietu** docelowa projektu aplikacji sieci web przy użyciu programu MSBuild. Polecenie powinien wyglądać następująco:


[!code-console[Main](manually-installing-web-packages/samples/sample1.cmd)]


Zależności podejścia, należy użyć, wynik końcowy jest taki sam. WPP tworzy pakietu wdrożeniowego sieci web jako plik zip, wraz z różnymi zasobami obsługi, w folderze wyjściowym dla projektu aplikacji sieci web.

![](manually-installing-web-packages/_static/image2.png)

Jeśli planujesz ręczne Importowanie pakietu sieci web, wymagane jest tylko plik zip. Skopiuj ten plik do docelowego serwera sieci web i można rozpocząć proces importowania.

## <a name="import-a-web-package-into-iis"></a>Importowanie pakietu sieci Web do usług IIS

Następna procedura służy do importowania pakietu wdrożeniowego sieci web z lokalnego systemu plików do witryny sieci Web usług IIS. Przed wykonaniem tej procedury upewnij się, że masz:

- Skopiować pakiet wdrożeniowy sieci web na serwerze sieci web.
- Skonfigurować serwer sieci web usług IIS do hostowania aplikacji.

Aby uzyskać więcej informacji na temat konfigurowania serwera sieci web usług IIS do obsługi sieci web pakiety wdrożeniowe, zobacz [Konfiguracja serwera sieci Web dla wdrożenia publikowania w sieci Web (wdrożenie w trybie Offline)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md).

**Aby zaimportować pakiet wdrażania sieci web za pomocą Menedżera usług IIS**

1. W Menedżerze usług IIS w **połączeń** okienku kliknij prawym przyciskiem myszy witryny sieci Web usług IIS, wskaż pozycję **Wdróż**, a następnie kliknij przycisk **Importuj aplikację**.

    ![](manually-installing-web-packages/_static/image3.png)
2. W Kreatorze importu pakietu aplikacji na **wybierz pakiet** , przejdź do lokalizacji pakietu wdrażania w sieci web, a następnie kliknij przycisk **dalej**.
3. Na **wybierz zawartość pakietu** zawartość, która nie wymagają, a następnie kliknij przycisk Wyczyść **dalej**.

    ![](manually-installing-web-packages/_static/image4.png)

    > [!NOTE]
    > W partii przypadków nie możesz zaimportować wszystko, co jest dostarczany z pakietu wdrożeniowego sieci web. Na przykład może nie chcesz zezwolić narzędzia Web Deploy zastąpić skojarzonej bazie danych.  
    > **Udzielić uprawnień** wpisy uprawnienia systemu plików docelowego, aby upewnić się, że tożsamość puli aplikacji dostęp do folderu fizycznego, którym jest przechowywana zawartość witryny sieci Web. Ponadto użytkownik uwierzytelnianie anonimowe jest przyznane uprawnienia do odczytu folderu pozwala udostępniać pliki poczty rozszerzenia MIME (Multipurpose Internet), typ aplikacji. Jeśli wolisz, możesz usunąć te wpisy i ręcznie skonfigurować uprawnienia.
4. Na **wprowadź informacje o pakiecie aplikacji** podaj żądane informacje.

    ![](manually-installing-web-packages/_static/image5.png)
5. Podczas tworzenia pakietu sieci web WPP analizuje pliku konfiguracji aplikacji i wykrywa wszystkie zmienne, takie jak parametry połączenia i punktów końcowych usługi. W takim przypadku:

    1. **Ścieżka aplikacji** jest ścieżka programu IIS, w którym chcesz zainstalować aplikację. To ustawienie jest wspólny dla wszystkich pakiety wdrożeniowe, które tworzy WPP.
    2. **Adres punktu końcowego usługi ContactService** jest adresem, która powinna być używana do komunikacji z usługą WCF wdrożonych aplikacji. To ustawienie odpowiada wpisowi w *web.config* pliku.
    3. Pierwszy **ciąg połączenia** ustawienie to ciąg połączenia wdrożenia sieci Web powinna być używana do wdrażania bazy danych skojarzone z aplikacją (w tym przypadku członkostwa bazy danych programu ASP.NET). To ustawienie odpowiada ustawieniu na **Pakuj/Publikuj SQL** kartę w programie Visual Studio.
    4. Drugi **ciąg połączenia** ustawienie to parametry połączenia, które faktycznie będzie używać aplikacja do komunikacji z bazą danych, po skonfigurowaniu i uruchomieniu. Odpowiada to wpisem ciągu połączenia w *web.config* pliku.

        > [!NOTE]
        > Aby uzyskać więcej informacji na skąd pochodzą te parametry, zobacz [konfigurowania parametrów wdrażania pakietu sieci Web](configuring-parameters-for-web-package-deployment.md).
6. Kliknij przycisk **Dalej**.
7. Jeśli nie jest pierwszym wdrożeniu aplikacji do tej witryny sieci Web, zostanie wyświetlony monit Określ, czy chcesz usunąć wszystkie istniejącej zawartości przed instalacją. Wybierz opcję odpowiednią do wymagań, a następnie kliknij przycisk **dalej**.

    ![](manually-installing-web-packages/_static/image6.png)
8. Gdy usługi IIS zakończył instalowanie pakietu, kliknij przycisk **Zakończ**.

    ![](manually-installing-web-packages/_static/image7.png)

W tym momencie został pomyślnie opublikowany aplikacji sieci web do usług IIS.

## <a name="conclusion"></a>Wniosek

W tym temacie opisano jak zaimportować pakiet wdrożeniowy sieci web do witryny sieci Web usług IIS przy użyciu Menedżera usług IIS. Takie podejście do publikowania aplikacji sieci web jest odpowiednia, kiedy ograniczenia zabezpieczeń lub infrastruktury wdrażania zdalnego niemożliwe lub niepożądane.

## <a name="further-reading"></a>Dalsze informacje

Aby uzyskać wskazówki dotyczące sposobu konfigurowania serwera sieci web usług IIS do obsługi Ręczne importowanie pakietu sieci web, zobacz [Konfiguracja serwera sieci Web dla wdrożenia publikowania w sieci Web (wdrożenie w trybie Offline)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md). Aby uzyskać bardziej ogólne wskazówki dotyczące wdrażania pakietów sieci web, zobacz [wskazówki: Wdrażanie pomocą projektu aplikacji sieci Web pakietu wdrożeniowego sieci Web (część 1 z 4)](https://msdn.microsoft.com/library/dd483479.aspx).

> [!div class="step-by-step"]
> [Poprzednie](creating-and-running-a-deployment-command-file.md)
