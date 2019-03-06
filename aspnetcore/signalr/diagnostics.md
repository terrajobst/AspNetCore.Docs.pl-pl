---
title: Rejestrowania i diagnostyki w biblioteki SignalR platformy ASP.NET Core
author: anurse
description: Dowiedz się, jak zbieranie diagnostyki aplikacji biblioteki SignalR platformy ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: signalr
ms.date: 02/27/2019
uid: signalr/diagnostics
ms.openlocfilehash: b6bd21314ed183488999bcff3553e53493537a11
ms.sourcegitcommit: 6ddd8a7675c1c1d997c8ab2d4498538e44954cac
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/05/2019
ms.locfileid: "57400992"
---
# <a name="logging-and-diagnostics-in-aspnet-core-signalr"></a><span data-ttu-id="c1ad7-103">Rejestrowania i diagnostyki w biblioteki SignalR platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c1ad7-103">Logging and diagnostics in ASP.NET Core SignalR</span></span>

<span data-ttu-id="c1ad7-104">Przez [Andrew Stanton pielęgniarki](https://twitter.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="c1ad7-104">By [Andrew Stanton-Nurse](https://twitter.com/anurse)</span></span>

<span data-ttu-id="c1ad7-105">Ten artykuł zawiera wskazówki dotyczące zbieranie diagnostyki z Twojej aplikacji biblioteki SignalR platformy ASP.NET Core, aby ułatwić rozwiązywanie problemów.</span><span class="sxs-lookup"><span data-stu-id="c1ad7-105">This article provides guidance for gathering diagnostics from your ASP.NET Core SignalR app to help troubleshoot issues.</span></span>

## <a name="server-side-logging"></a><span data-ttu-id="c1ad7-106">Rejestrowania po stronie serwera</span><span class="sxs-lookup"><span data-stu-id="c1ad7-106">Server-side logging</span></span>

> [!WARNING]
> <span data-ttu-id="c1ad7-107">Dzienniki po stronie serwera może zawierać poufne informacje z Twojej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c1ad7-107">Server-side logs may contain sensitive information from your app.</span></span> <span data-ttu-id="c1ad7-108">**Nigdy nie** Publikuj nieprzetworzonych dzienników z aplikacji produkcyjnych na forach publicznych, takich jak GitHub.</span><span class="sxs-lookup"><span data-stu-id="c1ad7-108">**Never** post raw logs from production apps to public forums like GitHub.</span></span>

<span data-ttu-id="c1ad7-109">Ponieważ SignalR jest częścią platformy ASP.NET Core, używa platformy ASP.NET Core rejestrowania systemu.</span><span class="sxs-lookup"><span data-stu-id="c1ad7-109">Since SignalR is part of ASP.NET Core, it uses the ASP.NET Core logging system.</span></span> <span data-ttu-id="c1ad7-110">W domyślnej konfiguracji SignalR rejestruje informacje w niewielkim stopniu, ale może to skonfigurować.</span><span class="sxs-lookup"><span data-stu-id="c1ad7-110">In the default configuration, SignalR logs very little information, but this can configured.</span></span> <span data-ttu-id="c1ad7-111">Znajdują się w dokumentacji na [platformy ASP.NET Core rejestrowania](xref:fundamentals/logging/index#configuration) Aby uzyskać szczegółowe informacje na temat konfigurowania programu ASP.NET Core rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="c1ad7-111">See the documentation on [ASP.NET Core logging](xref:fundamentals/logging/index#configuration) for details on configuring ASP.NET Core logging.</span></span>

<span data-ttu-id="c1ad7-112">SignalR używa dwóch kategorii rejestratora:</span><span class="sxs-lookup"><span data-stu-id="c1ad7-112">SignalR uses two logger categories:</span></span>

* <span data-ttu-id="c1ad7-113">`Microsoft.AspNetCore.SignalR` — w przypadku dzienników związane z Centrum protokołów aktywowanie koncentratorów, wywoływanie metod i inne działania związane z Centrum.</span><span class="sxs-lookup"><span data-stu-id="c1ad7-113">`Microsoft.AspNetCore.SignalR` - for logs related to Hub Protocols, activating Hubs, invoking methods, and other Hub-related activities.</span></span>
* <span data-ttu-id="c1ad7-114">`Microsoft.AspNetCore.Http.Connections` — w przypadku dzienników powiązane z transportami, takich jak funkcja WebSockets, długie sondowania i Server-Sent zdarzeń i niskiego poziomu infrastrukturą SignalR.</span><span class="sxs-lookup"><span data-stu-id="c1ad7-114">`Microsoft.AspNetCore.Http.Connections` - for logs related to transports such as WebSockets, Long Polling and Server-Sent Events and low-level SignalR infrastructure.</span></span>

<span data-ttu-id="c1ad7-115">Aby włączyć dzienniki z SignalR, skonfiguruj zarówno poprzedniego prefiksów `Debug` poziom w swojej `appsettings.json` pliku, dodając następujące elementy, aby `LogLevel` podsekcji w `Logging`:</span><span class="sxs-lookup"><span data-stu-id="c1ad7-115">To enable detailed logs from SignalR, configure both of the preceding prefixes to the `Debug` level in your `appsettings.json` file by adding the following items to the `LogLevel` sub-section in `Logging`:</span></span>

[!code-json[Configuring logging](diagnostics/logging-config.json?highlight=7-8)]

<span data-ttu-id="c1ad7-116">Można również skonfigurować ten w kodzie w swojej `CreateWebHostBuilder` metody:</span><span class="sxs-lookup"><span data-stu-id="c1ad7-116">You can also configure this in code in your `CreateWebHostBuilder` method:</span></span>

[!code-csharp[Configuring logging in code](diagnostics/logging-config-code.cs?highlight=5-6)]

<span data-ttu-id="c1ad7-117">Jeśli nie używasz konfiguracji opartej na formacie JSON, ustaw następujące wartości konfiguracji w Twoim systemie konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="c1ad7-117">If you aren't using JSON-based configuration, set the following configuration values in your configuration system:</span></span>

* `Logging:LogLevel:Microsoft.AspNetCore.SignalR` = `Debug`
* `Logging:LogLevel:Microsoft.AspNetCore.Http.Connections` = `Debug`

<span data-ttu-id="c1ad7-118">Sprawdź w dokumentacji systemu konfiguracji określić, jak określić wartości konfiguracji zagnieżdżonych.</span><span class="sxs-lookup"><span data-stu-id="c1ad7-118">Check the documentation for your configuration system to determine how to specify nested configuration values.</span></span> <span data-ttu-id="c1ad7-119">Na przykład w przypadku używania zmiennych środowiskowych dwóch `_` znaki są używane zamiast `:` (takich jak: `Logging__LogLevel__Microsoft.AspNetCore.SignalR`).</span><span class="sxs-lookup"><span data-stu-id="c1ad7-119">For example, when using environment variables, two `_` characters are used instead of the `:` (such as: `Logging__LogLevel__Microsoft.AspNetCore.SignalR`).</span></span>

<span data-ttu-id="c1ad7-120">Firma Microsoft zaleca używanie `Debug` poziomu podczas zbierania bardziej szczegółowych diagnostyki dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c1ad7-120">We recommend using the `Debug` level when gathering more detailed diagnostics for your app.</span></span> <span data-ttu-id="c1ad7-121">`Trace` Poziomu powoduje bardzo diagnostyki i rzadko jest potrzebne do diagnozowania problemów w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c1ad7-121">The `Trace` level produces very low-level diagnostics and is rarely needed to diagnose issues in your app.</span></span>

## <a name="access-server-side-logs"></a><span data-ttu-id="c1ad7-122">Uzyskiwanie dostępu do dzienników po stronie serwera</span><span class="sxs-lookup"><span data-stu-id="c1ad7-122">Access server-side logs</span></span>

<span data-ttu-id="c1ad7-123">Sposób uzyskiwania dostępu do dzienników po stronie serwera, zależy od środowiska, w którym pracujesz.</span><span class="sxs-lookup"><span data-stu-id="c1ad7-123">How you access server-side logs depends on the environment in which you're running.</span></span>

### <a name="as-a-console-app-outside-iis"></a><span data-ttu-id="c1ad7-124">Jako aplikację konsoli poza usług IIS</span><span class="sxs-lookup"><span data-stu-id="c1ad7-124">As a console app outside IIS</span></span>

<span data-ttu-id="c1ad7-125">Jeśli pracujesz w aplikacji konsolowej [rejestratora konsoli](xref:fundamentals/logging/index#console-provider) powinno być włączone domyślnie.</span><span class="sxs-lookup"><span data-stu-id="c1ad7-125">If you're running in a console app, the [Console logger](xref:fundamentals/logging/index#console-provider) should be enabled by default.</span></span> <span data-ttu-id="c1ad7-126">SignalR dzienników będą wyświetlane w konsoli.</span><span class="sxs-lookup"><span data-stu-id="c1ad7-126">SignalR logs will appear in the console.</span></span>

### <a name="within-iis-express-from-visual-studio"></a><span data-ttu-id="c1ad7-127">W ramach usług IIS Express z programu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c1ad7-127">Within IIS Express from Visual Studio</span></span>

<span data-ttu-id="c1ad7-128">Visual Studio wyświetla dane wyjściowe dziennika w **dane wyjściowe** okna.</span><span class="sxs-lookup"><span data-stu-id="c1ad7-128">Visual Studio displays the log output in the **Output** window.</span></span> <span data-ttu-id="c1ad7-129">Wybierz **serwera sieci Web programu ASP.NET Core** listy rozwijanej opcję.</span><span class="sxs-lookup"><span data-stu-id="c1ad7-129">Select the **ASP.NET Core Web Server** drop down option.</span></span>

### <a name="azure-app-service"></a><span data-ttu-id="c1ad7-130">Usługa Azure App Service</span><span class="sxs-lookup"><span data-stu-id="c1ad7-130">Azure App Service</span></span>

<span data-ttu-id="c1ad7-131">Włącz opcję "Rejestrowanie aplikacji (system plików)" w sekcji "Dzienniki diagnostyczne" w portalu usługi Azure App Service i skonfigurować poziom `Verbose`.</span><span class="sxs-lookup"><span data-stu-id="c1ad7-131">Enable the "Application Logging (Filesystem)" option in the "Diagnostics logs" section of the Azure App Service portal and configure the Level to `Verbose`.</span></span> <span data-ttu-id="c1ad7-132">Dzienniki powinny być dostępne usługi "Przesyłanie strumieniowe dzienników", a także, jak dzienniki usługi App Service w systemie plików.</span><span class="sxs-lookup"><span data-stu-id="c1ad7-132">Logs should be available from the "Log streaming" service, as well as in logs on the file system of your App Service.</span></span> <span data-ttu-id="c1ad7-133">Aby uzyskać więcej informacji, zobacz dokumentację na [przesyłanie strumieniowe dzienników platformy Azure](xref:fundamentals/logging/index#azure-log-streaming).</span><span class="sxs-lookup"><span data-stu-id="c1ad7-133">For more information, see the documentation on [Azure log streaming](xref:fundamentals/logging/index#azure-log-streaming).</span></span>

### <a name="other-environments"></a><span data-ttu-id="c1ad7-134">Innych środowisk</span><span class="sxs-lookup"><span data-stu-id="c1ad7-134">Other environments</span></span>

<span data-ttu-id="c1ad7-135">Jeśli używasz w innym środowisku (Docker, Kubernetes, usługa Windows itp.), zobacz pełną dokumentację na [rejestrowania programu ASP.NET Core](xref:fundamentals/logging/index) Aby uzyskać więcej informacji na temat konfigurowania rejestrowania dostawców odpowiednie dla danego środowiska.</span><span class="sxs-lookup"><span data-stu-id="c1ad7-135">If you're running in another environment (Docker, Kubernetes, Windows Service, etc.), see the full documentation on [ASP.NET Core Logging](xref:fundamentals/logging/index) for more information on how to configure logging providers suitable to your environment.</span></span>

## <a name="javascript-client-logging"></a><span data-ttu-id="c1ad7-136">Rejestrowanie klienta JavaScript</span><span class="sxs-lookup"><span data-stu-id="c1ad7-136">JavaScript client logging</span></span>

> [!WARNING]
> <span data-ttu-id="c1ad7-137">Dzienniki po stronie klienta mogą zawierać poufne informacje z Twojej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c1ad7-137">Client-side logs may contain sensitive information from your app.</span></span> <span data-ttu-id="c1ad7-138">**Nigdy nie** Publikuj nieprzetworzonych dzienników z aplikacji produkcyjnych na forach publicznych, takich jak GitHub.</span><span class="sxs-lookup"><span data-stu-id="c1ad7-138">**Never** post raw logs from production apps to public forums like GitHub.</span></span>

<span data-ttu-id="c1ad7-139">Podczas korzystania z klienta JavaScript, można skonfigurować opcje rejestrowania przy użyciu `configureLogging` metody `HubConnectionBuilder`:</span><span class="sxs-lookup"><span data-stu-id="c1ad7-139">When using the JavaScript client, you can configure logging options using the `configureLogging` method on `HubConnectionBuilder`:</span></span>

[!code-javascript[Configuring logging in the JavaScript client](diagnostics/logging-config-js.js?highlight=3)]

<span data-ttu-id="c1ad7-140">Aby wyłączyć rejestrowanie w całości, należy określić `signalR.LogLevel.None` w `configureLogging` metody.</span><span class="sxs-lookup"><span data-stu-id="c1ad7-140">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="c1ad7-141">W poniższej tabeli przedstawiono dostępne poziomy dziennika dla klienta język JavaScript.</span><span class="sxs-lookup"><span data-stu-id="c1ad7-141">The following table shows log levels available to the JavaScript client.</span></span> <span data-ttu-id="c1ad7-142">Ustawienie poziomu dziennika na jedną z następujących wartości umożliwia rejestrowanie na danym poziomie i wszystkie poziomy nad nim w tabeli.</span><span class="sxs-lookup"><span data-stu-id="c1ad7-142">Setting the log level to one of these values enables logging at that level and all levels above it in the table.</span></span>

| <span data-ttu-id="c1ad7-143">Poziom</span><span class="sxs-lookup"><span data-stu-id="c1ad7-143">Level</span></span> | <span data-ttu-id="c1ad7-144">Opis</span><span class="sxs-lookup"><span data-stu-id="c1ad7-144">Description</span></span> |
| ----- | ----------- |
| `None` | <span data-ttu-id="c1ad7-145">Żadne komunikaty są rejestrowane.</span><span class="sxs-lookup"><span data-stu-id="c1ad7-145">No messages are logged.</span></span> |
| `Critical` | <span data-ttu-id="c1ad7-146">Komunikaty, które wskazują błędy w całej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c1ad7-146">Messages that indicate a failure in the entire app.</span></span> |
| `Error` | <span data-ttu-id="c1ad7-147">Komunikaty, które wskazują błędy w bieżącej operacji.</span><span class="sxs-lookup"><span data-stu-id="c1ad7-147">Messages that indicate a failure in the current operation.</span></span> |
| `Warning` | <span data-ttu-id="c1ad7-148">Komunikaty, które wskazują na problem krytyczny.</span><span class="sxs-lookup"><span data-stu-id="c1ad7-148">Messages that indicate a non-fatal problem.</span></span> |
| `Information` | <span data-ttu-id="c1ad7-149">Komunikaty informacyjne.</span><span class="sxs-lookup"><span data-stu-id="c1ad7-149">Informational messages.</span></span> |
| `Debug` | <span data-ttu-id="c1ad7-150">Komunikaty diagnostyczne przydatne podczas debugowania.</span><span class="sxs-lookup"><span data-stu-id="c1ad7-150">Diagnostic messages useful for debugging.</span></span> |
| `Trace` | <span data-ttu-id="c1ad7-151">Bardzo szczegółowe komunikaty diagnostyczne przeznaczone dla diagnozowanie konkretnych problemów.</span><span class="sxs-lookup"><span data-stu-id="c1ad7-151">Very detailed diagnostic messages designed for diagnosing specific issues.</span></span> |

<span data-ttu-id="c1ad7-152">Po skonfigurowaniu poziom szczegółowości, dzienniki będą zapisywane w konsoli przeglądarki (lub standardowe dane wyjściowe w aplikacji node.js).</span><span class="sxs-lookup"><span data-stu-id="c1ad7-152">Once you've configured the verbosity, the logs will be written to the Browser Console (or Standard Output in a NodeJS app).</span></span>

<span data-ttu-id="c1ad7-153">Jeśli chcesz wysłać dzienniki do systemu rejestrowania niestandardowego, możesz podać implementacja obiektu JavaScript `ILogger` interfejsu.</span><span class="sxs-lookup"><span data-stu-id="c1ad7-153">If you want to send logs to a custom logging system, you can provide a JavaScript object implementing the `ILogger` interface.</span></span> <span data-ttu-id="c1ad7-154">Jest jedyną metodą, która musi zostać wdrożone `log`, który przyjmuje poziom zdarzenia i wiadomości skojarzonej ze zdarzeniem.</span><span class="sxs-lookup"><span data-stu-id="c1ad7-154">The only method that needs to be implemented is `log`, which takes the level of the event and the message associated with the event.</span></span> <span data-ttu-id="c1ad7-155">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="c1ad7-155">For example:</span></span>

[!code-typescript[Creating a custom logger](diagnostics/custom-logger.ts?highlight=3-7,13)]

## <a name="net-client-logging"></a><span data-ttu-id="c1ad7-156">Rejestrowanie klienta platformy .NET</span><span class="sxs-lookup"><span data-stu-id="c1ad7-156">.NET client logging</span></span>

> [!WARNING]
> <span data-ttu-id="c1ad7-157">Dzienniki po stronie klienta mogą zawierać poufne informacje z Twojej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c1ad7-157">Client-side logs may contain sensitive information from your app.</span></span> <span data-ttu-id="c1ad7-158">**Nigdy nie** Publikuj nieprzetworzonych dzienników z aplikacji produkcyjnych na forach publicznych, takich jak GitHub.</span><span class="sxs-lookup"><span data-stu-id="c1ad7-158">**Never** post raw logs from production apps to public forums like GitHub.</span></span>

<span data-ttu-id="c1ad7-159">Aby uzyskać dzienniki z klienta .NET, możesz użyć `ConfigureLogging` metody `HubConnectionBuilder`.</span><span class="sxs-lookup"><span data-stu-id="c1ad7-159">To get logs from the .NET client, you can use the `ConfigureLogging` method on `HubConnectionBuilder`.</span></span> <span data-ttu-id="c1ad7-160">To działa tak samo jak `ConfigureLogging` metody `WebHostBuilder` i `HostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="c1ad7-160">This works the same way as the `ConfigureLogging` method on `WebHostBuilder` and `HostBuilder`.</span></span> <span data-ttu-id="c1ad7-161">Można skonfigurować tego samego dostawcy rejestrowania używanej w programie ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c1ad7-161">You can configure the same logging providers you use in ASP.NET Core.</span></span> <span data-ttu-id="c1ad7-162">Jednak należy ręcznie zainstalować i włączyć pakietów NuGet dla indywidualnych rejestrowania dostawców.</span><span class="sxs-lookup"><span data-stu-id="c1ad7-162">However, you have to manually install and enable the NuGet packages for the individual logging providers.</span></span>

### <a name="console-logging"></a><span data-ttu-id="c1ad7-163">Rejestrowanie konsoli</span><span class="sxs-lookup"><span data-stu-id="c1ad7-163">Console logging</span></span>

<span data-ttu-id="c1ad7-164">Aby włączyć rejestrowanie konsoli, Dodaj [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) pakietu.</span><span class="sxs-lookup"><span data-stu-id="c1ad7-164">In order to enable Console logging, add the [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) package.</span></span> <span data-ttu-id="c1ad7-165">Następnie należy użyć `AddConsole` Metoda konfiguracji rejestratora konsoli:</span><span class="sxs-lookup"><span data-stu-id="c1ad7-165">Then, use the `AddConsole` method to configure the console logger:</span></span>

[!code-csharp[Configuring console logging in .NET client](diagnostics/net-client-console-log.cs?highlight=6)]

### <a name="debug-output-window-logging"></a><span data-ttu-id="c1ad7-166">Rejestrowanie okna dane wyjściowe debugowania</span><span class="sxs-lookup"><span data-stu-id="c1ad7-166">Debug output window logging</span></span>

<span data-ttu-id="c1ad7-167">Można również skonfigurować dzienniki, aby przejść do **dane wyjściowe** okna w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c1ad7-167">You can also configure logs to go to the **Output** window in Visual Studio.</span></span> <span data-ttu-id="c1ad7-168">Zainstaluj [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) pakietu i użyj `AddDebug` metody:</span><span class="sxs-lookup"><span data-stu-id="c1ad7-168">Install the [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) package and use the `AddDebug` method:</span></span>

[!code-csharp[Configuring debug output window logging in .NET client](diagnostics/net-client-debug-log.cs?highlight=6)]

### <a name="other-logging-providers"></a><span data-ttu-id="c1ad7-169">Innych dostawców logowania</span><span class="sxs-lookup"><span data-stu-id="c1ad7-169">Other logging providers</span></span>

<span data-ttu-id="c1ad7-170">SignalR obsługuje innych dostawców logowania, takie jak Serilog, Seq, NLog lub innego systemu rejestrowania, która integruje się z `Microsoft.Extensions.Logging`.</span><span class="sxs-lookup"><span data-stu-id="c1ad7-170">SignalR supports other logging providers such as Serilog, Seq, NLog, or any other logging system that integrates with `Microsoft.Extensions.Logging`.</span></span> <span data-ttu-id="c1ad7-171">Jeśli system rejestrowania udostępnia `ILoggerProvider`, można zarejestrować go za pomocą `AddProvider`:</span><span class="sxs-lookup"><span data-stu-id="c1ad7-171">If your logging system provides an `ILoggerProvider`, you can register it with `AddProvider`:</span></span>

[!code-csharp[Configuring a custom logging provider in .NET client](diagnostics/net-client-custom-log.cs?highlight=6)]

### <a name="control-verbosity"></a><span data-ttu-id="c1ad7-172">Szczegółowość kontroli</span><span class="sxs-lookup"><span data-stu-id="c1ad7-172">Control verbosity</span></span>

<span data-ttu-id="c1ad7-173">Jeśli logujesz się w innych miejscach w aplikacji, zmiana domyślnego poziomu do `Debug` może być zbyt pełne.</span><span class="sxs-lookup"><span data-stu-id="c1ad7-173">If you are logging from other places in your app, changing the default level to `Debug` may be too verbose.</span></span> <span data-ttu-id="c1ad7-174">Filtr umożliwia konfigurowanie poziomu rejestrowania dzienników SignalR.</span><span class="sxs-lookup"><span data-stu-id="c1ad7-174">You can use a Filter to configure the logging level for SignalR logs.</span></span> <span data-ttu-id="c1ad7-175">Można to zrobić w kodzie, w taki sam sposób, jak na serwerze:</span><span class="sxs-lookup"><span data-stu-id="c1ad7-175">This can be done in code, in much the same way as on the server:</span></span>

[!code-csharp[Controlling verbosity in .NET client](diagnostics/logging-config-client-code.cs?highlight=9-10)]

## <a name="network-traces"></a><span data-ttu-id="c1ad7-176">Danych śledzenia sieci</span><span class="sxs-lookup"><span data-stu-id="c1ad7-176">Network traces</span></span>

> [!WARNING]
> <span data-ttu-id="c1ad7-177">Śledzenie sieci zawiera całą zawartość każda wiadomość wysłana przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="c1ad7-177">A network trace contains the full contents of every message sent by your app.</span></span> <span data-ttu-id="c1ad7-178">**Nigdy nie** publikowanie danych śledzenia sieci pierwotnych z aplikacji produkcyjnych na forach publicznych, takich jak GitHub.</span><span class="sxs-lookup"><span data-stu-id="c1ad7-178">**Never** post raw network traces from production apps to public forums like GitHub.</span></span>

<span data-ttu-id="c1ad7-179">Jeśli wystąpi problem, śledzenia sieci czasami może zapewnić mnóstwo przydatnych informacji.</span><span class="sxs-lookup"><span data-stu-id="c1ad7-179">If you encounter an issue, a network trace can sometimes provide a lot of helpful information.</span></span> <span data-ttu-id="c1ad7-180">Jest to szczególnie przydatne, jeśli zamierzasz Prześlij zgłoszenie na nasze narzędzia do śledzenia błędów.</span><span class="sxs-lookup"><span data-stu-id="c1ad7-180">This is particularly useful if you're going to file an issue on our issue tracker.</span></span>

## <a name="collect-a-network-trace-with-fiddler-preferred-option"></a><span data-ttu-id="c1ad7-181">Zbierać dane śledzenia sieci przy użyciu programu Fiddler (preferowaną opcję)</span><span class="sxs-lookup"><span data-stu-id="c1ad7-181">Collect a network trace with Fiddler (preferred option)</span></span>

<span data-ttu-id="c1ad7-182">Ta metoda działa w przypadku wszystkich aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c1ad7-182">This method works for all apps.</span></span>

<span data-ttu-id="c1ad7-183">Jest bardzo zaawansowanego narzędzia do zbierania danych śledzenia protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="c1ad7-183">Fiddler is a very powerful tool for collecting HTTP traces.</span></span> <span data-ttu-id="c1ad7-184">Zainstaluj go z [telerik.com/fiddler](https://www.telerik.com/fiddler), uruchom ją, a następnie uruchom aplikację i odtworzyć problem.</span><span class="sxs-lookup"><span data-stu-id="c1ad7-184">Install it from [telerik.com/fiddler](https://www.telerik.com/fiddler), launch it, and then run your app and reproduce the issue.</span></span> <span data-ttu-id="c1ad7-185">Jest dostępna dla Windows, a istnieją wersje beta dla systemu macOS i Linux.</span><span class="sxs-lookup"><span data-stu-id="c1ad7-185">Fiddler is available for Windows, and there are beta versions for macOS and Linux.</span></span>

<span data-ttu-id="c1ad7-186">Jeśli łączysz się przy użyciu protokołu HTTPS, istnieją pewne dodatkowe kroki, aby upewnić się, że program Fiddler można odszyfrowywanie ruchu protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="c1ad7-186">If you connect using HTTPS, there are some extra steps to ensure Fiddler can decrypt the HTTPS traffic.</span></span> <span data-ttu-id="c1ad7-187">Aby uzyskać więcej informacji, zobacz [dokumentacji programu Fiddler](https://docs.telerik.com/fiddler/Configure-Fiddler/Tasks/DecryptHTTPS).</span><span class="sxs-lookup"><span data-stu-id="c1ad7-187">For more details, see the [Fiddler documentation](https://docs.telerik.com/fiddler/Configure-Fiddler/Tasks/DecryptHTTPS).</span></span>

<span data-ttu-id="c1ad7-188">Zostały zebrane śledzenia można eksportować śledzenia, wybierając **pliku** > **Zapisz** > **wszystkie sesje...**  z paska menu.</span><span class="sxs-lookup"><span data-stu-id="c1ad7-188">Once you've collected the trace, you can export the trace by choosing **File** > **Save** > **All Sessions...** from the menu bar.</span></span>

![Eksportowanie wszystkich sesji z programu Fiddler](diagnostics/fiddler-export.png)

## <a name="collect-a-network-trace-with-tcpdump-macos-and-linux-only"></a><span data-ttu-id="c1ad7-190">Zbierać dane śledzenia sieci za pomocą tcpdump (z systemem macOS i Linux tylko)</span><span class="sxs-lookup"><span data-stu-id="c1ad7-190">Collect a network trace with tcpdump (macOS and Linux only)</span></span>

<span data-ttu-id="c1ad7-191">Ta metoda działa w przypadku wszystkich aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c1ad7-191">This method works for all apps.</span></span>

<span data-ttu-id="c1ad7-192">Możesz zbierać pierwotne TCP danych śledzenia przy użyciu tcpdump, uruchamiając następujące polecenie z powłoki poleceń.</span><span class="sxs-lookup"><span data-stu-id="c1ad7-192">You can collect raw TCP traces using tcpdump by running the following command from a command shell.</span></span> <span data-ttu-id="c1ad7-193">Konieczne może być `root` lub prefiks polecenie `sudo` Jeśli zostanie wyświetlony błąd uprawnień:</span><span class="sxs-lookup"><span data-stu-id="c1ad7-193">You may need to be `root` or prefix the command with `sudo` if you get a permissions error:</span></span>

```console
tcpdump -i [interface] -w trace.pcap
```

<span data-ttu-id="c1ad7-194">Zastąp `[interface]` z interfejsem sieciowym, które mają być przechwytywane.</span><span class="sxs-lookup"><span data-stu-id="c1ad7-194">Replace `[interface]` with the network interface you wish to capture on.</span></span> <span data-ttu-id="c1ad7-195">Zazwyczaj jest podobny do `/dev/eth0` (w przypadku standardowego interfejsu Ethernet) lub `/dev/lo0` (dla ruchu hosta lokalnego).</span><span class="sxs-lookup"><span data-stu-id="c1ad7-195">Usually, this is something like `/dev/eth0` (for your standard Ethernet interface) or `/dev/lo0` (for localhost traffic).</span></span> <span data-ttu-id="c1ad7-196">Aby uzyskać więcej informacji, zobacz `tcpdump` strony ataków typu man w systemie hosta.</span><span class="sxs-lookup"><span data-stu-id="c1ad7-196">For more information, see the `tcpdump` man page on your host system.</span></span>

## <a name="collect-a-network-trace-in-the-browser"></a><span data-ttu-id="c1ad7-197">Zbierać dane śledzenia sieci w przeglądarce</span><span class="sxs-lookup"><span data-stu-id="c1ad7-197">Collect a network trace in the browser</span></span>

<span data-ttu-id="c1ad7-198">Ta metoda działa tylko dla aplikacji opartych na przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="c1ad7-198">This method only works for browser-based apps.</span></span>

<span data-ttu-id="c1ad7-199">Większość narzędzi dla deweloperów przeglądarki ma kartę "Sieć", która pozwala na przechwytywanie aktywności sieciowej między przeglądarką a serwerem.</span><span class="sxs-lookup"><span data-stu-id="c1ad7-199">Most browser Developer Tools have a "Network" tab that allows you to capture network activity between the browser and the server.</span></span> <span data-ttu-id="c1ad7-200">Ślady te nie mają jednak komunikaty protokołu WebSocket i Server-Sent zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="c1ad7-200">However, these traces don't include WebSocket and Server-Sent Event messages.</span></span> <span data-ttu-id="c1ad7-201">Jeśli używasz tych transportu przy użyciu narzędzia, takiego jak Fiddler lub TcpDump (opisanych poniżej) jest lepszym rozwiązaniem.</span><span class="sxs-lookup"><span data-stu-id="c1ad7-201">If you are using those transports, using a tool like Fiddler or TcpDump (described below) is a better approach.</span></span>

### <a name="microsoft-edge-and-internet-explorer"></a><span data-ttu-id="c1ad7-202">Przeglądarka Microsoft Edge i przeglądarki Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="c1ad7-202">Microsoft Edge and Internet Explorer</span></span>

<span data-ttu-id="c1ad7-203">(Instrukcje są takie same dla przeglądarki Edge i Internet Explorer)</span><span class="sxs-lookup"><span data-stu-id="c1ad7-203">(The instructions are the same for both Edge and Internet Explorer)</span></span>

1. <span data-ttu-id="c1ad7-204">Naciśnij klawisz F12, aby otworzyć narzędzia programistyczne</span><span class="sxs-lookup"><span data-stu-id="c1ad7-204">Press F12 to open the Dev Tools</span></span>
2. <span data-ttu-id="c1ad7-205">Kliknij kartę Sieć</span><span class="sxs-lookup"><span data-stu-id="c1ad7-205">Click the Network Tab</span></span>
3. <span data-ttu-id="c1ad7-206">(Jeśli jest to konieczne), Odśwież stronę i odtworzenia problemu</span><span class="sxs-lookup"><span data-stu-id="c1ad7-206">Refresh the page (if needed) and reproduce the problem</span></span>
4. <span data-ttu-id="c1ad7-207">Kliknij ikonę Zapisz na pasku narzędzi, aby wyeksportować śledzenia w formacie "HAR":</span><span class="sxs-lookup"><span data-stu-id="c1ad7-207">Click the Save icon in the toolbar to export the trace as a "HAR" file:</span></span>

![Zapisz ikonę na deweloperów przeglądarki Microsoft Edge narzędzi karty sieciowej](diagnostics/ie-edge-har-export.png)

### <a name="google-chrome"></a><span data-ttu-id="c1ad7-209">Google Chrome</span><span class="sxs-lookup"><span data-stu-id="c1ad7-209">Google Chrome</span></span>

1. <span data-ttu-id="c1ad7-210">Naciśnij klawisz F12, aby otworzyć narzędzia programistyczne</span><span class="sxs-lookup"><span data-stu-id="c1ad7-210">Press F12 to open the Dev Tools</span></span>
2. <span data-ttu-id="c1ad7-211">Kliknij kartę Sieć</span><span class="sxs-lookup"><span data-stu-id="c1ad7-211">Click the Network Tab</span></span>
3. <span data-ttu-id="c1ad7-212">(Jeśli jest to konieczne), Odśwież stronę i odtworzenia problemu</span><span class="sxs-lookup"><span data-stu-id="c1ad7-212">Refresh the page (if needed) and reproduce the problem</span></span>
4. <span data-ttu-id="c1ad7-213">Kliknij prawym przyciskiem myszy kliknij w dowolnym miejscu na liście żądań i wybierz polecenie "Zapisz jako plik HAR z zawartością":</span><span class="sxs-lookup"><span data-stu-id="c1ad7-213">Right click anywhere in the list of requests and choose "Save as HAR with content":</span></span>

![Opcja "Zapisz jako plik HAR z zawartością" Google Chrome Dev Tools sieci karcie](diagnostics/chrome-har-export.png)

### <a name="mozilla-firefox"></a><span data-ttu-id="c1ad7-215">Mozilla Firefox</span><span class="sxs-lookup"><span data-stu-id="c1ad7-215">Mozilla Firefox</span></span>

1. <span data-ttu-id="c1ad7-216">Naciśnij klawisz F12, aby otworzyć narzędzia programistyczne</span><span class="sxs-lookup"><span data-stu-id="c1ad7-216">Press F12 to open the Dev Tools</span></span>
2. <span data-ttu-id="c1ad7-217">Kliknij kartę Sieć</span><span class="sxs-lookup"><span data-stu-id="c1ad7-217">Click the Network Tab</span></span>
3. <span data-ttu-id="c1ad7-218">(Jeśli jest to konieczne), Odśwież stronę i odtworzenia problemu</span><span class="sxs-lookup"><span data-stu-id="c1ad7-218">Refresh the page (if needed) and reproduce the problem</span></span>
4. <span data-ttu-id="c1ad7-219">Kliknij prawym przyciskiem myszy kliknij w dowolnym miejscu na liście żądań i wybierz polecenie "Zapisz wszystkie jako plik HAR"</span><span class="sxs-lookup"><span data-stu-id="c1ad7-219">Right click anywhere in the list of requests and choose "Save All As HAR"</span></span>

![Opcja "Zapisz wszystko jako HAR" na karcie Narzędzia deweloperskie Mozilla Firefox w sieci](diagnostics/firefox-har-export.png)

## <a name="attach-diagnostics-files-to-github-issues"></a><span data-ttu-id="c1ad7-221">Dołącz pliki diagnostyki na problemy usługi GitHub</span><span class="sxs-lookup"><span data-stu-id="c1ad7-221">Attach diagnostics files to GitHub issues</span></span>

<span data-ttu-id="c1ad7-222">Możesz dołączyć pliki diagnostyki na problemy usługi GitHub, zmieniając ich nazwy, dzięki czemu mają one `.txt` rozszerzenie, a następnie przeciągając i upuszczając je do problemu.</span><span class="sxs-lookup"><span data-stu-id="c1ad7-222">You can attach Diagnostics files to GitHub issues by renaming them so they have a `.txt` extension and then dragging and dropping them on to the issue.</span></span>

> [!NOTE]
> <span data-ttu-id="c1ad7-223">Problem w usłudze GitHub, nie Wklej zawartość plików dziennika i danych śledzenia sieci.</span><span class="sxs-lookup"><span data-stu-id="c1ad7-223">Please don't paste the content of log files or network traces in GitHub issue.</span></span> <span data-ttu-id="c1ad7-224">Te dzienniki i dane śledzenia może być dość duży i GitHub zwykle obetnie je.</span><span class="sxs-lookup"><span data-stu-id="c1ad7-224">These logs and traces can be quite large and GitHub will usually truncate them.</span></span>

![Przeciąganie plików dziennika na problem w usłudze GitHub](diagnostics/attaching-diagnostics-files.png)

## <a name="additional-resources"></a><span data-ttu-id="c1ad7-226">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="c1ad7-226">Additional resources</span></span>

* <xref:signalr/configuration>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
