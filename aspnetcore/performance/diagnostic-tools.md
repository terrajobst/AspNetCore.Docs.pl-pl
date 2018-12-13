---
title: Narzędzia diagnostyki wydajności
author: mjrousos
description: Opis przydatnych narzędzi do diagnozowania problemów z wydajnością w aplikacji platformy ASP.NET Core.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.date: 12/07/2018
uid: performance/diagnostic-tools
ms.openlocfilehash: 3093b7d646e4fa943334c7b1e70ddc007ab18780
ms.sourcegitcommit: 1ea1b4fc58055c62728143388562689f1ef96cb2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/13/2018
ms.locfileid: "53329214"
---
# <a name="performance-diagnostic-tools"></a><span data-ttu-id="c11ab-103">Narzędzia diagnostyczne wydajności</span><span class="sxs-lookup"><span data-stu-id="c11ab-103">Performance Diagnostic Tools</span></span>

<span data-ttu-id="c11ab-104">Przez [Mike Rousos](https://github.com/mjrousos)</span><span class="sxs-lookup"><span data-stu-id="c11ab-104">By [Mike Rousos](https://github.com/mjrousos)</span></span>

<span data-ttu-id="c11ab-105">W tym artykule wymieniono narzędzia do diagnozowania problemów z wydajnością w programie ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c11ab-105">This article lists tools for diagnosing performance issues in ASP.NET Core.</span></span>

## <a name="visual-studio-diagnostic-tools"></a><span data-ttu-id="c11ab-106">Narzędzia diagnostyczne Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c11ab-106">Visual Studio Diagnostic Tools</span></span>

<span data-ttu-id="c11ab-107">[Profilowania i narzędzia diagnostyczne](/visualstudio/profiling) wbudowany w program Visual Studio są dobrym miejscem, aby rozpocząć badanie problemów z wydajnością.</span><span class="sxs-lookup"><span data-stu-id="c11ab-107">The [profiling and diagnostic tools](/visualstudio/profiling) built into Visual Studio are a good place to start investigating performance issues.</span></span> <span data-ttu-id="c11ab-108">Narzędzia te są wydajne i wygodnie korzystać ze środowiska projektowego programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c11ab-108">These tools are powerful and convenient to use from the Visual Studio development environment.</span></span> <span data-ttu-id="c11ab-109">Narzędzi umożliwia analizę użycia procesora CPU, użycie pamięci i zdarzenia dotyczące wydajności w aplikacji platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c11ab-109">The tooling allows analysis of CPU usage, memory usage, and performance events in ASP.NET Core apps.</span></span>

<span data-ttu-id="c11ab-110">Więcej informacji znajduje się w [dokumentację programu Visual Studio](/visualstudio/profiling/profiling-overview).</span><span class="sxs-lookup"><span data-stu-id="c11ab-110">More information is available in [Visual Studio documentation](/visualstudio/profiling/profiling-overview).</span></span>

## <a name="application-insights"></a><span data-ttu-id="c11ab-111">Application Insights</span><span class="sxs-lookup"><span data-stu-id="c11ab-111">Application Insights</span></span>

<span data-ttu-id="c11ab-112">[Usługa Application Insights](/azure/application-insights/app-insights-overview) zapewnia dane dotyczące wydajności szczegółowe dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c11ab-112">[Application Insights](/azure/application-insights/app-insights-overview) provides in-depth performance data for your app.</span></span> <span data-ttu-id="c11ab-113">Usługa Application Insights automatycznie zbiera dane o szybkości odpowiedzi, współczynniki błędów, zależności, czasy reakcji i.</span><span class="sxs-lookup"><span data-stu-id="c11ab-113">Application Insights automatically collects data on response rates, failure rates, dependency response times, and more.</span></span> <span data-ttu-id="c11ab-114">Usługa Application Insights obsługuje rejestrowanie niestandardowe zdarzenia i metryki określonych aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c11ab-114">Application Insights supports logging custom events and metrics specific to your app.</span></span>

<span data-ttu-id="c11ab-115">Usługa Application Insights może służyć w różnych środowiskach:</span><span class="sxs-lookup"><span data-stu-id="c11ab-115">Application Insights can be used in a variety environments:</span></span>

* <span data-ttu-id="c11ab-116">Zoptymalizowana pod kątem pracy na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="c11ab-116">Optimized to work in Azure.</span></span>
* <span data-ttu-id="c11ab-117">Działa w środowisku produkcyjnym, rozwoju i przemieszczania.</span><span class="sxs-lookup"><span data-stu-id="c11ab-117">Works in production, development, and staging.</span></span>
* <span data-ttu-id="c11ab-118">Działa lokalnie z [programu Visual Studio](/azure/application-insights/app-insights-visual-studio) lub w innych środowiskach hostingu.</span><span class="sxs-lookup"><span data-stu-id="c11ab-118">Works locally from [Visual Studio](/azure/application-insights/app-insights-visual-studio) or in other hosting environments.</span></span>

<span data-ttu-id="c11ab-119">Aby uzyskać więcej informacji, zobacz [usługi Application Insights dla platformy ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).</span><span class="sxs-lookup"><span data-stu-id="c11ab-119">For more information, see [Application Insights for ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="perfview"></a><span data-ttu-id="c11ab-120">Narzędzia PerfView</span><span class="sxs-lookup"><span data-stu-id="c11ab-120">PerfView</span></span>

<span data-ttu-id="c11ab-121">[Narzędzia PerfView](https://github.com/Microsoft/perfview) jest narzędzie do analizy wydajności utworzonych przez zespół .NET specjalnie do diagnozowania problemów z wydajnością .NET.</span><span class="sxs-lookup"><span data-stu-id="c11ab-121">[PerfView](https://github.com/Microsoft/perfview) is a performance analysis tool created by the .NET team specifically for diagnosing .NET performance issues.</span></span> <span data-ttu-id="c11ab-122">Narzędzia PerfView pozwala na analizę Procesora użycia pamięci i GC zachowanie, zdarzenia dotyczące wydajności i czas zegarowy.</span><span class="sxs-lookup"><span data-stu-id="c11ab-122">PerfView allows analysis of CPU usage, memory and GC behavior, performance events, and wall clock time.</span></span>

<span data-ttu-id="c11ab-123">Dowiedz się więcej na temat narzędzia PerfView i jak rozpocząć pracę z [samouczki wideo narzędzia PerfView](http://channel9.msdn.com/Series/PerfView-Tutorial) lub, zapoznając się z podręcznika użytkownika dostępnych w narzędziu lub [w serwisie GitHub](https://github.com/Microsoft/perfview).</span><span class="sxs-lookup"><span data-stu-id="c11ab-123">You can learn more about PerfView and how to get started with [PerfView video tutorials](http://channel9.msdn.com/Series/PerfView-Tutorial) or by reading the user's guide available in the tool or [on GitHub](https://github.com/Microsoft/perfview).</span></span>

## <a name="perfcollect"></a><span data-ttu-id="c11ab-124">PerfCollect</span><span class="sxs-lookup"><span data-stu-id="c11ab-124">PerfCollect</span></span>

<span data-ttu-id="c11ab-125">Narzędzia PerfView jest narzędzie do analizy wydajności przydatne w scenariuszach .NET, go działa tylko w Windows, aby nie używać, aby zbierać ślady z aplikacji platformy ASP.NET Core uruchomiony w środowisku systemu Linux.</span><span class="sxs-lookup"><span data-stu-id="c11ab-125">While PerfView is a useful performance analysis tool for .NET scenarios, it only runs on Windows so you can't use it to collect traces from ASP.NET Core apps running in Linux environments.</span></span>

<span data-ttu-id="c11ab-126">[PerfCollect](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/linux-performance-tracing.md) to skrypt powłoki bash, który używa natywnego systemu Linux, narzędzia profilowania ([wydajności](https://perf.wiki.kernel.org/index.php/Main_Page) i [LTTng](https://lttng.org/)) zbierać dane śledzenia w systemie Linux, które mogą być analizowane przez narzędzia PerfView.</span><span class="sxs-lookup"><span data-stu-id="c11ab-126">[PerfCollect](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/linux-performance-tracing.md) is a bash script that uses native Linux profiling tools ([Perf](https://perf.wiki.kernel.org/index.php/Main_Page) and [LTTng](https://lttng.org/)) to collect traces on Linux that can be analyzed by PerfView.</span></span> <span data-ttu-id="c11ab-127">PerfCollect jest przydatne, gdy problemy z wydajnością, pojawiają się w środowiskach Linux, gdzie narzędzia PerfView nie można używać bezpośrednio.</span><span class="sxs-lookup"><span data-stu-id="c11ab-127">PerfCollect is useful when performance problems show up in Linux environments where PerfView can't be used directly.</span></span> <span data-ttu-id="c11ab-128">Zamiast tego PerfCollect może zbierać ślady z aplikacji platformy .NET Core, które są następnie analizowane na komputerze Windows za pomocą narzędzia PerfView.</span><span class="sxs-lookup"><span data-stu-id="c11ab-128">Instead, PerfCollect can collect traces from .NET Core apps that are then analyzed on a Windows computer using PerfView.</span></span>

<span data-ttu-id="c11ab-129">Więcej informacji na temat sposobu instalowania i rozpoczynanie pracy z usługą PerfCollect jest dostępna [w serwisie GitHub](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/linux-performance-tracing.md).</span><span class="sxs-lookup"><span data-stu-id="c11ab-129">More information about how to install and get started with PerfCollect is available [on GitHub](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/linux-performance-tracing.md).</span></span>
