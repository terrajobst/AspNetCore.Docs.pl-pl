---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-test-environment-for-web-deployment
title: 'Scenariusz: Konfigurowanie środowiska testowego na potrzeby wdrażania w Internecie | Dokumentacja firmy Microsoft'
author: jrjlee
description: W tym temacie opisano scenariusz wdrażania web typowe dla dewelopera lub środowisk testowych i opisano zadania, które należy wykonać, aby skonfigurować zdarzenia serwisowego...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 44a22ac7-1fc7-4174-b946-c6129fb6a19b
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-test-environment-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: f8636fab82b63edab50fc13ae32f4dd536f133ff
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37382497"
---
<a name="scenario-configuring-a-test-environment-for-web-deployment"></a><span data-ttu-id="5f13c-103">Scenariusz: Konfigurowanie środowiska testowego na potrzeby wdrażania w Internecie</span><span class="sxs-lookup"><span data-stu-id="5f13c-103">Scenario: Configuring a Test Environment for Web Deployment</span></span>
====================
<span data-ttu-id="5f13c-104">przez [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="5f13c-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="5f13c-105">Pobierz plik PDF</span><span class="sxs-lookup"><span data-stu-id="5f13c-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="5f13c-106">W tym temacie opisano scenariusz wdrażania web typowe dla dewelopera lub środowisk testowych i opisano zadania, które należy wykonać, aby można było skonfigurować podobnie środowisko.</span><span class="sxs-lookup"><span data-stu-id="5f13c-106">This topic describes a typical web deployment scenario for developer or test environments and explains the tasks you need to complete in order to set up a similar environment.</span></span>


<span data-ttu-id="5f13c-107">Gdy deweloperzy pracują w aplikacjach sieci web, często mają one dostęp do środowiska serwera, który może służyć do testowania zmian w aplikacjach w ustawieniu realistyczne.</span><span class="sxs-lookup"><span data-stu-id="5f13c-107">When developers work on web applications, they're often given access to a server environment that they can use to test changes to their applications in a realistic setting.</span></span> <span data-ttu-id="5f13c-108">Tego rodzaju środowiska deweloperskie lub testowe zazwyczaj ma następujące cechy:</span><span class="sxs-lookup"><span data-stu-id="5f13c-108">This kind of development or test environment typically has these characteristics:</span></span>

- <span data-ttu-id="5f13c-109">Środowisko składa się z jednym serwerze sieci web a serwerem pojedynczej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="5f13c-109">The environment consists of a single web server and a single database server.</span></span>
- <span data-ttu-id="5f13c-110">Deweloperzy zazwyczaj mieć uprawnienia administratora na serwerach, aby umożliwić im skonfiguruj środowisko do wymagań aplikacji.</span><span class="sxs-lookup"><span data-stu-id="5f13c-110">The developers usually have administrator privileges on the servers, to let them configure the environment to the requirements of their applications.</span></span>
- <span data-ttu-id="5f13c-111">Zmiany w aplikacji są wdrażane na podstawie często, dlatego środowisko musi obsługiwać pojedynczy krok lub automatycznego wdrażania.</span><span class="sxs-lookup"><span data-stu-id="5f13c-111">Changes to applications are deployed on a frequent basis, so the environment needs to support single-step or automated deployment.</span></span>

<span data-ttu-id="5f13c-112">Na przykład w naszym [scenariusz samouczka](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md), Matt Hink jest programistą w firmie Fabrikam, Inc. Matt pracuje nad rozwiązania Contact Manager i regularnie musi wdrożyć zmiany w środowisku testowym.</span><span class="sxs-lookup"><span data-stu-id="5f13c-112">For example, in our [tutorial scenario](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md), Matt Hink is a developer at Fabrikam, Inc. Matt is working on the Contact Manager solution and regularly needs to deploy changes to a test environment.</span></span> <span data-ttu-id="5f13c-113">Matt jest administratorem na serwerze sieci web test i serwera bazy danych testów.</span><span class="sxs-lookup"><span data-stu-id="5f13c-113">Matt is an administrator on the test web server and the test database server.</span></span> <span data-ttu-id="5f13c-114">Początkowo Matt musi mieć możliwość wdrażania rozwiązania do środowiska testowego bezpośrednio.</span><span class="sxs-lookup"><span data-stu-id="5f13c-114">Initially, Matt needs to be able to deploy the solution to the test environment directly.</span></span>

![](scenario-configuring-a-test-environment-for-web-deployment/_static/image1.png)

<span data-ttu-id="5f13c-115">Jako praca postępów i liczby deweloperów, Dołącz do zespołu, Contact Manager rozwiązania jest skonfigurowany w celu zapewnienia ciągłej integracji (CI) w Team Foundation Server (TFS).</span><span class="sxs-lookup"><span data-stu-id="5f13c-115">As work progresses and more developers join the team, the Contact Manager solution is configured for continuous integration (CI) in Team Foundation Server (TFS).</span></span> <span data-ttu-id="5f13c-116">Zawsze, gdy programista zaewidencjonuje zawartości, Team Build powinien Skompiluj rozwiązanie, testy jednostkowe i automatycznie wdrażać rozwiązania do środowiska testowego.</span><span class="sxs-lookup"><span data-stu-id="5f13c-116">Whenever a developer checks in content, Team Build should build the solution, run any unit tests, and automatically deploy the solution to the test environment.</span></span>

![](scenario-configuring-a-test-environment-for-web-deployment/_static/image2.png)

## <a name="solution-overview"></a><span data-ttu-id="5f13c-117">Omówienie rozwiązania</span><span class="sxs-lookup"><span data-stu-id="5f13c-117">Solution Overview</span></span>

<span data-ttu-id="5f13c-118">Środowisko testowe musi obsługiwać pojedynczy krok lub automatycznego wdrażania z komputera zdalnego, dzięki czemu masz do wyboru dwie główne metody.</span><span class="sxs-lookup"><span data-stu-id="5f13c-118">The test environment needs to support single-step or automated deployment from a remote computer, so you have a choice of two main approaches.</span></span> <span data-ttu-id="5f13c-119">Można:</span><span class="sxs-lookup"><span data-stu-id="5f13c-119">You can:</span></span>

- <span data-ttu-id="5f13c-120">Skonfiguruj serwer sieci web test do obsługi wdrożenia przy użyciu usługi agenta wdrażania sieci Web ("agent zdalny").</span><span class="sxs-lookup"><span data-stu-id="5f13c-120">Configure the test web server to support deployment using the Web Deployment Agent Service (the "remote agent").</span></span>
- <span data-ttu-id="5f13c-121">Skonfiguruj serwer sieci web test do obsługi wdrożenia przy użyciu procedury obsługi narzędzia Web Deploy.</span><span class="sxs-lookup"><span data-stu-id="5f13c-121">Configure the test web server to support deployment using the Web Deploy handler.</span></span>

> [!NOTE]
> <span data-ttu-id="5f13c-122">Można także użyć [sieci Web wdrażanie na żądanie](https://technet.microsoft.com/library/ee517345(WS.10).aspx) ("temp agent").</span><span class="sxs-lookup"><span data-stu-id="5f13c-122">You could also use [Web Deploy On Demand](https://technet.microsoft.com/library/ee517345(WS.10).aspx) (the "temp agent").</span></span> <span data-ttu-id="5f13c-123">Jest to podobne do metody zdalnego agenta pod kątem wymagań i ograniczeń.</span><span class="sxs-lookup"><span data-stu-id="5f13c-123">This is similar to the remote agent approach in terms of requirements and constraints.</span></span>


<span data-ttu-id="5f13c-124">W tym przypadku deweloperzy mają uprawnienia administratora na serwerach docelowych, a środowisko testowe nie podlega ograniczenia ścisłymi zasadami kontrolnymi zabezpieczeń, więc logicznym wyborem jest skonfigurowanie serwera sieci web test do obsługi wdrożenia przy użyciu agenta zdalnego.</span><span class="sxs-lookup"><span data-stu-id="5f13c-124">In this case, the developers have administrator privileges on the destination servers, and the test environment is not subject to strict security constraints, so the logical choice is to configure the test web server to support deployment using the remote agent.</span></span> <span data-ttu-id="5f13c-125">Jest to mniej skomplikowane i wymaga mniej początkowej konfiguracji niż podejście program obsługi wdrażania w sieci Web.</span><span class="sxs-lookup"><span data-stu-id="5f13c-125">This is less complex and requires less initial configuration than the Web Deploy Handler approach.</span></span> <span data-ttu-id="5f13c-126">Należy również skonfigurować serwer bazy danych do obsługi dostępu zdalnego i wdrażania.</span><span class="sxs-lookup"><span data-stu-id="5f13c-126">You'll also need to configure your database server to support remote access and deployment.</span></span>

<span data-ttu-id="5f13c-127">Te tematy zawierają wszystkie informacje potrzebne do wykonania tych zadań:</span><span class="sxs-lookup"><span data-stu-id="5f13c-127">These topics provide all the information you need in order to complete these tasks:</span></span>

- <span data-ttu-id="5f13c-128">[Konfigurowanie serwera sieci Web dla usługi Web Deploy (Agent zdalny) publikowanie](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md).</span><span class="sxs-lookup"><span data-stu-id="5f13c-128">[Configure a Web Server for Web Deploy Publishing (Remote Agent)](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md).</span></span> <span data-ttu-id="5f13c-129">W tym temacie opisano sposób tworzenia serwera sieci web, który obsługuje narzędzie Web Deploy publikowania, przy użyciu metody agenta zdalnego od czysta kompilacja systemu Windows Server 2008 R2.</span><span class="sxs-lookup"><span data-stu-id="5f13c-129">This topic describes how to build a web server that supports Web Deploy publishing, using the remote agent approach, starting from a clean Windows Server 2008 R2 build.</span></span>
- <span data-ttu-id="5f13c-130">[Konfigurowanie serwera bazy danych dla publikowania Web Deploy](configuring-a-database-server-for-web-deploy-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="5f13c-130">[Configure a Database Server for Web Deploy Publishing](configuring-a-database-server-for-web-deploy-publishing.md).</span></span> <span data-ttu-id="5f13c-131">W tym temacie opisano sposób konfigurowania serwera bazy danych do obsługi dostępu zdalnego i wdrażania, zaczynając od domyślnej instalacji programu SQL Server 2008 R2.</span><span class="sxs-lookup"><span data-stu-id="5f13c-131">This topic describes how to configure a database server to support remote access and deployment, starting from a default installation of SQL Server 2008 R2.</span></span>

## <a name="further-reading"></a><span data-ttu-id="5f13c-132">Dalsze informacje</span><span class="sxs-lookup"><span data-stu-id="5f13c-132">Further Reading</span></span>

<span data-ttu-id="5f13c-133">Aby uzyskać wskazówki na temat konfigurowania typowe środowisko przejściowe, zobacz [scenariusz: Konfigurowanie środowiska przejściowego na potrzeby wdrażania w Internecie](scenario-configuring-a-staging-environment-for-web-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="5f13c-133">For guidance on configuring a typical staging environment, see [Scenario: Configuring a Staging Environment for Web Deployment](scenario-configuring-a-staging-environment-for-web-deployment.md).</span></span> <span data-ttu-id="5f13c-134">Aby uzyskać wskazówki na temat konfigurowania środowiska produkcji typowych, zobacz [scenariusz: Konfigurowanie środowiska produkcyjnego na potrzeby wdrażania w Internecie](scenario-configuring-a-production-environment-for-web-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="5f13c-134">For guidance on configuring a typical production environment, see [Scenario: Configuring a Production Environment for Web Deployment](scenario-configuring-a-production-environment-for-web-deployment.md).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="5f13c-135">[Poprzednie](choosing-the-right-approach-to-web-deployment.md)
> [dalej](scenario-configuring-a-staging-environment-for-web-deployment.md)</span><span class="sxs-lookup"><span data-stu-id="5f13c-135">[Previous](choosing-the-right-approach-to-web-deployment.md)
[Next](scenario-configuring-a-staging-environment-for-web-deployment.md)</span></span>
