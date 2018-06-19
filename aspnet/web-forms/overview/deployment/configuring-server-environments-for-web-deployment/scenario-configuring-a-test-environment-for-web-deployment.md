---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-test-environment-for-web-deployment
title: 'Scenariusz: Konfigurowanie środowiska testowego na potrzeby wdrażania w sieci Web | Dokumentacja firmy Microsoft'
author: jrjlee
description: W tym temacie opisano scenariusz wdrażania web typowa dla deweloperów lub środowisk testowania i opisano zadania, które należy wykonać, aby skonfigurować si...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 44a22ac7-1fc7-4174-b946-c6129fb6a19b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-test-environment-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 2976be642815e715ac19bd9db34485cf5474cb32
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879858"
---
<a name="scenario-configuring-a-test-environment-for-web-deployment"></a><span data-ttu-id="4db55-103">Scenariusz: Konfigurowanie środowiska testowego na potrzeby wdrażania w sieci Web</span><span class="sxs-lookup"><span data-stu-id="4db55-103">Scenario: Configuring a Test Environment for Web Deployment</span></span>
====================
<span data-ttu-id="4db55-104">przez [Lewandowski Jason](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="4db55-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="4db55-105">Pobierz plik PDF</span><span class="sxs-lookup"><span data-stu-id="4db55-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="4db55-106">W tym temacie opisano scenariusz wdrażania web typowa dla deweloperów lub środowisk testowania i opisano zadania, które należy wykonać, aby skonfigurować środowisko podobne.</span><span class="sxs-lookup"><span data-stu-id="4db55-106">This topic describes a typical web deployment scenario for developer or test environments and explains the tasks you need to complete in order to set up a similar environment.</span></span>


<span data-ttu-id="4db55-107">Gdy deweloperzy pracują w aplikacjach sieci web, ich często otrzymuje dostęp do środowiska serwera, które mają zostać przetestowane zmiany ich aplikacji w ustawieniu realistyczne mogą przy użyciu.</span><span class="sxs-lookup"><span data-stu-id="4db55-107">When developers work on web applications, they're often given access to a server environment that they can use to test changes to their applications in a realistic setting.</span></span> <span data-ttu-id="4db55-108">Tego rodzaju środowisko rozwoju lub testowania zwykle ma następujące cechy:</span><span class="sxs-lookup"><span data-stu-id="4db55-108">This kind of development or test environment typically has these characteristics:</span></span>

- <span data-ttu-id="4db55-109">Środowisko składa się z jednym serwerze sieci web a serwerem pojedynczej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="4db55-109">The environment consists of a single web server and a single database server.</span></span>
- <span data-ttu-id="4db55-110">Deweloperzy zazwyczaj są uprawnienia administratora na serwerach, w celu umożliwienia im skonfiguruj środowisko do wymagań aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4db55-110">The developers usually have administrator privileges on the servers, to let them configure the environment to the requirements of their applications.</span></span>
- <span data-ttu-id="4db55-111">Zmiany do aplikacji są wdrażane na podstawie często, dlatego środowisko musi obsługiwać pojedynczy krok lub automatycznego wdrażania.</span><span class="sxs-lookup"><span data-stu-id="4db55-111">Changes to applications are deployed on a frequent basis, so the environment needs to support single-step or automated deployment.</span></span>

<span data-ttu-id="4db55-112">Na przykład w naszym [samouczka scenariusza](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md), Matt Hink jest Deweloper pracujący u firmy Fabrikam, Inc. Matt działa w ramach rozwiązania kontaktów Menedżerze i regularnie konieczne wdrażanie zmiany do środowiska testowego.</span><span class="sxs-lookup"><span data-stu-id="4db55-112">For example, in our [tutorial scenario](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md), Matt Hink is a developer at Fabrikam, Inc. Matt is working on the Contact Manager solution and regularly needs to deploy changes to a test environment.</span></span> <span data-ttu-id="4db55-113">Matt jest administratorem na serwerze sieci web testów i na serwerze bazy danych testu.</span><span class="sxs-lookup"><span data-stu-id="4db55-113">Matt is an administrator on the test web server and the test database server.</span></span> <span data-ttu-id="4db55-114">Początkowo Matt musi mieć możliwość wdrażania rozwiązania bezpośrednio do środowiska testowego.</span><span class="sxs-lookup"><span data-stu-id="4db55-114">Initially, Matt needs to be able to deploy the solution to the test environment directly.</span></span>

![](scenario-configuring-a-test-environment-for-web-deployment/_static/image1.png)

<span data-ttu-id="4db55-115">Jako pracy realizowany i deweloperów więcej przyłączyć zespołu menedżera skontaktuj się z rozwiązania jest skonfigurowany dla ciągłej integracji (CI) w Team Foundation Server (TFS).</span><span class="sxs-lookup"><span data-stu-id="4db55-115">As work progresses and more developers join the team, the Contact Manager solution is configured for continuous integration (CI) in Team Foundation Server (TFS).</span></span> <span data-ttu-id="4db55-116">Zawsze, gdy projektant sprawdza w zawartości, Team Build powinien Skompiluj rozwiązanie, uruchom wszystkie testy jednostkowe i automatyczne wdrażanie rozwiązania do środowiska testowego.</span><span class="sxs-lookup"><span data-stu-id="4db55-116">Whenever a developer checks in content, Team Build should build the solution, run any unit tests, and automatically deploy the solution to the test environment.</span></span>

![](scenario-configuring-a-test-environment-for-web-deployment/_static/image2.png)

## <a name="solution-overview"></a><span data-ttu-id="4db55-117">Omówienie rozwiązania</span><span class="sxs-lookup"><span data-stu-id="4db55-117">Solution Overview</span></span>

<span data-ttu-id="4db55-118">Środowisko testowe musi obsługiwać pojedynczy krok lub zautomatyzowane wdrażanie na komputerze zdalnym, dlatego należy wybrać dwa podejścia głównego.</span><span class="sxs-lookup"><span data-stu-id="4db55-118">The test environment needs to support single-step or automated deployment from a remote computer, so you have a choice of two main approaches.</span></span> <span data-ttu-id="4db55-119">Można:</span><span class="sxs-lookup"><span data-stu-id="4db55-119">You can:</span></span>

- <span data-ttu-id="4db55-120">Skonfiguruj serwer testu sieci web do obsługi wdrożenia przy użyciu usługi sieci Web wdrożenia agenta ("agent zdalnego").</span><span class="sxs-lookup"><span data-stu-id="4db55-120">Configure the test web server to support deployment using the Web Deployment Agent Service (the "remote agent").</span></span>
- <span data-ttu-id="4db55-121">Skonfiguruj serwer sieci web testów do obsługi wdrożenia przy użyciu procedury obsługi narzędzia Web Deploy.</span><span class="sxs-lookup"><span data-stu-id="4db55-121">Configure the test web server to support deployment using the Web Deploy handler.</span></span>

> [!NOTE]
> <span data-ttu-id="4db55-122">Można także użyć [sieci Web wdrażanie na żądanie](https://technet.microsoft.com/library/ee517345(WS.10).aspx) ("agent tymczasowego").</span><span class="sxs-lookup"><span data-stu-id="4db55-122">You could also use [Web Deploy On Demand](https://technet.microsoft.com/library/ee517345(WS.10).aspx) (the "temp agent").</span></span> <span data-ttu-id="4db55-123">To jest podobna do metody zdalnego agenta pod względem wymagań i ograniczeń.</span><span class="sxs-lookup"><span data-stu-id="4db55-123">This is similar to the remote agent approach in terms of requirements and constraints.</span></span>


<span data-ttu-id="4db55-124">W takim przypadku deweloperzy mają uprawnienia administratora na serwerze docelowym, a środowisko testowe nie podlega ograniczenia ograniczeniami zabezpieczeń, wybór logicznej jest skonfigurowanie testu serwera sieci web do obsługi wdrożenia przy użyciu agenta zdalnego.</span><span class="sxs-lookup"><span data-stu-id="4db55-124">In this case, the developers have administrator privileges on the destination servers, and the test environment is not subject to strict security constraints, so the logical choice is to configure the test web server to support deployment using the remote agent.</span></span> <span data-ttu-id="4db55-125">To jest mniej złożona i wymaga mniej początkowej konfiguracji niż podejście program obsługi wdrażania w sieci Web.</span><span class="sxs-lookup"><span data-stu-id="4db55-125">This is less complex and requires less initial configuration than the Web Deploy Handler approach.</span></span> <span data-ttu-id="4db55-126">Należy także skonfigurować serwer bazy danych do obsługi dostępu zdalnego i wdrażania.</span><span class="sxs-lookup"><span data-stu-id="4db55-126">You'll also need to configure your database server to support remote access and deployment.</span></span>

<span data-ttu-id="4db55-127">Te tematy zawierają wszystkie informacje potrzebne do wykonania tych zadań:</span><span class="sxs-lookup"><span data-stu-id="4db55-127">These topics provide all the information you need in order to complete these tasks:</span></span>

- <span data-ttu-id="4db55-128">[Konfigurowanie serwera sieci Web narzędzia Web Deploy publikowania (agenta zdalnego)](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md).</span><span class="sxs-lookup"><span data-stu-id="4db55-128">[Configure a Web Server for Web Deploy Publishing (Remote Agent)](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md).</span></span> <span data-ttu-id="4db55-129">W tym temacie opisano sposób tworzenia serwera sieci web, który obsługuje narzędzia Web Deploy publikowania, stosując metodę agenta zdalnego od czystą kompilacji systemu Windows Server 2008 R2.</span><span class="sxs-lookup"><span data-stu-id="4db55-129">This topic describes how to build a web server that supports Web Deploy publishing, using the remote agent approach, starting from a clean Windows Server 2008 R2 build.</span></span>
- <span data-ttu-id="4db55-130">[Konfigurowanie serwera bazy danych dla narzędzia Web Deploy publikowanie](configuring-a-database-server-for-web-deploy-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="4db55-130">[Configure a Database Server for Web Deploy Publishing](configuring-a-database-server-for-web-deploy-publishing.md).</span></span> <span data-ttu-id="4db55-131">W tym temacie opisano sposób konfigurowania serwera bazy danych do obsługi dostępu zdalnego i wdrażania, zaczynając od domyślnej instalacji programu SQL Server 2008 R2.</span><span class="sxs-lookup"><span data-stu-id="4db55-131">This topic describes how to configure a database server to support remote access and deployment, starting from a default installation of SQL Server 2008 R2.</span></span>

## <a name="further-reading"></a><span data-ttu-id="4db55-132">Dalsze informacje</span><span class="sxs-lookup"><span data-stu-id="4db55-132">Further Reading</span></span>

<span data-ttu-id="4db55-133">Aby uzyskać wskazówki dotyczące konfigurowania typowe środowisko przejściowe, zobacz [scenariusz: Konfigurowanie środowisku przemieszczania Web Deployment](scenario-configuring-a-staging-environment-for-web-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="4db55-133">For guidance on configuring a typical staging environment, see [Scenario: Configuring a Staging Environment for Web Deployment](scenario-configuring-a-staging-environment-for-web-deployment.md).</span></span> <span data-ttu-id="4db55-134">Aby uzyskać wskazówki dotyczące konfigurowania środowiska produkcji, zobacz [scenariusz: Konfigurowanie środowiska produkcyjnego Web Deployment](scenario-configuring-a-production-environment-for-web-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="4db55-134">For guidance on configuring a typical production environment, see [Scenario: Configuring a Production Environment for Web Deployment](scenario-configuring-a-production-environment-for-web-deployment.md).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="4db55-135">[Poprzednie](choosing-the-right-approach-to-web-deployment.md)
> [dalej](scenario-configuring-a-staging-environment-for-web-deployment.md)</span><span class="sxs-lookup"><span data-stu-id="4db55-135">[Previous](choosing-the-right-approach-to-web-deployment.md)
[Next](scenario-configuring-a-staging-environment-for-web-deployment.md)</span></span>
