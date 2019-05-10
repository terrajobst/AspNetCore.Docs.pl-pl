---
title: Narzędzia diagnostyki wydajności
author: mjrousos
description: Opis przydatnych narzędzi do diagnozowania problemów z wydajnością w aplikacji platformy ASP.NET Core.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.date: 04/11/2019
uid: performance/diagnostic-tools
ms.openlocfilehash: 66676b5a2b95b87bfbbd50022e279e35a12b9793
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/27/2019
ms.locfileid: "64900382"
---
# <a name="performance-diagnostic-tools"></a>Narzędzia diagnostyczne wydajności

Przez [Mike Rousos](https://github.com/mjrousos)

W tym artykule wymieniono narzędzia do diagnozowania problemów z wydajnością w programie ASP.NET Core.

## <a name="visual-studio-diagnostic-tools"></a>Visual Studio Diagnostic Tools

[Profilowania i narzędzia diagnostyczne](/visualstudio/profiling) wbudowany w program Visual Studio są dobrym miejscem, aby rozpocząć badanie problemów z wydajnością. Narzędzia te są wydajne i wygodnie korzystać ze środowiska projektowego programu Visual Studio. Narzędzi umożliwia analizę użycia procesora CPU, użycie pamięci i zdarzenia dotyczące wydajności w aplikacji platformy ASP.NET Core. Wbudowane są sprawia, że łatwo profilowania w czasie projektowania.

Więcej informacji znajduje się w [dokumentację programu Visual Studio](/visualstudio/profiling/profiling-overview).

## <a name="application-insights"></a>Application Insights

[Usługa Application Insights](/azure/application-insights/app-insights-overview) zapewnia dane dotyczące wydajności szczegółowe dla aplikacji. Usługa Application Insights automatycznie zbiera dane o szybkości odpowiedzi, współczynniki błędów, zależności, czasy reakcji i. Usługa Application Insights obsługuje rejestrowanie niestandardowe zdarzenia i metryki określonych aplikacji.

Usługa Azure Application Insights oferuje wiele sposobów, aby podać szczegółowe informacje dla monitorowanej aplikacji:

- [Mapa aplikacji](/azure/application-insights/app-insights-app-map) — ułatwia wąskich gardeł wydajności dodatkowych lub chętnych hot niepowodzenie dotyczące wszystkich składników aplikacji rozproszonej.
- [Eksplorator metryk usługi Azure](/azure/azure-monitor/platform/metrics-getting-started) to składnik systemu Microsoft Azure portal, która umożliwia wykreślanie wykresów i wizualne korelowanie trendów, i badanie gwałtowne wzrosty i spadki wartości metryk.
- [Blok wydajność w portalu Application Insights](/azure/application-insights/app-insights-tutorial-performance):

  - Przedstawia szczegóły wydajności dla różnych operacji w monitorowanej aplikacji.
  - Umożliwia przechodzenie do szczegółów w jednej operacji, aby sprawdzić wszystkie części/zależności, składających się na długi czas.
  - Profiler może być wywoływany w tym miejscu, aby zebrać wydajności ślady na żądanie.

- [Usługa Azure Application Insights Profiler](/azure/azure-monitor/app/profiler) umożliwia regularne i na żądanie profilowanie aplikacji .NET.  Portalu platformy Azure zostanie wyświetlony komunikat przechwycone ślady wydajności za pomocą stosów wywołań i ścieżek krytycznych. Można również pobrać pliki śledzenia w celu przeprowadzenia dokładniejszej analizy, za pomocą narzędzia PerfView.

Usługa Application Insights może służyć w różnych środowiskach:

- Zoptymalizowana pod kątem pracy na platformie Azure.
- Działa w środowisku produkcyjnym, rozwoju i przemieszczania.
- Działa lokalnie z [programu Visual Studio](/azure/application-insights/app-insights-visual-studio) lub w innych środowiskach hostingu.

Aby uzyskać więcej informacji, zobacz [usługi Application Insights dla platformy ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).

## <a name="perfview"></a>Narzędzia PerfView

[Narzędzia PerfView](https://github.com/Microsoft/perfview) jest narzędzie do analizy wydajności utworzonych przez zespół .NET specjalnie do diagnozowania problemów z wydajnością .NET. Narzędzia PerfView pozwala na analizę Procesora użycia pamięci i GC zachowanie, zdarzenia dotyczące wydajności i czas zegarowy.

Dowiedz się więcej na temat narzędzia PerfView i jak rozpocząć pracę z [samouczki wideo narzędzia PerfView](http://channel9.msdn.com/Series/PerfView-Tutorial) lub, zapoznając się z podręcznika użytkownika dostępnych w narzędziu lub [w serwisie GitHub](https://github.com/Microsoft/perfview).

## <a name="windows-performance-toolkit"></a>Zestaw narzędzi wydajności Windows

[Zestaw narzędzi wydajności Windows](/windows-hardware/test/wpt/) (WPT) zawiera dwa składniki: Windows wydajności Rejestrator i Windows wydajności Analizator. Narzędzia tworzenia profilów szczegółowe wydajności systemów operacyjnych Windows i aplikacji. WPT ma bardziej rozbudowane sposoby wizualizowania danych, ale zbieranie danych jest mniej wydajne niż narzędzia PerfView firmy.

## <a name="perfcollect"></a>PerfCollect

Narzędzia PerfView jest narzędzie do analizy wydajności przydatne w scenariuszach .NET, go działa tylko w Windows, aby nie używać, aby zbierać ślady z aplikacji platformy ASP.NET Core uruchomiony w środowisku systemu Linux.

[PerfCollect](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/linux-performance-tracing.md) to skrypt powłoki bash, który używa natywnego systemu Linux, narzędzia profilowania ([wydajności](https://perf.wiki.kernel.org/index.php/Main_Page) i [LTTng](https://lttng.org/)) zbierać dane śledzenia w systemie Linux, które mogą być analizowane przez narzędzia PerfView. PerfCollect jest przydatne, gdy problemy z wydajnością, pojawiają się w środowiskach Linux, gdzie narzędzia PerfView nie można używać bezpośrednio. Zamiast tego PerfCollect może zbierać ślady z aplikacji platformy .NET Core, które są następnie analizowane na komputerze Windows za pomocą narzędzia PerfView.

Więcej informacji na temat sposobu instalowania i rozpoczynanie pracy z usługą PerfCollect jest dostępna [w serwisie GitHub](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/linux-performance-tracing.md).

## <a name="other-third-party-performance-tools"></a>Inne narzędzia innych firm wydajności

Poniższa lista zawiera niektóre narzędzia wydajność innych firm, które są przydatne w badaniu wydajności aplikacji .NET Core.

- [MiniProfiler](https://miniprofiler.com/)
- dotTrace i dotMemory firmy JetBrains
- VTune Intel
