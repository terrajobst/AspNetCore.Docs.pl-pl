---
title: Rozwiązywanie problemów z platformą ASP.NET Core w usługach IIS
author: guardrex
description: Dowiedz się, jak diagnozować problemy z wdrożeniami usług Internet Information Services (IIS) w aplikacji platformy ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/12/2019
uid: host-and-deploy/iis/troubleshoot
ms.openlocfilehash: e4c93459f2030c7c0a55ea90e0cc8c8d30b76c51
ms.sourcegitcommit: a04eb20e81243930ec829a9db5dd5de49f669450
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/03/2019
ms.locfileid: "66470461"
---
# <a name="troubleshoot-aspnet-core-on-iis"></a><span data-ttu-id="93ce2-103">Rozwiązywanie problemów z platformą ASP.NET Core w usługach IIS</span><span class="sxs-lookup"><span data-stu-id="93ce2-103">Troubleshoot ASP.NET Core on IIS</span></span>

<span data-ttu-id="93ce2-104">Przez [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="93ce2-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="93ce2-105">Ten artykuł zawiera instrukcje na temat platformy ASP.NET Core zdiagnozować problem uruchamiania aplikacji w przypadku hostowania za pomocą [Internet Information Services (IIS)](/iis).</span><span class="sxs-lookup"><span data-stu-id="93ce2-105">This article provides instructions on how to diagnose an ASP.NET Core app startup issue when hosting with [Internet Information Services (IIS)](/iis).</span></span> <span data-ttu-id="93ce2-106">Informacje przedstawione w tym artykule mają zastosowanie do hostowania w usługach IIS w systemie Windows Server i Windows Desktop.</span><span class="sxs-lookup"><span data-stu-id="93ce2-106">The information in this article applies to hosting in IIS on Windows Server and Windows Desktop.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="93ce2-107">W programie Visual Studio ma domyślnie wartość projektu ASP.NET Core [usług IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hostingu podczas debugowania.</span><span class="sxs-lookup"><span data-stu-id="93ce2-107">In Visual Studio, an ASP.NET Core project defaults to [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hosting during debugging.</span></span> <span data-ttu-id="93ce2-108">A *502.5 - niepowodzenia procesu* lub *500.30 - Start błąd* występuje, gdy debugowanie lokalne może być troubleshooted przy użyciu porady w tym temacie.</span><span class="sxs-lookup"><span data-stu-id="93ce2-108">A *502.5 - Process Failure* or a *500.30 - Start Failure* that occurs when debugging locally can be troubleshooted using the advice in this topic.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="93ce2-109">W programie Visual Studio ma domyślnie wartość projektu ASP.NET Core [usług IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hostingu podczas debugowania.</span><span class="sxs-lookup"><span data-stu-id="93ce2-109">In Visual Studio, an ASP.NET Core project defaults to [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hosting during debugging.</span></span> <span data-ttu-id="93ce2-110">A *502.5 niepowodzenia procesu* występuje, gdy debugowanie lokalne może być troubleshooted przy użyciu porady w tym temacie.</span><span class="sxs-lookup"><span data-stu-id="93ce2-110">A *502.5 Process Failure* that occurs when debugging locally can be troubleshooted using the advice in this topic.</span></span>

::: moniker-end

<span data-ttu-id="93ce2-111">Dodatkowe tematy dotyczące rozwiązywania problemów:</span><span class="sxs-lookup"><span data-stu-id="93ce2-111">Additional troubleshooting topics:</span></span>

<span data-ttu-id="93ce2-112"><xref:host-and-deploy/azure-apps/troubleshoot> Mimo że usługa App Service używa [modułu ASP.NET Core](xref:host-and-deploy/aspnet-core-module) i usługi IIS do hostowania aplikacji, zobacz temat dedykowanych instrukcje specyficzne dla usługi App Service.</span><span class="sxs-lookup"><span data-stu-id="93ce2-112"><xref:host-and-deploy/azure-apps/troubleshoot> Although App Service uses the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) and IIS to host apps, see the dedicated topic for instructions specific to App Service.</span></span>

<span data-ttu-id="93ce2-113"><xref:fundamentals/error-handling> Dowiedz się, jak do obsługi błędów w aplikacji platformy ASP.NET Core podczas programowania w systemie lokalnym.</span><span class="sxs-lookup"><span data-stu-id="93ce2-113"><xref:fundamentals/error-handling> Discover how to handle errors in ASP.NET Core apps during development on a local system.</span></span>

<span data-ttu-id="93ce2-114">[Naucz się debugować przy użyciu programu Visual Studio](/visualstudio/debugger/getting-started-with-the-debugger) w tym temacie przedstawiono funkcje debugera programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="93ce2-114">[Learn to debug using Visual Studio](/visualstudio/debugger/getting-started-with-the-debugger) This topic introduces the features of the Visual Studio debugger.</span></span>

<span data-ttu-id="93ce2-115">[Debugowanie za pomocą programu Visual Studio Code](https://code.visualstudio.com/docs/editor/debugging) więcej informacji na temat debugowania wbudowaną w programie Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="93ce2-115">[Debugging with Visual Studio Code](https://code.visualstudio.com/docs/editor/debugging) Learn about the debugging support built into Visual Studio Code.</span></span>

## <a name="app-startup-errors"></a><span data-ttu-id="93ce2-116">Błędy uruchamiania aplikacji</span><span class="sxs-lookup"><span data-stu-id="93ce2-116">App startup errors</span></span>

### <a name="5025-process-failure"></a><span data-ttu-id="93ce2-117">502.5 niepowodzenie procesu</span><span class="sxs-lookup"><span data-stu-id="93ce2-117">502.5 Process Failure</span></span>

<span data-ttu-id="93ce2-118">Proces roboczy kończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="93ce2-118">The worker process fails.</span></span> <span data-ttu-id="93ce2-119">Nie zaczyna się aplikacja.</span><span class="sxs-lookup"><span data-stu-id="93ce2-119">The app doesn't start.</span></span>

<span data-ttu-id="93ce2-120">Modułu ASP.NET Core próbuje uruchomić proces dotnet wewnętrznej bazy danych, ale nie została uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="93ce2-120">The ASP.NET Core Module attempts to start the backend dotnet process but it fails to start.</span></span> <span data-ttu-id="93ce2-121">Zazwyczaj można ustalić przyczyny niepowodzenia uruchamiania procesu na podstawie wpisów w [dziennik zdarzeń aplikacji](#application-event-log) i [dziennika stdout modułu ASP.NET Core](#aspnet-core-module-stdout-log).</span><span class="sxs-lookup"><span data-stu-id="93ce2-121">The cause of a process startup failure can usually be determined from entries in the [Application Event Log](#application-event-log) and the [ASP.NET Core Module stdout log](#aspnet-core-module-stdout-log).</span></span>

<span data-ttu-id="93ce2-122">Aplikacja jest błędnie skonfigurowane z powodu przeznaczony dla wersji udostępnionej platformy ASP.NET Core, która nie jest obecny jest jakiś wspólny warunek błędu.</span><span class="sxs-lookup"><span data-stu-id="93ce2-122">A common failure condition is the app is misconfigured due to targeting a version of the ASP.NET Core shared framework that isn't present.</span></span> <span data-ttu-id="93ce2-123">Sprawdź, które wersje udostępnionej platformy ASP.NET Core są zainstalowane na komputerze docelowym.</span><span class="sxs-lookup"><span data-stu-id="93ce2-123">Check which versions of the ASP.NET Core shared framework are installed on the target machine.</span></span> <span data-ttu-id="93ce2-124">*Udostępnionej platformy* to zbiór zestawów ( *.dll* plików), są zainstalowane na komputerze i odwołuje się meta Microsoft.aspnetcore.all, takich jak `Microsoft.AspNetCore.App`.</span><span class="sxs-lookup"><span data-stu-id="93ce2-124">The *shared framework* is the set of assemblies (*.dll* files) that are installed on the machine and referenced by a metapackage such as `Microsoft.AspNetCore.App`.</span></span> <span data-ttu-id="93ce2-125">Dokumentacja meta Microsoft.aspnetcore.all można określić minimalnej wymaganej wersji.</span><span class="sxs-lookup"><span data-stu-id="93ce2-125">The metapackage reference can specify a minimum required version.</span></span> <span data-ttu-id="93ce2-126">Aby uzyskać więcej informacji, zobacz [udostępnionej platformy](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/).</span><span class="sxs-lookup"><span data-stu-id="93ce2-126">For more information, see [The shared framework](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/).</span></span>

<span data-ttu-id="93ce2-127">*502.5 niepowodzenia procesu* strony błędu jest zwracany, jeśli aplikacji lub obsługującego błędnej konfiguracji powoduje niepowodzenie procesu roboczego:</span><span class="sxs-lookup"><span data-stu-id="93ce2-127">The *502.5 Process Failure* error page is returned when a hosting or app misconfiguration causes the worker process to fail:</span></span>

![Okno przeglądarki, przedstawiający 502.5 stronę niepowodzenia procesu](troubleshoot/_static/process-failure-page.png)

::: moniker range="= aspnetcore-2.2"

### <a name="50030-in-process-startup-failure"></a><span data-ttu-id="93ce2-129">500.30 w procesie Niepowodzenie uruchamiania</span><span class="sxs-lookup"><span data-stu-id="93ce2-129">500.30 In-Process Startup Failure</span></span>

<span data-ttu-id="93ce2-130">Proces roboczy kończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="93ce2-130">The worker process fails.</span></span> <span data-ttu-id="93ce2-131">Nie zaczyna się aplikacja.</span><span class="sxs-lookup"><span data-stu-id="93ce2-131">The app doesn't start.</span></span>

<span data-ttu-id="93ce2-132">Próbuje uruchomić program .NET Core CLR w procesie modułu ASP.NET Core, ale nie została uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="93ce2-132">The ASP.NET Core Module attempts to start the .NET Core CLR in-process, but it fails to start.</span></span> <span data-ttu-id="93ce2-133">Zazwyczaj można ustalić przyczyny niepowodzenia uruchamiania procesu na podstawie wpisów w [dziennik zdarzeń aplikacji](#application-event-log) i [dziennika stdout modułu ASP.NET Core](#aspnet-core-module-stdout-log).</span><span class="sxs-lookup"><span data-stu-id="93ce2-133">The cause of a process startup failure can usually be determined from entries in the [Application Event Log](#application-event-log) and the [ASP.NET Core Module stdout log](#aspnet-core-module-stdout-log).</span></span>

<span data-ttu-id="93ce2-134">Aplikacja jest błędnie skonfigurowane z powodu przeznaczony dla wersji udostępnionej platformy ASP.NET Core, która nie jest obecny jest jakiś wspólny warunek błędu.</span><span class="sxs-lookup"><span data-stu-id="93ce2-134">A common failure condition is the app is misconfigured due to targeting a version of the ASP.NET Core shared framework that isn't present.</span></span> <span data-ttu-id="93ce2-135">Sprawdź, które wersje udostępnionej platformy ASP.NET Core są zainstalowane na komputerze docelowym.</span><span class="sxs-lookup"><span data-stu-id="93ce2-135">Check which versions of the ASP.NET Core shared framework are installed on the target machine.</span></span>

### <a name="5000-in-process-handler-load-failure"></a><span data-ttu-id="93ce2-136">500.0 w procesie programu obsługi błędu ładowania</span><span class="sxs-lookup"><span data-stu-id="93ce2-136">500.0 In-Process Handler Load Failure</span></span>

<span data-ttu-id="93ce2-137">Proces roboczy kończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="93ce2-137">The worker process fails.</span></span> <span data-ttu-id="93ce2-138">Nie zaczyna się aplikacja.</span><span class="sxs-lookup"><span data-stu-id="93ce2-138">The app doesn't start.</span></span>

<span data-ttu-id="93ce2-139">Modułu ASP.NET Core nie powiedzie się znaleźć programu .NET Core CLR i Znajdź program obsługi żądania w trakcie (*aspnetcorev2_inprocess.dll*).</span><span class="sxs-lookup"><span data-stu-id="93ce2-139">The ASP.NET Core Module fails to find the .NET Core CLR and find the in-process request handler (*aspnetcorev2_inprocess.dll*).</span></span> <span data-ttu-id="93ce2-140">Sprawdź, czy:</span><span class="sxs-lookup"><span data-stu-id="93ce2-140">Check that:</span></span>

* <span data-ttu-id="93ce2-141">Aplikacja jest przeznaczona na albo [Microsoft.AspNetCore.Server.IIS](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IIS) pakietu NuGet lub [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="93ce2-141">The app targets either the [Microsoft.AspNetCore.Server.IIS](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IIS) NuGet package or the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>
* <span data-ttu-id="93ce2-142">Wersja udostępnionej platformy ASP.NET Core jest zainstalowanie aplikacji jest przeznaczony dla na komputerze docelowym.</span><span class="sxs-lookup"><span data-stu-id="93ce2-142">The version of the ASP.NET Core shared framework that the app targets is installed on the target machine.</span></span>

### <a name="5000-out-of-process-handler-load-failure"></a><span data-ttu-id="93ce2-143">500.0 Błąd ładowania poza procesem programu obsługi</span><span class="sxs-lookup"><span data-stu-id="93ce2-143">500.0 Out-Of-Process Handler Load Failure</span></span>

<span data-ttu-id="93ce2-144">Proces roboczy kończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="93ce2-144">The worker process fails.</span></span> <span data-ttu-id="93ce2-145">Nie zaczyna się aplikacja.</span><span class="sxs-lookup"><span data-stu-id="93ce2-145">The app doesn't start.</span></span>

<span data-ttu-id="93ce2-146">Modułu ASP.NET Core nie powiedzie się znaleźć spoza procesu hostingu Obsługa żądania.</span><span class="sxs-lookup"><span data-stu-id="93ce2-146">The ASP.NET Core Module fails to find the out-of-process hosting request handler.</span></span> <span data-ttu-id="93ce2-147">Upewnij się, że *aspnetcorev2_outofprocess.dll* znajduje się w podfolderze obok *aspnetcorev2.dll*.</span><span class="sxs-lookup"><span data-stu-id="93ce2-147">Make sure the *aspnetcorev2_outofprocess.dll* is present in a subfolder next to *aspnetcorev2.dll*.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

### <a name="50031-ancm-failed-to-find-native-dependencies"></a><span data-ttu-id="93ce2-148">500.31 nie można odnaleźć zależności natywnych ANCM</span><span class="sxs-lookup"><span data-stu-id="93ce2-148">500.31 ANCM Failed to Find Native Dependencies</span></span>

<span data-ttu-id="93ce2-149">Proces roboczy kończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="93ce2-149">The worker process fails.</span></span> <span data-ttu-id="93ce2-150">Nie zaczyna się aplikacja.</span><span class="sxs-lookup"><span data-stu-id="93ce2-150">The app doesn't start.</span></span>

<span data-ttu-id="93ce2-151">Próbuje uruchomić platformy .NET Core środowiska uruchomieniowego w procesie modułu ASP.NET Core, ale nie została uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="93ce2-151">The ASP.NET Core Module attempts to start the .NET Core runtime in-process, but it fails to start.</span></span> <span data-ttu-id="93ce2-152">Najczęstszą przyczyną tego błędu uruchamiania jest, gdy `Microsoft.NETCore.App` lub `Microsoft.AspNetCore.App` środowisko uruchomieniowe nie jest zainstalowany.</span><span class="sxs-lookup"><span data-stu-id="93ce2-152">The most common cause of this startup failure is when the `Microsoft.NETCore.App` or `Microsoft.AspNetCore.App` runtime isn't installed.</span></span> <span data-ttu-id="93ce2-153">Jeśli aplikacja jest wdrażana na docelowej platformy ASP.NET Core 3.0, a ta wersja nie istnieje na maszynie, ten błąd występuje.</span><span class="sxs-lookup"><span data-stu-id="93ce2-153">If the app is deployed to target ASP.NET Core 3.0 and that version doesn't exist on the machine, this error occurs.</span></span> <span data-ttu-id="93ce2-154">Przykładowy komunikat o błędzie:</span><span class="sxs-lookup"><span data-stu-id="93ce2-154">An example error message follows:</span></span>

```
The specified framework 'Microsoft.NETCore.App', version '3.0.0' was not found.
  - The following frameworks were found:
      2.2.1 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview5-27626-15 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview6-27713-13 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview6-27714-15 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview6-27723-08 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
```

<span data-ttu-id="93ce2-155">Komunikat o błędzie zawiera listę wszystkich zainstalowanych wersji platformy .NET Core i wersji, żądane przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="93ce2-155">The error message lists all the installed .NET Core versions and the version requested by the app.</span></span> <span data-ttu-id="93ce2-156">Aby naprawić ten błąd, to:</span><span class="sxs-lookup"><span data-stu-id="93ce2-156">To fix this error, either:</span></span>

* <span data-ttu-id="93ce2-157">Na komputerze, należy zainstalować odpowiednią wersję programu .NET Core.</span><span class="sxs-lookup"><span data-stu-id="93ce2-157">Install the appropriate version of .NET Core on the machine.</span></span>
* <span data-ttu-id="93ce2-158">Zmień aplikacji pod kątem określonej wersji programu .NET Core, który znajduje się na komputerze.</span><span class="sxs-lookup"><span data-stu-id="93ce2-158">Change the app to target a version of .NET Core that's present on the machine.</span></span>
* <span data-ttu-id="93ce2-159">Opublikuj aplikację jako [niezależna wdrożenia](/dotnet/core/deploying/#self-contained-deployments-scd).</span><span class="sxs-lookup"><span data-stu-id="93ce2-159">Publish the app as a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd).</span></span>

<span data-ttu-id="93ce2-160">Podczas uruchamiania w trakcie opracowywania ( `ASPNETCORE_ENVIRONMENT` ustawiono zmienną środowiskową `Development`), ten błąd jest zapisywany do odpowiedzi HTTP.</span><span class="sxs-lookup"><span data-stu-id="93ce2-160">When running in development (the `ASPNETCORE_ENVIRONMENT` environment variable is set to `Development`), the specific error is written to the HTTP response.</span></span> <span data-ttu-id="93ce2-161">Przyczyny niepowodzenia uruchamiania procesu znajduje się również w [dziennik zdarzeń aplikacji](#application-event-log).</span><span class="sxs-lookup"><span data-stu-id="93ce2-161">The cause of a process startup failure is also found in the [Application Event Log](#application-event-log).</span></span>

### <a name="50032-ancm-failed-to-load-dll"></a><span data-ttu-id="93ce2-162">500.32 ANCM nie udało się ładowanie biblioteki dll</span><span class="sxs-lookup"><span data-stu-id="93ce2-162">500.32 ANCM Failed to Load dll</span></span>

<span data-ttu-id="93ce2-163">Proces roboczy kończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="93ce2-163">The worker process fails.</span></span> <span data-ttu-id="93ce2-164">Nie zaczyna się aplikacja.</span><span class="sxs-lookup"><span data-stu-id="93ce2-164">The app doesn't start.</span></span>

<span data-ttu-id="93ce2-165">Najczęstszą przyczyną tego błędu jest to, że aplikacja została opublikowana na potrzeby architektury procesora niezgodne.</span><span class="sxs-lookup"><span data-stu-id="93ce2-165">The most common cause for this error is that the app is published for an incompatible processor architecture.</span></span> <span data-ttu-id="93ce2-166">Jeśli aplikacja została opublikowana do obiektu docelowego w 64-bitowy proces roboczy działa jako aplikacja 32-bitowych, ten błąd występuje.</span><span class="sxs-lookup"><span data-stu-id="93ce2-166">If the worker process is running as a 32-bit app and the app was published to target 64-bit, this error occurs.</span></span>

<span data-ttu-id="93ce2-167">Aby naprawić ten błąd, to:</span><span class="sxs-lookup"><span data-stu-id="93ce2-167">To fix this error, either:</span></span>

* <span data-ttu-id="93ce2-168">Ponownie opublikować aplikację dla architektury procesorów jako procesu roboczego.</span><span class="sxs-lookup"><span data-stu-id="93ce2-168">Republish the app for the same processor architecture as the worker process.</span></span>
* <span data-ttu-id="93ce2-169">Opublikuj aplikację jako [wdrożenia zależny od struktury](/dotnet/core/deploying/#framework-dependent-executables-fde).</span><span class="sxs-lookup"><span data-stu-id="93ce2-169">Publish the app as a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-executables-fde).</span></span>

### <a name="50033-ancm-request-handler-load-failure"></a><span data-ttu-id="93ce2-170">500.33 błąd ładowania program obsługi żądania ANCM</span><span class="sxs-lookup"><span data-stu-id="93ce2-170">500.33 ANCM Request Handler Load Failure</span></span>

<span data-ttu-id="93ce2-171">Proces roboczy kończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="93ce2-171">The worker process fails.</span></span> <span data-ttu-id="93ce2-172">Nie zaczyna się aplikacja.</span><span class="sxs-lookup"><span data-stu-id="93ce2-172">The app doesn't start.</span></span>

<span data-ttu-id="93ce2-173">Nie odwoływać się do aplikacji `Microsoft.AspNetCore.App` framework.</span><span class="sxs-lookup"><span data-stu-id="93ce2-173">The app didn't reference the `Microsoft.AspNetCore.App` framework.</span></span> <span data-ttu-id="93ce2-174">Tylko aplikacje przeznaczone dla `Microsoft.AspNetCore.App` framework może być obsługiwany przez modułu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="93ce2-174">Only apps targeting the `Microsoft.AspNetCore.App` framework can be hosted by the ASP.NET Core Module.</span></span>

<span data-ttu-id="93ce2-175">Aby naprawić ten błąd, upewnij się, że aplikacja jest przeznaczony dla `Microsoft.AspNetCore.App` framework.</span><span class="sxs-lookup"><span data-stu-id="93ce2-175">To fix this error, confirm that the app is targeting the `Microsoft.AspNetCore.App` framework.</span></span> <span data-ttu-id="93ce2-176">Sprawdź `.runtimeconfig.json` Aby zweryfikować framework docelowe przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="93ce2-176">Check the `.runtimeconfig.json` to verify the framework targeted by the app.</span></span>

### <a name="50034-ancm-mixed-hosting-models-not-supported"></a><span data-ttu-id="93ce2-177">500.34 ANCM mieszane modelach hostingu nie jest obsługiwane</span><span class="sxs-lookup"><span data-stu-id="93ce2-177">500.34 ANCM Mixed Hosting Models Not Supported</span></span>

<span data-ttu-id="93ce2-178">Proces roboczy nie może uruchomić aplikację spoza procesu i aplikacji w trakcie tego samego procesu.</span><span class="sxs-lookup"><span data-stu-id="93ce2-178">The worker process can't run both an in-process app and an out-of-process app in the same process.</span></span>

<span data-ttu-id="93ce2-179">Aby naprawić ten błąd, należy uruchamiać aplikacje w oddzielnych pul aplikacji usług IIS.</span><span class="sxs-lookup"><span data-stu-id="93ce2-179">To fix this error, run apps in separate IIS application pools.</span></span>

### <a name="50035-ancm-multiple-in-process-applications-in-same-process"></a><span data-ttu-id="93ce2-180">500.35 wiele aplikacji w trakcie ANCM w tym samym procesie</span><span class="sxs-lookup"><span data-stu-id="93ce2-180">500.35 ANCM Multiple In-Process Applications in same Process</span></span>

<span data-ttu-id="93ce2-181">Proces roboczy nie może uruchomić aplikację spoza procesu i aplikacji w trakcie tego samego procesu.</span><span class="sxs-lookup"><span data-stu-id="93ce2-181">The worker process can't run both an in-process app and an out-of-process app in the same process.</span></span>

<span data-ttu-id="93ce2-182">Aby naprawić ten błąd, należy uruchamiać aplikacje w oddzielnych pul aplikacji usług IIS.</span><span class="sxs-lookup"><span data-stu-id="93ce2-182">To fix this error, run apps in separate IIS application pools.</span></span>

### <a name="50036-ancm-out-of-process-handler-load-failure"></a><span data-ttu-id="93ce2-183">500.36 błąd ładowania spoza procesu obsługi ANCM</span><span class="sxs-lookup"><span data-stu-id="93ce2-183">500.36 ANCM Out-Of-Process Handler Load Failure</span></span>

<span data-ttu-id="93ce2-184">Obsługa żądań poza procesem, *aspnetcorev2_outofprocess.dll*, obok pozycji nie jest *aspnetcorev2.dll* pliku.</span><span class="sxs-lookup"><span data-stu-id="93ce2-184">The out-of-process request handler, *aspnetcorev2_outofprocess.dll*, isn't next to the *aspnetcorev2.dll* file.</span></span> <span data-ttu-id="93ce2-185">Oznacza to uszkodzenie instalacji modułu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="93ce2-185">This indicates a corrupted installation of the ASP.NET Core Module.</span></span>

<span data-ttu-id="93ce2-186">Aby naprawić ten błąd, napraw instalację [hostingu pakietu programu .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle) (dla usług IIS) lub Visual Studio (dla usług IIS Express).</span><span class="sxs-lookup"><span data-stu-id="93ce2-186">To fix this error, repair the installation of the [.NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle) (for IIS) or Visual Studio (for IIS Express).</span></span>

### <a name="50037-ancm-failed-to-start-within-startup-time-limit"></a><span data-ttu-id="93ce2-187">500.37 ANCM uruchomienie nie powiodło się przed upływem limitu czasu uruchamiania</span><span class="sxs-lookup"><span data-stu-id="93ce2-187">500.37 ANCM Failed to Start Within Startup Time Limit</span></span>

<span data-ttu-id="93ce2-188">ANCM nie można uruchomić w ramach limitu czasu uruchamiania dostarczają.</span><span class="sxs-lookup"><span data-stu-id="93ce2-188">ANCM failed to start within the provied startup time limit.</span></span> <span data-ttu-id="93ce2-189">Domyślnie limit czasu wynosi 120 sekund.</span><span class="sxs-lookup"><span data-stu-id="93ce2-189">By default, the timeout is 120 seconds.</span></span>

<span data-ttu-id="93ce2-190">Ten błąd może wystąpić podczas uruchamiania dużej liczby aplikacji na tym samym komputerze.</span><span class="sxs-lookup"><span data-stu-id="93ce2-190">This error can occur when starting a large number of apps on the same machine.</span></span> <span data-ttu-id="93ce2-191">Sprawdź, czy wzrostów użycia Procesora/pamięci na serwerze podczas uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="93ce2-191">Check for CPU/Memory usage spikes on the server during startup.</span></span> <span data-ttu-id="93ce2-192">Może być konieczne przesunąć procesu uruchamiania wielu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="93ce2-192">You may need to stagger the startup process of multiple apps.</span></span>

### <a name="50030-in-process-startup-failure"></a><span data-ttu-id="93ce2-193">500.30 w procesie Niepowodzenie uruchamiania</span><span class="sxs-lookup"><span data-stu-id="93ce2-193">500.30 In-Process Startup Failure</span></span>

<span data-ttu-id="93ce2-194">Proces roboczy kończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="93ce2-194">The worker process fails.</span></span> <span data-ttu-id="93ce2-195">Nie zaczyna się aplikacja.</span><span class="sxs-lookup"><span data-stu-id="93ce2-195">The app doesn't start.</span></span>

<span data-ttu-id="93ce2-196">Próbuje uruchomić platformy .NET Core środowiska uruchomieniowego w procesie modułu ASP.NET Core, ale nie została uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="93ce2-196">The ASP.NET Core Module attempts to start the .NET Core runtime in-process, but it fails to start.</span></span> <span data-ttu-id="93ce2-197">Przyczyny niepowodzenia uruchamiania procesu jest zazwyczaj określana na podstawie wpisów w [dziennik zdarzeń aplikacji](#application-event-log) i [dziennika stdout modułu ASP.NET Core](#aspnet-core-module-stdout-log).</span><span class="sxs-lookup"><span data-stu-id="93ce2-197">The cause of a process startup failure is usually determined from entries in the [Application Event Log](#application-event-log) and the [ASP.NET Core Module stdout log](#aspnet-core-module-stdout-log).</span></span>

### <a name="5000-in-process-handler-load-failure"></a><span data-ttu-id="93ce2-198">500.0 w procesie programu obsługi błędu ładowania</span><span class="sxs-lookup"><span data-stu-id="93ce2-198">500.0 In-Process Handler Load Failure</span></span>

<span data-ttu-id="93ce2-199">Proces roboczy kończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="93ce2-199">The worker process fails.</span></span> <span data-ttu-id="93ce2-200">Nie zaczyna się aplikacja.</span><span class="sxs-lookup"><span data-stu-id="93ce2-200">The app doesn't start.</span></span>

<span data-ttu-id="93ce2-201">Wystąpił nieznany błąd podczas ładowania składników modułu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="93ce2-201">An unknown error occurred loading ASP.NET Core Module components.</span></span> <span data-ttu-id="93ce2-202">Wykonaj jedną z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="93ce2-202">Take one of the following actions:</span></span>

* <span data-ttu-id="93ce2-203">Skontaktuj się z pomocą [Microsoft Support](https://support.microsoft.com/oas/default.aspx?prid=15832) (wybierz **narzędzi deweloperskich** następnie **platformy ASP.NET Core**).</span><span class="sxs-lookup"><span data-stu-id="93ce2-203">Contact [Microsoft Support](https://support.microsoft.com/oas/default.aspx?prid=15832) (select **Developer Tools** then **ASP.NET Core**).</span></span>
* <span data-ttu-id="93ce2-204">Zadaj pytanie w witrynie Stack Overflow.</span><span class="sxs-lookup"><span data-stu-id="93ce2-204">Ask a question on Stack Overflow.</span></span>
* <span data-ttu-id="93ce2-205">Prześlij zgłoszenie na naszych [repozytorium GitHub](https://github.com/aspnet/AspNetCore).</span><span class="sxs-lookup"><span data-stu-id="93ce2-205">File an issue on our [GitHub repository](https://github.com/aspnet/AspNetCore).</span></span>

::: moniker-end

### <a name="500-internal-server-error"></a><span data-ttu-id="93ce2-206">500 Wewnętrzny błąd serwera</span><span class="sxs-lookup"><span data-stu-id="93ce2-206">500 Internal Server Error</span></span>

<span data-ttu-id="93ce2-207">Uruchamia aplikację, ale błąd uniemożliwia spełnienie żądania przez serwer.</span><span class="sxs-lookup"><span data-stu-id="93ce2-207">The app starts, but an error prevents the server from fulfilling the request.</span></span>

<span data-ttu-id="93ce2-208">Ten błąd występuje w kodzie aplikacji, podczas uruchamiania lub podczas tworzenia odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="93ce2-208">This error occurs within the app's code during startup or while creating a response.</span></span> <span data-ttu-id="93ce2-209">Odpowiedź może zawierać żadnej zawartości lub odpowiedzi może być wyświetlana jako *500 Wewnętrzny błąd serwera* w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="93ce2-209">The response may contain no content, or the response may appear as a *500 Internal Server Error* in the browser.</span></span> <span data-ttu-id="93ce2-210">W dzienniku zdarzeń aplikacji stwierdza, zwykle uruchomiona aplikacja.</span><span class="sxs-lookup"><span data-stu-id="93ce2-210">The Application Event Log usually states that the app started normally.</span></span> <span data-ttu-id="93ce2-211">Z perspektywy serwera, który jest poprawna.</span><span class="sxs-lookup"><span data-stu-id="93ce2-211">From the server's perspective, that's correct.</span></span> <span data-ttu-id="93ce2-212">Aplikacja została uruchomiona, ale nie może wygenerować prawidłowej odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="93ce2-212">The app did start, but it can't generate a valid response.</span></span> <span data-ttu-id="93ce2-213">[Uruchamianie aplikacji w wierszu polecenia](#run-the-app-at-a-command-prompt) na serwerze lub [Włącz dziennik stdout modułu ASP.NET Core](#aspnet-core-module-stdout-log) do rozwiązania problemu.</span><span class="sxs-lookup"><span data-stu-id="93ce2-213">[Run the app at a command prompt](#run-the-app-at-a-command-prompt) on the server or [enable the ASP.NET Core Module stdout log](#aspnet-core-module-stdout-log) to troubleshoot the problem.</span></span>

### <a name="failed-to-start-application-errorcode-0x800700c1"></a><span data-ttu-id="93ce2-214">Nie można uruchomić aplikację (kod błędu "0x800700c1")</span><span class="sxs-lookup"><span data-stu-id="93ce2-214">Failed to start application (ErrorCode '0x800700c1')</span></span>

```
EventID: 1010
Source: IIS AspNetCore Module V2
Failed to start application '/LM/W3SVC/6/ROOT/', ErrorCode '0x800700c1'.
```

<span data-ttu-id="93ce2-215">Aplikacji nie powiodło się, ponieważ zestaw aplikacji ( *.dll*) nie można go załadować.</span><span class="sxs-lookup"><span data-stu-id="93ce2-215">The app failed to start because the app's assembly (*.dll*) couldn't be loaded.</span></span>

<span data-ttu-id="93ce2-216">Ten błąd występuje, gdy występuje niezgodność liczby bitów opublikowanej aplikacji i procesu w3wp/programu iisexpress.</span><span class="sxs-lookup"><span data-stu-id="93ce2-216">This error occurs when there's a bitness mismatch between the published app and the w3wp/iisexpress process.</span></span>

<span data-ttu-id="93ce2-217">Upewnij się, że ustawienie 32-bitowych puli aplikacji jest prawidłowy:</span><span class="sxs-lookup"><span data-stu-id="93ce2-217">Confirm that the app pool's 32-bit setting is correct:</span></span>

1. <span data-ttu-id="93ce2-218">Wybierz pulę aplikacji w Menedżerze usług IIS w **pul aplikacji**.</span><span class="sxs-lookup"><span data-stu-id="93ce2-218">Select the app pool in IIS Manager's **Application Pools**.</span></span>
1. <span data-ttu-id="93ce2-219">Wybierz **Zaawansowane ustawienia** w obszarze **edytowanie puli aplikacji** w **akcje** panelu.</span><span class="sxs-lookup"><span data-stu-id="93ce2-219">Select **Advanced Settings** under **Edit Application Pool** in the **Actions** panel.</span></span>
1. <span data-ttu-id="93ce2-220">Ustaw **Włącz aplikacje 32-bitowe**:</span><span class="sxs-lookup"><span data-stu-id="93ce2-220">Set **Enable 32-Bit Applications**:</span></span>
   * <span data-ttu-id="93ce2-221">Jeśli wdrażanie (x86) 32-bitowych aplikacji, ustaw wartość `True`.</span><span class="sxs-lookup"><span data-stu-id="93ce2-221">If deploying a 32-bit (x86) app, set the value to `True`.</span></span>
   * <span data-ttu-id="93ce2-222">Jeśli wdrażanie (x64) 64-bitowych aplikacji, ustaw wartość `False`.</span><span class="sxs-lookup"><span data-stu-id="93ce2-222">If deploying a 64-bit (x64) app, set the value to `False`.</span></span>

### <a name="connection-reset"></a><span data-ttu-id="93ce2-223">Resetowanie połączenia</span><span class="sxs-lookup"><span data-stu-id="93ce2-223">Connection reset</span></span>

<span data-ttu-id="93ce2-224">Jeśli błąd wystąpi po nagłówki są wysyłane, jest za późno serwera wysłać **500 Wewnętrzny błąd serwera** po wystąpieniu błędu.</span><span class="sxs-lookup"><span data-stu-id="93ce2-224">If an error occurs after the headers are sent, it's too late for the server to send a **500 Internal Server Error** when an error occurs.</span></span> <span data-ttu-id="93ce2-225">Dzieje się tak często, gdy wystąpi błąd podczas serializacji obiektów złożonych na odpowiedź.</span><span class="sxs-lookup"><span data-stu-id="93ce2-225">This often happens when an error occurs during the serialization of complex objects for a response.</span></span> <span data-ttu-id="93ce2-226">Tego typu błędu jest wyświetlany jako *resetowania połączenia* błąd na komputerze klienckim.</span><span class="sxs-lookup"><span data-stu-id="93ce2-226">This type of error appears as a *connection reset* error on the client.</span></span> <span data-ttu-id="93ce2-227">[Rejestrowanie aplikacji](xref:fundamentals/logging/index) mogą pomóc rozwiązać tego rodzaju błędów.</span><span class="sxs-lookup"><span data-stu-id="93ce2-227">[Application logging](xref:fundamentals/logging/index) can help troubleshoot these types of errors.</span></span>

## <a name="default-startup-limits"></a><span data-ttu-id="93ce2-228">Domyślne limity uruchamiania</span><span class="sxs-lookup"><span data-stu-id="93ce2-228">Default startup limits</span></span>

<span data-ttu-id="93ce2-229">Modułu ASP.NET Core jest skonfigurowany z domyślną *startupTimeLimit* 120 sekund.</span><span class="sxs-lookup"><span data-stu-id="93ce2-229">The ASP.NET Core Module is configured with a default *startupTimeLimit* of 120 seconds.</span></span> <span data-ttu-id="93ce2-230">Gdy pozostawić wartość domyślną, aplikacja może potrwać do dwóch minut przed moduł dzienniki awarii procesu.</span><span class="sxs-lookup"><span data-stu-id="93ce2-230">When left at the default value, an app may take up to two minutes to start before the module logs a process failure.</span></span> <span data-ttu-id="93ce2-231">Aby uzyskać informacje na temat konfigurowania modułu, zobacz [atrybuty elementu aspNetCore](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span><span class="sxs-lookup"><span data-stu-id="93ce2-231">For information on configuring the module, see [Attributes of the aspNetCore element](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span></span>

## <a name="troubleshoot-app-startup-errors"></a><span data-ttu-id="93ce2-232">Rozwiązywanie problemów z błędami uruchamiania aplikacji</span><span class="sxs-lookup"><span data-stu-id="93ce2-232">Troubleshoot app startup errors</span></span>

### <a name="enable-the-aspnet-core-module-debug-log"></a><span data-ttu-id="93ce2-233">Włącz dziennik debugowania modułu ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="93ce2-233">Enable the ASP.NET Core Module debug log</span></span>

<span data-ttu-id="93ce2-234">Dodaj poniższe ustawienia programu obsługi w aplikacji *web.config* plik, aby włączyć dzienniki debugowania modułu ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="93ce2-234">Add the following handler settings to the app's *web.config* file to enable ASP.NET Core Module debug logs:</span></span>

```xml
<aspNetCore ...>
  <handlerSettings>
    <handlerSetting name="debugLevel" value="file" />
    <handlerSetting name="debugFile" value="c:\temp\ancm.log" />
  </handlerSettings>
</aspNetCore>
```

<span data-ttu-id="93ce2-235">Upewnij się, czy ścieżka określona dla dziennika istnieje i że tożsamość puli aplikacji ma uprawnienia do zapisu do lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="93ce2-235">Confirm that the path specified for the log exists and that the app pool's identity has write permissions to the location.</span></span>

### <a name="application-event-log"></a><span data-ttu-id="93ce2-236">Dziennik zdarzeń aplikacji</span><span class="sxs-lookup"><span data-stu-id="93ce2-236">Application Event Log</span></span>

<span data-ttu-id="93ce2-237">Dostęp do dziennika zdarzeń aplikacji:</span><span class="sxs-lookup"><span data-stu-id="93ce2-237">Access the Application Event Log:</span></span>

1. <span data-ttu-id="93ce2-238">Otwieranie Start menu, wyszukaj **Podgląd zdarzeń**, a następnie wybierz pozycję **Podgląd zdarzeń** aplikacji.</span><span class="sxs-lookup"><span data-stu-id="93ce2-238">Open the Start menu, search for **Event Viewer**, and then select the **Event Viewer** app.</span></span>
1. <span data-ttu-id="93ce2-239">W **Podgląd zdarzeń**, otwórz **Dzienniki Windows** węzła.</span><span class="sxs-lookup"><span data-stu-id="93ce2-239">In **Event Viewer**, open the **Windows Logs** node.</span></span>
1. <span data-ttu-id="93ce2-240">Wybierz **aplikacji** można otworzyć dziennika zdarzeń aplikacji.</span><span class="sxs-lookup"><span data-stu-id="93ce2-240">Select **Application** to open the Application Event Log.</span></span>
1. <span data-ttu-id="93ce2-241">Wyszukaj błędy skojarzone z aplikacją się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="93ce2-241">Search for errors associated with the failing app.</span></span> <span data-ttu-id="93ce2-242">Błędy mają wartość *moduł AspNetCore IIS* lub *usług IIS Express AspNetCore modułu* w *źródła* kolumny.</span><span class="sxs-lookup"><span data-stu-id="93ce2-242">Errors have a value of *IIS AspNetCore Module* or *IIS Express AspNetCore Module* in the *Source* column.</span></span>

### <a name="run-the-app-at-a-command-prompt"></a><span data-ttu-id="93ce2-243">Uruchamianie aplikacji w wierszu polecenia</span><span class="sxs-lookup"><span data-stu-id="93ce2-243">Run the app at a command prompt</span></span>

<span data-ttu-id="93ce2-244">Wiele błędów uruchamiania przestaną generować przydatne informacje w dzienniku zdarzeń aplikacji.</span><span class="sxs-lookup"><span data-stu-id="93ce2-244">Many startup errors don't produce useful information in the Application Event Log.</span></span> <span data-ttu-id="93ce2-245">Przyczyną niektórych błędów można znaleźć, uruchamiając aplikację w wierszu polecenia w systemie hostingu.</span><span class="sxs-lookup"><span data-stu-id="93ce2-245">You can find the cause of some errors by running the app at a command prompt on the hosting system.</span></span>

#### <a name="framework-dependent-deployment"></a><span data-ttu-id="93ce2-246">Wdrożenie zależny od struktury</span><span class="sxs-lookup"><span data-stu-id="93ce2-246">Framework-dependent deployment</span></span>

<span data-ttu-id="93ce2-247">Jeśli aplikacja jest [wdrożenia zależny od struktury](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span><span class="sxs-lookup"><span data-stu-id="93ce2-247">If the app is a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>

1. <span data-ttu-id="93ce2-248">W wierszu polecenia przejdź do folderu wdrożenia i uruchomienia aplikacji, wykonując zestaw aplikacji za pomocą *dotnet.exe*.</span><span class="sxs-lookup"><span data-stu-id="93ce2-248">At a command prompt, navigate to the deployment folder and run the app by executing the app's assembly with *dotnet.exe*.</span></span> <span data-ttu-id="93ce2-249">W poniższym poleceniu zastąp nazwę zestawu aplikacji dla \<assembly_name >: `dotnet .\<assembly_name>.dll`.</span><span class="sxs-lookup"><span data-stu-id="93ce2-249">In the following command, substitute the name of the app's assembly for \<assembly_name>: `dotnet .\<assembly_name>.dll`.</span></span>
1. <span data-ttu-id="93ce2-250">Dane wyjściowe z aplikacji, przedstawiający wszystkie błędy z konsoli są zapisywane w oknie konsoli.</span><span class="sxs-lookup"><span data-stu-id="93ce2-250">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="93ce2-251">Jeśli wystąpią błędy, gdy kieruje żądanie do aplikacji, należy wysłać żądanie do hosta i portu, na którym nasłuchuje Kestrel.</span><span class="sxs-lookup"><span data-stu-id="93ce2-251">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="93ce2-252">Przy użyciu domyślnego hosta i post, zgłosić wniosek o `http://localhost:5000/`.</span><span class="sxs-lookup"><span data-stu-id="93ce2-252">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="93ce2-253">Jeśli aplikacja reaguje, zwykle pod adresem punktu końcowego Kestrel, problem najprawdopodobniej związanych z konfiguracją hostingu i mniej prawdopodobne w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="93ce2-253">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the hosting configuration and less likely within the app.</span></span>

#### <a name="self-contained-deployment"></a><span data-ttu-id="93ce2-254">Niezależne wdrożenia</span><span class="sxs-lookup"><span data-stu-id="93ce2-254">Self-contained deployment</span></span>

<span data-ttu-id="93ce2-255">Jeśli aplikacja jest [niezależna wdrożenia](/dotnet/core/deploying/#self-contained-deployments-scd):</span><span class="sxs-lookup"><span data-stu-id="93ce2-255">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>

1. <span data-ttu-id="93ce2-256">W wierszu polecenia przejdź do folderu wdrożenia i uruchomienia pliku wykonywalnego aplikacji.</span><span class="sxs-lookup"><span data-stu-id="93ce2-256">At a command prompt, navigate to the deployment folder and run the app's executable.</span></span> <span data-ttu-id="93ce2-257">W poniższym poleceniu zastąp nazwę zestawu aplikacji dla \<assembly_name >: `<assembly_name>.exe`.</span><span class="sxs-lookup"><span data-stu-id="93ce2-257">In the following command, substitute the name of the app's assembly for \<assembly_name>: `<assembly_name>.exe`.</span></span>
1. <span data-ttu-id="93ce2-258">Dane wyjściowe z aplikacji, przedstawiający wszystkie błędy z konsoli są zapisywane w oknie konsoli.</span><span class="sxs-lookup"><span data-stu-id="93ce2-258">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="93ce2-259">Jeśli wystąpią błędy, gdy kieruje żądanie do aplikacji, należy wysłać żądanie do hosta i portu, na którym nasłuchuje Kestrel.</span><span class="sxs-lookup"><span data-stu-id="93ce2-259">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="93ce2-260">Przy użyciu domyślnego hosta i post, zgłosić wniosek o `http://localhost:5000/`.</span><span class="sxs-lookup"><span data-stu-id="93ce2-260">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="93ce2-261">Jeśli aplikacja reaguje, zwykle pod adresem punktu końcowego Kestrel, problem najprawdopodobniej związanych z konfiguracją hostingu i mniej prawdopodobne w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="93ce2-261">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the hosting configuration and less likely within the app.</span></span>

### <a name="aspnet-core-module-stdout-log"></a><span data-ttu-id="93ce2-262">ASP.NET Core modułu stdout dziennika</span><span class="sxs-lookup"><span data-stu-id="93ce2-262">ASP.NET Core Module stdout log</span></span>

<span data-ttu-id="93ce2-263">Włączanie i wyświetlanie dzienników stdout:</span><span class="sxs-lookup"><span data-stu-id="93ce2-263">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="93ce2-264">Przejdź do folderu wdrożenia witryny w systemie hostingu.</span><span class="sxs-lookup"><span data-stu-id="93ce2-264">Navigate to the site's deployment folder on the hosting system.</span></span>
1. <span data-ttu-id="93ce2-265">Jeśli *dzienniki* folder nie jest obecny, Utwórz folder.</span><span class="sxs-lookup"><span data-stu-id="93ce2-265">If the *logs* folder isn't present, create the folder.</span></span> <span data-ttu-id="93ce2-266">Aby uzyskać instrukcje dotyczące włączania MSBuild tworzenia *dzienniki* folderu we wdrożeniu automatycznie, zobacz [strukturę katalogów](xref:host-and-deploy/directory-structure) tematu.</span><span class="sxs-lookup"><span data-stu-id="93ce2-266">For instructions on how to enable MSBuild to create the *logs* folder in the deployment automatically, see the [Directory structure](xref:host-and-deploy/directory-structure) topic.</span></span>
1. <span data-ttu-id="93ce2-267">Edytuj *web.config* pliku.</span><span class="sxs-lookup"><span data-stu-id="93ce2-267">Edit the *web.config* file.</span></span> <span data-ttu-id="93ce2-268">Ustaw **stdoutLogEnabled** do `true` i zmień **stdoutLogFile** ścieżki, aby wskazywał *dzienniki* folderu (na przykład `.\logs\stdout`).</span><span class="sxs-lookup"><span data-stu-id="93ce2-268">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to point to the *logs* folder (for example, `.\logs\stdout`).</span></span> <span data-ttu-id="93ce2-269">`stdout` w ścieżce jest prefiks nazwy pliku dziennika.</span><span class="sxs-lookup"><span data-stu-id="93ce2-269">`stdout` in the path is the log file name prefix.</span></span> <span data-ttu-id="93ce2-270">Sygnatura czasowa, identyfikator procesu i rozszerzenie pliku są dodawane automatycznie, gdy zostanie utworzony dziennik.</span><span class="sxs-lookup"><span data-stu-id="93ce2-270">A timestamp, process id, and file extension are added automatically when the log is created.</span></span> <span data-ttu-id="93ce2-271">Za pomocą `stdout` jako prefiks nazwy pliku, plik dziennika typowe o nazwie *stdout_20180205184032_5412.log*.</span><span class="sxs-lookup"><span data-stu-id="93ce2-271">Using `stdout` as the file name prefix, a typical log file is named *stdout_20180205184032_5412.log*.</span></span>
1. <span data-ttu-id="93ce2-272">Upewnij się, tożsamość puli aplikacji ma uprawnienia do zapisu *dzienniki* folderu.</span><span class="sxs-lookup"><span data-stu-id="93ce2-272">Ensure your application pool's identity has write permissions to the *logs* folder.</span></span>
1. <span data-ttu-id="93ce2-273">Zapisz zaktualizowany *web.config* pliku.</span><span class="sxs-lookup"><span data-stu-id="93ce2-273">Save the updated *web.config* file.</span></span>
1. <span data-ttu-id="93ce2-274">Wysłać żądanie do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="93ce2-274">Make a request to the app.</span></span>
1. <span data-ttu-id="93ce2-275">Przejdź do *dzienniki* folderu.</span><span class="sxs-lookup"><span data-stu-id="93ce2-275">Navigate to the *logs* folder.</span></span> <span data-ttu-id="93ce2-276">Znajdowanie i otwieranie najnowszych dziennika stdout.</span><span class="sxs-lookup"><span data-stu-id="93ce2-276">Find and open the most recent stdout log.</span></span>
1. <span data-ttu-id="93ce2-277">Badanie w dzienniku błędów.</span><span class="sxs-lookup"><span data-stu-id="93ce2-277">Study the log for errors.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="93ce2-278">Wyłącz rejestrowanie strumienia stdout, po zakończeniu rozwiązywania problemów.</span><span class="sxs-lookup"><span data-stu-id="93ce2-278">Disable stdout logging when troubleshooting is complete.</span></span>

1. <span data-ttu-id="93ce2-279">Edytuj *web.config* pliku.</span><span class="sxs-lookup"><span data-stu-id="93ce2-279">Edit the *web.config* file.</span></span>
1. <span data-ttu-id="93ce2-280">Ustaw **stdoutLogEnabled** do `false`.</span><span class="sxs-lookup"><span data-stu-id="93ce2-280">Set **stdoutLogEnabled** to `false`.</span></span>
1. <span data-ttu-id="93ce2-281">Zapisz plik.</span><span class="sxs-lookup"><span data-stu-id="93ce2-281">Save the file.</span></span>

> [!WARNING]
> <span data-ttu-id="93ce2-282">Nie można wyłączyć dziennika stdout może prowadzić do awarii aplikacji lub serwera.</span><span class="sxs-lookup"><span data-stu-id="93ce2-282">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="93ce2-283">Brak brak limitu rozmiaru pliku dziennika lub liczba pliki dziennika utworzone.</span><span class="sxs-lookup"><span data-stu-id="93ce2-283">There's no limit on log file size or the number of log files created.</span></span>
>
> <span data-ttu-id="93ce2-284">Rutynowe logujesz się w aplikacji ASP.NET Core, użytku bibliotekę rejestrowania, która ogranicza rozmiar pliku dziennika i obraca się loguje.</span><span class="sxs-lookup"><span data-stu-id="93ce2-284">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="93ce2-285">Aby uzyskać więcej informacji, zobacz [rejestrowania innych dostawców](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="93ce2-285">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

## <a name="enable-the-developer-exception-page"></a><span data-ttu-id="93ce2-286">Włącz na stronie wyjątków dla deweloperów</span><span class="sxs-lookup"><span data-stu-id="93ce2-286">Enable the Developer Exception Page</span></span>

<span data-ttu-id="93ce2-287">`ASPNETCORE_ENVIRONMENT` [Zmiennej środowiskowej, można dodać do pliku web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) do uruchomienia aplikacji w środowisku programistycznym.</span><span class="sxs-lookup"><span data-stu-id="93ce2-287">The `ASPNETCORE_ENVIRONMENT` [environment variable can be added to web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) to run the app in the Development environment.</span></span> <span data-ttu-id="93ce2-288">Tak długo, jak środowisko nie jest zastąpione przy uruchamianiu aplikacji przez `UseEnvironment` umożliwia ustawienie zmiennej środowiskowej w Konstruktorze hosta [stronie wyjątków deweloperów](xref:fundamentals/error-handling) się pojawiać po uruchomieniu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="93ce2-288">As long as the environment isn't overridden in app startup by `UseEnvironment` on the host builder, setting the environment variable allows the [Developer Exception Page](xref:fundamentals/error-handling) to appear when the app is run.</span></span>

::: moniker range=">= aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile=".\logs\stdout"
      hostingModel="InProcess">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
  </environmentVariables>
</aspNetCore>
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile=".\logs\stdout">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
  </environmentVariables>
</aspNetCore>
```

::: moniker-end

<span data-ttu-id="93ce2-289">Ustawienie zmiennej środowiskowej, aby uzyskać `ASPNETCORE_ENVIRONMENT` jest zalecane tylko dla używane w przejściowym i testowania serwerów, które nie są połączone z Internetem.</span><span class="sxs-lookup"><span data-stu-id="93ce2-289">Setting the environment variable for `ASPNETCORE_ENVIRONMENT` is only recommended for use on staging and testing servers that aren't exposed to the Internet.</span></span> <span data-ttu-id="93ce2-290">Usuń zmienną środowiskową z *web.config* plik po rozwiązywania problemów.</span><span class="sxs-lookup"><span data-stu-id="93ce2-290">Remove the environment variable from the *web.config* file after troubleshooting.</span></span> <span data-ttu-id="93ce2-291">Aby uzyskać informacje na temat ustawiania zmiennych środowiskowych *web.config*, zobacz [environmentVariables element podrzędny elementu aspNetCore](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).</span><span class="sxs-lookup"><span data-stu-id="93ce2-291">For information on setting environment variables in *web.config*, see [environmentVariables child element of aspNetCore](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).</span></span>

## <a name="common-startup-errors"></a><span data-ttu-id="93ce2-292">Typowe błędy uruchamiania</span><span class="sxs-lookup"><span data-stu-id="93ce2-292">Common startup errors</span></span>

<span data-ttu-id="93ce2-293">Zobacz <xref:host-and-deploy/azure-iis-errors-reference>.</span><span class="sxs-lookup"><span data-stu-id="93ce2-293">See <xref:host-and-deploy/azure-iis-errors-reference>.</span></span> <span data-ttu-id="93ce2-294">Najbardziej typowe problemy, które uniemożliwiają uruchamianie aplikacji znajdują się w temacie odwołania.</span><span class="sxs-lookup"><span data-stu-id="93ce2-294">Most of the common problems that prevent app startup are covered in the reference topic.</span></span>

## <a name="obtain-data-from-an-app"></a><span data-ttu-id="93ce2-295">Uzyskiwanie danych z aplikacji</span><span class="sxs-lookup"><span data-stu-id="93ce2-295">Obtain data from an app</span></span>

<span data-ttu-id="93ce2-296">Jeśli aplikacja jest w stanie odpowiadać na żądania, żądania, połączenia i dodatkowych danych można uzyskać z aplikację za pomocą oprogramowania pośredniczącego terminalu wbudowanego.</span><span class="sxs-lookup"><span data-stu-id="93ce2-296">If an app is capable of responding to requests, obtain request, connection, and additional data from the app using terminal inline middleware.</span></span> <span data-ttu-id="93ce2-297">Aby uzyskać więcej informacji i przykładowy kod, zobacz <xref:test/troubleshoot#obtain-data-from-an-app>.</span><span class="sxs-lookup"><span data-stu-id="93ce2-297">For more information and sample code, see <xref:test/troubleshoot#obtain-data-from-an-app>.</span></span>

## <a name="create-a-dump"></a><span data-ttu-id="93ce2-298">Utwórz zrzut</span><span class="sxs-lookup"><span data-stu-id="93ce2-298">Create a dump</span></span>

<span data-ttu-id="93ce2-299">A *zrzutu* jest migawką pamięci systemowej i mogą pomóc w określeniu przyczyny awarii aplikacji, Niepowodzenie uruchamiania lub powolne aplikacji.</span><span class="sxs-lookup"><span data-stu-id="93ce2-299">A *dump* is a snapshot of the system's memory and can help determine the cause of an app crash, startup failure, or slow app.</span></span>

### <a name="app-crashes-or-encounters-an-exception"></a><span data-ttu-id="93ce2-300">Aplikacja ulegnie awarii lub napotka wyjątek</span><span class="sxs-lookup"><span data-stu-id="93ce2-300">App crashes or encounters an exception</span></span>

<span data-ttu-id="93ce2-301">Uzyskaj i analizowanie zrzutu z [raportowania błędów Windows (WER)](/windows/desktop/wer/windows-error-reporting):</span><span class="sxs-lookup"><span data-stu-id="93ce2-301">Obtain and analyze a dump from [Windows Error Reporting (WER)](/windows/desktop/wer/windows-error-reporting):</span></span>

1. <span data-ttu-id="93ce2-302">Utwórz folder do przechowywania plików zrzutu awaryjnego na `c:\dumps`.</span><span class="sxs-lookup"><span data-stu-id="93ce2-302">Create a folder to hold crash dump files at `c:\dumps`.</span></span> <span data-ttu-id="93ce2-303">Pula aplikacji musi mieć dostęp do zapisu do folderu.</span><span class="sxs-lookup"><span data-stu-id="93ce2-303">The app pool must have write access to the folder.</span></span>
1. <span data-ttu-id="93ce2-304">Uruchom [skrypt programu EnableDumps PowerShell](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/iis/troubleshoot/scripts/EnableDumps.ps1):</span><span class="sxs-lookup"><span data-stu-id="93ce2-304">Run the [EnableDumps PowerShell script](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/iis/troubleshoot/scripts/EnableDumps.ps1):</span></span>
   * <span data-ttu-id="93ce2-305">Jeśli aplikacja korzysta z [modelu hostingu w trakcie](xref:fundamentals/servers/index#in-process-hosting-model), uruchom skrypt *w3wp.exe*:</span><span class="sxs-lookup"><span data-stu-id="93ce2-305">If the app uses the [in-process hosting model](xref:fundamentals/servers/index#in-process-hosting-model), run the script for *w3wp.exe*:</span></span>

     ```console
     .\EnableDumps w3wp.exe c:\dumps
     ```

   * <span data-ttu-id="93ce2-306">Jeśli aplikacja korzysta z [modelu hostingu poza procesem](xref:fundamentals/servers/index#out-of-process-hosting-model), uruchom skrypt *dotnet.exe*:</span><span class="sxs-lookup"><span data-stu-id="93ce2-306">If the app uses the [out-of-process hosting model](xref:fundamentals/servers/index#out-of-process-hosting-model), run the script for *dotnet.exe*:</span></span>

     ```console
     .\EnableDumps dotnet.exe c:\dumps
     ```

1. <span data-ttu-id="93ce2-307">Uruchom aplikację zgodnie z warunkami, które powodują awarię wystąpić.</span><span class="sxs-lookup"><span data-stu-id="93ce2-307">Run the app under the conditions that cause the crash to occur.</span></span>
1. <span data-ttu-id="93ce2-308">Po przeprowadzeniu awarii Uruchom [skrypt programu DisableDumps PowerShell](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/iis/troubleshoot/scripts/DisableDumps.ps1):</span><span class="sxs-lookup"><span data-stu-id="93ce2-308">After the crash has occurred, run the [DisableDumps PowerShell script](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/iis/troubleshoot/scripts/DisableDumps.ps1):</span></span>
   * <span data-ttu-id="93ce2-309">Jeśli aplikacja korzysta z [modelu hostingu w trakcie](xref:fundamentals/servers/index#in-process-hosting-model), uruchom skrypt *w3wp.exe*:</span><span class="sxs-lookup"><span data-stu-id="93ce2-309">If the app uses the [in-process hosting model](xref:fundamentals/servers/index#in-process-hosting-model), run the script for *w3wp.exe*:</span></span>

     ```console
     .\DisableDumps w3wp.exe
     ```

   * <span data-ttu-id="93ce2-310">Jeśli aplikacja korzysta z [modelu hostingu poza procesem](xref:fundamentals/servers/index#out-of-process-hosting-model), uruchom skrypt *dotnet.exe*:</span><span class="sxs-lookup"><span data-stu-id="93ce2-310">If the app uses the [out-of-process hosting model](xref:fundamentals/servers/index#out-of-process-hosting-model), run the script for *dotnet.exe*:</span></span>

     ```console
     .\DisableDumps dotnet.exe
     ```

<span data-ttu-id="93ce2-311">Po zakończeniu zbierania zrzutu awarii aplikacji oraz aplikację może zakończyć w zwykły sposób.</span><span class="sxs-lookup"><span data-stu-id="93ce2-311">After an app crashes and dump collection is complete, the app is allowed to terminate normally.</span></span> <span data-ttu-id="93ce2-312">Skrypt programu PowerShell umożliwia skonfigurowanie raportowania błędów systemu Windows do zbierania zrzutów maksymalnie pięć na aplikację.</span><span class="sxs-lookup"><span data-stu-id="93ce2-312">The PowerShell script configures WER to collect up to five dumps per app.</span></span>

> [!WARNING]
> <span data-ttu-id="93ce2-313">Zrzuty awaryjne może potrwać dużą ilość miejsca na dysku (maksymalnie kilka gigabajtów każdego).</span><span class="sxs-lookup"><span data-stu-id="93ce2-313">Crash dumps might take up a large amount of disk space (up to several gigabytes each).</span></span>

### <a name="app-hangs-fails-during-startup-or-runs-normally"></a><span data-ttu-id="93ce2-314">Aplikacja zawiesza się zakończy się niepowodzeniem podczas uruchamiania i działa normalnie</span><span class="sxs-lookup"><span data-stu-id="93ce2-314">App hangs, fails during startup, or runs normally</span></span>

<span data-ttu-id="93ce2-315">Gdy aplikacja *zawiesza się* (przestaje odpowiadać, ale nie awarii), zakończy się niepowodzeniem podczas uruchamiania lub działa normalnie, zobacz [plików zrzut trybu użytkownika: Wybieranie najlepszych narzędzi](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool) do wybrania odpowiedniego narzędzia do tworzenia zrzutu.</span><span class="sxs-lookup"><span data-stu-id="93ce2-315">When an app *hangs* (stops responding but doesn't crash), fails during startup, or runs normally, see [User-Mode Dump Files: Choosing the Best Tool](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool) to select an appropriate tool to produce the dump.</span></span>

### <a name="analyze-the-dump"></a><span data-ttu-id="93ce2-316">Analizowanie zrzutu</span><span class="sxs-lookup"><span data-stu-id="93ce2-316">Analyze the dump</span></span>

<span data-ttu-id="93ce2-317">Zrzut można analizować przy użyciu kilku metod.</span><span class="sxs-lookup"><span data-stu-id="93ce2-317">A dump can be analyzed using several approaches.</span></span> <span data-ttu-id="93ce2-318">Aby uzyskać więcej informacji, zobacz [analizowanie plik zrzutu trybu użytkownika](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file).</span><span class="sxs-lookup"><span data-stu-id="93ce2-318">For more information, see [Analyzing a User-Mode Dump File](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file).</span></span>

## <a name="remote-debugging"></a><span data-ttu-id="93ce2-319">Debugowanie zdalne</span><span class="sxs-lookup"><span data-stu-id="93ce2-319">Remote debugging</span></span>

<span data-ttu-id="93ce2-320">Zobacz [zdalne debugowanie platformy ASP.NET Core na komputerze zdalnym usług IIS w programie Visual Studio 2017](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer) w dokumentacji programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="93ce2-320">See [Remote Debug ASP.NET Core on a Remote IIS Computer in Visual Studio 2017](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer) in the Visual Studio documentation.</span></span>

## <a name="application-insights"></a><span data-ttu-id="93ce2-321">Application Insights</span><span class="sxs-lookup"><span data-stu-id="93ce2-321">Application Insights</span></span>

<span data-ttu-id="93ce2-322">[Usługa Application Insights](/azure/application-insights/) udostępnia dane telemetryczne z aplikacji hostowanych przez usługi IIS, w tym rejestrowanie i funkcji raportowania błędów.</span><span class="sxs-lookup"><span data-stu-id="93ce2-322">[Application Insights](/azure/application-insights/) provides telemetry from apps hosted by IIS, including error logging and reporting features.</span></span> <span data-ttu-id="93ce2-323">Usługa Application Insights można tylko raporty dotyczące błędów występujących po uruchomieniu aplikacji, gdy staną się dostępne funkcje rejestrowania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="93ce2-323">Application Insights can only report on errors that occur after the app starts when the app's logging features become available.</span></span> <span data-ttu-id="93ce2-324">Aby uzyskać więcej informacji, zobacz [usługi Application Insights dla platformy ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).</span><span class="sxs-lookup"><span data-stu-id="93ce2-324">For more information, see [Application Insights for ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="additional-advice"></a><span data-ttu-id="93ce2-325">Dodatkowe porady</span><span class="sxs-lookup"><span data-stu-id="93ce2-325">Additional advice</span></span>

<span data-ttu-id="93ce2-326">Czasami funkcjonalności aplikacji nie powiedzie się natychmiast po uaktualnieniu albo .NET Core SDK w wersjach maszyny lub pakiet rozwoju w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="93ce2-326">Sometimes a functioning app fails immediately after upgrading either the .NET Core SDK on the development machine or package versions within the app.</span></span> <span data-ttu-id="93ce2-327">W niektórych przypadkach niespójne pakietów może spowodować uszkodzenie aplikacji podczas przeprowadzania uaktualnienia głównych.</span><span class="sxs-lookup"><span data-stu-id="93ce2-327">In some cases, incoherent packages may break an app when performing major upgrades.</span></span> <span data-ttu-id="93ce2-328">Większość z tych problemów można naprawić, wykonując następujące instrukcje:</span><span class="sxs-lookup"><span data-stu-id="93ce2-328">Most of these issues can be fixed by following these instructions:</span></span>

1. <span data-ttu-id="93ce2-329">Usuń *bin* i *obj* folderów.</span><span class="sxs-lookup"><span data-stu-id="93ce2-329">Delete the *bin* and *obj* folders.</span></span>
1. <span data-ttu-id="93ce2-330">Wyczyść pakiet zapisuje w pamięci podręcznej w *% UserProfile %\\.nuget\\pakietów* i *% LocalAppData %\\Nuget\\pamięci podręcznej v3*.</span><span class="sxs-lookup"><span data-stu-id="93ce2-330">Clear the package caches at *%UserProfile%\\.nuget\\packages* and *%LocalAppData%\\Nuget\\v3-cache*.</span></span>
1. <span data-ttu-id="93ce2-331">Przywróć i skompiluj ponownie projekt.</span><span class="sxs-lookup"><span data-stu-id="93ce2-331">Restore and rebuild the project.</span></span>
1. <span data-ttu-id="93ce2-332">Upewnij się, że poprzedniego wdrożenia na serwerze została całkowicie usunięta przed ponownego wdrażania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="93ce2-332">Confirm that the prior deployment on the server has been completely deleted prior to redeploying the app.</span></span>

> [!TIP]
> <span data-ttu-id="93ce2-333">Wygodnym sposobem Wyczyść pamięć podręczną pakietu ma wykonać `dotnet nuget locals all --clear` z poziomu wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="93ce2-333">A convenient way to clear package caches is to execute `dotnet nuget locals all --clear` from a command prompt.</span></span>
>
> <span data-ttu-id="93ce2-334">Wyczyszczenie pamięci podręcznej pakietu może być również wykonywane przy użyciu [nuget.exe](https://www.nuget.org/downloads) narzędzie i wykonywania polecenia `nuget locals all -clear`.</span><span class="sxs-lookup"><span data-stu-id="93ce2-334">Clearing package caches can also be accomplished by using the [nuget.exe](https://www.nuget.org/downloads) tool and executing the command `nuget locals all -clear`.</span></span> <span data-ttu-id="93ce2-335">*nuget.exe* nie jest powiązane instalacji z pulpitu systemu operacyjnego Windows i należy uzyskać oddzielnie od [NuGet witryny sieci Web](https://www.nuget.org/downloads).</span><span class="sxs-lookup"><span data-stu-id="93ce2-335">*nuget.exe* isn't a bundled install with the Windows desktop operating system and must be obtained separately from the [NuGet website](https://www.nuget.org/downloads).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="93ce2-336">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="93ce2-336">Additional resources</span></span>

* <xref:test/troubleshoot>
* <xref:fundamentals/error-handling>
* <xref:host-and-deploy/azure-iis-errors-reference>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/azure-apps/troubleshoot>
