---
title: Rozwiązywanie problemów z platformą ASP.NET Core w usługach IIS
author: guardrex
description: Dowiedz się, jak diagnozować problemy z wdrożeniami usług Internet Information Services (IIS) w aplikacji platformy ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 03/06/2019
uid: host-and-deploy/iis/troubleshoot
ms.openlocfilehash: 2f36ae2bda8537e91a3bc925505986bdd6a22a47
ms.sourcegitcommit: 34bf9fc6ea814c039401fca174642f0acb14be3c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/14/2019
ms.locfileid: "57841556"
---
# <a name="troubleshoot-aspnet-core-on-iis"></a><span data-ttu-id="4cd1f-103">Rozwiązywanie problemów z platformą ASP.NET Core w usługach IIS</span><span class="sxs-lookup"><span data-stu-id="4cd1f-103">Troubleshoot ASP.NET Core on IIS</span></span>

<span data-ttu-id="4cd1f-104">Przez [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="4cd1f-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="4cd1f-105">Ten artykuł zawiera instrukcje na temat platformy ASP.NET Core zdiagnozować problem uruchamiania aplikacji w przypadku hostowania za pomocą [Internet Information Services (IIS)](/iis).</span><span class="sxs-lookup"><span data-stu-id="4cd1f-105">This article provides instructions on how to diagnose an ASP.NET Core app startup issue when hosting with [Internet Information Services (IIS)](/iis).</span></span> <span data-ttu-id="4cd1f-106">Informacje przedstawione w tym artykule mają zastosowanie do hostowania w usługach IIS w systemie Windows Server i Windows Desktop.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-106">The information in this article applies to hosting in IIS on Windows Server and Windows Desktop.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="4cd1f-107">W programie Visual Studio ma domyślnie wartość projektu ASP.NET Core [usług IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hostingu podczas debugowania.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-107">In Visual Studio, an ASP.NET Core project defaults to [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hosting during debugging.</span></span> <span data-ttu-id="4cd1f-108">A *502.5 - niepowodzenia procesu* lub *500.30 - Start błąd* występuje, gdy debugowanie lokalne może być troubleshooted przy użyciu porady w tym temacie.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-108">A *502.5 - Process Failure* or a *500.30 - Start Failure* that occurs when debugging locally can be troubleshooted using the advice in this topic.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="4cd1f-109">W programie Visual Studio ma domyślnie wartość projektu ASP.NET Core [usług IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hostingu podczas debugowania.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-109">In Visual Studio, an ASP.NET Core project defaults to [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hosting during debugging.</span></span> <span data-ttu-id="4cd1f-110">A *502.5 niepowodzenia procesu* występuje, gdy debugowanie lokalne może być troubleshooted przy użyciu porady w tym temacie.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-110">A *502.5 Process Failure* that occurs when debugging locally can be troubleshooted using the advice in this topic.</span></span>

::: moniker-end

<span data-ttu-id="4cd1f-111">Dodatkowe tematy dotyczące rozwiązywania problemów:</span><span class="sxs-lookup"><span data-stu-id="4cd1f-111">Additional troubleshooting topics:</span></span>

<xref:host-and-deploy/azure-apps/troubleshoot>  
<span data-ttu-id="4cd1f-112">Mimo że usługa App Service używa [modułu ASP.NET Core](xref:host-and-deploy/aspnet-core-module) i usługi IIS do hostowania aplikacji, zobacz temat dedykowanych instrukcje specyficzne dla usługi App Service.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-112">Although App Service uses the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) and IIS to host apps, see the dedicated topic for instructions specific to App Service.</span></span>

<xref:fundamentals/error-handling>  
<span data-ttu-id="4cd1f-113">Dowiedz się, jak do obsługi błędów w aplikacji platformy ASP.NET Core podczas programowania w systemie lokalnym.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-113">Discover how to handle errors in ASP.NET Core apps during development on a local system.</span></span>

[<span data-ttu-id="4cd1f-114">Naucz się debugować przy użyciu programu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4cd1f-114">Learn to debug using Visual Studio</span></span>](/visualstudio/debugger/getting-started-with-the-debugger)  
<span data-ttu-id="4cd1f-115">W tym temacie przedstawiono funkcje debugera programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-115">This topic introduces the features of the Visual Studio debugger.</span></span>

[<span data-ttu-id="4cd1f-116">Debugowanie za pomocą programu Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="4cd1f-116">Debugging with Visual Studio Code</span></span>](https://code.visualstudio.com/docs/editor/debugging)  
<span data-ttu-id="4cd1f-117">Więcej informacji na temat debugowania wbudowaną w programie Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-117">Learn about the debugging support built into Visual Studio Code.</span></span>

## <a name="app-startup-errors"></a><span data-ttu-id="4cd1f-118">Błędy uruchamiania aplikacji</span><span class="sxs-lookup"><span data-stu-id="4cd1f-118">App startup errors</span></span>

### <a name="5025-process-failure"></a><span data-ttu-id="4cd1f-119">502.5 niepowodzenie procesu</span><span class="sxs-lookup"><span data-stu-id="4cd1f-119">502.5 Process Failure</span></span>

<span data-ttu-id="4cd1f-120">Proces roboczy kończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-120">The worker process fails.</span></span> <span data-ttu-id="4cd1f-121">Nie zaczyna się aplikacja.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-121">The app doesn't start.</span></span>

<span data-ttu-id="4cd1f-122">Modułu ASP.NET Core próbuje uruchomić proces dotnet wewnętrznej bazy danych, ale nie została uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-122">The ASP.NET Core Module attempts to start the backend dotnet process but it fails to start.</span></span> <span data-ttu-id="4cd1f-123">Zazwyczaj można ustalić przyczyny niepowodzenia uruchamiania procesu na podstawie wpisów w [dziennik zdarzeń aplikacji](#application-event-log) i [dziennika stdout modułu ASP.NET Core](#aspnet-core-module-stdout-log).</span><span class="sxs-lookup"><span data-stu-id="4cd1f-123">The cause of a process startup failure can usually be determined from entries in the [Application Event Log](#application-event-log) and the [ASP.NET Core Module stdout log](#aspnet-core-module-stdout-log).</span></span> 

<span data-ttu-id="4cd1f-124">Aplikacja jest błędnie skonfigurowane z powodu przeznaczony dla wersji udostępnionej platformy ASP.NET Core, która nie jest obecny jest jakiś wspólny warunek błędu.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-124">A common failure condition is the app is misconfigured due to targeting a version of the ASP.NET Core shared framework that isn't present.</span></span> <span data-ttu-id="4cd1f-125">Sprawdź, które wersje udostępnionej platformy ASP.NET Core są zainstalowane na komputerze docelowym.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-125">Check which versions of the ASP.NET Core shared framework are installed on the target machine.</span></span>

<span data-ttu-id="4cd1f-126">*502.5 niepowodzenia procesu* strony błędu jest zwracany, jeśli aplikacji lub obsługującego błędnej konfiguracji powoduje niepowodzenie procesu roboczego:</span><span class="sxs-lookup"><span data-stu-id="4cd1f-126">The *502.5 Process Failure* error page is returned when a hosting or app misconfiguration causes the worker process to fail:</span></span>

![Okno przeglądarki, przedstawiający 502.5 stronę niepowodzenia procesu](troubleshoot/_static/process-failure-page.png)

::: moniker range=">= aspnetcore-2.2"

### <a name="50030-in-process-startup-failure"></a><span data-ttu-id="4cd1f-128">500.30 w procesie Niepowodzenie uruchamiania</span><span class="sxs-lookup"><span data-stu-id="4cd1f-128">500.30 In-Process Startup Failure</span></span>

<span data-ttu-id="4cd1f-129">Proces roboczy kończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-129">The worker process fails.</span></span> <span data-ttu-id="4cd1f-130">Nie zaczyna się aplikacja.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-130">The app doesn't start.</span></span>

<span data-ttu-id="4cd1f-131">Próbuje uruchomić program .NET Core CLR w procesie modułu ASP.NET Core, ale nie została uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-131">The ASP.NET Core Module attempts to start the .NET Core CLR in-process, but it fails to start.</span></span> <span data-ttu-id="4cd1f-132">Zazwyczaj można ustalić przyczyny niepowodzenia uruchamiania procesu na podstawie wpisów w [dziennik zdarzeń aplikacji](#application-event-log) i [dziennika stdout modułu ASP.NET Core](#aspnet-core-module-stdout-log).</span><span class="sxs-lookup"><span data-stu-id="4cd1f-132">The cause of a process startup failure can usually be determined from entries in the [Application Event Log](#application-event-log) and the [ASP.NET Core Module stdout log](#aspnet-core-module-stdout-log).</span></span> 

<span data-ttu-id="4cd1f-133">Aplikacja jest błędnie skonfigurowane z powodu przeznaczony dla wersji udostępnionej platformy ASP.NET Core, która nie jest obecny jest jakiś wspólny warunek błędu.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-133">A common failure condition is the app is misconfigured due to targeting a version of the ASP.NET Core shared framework that isn't present.</span></span> <span data-ttu-id="4cd1f-134">Sprawdź, które wersje udostępnionej platformy ASP.NET Core są zainstalowane na komputerze docelowym.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-134">Check which versions of the ASP.NET Core shared framework are installed on the target machine.</span></span>

### <a name="5000-in-process-handler-load-failure"></a><span data-ttu-id="4cd1f-135">500.0 w procesie programu obsługi błędu ładowania</span><span class="sxs-lookup"><span data-stu-id="4cd1f-135">500.0 In-Process Handler Load Failure</span></span>

<span data-ttu-id="4cd1f-136">Proces roboczy kończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-136">The worker process fails.</span></span> <span data-ttu-id="4cd1f-137">Nie zaczyna się aplikacja.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-137">The app doesn't start.</span></span>

<span data-ttu-id="4cd1f-138">Modułu ASP.NET Core nie powiedzie się znaleźć programu .NET Core CLR i Znajdź program obsługi żądania w trakcie (*aspnetcorev2_inprocess.dll*).</span><span class="sxs-lookup"><span data-stu-id="4cd1f-138">The ASP.NET Core Module fails to find the .NET Core CLR and find the in-process request handler (*aspnetcorev2_inprocess.dll*).</span></span> <span data-ttu-id="4cd1f-139">Sprawdź, czy:</span><span class="sxs-lookup"><span data-stu-id="4cd1f-139">Check that:</span></span>

* <span data-ttu-id="4cd1f-140">Aplikacja jest przeznaczona na albo [Microsoft.AspNetCore.Server.IIS](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IIS) pakietu NuGet lub [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="4cd1f-140">The app targets either the [Microsoft.AspNetCore.Server.IIS](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IIS) NuGet package or the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>
* <span data-ttu-id="4cd1f-141">Wersja udostępnionej platformy ASP.NET Core jest zainstalowanie aplikacji jest przeznaczony dla na komputerze docelowym.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-141">The version of the ASP.NET Core shared framework that the app targets is installed on the target machine.</span></span>

### <a name="5000-out-of-process-handler-load-failure"></a><span data-ttu-id="4cd1f-142">500.0 Błąd ładowania poza procesem programu obsługi</span><span class="sxs-lookup"><span data-stu-id="4cd1f-142">500.0 Out-Of-Process Handler Load Failure</span></span>

<span data-ttu-id="4cd1f-143">Proces roboczy kończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-143">The worker process fails.</span></span> <span data-ttu-id="4cd1f-144">Nie zaczyna się aplikacja.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-144">The app doesn't start.</span></span>

<span data-ttu-id="4cd1f-145">Modułu ASP.NET Core nie powiedzie się znaleźć spoza procesu hostingu Obsługa żądania.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-145">The ASP.NET Core Module fails to find the out-of-process hosting request handler.</span></span> <span data-ttu-id="4cd1f-146">Upewnij się, że *aspnetcorev2_outofprocess.dll* znajduje się w podfolderze obok *aspnetcorev2.dll*.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-146">Make sure the *aspnetcorev2_outofprocess.dll* is present in a subfolder next to *aspnetcorev2.dll*.</span></span> 

::: moniker-end

### <a name="500-internal-server-error"></a><span data-ttu-id="4cd1f-147">500 Wewnętrzny błąd serwera</span><span class="sxs-lookup"><span data-stu-id="4cd1f-147">500 Internal Server Error</span></span>

<span data-ttu-id="4cd1f-148">Uruchamia aplikację, ale błąd uniemożliwia spełnienie żądania przez serwer.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-148">The app starts, but an error prevents the server from fulfilling the request.</span></span>

<span data-ttu-id="4cd1f-149">Ten błąd występuje w kodzie aplikacji, podczas uruchamiania lub podczas tworzenia odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-149">This error occurs within the app's code during startup or while creating a response.</span></span> <span data-ttu-id="4cd1f-150">Odpowiedź może zawierać żadnej zawartości lub odpowiedzi może być wyświetlana jako *500 Wewnętrzny błąd serwera* w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-150">The response may contain no content, or the response may appear as a *500 Internal Server Error* in the browser.</span></span> <span data-ttu-id="4cd1f-151">W dzienniku zdarzeń aplikacji stwierdza, zwykle uruchomiona aplikacja.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-151">The Application Event Log usually states that the app started normally.</span></span> <span data-ttu-id="4cd1f-152">Z perspektywy serwera, który jest poprawna.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-152">From the server's perspective, that's correct.</span></span> <span data-ttu-id="4cd1f-153">Aplikacja została uruchomiona, ale nie może wygenerować prawidłowej odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-153">The app did start, but it can't generate a valid response.</span></span> <span data-ttu-id="4cd1f-154">[Uruchamianie aplikacji w wierszu polecenia](#run-the-app-at-a-command-prompt) na serwerze lub [Włącz dziennik stdout modułu ASP.NET Core](#aspnet-core-module-stdout-log) do rozwiązania problemu.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-154">[Run the app at a command prompt](#run-the-app-at-a-command-prompt) on the server or [enable the ASP.NET Core Module stdout log](#aspnet-core-module-stdout-log) to troubleshoot the problem.</span></span>

### <a name="failed-to-start-application-errorcode-0x800700c1"></a><span data-ttu-id="4cd1f-155">Nie można uruchomić aplikację (kod błędu "0x800700c1")</span><span class="sxs-lookup"><span data-stu-id="4cd1f-155">Failed to start application (ErrorCode '0x800700c1')</span></span>

```
EventID: 1010
Source: IIS AspNetCore Module V2
Failed to start application '/LM/W3SVC/6/ROOT/', ErrorCode '0x800700c1'.
```

<span data-ttu-id="4cd1f-156">Aplikacji nie powiodło się, ponieważ zestaw aplikacji (*.dll*) nie można go załadować.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-156">The app failed to start because the app's assembly (*.dll*) couldn't be loaded.</span></span>

<span data-ttu-id="4cd1f-157">Ten błąd występuje, gdy występuje niezgodność liczby bitów opublikowanej aplikacji i procesu w3wp/programu iisexpress.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-157">This error occurs when there's a bitness mismatch between the published app and the w3wp/iisexpress process.</span></span>

<span data-ttu-id="4cd1f-158">Upewnij się, że ustawienie 32-bitowych puli aplikacji jest prawidłowy:</span><span class="sxs-lookup"><span data-stu-id="4cd1f-158">Confirm that the app pool's 32-bit setting is correct:</span></span>

1. <span data-ttu-id="4cd1f-159">Wybierz pulę aplikacji w Menedżerze usług IIS w **pul aplikacji**.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-159">Select the app pool in IIS Manager's **Application Pools**.</span></span>
1. <span data-ttu-id="4cd1f-160">Wybierz **Zaawansowane ustawienia** w obszarze **edytowanie puli aplikacji** w **akcje** panelu.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-160">Select **Advanced Settings** under **Edit Application Pool** in the **Actions** panel.</span></span>
1. <span data-ttu-id="4cd1f-161">Ustaw **Włącz aplikacje 32-bitowe**:</span><span class="sxs-lookup"><span data-stu-id="4cd1f-161">Set **Enable 32-Bit Applications**:</span></span>
   * <span data-ttu-id="4cd1f-162">Jeśli wdrażanie (x86) 32-bitowych aplikacji, ustaw wartość `True`.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-162">If deploying a 32-bit (x86) app, set the value to `True`.</span></span>
   * <span data-ttu-id="4cd1f-163">Jeśli wdrażanie (x64) 64-bitowych aplikacji, ustaw wartość `False`.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-163">If deploying a 64-bit (x64) app, set the value to `False`.</span></span>

### <a name="connection-reset"></a><span data-ttu-id="4cd1f-164">Resetowanie połączenia</span><span class="sxs-lookup"><span data-stu-id="4cd1f-164">Connection reset</span></span>

<span data-ttu-id="4cd1f-165">Jeśli błąd wystąpi po nagłówki są wysyłane, jest za późno serwera wysłać **500 Wewnętrzny błąd serwera** po wystąpieniu błędu.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-165">If an error occurs after the headers are sent, it's too late for the server to send a **500 Internal Server Error** when an error occurs.</span></span> <span data-ttu-id="4cd1f-166">Dzieje się tak często, gdy wystąpi błąd podczas serializacji obiektów złożonych na odpowiedź.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-166">This often happens when an error occurs during the serialization of complex objects for a response.</span></span> <span data-ttu-id="4cd1f-167">Tego typu błędu jest wyświetlany jako *resetowania połączenia* błąd na komputerze klienckim.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-167">This type of error appears as a *connection reset* error on the client.</span></span> <span data-ttu-id="4cd1f-168">[Rejestrowanie aplikacji](xref:fundamentals/logging/index) mogą pomóc rozwiązać tego rodzaju błędów.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-168">[Application logging](xref:fundamentals/logging/index) can help troubleshoot these types of errors.</span></span>

## <a name="default-startup-limits"></a><span data-ttu-id="4cd1f-169">Domyślne limity uruchamiania</span><span class="sxs-lookup"><span data-stu-id="4cd1f-169">Default startup limits</span></span>

<span data-ttu-id="4cd1f-170">Modułu ASP.NET Core jest skonfigurowany z domyślną *startupTimeLimit* 120 sekund.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-170">The ASP.NET Core Module is configured with a default *startupTimeLimit* of 120 seconds.</span></span> <span data-ttu-id="4cd1f-171">Gdy pozostawić wartość domyślną, aplikacja może potrwać do dwóch minut przed moduł dzienniki awarii procesu.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-171">When left at the default value, an app may take up to two minutes to start before the module logs a process failure.</span></span> <span data-ttu-id="4cd1f-172">Aby uzyskać informacje na temat konfigurowania modułu, zobacz [atrybuty elementu aspNetCore](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span><span class="sxs-lookup"><span data-stu-id="4cd1f-172">For information on configuring the module, see [Attributes of the aspNetCore element](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span></span>

## <a name="troubleshoot-app-startup-errors"></a><span data-ttu-id="4cd1f-173">Rozwiązywanie problemów z błędami uruchamiania aplikacji</span><span class="sxs-lookup"><span data-stu-id="4cd1f-173">Troubleshoot app startup errors</span></span>

### <a name="enable-the-aspnet-core-module-debug-log"></a><span data-ttu-id="4cd1f-174">Włącz dziennik debugowania modułu ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4cd1f-174">Enable the ASP.NET Core Module debug log</span></span>

<span data-ttu-id="4cd1f-175">Dodaj poniższe ustawienia programu obsługi w aplikacji *web.config* plik, aby włączyć dzienniki debugowania modułu ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="4cd1f-175">Add the following handler settings to the app's *web.config* file to enable ASP.NET Core Module debug logs:</span></span>

```xml
<aspNetCore ...>
  <handlerSettings>
    <handlerSetting name="debugLevel" value="file" />
    <handlerSetting name="debugFile" value="c:\temp\ancm.log" />
  </handlerSettings>
</aspNetCore>
```

<span data-ttu-id="4cd1f-176">Upewnij się, czy ścieżka określona dla dziennika istnieje i że tożsamość puli aplikacji ma uprawnienia do zapisu do lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-176">Confirm that the path specified for the log exists and that the app pool's identity has write permissions to the location.</span></span>

### <a name="application-event-log"></a><span data-ttu-id="4cd1f-177">Dziennik zdarzeń aplikacji</span><span class="sxs-lookup"><span data-stu-id="4cd1f-177">Application Event Log</span></span>

<span data-ttu-id="4cd1f-178">Dostęp do dziennika zdarzeń aplikacji:</span><span class="sxs-lookup"><span data-stu-id="4cd1f-178">Access the Application Event Log:</span></span>

1. <span data-ttu-id="4cd1f-179">Otwieranie Start menu, wyszukaj **Podgląd zdarzeń**, a następnie wybierz pozycję **Podgląd zdarzeń** aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-179">Open the Start menu, search for **Event Viewer**, and then select the **Event Viewer** app.</span></span>
1. <span data-ttu-id="4cd1f-180">W **Podgląd zdarzeń**, otwórz **Dzienniki Windows** węzła.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-180">In **Event Viewer**, open the **Windows Logs** node.</span></span>
1. <span data-ttu-id="4cd1f-181">Wybierz **aplikacji** można otworzyć dziennika zdarzeń aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-181">Select **Application** to open the Application Event Log.</span></span>
1. <span data-ttu-id="4cd1f-182">Wyszukaj błędy skojarzone z aplikacją się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-182">Search for errors associated with the failing app.</span></span> <span data-ttu-id="4cd1f-183">Błędy mają wartość *moduł AspNetCore IIS* lub *usług IIS Express AspNetCore modułu* w *źródła* kolumny.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-183">Errors have a value of *IIS AspNetCore Module* or *IIS Express AspNetCore Module* in the *Source* column.</span></span>

### <a name="run-the-app-at-a-command-prompt"></a><span data-ttu-id="4cd1f-184">Uruchamianie aplikacji w wierszu polecenia</span><span class="sxs-lookup"><span data-stu-id="4cd1f-184">Run the app at a command prompt</span></span>

<span data-ttu-id="4cd1f-185">Wiele błędów uruchamiania przestaną generować przydatne informacje w dzienniku zdarzeń aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-185">Many startup errors don't produce useful information in the Application Event Log.</span></span> <span data-ttu-id="4cd1f-186">Przyczyną niektórych błędów można znaleźć, uruchamiając aplikację w wierszu polecenia w systemie hostingu.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-186">You can find the cause of some errors by running the app at a command prompt on the hosting system.</span></span>

#### <a name="framework-dependent-deployment"></a><span data-ttu-id="4cd1f-187">Wdrożenie zależny od struktury</span><span class="sxs-lookup"><span data-stu-id="4cd1f-187">Framework-dependent deployment</span></span>

<span data-ttu-id="4cd1f-188">Jeśli aplikacja jest [wdrożenia zależny od struktury](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span><span class="sxs-lookup"><span data-stu-id="4cd1f-188">If the app is a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>

1. <span data-ttu-id="4cd1f-189">W wierszu polecenia przejdź do folderu wdrożenia i uruchomienia aplikacji, wykonując zestaw aplikacji za pomocą *dotnet.exe*.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-189">At a command prompt, navigate to the deployment folder and run the app by executing the app's assembly with *dotnet.exe*.</span></span> <span data-ttu-id="4cd1f-190">W poniższym poleceniu zastąp nazwę zestawu aplikacji dla \<assembly_name >: `dotnet .\<assembly_name>.dll`.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-190">In the following command, substitute the name of the app's assembly for \<assembly_name>: `dotnet .\<assembly_name>.dll`.</span></span>
1. <span data-ttu-id="4cd1f-191">Dane wyjściowe z aplikacji, przedstawiający wszystkie błędy z konsoli są zapisywane w oknie konsoli.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-191">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="4cd1f-192">Jeśli wystąpią błędy, gdy kieruje żądanie do aplikacji, należy wysłać żądanie do hosta i portu, na którym nasłuchuje Kestrel.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-192">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="4cd1f-193">Przy użyciu domyślnego hosta i post, zgłosić wniosek o `http://localhost:5000/`.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-193">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="4cd1f-194">Jeśli aplikacja reaguje, zwykle pod adresem punktu końcowego Kestrel, problem najprawdopodobniej związanych z konfiguracją hostingu i mniej prawdopodobne w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-194">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the hosting configuration and less likely within the app.</span></span>

#### <a name="self-contained-deployment"></a><span data-ttu-id="4cd1f-195">Niezależne wdrożenia</span><span class="sxs-lookup"><span data-stu-id="4cd1f-195">Self-contained deployment</span></span>

<span data-ttu-id="4cd1f-196">Jeśli aplikacja jest [niezależna wdrożenia](/dotnet/core/deploying/#self-contained-deployments-scd):</span><span class="sxs-lookup"><span data-stu-id="4cd1f-196">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>

1. <span data-ttu-id="4cd1f-197">W wierszu polecenia przejdź do folderu wdrożenia i uruchomienia pliku wykonywalnego aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-197">At a command prompt, navigate to the deployment folder and run the app's executable.</span></span> <span data-ttu-id="4cd1f-198">W poniższym poleceniu zastąp nazwę zestawu aplikacji dla \<assembly_name >: `<assembly_name>.exe`.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-198">In the following command, substitute the name of the app's assembly for \<assembly_name>: `<assembly_name>.exe`.</span></span>
1. <span data-ttu-id="4cd1f-199">Dane wyjściowe z aplikacji, przedstawiający wszystkie błędy z konsoli są zapisywane w oknie konsoli.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-199">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="4cd1f-200">Jeśli wystąpią błędy, gdy kieruje żądanie do aplikacji, należy wysłać żądanie do hosta i portu, na którym nasłuchuje Kestrel.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-200">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="4cd1f-201">Przy użyciu domyślnego hosta i post, zgłosić wniosek o `http://localhost:5000/`.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-201">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="4cd1f-202">Jeśli aplikacja reaguje, zwykle pod adresem punktu końcowego Kestrel, problem najprawdopodobniej związanych z konfiguracją hostingu i mniej prawdopodobne w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-202">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the hosting configuration and less likely within the app.</span></span>

### <a name="aspnet-core-module-stdout-log"></a><span data-ttu-id="4cd1f-203">ASP.NET Core modułu stdout dziennika</span><span class="sxs-lookup"><span data-stu-id="4cd1f-203">ASP.NET Core Module stdout log</span></span>

<span data-ttu-id="4cd1f-204">Włączanie i wyświetlanie dzienników stdout:</span><span class="sxs-lookup"><span data-stu-id="4cd1f-204">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="4cd1f-205">Przejdź do folderu wdrożenia witryny w systemie hostingu.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-205">Navigate to the site's deployment folder on the hosting system.</span></span>
1. <span data-ttu-id="4cd1f-206">Jeśli *dzienniki* folder nie jest obecny, Utwórz folder.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-206">If the *logs* folder isn't present, create the folder.</span></span> <span data-ttu-id="4cd1f-207">Aby uzyskać instrukcje dotyczące włączania MSBuild tworzenia *dzienniki* folderu we wdrożeniu automatycznie, zobacz [strukturę katalogów](xref:host-and-deploy/directory-structure) tematu.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-207">For instructions on how to enable MSBuild to create the *logs* folder in the deployment automatically, see the [Directory structure](xref:host-and-deploy/directory-structure) topic.</span></span>
1. <span data-ttu-id="4cd1f-208">Edytuj *web.config* pliku.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-208">Edit the *web.config* file.</span></span> <span data-ttu-id="4cd1f-209">Ustaw **stdoutLogEnabled** do `true` i zmień **stdoutLogFile** ścieżki, aby wskazywał *dzienniki* folderu (na przykład `.\logs\stdout`).</span><span class="sxs-lookup"><span data-stu-id="4cd1f-209">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to point to the *logs* folder (for example, `.\logs\stdout`).</span></span> <span data-ttu-id="4cd1f-210">`stdout` w ścieżce jest prefiks nazwy pliku dziennika.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-210">`stdout` in the path is the log file name prefix.</span></span> <span data-ttu-id="4cd1f-211">Sygnatura czasowa, identyfikator procesu i rozszerzenie pliku są dodawane automatycznie, gdy zostanie utworzony dziennik.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-211">A timestamp, process id, and file extension are added automatically when the log is created.</span></span> <span data-ttu-id="4cd1f-212">Za pomocą `stdout` jako prefiks nazwy pliku, plik dziennika typowe o nazwie *stdout_20180205184032_5412.log*.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-212">Using `stdout` as the file name prefix, a typical log file is named *stdout_20180205184032_5412.log*.</span></span> 
1. <span data-ttu-id="4cd1f-213">Upewnij się, tożsamość puli aplikacji ma uprawnienia do zapisu *dzienniki* folderu.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-213">Ensure your application pool's identity has write permissions to the *logs* folder.</span></span>
1. <span data-ttu-id="4cd1f-214">Zapisz zaktualizowany *web.config* pliku.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-214">Save the updated *web.config* file.</span></span>
1. <span data-ttu-id="4cd1f-215">Wysłać żądanie do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-215">Make a request to the app.</span></span>
1. <span data-ttu-id="4cd1f-216">Przejdź do *dzienniki* folderu.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-216">Navigate to the *logs* folder.</span></span> <span data-ttu-id="4cd1f-217">Znajdowanie i otwieranie najnowszych dziennika stdout.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-217">Find and open the most recent stdout log.</span></span>
1. <span data-ttu-id="4cd1f-218">Badanie w dzienniku błędów.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-218">Study the log for errors.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="4cd1f-219">Wyłącz rejestrowanie strumienia stdout, po zakończeniu rozwiązywania problemów.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-219">Disable stdout logging when troubleshooting is complete.</span></span>

1. <span data-ttu-id="4cd1f-220">Edytuj *web.config* pliku.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-220">Edit the *web.config* file.</span></span>
1. <span data-ttu-id="4cd1f-221">Ustaw **stdoutLogEnabled** do `false`.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-221">Set **stdoutLogEnabled** to `false`.</span></span>
1. <span data-ttu-id="4cd1f-222">Zapisz plik.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-222">Save the file.</span></span>

> [!WARNING]
> <span data-ttu-id="4cd1f-223">Nie można wyłączyć dziennika stdout może prowadzić do awarii aplikacji lub serwera.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-223">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="4cd1f-224">Brak brak limitu rozmiaru pliku dziennika lub liczba pliki dziennika utworzone.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-224">There's no limit on log file size or the number of log files created.</span></span>
>
> <span data-ttu-id="4cd1f-225">Rutynowe logujesz się w aplikacji ASP.NET Core, użytku bibliotekę rejestrowania, która ogranicza rozmiar pliku dziennika i obraca się loguje.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-225">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="4cd1f-226">Aby uzyskać więcej informacji, zobacz [rejestrowania innych dostawców](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="4cd1f-226">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

## <a name="enable-the-developer-exception-page"></a><span data-ttu-id="4cd1f-227">Włącz na stronie wyjątków dla deweloperów</span><span class="sxs-lookup"><span data-stu-id="4cd1f-227">Enable the Developer Exception Page</span></span>

<span data-ttu-id="4cd1f-228">`ASPNETCORE_ENVIRONMENT` [Zmiennej środowiskowej, można dodać do pliku web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) do uruchomienia aplikacji w środowisku programistycznym.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-228">The `ASPNETCORE_ENVIRONMENT` [environment variable can be added to web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) to run the app in the Development environment.</span></span> <span data-ttu-id="4cd1f-229">Tak długo, jak środowisko nie jest zastąpione przy uruchamianiu aplikacji przez `UseEnvironment` umożliwia ustawienie zmiennej środowiskowej w Konstruktorze hosta [stronie wyjątków deweloperów](xref:fundamentals/error-handling) się pojawiać po uruchomieniu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-229">As long as the environment isn't overridden in app startup by `UseEnvironment` on the host builder, setting the environment variable allows the [Developer Exception Page](xref:fundamentals/error-handling) to appear when the app is run.</span></span>

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

<span data-ttu-id="4cd1f-230">Ustawienie zmiennej środowiskowej, aby uzyskać `ASPNETCORE_ENVIRONMENT` jest zalecane tylko dla używane w przejściowym i testowania serwerów, które nie są połączone z Internetem.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-230">Setting the environment variable for `ASPNETCORE_ENVIRONMENT` is only recommended for use on staging and testing servers that aren't exposed to the Internet.</span></span> <span data-ttu-id="4cd1f-231">Usuń zmienną środowiskową z *web.config* plik po rozwiązywania problemów.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-231">Remove the environment variable from the *web.config* file after troubleshooting.</span></span> <span data-ttu-id="4cd1f-232">Aby uzyskać informacje na temat ustawiania zmiennych środowiskowych *web.config*, zobacz [environmentVariables element podrzędny elementu aspNetCore](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).</span><span class="sxs-lookup"><span data-stu-id="4cd1f-232">For information on setting environment variables in *web.config*, see [environmentVariables child element of aspNetCore](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).</span></span>

## <a name="common-startup-errors"></a><span data-ttu-id="4cd1f-233">Typowe błędy uruchamiania</span><span class="sxs-lookup"><span data-stu-id="4cd1f-233">Common startup errors</span></span>

<span data-ttu-id="4cd1f-234">Zobacz <xref:host-and-deploy/azure-iis-errors-reference>.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-234">See <xref:host-and-deploy/azure-iis-errors-reference>.</span></span> <span data-ttu-id="4cd1f-235">Najbardziej typowe problemy, które uniemożliwiają uruchamianie aplikacji znajdują się w temacie odwołania.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-235">Most of the common problems that prevent app startup are covered in the reference topic.</span></span>

## <a name="obtain-data-from-an-app"></a><span data-ttu-id="4cd1f-236">Uzyskiwanie danych z aplikacji</span><span class="sxs-lookup"><span data-stu-id="4cd1f-236">Obtain data from an app</span></span>

<span data-ttu-id="4cd1f-237">Jeśli aplikacja jest w stanie odpowiadać na żądania, żądania, połączenia i dodatkowych danych można uzyskać z aplikację za pomocą oprogramowania pośredniczącego terminalu wbudowanego.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-237">If an app is capable of responding to requests, obtain request, connection, and additional data from the app using terminal inline middleware.</span></span> <span data-ttu-id="4cd1f-238">Aby uzyskać więcej informacji i przykładowy kod, zobacz <xref:test/troubleshoot#obtain-data-from-an-app>.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-238">For more information and sample code, see <xref:test/troubleshoot#obtain-data-from-an-app>.</span></span>

## <a name="create-a-dump"></a><span data-ttu-id="4cd1f-239">Utwórz zrzut</span><span class="sxs-lookup"><span data-stu-id="4cd1f-239">Create a dump</span></span>

<span data-ttu-id="4cd1f-240">A *zrzutu* jest migawką pamięci systemowej i mogą pomóc w określeniu przyczyny awarii aplikacji, Niepowodzenie uruchamiania lub powolne aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-240">A *dump* is a snapshot of the system's memory and can help determine the cause of an app crash, startup failure, or slow app.</span></span>

### <a name="app-crashes-or-encounters-an-exception"></a><span data-ttu-id="4cd1f-241">Aplikacja ulegnie awarii lub napotka wyjątek</span><span class="sxs-lookup"><span data-stu-id="4cd1f-241">App crashes or encounters an exception</span></span>

<span data-ttu-id="4cd1f-242">Uzyskaj i analizowanie zrzutu z [raportowania błędów Windows (WER)](/windows/desktop/wer/windows-error-reporting):</span><span class="sxs-lookup"><span data-stu-id="4cd1f-242">Obtain and analyze a dump from [Windows Error Reporting (WER)](/windows/desktop/wer/windows-error-reporting):</span></span>

1. <span data-ttu-id="4cd1f-243">Utwórz folder do przechowywania plików zrzutu awaryjnego na `c:\dumps`.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-243">Create a folder to hold crash dump files at `c:\dumps`.</span></span> <span data-ttu-id="4cd1f-244">Pula aplikacji musi mieć dostęp do zapisu do folderu.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-244">The app pool must have write access to the folder.</span></span>
1. <span data-ttu-id="4cd1f-245">Uruchom [skrypt programu EnableDumps PowerShell](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/troubleshoot/scripts/EnableDumps.ps1):</span><span class="sxs-lookup"><span data-stu-id="4cd1f-245">Run the [EnableDumps PowerShell script](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/troubleshoot/scripts/EnableDumps.ps1):</span></span>
   * <span data-ttu-id="4cd1f-246">Jeśli aplikacja korzysta z [modelu hostingu w trakcie](xref:fundamentals/servers/index#in-process-hosting-model), uruchom skrypt *w3wp.exe*:</span><span class="sxs-lookup"><span data-stu-id="4cd1f-246">If the app uses the [in-process hosting model](xref:fundamentals/servers/index#in-process-hosting-model), run the script for *w3wp.exe*:</span></span>

     ```console
     .\EnableDumps w3wp.exe c:\dumps
     ```
   * <span data-ttu-id="4cd1f-247">Jeśli aplikacja korzysta z [modelu hostingu poza procesem](xref:fundamentals/servers/index#out-of-process-hosting-model), uruchom skrypt *dotnet.exe*:</span><span class="sxs-lookup"><span data-stu-id="4cd1f-247">If the app uses the [out-of-process hosting model](xref:fundamentals/servers/index#out-of-process-hosting-model), run the script for *dotnet.exe*:</span></span>

     ```console
     .\EnableDumps dotnet.exe c:\dumps
     ```
1. <span data-ttu-id="4cd1f-248">Uruchom aplikację zgodnie z warunkami, które powodują awarię wystąpić.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-248">Run the app under the conditions that cause the crash to occur.</span></span>
1. <span data-ttu-id="4cd1f-249">Po przeprowadzeniu awarii Uruchom [skrypt programu DisableDumps PowerShell](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/troubleshoot/scripts/DisableDumps.ps1):</span><span class="sxs-lookup"><span data-stu-id="4cd1f-249">After the crash has occurred, run the [DisableDumps PowerShell script](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/troubleshoot/scripts/DisableDumps.ps1):</span></span>
   * <span data-ttu-id="4cd1f-250">Jeśli aplikacja korzysta z [modelu hostingu w trakcie](xref:fundamentals/servers/index#in-process-hosting-model), uruchom skrypt *w3wp.exe*:</span><span class="sxs-lookup"><span data-stu-id="4cd1f-250">If the app uses the [in-process hosting model](xref:fundamentals/servers/index#in-process-hosting-model), run the script for *w3wp.exe*:</span></span>

     ```console
     .\DisableDumps w3wp.exe
     ```
   * <span data-ttu-id="4cd1f-251">Jeśli aplikacja korzysta z [modelu hostingu poza procesem](xref:fundamentals/servers/index#out-of-process-hosting-model), uruchom skrypt *dotnet.exe*:</span><span class="sxs-lookup"><span data-stu-id="4cd1f-251">If the app uses the [out-of-process hosting model](xref:fundamentals/servers/index#out-of-process-hosting-model), run the script for *dotnet.exe*:</span></span>

     ```console
     .\DisableDumps dotnet.exe
     ```

<span data-ttu-id="4cd1f-252">Po zakończeniu zbierania zrzutu awarii aplikacji oraz aplikację może zakończyć w zwykły sposób.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-252">After an app crashes and dump collection is complete, the app is allowed to terminate normally.</span></span> <span data-ttu-id="4cd1f-253">Skrypt programu PowerShell umożliwia skonfigurowanie raportowania błędów systemu Windows do zbierania zrzutów maksymalnie pięć na aplikację.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-253">The PowerShell script configures WER to collect up to five dumps per app.</span></span>

> [!WARNING]
> <span data-ttu-id="4cd1f-254">Zrzuty awaryjne może potrwać dużą ilość miejsca na dysku (maksymalnie kilka gigabajtów każdego).</span><span class="sxs-lookup"><span data-stu-id="4cd1f-254">Crash dumps might take up a large amount of disk space (up to several gigabytes each).</span></span>

### <a name="app-hangs-fails-during-startup-or-runs-normally"></a><span data-ttu-id="4cd1f-255">Aplikacja zawiesza się zakończy się niepowodzeniem podczas uruchamiania i działa normalnie</span><span class="sxs-lookup"><span data-stu-id="4cd1f-255">App hangs, fails during startup, or runs normally</span></span>

<span data-ttu-id="4cd1f-256">Gdy aplikacja *zawiesza się* (przestaje odpowiadać, ale nie awarii), zakończy się niepowodzeniem podczas uruchamiania lub działa normalnie, zobacz [plików zrzut trybu użytkownika: Wybieranie najlepszych narzędzi](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool) do wybrania odpowiedniego narzędzia do tworzenia zrzutu.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-256">When an app *hangs* (stops responding but doesn't crash), fails during startup, or runs normally, see [User-Mode Dump Files: Choosing the Best Tool](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool) to select an appropriate tool to produce the dump.</span></span>

### <a name="analyze-the-dump"></a><span data-ttu-id="4cd1f-257">Analizowanie zrzutu</span><span class="sxs-lookup"><span data-stu-id="4cd1f-257">Analyze the dump</span></span>

<span data-ttu-id="4cd1f-258">Zrzut można analizować przy użyciu kilku metod.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-258">A dump can be analyzed using several approaches.</span></span> <span data-ttu-id="4cd1f-259">Aby uzyskać więcej informacji, zobacz [analizowanie plik zrzutu trybu użytkownika](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file).</span><span class="sxs-lookup"><span data-stu-id="4cd1f-259">For more information, see [Analyzing a User-Mode Dump File](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file).</span></span>

## <a name="remote-debugging"></a><span data-ttu-id="4cd1f-260">Debugowanie zdalne</span><span class="sxs-lookup"><span data-stu-id="4cd1f-260">Remote debugging</span></span>

<span data-ttu-id="4cd1f-261">Zobacz [zdalne debugowanie platformy ASP.NET Core na komputerze zdalnym usług IIS w programie Visual Studio 2017](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer) w dokumentacji programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-261">See [Remote Debug ASP.NET Core on a Remote IIS Computer in Visual Studio 2017](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer) in the Visual Studio documentation.</span></span>

## <a name="application-insights"></a><span data-ttu-id="4cd1f-262">Application Insights</span><span class="sxs-lookup"><span data-stu-id="4cd1f-262">Application Insights</span></span>

<span data-ttu-id="4cd1f-263">[Usługa Application Insights](/azure/application-insights/) udostępnia dane telemetryczne z aplikacji hostowanych przez usługi IIS, w tym rejestrowanie i funkcji raportowania błędów.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-263">[Application Insights](/azure/application-insights/) provides telemetry from apps hosted by IIS, including error logging and reporting features.</span></span> <span data-ttu-id="4cd1f-264">Usługa Application Insights można tylko raporty dotyczące błędów występujących po uruchomieniu aplikacji, gdy staną się dostępne funkcje rejestrowania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-264">Application Insights can only report on errors that occur after the app starts when the app's logging features become available.</span></span> <span data-ttu-id="4cd1f-265">Aby uzyskać więcej informacji, zobacz [usługi Application Insights dla platformy ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).</span><span class="sxs-lookup"><span data-stu-id="4cd1f-265">For more information, see [Application Insights for ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="additional-advice"></a><span data-ttu-id="4cd1f-266">Dodatkowe porady</span><span class="sxs-lookup"><span data-stu-id="4cd1f-266">Additional advice</span></span>

<span data-ttu-id="4cd1f-267">Czasami funkcjonalności aplikacji nie powiedzie się natychmiast po uaktualnieniu albo .NET Core SDK w wersjach maszyny lub pakiet rozwoju w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-267">Sometimes a functioning app fails immediately after upgrading either the .NET Core SDK on the development machine or package versions within the app.</span></span> <span data-ttu-id="4cd1f-268">W niektórych przypadkach niespójne pakietów może spowodować uszkodzenie aplikacji podczas przeprowadzania uaktualnienia głównych.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-268">In some cases, incoherent packages may break an app when performing major upgrades.</span></span> <span data-ttu-id="4cd1f-269">Większość z tych problemów można naprawić, wykonując następujące instrukcje:</span><span class="sxs-lookup"><span data-stu-id="4cd1f-269">Most of these issues can be fixed by following these instructions:</span></span>

1. <span data-ttu-id="4cd1f-270">Usuń *bin* i *obj* folderów.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-270">Delete the *bin* and *obj* folders.</span></span>
1. <span data-ttu-id="4cd1f-271">Wyczyść pakiet zapisuje w pamięci podręcznej w *% UserProfile %\\.nuget\\pakietów* i *% LocalAppData %\\Nuget\\pamięci podręcznej v3*.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-271">Clear the package caches at *%UserProfile%\\.nuget\\packages* and *%LocalAppData%\\Nuget\\v3-cache*.</span></span>
1. <span data-ttu-id="4cd1f-272">Przywróć i skompiluj ponownie projekt.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-272">Restore and rebuild the project.</span></span>
1. <span data-ttu-id="4cd1f-273">Upewnij się, że poprzedniego wdrożenia na serwerze została całkowicie usunięta przed ponownego wdrażania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-273">Confirm that the prior deployment on the server has been completely deleted prior to redeploying the app.</span></span>

> [!TIP]
> <span data-ttu-id="4cd1f-274">Wygodnym sposobem Wyczyść pamięć podręczną pakietu ma wykonać `dotnet nuget locals all --clear` z poziomu wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-274">A convenient way to clear package caches is to execute `dotnet nuget locals all --clear` from a command prompt.</span></span>
>
> <span data-ttu-id="4cd1f-275">Wyczyszczenie pamięci podręcznej pakietu może być również wykonywane przy użyciu [nuget.exe](https://www.nuget.org/downloads) narzędzie i wykonywania polecenia `nuget locals all -clear`.</span><span class="sxs-lookup"><span data-stu-id="4cd1f-275">Clearing package caches can also be accomplished by using the [nuget.exe](https://www.nuget.org/downloads) tool and executing the command `nuget locals all -clear`.</span></span> <span data-ttu-id="4cd1f-276">*nuget.exe* nie jest powiązane instalacji z pulpitu systemu operacyjnego Windows i należy uzyskać oddzielnie od [NuGet witryny sieci Web](https://www.nuget.org/downloads).</span><span class="sxs-lookup"><span data-stu-id="4cd1f-276">*nuget.exe* isn't a bundled install with the Windows desktop operating system and must be obtained separately from the [NuGet website](https://www.nuget.org/downloads).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4cd1f-277">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="4cd1f-277">Additional resources</span></span>

* <xref:test/troubleshoot>
* <xref:fundamentals/error-handling>
* <xref:host-and-deploy/azure-iis-errors-reference>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/azure-apps/troubleshoot>
