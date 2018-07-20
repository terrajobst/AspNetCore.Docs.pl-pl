---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-permissions-for-team-build-deployment
title: Konfigurowanie uprawnień dla zespołu wdrożenia kompilacji | Dokumentacja firmy Microsoft
author: jrjlee
description: W tym temacie opisano sposób konfigurowania uprawnień, aby włączyć serwer kompilacji do wdrażania zawartości serwerów sieci web i serwery baz danych w ramach zautomatyzowanej b...
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: 2488a91e-b0a8-465a-b874-3233f724b56b
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-permissions-for-team-build-deployment
msc.type: authoredcontent
ms.openlocfilehash: b577d887b4a4476b6796ae9f1df538d16eededa3
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37820363"
---
<a name="configuring-permissions-for-team-build-deployment"></a>Konfigurowanie uprawnień dla zespołu wdrożenia kompilacji
====================
przez [Jason Lee](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> W tym temacie opisano sposób konfigurowania uprawnień, aby włączyć serwer kompilacji do wdrażania zawartości serwerów sieci web i serwery baz danych w ramach zautomatyzowanego procesu kompilacji.


Ten temat jest częścią serii samouczków na podstawie wymagania dotyczące wdrażania enterprise fikcyjnej firmy o nazwie firmy Fabrikam, Inc. Przykładowe rozwiązanie korzysta z tej serii samouczków&#x2014; [rozwiązania Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;do reprezentowania aplikacji sieci web przy użyciu realistycznej stopień złożoności, łącznie z aplikacją ASP.NET MVC 3 komunikacji Windows Usługa Foundation (WCF), a projekt bazy danych.

Metody wdrażania w ramach tego samouczka opiera się na podejście pliku projektu Podziel opisane w [objaśnienie pliku projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md), w którym proces kompilacji jest kontrolowana przez dwa pliki projektu&#x2014;jeden zawierający Tworzenie instrukcji, które mają zastosowanie do każdego środowiska docelowego i jeden zawierający ustawienia specyficzne dla środowiska kompilacji i wdrażania. W czasie kompilacji pliku projektu specyficznymi dla środowiska jest scalana w pliku projektu niezależnego od środowiska w celu utworzenia kompletny zestaw instrukcji kompilacji.

## <a name="task-overview"></a>Omówienie zadań

Po zainstalowaniu usługi kompilacji 2010 Team Foundation Server (TFS), możesz określić tożsamość za pomocą którego chcesz, aby usługa działała. Domyślnie to konto Usługa sieciowa. Alternatywnie można skonfigurować usługi kompilacji do uruchamiania przy użyciu konta domeny.

Wszystkie zadania wdrażania, które wymagają uwierzytelniania Windows i planujesz zautomatyzować za pomocą kompilacji zespołowej, uruchamiana, za pomocą tożsamości usługi kompilacji. Jako takie należy przyznać tożsamości usługi kompilacji wszelkich wymaganych uprawnień na serwerach sieci web i serwery bazy danych.

> [!NOTE]
> Konto Usługa sieciowa używa konta komputera do uwierzytelniania na innych komputerach. Konta komputerów mieć postać * [nazwa domeny]\[nazwa komputera] ***$**&#x2014;na przykład **FABRIKAM\TFSBUILD$**. Jako takie swoją usługę kompilacji jest uruchamiany, za pomocą tożsamości usługi sieciowej, należy udzielić wszystkie uprawnienia wymagane do tożsamość konta komputera dla serwera kompilacji.


## <a name="configuring-web-server-permissions"></a>Konfigurowanie uprawnień serwera sieci Web

Zgodnie z opisem w [Wybieranie podejścia prawo do wdrażania w Internecie](../configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment.md), dostępne są dwie główne opcje możesz użyć, jeśli chcesz wdrożyć pakietów sieci web do zdalnego serwera:

- Wdrażanie aplikacji z lokalizacji zdalnej, określając jako docelowe *Usługa agenta wdrażania sieci Web* (znany także jako agent zdalny) na serwerze docelowym.
- Wdrażanie aplikacji z lokalizacji zdalnej, określając jako docelowe *Internetowe usługi informacyjne* (*usług IIS) program obsługi wdrażania w sieci Web* na serwerze docelowym.

Agent zdalny ma dwa ograniczenia klucza w tym przypadku:

- Agent zdalny obsługuje tylko uwierzytelnianie NTLM. Innymi słowy, wdrożenia należy użyć tożsamości usługi kompilacji&#x2014;nie można spersonifikować innego konta.
- Aby użyć agenta zdalnego, konto które wykonuje wdrożenia musi być administratorem na serwerze docelowym.

Razem te dwa ograniczenia uczynienia agenta zdalnego niepożądane dla automatycznego wdrażania kompilacji zespołowej. Aby użyć tego podejścia, należałoby ułatwi zarządzanie usługą kompilacji, konto administratora na wszystkich serwerach sieci web docelowego.

Z kolei podejście program obsługi wdrażania w sieci Web oferuje różne korzyści:

- Program obsługi wdrażania sieci Web obsługuje uwierzytelnianie podstawowe, za pośrednictwem protokołu HTTPS, który umożliwia przekazywanie poświadczeń alternatywnych konta do narzędzia wdrażania Web usług IIS (Web Deploy).
- Można skonfigurować docelowych serwerów sieci web umożliwia użytkownikom niebędącym administratorami wdrażania zawartości do określonych witryn internetowych usług IIS przy użyciu procedury obsługi wdrażania sieci Web.

Dlatego zaleca się wyraźnie pod kątem obsługi wdrażania sieci Web podczas automatyzacji wdrożenia pakietu internetowego poziomu kompilacji zespołowej. Jest to zalecany proces:

1. Utwórz konto domeny z niskim poziomem uprawnień na potrzeby wdrożenia.
2. Skonfiguruj program obsługi wdrażania sieci Web i Przyznaj kontu uprawnienia wymagane do wdrażania zawartości określonej witryny sieci Web usług IIS, zgodnie z opisem w [Konfigurowanie serwera sieci Web na potrzeby wdrażania publikowania w sieci Web (Web wdrażanie obsługi)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md).
3. Wywoływanie narzędzia Web Deploy i docelowy program obsługi wdrażania sieci Web, przy użyciu uwierzytelniania podstawowego i dostarczenie poświadczeń konta domeny został utworzony, aby wykonać wdrożenie.

W [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) przykładowe rozwiązanie, należy określić typ uwierzytelniania (podstawowe lub NTLM), poświadczenia narzędzia Web Deploy i adres punktu końcowego (agent zdalny lub program obsługi wdrażania w sieci Web) w pliku projektu specyficznego dla danego środowiska. Te wartości są używane do sformułowania i uruchom polecenie Narzędzia Web Deploy, gdy plik projektu jest wykonywane. Aby uzyskać więcej informacji, zobacz [wdrażanie pakietów internetowych](../web-deployment-in-the-enterprise/deploying-web-packages.md).

Aby uzyskać więcej informacji na temat konfigurowania obsługi wdrażania sieci Web, w tym sposób konfigurowania uprawnień, zobacz [Konfigurowanie serwera sieci Web na potrzeby wdrażania publikowania w sieci Web (Web wdrażanie obsługi)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md). Aby uzyskać więcej informacji na temat konfigurowania agenta zdalnego, zobacz [Konfigurowanie serwera sieci Web na potrzeby wdrażania publikowania w sieci Web (Agent zdalny)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent.md).

## <a name="configuring-database-server-permissions"></a>Konfigurowanie uprawnień serwera bazy danych

Aby wdrożyć bazę danych programu SQL Server, musisz mieć:

- Utwórz identyfikator logowania dla konta wdrażanie w wystąpieniu programu SQL Server.
- Przyznaj logowania **DBCreator** uprawnienia w wystąpieniu programu SQL Server.
- Po początkowym wdrożeniu należy dodać logowanie do **db\_właściciela** roli w docelowej bazie danych. Jest to wymagane, ponieważ na kolejne wdrożenia jest zmodyfikowanie istniejącej bazy danych zamiast tworzenia nowej bazy danych.

Istnieje możliwość uwierzytelnienia się wystąpienia programu SQL Server przy użyciu uwierzytelniania NTLM lub uwierzytelniania programu SQL Server:

- Jeśli używasz uwierzytelniania NTLM, należy przyznać uprawnienia opisane powyżej, aby konto usługi kompilacji.
- Jeśli używasz uwierzytelniania programu SQL Server, należy przyznać uprawnienia opisane powyżej do konta programu SQL Server. Należy również uwzględnić w parametrach połączenia, który zostanie użyty do wdrożenia bazy danych programu SQL Server, nazwę użytkownika i hasło.

Aby uzyskać szczegółowe informacje krok po kroku dotyczące sposobu konfigurowania uprawnień do wdrożenia bazy danych, zobacz [Konfigurowanie serwera bazy danych na potrzeby wdrażania publikowania w sieci Web](../configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing.md).

## <a name="conclusion"></a>Wniosek

W tym momencie należy zrozumieć uprawnienia wymagane wraz z opcji uwierzytelniania, Otwórz, gdy Automatyzowanie wdrożeń aplikacji i baz danych w sieci web poziomu kompilacji zespołowej. Należy również możliwość wdrożenia wymaganych uprawnień na serwerach sieci web usług IIS i serwery baz danych programu SQL Server.

## <a name="further-reading"></a>Dalsze informacje

Aby uzyskać więcej informacji na temat konfigurowania środowisk serwera Windows do obsługi zdalnego wdrażania, zobacz [Konfigurowanie środowisk serwera wdrażania sieci Web](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md).

> [!div class="step-by-step"]
> [Poprzednie](deploying-a-specific-build.md)
