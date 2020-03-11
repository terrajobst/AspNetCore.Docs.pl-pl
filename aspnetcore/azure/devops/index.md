---
title: Metodyka DevOps z platformami ASP.NET Core i Azure
author: CamSoper
description: Przewodnik, który dostarcza wskazówki end-to-end na tworzeniu potoku metodyki DevOps dla aplikacji ASP.NET Core hostowanych na platformie Azure.
ms.author: casoper
ms.date: 08/07/2018
ms.custom: mvc, seodec18
uid: azure/devops/index
ms.openlocfilehash: f45bb2a5dd4b3d1a820085ede7ce3219045ed80b
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78658082"
---
# <a name="devops-with-aspnet-core-and-azure"></a><span data-ttu-id="84e58-103">Metodyka DevOps z platformami ASP.NET Core i Azure</span><span class="sxs-lookup"><span data-stu-id="84e58-103">DevOps with ASP.NET Core and Azure</span></span>

<span data-ttu-id="84e58-104">[![obrazu okładki](./media/cover-large.png)](https://aka.ms/devopsbook)</span><span class="sxs-lookup"><span data-stu-id="84e58-104">[![Cover Image](./media/cover-large.png)](https://aka.ms/devopsbook)</span></span>

<span data-ttu-id="84e58-105">Według [Soper](https://twitter.com/camsoper) i [Scott Addie](https://twitter.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="84e58-105">By [Cam Soper](https://twitter.com/camsoper) and [Scott Addie](https://twitter.com/scottaddie)</span></span>

<span data-ttu-id="84e58-106">Ten przewodnik jest dostępny jako [książka elektroniczna do pobrania](https://aka.ms/devopsbook).</span><span class="sxs-lookup"><span data-stu-id="84e58-106">This guide is available as a [downloadable PDF e-book](https://aka.ms/devopsbook).</span></span>

## <a name="welcome"></a><span data-ttu-id="84e58-107">Powitanie</span><span class="sxs-lookup"><span data-stu-id="84e58-107">Welcome</span></span> 

<span data-ttu-id="84e58-108">Przewodnik cyklu tworzenia oprogramowania platformy Azure dla platformy .NET — Zapraszamy!</span><span class="sxs-lookup"><span data-stu-id="84e58-108">Welcome to the Azure Development Lifecycle guide for .NET!</span></span> <span data-ttu-id="84e58-109">Ten przewodnik wprowadzenie podstawowych pojęć dotyczących tworzenia cyklu życia na platformie Azure przy użyciu platformy .NET, narzędziami i procesami.</span><span class="sxs-lookup"><span data-stu-id="84e58-109">This guide introduces the basic concepts of building a development lifecycle around Azure using .NET tools and processes.</span></span> <span data-ttu-id="84e58-110">Po zakończeniu tego przewodnika, możesz czerpać korzyści dojrzała łańcucha narzędzi DevOps.</span><span class="sxs-lookup"><span data-stu-id="84e58-110">After finishing this guide, you'll reap the benefits of a mature DevOps toolchain.</span></span>

## <a name="who-this-guide-is-for"></a><span data-ttu-id="84e58-111">Kto ten przewodnik jest przeznaczony dla</span><span class="sxs-lookup"><span data-stu-id="84e58-111">Who this guide is for</span></span>

<span data-ttu-id="84e58-112">Powinno być doświadczonym deweloperom platformy ASP.NET Core (poziom 200-300).</span><span class="sxs-lookup"><span data-stu-id="84e58-112">You should be an experienced ASP.NET Core developer (200-300 level).</span></span> <span data-ttu-id="84e58-113">Nie trzeba nic wiedzieć o platformie Azure, jak omówimy to w ramach tego wprowadzenia.</span><span class="sxs-lookup"><span data-stu-id="84e58-113">You don't need to know anything about Azure, as we'll cover that in this introduction.</span></span> <span data-ttu-id="84e58-114">Ten przewodnik może być również przydatne w przypadku inżynierom DevOps, którzy są bardziej skupia się na operacje niż rozwoju.</span><span class="sxs-lookup"><span data-stu-id="84e58-114">This guide may also be useful for DevOps engineers who are more focused on operations than development.</span></span>

<span data-ttu-id="84e58-115">Ten przewodnik jest przeznaczony dla deweloperów Windows.</span><span class="sxs-lookup"><span data-stu-id="84e58-115">This guide targets Windows developers.</span></span> <span data-ttu-id="84e58-116">Jednak z systemami Linux i macOS są w pełni obsługiwane przez .NET Core.</span><span class="sxs-lookup"><span data-stu-id="84e58-116">However, Linux and macOS are fully supported by .NET Core.</span></span> <span data-ttu-id="84e58-117">Można dostosować ten przewodnik dla systemu Linux/macOS, obserwuj objaśnienia na różnice Linux/macOS.</span><span class="sxs-lookup"><span data-stu-id="84e58-117">To adapt this guide for Linux/macOS, watch for callouts for Linux/macOS differences.</span></span>

## <a name="what-this-guide-doesnt-cover"></a><span data-ttu-id="84e58-118">Co ten przewodnik nie obejmuje</span><span class="sxs-lookup"><span data-stu-id="84e58-118">What this guide doesn't cover</span></span>

<span data-ttu-id="84e58-119">Ten przewodnik koncentruje się na środowisko end-to-end ciągłego wdrażania dla deweloperów platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="84e58-119">This guide is focused on an end-to-end continuous deployment experience for .NET developers.</span></span> <span data-ttu-id="84e58-120">Nie jest wyczerpująca przewodnik wszystkimi aspektami platformy Azure, a jej nie skupić często interfejsów API platformy .NET dla usług platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="84e58-120">It's not an exhaustive guide to all things Azure, and it doesn't focus extensively on .NET APIs for Azure services.</span></span> <span data-ttu-id="84e58-121">Jest nacisk na całym ciągłej integracji i wdrażania, monitorowania i debugowania.</span><span class="sxs-lookup"><span data-stu-id="84e58-121">The emphasis is all around continuous integration, deployment, monitoring, and debugging.</span></span> <span data-ttu-id="84e58-122">Pod koniec przewodnika są oferowane zalecenia dotyczące następnych kroków.</span><span class="sxs-lookup"><span data-stu-id="84e58-122">Near the end of the guide, recommendations for next steps are offered.</span></span> <span data-ttu-id="84e58-123">Sugestie obejmuje usług platformy Azure, które są przydatne dla deweloperów platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="84e58-123">Included in the suggestions are Azure platform services that are useful to ASP.NET Core developers.</span></span>

## <a name="whats-in-this-guide"></a><span data-ttu-id="84e58-124">Co to jest w tym przewodniku</span><span class="sxs-lookup"><span data-stu-id="84e58-124">What's in this guide</span></span>

### <a name="tools-and-downloads"></a>[<span data-ttu-id="84e58-125">Narzędzia i pliki do pobrania</span><span class="sxs-lookup"><span data-stu-id="84e58-125">Tools and downloads</span></span>](xref:azure/devops/tools-and-downloads)

<span data-ttu-id="84e58-126">Dowiedz się, gdzie można uzyskać narzędzia używane w tym przewodniku.</span><span class="sxs-lookup"><span data-stu-id="84e58-126">Learn where to acquire the tools used in this guide.</span></span>

### <a name="deploy-to-app-service"></a>[<span data-ttu-id="84e58-127">Wdrażanie do usługi App Service</span><span class="sxs-lookup"><span data-stu-id="84e58-127">Deploy to App Service</span></span>](xref:azure/devops/deploy-to-app-service)

<span data-ttu-id="84e58-128">Dowiedz się, różne metody wdrażania aplikacji platformy ASP.NET Core w usłudze Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="84e58-128">Learn the various methods for deploying an ASP.NET Core app to Azure App Service.</span></span>

### <a name="continuous-integration-and-deployment"></a>[<span data-ttu-id="84e58-129">Ciągła integracja i ciągłe wdrażanie</span><span class="sxs-lookup"><span data-stu-id="84e58-129">Continuous integration and deployment</span></span>](xref:azure/devops/cicd)

<span data-ttu-id="84e58-130">Twórz end-to-end ciągłej integracji i wdrażania rozwiązania dla aplikacji platformy ASP.NET Core za pomocą usługi GitHub, usługom DevOps platformy Azure i platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="84e58-130">Build an end-to-end continuous integration and deployment solution for your ASP.NET Core app with GitHub, Azure DevOps Services, and Azure.</span></span>

### <a name="monitor-and-debug"></a>[<span data-ttu-id="84e58-131">Monitorowanie i debugowanie</span><span class="sxs-lookup"><span data-stu-id="84e58-131">Monitor and debug</span></span>](xref:azure/devops/monitor)

<span data-ttu-id="84e58-132">Narzędzia platformy Azure umożliwia monitorowanie, rozwiązywanie problemów i dostosowywanie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="84e58-132">Use Azure's tools to monitor, troubleshoot, and tune your application.</span></span>

### <a name="next-steps"></a>[<span data-ttu-id="84e58-133">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="84e58-133">Next steps</span></span>](xref:azure/devops/next-steps)

<span data-ttu-id="84e58-134">Inne ścieżki szkoleniowe dla deweloperów platformy ASP.NET Core poznawania platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="84e58-134">Other learning paths for the ASP.NET Core developer learning Azure.</span></span>

## <a name="additional-introductory-reading"></a><span data-ttu-id="84e58-135">Dodatkowe materiały wprowadzające</span><span class="sxs-lookup"><span data-stu-id="84e58-135">Additional introductory reading</span></span>

<span data-ttu-id="84e58-136">Jeśli jest to pierwszy narażenia na przetwarzanie w chmurze, artykuły te wyjaśniają zasady podstawowe.</span><span class="sxs-lookup"><span data-stu-id="84e58-136">If this is your first exposure to cloud computing, these articles explain the basics.</span></span>

* [<span data-ttu-id="84e58-137">Co to jest chmura obliczeniowa?</span><span class="sxs-lookup"><span data-stu-id="84e58-137">What is Cloud Computing?</span></span>](https://azure.microsoft.com/overview/what-is-cloud-computing/)
* [<span data-ttu-id="84e58-138">Przykłady chmury obliczeniowej</span><span class="sxs-lookup"><span data-stu-id="84e58-138">Examples of Cloud Computing</span></span>](https://azure.microsoft.com/overview/examples-of-cloud-computing/)
* [<span data-ttu-id="84e58-139">Co to jest IaaS?</span><span class="sxs-lookup"><span data-stu-id="84e58-139">What is IaaS?</span></span>](https://azure.microsoft.com/overview/what-is-iaas/)
* [<span data-ttu-id="84e58-140">Co to jest PaaS?</span><span class="sxs-lookup"><span data-stu-id="84e58-140">What is PaaS?</span></span>](https://azure.microsoft.com/overview/what-is-paas/)
