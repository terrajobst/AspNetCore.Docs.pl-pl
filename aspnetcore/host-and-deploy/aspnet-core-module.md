---
title: "Konfiguracja modułu Core programu ASP.NET"
author: guardrex
description: "Dowiedz się, jak skonfigurować moduł platformy ASP.NET Core do hostowania aplikacji platformy ASP.NET Core."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 02/15/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/aspnet-core-module
ms.openlocfilehash: c01abed767a226eae68725c1c53d922eac2f705e
ms.sourcegitcommit: 49fb3b7669b504d35edad34db8285e56b958a9fc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/23/2018
---
# <a name="aspnet-core-module-configuration-reference"></a><span data-ttu-id="c382d-103">Konfiguracja modułu Core programu ASP.NET</span><span class="sxs-lookup"><span data-stu-id="c382d-103">ASP.NET Core Module configuration reference</span></span>

<span data-ttu-id="c382d-104">Przez [Luke Latham](https://github.com/guardrex), [Rick Anderson](https://twitter.com/RickAndMSFT), i [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="c382d-104">By [Luke Latham](https://github.com/guardrex), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="c382d-105">Ten dokument zawiera instrukcje dotyczące sposobu konfigurowania modułu Core ASP.NET do obsługi aplikacji platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c382d-105">This document provides instructions on how to configure the ASP.NET Core Module for hosting ASP.NET Core apps.</span></span> <span data-ttu-id="c382d-106">Aby obejrzeć wprowadzenie do platformy ASP.NET Core moduł i instrukcje dotyczące instalacji, zobacz [Przegląd platformy ASP.NET Core modułu](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="c382d-106">For an introduction to the ASP.NET Core Module and installation instructions, see the [ASP.NET Core Module overview](xref:fundamentals/servers/aspnet-core-module).</span></span>

## <a name="configuration-with-webconfig"></a><span data-ttu-id="c382d-107">Konfiguracja z pliku web.config</span><span class="sxs-lookup"><span data-stu-id="c382d-107">Configuration with web.config</span></span>

<span data-ttu-id="c382d-108">Moduł Core ASP.NET jest skonfigurowany z `aspNetCore` sekcji `system.webServer` węzła w tej witrynie *web.config* pliku.</span><span class="sxs-lookup"><span data-stu-id="c382d-108">The ASP.NET Core Module is configured with the `aspNetCore` section of the `system.webServer` node in the site's *web.config* file.</span></span>

<span data-ttu-id="c382d-109">Następujące *web.config* pliku została opublikowana na potrzeby [wdrożenia zależne od framework](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) i konfiguruje moduł Core ASP.NET do obsługi żądań lokacji:</span><span class="sxs-lookup"><span data-stu-id="c382d-109">The following *web.config* file is published for a [framework-dependent deployment](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) and configures the ASP.NET Core Module to handle site requests:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath="dotnet" 
                arguments=".\MyApp.dll" 
                stdoutLogEnabled="false" 
                stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

<span data-ttu-id="c382d-110">Następujące *web.config* została opublikowana na potrzeby [niezależne wdrożenia](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span><span class="sxs-lookup"><span data-stu-id="c382d-110">The following *web.config* is published for a [self-contained deployment](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath=".\MyApp.exe" 
                stdoutLogEnabled="false" 
                stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

<span data-ttu-id="c382d-111">Gdy aplikacja jest wdrażana na [usłudze Azure App Service](https://azure.microsoft.com/services/app-service/), `stdoutLogFile` ustawiono ścieżkę `\\?\%home%\LogFiles\stdout`.</span><span class="sxs-lookup"><span data-stu-id="c382d-111">When an app is deployed to [Azure App Service](https://azure.microsoft.com/services/app-service/), the `stdoutLogFile` path is set to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="c382d-112">Ścieżka zapisuje dzienniki stdout *LogFiles* folderu, w którym znajduje się automatycznie utworzone przez usługę.</span><span class="sxs-lookup"><span data-stu-id="c382d-112">The path saves stdout logs to the *LogFiles* folder, which is a location automatically created by the service.</span></span>

<span data-ttu-id="c382d-113">Zobacz [podrzędna aplikacji konfiguracji](xref:host-and-deploy/iis/index#sub-application-configuration) ważne uwagi dotyczące konfiguracji *web.config* plików w aplikacjach podrzędnych.</span><span class="sxs-lookup"><span data-stu-id="c382d-113">See [Sub-application configuration](xref:host-and-deploy/iis/index#sub-application-configuration) for an important note pertaining to the configuration of *web.config* files in sub-apps.</span></span>

### <a name="attributes-of-the-aspnetcore-element"></a><span data-ttu-id="c382d-114">Atrybuty elementu aspNetCore</span><span class="sxs-lookup"><span data-stu-id="c382d-114">Attributes of the aspNetCore element</span></span>

| <span data-ttu-id="c382d-115">Atrybut</span><span class="sxs-lookup"><span data-stu-id="c382d-115">Attribute</span></span> | <span data-ttu-id="c382d-116">Opis</span><span class="sxs-lookup"><span data-stu-id="c382d-116">Description</span></span> | <span data-ttu-id="c382d-117">Domyślny</span><span class="sxs-lookup"><span data-stu-id="c382d-117">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="c382d-118">Opcjonalny atrybut ciągu.</span><span class="sxs-lookup"><span data-stu-id="c382d-118">Optional string attribute.</span></span></p><p><span data-ttu-id="c382d-119">Argumenty plik wykonywalny określony w **processPath**.</span><span class="sxs-lookup"><span data-stu-id="c382d-119">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <span data-ttu-id="c382d-120">prawda lub fałsz.</span><span class="sxs-lookup"><span data-stu-id="c382d-120">true or false.</span></span></p><p><span data-ttu-id="c382d-121">Jeśli PRAWDA, **502.5 — niepowodzenie procesu** strony jest pomijane, a strona kodowa 502 stan skonfigurowane w *web.config* pierwszeństwo.</span><span class="sxs-lookup"><span data-stu-id="c382d-121">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <span data-ttu-id="c382d-122">prawda lub fałsz.</span><span class="sxs-lookup"><span data-stu-id="c382d-122">true or false.</span></span></p><p><span data-ttu-id="c382d-123">Jeśli PRAWDA, token jest przekazywane do procesu podrzędnego nasłuchiwanie ASPNETCORE_PORT % jako nagłówek "MS-ASPNETCORE-WINAUTHTOKEN" na żądanie.</span><span class="sxs-lookup"><span data-stu-id="c382d-123">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="c382d-124">Jest odpowiedzialny za ten proces może wywołać funkcji CloseHandle: ten token na żądanie.</span><span class="sxs-lookup"><span data-stu-id="c382d-124">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processPath` | <p><span data-ttu-id="c382d-125">Atrybut wymaganych parametrów.</span><span class="sxs-lookup"><span data-stu-id="c382d-125">Required string attribute.</span></span></p><p><span data-ttu-id="c382d-126">Ścieżka do pliku wykonywalnego, który uruchamia proces nasłuchiwanie żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="c382d-126">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="c382d-127">Ścieżki względne są obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="c382d-127">Relative paths are supported.</span></span> <span data-ttu-id="c382d-128">Jeśli ścieżka zaczyna się od `.`, ścieżka jest traktowany jako względem katalogu głównego witryny.</span><span class="sxs-lookup"><span data-stu-id="c382d-128">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="c382d-129">Opcjonalny atrybut całkowity.</span><span class="sxs-lookup"><span data-stu-id="c382d-129">Optional integer attribute.</span></span></p><p><span data-ttu-id="c382d-130">Określa liczbę powtórzeń procesu w **processPath** może awarii na minutę.</span><span class="sxs-lookup"><span data-stu-id="c382d-130">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="c382d-131">Po przekroczeniu tego limitu modułu zatrzymuje uruchamiania procesu w pozostałej części minutę.</span><span class="sxs-lookup"><span data-stu-id="c382d-131">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | `10` |
| `requestTimeout` | <p><span data-ttu-id="c382d-132">Atrybut opcjonalny timespan.</span><span class="sxs-lookup"><span data-stu-id="c382d-132">Optional timespan attribute.</span></span></p><p><span data-ttu-id="c382d-133">Określa okres czasu, dla którego moduł platformy ASP.NET Core czeka na odpowiedź z procesu nasłuchiwanie ASPNETCORE_PORT %.</span><span class="sxs-lookup"><span data-stu-id="c382d-133">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="c382d-134">`requestTimeout` Muszą być określone w pełnych minutach, w przeciwnym razie domyślne 2 minuty.</span><span class="sxs-lookup"><span data-stu-id="c382d-134">The `requestTimeout` must be specified in whole minutes only, otherwise it defaults to 2 minutes.</span></span></p> | `00:02:00` |
| `shutdownTimeLimit` | <p><span data-ttu-id="c382d-135">Opcjonalny atrybut całkowity.</span><span class="sxs-lookup"><span data-stu-id="c382d-135">Optional integer attribute.</span></span></p><p><span data-ttu-id="c382d-136">Czas w sekundach modułu dla pliku wykonywalnego jest bezpiecznie zamknąć po *app_offline.htm* Wykryto plik.</span><span class="sxs-lookup"><span data-stu-id="c382d-136">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | `10` |
| `startupTimeLimit` | <p><span data-ttu-id="c382d-137">Opcjonalny atrybut całkowity.</span><span class="sxs-lookup"><span data-stu-id="c382d-137">Optional integer attribute.</span></span></p><p><span data-ttu-id="c382d-138">Czas w sekundach modułu dla pliku wykonywalnego do uruchomienia procesu nasłuchiwanie na porcie.</span><span class="sxs-lookup"><span data-stu-id="c382d-138">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="c382d-139">Po przekroczeniu tego limitu czasu modułu kasuje procesu.</span><span class="sxs-lookup"><span data-stu-id="c382d-139">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="c382d-140">Moduł próbuje uruchomić proces, kiedy odbierze żądanie nowej i podejmować próby ponownego uruchomienia procesu dla kolejnych żądań przychodzących, chyba że aplikacja nie została uruchomiona w dalszym ciągu się **rapidFailsPerMinute** razy w ciągu ostatnich wycofanie minutę.</span><span class="sxs-lookup"><span data-stu-id="c382d-140">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p> | `120` |
| `stdoutLogEnabled` | <p><span data-ttu-id="c382d-141">Opcjonalny logiczny atrybut.</span><span class="sxs-lookup"><span data-stu-id="c382d-141">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="c382d-142">Jeśli PRAWDA, **stdout** i **stderr** dla określonym w procesie **processPath** są przekierowywane do pliku określonego w **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="c382d-142">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="c382d-143">Opcjonalny atrybut ciągu.</span><span class="sxs-lookup"><span data-stu-id="c382d-143">Optional string attribute.</span></span></p><p><span data-ttu-id="c382d-144">Określa ścieżkę względną lub bezwzględną, dla którego **stdout** i **stderr** z określonym w procesie **processPath** są rejestrowane.</span><span class="sxs-lookup"><span data-stu-id="c382d-144">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="c382d-145">Ścieżki względne są względem katalogu głównego witryny.</span><span class="sxs-lookup"><span data-stu-id="c382d-145">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="c382d-146">Dowolną ścieżkę, począwszy od `.` są względem lokacji głównej i wszystkich innych ścieżek są traktowane jako ścieżki bezwzględne.</span><span class="sxs-lookup"><span data-stu-id="c382d-146">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="c382d-147">Wszystkie foldery w ścieżce musi istnieć w kolejności dla modułu utworzyć plik dziennika.</span><span class="sxs-lookup"><span data-stu-id="c382d-147">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="c382d-148">Przy użyciu ograniczników podkreślenia, timestamp, identyfikator procesu i rozszerzenie pliku (*log*) są dodawane do ostatniego segment **stdoutLogFile** ścieżki.</span><span class="sxs-lookup"><span data-stu-id="c382d-148">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="c382d-149">Jeśli `.\logs\stdout` jest podana jako wartość przykład stdout dziennik jest zapisywany jako *stdout_20180205194132_1934.log* w *dzienniki* folder po zapisaniu na 2/5/2018 na 19:41:32 z procesu o identyfikatorze 1934.</span><span class="sxs-lookup"><span data-stu-id="c382d-149">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

### <a name="setting-environment-variables"></a><span data-ttu-id="c382d-150">Ustawianie zmiennych środowiskowych</span><span class="sxs-lookup"><span data-stu-id="c382d-150">Setting environment variables</span></span>

<span data-ttu-id="c382d-151">Zmienne środowiskowe można określić dla tego procesu w `processPath` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="c382d-151">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="c382d-152">Określ zmienną środowiskową o `environmentVariable` elementem podrzędnym `environmentVariables` elementu kolekcji.</span><span class="sxs-lookup"><span data-stu-id="c382d-152">Specify an environment variable with the `environmentVariable` child element of an `environmentVariables` collection element.</span></span> <span data-ttu-id="c382d-153">Zmienne środowiskowe w tej sekcji mają pierwszeństwo względem systemu zmiennych środowiskowych.</span><span class="sxs-lookup"><span data-stu-id="c382d-153">Environment variables set in this section take precedence over system environment variables.</span></span>

<span data-ttu-id="c382d-154">Poniższy przykład przedstawia dwa zmiennych środowiskowych.</span><span class="sxs-lookup"><span data-stu-id="c382d-154">The following example sets two environment variables.</span></span> <span data-ttu-id="c382d-155">`ASPNETCORE_ENVIRONMENT` Konfiguruje aplikację w środowisku `Development`.</span><span class="sxs-lookup"><span data-stu-id="c382d-155">`ASPNETCORE_ENVIRONMENT` configures the app's environment to `Development`.</span></span> <span data-ttu-id="c382d-156">Deweloper może tymczasowo Ustaw tę wartość *web.config* pliku, aby wymusić [strona wyjątek dewelopera](xref:fundamentals/error-handling) załadować podczas debugowania aplikacji wyjątek.</span><span class="sxs-lookup"><span data-stu-id="c382d-156">A developer may temporarily set this value in the *web.config* file in order to force the [Developer Exception Page](xref:fundamentals/error-handling) to load when debugging an app exception.</span></span> <span data-ttu-id="c382d-157">`CONFIG_DIR` jest przykład zmiennej środowiska użytkownika, gdy projektanta został zapisany kod odczytuje wartość przy uruchamianiu do utworzenia ścieżka pliku konfiguracji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c382d-157">`CONFIG_DIR` is an example of a user-defined environment variable, where the developer has written code that reads the value on startup to form a path for loading the app's configuration file.</span></span>

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile="\\?\%home%\LogFiles\stdout">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
    <environmentVariable name="CONFIG_DIR" value="f:\application_config" />
  </environmentVariables>
</aspNetCore>
```

> [!WARNING]
> <span data-ttu-id="c382d-158">Ustawić tylko `ASPNETCORE_ENVIRONMENT` zmiennej envirnonment `Development` przemieszczania i testowanie serwerów, które nie są dostępne z niezaufanymi sieciami, na przykład Internetem.</span><span class="sxs-lookup"><span data-stu-id="c382d-158">Only set the `ASPNETCORE_ENVIRONMENT` envirnonment variable to `Development` on staging and testing servers that aren't accessible to untrusted networks, such as the Internet.</span></span>

## <a name="appofflinehtm"></a><span data-ttu-id="c382d-159">app_offline.htm</span><span class="sxs-lookup"><span data-stu-id="c382d-159">app_offline.htm</span></span>

<span data-ttu-id="c382d-160">Jeśli plik o nazwie *app_offline.htm* została wykryta w katalogu głównym aplikacji platformy ASP.NET Core moduł próbuje bezpiecznie zamknięcia aplikacji i Zatrzymaj przetwarzania przychodzących żądań.</span><span class="sxs-lookup"><span data-stu-id="c382d-160">If a file with the name *app_offline.htm* is detected in the root directory of an app, the ASP.NET Core Module attempts to gracefully shutdown the app and stop processing incoming requests.</span></span> <span data-ttu-id="c382d-161">Jeśli aplikacja nadal działa po upływie liczby sekund zdefiniowane w `shutdownTimeLimit`, moduł platformy ASP.NET Core kasuje uruchomionego procesu.</span><span class="sxs-lookup"><span data-stu-id="c382d-161">If the app is still running after the number of seconds defined in `shutdownTimeLimit`, the ASP.NET Core Module kills the running process.</span></span>

<span data-ttu-id="c382d-162">Gdy *app_offline.htm* plik jest obecny, moduł platformy ASP.NET Core odpowiada na żądania odsyła zawartość *app_offline.htm* pliku.</span><span class="sxs-lookup"><span data-stu-id="c382d-162">While the *app_offline.htm* file is present, the ASP.NET Core Module responds to requests by sending back the contents of the *app_offline.htm* file.</span></span> <span data-ttu-id="c382d-163">Gdy *app_offline.htm* plik zostanie usunięty, przy następnym żądaniu uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c382d-163">When the *app_offline.htm* file is removed, the next request starts the app.</span></span>

## <a name="start-up-error-page"></a><span data-ttu-id="c382d-164">Strona błędu uruchamiania</span><span class="sxs-lookup"><span data-stu-id="c382d-164">Start-up error page</span></span>

<span data-ttu-id="c382d-165">Jeśli moduł platformy ASP.NET Core nie można uruchomić procesu zaplecza lub uruchamia proces wewnętrznej bazy danych, ale nie może nasłuchiwać na porcie skonfigurowanym *502.5 awarii procesu* zostanie wyświetlona strona kodu stanu.</span><span class="sxs-lookup"><span data-stu-id="c382d-165">If the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 Process Failure* status code page appears.</span></span> <span data-ttu-id="c382d-166">Aby pominąć tę stronę i przywrócić domyślną stronę kodową stan 502 usług IIS, należy użyć `disableStartUpErrorPage` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="c382d-166">To suppress this page and revert to the default IIS 502 status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="c382d-167">Aby uzyskać więcej informacji na temat konfigurowania niestandardowych komunikatów o błędach, zobacz [błędy HTTP `<httpErrors>` ](/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="c382d-167">For more information on configuring custom error messages, see [HTTP Errors `<httpErrors>`](/iis/configuration/system.webServer/httpErrors/).</span></span>

![502.5 strona kodowa stan niepowodzenia procesu](aspnet-core-module/_static/ANCM-502_5.png)

## <a name="log-creation-and-redirection"></a><span data-ttu-id="c382d-169">Tworzenie dziennika i Przekierowanie</span><span class="sxs-lookup"><span data-stu-id="c382d-169">Log creation and redirection</span></span>

<span data-ttu-id="c382d-170">Moduł platformy ASP.NET Core przekierowuje `stdout` i `stderr` dzienniki na dysku, jeśli `stdoutLogEnabled` i `stdoutLogFile` atrybuty `aspNetCore` ustawiono element.</span><span class="sxs-lookup"><span data-stu-id="c382d-170">The ASP.NET Core Module redirects `stdout` and `stderr` logs to disk if the `stdoutLogEnabled` and `stdoutLogFile` attributes of the `aspNetCore` element are set.</span></span> <span data-ttu-id="c382d-171">Wszystkie foldery w `stdoutLogFile` ścieżka musi istnieć w kolejności dla modułu utworzyć plik dziennika.</span><span class="sxs-lookup"><span data-stu-id="c382d-171">Any folders in the `stdoutLogFile` path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="c382d-172">Rozszerzenia znaczników czasu i pliku są dodawane automatycznie podczas tworzenia pliku dziennika.</span><span class="sxs-lookup"><span data-stu-id="c382d-172">A timestamp and file extension are added automatically when the log file is created.</span></span> <span data-ttu-id="c382d-173">Dzienniki nie są obracane, chyba że występuje procesu recykling/ponowne uruchomienie.</span><span class="sxs-lookup"><span data-stu-id="c382d-173">Logs aren't rotated, unless process recycling/restart occurs.</span></span> <span data-ttu-id="c382d-174">Jest odpowiedzialny za dostawcy usług hostingowych, aby ograniczyć miejsce na dysku korzystać z dzienników.</span><span class="sxs-lookup"><span data-stu-id="c382d-174">It's the responsibility of the hoster to limit the disk space the logs consume.</span></span> <span data-ttu-id="c382d-175">Przy użyciu `stdout` dziennika jest zalecane tylko dla rozwiązywania problemów uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c382d-175">Using the `stdout` log is only recommended for troubleshooting app startup issues.</span></span> <span data-ttu-id="c382d-176">Dziennik stdout nie jest używany do rejestrowania aplikacji ogólnych celów.</span><span class="sxs-lookup"><span data-stu-id="c382d-176">Don't use the stdout log for general app logging purposes.</span></span> <span data-ttu-id="c382d-177">Użyć biblioteki rejestrowania, który ogranicza rozmiar pliku dziennika i dzienników obraca rutynowych rejestrowania w aplikacji platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c382d-177">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="c382d-178">Aby uzyskać więcej informacji, zobacz [dostawców innych firm rejestrowania](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="c382d-178">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

<span data-ttu-id="c382d-179">Nazwa pliku dziennika składa się przez dodanie sygnatury czasowej ID procesu i rozszerzenie pliku (*log*) do ostatni segment `stdoutLogFile` ścieżki (zazwyczaj *stdout*) rozdzielane znakami podkreślenia.</span><span class="sxs-lookup"><span data-stu-id="c382d-179">The log file name is composed by appending the timestamp, process ID, and file extension (*.log*) to the last segment of the `stdoutLogFile` path (typically *stdout*) delimited by underscores.</span></span> <span data-ttu-id="c382d-180">Jeśli `stdoutLogFile` ścieżka kończy się *stdout*, nazwa pliku ma dziennika dla aplikacji za pomocą identyfikatora PID 1934 utworzony na 2/5/2018 w 19:42:32 *stdout_20180205194132_1934.log*.</span><span class="sxs-lookup"><span data-stu-id="c382d-180">If the `stdoutLogFile` path ends with *stdout*, a log for an app with a PID of 1934 created on 2/5/2018 at 19:42:32 has the file name *stdout_20180205194132_1934.log*.</span></span>

<span data-ttu-id="c382d-181">Poniższy przykład `aspNetCore` konfiguruje element `stdout` rejestrowania dla aplikacji hostowanej w usłudze Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="c382d-181">The following sample `aspNetCore` element configures `stdout` logging for an app hosted in Azure App Service.</span></span> <span data-ttu-id="c382d-182">Ścieżkę lokalną lub ścieżkę do udziału sieciowego jest dopuszczalne do lokalnego logowania.</span><span class="sxs-lookup"><span data-stu-id="c382d-182">A local path or network share path is acceptable for local logging.</span></span> <span data-ttu-id="c382d-183">Upewnij się, że tożsamość puli aplikacji ma uprawnienia do zapisu w podanej ścieżce.</span><span class="sxs-lookup"><span data-stu-id="c382d-183">Confirm that the AppPool user identity has permission to write to the path provided.</span></span>

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout">
</aspNetCore>
```

<span data-ttu-id="c382d-184">Zobacz [konfiguracji z pliku web.config](#configuration-with-webconfig) przykład `aspNetCore` element *web.config* pliku.</span><span class="sxs-lookup"><span data-stu-id="c382d-184">See [Configuration with web.config](#configuration-with-webconfig) for an example of the `aspNetCore` element in the *web.config* file.</span></span>

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a><span data-ttu-id="c382d-185">Moduł platformy ASP.NET Core z programem IIS konfiguracji udostępnionej</span><span class="sxs-lookup"><span data-stu-id="c382d-185">ASP.NET Core Module with an IIS Shared Configuration</span></span>

<span data-ttu-id="c382d-186">Instalator platformy ASP.NET Core modułu jest uruchamiany z uprawnieniami **systemu** konta.</span><span class="sxs-lookup"><span data-stu-id="c382d-186">The ASP.NET Core Module installer runs with the privileges of the **SYSTEM** account.</span></span> <span data-ttu-id="c382d-187">Ponieważ lokalne konto systemowe nie ma uprawnienia do modyfikowania dla ścieżki udziału konfiguracji udostępnionej usług IIS, Instalator trafienia dostępu błąd podczas próby skonfigurowania ustawienia modułu w *applicationHost.config* w udziale.</span><span class="sxs-lookup"><span data-stu-id="c382d-187">Because the local system account doesn't have modify permission for the share path used by the IIS Shared Configuration, the installer hits an access denied error when attempting to configure the module settings in *applicationHost.config* on the share.</span></span> <span data-ttu-id="c382d-188">Podczas korzystania z konfiguracji udostępnionej usług IIS, wykonaj następujące kroki:</span><span class="sxs-lookup"><span data-stu-id="c382d-188">When using an IIS Shared Configuration, follow these steps:</span></span>

1. <span data-ttu-id="c382d-189">Wyłącz konfiguracji udostępnionej usług IIS.</span><span class="sxs-lookup"><span data-stu-id="c382d-189">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="c382d-190">Uruchom Instalatora.</span><span class="sxs-lookup"><span data-stu-id="c382d-190">Run the installer.</span></span>
1. <span data-ttu-id="c382d-191">Eksportuj zaktualizowanego *applicationHost.config* plików do udziału.</span><span class="sxs-lookup"><span data-stu-id="c382d-191">Export the updated *applicationHost.config* file to the share.</span></span>
1. <span data-ttu-id="c382d-192">Ponownie włączyć konfiguracji udostępnionej usług IIS.</span><span class="sxs-lookup"><span data-stu-id="c382d-192">Re-enable the IIS Shared Configuration.</span></span>

## <a name="module-version-and-hosting-bundle-installer-logs"></a><span data-ttu-id="c382d-193">Wersja modułu oraz hosting dzienniki Instalatora pakietu</span><span class="sxs-lookup"><span data-stu-id="c382d-193">Module version and hosting bundle installer logs</span></span>

<span data-ttu-id="c382d-194">Aby określić wersję zainstalowanego modułu Core ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="c382d-194">To determine the version of the installed ASP.NET Core Module:</span></span>

1. <span data-ttu-id="c382d-195">W systemie hostingu, przejdź do *%windir%\System32\inetsrv*.</span><span class="sxs-lookup"><span data-stu-id="c382d-195">On the hosting system, navigate to *%windir%\System32\inetsrv*.</span></span>
1. <span data-ttu-id="c382d-196">Zlokalizuj *aspnetcore.dll* pliku.</span><span class="sxs-lookup"><span data-stu-id="c382d-196">Locate the *aspnetcore.dll* file.</span></span>
1. <span data-ttu-id="c382d-197">Kliknij prawym przyciskiem myszy plik i wybierz **właściwości** z menu kontekstowego.</span><span class="sxs-lookup"><span data-stu-id="c382d-197">Right-click the file and select **Properties** from the contextual menu.</span></span>
1. <span data-ttu-id="c382d-198">Wybierz **szczegóły** kartę. **Wersja pliku** i **wersji produktu** reprezentują zainstalowanej wersji modułu.</span><span class="sxs-lookup"><span data-stu-id="c382d-198">Select the **Details** tab. The **File version** and **Product version** represent the installed version of the module.</span></span>

<span data-ttu-id="c382d-199">Dzienniki Instalatora pakietu systemu Windows serwer obsługujący modułu znajdują się w *C:\\użytkowników\\% UserName %\\AppData\\lokalnego\\Temp*. Plik ma nazwę *dd_DotNetCoreWinSvrHosting__\<sygnatury czasowej > _000_AspNetCoreModule_x64.log*.</span><span class="sxs-lookup"><span data-stu-id="c382d-199">The Windows Server Hosting bundle installer logs for the module are found at *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. The file is named *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span></span>

## <a name="module-schema-and-configuration-file-locations"></a><span data-ttu-id="c382d-200">Moduł, schematów i konfiguracji lokalizacje plików</span><span class="sxs-lookup"><span data-stu-id="c382d-200">Module, schema, and configuration file locations</span></span>

### <a name="module"></a><span data-ttu-id="c382d-201">Moduł</span><span class="sxs-lookup"><span data-stu-id="c382d-201">Module</span></span>

<span data-ttu-id="c382d-202">**Usługi IIS (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="c382d-202">**IIS (x86/amd64):**</span></span>

   * <span data-ttu-id="c382d-203">%windir%\System32\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="c382d-203">%windir%\System32\inetsrv\aspnetcore.dll</span></span>

   * <span data-ttu-id="c382d-204">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="c382d-204">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span></span>

<span data-ttu-id="c382d-205">**Usługi IIS Express (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="c382d-205">**IIS Express (x86/amd64):**</span></span>

   * <span data-ttu-id="c382d-206">%ProgramFiles%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="c382d-206">%ProgramFiles%\IIS Express\aspnetcore.dll</span></span>

   * <span data-ttu-id="c382d-207">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="c382d-207">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span></span>

### <a name="schema"></a><span data-ttu-id="c382d-208">Schemat</span><span class="sxs-lookup"><span data-stu-id="c382d-208">Schema</span></span>

<span data-ttu-id="c382d-209">**IIS**</span><span class="sxs-lookup"><span data-stu-id="c382d-209">**IIS**</span></span>

   * <span data-ttu-id="c382d-210">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="c382d-210">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span></span>

<span data-ttu-id="c382d-211">**Usługi IIS Express**</span><span class="sxs-lookup"><span data-stu-id="c382d-211">**IIS Express**</span></span>

   * <span data-ttu-id="c382d-212">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="c382d-212">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span></span>

### <a name="configuration"></a><span data-ttu-id="c382d-213">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="c382d-213">Configuration</span></span>

<span data-ttu-id="c382d-214">**IIS**</span><span class="sxs-lookup"><span data-stu-id="c382d-214">**IIS**</span></span>

   * <span data-ttu-id="c382d-215">%windir%\System32\inetsrv\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="c382d-215">%windir%\System32\inetsrv\config\applicationHost.config</span></span>

<span data-ttu-id="c382d-216">**Usługi IIS Express**</span><span class="sxs-lookup"><span data-stu-id="c382d-216">**IIS Express**</span></span>

   * <span data-ttu-id="c382d-217">.vs\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="c382d-217">.vs\config\applicationHost.config</span></span>

<span data-ttu-id="c382d-218">Pliki można znaleźć, wyszukując *aspnetcore.dll* w *applicationHost.config* pliku.</span><span class="sxs-lookup"><span data-stu-id="c382d-218">The files can be found by searching for *aspnetcore.dll* in the *applicationHost.config* file.</span></span> <span data-ttu-id="c382d-219">Dla usług IIS Express *applicationHost.config* plik nie istnieje domyślnie.</span><span class="sxs-lookup"><span data-stu-id="c382d-219">For IIS Express, the *applicationHost.config* file won't exist by default.</span></span> <span data-ttu-id="c382d-220">Plik jest tworzony w  *\<application_root >\\.vs\\config* podczas uruchamiania żadnego projektu aplikacji sieci web w rozwiązaniu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c382d-220">The file is created at *\<application_root>\\.vs\\config* when starting any web app project in the Visual Studio solution.</span></span>
