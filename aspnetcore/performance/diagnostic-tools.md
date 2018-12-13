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
# <a name="performance-diagnostic-tools"></a>Narzędzia diagnostyczne wydajności

Przez [Mike Rousos](https://github.com/mjrousos)

W tym artykule wymieniono narzędzia do diagnozowania problemów z wydajnością w programie ASP.NET Core.

## <a name="visual-studio-diagnostic-tools"></a>Narzędzia diagnostyczne Visual Studio

[Profilowania i narzędzia diagnostyczne](/visualstudio/profiling) wbudowany w program Visual Studio są dobrym miejscem, aby rozpocząć badanie problemów z wydajnością. Narzędzia te są wydajne i wygodnie korzystać ze środowiska projektowego programu Visual Studio. Narzędzi umożliwia analizę użycia procesora CPU, użycie pamięci i zdarzenia dotyczące wydajności w aplikacji platformy ASP.NET Core.

Więcej informacji znajduje się w [dokumentację programu Visual Studio](/visualstudio/profiling/profiling-overview).

## <a name="application-insights"></a>Application Insights

[Usługa Application Insights](/azure/application-insights/app-insights-overview) zapewnia dane dotyczące wydajności szczegółowe dla aplikacji. Usługa Application Insights automatycznie zbiera dane o szybkości odpowiedzi, współczynniki błędów, zależności, czasy reakcji i. Usługa Application Insights obsługuje rejestrowanie niestandardowe zdarzenia i metryki określonych aplikacji.

Usługa Application Insights może służyć w różnych środowiskach:

* Zoptymalizowana pod kątem pracy na platformie Azure.
* Działa w środowisku produkcyjnym, rozwoju i przemieszczania.
* Działa lokalnie z [programu Visual Studio](/azure/application-insights/app-insights-visual-studio) lub w innych środowiskach hostingu.

Aby uzyskać więcej informacji, zobacz [usługi Application Insights dla platformy ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).

## <a name="perfview"></a>Narzędzia PerfView

[Narzędzia PerfView](https://github.com/Microsoft/perfview) jest narzędzie do analizy wydajności utworzonych przez zespół .NET specjalnie do diagnozowania problemów z wydajnością .NET. Narzędzia PerfView pozwala na analizę Procesora użycia pamięci i GC zachowanie, zdarzenia dotyczące wydajności i czas zegarowy.

Dowiedz się więcej na temat narzędzia PerfView i jak rozpocząć pracę z [samouczki wideo narzędzia PerfView](http://channel9.msdn.com/Series/PerfView-Tutorial) lub, zapoznając się z podręcznika użytkownika dostępnych w narzędziu lub [w serwisie GitHub](https://github.com/Microsoft/perfview).

## <a name="perfcollect"></a>PerfCollect

Narzędzia PerfView jest narzędzie do analizy wydajności przydatne w scenariuszach .NET, go działa tylko w Windows, aby nie używać, aby zbierać ślady z aplikacji platformy ASP.NET Core uruchomiony w środowisku systemu Linux.

[PerfCollect](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/linux-performance-tracing.md) to skrypt powłoki bash, który używa natywnego systemu Linux, narzędzia profilowania ([wydajności](https://perf.wiki.kernel.org/index.php/Main_Page) i [LTTng](https://lttng.org/)) zbierać dane śledzenia w systemie Linux, które mogą być analizowane przez narzędzia PerfView. PerfCollect jest przydatne, gdy problemy z wydajnością, pojawiają się w środowiskach Linux, gdzie narzędzia PerfView nie można używać bezpośrednio. Zamiast tego PerfCollect może zbierać ślady z aplikacji platformy .NET Core, które są następnie analizowane na komputerze Windows za pomocą narzędzia PerfView.

Więcej informacji na temat sposobu instalowania i rozpoczynanie pracy z usługą PerfCollect jest dostępna [w serwisie GitHub](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/linux-performance-tracing.md).
