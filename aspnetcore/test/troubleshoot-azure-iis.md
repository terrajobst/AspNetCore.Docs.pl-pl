---
title: Rozwiązywanie problemów ASP.NET Core na Azure App Service i usługach IIS
author: guardrex
description: Dowiedz się, jak zdiagnozować problemy z wdrożeniami Azure App Service i Internet Information Services (IIS) ASP.NET Core aplikacji.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 07/17/2019
uid: test/troubleshoot-azure-iis
ms.openlocfilehash: 46d4a11c594844e059fa8555255d7321f7b48631
ms.sourcegitcommit: b40613c603d6f0cc71f3232c16df61550907f550
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/18/2019
ms.locfileid: "68308822"
---
# <a name="troubleshoot-aspnet-core-on-azure-app-service-and-iis"></a><span data-ttu-id="b6237-103">Rozwiązywanie problemów ASP.NET Core na Azure App Service i usługach IIS</span><span class="sxs-lookup"><span data-stu-id="b6237-103">Troubleshoot ASP.NET Core on Azure App Service and IIS</span></span>

<span data-ttu-id="b6237-104">Autorzy [Luke Latham](https://github.com/guardrex) i [Justin Kotalik](https://github.com/jkotalik)</span><span class="sxs-lookup"><span data-stu-id="b6237-104">By [Luke Latham](https://github.com/guardrex) and [Justin Kotalik](https://github.com/jkotalik)</span></span>

<span data-ttu-id="b6237-105">Ten artykuł zawiera informacje dotyczące typowych błędów uruchamiania aplikacji oraz instrukcje dotyczące sposobu diagnozowania błędów podczas wdrażania aplikacji w usłudze Azure App Service lub IIS:</span><span class="sxs-lookup"><span data-stu-id="b6237-105">This article provides information on common app startup errors and instructions on how to diagnose errors when an app is deployed to Azure App Service or IIS:</span></span>

[<span data-ttu-id="b6237-106">Błędy uruchamiania aplikacji</span><span class="sxs-lookup"><span data-stu-id="b6237-106">App startup errors</span></span>](#app-startup-errors)  
<span data-ttu-id="b6237-107">Objaśnia typowe scenariusze uruchamiania kodu stanu HTTP.</span><span class="sxs-lookup"><span data-stu-id="b6237-107">Explains common startup HTTP status code scenarios.</span></span>

[<span data-ttu-id="b6237-108">Rozwiązywanie problemów dotyczących Azure App Service</span><span class="sxs-lookup"><span data-stu-id="b6237-108">Troubleshoot on Azure App Service</span></span>](#troubleshoot-on-azure-app-service)  
<span data-ttu-id="b6237-109">Zawiera porady dotyczące rozwiązywania problemów z aplikacjami wdrożonymi w celu Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="b6237-109">Provides troubleshooting advice for apps deployed to Azure App Service.</span></span>

[<span data-ttu-id="b6237-110">Rozwiązywanie problemów w usługach IIS</span><span class="sxs-lookup"><span data-stu-id="b6237-110">Troubleshoot on IIS</span></span>](#troubleshoot-on-iis)  
<span data-ttu-id="b6237-111">Zawiera porady dotyczące rozwiązywania problemów z aplikacjami wdrożonymi w usługach IIS lub lokalnie uruchomionymi IIS Express.</span><span class="sxs-lookup"><span data-stu-id="b6237-111">Provides troubleshooting advice for apps deployed to IIS or running on IIS Express locally.</span></span> <span data-ttu-id="b6237-112">Wskazówki dotyczą zarówno wdrożeń systemu Windows Server, jak i pulpitu systemu Windows.</span><span class="sxs-lookup"><span data-stu-id="b6237-112">The guidance applies to both Windows Server and Windows desktop deployments.</span></span>

[<span data-ttu-id="b6237-113">Wyczyść pamięć podręczną pakietów</span><span class="sxs-lookup"><span data-stu-id="b6237-113">Clear package caches</span></span>](#clear-package-caches)  
<span data-ttu-id="b6237-114">Wyjaśnia, co należy zrobić, gdy niespójne pakiety przerywają działanie aplikacji podczas przeprowadzania uaktualnień głównych lub zmiany wersji pakietu.</span><span class="sxs-lookup"><span data-stu-id="b6237-114">Explains what to do when incoherent packages break an app when performing major upgrades or changing package versions.</span></span>

[<span data-ttu-id="b6237-115">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="b6237-115">Additional resources</span></span>](#additional-resources)  
<span data-ttu-id="b6237-116">Wyświetla listę dodatkowych tematów dotyczących rozwiązywania problemów.</span><span class="sxs-lookup"><span data-stu-id="b6237-116">Lists additional troubleshooting topics.</span></span>

## <a name="app-startup-errors"></a><span data-ttu-id="b6237-117">Błędy uruchamiania aplikacji</span><span class="sxs-lookup"><span data-stu-id="b6237-117">App startup errors</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="b6237-118">W programie Visual Studio ma domyślnie wartość projektu ASP.NET Core [usług IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hostingu podczas debugowania.</span><span class="sxs-lookup"><span data-stu-id="b6237-118">In Visual Studio, an ASP.NET Core project defaults to [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hosting during debugging.</span></span> <span data-ttu-id="b6237-119">*502,5 — błąd procesu* lub *błąd uruchomienia 500,30* , który występuje, gdy debugowanie lokalne można zdiagnozować przy użyciu porady w tym temacie.</span><span class="sxs-lookup"><span data-stu-id="b6237-119">A *502.5 - Process Failure* or a *500.30 - Start Failure* that occurs when debugging locally can be diagnosed using the advice in this topic.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="b6237-120">W programie Visual Studio ma domyślnie wartość projektu ASP.NET Core [usług IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hostingu podczas debugowania.</span><span class="sxs-lookup"><span data-stu-id="b6237-120">In Visual Studio, an ASP.NET Core project defaults to [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hosting during debugging.</span></span> <span data-ttu-id="b6237-121">*Błąd procesu 502,5* , który występuje, gdy debugowanie lokalne można zdiagnozować przy użyciu porady w tym temacie.</span><span class="sxs-lookup"><span data-stu-id="b6237-121">A *502.5 Process Failure* that occurs when debugging locally can be diagnosed using the advice in this topic.</span></span>

::: moniker-end

### <a name="500-internal-server-error"></a><span data-ttu-id="b6237-122">500 Wewnętrzny błąd serwera</span><span class="sxs-lookup"><span data-stu-id="b6237-122">500 Internal Server Error</span></span>

<span data-ttu-id="b6237-123">Uruchamia aplikację, ale błąd uniemożliwia spełnienie żądania przez serwer.</span><span class="sxs-lookup"><span data-stu-id="b6237-123">The app starts, but an error prevents the server from fulfilling the request.</span></span>

<span data-ttu-id="b6237-124">Ten błąd występuje w kodzie aplikacji, podczas uruchamiania lub podczas tworzenia odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="b6237-124">This error occurs within the app's code during startup or while creating a response.</span></span> <span data-ttu-id="b6237-125">Odpowiedź może zawierać żadnej zawartości lub odpowiedzi może być wyświetlana jako *500 Wewnętrzny błąd serwera* w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="b6237-125">The response may contain no content, or the response may appear as a *500 Internal Server Error* in the browser.</span></span> <span data-ttu-id="b6237-126">W dzienniku zdarzeń aplikacji stwierdza, zwykle uruchomiona aplikacja.</span><span class="sxs-lookup"><span data-stu-id="b6237-126">The Application Event Log usually states that the app started normally.</span></span> <span data-ttu-id="b6237-127">Z perspektywy serwera, który jest poprawna.</span><span class="sxs-lookup"><span data-stu-id="b6237-127">From the server's perspective, that's correct.</span></span> <span data-ttu-id="b6237-128">Aplikacja została uruchomiona, ale nie może wygenerować prawidłowej odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="b6237-128">The app did start, but it can't generate a valid response.</span></span> <span data-ttu-id="b6237-129">Uruchom aplikację w wierszu polecenia na serwerze lub Włącz dziennik stdout modułu ASP.NET Core, aby rozwiązać problem.</span><span class="sxs-lookup"><span data-stu-id="b6237-129">Run the app at a command prompt on the server or enable the ASP.NET Core Module stdout log to troubleshoot the problem.</span></span>

::: moniker range="= aspnetcore-2.2"

### <a name="5000-in-process-handler-load-failure"></a><span data-ttu-id="b6237-130">500.0 w procesie programu obsługi błędu ładowania</span><span class="sxs-lookup"><span data-stu-id="b6237-130">500.0 In-Process Handler Load Failure</span></span>

<span data-ttu-id="b6237-131">Proces roboczy kończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="b6237-131">The worker process fails.</span></span> <span data-ttu-id="b6237-132">Nie zaczyna się aplikacja.</span><span class="sxs-lookup"><span data-stu-id="b6237-132">The app doesn't start.</span></span>

<span data-ttu-id="b6237-133">[Moduł ASP.NET Core](xref:host-and-deploy/aspnet-core-module) nie może znaleźć platformy .NET Core CLR i znaleźć procedury obsługi żądań w procesie (*aspnetcorev2_inprocess. dll*).</span><span class="sxs-lookup"><span data-stu-id="b6237-133">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) fails to find the .NET Core CLR and find the in-process request handler (*aspnetcorev2_inprocess.dll*).</span></span> <span data-ttu-id="b6237-134">Sprawdź, czy:</span><span class="sxs-lookup"><span data-stu-id="b6237-134">Check that:</span></span>

* <span data-ttu-id="b6237-135">Aplikacja jest przeznaczona na albo [Microsoft.AspNetCore.Server.IIS](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IIS) pakietu NuGet lub [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="b6237-135">The app targets either the [Microsoft.AspNetCore.Server.IIS](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IIS) NuGet package or the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>
* <span data-ttu-id="b6237-136">Wersja udostępnionej platformy ASP.NET Core jest zainstalowanie aplikacji jest przeznaczony dla na komputerze docelowym.</span><span class="sxs-lookup"><span data-stu-id="b6237-136">The version of the ASP.NET Core shared framework that the app targets is installed on the target machine.</span></span>

### <a name="5000-out-of-process-handler-load-failure"></a><span data-ttu-id="b6237-137">500.0 Błąd ładowania poza procesem programu obsługi</span><span class="sxs-lookup"><span data-stu-id="b6237-137">500.0 Out-Of-Process Handler Load Failure</span></span>

<span data-ttu-id="b6237-138">Proces roboczy kończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="b6237-138">The worker process fails.</span></span> <span data-ttu-id="b6237-139">Nie zaczyna się aplikacja.</span><span class="sxs-lookup"><span data-stu-id="b6237-139">The app doesn't start.</span></span>

<span data-ttu-id="b6237-140">[Moduł ASP.NET Core](xref:host-and-deploy/aspnet-core-module) nie może odnaleźć procedury obsługi żądania hostingu poza procesem.</span><span class="sxs-lookup"><span data-stu-id="b6237-140">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) fails to find the out-of-process hosting request handler.</span></span> <span data-ttu-id="b6237-141">Upewnij się, że *aspnetcorev2_outofprocess.dll* znajduje się w podfolderze obok *aspnetcorev2.dll*.</span><span class="sxs-lookup"><span data-stu-id="b6237-141">Make sure the *aspnetcorev2_outofprocess.dll* is present in a subfolder next to *aspnetcorev2.dll*.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

### <a name="5000-in-process-handler-load-failure"></a><span data-ttu-id="b6237-142">500.0 w procesie programu obsługi błędu ładowania</span><span class="sxs-lookup"><span data-stu-id="b6237-142">500.0 In-Process Handler Load Failure</span></span>

<span data-ttu-id="b6237-143">Proces roboczy kończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="b6237-143">The worker process fails.</span></span> <span data-ttu-id="b6237-144">Nie zaczyna się aplikacja.</span><span class="sxs-lookup"><span data-stu-id="b6237-144">The app doesn't start.</span></span>

<span data-ttu-id="b6237-145">Wystąpił nieznany błąd podczas ładowania składników [modułu ASP.NET Core](xref:host-and-deploy/aspnet-core-module) .</span><span class="sxs-lookup"><span data-stu-id="b6237-145">An unknown error occurred loading [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) components.</span></span> <span data-ttu-id="b6237-146">Wykonaj jedną z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="b6237-146">Take one of the following actions:</span></span>

* <span data-ttu-id="b6237-147">[Pomoc techniczna firmy Microsoft](https://support.microsoft.com/oas/default.aspx?prid=15832) kontaktu (wybierz **Narzędzia deweloperskie** następnie **ASP.NET Core**).</span><span class="sxs-lookup"><span data-stu-id="b6237-147">Contact [Microsoft Support](https://support.microsoft.com/oas/default.aspx?prid=15832) (select **Developer Tools** then **ASP.NET Core**).</span></span>
* <span data-ttu-id="b6237-148">Zadawaj pytanie na Stack Overflow.</span><span class="sxs-lookup"><span data-stu-id="b6237-148">Ask a question on Stack Overflow.</span></span>
* <span data-ttu-id="b6237-149">Zajrzyj do problemu w naszym [repozytorium GitHub](https://github.com/aspnet/AspNetCore).</span><span class="sxs-lookup"><span data-stu-id="b6237-149">File an issue on our [GitHub repository](https://github.com/aspnet/AspNetCore).</span></span>

### <a name="50030-in-process-startup-failure"></a><span data-ttu-id="b6237-150">500.30 w procesie Niepowodzenie uruchamiania</span><span class="sxs-lookup"><span data-stu-id="b6237-150">500.30 In-Process Startup Failure</span></span>

<span data-ttu-id="b6237-151">Proces roboczy kończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="b6237-151">The worker process fails.</span></span> <span data-ttu-id="b6237-152">Nie zaczyna się aplikacja.</span><span class="sxs-lookup"><span data-stu-id="b6237-152">The app doesn't start.</span></span>

<span data-ttu-id="b6237-153">[Moduł ASP.NET Core](xref:host-and-deploy/aspnet-core-module) próbuje uruchomić program .NET Core CLR w procesie, ale nie można go uruchomić.</span><span class="sxs-lookup"><span data-stu-id="b6237-153">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) attempts to start the .NET Core CLR in-process, but it fails to start.</span></span> <span data-ttu-id="b6237-154">Przyczyna niepowodzenia uruchomienia procesu zwykle można ustalić na podstawie wpisów w dzienniku zdarzeń aplikacji i dzienniku modułu ASP.NET Core stdout.</span><span class="sxs-lookup"><span data-stu-id="b6237-154">The cause of a process startup failure can usually be determined from entries in the Application Event Log and the ASP.NET Core Module stdout log.</span></span>

<span data-ttu-id="b6237-155">Aplikacja jest błędnie skonfigurowane z powodu przeznaczony dla wersji udostępnionej platformy ASP.NET Core, która nie jest obecny jest jakiś wspólny warunek błędu.</span><span class="sxs-lookup"><span data-stu-id="b6237-155">A common failure condition is the app is misconfigured due to targeting a version of the ASP.NET Core shared framework that isn't present.</span></span> <span data-ttu-id="b6237-156">Sprawdź, które wersje udostępnionej platformy ASP.NET Core są zainstalowane na komputerze docelowym.</span><span class="sxs-lookup"><span data-stu-id="b6237-156">Check which versions of the ASP.NET Core shared framework are installed on the target machine.</span></span>

### <a name="50031-ancm-failed-to-find-native-dependencies"></a><span data-ttu-id="b6237-157">500,31 ANCM nie może odnaleźć natywnych zależności</span><span class="sxs-lookup"><span data-stu-id="b6237-157">500.31 ANCM Failed to Find Native Dependencies</span></span>

<span data-ttu-id="b6237-158">Proces roboczy kończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="b6237-158">The worker process fails.</span></span> <span data-ttu-id="b6237-159">Nie zaczyna się aplikacja.</span><span class="sxs-lookup"><span data-stu-id="b6237-159">The app doesn't start.</span></span>

<span data-ttu-id="b6237-160">[Moduł ASP.NET Core](xref:host-and-deploy/aspnet-core-module) próbuje uruchomić środowisko uruchomieniowe programu .NET Core w procesie, ale nie może go uruchomić.</span><span class="sxs-lookup"><span data-stu-id="b6237-160">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) attempts to start the .NET Core runtime in-process, but it fails to start.</span></span> <span data-ttu-id="b6237-161">Najczęstszym powodem tego błędu uruchomienia jest to, że `Microsoft.NETCore.App` środowisko `Microsoft.AspNetCore.App` uruchomieniowe lub nie jest zainstalowane.</span><span class="sxs-lookup"><span data-stu-id="b6237-161">The most common cause of this startup failure is when the `Microsoft.NETCore.App` or `Microsoft.AspNetCore.App` runtime isn't installed.</span></span> <span data-ttu-id="b6237-162">Jeśli aplikacja jest wdrożona w programie docelowym ASP.NET Core 3,0 i ta wersja nie istnieje na maszynie, wystąpi błąd.</span><span class="sxs-lookup"><span data-stu-id="b6237-162">If the app is deployed to target ASP.NET Core 3.0 and that version doesn't exist on the machine, this error occurs.</span></span> <span data-ttu-id="b6237-163">Poniżej znajduje się przykładowy komunikat o błędzie:</span><span class="sxs-lookup"><span data-stu-id="b6237-163">An example error message follows:</span></span>

```
The specified framework 'Microsoft.NETCore.App', version '3.0.0' was not found.
  - The following frameworks were found:
      2.2.1 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview5-27626-15 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview6-27713-13 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview6-27714-15 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview6-27723-08 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
```

<span data-ttu-id="b6237-164">W komunikacie o błędzie są wyświetlane wszystkie zainstalowane wersje programu .NET Core i wersja żądana przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="b6237-164">The error message lists all the installed .NET Core versions and the version requested by the app.</span></span> <span data-ttu-id="b6237-165">Aby naprawić ten błąd, należy:</span><span class="sxs-lookup"><span data-stu-id="b6237-165">To fix this error, either:</span></span>

* <span data-ttu-id="b6237-166">Zainstaluj na komputerze odpowiednią wersję programu .NET Core.</span><span class="sxs-lookup"><span data-stu-id="b6237-166">Install the appropriate version of .NET Core on the machine.</span></span>
* <span data-ttu-id="b6237-167">Zmień aplikację na wersję docelową platformy .NET Core, która jest obecna na maszynie.</span><span class="sxs-lookup"><span data-stu-id="b6237-167">Change the app to target a version of .NET Core that's present on the machine.</span></span>
* <span data-ttu-id="b6237-168">Publikowanie aplikacji jako samodzielnego [wdrożenia](/dotnet/core/deploying/#self-contained-deployments-scd).</span><span class="sxs-lookup"><span data-stu-id="b6237-168">Publish the app as a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd).</span></span>

<span data-ttu-id="b6237-169">Podczas pracy w `ASPNETCORE_ENVIRONMENT` środowisku deweloperskim (zmienna środowiskowa jest `Development`ustawiona na) określony błąd jest zapisywana w odpowiedzi HTTP.</span><span class="sxs-lookup"><span data-stu-id="b6237-169">When running in development (the `ASPNETCORE_ENVIRONMENT` environment variable is set to `Development`), the specific error is written to the HTTP response.</span></span> <span data-ttu-id="b6237-170">Przyczyna niepowodzenia uruchomienia procesu znajduje się również w dzienniku zdarzeń aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b6237-170">The cause of a process startup failure is also found in the Application Event Log.</span></span>

### <a name="50032-ancm-failed-to-load-dll"></a><span data-ttu-id="b6237-171">500,32 ANCM nie może załadować biblioteki DLL</span><span class="sxs-lookup"><span data-stu-id="b6237-171">500.32 ANCM Failed to Load dll</span></span>

<span data-ttu-id="b6237-172">Proces roboczy kończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="b6237-172">The worker process fails.</span></span> <span data-ttu-id="b6237-173">Nie zaczyna się aplikacja.</span><span class="sxs-lookup"><span data-stu-id="b6237-173">The app doesn't start.</span></span>

<span data-ttu-id="b6237-174">Najbardziej typową przyczyną tego błędu jest to, że aplikacja jest publikowana dla niezgodnej architektury procesora.</span><span class="sxs-lookup"><span data-stu-id="b6237-174">The most common cause for this error is that the app is published for an incompatible processor architecture.</span></span> <span data-ttu-id="b6237-175">Jeśli proces roboczy jest uruchomiony jako aplikacja 32-bitowa, a aplikacja została opublikowana w docelowym 64-bitowym, ten błąd wystąpi.</span><span class="sxs-lookup"><span data-stu-id="b6237-175">If the worker process is running as a 32-bit app and the app was published to target 64-bit, this error occurs.</span></span>

<span data-ttu-id="b6237-176">Aby naprawić ten błąd, należy:</span><span class="sxs-lookup"><span data-stu-id="b6237-176">To fix this error, either:</span></span>

* <span data-ttu-id="b6237-177">Opublikuj ponownie aplikację dla tej samej architektury procesora co proces roboczy.</span><span class="sxs-lookup"><span data-stu-id="b6237-177">Republish the app for the same processor architecture as the worker process.</span></span>
* <span data-ttu-id="b6237-178">Opublikuj aplikację jako [wdrożenie zależne od platformy](/dotnet/core/deploying/#framework-dependent-executables-fde).</span><span class="sxs-lookup"><span data-stu-id="b6237-178">Publish the app as a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-executables-fde).</span></span>

### <a name="50033-ancm-request-handler-load-failure"></a><span data-ttu-id="b6237-179">500,33 błąd ładowania obsługi żądania ANCM</span><span class="sxs-lookup"><span data-stu-id="b6237-179">500.33 ANCM Request Handler Load Failure</span></span>

<span data-ttu-id="b6237-180">Proces roboczy kończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="b6237-180">The worker process fails.</span></span> <span data-ttu-id="b6237-181">Nie zaczyna się aplikacja.</span><span class="sxs-lookup"><span data-stu-id="b6237-181">The app doesn't start.</span></span>

<span data-ttu-id="b6237-182">Aplikacja nie odwołuje się `Microsoft.AspNetCore.App` do struktury.</span><span class="sxs-lookup"><span data-stu-id="b6237-182">The app didn't reference the `Microsoft.AspNetCore.App` framework.</span></span> <span data-ttu-id="b6237-183">[Moduł ASP.NET Core](xref:host-and-deploy/aspnet-core-module)może obsługiwać `Microsoft.AspNetCore.App` tylko aplikacje ukierunkowane na platformę.</span><span class="sxs-lookup"><span data-stu-id="b6237-183">Only apps targeting the `Microsoft.AspNetCore.App` framework can be hosted by the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module).</span></span>

<span data-ttu-id="b6237-184">Aby naprawić ten błąd, upewnij się, że aplikacja jest ukierunkowana na `Microsoft.AspNetCore.App` platformę.</span><span class="sxs-lookup"><span data-stu-id="b6237-184">To fix this error, confirm that the app is targeting the `Microsoft.AspNetCore.App` framework.</span></span> <span data-ttu-id="b6237-185">Sprawdź, `.runtimeconfig.json` czy w celu zweryfikowania struktury wskazywanej przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="b6237-185">Check the `.runtimeconfig.json` to verify the framework targeted by the app.</span></span>

### <a name="50034-ancm-mixed-hosting-models-not-supported"></a><span data-ttu-id="b6237-186">500,34 ANCM mieszane modele hostingu nie są obsługiwane</span><span class="sxs-lookup"><span data-stu-id="b6237-186">500.34 ANCM Mixed Hosting Models Not Supported</span></span>

<span data-ttu-id="b6237-187">Proces roboczy nie może uruchomić zarówno aplikacji w procesie, jak i aplikacji pozaprocesowej w tym samym procesie.</span><span class="sxs-lookup"><span data-stu-id="b6237-187">The worker process can't run both an in-process app and an out-of-process app in the same process.</span></span>

<span data-ttu-id="b6237-188">Aby naprawić ten błąd, uruchom aplikacje w osobnych pulach aplikacji usług IIS.</span><span class="sxs-lookup"><span data-stu-id="b6237-188">To fix this error, run apps in separate IIS application pools.</span></span>

### <a name="50035-ancm-multiple-in-process-applications-in-same-process"></a><span data-ttu-id="b6237-189">500,35 ANCM wiele aplikacji w procesie w tym samym procesie</span><span class="sxs-lookup"><span data-stu-id="b6237-189">500.35 ANCM Multiple In-Process Applications in same Process</span></span>

<span data-ttu-id="b6237-190">Proces roboczy nie może uruchomić zarówno aplikacji w procesie, jak i aplikacji pozaprocesowej w tym samym procesie.</span><span class="sxs-lookup"><span data-stu-id="b6237-190">The worker process can't run both an in-process app and an out-of-process app in the same process.</span></span>

<span data-ttu-id="b6237-191">Aby naprawić ten błąd, uruchom aplikacje w osobnych pulach aplikacji usług IIS.</span><span class="sxs-lookup"><span data-stu-id="b6237-191">To fix this error, run apps in separate IIS application pools.</span></span>

### <a name="50036-ancm-out-of-process-handler-load-failure"></a><span data-ttu-id="b6237-192">500,36 ANCM błąd ładowania procedury obsługi poza procesem</span><span class="sxs-lookup"><span data-stu-id="b6237-192">500.36 ANCM Out-Of-Process Handler Load Failure</span></span>

<span data-ttu-id="b6237-193">Procedura obsługi żądań poza procesem, *aspnetcorev2_outofprocess. dll*, nie jest obok pliku *aspnetcorev2. dll* .</span><span class="sxs-lookup"><span data-stu-id="b6237-193">The out-of-process request handler, *aspnetcorev2_outofprocess.dll*, isn't next to the *aspnetcorev2.dll* file.</span></span> <span data-ttu-id="b6237-194">Oznacza to uszkodzenie instalacji [modułu ASP.NET Core](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="b6237-194">This indicates a corrupted installation of the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module).</span></span>

<span data-ttu-id="b6237-195">Aby naprawić ten błąd, napraw instalację [pakietu hostingu platformy .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle) (dla usług IIS) lub programu Visual Studio (w przypadku IIS Express).</span><span class="sxs-lookup"><span data-stu-id="b6237-195">To fix this error, repair the installation of the [.NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle) (for IIS) or Visual Studio (for IIS Express).</span></span>

### <a name="50037-ancm-failed-to-start-within-startup-time-limit"></a><span data-ttu-id="b6237-196">Nie można uruchomić 500,37 ANCM w ramach limitu czasu uruchamiania</span><span class="sxs-lookup"><span data-stu-id="b6237-196">500.37 ANCM Failed to Start Within Startup Time Limit</span></span>

<span data-ttu-id="b6237-197">Nie można uruchomić ANCM w limicie czasu uruchamiania dostarczają.</span><span class="sxs-lookup"><span data-stu-id="b6237-197">ANCM failed to start within the provied startup time limit.</span></span> <span data-ttu-id="b6237-198">Domyślnie limit czasu wynosi 120 sekund.</span><span class="sxs-lookup"><span data-stu-id="b6237-198">By default, the timeout is 120 seconds.</span></span>

<span data-ttu-id="b6237-199">Ten błąd może wystąpić podczas uruchamiania dużej liczby aplikacji na tym samym komputerze.</span><span class="sxs-lookup"><span data-stu-id="b6237-199">This error can occur when starting a large number of apps on the same machine.</span></span> <span data-ttu-id="b6237-200">Podczas uruchamiania Sprawdź, czy na serwerze są naskoki użycie procesora CPU/pamięci.</span><span class="sxs-lookup"><span data-stu-id="b6237-200">Check for CPU/Memory usage spikes on the server during startup.</span></span> <span data-ttu-id="b6237-201">Może być konieczne rozłożenie procesu uruchamiania wielu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b6237-201">You may need to stagger the startup process of multiple apps.</span></span>

::: moniker-end

### <a name="5025-process-failure"></a><span data-ttu-id="b6237-202">502.5 niepowodzenie procesu</span><span class="sxs-lookup"><span data-stu-id="b6237-202">502.5 Process Failure</span></span>

<span data-ttu-id="b6237-203">Proces roboczy kończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="b6237-203">The worker process fails.</span></span> <span data-ttu-id="b6237-204">Nie zaczyna się aplikacja.</span><span class="sxs-lookup"><span data-stu-id="b6237-204">The app doesn't start.</span></span>

<span data-ttu-id="b6237-205">[Moduł ASP.NET Core](xref:host-and-deploy/aspnet-core-module) próbuje uruchomić proces roboczy, ale jego uruchomienie nie powiedzie się.</span><span class="sxs-lookup"><span data-stu-id="b6237-205">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) attempts to start the worker process but it fails to start.</span></span> <span data-ttu-id="b6237-206">Przyczyna niepowodzenia uruchomienia procesu zwykle można ustalić na podstawie wpisów w dzienniku zdarzeń aplikacji i dzienniku modułu ASP.NET Core stdout.</span><span class="sxs-lookup"><span data-stu-id="b6237-206">The cause of a process startup failure can usually be determined from entries in the Application Event Log and the ASP.NET Core Module stdout log.</span></span>

<span data-ttu-id="b6237-207">Aplikacja jest błędnie skonfigurowane z powodu przeznaczony dla wersji udostępnionej platformy ASP.NET Core, która nie jest obecny jest jakiś wspólny warunek błędu.</span><span class="sxs-lookup"><span data-stu-id="b6237-207">A common failure condition is the app is misconfigured due to targeting a version of the ASP.NET Core shared framework that isn't present.</span></span> <span data-ttu-id="b6237-208">Sprawdź, które wersje udostępnionej platformy ASP.NET Core są zainstalowane na komputerze docelowym.</span><span class="sxs-lookup"><span data-stu-id="b6237-208">Check which versions of the ASP.NET Core shared framework are installed on the target machine.</span></span> <span data-ttu-id="b6237-209">*Platforma udostępniona* jest zestawem zestawów (plików*dll* ), które są zainstalowane na maszynie i do których odwołuje się `Microsoft.AspNetCore.App`pakiet.</span><span class="sxs-lookup"><span data-stu-id="b6237-209">The *shared framework* is the set of assemblies (*.dll* files) that are installed on the machine and referenced by a metapackage such as `Microsoft.AspNetCore.App`.</span></span> <span data-ttu-id="b6237-210">Odwołanie do pakietu nie może określać minimalnej wymaganej wersji.</span><span class="sxs-lookup"><span data-stu-id="b6237-210">The metapackage reference can specify a minimum required version.</span></span> <span data-ttu-id="b6237-211">Aby uzyskać więcej informacji, zobacz [udostępnioną strukturę](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/).</span><span class="sxs-lookup"><span data-stu-id="b6237-211">For more information, see [The shared framework](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/).</span></span>

<span data-ttu-id="b6237-212">*502.5 niepowodzenia procesu* strony błędu jest zwracany, jeśli aplikacji lub obsługującego błędnej konfiguracji powoduje niepowodzenie procesu roboczego:</span><span class="sxs-lookup"><span data-stu-id="b6237-212">The *502.5 Process Failure* error page is returned when a hosting or app misconfiguration causes the worker process to fail:</span></span>

### <a name="failed-to-start-application-errorcode-0x800700c1"></a><span data-ttu-id="b6237-213">Nie można uruchomić aplikację (kod błędu "0x800700c1")</span><span class="sxs-lookup"><span data-stu-id="b6237-213">Failed to start application (ErrorCode '0x800700c1')</span></span>

```
EventID: 1010
Source: IIS AspNetCore Module V2
Failed to start application '/LM/W3SVC/6/ROOT/', ErrorCode '0x800700c1'.
```

<span data-ttu-id="b6237-214">Aplikacji nie powiodło się, ponieważ zestaw aplikacji ( *.dll*) nie można go załadować.</span><span class="sxs-lookup"><span data-stu-id="b6237-214">The app failed to start because the app's assembly (*.dll*) couldn't be loaded.</span></span>

<span data-ttu-id="b6237-215">Ten błąd występuje, gdy występuje niezgodność liczby bitów opublikowanej aplikacji i procesu w3wp/programu iisexpress.</span><span class="sxs-lookup"><span data-stu-id="b6237-215">This error occurs when there's a bitness mismatch between the published app and the w3wp/iisexpress process.</span></span>

<span data-ttu-id="b6237-216">Upewnij się, że ustawienie 32-bitowych puli aplikacji jest prawidłowy:</span><span class="sxs-lookup"><span data-stu-id="b6237-216">Confirm that the app pool's 32-bit setting is correct:</span></span>

1. <span data-ttu-id="b6237-217">Wybierz pulę aplikacji w Menedżerze usług IIS w **pul aplikacji**.</span><span class="sxs-lookup"><span data-stu-id="b6237-217">Select the app pool in IIS Manager's **Application Pools**.</span></span>
1. <span data-ttu-id="b6237-218">Wybierz **Zaawansowane ustawienia** w obszarze **edytowanie puli aplikacji** w **akcje** panelu.</span><span class="sxs-lookup"><span data-stu-id="b6237-218">Select **Advanced Settings** under **Edit Application Pool** in the **Actions** panel.</span></span>
1. <span data-ttu-id="b6237-219">Ustaw **Włącz aplikacje 32-bitowe**:</span><span class="sxs-lookup"><span data-stu-id="b6237-219">Set **Enable 32-Bit Applications**:</span></span>
   * <span data-ttu-id="b6237-220">Jeśli wdrażanie (x86) 32-bitowych aplikacji, ustaw wartość `True`.</span><span class="sxs-lookup"><span data-stu-id="b6237-220">If deploying a 32-bit (x86) app, set the value to `True`.</span></span>
   * <span data-ttu-id="b6237-221">Jeśli wdrażanie (x64) 64-bitowych aplikacji, ustaw wartość `False`.</span><span class="sxs-lookup"><span data-stu-id="b6237-221">If deploying a 64-bit (x64) app, set the value to `False`.</span></span>

### <a name="connection-reset"></a><span data-ttu-id="b6237-222">Resetowanie połączenia</span><span class="sxs-lookup"><span data-stu-id="b6237-222">Connection reset</span></span>

<span data-ttu-id="b6237-223">Jeśli błąd wystąpi po nagłówki są wysyłane, jest za późno serwera wysłać **500 Wewnętrzny błąd serwera** po wystąpieniu błędu.</span><span class="sxs-lookup"><span data-stu-id="b6237-223">If an error occurs after the headers are sent, it's too late for the server to send a **500 Internal Server Error** when an error occurs.</span></span> <span data-ttu-id="b6237-224">Dzieje się tak często, gdy wystąpi błąd podczas serializacji obiektów złożonych na odpowiedź.</span><span class="sxs-lookup"><span data-stu-id="b6237-224">This often happens when an error occurs during the serialization of complex objects for a response.</span></span> <span data-ttu-id="b6237-225">Tego typu błędu jest wyświetlany jako *resetowania połączenia* błąd na komputerze klienckim.</span><span class="sxs-lookup"><span data-stu-id="b6237-225">This type of error appears as a *connection reset* error on the client.</span></span> <span data-ttu-id="b6237-226">[Rejestrowanie aplikacji](xref:fundamentals/logging/index) mogą pomóc rozwiązać tego rodzaju błędów.</span><span class="sxs-lookup"><span data-stu-id="b6237-226">[Application logging](xref:fundamentals/logging/index) can help troubleshoot these types of errors.</span></span>

### <a name="default-startup-limits"></a><span data-ttu-id="b6237-227">Domyślne limity uruchamiania</span><span class="sxs-lookup"><span data-stu-id="b6237-227">Default startup limits</span></span>

<span data-ttu-id="b6237-228">[Moduł ASP.NET Core](xref:host-and-deploy/aspnet-core-module) jest skonfigurowany z domyślną startupTimeLimitą  120 sekund.</span><span class="sxs-lookup"><span data-stu-id="b6237-228">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) is configured with a default *startupTimeLimit* of 120 seconds.</span></span> <span data-ttu-id="b6237-229">Gdy pozostawić wartość domyślną, aplikacja może potrwać do dwóch minut przed moduł dzienniki awarii procesu.</span><span class="sxs-lookup"><span data-stu-id="b6237-229">When left at the default value, an app may take up to two minutes to start before the module logs a process failure.</span></span> <span data-ttu-id="b6237-230">Aby uzyskać informacje na temat konfigurowania modułu, zobacz [atrybuty elementu aspNetCore](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span><span class="sxs-lookup"><span data-stu-id="b6237-230">For information on configuring the module, see [Attributes of the aspNetCore element](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span></span>

## <a name="troubleshoot-on-azure-app-service"></a><span data-ttu-id="b6237-231">Rozwiązywanie problemów dotyczących Azure App Service</span><span class="sxs-lookup"><span data-stu-id="b6237-231">Troubleshoot on Azure App Service</span></span>

[!INCLUDE [Azure App Service Preview Notice](~/includes/azure-apps-preview-notice.md)]

### <a name="application-event-log-azure-app-service"></a><span data-ttu-id="b6237-232">Dziennik zdarzeń aplikacji (Azure App Service)</span><span class="sxs-lookup"><span data-stu-id="b6237-232">Application Event Log (Azure App Service)</span></span>

<span data-ttu-id="b6237-233">Aby uzyskać dostęp do dziennika zdarzeń aplikacji, użyj bloku **diagnozowanie i rozwiązywanie problemów** w Azure Portal:</span><span class="sxs-lookup"><span data-stu-id="b6237-233">To access the Application Event Log, use the **Diagnose and solve problems** blade in the Azure portal:</span></span>

1. <span data-ttu-id="b6237-234">W Azure Portal Otwórz aplikację w **App Services**.</span><span class="sxs-lookup"><span data-stu-id="b6237-234">In the Azure portal, open the app in **App Services**.</span></span>
1. <span data-ttu-id="b6237-235">Wybierz pozycję **Diagnozuj i rozwiąż problemy**.</span><span class="sxs-lookup"><span data-stu-id="b6237-235">Select **Diagnose and solve problems**.</span></span>
1. <span data-ttu-id="b6237-236">Wybierz nagłówek **Narzędzia diagnostyczne** .</span><span class="sxs-lookup"><span data-stu-id="b6237-236">Select the **Diagnostic Tools** heading.</span></span>
1. <span data-ttu-id="b6237-237">W obszarze **Narzędzia obsługi**wybierz przycisk **zdarzenia aplikacji** .</span><span class="sxs-lookup"><span data-stu-id="b6237-237">Under **Support Tools**, select the **Application Events** button.</span></span>
1. <span data-ttu-id="b6237-238">Zapoznaj się z najnowszym błędem podanym w pozycji *AspNetCoreModule IIS* lub *IIS AspNetCoreModule v2* w kolumnie **Źródło** .</span><span class="sxs-lookup"><span data-stu-id="b6237-238">Examine the latest error provided by the *IIS AspNetCoreModule* or *IIS AspNetCoreModule V2* entry in the **Source** column.</span></span>

<span data-ttu-id="b6237-239">Alternatywą dla korzystania z bloku **diagnozowanie i rozwiązywanie problemów** jest przetestowanie pliku dziennika zdarzeń aplikacji bezpośrednio przy użyciu [kudu](https://github.com/projectkudu/kudu/wiki):</span><span class="sxs-lookup"><span data-stu-id="b6237-239">An alternative to using the **Diagnose and solve problems** blade is to examine the Application Event Log file directly using [Kudu](https://github.com/projectkudu/kudu/wiki):</span></span>

1. <span data-ttu-id="b6237-240">Otwórz **Narzędzia zaawansowane** w obszarze **Narzędzia programistyczne** .</span><span class="sxs-lookup"><span data-stu-id="b6237-240">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="b6237-241">Wybierz przycisk **Przejdź&rarr;**  .</span><span class="sxs-lookup"><span data-stu-id="b6237-241">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="b6237-242">Konsola kudu otwiera się w nowej karcie lub oknie przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="b6237-242">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="b6237-243">Korzystając z paska nawigacyjnego w górnej części strony, Otwórz **konsolę debugowanie** i wybierz polecenie **cmd**.</span><span class="sxs-lookup"><span data-stu-id="b6237-243">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="b6237-244">Otwórz folder **LogFiles** .</span><span class="sxs-lookup"><span data-stu-id="b6237-244">Open the **LogFiles** folder.</span></span>
1. <span data-ttu-id="b6237-245">Wybierz ikonę ołówka obok pliku *EventLog. XML* .</span><span class="sxs-lookup"><span data-stu-id="b6237-245">Select the pencil icon next to the *eventlog.xml* file.</span></span>
1. <span data-ttu-id="b6237-246">Przejrzyj dziennik.</span><span class="sxs-lookup"><span data-stu-id="b6237-246">Examine the log.</span></span> <span data-ttu-id="b6237-247">Przewiń w dół dziennika, aby zobaczyć najnowsze zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="b6237-247">Scroll to the bottom of the log to see the most recent events.</span></span>

### <a name="run-the-app-in-the-kudu-console"></a><span data-ttu-id="b6237-248">Uruchamianie aplikacji w konsoli kudu</span><span class="sxs-lookup"><span data-stu-id="b6237-248">Run the app in the Kudu console</span></span>

<span data-ttu-id="b6237-249">Wiele błędów uruchamiania przestaną generować przydatne informacje w dzienniku zdarzeń aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b6237-249">Many startup errors don't produce useful information in the Application Event Log.</span></span> <span data-ttu-id="b6237-250">Możesz uruchomić aplikację w konsoli zdalnego wykonywania [kudu](https://github.com/projectkudu/kudu/wiki) , aby wykryć błąd:</span><span class="sxs-lookup"><span data-stu-id="b6237-250">You can run the app in the [Kudu](https://github.com/projectkudu/kudu/wiki) Remote Execution Console to discover the error:</span></span>

1. <span data-ttu-id="b6237-251">Otwórz **Narzędzia zaawansowane** w obszarze **Narzędzia programistyczne** .</span><span class="sxs-lookup"><span data-stu-id="b6237-251">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="b6237-252">Wybierz przycisk **Przejdź&rarr;**  .</span><span class="sxs-lookup"><span data-stu-id="b6237-252">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="b6237-253">Konsola kudu otwiera się w nowej karcie lub oknie przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="b6237-253">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="b6237-254">Korzystając z paska nawigacyjnego w górnej części strony, Otwórz **konsolę debugowanie** i wybierz polecenie **cmd**.</span><span class="sxs-lookup"><span data-stu-id="b6237-254">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>

#### <a name="test-a-32-bit-x86-app"></a><span data-ttu-id="b6237-255">Testowanie aplikacji 32-bitowej (x86)</span><span class="sxs-lookup"><span data-stu-id="b6237-255">Test a 32-bit (x86) app</span></span>

<span data-ttu-id="b6237-256">**Bieżąca wersja**</span><span class="sxs-lookup"><span data-stu-id="b6237-256">**Current release**</span></span>

1. `cd d:\home\site\wwwroot`
1. <span data-ttu-id="b6237-257">Uruchom aplikację:</span><span class="sxs-lookup"><span data-stu-id="b6237-257">Run the app:</span></span>
   * <span data-ttu-id="b6237-258">Jeśli aplikacja jest [wdrożenia zależny od struktury](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span><span class="sxs-lookup"><span data-stu-id="b6237-258">If the app is a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>

     ```console
     dotnet .\{ASSEMBLY NAME}.dll
     ```

   * <span data-ttu-id="b6237-259">Jeśli aplikacja jest [niezależna wdrożenia](/dotnet/core/deploying/#self-contained-deployments-scd):</span><span class="sxs-lookup"><span data-stu-id="b6237-259">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>

     ```console
     {ASSEMBLY NAME}.exe
     ```

<span data-ttu-id="b6237-260">Dane wyjściowe konsoli z aplikacji, pokazujące błędy, są przekazywane do konsoli kudu.</span><span class="sxs-lookup"><span data-stu-id="b6237-260">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

<span data-ttu-id="b6237-261">**Wdrożenie zależne od platformy uruchomione w wersji zapoznawczej**</span><span class="sxs-lookup"><span data-stu-id="b6237-261">**Framework-dependent deployment running on a preview release**</span></span>

<span data-ttu-id="b6237-262">*Wymaga zainstalowania rozszerzenia witryny środowiska uruchomieniowego ASP.NET Core {VERSION} (x86).*</span><span class="sxs-lookup"><span data-stu-id="b6237-262">*Requires installing the ASP.NET Core {VERSION} (x86) Runtime site extension.*</span></span>

1. <span data-ttu-id="b6237-263">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x32`(`{X.Y}` to wersja środowiska uruchomieniowego)</span><span class="sxs-lookup"><span data-stu-id="b6237-263">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x32` (`{X.Y}` is the runtime version)</span></span>
1. <span data-ttu-id="b6237-264">Uruchom aplikację:`dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span><span class="sxs-lookup"><span data-stu-id="b6237-264">Run the app: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span></span>

<span data-ttu-id="b6237-265">Dane wyjściowe konsoli z aplikacji, pokazujące błędy, są przekazywane do konsoli kudu.</span><span class="sxs-lookup"><span data-stu-id="b6237-265">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

#### <a name="test-a-64-bit-x64-app"></a><span data-ttu-id="b6237-266">Testowanie aplikacji 64-bitowej (x64)</span><span class="sxs-lookup"><span data-stu-id="b6237-266">Test a 64-bit (x64) app</span></span>

<span data-ttu-id="b6237-267">**Bieżąca wersja**</span><span class="sxs-lookup"><span data-stu-id="b6237-267">**Current release**</span></span>

* <span data-ttu-id="b6237-268">Jeśli aplikacja jest wdrożeniem 64-bitowym (x64), [zależnym od platformy](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span><span class="sxs-lookup"><span data-stu-id="b6237-268">If the app is a 64-bit (x64) [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>
  1. `cd D:\Program Files\dotnet`
  1. <span data-ttu-id="b6237-269">Uruchom aplikację:`dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span><span class="sxs-lookup"><span data-stu-id="b6237-269">Run the app: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span></span>
* <span data-ttu-id="b6237-270">Jeśli aplikacja jest [niezależna wdrożenia](/dotnet/core/deploying/#self-contained-deployments-scd):</span><span class="sxs-lookup"><span data-stu-id="b6237-270">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>
  1. `cd D:\home\site\wwwroot`
  1. <span data-ttu-id="b6237-271">Uruchom aplikację:`{ASSEMBLY NAME}.exe`</span><span class="sxs-lookup"><span data-stu-id="b6237-271">Run the app: `{ASSEMBLY NAME}.exe`</span></span>

<span data-ttu-id="b6237-272">Dane wyjściowe konsoli z aplikacji, pokazujące błędy, są przekazywane do konsoli kudu.</span><span class="sxs-lookup"><span data-stu-id="b6237-272">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

<span data-ttu-id="b6237-273">**Wdrożenie zależne od platformy uruchomione w wersji zapoznawczej**</span><span class="sxs-lookup"><span data-stu-id="b6237-273">**Framework-dependent deployment running on a preview release**</span></span>

<span data-ttu-id="b6237-274">*Wymaga zainstalowania rozszerzenia witryny środowiska uruchomieniowego ASP.NET Core {VERSION} (x64).*</span><span class="sxs-lookup"><span data-stu-id="b6237-274">*Requires installing the ASP.NET Core {VERSION} (x64) Runtime site extension.*</span></span>

1. <span data-ttu-id="b6237-275">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x64`(`{X.Y}` to wersja środowiska uruchomieniowego)</span><span class="sxs-lookup"><span data-stu-id="b6237-275">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x64` (`{X.Y}` is the runtime version)</span></span>
1. <span data-ttu-id="b6237-276">Uruchom aplikację:`dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span><span class="sxs-lookup"><span data-stu-id="b6237-276">Run the app: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span></span>

<span data-ttu-id="b6237-277">Dane wyjściowe konsoli z aplikacji, pokazujące błędy, są przekazywane do konsoli kudu.</span><span class="sxs-lookup"><span data-stu-id="b6237-277">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

### <a name="aspnet-core-module-stdout-log-azure-app-service"></a><span data-ttu-id="b6237-278">Dziennik stdout modułu ASP.NET Core (Azure App Service)</span><span class="sxs-lookup"><span data-stu-id="b6237-278">ASP.NET Core Module stdout log (Azure App Service)</span></span>

<span data-ttu-id="b6237-279">Dziennik modułu ASP.NET Core stdout często rejestruje przydatne komunikaty o błędach, które nie są dostępne w dzienniku zdarzeń aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b6237-279">The ASP.NET Core Module stdout log often records useful error messages not found in the Application Event Log.</span></span> <span data-ttu-id="b6237-280">Włączanie i wyświetlanie dzienników stdout:</span><span class="sxs-lookup"><span data-stu-id="b6237-280">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="b6237-281">Przejdź do bloku **diagnozowanie i rozwiązywanie problemów** w Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="b6237-281">Navigate to the **Diagnose and solve problems** blade in the Azure portal.</span></span>
1. <span data-ttu-id="b6237-282">W obszarze **Wybierz kategorię problemu**wybierz przycisk **aplikacji sieci Web w dół** .</span><span class="sxs-lookup"><span data-stu-id="b6237-282">Under **SELECT PROBLEM CATEGORY**, select the **Web App Down** button.</span></span>
1. <span data-ttu-id="b6237-283">W obszarze **sugerowane rozwiązania** > **Włącz przekierowywanie dziennika stdout**, wybierz przycisk, aby **otworzyć konsolę kudu, aby edytować plik Web. config**.</span><span class="sxs-lookup"><span data-stu-id="b6237-283">Under **Suggested Solutions** > **Enable Stdout Log Redirection**, select the button to **Open Kudu Console to edit Web.Config**.</span></span>
1. <span data-ttu-id="b6237-284">W **konsoli diagnostyki**kudu Otwórz foldery w **witrynie** > Path**wwwroot**.</span><span class="sxs-lookup"><span data-stu-id="b6237-284">In the Kudu **Diagnostic Console**, open the folders to the path **site** > **wwwroot**.</span></span> <span data-ttu-id="b6237-285">Przewiń w dół, aby wyświetlić plik *Web. config* w dolnej części listy.</span><span class="sxs-lookup"><span data-stu-id="b6237-285">Scroll down to reveal the *web.config* file at the bottom of the list.</span></span>
1. <span data-ttu-id="b6237-286">Kliknij ikonę ołówka obok pliku *Web. config* .</span><span class="sxs-lookup"><span data-stu-id="b6237-286">Click the pencil icon next to the *web.config* file.</span></span>
1. <span data-ttu-id="b6237-287">Ustaw  wartość stdoutLogEnabled `true` na i zmień ścieżkę **stdoutLogFile** na: `\\?\%home%\LogFiles\stdout`.</span><span class="sxs-lookup"><span data-stu-id="b6237-287">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to: `\\?\%home%\LogFiles\stdout`.</span></span>
1. <span data-ttu-id="b6237-288">Wybierz pozycję **Zapisz** , aby zapisać zaktualizowany plik *Web. config* .</span><span class="sxs-lookup"><span data-stu-id="b6237-288">Select **Save** to save the updated *web.config* file.</span></span>
1. <span data-ttu-id="b6237-289">Wysłać żądanie do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b6237-289">Make a request to the app.</span></span>
1. <span data-ttu-id="b6237-290">Wróć do Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="b6237-290">Return to the Azure portal.</span></span> <span data-ttu-id="b6237-291">Wybierz blok **Narzędzia zaawansowane** w obszarze **Narzędzia programistyczne** .</span><span class="sxs-lookup"><span data-stu-id="b6237-291">Select the **Advanced Tools** blade in the **DEVELOPMENT TOOLS** area.</span></span> <span data-ttu-id="b6237-292">Wybierz przycisk **Przejdź&rarr;**  .</span><span class="sxs-lookup"><span data-stu-id="b6237-292">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="b6237-293">Konsola kudu otwiera się w nowej karcie lub oknie przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="b6237-293">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="b6237-294">Korzystając z paska nawigacyjnego w górnej części strony, Otwórz **konsolę debugowanie** i wybierz polecenie **cmd**.</span><span class="sxs-lookup"><span data-stu-id="b6237-294">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="b6237-295">Wybierz folder **LogFiles** .</span><span class="sxs-lookup"><span data-stu-id="b6237-295">Select the **LogFiles** folder.</span></span>
1. <span data-ttu-id="b6237-296">Sprawdź **zmodyfikowaną** kolumnę i wybierz ikonę ołówka, aby edytować dziennik stdout z datą ostatniej modyfikacji.</span><span class="sxs-lookup"><span data-stu-id="b6237-296">Inspect the **Modified** column and select the pencil icon to edit the stdout log with the latest modification date.</span></span>
1. <span data-ttu-id="b6237-297">Po otwarciu pliku dziennika zostanie wyświetlony komunikat o błędzie.</span><span class="sxs-lookup"><span data-stu-id="b6237-297">When the log file opens, the error is displayed.</span></span>

<span data-ttu-id="b6237-298">Wyłącz rejestrowanie stdout po zakończeniu rozwiązywania problemów:</span><span class="sxs-lookup"><span data-stu-id="b6237-298">Disable stdout logging when troubleshooting is complete:</span></span>

1. <span data-ttu-id="b6237-299">W **konsoli diagnostyki**kudu Wróć do **witryny** > ścieżki**wwwroot** , aby wyświetlić plik *Web. config* .</span><span class="sxs-lookup"><span data-stu-id="b6237-299">In the Kudu **Diagnostic Console**, return to the path **site** > **wwwroot** to reveal the *web.config* file.</span></span> <span data-ttu-id="b6237-300">Otwórz plik **Web. config** ponownie, wybierając ikonę ołówka.</span><span class="sxs-lookup"><span data-stu-id="b6237-300">Open the **web.config** file again by selecting the pencil icon.</span></span>
1. <span data-ttu-id="b6237-301">Ustaw **stdoutLogEnabled** do `false`.</span><span class="sxs-lookup"><span data-stu-id="b6237-301">Set **stdoutLogEnabled** to `false`.</span></span>
1. <span data-ttu-id="b6237-302">Wybierz pozycję **Zapisz** , aby zapisać plik.</span><span class="sxs-lookup"><span data-stu-id="b6237-302">Select **Save** to save the file.</span></span>

<span data-ttu-id="b6237-303">Aby uzyskać więcej informacji, zobacz <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span><span class="sxs-lookup"><span data-stu-id="b6237-303">For more information, see <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span></span>

> [!WARNING]
> <span data-ttu-id="b6237-304">Nie można wyłączyć dziennika stdout może prowadzić do awarii aplikacji lub serwera.</span><span class="sxs-lookup"><span data-stu-id="b6237-304">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="b6237-305">Brak brak limitu rozmiaru pliku dziennika lub liczba pliki dziennika utworzone.</span><span class="sxs-lookup"><span data-stu-id="b6237-305">There's no limit on log file size or the number of log files created.</span></span> <span data-ttu-id="b6237-306">Rejestrowania stdout można używać tylko w celu rozwiązywania problemów z uruchamianiem aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b6237-306">Only use stdout logging to troubleshoot app startup problems.</span></span>
>
> <span data-ttu-id="b6237-307">Aby uzyskać ogólne rejestrowanie w aplikacji ASP.NET Core po uruchomieniu, należy użyć biblioteki rejestrowania, która ogranicza rozmiar pliku dziennika i obraca dzienniki.</span><span class="sxs-lookup"><span data-stu-id="b6237-307">For general logging in an ASP.NET Core app after startup, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="b6237-308">Aby uzyskać więcej informacji, zobacz [rejestrowania innych dostawców](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="b6237-308">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

::: moniker range=">= aspnetcore-2.2"

### <a name="aspnet-core-module-debug-log-azure-app-service"></a><span data-ttu-id="b6237-309">Dziennik debugowania modułu ASP.NET Core (Azure App Service)</span><span class="sxs-lookup"><span data-stu-id="b6237-309">ASP.NET Core Module debug log (Azure App Service)</span></span>

<span data-ttu-id="b6237-310">Dziennik debugowania modułu ASP.NET Core zapewnia dodatkowe, dokładniejsze rejestrowanie z modułu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b6237-310">The ASP.NET Core Module debug log provides additional, deeper logging from the ASP.NET Core Module.</span></span> <span data-ttu-id="b6237-311">Włączanie i wyświetlanie dzienników stdout:</span><span class="sxs-lookup"><span data-stu-id="b6237-311">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="b6237-312">Aby włączyć Rozszerzony Dziennik diagnostyczny, wykonaj jedną z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="b6237-312">To enable the enhanced diagnostic log, perform either of the following:</span></span>
   * <span data-ttu-id="b6237-313">Postępuj zgodnie z instrukcjami w temacie [udoskonalone dzienniki diagnostyczne](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) , aby skonfigurować aplikację do rozszerzonego rejestrowania diagnostycznego.</span><span class="sxs-lookup"><span data-stu-id="b6237-313">Follow the instructions in [Enhanced diagnostic logs](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) to configure the app for an enhanced diagnostic logging.</span></span> <span data-ttu-id="b6237-314">Wdróż ponownie aplikację.</span><span class="sxs-lookup"><span data-stu-id="b6237-314">Redeploy the app.</span></span>
   * <span data-ttu-id="b6237-315">Dodaj pokazany w [ulepszonych dziennikach diagnostycznych](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) do pliku *Web. config* aplikacji na żywo za pomocą konsoli kudu: `<handlerSettings>`</span><span class="sxs-lookup"><span data-stu-id="b6237-315">Add the `<handlerSettings>` shown in [Enhanced diagnostic logs](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) to the live app's *web.config* file using the Kudu console:</span></span>
     1. <span data-ttu-id="b6237-316">Otwórz **Narzędzia zaawansowane** w obszarze **Narzędzia programistyczne** .</span><span class="sxs-lookup"><span data-stu-id="b6237-316">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="b6237-317">Wybierz przycisk **Przejdź&rarr;**  .</span><span class="sxs-lookup"><span data-stu-id="b6237-317">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="b6237-318">Konsola kudu otwiera się w nowej karcie lub oknie przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="b6237-318">The Kudu console opens in a new browser tab or window.</span></span>
     1. <span data-ttu-id="b6237-319">Korzystając z paska nawigacyjnego w górnej części strony, Otwórz **konsolę debugowanie** i wybierz polecenie **cmd**.</span><span class="sxs-lookup"><span data-stu-id="b6237-319">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
     1. <span data-ttu-id="b6237-320">Otwórz foldery w **witrynie** > Path**wwwroot**.</span><span class="sxs-lookup"><span data-stu-id="b6237-320">Open the folders to the path **site** > **wwwroot**.</span></span> <span data-ttu-id="b6237-321">Edytuj plik *Web. config* , wybierając przycisk ołówka.</span><span class="sxs-lookup"><span data-stu-id="b6237-321">Edit the *web.config* file by selecting the pencil button.</span></span> <span data-ttu-id="b6237-322">Dodaj sekcję, jak pokazano w [udoskonalonych dziennikach diagnostycznych.](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) `<handlerSettings>`</span><span class="sxs-lookup"><span data-stu-id="b6237-322">Add the `<handlerSettings>` section as shown in [Enhanced diagnostic logs](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs).</span></span> <span data-ttu-id="b6237-323">Wybierz ikonę **Zapisz**.</span><span class="sxs-lookup"><span data-stu-id="b6237-323">Select the **Save** button.</span></span>
1. <span data-ttu-id="b6237-324">Otwórz **Narzędzia zaawansowane** w obszarze **Narzędzia programistyczne** .</span><span class="sxs-lookup"><span data-stu-id="b6237-324">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="b6237-325">Wybierz przycisk **Przejdź&rarr;**  .</span><span class="sxs-lookup"><span data-stu-id="b6237-325">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="b6237-326">Konsola kudu otwiera się w nowej karcie lub oknie przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="b6237-326">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="b6237-327">Korzystając z paska nawigacyjnego w górnej części strony, Otwórz **konsolę debugowanie** i wybierz polecenie **cmd**.</span><span class="sxs-lookup"><span data-stu-id="b6237-327">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="b6237-328">Otwórz foldery w **witrynie** > Path**wwwroot**.</span><span class="sxs-lookup"><span data-stu-id="b6237-328">Open the folders to the path **site** > **wwwroot**.</span></span> <span data-ttu-id="b6237-329">Jeśli nie podano ścieżki do pliku *aspnetcore-Debug. log* , plik zostanie wyświetlony na liście.</span><span class="sxs-lookup"><span data-stu-id="b6237-329">If you didn't supply a path for the *aspnetcore-debug.log* file, the file appears in the list.</span></span> <span data-ttu-id="b6237-330">Jeśli podano ścieżkę, przejdź do lokalizacji pliku dziennika.</span><span class="sxs-lookup"><span data-stu-id="b6237-330">If you supplied a path, navigate to the location of the log file.</span></span>
1. <span data-ttu-id="b6237-331">Otwórz plik dziennika z przyciskiem ołówek obok nazwy pliku.</span><span class="sxs-lookup"><span data-stu-id="b6237-331">Open the log file with the pencil button next to the file name.</span></span>

<span data-ttu-id="b6237-332">Wyłącz rejestrowanie debugowania po zakończeniu rozwiązywania problemów:</span><span class="sxs-lookup"><span data-stu-id="b6237-332">Disable debug logging when troubleshooting is complete:</span></span>

<span data-ttu-id="b6237-333">Aby wyłączyć rozszerzony Dziennik debugowania, wykonaj jedną z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="b6237-333">To disable the enhanced debug log, perform either of the following:</span></span>

* <span data-ttu-id="b6237-334">Usuń plik z pliku *Web. config* lokalnie i Wdróż ponownie aplikację. `<handlerSettings>`</span><span class="sxs-lookup"><span data-stu-id="b6237-334">Remove the `<handlerSettings>` from the *web.config* file locally and redeploy the app.</span></span>
* <span data-ttu-id="b6237-335">Za pomocą konsoli kudu Edytuj plik *Web. config* i Usuń `<handlerSettings>` sekcję.</span><span class="sxs-lookup"><span data-stu-id="b6237-335">Use the Kudu console to edit the *web.config* file and remove the `<handlerSettings>` section.</span></span> <span data-ttu-id="b6237-336">Zapisz plik.</span><span class="sxs-lookup"><span data-stu-id="b6237-336">Save the file.</span></span>

<span data-ttu-id="b6237-337">Aby uzyskać więcej informacji, zobacz <xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs>.</span><span class="sxs-lookup"><span data-stu-id="b6237-337">For more information, see <xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs>.</span></span>

> [!WARNING]
> <span data-ttu-id="b6237-338">Niepowodzenie wyłączenia dziennika debugowania może prowadzić do awarii aplikacji lub serwera.</span><span class="sxs-lookup"><span data-stu-id="b6237-338">Failure to disable the debug log can lead to app or server failure.</span></span> <span data-ttu-id="b6237-339">Nie ma limitu rozmiaru pliku dziennika.</span><span class="sxs-lookup"><span data-stu-id="b6237-339">There's no limit on log file size.</span></span> <span data-ttu-id="b6237-340">Funkcja rejestrowania debugowania służy tylko do rozwiązywania problemów z uruchamianiem aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b6237-340">Only use debug logging to troubleshoot app startup problems.</span></span>
>
> <span data-ttu-id="b6237-341">Aby uzyskać ogólne rejestrowanie w aplikacji ASP.NET Core po uruchomieniu, należy użyć biblioteki rejestrowania, która ogranicza rozmiar pliku dziennika i obraca dzienniki.</span><span class="sxs-lookup"><span data-stu-id="b6237-341">For general logging in an ASP.NET Core app after startup, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="b6237-342">Aby uzyskać więcej informacji, zobacz [rejestrowania innych dostawców](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="b6237-342">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

::: moniker-end

### <a name="slow-or-hanging-app-azure-app-service"></a><span data-ttu-id="b6237-343">Aplikacja wolna lub wysunięta (Azure App Service)</span><span class="sxs-lookup"><span data-stu-id="b6237-343">Slow or hanging app (Azure App Service)</span></span>

<span data-ttu-id="b6237-344">Gdy aplikacja reaguje powoli lub zawiesza się na żądanie, zobacz następujące artykuły:</span><span class="sxs-lookup"><span data-stu-id="b6237-344">When an app responds slowly or hangs on a request, see the following articles:</span></span>

* [<span data-ttu-id="b6237-345">Rozwiązywanie problemów z wydajnością aplikacji sieci Web w Azure App Service</span><span class="sxs-lookup"><span data-stu-id="b6237-345">Troubleshoot slow web app performance issues in Azure App Service</span></span>](/azure/app-service/app-service-web-troubleshoot-performance-degradation)
* [<span data-ttu-id="b6237-346">Użyj rozszerzenia witryny diagnostyki awarii, aby przechwycić zrzut dla sporadycznych problemów z wyjątkami lub problemów z wydajnością w usłudze Azure Web App</span><span class="sxs-lookup"><span data-stu-id="b6237-346">Use Crash Diagnoser Site Extension to Capture Dump for Intermittent Exception issues or performance issues on Azure Web App</span></span>](https://blogs.msdn.microsoft.com/asiatech/2015/12/28/use-crash-diagnoser-site-extension-to-capture-dump-for-intermittent-exception-issues-or-performance-issues-on-azure-web-app/)

### <a name="monitoring-blades"></a><span data-ttu-id="b6237-347">Bloki monitorowania</span><span class="sxs-lookup"><span data-stu-id="b6237-347">Monitoring blades</span></span>

<span data-ttu-id="b6237-348">Bloki monitorowania zapewniają alternatywne środowisko rozwiązywania problemów z metodami opisanymi wcześniej w temacie.</span><span class="sxs-lookup"><span data-stu-id="b6237-348">Monitoring blades provide an alternative troubleshooting experience to the methods described earlier in the topic.</span></span> <span data-ttu-id="b6237-349">Te bloki mogą służyć do diagnozowania błędów serii 500.</span><span class="sxs-lookup"><span data-stu-id="b6237-349">These blades can be used to diagnose 500-series errors.</span></span>

<span data-ttu-id="b6237-350">Upewnij się, że rozszerzenia ASP.NET Core są zainstalowane.</span><span class="sxs-lookup"><span data-stu-id="b6237-350">Confirm that the ASP.NET Core Extensions are installed.</span></span> <span data-ttu-id="b6237-351">Jeśli rozszerzenia nie są zainstalowane, zainstaluj je ręcznie:</span><span class="sxs-lookup"><span data-stu-id="b6237-351">If the extensions aren't installed, install them manually:</span></span>

1. <span data-ttu-id="b6237-352">W sekcji blok **narzędzi programistycznych** wybierz blok **rozszerzenia** .</span><span class="sxs-lookup"><span data-stu-id="b6237-352">In the **DEVELOPMENT TOOLS** blade section, select the **Extensions** blade.</span></span>
1. <span data-ttu-id="b6237-353">**Rozszerzenia ASP.NET Core** powinny znajdować się na liście.</span><span class="sxs-lookup"><span data-stu-id="b6237-353">The **ASP.NET Core Extensions** should appear in the list.</span></span>
1. <span data-ttu-id="b6237-354">Jeśli rozszerzenia nie są zainstalowane, wybierz przycisk **Dodaj** .</span><span class="sxs-lookup"><span data-stu-id="b6237-354">If the extensions aren't installed, select the **Add** button.</span></span>
1. <span data-ttu-id="b6237-355">Wybierz z listy **rozszerzenia ASP.NET Core** .</span><span class="sxs-lookup"><span data-stu-id="b6237-355">Choose the **ASP.NET Core Extensions** from the list.</span></span>
1. <span data-ttu-id="b6237-356">Wybierz **przycisk OK** , aby zaakceptować postanowienia prawne.</span><span class="sxs-lookup"><span data-stu-id="b6237-356">Select **OK** to accept the legal terms.</span></span>
1. <span data-ttu-id="b6237-357">W bloku **Dodaj rozszerzenie** wybierz pozycję **OK** .</span><span class="sxs-lookup"><span data-stu-id="b6237-357">Select **OK** on the **Add extension** blade.</span></span>
1. <span data-ttu-id="b6237-358">Komunikat podręczny informujący o pomyślnym zainstalowaniu rozszerzeń.</span><span class="sxs-lookup"><span data-stu-id="b6237-358">An informational pop-up message indicates when the extensions are successfully installed.</span></span>

<span data-ttu-id="b6237-359">Jeśli rejestrowanie stdout nie jest włączone, wykonaj następujące kroki:</span><span class="sxs-lookup"><span data-stu-id="b6237-359">If stdout logging isn't enabled, follow these steps:</span></span>

1. <span data-ttu-id="b6237-360">W Azure Portal wybierz blok **Narzędzia zaawansowane** w obszarze **Narzędzia programistyczne** .</span><span class="sxs-lookup"><span data-stu-id="b6237-360">In the Azure portal, select the **Advanced Tools** blade in the **DEVELOPMENT TOOLS** area.</span></span> <span data-ttu-id="b6237-361">Wybierz przycisk **Przejdź&rarr;**  .</span><span class="sxs-lookup"><span data-stu-id="b6237-361">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="b6237-362">Konsola kudu otwiera się w nowej karcie lub oknie przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="b6237-362">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="b6237-363">Korzystając z paska nawigacyjnego w górnej części strony, Otwórz **konsolę debugowanie** i wybierz polecenie **cmd**.</span><span class="sxs-lookup"><span data-stu-id="b6237-363">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="b6237-364">Otwórz foldery w **witrynie** > Path**wwwroot** i przewiń w dół, aby wyświetlić plik *Web. config* w dolnej części listy.</span><span class="sxs-lookup"><span data-stu-id="b6237-364">Open the folders to the path **site** > **wwwroot** and scroll down to reveal the *web.config* file at the bottom of the list.</span></span>
1. <span data-ttu-id="b6237-365">Kliknij ikonę ołówka obok pliku *Web. config* .</span><span class="sxs-lookup"><span data-stu-id="b6237-365">Click the pencil icon next to the *web.config* file.</span></span>
1. <span data-ttu-id="b6237-366">Ustaw  wartość stdoutLogEnabled `true` na i zmień ścieżkę **stdoutLogFile** na: `\\?\%home%\LogFiles\stdout`.</span><span class="sxs-lookup"><span data-stu-id="b6237-366">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to: `\\?\%home%\LogFiles\stdout`.</span></span>
1. <span data-ttu-id="b6237-367">Wybierz pozycję **Zapisz** , aby zapisać zaktualizowany plik *Web. config* .</span><span class="sxs-lookup"><span data-stu-id="b6237-367">Select **Save** to save the updated *web.config* file.</span></span>

<span data-ttu-id="b6237-368">Wykonaj aktywację rejestrowania diagnostycznego:</span><span class="sxs-lookup"><span data-stu-id="b6237-368">Proceed to activate diagnostic logging:</span></span>

1. <span data-ttu-id="b6237-369">W Azure Portal wybierz blok **dzienników diagnostycznych** .</span><span class="sxs-lookup"><span data-stu-id="b6237-369">In the Azure portal, select the **Diagnostics logs** blade.</span></span>
1. <span data-ttu-id="b6237-370">Wybierz pozycję **Włącz** , aby włączyć **Rejestrowanie aplikacji (system plików)** i **szczegółowe komunikaty o błędach**.</span><span class="sxs-lookup"><span data-stu-id="b6237-370">Select the **On** switch for **Application Logging (Filesystem)** and **Detailed error messages**.</span></span> <span data-ttu-id="b6237-371">Wybierz przycisk **Zapisz** znajdujący się u góry bloku.</span><span class="sxs-lookup"><span data-stu-id="b6237-371">Select the **Save** button at the top of the blade.</span></span>
1. <span data-ttu-id="b6237-372">Aby uwzględnić śledzenie nieudanych żądań, znane także jako rejestrowanie nieudanych żądań buforowania zdarzeń (FREB), wybierz **przełącznik dla** **śledzenia nieudanych żądań**.</span><span class="sxs-lookup"><span data-stu-id="b6237-372">To include failed request tracing, also known as Failed Request Event Buffering (FREB) logging, select the **On** switch for **Failed request tracing**.</span></span>
1. <span data-ttu-id="b6237-373">Wybierz blok **strumień dziennika** , który jest wyświetlany bezpośrednio w bloku **dzienników diagnostycznych** w portalu.</span><span class="sxs-lookup"><span data-stu-id="b6237-373">Select the **Log stream** blade, which is listed immediately under the **Diagnostics logs** blade in the portal.</span></span>
1. <span data-ttu-id="b6237-374">Wysłać żądanie do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b6237-374">Make a request to the app.</span></span>
1. <span data-ttu-id="b6237-375">W danych strumienia dziennika jest wskazywana Przyczyna błędu.</span><span class="sxs-lookup"><span data-stu-id="b6237-375">Within the log stream data, the cause of the error is indicated.</span></span>

<span data-ttu-id="b6237-376">Należy pamiętać o wyłączeniu rejestrowania stdout po zakończeniu rozwiązywania problemów.</span><span class="sxs-lookup"><span data-stu-id="b6237-376">Be sure to disable stdout logging when troubleshooting is complete.</span></span>

<span data-ttu-id="b6237-377">Aby wyświetlić dzienniki śledzenia niepomyślnych żądań (dzienniki FREB):</span><span class="sxs-lookup"><span data-stu-id="b6237-377">To view the failed request tracing logs (FREB logs):</span></span>

1. <span data-ttu-id="b6237-378">Przejdź do bloku **diagnozowanie i rozwiązywanie problemów** w Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="b6237-378">Navigate to the **Diagnose and solve problems** blade in the Azure portal.</span></span>
1. <span data-ttu-id="b6237-379">Wybierz pozycję **dzienniki śledzenia niepomyślnych żądań** z obszaru **Narzędzia obsługi** na pasku bocznym.</span><span class="sxs-lookup"><span data-stu-id="b6237-379">Select **Failed Request Tracing Logs** from the **SUPPORT TOOLS** area of the sidebar.</span></span>

<span data-ttu-id="b6237-380">Zobacz [sekcję śledzenie niepomyślnych żądań w temacie Włączanie rejestrowania diagnostyki dla aplikacji sieci Web w programie Azure App Service](/azure/app-service/web-sites-enable-diagnostic-log#failed-request-traces) i zapoznaj się [z wydajnością aplikacji Web Apps na platformie Azure: Jak mogę włączyć śledzenia nieudanych żądań? ](/azure/app-service/app-service-web-availability-performance-application-issues-faq#how-do-i-turn-on-failed-request-tracing) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="b6237-380">See [Failed request traces section of the Enable diagnostics logging for web apps in Azure App Service topic](/azure/app-service/web-sites-enable-diagnostic-log#failed-request-traces) and the [Application performance FAQs for Web Apps in Azure: How do I turn on failed request tracing?](/azure/app-service/app-service-web-availability-performance-application-issues-faq#how-do-i-turn-on-failed-request-tracing) for more information.</span></span>

<span data-ttu-id="b6237-381">Aby uzyskać więcej informacji, zobacz [Włączanie rejestrowania diagnostycznego dla aplikacji sieci Web w Azure App Service](/azure/app-service/web-sites-enable-diagnostic-log).</span><span class="sxs-lookup"><span data-stu-id="b6237-381">For more information, see [Enable diagnostics logging for web apps in Azure App Service](/azure/app-service/web-sites-enable-diagnostic-log).</span></span>

> [!WARNING]
> <span data-ttu-id="b6237-382">Nie można wyłączyć dziennika stdout może prowadzić do awarii aplikacji lub serwera.</span><span class="sxs-lookup"><span data-stu-id="b6237-382">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="b6237-383">Brak brak limitu rozmiaru pliku dziennika lub liczba pliki dziennika utworzone.</span><span class="sxs-lookup"><span data-stu-id="b6237-383">There's no limit on log file size or the number of log files created.</span></span>
>
> <span data-ttu-id="b6237-384">Rutynowe logujesz się w aplikacji ASP.NET Core, użytku bibliotekę rejestrowania, która ogranicza rozmiar pliku dziennika i obraca się loguje.</span><span class="sxs-lookup"><span data-stu-id="b6237-384">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="b6237-385">Aby uzyskać więcej informacji, zobacz [rejestrowania innych dostawców](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="b6237-385">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

## <a name="troubleshoot-on-iis"></a><span data-ttu-id="b6237-386">Rozwiązywanie problemów dotyczących usług IIS</span><span class="sxs-lookup"><span data-stu-id="b6237-386">Troubleshoot on IIS</span></span>

### <a name="application-event-log-iis"></a><span data-ttu-id="b6237-387">Dziennik zdarzeń aplikacji (IIS)</span><span class="sxs-lookup"><span data-stu-id="b6237-387">Application Event Log (IIS)</span></span>

<span data-ttu-id="b6237-388">Dostęp do dziennika zdarzeń aplikacji:</span><span class="sxs-lookup"><span data-stu-id="b6237-388">Access the Application Event Log:</span></span>

1. <span data-ttu-id="b6237-389">Otwieranie Start menu, wyszukaj **Podgląd zdarzeń**, a następnie wybierz pozycję **Podgląd zdarzeń** aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b6237-389">Open the Start menu, search for **Event Viewer**, and then select the **Event Viewer** app.</span></span>
1. <span data-ttu-id="b6237-390">W **Podgląd zdarzeń**, otwórz **Dzienniki Windows** węzła.</span><span class="sxs-lookup"><span data-stu-id="b6237-390">In **Event Viewer**, open the **Windows Logs** node.</span></span>
1. <span data-ttu-id="b6237-391">Wybierz **aplikacji** można otworzyć dziennika zdarzeń aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b6237-391">Select **Application** to open the Application Event Log.</span></span>
1. <span data-ttu-id="b6237-392">Wyszukaj błędy skojarzone z aplikacją się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="b6237-392">Search for errors associated with the failing app.</span></span> <span data-ttu-id="b6237-393">Błędy mają wartość *moduł AspNetCore IIS* lub *usług IIS Express AspNetCore modułu* w *źródła* kolumny.</span><span class="sxs-lookup"><span data-stu-id="b6237-393">Errors have a value of *IIS AspNetCore Module* or *IIS Express AspNetCore Module* in the *Source* column.</span></span>

### <a name="run-the-app-at-a-command-prompt"></a><span data-ttu-id="b6237-394">Uruchamianie aplikacji w wierszu polecenia</span><span class="sxs-lookup"><span data-stu-id="b6237-394">Run the app at a command prompt</span></span>

<span data-ttu-id="b6237-395">Wiele błędów uruchamiania przestaną generować przydatne informacje w dzienniku zdarzeń aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b6237-395">Many startup errors don't produce useful information in the Application Event Log.</span></span> <span data-ttu-id="b6237-396">Przyczyną niektórych błędów można znaleźć, uruchamiając aplikację w wierszu polecenia w systemie hostingu.</span><span class="sxs-lookup"><span data-stu-id="b6237-396">You can find the cause of some errors by running the app at a command prompt on the hosting system.</span></span>

#### <a name="framework-dependent-deployment"></a><span data-ttu-id="b6237-397">Wdrożenie zależny od struktury</span><span class="sxs-lookup"><span data-stu-id="b6237-397">Framework-dependent deployment</span></span>

<span data-ttu-id="b6237-398">Jeśli aplikacja jest [wdrożenia zależny od struktury](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span><span class="sxs-lookup"><span data-stu-id="b6237-398">If the app is a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>

1. <span data-ttu-id="b6237-399">W wierszu polecenia przejdź do folderu wdrożenia i uruchomienia aplikacji, wykonując zestaw aplikacji za pomocą *dotnet.exe*.</span><span class="sxs-lookup"><span data-stu-id="b6237-399">At a command prompt, navigate to the deployment folder and run the app by executing the app's assembly with *dotnet.exe*.</span></span> <span data-ttu-id="b6237-400">W poniższym poleceniu zastąp nazwę zestawu aplikacji dla \<assembly_name >: `dotnet .\<assembly_name>.dll`.</span><span class="sxs-lookup"><span data-stu-id="b6237-400">In the following command, substitute the name of the app's assembly for \<assembly_name>: `dotnet .\<assembly_name>.dll`.</span></span>
1. <span data-ttu-id="b6237-401">Dane wyjściowe z aplikacji, przedstawiający wszystkie błędy z konsoli są zapisywane w oknie konsoli.</span><span class="sxs-lookup"><span data-stu-id="b6237-401">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="b6237-402">Jeśli wystąpią błędy, gdy kieruje żądanie do aplikacji, należy wysłać żądanie do hosta i portu, na którym nasłuchuje Kestrel.</span><span class="sxs-lookup"><span data-stu-id="b6237-402">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="b6237-403">Przy użyciu domyślnego hosta i post, zgłosić wniosek o `http://localhost:5000/`.</span><span class="sxs-lookup"><span data-stu-id="b6237-403">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="b6237-404">Jeśli aplikacja reaguje, zwykle pod adresem punktu końcowego Kestrel, problem najprawdopodobniej związanych z konfiguracją hostingu i mniej prawdopodobne w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b6237-404">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the hosting configuration and less likely within the app.</span></span>

#### <a name="self-contained-deployment"></a><span data-ttu-id="b6237-405">Niezależne wdrożenia</span><span class="sxs-lookup"><span data-stu-id="b6237-405">Self-contained deployment</span></span>

<span data-ttu-id="b6237-406">Jeśli aplikacja jest [niezależna wdrożenia](/dotnet/core/deploying/#self-contained-deployments-scd):</span><span class="sxs-lookup"><span data-stu-id="b6237-406">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>

1. <span data-ttu-id="b6237-407">W wierszu polecenia przejdź do folderu wdrożenia i uruchomienia pliku wykonywalnego aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b6237-407">At a command prompt, navigate to the deployment folder and run the app's executable.</span></span> <span data-ttu-id="b6237-408">W poniższym poleceniu zastąp nazwę zestawu aplikacji dla \<assembly_name >: `<assembly_name>.exe`.</span><span class="sxs-lookup"><span data-stu-id="b6237-408">In the following command, substitute the name of the app's assembly for \<assembly_name>: `<assembly_name>.exe`.</span></span>
1. <span data-ttu-id="b6237-409">Dane wyjściowe z aplikacji, przedstawiający wszystkie błędy z konsoli są zapisywane w oknie konsoli.</span><span class="sxs-lookup"><span data-stu-id="b6237-409">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="b6237-410">Jeśli wystąpią błędy, gdy kieruje żądanie do aplikacji, należy wysłać żądanie do hosta i portu, na którym nasłuchuje Kestrel.</span><span class="sxs-lookup"><span data-stu-id="b6237-410">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="b6237-411">Przy użyciu domyślnego hosta i post, zgłosić wniosek o `http://localhost:5000/`.</span><span class="sxs-lookup"><span data-stu-id="b6237-411">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="b6237-412">Jeśli aplikacja reaguje, zwykle pod adresem punktu końcowego Kestrel, problem najprawdopodobniej związanych z konfiguracją hostingu i mniej prawdopodobne w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b6237-412">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the hosting configuration and less likely within the app.</span></span>

### <a name="aspnet-core-module-stdout-log-iis"></a><span data-ttu-id="b6237-413">Dziennik stdout modułu ASP.NET Core (IIS)</span><span class="sxs-lookup"><span data-stu-id="b6237-413">ASP.NET Core Module stdout log (IIS)</span></span>

<span data-ttu-id="b6237-414">Włączanie i wyświetlanie dzienników stdout:</span><span class="sxs-lookup"><span data-stu-id="b6237-414">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="b6237-415">Przejdź do folderu wdrożenia witryny w systemie hostingu.</span><span class="sxs-lookup"><span data-stu-id="b6237-415">Navigate to the site's deployment folder on the hosting system.</span></span>
1. <span data-ttu-id="b6237-416">Jeśli *dzienniki* folder nie jest obecny, Utwórz folder.</span><span class="sxs-lookup"><span data-stu-id="b6237-416">If the *logs* folder isn't present, create the folder.</span></span> <span data-ttu-id="b6237-417">Aby uzyskać instrukcje dotyczące włączania MSBuild tworzenia *dzienniki* folderu we wdrożeniu automatycznie, zobacz [strukturę katalogów](xref:host-and-deploy/directory-structure) tematu.</span><span class="sxs-lookup"><span data-stu-id="b6237-417">For instructions on how to enable MSBuild to create the *logs* folder in the deployment automatically, see the [Directory structure](xref:host-and-deploy/directory-structure) topic.</span></span>
1. <span data-ttu-id="b6237-418">Edytuj *web.config* pliku.</span><span class="sxs-lookup"><span data-stu-id="b6237-418">Edit the *web.config* file.</span></span> <span data-ttu-id="b6237-419">Ustaw **stdoutLogEnabled** do `true` i zmień **stdoutLogFile** ścieżki, aby wskazywał *dzienniki* folderu (na przykład `.\logs\stdout`).</span><span class="sxs-lookup"><span data-stu-id="b6237-419">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to point to the *logs* folder (for example, `.\logs\stdout`).</span></span> <span data-ttu-id="b6237-420">`stdout` w ścieżce jest prefiks nazwy pliku dziennika.</span><span class="sxs-lookup"><span data-stu-id="b6237-420">`stdout` in the path is the log file name prefix.</span></span> <span data-ttu-id="b6237-421">Sygnatura czasowa, identyfikator procesu i rozszerzenie pliku są dodawane automatycznie, gdy zostanie utworzony dziennik.</span><span class="sxs-lookup"><span data-stu-id="b6237-421">A timestamp, process id, and file extension are added automatically when the log is created.</span></span> <span data-ttu-id="b6237-422">Za pomocą `stdout` jako prefiks nazwy pliku, plik dziennika typowe o nazwie *stdout_20180205184032_5412.log*.</span><span class="sxs-lookup"><span data-stu-id="b6237-422">Using `stdout` as the file name prefix, a typical log file is named *stdout_20180205184032_5412.log*.</span></span>
1. <span data-ttu-id="b6237-423">Upewnij się, tożsamość puli aplikacji ma uprawnienia do zapisu *dzienniki* folderu.</span><span class="sxs-lookup"><span data-stu-id="b6237-423">Ensure your application pool's identity has write permissions to the *logs* folder.</span></span>
1. <span data-ttu-id="b6237-424">Zapisz zaktualizowany *web.config* pliku.</span><span class="sxs-lookup"><span data-stu-id="b6237-424">Save the updated *web.config* file.</span></span>
1. <span data-ttu-id="b6237-425">Wysłać żądanie do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b6237-425">Make a request to the app.</span></span>
1. <span data-ttu-id="b6237-426">Przejdź do *dzienniki* folderu.</span><span class="sxs-lookup"><span data-stu-id="b6237-426">Navigate to the *logs* folder.</span></span> <span data-ttu-id="b6237-427">Znajdowanie i otwieranie najnowszych dziennika stdout.</span><span class="sxs-lookup"><span data-stu-id="b6237-427">Find and open the most recent stdout log.</span></span>
1. <span data-ttu-id="b6237-428">Badanie w dzienniku błędów.</span><span class="sxs-lookup"><span data-stu-id="b6237-428">Study the log for errors.</span></span>

<span data-ttu-id="b6237-429">Wyłącz rejestrowanie stdout po zakończeniu rozwiązywania problemów:</span><span class="sxs-lookup"><span data-stu-id="b6237-429">Disable stdout logging when troubleshooting is complete:</span></span>

1. <span data-ttu-id="b6237-430">Edytuj *web.config* pliku.</span><span class="sxs-lookup"><span data-stu-id="b6237-430">Edit the *web.config* file.</span></span>
1. <span data-ttu-id="b6237-431">Ustaw **stdoutLogEnabled** do `false`.</span><span class="sxs-lookup"><span data-stu-id="b6237-431">Set **stdoutLogEnabled** to `false`.</span></span>
1. <span data-ttu-id="b6237-432">Zapisz plik.</span><span class="sxs-lookup"><span data-stu-id="b6237-432">Save the file.</span></span>

<span data-ttu-id="b6237-433">Aby uzyskać więcej informacji, zobacz <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span><span class="sxs-lookup"><span data-stu-id="b6237-433">For more information, see <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span></span>

> [!WARNING]
> <span data-ttu-id="b6237-434">Nie można wyłączyć dziennika stdout może prowadzić do awarii aplikacji lub serwera.</span><span class="sxs-lookup"><span data-stu-id="b6237-434">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="b6237-435">Brak brak limitu rozmiaru pliku dziennika lub liczba pliki dziennika utworzone.</span><span class="sxs-lookup"><span data-stu-id="b6237-435">There's no limit on log file size or the number of log files created.</span></span>
>
> <span data-ttu-id="b6237-436">Rutynowe logujesz się w aplikacji ASP.NET Core, użytku bibliotekę rejestrowania, która ogranicza rozmiar pliku dziennika i obraca się loguje.</span><span class="sxs-lookup"><span data-stu-id="b6237-436">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="b6237-437">Aby uzyskać więcej informacji, zobacz [rejestrowania innych dostawców](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="b6237-437">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

::: moniker range=">= aspnetcore-2.2"

### <a name="aspnet-core-module-debug-log-iis"></a><span data-ttu-id="b6237-438">Dziennik debugowania modułu ASP.NET Core (IIS)</span><span class="sxs-lookup"><span data-stu-id="b6237-438">ASP.NET Core Module debug log (IIS)</span></span>

<span data-ttu-id="b6237-439">Dodaj następujące ustawienia programu obsługi do pliku *Web. config* aplikacji, aby włączyć Dziennik debugowania modułu ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="b6237-439">Add the following handler settings to the app's *web.config* file to enable ASP.NET Core Module debug log:</span></span>

```xml
<aspNetCore ...>
  <handlerSettings>
    <handlerSetting name="debugLevel" value="file" />
    <handlerSetting name="debugFile" value="c:\temp\ancm.log" />
  </handlerSettings>
</aspNetCore>
```

<span data-ttu-id="b6237-440">Upewnij się, czy ścieżka określona dla dziennika istnieje i że tożsamość puli aplikacji ma uprawnienia do zapisu do lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="b6237-440">Confirm that the path specified for the log exists and that the app pool's identity has write permissions to the location.</span></span>

<span data-ttu-id="b6237-441">Aby uzyskać więcej informacji, zobacz <xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs>.</span><span class="sxs-lookup"><span data-stu-id="b6237-441">For more information, see <xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs>.</span></span>

::: moniker-end

### <a name="enable-the-developer-exception-page"></a><span data-ttu-id="b6237-442">Włącz na stronie wyjątków dla deweloperów</span><span class="sxs-lookup"><span data-stu-id="b6237-442">Enable the Developer Exception Page</span></span>

<span data-ttu-id="b6237-443">`ASPNETCORE_ENVIRONMENT` [Zmiennej środowiskowej, można dodać do pliku web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) do uruchomienia aplikacji w środowisku programistycznym.</span><span class="sxs-lookup"><span data-stu-id="b6237-443">The `ASPNETCORE_ENVIRONMENT` [environment variable can be added to web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) to run the app in the Development environment.</span></span> <span data-ttu-id="b6237-444">Tak długo, jak środowisko nie jest zastąpione przy uruchamianiu aplikacji przez `UseEnvironment` umożliwia ustawienie zmiennej środowiskowej w Konstruktorze hosta [stronie wyjątków deweloperów](xref:fundamentals/error-handling) się pojawiać po uruchomieniu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b6237-444">As long as the environment isn't overridden in app startup by `UseEnvironment` on the host builder, setting the environment variable allows the [Developer Exception Page](xref:fundamentals/error-handling) to appear when the app is run.</span></span>

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

<span data-ttu-id="b6237-445">Ustawienie zmiennej środowiskowej, aby uzyskać `ASPNETCORE_ENVIRONMENT` jest zalecane tylko dla używane w przejściowym i testowania serwerów, które nie są połączone z Internetem.</span><span class="sxs-lookup"><span data-stu-id="b6237-445">Setting the environment variable for `ASPNETCORE_ENVIRONMENT` is only recommended for use on staging and testing servers that aren't exposed to the Internet.</span></span> <span data-ttu-id="b6237-446">Usuń zmienną środowiskową z *web.config* plik po rozwiązywania problemów.</span><span class="sxs-lookup"><span data-stu-id="b6237-446">Remove the environment variable from the *web.config* file after troubleshooting.</span></span> <span data-ttu-id="b6237-447">Aby uzyskać informacje na temat ustawiania zmiennych środowiskowych *web.config*, zobacz [environmentVariables element podrzędny elementu aspNetCore](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).</span><span class="sxs-lookup"><span data-stu-id="b6237-447">For information on setting environment variables in *web.config*, see [environmentVariables child element of aspNetCore](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).</span></span>

### <a name="obtain-data-from-an-app"></a><span data-ttu-id="b6237-448">Uzyskiwanie danych z aplikacji</span><span class="sxs-lookup"><span data-stu-id="b6237-448">Obtain data from an app</span></span>

<span data-ttu-id="b6237-449">Jeśli aplikacja jest w stanie odpowiadać na żądania, żądania, połączenia i dodatkowych danych można uzyskać z aplikację za pomocą oprogramowania pośredniczącego terminalu wbudowanego.</span><span class="sxs-lookup"><span data-stu-id="b6237-449">If an app is capable of responding to requests, obtain request, connection, and additional data from the app using terminal inline middleware.</span></span> <span data-ttu-id="b6237-450">Aby uzyskać więcej informacji i przykładowy kod, zobacz <xref:test/troubleshoot#obtain-data-from-an-app>.</span><span class="sxs-lookup"><span data-stu-id="b6237-450">For more information and sample code, see <xref:test/troubleshoot#obtain-data-from-an-app>.</span></span>

### <a name="slow-or-hanging-app-iis"></a><span data-ttu-id="b6237-451">Aplikacja wolna lub wysunięta (IIS)</span><span class="sxs-lookup"><span data-stu-id="b6237-451">Slow or hanging app (IIS)</span></span>

<span data-ttu-id="b6237-452">*Zrzut awaryjny* to migawka pamięci systemu, która może pomóc w ustaleniu przyczyny awarii aplikacji, awarii uruchamiania lub powolnej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b6237-452">A *crash dump* is a snapshot of the system's memory and can help determine the cause of an app crash, startup failure, or slow app.</span></span>

#### <a name="app-crashes-or-encounters-an-exception"></a><span data-ttu-id="b6237-453">Awaria aplikacji lub napotka wyjątek</span><span class="sxs-lookup"><span data-stu-id="b6237-453">App crashes or encounters an exception</span></span>

<span data-ttu-id="b6237-454">Uzyskaj i Analizuj Zrzut z [raportowanie błędów systemu Windows (raportowanie błędów systemu Windows)](/windows/desktop/wer/windows-error-reporting):</span><span class="sxs-lookup"><span data-stu-id="b6237-454">Obtain and analyze a dump from [Windows Error Reporting (WER)](/windows/desktop/wer/windows-error-reporting):</span></span>

1. <span data-ttu-id="b6237-455">Utwórz folder do przechowywania plików zrzutu awaryjnego `c:\dumps`w.</span><span class="sxs-lookup"><span data-stu-id="b6237-455">Create a folder to hold crash dump files at `c:\dumps`.</span></span> <span data-ttu-id="b6237-456">Pula aplikacji musi mieć dostęp do zapisu w folderze.</span><span class="sxs-lookup"><span data-stu-id="b6237-456">The app pool must have write access to the folder.</span></span>
1. <span data-ttu-id="b6237-457">Uruchom [skrypt programu PowerShell](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/EnableDumps.ps1)w programie EnableDumps:</span><span class="sxs-lookup"><span data-stu-id="b6237-457">Run the [EnableDumps PowerShell script](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/EnableDumps.ps1):</span></span>
   * <span data-ttu-id="b6237-458">Jeśli aplikacja korzysta z [modelu hostingu w procesie](xref:host-and-deploy/iis/index#in-process-hosting-model), uruchom skrypt dla programu *w3wp. exe*:</span><span class="sxs-lookup"><span data-stu-id="b6237-458">If the app uses the [in-process hosting model](xref:host-and-deploy/iis/index#in-process-hosting-model), run the script for *w3wp.exe*:</span></span>

     ```console
     .\EnableDumps w3wp.exe c:\dumps
     ```

   * <span data-ttu-id="b6237-459">Jeśli aplikacja korzysta z [modelu hostingu poza procesem](xref:host-and-deploy/iis/index#out-of-process-hosting-model), uruchom skrypt dla programu *dotnet. exe*:</span><span class="sxs-lookup"><span data-stu-id="b6237-459">If the app uses the [out-of-process hosting model](xref:host-and-deploy/iis/index#out-of-process-hosting-model), run the script for *dotnet.exe*:</span></span>

     ```console
     .\EnableDumps dotnet.exe c:\dumps
     ```

1. <span data-ttu-id="b6237-460">Uruchom aplikację w warunkach, które powodują awarię.</span><span class="sxs-lookup"><span data-stu-id="b6237-460">Run the app under the conditions that cause the crash to occur.</span></span>
1. <span data-ttu-id="b6237-461">Po wystąpieniu awarii Uruchom [skrypt programu DisableDumps PowerShell](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/DisableDumps.ps1):</span><span class="sxs-lookup"><span data-stu-id="b6237-461">After the crash has occurred, run the [DisableDumps PowerShell script](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/DisableDumps.ps1):</span></span>
   * <span data-ttu-id="b6237-462">Jeśli aplikacja korzysta z [modelu hostingu w procesie](xref:host-and-deploy/iis/index#in-process-hosting-model), uruchom skrypt dla programu *w3wp. exe*:</span><span class="sxs-lookup"><span data-stu-id="b6237-462">If the app uses the [in-process hosting model](xref:host-and-deploy/iis/index#in-process-hosting-model), run the script for *w3wp.exe*:</span></span>

     ```console
     .\DisableDumps w3wp.exe
     ```

   * <span data-ttu-id="b6237-463">Jeśli aplikacja korzysta z [modelu hostingu poza procesem](xref:host-and-deploy/iis/index#out-of-process-hosting-model), uruchom skrypt dla programu *dotnet. exe*:</span><span class="sxs-lookup"><span data-stu-id="b6237-463">If the app uses the [out-of-process hosting model](xref:host-and-deploy/iis/index#out-of-process-hosting-model), run the script for *dotnet.exe*:</span></span>

     ```console
     .\DisableDumps dotnet.exe
     ```

<span data-ttu-id="b6237-464">Po awarii aplikacji i zakończeniu zbierania zrzutów aplikacja może zakończyć normalne działanie.</span><span class="sxs-lookup"><span data-stu-id="b6237-464">After an app crashes and dump collection is complete, the app is allowed to terminate normally.</span></span> <span data-ttu-id="b6237-465">Skrypt programu PowerShell konfiguruje raportowanie błędów systemu Windows w celu zebrania do pięciu zrzutów na aplikację.</span><span class="sxs-lookup"><span data-stu-id="b6237-465">The PowerShell script configures WER to collect up to five dumps per app.</span></span>

> [!WARNING]
> <span data-ttu-id="b6237-466">Zrzuty awaryjne mogą wymagać dużej ilości miejsca na dysku (do kilku gigabajtów).</span><span class="sxs-lookup"><span data-stu-id="b6237-466">Crash dumps might take up a large amount of disk space (up to several gigabytes each).</span></span>

#### <a name="app-hangs-fails-during-startup-or-runs-normally"></a><span data-ttu-id="b6237-467">Aplikacja zawiesza się, kończy się niepowodzeniem podczas uruchamiania lub działa normalnie</span><span class="sxs-lookup"><span data-stu-id="b6237-467">App hangs, fails during startup, or runs normally</span></span>

<span data-ttu-id="b6237-468">Gdy aplikacja *zawiesza* się (bez awarii), kończy się niepowodzeniem podczas uruchamiania lub działa normalnie, zobacz [pliki zrzutu w trybie użytkownika: Wybieranie najlepszego narzędzia](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool) do wybrania odpowiedniego narzędzia do wyprodukowania zrzutu.</span><span class="sxs-lookup"><span data-stu-id="b6237-468">When an app *hangs* (stops responding but doesn't crash), fails during startup, or runs normally, see [User-Mode Dump Files: Choosing the Best Tool](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool) to select an appropriate tool to produce the dump.</span></span>

#### <a name="analyze-the-dump"></a><span data-ttu-id="b6237-469">Analizowanie zrzutu</span><span class="sxs-lookup"><span data-stu-id="b6237-469">Analyze the dump</span></span>

<span data-ttu-id="b6237-470">Zrzut można analizować przy użyciu kilku metod.</span><span class="sxs-lookup"><span data-stu-id="b6237-470">A dump can be analyzed using several approaches.</span></span> <span data-ttu-id="b6237-471">Aby uzyskać więcej informacji, zobacz [Analizowanie pliku zrzutu w trybie użytkownika](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file).</span><span class="sxs-lookup"><span data-stu-id="b6237-471">For more information, see [Analyzing a User-Mode Dump File](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file).</span></span>

## <a name="clear-package-caches"></a><span data-ttu-id="b6237-472">Wyczyść pamięć podręczną pakietów</span><span class="sxs-lookup"><span data-stu-id="b6237-472">Clear package caches</span></span>

<span data-ttu-id="b6237-473">Czasami działająca aplikacja kończy się natychmiast po uaktualnieniu zestaw .NET Core SDK na komputerze deweloperskim lub zmianie wersji pakietu w ramach aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b6237-473">Sometimes a functioning app fails immediately after upgrading either the .NET Core SDK on the development machine or changing package versions within the app.</span></span> <span data-ttu-id="b6237-474">W niektórych przypadkach niespójne pakietów może spowodować uszkodzenie aplikacji podczas przeprowadzania uaktualnienia głównych.</span><span class="sxs-lookup"><span data-stu-id="b6237-474">In some cases, incoherent packages may break an app when performing major upgrades.</span></span> <span data-ttu-id="b6237-475">Większość z tych problemów można naprawić, wykonując następujące instrukcje:</span><span class="sxs-lookup"><span data-stu-id="b6237-475">Most of these issues can be fixed by following these instructions:</span></span>

1. <span data-ttu-id="b6237-476">Usuń *bin* i *obj* folderów.</span><span class="sxs-lookup"><span data-stu-id="b6237-476">Delete the *bin* and *obj* folders.</span></span>
1. <span data-ttu-id="b6237-477">Wyczyść pamięć podręczną pakietów, wykonując `dotnet nuget locals all --clear` je z poziomu powłoki poleceń.</span><span class="sxs-lookup"><span data-stu-id="b6237-477">Clear the package caches by executing `dotnet nuget locals all --clear` from a command shell.</span></span>

   <span data-ttu-id="b6237-478">Czyszczenie pamięci podręcznych pakietów można także wykonać przy użyciu narzędzia [NuGet. exe](https://www.nuget.org/downloads) i wykonując polecenie `nuget locals all -clear`.</span><span class="sxs-lookup"><span data-stu-id="b6237-478">Clearing package caches can also be accomplished with the [nuget.exe](https://www.nuget.org/downloads) tool and executing the command `nuget locals all -clear`.</span></span> <span data-ttu-id="b6237-479">*nuget.exe* nie jest powiązane instalacji z pulpitu systemu operacyjnego Windows i należy uzyskać oddzielnie od [NuGet witryny sieci Web](https://www.nuget.org/downloads).</span><span class="sxs-lookup"><span data-stu-id="b6237-479">*nuget.exe* isn't a bundled install with the Windows desktop operating system and must be obtained separately from the [NuGet website](https://www.nuget.org/downloads).</span></span>

1. <span data-ttu-id="b6237-480">Przywróć i skompiluj ponownie projekt.</span><span class="sxs-lookup"><span data-stu-id="b6237-480">Restore and rebuild the project.</span></span>
1. <span data-ttu-id="b6237-481">Usuń wszystkie pliki z folderu wdrożenia na serwerze przed ponownym wdrożeniem aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b6237-481">Delete all of the files in the deployment folder on the server prior to redeploying the app.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b6237-482">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="b6237-482">Additional resources</span></span>

* <xref:test/troubleshoot>
* <xref:host-and-deploy/azure-iis-errors-reference>
* <xref:fundamentals/error-handling>
* <xref:host-and-deploy/aspnet-core-module>

### <a name="azure-documentation"></a><span data-ttu-id="b6237-483">Dokumentacja platformy Azure</span><span class="sxs-lookup"><span data-stu-id="b6237-483">Azure documentation</span></span>

* [<span data-ttu-id="b6237-484">Application Insights ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b6237-484">Application Insights for ASP.NET Core</span></span>](/azure/application-insights/app-insights-asp-net-core)
* [<span data-ttu-id="b6237-485">Sekcja zdalne debugowanie aplikacji sieci Web Rozwiązywanie problemów z aplikacją sieci Web w Azure App Service przy użyciu programu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b6237-485">Remote debugging web apps section of Troubleshoot a web app in Azure App Service using Visual Studio</span></span>](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio#remotedebug)
* [<span data-ttu-id="b6237-486">Omówienie diagnostyki Azure App Service</span><span class="sxs-lookup"><span data-stu-id="b6237-486">Azure App Service diagnostics overview</span></span>](/azure/app-service/app-service-diagnostics)
* [<span data-ttu-id="b6237-487">Instrukcje: Monitorowanie aplikacji w Azure App Service</span><span class="sxs-lookup"><span data-stu-id="b6237-487">How to: Monitor Apps in Azure App Service</span></span>](/azure/app-service/web-sites-monitor)
* [<span data-ttu-id="b6237-488">Rozwiązywanie problemów z aplikacją sieci web w usłudze Azure App Service przy użyciu programu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b6237-488">Troubleshoot a web app in Azure App Service using Visual Studio</span></span>](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio)
* [<span data-ttu-id="b6237-489">Rozwiązywanie problemów z błędami HTTP "502 złej Gateway" i "503 Usługa niedostępna" w usłudze Azure Web Apps</span><span class="sxs-lookup"><span data-stu-id="b6237-489">Troubleshoot HTTP errors of "502 bad gateway" and "503 service unavailable" in your Azure web apps</span></span>](/azure/app-service/app-service-web-troubleshoot-http-502-http-503)
* [<span data-ttu-id="b6237-490">Rozwiązywanie problemów z wydajnością aplikacji sieci Web w Azure App Service</span><span class="sxs-lookup"><span data-stu-id="b6237-490">Troubleshoot slow web app performance issues in Azure App Service</span></span>](/azure/app-service/app-service-web-troubleshoot-performance-degradation)
* [<span data-ttu-id="b6237-491">Często zadawane pytania dotyczące wydajności aplikacji dla Web Apps na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="b6237-491">Application performance FAQs for Web Apps in Azure</span></span>](/azure/app-service/app-service-web-availability-performance-application-issues-faq)
* [<span data-ttu-id="b6237-492">Piaskownica usługi Azure Web App (ograniczenia wykonywania App Service Runtime)</span><span class="sxs-lookup"><span data-stu-id="b6237-492">Azure Web App sandbox (App Service runtime execution limitations)</span></span>](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)
* [<span data-ttu-id="b6237-493">Piątek Azure: Azure App Service środowisko diagnostyczne i rozwiązywania problemów (wideo 12-minutowy)</span><span class="sxs-lookup"><span data-stu-id="b6237-493">Azure Friday: Azure App Service Diagnostic and Troubleshooting Experience (12-minute video)</span></span>](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)

### <a name="visual-studio-documentation"></a><span data-ttu-id="b6237-494">Dokumentacja programu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b6237-494">Visual Studio documentation</span></span>

* [<span data-ttu-id="b6237-495">ASP.NET Core debugowania zdalnego na serwerze IIS na platformie Azure w programie Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="b6237-495">Remote Debug ASP.NET Core on IIS in Azure in Visual Studio 2017</span></span>](/visualstudio/debugger/remote-debugging-azure)
* [<span data-ttu-id="b6237-496">Zdalne debugowanie ASP.NET Core na zdalnym komputerze IIS w programie Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="b6237-496">Remote Debug ASP.NET Core on a Remote IIS Computer in Visual Studio 2017</span></span>](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer)
* [<span data-ttu-id="b6237-497">Naucz się debugować przy użyciu programu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b6237-497">Learn to debug using Visual Studio</span></span>](/visualstudio/debugger/getting-started-with-the-debugger)

### <a name="visual-studio-code-documentation"></a><span data-ttu-id="b6237-498">Dokumentacja Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b6237-498">Visual Studio Code documentation</span></span>

* [<span data-ttu-id="b6237-499">Debugowanie za pomocą programu Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b6237-499">Debugging with Visual Studio Code</span></span>](https://code.visualstudio.com/docs/editor/debugging)
