---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-permissions-for-team-build-deployment
title: Wdrożenie kompilacji konfigurowania uprawnień dla zespołu | Dokumentacja firmy Microsoft
author: jrjlee
description: W tym temacie opisano sposób konfigurowania uprawnień do włączania na serwerze kompilacji w celu wdrażania zawartości serwerów sieci web i serwery baz danych jako część automatycznych b...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 2488a91e-b0a8-465a-b874-3233f724b56b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-permissions-for-team-build-deployment
msc.type: authoredcontent
ms.openlocfilehash: 4698349d664816ec49475bbfe71fb32af79ea96d
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/08/2018
ms.locfileid: "30890284"
---
<a name="configuring-permissions-for-team-build-deployment"></a>Konfigurowanie uprawnień dla zespołu wdrożenie kompilacji
====================
przez [Lewandowski Jason](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> W tym temacie opisano sposób konfigurowania uprawnień, aby włączyć serwer kompilacji w celu wdrażania zawartości serwerów sieci web i serwery baz danych jako część procesu automatycznego tworzenia.


Ten temat jest częścią serii samouczków na podstawie tych wymagań związanych z przedsiębiorstwa wdrażaniem fikcyjnej firmy o nazwie firmy Fabrikam, Inc. Przykładowe rozwiązanie korzysta z tego samouczka serii&#x2014; [rozwiązania kontaktów Menedżerze](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;do reprezentowania aplikacji sieci web z realistyczne poziom złożoności, w tym aplikacji ASP.NET MVC 3, Windows Communication Usługa Foundation (WCF), a projekt bazy danych.

Istotą te samouczki metody wdrażania opiera się na podejście pliku projektu podziału opisane w [opis pliku projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md), w którym jest kontrolowany przez proces kompilacji dwa pliki projektu&#x2014;jeden zawierający Tworzenie instrukcji, które mają zastosowanie do każdego środowiska docelowego i dysk zawierający ustawienia kompilacji i wdrożenia określonego środowiska. W czasie kompilacji pliku projektu określonego środowiska jest scalany pliku projektu niezależny od środowiska pełny zestaw instrukcji kompilacji.

## <a name="task-overview"></a>Omówienie zadań

Podczas instalowania usługi kompilacji 2010 Team Foundation Server (TFS), można określić tożsamości, z którym ma zostać usługę w celu uruchomienia. Domyślnie jest to konto Usługa sieciowa. Alternatywnie można skonfigurować usługę kompilacji do uruchamiania przy użyciu konta domeny.

Wszystkie zadania wdrażania, które wymagają uwierzytelniania systemu Windows i planujesz zautomatyzować za pomocą Team Build, zostanie uruchomiony przy użyciu tożsamości usługi kompilacji. Tak należy udzielić tożsamości usługi kompilacji wszelkich wymaganych uprawnień na serwerach bazy danych i serwerów sieci web.

> [!NOTE]
> Konto Usługa sieciowa używa konta komputera do uwierzytelniania na inne komputery. Konta komputera formę * [nazwa domeny]\[nazwa komputera] ***$**&#x2014;na przykład **FABRIKAM\TFSBUILD$**. Tak usługa kompilacji jest uruchomiona przy użyciu tożsamości Network Service, należy udzielić uprawnień wymaganych do tożsamość konta komputera dla serwera kompilacji.


## <a name="configuring-web-server-permissions"></a>Konfigurowanie uprawnień serwera sieci Web

Zgodnie z opisem w [Wybieranie podejście prawo do wdrożenia w sieci Web](../configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment.md), istnieją dwie metody main, można użyć, jeśli chcesz wdrożyć pakietów sieci web do zdalnego serwera:

- Wdrażanie aplikacji z lokalizacji zdalnej, wybierając *Usługa agenta sieci Web wdrożenia* (znanej także jako agenta zdalnego) na serwerze docelowym.
- Wdrażanie aplikacji z lokalizacji zdalnej, wybierając *Internetowe usługi informacyjne* (*usług IIS) program obsługi wdrażania w sieci Web* na serwerze docelowym.

Agent zdalnego ma dwa ograniczenia klucza w takim przypadku:

- Agent zdalnego obsługuje tylko uwierzytelnianie NTLM. Innymi słowy, wdrożenie musi używać tożsamości usługi kompilacji&#x2014;nie może spersonifikować innego konta.
- Aby używać zdalnego agenta, konto, które wykonuje wdrożenia musi być administratorem na serwerze docelowym.

Ze sobą te dwie ograniczenia uczynienia agenta zdalnego niepożądanych dla automatycznego wdrażania Team Build. Aby użyć tej metody, konieczne będzie konto administratora na wszystkich serwerach sieci web docelowy usługa kompilacji.

Z kolei podejście program obsługi wdrażania w sieci Web ma różne zalety:

- Obsługa wdrażania w sieci Web obsługuje uwierzytelnianie podstawowe, za pośrednictwem protokołu HTTPS, dzięki czemu można przekazać poświadczenia alternatywne konta do narzędzia wdrażania usług IIS sieci Web (Web Deploy).
- Można skonfigurować docelowych serwerów sieci web umożliwia użytkownikom niebędącym administratorami w celu wdrażania zawartości do określonych witryn internetowych usług IIS przy użyciu procedury obsługi wdrażania w sieci Web.

W związku z tym zaleca wyraźnie pod kątem obsługi wdrażania sieci Web podczas wdrażania pakietu sieci web z poziomu kompilacji zespołowej automatyzacji. Jest to zalecany proces:

1. Utwórz konto domeny o niskich uprawnieniach, które będzie używane dla wdrożenia.
2. Konfigurowanie obsługi wdrażania sieci Web i Przyznaj kontu uprawnienia wymagane do wdrażania zawartości do określonej witryny sieci Web usług IIS, zgodnie z opisem w [Konfigurowanie serwera sieci Web na potrzeby wdrażania publikowania w sieci Web (Obsługa wdrażania w sieci Web)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md).
3. Wywoływanie narzędzia Web Deploy i docelowa obsługi wdrażania w sieci Web, przy użyciu uwierzytelniania podstawowego i dostarczenie poświadczeń konta domeny został utworzony, aby wykonać wdrożenie.

W [kontaktów Menedżerze](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) przykładowe rozwiązanie, określ typ uwierzytelniania (podstawowe lub NTLM), poświadczenia narzędzia Web Deploy i adres punktu końcowego (agenta zdalnego lub program obsługi wdrażania w sieci Web) w pliku projektu określonego środowiska. Te wartości są używane do sformułować i uruchom polecenie Narzędzia Web Deploy, podczas wykonywania pliku projektu. Aby uzyskać więcej informacji, zobacz [wdrażanie pakietów sieci Web](../web-deployment-in-the-enterprise/deploying-web-packages.md).

Aby uzyskać więcej informacji na temat konfigurowania obsługi wdrażania w sieci Web, oraz o sposobie konfigurowania uprawnień, zobacz [Konfigurowanie serwera sieci Web na potrzeby wdrażania publikowania w sieci Web (Obsługa wdrażania w sieci Web)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md). Aby uzyskać więcej informacji na temat konfigurowania agenta zdalnego, zobacz [Konfigurowanie serwera sieci Web na potrzeby wdrażania publikowania w sieci Web (agenta zdalnego)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent.md).

## <a name="configuring-database-server-permissions"></a>Konfigurowanie uprawnień serwera bazy danych

Aby wdrożyć bazę danych programu SQL Server, należy:

- Utwórz dane logowania dla konta wdrażanie w wystąpieniu programu SQL Server.
- Przyznaj logowanie **DBCreator** uprawnienia w wystąpieniu programu SQL Server.
- Po początkowym wdrożeniu, Dodaj logowanie do **db\_właściciela** roli w docelowej bazie danych. Jest to wymagane, ponieważ na kolejne wdrożenia jest zmodyfikowanie istniejącej bazy danych zamiast tworzenia nowej bazy danych.

Można uwierzytelniać do wystąpienia programu SQL Server przy użyciu uwierzytelniania NTLM lub uwierzytelniania programu SQL Server:

- Jeśli używane jest uwierzytelnianie NTLM, należy udzielić uprawnień opisano powyżej, aby konto usługi kompilacji.
- Jeśli używasz uwierzytelniania programu SQL Server, należy udzielić uprawnień opisano powyżej, aby konto programu SQL Server. Należy również uwzględnić w ciągu połączenia używanego do wdrażania bazy danych programu SQL Server, nazwę użytkownika i hasło.

Aby uzyskać szczegółowe informacje krok po kroku dotyczące sposobu konfigurowania uprawnień dla wdrożenia bazy danych, zobacz [Konfigurowanie serwera bazy danych na potrzeby wdrażania publikowania w sieci Web](../configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing.md).

## <a name="conclusion"></a>Wniosek

W tym momencie należy zrozumieć uprawnień wymaganych, wraz z, Otwórz opcje uwierzytelniania podczas automatyzacji wdrożenia aplikacji i baz danych w sieci web z poziomu kompilacji zespołowej. Należy również możliwość wdrożenia odpowiednie uprawnienia na serwerach bazy danych programu SQL Server i serwery sieci web usług IIS.

## <a name="further-reading"></a>Dalsze informacje

Aby uzyskać więcej informacji na temat konfigurowania systemu Windows server w środowiskach do obsługi zdalnego wdrażania, zobacz [Konfigurowanie środowiska serwera sieci Web wdrożenia](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md).

> [!div class="step-by-step"]
> [Poprzednie](deploying-a-specific-build.md)
