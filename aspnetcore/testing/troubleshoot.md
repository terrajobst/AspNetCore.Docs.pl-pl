---
title: Rozwiązywanie problemów z dla platformy ASP.NET Core
author: Rick-Anderson
description: Omówienie i rozwiązywanie problemów ostrzeżeń i błędów z projektów platformy ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 04/05/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: content
uid: testing/troubleshoot
ms.openlocfilehash: 98077081409949db14b19c7934bc162990ffc302
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
# <a name="troubleshoot-aspnet-core-projects"></a><span data-ttu-id="72aa3-103">Rozwiązywanie problemów z projektów platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="72aa3-103">Troubleshoot ASP.NET Core projects</span></span>

<span data-ttu-id="72aa3-104">przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="72aa3-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="72aa3-105">Poniższe łącza zawierają wskazówki dotyczące rozwiązywania problemów:</span><span class="sxs-lookup"><span data-stu-id="72aa3-105">The following links provide troubleshooting guidance:</span></span>

* [<span data-ttu-id="72aa3-106">Rozwiązywanie problemów z platformą ASP.NET Core w usłudze Azure App Service</span><span class="sxs-lookup"><span data-stu-id="72aa3-106">Troubleshoot ASP.NET Core on Azure App Service</span></span>](xref:host-and-deploy/azure-apps/troubleshoot)
* [<span data-ttu-id="72aa3-107">Rozwiązywanie problemów z platformą ASP.NET Core w usługach IIS</span><span class="sxs-lookup"><span data-stu-id="72aa3-107">Troubleshoot ASP.NET Core on IIS</span></span>](xref:host-and-deploy/iis/troubleshoot)
* [<span data-ttu-id="72aa3-108">Typowe błędy odwołania dla usługi Azure App Service i IIS z platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="72aa3-108">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>](xref:host-and-deploy/azure-iis-errors-reference)

<a name="sdk"></a>
## <a name="net-core-sdk-warnings"></a><span data-ttu-id="72aa3-109">Ostrzeżenia .NET core SDK</span><span class="sxs-lookup"><span data-stu-id="72aa3-109">.NET Core SDK warnings</span></span>

### <a name="both-the-32-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a><span data-ttu-id="72aa3-110">Zarówno w 32- i 64 bitowych wersjach zestawu SDK .NET Core są zainstalowane</span><span class="sxs-lookup"><span data-stu-id="72aa3-110">Both the 32 and 64 bit versions of the .NET Core SDK are installed</span></span>
<span data-ttu-id="72aa3-111">W **nowy projekt** okna dialogowego dla platformy ASP.NET Core, mogą pojawić się następujące ostrzeżenie pojawiają się u góry:</span><span class="sxs-lookup"><span data-stu-id="72aa3-111">In the **New Project** dialog for ASP.NET Core, you may see the following warning appear at the top:</span></span> 

    Both 32 and 64 bit versions of the .NET Core SDK are installed. Only templates from the 64 bit version(s) installed at C:\Program Files\dotnet\sdk\" will be displayed.

![Zrzut ekranu przedstawiający komunikat ostrzegawczy okna dialogowego OneASP.NET](troubleshoot/_static/both32and64bit.png)

<span data-ttu-id="72aa3-113">To ostrzeżenie jest wyświetlane, gdy zarówno wersji 64-bitowej (x 64), jak i 32-bitowych (x86) [.NET Core SDK](https://www.microsoft.com/net/download/all) są zainstalowane.</span><span class="sxs-lookup"><span data-stu-id="72aa3-113">This warning appears when both 32-bit (x86) and 64-bit (x64) versions of the [.NET Core SDK](https://www.microsoft.com/net/download/all) are installed.</span></span> <span data-ttu-id="72aa3-114">Typowe przyczyny, można ją zainstalować obie wersje obejmują:</span><span class="sxs-lookup"><span data-stu-id="72aa3-114">Common reasons both versions can be installed include:</span></span>

* <span data-ttu-id="72aa3-115">Pierwotnie pobrany przy użyciu komputera 32-bitowego, Instalator .NET Core SDK, ale następnie skopiować go na i instalować go na komputerze 64-bitowych.</span><span class="sxs-lookup"><span data-stu-id="72aa3-115">You originally downloaded the .NET Core SDK installer using a 32-bit machine, but then copied it across and installed it on a 64-bit machine.</span></span> 
* <span data-ttu-id="72aa3-116">32-bitowej platformy .NET Core SDK został zainstalowany przez inną aplikację.</span><span class="sxs-lookup"><span data-stu-id="72aa3-116">The 32-bit .NET Core SDK was installed by another application.</span></span>
* <span data-ttu-id="72aa3-117">Nieprawidłowa wersja została pobrana i zainstalowana.</span><span class="sxs-lookup"><span data-stu-id="72aa3-117">The wrong version was downloaded and installed.</span></span>

<span data-ttu-id="72aa3-118">Odinstaluj 32-bitowej platformy .NET Core SDK, aby uniknąć tego ostrzeżenia.</span><span class="sxs-lookup"><span data-stu-id="72aa3-118">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="72aa3-119">Odinstaluj z **Panelu sterowania** > **programy i funkcje** > **Odinstaluj lub zmień program**.</span><span class="sxs-lookup"><span data-stu-id="72aa3-119">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="72aa3-120">Jeśli wiesz, dlaczego występuje ostrzeżenie i jej wpływ, można zignorować to ostrzeżenie.</span><span class="sxs-lookup"><span data-stu-id="72aa3-120">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a><span data-ttu-id="72aa3-121">.NET Core SDK jest zainstalowany w wielu lokalizacjach</span><span class="sxs-lookup"><span data-stu-id="72aa3-121">The .NET Core SDK is installed in multiple locations</span></span>
<span data-ttu-id="72aa3-122">W **nowy projekt** okno dialogowe dla platformy ASP.NET Core mogą pojawić się następujące ostrzeżenie pojawiają się u góry:</span><span class="sxs-lookup"><span data-stu-id="72aa3-122">In the **New Project** dialog for ASP.NET Core you may see the following warning appear at the top:</span></span> 

 <span data-ttu-id="72aa3-123">Zestaw SDK .NET Core zainstalowano w wielu lokalizacjach.</span><span class="sxs-lookup"><span data-stu-id="72aa3-123">The .NET Core SDK is installed in multiple locations.</span></span> <span data-ttu-id="72aa3-124">Tylko szablony z Jeśli zainstalowana w "C:\Program Files\dotnet\sdk\' będą wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="72aa3-124">Only templates from the SDK(s) installed at 'C:\Program Files\dotnet\sdk\' will be displayed.</span></span>

![Zrzut ekranu przedstawiający komunikat ostrzegawczy okna dialogowego OneASP.NET](troubleshoot/_static/multiplelocations.png)

<span data-ttu-id="72aa3-126">Ten komunikat jest wyświetlany, ponieważ ma co najmniej jedna instalacja zestawu SDK .NET Core w katalogu poza * C:\Program Files\dotnet\sdk\*.</span><span class="sxs-lookup"><span data-stu-id="72aa3-126">You are seeing this message because you have at least one installation of the .NET Core SDK in a directory outside of *C:\Program Files\dotnet\sdk\*.</span></span> <span data-ttu-id="72aa3-127">Zwykle ma to miejsce podczas .NET Core SDK został wdrożony na komputerze za pomocą kopiowania i wklejania zamiast Instalatora MSI.</span><span class="sxs-lookup"><span data-stu-id="72aa3-127">Usually that happens when the .NET Core SDK has been deployed on a machine using copy/paste instead of the MSI installer.</span></span>

<span data-ttu-id="72aa3-128">Odinstaluj 32-bitowej platformy .NET Core SDK, aby uniknąć tego ostrzeżenia.</span><span class="sxs-lookup"><span data-stu-id="72aa3-128">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="72aa3-129">Odinstaluj z **Panelu sterowania** > **programy i funkcje** > **Odinstaluj lub zmień program**.</span><span class="sxs-lookup"><span data-stu-id="72aa3-129">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="72aa3-130">Jeśli wiesz, dlaczego występuje ostrzeżenie i jej wpływ, można zignorować to ostrzeżenie.</span><span class="sxs-lookup"><span data-stu-id="72aa3-130">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>
