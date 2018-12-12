---
title: Monitorowania i debugowania - DevOps z platformą ASP.NET Core i platformy Azure
author: CamSoper
description: Monitorowania i debugowania kodu jako części rozwiązania DevOps z platformą ASP.NET Core i platformy Azure
ms.author: casoper
ms.custom: mvc, seodec18
ms.date: 10/24/2018
uid: azure/devops/monitor
ms.openlocfilehash: 00489bd92dfff8fd80bd24c2e60193d32031d7c4
ms.sourcegitcommit: b34b25da2ab68e6495b2460ff570468f16a9bf0d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53284373"
---
# <a name="monitor-and-debug"></a>Monitorowanie i debugowania

Po wdrożeniu aplikacji i zbudowany potok DevOps, ważne jest, aby dowiedzieć się, jak monitorowanie i rozwiązywanie problemów z aplikacją.

W tej sekcji zostaną wykonane następujące zadania:

* Znajdź podstawowe monitorowanie i rozwiązywanie problemów z danych w witrynie Azure portal
* Dowiedz się, jak usługa Azure Monitor udostępnia lepiej poznać metryki dla wszystkich usług platformy Azure
* Łączenie aplikacji sieci web za pomocą usługi Application Insights dla profilowanie aplikacji
* Włącz rejestrowanie i Dowiedz się, skąd pobrać dzienniki
* Stream dzienników w czasie rzeczywistym
* Dowiedz się, gdzie można skonfigurować alerty
* Informacje o zdalnym debugowaniu aplikacji sieci web usługi Azure App Service.

## <a name="basic-monitoring-and-troubleshooting"></a>Podstawowe monitorowanie i rozwiązywanie problemów

Aplikacje sieci web usługi App Service są łatwo monitorowana w czasie rzeczywistym. Witryna Azure portal renderuje metryki w łatwych do zrozumienia wykresów i schematów.

1. Otwórz [witryny Azure portal](https://portal.azure.com), a następnie przejdź do *mywebapp\<unique_number\>*  usługi App Service.

1. **Przegląd** karcie są wyświetlane użyteczne informacje "w skrócie", w tym wykresy wyświetlane ostatnie metryki.

    ![Zrzut ekranu przedstawiający Przegląd panelu](./media/monitoring/overview.png)

    * **Http 5xx**: Liczba błędów po stronie serwera, zwykle wyjątków w kodzie ASP.NET Core.
    * **Dane w**: Transfer danych przychodzących do aplikacji sieci web.
    * **Dane wyjściowe**: Wyjście danych z aplikacji sieci web dla klientów.
    * **Żądania**: Liczba HTTP żądania.
    * **Średni czas odpowiedzi**: Średni czas dla aplikacji sieci web do odpowiadania na żądania HTTP.

    Kilka Samoobsługowe narzędzia do optymalizacji i rozwiązywania problemów znajdują się również na tej stronie.

    ![Zrzut ekranu przedstawiający Samoobsługowe narzędzia](./media/monitoring/wizards.png)

    * **Diagnozowanie i rozwiązywanie problemów** jest rozwiązywanie problemów z samoobsługi.
    * **Usługa Application Insights** jest profilowania wydajności i zachowań aplikacji, a następnie omówiono w dalszej części w tej sekcji.
    * **App Service Advisor** sprawia, że zalecenia dotyczące dostrajania doskonałe działanie aplikacji.

## <a name="advanced-monitoring"></a>Zaawansowane monitorowanie

[Usługa Azure Monitor](/azure/monitoring-and-diagnostics/) jest usługa scentralizowanego monitorowania wszystkich metryk i ustawianie alertów w usługach platformy Azure. W ramach usługi Azure Monitor Administratorzy mogą szczegółowego śledzenia wydajności i identyfikować trendy. Każda z usług Azure oferuje swój własny [zestaw metryk](/azure/monitoring-and-diagnostics/monitoring-supported-metrics#microsoftwebsites-excluding-functions) do usługi Azure Monitor.

## <a name="profile-with-application-insights"></a>Profil z usługi Application Insights

[Usługa Application Insights](/azure/application-insights/app-insights-overview) jest usługą platformy Azure do analizowania wydajności i stabilności aplikacji sieci web i jak użytkownicy z nich korzystać. Dane z usługi Application Insights jest szerszy i głębiej niż w przypadku usługi Azure Monitor. Danych może zapewnić deweloperom i administratorom podstawowych informacji w celu podniesienia jakości aplikacji. Można dodać usługę Application Insights z zasobem usługi Azure App Service bez zmian w kodzie.

1. Otwórz [witryny Azure portal](https://portal.azure.com), a następnie przejdź do *mywebapp\<unique_number\>*  usługi App Service.
1. Z **Przegląd** kliknij pozycję **usługi Application Insights** kafelka.

    ![Kafelek Application Insights](./media/monitoring/app-insights.png)

1. Wybierz **Tworzenie nowego zasobu** przycisku radiowego. Użyj domyślnej nazwy zasobów, a następnie wybierz lokalizację dla zasobu usługi Application Insights. Lokalizacja nie muszą być zgodne z aplikacją sieci web.

    ![Application Insights instalacji](./media/monitoring/new-app-insights.png)

1. Aby uzyskać **środowiska wykonawczego/struktury**, wybierz opcję **platformy ASP.NET Core**. Zaakceptuj ustawienia domyślne.
1. Kliknij przycisk **OK**. Jeśli zostanie wyświetlony monit, aby upewnić się, zaznaczyć **Kontynuuj**.
1. Po utworzeniu zasobu kliknij nazwę zasobu usługi Application Insights, aby przejść bezpośrednio do strony usługi Application Insights.

    ![Nowy zasób usługi Application Insights jest gotowy](./media/monitoring/new-app-insights-done.png)

Dane są gromadzone w przypadku użycia aplikacji. Wybierz **Odśwież** ponownie załadować bloku za pomocą nowych danych.

![Karta Przegląd szczegółowych informacji w aplikacji](./media/monitoring/app-insights-overview.png)

Application Insights udostępnia przydatne informacje po stronie serwera bez konieczności dodatkowej konfiguracji. Aby uzyskać największe korzyści z usługi Application Insights [instrumentacji aplikacji za pomocą zestawu SDK usługi Application Insights](/azure/application-insights/app-insights-asp-net-core). Po poprawnym skonfigurowaniu udostępnianych przez usługę monitorowania end-to-end, w przeglądarce, w tym wydajności po stronie klienta i serwera sieci web. Aby uzyskać więcej informacji, zobacz [dokumentacja usługi Application Insights](/azure/application-insights/app-insights-overview).

## <a name="logging"></a>Rejestrowanie

Dzienniki serwera i aplikacji sieci Web są wyłączone domyślnie w usłudze Azure App Service. Włącz dzienniki wykonując następujące kroki:

1. Otwórz [witryny Azure portal](https://portal.azure.com), a następnie przejdź do *mywebapp\<unique_number\>*  usługi App Service.
1. W menu po lewej stronie, przewiń w dół do **monitorowanie** sekcji. Wybierz **dzienniki diagnostyczne**.

    ![Link do dzienników diagnostycznych](./media/monitoring/logging.png)

1. Włącz **rejestrowanie aplikacji (system plików)**. Jeśli zostanie wyświetlony monit, kliknij pole, aby zainstalować rozszerzenia, aby włączyć rejestrowanie w aplikacji sieci web aplikacji.
1. Ustaw **rejestrowanie serwera sieci Web** do **System plików**.
1. Wprowadź **okres przechowywania** w dniach. Na przykład 30.
1. Kliknij pozycję **Zapisz**.

Dzienniki serwera (usługa App Service) platformy ASP.NET Core i sieci web są generowane dla aplikacji sieci web. Mogą być pobierane przy użyciu protokołu FTP/FTPS wyświetlane informacje. Hasło jest taka sama jak poświadczenia wdrażania utworzone wcześniej w tym przewodniku. Dzienniki mogą być [przesyłane strumieniowo bezpośrednio na komputerze lokalnym przy użyciu programu PowerShell lub wiersza polecenia platformy Azure](/azure/app-service/web-sites-enable-diagnostic-log#download). Dzienniki można też [wyświetlane w usłudze Application Insights](/azure/app-service/web-sites-enable-diagnostic-log#how-to-view-logs-in-application-insights).

## <a name="log-streaming"></a>Przesyłanie strumieniowe dzienników

Dzienniki serwera sieci web i aplikacji, może być przesyłany strumieniowo w czasie rzeczywistym za pośrednictwem portalu.

1. Otwórz [witryny Azure portal](https://portal.azure.com), a następnie przejdź do *mywebapp\<unique_number\>*  usługi App Service.
1. W menu po lewej stronie, przewiń w dół do **monitorowanie** i wybierz pozycję **strumień dziennika**.

    ![Zrzut ekranu przedstawiający dziennika strumienia link](./media/monitoring/log-stream.png)

Dzienniki można też [strumieniowo przy użyciu wiersza polecenia platformy Azure lub programu Azure PowerShell](/azure/app-service/web-sites-enable-diagnostic-log#streamlogs), w tym w usłudze Cloud Shell.

## <a name="alerts"></a>Alerty

Usługa Azure Monitor udostępnia również [alerty w czasie rzeczywistym](/azure/monitoring-and-diagnostics/insights-alerts-portal) na podstawie metryk, zdarzenia administracyjne i innych kryteriów.

> *Uwaga: Obecnie alertów dotyczących metryk aplikacji sieci web jest dostępna tylko w usłudze alerty (klasyczne).*

[Alerty (klasyczne) usługa](/azure/monitoring-and-diagnostics/monitor-quick-resource-metric-alert-portal) znajduje się w usłudze Azure Monitor lub w obszarze **monitorowanie** sekcji ustawień usługi App Service.

![Alerty (klasyczne) link](./media/monitoring/alerts.png)

## <a name="live-debugging"></a>Debugowanie na żywo

Usługa Azure App Service może być [zdalnie debugować za pomocą programu Visual Studio](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio#remotedebug) po dzienniki nie zawierają wystarczającej ilości informacji. Zdalne debugowanie wymaga jednak wykonania aplikacji można skompilować przy użyciu symboli debugowania. Debugowanie nie powinny odbywać się w środowisku produkcyjnym, z wyjątkiem w ostateczności.

## <a name="conclusion"></a>Wniosek

W tej sekcji należy wykonać następujące zadania:

* Znajdź podstawowe monitorowanie i rozwiązywanie problemów z danych w witrynie Azure portal
* Dowiedz się, jak usługa Azure Monitor udostępnia lepiej poznać metryki dla wszystkich usług platformy Azure
* Łączenie aplikacji sieci web za pomocą usługi Application Insights dla profilowanie aplikacji
* Włącz rejestrowanie i Dowiedz się, skąd pobrać dzienniki
* Stream dzienników w czasie rzeczywistym
* Dowiedz się, gdzie można skonfigurować alerty
* Informacje o zdalnym debugowaniu aplikacji sieci web usługi Azure App Service.

## <a name="additional-reading"></a>Materiały uzupełniające

* <xref:host-and-deploy/azure-apps/troubleshoot>
* <xref:host-and-deploy/azure-iis-errors-reference>
* [Monitorowanie wydajności aplikacji sieci web platformy Azure z usługą Application Insights](/azure/application-insights/app-insights-azure-web-apps)
* [Włączanie rejestrowania diagnostycznego dla aplikacji sieci web w usłudze Azure App Service](/azure/app-service/web-sites-enable-diagnostic-log)
* [Rozwiązywanie problemów z aplikacją sieci web w usłudze Azure App Service przy użyciu programu Visual Studio](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio)
* [Tworzenie klasycznego alertów dotyczących metryk w usłudze Azure Monitor dla usług platformy Azure — witryna Azure portal](/azure/monitoring-and-diagnostics/insights-alerts-portal)
