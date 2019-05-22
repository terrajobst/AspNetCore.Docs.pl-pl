---
title: Dokumentacja typowych błędów dla usługi Azure App Service i IIS za pomocą programu ASP.NET Core
author: guardrex
description: Uzyskaj porady dotyczące rozwiązywania problemów dla typowych błędów, odnośnie do hostowania aplikacji platformy ASP.NET Core na usługi aplikacji Azure i usług IIS.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/12/2019
uid: host-and-deploy/azure-iis-errors-reference
ms.openlocfilehash: 0191460f8c3dab98e6f977a29eacf0396b6789d8
ms.sourcegitcommit: b4ef2b00f3e1eb287138f8b43c811cb35a100d3e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/21/2019
ms.locfileid: "65970073"
---
# <a name="common-errors-reference-for-azure-app-service-and-iis-with-aspnet-core"></a><span data-ttu-id="729cb-103">Dokumentacja typowych błędów dla usługi Azure App Service i IIS za pomocą programu ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="729cb-103">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>

<span data-ttu-id="729cb-104">Przez [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="729cb-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="729cb-105">W tym temacie oferuje porady dotyczące rozwiązywania problemów dla typowych błędów, odnośnie do hostowania aplikacji platformy ASP.NET Core na usługi aplikacji Azure i usług IIS.</span><span class="sxs-lookup"><span data-stu-id="729cb-105">This topic offers troubleshooting advice for common errors when hosting ASP.NET Core apps on Azure Apps Service and IIS.</span></span>

<span data-ttu-id="729cb-106">Zbierz wymienione poniżej informacje.</span><span class="sxs-lookup"><span data-stu-id="729cb-106">Collect the following information:</span></span>

* <span data-ttu-id="729cb-107">Zachowanie przeglądarki (stan kodu i komunikatu o błędzie)</span><span class="sxs-lookup"><span data-stu-id="729cb-107">Browser behavior (status code and error message)</span></span>
* <span data-ttu-id="729cb-108">Wpisy dziennika zdarzeń aplikacji</span><span class="sxs-lookup"><span data-stu-id="729cb-108">Application Event Log entries</span></span>
  * <span data-ttu-id="729cb-109">Usługa Azure App Service &ndash; zobacz <xref:host-and-deploy/azure-apps/troubleshoot>.</span><span class="sxs-lookup"><span data-stu-id="729cb-109">Azure App Service &ndash; See <xref:host-and-deploy/azure-apps/troubleshoot>.</span></span>
  * <span data-ttu-id="729cb-110">IIS</span><span class="sxs-lookup"><span data-stu-id="729cb-110">IIS</span></span>
    1. <span data-ttu-id="729cb-111">Wybierz **Start** na **Windows** menu, typ *Podgląd zdarzeń*i naciśnij klawisz **Enter**.</span><span class="sxs-lookup"><span data-stu-id="729cb-111">Select **Start** on the **Windows** menu, type *Event Viewer*, and press **Enter**.</span></span>
    1. <span data-ttu-id="729cb-112">Po **Podgląd zdarzeń** zostanie otwarta, rozwiń węzeł **Dzienniki Windows** > **aplikacji** na pasku bocznym.</span><span class="sxs-lookup"><span data-stu-id="729cb-112">After the **Event Viewer** opens, expand **Windows Logs** > **Application** in the sidebar.</span></span>
* <span data-ttu-id="729cb-113">Wpisy dziennika platformy ASP.NET Core modułu stdout i debugowania</span><span class="sxs-lookup"><span data-stu-id="729cb-113">ASP.NET Core Module stdout and debug log entries</span></span>
  * <span data-ttu-id="729cb-114">Usługa Azure App Service &ndash; zobacz <xref:host-and-deploy/azure-apps/troubleshoot>.</span><span class="sxs-lookup"><span data-stu-id="729cb-114">Azure App Service &ndash; See <xref:host-and-deploy/azure-apps/troubleshoot>.</span></span>
  * <span data-ttu-id="729cb-115">Usługi IIS &ndash; postępuj zgodnie z instrukcjami w [tworzenia i Przekierowanie dziennika](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection) i [rozszerzone dzienniki diagnostyczne](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) sekcjach tematu modułu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="729cb-115">IIS &ndash; Follow the instructions in the [Log creation and redirection](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection) and [Enhanced diagnostic logs](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) sections of the ASP.NET Core Module topic.</span></span>

<span data-ttu-id="729cb-116">Porównywanie informacji o błędzie do następujących typowych błędów.</span><span class="sxs-lookup"><span data-stu-id="729cb-116">Compare error information to the following common errors.</span></span> <span data-ttu-id="729cb-117">Jeśli zostanie znalezione dopasowanie, wykonaj porady dotyczące rozwiązywania problemów.</span><span class="sxs-lookup"><span data-stu-id="729cb-117">If a match is found, follow the troubleshooting advice.</span></span>

<span data-ttu-id="729cb-118">Lista błędów, w tym temacie nie jest wyczerpująca.</span><span class="sxs-lookup"><span data-stu-id="729cb-118">The list of errors in this topic isn't exhaustive.</span></span> <span data-ttu-id="729cb-119">Jeśli wystąpi błąd niewymienione w tym Otwórz nowy problem za pomocą **zawartości opinii** przycisk w dolnej części tego tematu zawiera szczegółowe instrukcje dotyczące sposobu odtwarzania błędu.</span><span class="sxs-lookup"><span data-stu-id="729cb-119">If you encounter an error not listed here, open a new issue using the **Content feedback** button at the bottom of this topic with detailed instructions on how to reproduce the error.</span></span>

[!INCLUDE[Azure App Service Preview Notice](../includes/azure-apps-preview-notice.md)]

## <a name="installer-unable-to-obtain-vc-redistributable"></a><span data-ttu-id="729cb-120">Instalator nie może uzyskać VC ++ do dystrybucji</span><span class="sxs-lookup"><span data-stu-id="729cb-120">Installer unable to obtain VC++ Redistributable</span></span>

* <span data-ttu-id="729cb-121">**Instalator wyjątek:** 0x80072EFD **--lub--** 0x80072f76 — nieokreślony błąd</span><span class="sxs-lookup"><span data-stu-id="729cb-121">**Installer Exception:** 0x80072efd **--OR--** 0x80072f76 - Unspecified error</span></span>

* <span data-ttu-id="729cb-122">**Wyjątek dziennika Instalatora&#8224;:** Błąd 0x80072efd **--lub--** 0x80072f76: Nie można uruchomić pakietu EXE</span><span class="sxs-lookup"><span data-stu-id="729cb-122">**Installer Log Exception&#8224;:** Error 0x80072efd **--OR--** 0x80072f76: Failed to execute EXE package</span></span>

  <span data-ttu-id="729cb-123">&#8224;Dziennik znajduje się w *C:\Users\{USER}\AppData\Local\Temp\dd_DotNetCoreWinSvrHosting__{TIMESTAMP}.log*.</span><span class="sxs-lookup"><span data-stu-id="729cb-123">&#8224;The log is located at *C:\Users\{USER}\AppData\Local\Temp\dd_DotNetCoreWinSvrHosting__{TIMESTAMP}.log*.</span></span>

<span data-ttu-id="729cb-124">Rozwiązywanie problemów:</span><span class="sxs-lookup"><span data-stu-id="729cb-124">Troubleshooting:</span></span>

<span data-ttu-id="729cb-125">Jeśli system nie ma dostępu do Internetu podczas [Instalowanie pakietu hostingu platformy .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle), ten wyjątek występuje w przypadku uzyskiwania Instalator będzie mógł *Microsoft Visual C++ 2015 Redistributable*.</span><span class="sxs-lookup"><span data-stu-id="729cb-125">If the system doesn't have Internet access while [installing the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle), this exception occurs when the installer is prevented from obtaining the *Microsoft Visual C++ 2015 Redistributable*.</span></span> <span data-ttu-id="729cb-126">Uzyskaj Instalator [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840).</span><span class="sxs-lookup"><span data-stu-id="729cb-126">Obtain an installer from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840).</span></span> <span data-ttu-id="729cb-127">Jeśli Instalator zakończy się niepowodzeniem, serwer może nie otrzymać wymagane do obsługi środowiska uruchomieniowego .NET Core [zależny od struktury wdrożenia (stacje)](/dotnet/core/deploying/#framework-dependent-deployments-fdd).</span><span class="sxs-lookup"><span data-stu-id="729cb-127">If the installer fails, the server may not receive the .NET Core runtime required to host a [framework-dependent deployment (FDD)](/dotnet/core/deploying/#framework-dependent-deployments-fdd).</span></span> <span data-ttu-id="729cb-128">Jeśli hostingu Dyskietki, upewnij się, że środowisko uruchomieniowe jest zainstalowany w **programy i funkcje** lub **aplikacje i funkcje**.</span><span class="sxs-lookup"><span data-stu-id="729cb-128">If hosting an FDD, confirm that the runtime is installed in **Programs & Features** or **Apps & features**.</span></span> <span data-ttu-id="729cb-129">Jeśli wymagana jest określonego środowiska uruchomieniowego, Pobierz środowisko uruchomieniowe z [archiwa Pobierz .NET](https://dotnet.microsoft.com/download/archives) i zainstaluj go na system.</span><span class="sxs-lookup"><span data-stu-id="729cb-129">If a specific runtime is required, download the runtime from the [.NET Download Archives](https://dotnet.microsoft.com/download/archives) and install it on the system.</span></span> <span data-ttu-id="729cb-130">Po zainstalowaniu środowiska uruchomieniowego, uruchom ponownie komputer lub uruchom ponownie usługi IIS, wykonując **net stop został /y** następuje **net start w3svc** z poziomu wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="729cb-130">After installing the runtime, restart the system or restart IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

## <a name="os-upgrade-removed-the-32-bit-aspnet-core-module"></a><span data-ttu-id="729cb-131">Uaktualnienie systemu operacyjnego usunięte 32-bitowych modułu ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="729cb-131">OS upgrade removed the 32-bit ASP.NET Core Module</span></span>

<span data-ttu-id="729cb-132">**Dziennik aplikacji:** Biblioteka DLL modułu **C:\WINDOWS\system32\inetsrv\aspnetcore.dll** udało się załadować.</span><span class="sxs-lookup"><span data-stu-id="729cb-132">**Application Log:** The Module DLL **C:\WINDOWS\system32\inetsrv\aspnetcore.dll** failed to load.</span></span> <span data-ttu-id="729cb-133">Dane są błędne.</span><span class="sxs-lookup"><span data-stu-id="729cb-133">The data is the error.</span></span>

<span data-ttu-id="729cb-134">Rozwiązywanie problemów:</span><span class="sxs-lookup"><span data-stu-id="729cb-134">Troubleshooting:</span></span>

<span data-ttu-id="729cb-135">Pliki systemu operacyjnego bez **C:\Windows\SysWOW64\inetsrv** katalogu nie są zachowywane w trakcie systemu operacyjnego uaktualnienia.</span><span class="sxs-lookup"><span data-stu-id="729cb-135">Non-OS files in the **C:\Windows\SysWOW64\inetsrv** directory aren't preserved during an OS upgrade.</span></span> <span data-ttu-id="729cb-136">Jeśli zainstalowano modułu ASP.NET Core przed uaktualnienie systemu operacyjnego, a następnie każdej puli aplikacji jest uruchamiana w trybie 32-bitowych po uaktualnieniu systemu operacyjnego, ten problem zostanie osiągnięty.</span><span class="sxs-lookup"><span data-stu-id="729cb-136">If the ASP.NET Core Module is installed prior to an OS upgrade and then any app pool is run in 32-bit mode after an OS upgrade, this issue is encountered.</span></span> <span data-ttu-id="729cb-137">Po uaktualnieniu systemu operacyjnego napraw modułu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="729cb-137">After an OS upgrade, repair the ASP.NET Core Module.</span></span> <span data-ttu-id="729cb-138">Zobacz [instalacji pakietu .NET Core hostingu](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span><span class="sxs-lookup"><span data-stu-id="729cb-138">See [Install the .NET Core Hosting bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span> <span data-ttu-id="729cb-139">Wybierz **naprawy** po uruchomieniu Instalatora.</span><span class="sxs-lookup"><span data-stu-id="729cb-139">Select **Repair** when the installer is run.</span></span>

## <a name="missing-site-extension-32-bit-x86-and-64-bit-x64-site-extensions-installed-or-wrong-process-bitness-set"></a><span data-ttu-id="729cb-140">Brak rozszerzenia usługi site, (x86) 32-bitowych i 64-bitowych (x64) zainstalowane rozszerzenia witryny lub ustawić liczbę bitów procesu problem</span><span class="sxs-lookup"><span data-stu-id="729cb-140">Missing site extension, 32-bit (x86) and 64-bit (x64) site extensions installed, or wrong process bitness set</span></span>

<span data-ttu-id="729cb-141">*Dotyczy aplikacji hostowanych przez usługi Azure App Service.*</span><span class="sxs-lookup"><span data-stu-id="729cb-141">*Applies to apps hosted by Azure App Services.*</span></span>

* <span data-ttu-id="729cb-142">**Przeglądarka:** Błąd HTTP 500.0 - ANCM w procesie programu obsługi błędu ładowania</span><span class="sxs-lookup"><span data-stu-id="729cb-142">**Browser:** HTTP Error 500.0 - ANCM In-Process Handler Load Failure</span></span>

* <span data-ttu-id="729cb-143">**Dziennik aplikacji:** Wywoływanie hostfxr można znaleźć programu obsługi żądania inprocess nie powiodła się bez znajdowanie wszelkie zależności natywnych.</span><span class="sxs-lookup"><span data-stu-id="729cb-143">**Application Log:** Invoking hostfxr to find the inprocess request handler failed without finding any native dependencies.</span></span> <span data-ttu-id="729cb-144">Nie można odnaleźć inprocess żądania obsługi.</span><span class="sxs-lookup"><span data-stu-id="729cb-144">Could not find inprocess request handler.</span></span> <span data-ttu-id="729cb-145">Przechwycone dane wyjściowe z wywołaniem hostfxr: Nie można odnaleźć żadnych zgodnych framework w wersji.</span><span class="sxs-lookup"><span data-stu-id="729cb-145">Captured output from invoking hostfxr: It was not possible to find any compatible framework version.</span></span> <span data-ttu-id="729cb-146">Określony framework "Microsoft.AspNetCore.App", wersja "{VERSION} - preview -\*" nie został znaleziony.</span><span class="sxs-lookup"><span data-stu-id="729cb-146">The specified framework 'Microsoft.AspNetCore.App', version '{VERSION}-preview-\*' was not found.</span></span> <span data-ttu-id="729cb-147">Nie można uruchomić aplikacji "/ LM/W3SVC/1416782824/ROOT", kod błędu "0x8000ffff".</span><span class="sxs-lookup"><span data-stu-id="729cb-147">Failed to start application '/LM/W3SVC/1416782824/ROOT', ErrorCode '0x8000ffff'.</span></span>

* <span data-ttu-id="729cb-148">**ASP.NET Core modułu strumienia wyjściowego stdout dziennika:** Nie można odnaleźć żadnych zgodnych framework w wersji.</span><span class="sxs-lookup"><span data-stu-id="729cb-148">**ASP.NET Core Module stdout Log:** It was not possible to find any compatible framework version.</span></span> <span data-ttu-id="729cb-149">Określony framework "Microsoft.AspNetCore.App", wersja "{VERSION} - preview -\*" nie został znaleziony.</span><span class="sxs-lookup"><span data-stu-id="729cb-149">The specified framework 'Microsoft.AspNetCore.App', version '{VERSION}-preview-\*' was not found.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="729cb-150">**Moduł ASP.NET Core dziennik debugowania:** Wywoływanie hostfxr można znaleźć programu obsługi żądania inprocess nie powiodła się bez znajdowanie wszelkie zależności natywnych.</span><span class="sxs-lookup"><span data-stu-id="729cb-150">**ASP.NET Core Module Debug Log:** Invoking hostfxr to find the inprocess request handler failed without finding any native dependencies.</span></span> <span data-ttu-id="729cb-151">To najprawdopodobniej oznacza, że aplikacja jest nieprawidłowo skonfigurowane, sprawdź, czy wersje pakietów Microsoft.NetCore.App i Microsoft.AspNetCore.App są objęci aplikacji, które są zainstalowane na komputerze.</span><span class="sxs-lookup"><span data-stu-id="729cb-151">This most likely means the app is misconfigured, please check the versions of Microsoft.NetCore.App and Microsoft.AspNetCore.App that are targeted by the application and are installed on the machine.</span></span> <span data-ttu-id="729cb-152">Zwrócone HRESULT nie powiodło się: 0x8000ffff.</span><span class="sxs-lookup"><span data-stu-id="729cb-152">Failed HRESULT returned: 0x8000ffff.</span></span> <span data-ttu-id="729cb-153">Nie można odnaleźć inprocess żądania obsługi.</span><span class="sxs-lookup"><span data-stu-id="729cb-153">Could not find inprocess request handler.</span></span> <span data-ttu-id="729cb-154">Nie można odnaleźć żadnych zgodnych framework w wersji.</span><span class="sxs-lookup"><span data-stu-id="729cb-154">It was not possible to find any compatible framework version.</span></span> <span data-ttu-id="729cb-155">Określony framework "Microsoft.AspNetCore.App", wersja "{VERSION} - preview -\*" nie został znaleziony.</span><span class="sxs-lookup"><span data-stu-id="729cb-155">The specified framework 'Microsoft.AspNetCore.App', version '{VERSION}-preview-\*' was not found.</span></span>

::: moniker-end

<span data-ttu-id="729cb-156">Rozwiązywanie problemów:</span><span class="sxs-lookup"><span data-stu-id="729cb-156">Troubleshooting:</span></span>

* <span data-ttu-id="729cb-157">Jeśli uruchamianie aplikacji w środowisku uruchomieniowym w wersji zapoznawczej, należy zainstalować (x86) 32-bitowych **lub** (x64) 64-bitowych lokacji rozszerzeniem odpowiadającym typowi bitowość wersji środowiska uruchomieniowego aplikacji i aplikacji.</span><span class="sxs-lookup"><span data-stu-id="729cb-157">If running the app on a preview runtime, install either the 32-bit (x86) **or** 64-bit (x64) site extension that matches the bitness of the app and the app's runtime version.</span></span> <span data-ttu-id="729cb-158">**Nie należy instalować zarówno rozszerzenia lub wielu wersji środowiska uruchomieniowego rozszerzenia.**</span><span class="sxs-lookup"><span data-stu-id="729cb-158">**Don't install both extensions or multiple runtime versions of the extension.**</span></span>

  * <span data-ttu-id="729cb-159">ASP.NET Core {RUNTIME VERSION} (x86) Runtime</span><span class="sxs-lookup"><span data-stu-id="729cb-159">ASP.NET Core {RUNTIME VERSION} (x86) Runtime</span></span>
  * <span data-ttu-id="729cb-160">Platforma ASP.NET Core {wersja środowiska URUCHOMIENIOWEGO} (x 64) środowiska uruchomieniowego</span><span class="sxs-lookup"><span data-stu-id="729cb-160">ASP.NET Core {RUNTIME VERSION} (x64) Runtime</span></span>

  <span data-ttu-id="729cb-161">Uruchom ponownie aplikację.</span><span class="sxs-lookup"><span data-stu-id="729cb-161">Restart the app.</span></span> <span data-ttu-id="729cb-162">Odczekaj kilka sekund dla aplikacji, aby ponownie uruchomić.</span><span class="sxs-lookup"><span data-stu-id="729cb-162">Wait several seconds for the app to restart.</span></span>

* <span data-ttu-id="729cb-163">Jeśli aplikacja jest uruchomiona na runtime (wersja zapoznawcza) i (x86) 32-bitowych i 64-bitowych (x64) [rozszerzeń witryny](xref:host-and-deploy/azure-apps/index#install-the-preview-site-extension) są zainstalowane, odinstaluj rozszerzenie witryny, która nie odpowiada liczbie bitów aplikacji.</span><span class="sxs-lookup"><span data-stu-id="729cb-163">If running the app on a preview runtime and both the 32-bit (x86) and 64-bit (x64) [site extensions](xref:host-and-deploy/azure-apps/index#install-the-preview-site-extension) are installed, uninstall the site extension that doesn't match the bitness of the app.</span></span> <span data-ttu-id="729cb-164">Po usunięciu rozszerzenia witryny, należy ponownie uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="729cb-164">After removing the site extension, restart the app.</span></span> <span data-ttu-id="729cb-165">Odczekaj kilka sekund dla aplikacji, aby ponownie uruchomić.</span><span class="sxs-lookup"><span data-stu-id="729cb-165">Wait several seconds for the app to restart.</span></span>

* <span data-ttu-id="729cb-166">Jeśli aplikacja uruchomiona w środowisku uruchomieniowym w wersji zapoznawczej i rozszerzenia bitowości dopasowań, które aplikacji, upewnij się, które korzystania z wersji zapoznawczej witryny rozszerzenia *wersji środowiska uruchomieniowego* jest zgodna wersja środowiska uruchomieniowego aplikacji.</span><span class="sxs-lookup"><span data-stu-id="729cb-166">If running the app on a preview runtime and the site extension's bitness matches that of the app, confirm that the preview site extension's *runtime version* matches the app's runtime version.</span></span>

* <span data-ttu-id="729cb-167">Upewnij się, że aplikacja **platformy** w **ustawienia aplikacji** odpowiada liczbie bitów w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="729cb-167">Confirm that the app's **Platform** in **Application Settings** matches the bitness of the app.</span></span>

<span data-ttu-id="729cb-168">Aby uzyskać więcej informacji, zobacz <xref:host-and-deploy/azure-apps/index#install-the-preview-site-extension>.</span><span class="sxs-lookup"><span data-stu-id="729cb-168">For more information, see <xref:host-and-deploy/azure-apps/index#install-the-preview-site-extension>.</span></span>

## <a name="an-x86-app-is-deployed-but-the-app-pool-isnt-enabled-for-32-bit-apps"></a><span data-ttu-id="729cb-169">X86 aplikacja jest wdrożona, ale pula aplikacji nie jest włączony dla aplikacji 32-bitowych</span><span class="sxs-lookup"><span data-stu-id="729cb-169">An x86 app is deployed but the app pool isn't enabled for 32-bit apps</span></span>

* <span data-ttu-id="729cb-170">**Przeglądarka:** Błąd HTTP 500.30 — błąd w trakcie uruchamiania ANCM</span><span class="sxs-lookup"><span data-stu-id="729cb-170">**Browser:** HTTP Error 500.30 - ANCM In-Process Start Failure</span></span>

* <span data-ttu-id="729cb-171">**Dziennik aplikacji:** Aplikacja "/ LM/W3SVC/5/ROOT" za pomocą fizycznych głównego {PATH} trafień nieoczekiwany wyjątek zarządzany kod wyjątku = "0xe0434352".</span><span class="sxs-lookup"><span data-stu-id="729cb-171">**Application Log:** Application '/LM/W3SVC/5/ROOT' with physical root '{PATH}' hit unexpected managed exception, exception code = '0xe0434352'.</span></span> <span data-ttu-id="729cb-172">Sprawdź dzienniki stderr, aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="729cb-172">Please check the stderr logs for more information.</span></span> <span data-ttu-id="729cb-173">Aplikacja "/ LM/W3SVC/5/ROOT" za pomocą fizycznych głównego "{PATH}" nie można załadować środowiska clr i zarządzanych aplikacji.</span><span class="sxs-lookup"><span data-stu-id="729cb-173">Application '/LM/W3SVC/5/ROOT' with physical root '{PATH}' failed to load clr and managed application.</span></span> <span data-ttu-id="729cb-174">Wątek roboczy CLR przedwcześnie zakończył działanie</span><span class="sxs-lookup"><span data-stu-id="729cb-174">CLR worker thread exited prematurely</span></span>

* <span data-ttu-id="729cb-175">**ASP.NET Core modułu strumienia wyjściowego stdout dziennika:** Plik dziennika jest utworzony, ale puste.</span><span class="sxs-lookup"><span data-stu-id="729cb-175">**ASP.NET Core Module stdout Log:** The log file is created but empty.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="729cb-176">**Moduł ASP.NET Core dziennik debugowania:** Zwrócone HRESULT nie powiodło się: 0x8007023e</span><span class="sxs-lookup"><span data-stu-id="729cb-176">**ASP.NET Core Module Debug Log:** Failed HRESULT returned: 0x8007023e</span></span>

::: moniker-end

<span data-ttu-id="729cb-177">W tym scenariuszu jest to spowodowane zestawu SDK podczas publikowania aplikacja samodzielna.</span><span class="sxs-lookup"><span data-stu-id="729cb-177">This scenario is trapped by the SDK when publishing a self-contained app.</span></span> <span data-ttu-id="729cb-178">Zestaw SDK generuje błąd, jeśli RID nie jest zgodny platformy docelowej (na przykład `win10-x64` RID z `<PlatformTarget>x86</PlatformTarget>` w pliku projektu).</span><span class="sxs-lookup"><span data-stu-id="729cb-178">The SDK produces an error if the RID doesn't match the platform target (for example, `win10-x64` RID with `<PlatformTarget>x86</PlatformTarget>` in the project file).</span></span>

<span data-ttu-id="729cb-179">Rozwiązywanie problemów:</span><span class="sxs-lookup"><span data-stu-id="729cb-179">Troubleshooting:</span></span>

<span data-ttu-id="729cb-180">Dla x86 zależny od struktury wdrożenia (`<PlatformTarget>x86</PlatformTarget>`), Włącz puli aplikacji usług IIS dla aplikacji 32-bitowych.</span><span class="sxs-lookup"><span data-stu-id="729cb-180">For an x86 framework-dependent deployment (`<PlatformTarget>x86</PlatformTarget>`), enable the IIS app pool for 32-bit apps.</span></span> <span data-ttu-id="729cb-181">W Menedżerze usług IIS, otwórz puli aplikacji **Zaawansowane ustawienia** i ustaw **Włącz 32-bitowych aplikacji** do **True**.</span><span class="sxs-lookup"><span data-stu-id="729cb-181">In IIS Manager, open the app pool's **Advanced Settings** and set **Enable 32-Bit Applications** to **True**.</span></span>

## <a name="platform-conflicts-with-rid"></a><span data-ttu-id="729cb-182">Platforma jest w konflikcie z identyfikatorów RID</span><span class="sxs-lookup"><span data-stu-id="729cb-182">Platform conflicts with RID</span></span>

* <span data-ttu-id="729cb-183">**Przeglądarka:** Błąd HTTP 502.5 — niepowodzenia procesu</span><span class="sxs-lookup"><span data-stu-id="729cb-183">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="729cb-184">**Dziennik aplikacji:** Aplikacja "APPHOST-MACHINE/WEBROOT / {zestawu}" z certyfikatem głównym fizycznych "C:\{ścieżki}\' nie można uruchomić procesu przy użyciu wiersza polecenia" "C:\{ścieżki} {zestawu}. { plik exe | dll} "", kod błędu = "0x80004005: ff.</span><span class="sxs-lookup"><span data-stu-id="729cb-184">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' failed to start process with commandline '"C:\{PATH}{ASSEMBLY}.{exe|dll}" ', ErrorCode = '0x80004005 : ff.</span></span>

* <span data-ttu-id="729cb-185">**ASP.NET Core modułu strumienia wyjściowego stdout dziennika:** Nieobsługiwany wyjątek: System.BadImageFormatException: Nie można załadować pliku lub zestawu "{zestawu} .dll".</span><span class="sxs-lookup"><span data-stu-id="729cb-185">**ASP.NET Core Module stdout Log:** Unhandled Exception: System.BadImageFormatException: Could not load file or assembly '{ASSEMBLY}.dll'.</span></span> <span data-ttu-id="729cb-186">Próbowano załadować program w niepoprawnym formacie.</span><span class="sxs-lookup"><span data-stu-id="729cb-186">An attempt was made to load a program with an incorrect format.</span></span>

<span data-ttu-id="729cb-187">Rozwiązywanie problemów:</span><span class="sxs-lookup"><span data-stu-id="729cb-187">Troubleshooting:</span></span>

* <span data-ttu-id="729cb-188">Upewnij się, że aplikacja działa lokalnie na Kestrel.</span><span class="sxs-lookup"><span data-stu-id="729cb-188">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="729cb-189">Niepowodzenia procesu może wynikać z problemu w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="729cb-189">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="729cb-190">Aby uzyskać więcej informacji, zobacz [Rozwiązywanie problemów z (IIS)](xref:host-and-deploy/iis/troubleshoot) lub [rozwiązywania problemów (Azure App Service)](xref:host-and-deploy/azure-apps/troubleshoot).</span><span class="sxs-lookup"><span data-stu-id="729cb-190">For more information, see [Troubleshoot (IIS)](xref:host-and-deploy/iis/troubleshoot) or [Troubleshoot (Azure App Service)](xref:host-and-deploy/azure-apps/troubleshoot).</span></span>

* <span data-ttu-id="729cb-191">Jeśli ten wyjątek występuje podczas wdrażania aplikacji platformy Azure, podczas uaktualniania aplikacji i wdrażanie zestawów nowszej ręcznie usunąć wszystkie pliki z poprzedniego wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="729cb-191">If this exception occurs for an Azure Apps deployment when upgrading an app and deploying newer assemblies, manually delete all files from the prior deployment.</span></span> <span data-ttu-id="729cb-192">Lingering niezgodne zestawów może spowodować `System.BadImageFormatException` wyjątku w przypadku wdrażania uaktualnionego aplikacji.</span><span class="sxs-lookup"><span data-stu-id="729cb-192">Lingering incompatible assemblies can result in a `System.BadImageFormatException` exception when deploying an upgraded app.</span></span>

## <a name="uri-endpoint-wrong-or-stopped-website"></a><span data-ttu-id="729cb-193">Identyfikator URI punktu końcowego problem lub zatrzymana witryny sieci Web</span><span class="sxs-lookup"><span data-stu-id="729cb-193">URI endpoint wrong or stopped website</span></span>

* <span data-ttu-id="729cb-194">**Przeglądarka:** ERR_CONNECTION_REFUSED **--lub--** nie można połączyć z</span><span class="sxs-lookup"><span data-stu-id="729cb-194">**Browser:** ERR_CONNECTION_REFUSED **--OR--** Unable to connect</span></span>

* <span data-ttu-id="729cb-195">**Dziennik aplikacji:** Brak wpisu</span><span class="sxs-lookup"><span data-stu-id="729cb-195">**Application Log:** No entry</span></span>

* <span data-ttu-id="729cb-196">**ASP.NET Core modułu strumienia wyjściowego stdout dziennika:** Plik dziennika nie jest tworzony.</span><span class="sxs-lookup"><span data-stu-id="729cb-196">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="729cb-197">**Moduł ASP.NET Core dziennik debugowania:** Plik dziennika nie jest tworzony.</span><span class="sxs-lookup"><span data-stu-id="729cb-197">**ASP.NET Core Module Debug Log:** The log file isn't created.</span></span>

::: moniker-end

<span data-ttu-id="729cb-198">Rozwiązywanie problemów:</span><span class="sxs-lookup"><span data-stu-id="729cb-198">Troubleshooting:</span></span>

* <span data-ttu-id="729cb-199">Upewnij się, że właściwego punktu końcowego identyfikator URI aplikacji jest używana.</span><span class="sxs-lookup"><span data-stu-id="729cb-199">Confirm the correct URI endpoint for the app is in use.</span></span> <span data-ttu-id="729cb-200">Sprawdź wiązania.</span><span class="sxs-lookup"><span data-stu-id="729cb-200">Check the bindings.</span></span>

* <span data-ttu-id="729cb-201">Upewnij się, że witryny sieci Web usług IIS nie znajduje się w *zatrzymane* stanu.</span><span class="sxs-lookup"><span data-stu-id="729cb-201">Confirm that the IIS website isn't in the *Stopped* state.</span></span>

## <a name="corewebengine-or-w3svc-server-features-disabled"></a><span data-ttu-id="729cb-202">Serwer CoreWebEngine lub W3SVC funkcje wyłączone</span><span class="sxs-lookup"><span data-stu-id="729cb-202">CoreWebEngine or W3SVC server features disabled</span></span>

<span data-ttu-id="729cb-203">**Wyjątek systemu operacyjnego:** Użyj modułu ASP.NET Core można zainstalować funkcje usług IIS 7.0 CoreWebEngine i W3SVC.</span><span class="sxs-lookup"><span data-stu-id="729cb-203">**OS Exception:** The IIS 7.0 CoreWebEngine and W3SVC features must be installed to use the ASP.NET Core Module.</span></span>

<span data-ttu-id="729cb-204">Rozwiązywanie problemów:</span><span class="sxs-lookup"><span data-stu-id="729cb-204">Troubleshooting:</span></span>

<span data-ttu-id="729cb-205">Upewnij się, czy są włączone odpowiednie role i funkcje.</span><span class="sxs-lookup"><span data-stu-id="729cb-205">Confirm that the proper role and features are enabled.</span></span> <span data-ttu-id="729cb-206">Zobacz [konfiguracji programu IIS](xref:host-and-deploy/iis/index#iis-configuration).</span><span class="sxs-lookup"><span data-stu-id="729cb-206">See [IIS Configuration](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

## <a name="incorrect-website-physical-path-or-app-missing"></a><span data-ttu-id="729cb-207">Ścieżka fizyczna niepoprawna witryna sieci Web lub Brak aplikacji</span><span class="sxs-lookup"><span data-stu-id="729cb-207">Incorrect website physical path or app missing</span></span>

* <span data-ttu-id="729cb-208">**Przeglądarka:** 403 Zabroniony — odmowa dostępu **--lub--** zabronione 403.14 — serwer sieci Web jest skonfigurowany, aby nie wyświetlać listę zawartości tego katalogu.</span><span class="sxs-lookup"><span data-stu-id="729cb-208">**Browser:** 403 Forbidden - Access is denied **--OR--** 403.14 Forbidden - The Web server is configured to not list the contents of this directory.</span></span>

* <span data-ttu-id="729cb-209">**Dziennik aplikacji:** Brak wpisu</span><span class="sxs-lookup"><span data-stu-id="729cb-209">**Application Log:** No entry</span></span>

* <span data-ttu-id="729cb-210">**ASP.NET Core modułu strumienia wyjściowego stdout dziennika:** Plik dziennika nie jest tworzony.</span><span class="sxs-lookup"><span data-stu-id="729cb-210">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="729cb-211">**Moduł ASP.NET Core dziennik debugowania:** Plik dziennika nie jest tworzony.</span><span class="sxs-lookup"><span data-stu-id="729cb-211">**ASP.NET Core Module Debug Log:** The log file isn't created.</span></span>

::: moniker-end

<span data-ttu-id="729cb-212">Rozwiązywanie problemów:</span><span class="sxs-lookup"><span data-stu-id="729cb-212">Troubleshooting:</span></span>

<span data-ttu-id="729cb-213">Sprawdź witrynę sieci Web usług IIS **podstawowych ustawień** i folderu fizycznego aplikacji.</span><span class="sxs-lookup"><span data-stu-id="729cb-213">Check the IIS website **Basic Settings** and the physical app folder.</span></span> <span data-ttu-id="729cb-214">Upewnij się, że aplikacja znajduje się w folderze w witrynie sieci Web usług IIS **ścieżkę fizyczną**.</span><span class="sxs-lookup"><span data-stu-id="729cb-214">Confirm that the app is in the folder at the IIS website **Physical path**.</span></span>

## <a name="incorrect-role-aspnet-core-module-not-installed-or-incorrect-permissions"></a><span data-ttu-id="729cb-215">Nieprawidłowa rola, nie zainstalowano modułu ASP.NET Core lub niepoprawne uprawnienia</span><span class="sxs-lookup"><span data-stu-id="729cb-215">Incorrect role, ASP.NET Core Module not installed, or incorrect permissions</span></span>

* <span data-ttu-id="729cb-216">**Przeglądarka:** 500.19 — wewnętrzny błąd serwera — żądana strona nie są dostępne, ponieważ odpowiednie dane konfiguracyjne dla strony jest nieprawidłowy.</span><span class="sxs-lookup"><span data-stu-id="729cb-216">**Browser:** 500.19 Internal Server Error - The requested page cannot be accessed because the related configuration data for the page is invalid.</span></span> <span data-ttu-id="729cb-217">**--LUB--** nie można wyświetlić tej strony</span><span class="sxs-lookup"><span data-stu-id="729cb-217">**--OR--** This page can't be displayed</span></span>

* <span data-ttu-id="729cb-218">**Dziennik aplikacji:** Brak wpisu</span><span class="sxs-lookup"><span data-stu-id="729cb-218">**Application Log:** No entry</span></span>

* <span data-ttu-id="729cb-219">**ASP.NET Core modułu strumienia wyjściowego stdout dziennika:** Plik dziennika nie jest tworzony.</span><span class="sxs-lookup"><span data-stu-id="729cb-219">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="729cb-220">**Moduł ASP.NET Core dziennik debugowania:** Plik dziennika nie jest tworzony.</span><span class="sxs-lookup"><span data-stu-id="729cb-220">**ASP.NET Core Module Debug Log:** The log file isn't created.</span></span>

::: moniker-end

<span data-ttu-id="729cb-221">Rozwiązywanie problemów:</span><span class="sxs-lookup"><span data-stu-id="729cb-221">Troubleshooting:</span></span>

* <span data-ttu-id="729cb-222">Upewnij się, że włączono właściwej roli.</span><span class="sxs-lookup"><span data-stu-id="729cb-222">Confirm that the proper role is enabled.</span></span> <span data-ttu-id="729cb-223">Zobacz [konfiguracji programu IIS](xref:host-and-deploy/iis/index#iis-configuration).</span><span class="sxs-lookup"><span data-stu-id="729cb-223">See [IIS Configuration](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

* <span data-ttu-id="729cb-224">Otwórz **programy i funkcje** lub **aplikacje i funkcje** i upewnij się, że **systemu Windows serwer obsługujący** jest zainstalowany.</span><span class="sxs-lookup"><span data-stu-id="729cb-224">Open **Programs & Features** or **Apps & features** and confirm that **Windows Server Hosting** is installed.</span></span> <span data-ttu-id="729cb-225">Jeśli **systemu Windows serwer obsługujący** nie ma na liście zainstalowanych programów, Pobierz i zainstaluj pakiet hostingu platformy .NET Core.</span><span class="sxs-lookup"><span data-stu-id="729cb-225">If **Windows Server Hosting** isn't present in the list of installed programs, download and install the .NET Core Hosting Bundle.</span></span>

  [<span data-ttu-id="729cb-226">Bieżący Instalatora pakietu hostingu programu .NET Core (pobieranie bezpośrednie)</span><span class="sxs-lookup"><span data-stu-id="729cb-226">Current .NET Core Hosting Bundle installer (direct download)</span></span>](https://www.microsoft.com/net/permalink/dotnetcore-current-windows-runtime-bundle-installer)

  <span data-ttu-id="729cb-227">Aby uzyskać więcej informacji, zobacz [jest instalowany pakiet hostingu platformy .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span><span class="sxs-lookup"><span data-stu-id="729cb-227">For more information, see [Install the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span>

* <span data-ttu-id="729cb-228">Upewnij się, że **puli aplikacji** > **Model procesu** > **tożsamości** ustawiono **ApplicationPoolIdentity** lub tożsamość niestandardowa ma odpowiednie uprawnienia dostępu do folderu wdrożenia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="729cb-228">Make sure that the **Application Pool** > **Process Model** > **Identity** is set to **ApplicationPoolIdentity** or the custom identity has the correct permissions to access the app's deployment folder.</span></span>

* <span data-ttu-id="729cb-229">Jeśli odinstalowanie pakietu hostingu platformy ASP.NET Core i zainstalować starszą wersję pakietu hostingu *applicationHost.config* plik nie zawiera sekcji dla modułu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="729cb-229">If you uninstalled the ASP.NET Core Hosting Bundle and installed an earlier version of the hosting bundle, the *applicationHost.config* file doesn't include a section for the ASP.NET Core Module.</span></span> <span data-ttu-id="729cb-230">Otwórz *applicationHost.config* na *%windir%/System32/inetsrv/config* i Znajdź `<configuration><configSections><sectionGroup name="system.webServer">` grupy sekcji.</span><span class="sxs-lookup"><span data-stu-id="729cb-230">Open *applicationHost.config* at *%windir%/System32/inetsrv/config* and find the `<configuration><configSections><sectionGroup name="system.webServer">` section group.</span></span> <span data-ttu-id="729cb-231">Jeśli grupy sekcji brakuje sekcji dla modułu ASP.NET Core, Dodaj element sekcji:</span><span class="sxs-lookup"><span data-stu-id="729cb-231">If the section for the ASP.NET Core Module is missing from the section group, add the section element:</span></span>

  ```xml
  <section name="aspNetCore" overrideModeDefault="Allow" />
  ```

  <span data-ttu-id="729cb-232">Alternatywnie zainstalować najnowszą wersję pakietu hostingu platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="729cb-232">Alternatively, install the latest version of the ASP.NET Core Hosting Bundle.</span></span> <span data-ttu-id="729cb-233">Najnowsza wersja jest wstecznie zgodny z obsługiwane aplikacje platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="729cb-233">The latest version is backwards-compatible with supported ASP.NET Core apps.</span></span>

## <a name="incorrect-processpath-missing-path-variable-hosting-bundle-not-installed-systemiis-not-restarted-vc-redistributable-not-installed-or-dotnetexe-access-violation"></a><span data-ttu-id="729cb-234">Niepoprawne processPath, Brak zmiennej PATH, hostingu pakietu nie jest zainstalowany, nie uruchomiono ponownie system/IIS, VC ++ Redistributable nie jest zainstalowany lub naruszenie zasad dostępu dotnet.exe</span><span class="sxs-lookup"><span data-stu-id="729cb-234">Incorrect processPath, missing PATH variable, Hosting Bundle not installed, system/IIS not restarted, VC++ Redistributable not installed, or dotnet.exe access violation</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="729cb-235">**Przeglądarka:** Błąd HTTP 500.0 - ANCM w procesie programu obsługi błędu ładowania</span><span class="sxs-lookup"><span data-stu-id="729cb-235">**Browser:** HTTP Error 500.0 - ANCM In-Process Handler Load Failure</span></span>

* <span data-ttu-id="729cb-236">**Dziennik aplikacji:** Aplikacja "MACHINE/WEBROOT/APPHOST / {zestawu}" z certyfikatem głównym fizycznych "C:\{ścieżki}\' nie można uruchomić procesu przy użyciu wiersza polecenia" "{...}"</span><span class="sxs-lookup"><span data-stu-id="729cb-236">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' failed to start process with commandline '"{...}"</span></span> <span data-ttu-id="729cb-237">', ErrorCode = '0x80070002 : 0.</span><span class="sxs-lookup"><span data-stu-id="729cb-237">', ErrorCode = '0x80070002 : 0.</span></span> <span data-ttu-id="729cb-238">Aplikacja {PATH} nie był w stanie uruchomić.</span><span class="sxs-lookup"><span data-stu-id="729cb-238">Application '{PATH}' wasn't able to start.</span></span> <span data-ttu-id="729cb-239">Nie można odnaleźć pliku wykonywalnego w lokalizacji {PATH}.</span><span class="sxs-lookup"><span data-stu-id="729cb-239">Executable was not found at '{PATH}'.</span></span> <span data-ttu-id="729cb-240">Nie można uruchomić aplikacji "/ LM/W3SVC/2/ROOT", kod błędu "0x8007023e".</span><span class="sxs-lookup"><span data-stu-id="729cb-240">Failed to start application '/LM/W3SVC/2/ROOT', ErrorCode '0x8007023e'.</span></span>

* <span data-ttu-id="729cb-241">**ASP.NET Core modułu strumienia wyjściowego stdout dziennika:** Plik dziennika nie jest tworzony.</span><span class="sxs-lookup"><span data-stu-id="729cb-241">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

* <span data-ttu-id="729cb-242">**Moduł ASP.NET Core dziennik debugowania:** Dziennik zdarzeń: "Aplikacja"{PATH}"nie był w stanie uruchomić.</span><span class="sxs-lookup"><span data-stu-id="729cb-242">**ASP.NET Core Module Debug Log:** Event Log: 'Application '{PATH}' wasn't able to start.</span></span> <span data-ttu-id="729cb-243">Nie można odnaleźć pliku wykonywalnego w lokalizacji {PATH}.</span><span class="sxs-lookup"><span data-stu-id="729cb-243">Executable was not found at '{PATH}'.</span></span> <span data-ttu-id="729cb-244">Zwrócone HRESULT nie powiodło się: 0x8007023e</span><span class="sxs-lookup"><span data-stu-id="729cb-244">Failed HRESULT returned: 0x8007023e</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="729cb-245">**Przeglądarka:** Błąd HTTP 502.5 — niepowodzenia procesu</span><span class="sxs-lookup"><span data-stu-id="729cb-245">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="729cb-246">**Dziennik aplikacji:** Aplikacja "MACHINE/WEBROOT/APPHOST / {zestawu}" z certyfikatem głównym fizycznych "C:\{ścieżki}\' nie można uruchomić procesu przy użyciu wiersza polecenia" "{...}"</span><span class="sxs-lookup"><span data-stu-id="729cb-246">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' failed to start process with commandline '"{...}"</span></span> <span data-ttu-id="729cb-247">', ErrorCode = '0x80070002 : 0.</span><span class="sxs-lookup"><span data-stu-id="729cb-247">', ErrorCode = '0x80070002 : 0.</span></span>

* <span data-ttu-id="729cb-248">**ASP.NET Core modułu strumienia wyjściowego stdout dziennika:** Plik dziennika jest utworzony, ale puste.</span><span class="sxs-lookup"><span data-stu-id="729cb-248">**ASP.NET Core Module stdout Log:** The log file is created but empty.</span></span>

::: moniker-end

<span data-ttu-id="729cb-249">Rozwiązywanie problemów:</span><span class="sxs-lookup"><span data-stu-id="729cb-249">Troubleshooting:</span></span>

* <span data-ttu-id="729cb-250">Upewnij się, że aplikacja działa lokalnie na Kestrel.</span><span class="sxs-lookup"><span data-stu-id="729cb-250">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="729cb-251">Niepowodzenia procesu może wynikać z problemu w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="729cb-251">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="729cb-252">Aby uzyskać więcej informacji, zobacz [Rozwiązywanie problemów z (IIS)](xref:host-and-deploy/iis/troubleshoot) lub [rozwiązywania problemów (Azure App Service)](xref:host-and-deploy/azure-apps/troubleshoot).</span><span class="sxs-lookup"><span data-stu-id="729cb-252">For more information, see [Troubleshoot (IIS)](xref:host-and-deploy/iis/troubleshoot) or [Troubleshoot (Azure App Service)](xref:host-and-deploy/azure-apps/troubleshoot).</span></span>

* <span data-ttu-id="729cb-253">Sprawdź *processPath* atrybutu na `<aspNetCore>` element *web.config* aby upewnić się, że jest `dotnet` wdrożenia zależny od struktury (stacje) lub `.\{ASSEMBLY}.exe` dla [niezależna wdrożenia (— SCD)](/dotnet/core/deploying/#self-contained-deployments-scd).</span><span class="sxs-lookup"><span data-stu-id="729cb-253">Check the *processPath* attribute on the `<aspNetCore>` element in *web.config* to confirm that it's `dotnet` for a framework-dependent deployment (FDD) or `.\{ASSEMBLY}.exe` for a [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd).</span></span>

* <span data-ttu-id="729cb-254">Dla Dyskietki *dotnet.exe* może nie być dostępne za pośrednictwem ustawienia ścieżki.</span><span class="sxs-lookup"><span data-stu-id="729cb-254">For an FDD, *dotnet.exe* might not be accessible via the PATH settings.</span></span> <span data-ttu-id="729cb-255">Upewnij się, że *C:\Program Files\dotnet\\*  istnieje w ustawieniach ścieżki systemowej.</span><span class="sxs-lookup"><span data-stu-id="729cb-255">Confirm that *C:\Program Files\dotnet\\* exists in the System PATH settings.</span></span>

* <span data-ttu-id="729cb-256">Dla Dyskietki *dotnet.exe* może nie być dostępny dla tożsamości puli aplikacji.</span><span class="sxs-lookup"><span data-stu-id="729cb-256">For an FDD, *dotnet.exe* might not be accessible for the user identity of the app pool.</span></span> <span data-ttu-id="729cb-257">Upewnij się, że tożsamość puli aplikacji ma dostęp do *C:\Program Files\dotnet* katalogu.</span><span class="sxs-lookup"><span data-stu-id="729cb-257">Confirm that the app pool user identity has access to the *C:\Program Files\dotnet* directory.</span></span> <span data-ttu-id="729cb-258">Upewnij się, że nie istnieją żadne reguły odmowy skonfigurowane dla tożsamości użytkownika puli aplikacji na *C:\Program Files\dotnet* i katalogów aplikacji.</span><span class="sxs-lookup"><span data-stu-id="729cb-258">Confirm that there are no deny rules configured for the app pool user identity on the *C:\Program Files\dotnet* and app directories.</span></span>

* <span data-ttu-id="729cb-259">STACJE zostały wdrożone, oraz platformy .NET Core zainstalowane bez ponownego uruchamiania usług IIS.</span><span class="sxs-lookup"><span data-stu-id="729cb-259">An FDD may have been deployed and .NET Core installed without restarting IIS.</span></span> <span data-ttu-id="729cb-260">Uruchom ponownie serwer, albo ponownego uruchomienia usług IIS, wykonując **net stop został /y** następuje **net start w3svc** z poziomu wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="729cb-260">Either restart the server or restart IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

* <span data-ttu-id="729cb-261">Dyskietki mogą wdrożyć bez konieczności instalowania środowiska uruchomieniowego .NET Core w systemie hostingu.</span><span class="sxs-lookup"><span data-stu-id="729cb-261">An FDD may have been deployed without installing the .NET Core runtime on the hosting system.</span></span> <span data-ttu-id="729cb-262">Jeśli nie zainstalowano środowisko uruchomieniowe platformy .NET Core, uruchom **Instalatora programu .NET Core hostingu pakietu** w systemie.</span><span class="sxs-lookup"><span data-stu-id="729cb-262">If the .NET Core runtime hasn't been installed, run the **.NET Core Hosting Bundle installer** on the system.</span></span>

  [<span data-ttu-id="729cb-263">Bieżący Instalatora pakietu hostingu programu .NET Core (pobieranie bezpośrednie)</span><span class="sxs-lookup"><span data-stu-id="729cb-263">Current .NET Core Hosting Bundle installer (direct download)</span></span>](https://www.microsoft.com/net/permalink/dotnetcore-current-windows-runtime-bundle-installer)

  <span data-ttu-id="729cb-264">Aby uzyskać więcej informacji, zobacz [jest instalowany pakiet hostingu platformy .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span><span class="sxs-lookup"><span data-stu-id="729cb-264">For more information, see [Install the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span>

  <span data-ttu-id="729cb-265">Jeśli wymagana jest określonego środowiska uruchomieniowego, Pobierz środowisko uruchomieniowe z [archiwa Pobierz .NET](https://dotnet.microsoft.com/download/archives) i zainstaluj go na system.</span><span class="sxs-lookup"><span data-stu-id="729cb-265">If a specific runtime is required, download the runtime from the [.NET Download Archives](https://dotnet.microsoft.com/download/archives) and install it on the system.</span></span> <span data-ttu-id="729cb-266">Zakończ instalację, ponowne uruchamianie systemu lub ponowne uruchomienie usług IIS, wykonując **net stop został /y** następuje **net start w3svc** z poziomu wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="729cb-266">Complete the installation by restarting the system or restarting IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

* <span data-ttu-id="729cb-267">STACJE zostały wdrożone i *Microsoft Visual C++ 2015 Redistributable (x64)* nie jest zainstalowany w systemie.</span><span class="sxs-lookup"><span data-stu-id="729cb-267">An FDD may have been deployed and the *Microsoft Visual C++ 2015 Redistributable (x64)* isn't installed on the system.</span></span> <span data-ttu-id="729cb-268">Uzyskaj Instalator [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840).</span><span class="sxs-lookup"><span data-stu-id="729cb-268">Obtain an installer from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840).</span></span>

## <a name="incorrect-arguments-of-aspnetcore-element"></a><span data-ttu-id="729cb-269">Nieprawidłowe argumenty \<aspNetCore > element</span><span class="sxs-lookup"><span data-stu-id="729cb-269">Incorrect arguments of \<aspNetCore> element</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="729cb-270">**Przeglądarka:** Błąd HTTP 500.0 - ANCM w procesie programu obsługi błędu ładowania</span><span class="sxs-lookup"><span data-stu-id="729cb-270">**Browser:** HTTP Error 500.0 - ANCM In-Process Handler Load Failure</span></span>

* <span data-ttu-id="729cb-271">**Dziennik aplikacji:** Wywoływanie hostfxr można znaleźć programu obsługi żądania inprocess nie powiodła się bez znajdowanie wszelkie zależności natywnych.</span><span class="sxs-lookup"><span data-stu-id="729cb-271">**Application Log:** Invoking hostfxr to find the inprocess request handler failed without finding any native dependencies.</span></span> <span data-ttu-id="729cb-272">To najprawdopodobniej oznacza, że aplikacja jest nieprawidłowo skonfigurowane, sprawdź, czy wersje pakietów Microsoft.NetCore.App i Microsoft.AspNetCore.App są objęci aplikacji, które są zainstalowane na komputerze.</span><span class="sxs-lookup"><span data-stu-id="729cb-272">This most likely means the app is misconfigured, please check the versions of Microsoft.NetCore.App and Microsoft.AspNetCore.App that are targeted by the application and are installed on the machine.</span></span> <span data-ttu-id="729cb-273">Nie można odnaleźć inprocess żądania obsługi.</span><span class="sxs-lookup"><span data-stu-id="729cb-273">Could not find inprocess request handler.</span></span> <span data-ttu-id="729cb-274">Przechwycone dane wyjściowe z wywołaniem hostfxr: Czy chodziło Ci o polecenia zestawu SDK platformy dotnet?</span><span class="sxs-lookup"><span data-stu-id="729cb-274">Captured output from invoking hostfxr: Did you mean to run dotnet SDK commands?</span></span> <span data-ttu-id="729cb-275">Zainstaluj zestaw SDK platformy dotnet z: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409 Nie można uruchomić aplikacji "/ LM/W3SVC/3/ROOT", kod błędu "0x8000ffff".</span><span class="sxs-lookup"><span data-stu-id="729cb-275">Please install dotnet SDK from: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409 Failed to start application '/LM/W3SVC/3/ROOT', ErrorCode '0x8000ffff'.</span></span>

* <span data-ttu-id="729cb-276">**ASP.NET Core modułu strumienia wyjściowego stdout dziennika:** Czy chodziło Ci o polecenia zestawu SDK platformy dotnet?</span><span class="sxs-lookup"><span data-stu-id="729cb-276">**ASP.NET Core Module stdout Log:** Did you mean to run dotnet SDK commands?</span></span> <span data-ttu-id="729cb-277">Zainstaluj zestaw SDK platformy dotnet z: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409</span><span class="sxs-lookup"><span data-stu-id="729cb-277">Please install dotnet SDK from: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409</span></span>

* <span data-ttu-id="729cb-278">**Moduł ASP.NET Core dziennik debugowania:** Wywoływanie hostfxr można znaleźć programu obsługi żądania inprocess nie powiodła się bez znajdowanie wszelkie zależności natywnych.</span><span class="sxs-lookup"><span data-stu-id="729cb-278">**ASP.NET Core Module Debug Log:** Invoking hostfxr to find the inprocess request handler failed without finding any native dependencies.</span></span> <span data-ttu-id="729cb-279">To najprawdopodobniej oznacza, że aplikacja jest nieprawidłowo skonfigurowane, sprawdź, czy wersje pakietów Microsoft.NetCore.App i Microsoft.AspNetCore.App są objęci aplikacji, które są zainstalowane na komputerze.</span><span class="sxs-lookup"><span data-stu-id="729cb-279">This most likely means the app is misconfigured, please check the versions of Microsoft.NetCore.App and Microsoft.AspNetCore.App that are targeted by the application and are installed on the machine.</span></span> <span data-ttu-id="729cb-280">Zwrócone HRESULT nie powiodło się: 0x8000ffff nie można odnaleźć inprocess żądania obsługi.</span><span class="sxs-lookup"><span data-stu-id="729cb-280">Failed HRESULT returned: 0x8000ffff Could not find inprocess request handler.</span></span> <span data-ttu-id="729cb-281">Przechwycone dane wyjściowe z wywołaniem hostfxr: Czy chodziło Ci o polecenia zestawu SDK platformy dotnet?</span><span class="sxs-lookup"><span data-stu-id="729cb-281">Captured output from invoking hostfxr: Did you mean to run dotnet SDK commands?</span></span> <span data-ttu-id="729cb-282">Zainstaluj zestaw SDK platformy dotnet z: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409 Zwrócone HRESULT nie powiodło się: 0x8000ffff</span><span class="sxs-lookup"><span data-stu-id="729cb-282">Please install dotnet SDK from: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409 Failed HRESULT returned: 0x8000ffff</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="729cb-283">**Przeglądarka:** Błąd HTTP 502.5 — niepowodzenia procesu</span><span class="sxs-lookup"><span data-stu-id="729cb-283">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="729cb-284">**Dziennik aplikacji:** Aplikacja "MACHINE/WEBROOT/APPHOST / {zestawu}" z certyfikatem głównym fizycznych "C:\{ścieżki}\' nie można uruchomić procesu przy użyciu wiersza polecenia" "dotnet".\{ Plik .dll zestawu} ", kod błędu =" 0x80004005: 80008081.</span><span class="sxs-lookup"><span data-stu-id="729cb-284">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' failed to start process with commandline '"dotnet" .\{ASSEMBLY}.dll', ErrorCode = '0x80004005 : 80008081.</span></span>

* <span data-ttu-id="729cb-285">**ASP.NET Core modułu strumienia wyjściowego stdout dziennika:** Wykonywanie aplikacji nie istnieje: 'PATH\{ASSEMBLY}.dll'</span><span class="sxs-lookup"><span data-stu-id="729cb-285">**ASP.NET Core Module stdout Log:** The application to execute does not exist: 'PATH\{ASSEMBLY}.dll'</span></span>

::: moniker-end

<span data-ttu-id="729cb-286">Rozwiązywanie problemów:</span><span class="sxs-lookup"><span data-stu-id="729cb-286">Troubleshooting:</span></span>

* <span data-ttu-id="729cb-287">Upewnij się, że aplikacja działa lokalnie na Kestrel.</span><span class="sxs-lookup"><span data-stu-id="729cb-287">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="729cb-288">Niepowodzenia procesu może wynikać z problemu w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="729cb-288">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="729cb-289">Aby uzyskać więcej informacji, zobacz [Rozwiązywanie problemów z (IIS)](xref:host-and-deploy/iis/troubleshoot) lub [rozwiązywania problemów (Azure App Service)](xref:host-and-deploy/azure-apps/troubleshoot).</span><span class="sxs-lookup"><span data-stu-id="729cb-289">For more information, see [Troubleshoot (IIS)](xref:host-and-deploy/iis/troubleshoot) or [Troubleshoot (Azure App Service)](xref:host-and-deploy/azure-apps/troubleshoot).</span></span>

* <span data-ttu-id="729cb-290">Sprawdź *argumenty* atrybutu na `<aspNetCore>` elementu w *web.config* aby upewnić się, że jest ona () `.\{ASSEMBLY}.dll` wdrożenia zależny od struktury (stacje); lub (b) nie istnieje pusty ciąg (`arguments=""`), lub Podaj listę aplikacji argumentów (`arguments="{ARGUMENT_1}, {ARGUMENT_2}, ... {ARGUMENT_X}"`) niezależna wdrożenia (— SCD).</span><span class="sxs-lookup"><span data-stu-id="729cb-290">Examine the *arguments* attribute on the `<aspNetCore>` element in *web.config* to confirm that it's either (a) `.\{ASSEMBLY}.dll` for a framework-dependent deployment (FDD); or (b) not present, an empty string (`arguments=""`), or a list of the app's arguments (`arguments="{ARGUMENT_1}, {ARGUMENT_2}, ... {ARGUMENT_X}"`) for a self-contained deployment (SCD).</span></span>

::: moniker range=">= aspnetcore-2.2"

## <a name="missing-net-core-shared-framework"></a><span data-ttu-id="729cb-291">Brak udostępnionej platformy .NET Core</span><span class="sxs-lookup"><span data-stu-id="729cb-291">Missing .NET Core shared framework</span></span>

* <span data-ttu-id="729cb-292">**Przeglądarka:** Błąd HTTP 500.0 - ANCM w procesie programu obsługi błędu ładowania</span><span class="sxs-lookup"><span data-stu-id="729cb-292">**Browser:** HTTP Error 500.0 - ANCM In-Process Handler Load Failure</span></span>

* <span data-ttu-id="729cb-293">**Dziennik aplikacji:** Wywoływanie hostfxr można znaleźć programu obsługi żądania inprocess nie powiodła się bez znajdowanie wszelkie zależności natywnych.</span><span class="sxs-lookup"><span data-stu-id="729cb-293">**Application Log:** Invoking hostfxr to find the inprocess request handler failed without finding any native dependencies.</span></span> <span data-ttu-id="729cb-294">To najprawdopodobniej oznacza, że aplikacja jest nieprawidłowo skonfigurowane, sprawdź, czy wersje pakietów Microsoft.NetCore.App i Microsoft.AspNetCore.App są objęci aplikacji, które są zainstalowane na komputerze.</span><span class="sxs-lookup"><span data-stu-id="729cb-294">This most likely means the app is misconfigured, please check the versions of Microsoft.NetCore.App and Microsoft.AspNetCore.App that are targeted by the application and are installed on the machine.</span></span> <span data-ttu-id="729cb-295">Nie można odnaleźć inprocess żądania obsługi.</span><span class="sxs-lookup"><span data-stu-id="729cb-295">Could not find inprocess request handler.</span></span> <span data-ttu-id="729cb-296">Przechwycone dane wyjściowe z wywołaniem hostfxr: Nie można odnaleźć żadnych zgodnych framework w wersji.</span><span class="sxs-lookup"><span data-stu-id="729cb-296">Captured output from invoking hostfxr: It was not possible to find any compatible framework version.</span></span> <span data-ttu-id="729cb-297">Określony framework "Microsoft.AspNetCore.App", wersja {VERSION} nie został znaleziony.</span><span class="sxs-lookup"><span data-stu-id="729cb-297">The specified framework 'Microsoft.AspNetCore.App', version '{VERSION}' was not found.</span></span>

<span data-ttu-id="729cb-298">Nie można uruchomić aplikacji "/ LM/W3SVC/5/ROOT", kod błędu "0x8000ffff".</span><span class="sxs-lookup"><span data-stu-id="729cb-298">Failed to start application '/LM/W3SVC/5/ROOT', ErrorCode '0x8000ffff'.</span></span>

* <span data-ttu-id="729cb-299">**ASP.NET Core modułu strumienia wyjściowego stdout dziennika:** Nie można odnaleźć żadnych zgodnych framework w wersji.</span><span class="sxs-lookup"><span data-stu-id="729cb-299">**ASP.NET Core Module stdout Log:** It was not possible to find any compatible framework version.</span></span> <span data-ttu-id="729cb-300">Określony framework "Microsoft.AspNetCore.App", wersja {VERSION} nie został znaleziony.</span><span class="sxs-lookup"><span data-stu-id="729cb-300">The specified framework 'Microsoft.AspNetCore.App', version '{VERSION}' was not found.</span></span>

* <span data-ttu-id="729cb-301">**Moduł ASP.NET Core dziennik debugowania:** Zwrócone HRESULT nie powiodło się: 0x8000ffff</span><span class="sxs-lookup"><span data-stu-id="729cb-301">**ASP.NET Core Module Debug Log:** Failed HRESULT returned: 0x8000ffff</span></span>

::: moniker-end

<span data-ttu-id="729cb-302">Rozwiązywanie problemów:</span><span class="sxs-lookup"><span data-stu-id="729cb-302">Troubleshooting:</span></span>

<span data-ttu-id="729cb-303">W przypadku wdrożenia zależny od struktury (stacje) upewnij się, że poprawne środowisko uruchomieniowe zainstalowane w systemie.</span><span class="sxs-lookup"><span data-stu-id="729cb-303">For a framework-dependent deployment (FDD), confirm that the correct runtime installed on the system.</span></span>

## <a name="stopped-application-pool"></a><span data-ttu-id="729cb-304">Zatrzymanej puli aplikacji</span><span class="sxs-lookup"><span data-stu-id="729cb-304">Stopped Application Pool</span></span>

* <span data-ttu-id="729cb-305">**Przeglądarka:** 503 Usługa niedostępna</span><span class="sxs-lookup"><span data-stu-id="729cb-305">**Browser:** 503 Service Unavailable</span></span>

* <span data-ttu-id="729cb-306">**Dziennik aplikacji:** Brak wpisu</span><span class="sxs-lookup"><span data-stu-id="729cb-306">**Application Log:** No entry</span></span>

* <span data-ttu-id="729cb-307">**ASP.NET Core modułu strumienia wyjściowego stdout dziennika:** Plik dziennika nie jest tworzony.</span><span class="sxs-lookup"><span data-stu-id="729cb-307">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="729cb-308">**Moduł ASP.NET Core dziennik debugowania:** Plik dziennika nie jest tworzony.</span><span class="sxs-lookup"><span data-stu-id="729cb-308">**ASP.NET Core Module Debug Log:** The log file isn't created.</span></span>

::: moniker-end

<span data-ttu-id="729cb-309">Rozwiązywanie problemów:</span><span class="sxs-lookup"><span data-stu-id="729cb-309">Troubleshooting:</span></span>

<span data-ttu-id="729cb-310">Upewnij się, że pula aplikacji nie znajduje się w *zatrzymane* stanu.</span><span class="sxs-lookup"><span data-stu-id="729cb-310">Confirm that the Application Pool isn't in the *Stopped* state.</span></span>

## <a name="sub-application-includes-a-handlers-section"></a><span data-ttu-id="729cb-311">Aplikacja podrzędnych zawiera \<obsługi > sekcji</span><span class="sxs-lookup"><span data-stu-id="729cb-311">Sub-application includes a \<handlers> section</span></span>

* <span data-ttu-id="729cb-312">**Przeglądarka:** Błąd HTTP 500.19 — wewnętrzny błąd serwera</span><span class="sxs-lookup"><span data-stu-id="729cb-312">**Browser:** HTTP Error 500.19 - Internal Server Error</span></span>

* <span data-ttu-id="729cb-313">**Dziennik aplikacji:** Brak wpisu</span><span class="sxs-lookup"><span data-stu-id="729cb-313">**Application Log:** No entry</span></span>

* <span data-ttu-id="729cb-314">**ASP.NET Core modułu strumienia wyjściowego stdout dziennika:** Aplikacja główny plik dziennika jest tworzony i pokazuje normalnego działania.</span><span class="sxs-lookup"><span data-stu-id="729cb-314">**ASP.NET Core Module stdout Log:** The root app's log file is created and shows normal operation.</span></span> <span data-ttu-id="729cb-315">Nie jest tworzony plik dziennika aplikacji podrzędnej.</span><span class="sxs-lookup"><span data-stu-id="729cb-315">The sub-app's log file isn't created.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="729cb-316">**Moduł ASP.NET Core dziennik debugowania:** Aplikacja główny plik dziennika jest tworzony i pokazuje normalnego działania.</span><span class="sxs-lookup"><span data-stu-id="729cb-316">**ASP.NET Core Module Debug Log:** The root app's log file is created and shows normal operation.</span></span> <span data-ttu-id="729cb-317">Nie jest tworzony plik dziennika aplikacji podrzędnej.</span><span class="sxs-lookup"><span data-stu-id="729cb-317">The sub-app's log file isn't created.</span></span>

::: moniker-end

<span data-ttu-id="729cb-318">Rozwiązywanie problemów:</span><span class="sxs-lookup"><span data-stu-id="729cb-318">Troubleshooting:</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="729cb-319">Upewnij się, że aplikacja sub *web.config* file nie dołącza `<handlers>` sekcji lub że aplikacji podrzędnej nie dziedziczy obsługi aplikacji nadrzędnej.</span><span class="sxs-lookup"><span data-stu-id="729cb-319">Confirm that the sub-app's *web.config* file doesn't include a `<handlers>` section or that the sub-app doesn't inherit the parent app's handlers.</span></span>

<span data-ttu-id="729cb-320">Aplikacja nadrzędna `<system.webServer>` części *web.config* znajduje się wewnątrz `<location>` elementu.</span><span class="sxs-lookup"><span data-stu-id="729cb-320">The parent app's `<system.webServer>` section of *web.config* is placed inside of a `<location>` element.</span></span> <span data-ttu-id="729cb-321"><xref:System.Configuration.SectionInformation.InheritInChildApplications*> Właściwość jest ustawiona na `false` do wskazania, że ustawienia określone w [ \<lokalizacja >](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) elementu nie są dziedziczone przez aplikacje, które znajdują się w podkatalogu aplikacja nadrzędna.</span><span class="sxs-lookup"><span data-stu-id="729cb-321">The <xref:System.Configuration.SectionInformation.InheritInChildApplications*> property is set to `false` to indicate that the settings specified within the [\<location>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) element aren't inherited by apps that reside in a subdirectory of the parent app.</span></span> <span data-ttu-id="729cb-322">Aby uzyskać więcej informacji, zobacz <xref:host-and-deploy/aspnet-core-module>.</span><span class="sxs-lookup"><span data-stu-id="729cb-322">For more information, see <xref:host-and-deploy/aspnet-core-module>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="729cb-323">Upewnij się, że aplikacja sub *web.config* file nie dołącza `<handlers>` sekcji.</span><span class="sxs-lookup"><span data-stu-id="729cb-323">Confirm that the sub-app's *web.config* file doesn't include a `<handlers>` section.</span></span>

::: moniker-end

## <a name="stdout-log-path-incorrect"></a><span data-ttu-id="729cb-324">Nieprawidłowa ścieżka dziennika stdout</span><span class="sxs-lookup"><span data-stu-id="729cb-324">stdout log path incorrect</span></span>

* <span data-ttu-id="729cb-325">**Przeglądarka:** Aplikacja reaguje, normalnie.</span><span class="sxs-lookup"><span data-stu-id="729cb-325">**Browser:** The app responds normally.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="729cb-326">**Dziennik aplikacji:** Nie można uruchomić przekierowania stdout C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span><span class="sxs-lookup"><span data-stu-id="729cb-326">**Application Log:** Could not start stdout redirection in C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span></span> <span data-ttu-id="729cb-327">Komunikat o wyjątku: HRESULT 0x80070005 zwracanych w {PATH}\aspnetcoremodulev2\commonlib\fileoutputmanager.cpp:84.</span><span class="sxs-lookup"><span data-stu-id="729cb-327">Exception message: HRESULT 0x80070005 returned at {PATH}\aspnetcoremodulev2\commonlib\fileoutputmanager.cpp:84.</span></span> <span data-ttu-id="729cb-328">Nie można zatrzymać przekierowanie stdout w C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span><span class="sxs-lookup"><span data-stu-id="729cb-328">Could not stop stdout redirection in C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span></span> <span data-ttu-id="729cb-329">Komunikat o wyjątku: HRESULT: 0x80070002 zwrócił w lokalizacji {PATH}.</span><span class="sxs-lookup"><span data-stu-id="729cb-329">Exception message: HRESULT 0x80070002 returned at {PATH}.</span></span> <span data-ttu-id="729cb-330">Nie można uruchomić przekierowanie stdout w {PATH}\aspnetcorev2_inprocess.dll.</span><span class="sxs-lookup"><span data-stu-id="729cb-330">Could not start stdout redirection in {PATH}\aspnetcorev2_inprocess.dll.</span></span>

* <span data-ttu-id="729cb-331">**ASP.NET Core modułu strumienia wyjściowego stdout dziennika:** Plik dziennika nie jest tworzony.</span><span class="sxs-lookup"><span data-stu-id="729cb-331">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

* <span data-ttu-id="729cb-332">**Moduł ASP.NET Core dziennik debugowania:** Nie można uruchomić przekierowania stdout C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span><span class="sxs-lookup"><span data-stu-id="729cb-332">**ASP.NET Core Module debug Log:** Could not start stdout redirection in C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span></span> <span data-ttu-id="729cb-333">Komunikat o wyjątku: HRESULT 0x80070005 zwracanych w {PATH}\aspnetcoremodulev2\commonlib\fileoutputmanager.cpp:84.</span><span class="sxs-lookup"><span data-stu-id="729cb-333">Exception message: HRESULT 0x80070005 returned at {PATH}\aspnetcoremodulev2\commonlib\fileoutputmanager.cpp:84.</span></span> <span data-ttu-id="729cb-334">Nie można zatrzymać przekierowanie stdout w C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span><span class="sxs-lookup"><span data-stu-id="729cb-334">Could not stop stdout redirection in C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span></span> <span data-ttu-id="729cb-335">Komunikat o wyjątku: HRESULT: 0x80070002 zwrócił w lokalizacji {PATH}.</span><span class="sxs-lookup"><span data-stu-id="729cb-335">Exception message: HRESULT 0x80070002 returned at {PATH}.</span></span> <span data-ttu-id="729cb-336">Nie można uruchomić przekierowanie stdout w {PATH}\aspnetcorev2_inprocess.dll.</span><span class="sxs-lookup"><span data-stu-id="729cb-336">Could not start stdout redirection in {PATH}\aspnetcorev2_inprocess.dll.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="729cb-337">**Dziennik aplikacji:** Ostrzeżenie: Nie można utworzyć stdoutLogFile \\?\{ ŚCIEŻKA} \path_doesnt_exist\stdout_ {identyfikator procesu} _ {sygnatura CZASOWA} .log, kod błędu =-2147024893.</span><span class="sxs-lookup"><span data-stu-id="729cb-337">**Application Log:** Warning: Could not create stdoutLogFile \\?\{PATH}\path_doesnt_exist\stdout_{PROCESS ID}_{TIMESTAMP}.log, ErrorCode = -2147024893.</span></span>

* <span data-ttu-id="729cb-338">**ASP.NET Core modułu strumienia wyjściowego stdout dziennika:** Plik dziennika nie jest tworzony.</span><span class="sxs-lookup"><span data-stu-id="729cb-338">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

::: moniker-end

<span data-ttu-id="729cb-339">Rozwiązywanie problemów:</span><span class="sxs-lookup"><span data-stu-id="729cb-339">Troubleshooting:</span></span>

* <span data-ttu-id="729cb-340">`stdoutLogFile` Ścieżce określonej w `<aspNetCore>` elementu *web.config* nie istnieje.</span><span class="sxs-lookup"><span data-stu-id="729cb-340">The `stdoutLogFile` path specified in the `<aspNetCore>` element of *web.config* doesn't exist.</span></span> <span data-ttu-id="729cb-341">Aby uzyskać więcej informacji, zobacz [modułu ASP.NET Core: Tworzenie i Przekierowanie dziennika](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection).</span><span class="sxs-lookup"><span data-stu-id="729cb-341">For more information, see [ASP.NET Core Module: Log creation and redirection](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection).</span></span>

* <span data-ttu-id="729cb-342">Użytkownik puli aplikacji nie ma uprawnienia do zapisu w ścieżce dziennika stdout.</span><span class="sxs-lookup"><span data-stu-id="729cb-342">The app pool user doesn't have write access to the stdout log path.</span></span>

## <a name="application-configuration-general-issue"></a><span data-ttu-id="729cb-343">Ogólny problem z konfiguracją aplikacji</span><span class="sxs-lookup"><span data-stu-id="729cb-343">Application configuration general issue</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="729cb-344">**Przeglądarka:** Błąd HTTP 500.0 - ANCM w procesie programu obsługi błędu ładowania **--lub--** błąd HTTP 500.30 — błąd w trakcie uruchamiania ANCM</span><span class="sxs-lookup"><span data-stu-id="729cb-344">**Browser:** HTTP Error 500.0 - ANCM In-Process Handler Load Failure **--OR--** HTTP Error 500.30 - ANCM In-Process Start Failure</span></span>

* <span data-ttu-id="729cb-345">**Dziennik aplikacji:** Zmienna</span><span class="sxs-lookup"><span data-stu-id="729cb-345">**Application Log:** Variable</span></span>

* <span data-ttu-id="729cb-346">**ASP.NET Core modułu strumienia wyjściowego stdout dziennika:** Plik dziennika został utworzony, ale puste lub utworzony przy użyciu normalnych pozycji aż do punktu niepowodzenie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="729cb-346">**ASP.NET Core Module stdout Log:** The log file is created but empty or created with normal entries until the point of the app failing.</span></span>

* <span data-ttu-id="729cb-347">**Moduł ASP.NET Core dziennik debugowania:** Zmienna</span><span class="sxs-lookup"><span data-stu-id="729cb-347">**ASP.NET Core Module Debug Log:** Variable</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="729cb-348">**Przeglądarka:** Błąd HTTP 502.5 — niepowodzenia procesu</span><span class="sxs-lookup"><span data-stu-id="729cb-348">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="729cb-349">**Dziennik aplikacji:** Aplikacji "MACHINE/WEBROOT/APPHOST / {zestawu}" z certyfikatem głównym fizycznej "C:\{ścieżki}\' utworzone procesu przy użyciu wiersza polecenia" "C:\{ścieżki}\{zestawu}. { plik exe | dll} "" ale wystąpiła awaria lub nie odpowiada lub Nasłuchuj na podany port "{PORT}", kod błędu = "{Kod błędu:}"</span><span class="sxs-lookup"><span data-stu-id="729cb-349">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' created process with commandline '"C:\{PATH}\{ASSEMBLY}.{exe|dll}" ' but either crashed or did not respond or did not listen on the given port '{PORT}', ErrorCode = '{ERROR CODE}'</span></span>

* <span data-ttu-id="729cb-350">**ASP.NET Core modułu strumienia wyjściowego stdout dziennika:** Plik dziennika jest utworzony, ale puste.</span><span class="sxs-lookup"><span data-stu-id="729cb-350">**ASP.NET Core Module stdout Log:** The log file is created but empty.</span></span>

::: moniker-end

<span data-ttu-id="729cb-351">Rozwiązywanie problemów:</span><span class="sxs-lookup"><span data-stu-id="729cb-351">Troubleshooting:</span></span>

<span data-ttu-id="729cb-352">Proces nie mógł uruchomić, najprawdopodobniej z powodu konfiguracji aplikacji lub programowania problem.</span><span class="sxs-lookup"><span data-stu-id="729cb-352">The process failed to start, most likely due to an app configuration or programming issue.</span></span>

<span data-ttu-id="729cb-353">Więcej informacji znajduje się w następujących tematach:</span><span class="sxs-lookup"><span data-stu-id="729cb-353">For more information, see the following topics:</span></span>

* <xref:host-and-deploy/iis/troubleshoot>
* <xref:host-and-deploy/azure-apps/troubleshoot>
* <xref:test/troubleshoot>
