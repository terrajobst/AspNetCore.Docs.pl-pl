---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-a-tfs-build-server-for-web-deployment
title: Konfigurowanie TFS serwer kompilacji dla wdrażania w Internecie | Dokumentacja firmy Microsoft
author: jrjlee
description: W tym temacie opisano sposób przygotowania serwera kompilacji Team Foundation Server (TFS) do tworzenia i wdrażania rozwiązań przy użyciu kompilacji zespołowej i Informat internetowego...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: f8400241-4f4b-4bbd-9994-54fb64909e6e
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-a-tfs-build-server-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 534a0edf5c26dd2daec3c44e41410f8c5de96c15
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756901"
---
<a name="configuring-a-tfs-build-server-for-web-deployment"></a>Konfigurowanie serwera kompilacji TFS dla wdrażania w Internecie
====================
przez [Jason Lee](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> W tym temacie opisano sposób przygotowania serwera kompilacji Team Foundation Server (TFS), aby tworzyć i wdrażać rozwiązania przy użyciu kompilacji zespołowej i usługi Internet Information Services (IIS) Narzędzie wdrażania sieci Web (Web Deploy).


Ten temat jest częścią serii samouczków na podstawie wymagania dotyczące wdrażania enterprise fikcyjnej firmy o nazwie firmy Fabrikam, Inc. Przykładowe rozwiązanie korzysta z tej serii samouczków&#x2014; [rozwiązania Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;do reprezentowania aplikacji sieci web przy użyciu realistycznej stopień złożoności, łącznie z aplikacją ASP.NET MVC 3 komunikacji Windows Usługa Foundation (WCF), a projekt bazy danych.

Metody wdrażania w ramach tego samouczka opiera się na podejście pliku projektu Podziel opisane w [objaśnienie pliku projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md), w którym proces kompilacji jest kontrolowana przez dwa pliki projektu&#x2014;jeden zawierający Tworzenie instrukcji, które mają zastosowanie do każdego środowiska docelowego i jeden zawierający ustawienia specyficzne dla środowiska kompilacji i wdrażania. W czasie kompilacji pliku projektu specyficznymi dla środowiska jest scalana w pliku projektu niezależnego od środowiska w celu utworzenia kompletny zestaw instrukcji kompilacji.

## <a name="task-overview"></a>Omówienie zadań

Aby przygotować serwer kompilacji do tworzenia i wdrażania rozwiązania, należy:

- Instalowanie i konfigurowanie usługi kompilacji TFS.
- Zainstaluj program Visual Studio 2010.
- Zainstaluj wszystkie produkty lub składniki, które są wymagane do utworzenia rozwiązania, takie jak wersje .NET Framework i ASP.NET MVC.
- Zainstaluj narzędzie Web Deploy 2.0 lub nowszej.

W tym temacie pokazują sposób wykonania tych procedur lub wskazywać na inne zasoby, jeśli takie istnieją. Zadania i wskazówki, w tym temacie przyjęto założenie, że:

- Zaczynasz z kompilacją serwera czyste uruchamiania systemu Windows Server 2008 R2 z dodatkiem Service Pack 1.
- Serwer jest przyłączony do domeny za pomocą statycznego adresu IP.
- Po zainstalowaniu warstwy aplikacji TFS na osobnym serwerze, zgodnie z opisem w [wdrażania sieci Web w przedsiębiorstwie: omówienie scenariusza](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md).

### <a name="who-performs-these-procedures"></a>Osób wykonujących te procedury?

W większości przypadków będzie odpowiedzialny za konfigurowanie serwerów kompilacji administratora TFS. W niektórych przypadkach zespołu deweloperów może przejąć prawo własności serwerami konkretnej kompilacji.

## <a name="install-and-configure-the-tfs-build-service"></a>Instalowanie i konfigurowanie usługi kompilacji serwera TFS

Podczas konfigurowania serwera kompilacji, najpierw jest instalowanie i konfigurowanie usługi kompilacji TFS. W ramach tego procesu musisz:

- Zainstaluj usługi kompilacji serwera TFS i skonfiguruj konto usługi. Wszystkie zadania kompilacji, w tym wdrażania, uruchamiana przy użyciu tożsamości konta usługi kompilacji.
- Tworzenie *kontroler kompilacji* oraz jednego lub więcej *agentów kompilacji*. Każdy kontroler kompilacji zarządza zbiorem agentów kompilacji. Po dodaniu kompilacji do kolejki, kontroler kompilacji przypisuje zadanie kompilacji w agencie kompilacji dostępne. Każdej kolekcji projektów zespołowych w programie TFS jest zamapowana na kontroler pojedynczej kompilacji.
- Skonfiguruj folder do wrzucania dla dane wyjściowe kompilacji. Jest to udział sieciowy. Dowolny można tworzyć dane wyjściowe, takie jak pakiety wdrażania sieci web, są wysyłane do folderu docelowego.

[Administrowanie programem Team Foundation Build](https://msdn.microsoft.com/library/ms252495.aspx) rozdziału w witrynie MSDN zawiera wszystkie zasoby potrzebne do wykonywania następujących zadań:

- Omówienie koncepcji programu Team Foundation Build, w tym usługi kompilacji, kontrolerami kompilacji i agentów kompilacji, zobacz [opis systemu rozszerzenia Team Foundation Build](https://msdn.microsoft.com/library/dd793166.aspx).
- Aby uzyskać informacje dotyczące instalowania i konfigurowania usługi kompilacji, zobacz [skonfigurować maszynę kompilacji](https://msdn.microsoft.com/library/ms181712.aspx).
- Aby uzyskać informacje na temat tworzenia kontrolerów kompilacji, zobacz [Create and Work za pomocą kontrolera kompilacji](https://msdn.microsoft.com/library/ee330987.aspx).
- Aby uzyskać informacje na temat tworzenia agentów kompilacji, zobacz [tworzenie i Praca z agentami kompilacji](https://msdn.microsoft.com/library/bb399135.aspx).
- Aby uzyskać informacje na temat tworzenia i konfigurowania folderów do wrzucania, zobacz [Ustaw się usunąć foldery](https://msdn.microsoft.com/library/bb778394.aspx).

## <a name="install-required-products-and-components"></a>Instalowanie wymaganych produktów i składników

Aby włączyć serwer kompilacji, tworzyć swoje rozwiązania, należy zainstalować żadnych produktów, składników lub zestawów, które wymaga rozwiązania. Przed zainstalowaniem składników platformy sieci web, należy zainstalować program Visual Studio 2010 (dowolna wersja) na serwerze kompilacji. Dzięki temu, że pliki docelowe core aparatu Microsoft Build Engine (MSBuild) i pliki docelowe Web potok publikowania (WPP) są dostępne dla usługi kompilacji. Instalator programu Visual Studio należy także zainstalować narzędzia Web Deploy, który będziesz potrzebować, jeśli planujesz wdrażanie pakietów internetowych jako część procesu kompilacji.

Najlepszym sposobem instalowania wspólnych składników platformy sieci web jest użycie [Instalatora platformy sieci Web](https://go.microsoft.com/?linkid=9805118). Dzięki temu użytkownik instaluje najnowszą wersję każdego produktu, a także automatycznie wykryje i zainstaluje wszystkie wymagania wstępne dla każdego produktu. W przypadku właściwości [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) rozwiązania, należy używać Instalatora platformy sieci Web do zainstalowania tych produktów i składników:

- **.NET framework 4.0**. Jest to wymagane do uruchamiania aplikacji, które zostały utworzone w tej wersji programu .NET Framework.
- **Sieci Web narzędzia do wdrażania 2.1 lub nowszej**. Spowoduje to zainstalowanie narzędzia Web Deploy (i jego podstawowego pliku wykonywalnego, MSDeploy.exe) na serwerze. W ramach tego procesu instaluje i uruchamia usługę agenta wdrażania sieci Web. Ta usługa umożliwia wdrażanie pakietów internetowych z komputera zdalnego.
- **ASP.NET MVC 3**. Spowoduje to zainstalowanie zestawów potrzebne do uruchamiania aplikacji ASP.NET MVC 3.

**Aby zainstalować wymagane produktów i składników**

1. Zainstaluj program Visual Studio 2010. Po wyświetleniu monitu wybierz funkcje do zainstalowania powinien zawierać:

    1. Wszystkie języki programowania, które należy przeprowadzić kompilowanie.
    2. Visual Web Developer. Daje to gwarancję, że elementy docelowe WPP zostały dodane do serwera kompilacji.

        ![](configuring-a-tfs-build-server-for-web-deployment/_static/image1.png)
2. Po zakończeniu instalacji programu Visual Studio 2010, Pobierz i zainstaluj [dodatku Service Pack 1 dla programu Visual Studio 2010](https://go.microsoft.com/?linkid=9805133) (jeśli jeszcze nie wchodzi on w nośniku instalacyjnym).

    > [!NOTE]
    > Visual Studio 2010 z dodatkiem Service Pack 1 jest rozpoznawany jako usterki, które mogą uniemożliwić MSBuild z łączami, wykonywalny MSDeploy.
3. Pobierz i uruchom [Instalatora platformy sieci Web](https://go.microsoft.com/?linkid=9805118).
4. W górnej części **3.0 Instalatora platformy sieci Web** okna, kliknij przycisk **produktów**.
5. W lewej części okna, w okienku nawigacji kliknij **struktur**.
6. W **Microsoft .NET Framework 4** wiersz, jeśli nie zainstalowano jeszcze programu .NET Framework, kliknij przycisk **Dodaj**.

    > [!NOTE]
    > Może być już zainstalowano program .NET Framework 4.0, za pośrednictwem usługi Windows Update. W przypadku produktu lub składnik jest już zainstalowany, Instalator platformy sieci Web poda to przez zastąpienie **Dodaj** przycisk z tekstem **zainstalowane**.

    ![](configuring-a-tfs-build-server-for-web-deployment/_static/image2.png)
7. W **ASP.NET MVC 3 (Visual Studio 2010)** wiersz, kliknij przycisk **Dodaj**.
8. W okienku nawigacji kliknij **serwera**.
9. W **2.1 narzędzia wdrażania Web** wiersz, kliknij przycisk **Dodaj**.
10. Kliknij przycisk **zainstalować**. Instalator platformy sieci Web zostanie wyświetlona lista produktów&#x2014;oraz wszystkie powiązane zależności&#x2014;do zainstalowania i zostanie wyświetlony monit o zaakceptowanie postanowień licencyjnych.
11. Przejrzyj postanowienia licencyjne, a jeśli wyrażasz zgodę na warunki, kliknij przycisk **akceptuję**.
12. Po zakończeniu instalacji kliknij przycisk **Zakończ**, a następnie Zamknij **3.0 Instalatora platformy sieci Web** okna.

> [!NOTE]
> Jeśli proces wdrażania obejmuje korzystanie z narzędzi, takich jak VSDBCMD.exe lub SQLCMD.exe, należy zagwarantować, że są one zainstalowane na serwerze kompilacji. VSDBCMD.exe to narzędzie programu Visual Studio i zwykle jest dodawany do serwera podczas instalacji Team Foundation Build. SQLCMD.exe to narzędzie programu SQL Server. Możesz pobrać autonomiczną wersję SQLCMD.exe z [programu Microsoft SQL Server 2008 R2 Feature Pack](https://go.microsoft.com/?linkid=9805134) strony.


## <a name="conclusion"></a>Wniosek

W tym momencie serwer kompilacji jest gotowe do rozpoczęcia tworzenia i wdrażania projektów aplikacji sieci web. Następny temat [tworzenie kompilacji definicji, obsługuje wdrożenia](creating-a-build-definition-that-supports-deployment.md), opisuje, jak utworzyć i skonfigurować definicję kompilacji do kontrolowania, kiedy i jak utworzeniu i wdrożeniu Twoich projektów.

## <a name="further-reading"></a>Dalsze informacje

Aby uzyskać bardziej ogólne wskazówki na temat pracy z kompilacją zespołową, zobacz [administrowanie programem Team Foundation Build](https://msdn.microsoft.com/library/ms252495.aspx).

> [!div class="step-by-step"]
> [Poprzednie](adding-content-to-source-control.md)
> [dalej](creating-a-build-definition-that-supports-deployment.md)
