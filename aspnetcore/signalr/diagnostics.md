---
title: Rejestrowanie i Diagnostyka w ASP.NET Core SignalR
author: anurse
description: Dowiedz się, jak zbierać diagnostykę z aplikacji SignalR ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: signalr
ms.date: 11/12/2019
no-loc:
- SignalR
uid: signalr/diagnostics
ms.openlocfilehash: c5bd2ac27f8ca486b0d75aed8439747f72448625
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78660973"
---
# <a name="logging-and-diagnostics-in-aspnet-core-signalr"></a><span data-ttu-id="41012-103">Rejestrowanie i Diagnostyka w ASP.NET Core sygnalizujący</span><span class="sxs-lookup"><span data-stu-id="41012-103">Logging and diagnostics in ASP.NET Core SignalR</span></span>

<span data-ttu-id="41012-104">Według [Andrew Stanton-pielęgniarki](https://twitter.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="41012-104">By [Andrew Stanton-Nurse](https://twitter.com/anurse)</span></span>

<span data-ttu-id="41012-105">Ten artykuł zawiera wskazówki dotyczące zbierania danych diagnostycznych z aplikacji sygnalizującej ASP.NET Core, aby pomóc w rozwiązywaniu problemów.</span><span class="sxs-lookup"><span data-stu-id="41012-105">This article provides guidance for gathering diagnostics from your ASP.NET Core SignalR app to help troubleshoot issues.</span></span>

## <a name="server-side-logging"></a><span data-ttu-id="41012-106">Rejestrowanie po stronie serwera</span><span class="sxs-lookup"><span data-stu-id="41012-106">Server-side logging</span></span>

> [!WARNING]
> <span data-ttu-id="41012-107">Dzienniki po stronie serwera mogą zawierać poufne informacje z aplikacji.</span><span class="sxs-lookup"><span data-stu-id="41012-107">Server-side logs may contain sensitive information from your app.</span></span> <span data-ttu-id="41012-108">**Nigdy nie** Publikuj nieprzetworzonych dzienników z aplikacji produkcyjnych na forach publicznych, takich jak GitHub.</span><span class="sxs-lookup"><span data-stu-id="41012-108">**Never** post raw logs from production apps to public forums like GitHub.</span></span>

<span data-ttu-id="41012-109">Ponieważ sygnalizujący jest częścią ASP.NET Core, używa systemu rejestrowania ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="41012-109">Since SignalR is part of ASP.NET Core, it uses the ASP.NET Core logging system.</span></span> <span data-ttu-id="41012-110">W konfiguracji domyślnej program sygnalizujący rejestruje bardzo mało informacji, ale może to być skonfigurowane.</span><span class="sxs-lookup"><span data-stu-id="41012-110">In the default configuration, SignalR logs very little information, but this can configured.</span></span> <span data-ttu-id="41012-111">Szczegółowe informacje na temat konfigurowania rejestrowania ASP.NET Core można znaleźć w dokumentacji dotyczącej [rejestrowania ASP.NET Core](xref:fundamentals/logging/index#configuration) .</span><span class="sxs-lookup"><span data-stu-id="41012-111">See the documentation on [ASP.NET Core logging](xref:fundamentals/logging/index#configuration) for details on configuring ASP.NET Core logging.</span></span>

<span data-ttu-id="41012-112">Sygnalizujący używa dwóch kategorii rejestratora:</span><span class="sxs-lookup"><span data-stu-id="41012-112">SignalR uses two logger categories:</span></span>

* <span data-ttu-id="41012-113">`Microsoft.AspNetCore.SignalR` &ndash; dzienników związanych z protokołami centrów, aktywowanie centrów, wywoływanie metod i innych działań związanych z centrum.</span><span class="sxs-lookup"><span data-stu-id="41012-113">`Microsoft.AspNetCore.SignalR` &ndash; for logs related to Hub Protocols, activating Hubs, invoking methods, and other Hub-related activities.</span></span>
* <span data-ttu-id="41012-114">`Microsoft.AspNetCore.Http.Connections` &ndash; dzienników związanych z transportami, takimi jak obiekty WebSockets, długotrwałe sondowanie i zdarzenia wysyłane przez serwer oraz infrastruktura sygnałów niskiego poziomu.</span><span class="sxs-lookup"><span data-stu-id="41012-114">`Microsoft.AspNetCore.Http.Connections` &ndash; for logs related to transports such as WebSockets, Long Polling and Server-Sent Events and low-level SignalR infrastructure.</span></span>

<span data-ttu-id="41012-115">Aby włączyć szczegółowe dzienniki od sygnalizującego, skonfiguruj obie powyższe prefiksy na poziomie `Debug` w pliku *appSettings. JSON* , dodając następujące elementy do podsekcji `LogLevel` w `Logging`:</span><span class="sxs-lookup"><span data-stu-id="41012-115">To enable detailed logs from SignalR, configure both of the preceding prefixes to the `Debug` level in your *appsettings.json* file by adding the following items to the `LogLevel` sub-section in `Logging`:</span></span>

[!code-json[](diagnostics/logging-config.json?highlight=7-8)]

<span data-ttu-id="41012-116">Możesz również skonfigurować ten kod w kodzie w metodzie `CreateWebHostBuilder`:</span><span class="sxs-lookup"><span data-stu-id="41012-116">You can also configure this in code in your `CreateWebHostBuilder` method:</span></span>

[!code-csharp[](diagnostics/logging-config-code.cs?highlight=5-6)]

<span data-ttu-id="41012-117">Jeśli nie korzystasz z konfiguracji opartej na notacji JSON, ustaw następujące wartości konfiguracji w systemie konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="41012-117">If you aren't using JSON-based configuration, set the following configuration values in your configuration system:</span></span>

* `Logging:LogLevel:Microsoft.AspNetCore.SignalR` = `Debug`
* `Logging:LogLevel:Microsoft.AspNetCore.Http.Connections` = `Debug`

<span data-ttu-id="41012-118">Zapoznaj się z dokumentacją systemu konfiguracyjnego, aby określić sposób określania zagnieżdżonych wartości konfiguracyjnych.</span><span class="sxs-lookup"><span data-stu-id="41012-118">Check the documentation for your configuration system to determine how to specify nested configuration values.</span></span> <span data-ttu-id="41012-119">Na przykład w przypadku używania zmiennych środowiskowych zamiast `:` są używane dwa `_` znaki (na przykład `Logging__LogLevel__Microsoft.AspNetCore.SignalR`).</span><span class="sxs-lookup"><span data-stu-id="41012-119">For example, when using environment variables, two `_` characters are used instead of the `:` (for example, `Logging__LogLevel__Microsoft.AspNetCore.SignalR`).</span></span>

<span data-ttu-id="41012-120">Zalecamy użycie poziomu `Debug` podczas zbierania bardziej szczegółowych informacji diagnostycznych dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="41012-120">We recommend using the `Debug` level when gathering more detailed diagnostics for your app.</span></span> <span data-ttu-id="41012-121">Na poziomie `Trace` powstaje Diagnostyka bardzo niskiego poziomu i jest rzadko wymagana do diagnozowania problemów w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="41012-121">The `Trace` level produces very low-level diagnostics and is rarely needed to diagnose issues in your app.</span></span>

## <a name="access-server-side-logs"></a><span data-ttu-id="41012-122">Dostęp do dzienników po stronie serwera</span><span class="sxs-lookup"><span data-stu-id="41012-122">Access server-side logs</span></span>

<span data-ttu-id="41012-123">Sposób dostępu do dzienników po stronie serwera zależy od środowiska, w którym jest uruchomiony program.</span><span class="sxs-lookup"><span data-stu-id="41012-123">How you access server-side logs depends on the environment in which you're running.</span></span>

### <a name="as-a-console-app-outside-iis"></a><span data-ttu-id="41012-124">Jako Aplikacja konsolowa poza usługami IIS</span><span class="sxs-lookup"><span data-stu-id="41012-124">As a console app outside IIS</span></span>

<span data-ttu-id="41012-125">Jeśli używasz programu w aplikacji konsolowej, [Rejestrator konsoli](xref:fundamentals/logging/index#console-provider) powinien być domyślnie włączony.</span><span class="sxs-lookup"><span data-stu-id="41012-125">If you're running in a console app, the [Console logger](xref:fundamentals/logging/index#console-provider) should be enabled by default.</span></span> <span data-ttu-id="41012-126">Dzienniki sygnalizujące będą wyświetlane w konsoli programu.</span><span class="sxs-lookup"><span data-stu-id="41012-126">SignalR logs will appear in the console.</span></span>

### <a name="within-iis-express-from-visual-studio"></a><span data-ttu-id="41012-127">W IIS Express z programu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="41012-127">Within IIS Express from Visual Studio</span></span>

<span data-ttu-id="41012-128">Program Visual Studio Wyświetla dane wyjściowe dziennika w oknie **danych wyjściowych** .</span><span class="sxs-lookup"><span data-stu-id="41012-128">Visual Studio displays the log output in the **Output** window.</span></span> <span data-ttu-id="41012-129">Wybierz opcję listy rozwijanej **ASP.NET Core serwera sieci Web** .</span><span class="sxs-lookup"><span data-stu-id="41012-129">Select the **ASP.NET Core Web Server** drop down option.</span></span>

### <a name="azure-app-service"></a><span data-ttu-id="41012-130">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="41012-130">Azure App Service</span></span>

<span data-ttu-id="41012-131">Włącz opcję **Rejestrowanie aplikacji (system plików)** w sekcji **dzienniki diagnostyki** w portalu Azure App Service i skonfiguruj **poziom** `Verbose`.</span><span class="sxs-lookup"><span data-stu-id="41012-131">Enable the **Application Logging (Filesystem)** option in the **Diagnostics logs** section of the Azure App Service portal and configure the **Level** to `Verbose`.</span></span> <span data-ttu-id="41012-132">Dzienniki powinny być dostępne w usłudze **przesyłania strumieniowego dzienników** oraz w dziennikach w systemie plików App Service.</span><span class="sxs-lookup"><span data-stu-id="41012-132">Logs should be available from the **Log streaming** service and in logs on the file system of the App Service.</span></span> <span data-ttu-id="41012-133">Aby uzyskać więcej informacji, zobacz [przesyłanie strumieniowe dzienników Azure](xref:fundamentals/logging/index#azure-log-streaming).</span><span class="sxs-lookup"><span data-stu-id="41012-133">For more information, see [Azure log streaming](xref:fundamentals/logging/index#azure-log-streaming).</span></span>

### <a name="other-environments"></a><span data-ttu-id="41012-134">Inne środowiska</span><span class="sxs-lookup"><span data-stu-id="41012-134">Other environments</span></span>

<span data-ttu-id="41012-135">Jeśli aplikacja jest wdrażana w innym środowisku (na przykład Docker, Kubernetes lub Windows Service), zobacz <xref:fundamentals/logging/index>, aby uzyskać więcej informacji na temat konfigurowania dostawców rejestrowania odpowiednie dla danego środowiska.</span><span class="sxs-lookup"><span data-stu-id="41012-135">If the app is deployed to another environment (for example, Docker, Kubernetes, or Windows Service), see <xref:fundamentals/logging/index> for more information on how to configure logging providers suitable for the environment.</span></span>

## <a name="javascript-client-logging"></a><span data-ttu-id="41012-136">Rejestrowanie klientów JavaScript</span><span class="sxs-lookup"><span data-stu-id="41012-136">JavaScript client logging</span></span>

> [!WARNING]
> <span data-ttu-id="41012-137">Dzienniki po stronie klienta mogą zawierać poufne informacje z aplikacji.</span><span class="sxs-lookup"><span data-stu-id="41012-137">Client-side logs may contain sensitive information from your app.</span></span> <span data-ttu-id="41012-138">**Nigdy nie** Publikuj nieprzetworzonych dzienników z aplikacji produkcyjnych na forach publicznych, takich jak GitHub.</span><span class="sxs-lookup"><span data-stu-id="41012-138">**Never** post raw logs from production apps to public forums like GitHub.</span></span>

<span data-ttu-id="41012-139">Korzystając z klienta JavaScript, można skonfigurować opcje rejestrowania za pomocą metody `configureLogging` na `HubConnectionBuilder`:</span><span class="sxs-lookup"><span data-stu-id="41012-139">When using the JavaScript client, you can configure logging options using the `configureLogging` method on `HubConnectionBuilder`:</span></span>

[!code-javascript[](diagnostics/logging-config-js.js?highlight=3)]

<span data-ttu-id="41012-140">Aby całkowicie wyłączyć rejestrowanie, określ `signalR.LogLevel.None` w `configureLogging` metodzie.</span><span class="sxs-lookup"><span data-stu-id="41012-140">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="41012-141">W poniższej tabeli przedstawiono poziomy dziennika dostępne dla klienta JavaScript.</span><span class="sxs-lookup"><span data-stu-id="41012-141">The following table shows log levels available to the JavaScript client.</span></span> <span data-ttu-id="41012-142">Ustawienie poziomu dziennika na jedną z tych wartości umożliwia rejestrowanie na tym poziomie i wszystkich poziomów powyżej niego w tabeli.</span><span class="sxs-lookup"><span data-stu-id="41012-142">Setting the log level to one of these values enables logging at that level and all levels above it in the table.</span></span>

| <span data-ttu-id="41012-143">Poziom</span><span class="sxs-lookup"><span data-stu-id="41012-143">Level</span></span> | <span data-ttu-id="41012-144">Opis</span><span class="sxs-lookup"><span data-stu-id="41012-144">Description</span></span> |
| ----- | ----------- |
| `None` | <span data-ttu-id="41012-145">Żadne komunikaty nie są rejestrowane.</span><span class="sxs-lookup"><span data-stu-id="41012-145">No messages are logged.</span></span> |
| `Critical` | <span data-ttu-id="41012-146">Komunikaty wskazujące niepowodzenie w całej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="41012-146">Messages that indicate a failure in the entire app.</span></span> |
| `Error` | <span data-ttu-id="41012-147">Komunikaty wskazujące niepowodzenie w bieżącej operacji.</span><span class="sxs-lookup"><span data-stu-id="41012-147">Messages that indicate a failure in the current operation.</span></span> |
| `Warning` | <span data-ttu-id="41012-148">Komunikaty wskazujące na problem niekrytyczny.</span><span class="sxs-lookup"><span data-stu-id="41012-148">Messages that indicate a non-fatal problem.</span></span> |
| `Information` | <span data-ttu-id="41012-149">Komunikaty informacyjne.</span><span class="sxs-lookup"><span data-stu-id="41012-149">Informational messages.</span></span> |
| `Debug` | <span data-ttu-id="41012-150">Komunikaty diagnostyczne przydatne do debugowania.</span><span class="sxs-lookup"><span data-stu-id="41012-150">Diagnostic messages useful for debugging.</span></span> |
| `Trace` | <span data-ttu-id="41012-151">Bardzo szczegółowe komunikaty diagnostyczne przeznaczone do diagnozowania określonych problemów.</span><span class="sxs-lookup"><span data-stu-id="41012-151">Very detailed diagnostic messages designed for diagnosing specific issues.</span></span> |

<span data-ttu-id="41012-152">Po skonfigurowaniu szczegółowości dzienniki zostaną zapisane w konsoli przeglądarki (lub w standardowym wyjściu w aplikacji NodeJS).</span><span class="sxs-lookup"><span data-stu-id="41012-152">Once you've configured the verbosity, the logs will be written to the Browser Console (or Standard Output in a NodeJS app).</span></span>

<span data-ttu-id="41012-153">Jeśli chcesz wysłać dzienniki do niestandardowego systemu rejestrowania, możesz dostarczyć obiekt JavaScript implementujący interfejs `ILogger`.</span><span class="sxs-lookup"><span data-stu-id="41012-153">If you want to send logs to a custom logging system, you can provide a JavaScript object implementing the `ILogger` interface.</span></span> <span data-ttu-id="41012-154">Jedyną metodą, która musi zostać wdrożona, jest `log`, która pobiera poziom zdarzenia i komunikat skojarzony ze zdarzeniem.</span><span class="sxs-lookup"><span data-stu-id="41012-154">The only method that needs to be implemented is `log`, which takes the level of the event and the message associated with the event.</span></span> <span data-ttu-id="41012-155">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="41012-155">For example:</span></span>

[!code-typescript[](diagnostics/custom-logger.ts?highlight=3-7,13)]

## <a name="net-client-logging"></a><span data-ttu-id="41012-156">Rejestrowanie klienta platformy .NET</span><span class="sxs-lookup"><span data-stu-id="41012-156">.NET client logging</span></span>

> [!WARNING]
> <span data-ttu-id="41012-157">Dzienniki po stronie klienta mogą zawierać poufne informacje z aplikacji.</span><span class="sxs-lookup"><span data-stu-id="41012-157">Client-side logs may contain sensitive information from your app.</span></span> <span data-ttu-id="41012-158">**Nigdy nie** Publikuj nieprzetworzonych dzienników z aplikacji produkcyjnych na forach publicznych, takich jak GitHub.</span><span class="sxs-lookup"><span data-stu-id="41012-158">**Never** post raw logs from production apps to public forums like GitHub.</span></span>

<span data-ttu-id="41012-159">Aby pobrać dzienniki z klienta .NET, można użyć metody `ConfigureLogging` w `HubConnectionBuilder`.</span><span class="sxs-lookup"><span data-stu-id="41012-159">To get logs from the .NET client, you can use the `ConfigureLogging` method on `HubConnectionBuilder`.</span></span> <span data-ttu-id="41012-160">Działa tak samo jak Metoda `ConfigureLogging` na `WebHostBuilder` i `HostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="41012-160">This works the same way as the `ConfigureLogging` method on `WebHostBuilder` and `HostBuilder`.</span></span> <span data-ttu-id="41012-161">Można skonfigurować tych samych dostawców rejestrowania, których używasz w ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="41012-161">You can configure the same logging providers you use in ASP.NET Core.</span></span> <span data-ttu-id="41012-162">Należy jednak ręcznie zainstalować i włączyć pakiety NuGet dla poszczególnych dostawców rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="41012-162">However, you have to manually install and enable the NuGet packages for the individual logging providers.</span></span>

### <a name="console-logging"></a><span data-ttu-id="41012-163">Rejestrowanie konsoli</span><span class="sxs-lookup"><span data-stu-id="41012-163">Console logging</span></span>

<span data-ttu-id="41012-164">Aby włączyć rejestrowanie konsoli, Dodaj pakiet [Microsoft. Extensions. Logging. Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) .</span><span class="sxs-lookup"><span data-stu-id="41012-164">In order to enable Console logging, add the [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) package.</span></span> <span data-ttu-id="41012-165">Następnie użyj metody `AddConsole`, aby skonfigurować rejestratora konsoli:</span><span class="sxs-lookup"><span data-stu-id="41012-165">Then, use the `AddConsole` method to configure the console logger:</span></span>

[!code-csharp[](diagnostics/net-client-console-log.cs?highlight=6)]

### <a name="debug-output-window-logging"></a><span data-ttu-id="41012-166">Rejestrowanie okna danych wyjściowych debugowania</span><span class="sxs-lookup"><span data-stu-id="41012-166">Debug output window logging</span></span>

<span data-ttu-id="41012-167">Możesz również skonfigurować dzienniki, aby przejść do okna **danych wyjściowych** w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="41012-167">You can also configure logs to go to the **Output** window in Visual Studio.</span></span> <span data-ttu-id="41012-168">Zainstaluj pakiet [Microsoft. Extensions. Logging. Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) i użyj metody `AddDebug`:</span><span class="sxs-lookup"><span data-stu-id="41012-168">Install the [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) package and use the `AddDebug` method:</span></span>

[!code-csharp[](diagnostics/net-client-debug-log.cs?highlight=6)]

### <a name="other-logging-providers"></a><span data-ttu-id="41012-169">Inni dostawcy rejestrowania</span><span class="sxs-lookup"><span data-stu-id="41012-169">Other logging providers</span></span>

SignalR<span data-ttu-id="41012-170"> obsługuje innych dostawców rejestrowania, takich jak Serilog, SEQ, NLog lub jakikolwiek inny system rejestrowania, który integruje się z `Microsoft.Extensions.Logging`.</span><span class="sxs-lookup"><span data-stu-id="41012-170"> supports other logging providers such as Serilog, Seq, NLog, or any other logging system that integrates with `Microsoft.Extensions.Logging`.</span></span> <span data-ttu-id="41012-171">Jeśli system rejestrowania zawiera `ILoggerProvider`, można zarejestrować go za pomocą `AddProvider`:</span><span class="sxs-lookup"><span data-stu-id="41012-171">If your logging system provides an `ILoggerProvider`, you can register it with `AddProvider`:</span></span>

[!code-csharp[](diagnostics/net-client-custom-log.cs?highlight=6)]

### <a name="control-verbosity"></a><span data-ttu-id="41012-172">Szczegółowość kontroli</span><span class="sxs-lookup"><span data-stu-id="41012-172">Control verbosity</span></span>

<span data-ttu-id="41012-173">Jeśli rejestrujesz się z innych miejsc w aplikacji, zmiana domyślnego poziomu na `Debug` może być zbyt pełna.</span><span class="sxs-lookup"><span data-stu-id="41012-173">If you are logging from other places in your app, changing the default level to `Debug` may be too verbose.</span></span> <span data-ttu-id="41012-174">Możesz użyć filtru, aby skonfigurować poziom rejestrowania dla dzienników SignalR.</span><span class="sxs-lookup"><span data-stu-id="41012-174">You can use a Filter to configure the logging level for SignalR logs.</span></span> <span data-ttu-id="41012-175">Można to zrobić w kodzie w taki sam sposób jak na serwerze:</span><span class="sxs-lookup"><span data-stu-id="41012-175">This can be done in code, in much the same way as on the server:</span></span>

[!code-csharp[Controlling verbosity in .NET client](diagnostics/logging-config-client-code.cs?highlight=9-10)]

## <a name="network-traces"></a><span data-ttu-id="41012-176">Ślady sieci</span><span class="sxs-lookup"><span data-stu-id="41012-176">Network traces</span></span>

> [!WARNING]
> <span data-ttu-id="41012-177">Śledzenie sieci zawiera pełną zawartość każdej wiadomości wysyłanej przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="41012-177">A network trace contains the full contents of every message sent by your app.</span></span> <span data-ttu-id="41012-178">**Nigdy nie** Publikuj nieprzetworzonych danych śledzenia sieci z aplikacji produkcyjnych na forach publicznych, takich jak GitHub.</span><span class="sxs-lookup"><span data-stu-id="41012-178">**Never** post raw network traces from production apps to public forums like GitHub.</span></span>

<span data-ttu-id="41012-179">W przypadku wystąpienia problemu śledzenie sieci może czasami dostarczyć wiele przydatnych informacji.</span><span class="sxs-lookup"><span data-stu-id="41012-179">If you encounter an issue, a network trace can sometimes provide a lot of helpful information.</span></span> <span data-ttu-id="41012-180">Jest to szczególnie przydatne, jeśli zamierzasz rozwiązać problem z naszym narzędziem do śledzenia problemów.</span><span class="sxs-lookup"><span data-stu-id="41012-180">This is particularly useful if you're going to file an issue on our issue tracker.</span></span>

## <a name="collect-a-network-trace-with-fiddler-preferred-option"></a><span data-ttu-id="41012-181">Zbieranie danych śledzenia sieci za pomocą programu Fiddler (preferowana opcja)</span><span class="sxs-lookup"><span data-stu-id="41012-181">Collect a network trace with Fiddler (preferred option)</span></span>

<span data-ttu-id="41012-182">Ta metoda działa w przypadku wszystkich aplikacji.</span><span class="sxs-lookup"><span data-stu-id="41012-182">This method works for all apps.</span></span>

<span data-ttu-id="41012-183">Programu Fiddler to bardzo zaawansowane narzędzie do zbierania śladów HTTP.</span><span class="sxs-lookup"><span data-stu-id="41012-183">Fiddler is a very powerful tool for collecting HTTP traces.</span></span> <span data-ttu-id="41012-184">Zainstaluj ją z [Telerik.com/Fiddler](https://www.telerik.com/fiddler), uruchom ją, a następnie uruchom aplikację i Odtwórz problem.</span><span class="sxs-lookup"><span data-stu-id="41012-184">Install it from [telerik.com/fiddler](https://www.telerik.com/fiddler), launch it, and then run your app and reproduce the issue.</span></span> <span data-ttu-id="41012-185">Programu Fiddler jest dostępny dla systemu Windows, a dostępne wersje beta dla macOS i Linux.</span><span class="sxs-lookup"><span data-stu-id="41012-185">Fiddler is available for Windows, and there are beta versions for macOS and Linux.</span></span>

<span data-ttu-id="41012-186">Jeśli łączysz się przy użyciu protokołu HTTPS, należy wykonać kilka dodatkowych kroków, aby programu Fiddler można było odszyfrować ruch HTTPS.</span><span class="sxs-lookup"><span data-stu-id="41012-186">If you connect using HTTPS, there are some extra steps to ensure Fiddler can decrypt the HTTPS traffic.</span></span> <span data-ttu-id="41012-187">Aby uzyskać więcej informacji, zobacz [dokumentację programu Fiddler](https://docs.telerik.com/fiddler/Configure-Fiddler/Tasks/DecryptHTTPS).</span><span class="sxs-lookup"><span data-stu-id="41012-187">For more details, see the [Fiddler documentation](https://docs.telerik.com/fiddler/Configure-Fiddler/Tasks/DecryptHTTPS).</span></span>

<span data-ttu-id="41012-188">Po zebraniu śladu można wyeksportować śledzenie, wybierając pozycję **plik** > **Zapisz** > **wszystkie sesje** z paska menu.</span><span class="sxs-lookup"><span data-stu-id="41012-188">Once you've collected the trace, you can export the trace by choosing **File** > **Save** > **All Sessions** from the menu bar.</span></span>

![Eksportowanie wszystkich sesji z programu Fiddler](diagnostics/fiddler-export.png)

## <a name="collect-a-network-trace-with-tcpdump-macos-and-linux-only"></a><span data-ttu-id="41012-190">Zbieraj dane śledzenia sieci z tcpdump (tylko macOS i Linux)</span><span class="sxs-lookup"><span data-stu-id="41012-190">Collect a network trace with tcpdump (macOS and Linux only)</span></span>

<span data-ttu-id="41012-191">Ta metoda działa w przypadku wszystkich aplikacji.</span><span class="sxs-lookup"><span data-stu-id="41012-191">This method works for all apps.</span></span>

<span data-ttu-id="41012-192">Można zbierać pierwotne ślady TCP przy użyciu tcpdump, uruchamiając następujące polecenie z poziomu powłoki poleceń.</span><span class="sxs-lookup"><span data-stu-id="41012-192">You can collect raw TCP traces using tcpdump by running the following command from a command shell.</span></span> <span data-ttu-id="41012-193">Może być konieczne `root` lub prefiks polecenia z `sudo`, jeśli wystąpi błąd uprawnień:</span><span class="sxs-lookup"><span data-stu-id="41012-193">You may need to be `root` or prefix the command with `sudo` if you get a permissions error:</span></span>

```console
tcpdump -i [interface] -w trace.pcap
```

<span data-ttu-id="41012-194">Zastąp `[interface]` interfejsem sieciowym, w którym chcesz przechwytywać.</span><span class="sxs-lookup"><span data-stu-id="41012-194">Replace `[interface]` with the network interface you wish to capture on.</span></span> <span data-ttu-id="41012-195">Zwykle jest to podobne `/dev/eth0` (dla standardowego interfejsu Ethernet) lub `/dev/lo0` (dla ruchu localhost).</span><span class="sxs-lookup"><span data-stu-id="41012-195">Usually, this is something like `/dev/eth0` (for your standard Ethernet interface) or `/dev/lo0` (for localhost traffic).</span></span> <span data-ttu-id="41012-196">Aby uzyskać więcej informacji, zobacz stronę `tcpdump` Man w systemie hosta.</span><span class="sxs-lookup"><span data-stu-id="41012-196">For more information, see the `tcpdump` man page on your host system.</span></span>

## <a name="collect-a-network-trace-in-the-browser"></a><span data-ttu-id="41012-197">Zbieranie danych śledzenia sieci w przeglądarce</span><span class="sxs-lookup"><span data-stu-id="41012-197">Collect a network trace in the browser</span></span>

<span data-ttu-id="41012-198">Ta metoda działa tylko w przypadku aplikacji opartych na przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="41012-198">This method only works for browser-based apps.</span></span>

<span data-ttu-id="41012-199">Większość przeglądarek Narzędzia deweloperskie ma kartę sieciową, która umożliwia przechwytywanie aktywności sieciowej między przeglądarką a serwerem.</span><span class="sxs-lookup"><span data-stu-id="41012-199">Most browser Developer Tools have a "Network" tab that allows you to capture network activity between the browser and the server.</span></span> <span data-ttu-id="41012-200">Jednak te ślady nie obejmują komunikatów o zdarzeniach dotyczących protokołu WebSocket i wysyłanych przez serwer.</span><span class="sxs-lookup"><span data-stu-id="41012-200">However, these traces don't include WebSocket and Server-Sent Event messages.</span></span> <span data-ttu-id="41012-201">Jeśli są używane te transporty, należy użyć narzędzia, takiego jak programu Fiddler lub TcpDump (opisane poniżej).</span><span class="sxs-lookup"><span data-stu-id="41012-201">If you are using those transports, using a tool like Fiddler or TcpDump (described below) is a better approach.</span></span>

### <a name="microsoft-edge-and-internet-explorer"></a><span data-ttu-id="41012-202">Microsoft Edge i Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="41012-202">Microsoft Edge and Internet Explorer</span></span>

<span data-ttu-id="41012-203">(Instrukcje są takie same dla przeglądarki Edge i Internet Explorer)</span><span class="sxs-lookup"><span data-stu-id="41012-203">(The instructions are the same for both Edge and Internet Explorer)</span></span>

1. <span data-ttu-id="41012-204">Naciśnij klawisz F12, aby otworzyć narzędzia deweloperskie</span><span class="sxs-lookup"><span data-stu-id="41012-204">Press F12 to open the Dev Tools</span></span>
2. <span data-ttu-id="41012-205">Kliknij kartę Sieć</span><span class="sxs-lookup"><span data-stu-id="41012-205">Click the Network Tab</span></span>
3. <span data-ttu-id="41012-206">Odśwież stronę (w razie konieczności) i Odtwórz problem</span><span class="sxs-lookup"><span data-stu-id="41012-206">Refresh the page (if needed) and reproduce the problem</span></span>
4. <span data-ttu-id="41012-207">Kliknij ikonę Zapisz na pasku narzędzi, aby wyeksportować śledzenie jako plik "HAR":</span><span class="sxs-lookup"><span data-stu-id="41012-207">Click the Save icon in the toolbar to export the trace as a "HAR" file:</span></span>

![Ikona Zapisz na karcie Sieć narzędzi deweloperskich Microsoft Edge](diagnostics/ie-edge-har-export.png)

### <a name="google-chrome"></a><span data-ttu-id="41012-209">Google Chrome</span><span class="sxs-lookup"><span data-stu-id="41012-209">Google Chrome</span></span>

1. <span data-ttu-id="41012-210">Naciśnij klawisz F12, aby otworzyć narzędzia deweloperskie</span><span class="sxs-lookup"><span data-stu-id="41012-210">Press F12 to open the Dev Tools</span></span>
2. <span data-ttu-id="41012-211">Kliknij kartę Sieć</span><span class="sxs-lookup"><span data-stu-id="41012-211">Click the Network Tab</span></span>
3. <span data-ttu-id="41012-212">Odśwież stronę (w razie konieczności) i Odtwórz problem</span><span class="sxs-lookup"><span data-stu-id="41012-212">Refresh the page (if needed) and reproduce the problem</span></span>
4. <span data-ttu-id="41012-213">Kliknij prawym przyciskiem myszy w dowolnym miejscu na liście żądań i wybierz polecenie "Zapisz jako HAR z zawartością":</span><span class="sxs-lookup"><span data-stu-id="41012-213">Right click anywhere in the list of requests and choose "Save as HAR with content":</span></span>

![Opcja "Zapisz jako HAR z zawartością" w karcie Sieć narzędzi deweloperskich Google Chrome](diagnostics/chrome-har-export.png)

### <a name="mozilla-firefox"></a><span data-ttu-id="41012-215">Mozilla Firefox</span><span class="sxs-lookup"><span data-stu-id="41012-215">Mozilla Firefox</span></span>

1. <span data-ttu-id="41012-216">Naciśnij klawisz F12, aby otworzyć narzędzia deweloperskie</span><span class="sxs-lookup"><span data-stu-id="41012-216">Press F12 to open the Dev Tools</span></span>
2. <span data-ttu-id="41012-217">Kliknij kartę Sieć</span><span class="sxs-lookup"><span data-stu-id="41012-217">Click the Network Tab</span></span>
3. <span data-ttu-id="41012-218">Odśwież stronę (w razie konieczności) i Odtwórz problem</span><span class="sxs-lookup"><span data-stu-id="41012-218">Refresh the page (if needed) and reproduce the problem</span></span>
4. <span data-ttu-id="41012-219">Kliknij prawym przyciskiem myszy w dowolnym miejscu na liście żądań i wybierz pozycję "Zapisz wszystko jako HAR"</span><span class="sxs-lookup"><span data-stu-id="41012-219">Right click anywhere in the list of requests and choose "Save All As HAR"</span></span>

![Opcja "Zapisz wszystko jako HAR" w obszarze Mozilla Firefox dev Tools Network karta](diagnostics/firefox-har-export.png)

## <a name="attach-diagnostics-files-to-github-issues"></a><span data-ttu-id="41012-221">Dołączanie plików diagnostycznych do problemów z usługą GitHub</span><span class="sxs-lookup"><span data-stu-id="41012-221">Attach diagnostics files to GitHub issues</span></span>

<span data-ttu-id="41012-222">Pliki diagnostyczne można dołączać do problemów z usługą GitHub, zmieniając ich nazwy, aby miały `.txt` rozszerzenie, a następnie przeciąganie i upuszczanie na ten problem.</span><span class="sxs-lookup"><span data-stu-id="41012-222">You can attach Diagnostics files to GitHub issues by renaming them so they have a `.txt` extension and then dragging and dropping them on to the issue.</span></span>

> [!NOTE]
> <span data-ttu-id="41012-223">Nie należy wklejać zawartości plików dziennika ani śladów sieci do problemu w usłudze GitHub.</span><span class="sxs-lookup"><span data-stu-id="41012-223">Please don't paste the content of log files or network traces into a GitHub issue.</span></span> <span data-ttu-id="41012-224">Te dzienniki i ślady mogą być bardzo duże, a usługi GitHub zwykle obcinają je.</span><span class="sxs-lookup"><span data-stu-id="41012-224">These logs and traces can be quite large, and GitHub usually truncates them.</span></span>

![Przeciąganie plików dziennika do problemu z usługą GitHub](diagnostics/attaching-diagnostics-files.png)

## <a name="additional-resources"></a><span data-ttu-id="41012-226">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="41012-226">Additional resources</span></span>

* <xref:signalr/configuration>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
