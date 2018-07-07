---
title: Rozwiązywanie problemów z projektami ASP.NET Core
author: Rick-Anderson
description: Omówienie i rozwiązywanie problemów, ostrzeżenia i błędy w projektach programu ASP.NET Core.
ms.author: riande
ms.date: 04/05/2018
uid: test/troubleshoot
ms.openlocfilehash: b4d90f541a4cda2d41b49101b7ea39af87a4dcb4
ms.sourcegitcommit: a09820f91e71a7d98b7347bf93210abb9e995e22
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/06/2018
ms.locfileid: "37889015"
---
# <a name="troubleshoot-aspnet-core-projects"></a><span data-ttu-id="9310b-103">Rozwiązywanie problemów z projektami ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9310b-103">Troubleshoot ASP.NET Core projects</span></span>

<span data-ttu-id="9310b-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="9310b-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="9310b-105">Poniższe łącza zapewniają wskazówki dotyczące rozwiązywania problemów:</span><span class="sxs-lookup"><span data-stu-id="9310b-105">The following links provide troubleshooting guidance:</span></span>

* [<span data-ttu-id="9310b-106">Rozwiązywanie problemów z platformą ASP.NET Core w usłudze Azure App Service</span><span class="sxs-lookup"><span data-stu-id="9310b-106">Troubleshoot ASP.NET Core on Azure App Service</span></span>](xref:host-and-deploy/azure-apps/troubleshoot)
* [<span data-ttu-id="9310b-107">Rozwiązywanie problemów z platformą ASP.NET Core w usługach IIS</span><span class="sxs-lookup"><span data-stu-id="9310b-107">Troubleshoot ASP.NET Core on IIS</span></span>](xref:host-and-deploy/iis/troubleshoot)
* [<span data-ttu-id="9310b-108">Dokumentacja typowych błędów dla usługi Azure App Service i IIS za pomocą programu ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9310b-108">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>](xref:host-and-deploy/azure-iis-errors-reference)
* [<span data-ttu-id="9310b-109">Ndc Developers Conference (2018 r. Londyn;): Diagnozowanie problemów w aplikacji programu ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9310b-109">NDC Conference (London, 2018): Diagnosing issues in ASP.NET Core Applications</span></span>](https://www.youtube.com/watch?v=RYI0DHoIVaA)
* [<span data-ttu-id="9310b-110">Blog platformy ASP.NET: Rozwiązywanie problemów z problemów z wydajnością programu ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9310b-110">ASP.NET Blog: Troubleshooting ASP.NET Core Performance Problems</span></span>](https://blogs.msdn.microsoft.com/webdev/2018/05/23/asp-net-core-performance-improvements/)

## <a name="net-core-sdk-warnings"></a><span data-ttu-id="9310b-111">Ostrzeżenia dotyczące zestawu .NET core SDK</span><span class="sxs-lookup"><span data-stu-id="9310b-111">.NET Core SDK warnings</span></span>

### <a name="both-the-32-bit-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a><span data-ttu-id="9310b-112">32- i 64-bitowe wersje programu .NET Core SDK są instalowane.</span><span class="sxs-lookup"><span data-stu-id="9310b-112">Both the 32 bit and 64 bit versions of the .NET Core SDK are installed</span></span>

<span data-ttu-id="9310b-113">W **nowy projekt** okno dla platformy ASP.NET Core, może zostać wyświetlony następujące ostrzeżenie:</span><span class="sxs-lookup"><span data-stu-id="9310b-113">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="9310b-114">Zarówno 32- i 64 bitowych wersjach programu .NET Core SDK są instalowane.</span><span class="sxs-lookup"><span data-stu-id="9310b-114">Both 32 and 64 bit versions of the .NET Core SDK are installed.</span></span> <span data-ttu-id="9310b-115">Tylko szablony z wersji 64-bitowych zainstalowanej w lokalizacji "C:\\Program Files\\dotnet\\sdk\\" będą wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="9310b-115">Only templates from the 64 bit version(s) installed at 'C:\\Program Files\\dotnet\\sdk\\' will be displayed.</span></span>

![Zrzut ekranu okna dialogowego OneASP.NET przedstawiający komunikat ostrzegawczy](troubleshoot/_static/both32and64bit.png)

<span data-ttu-id="9310b-117">To ostrzeżenie jest wyświetlane, gdy zarówno wersji 64-bitowych (x 64), jak i 32-bitowych (x86) [zestawu .NET Core SDK](https://www.microsoft.com/net/download/all) są zainstalowane.</span><span class="sxs-lookup"><span data-stu-id="9310b-117">This warning appears when both 32-bit (x86) and 64-bit (x64) versions of the [.NET Core SDK](https://www.microsoft.com/net/download/all) are installed.</span></span> <span data-ttu-id="9310b-118">Typowe przyczyny, które można zainstalować obie wersje obejmują:</span><span class="sxs-lookup"><span data-stu-id="9310b-118">Common reasons both versions may be installed include:</span></span>

* <span data-ttu-id="9310b-119">Pierwotnie pobrany przy użyciu komputera z 32-bitowy Instalator zestawu .NET Core SDK, ale następnie skopiować go na i zainstalować je na komputerze 64-bitowym.</span><span class="sxs-lookup"><span data-stu-id="9310b-119">You originally downloaded the .NET Core SDK installer using a 32-bit machine but then copied it across and installed it on a 64-bit machine.</span></span>
* <span data-ttu-id="9310b-120">32-bitowych .NET Core SDK został zainstalowany przez inną aplikację.</span><span class="sxs-lookup"><span data-stu-id="9310b-120">The 32-bit .NET Core SDK was installed by another application.</span></span>
* <span data-ttu-id="9310b-121">Niewłaściwa wersja został pobrany i zainstalowany.</span><span class="sxs-lookup"><span data-stu-id="9310b-121">The wrong version was downloaded and installed.</span></span>

<span data-ttu-id="9310b-122">Odinstaluj 32-bitowych .NET Core SDK, aby uniknąć tego ostrzeżenia.</span><span class="sxs-lookup"><span data-stu-id="9310b-122">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="9310b-123">Odinstaluj z **Panelu sterowania** > **programy i funkcje** > **Odinstaluj lub zmień program**.</span><span class="sxs-lookup"><span data-stu-id="9310b-123">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="9310b-124">Jeśli zrozumiesz, dlaczego występuje ostrzeżenie i jego skutków, możesz zignorować to ostrzeżenie.</span><span class="sxs-lookup"><span data-stu-id="9310b-124">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a><span data-ttu-id="9310b-125">.NET Core SDK jest zainstalowany w wielu lokalizacjach</span><span class="sxs-lookup"><span data-stu-id="9310b-125">The .NET Core SDK is installed in multiple locations</span></span>

<span data-ttu-id="9310b-126">W **nowy projekt** okno dla platformy ASP.NET Core, może zostać wyświetlony następujące ostrzeżenie:</span><span class="sxs-lookup"><span data-stu-id="9310b-126">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="9310b-127">.NET Core SDK jest zainstalowany w wielu lokalizacjach.</span><span class="sxs-lookup"><span data-stu-id="9310b-127">The .NET Core SDK is installed in multiple locations.</span></span> <span data-ttu-id="9310b-128">Tylko szablony z zestawów SDK zainstalowanych na "C:\\Program Files\\dotnet\\sdk\\" będą wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="9310b-128">Only templates from the SDK(s) installed at 'C:\\Program Files\\dotnet\\sdk\\' will be displayed.</span></span>

![Zrzut ekranu okna dialogowego OneASP.NET przedstawiający komunikat ostrzegawczy](troubleshoot/_static/multiplelocations.png)

<span data-ttu-id="9310b-130">Ten komunikat jest wyświetlany, gdy masz co najmniej jedna instalacja zestawu SDK programu .NET Core w katalogu, poza *C:\\Program Files\\dotnet\\sdk\\*.</span><span class="sxs-lookup"><span data-stu-id="9310b-130">You see this message when you have at least one installation of the .NET Core SDK in a directory outside of *C:\\Program Files\\dotnet\\sdk\\*.</span></span> <span data-ttu-id="9310b-131">Zwykle dzieje się tak w przypadku zestawu .NET Core SDK został wdrożony na maszynie za pomocą kopiowania/wklejania zamiast Instalatora MSI.</span><span class="sxs-lookup"><span data-stu-id="9310b-131">Usually this happens when the .NET Core SDK has been deployed on a machine using copy/paste instead of the MSI installer.</span></span>

<span data-ttu-id="9310b-132">Odinstaluj 32-bitowych .NET Core SDK, aby uniknąć tego ostrzeżenia.</span><span class="sxs-lookup"><span data-stu-id="9310b-132">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="9310b-133">Odinstaluj z **Panelu sterowania** > **programy i funkcje** > **Odinstaluj lub zmień program**.</span><span class="sxs-lookup"><span data-stu-id="9310b-133">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="9310b-134">Jeśli zrozumiesz, dlaczego występuje ostrzeżenie i jego skutków, możesz zignorować to ostrzeżenie.</span><span class="sxs-lookup"><span data-stu-id="9310b-134">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="no-net-core-sdks-were-detected"></a><span data-ttu-id="9310b-135">Nie wykryto żadnych zestawów .NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="9310b-135">No .NET Core SDKs were detected</span></span>

<span data-ttu-id="9310b-136">W **nowy projekt** okno dla platformy ASP.NET Core, może zostać wyświetlony następujące ostrzeżenie:</span><span class="sxs-lookup"><span data-stu-id="9310b-136">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="9310b-137">Nie wykryto żadnych zestawów .NET Core SDK, upewnij się, że są one uwzględnione w zmiennej środowiskowej "PATH".</span><span class="sxs-lookup"><span data-stu-id="9310b-137">No .NET Core SDKs were detected, ensure they are included in the environment variable 'PATH'.</span></span>

![Zrzut ekranu okna dialogowego OneASP.NET przedstawiający komunikat ostrzegawczy](troubleshoot/_static/NoNetCore.png)

<span data-ttu-id="9310b-139">To ostrzeżenie jest wyświetlane, gdy zmienna środowiskowa `PATH` nie wskazuje na żadnych zestawów .NET Core SDK na maszynie.</span><span class="sxs-lookup"><span data-stu-id="9310b-139">This warning appears when the environment variable `PATH` doesn't point to any .NET Core SDKs on the machine.</span></span> <span data-ttu-id="9310b-140">Aby rozwiązać ten problem:</span><span class="sxs-lookup"><span data-stu-id="9310b-140">To resolve this problem:</span></span>

* <span data-ttu-id="9310b-141">Zainstaluj lub sprawdzić, czy jest zainstalowany zestaw .NET Core SDK.</span><span class="sxs-lookup"><span data-stu-id="9310b-141">Install or verify the .NET Core SDK is installed.</span></span>
* <span data-ttu-id="9310b-142">Upewnij się, że `PATH` zmienna środowiskowa wskazuje lokalizację, w którym jest zainstalowany zestaw SDK.</span><span class="sxs-lookup"><span data-stu-id="9310b-142">Verify that the `PATH` environment variable points to the location in which the SDK is installed.</span></span> <span data-ttu-id="9310b-143">Instalator zwykle ustawia `PATH`.</span><span class="sxs-lookup"><span data-stu-id="9310b-143">The installer normally sets the `PATH`.</span></span>