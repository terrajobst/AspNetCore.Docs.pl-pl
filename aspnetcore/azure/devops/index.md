---
title: Metodyka DevOps z platformą ASP.NET Core i platformy Azure
author: CamSoper
description: Przewodnik, który dostarcza wskazówki end-to-end na tworzeniu potoku metodyki DevOps dla aplikacji ASP.NET Core hostowanych na platformie Azure.
ms.author: casoper
ms.date: 08/07/2018
uid: azure/devops/index
ms.openlocfilehash: 09ca835e908e81c6f38f9430fb40638ba6dc3350
ms.sourcegitcommit: 29dfe436f54a27fbb4f6494bc639d16c75001fab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/09/2018
ms.locfileid: "39722671"
---
# <a name="devops-with-aspnet-core-and-azure"></a><span data-ttu-id="8cbe9-103">Metodyka DevOps z platformą ASP.NET Core i platformy Azure</span><span class="sxs-lookup"><span data-stu-id="8cbe9-103">DevOps with ASP.NET Core and Azure</span></span>

<span data-ttu-id="8cbe9-104">Przewodnik cyklu tworzenia oprogramowania platformy Azure dla platformy .NET — Zapraszamy!</span><span class="sxs-lookup"><span data-stu-id="8cbe9-104">Welcome to the Azure Development Lifecycle guide for .NET!</span></span> <span data-ttu-id="8cbe9-105">Ten przewodnik wprowadzenie podstawowych pojęć dotyczących tworzenia cyklu życia na platformie Azure przy użyciu platformy .NET, narzędziami i procesami.</span><span class="sxs-lookup"><span data-stu-id="8cbe9-105">This guide introduces the basic concepts of building a development lifecycle around Azure using .NET tools and processes.</span></span> <span data-ttu-id="8cbe9-106">Po zakończeniu tego przewodnika, możesz czerpać korzyści dojrzała łańcucha narzędzi DevOps.</span><span class="sxs-lookup"><span data-stu-id="8cbe9-106">After finishing this guide, you'll reap the benefits of a mature DevOps toolchain.</span></span>

## <a name="who-this-guide-is-for"></a><span data-ttu-id="8cbe9-107">Kto ten przewodnik jest przeznaczony dla</span><span class="sxs-lookup"><span data-stu-id="8cbe9-107">Who this guide is for</span></span>

<span data-ttu-id="8cbe9-108">Powinno być doświadczonym deweloperom platformy ASP.NET (poziom 200-300).</span><span class="sxs-lookup"><span data-stu-id="8cbe9-108">You should be an experienced ASP.NET developer (200-300 level).</span></span> <span data-ttu-id="8cbe9-109">Nie trzeba nic wiedzieć o platformie Azure, jak omówimy to w ramach tego wprowadzenia.</span><span class="sxs-lookup"><span data-stu-id="8cbe9-109">You don't need to know anything about Azure, as we'll cover that in this introduction.</span></span> <span data-ttu-id="8cbe9-110">Ten przewodnik może być również przydatne w przypadku inżynierom DevOps, którzy są bardziej skupia się na operacje niż rozwoju.</span><span class="sxs-lookup"><span data-stu-id="8cbe9-110">This guide may also be useful for DevOps engineers who are more focused on operations than development.</span></span>

<span data-ttu-id="8cbe9-111">Ten przewodnik jest przeznaczony dla deweloperów Windows.</span><span class="sxs-lookup"><span data-stu-id="8cbe9-111">This guide targets Windows developers.</span></span> <span data-ttu-id="8cbe9-112">Jednak z systemami Linux i macOS są w pełni obsługiwane przez .NET Core.</span><span class="sxs-lookup"><span data-stu-id="8cbe9-112">However, Linux and macOS are fully supported by .NET Core.</span></span> <span data-ttu-id="8cbe9-113">Można dostosować ten przewodnik dla systemu Linux/macOS, obserwuj objaśnienia na różnice Linux/macOS.</span><span class="sxs-lookup"><span data-stu-id="8cbe9-113">To adapt this guide for Linux/macOS, watch for callouts for Linux/macOS differences.</span></span>

## <a name="what-this-guide-doesnt-cover"></a><span data-ttu-id="8cbe9-114">Co ten przewodnik nie obejmuje</span><span class="sxs-lookup"><span data-stu-id="8cbe9-114">What this guide doesn't cover</span></span>

<span data-ttu-id="8cbe9-115">Ten przewodnik koncentruje się na środowisko end-to-end ciągłego wdrażania dla deweloperów platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="8cbe9-115">This guide is focused on an end-to-end continuous deployment experience for .NET developers.</span></span> <span data-ttu-id="8cbe9-116">Nie jest wyczerpująca przewodnik wszystkimi aspektami platformy Azure, a jej nie skupić często interfejsów API platformy .NET dla usług platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="8cbe9-116">It's not an exhaustive guide to all things Azure, and it doesn't focus extensively on .NET APIs for Azure services.</span></span> <span data-ttu-id="8cbe9-117">Jest nacisk na całym ciągłej integracji i wdrażania, monitorowania i debugowania.</span><span class="sxs-lookup"><span data-stu-id="8cbe9-117">The emphasis is all around continuous integration, deployment, monitoring, and debugging.</span></span> <span data-ttu-id="8cbe9-118">Pod koniec przewodnika są oferowane zalecenia dotyczące następnych kroków.</span><span class="sxs-lookup"><span data-stu-id="8cbe9-118">Near the end of the guide, recommendations for next steps are offered.</span></span> <span data-ttu-id="8cbe9-119">Sugestie obejmuje usług platformy Azure, które są przydatne dla deweloperów platformy ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="8cbe9-119">Included in the suggestions are Azure platform services that are useful to ASP.NET developers.</span></span>

## <a name="whats-in-this-guide"></a><span data-ttu-id="8cbe9-120">Co to jest w tym przewodniku</span><span class="sxs-lookup"><span data-stu-id="8cbe9-120">What's in this guide</span></span>

### <a name="tools-and-downloadsxrefazuredevopstools-and-downloads"></a>[<span data-ttu-id="8cbe9-121">Narzędzia i pliki do pobrania</span><span class="sxs-lookup"><span data-stu-id="8cbe9-121">Tools and downloads</span></span>](xref:azure/devops/tools-and-downloads)

<span data-ttu-id="8cbe9-122">Dowiedz się, gdzie można uzyskać narzędzia używane w tym przewodniku.</span><span class="sxs-lookup"><span data-stu-id="8cbe9-122">Learn where to acquire the tools used in this guide.</span></span>

### <a name="deploy-to-app-servicexrefazuredevopsdeploy-to-app-service"></a>[<span data-ttu-id="8cbe9-123">Wdrażanie w usłudze App Service</span><span class="sxs-lookup"><span data-stu-id="8cbe9-123">Deploy to App Service</span></span>](xref:azure/devops/deploy-to-app-service)

<span data-ttu-id="8cbe9-124">Dowiedz się, różne metody wdrażania aplikacji platformy ASP.NET Core w usłudze Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="8cbe9-124">Learn the various methods for deploying an ASP.NET Core app to Azure App Service.</span></span>

### <a name="continuous-integration-and-deploymentxrefazuredevopscicd"></a>[<span data-ttu-id="8cbe9-125">Ciągła integracja i ciągłe wdrażanie</span><span class="sxs-lookup"><span data-stu-id="8cbe9-125">Continuous integration and deployment</span></span>](xref:azure/devops/cicd)

<span data-ttu-id="8cbe9-126">Twórz end-to-end ciągłej integracji i wdrażania rozwiązania dla aplikacji platformy ASP.NET Core przy użyciu serwisu GitHub, VSTS i platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="8cbe9-126">Build an end-to-end continuous integration and deployment solution for your ASP.NET Core app with GitHub, VSTS, and Azure.</span></span>

### <a name="monitor-and-debugxrefazuredevopsmonitor"></a>[<span data-ttu-id="8cbe9-127">Monitorowanie i debugowania</span><span class="sxs-lookup"><span data-stu-id="8cbe9-127">Monitor and debug</span></span>](xref:azure/devops/monitor)

<span data-ttu-id="8cbe9-128">Narzędzia platformy Azure umożliwia monitorowanie, rozwiązywanie problemów i dostosowywanie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8cbe9-128">Use Azure's tools to monitor, troubleshoot, and tune your application.</span></span>

### <a name="next-stepsxrefazuredevopsnext-steps"></a>[<span data-ttu-id="8cbe9-129">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="8cbe9-129">Next steps</span></span>](xref:azure/devops/next-steps)

<span data-ttu-id="8cbe9-130">Inne ścieżki szkoleniowe dla deweloperów platformy ASP.NET Core poznawania platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="8cbe9-130">Other learning paths for the ASP.NET Core developer learning Azure.</span></span>

## <a name="acknowledgments"></a><span data-ttu-id="8cbe9-131">Potwierdzenia</span><span class="sxs-lookup"><span data-stu-id="8cbe9-131">Acknowledgments</span></span>

<span data-ttu-id="8cbe9-132">Dziękujemy wszystkim w społeczności platformy .NET, które przyczyniły się do tego przewodnika z sugestiami przydatne!</span><span class="sxs-lookup"><span data-stu-id="8cbe9-132">Thank you to everyone in the .NET community who contributed to this guide with helpful suggestions!</span></span> <span data-ttu-id="8cbe9-133">Chcielibyśmy szczególnie podziękować następujących członków społeczności, które przyczyniły się do końcowej oceny tego materiału:</span><span class="sxs-lookup"><span data-stu-id="8cbe9-133">We'd like to specifically thank the following community members who contributed to the final review of this material:</span></span>

* [<span data-ttu-id="8cbe9-134">Sam Wronski</span><span class="sxs-lookup"><span data-stu-id="8cbe9-134">Sam Wronski</span></span>](https://www.youtube.com/c/worldofzerodevelopment)
* [<span data-ttu-id="8cbe9-135">Jeffrey Palermo</span><span class="sxs-lookup"><span data-stu-id="8cbe9-135">Jeffrey Palermo</span></span>](https://twitter.com/jeffreypalermo)

## <a name="conclusion"></a><span data-ttu-id="8cbe9-136">Wniosek</span><span class="sxs-lookup"><span data-stu-id="8cbe9-136">Conclusion</span></span>

<span data-ttu-id="8cbe9-137">Ten przewodnik samodzielnie przygotowuje Cię do kompilacji ciągłej integracji cyklu tworzenia oprogramowania, korzystające z platformy ASP.NET Core i usługi Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="8cbe9-137">This guide prepares you to build a continuous integration development lifecycle built around ASP.NET Core and Azure App Service.</span></span>

## <a name="additional-reading"></a><span data-ttu-id="8cbe9-138">Materiały uzupełniające</span><span class="sxs-lookup"><span data-stu-id="8cbe9-138">Additional reading</span></span>

* [<span data-ttu-id="8cbe9-139">Co to jest chmura obliczeniowa?</span><span class="sxs-lookup"><span data-stu-id="8cbe9-139">What is Cloud Computing?</span></span>](https://azure.microsoft.com/overview/what-is-cloud-computing/)
* [<span data-ttu-id="8cbe9-140">Przykłady chmury obliczeniowej</span><span class="sxs-lookup"><span data-stu-id="8cbe9-140">Examples of Cloud Computing</span></span>](https://azure.microsoft.com/overview/examples-of-cloud-computing/)
* [<span data-ttu-id="8cbe9-141">Co to jest IaaS?</span><span class="sxs-lookup"><span data-stu-id="8cbe9-141">What is IaaS?</span></span>](https://azure.microsoft.com/overview/what-is-iaas/)
* [<span data-ttu-id="8cbe9-142">Co to jest PaaS?</span><span class="sxs-lookup"><span data-stu-id="8cbe9-142">What is PaaS?</span></span>](https://azure.microsoft.com/overview/what-is-paas/)
