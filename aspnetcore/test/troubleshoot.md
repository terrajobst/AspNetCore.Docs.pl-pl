---
title: Rozwiązywanie problemów z projektów platformy ASP.NET Core
author: Rick-Anderson
description: Omówienie i rozwiązywanie problemów ostrzeżeń i błędów z projektów platformy ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 04/05/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: content
uid: test/troubleshoot
ms.openlocfilehash: 8ff8ddaf4a35a02db650ff429ffbbf4e76a53ecf
ms.sourcegitcommit: 4e3497bda0c3e5011ffba3717eb61a1d46c61c15
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/14/2018
ms.locfileid: "35613008"
---
# <a name="troubleshoot-aspnet-core-projects"></a><span data-ttu-id="0f0a2-103">Rozwiązywanie problemów z projektów platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0f0a2-103">Troubleshoot ASP.NET Core projects</span></span>

<span data-ttu-id="0f0a2-104">przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="0f0a2-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="0f0a2-105">Poniższe łącza zawierają wskazówki dotyczące rozwiązywania problemów:</span><span class="sxs-lookup"><span data-stu-id="0f0a2-105">The following links provide troubleshooting guidance:</span></span>

* [<span data-ttu-id="0f0a2-106">Rozwiązywanie problemów z platformą ASP.NET Core w usłudze Azure App Service</span><span class="sxs-lookup"><span data-stu-id="0f0a2-106">Troubleshoot ASP.NET Core on Azure App Service</span></span>](xref:host-and-deploy/azure-apps/troubleshoot)
* [<span data-ttu-id="0f0a2-107">Rozwiązywanie problemów z platformą ASP.NET Core w usługach IIS</span><span class="sxs-lookup"><span data-stu-id="0f0a2-107">Troubleshoot ASP.NET Core on IIS</span></span>](xref:host-and-deploy/iis/troubleshoot)
* [<span data-ttu-id="0f0a2-108">Typowe błędy odwołania dla usługi Azure App Service i IIS z platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0f0a2-108">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>](xref:host-and-deploy/azure-iis-errors-reference)
* [<span data-ttu-id="0f0a2-109">Konferencja NDC (Londynie 2018): Diagnozowanie problemów w aplikacjach ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0f0a2-109">NDC Conference (London, 2018): Diagnosing issues in ASP.NET Core Applications</span></span>](https://www.youtube.com/watch?v=RYI0DHoIVaA)
* [<span data-ttu-id="0f0a2-110">Blog platformy ASP.NET: Rozwiązywanie problemów z platformy ASP.NET Core problemy z wydajnością</span><span class="sxs-lookup"><span data-stu-id="0f0a2-110">ASP.NET Blog: Troubleshooting ASP.NET Core Performance Problems</span></span>](https://blogs.msdn.microsoft.com/webdev/2018/05/23/asp-net-core-performance-improvements/)

## <a name="net-core-sdk-warnings"></a><span data-ttu-id="0f0a2-111">Ostrzeżenia .NET core SDK</span><span class="sxs-lookup"><span data-stu-id="0f0a2-111">.NET Core SDK warnings</span></span>

### <a name="both-the-32-bit-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a><span data-ttu-id="0f0a2-112">Zainstalowano 32-bitowe i 64-bitowe wersje .NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="0f0a2-112">Both the 32 bit and 64 bit versions of the .NET Core SDK are installed</span></span>

<span data-ttu-id="0f0a2-113">W **nowy projekt** okna dialogowego dla platformy ASP.NET Core, mogą pojawić się następujące ostrzeżenie:</span><span class="sxs-lookup"><span data-stu-id="0f0a2-113">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="0f0a2-114">Zarówno 32- i 64 bitowych wersji zestawu SDK .NET Core są zainstalowane.</span><span class="sxs-lookup"><span data-stu-id="0f0a2-114">Both 32 and 64 bit versions of the .NET Core SDK are installed.</span></span> <span data-ttu-id="0f0a2-115">Tylko szablony z wersje 64-bitowym zainstalowany na "C:\\Program Files\\dotnet\\sdk\\" będą wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="0f0a2-115">Only templates from the 64 bit version(s) installed at 'C:\\Program Files\\dotnet\\sdk\\' will be displayed.</span></span>

![Zrzut ekranu przedstawiający komunikat ostrzegawczy okna dialogowego OneASP.NET](troubleshoot/_static/both32and64bit.png)

<span data-ttu-id="0f0a2-117">To ostrzeżenie jest wyświetlane, gdy zarówno wersji 64-bitowej (x 64), jak i 32-bitowych (x86) [.NET Core SDK](https://www.microsoft.com/net/download/all) są zainstalowane.</span><span class="sxs-lookup"><span data-stu-id="0f0a2-117">This warning appears when both 32-bit (x86) and 64-bit (x64) versions of the [.NET Core SDK](https://www.microsoft.com/net/download/all) are installed.</span></span> <span data-ttu-id="0f0a2-118">Typowe przyczyny mogą być zainstalowane obie wersje obejmują:</span><span class="sxs-lookup"><span data-stu-id="0f0a2-118">Common reasons both versions may be installed include:</span></span>

* <span data-ttu-id="0f0a2-119">Pierwotnie pobrany Instalator zestawu SDK programu .NET Core za pomocą komputera 32-bitowego, ale następnie skopiowana między i instalować go na komputerze 64-bitowych.</span><span class="sxs-lookup"><span data-stu-id="0f0a2-119">You originally downloaded the .NET Core SDK installer using a 32-bit machine but then copied it across and installed it on a 64-bit machine.</span></span>
* <span data-ttu-id="0f0a2-120">32-bitowej platformy .NET Core SDK został zainstalowany przez inną aplikację.</span><span class="sxs-lookup"><span data-stu-id="0f0a2-120">The 32-bit .NET Core SDK was installed by another application.</span></span>
* <span data-ttu-id="0f0a2-121">Nieprawidłowa wersja została pobrana i zainstalowana.</span><span class="sxs-lookup"><span data-stu-id="0f0a2-121">The wrong version was downloaded and installed.</span></span>

<span data-ttu-id="0f0a2-122">Odinstaluj 32-bitowej platformy .NET Core SDK, aby uniknąć tego ostrzeżenia.</span><span class="sxs-lookup"><span data-stu-id="0f0a2-122">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="0f0a2-123">Odinstaluj z **Panelu sterowania** > **programy i funkcje** > **Odinstaluj lub zmień program**.</span><span class="sxs-lookup"><span data-stu-id="0f0a2-123">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="0f0a2-124">Jeśli wiesz, dlaczego występuje ostrzeżenie i jej wpływ, można zignorować to ostrzeżenie.</span><span class="sxs-lookup"><span data-stu-id="0f0a2-124">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a><span data-ttu-id="0f0a2-125">.NET Core SDK jest zainstalowany w wielu lokalizacjach</span><span class="sxs-lookup"><span data-stu-id="0f0a2-125">The .NET Core SDK is installed in multiple locations</span></span>

<span data-ttu-id="0f0a2-126">W **nowy projekt** okna dialogowego dla platformy ASP.NET Core, mogą pojawić się następujące ostrzeżenie:</span><span class="sxs-lookup"><span data-stu-id="0f0a2-126">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="0f0a2-127">Zestaw SDK .NET Core zainstalowano w wielu lokalizacjach.</span><span class="sxs-lookup"><span data-stu-id="0f0a2-127">The .NET Core SDK is installed in multiple locations.</span></span> <span data-ttu-id="0f0a2-128">Tylko szablony z zestawów SDK zainstalowany w "C:\\Program Files\\dotnet\\sdk\\" będą wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="0f0a2-128">Only templates from the SDK(s) installed at 'C:\\Program Files\\dotnet\\sdk\\' will be displayed.</span></span>

![Zrzut ekranu przedstawiający komunikat ostrzegawczy okna dialogowego OneASP.NET](troubleshoot/_static/multiplelocations.png)

<span data-ttu-id="0f0a2-130">Ten komunikat zostanie wyświetlony, jeśli masz co najmniej jedna instalacja zestawu SDK .NET Core w katalogu poza *C:\\Program Files\\dotnet\\sdk\\*.</span><span class="sxs-lookup"><span data-stu-id="0f0a2-130">You see this message when you have at least one installation of the .NET Core SDK in a directory outside of *C:\\Program Files\\dotnet\\sdk\\*.</span></span> <span data-ttu-id="0f0a2-131">Zazwyczaj dzieje się tak podczas .NET Core SDK został wdrożony na komputerze za pomocą kopiowania i wklejania zamiast Instalatora MSI.</span><span class="sxs-lookup"><span data-stu-id="0f0a2-131">Usually this happens when the .NET Core SDK has been deployed on a machine using copy/paste instead of the MSI installer.</span></span>

<span data-ttu-id="0f0a2-132">Odinstaluj 32-bitowej platformy .NET Core SDK, aby uniknąć tego ostrzeżenia.</span><span class="sxs-lookup"><span data-stu-id="0f0a2-132">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="0f0a2-133">Odinstaluj z **Panelu sterowania** > **programy i funkcje** > **Odinstaluj lub zmień program**.</span><span class="sxs-lookup"><span data-stu-id="0f0a2-133">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="0f0a2-134">Jeśli wiesz, dlaczego występuje ostrzeżenie i jej wpływ, można zignorować to ostrzeżenie.</span><span class="sxs-lookup"><span data-stu-id="0f0a2-134">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="no-net-core-sdks-were-detected"></a><span data-ttu-id="0f0a2-135">Nie wykryto nie .NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="0f0a2-135">No .NET Core SDKs were detected</span></span>

<span data-ttu-id="0f0a2-136">W **nowy projekt** okna dialogowego dla platformy ASP.NET Core, mogą pojawić się następujące ostrzeżenie:</span><span class="sxs-lookup"><span data-stu-id="0f0a2-136">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="0f0a2-137">Nie wykryto nie .NET Core SDK, upewnij się, że są one uwzględnione w zmiennej środowiskowej "PATH".</span><span class="sxs-lookup"><span data-stu-id="0f0a2-137">No .NET Core SDKs were detected, ensure they are included in the environment variable 'PATH'.</span></span>

![Zrzut ekranu przedstawiający komunikat ostrzegawczy okna dialogowego OneASP.NET](troubleshoot/_static/NoNetCore.png)

<span data-ttu-id="0f0a2-139">To ostrzeżenie jest wyświetlane, gdy zmienna środowiskowa `PATH` nie wskazuje na żadnych zestawów SDK Core .NET na tym komputerze.</span><span class="sxs-lookup"><span data-stu-id="0f0a2-139">This warning appears when the environment variable `PATH` doesn't point to any .NET Core SDKs on the machine.</span></span> <span data-ttu-id="0f0a2-140">Aby rozwiązać ten problem:</span><span class="sxs-lookup"><span data-stu-id="0f0a2-140">To resolve this problem:</span></span>

* <span data-ttu-id="0f0a2-141">Zainstaluj lub sprawdź, czy jest zainstalowany zestaw SDK .NET Core.</span><span class="sxs-lookup"><span data-stu-id="0f0a2-141">Install or verify the .NET Core SDK is installed.</span></span>
* <span data-ttu-id="0f0a2-142">Sprawdź `PATH` zmiennej środowiskowej wskazuje lokalizację, jest zainstalowany zestaw SDK.</span><span class="sxs-lookup"><span data-stu-id="0f0a2-142">Verify the `PATH` environment variable points to the location the SDK is installed.</span></span> <span data-ttu-id="0f0a2-143">Instalator zwykle ustawia `PATH`.</span><span class="sxs-lookup"><span data-stu-id="0f0a2-143">The installer normally sets the `PATH`.</span></span>

::: moniker range=">= aspnetcore-2.1"

### <a name="use-of-ihtmlhelperpartial-may-result-in-app-deadlocks"></a><span data-ttu-id="0f0a2-144">Użyj IHtmlHelper.Partial może spowodować zakleszczenie aplikacji</span><span class="sxs-lookup"><span data-stu-id="0f0a2-144">Use of IHtmlHelper.Partial may result in app deadlocks</span></span>

<span data-ttu-id="0f0a2-145">Wywołanie platformy ASP.NET Core 2.1 i nowsze, `Html.Partial` powoduje analizatora ostrzeżenie ze względu na możliwe zakleszczenie.</span><span class="sxs-lookup"><span data-stu-id="0f0a2-145">In ASP.NET Core 2.1 and later, calling `Html.Partial` results in an analyzer warning due to the potential for deadlocks.</span></span> <span data-ttu-id="0f0a2-146">Komunikat ostrzegawczy jest:</span><span class="sxs-lookup"><span data-stu-id="0f0a2-146">The warning message is:</span></span>

> <span data-ttu-id="0f0a2-147">Użyj IHtmlHelper.Partial może spowodować zakleszczenie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0f0a2-147">Use of IHtmlHelper.Partial may result in application deadlocks.</span></span> <span data-ttu-id="0f0a2-148">Należy rozważyć użycie `<partial>` pomocnika tagów lub `IHtmlHelper.PartialAsync`.</span><span class="sxs-lookup"><span data-stu-id="0f0a2-148">Consider using `<partial>` Tag Helper or `IHtmlHelper.PartialAsync`.</span></span>

<span data-ttu-id="0f0a2-149">Wywołuje się `@Html.Partial` powinna zostać zastąpiona `@await Html.PartialAsync` lub pomocnika częściowe tagu `<partial name="_Partial" />`.</span><span class="sxs-lookup"><span data-stu-id="0f0a2-149">Calls to `@Html.Partial` should be replaced by `@await Html.PartialAsync` or the partial tag helper `<partial name="_Partial" />`.</span></span>

::: moniker-end
