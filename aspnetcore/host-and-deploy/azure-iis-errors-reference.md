---
title: Dokumentacja typowych błędów dla usługi Azure App Service i IIS za pomocą programu ASP.NET Core
author: guardrex
description: Uzyskaj porady dotyczące rozwiązywania problemów dla typowych błędów, odnośnie do hostowania aplikacji platformy ASP.NET Core na usługi aplikacji Azure i usług IIS.
ms.author: riande
ms.custom: mvc
ms.date: 02/05/2019
uid: host-and-deploy/azure-iis-errors-reference
ms.openlocfilehash: 976f7e3fbeab9e81ba99e2dd7d09a892b854651b
ms.sourcegitcommit: 3c2ba9a0d833d2a096d9d800ba67a1a7f9491af0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55854464"
---
# <a name="common-errors-reference-for-azure-app-service-and-iis-with-aspnet-core"></a><span data-ttu-id="36d34-103">Dokumentacja typowych błędów dla usługi Azure App Service i IIS za pomocą programu ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="36d34-103">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>

<span data-ttu-id="36d34-104">Przez [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="36d34-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="36d34-105">W tym temacie oferuje porady dotyczące rozwiązywania problemów dla typowych błędów, odnośnie do hostowania aplikacji platformy ASP.NET Core na usługi aplikacji Azure i usług IIS.</span><span class="sxs-lookup"><span data-stu-id="36d34-105">This topic offers troubleshooting advice for common errors when hosting ASP.NET Core apps on Azure Apps Service and IIS.</span></span>

<span data-ttu-id="36d34-106">Zbierz wymienione poniżej informacje.</span><span class="sxs-lookup"><span data-stu-id="36d34-106">Collect the following information:</span></span>

* <span data-ttu-id="36d34-107">Zachowanie przeglądarki (stan kodu i komunikatu o błędzie)</span><span class="sxs-lookup"><span data-stu-id="36d34-107">Browser behavior (status code and error message)</span></span>
* <span data-ttu-id="36d34-108">Wpisy dziennika zdarzeń aplikacji</span><span class="sxs-lookup"><span data-stu-id="36d34-108">Application Event Log entries</span></span>
  * <span data-ttu-id="36d34-109">Usługa Azure App Service &ndash; zobacz <xref:host-and-deploy/azure-apps/troubleshoot>.</span><span class="sxs-lookup"><span data-stu-id="36d34-109">Azure App Service &ndash; See <xref:host-and-deploy/azure-apps/troubleshoot>.</span></span>
  * <span data-ttu-id="36d34-110">IIS</span><span class="sxs-lookup"><span data-stu-id="36d34-110">IIS</span></span>
    1. <span data-ttu-id="36d34-111">Wybierz **Start** na **Windows** menu, typ *Podgląd zdarzeń*i naciśnij klawisz **Enter**.</span><span class="sxs-lookup"><span data-stu-id="36d34-111">Select **Start** on the **Windows** menu, type *Event Viewer*, and press **Enter**.</span></span>
    1. <span data-ttu-id="36d34-112">Po **Podgląd zdarzeń** zostanie otwarta, rozwiń węzeł **Dzienniki Windows** > **aplikacji** na pasku bocznym.</span><span class="sxs-lookup"><span data-stu-id="36d34-112">After the **Event Viewer** opens, expand **Windows Logs** > **Application** in the sidebar.</span></span>
* <span data-ttu-id="36d34-113">Wpisy dziennika platformy ASP.NET Core modułu stdout i debugowania</span><span class="sxs-lookup"><span data-stu-id="36d34-113">ASP.NET Core Module stdout and debug log entries</span></span>
  * <span data-ttu-id="36d34-114">Usługa Azure App Service &ndash; zobacz <xref:host-and-deploy/azure-apps/troubleshoot>.</span><span class="sxs-lookup"><span data-stu-id="36d34-114">Azure App Service &ndash; See <xref:host-and-deploy/azure-apps/troubleshoot>.</span></span>
  * <span data-ttu-id="36d34-115">Usługi IIS &ndash; postępuj zgodnie z instrukcjami w [tworzenia i Przekierowanie dziennika](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection) i [rozszerzone dzienniki diagnostyczne](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) sekcjach tematu modułu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="36d34-115">IIS &ndash; Follow the instructions in the [Log creation and redirection](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection) and [Enhanced diagnostic logs](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) sections of the ASP.NET Core Module topic.</span></span>

<span data-ttu-id="36d34-116">Porównywanie informacji o błędzie do następujących typowych błędów.</span><span class="sxs-lookup"><span data-stu-id="36d34-116">Compare error information to the following common errors.</span></span> <span data-ttu-id="36d34-117">Jeśli zostanie znalezione dopasowanie, wykonaj porady dotyczące rozwiązywania problemów.</span><span class="sxs-lookup"><span data-stu-id="36d34-117">If a match is found, follow the troubleshooting advice.</span></span>

<span data-ttu-id="36d34-118">Lista błędów, w tym temacie nie jest wyczerpująca.</span><span class="sxs-lookup"><span data-stu-id="36d34-118">The list of errors in this topic isn't exhaustive.</span></span> <span data-ttu-id="36d34-119">Jeśli wystąpi błąd niewymienione w tym Otwórz nowy problem za pomocą **zawartości opinii** przycisk w dolnej części tego tematu zawiera szczegółowe instrukcje dotyczące sposobu odtwarzania błędu.</span><span class="sxs-lookup"><span data-stu-id="36d34-119">If you encounter an error not listed here, open a new issue using the **Content feedback** button at the bottom of this topic with detailed instructions on how to reproduce the error.</span></span>

[!INCLUDE[Azure App Service Preview Notice](../includes/azure-apps-preview-notice.md)]

## <a name="installer-unable-to-obtain-vc-redistributable"></a><span data-ttu-id="36d34-120">Instalator nie może uzyskać VC ++ do dystrybucji</span><span class="sxs-lookup"><span data-stu-id="36d34-120">Installer unable to obtain VC++ Redistributable</span></span>

* <span data-ttu-id="36d34-121">**Instalator wyjątek:** 0x80072EFD **--lub--** 0x80072f76 — nieokreślony błąd</span><span class="sxs-lookup"><span data-stu-id="36d34-121">**Installer Exception:** 0x80072efd **--OR--** 0x80072f76 - Unspecified error</span></span>

* <span data-ttu-id="36d34-122">**Wyjątek dziennika Instalatora&#8224;:** Błąd 0x80072efd **--lub--** 0x80072f76: Nie można uruchomić pakietu EXE</span><span class="sxs-lookup"><span data-stu-id="36d34-122">**Installer Log Exception&#8224;:** Error 0x80072efd **--OR--** 0x80072f76: Failed to execute EXE package</span></span>

  <span data-ttu-id="36d34-123">&#8224;Dziennik znajduje się w *C:\Users\{USER}\AppData\Local\Temp\dd_DotNetCoreWinSvrHosting__{TIMESTAMP}.log*.</span><span class="sxs-lookup"><span data-stu-id="36d34-123">&#8224;The log is located at *C:\Users\{USER}\AppData\Local\Temp\dd_DotNetCoreWinSvrHosting__{TIMESTAMP}.log*.</span></span>

<span data-ttu-id="36d34-124">Rozwiązywanie problemów:</span><span class="sxs-lookup"><span data-stu-id="36d34-124">Troubleshooting:</span></span>

<span data-ttu-id="36d34-125">Jeśli system nie ma dostępu do Internetu podczas [Instalowanie pakietu hostingu platformy .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle), ten wyjątek występuje w przypadku uzyskiwania Instalator będzie mógł *Microsoft Visual C++ 2015 Redistributable*.</span><span class="sxs-lookup"><span data-stu-id="36d34-125">If the system doesn't have Internet access while [installing the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle), this exception occurs when the installer is prevented from obtaining the *Microsoft Visual C++ 2015 Redistributable*.</span></span> <span data-ttu-id="36d34-126">Uzyskaj Instalator [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840).</span><span class="sxs-lookup"><span data-stu-id="36d34-126">Obtain an installer from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840).</span></span> <span data-ttu-id="36d34-127">Jeśli Instalator zakończy się niepowodzeniem, serwer może nie otrzymać wymagane do obsługi środowiska uruchomieniowego .NET Core [zależny od struktury wdrożenia (stacje)](/dotnet/core/deploying/#framework-dependent-deployments-fdd).</span><span class="sxs-lookup"><span data-stu-id="36d34-127">If the installer fails, the server may not receive the .NET Core runtime required to host a [framework-dependent deployment (FDD)](/dotnet/core/deploying/#framework-dependent-deployments-fdd).</span></span> <span data-ttu-id="36d34-128">Jeśli hostingu Dyskietki, upewnij się, że środowisko uruchomieniowe jest zainstalowany w **programy i funkcje** lub **aplikacje i funkcje**.</span><span class="sxs-lookup"><span data-stu-id="36d34-128">If hosting an FDD, confirm that the runtime is installed in **Programs & Features** or **Apps & features**.</span></span> <span data-ttu-id="36d34-129">Jeśli wymagana jest określonego środowiska uruchomieniowego, Pobierz środowisko uruchomieniowe z [archiwa Pobierz .NET](https://dotnet.microsoft.com/download/archives) i zainstaluj go na system.</span><span class="sxs-lookup"><span data-stu-id="36d34-129">If a specific runtime is required, download the runtime from the [.NET Download Archives](https://dotnet.microsoft.com/download/archives) and install it on the system.</span></span> <span data-ttu-id="36d34-130">Po zainstalowaniu środowiska uruchomieniowego, uruchom ponownie komputer lub uruchom ponownie usługi IIS, wykonując **net stop został /y** następuje **net start w3svc** z poziomu wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="36d34-130">After installing the runtime, restart the system or restart IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

## <a name="os-upgrade-removed-the-32-bit-aspnet-core-module"></a><span data-ttu-id="36d34-131">Uaktualnienie systemu operacyjnego usunięte 32-bitowych modułu ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="36d34-131">OS upgrade removed the 32-bit ASP.NET Core Module</span></span>

<span data-ttu-id="36d34-132">**Dziennik aplikacji:** Biblioteka DLL modułu **C:\WINDOWS\system32\inetsrv\aspnetcore.dll** udało się załadować.</span><span class="sxs-lookup"><span data-stu-id="36d34-132">**Application Log:** The Module DLL **C:\WINDOWS\system32\inetsrv\aspnetcore.dll** failed to load.</span></span> <span data-ttu-id="36d34-133">Dane są błędne.</span><span class="sxs-lookup"><span data-stu-id="36d34-133">The data is the error.</span></span>

<span data-ttu-id="36d34-134">Rozwiązywanie problemów:</span><span class="sxs-lookup"><span data-stu-id="36d34-134">Troubleshooting:</span></span>

<span data-ttu-id="36d34-135">Pliki systemu operacyjnego bez **C:\Windows\SysWOW64\inetsrv** katalogu nie są zachowywane w trakcie systemu operacyjnego uaktualnienia.</span><span class="sxs-lookup"><span data-stu-id="36d34-135">Non-OS files in the **C:\Windows\SysWOW64\inetsrv** directory aren't preserved during an OS upgrade.</span></span> <span data-ttu-id="36d34-136">Jeśli zainstalowano modułu ASP.NET Core przed uaktualnienie systemu operacyjnego, a następnie każdej puli aplikacji jest uruchamiana w trybie 32-bitowych po uaktualnieniu systemu operacyjnego, ten problem zostanie osiągnięty.</span><span class="sxs-lookup"><span data-stu-id="36d34-136">If the ASP.NET Core Module is installed prior to an OS upgrade and then any app pool is run in 32-bit mode after an OS upgrade, this issue is encountered.</span></span> <span data-ttu-id="36d34-137">Po uaktualnieniu systemu operacyjnego napraw modułu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="36d34-137">After an OS upgrade, repair the ASP.NET Core Module.</span></span> <span data-ttu-id="36d34-138">Zobacz [instalacji pakietu .NET Core hostingu](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span><span class="sxs-lookup"><span data-stu-id="36d34-138">See [Install the .NET Core Hosting bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span> <span data-ttu-id="36d34-139">Wybierz **naprawy** po uruchomieniu Instalatora.</span><span class="sxs-lookup"><span data-stu-id="36d34-139">Select **Repair** when the installer is run.</span></span>

## <a name="an-x86-app-is-deployed-but-the-app-pool-isnt-enabled-for-32-bit-apps"></a><span data-ttu-id="36d34-140">X86 aplikacja jest wdrożona, ale pula aplikacji nie jest włączony dla aplikacji 32-bitowych</span><span class="sxs-lookup"><span data-stu-id="36d34-140">An x86 app is deployed but the app pool isn't enabled for 32-bit apps</span></span>

* <span data-ttu-id="36d34-141">**Przeglądarka:** Błąd HTTP 500.30 — błąd w trakcie uruchamiania ANCM</span><span class="sxs-lookup"><span data-stu-id="36d34-141">**Browser:** HTTP Error 500.30 - ANCM In-Process Start Failure</span></span>

* <span data-ttu-id="36d34-142">**Dziennik aplikacji:** Aplikacja "/ LM/W3SVC/5/ROOT" za pomocą fizycznych głównego {PATH} trafień nieoczekiwany wyjątek zarządzany kod wyjątku = "0xe0434352".</span><span class="sxs-lookup"><span data-stu-id="36d34-142">**Application Log:** Application '/LM/W3SVC/5/ROOT' with physical root '{PATH}' hit unexpected managed exception, exception code = '0xe0434352'.</span></span> <span data-ttu-id="36d34-143">Sprawdź dzienniki stderr, aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="36d34-143">Please check the stderr logs for more information.</span></span> <span data-ttu-id="36d34-144">Aplikacja "/ LM/W3SVC/5/ROOT" za pomocą fizycznych głównego "{PATH}" nie można załadować środowiska clr i zarządzanych aplikacji.</span><span class="sxs-lookup"><span data-stu-id="36d34-144">Application '/LM/W3SVC/5/ROOT' with physical root '{PATH}' failed to load clr and managed application.</span></span> <span data-ttu-id="36d34-145">Wątek roboczy CLR przedwcześnie zakończył działanie</span><span class="sxs-lookup"><span data-stu-id="36d34-145">CLR worker thread exited prematurely</span></span>

* <span data-ttu-id="36d34-146">**ASP.NET Core modułu strumienia wyjściowego stdout dziennika:** Plik dziennika jest utworzony, ale puste.</span><span class="sxs-lookup"><span data-stu-id="36d34-146">**ASP.NET Core Module stdout Log:** The log file is created but empty.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="36d34-147">**Moduł ASP.NET Core dziennik debugowania:** Zwrócone HRESULT nie powiodło się: 0x8007023e</span><span class="sxs-lookup"><span data-stu-id="36d34-147">**ASP.NET Core Module Debug Log:** Failed HRESULT returned: 0x8007023e</span></span>

::: moniker-end

<span data-ttu-id="36d34-148">W tym scenariuszu jest to spowodowane zestawu SDK podczas publikowania aplikacja samodzielna.</span><span class="sxs-lookup"><span data-stu-id="36d34-148">This scenario is trapped by the SDK when publishing a self-contained app.</span></span> <span data-ttu-id="36d34-149">Zestaw SDK generuje błąd, jeśli RID nie jest zgodny platformy docelowej (na przykład `win10-x64` RID z `<PlatformTarget>x86</PlatformTarget>` w pliku projektu).</span><span class="sxs-lookup"><span data-stu-id="36d34-149">The SDK produces an error if the RID doesn't match the platform target (for example, `win10-x64` RID with `<PlatformTarget>x86</PlatformTarget>` in the project file).</span></span>

<span data-ttu-id="36d34-150">Rozwiązywanie problemów:</span><span class="sxs-lookup"><span data-stu-id="36d34-150">Troubleshooting:</span></span>

<span data-ttu-id="36d34-151">Dla x86 zależny od struktury wdrożenia (`<PlatformTarget>x86</PlatformTarget>`), Włącz puli aplikacji usług IIS dla aplikacji 32-bitowych.</span><span class="sxs-lookup"><span data-stu-id="36d34-151">For an x86 framework-dependent deployment (`<PlatformTarget>x86</PlatformTarget>`), enable the IIS app pool for 32-bit apps.</span></span> <span data-ttu-id="36d34-152">W Menedżerze usług IIS, otwórz puli aplikacji **Zaawansowane ustawienia** i ustaw **Włącz 32-bitowych aplikacji** do **True**.</span><span class="sxs-lookup"><span data-stu-id="36d34-152">In IIS Manager, open the app pool's **Advanced Settings** and set **Enable 32-Bit Applications** to **True**.</span></span>

## <a name="platform-conflicts-with-rid"></a><span data-ttu-id="36d34-153">Platforma jest w konflikcie z identyfikatorów RID</span><span class="sxs-lookup"><span data-stu-id="36d34-153">Platform conflicts with RID</span></span>

* <span data-ttu-id="36d34-154">**Przeglądarka:** Błąd HTTP 502.5 — niepowodzenia procesu</span><span class="sxs-lookup"><span data-stu-id="36d34-154">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="36d34-155">**Dziennik aplikacji:** Aplikacja "APPHOST-MACHINE/WEBROOT / {zestawu}" z certyfikatem głównym fizycznych "C:\{ścieżki}\' nie można uruchomić procesu przy użyciu wiersza polecenia" "C:\{ścieżki} {zestawu}. { plik exe | dll} "", kod błędu = "0x80004005: ff.</span><span class="sxs-lookup"><span data-stu-id="36d34-155">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' failed to start process with commandline '"C:\{PATH}{ASSEMBLY}.{exe|dll}" ', ErrorCode = '0x80004005 : ff.</span></span>

* <span data-ttu-id="36d34-156">**ASP.NET Core modułu strumienia wyjściowego stdout dziennika:** Nieobsługiwany wyjątek: System.BadImageFormatException: Nie można załadować pliku lub zestawu "{zestawu} .dll".</span><span class="sxs-lookup"><span data-stu-id="36d34-156">**ASP.NET Core Module stdout Log:** Unhandled Exception: System.BadImageFormatException: Could not load file or assembly '{ASSEMBLY}.dll'.</span></span> <span data-ttu-id="36d34-157">Próbowano załadować program w niepoprawnym formacie.</span><span class="sxs-lookup"><span data-stu-id="36d34-157">An attempt was made to load a program with an incorrect format.</span></span>

<span data-ttu-id="36d34-158">Rozwiązywanie problemów:</span><span class="sxs-lookup"><span data-stu-id="36d34-158">Troubleshooting:</span></span>

* <span data-ttu-id="36d34-159">Upewnij się, że aplikacja działa lokalnie na Kestrel.</span><span class="sxs-lookup"><span data-stu-id="36d34-159">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="36d34-160">Niepowodzenia procesu może wynikać z problemu w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="36d34-160">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="36d34-161">Aby uzyskać więcej informacji, zobacz [Rozwiązywanie problemów z (IIS)](xref:host-and-deploy/iis/troubleshoot) lub [rozwiązywania problemów (Azure App Service)](xref:host-and-deploy/azure-apps/troubleshoot).</span><span class="sxs-lookup"><span data-stu-id="36d34-161">For more information, see [Troubleshoot (IIS)](xref:host-and-deploy/iis/troubleshoot) or [Troubleshoot (Azure App Service)](xref:host-and-deploy/azure-apps/troubleshoot).</span></span>

* <span data-ttu-id="36d34-162">Jeśli ten wyjątek występuje podczas wdrażania aplikacji platformy Azure, podczas uaktualniania aplikacji i wdrażanie zestawów nowszej ręcznie usunąć wszystkie pliki z poprzedniego wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="36d34-162">If this exception occurs for an Azure Apps deployment when upgrading an app and deploying newer assemblies, manually delete all files from the prior deployment.</span></span> <span data-ttu-id="36d34-163">Lingering niezgodne zestawów może spowodować `System.BadImageFormatException` wyjątku w przypadku wdrażania uaktualnionego aplikacji.</span><span class="sxs-lookup"><span data-stu-id="36d34-163">Lingering incompatible assemblies can result in a `System.BadImageFormatException` exception when deploying an upgraded app.</span></span>

## <a name="uri-endpoint-wrong-or-stopped-website"></a><span data-ttu-id="36d34-164">Identyfikator URI punktu końcowego problem lub zatrzymana witryny sieci Web</span><span class="sxs-lookup"><span data-stu-id="36d34-164">URI endpoint wrong or stopped website</span></span>

* <span data-ttu-id="36d34-165">**Przeglądarka:** ERR_CONNECTION_REFUSED **--lub--** nie można połączyć z</span><span class="sxs-lookup"><span data-stu-id="36d34-165">**Browser:** ERR_CONNECTION_REFUSED **--OR--** Unable to connect</span></span>

* <span data-ttu-id="36d34-166">**Dziennik aplikacji:** Brak wpisu</span><span class="sxs-lookup"><span data-stu-id="36d34-166">**Application Log:** No entry</span></span>

* <span data-ttu-id="36d34-167">**ASP.NET Core modułu strumienia wyjściowego stdout dziennika:** Plik dziennika nie jest tworzony.</span><span class="sxs-lookup"><span data-stu-id="36d34-167">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="36d34-168">**Moduł ASP.NET Core dziennik debugowania:** Plik dziennika nie jest tworzony.</span><span class="sxs-lookup"><span data-stu-id="36d34-168">**ASP.NET Core Module Debug Log:** The log file isn't created.</span></span>

::: moniker-end

<span data-ttu-id="36d34-169">Rozwiązywanie problemów:</span><span class="sxs-lookup"><span data-stu-id="36d34-169">Troubleshooting:</span></span>

* <span data-ttu-id="36d34-170">Upewnij się, że właściwego punktu końcowego identyfikator URI aplikacji jest używana.</span><span class="sxs-lookup"><span data-stu-id="36d34-170">Confirm the correct URI endpoint for the app is in use.</span></span> <span data-ttu-id="36d34-171">Sprawdź wiązania.</span><span class="sxs-lookup"><span data-stu-id="36d34-171">Check the bindings.</span></span>

* <span data-ttu-id="36d34-172">Upewnij się, że witryny sieci Web usług IIS nie znajduje się w *zatrzymane* stanu.</span><span class="sxs-lookup"><span data-stu-id="36d34-172">Confirm that the IIS website isn't in the *Stopped* state.</span></span>

## <a name="corewebengine-or-w3svc-server-features-disabled"></a><span data-ttu-id="36d34-173">Serwer CoreWebEngine lub W3SVC funkcje wyłączone</span><span class="sxs-lookup"><span data-stu-id="36d34-173">CoreWebEngine or W3SVC server features disabled</span></span>

<span data-ttu-id="36d34-174">**Wyjątek systemu operacyjnego:** Użyj modułu ASP.NET Core można zainstalować funkcje usług IIS 7.0 CoreWebEngine i W3SVC.</span><span class="sxs-lookup"><span data-stu-id="36d34-174">**OS Exception:** The IIS 7.0 CoreWebEngine and W3SVC features must be installed to use the ASP.NET Core Module.</span></span>

<span data-ttu-id="36d34-175">Rozwiązywanie problemów:</span><span class="sxs-lookup"><span data-stu-id="36d34-175">Troubleshooting:</span></span>

<span data-ttu-id="36d34-176">Upewnij się, czy są włączone odpowiednie role i funkcje.</span><span class="sxs-lookup"><span data-stu-id="36d34-176">Confirm that the proper role and features are enabled.</span></span> <span data-ttu-id="36d34-177">Zobacz [konfiguracji programu IIS](xref:host-and-deploy/iis/index#iis-configuration).</span><span class="sxs-lookup"><span data-stu-id="36d34-177">See [IIS Configuration](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

## <a name="incorrect-website-physical-path-or-app-missing"></a><span data-ttu-id="36d34-178">Ścieżka fizyczna niepoprawna witryna sieci Web lub Brak aplikacji</span><span class="sxs-lookup"><span data-stu-id="36d34-178">Incorrect website physical path or app missing</span></span>

* <span data-ttu-id="36d34-179">**Przeglądarka:** 403 Zabroniony — odmowa dostępu **--lub--** zabronione 403.14 — serwer sieci Web jest skonfigurowany, aby nie wyświetlać listę zawartości tego katalogu.</span><span class="sxs-lookup"><span data-stu-id="36d34-179">**Browser:** 403 Forbidden - Access is denied **--OR--** 403.14 Forbidden - The Web server is configured to not list the contents of this directory.</span></span>

* <span data-ttu-id="36d34-180">**Dziennik aplikacji:** Brak wpisu</span><span class="sxs-lookup"><span data-stu-id="36d34-180">**Application Log:** No entry</span></span>

* <span data-ttu-id="36d34-181">**ASP.NET Core modułu strumienia wyjściowego stdout dziennika:** Plik dziennika nie jest tworzony.</span><span class="sxs-lookup"><span data-stu-id="36d34-181">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="36d34-182">**Moduł ASP.NET Core dziennik debugowania:** Plik dziennika nie jest tworzony.</span><span class="sxs-lookup"><span data-stu-id="36d34-182">**ASP.NET Core Module Debug Log:** The log file isn't created.</span></span>

::: moniker-end

<span data-ttu-id="36d34-183">Rozwiązywanie problemów:</span><span class="sxs-lookup"><span data-stu-id="36d34-183">Troubleshooting:</span></span>

<span data-ttu-id="36d34-184">Sprawdź witrynę sieci Web usług IIS **podstawowych ustawień** i folderu fizycznego aplikacji.</span><span class="sxs-lookup"><span data-stu-id="36d34-184">Check the IIS website **Basic Settings** and the physical app folder.</span></span> <span data-ttu-id="36d34-185">Upewnij się, że aplikacja znajduje się w folderze w witrynie sieci Web usług IIS **ścieżkę fizyczną**.</span><span class="sxs-lookup"><span data-stu-id="36d34-185">Confirm that the app is in the folder at the IIS website **Physical path**.</span></span>

## <a name="incorrect-role-aspnet-core-module-not-installed-or-incorrect-permissions"></a><span data-ttu-id="36d34-186">Nieprawidłowa rola, nie zainstalowano modułu ASP.NET Core lub niepoprawne uprawnienia</span><span class="sxs-lookup"><span data-stu-id="36d34-186">Incorrect role, ASP.NET Core Module not installed, or incorrect permissions</span></span>

* <span data-ttu-id="36d34-187">**Przeglądarka:** 500.19 — wewnętrzny błąd serwera — żądana strona nie są dostępne, ponieważ odpowiednie dane konfiguracyjne dla strony jest nieprawidłowy.</span><span class="sxs-lookup"><span data-stu-id="36d34-187">**Browser:** 500.19 Internal Server Error - The requested page cannot be accessed because the related configuration data for the page is invalid.</span></span> <span data-ttu-id="36d34-188">**--LUB--** nie można wyświetlić tej strony</span><span class="sxs-lookup"><span data-stu-id="36d34-188">**--OR--** This page can't be displayed</span></span>

* <span data-ttu-id="36d34-189">**Dziennik aplikacji:** Brak wpisu</span><span class="sxs-lookup"><span data-stu-id="36d34-189">**Application Log:** No entry</span></span>

* <span data-ttu-id="36d34-190">**ASP.NET Core modułu strumienia wyjściowego stdout dziennika:** Plik dziennika nie jest tworzony.</span><span class="sxs-lookup"><span data-stu-id="36d34-190">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="36d34-191">**Moduł ASP.NET Core dziennik debugowania:** Plik dziennika nie jest tworzony.</span><span class="sxs-lookup"><span data-stu-id="36d34-191">**ASP.NET Core Module Debug Log:** The log file isn't created.</span></span>

::: moniker-end

<span data-ttu-id="36d34-192">Rozwiązywanie problemów:</span><span class="sxs-lookup"><span data-stu-id="36d34-192">Troubleshooting:</span></span>

* <span data-ttu-id="36d34-193">Upewnij się, że włączono właściwej roli.</span><span class="sxs-lookup"><span data-stu-id="36d34-193">Confirm that the proper role is enabled.</span></span> <span data-ttu-id="36d34-194">Zobacz [konfiguracji programu IIS](xref:host-and-deploy/iis/index#iis-configuration).</span><span class="sxs-lookup"><span data-stu-id="36d34-194">See [IIS Configuration](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

* <span data-ttu-id="36d34-195">Otwórz **programy i funkcje** lub **aplikacje i funkcje** i upewnij się, że **systemu Windows serwer obsługujący** jest zainstalowany.</span><span class="sxs-lookup"><span data-stu-id="36d34-195">Open **Programs & Features** or **Apps & features** and confirm that **Windows Server Hosting** is installed.</span></span> <span data-ttu-id="36d34-196">Jeśli **systemu Windows serwer obsługujący** nie ma na liście zainstalowanych programów, Pobierz i zainstaluj pakiet hostingu platformy .NET Core.</span><span class="sxs-lookup"><span data-stu-id="36d34-196">If **Windows Server Hosting** isn't present in the list of installed programs, download and install the .NET Core Hosting Bundle.</span></span>

  [<span data-ttu-id="36d34-197">Bieżący Instalatora pakietu hostingu programu .NET Core (pobieranie bezpośrednie)</span><span class="sxs-lookup"><span data-stu-id="36d34-197">Current .NET Core Hosting Bundle installer (direct download)</span></span>](https://www.microsoft.com/net/permalink/dotnetcore-current-windows-runtime-bundle-installer)

  <span data-ttu-id="36d34-198">Aby uzyskać więcej informacji, zobacz [jest instalowany pakiet hostingu platformy .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span><span class="sxs-lookup"><span data-stu-id="36d34-198">For more information, see [Install the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span>

* <span data-ttu-id="36d34-199">Upewnij się, że **puli aplikacji** > **Model procesu** > **tożsamości** ustawiono **ApplicationPoolIdentity** lub tożsamość niestandardowa ma odpowiednie uprawnienia dostępu do folderu wdrożenia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="36d34-199">Make sure that the **Application Pool** > **Process Model** > **Identity** is set to **ApplicationPoolIdentity** or the custom identity has the correct permissions to access the app's deployment folder.</span></span>

## <a name="incorrect-processpath-missing-path-variable-hosting-bundle-not-installed-systemiis-not-restarted-vc-redistributable-not-installed-or-dotnetexe-access-violation"></a><span data-ttu-id="36d34-200">Niepoprawne processPath, Brak zmiennej PATH, hostingu pakietu nie jest zainstalowany, nie uruchomiono ponownie system/IIS, VC ++ Redistributable nie jest zainstalowany lub naruszenie zasad dostępu dotnet.exe</span><span class="sxs-lookup"><span data-stu-id="36d34-200">Incorrect processPath, missing PATH variable, Hosting Bundle not installed, system/IIS not restarted, VC++ Redistributable not installed, or dotnet.exe access violation</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="36d34-201">**Przeglądarka:** Błąd HTTP 500.0 - ANCM w procesie programu obsługi błędu ładowania</span><span class="sxs-lookup"><span data-stu-id="36d34-201">**Browser:** HTTP Error 500.0 - ANCM In-Process Handler Load Failure</span></span>

* <span data-ttu-id="36d34-202">**Dziennik aplikacji:** Aplikacja "MACHINE/WEBROOT/APPHOST / {zestawu}" z certyfikatem głównym fizycznych "C:\{ścieżki}\' nie można uruchomić procesu przy użyciu wiersza polecenia" "{...}"</span><span class="sxs-lookup"><span data-stu-id="36d34-202">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' failed to start process with commandline '"{...}"</span></span> <span data-ttu-id="36d34-203">', ErrorCode = '0x80070002 : 0.</span><span class="sxs-lookup"><span data-stu-id="36d34-203">', ErrorCode = '0x80070002 : 0.</span></span> <span data-ttu-id="36d34-204">Aplikacja {PATH} nie był w stanie uruchomić.</span><span class="sxs-lookup"><span data-stu-id="36d34-204">Application '{PATH}' wasn't able to start.</span></span> <span data-ttu-id="36d34-205">Nie można odnaleźć pliku wykonywalnego w lokalizacji {PATH}.</span><span class="sxs-lookup"><span data-stu-id="36d34-205">Executable was not found at '{PATH}'.</span></span> <span data-ttu-id="36d34-206">Nie można uruchomić aplikacji "/ LM/W3SVC/2/ROOT", kod błędu "0x8007023e".</span><span class="sxs-lookup"><span data-stu-id="36d34-206">Failed to start application '/LM/W3SVC/2/ROOT', ErrorCode '0x8007023e'.</span></span>

* <span data-ttu-id="36d34-207">**ASP.NET Core modułu strumienia wyjściowego stdout dziennika:** Plik dziennika nie jest tworzony.</span><span class="sxs-lookup"><span data-stu-id="36d34-207">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

* <span data-ttu-id="36d34-208">**Moduł ASP.NET Core dziennik debugowania:** Dziennik zdarzeń: "Aplikacja"{PATH}"nie był w stanie uruchomić.</span><span class="sxs-lookup"><span data-stu-id="36d34-208">**ASP.NET Core Module Debug Log:** Event Log: 'Application '{PATH}' wasn't able to start.</span></span> <span data-ttu-id="36d34-209">Nie można odnaleźć pliku wykonywalnego w lokalizacji {PATH}.</span><span class="sxs-lookup"><span data-stu-id="36d34-209">Executable was not found at '{PATH}'.</span></span> <span data-ttu-id="36d34-210">Zwrócone HRESULT nie powiodło się: 0x8007023e</span><span class="sxs-lookup"><span data-stu-id="36d34-210">Failed HRESULT returned: 0x8007023e</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="36d34-211">**Przeglądarka:** Błąd HTTP 502.5 — niepowodzenia procesu</span><span class="sxs-lookup"><span data-stu-id="36d34-211">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="36d34-212">**Dziennik aplikacji:** Aplikacja "MACHINE/WEBROOT/APPHOST / {zestawu}" z certyfikatem głównym fizycznych "C:\{ścieżki}\' nie można uruchomić procesu przy użyciu wiersza polecenia" "{...}"</span><span class="sxs-lookup"><span data-stu-id="36d34-212">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' failed to start process with commandline '"{...}"</span></span> <span data-ttu-id="36d34-213">', ErrorCode = '0x80070002 : 0.</span><span class="sxs-lookup"><span data-stu-id="36d34-213">', ErrorCode = '0x80070002 : 0.</span></span>

* <span data-ttu-id="36d34-214">**ASP.NET Core modułu strumienia wyjściowego stdout dziennika:** Plik dziennika jest utworzony, ale puste.</span><span class="sxs-lookup"><span data-stu-id="36d34-214">**ASP.NET Core Module stdout Log:** The log file is created but empty.</span></span>

::: moniker-end

<span data-ttu-id="36d34-215">Rozwiązywanie problemów:</span><span class="sxs-lookup"><span data-stu-id="36d34-215">Troubleshooting:</span></span>

* <span data-ttu-id="36d34-216">Upewnij się, że aplikacja działa lokalnie na Kestrel.</span><span class="sxs-lookup"><span data-stu-id="36d34-216">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="36d34-217">Niepowodzenia procesu może wynikać z problemu w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="36d34-217">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="36d34-218">Aby uzyskać więcej informacji, zobacz [Rozwiązywanie problemów z (IIS)](xref:host-and-deploy/iis/troubleshoot) lub [rozwiązywania problemów (Azure App Service)](xref:host-and-deploy/azure-apps/troubleshoot).</span><span class="sxs-lookup"><span data-stu-id="36d34-218">For more information, see [Troubleshoot (IIS)](xref:host-and-deploy/iis/troubleshoot) or [Troubleshoot (Azure App Service)](xref:host-and-deploy/azure-apps/troubleshoot).</span></span>

* <span data-ttu-id="36d34-219">Sprawdź *processPath* atrybutu na `<aspNetCore>` element *web.config* aby upewnić się, że jest `dotnet` wdrożenia zależny od struktury (stacje) lub `.\{ASSEMBLY}.exe` dla [niezależna wdrożenia (— SCD)](/dotnet/core/deploying/#self-contained-deployments-scd).</span><span class="sxs-lookup"><span data-stu-id="36d34-219">Check the *processPath* attribute on the `<aspNetCore>` element in *web.config* to confirm that it's `dotnet` for a framework-dependent deployment (FDD) or `.\{ASSEMBLY}.exe` for a [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd).</span></span>

* <span data-ttu-id="36d34-220">Dla Dyskietki *dotnet.exe* może nie być dostępne za pośrednictwem ustawienia ścieżki.</span><span class="sxs-lookup"><span data-stu-id="36d34-220">For an FDD, *dotnet.exe* might not be accessible via the PATH settings.</span></span> <span data-ttu-id="36d34-221">Upewnij się, że *C:\Program Files\dotnet\\*  istnieje w ustawieniach ścieżki systemowej.</span><span class="sxs-lookup"><span data-stu-id="36d34-221">Confirm that *C:\Program Files\dotnet\\* exists in the System PATH settings.</span></span>

* <span data-ttu-id="36d34-222">Dla Dyskietki *dotnet.exe* może nie być dostępny dla tożsamości puli aplikacji.</span><span class="sxs-lookup"><span data-stu-id="36d34-222">For an FDD, *dotnet.exe* might not be accessible for the user identity of the app pool.</span></span> <span data-ttu-id="36d34-223">Upewnij się, że tożsamość puli aplikacji ma dostęp do *C:\Program Files\dotnet* katalogu.</span><span class="sxs-lookup"><span data-stu-id="36d34-223">Confirm that the app pool user identity has access to the *C:\Program Files\dotnet* directory.</span></span> <span data-ttu-id="36d34-224">Upewnij się, że nie istnieją żadne reguły odmowy skonfigurowane dla tożsamości użytkownika puli aplikacji na *C:\Program Files\dotnet* i katalogów aplikacji.</span><span class="sxs-lookup"><span data-stu-id="36d34-224">Confirm that there are no deny rules configured for the app pool user identity on the *C:\Program Files\dotnet* and app directories.</span></span>

* <span data-ttu-id="36d34-225">STACJE zostały wdrożone, oraz platformy .NET Core zainstalowane bez ponownego uruchamiania usług IIS.</span><span class="sxs-lookup"><span data-stu-id="36d34-225">An FDD may have been deployed and .NET Core installed without restarting IIS.</span></span> <span data-ttu-id="36d34-226">Uruchom ponownie serwer, albo ponownego uruchomienia usług IIS, wykonując **net stop został /y** następuje **net start w3svc** z poziomu wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="36d34-226">Either restart the server or restart IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

* <span data-ttu-id="36d34-227">Dyskietki mogą wdrożyć bez konieczności instalowania środowiska uruchomieniowego .NET Core w systemie hostingu.</span><span class="sxs-lookup"><span data-stu-id="36d34-227">An FDD may have been deployed without installing the .NET Core runtime on the hosting system.</span></span> <span data-ttu-id="36d34-228">Jeśli nie zainstalowano środowisko uruchomieniowe platformy .NET Core, uruchom **Instalatora programu .NET Core hostingu pakietu** w systemie.</span><span class="sxs-lookup"><span data-stu-id="36d34-228">If the .NET Core runtime hasn't been installed, run the **.NET Core Hosting Bundle installer** on the system.</span></span>

  [<span data-ttu-id="36d34-229">Bieżący Instalatora pakietu hostingu programu .NET Core (pobieranie bezpośrednie)</span><span class="sxs-lookup"><span data-stu-id="36d34-229">Current .NET Core Hosting Bundle installer (direct download)</span></span>](https://www.microsoft.com/net/permalink/dotnetcore-current-windows-runtime-bundle-installer)

  <span data-ttu-id="36d34-230">Aby uzyskać więcej informacji, zobacz [jest instalowany pakiet hostingu platformy .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span><span class="sxs-lookup"><span data-stu-id="36d34-230">For more information, see [Install the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span>

  <span data-ttu-id="36d34-231">Jeśli wymagana jest określonego środowiska uruchomieniowego, Pobierz środowisko uruchomieniowe z [archiwa Pobierz .NET](https://dotnet.microsoft.com/download/archives) i zainstaluj go na system.</span><span class="sxs-lookup"><span data-stu-id="36d34-231">If a specific runtime is required, download the runtime from the [.NET Download Archives](https://dotnet.microsoft.com/download/archives) and install it on the system.</span></span> <span data-ttu-id="36d34-232">Zakończ instalację, ponowne uruchamianie systemu lub ponowne uruchomienie usług IIS, wykonując **net stop został /y** następuje **net start w3svc** z poziomu wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="36d34-232">Complete the installation by restarting the system or restarting IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

* <span data-ttu-id="36d34-233">STACJE zostały wdrożone i *Microsoft Visual C++ 2015 Redistributable (x64)* nie jest zainstalowany w systemie.</span><span class="sxs-lookup"><span data-stu-id="36d34-233">An FDD may have been deployed and the *Microsoft Visual C++ 2015 Redistributable (x64)* isn't installed on the system.</span></span> <span data-ttu-id="36d34-234">Uzyskaj Instalator [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840).</span><span class="sxs-lookup"><span data-stu-id="36d34-234">Obtain an installer from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840).</span></span>

## <a name="incorrect-arguments-of-aspnetcore-element"></a><span data-ttu-id="36d34-235">Nieprawidłowe argumenty \<aspNetCore > element</span><span class="sxs-lookup"><span data-stu-id="36d34-235">Incorrect arguments of \<aspNetCore> element</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="36d34-236">**Przeglądarka:** Błąd HTTP 500.0 - ANCM w procesie programu obsługi błędu ładowania</span><span class="sxs-lookup"><span data-stu-id="36d34-236">**Browser:** HTTP Error 500.0 - ANCM In-Process Handler Load Failure</span></span>

* <span data-ttu-id="36d34-237">**Dziennik aplikacji:** Wywoływanie hostfxr można znaleźć programu obsługi żądania inprocess nie powiodła się bez znajdowanie wszelkie zależności natywnych.</span><span class="sxs-lookup"><span data-stu-id="36d34-237">**Application Log:** Invoking hostfxr to find the inprocess request handler failed without finding any native dependencies.</span></span> <span data-ttu-id="36d34-238">To najprawdopodobniej oznacza, że aplikacja jest nieprawidłowo skonfigurowane, sprawdź, czy wersje pakietów Microsoft.NetCore.App i Microsoft.AspNetCore.App są objęci aplikacji, które są zainstalowane na komputerze.</span><span class="sxs-lookup"><span data-stu-id="36d34-238">This most likely means the app is misconfigured, please check the versions of Microsoft.NetCore.App and Microsoft.AspNetCore.App that are targeted by the application and are installed on the machine.</span></span> <span data-ttu-id="36d34-239">Nie można odnaleźć inprocess żądania obsługi.</span><span class="sxs-lookup"><span data-stu-id="36d34-239">Could not find inprocess request handler.</span></span> <span data-ttu-id="36d34-240">Przechwycone dane wyjściowe z wywołaniem hostfxr: Czy chodziło Ci o polecenia zestawu SDK platformy dotnet?</span><span class="sxs-lookup"><span data-stu-id="36d34-240">Captured output from invoking hostfxr: Did you mean to run dotnet SDK commands?</span></span> <span data-ttu-id="36d34-241">Zainstaluj zestaw SDK platformy dotnet z: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409 Nie można uruchomić aplikacji "/ LM/W3SVC/3/ROOT", kod błędu "0x8000ffff".</span><span class="sxs-lookup"><span data-stu-id="36d34-241">Please install dotnet SDK from: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409 Failed to start application '/LM/W3SVC/3/ROOT', ErrorCode '0x8000ffff'.</span></span>

* <span data-ttu-id="36d34-242">**ASP.NET Core modułu strumienia wyjściowego stdout dziennika:** Czy chodziło Ci o polecenia zestawu SDK platformy dotnet?</span><span class="sxs-lookup"><span data-stu-id="36d34-242">**ASP.NET Core Module stdout Log:** Did you mean to run dotnet SDK commands?</span></span> <span data-ttu-id="36d34-243">Zainstaluj zestaw SDK platformy dotnet z: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409</span><span class="sxs-lookup"><span data-stu-id="36d34-243">Please install dotnet SDK from: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409</span></span>

* <span data-ttu-id="36d34-244">**Moduł ASP.NET Core dziennik debugowania:** Wywoływanie hostfxr można znaleźć programu obsługi żądania inprocess nie powiodła się bez znajdowanie wszelkie zależności natywnych.</span><span class="sxs-lookup"><span data-stu-id="36d34-244">**ASP.NET Core Module Debug Log:** Invoking hostfxr to find the inprocess request handler failed without finding any native dependencies.</span></span> <span data-ttu-id="36d34-245">To najprawdopodobniej oznacza, że aplikacja jest nieprawidłowo skonfigurowane, sprawdź, czy wersje pakietów Microsoft.NetCore.App i Microsoft.AspNetCore.App są objęci aplikacji, które są zainstalowane na komputerze.</span><span class="sxs-lookup"><span data-stu-id="36d34-245">This most likely means the app is misconfigured, please check the versions of Microsoft.NetCore.App and Microsoft.AspNetCore.App that are targeted by the application and are installed on the machine.</span></span> <span data-ttu-id="36d34-246">Zwrócone HRESULT nie powiodło się: 0x8000ffff nie można odnaleźć inprocess żądania obsługi.</span><span class="sxs-lookup"><span data-stu-id="36d34-246">Failed HRESULT returned: 0x8000ffff Could not find inprocess request handler.</span></span> <span data-ttu-id="36d34-247">Przechwycone dane wyjściowe z wywołaniem hostfxr: Czy chodziło Ci o polecenia zestawu SDK platformy dotnet?</span><span class="sxs-lookup"><span data-stu-id="36d34-247">Captured output from invoking hostfxr: Did you mean to run dotnet SDK commands?</span></span> <span data-ttu-id="36d34-248">Zainstaluj zestaw SDK platformy dotnet z: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409 Zwrócone HRESULT nie powiodło się: 0x8000ffff</span><span class="sxs-lookup"><span data-stu-id="36d34-248">Please install dotnet SDK from: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409 Failed HRESULT returned: 0x8000ffff</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="36d34-249">**Przeglądarka:** Błąd HTTP 502.5 — niepowodzenia procesu</span><span class="sxs-lookup"><span data-stu-id="36d34-249">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="36d34-250">**Dziennik aplikacji:** Aplikacja "MACHINE/WEBROOT/APPHOST / {zestawu}" z certyfikatem głównym fizycznych "C:\{ścieżki}\' nie można uruchomić procesu przy użyciu wiersza polecenia" "dotnet".\{ Plik .dll zestawu} ", kod błędu =" 0x80004005: 80008081.</span><span class="sxs-lookup"><span data-stu-id="36d34-250">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' failed to start process with commandline '"dotnet" .\{ASSEMBLY}.dll', ErrorCode = '0x80004005 : 80008081.</span></span>

* <span data-ttu-id="36d34-251">**ASP.NET Core modułu strumienia wyjściowego stdout dziennika:** Wykonywanie aplikacji nie istnieje: 'PATH\{ASSEMBLY}.dll'</span><span class="sxs-lookup"><span data-stu-id="36d34-251">**ASP.NET Core Module stdout Log:** The application to execute does not exist: 'PATH\{ASSEMBLY}.dll'</span></span>

::: moniker-end

<span data-ttu-id="36d34-252">Rozwiązywanie problemów:</span><span class="sxs-lookup"><span data-stu-id="36d34-252">Troubleshooting:</span></span>

* <span data-ttu-id="36d34-253">Upewnij się, że aplikacja działa lokalnie na Kestrel.</span><span class="sxs-lookup"><span data-stu-id="36d34-253">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="36d34-254">Niepowodzenia procesu może wynikać z problemu w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="36d34-254">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="36d34-255">Aby uzyskać więcej informacji, zobacz [Rozwiązywanie problemów z (IIS)](xref:host-and-deploy/iis/troubleshoot) lub [rozwiązywania problemów (Azure App Service)](xref:host-and-deploy/azure-apps/troubleshoot).</span><span class="sxs-lookup"><span data-stu-id="36d34-255">For more information, see [Troubleshoot (IIS)](xref:host-and-deploy/iis/troubleshoot) or [Troubleshoot (Azure App Service)](xref:host-and-deploy/azure-apps/troubleshoot).</span></span>

* <span data-ttu-id="36d34-256">Sprawdź *argumenty* atrybutu na `<aspNetCore>` elementu w *web.config* aby upewnić się, że jest ona () `.\{ASSEMBLY}.dll` wdrożenia zależny od struktury (stacje); lub (b) nie istnieje pusty ciąg (`arguments=""`), lub Podaj listę aplikacji argumentów (`arguments="{ARGUMENT_1}, {ARGUMENT_2}, ... {ARGUMENT_X}"`) niezależna wdrożenia (— SCD).</span><span class="sxs-lookup"><span data-stu-id="36d34-256">Examine the *arguments* attribute on the `<aspNetCore>` element in *web.config* to confirm that it's either (a) `.\{ASSEMBLY}.dll` for a framework-dependent deployment (FDD); or (b) not present, an empty string (`arguments=""`), or a list of the app's arguments (`arguments="{ARGUMENT_1}, {ARGUMENT_2}, ... {ARGUMENT_X}"`) for a self-contained deployment (SCD).</span></span>

::: moniker range=">= aspnetcore-2.2"

## <a name="missing-net-core-shared-framework"></a><span data-ttu-id="36d34-257">Brak udostępnionej platformy .NET Core</span><span class="sxs-lookup"><span data-stu-id="36d34-257">Missing .NET Core shared framework</span></span>

* <span data-ttu-id="36d34-258">**Przeglądarka:** Błąd HTTP 500.0 - ANCM w procesie programu obsługi błędu ładowania</span><span class="sxs-lookup"><span data-stu-id="36d34-258">**Browser:** HTTP Error 500.0 - ANCM In-Process Handler Load Failure</span></span>

* <span data-ttu-id="36d34-259">**Dziennik aplikacji:** Wywoływanie hostfxr można znaleźć programu obsługi żądania inprocess nie powiodła się bez znajdowanie wszelkie zależności natywnych.</span><span class="sxs-lookup"><span data-stu-id="36d34-259">**Application Log:** Invoking hostfxr to find the inprocess request handler failed without finding any native dependencies.</span></span> <span data-ttu-id="36d34-260">To najprawdopodobniej oznacza, że aplikacja jest nieprawidłowo skonfigurowane, sprawdź, czy wersje pakietów Microsoft.NetCore.App i Microsoft.AspNetCore.App są objęci aplikacji, które są zainstalowane na komputerze.</span><span class="sxs-lookup"><span data-stu-id="36d34-260">This most likely means the app is misconfigured, please check the versions of Microsoft.NetCore.App and Microsoft.AspNetCore.App that are targeted by the application and are installed on the machine.</span></span> <span data-ttu-id="36d34-261">Nie można odnaleźć inprocess żądania obsługi.</span><span class="sxs-lookup"><span data-stu-id="36d34-261">Could not find inprocess request handler.</span></span> <span data-ttu-id="36d34-262">Przechwycone dane wyjściowe z wywołaniem hostfxr: Nie można odnaleźć żadnych zgodnych framework w wersji.</span><span class="sxs-lookup"><span data-stu-id="36d34-262">Captured output from invoking hostfxr: It was not possible to find any compatible framework version.</span></span> <span data-ttu-id="36d34-263">Określony framework "Microsoft.AspNetCore.App", wersja {VERSION} nie został znaleziony.</span><span class="sxs-lookup"><span data-stu-id="36d34-263">The specified framework 'Microsoft.AspNetCore.App', version '{VERSION}' was not found.</span></span>

<span data-ttu-id="36d34-264">Nie można uruchomić aplikacji "/ LM/W3SVC/5/ROOT", kod błędu "0x8000ffff".</span><span class="sxs-lookup"><span data-stu-id="36d34-264">Failed to start application '/LM/W3SVC/5/ROOT', ErrorCode '0x8000ffff'.</span></span>

* <span data-ttu-id="36d34-265">**ASP.NET Core modułu strumienia wyjściowego stdout dziennika:** Nie można odnaleźć żadnych zgodnych framework w wersji.</span><span class="sxs-lookup"><span data-stu-id="36d34-265">**ASP.NET Core Module stdout Log:** It was not possible to find any compatible framework version.</span></span> <span data-ttu-id="36d34-266">Określony framework "Microsoft.AspNetCore.App", wersja {VERSION} nie został znaleziony.</span><span class="sxs-lookup"><span data-stu-id="36d34-266">The specified framework 'Microsoft.AspNetCore.App', version '{VERSION}' was not found.</span></span>

* <span data-ttu-id="36d34-267">**Moduł ASP.NET Core dziennik debugowania:** Zwrócone HRESULT nie powiodło się: 0x8000ffff</span><span class="sxs-lookup"><span data-stu-id="36d34-267">**ASP.NET Core Module Debug Log:** Failed HRESULT returned: 0x8000ffff</span></span>

::: moniker-end

<span data-ttu-id="36d34-268">Rozwiązywanie problemów:</span><span class="sxs-lookup"><span data-stu-id="36d34-268">Troubleshooting:</span></span>

<span data-ttu-id="36d34-269">W przypadku wdrożenia zależny od struktury (stacje) upewnij się, że poprawne środowisko uruchomieniowe zainstalowane w systemie.</span><span class="sxs-lookup"><span data-stu-id="36d34-269">For a framework-dependent deployment (FDD), confirm that the correct runtime installed on the system.</span></span>

## <a name="stopped-application-pool"></a><span data-ttu-id="36d34-270">Zatrzymanej puli aplikacji</span><span class="sxs-lookup"><span data-stu-id="36d34-270">Stopped Application Pool</span></span>

* <span data-ttu-id="36d34-271">**Przeglądarka:** 503 Usługa niedostępna</span><span class="sxs-lookup"><span data-stu-id="36d34-271">**Browser:** 503 Service Unavailable</span></span>

* <span data-ttu-id="36d34-272">**Dziennik aplikacji:** Brak wpisu</span><span class="sxs-lookup"><span data-stu-id="36d34-272">**Application Log:** No entry</span></span>

* <span data-ttu-id="36d34-273">**ASP.NET Core modułu strumienia wyjściowego stdout dziennika:** Plik dziennika nie jest tworzony.</span><span class="sxs-lookup"><span data-stu-id="36d34-273">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="36d34-274">**Moduł ASP.NET Core dziennik debugowania:** Plik dziennika nie jest tworzony.</span><span class="sxs-lookup"><span data-stu-id="36d34-274">**ASP.NET Core Module Debug Log:** The log file isn't created.</span></span>

::: moniker-end

<span data-ttu-id="36d34-275">Rozwiązywanie problemów:</span><span class="sxs-lookup"><span data-stu-id="36d34-275">Troubleshooting:</span></span>

<span data-ttu-id="36d34-276">Upewnij się, że pula aplikacji nie znajduje się w *zatrzymane* stanu.</span><span class="sxs-lookup"><span data-stu-id="36d34-276">Confirm that the Application Pool isn't in the *Stopped* state.</span></span>

## <a name="sub-application-includes-a-handlers-section"></a><span data-ttu-id="36d34-277">Aplikacja podrzędnych zawiera \<obsługi > sekcji</span><span class="sxs-lookup"><span data-stu-id="36d34-277">Sub-application includes a \<handlers> section</span></span>

* <span data-ttu-id="36d34-278">**Przeglądarka:** Błąd HTTP 500.19 — wewnętrzny błąd serwera</span><span class="sxs-lookup"><span data-stu-id="36d34-278">**Browser:** HTTP Error 500.19 - Internal Server Error</span></span>

* <span data-ttu-id="36d34-279">**Dziennik aplikacji:** Brak wpisu</span><span class="sxs-lookup"><span data-stu-id="36d34-279">**Application Log:** No entry</span></span>

* <span data-ttu-id="36d34-280">**ASP.NET Core modułu strumienia wyjściowego stdout dziennika:** Aplikacja główny plik dziennika jest tworzony i pokazuje normalnego działania.</span><span class="sxs-lookup"><span data-stu-id="36d34-280">**ASP.NET Core Module stdout Log:** The root app's log file is created and shows normal operation.</span></span> <span data-ttu-id="36d34-281">Nie jest tworzony plik dziennika aplikacji podrzędnej.</span><span class="sxs-lookup"><span data-stu-id="36d34-281">The sub-app's log file isn't created.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="36d34-282">**Moduł ASP.NET Core dziennik debugowania:** Aplikacja główny plik dziennika jest tworzony i pokazuje normalnego działania.</span><span class="sxs-lookup"><span data-stu-id="36d34-282">**ASP.NET Core Module Debug Log:** The root app's log file is created and shows normal operation.</span></span> <span data-ttu-id="36d34-283">Nie jest tworzony plik dziennika aplikacji podrzędnej.</span><span class="sxs-lookup"><span data-stu-id="36d34-283">The sub-app's log file isn't created.</span></span>

::: moniker-end

<span data-ttu-id="36d34-284">Rozwiązywanie problemów:</span><span class="sxs-lookup"><span data-stu-id="36d34-284">Troubleshooting:</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="36d34-285">Upewnij się, że aplikacja sub *web.config* file nie dołącza `<handlers>` sekcji lub że aplikacji podrzędnej nie dziedziczy obsługi aplikacji nadrzędnej.</span><span class="sxs-lookup"><span data-stu-id="36d34-285">Confirm that the sub-app's *web.config* file doesn't include a `<handlers>` section or that the sub-app doesn't inherit the parent app's handlers.</span></span>

<span data-ttu-id="36d34-286">Aplikacja nadrzędna `<system.webServer>` części *web.config* znajduje się wewnątrz `<location>` elementu.</span><span class="sxs-lookup"><span data-stu-id="36d34-286">The parent app's `<system.webServer>` section of *web.config* is placed inside of a `<location>` element.</span></span> <span data-ttu-id="36d34-287"><xref:System.Configuration.SectionInformation.InheritInChildApplications*> Właściwość jest ustawiona na `false` do wskazania, że ustawienia określone w [ \<lokalizacja >](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) elementu nie są dziedziczone przez aplikacje, które znajdują się w podkatalogu aplikacja nadrzędna.</span><span class="sxs-lookup"><span data-stu-id="36d34-287">The <xref:System.Configuration.SectionInformation.InheritInChildApplications*> property is set to `false` to indicate that the settings specified within the [\<location>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) element aren't inherited by apps that reside in a subdirectory of the parent app.</span></span> <span data-ttu-id="36d34-288">Aby uzyskać więcej informacji, zobacz <xref:host-and-deploy/aspnet-core-module>.</span><span class="sxs-lookup"><span data-stu-id="36d34-288">For more information, see <xref:host-and-deploy/aspnet-core-module>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="36d34-289">Upewnij się, że aplikacja sub *web.config* file nie dołącza `<handlers>` sekcji.</span><span class="sxs-lookup"><span data-stu-id="36d34-289">Confirm that the sub-app's *web.config* file doesn't include a `<handlers>` section.</span></span>

::: moniker-end

## <a name="stdout-log-path-incorrect"></a><span data-ttu-id="36d34-290">Nieprawidłowa ścieżka dziennika stdout</span><span class="sxs-lookup"><span data-stu-id="36d34-290">stdout log path incorrect</span></span>

* <span data-ttu-id="36d34-291">**Przeglądarka:** Aplikacja reaguje, normalnie.</span><span class="sxs-lookup"><span data-stu-id="36d34-291">**Browser:** The app responds normally.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="36d34-292">**Dziennik aplikacji:** Nie można uruchomić przekierowania stdout C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span><span class="sxs-lookup"><span data-stu-id="36d34-292">**Application Log:** Could not start stdout redirection in C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span></span> <span data-ttu-id="36d34-293">Komunikat o wyjątku: HRESULT 0x80070005 zwracanych w {PATH}\aspnetcoremodulev2\commonlib\fileoutputmanager.cpp:84.</span><span class="sxs-lookup"><span data-stu-id="36d34-293">Exception message: HRESULT 0x80070005 returned at {PATH}\aspnetcoremodulev2\commonlib\fileoutputmanager.cpp:84.</span></span> <span data-ttu-id="36d34-294">Nie można zatrzymać przekierowanie stdout w C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span><span class="sxs-lookup"><span data-stu-id="36d34-294">Could not stop stdout redirection in C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span></span> <span data-ttu-id="36d34-295">Komunikat o wyjątku: HRESULT: 0x80070002 zwrócił w lokalizacji {PATH}.</span><span class="sxs-lookup"><span data-stu-id="36d34-295">Exception message: HRESULT 0x80070002 returned at {PATH}.</span></span> <span data-ttu-id="36d34-296">Nie można uruchomić przekierowanie stdout w {PATH}\aspnetcorev2_inprocess.dll.</span><span class="sxs-lookup"><span data-stu-id="36d34-296">Could not start stdout redirection in {PATH}\aspnetcorev2_inprocess.dll.</span></span>

* <span data-ttu-id="36d34-297">**ASP.NET Core modułu strumienia wyjściowego stdout dziennika:** Plik dziennika nie jest tworzony.</span><span class="sxs-lookup"><span data-stu-id="36d34-297">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

* <span data-ttu-id="36d34-298">**Moduł ASP.NET Core dziennik debugowania:** Nie można uruchomić przekierowania stdout C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span><span class="sxs-lookup"><span data-stu-id="36d34-298">**ASP.NET Core Module debug Log:** Could not start stdout redirection in C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span></span> <span data-ttu-id="36d34-299">Komunikat o wyjątku: HRESULT 0x80070005 zwracanych w {PATH}\aspnetcoremodulev2\commonlib\fileoutputmanager.cpp:84.</span><span class="sxs-lookup"><span data-stu-id="36d34-299">Exception message: HRESULT 0x80070005 returned at {PATH}\aspnetcoremodulev2\commonlib\fileoutputmanager.cpp:84.</span></span> <span data-ttu-id="36d34-300">Nie można zatrzymać przekierowanie stdout w C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span><span class="sxs-lookup"><span data-stu-id="36d34-300">Could not stop stdout redirection in C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span></span> <span data-ttu-id="36d34-301">Komunikat o wyjątku: HRESULT: 0x80070002 zwrócił w lokalizacji {PATH}.</span><span class="sxs-lookup"><span data-stu-id="36d34-301">Exception message: HRESULT 0x80070002 returned at {PATH}.</span></span> <span data-ttu-id="36d34-302">Nie można uruchomić przekierowanie stdout w {PATH}\aspnetcorev2_inprocess.dll.</span><span class="sxs-lookup"><span data-stu-id="36d34-302">Could not start stdout redirection in {PATH}\aspnetcorev2_inprocess.dll.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="36d34-303">**Dziennik aplikacji:** Ostrzeżenie: Nie można utworzyć stdoutLogFile \\?\{ ŚCIEŻKA} \path_doesnt_exist\stdout_ {identyfikator procesu} _ {sygnatura CZASOWA} .log, kod błędu =-2147024893.</span><span class="sxs-lookup"><span data-stu-id="36d34-303">**Application Log:** Warning: Could not create stdoutLogFile \\?\{PATH}\path_doesnt_exist\stdout_{PROCESS ID}_{TIMESTAMP}.log, ErrorCode = -2147024893.</span></span>

* <span data-ttu-id="36d34-304">**ASP.NET Core modułu strumienia wyjściowego stdout dziennika:** Plik dziennika nie jest tworzony.</span><span class="sxs-lookup"><span data-stu-id="36d34-304">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

::: moniker-end

<span data-ttu-id="36d34-305">Rozwiązywanie problemów:</span><span class="sxs-lookup"><span data-stu-id="36d34-305">Troubleshooting:</span></span>

* <span data-ttu-id="36d34-306">`stdoutLogFile` Ścieżce określonej w `<aspNetCore>` elementu *web.config* nie istnieje.</span><span class="sxs-lookup"><span data-stu-id="36d34-306">The `stdoutLogFile` path specified in the `<aspNetCore>` element of *web.config* doesn't exist.</span></span> <span data-ttu-id="36d34-307">Aby uzyskać więcej informacji, zobacz [modułu ASP.NET Core: Tworzenie i Przekierowanie dziennika](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection).</span><span class="sxs-lookup"><span data-stu-id="36d34-307">For more information, see [ASP.NET Core Module: Log creation and redirection](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection).</span></span>

* <span data-ttu-id="36d34-308">Użytkownik puli aplikacji nie ma uprawnienia do zapisu w ścieżce dziennika stdout.</span><span class="sxs-lookup"><span data-stu-id="36d34-308">The app pool user doesn't have write access to the stdout log path.</span></span>

## <a name="application-configuration-general-issue"></a><span data-ttu-id="36d34-309">Ogólny problem z konfiguracją aplikacji</span><span class="sxs-lookup"><span data-stu-id="36d34-309">Application configuration general issue</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="36d34-310">**Przeglądarka:** Błąd HTTP 500.0 - ANCM w procesie programu obsługi błędu ładowania **--lub--** błąd HTTP 500.30 — błąd w trakcie uruchamiania ANCM</span><span class="sxs-lookup"><span data-stu-id="36d34-310">**Browser:** HTTP Error 500.0 - ANCM In-Process Handler Load Failure **--OR--** HTTP Error 500.30 - ANCM In-Process Start Failure</span></span>

* <span data-ttu-id="36d34-311">**Dziennik aplikacji:** Zmienna</span><span class="sxs-lookup"><span data-stu-id="36d34-311">**Application Log:** Variable</span></span>

* <span data-ttu-id="36d34-312">**ASP.NET Core modułu strumienia wyjściowego stdout dziennika:** Plik dziennika został utworzony, ale puste lub utworzony przy użyciu normalnych pozycji aż do punktu niepowodzenie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="36d34-312">**ASP.NET Core Module stdout Log:** The log file is created but empty or created with normal entries until the point of the app failing.</span></span>

* <span data-ttu-id="36d34-313">**Moduł ASP.NET Core dziennik debugowania:** Zmienna</span><span class="sxs-lookup"><span data-stu-id="36d34-313">**ASP.NET Core Module Debug Log:** Variable</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="36d34-314">**Przeglądarka:** Błąd HTTP 502.5 — niepowodzenia procesu</span><span class="sxs-lookup"><span data-stu-id="36d34-314">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="36d34-315">**Dziennik aplikacji:** Aplikacji "MACHINE/WEBROOT/APPHOST / {zestawu}" z certyfikatem głównym fizycznej "C:\{ścieżki}\' utworzone procesu przy użyciu wiersza polecenia" "C:\{ścieżki}\{zestawu}. { plik exe | dll} "" ale wystąpiła awaria lub nie odpowiada lub Nasłuchuj na podany port "{PORT}", kod błędu = "{Kod błędu:}"</span><span class="sxs-lookup"><span data-stu-id="36d34-315">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' created process with commandline '"C:\{PATH}\{ASSEMBLY}.{exe|dll}" ' but either crashed or did not respond or did not listen on the given port '{PORT}', ErrorCode = '{ERROR CODE}'</span></span>

* <span data-ttu-id="36d34-316">**ASP.NET Core modułu strumienia wyjściowego stdout dziennika:** Plik dziennika jest utworzony, ale puste.</span><span class="sxs-lookup"><span data-stu-id="36d34-316">**ASP.NET Core Module stdout Log:** The log file is created but empty.</span></span>

::: moniker-end

<span data-ttu-id="36d34-317">Rozwiązywanie problemów:</span><span class="sxs-lookup"><span data-stu-id="36d34-317">Troubleshooting:</span></span>

<span data-ttu-id="36d34-318">Proces nie mógł uruchomić, najprawdopodobniej z powodu konfiguracji aplikacji lub programowania problem.</span><span class="sxs-lookup"><span data-stu-id="36d34-318">The process failed to start, most likely due to an app configuration or programming issue.</span></span>

<span data-ttu-id="36d34-319">Więcej informacji znajduje się w następujących tematach:</span><span class="sxs-lookup"><span data-stu-id="36d34-319">For more information, see the following topics:</span></span>

* <xref:host-and-deploy/iis/troubleshoot>
* <xref:host-and-deploy/azure-apps/troubleshoot>
* <xref:test/troubleshoot>
