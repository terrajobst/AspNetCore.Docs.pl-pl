---
title: Monitorowania i debugowania - DevOps z platformą ASP.NET Core i platformy Azure
author: CamSoper
description: Monitorowania i debugowania kodu jako części rozwiązania DevOps z platformą ASP.NET Core i platformy Azure
ms.author: casoper
ms.custom: mvc, seodec18
ms.date: 07/10/2019
uid: azure/devops/monitor
ms.openlocfilehash: 1d8ed99f4387dbc99929164c558cc2ce14bd9ea0
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78659503"
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

1. Otwórz [Azure Portal](https://portal.azure.com), a następnie przejdź do *mywebapp\<unique_number\>* App Service.

1. Karta **Przegląd** przedstawia przydatne informacje, w tym Wykresy zawierające ostatnie metryki.

    ![Zrzut ekranu przedstawiający Przegląd panelu](./media/monitoring/overview.png)

    * **Http 5xx**: liczba błędów po stronie serwera, zazwyczaj wyjątków w kodzie ASP.NET Core.
    * **Dane w**: dane przychodzące z aplikacji sieci Web.
    * **Dane wyjściowe: dane wychodzące**z aplikacji sieci Web do klientów.
    * **Żądania**: liczba żądań HTTP.
    * **Średni czas odpowiedzi**: Średni czas odpowiedzi aplikacji sieci Web na żądania HTTP.

    Kilka Samoobsługowe narzędzia do optymalizacji i rozwiązywania problemów znajdują się również na tej stronie.

    ![Zrzut ekranu przedstawiający Samoobsługowe narzędzia](./media/monitoring/wizards.png)

    * **Diagnozowanie i rozwiązywanie problemów** to samoobsługowe Rozwiązywanie problemów.
    * **Application Insights** służy do profilowania wydajności i zachowania aplikacji, a następnie omówiono w dalszej części tej sekcji.
    * **App Service Advisor** udostępnia zalecenia dotyczące dostrajania środowiska aplikacji.

## <a name="advanced-monitoring"></a>Zaawansowane monitorowanie

[Azure monitor](/azure/monitoring-and-diagnostics/) to scentralizowana usługa do monitorowania wszystkich metryk i ustawiania alertów w ramach usług platformy Azure. W ramach usługi Azure Monitor Administratorzy mogą szczegółowego śledzenia wydajności i identyfikować trendy. Każda usługa platformy Azure oferuje swój własny [zestaw metryk](/azure/monitoring-and-diagnostics/monitoring-supported-metrics#microsoftwebsites-excluding-functions) do Azure monitor.

## <a name="profile-with-application-insights"></a>Profil z usługi Application Insights

[Application Insights](/azure/application-insights/app-insights-overview) to usługa platformy Azure do analizowania wydajności i stabilności aplikacji sieci Web oraz sposobu ich używania przez użytkowników. Dane z usługi Application Insights jest szerszy i głębiej niż w przypadku usługi Azure Monitor. Danych może zapewnić deweloperom i administratorom podstawowych informacji w celu podniesienia jakości aplikacji. Można dodać usługę Application Insights z zasobem usługi Azure App Service bez zmian w kodzie.

1. Otwórz [Azure Portal](https://portal.azure.com), a następnie przejdź do *mywebapp\<unique_number\>* App Service.
1. Na karcie **Omówienie** kliknij kafelek **Application Insights** .

    ![Kafelek Application Insights](./media/monitoring/app-insights.png)

1. Wybierz przycisk radiowy **Utwórz nowy zasób** . Użyj domyślnej nazwy zasobów, a następnie wybierz lokalizację dla zasobu usługi Application Insights. Lokalizacja nie muszą być zgodne z aplikacją sieci web.

    ![Application Insights instalacji](./media/monitoring/new-app-insights.png)

1. Dla **środowiska uruchomieniowego/platformy**wybierz pozycję **ASP.NET Core**. Zaakceptuj ustawienia domyślne.
1. Kliknij przycisk **OK**. Jeśli zostanie wyświetlony monit o potwierdzenie, wybierz pozycję **Kontynuuj**.
1. Po utworzeniu zasobu kliknij nazwę zasobu usługi Application Insights, aby przejść bezpośrednio do strony usługi Application Insights.

    ![Nowy zasób usługi Application Insights jest gotowy](./media/monitoring/new-app-insights-done.png)

Dane są gromadzone w przypadku użycia aplikacji. Wybierz pozycję **Odśwież** , aby ponownie załadować blok przy użyciu nowych danych.

![Karta Przegląd szczegółowych informacji w aplikacji](./media/monitoring/app-insights-overview.png)

Application Insights udostępnia przydatne informacje po stronie serwera bez konieczności dodatkowej konfiguracji. Aby uzyskać największą wartość z Application Insights, [Instrumentacja aplikacji za pomocą zestawu Application Insights SDK](/azure/application-insights/app-insights-asp-net-core). Po poprawnym skonfigurowaniu udostępnianych przez usługę monitorowania end-to-end, w przeglądarce, w tym wydajności po stronie klienta i serwera sieci web. Aby uzyskać więcej informacji, zobacz [dokumentację Application Insights](/azure/application-insights/app-insights-overview).

## <a name="logging"></a>Rejestrowanie

Dzienniki serwera i aplikacji sieci Web są wyłączone domyślnie w usłudze Azure App Service. Włącz dzienniki wykonując następujące kroki:

1. Otwórz [Azure Portal](https://portal.azure.com)i przejdź do *mywebapp\<unique_number\>* App Service.
1. W menu po lewej stronie przewiń w dół do sekcji **monitorowanie** . Wybierz pozycję **dzienniki diagnostyczne**.

    ![Link do dzienników diagnostycznych](./media/monitoring/logging.png)

1. Włącz **Rejestrowanie aplikacji (system plików)** . Jeśli zostanie wyświetlony monit, kliknij pole, aby zainstalować rozszerzenia, aby włączyć rejestrowanie w aplikacji sieci web aplikacji.
1. Ustaw **rejestrowanie serwera sieci Web** w **systemie plików**.
1. Wprowadź **okres przechowywania** w dniach. Na przykład 30.
1. Kliknij przycisk **Save** (Zapisz).

Dzienniki serwera (usługa App Service) platformy ASP.NET Core i sieci web są generowane dla aplikacji sieci web. Mogą być pobierane przy użyciu protokołu FTP/FTPS wyświetlane informacje. Hasło jest taka sama jak poświadczenia wdrażania utworzone wcześniej w tym przewodniku. Dzienniki mogą być [przesyłane strumieniowo bezpośrednio do komputera lokalnego przy użyciu programu PowerShell lub interfejsu wiersza polecenia platformy Azure](/azure/app-service/web-sites-enable-diagnostic-log#download). Dzienniki mogą być również [wyświetlane w Application Insights](/azure/app-service/web-sites-enable-diagnostic-log#how-to-view-logs-in-application-insights).

## <a name="log-streaming"></a>Przesyłanie strumieniowe dzienników

Dzienniki serwera sieci web i aplikacji, może być przesyłany strumieniowo w czasie rzeczywistym za pośrednictwem portalu.

1. Otwórz [Azure Portal](https://portal.azure.com)i przejdź do *mywebapp\<unique_number\>* App Service.
1. W menu po lewej stronie przewiń w dół do sekcji **monitorowanie** i wybierz pozycję **strumień dzienników**.

    ![Zrzut ekranu przedstawiający dziennika strumienia link](./media/monitoring/log-stream.png)

Dzienniki mogą być [przesyłane strumieniowo za pośrednictwem interfejsu wiersza polecenia platformy Azure lub Azure PowerShell](/azure/app-service/web-sites-enable-diagnostic-log#streamlogs), w tym Cloud Shell.

## <a name="alerts"></a>Alerty

Azure Monitor również zawiera [alerty w czasie rzeczywistym](/azure/monitoring-and-diagnostics/insights-alerts-portal) na podstawie metryk, zdarzeń administracyjnych i innych kryteriów.

> *Uwaga: obecnie alerty dotyczące metryk aplikacji sieci Web są dostępne tylko w usłudze alertów (klasycznych).*

[Usługę alertów (klasyczną)](/azure/monitoring-and-diagnostics/monitor-quick-resource-metric-alert-portal) można znaleźć w sekcji Azure monitor lub w obszarze **monitorowanie** ustawień App Service.

![Alerty (klasyczne) link](./media/monitoring/alerts.png)

## <a name="live-debugging"></a>Debugowanie na żywo

Azure App Service może być [debugowany zdalnie z programem Visual Studio](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio#remotedebug) , gdy dzienniki nie zapewniają wystarczającej ilości informacji. Zdalne debugowanie wymaga jednak wykonania aplikacji można skompilować przy użyciu symboli debugowania. Debugowanie nie powinny odbywać się w środowisku produkcyjnym, z wyjątkiem w ostateczności.

## <a name="conclusion"></a>Podsumowanie

W tej sekcji należy wykonać następujące zadania:

* Znajdź podstawowe monitorowanie i rozwiązywanie problemów z danych w witrynie Azure portal
* Dowiedz się, jak usługa Azure Monitor udostępnia lepiej poznać metryki dla wszystkich usług platformy Azure
* Łączenie aplikacji sieci web za pomocą usługi Application Insights dla profilowanie aplikacji
* Włącz rejestrowanie i Dowiedz się, skąd pobrać dzienniki
* Stream dzienników w czasie rzeczywistym
* Dowiedz się, gdzie można skonfigurować alerty
* Informacje o zdalnym debugowaniu aplikacji sieci web usługi Azure App Service.

## <a name="additional-reading"></a>Materiały uzupełniające

* <xref:test/troubleshoot-azure-iis>
* <xref:host-and-deploy/azure-iis-errors-reference>
* [Monitorowanie wydajności aplikacji sieci Web platformy Azure za pomocą Application Insights](/azure/application-insights/app-insights-azure-web-apps)
* [Włączanie rejestrowania diagnostycznego dla aplikacji sieci Web w programie Azure App Service](/azure/app-service/web-sites-enable-diagnostic-log)
* [Rozwiązywanie problemów z aplikacją sieci Web w Azure App Service przy użyciu programu Visual Studio](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio)
* [Tworzenie klasycznych alertów metryk w Azure Monitor dla usług platformy Azure — Azure Portal](/azure/monitoring-and-diagnostics/insights-alerts-portal)
