---
title: Monitorowania i debugowania - DevOps z platformą ASP.NET Core i platformy Azure
author: CamSoper
description: Monitorowania i debugowania kodu jako części rozwiązania DevOps z platformą ASP.NET Core i platformy Azure
ms.author: casoper
ms.custom: mvc, seodec18
ms.date: 10/24/2018
uid: azure/devops/monitor
ms.openlocfilehash: 00489bd92dfff8fd80bd24c2e60193d32031d7c4
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/27/2019
ms.locfileid: "64899380"
---
# <a name="monitor-and-debug"></a><span data-ttu-id="9b672-103">Monitorowanie i debugowania</span><span class="sxs-lookup"><span data-stu-id="9b672-103">Monitor and debug</span></span>

<span data-ttu-id="9b672-104">Po wdrożeniu aplikacji i zbudowany potok DevOps, ważne jest, aby dowiedzieć się, jak monitorowanie i rozwiązywanie problemów z aplikacją.</span><span class="sxs-lookup"><span data-stu-id="9b672-104">Having deployed the app and built a DevOps pipeline, it's important to understand how to monitor and troubleshoot the app.</span></span>

<span data-ttu-id="9b672-105">W tej sekcji zostaną wykonane następujące zadania:</span><span class="sxs-lookup"><span data-stu-id="9b672-105">In this section, you'll complete the following tasks:</span></span>

* <span data-ttu-id="9b672-106">Znajdź podstawowe monitorowanie i rozwiązywanie problemów z danych w witrynie Azure portal</span><span class="sxs-lookup"><span data-stu-id="9b672-106">Find basic monitoring and troubleshooting data in the Azure portal</span></span>
* <span data-ttu-id="9b672-107">Dowiedz się, jak usługa Azure Monitor udostępnia lepiej poznać metryki dla wszystkich usług platformy Azure</span><span class="sxs-lookup"><span data-stu-id="9b672-107">Learn how Azure Monitor provides a deeper look at metrics across all Azure services</span></span>
* <span data-ttu-id="9b672-108">Łączenie aplikacji sieci web za pomocą usługi Application Insights dla profilowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="9b672-108">Connect the web app with Application Insights for app profiling</span></span>
* <span data-ttu-id="9b672-109">Włącz rejestrowanie i Dowiedz się, skąd pobrać dzienniki</span><span class="sxs-lookup"><span data-stu-id="9b672-109">Turn on logging and learn where to download logs</span></span>
* <span data-ttu-id="9b672-110">Stream dzienników w czasie rzeczywistym</span><span class="sxs-lookup"><span data-stu-id="9b672-110">Stream logs in real time</span></span>
* <span data-ttu-id="9b672-111">Dowiedz się, gdzie można skonfigurować alerty</span><span class="sxs-lookup"><span data-stu-id="9b672-111">Learn where to set up alerts</span></span>
* <span data-ttu-id="9b672-112">Informacje o zdalnym debugowaniu aplikacji sieci web usługi Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="9b672-112">Learn about remote debugging Azure App Service web apps.</span></span>

## <a name="basic-monitoring-and-troubleshooting"></a><span data-ttu-id="9b672-113">Podstawowe monitorowanie i rozwiązywanie problemów</span><span class="sxs-lookup"><span data-stu-id="9b672-113">Basic monitoring and troubleshooting</span></span>

<span data-ttu-id="9b672-114">Aplikacje sieci web usługi App Service są łatwo monitorowana w czasie rzeczywistym.</span><span class="sxs-lookup"><span data-stu-id="9b672-114">App Service web apps are easily monitored in real time.</span></span> <span data-ttu-id="9b672-115">Witryna Azure portal renderuje metryki w łatwych do zrozumienia wykresów i schematów.</span><span class="sxs-lookup"><span data-stu-id="9b672-115">The Azure portal renders metrics in easy-to-understand charts and graphs.</span></span>

1. <span data-ttu-id="9b672-116">Otwórz [witryny Azure portal](https://portal.azure.com), a następnie przejdź do *mywebapp\<unique_number\>*  usługi App Service.</span><span class="sxs-lookup"><span data-stu-id="9b672-116">Open the [Azure portal](https://portal.azure.com), and then navigate to the *mywebapp\<unique_number\>* App Service.</span></span>

1. <span data-ttu-id="9b672-117">**Przegląd** karcie są wyświetlane użyteczne informacje "w skrócie", w tym wykresy wyświetlane ostatnie metryki.</span><span class="sxs-lookup"><span data-stu-id="9b672-117">The **Overview** tab displays useful "at-a-glance" information, including graphs displaying recent metrics.</span></span>

    ![Zrzut ekranu przedstawiający Przegląd panelu](./media/monitoring/overview.png)

    * <span data-ttu-id="9b672-119">**Http 5xx**: Liczba błędów po stronie serwera, zwykle wyjątków w kodzie ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="9b672-119">**Http 5xx**: Count of server-side errors, usually exceptions in ASP.NET Core code.</span></span>
    * <span data-ttu-id="9b672-120">**Dane w**: Transfer danych przychodzących do aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="9b672-120">**Data In**: Data ingress coming into your web app.</span></span>
    * <span data-ttu-id="9b672-121">**Dane wyjściowe**: Wyjście danych z aplikacji sieci web dla klientów.</span><span class="sxs-lookup"><span data-stu-id="9b672-121">**Data Out**: Data egress from your web app to clients.</span></span>
    * <span data-ttu-id="9b672-122">**Żądania**: Liczba HTTP żądania.</span><span class="sxs-lookup"><span data-stu-id="9b672-122">**Requests**: Count of HTTP requests.</span></span>
    * <span data-ttu-id="9b672-123">**Średni czas odpowiedzi**: Średni czas dla aplikacji sieci web do odpowiadania na żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="9b672-123">**Average Response Time**: Average time for the web app to respond to HTTP requests.</span></span>

    <span data-ttu-id="9b672-124">Kilka Samoobsługowe narzędzia do optymalizacji i rozwiązywania problemów znajdują się również na tej stronie.</span><span class="sxs-lookup"><span data-stu-id="9b672-124">Several self-service tools for troubleshooting and optimization are also found on this page.</span></span>

    ![Zrzut ekranu przedstawiający Samoobsługowe narzędzia](./media/monitoring/wizards.png)

    * <span data-ttu-id="9b672-126">**Diagnozowanie i rozwiązywanie problemów** jest rozwiązywanie problemów z samoobsługi.</span><span class="sxs-lookup"><span data-stu-id="9b672-126">**Diagnose and solve problems** is a self-service troubleshooter.</span></span>
    * <span data-ttu-id="9b672-127">**Usługa Application Insights** jest profilowania wydajności i zachowań aplikacji, a następnie omówiono w dalszej części w tej sekcji.</span><span class="sxs-lookup"><span data-stu-id="9b672-127">**Application Insights** is for profiling performance and app behavior, and is discussed later in this section.</span></span>
    * <span data-ttu-id="9b672-128">**App Service Advisor** sprawia, że zalecenia dotyczące dostrajania doskonałe działanie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9b672-128">**App Service Advisor** makes recommendations to tune your app experience.</span></span>

## <a name="advanced-monitoring"></a><span data-ttu-id="9b672-129">Zaawansowane monitorowanie</span><span class="sxs-lookup"><span data-stu-id="9b672-129">Advanced monitoring</span></span>

<span data-ttu-id="9b672-130">[Usługa Azure Monitor](/azure/monitoring-and-diagnostics/) jest usługa scentralizowanego monitorowania wszystkich metryk i ustawianie alertów w usługach platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="9b672-130">[Azure Monitor](/azure/monitoring-and-diagnostics/) is the centralized service for monitoring all metrics and setting alerts across Azure services.</span></span> <span data-ttu-id="9b672-131">W ramach usługi Azure Monitor Administratorzy mogą szczegółowego śledzenia wydajności i identyfikować trendy.</span><span class="sxs-lookup"><span data-stu-id="9b672-131">Within Azure Monitor, administrators can granularly track performance and identify trends.</span></span> <span data-ttu-id="9b672-132">Każda z usług Azure oferuje swój własny [zestaw metryk](/azure/monitoring-and-diagnostics/monitoring-supported-metrics#microsoftwebsites-excluding-functions) do usługi Azure Monitor.</span><span class="sxs-lookup"><span data-stu-id="9b672-132">Each Azure service offers its own [set of metrics](/azure/monitoring-and-diagnostics/monitoring-supported-metrics#microsoftwebsites-excluding-functions) to Azure Monitor.</span></span>

## <a name="profile-with-application-insights"></a><span data-ttu-id="9b672-133">Profil z usługi Application Insights</span><span class="sxs-lookup"><span data-stu-id="9b672-133">Profile with Application Insights</span></span>

<span data-ttu-id="9b672-134">[Usługa Application Insights](/azure/application-insights/app-insights-overview) jest usługą platformy Azure do analizowania wydajności i stabilności aplikacji sieci web i jak użytkownicy z nich korzystać.</span><span class="sxs-lookup"><span data-stu-id="9b672-134">[Application Insights](/azure/application-insights/app-insights-overview) is an Azure service for analyzing the performance and stability of web apps and how users use them.</span></span> <span data-ttu-id="9b672-135">Dane z usługi Application Insights jest szerszy i głębiej niż w przypadku usługi Azure Monitor.</span><span class="sxs-lookup"><span data-stu-id="9b672-135">The data from Application Insights is broader and deeper than that of Azure Monitor.</span></span> <span data-ttu-id="9b672-136">Danych może zapewnić deweloperom i administratorom podstawowych informacji w celu podniesienia jakości aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9b672-136">The data can provide developers and administrators with key information for improving apps.</span></span> <span data-ttu-id="9b672-137">Można dodać usługę Application Insights z zasobem usługi Azure App Service bez zmian w kodzie.</span><span class="sxs-lookup"><span data-stu-id="9b672-137">Application Insights can be added to an Azure App Service resource without code changes.</span></span>

1. <span data-ttu-id="9b672-138">Otwórz [witryny Azure portal](https://portal.azure.com), a następnie przejdź do *mywebapp\<unique_number\>*  usługi App Service.</span><span class="sxs-lookup"><span data-stu-id="9b672-138">Open the [Azure portal](https://portal.azure.com), and then navigate to the *mywebapp\<unique_number\>* App Service.</span></span>
1. <span data-ttu-id="9b672-139">Z **Przegląd** kliknij pozycję **usługi Application Insights** kafelka.</span><span class="sxs-lookup"><span data-stu-id="9b672-139">From the **Overview** tab, click the **Application Insights** tile.</span></span>

    ![Kafelek Application Insights](./media/monitoring/app-insights.png)

1. <span data-ttu-id="9b672-141">Wybierz **Tworzenie nowego zasobu** przycisku radiowego.</span><span class="sxs-lookup"><span data-stu-id="9b672-141">Select the **Create new resource** radio button.</span></span> <span data-ttu-id="9b672-142">Użyj domyślnej nazwy zasobów, a następnie wybierz lokalizację dla zasobu usługi Application Insights.</span><span class="sxs-lookup"><span data-stu-id="9b672-142">Use the default resource name, and select the location for the Application Insights resource.</span></span> <span data-ttu-id="9b672-143">Lokalizacja nie muszą być zgodne z aplikacją sieci web.</span><span class="sxs-lookup"><span data-stu-id="9b672-143">The location doesn't need to match that of your web app.</span></span>

    ![Application Insights instalacji](./media/monitoring/new-app-insights.png)

1. <span data-ttu-id="9b672-145">Aby uzyskać **środowiska wykonawczego/struktury**, wybierz opcję **platformy ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="9b672-145">For **Runtime/Framework**, select **ASP.NET Core**.</span></span> <span data-ttu-id="9b672-146">Zaakceptuj ustawienia domyślne.</span><span class="sxs-lookup"><span data-stu-id="9b672-146">Accept the default settings.</span></span>
1. <span data-ttu-id="9b672-147">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="9b672-147">Select **OK**.</span></span> <span data-ttu-id="9b672-148">Jeśli zostanie wyświetlony monit, aby upewnić się, zaznaczyć **Kontynuuj**.</span><span class="sxs-lookup"><span data-stu-id="9b672-148">If prompted to confirm, select **Continue**.</span></span>
1. <span data-ttu-id="9b672-149">Po utworzeniu zasobu kliknij nazwę zasobu usługi Application Insights, aby przejść bezpośrednio do strony usługi Application Insights.</span><span class="sxs-lookup"><span data-stu-id="9b672-149">After the resource has been created, click the name of Application Insights resource to navigate directly to the Application Insights page.</span></span>

    ![Nowy zasób usługi Application Insights jest gotowy](./media/monitoring/new-app-insights-done.png)

<span data-ttu-id="9b672-151">Dane są gromadzone w przypadku użycia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9b672-151">As the app is used, data accumulates.</span></span> <span data-ttu-id="9b672-152">Wybierz **Odśwież** ponownie załadować bloku za pomocą nowych danych.</span><span class="sxs-lookup"><span data-stu-id="9b672-152">Select **Refresh** to reload the blade with new data.</span></span>

![Karta Przegląd szczegółowych informacji w aplikacji](./media/monitoring/app-insights-overview.png)

<span data-ttu-id="9b672-154">Application Insights udostępnia przydatne informacje po stronie serwera bez konieczności dodatkowej konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="9b672-154">Application Insights provides useful server-side information with no additional configuration.</span></span> <span data-ttu-id="9b672-155">Aby uzyskać największe korzyści z usługi Application Insights [instrumentacji aplikacji za pomocą zestawu SDK usługi Application Insights](/azure/application-insights/app-insights-asp-net-core).</span><span class="sxs-lookup"><span data-stu-id="9b672-155">To get the most value from Application Insights, [instrument your app with the Application Insights SDK](/azure/application-insights/app-insights-asp-net-core).</span></span> <span data-ttu-id="9b672-156">Po poprawnym skonfigurowaniu udostępnianych przez usługę monitorowania end-to-end, w przeglądarce, w tym wydajności po stronie klienta i serwera sieci web.</span><span class="sxs-lookup"><span data-stu-id="9b672-156">When properly configured, the service provides end-to-end monitoring across the web server and browser, including client-side performance.</span></span> <span data-ttu-id="9b672-157">Aby uzyskać więcej informacji, zobacz [dokumentacja usługi Application Insights](/azure/application-insights/app-insights-overview).</span><span class="sxs-lookup"><span data-stu-id="9b672-157">For more information, see the [Application Insights documentation](/azure/application-insights/app-insights-overview).</span></span>

## <a name="logging"></a><span data-ttu-id="9b672-158">Rejestrowanie</span><span class="sxs-lookup"><span data-stu-id="9b672-158">Logging</span></span>

<span data-ttu-id="9b672-159">Dzienniki serwera i aplikacji sieci Web są wyłączone domyślnie w usłudze Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="9b672-159">Web server and app logs are disabled by default in Azure App Service.</span></span> <span data-ttu-id="9b672-160">Włącz dzienniki wykonując następujące kroki:</span><span class="sxs-lookup"><span data-stu-id="9b672-160">Enable the logs with the following steps:</span></span>

1. <span data-ttu-id="9b672-161">Otwórz [witryny Azure portal](https://portal.azure.com), a następnie przejdź do *mywebapp\<unique_number\>*  usługi App Service.</span><span class="sxs-lookup"><span data-stu-id="9b672-161">Open the [Azure portal](https://portal.azure.com), and navigate to the *mywebapp\<unique_number\>* App Service.</span></span>
1. <span data-ttu-id="9b672-162">W menu po lewej stronie, przewiń w dół do **monitorowanie** sekcji.</span><span class="sxs-lookup"><span data-stu-id="9b672-162">In the menu to the left, scroll down to the **Monitoring** section.</span></span> <span data-ttu-id="9b672-163">Wybierz **dzienniki diagnostyczne**.</span><span class="sxs-lookup"><span data-stu-id="9b672-163">Select **Diagnostics logs**.</span></span>

    ![Link do dzienników diagnostycznych](./media/monitoring/logging.png)

1. <span data-ttu-id="9b672-165">Włącz **rejestrowanie aplikacji (system plików)**.</span><span class="sxs-lookup"><span data-stu-id="9b672-165">Turn on **Application Logging (Filesystem)**.</span></span> <span data-ttu-id="9b672-166">Jeśli zostanie wyświetlony monit, kliknij pole, aby zainstalować rozszerzenia, aby włączyć rejestrowanie w aplikacji sieci web aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9b672-166">If prompted, click the box to install the extensions to enable app logging in the web app.</span></span>
1. <span data-ttu-id="9b672-167">Ustaw **rejestrowanie serwera sieci Web** do **System plików**.</span><span class="sxs-lookup"><span data-stu-id="9b672-167">Set **Web server logging** to **File System**.</span></span>
1. <span data-ttu-id="9b672-168">Wprowadź **okres przechowywania** w dniach.</span><span class="sxs-lookup"><span data-stu-id="9b672-168">Enter the **Retention Period** in days.</span></span> <span data-ttu-id="9b672-169">Na przykład 30.</span><span class="sxs-lookup"><span data-stu-id="9b672-169">For example, 30.</span></span>
1. <span data-ttu-id="9b672-170">Kliknij pozycję **Zapisz**.</span><span class="sxs-lookup"><span data-stu-id="9b672-170">Click **Save**.</span></span>

<span data-ttu-id="9b672-171">Dzienniki serwera (usługa App Service) platformy ASP.NET Core i sieci web są generowane dla aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="9b672-171">ASP.NET Core and web server (App Service) logs are generated for the web app.</span></span> <span data-ttu-id="9b672-172">Mogą być pobierane przy użyciu protokołu FTP/FTPS wyświetlane informacje.</span><span class="sxs-lookup"><span data-stu-id="9b672-172">They can be downloaded using the FTP/FTPS information displayed.</span></span> <span data-ttu-id="9b672-173">Hasło jest taka sama jak poświadczenia wdrażania utworzone wcześniej w tym przewodniku.</span><span class="sxs-lookup"><span data-stu-id="9b672-173">The password is the same as the deployment credentials created earlier in this guide.</span></span> <span data-ttu-id="9b672-174">Dzienniki mogą być [przesyłane strumieniowo bezpośrednio na komputerze lokalnym przy użyciu programu PowerShell lub wiersza polecenia platformy Azure](/azure/app-service/web-sites-enable-diagnostic-log#download).</span><span class="sxs-lookup"><span data-stu-id="9b672-174">The logs can be [streamed directly to your local machine with PowerShell or Azure CLI](/azure/app-service/web-sites-enable-diagnostic-log#download).</span></span> <span data-ttu-id="9b672-175">Dzienniki można też [wyświetlane w usłudze Application Insights](/azure/app-service/web-sites-enable-diagnostic-log#how-to-view-logs-in-application-insights).</span><span class="sxs-lookup"><span data-stu-id="9b672-175">Logs can also be [viewed in Application Insights](/azure/app-service/web-sites-enable-diagnostic-log#how-to-view-logs-in-application-insights).</span></span>

## <a name="log-streaming"></a><span data-ttu-id="9b672-176">Przesyłanie strumieniowe dzienników</span><span class="sxs-lookup"><span data-stu-id="9b672-176">Log streaming</span></span>

<span data-ttu-id="9b672-177">Dzienniki serwera sieci web i aplikacji, może być przesyłany strumieniowo w czasie rzeczywistym za pośrednictwem portalu.</span><span class="sxs-lookup"><span data-stu-id="9b672-177">App and web server logs can be streamed in real time through the portal.</span></span>

1. <span data-ttu-id="9b672-178">Otwórz [witryny Azure portal](https://portal.azure.com), a następnie przejdź do *mywebapp\<unique_number\>*  usługi App Service.</span><span class="sxs-lookup"><span data-stu-id="9b672-178">Open the [Azure portal](https://portal.azure.com), and navigate to the *mywebapp\<unique_number\>* App Service.</span></span>
1. <span data-ttu-id="9b672-179">W menu po lewej stronie, przewiń w dół do **monitorowanie** i wybierz pozycję **strumień dziennika**.</span><span class="sxs-lookup"><span data-stu-id="9b672-179">In the menu to the left, scroll down to the **Monitoring** section and select **Log stream**.</span></span>

    ![Zrzut ekranu przedstawiający dziennika strumienia link](./media/monitoring/log-stream.png)

<span data-ttu-id="9b672-181">Dzienniki można też [strumieniowo przy użyciu wiersza polecenia platformy Azure lub programu Azure PowerShell](/azure/app-service/web-sites-enable-diagnostic-log#streamlogs), w tym w usłudze Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="9b672-181">Logs can also be [streamed via Azure CLI or Azure PowerShell](/azure/app-service/web-sites-enable-diagnostic-log#streamlogs), including through the Cloud Shell.</span></span>

## <a name="alerts"></a><span data-ttu-id="9b672-182">Alerty</span><span class="sxs-lookup"><span data-stu-id="9b672-182">Alerts</span></span>

<span data-ttu-id="9b672-183">Usługa Azure Monitor udostępnia również [alerty w czasie rzeczywistym](/azure/monitoring-and-diagnostics/insights-alerts-portal) na podstawie metryk, zdarzenia administracyjne i innych kryteriów.</span><span class="sxs-lookup"><span data-stu-id="9b672-183">Azure Monitor also provides [real time alerts](/azure/monitoring-and-diagnostics/insights-alerts-portal) based on metrics, administrative events, and other criteria.</span></span>

> <span data-ttu-id="9b672-184">*Uwaga: Obecnie alertów dotyczących metryk aplikacji sieci web jest dostępna tylko w usłudze alerty (klasyczne).*</span><span class="sxs-lookup"><span data-stu-id="9b672-184">*Note: Currently alerting on web app metrics is only available in the Alerts (classic) service.*</span></span>

<span data-ttu-id="9b672-185">[Alerty (klasyczne) usługa](/azure/monitoring-and-diagnostics/monitor-quick-resource-metric-alert-portal) znajduje się w usłudze Azure Monitor lub w obszarze **monitorowanie** sekcji ustawień usługi App Service.</span><span class="sxs-lookup"><span data-stu-id="9b672-185">The [Alerts (classic) service](/azure/monitoring-and-diagnostics/monitor-quick-resource-metric-alert-portal) can be found in Azure Monitor or under the **Monitoring** section of the App Service settings.</span></span>

![Alerty (klasyczne) link](./media/monitoring/alerts.png)

## <a name="live-debugging"></a><span data-ttu-id="9b672-187">Debugowanie na żywo</span><span class="sxs-lookup"><span data-stu-id="9b672-187">Live debugging</span></span>

<span data-ttu-id="9b672-188">Usługa Azure App Service może być [zdalnie debugować za pomocą programu Visual Studio](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio#remotedebug) po dzienniki nie zawierają wystarczającej ilości informacji.</span><span class="sxs-lookup"><span data-stu-id="9b672-188">Azure App Service can be [debugged remotely with Visual Studio](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio#remotedebug) when logs don't provide enough information.</span></span> <span data-ttu-id="9b672-189">Zdalne debugowanie wymaga jednak wykonania aplikacji można skompilować przy użyciu symboli debugowania.</span><span class="sxs-lookup"><span data-stu-id="9b672-189">However, remote debugging requires the app to be compiled with debug symbols.</span></span> <span data-ttu-id="9b672-190">Debugowanie nie powinny odbywać się w środowisku produkcyjnym, z wyjątkiem w ostateczności.</span><span class="sxs-lookup"><span data-stu-id="9b672-190">Debugging shouldn't be done in production, except as a last resort.</span></span>

## <a name="conclusion"></a><span data-ttu-id="9b672-191">Wniosek</span><span class="sxs-lookup"><span data-stu-id="9b672-191">Conclusion</span></span>

<span data-ttu-id="9b672-192">W tej sekcji należy wykonać następujące zadania:</span><span class="sxs-lookup"><span data-stu-id="9b672-192">In this section, you completed the following tasks:</span></span>

* <span data-ttu-id="9b672-193">Znajdź podstawowe monitorowanie i rozwiązywanie problemów z danych w witrynie Azure portal</span><span class="sxs-lookup"><span data-stu-id="9b672-193">Find basic monitoring and troubleshooting data in the Azure portal</span></span>
* <span data-ttu-id="9b672-194">Dowiedz się, jak usługa Azure Monitor udostępnia lepiej poznać metryki dla wszystkich usług platformy Azure</span><span class="sxs-lookup"><span data-stu-id="9b672-194">Learn how Azure Monitor provides a deeper look at metrics across all Azure services</span></span>
* <span data-ttu-id="9b672-195">Łączenie aplikacji sieci web za pomocą usługi Application Insights dla profilowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="9b672-195">Connect the web app with Application Insights for app profiling</span></span>
* <span data-ttu-id="9b672-196">Włącz rejestrowanie i Dowiedz się, skąd pobrać dzienniki</span><span class="sxs-lookup"><span data-stu-id="9b672-196">Turn on logging and learn where to download logs</span></span>
* <span data-ttu-id="9b672-197">Stream dzienników w czasie rzeczywistym</span><span class="sxs-lookup"><span data-stu-id="9b672-197">Stream logs in real time</span></span>
* <span data-ttu-id="9b672-198">Dowiedz się, gdzie można skonfigurować alerty</span><span class="sxs-lookup"><span data-stu-id="9b672-198">Learn where to set up alerts</span></span>
* <span data-ttu-id="9b672-199">Informacje o zdalnym debugowaniu aplikacji sieci web usługi Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="9b672-199">Learn about remote debugging Azure App Service web apps.</span></span>

## <a name="additional-reading"></a><span data-ttu-id="9b672-200">Materiały uzupełniające</span><span class="sxs-lookup"><span data-stu-id="9b672-200">Additional reading</span></span>

* <xref:host-and-deploy/azure-apps/troubleshoot>
* <xref:host-and-deploy/azure-iis-errors-reference>
* [<span data-ttu-id="9b672-201">Monitorowanie wydajności aplikacji sieci web platformy Azure z usługą Application Insights</span><span class="sxs-lookup"><span data-stu-id="9b672-201">Monitor Azure web app performance with Application Insights</span></span>](/azure/application-insights/app-insights-azure-web-apps)
* [<span data-ttu-id="9b672-202">Włączanie rejestrowania diagnostycznego dla aplikacji sieci web w usłudze Azure App Service</span><span class="sxs-lookup"><span data-stu-id="9b672-202">Enable diagnostics logging for web apps in Azure App Service</span></span>](/azure/app-service/web-sites-enable-diagnostic-log)
* [<span data-ttu-id="9b672-203">Rozwiązywanie problemów z aplikacją sieci web w usłudze Azure App Service przy użyciu programu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9b672-203">Troubleshoot a web app in Azure App Service using Visual Studio</span></span>](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio)
* [<span data-ttu-id="9b672-204">Tworzenie klasycznego alertów dotyczących metryk w usłudze Azure Monitor dla usług platformy Azure — witryna Azure portal</span><span class="sxs-lookup"><span data-stu-id="9b672-204">Create classic metric alerts in Azure Monitor for Azure services - Azure portal</span></span>](/azure/monitoring-and-diagnostics/insights-alerts-portal)
