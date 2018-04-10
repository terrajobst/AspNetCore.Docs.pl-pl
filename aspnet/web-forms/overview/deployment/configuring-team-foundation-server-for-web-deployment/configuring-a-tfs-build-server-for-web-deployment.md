---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-a-tfs-build-server-for-web-deployment
title: Konfigurowanie TFS kompilacji serwera sieci Web wdrożenia | Dokumentacja firmy Microsoft
author: jrjlee
description: W tym temacie opisano sposób przygotowania serwera kompilacji Team Foundation Server (TFS) do tworzenia i wdrażania rozwiązań za pomocą Team Build i Informat internetowych...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: f8400241-4f4b-4bbd-9994-54fb64909e6e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-a-tfs-build-server-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 7b3130ca7d36ffec457e1871fa62c1077b5e3174
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/10/2018
---
<a name="configuring-a-tfs-build-server-for-web-deployment"></a>Konfigurowanie serwera kompilacji TFS do wdrożenia sieci Web
====================
przez [Lewandowski Jason](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> W tym temacie opisano sposób przygotowania serwera kompilacji Team Foundation Server (TFS) do tworzenia i wdrażania rozwiązań za pomocą Team Build i Internet Information Services (IIS) Narzędzie wdrażania Web (Web Deploy).


Ten temat jest częścią serii samouczków na podstawie tych wymagań związanych z przedsiębiorstwa wdrażaniem fikcyjnej firmy o nazwie firmy Fabrikam, Inc. Przykładowe rozwiązanie korzysta z tego samouczka serii&#x2014; [rozwiązania kontaktów Menedżerze](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;do reprezentowania aplikacji sieci web z realistyczne poziom złożoności, w tym aplikacji ASP.NET MVC 3, Windows Communication Usługa Foundation (WCF), a projekt bazy danych.

Istotą te samouczki metody wdrażania opiera się na podejście pliku projektu podziału opisane w [opis pliku projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md), w którym jest kontrolowany przez proces kompilacji dwa pliki projektu&#x2014;jeden zawierający Tworzenie instrukcji, które mają zastosowanie do każdego środowiska docelowego i dysk zawierający ustawienia kompilacji i wdrożenia określonego środowiska. W czasie kompilacji pliku projektu określonego środowiska jest scalany pliku projektu niezależny od środowiska pełny zestaw instrukcji kompilacji.

## <a name="task-overview"></a>Omówienie zadań

Aby przygotować serwer kompilacji do tworzenia i wdrażania rozwiązań, musisz:

- Zainstaluj i skonfiguruj usługę kompilacji TFS.
- Zainstaluj program Visual Studio 2010.
- Zainstaluj wszelkie produkty lub składniki, które są wymagane do utworzenia rozwiązania, takie jak wersji programu .NET Framework lub ASP.NET MVC.
- Zainstaluj narzędzie Web Deploy 2.0 lub nowszej.

W tym temacie opisano, jak wykonać te procedury lub wskaż inne zasoby, jeśli takie istnieją. Zadania i wskazówki, w tym temacie założono, że:

- Zaczynasz kompilację czystą serwera z systemem Windows Server 2008 R2 z dodatkiem Service Pack 1.
- Serwer jest przyłączony do domeny za pomocą statycznego adresu IP.
- Po zainstalowaniu warstwie aplikacji TFS na osobnym serwerze, zgodnie z opisem w [wdrożenia sieci Web w przedsiębiorstwie: omówienie scenariusza](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md).

### <a name="who-performs-these-procedures"></a>Kto wykonuje te procedury?

W większości przypadków administratora TFS będzie służyć do konfigurowania serwerów kompilacji. W niektórych przypadkach zespół deweloperów może przejąć na własność kompilacji określonych serwerów.

## <a name="install-and-configure-the-tfs-build-service"></a>Instalowanie i konfigurowanie usługi kompilacji TFS

Po skonfigurowaniu serwera kompilacji, najpierw jest aby zainstalować i skonfigurować usługę kompilacji TFS. W ramach tego procesu musisz:

- Zainstaluj usługę kompilacji TFS i konfigurowanie konta usługi. Wszystkie zadania kompilacji, w tym wdrażania, zostanie uruchomiony przy użyciu tożsamości konta usługi kompilacji.
- Utwórz *kontroler kompilacji* i co najmniej jeden *agentów kompilacji*. Każdy kontroler kompilacji zarządza zestawem agentów kompilacji. Gdy kolejka kompilacji, kontroler kompilacji przypisuje zadania kompilacji do agenta kompilacji dostępne. Każdej kolekcji projektów zespołowych w programie TFS jest mapowana na kontrolerze jednej kompilacji.
- Skonfiguruj folderu docelowego dla danych wyjściowych z kompilacji. Jest to w udziale sieciowym. Wszelkie wyniki, takie jak pakiety wdrażania web kompilacji, są wysyłane do folderu docelowego.

[Administrowanie Team Foundation Build](https://msdn.microsoft.com/library/ms252495.aspx) rozdziału w witrynie MSDN zawiera wszystkie zasoby potrzebne do wykonywania następujących zadań:

- Omówienie pojęć dotyczących Team Foundation Build, w tym usługi kompilacji, kontrolerów kompilacji i agentów kompilacji, zobacz [opis System kompilacji Team Foundation](https://msdn.microsoft.com/library/dd793166.aspx).
- Aby uzyskać informacje na temat instalowania i konfigurowania usługi kompilacji, zobacz [skonfigurować maszynę kompilacji](https://msdn.microsoft.com/library/ms181712.aspx).
- Aby uzyskać informacje dotyczące tworzenia kontrolerów kompilacji, zobacz [tworzenie i Praca z kontrolera kompilacji](https://msdn.microsoft.com/library/ee330987.aspx).
- Aby uzyskać informacje dotyczące tworzenia agentów kompilacji, zobacz [tworzenie i Praca z agentami kompilacji](https://msdn.microsoft.com/library/bb399135.aspx).
- Aby uzyskać informacje na temat tworzenia i konfigurowania folderach do wrzucania, zobacz [ustawić zapasową folderów porzucić](https://msdn.microsoft.com/library/bb778394.aspx).

## <a name="install-required-products-and-components"></a>Instalowanie wymaganych produktów i składników

Aby włączyć serwer kompilacji do tworzenia rozwiązań, należy zainstalować wszystkie produkty, składniki lub zestawy, które wymaga rozwiązania. Przed zainstalowaniem żadnych składników platformy sieci web należy zainstalować program Visual Studio 2010 (dowolna wersja) na serwerze kompilacji. Dzięki temu, że podstawowe pliki docelowy Microsoft kompilacji Engine (MSBuild) i pliki docelowe Web potok publikowania (WPP) są dostępne dla usługi kompilacji. Instalator programu Visual Studio należy również zainstalować narzędzie Web Deploy, które będą potrzebne, jeśli planujesz wdrożyć pakietów sieci web jako część procesu kompilacji.

Najlepszym sposobem instalowania wspólnych składników platformy sieci web jest użycie [Instalatora platformy sieci Web](https://go.microsoft.com/?linkid=9805118). Dzięki temu, że użytkownik instaluje najnowszą wersję każdego produktu, a także automatycznie wykrywa i instaluje wszystkie wymagania wstępne dla każdego produktu. W przypadku liczby [kontaktów Menedżerze](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) rozwiązanie, należy używać Instalatora platformy sieci Web do zainstalowania tych produktów i składników:

- **.NET Framework 4.0**. Jest to wymagane do uruchamiania aplikacji, które zostały utworzone w tej wersji programu .NET Framework.
- **Narzędzia Deployment Tool w wersji 2.1 lub nowszej w sieci Web**. Spowoduje to zainstalowanie narzędzia Web Deploy (i jego podstawowy plik wykonywalny MSDeploy.exe) na serwerze. W ramach tego procesu instaluje i uruchamia usługę sieci Web wdrażania agenta. Usługa ta umożliwia wdrażanie pakietów sieci web z komputera zdalnego.
- **ASP.NET MVC 3**. Spowoduje to zainstalowanie zestawów potrzebne do uruchamiania aplikacji ASP.NET MVC 3.

**Aby zainstalować wymagane produkty i składniki**

1. Zainstaluj program Visual Studio 2010. Po wyświetleniu monitu wybierz funkcje do zainstalowania, należy uwzględnić:

    1. Wszelkie języków programowania, które należy do kompilacji.
    2. Visual Web Developer. Dzięki temu, że elementy docelowe WPP są dodawane do serwera kompilacji.

        ![](configuring-a-tfs-build-server-for-web-deployment/_static/image1.png)
2. Po zakończeniu instalacji programu Visual Studio 2010, Pobierz i zainstaluj [dodatku Service Pack 1 dla programu Visual Studio 2010](https://go.microsoft.com/?linkid=9805133) (jeśli jeszcze nie należy ono do nośnika instalacyjnego programu).

    > [!NOTE]
    > Visual Studio 2010 z dodatkiem Service Pack 1 rozwiązuje usterki, które mogą uniemożliwić MSBuild Lokalizowanie pliku wykonywalnego MSDeploy.
3. Pobierz i uruchom [Instalatora platformy sieci Web](https://go.microsoft.com/?linkid=9805118).
4. W górnej części **3.0 Instalatora platformy sieci Web** okna, kliknij przycisk **produkty**.
5. Po lewej stronie okna, w okienku nawigacji kliknij **struktury**.
6. W **Microsoft .NET Framework 4** wiersz, jeśli nie zainstalowano jeszcze programu .NET Framework, kliknij przycisk **Dodaj**.

    > [!NOTE]
    > Być może został już zainstalowany programu .NET Framework 4.0 za pośrednictwem usługi Windows Update. Jeśli produktu lub składnik jest już zainstalowane, Instalator platformy sieci Web będzie tę informację, zastępując **Dodaj** button z tekstem **zainstalowana**.

    ![](configuring-a-tfs-build-server-for-web-deployment/_static/image2.png)
7. W **ASP.NET MVC 3 (Visual Studio 2010)** wiersz, kliknij przycisk **Dodaj**.
8. W okienku nawigacji kliknij **serwera**.
9. W **2.1 narzędzia wdrażania Web** wiersz, kliknij przycisk **Dodaj**.
10. Kliknij przycisk **zainstalować**. Instalator platformy sieci Web zostanie wyświetlona lista produktów&#x2014;oraz wszystkie skojarzone zależności&#x2014;do zainstalowania i wyświetli monit o zaakceptowanie postanowień licencyjnych.
11. Przejrzyj postanowienia licencyjne, a użytkownik wyraża zgodę na warunki, kliknij przycisk **akceptuję**.
12. Po zakończeniu instalacji kliknij przycisk **Zakończ**, a następnie Zamknij **3.0 Instalatora platformy sieci Web** okna.

> [!NOTE]
> Jeśli procesu wdrażania zawiera użyj narzędzi, takich jak VSDBCMD.exe lub SQLCMD.exe, należy upewnić się, że te pliki są zainstalowane na serwerze kompilacji. VSDBCMD.exe to narzędzie Visual Studio i zazwyczaj dodaje się do serwera podczas instalowania Team Foundation Build. SQLCMD.exe jest narzędziem do programu SQL Server. Możesz pobrać wersję autonomicznej SQLCMD.exe z [Microsoft SQL Server 2008 R2 Feature Pack](https://go.microsoft.com/?linkid=9805134) strony.


## <a name="conclusion"></a>Wniosek

W tym momencie serwer kompilacji jest gotowy do rozpoczęcia tworzenia i wdrażania projektów aplikacji sieci web. Następnym temacie [tworzenie kompilacji definicji czy obsługuje wdrożenia](creating-a-build-definition-that-supports-deployment.md), w tym artykule opisano sposób tworzenia i konfigurowania definicję kompilacji w celu kontrolowania, kiedy i jak projekty są wbudowane i wdrażane.

## <a name="further-reading"></a>Dalsze informacje

Aby uzyskać bardziej ogólne wskazówki dotyczące pracy z Team Build, zobacz [administrowanie Team Foundation Build](https://msdn.microsoft.com/library/ms252495.aspx).

> [!div class="step-by-step"]
> [Poprzednie](adding-content-to-source-control.md)
> [dalej](creating-a-build-definition-that-supports-deployment.md)
