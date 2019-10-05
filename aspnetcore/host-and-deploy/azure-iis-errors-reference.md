---
title: Informacje dotyczące typowych błędów dla Azure App Service i usług IIS z ASP.NET Core
author: guardrex
description: Uzyskaj porady dotyczące rozwiązywania problemów z typowymi błędami podczas hostowania aplikacji ASP.NET Core w usłudze Azure Apps i usługach IIS.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 09/11/2019
uid: host-and-deploy/azure-iis-errors-reference
ms.openlocfilehash: 047ef23bd2f4d349d2d342d17764c7edd3e0de4a
ms.sourcegitcommit: 4649814d1ae32248419da4e8f8242850fd8679a5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/05/2019
ms.locfileid: "71975676"
---
# <a name="common-errors-reference-for-azure-app-service-and-iis-with-aspnet-core"></a><span data-ttu-id="f6b9a-103">Informacje dotyczące typowych błędów dla Azure App Service i usług IIS z ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f6b9a-103">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>

<span data-ttu-id="f6b9a-104">Przez [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="f6b9a-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="f6b9a-105">W tym temacie opisano typowe błędy i przedstawiono porady dotyczące rozwiązywania problemów w przypadku hostowania aplikacji ASP.NET Core w usłudze Azure Apps i usługach IIS.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-105">This topic describes common errors and provides troubleshooting advice for specific errors when hosting ASP.NET Core apps on Azure Apps Service and IIS.</span></span>

<span data-ttu-id="f6b9a-106">Aby uzyskać ogólne wskazówki dotyczące rozwiązywania problemów, zobacz <xref:test/troubleshoot-azure-iis>.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-106">For general troubleshooting guidance, see <xref:test/troubleshoot-azure-iis>.</span></span>

<span data-ttu-id="f6b9a-107">Zbierz wymienione poniżej informacje.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-107">Collect the following information:</span></span>

* <span data-ttu-id="f6b9a-108">Zachowanie przeglądarki (kod stanu i komunikat o błędzie)</span><span class="sxs-lookup"><span data-stu-id="f6b9a-108">Browser behavior (status code and error message)</span></span>
* <span data-ttu-id="f6b9a-109">Wpisy dziennika zdarzeń aplikacji</span><span class="sxs-lookup"><span data-stu-id="f6b9a-109">Application Event Log entries</span></span>
  * <span data-ttu-id="f6b9a-110">Azure App Service &ndash; Zobacz <xref:test/troubleshoot-azure-iis>.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-110">Azure App Service &ndash; See <xref:test/troubleshoot-azure-iis>.</span></span>
  * <span data-ttu-id="f6b9a-111">IIS</span><span class="sxs-lookup"><span data-stu-id="f6b9a-111">IIS</span></span>
    1. <span data-ttu-id="f6b9a-112">Wybierz pozycję **Rozpocznij** w menu **systemu Windows** , wpisz *Podgląd zdarzeń*i naciśnij klawisz **Enter**.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-112">Select **Start** on the **Windows** menu, type *Event Viewer*, and press **Enter**.</span></span>
    1. <span data-ttu-id="f6b9a-113">Po otwarciu **Podgląd zdarzeń** rozwiń pozycję **dzienniki systemu Windows**@no__t**aplikacji** na pasku bocznym.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-113">After the **Event Viewer** opens, expand **Windows Logs** > **Application** in the sidebar.</span></span>
* <span data-ttu-id="f6b9a-114">Moduł ASP.NET Core stdout i wpisy dziennika debugowania</span><span class="sxs-lookup"><span data-stu-id="f6b9a-114">ASP.NET Core Module stdout and debug log entries</span></span>
  * <span data-ttu-id="f6b9a-115">Azure App Service &ndash; Zobacz <xref:test/troubleshoot-azure-iis>.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-115">Azure App Service &ndash; See <xref:test/troubleshoot-azure-iis>.</span></span>
  * <span data-ttu-id="f6b9a-116">IIS &ndash; postępuj zgodnie z instrukcjami w sekcjach [Tworzenie dziennika i przekierowanie](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection) i [udoskonalone dzienniki diagnostyczne](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) w temacie ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-116">IIS &ndash; Follow the instructions in the [Log creation and redirection](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection) and [Enhanced diagnostic logs](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) sections of the ASP.NET Core Module topic.</span></span>

<span data-ttu-id="f6b9a-117">Porównaj informacje o błędach z następującymi typowymi błędami.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-117">Compare error information to the following common errors.</span></span> <span data-ttu-id="f6b9a-118">Jeśli zostanie znalezione dopasowanie, postępuj zgodnie z zaleceniami dotyczącymi rozwiązywania problemów.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-118">If a match is found, follow the troubleshooting advice.</span></span>

<span data-ttu-id="f6b9a-119">Lista błędów w tym temacie nie jest wyczerpująca.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-119">The list of errors in this topic isn't exhaustive.</span></span> <span data-ttu-id="f6b9a-120">Jeśli wystąpi błąd niewymieniony w tym miejscu, Otwórz nowy problem przy użyciu przycisku **opinii o zawartości** w dolnej części tego tematu ze szczegółowymi instrukcjami dotyczącymi sposobu odtworzenia błędu.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-120">If you encounter an error not listed here, open a new issue using the **Content feedback** button at the bottom of this topic with detailed instructions on how to reproduce the error.</span></span>

[!INCLUDE[Azure App Service Preview Notice](../includes/azure-apps-preview-notice.md)]

## <a name="os-upgrade-removed-the-32-bit-aspnet-core-module"></a><span data-ttu-id="f6b9a-121">Uaktualnienie systemu operacyjnego usunęło 32-bitowy moduł ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f6b9a-121">OS upgrade removed the 32-bit ASP.NET Core Module</span></span>

<span data-ttu-id="f6b9a-122">**Dziennik aplikacji:** Nie można załadować **C:\WINDOWS\system32\inetsrv\aspnetcore.dll** biblioteki DLL modułu.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-122">**Application Log:** The Module DLL **C:\WINDOWS\system32\inetsrv\aspnetcore.dll** failed to load.</span></span> <span data-ttu-id="f6b9a-123">Dane są błędem.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-123">The data is the error.</span></span>

<span data-ttu-id="f6b9a-124">Rozwiązywanie problemów:</span><span class="sxs-lookup"><span data-stu-id="f6b9a-124">Troubleshooting:</span></span>

<span data-ttu-id="f6b9a-125">Pliki inne niż systemowe w katalogu **C:\Windows\SysWOW64\inetsrv** nie są zachowywane podczas uaktualniania systemu operacyjnego.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-125">Non-OS files in the **C:\Windows\SysWOW64\inetsrv** directory aren't preserved during an OS upgrade.</span></span> <span data-ttu-id="f6b9a-126">Jeśli moduł ASP.NET Core zostanie zainstalowany przed uaktualnieniem systemu operacyjnego, a następnie każda pula aplikacji zostanie uruchomiona w trybie 32-bitowym po uaktualnieniu systemu operacyjnego, ten problem wystąpi.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-126">If the ASP.NET Core Module is installed prior to an OS upgrade and then any app pool is run in 32-bit mode after an OS upgrade, this issue is encountered.</span></span> <span data-ttu-id="f6b9a-127">Po uaktualnieniu systemu operacyjnego napraw moduł ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-127">After an OS upgrade, repair the ASP.NET Core Module.</span></span> <span data-ttu-id="f6b9a-128">Zobacz [Instalowanie pakietu hostingu platformy .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span><span class="sxs-lookup"><span data-stu-id="f6b9a-128">See [Install the .NET Core Hosting bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span> <span data-ttu-id="f6b9a-129">Wybierz pozycję **napraw** , gdy Instalator zostanie uruchomiony.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-129">Select **Repair** when the installer is run.</span></span>

## <a name="missing-site-extension-32-bit-x86-and-64-bit-x64-site-extensions-installed-or-wrong-process-bitness-set"></a><span data-ttu-id="f6b9a-130">Brak zainstalowanego rozszerzenia witryny, 32-bitowe (x86) i 64-bit (x64) lub niewłaściwy zestaw bitów procesu</span><span class="sxs-lookup"><span data-stu-id="f6b9a-130">Missing site extension, 32-bit (x86) and 64-bit (x64) site extensions installed, or wrong process bitness set</span></span>

<span data-ttu-id="f6b9a-131">*Dotyczy aplikacji hostowanych przez usługę Azure App Services.*</span><span class="sxs-lookup"><span data-stu-id="f6b9a-131">*Applies to apps hosted by Azure App Services.*</span></span>

* <span data-ttu-id="f6b9a-132">**Przeglądarka:** Błąd HTTP 500,0 — błąd ładowania procedury obsługi ANCM w procesie</span><span class="sxs-lookup"><span data-stu-id="f6b9a-132">**Browser:** HTTP Error 500.0 - ANCM In-Process Handler Load Failure</span></span>

* <span data-ttu-id="f6b9a-133">**Dziennik aplikacji:** Wywoływanie hostfxr w celu znalezienia procedury obsługi żądania przetworzenia nie powiodło się bez wyszukiwania natywnych zależności.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-133">**Application Log:** Invoking hostfxr to find the inprocess request handler failed without finding any native dependencies.</span></span> <span data-ttu-id="f6b9a-134">Nie można znaleźć procedury obsługi żądania nieprzetwarzania.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-134">Could not find inprocess request handler.</span></span> <span data-ttu-id="f6b9a-135">Przechwycono dane wyjściowe z wywołania hostfxr: Nie można znaleźć żadnej zgodnej wersji platformy.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-135">Captured output from invoking hostfxr: It was not possible to find any compatible framework version.</span></span> <span data-ttu-id="f6b9a-136">Nie znaleziono określonej struktury "Microsoft. AspNetCore. app" w wersji "{VERSION}-Preview-\*".</span><span class="sxs-lookup"><span data-stu-id="f6b9a-136">The specified framework 'Microsoft.AspNetCore.App', version '{VERSION}-preview-\*' was not found.</span></span> <span data-ttu-id="f6b9a-137">Nie można uruchomić aplikacji "/LM/W3SVC/1416782824/ROOT", ErrorCode "0x8000FFFF".</span><span class="sxs-lookup"><span data-stu-id="f6b9a-137">Failed to start application '/LM/W3SVC/1416782824/ROOT', ErrorCode '0x8000ffff'.</span></span>

* <span data-ttu-id="f6b9a-138">**Dziennik stdout modułu ASP.NET Core:** Nie można znaleźć żadnej zgodnej wersji platformy.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-138">**ASP.NET Core Module stdout Log:** It was not possible to find any compatible framework version.</span></span> <span data-ttu-id="f6b9a-139">Nie znaleziono określonej struktury "Microsoft. AspNetCore. app" w wersji "{VERSION}-Preview-\*".</span><span class="sxs-lookup"><span data-stu-id="f6b9a-139">The specified framework 'Microsoft.AspNetCore.App', version '{VERSION}-preview-\*' was not found.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="f6b9a-140">**Dziennik debugowania modułu ASP.NET Core:** Wywoływanie hostfxr w celu znalezienia procedury obsługi żądania przetworzenia nie powiodło się bez wyszukiwania natywnych zależności.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-140">**ASP.NET Core Module Debug Log:** Invoking hostfxr to find the inprocess request handler failed without finding any native dependencies.</span></span> <span data-ttu-id="f6b9a-141">Najbardziej prawdopodobną przyczyną jest to, że aplikacja jest nieprawidłowo skonfigurowana. Sprawdź wersje programów Microsoft. servicecore. app i Microsoft. AspNetCore. app, które są przeznaczone dla aplikacji, i są zainstalowane na komputerze.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-141">This most likely means the app is misconfigured, please check the versions of Microsoft.NetCore.App and Microsoft.AspNetCore.App that are targeted by the application and are installed on the machine.</span></span> <span data-ttu-id="f6b9a-142">Zwrócony błąd HRESULT: 0x8000ffff.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-142">Failed HRESULT returned: 0x8000ffff.</span></span> <span data-ttu-id="f6b9a-143">Nie można znaleźć procedury obsługi żądania nieprzetwarzania.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-143">Could not find inprocess request handler.</span></span> <span data-ttu-id="f6b9a-144">Nie można znaleźć żadnej zgodnej wersji platformy.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-144">It was not possible to find any compatible framework version.</span></span> <span data-ttu-id="f6b9a-145">Nie znaleziono określonej struktury "Microsoft. AspNetCore. app" w wersji "{VERSION}-Preview-\*".</span><span class="sxs-lookup"><span data-stu-id="f6b9a-145">The specified framework 'Microsoft.AspNetCore.App', version '{VERSION}-preview-\*' was not found.</span></span>

::: moniker-end

<span data-ttu-id="f6b9a-146">Rozwiązywanie problemów:</span><span class="sxs-lookup"><span data-stu-id="f6b9a-146">Troubleshooting:</span></span>

* <span data-ttu-id="f6b9a-147">Jeśli aplikacja jest uruchamiana w środowisku uruchomieniowym w wersji zapoznawczej, należy zainstalować rozszerzenie witryny 32-bitowe (x86) **lub** 64-bit (x64), które jest zgodne z bitową aplikacją i wersją środowiska uruchomieniowego aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-147">If running the app on a preview runtime, install either the 32-bit (x86) **or** 64-bit (x64) site extension that matches the bitness of the app and the app's runtime version.</span></span> <span data-ttu-id="f6b9a-148">**Nie instaluj obu rozszerzeń ani wielu wersji tego rozszerzenia.**</span><span class="sxs-lookup"><span data-stu-id="f6b9a-148">**Don't install both extensions or multiple runtime versions of the extension.**</span></span>

  * <span data-ttu-id="f6b9a-149">Środowisko uruchomieniowe ASP.NET Core {RUNTIME} (x86)</span><span class="sxs-lookup"><span data-stu-id="f6b9a-149">ASP.NET Core {RUNTIME VERSION} (x86) Runtime</span></span>
  * <span data-ttu-id="f6b9a-150">Środowisko uruchomieniowe ASP.NET Core {RUNTIME} (x64)</span><span class="sxs-lookup"><span data-stu-id="f6b9a-150">ASP.NET Core {RUNTIME VERSION} (x64) Runtime</span></span>

  <span data-ttu-id="f6b9a-151">Uruchom ponownie aplikację.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-151">Restart the app.</span></span> <span data-ttu-id="f6b9a-152">Poczekaj kilka sekund, aż aplikacja zostanie ponownie uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-152">Wait several seconds for the app to restart.</span></span>

* <span data-ttu-id="f6b9a-153">Jeśli aplikacja jest uruchamiana w środowisku uruchomieniowym wersji zapoznawczej, a zainstalowane są [rozszerzenia lokacji](xref:host-and-deploy/azure-apps/index#install-the-preview-site-extension) 32-bitowe (x86) i 64-bitowe (x64), Odinstaluj rozszerzenie lokacji, które nie jest zgodne z bitową aplikacją.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-153">If running the app on a preview runtime and both the 32-bit (x86) and 64-bit (x64) [site extensions](xref:host-and-deploy/azure-apps/index#install-the-preview-site-extension) are installed, uninstall the site extension that doesn't match the bitness of the app.</span></span> <span data-ttu-id="f6b9a-154">Po usunięciu rozszerzenia witryny należy ponownie uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-154">After removing the site extension, restart the app.</span></span> <span data-ttu-id="f6b9a-155">Poczekaj kilka sekund, aż aplikacja zostanie ponownie uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-155">Wait several seconds for the app to restart.</span></span>

* <span data-ttu-id="f6b9a-156">Jeśli aplikacja jest uruchamiana w środowisku uruchomieniowym w wersji zapoznawczej, a liczba bitów rozszerzenia lokacji jest zgodna z tą, upewnij się, że *wersja środowiska uruchomieniowego* rozszerzenia witryny w wersji zapoznawczej jest zgodna z wersją środowiska uruchomieniowego aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-156">If running the app on a preview runtime and the site extension's bitness matches that of the app, confirm that the preview site extension's *runtime version* matches the app's runtime version.</span></span>

* <span data-ttu-id="f6b9a-157">Upewnij się, że **platforma** aplikacji w **ustawieniach aplikacji** jest zgodna z bitową aplikacją.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-157">Confirm that the app's **Platform** in **Application Settings** matches the bitness of the app.</span></span>

<span data-ttu-id="f6b9a-158">Aby uzyskać więcej informacji, zobacz <xref:host-and-deploy/azure-apps/index#install-the-preview-site-extension>.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-158">For more information, see <xref:host-and-deploy/azure-apps/index#install-the-preview-site-extension>.</span></span>

## <a name="an-x86-app-is-deployed-but-the-app-pool-isnt-enabled-for-32-bit-apps"></a><span data-ttu-id="f6b9a-159">Aplikacja x86 została wdrożona, ale Pula aplikacji nie jest włączona dla aplikacji 32-bitowych</span><span class="sxs-lookup"><span data-stu-id="f6b9a-159">An x86 app is deployed but the app pool isn't enabled for 32-bit apps</span></span>

* <span data-ttu-id="f6b9a-160">**Przeglądarka:** Błąd HTTP 500,30 — niepowodzenie uruchomienia ANCM w procesie</span><span class="sxs-lookup"><span data-stu-id="f6b9a-160">**Browser:** HTTP Error 500.30 - ANCM In-Process Start Failure</span></span>

* <span data-ttu-id="f6b9a-161">**Dziennik aplikacji:** Aplikacja "/LM/W3SVC/5/ROOT" z fizycznym elementem głównym "{PATH}" napotkała nieoczekiwany wyjątek zarządzany, kod wyjątku = "0xe0434352".</span><span class="sxs-lookup"><span data-stu-id="f6b9a-161">**Application Log:** Application '/LM/W3SVC/5/ROOT' with physical root '{PATH}' hit unexpected managed exception, exception code = '0xe0434352'.</span></span> <span data-ttu-id="f6b9a-162">Aby uzyskać więcej informacji, Sprawdź dzienniki stderr.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-162">Please check the stderr logs for more information.</span></span> <span data-ttu-id="f6b9a-163">Aplikacja "/LM/W3SVC/5/ROOT" z fizycznym elementem głównym "{PATH}" nie może załadować środowiska CLR i aplikacji zarządzanej.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-163">Application '/LM/W3SVC/5/ROOT' with physical root '{PATH}' failed to load clr and managed application.</span></span> <span data-ttu-id="f6b9a-164">Wątek roboczy CLR zakończony przedwcześnie</span><span class="sxs-lookup"><span data-stu-id="f6b9a-164">CLR worker thread exited prematurely</span></span>

* <span data-ttu-id="f6b9a-165">**Dziennik stdout modułu ASP.NET Core:** Plik dziennika jest tworzony, ale pusty.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-165">**ASP.NET Core Module stdout Log:** The log file is created but empty.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="f6b9a-166">**Dziennik debugowania modułu ASP.NET Core:** Zwrócony błąd HRESULT: 0x8007023e</span><span class="sxs-lookup"><span data-stu-id="f6b9a-166">**ASP.NET Core Module Debug Log:** Failed HRESULT returned: 0x8007023e</span></span>

::: moniker-end

<span data-ttu-id="f6b9a-167">Ten scenariusz jest zalewkowany przez zestaw SDK podczas publikowania aplikacji samodzielnej.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-167">This scenario is trapped by the SDK when publishing a self-contained app.</span></span> <span data-ttu-id="f6b9a-168">Zestaw SDK generuje błąd, jeśli identyfikator RID nie jest zgodny z obiektem docelowym platformy (na przykład `win10-x64` RID z `<PlatformTarget>x86</PlatformTarget>` w pliku projektu).</span><span class="sxs-lookup"><span data-stu-id="f6b9a-168">The SDK produces an error if the RID doesn't match the platform target (for example, `win10-x64` RID with `<PlatformTarget>x86</PlatformTarget>` in the project file).</span></span>

<span data-ttu-id="f6b9a-169">Rozwiązywanie problemów:</span><span class="sxs-lookup"><span data-stu-id="f6b9a-169">Troubleshooting:</span></span>

<span data-ttu-id="f6b9a-170">W przypadku wdrożenia opartego na architekturze x86 (`<PlatformTarget>x86</PlatformTarget>`) należy włączyć pulę aplikacji usług IIS dla aplikacji 32-bitowych.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-170">For an x86 framework-dependent deployment (`<PlatformTarget>x86</PlatformTarget>`), enable the IIS app pool for 32-bit apps.</span></span> <span data-ttu-id="f6b9a-171">W Menedżerze usług IIS Otwórz **Zaawansowane ustawienia** puli aplikacji i ustaw **wartość True**dla **aplikacji 32-bitowych** .</span><span class="sxs-lookup"><span data-stu-id="f6b9a-171">In IIS Manager, open the app pool's **Advanced Settings** and set **Enable 32-Bit Applications** to **True**.</span></span>

## <a name="platform-conflicts-with-rid"></a><span data-ttu-id="f6b9a-172">Konflikty platformy z identyfikatorem RID</span><span class="sxs-lookup"><span data-stu-id="f6b9a-172">Platform conflicts with RID</span></span>

* <span data-ttu-id="f6b9a-173">**Przeglądarka:** Błąd HTTP 502,5 — niepowodzenie procesu</span><span class="sxs-lookup"><span data-stu-id="f6b9a-173">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="f6b9a-174">**Dziennik aplikacji:** Nie można uruchomić aplikacji "MACHINE/WEBROOT/APPHOST/{ASSEMBLY}" z fizycznym głównym katalogiem "C: \{PATH} \' z wierszem polecenia" "C: \{PATH} {ASSEMBLY}. {exe | dll} "", ErrorCode = "0x80004005: FF.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-174">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' failed to start process with commandline '"C:\{PATH}{ASSEMBLY}.{exe|dll}" ', ErrorCode = '0x80004005 : ff.</span></span>

* <span data-ttu-id="f6b9a-175">**Dziennik stdout modułu ASP.NET Core:** Nieobsłużony wyjątek: System.BadImageFormatException: Nie można załadować pliku lub zestawu "{ASSEMBLY}. dll".</span><span class="sxs-lookup"><span data-stu-id="f6b9a-175">**ASP.NET Core Module stdout Log:** Unhandled Exception: System.BadImageFormatException: Could not load file or assembly '{ASSEMBLY}.dll'.</span></span> <span data-ttu-id="f6b9a-176">Podjęto próbę załadowania programu z nieprawidłowym formatem.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-176">An attempt was made to load a program with an incorrect format.</span></span>

<span data-ttu-id="f6b9a-177">Rozwiązywanie problemów:</span><span class="sxs-lookup"><span data-stu-id="f6b9a-177">Troubleshooting:</span></span>

* <span data-ttu-id="f6b9a-178">Upewnij się, że aplikacja działa lokalnie na Kestrel.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-178">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="f6b9a-179">Niepowodzenie procesu może być wynikiem problemu w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-179">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="f6b9a-180">Aby uzyskać więcej informacji, zobacz <xref:test/troubleshoot-azure-iis>.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-180">For more information, see <xref:test/troubleshoot-azure-iis>.</span></span>

* <span data-ttu-id="f6b9a-181">Jeśli ten wyjątek występuje w przypadku wdrożenia aplikacji platformy Azure podczas uaktualniania aplikacji i wdrażania nowszych zestawów, ręcznie usuń wszystkie pliki z wcześniejszego wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-181">If this exception occurs for an Azure Apps deployment when upgrading an app and deploying newer assemblies, manually delete all files from the prior deployment.</span></span> <span data-ttu-id="f6b9a-182">Niezgodne zestawy mogą powodować wyjątek `System.BadImageFormatException` podczas wdrażania uaktualnionej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-182">Lingering incompatible assemblies can result in a `System.BadImageFormatException` exception when deploying an upgraded app.</span></span>

## <a name="uri-endpoint-wrong-or-stopped-website"></a><span data-ttu-id="f6b9a-183">Nieprawidłowy punkt końcowy identyfikatora URI lub zatrzymano witrynę sieci Web</span><span class="sxs-lookup"><span data-stu-id="f6b9a-183">URI endpoint wrong or stopped website</span></span>

* <span data-ttu-id="f6b9a-184">**Przeglądarka:** ERR_CONNECTION_REFUSED **--lub--** nie można nawiązać połączenia</span><span class="sxs-lookup"><span data-stu-id="f6b9a-184">**Browser:** ERR_CONNECTION_REFUSED **--OR--** Unable to connect</span></span>

* <span data-ttu-id="f6b9a-185">**Dziennik aplikacji:** Brak wpisu</span><span class="sxs-lookup"><span data-stu-id="f6b9a-185">**Application Log:** No entry</span></span>

* <span data-ttu-id="f6b9a-186">**Dziennik stdout modułu ASP.NET Core:** Plik dziennika nie został utworzony.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-186">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="f6b9a-187">**Dziennik debugowania modułu ASP.NET Core:** Plik dziennika nie został utworzony.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-187">**ASP.NET Core Module Debug Log:** The log file isn't created.</span></span>

::: moniker-end

<span data-ttu-id="f6b9a-188">Rozwiązywanie problemów:</span><span class="sxs-lookup"><span data-stu-id="f6b9a-188">Troubleshooting:</span></span>

* <span data-ttu-id="f6b9a-189">Potwierdź, że prawidłowy punkt końcowy URI aplikacji jest używany.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-189">Confirm the correct URI endpoint for the app is in use.</span></span> <span data-ttu-id="f6b9a-190">Sprawdź powiązania.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-190">Check the bindings.</span></span>

* <span data-ttu-id="f6b9a-191">Upewnij się, że witryna internetowa usług IIS nie jest w stanie *zatrzymania* .</span><span class="sxs-lookup"><span data-stu-id="f6b9a-191">Confirm that the IIS website isn't in the *Stopped* state.</span></span>

## <a name="corewebengine-or-w3svc-server-features-disabled"></a><span data-ttu-id="f6b9a-192">Funkcje serwera CoreWebEngine lub W3SVC są wyłączone</span><span class="sxs-lookup"><span data-stu-id="f6b9a-192">CoreWebEngine or W3SVC server features disabled</span></span>

<span data-ttu-id="f6b9a-193">**Wyjątek systemu operacyjnego:** Aby można było używać modułu ASP.NET Core, należy zainstalować funkcje CoreWebEngine i W3SVC usług IIS 7,0.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-193">**OS Exception:** The IIS 7.0 CoreWebEngine and W3SVC features must be installed to use the ASP.NET Core Module.</span></span>

<span data-ttu-id="f6b9a-194">Rozwiązywanie problemów:</span><span class="sxs-lookup"><span data-stu-id="f6b9a-194">Troubleshooting:</span></span>

<span data-ttu-id="f6b9a-195">Upewnij się, że są włączone odpowiednie role i funkcje.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-195">Confirm that the proper role and features are enabled.</span></span> <span data-ttu-id="f6b9a-196">Zobacz [Konfiguracja usług IIS](xref:host-and-deploy/iis/index#iis-configuration).</span><span class="sxs-lookup"><span data-stu-id="f6b9a-196">See [IIS Configuration](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

## <a name="incorrect-website-physical-path-or-app-missing"></a><span data-ttu-id="f6b9a-197">Brak nieprawidłowej ścieżki fizycznej witryny sieci Web lub aplikacji</span><span class="sxs-lookup"><span data-stu-id="f6b9a-197">Incorrect website physical path or app missing</span></span>

* <span data-ttu-id="f6b9a-198">**Przeglądarka:** 403 — Dostęp zabroniony — odmowa dostępu **--lub--** 403,14 zabronione — serwer sieci Web jest skonfigurowany tak, aby nie wyświetlać zawartości tego katalogu.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-198">**Browser:** 403 Forbidden - Access is denied **--OR--** 403.14 Forbidden - The Web server is configured to not list the contents of this directory.</span></span>

* <span data-ttu-id="f6b9a-199">**Dziennik aplikacji:** Brak wpisu</span><span class="sxs-lookup"><span data-stu-id="f6b9a-199">**Application Log:** No entry</span></span>

* <span data-ttu-id="f6b9a-200">**Dziennik stdout modułu ASP.NET Core:** Plik dziennika nie został utworzony.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-200">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="f6b9a-201">**Dziennik debugowania modułu ASP.NET Core:** Plik dziennika nie został utworzony.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-201">**ASP.NET Core Module Debug Log:** The log file isn't created.</span></span>

::: moniker-end

<span data-ttu-id="f6b9a-202">Rozwiązywanie problemów:</span><span class="sxs-lookup"><span data-stu-id="f6b9a-202">Troubleshooting:</span></span>

<span data-ttu-id="f6b9a-203">Sprawdź **Ustawienia podstawowe** witryny sieci Web usług IIS i folder aplikacji fizycznych.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-203">Check the IIS website **Basic Settings** and the physical app folder.</span></span> <span data-ttu-id="f6b9a-204">Upewnij się, że aplikacja znajduje się w folderze w **ścieżce fizycznej**witryny sieci Web usług IIS.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-204">Confirm that the app is in the folder at the IIS website **Physical path**.</span></span>

## <a name="incorrect-role-aspnet-core-module-not-installed-or-incorrect-permissions"></a><span data-ttu-id="f6b9a-205">Nieprawidłowa rola, moduł ASP.NET Core nie jest zainstalowany lub nieprawidłowe uprawnienia</span><span class="sxs-lookup"><span data-stu-id="f6b9a-205">Incorrect role, ASP.NET Core Module not installed, or incorrect permissions</span></span>

* <span data-ttu-id="f6b9a-206">**Przeglądarka:** 500,19 wewnętrzny błąd serwera — nie można uzyskać dostępu do żądanej strony, ponieważ powiązane dane konfiguracji strony są nieprawidłowe.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-206">**Browser:** 500.19 Internal Server Error - The requested page cannot be accessed because the related configuration data for the page is invalid.</span></span> <span data-ttu-id="f6b9a-207">**--Lub--** Nie można wyświetlić tej strony</span><span class="sxs-lookup"><span data-stu-id="f6b9a-207">**--OR--** This page can't be displayed</span></span>

* <span data-ttu-id="f6b9a-208">**Dziennik aplikacji:** Brak wpisu</span><span class="sxs-lookup"><span data-stu-id="f6b9a-208">**Application Log:** No entry</span></span>

* <span data-ttu-id="f6b9a-209">**Dziennik stdout modułu ASP.NET Core:** Plik dziennika nie został utworzony.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-209">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="f6b9a-210">**Dziennik debugowania modułu ASP.NET Core:** Plik dziennika nie został utworzony.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-210">**ASP.NET Core Module Debug Log:** The log file isn't created.</span></span>

::: moniker-end

<span data-ttu-id="f6b9a-211">Rozwiązywanie problemów:</span><span class="sxs-lookup"><span data-stu-id="f6b9a-211">Troubleshooting:</span></span>

* <span data-ttu-id="f6b9a-212">Upewnij się, że jest włączona właściwa rola.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-212">Confirm that the proper role is enabled.</span></span> <span data-ttu-id="f6b9a-213">Zobacz [Konfiguracja usług IIS](xref:host-and-deploy/iis/index#iis-configuration).</span><span class="sxs-lookup"><span data-stu-id="f6b9a-213">See [IIS Configuration](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

* <span data-ttu-id="f6b9a-214">Otwórz aplet **programy & funkcje** lub **aplikacje & funkcje** i upewnij się, że **Host systemu Windows Server** jest zainstalowany.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-214">Open **Programs & Features** or **Apps & features** and confirm that **Windows Server Hosting** is installed.</span></span> <span data-ttu-id="f6b9a-215">Jeśli **Host systemu Windows Server** nie znajduje się na liście zainstalowanych programów, Pobierz i zainstaluj pakiet hostingu platformy .NET Core.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-215">If **Windows Server Hosting** isn't present in the list of installed programs, download and install the .NET Core Hosting Bundle.</span></span>

  [<span data-ttu-id="f6b9a-216">Bieżący Instalatora pakietu hostingu programu .NET Core (pobieranie bezpośrednie)</span><span class="sxs-lookup"><span data-stu-id="f6b9a-216">Current .NET Core Hosting Bundle installer (direct download)</span></span>](https://www.microsoft.com/net/permalink/dotnetcore-current-windows-runtime-bundle-installer)

  <span data-ttu-id="f6b9a-217">Aby uzyskać więcej informacji, zobacz [Instalowanie pakietu hostingu .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span><span class="sxs-lookup"><span data-stu-id="f6b9a-217">For more information, see [Install the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span>

* <span data-ttu-id="f6b9a-218">Upewnij się, że dla **puli aplikacji** > **proces** > **tożsamość** jest ustawiona na **ApplicationPoolIdentity** , lub tożsamość niestandardowa ma odpowiednie uprawnienia dostępu do folderu wdrożenia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-218">Make sure that the **Application Pool** > **Process Model** > **Identity** is set to **ApplicationPoolIdentity** or the custom identity has the correct permissions to access the app's deployment folder.</span></span>

* <span data-ttu-id="f6b9a-219">Jeśli odinstalowano pakiet hostingu ASP.NET Core i zainstalowano wcześniejszą wersję pakietu hostingu, plik *ApplicationHost. config* nie zawiera sekcji dla modułu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-219">If you uninstalled the ASP.NET Core Hosting Bundle and installed an earlier version of the hosting bundle, the *applicationHost.config* file doesn't include a section for the ASP.NET Core Module.</span></span> <span data-ttu-id="f6b9a-220">Otwórz *plik ApplicationHost. config* w *folderze% windir%/system32/inetsrv/config* i Znajdź grupę sekcji `<configuration><configSections><sectionGroup name="system.webServer">`.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-220">Open *applicationHost.config* at *%windir%/System32/inetsrv/config* and find the `<configuration><configSections><sectionGroup name="system.webServer">` section group.</span></span> <span data-ttu-id="f6b9a-221">Jeśli w grupie sekcji brakuje sekcji modułu ASP.NET Core, Dodaj element Section:</span><span class="sxs-lookup"><span data-stu-id="f6b9a-221">If the section for the ASP.NET Core Module is missing from the section group, add the section element:</span></span>

  ```xml
  <section name="aspNetCore" overrideModeDefault="Allow" />
  ```

  <span data-ttu-id="f6b9a-222">Alternatywnie Zainstaluj najnowszą wersję pakietu hostingu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-222">Alternatively, install the latest version of the ASP.NET Core Hosting Bundle.</span></span> <span data-ttu-id="f6b9a-223">Najnowsza wersja jest wstecznie zgodna z obsługiwanymi ASP.NET Core aplikacjami.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-223">The latest version is backwards-compatible with supported ASP.NET Core apps.</span></span>

## <a name="incorrect-processpath-missing-path-variable-hosting-bundle-not-installed-systemiis-not-restarted-vc-redistributable-not-installed-or-dotnetexe-access-violation"></a><span data-ttu-id="f6b9a-224">Nieprawidłowa processPath, brakująca zmienna PATH, pakiet hostingu nie został zainstalowany, nie uruchomiono systemu/usług IIS, pakiet redystrybucyjny programu VC + + nie został zainstalowany lub naruszenie zasad dostępu dotnet. exe</span><span class="sxs-lookup"><span data-stu-id="f6b9a-224">Incorrect processPath, missing PATH variable, Hosting Bundle not installed, system/IIS not restarted, VC++ Redistributable not installed, or dotnet.exe access violation</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="f6b9a-225">**Przeglądarka:** Błąd HTTP 500,0 — błąd ładowania procedury obsługi ANCM w procesie</span><span class="sxs-lookup"><span data-stu-id="f6b9a-225">**Browser:** HTTP Error 500.0 - ANCM In-Process Handler Load Failure</span></span>

* <span data-ttu-id="f6b9a-226">**Dziennik aplikacji:** Nie można uruchomić procesu "MACHINE/WEBROOT/APPHOST/{ASSEMBLY}" z fizycznym elementem głównym "C: \{PATH} \'" przy użyciu wiersza polecenia "" {...} "</span><span class="sxs-lookup"><span data-stu-id="f6b9a-226">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' failed to start process with commandline '"{...}"</span></span> <span data-ttu-id="f6b9a-227">", ErrorCode =" 0x80070002: 0.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-227">', ErrorCode = '0x80070002 : 0.</span></span> <span data-ttu-id="f6b9a-228">Aplikacja "{PATH}" nie mogła zostać uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-228">Application '{PATH}' wasn't able to start.</span></span> <span data-ttu-id="f6b9a-229">Nie znaleziono pliku wykonywalnego w lokalizacji "{PATH}".</span><span class="sxs-lookup"><span data-stu-id="f6b9a-229">Executable was not found at '{PATH}'.</span></span> <span data-ttu-id="f6b9a-230">Nie można uruchomić aplikacji "/LM/W3SVC/2/ROOT", ErrorCode "0x8007023e".</span><span class="sxs-lookup"><span data-stu-id="f6b9a-230">Failed to start application '/LM/W3SVC/2/ROOT', ErrorCode '0x8007023e'.</span></span>

* <span data-ttu-id="f6b9a-231">**Dziennik stdout modułu ASP.NET Core:** Plik dziennika nie został utworzony.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-231">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

* <span data-ttu-id="f6b9a-232">**Dziennik debugowania modułu ASP.NET Core:** Dziennik zdarzeń: Nie można uruchomić aplikacji "{PATH}".</span><span class="sxs-lookup"><span data-stu-id="f6b9a-232">**ASP.NET Core Module Debug Log:** Event Log: 'Application '{PATH}' wasn't able to start.</span></span> <span data-ttu-id="f6b9a-233">Nie znaleziono pliku wykonywalnego w lokalizacji "{PATH}".</span><span class="sxs-lookup"><span data-stu-id="f6b9a-233">Executable was not found at '{PATH}'.</span></span> <span data-ttu-id="f6b9a-234">Zwrócony błąd HRESULT: 0x8007023e</span><span class="sxs-lookup"><span data-stu-id="f6b9a-234">Failed HRESULT returned: 0x8007023e</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="f6b9a-235">**Przeglądarka:** Błąd HTTP 502,5 — niepowodzenie procesu</span><span class="sxs-lookup"><span data-stu-id="f6b9a-235">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="f6b9a-236">**Dziennik aplikacji:** Nie można uruchomić procesu "MACHINE/WEBROOT/APPHOST/{ASSEMBLY}" z fizycznym elementem głównym "C: \{PATH} \'" przy użyciu wiersza polecenia "" {...} "</span><span class="sxs-lookup"><span data-stu-id="f6b9a-236">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' failed to start process with commandline '"{...}"</span></span> <span data-ttu-id="f6b9a-237">", ErrorCode =" 0x80070002: 0.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-237">', ErrorCode = '0x80070002 : 0.</span></span>

* <span data-ttu-id="f6b9a-238">**Dziennik stdout modułu ASP.NET Core:** Plik dziennika jest tworzony, ale pusty.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-238">**ASP.NET Core Module stdout Log:** The log file is created but empty.</span></span>

::: moniker-end

<span data-ttu-id="f6b9a-239">Rozwiązywanie problemów:</span><span class="sxs-lookup"><span data-stu-id="f6b9a-239">Troubleshooting:</span></span>

* <span data-ttu-id="f6b9a-240">Upewnij się, że aplikacja działa lokalnie na Kestrel.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-240">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="f6b9a-241">Niepowodzenie procesu może być wynikiem problemu w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-241">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="f6b9a-242">Aby uzyskać więcej informacji, zobacz <xref:test/troubleshoot-azure-iis>.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-242">For more information, see <xref:test/troubleshoot-azure-iis>.</span></span>

* <span data-ttu-id="f6b9a-243">Sprawdź atrybut *processPath* w elemencie `<aspNetCore>` w *pliku Web. config* , aby upewnić się, że jest `dotnet` dla wdrożenia zależnego od platformy (FDD) lub `.\{ASSEMBLY}.exe` dla [wdrożenia samodzielnego (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd).</span><span class="sxs-lookup"><span data-stu-id="f6b9a-243">Check the *processPath* attribute on the `<aspNetCore>` element in *web.config* to confirm that it's `dotnet` for a framework-dependent deployment (FDD) or `.\{ASSEMBLY}.exe` for a [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd).</span></span>

* <span data-ttu-id="f6b9a-244">W przypadku elementu FDD program *dotnet. exe* może być niedostępny za pośrednictwem ustawień ścieżki.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-244">For an FDD, *dotnet.exe* might not be accessible via the PATH settings.</span></span> <span data-ttu-id="f6b9a-245">Upewnij się, że w ustawieniach ścieżki systemowej istnieje wartość *C:\Program Files\dotnet @ no__t-1* .</span><span class="sxs-lookup"><span data-stu-id="f6b9a-245">Confirm that *C:\Program Files\dotnet\\* exists in the System PATH settings.</span></span>

* <span data-ttu-id="f6b9a-246">W przypadku programu FDD program *dotnet. exe* może być niedostępny dla tożsamości użytkownika puli aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-246">For an FDD, *dotnet.exe* might not be accessible for the user identity of the app pool.</span></span> <span data-ttu-id="f6b9a-247">Upewnij się, że tożsamość użytkownika puli aplikacji ma dostęp do katalogu *C:\Program Files\dotnet* .</span><span class="sxs-lookup"><span data-stu-id="f6b9a-247">Confirm that the app pool user identity has access to the *C:\Program Files\dotnet* directory.</span></span> <span data-ttu-id="f6b9a-248">Upewnij się, że nie ma skonfigurowanych reguł odmowy dla tożsamości użytkownika puli aplikacji w *folderze C:\Program Files\dotnet* i katalogach aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-248">Confirm that there are no deny rules configured for the app pool user identity on the *C:\Program Files\dotnet* and app directories.</span></span>

* <span data-ttu-id="f6b9a-249">FDD mógł zostać wdrożony, a platforma .NET Core została zainstalowana bez ponownego uruchamiania usług IIS.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-249">An FDD may have been deployed and .NET Core installed without restarting IIS.</span></span> <span data-ttu-id="f6b9a-250">Uruchom ponownie serwer lub Uruchom ponownie usługi IIS, wykonując polecenie **net stop** , a następnie **net start W3SVC** z wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-250">Either restart the server or restart IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

* <span data-ttu-id="f6b9a-251">FDD mógł zostać wdrożony bez instalowania środowiska uruchomieniowego .NET Core w systemie hostingu.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-251">An FDD may have been deployed without installing the .NET Core runtime on the hosting system.</span></span> <span data-ttu-id="f6b9a-252">Jeśli środowisko uruchomieniowe platformy .NET Core nie zostało zainstalowane, uruchom **Instalatora pakietu hostingu programu .NET Core** w systemie.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-252">If the .NET Core runtime hasn't been installed, run the **.NET Core Hosting Bundle installer** on the system.</span></span>

  [<span data-ttu-id="f6b9a-253">Bieżący Instalatora pakietu hostingu programu .NET Core (pobieranie bezpośrednie)</span><span class="sxs-lookup"><span data-stu-id="f6b9a-253">Current .NET Core Hosting Bundle installer (direct download)</span></span>](https://www.microsoft.com/net/permalink/dotnetcore-current-windows-runtime-bundle-installer)

  <span data-ttu-id="f6b9a-254">Aby uzyskać więcej informacji, zobacz [Instalowanie pakietu hostingu .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span><span class="sxs-lookup"><span data-stu-id="f6b9a-254">For more information, see [Install the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span>

  <span data-ttu-id="f6b9a-255">Jeśli jest wymagane określone środowisko uruchomieniowe, Pobierz środowisko uruchomieniowe z [archiwów pobierania programu .NET](https://dotnet.microsoft.com/download/archives) i zainstaluj je w systemie.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-255">If a specific runtime is required, download the runtime from the [.NET Download Archives](https://dotnet.microsoft.com/download/archives) and install it on the system.</span></span> <span data-ttu-id="f6b9a-256">Aby ukończyć instalację, należy ponownie uruchomić system lub ponownie uruchomić usługi IIS, wykonując polecenie **net stop** , a następnie **net start W3SVC** z wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-256">Complete the installation by restarting the system or restarting IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

## <a name="incorrect-arguments-of-aspnetcore-element"></a><span data-ttu-id="f6b9a-257">Nieprawidłowe argumenty elementu \<aspNetCore ></span><span class="sxs-lookup"><span data-stu-id="f6b9a-257">Incorrect arguments of \<aspNetCore> element</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="f6b9a-258">**Przeglądarka:** Błąd HTTP 500,0 — błąd ładowania procedury obsługi ANCM w procesie</span><span class="sxs-lookup"><span data-stu-id="f6b9a-258">**Browser:** HTTP Error 500.0 - ANCM In-Process Handler Load Failure</span></span>

* <span data-ttu-id="f6b9a-259">**Dziennik aplikacji:** Wywoływanie hostfxr w celu znalezienia procedury obsługi żądania przetworzenia nie powiodło się bez wyszukiwania natywnych zależności.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-259">**Application Log:** Invoking hostfxr to find the inprocess request handler failed without finding any native dependencies.</span></span> <span data-ttu-id="f6b9a-260">Najbardziej prawdopodobną przyczyną jest to, że aplikacja jest nieprawidłowo skonfigurowana. Sprawdź wersje programów Microsoft. servicecore. app i Microsoft. AspNetCore. app, które są przeznaczone dla aplikacji, i są zainstalowane na komputerze.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-260">This most likely means the app is misconfigured, please check the versions of Microsoft.NetCore.App and Microsoft.AspNetCore.App that are targeted by the application and are installed on the machine.</span></span> <span data-ttu-id="f6b9a-261">Nie można znaleźć procedury obsługi żądania nieprzetwarzania.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-261">Could not find inprocess request handler.</span></span> <span data-ttu-id="f6b9a-262">Przechwycono dane wyjściowe z wywołania hostfxr: Czy chodziło o uruchamianie poleceń zestawu SDK dotnet?</span><span class="sxs-lookup"><span data-stu-id="f6b9a-262">Captured output from invoking hostfxr: Did you mean to run dotnet SDK commands?</span></span> <span data-ttu-id="f6b9a-263">Zainstaluj zestaw SDK dotnet z: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409 nie można uruchomić aplikacji "/LM/W3SVC/3/ROOT", ErrorCode "0x8000FFFF".</span><span class="sxs-lookup"><span data-stu-id="f6b9a-263">Please install dotnet SDK from: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409 Failed to start application '/LM/W3SVC/3/ROOT', ErrorCode '0x8000ffff'.</span></span>

* <span data-ttu-id="f6b9a-264">**Dziennik stdout modułu ASP.NET Core:** Czy chodziło o uruchamianie poleceń zestawu SDK dotnet?</span><span class="sxs-lookup"><span data-stu-id="f6b9a-264">**ASP.NET Core Module stdout Log:** Did you mean to run dotnet SDK commands?</span></span> <span data-ttu-id="f6b9a-265">Zainstaluj zestaw SDK dotnet z: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409</span><span class="sxs-lookup"><span data-stu-id="f6b9a-265">Please install dotnet SDK from: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409</span></span>

* <span data-ttu-id="f6b9a-266">**Dziennik debugowania modułu ASP.NET Core:** Wywoływanie hostfxr w celu znalezienia procedury obsługi żądania przetworzenia nie powiodło się bez wyszukiwania natywnych zależności.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-266">**ASP.NET Core Module Debug Log:** Invoking hostfxr to find the inprocess request handler failed without finding any native dependencies.</span></span> <span data-ttu-id="f6b9a-267">Najbardziej prawdopodobną przyczyną jest to, że aplikacja jest nieprawidłowo skonfigurowana. Sprawdź wersje programów Microsoft. servicecore. app i Microsoft. AspNetCore. app, które są przeznaczone dla aplikacji, i są zainstalowane na komputerze.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-267">This most likely means the app is misconfigured, please check the versions of Microsoft.NetCore.App and Microsoft.AspNetCore.App that are targeted by the application and are installed on the machine.</span></span> <span data-ttu-id="f6b9a-268">Zwrócony błąd HRESULT: 0x8000FFFF nie może odnaleźć procedury obsługi żądania przetwarzania.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-268">Failed HRESULT returned: 0x8000ffff Could not find inprocess request handler.</span></span> <span data-ttu-id="f6b9a-269">Przechwycono dane wyjściowe z wywołania hostfxr: Czy chodziło o uruchamianie poleceń zestawu SDK dotnet?</span><span class="sxs-lookup"><span data-stu-id="f6b9a-269">Captured output from invoking hostfxr: Did you mean to run dotnet SDK commands?</span></span> <span data-ttu-id="f6b9a-270">Zainstaluj zestaw SDK dotnet z: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409 zwrócony błąd HRESULT: 0x8000ffff</span><span class="sxs-lookup"><span data-stu-id="f6b9a-270">Please install dotnet SDK from: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409 Failed HRESULT returned: 0x8000ffff</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="f6b9a-271">**Przeglądarka:** Błąd HTTP 502,5 — niepowodzenie procesu</span><span class="sxs-lookup"><span data-stu-id="f6b9a-271">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="f6b9a-272">**Dziennik aplikacji:** Nie można uruchomić aplikacji "MACHINE/WEBROOT/APPHOST/{ASSEMBLY}" z fizycznym głównym katalogiem "C: \{PATH} \' z elementem CommandLine" "dotnet". \{ASSEMBLY}. dll ", ErrorCode = ' 0x80004005: 80008081.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-272">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' failed to start process with commandline '"dotnet" .\{ASSEMBLY}.dll', ErrorCode = '0x80004005 : 80008081.</span></span>

* <span data-ttu-id="f6b9a-273">**Dziennik stdout modułu ASP.NET Core:** Aplikacja do wykonania nie istnieje: 'PATH\{ASSEMBLY}.dll'</span><span class="sxs-lookup"><span data-stu-id="f6b9a-273">**ASP.NET Core Module stdout Log:** The application to execute does not exist: 'PATH\{ASSEMBLY}.dll'</span></span>

::: moniker-end

<span data-ttu-id="f6b9a-274">Rozwiązywanie problemów:</span><span class="sxs-lookup"><span data-stu-id="f6b9a-274">Troubleshooting:</span></span>

* <span data-ttu-id="f6b9a-275">Upewnij się, że aplikacja działa lokalnie na Kestrel.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-275">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="f6b9a-276">Niepowodzenie procesu może być wynikiem problemu w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-276">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="f6b9a-277">Aby uzyskać więcej informacji, zobacz <xref:test/troubleshoot-azure-iis>.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-277">For more information, see <xref:test/troubleshoot-azure-iis>.</span></span>

* <span data-ttu-id="f6b9a-278">Sprawdź atrybut *arguments* w elemencie `<aspNetCore>` w *pliku Web. config* , aby upewnić się, że jest to (a) `.\{ASSEMBLY}.dll` dla wdrożenia zależnego od platformy (FDD); lub (b) nieobecny, pusty ciąg (`arguments=""`) lub lista argumentów aplikacji (`arguments="{ARGUMENT_1}, {ARGUMENT_2}, ... {ARGUMENT_X}"`) dla wdrożenia samodzielnego (SCD).</span><span class="sxs-lookup"><span data-stu-id="f6b9a-278">Examine the *arguments* attribute on the `<aspNetCore>` element in *web.config* to confirm that it's either (a) `.\{ASSEMBLY}.dll` for a framework-dependent deployment (FDD); or (b) not present, an empty string (`arguments=""`), or a list of the app's arguments (`arguments="{ARGUMENT_1}, {ARGUMENT_2}, ... {ARGUMENT_X}"`) for a self-contained deployment (SCD).</span></span>

::: moniker range=">= aspnetcore-2.2"

## <a name="missing-net-core-shared-framework"></a><span data-ttu-id="f6b9a-279">Brak współdzielonej platformy .NET Core</span><span class="sxs-lookup"><span data-stu-id="f6b9a-279">Missing .NET Core shared framework</span></span>

* <span data-ttu-id="f6b9a-280">**Przeglądarka:** Błąd HTTP 500,0 — błąd ładowania procedury obsługi ANCM w procesie</span><span class="sxs-lookup"><span data-stu-id="f6b9a-280">**Browser:** HTTP Error 500.0 - ANCM In-Process Handler Load Failure</span></span>

* <span data-ttu-id="f6b9a-281">**Dziennik aplikacji:** Wywoływanie hostfxr w celu znalezienia procedury obsługi żądania przetworzenia nie powiodło się bez wyszukiwania natywnych zależności.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-281">**Application Log:** Invoking hostfxr to find the inprocess request handler failed without finding any native dependencies.</span></span> <span data-ttu-id="f6b9a-282">Najbardziej prawdopodobną przyczyną jest to, że aplikacja jest nieprawidłowo skonfigurowana. Sprawdź wersje programów Microsoft. servicecore. app i Microsoft. AspNetCore. app, które są przeznaczone dla aplikacji, i są zainstalowane na komputerze.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-282">This most likely means the app is misconfigured, please check the versions of Microsoft.NetCore.App and Microsoft.AspNetCore.App that are targeted by the application and are installed on the machine.</span></span> <span data-ttu-id="f6b9a-283">Nie można znaleźć procedury obsługi żądania nieprzetwarzania.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-283">Could not find inprocess request handler.</span></span> <span data-ttu-id="f6b9a-284">Przechwycono dane wyjściowe z wywołania hostfxr: Nie można znaleźć żadnej zgodnej wersji platformy.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-284">Captured output from invoking hostfxr: It was not possible to find any compatible framework version.</span></span> <span data-ttu-id="f6b9a-285">Nie znaleziono określonej struktury "Microsoft. AspNetCore. app" w wersji "{VERSION}".</span><span class="sxs-lookup"><span data-stu-id="f6b9a-285">The specified framework 'Microsoft.AspNetCore.App', version '{VERSION}' was not found.</span></span>

<span data-ttu-id="f6b9a-286">Nie można uruchomić aplikacji "/LM/W3SVC/5/ROOT", ErrorCode "0x8000FFFF".</span><span class="sxs-lookup"><span data-stu-id="f6b9a-286">Failed to start application '/LM/W3SVC/5/ROOT', ErrorCode '0x8000ffff'.</span></span>

* <span data-ttu-id="f6b9a-287">**Dziennik stdout modułu ASP.NET Core:** Nie można znaleźć żadnej zgodnej wersji platformy.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-287">**ASP.NET Core Module stdout Log:** It was not possible to find any compatible framework version.</span></span> <span data-ttu-id="f6b9a-288">Nie znaleziono określonej struktury "Microsoft. AspNetCore. app" w wersji "{VERSION}".</span><span class="sxs-lookup"><span data-stu-id="f6b9a-288">The specified framework 'Microsoft.AspNetCore.App', version '{VERSION}' was not found.</span></span>

* <span data-ttu-id="f6b9a-289">**Dziennik debugowania modułu ASP.NET Core:** Zwrócony błąd HRESULT: 0x8000ffff</span><span class="sxs-lookup"><span data-stu-id="f6b9a-289">**ASP.NET Core Module Debug Log:** Failed HRESULT returned: 0x8000ffff</span></span>

::: moniker-end

<span data-ttu-id="f6b9a-290">Rozwiązywanie problemów:</span><span class="sxs-lookup"><span data-stu-id="f6b9a-290">Troubleshooting:</span></span>

<span data-ttu-id="f6b9a-291">W przypadku wdrażania zależnego od platformy (FDD) Upewnij się, że w systemie jest zainstalowane odpowiednie środowisko uruchomieniowe.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-291">For a framework-dependent deployment (FDD), confirm that the correct runtime installed on the system.</span></span>

## <a name="stopped-application-pool"></a><span data-ttu-id="f6b9a-292">Zatrzymana Pula aplikacji</span><span class="sxs-lookup"><span data-stu-id="f6b9a-292">Stopped Application Pool</span></span>

* <span data-ttu-id="f6b9a-293">**Przeglądarka:** Usługa 503 jest niedostępna</span><span class="sxs-lookup"><span data-stu-id="f6b9a-293">**Browser:** 503 Service Unavailable</span></span>

* <span data-ttu-id="f6b9a-294">**Dziennik aplikacji:** Brak wpisu</span><span class="sxs-lookup"><span data-stu-id="f6b9a-294">**Application Log:** No entry</span></span>

* <span data-ttu-id="f6b9a-295">**Dziennik stdout modułu ASP.NET Core:** Plik dziennika nie został utworzony.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-295">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="f6b9a-296">**Dziennik debugowania modułu ASP.NET Core:** Plik dziennika nie został utworzony.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-296">**ASP.NET Core Module Debug Log:** The log file isn't created.</span></span>

::: moniker-end

<span data-ttu-id="f6b9a-297">Rozwiązywanie problemów:</span><span class="sxs-lookup"><span data-stu-id="f6b9a-297">Troubleshooting:</span></span>

<span data-ttu-id="f6b9a-298">Upewnij się, że Pula aplikacji nie jest w stanie *zatrzymania* .</span><span class="sxs-lookup"><span data-stu-id="f6b9a-298">Confirm that the Application Pool isn't in the *Stopped* state.</span></span>

## <a name="sub-application-includes-a-handlers-section"></a><span data-ttu-id="f6b9a-299">Podaplikacja zawiera sekcję \<handlers ></span><span class="sxs-lookup"><span data-stu-id="f6b9a-299">Sub-application includes a \<handlers> section</span></span>

* <span data-ttu-id="f6b9a-300">**Przeglądarka:** Błąd HTTP 500,19 — wewnętrzny błąd serwera</span><span class="sxs-lookup"><span data-stu-id="f6b9a-300">**Browser:** HTTP Error 500.19 - Internal Server Error</span></span>

* <span data-ttu-id="f6b9a-301">**Dziennik aplikacji:** Brak wpisu</span><span class="sxs-lookup"><span data-stu-id="f6b9a-301">**Application Log:** No entry</span></span>

* <span data-ttu-id="f6b9a-302">**Dziennik stdout modułu ASP.NET Core:** Plik dziennika aplikacji głównej jest tworzony i pokazuje normalną operację.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-302">**ASP.NET Core Module stdout Log:** The root app's log file is created and shows normal operation.</span></span> <span data-ttu-id="f6b9a-303">Nie utworzono pliku dziennika aplikacji podrzędnej.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-303">The sub-app's log file isn't created.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="f6b9a-304">**Dziennik debugowania modułu ASP.NET Core:** Plik dziennika aplikacji głównej jest tworzony i pokazuje normalną operację.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-304">**ASP.NET Core Module Debug Log:** The root app's log file is created and shows normal operation.</span></span> <span data-ttu-id="f6b9a-305">Nie utworzono pliku dziennika aplikacji podrzędnej.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-305">The sub-app's log file isn't created.</span></span>

::: moniker-end

<span data-ttu-id="f6b9a-306">Rozwiązywanie problemów:</span><span class="sxs-lookup"><span data-stu-id="f6b9a-306">Troubleshooting:</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="f6b9a-307">Upewnij się, że plik *Web. config* aplikacji podrzędnej nie zawiera sekcji `<handlers>` lub że aplikacja podrzędna nie dziedziczy programów obsługi aplikacji nadrzędnej.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-307">Confirm that the sub-app's *web.config* file doesn't include a `<handlers>` section or that the sub-app doesn't inherit the parent app's handlers.</span></span>

<span data-ttu-id="f6b9a-308">Sekcja `<system.webServer>` aplikacji nadrzędnej w *pliku Web. config* jest umieszczona wewnątrz elementu `<location>`.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-308">The parent app's `<system.webServer>` section of *web.config* is placed inside of a `<location>` element.</span></span> <span data-ttu-id="f6b9a-309">Właściwość <xref:System.Configuration.SectionInformation.InheritInChildApplications*> jest ustawiona na `false` w celu wskazania, że ustawienia określone w elemencie [> \<location](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) nie są dziedziczone przez aplikacje, które znajdują się w podkatalogu aplikacji nadrzędnej.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-309">The <xref:System.Configuration.SectionInformation.InheritInChildApplications*> property is set to `false` to indicate that the settings specified within the [\<location>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) element aren't inherited by apps that reside in a subdirectory of the parent app.</span></span> <span data-ttu-id="f6b9a-310">Aby uzyskać więcej informacji, zobacz <xref:host-and-deploy/aspnet-core-module>.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-310">For more information, see <xref:host-and-deploy/aspnet-core-module>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="f6b9a-311">Upewnij się, że plik *Web. config* aplikacji podrzędnej nie zawiera sekcji `<handlers>`.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-311">Confirm that the sub-app's *web.config* file doesn't include a `<handlers>` section.</span></span>

::: moniker-end

## <a name="stdout-log-path-incorrect"></a><span data-ttu-id="f6b9a-312">Nieprawidłowa ścieżka dziennika stdout</span><span class="sxs-lookup"><span data-stu-id="f6b9a-312">stdout log path incorrect</span></span>

* <span data-ttu-id="f6b9a-313">**Przeglądarka:** Aplikacja reaguje zwykle.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-313">**Browser:** The app responds normally.</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="f6b9a-314">**Dziennik aplikacji:** Nie można uruchomić przekierowania stdout w lokalizacji C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-314">**Application Log:** Could not start stdout redirection in C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span></span> <span data-ttu-id="f6b9a-315">Komunikat wyjątku: Wartość HRESULT 0x80070005 zwrócona w {PATH} \aspnetcoremodulev2\commonlib\fileoutputmanager.cpp: 84.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-315">Exception message: HRESULT 0x80070005 returned at {PATH}\aspnetcoremodulev2\commonlib\fileoutputmanager.cpp:84.</span></span> <span data-ttu-id="f6b9a-316">Nie można zatrzymać przekierowania strumienia stdout w folderze C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-316">Could not stop stdout redirection in C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span></span> <span data-ttu-id="f6b9a-317">Komunikat wyjątku: Wartość HRESULT 0x80070002 zwrócona w lokalizacji {PATH}.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-317">Exception message: HRESULT 0x80070002 returned at {PATH}.</span></span> <span data-ttu-id="f6b9a-318">Nie można uruchomić przekierowania stdout w lokalizacji {PATH} \aspnetcorev2_inprocess.dll.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-318">Could not start stdout redirection in {PATH}\aspnetcorev2_inprocess.dll.</span></span>

* <span data-ttu-id="f6b9a-319">**Dziennik stdout modułu ASP.NET Core:** Plik dziennika nie został utworzony.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-319">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

* <span data-ttu-id="f6b9a-320">**Dziennik debugowania modułu ASP.NET Core:** Nie można uruchomić przekierowania stdout w lokalizacji C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-320">**ASP.NET Core Module debug Log:** Could not start stdout redirection in C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span></span> <span data-ttu-id="f6b9a-321">Komunikat wyjątku: Wartość HRESULT 0x80070005 zwrócona w {PATH} \aspnetcoremodulev2\commonlib\fileoutputmanager.cpp: 84.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-321">Exception message: HRESULT 0x80070005 returned at {PATH}\aspnetcoremodulev2\commonlib\fileoutputmanager.cpp:84.</span></span> <span data-ttu-id="f6b9a-322">Nie można zatrzymać przekierowania strumienia stdout w folderze C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-322">Could not stop stdout redirection in C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span></span> <span data-ttu-id="f6b9a-323">Komunikat wyjątku: Wartość HRESULT 0x80070002 zwrócona w lokalizacji {PATH}.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-323">Exception message: HRESULT 0x80070002 returned at {PATH}.</span></span> <span data-ttu-id="f6b9a-324">Nie można uruchomić przekierowania stdout w lokalizacji {PATH} \aspnetcorev2_inprocess.dll.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-324">Could not start stdout redirection in {PATH}\aspnetcorev2_inprocess.dll.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="f6b9a-325">**Dziennik aplikacji:** Ostrzeżenie: Nie można utworzyć stdoutLogFile \\? \{PATH} \path_doesnt_exist\stdout_{PROCESS ID} _ {TIMESTAMP}. log, ErrorCode =-2147024893.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-325">**Application Log:** Warning: Could not create stdoutLogFile \\?\{PATH}\path_doesnt_exist\stdout_{PROCESS ID}_{TIMESTAMP}.log, ErrorCode = -2147024893.</span></span>

* <span data-ttu-id="f6b9a-326">**Dziennik stdout modułu ASP.NET Core:** Plik dziennika nie został utworzony.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-326">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

::: moniker-end

<span data-ttu-id="f6b9a-327">Rozwiązywanie problemów:</span><span class="sxs-lookup"><span data-stu-id="f6b9a-327">Troubleshooting:</span></span>

* <span data-ttu-id="f6b9a-328">Ścieżka `stdoutLogFile` określona w elemencie `<aspNetCore>` *pliku Web. config* nie istnieje.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-328">The `stdoutLogFile` path specified in the `<aspNetCore>` element of *web.config* doesn't exist.</span></span> <span data-ttu-id="f6b9a-329">Aby uzyskać więcej informacji, zobacz [ASP.NET Core Module: Tworzenie i przekierowywanie dziennika @ no__t-0.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-329">For more information, see [ASP.NET Core Module: Log creation and redirection](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection).</span></span>

* <span data-ttu-id="f6b9a-330">Użytkownik puli aplikacji nie ma dostępu do zapisu w ścieżce dziennika stdout.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-330">The app pool user doesn't have write access to the stdout log path.</span></span>

## <a name="application-configuration-general-issue"></a><span data-ttu-id="f6b9a-331">Ogólny problem z konfiguracją aplikacji</span><span class="sxs-lookup"><span data-stu-id="f6b9a-331">Application configuration general issue</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="f6b9a-332">**Przeglądarka:** Błąd HTTP 500,0 — błąd ładowania procedury obsługi ANCM w procesie — **lub —** błąd HTTP 500,30-ANCM w procesie — niepowodzenie uruchamiania</span><span class="sxs-lookup"><span data-stu-id="f6b9a-332">**Browser:** HTTP Error 500.0 - ANCM In-Process Handler Load Failure **--OR--** HTTP Error 500.30 - ANCM In-Process Start Failure</span></span>

* <span data-ttu-id="f6b9a-333">**Dziennik aplikacji:** Zmienna</span><span class="sxs-lookup"><span data-stu-id="f6b9a-333">**Application Log:** Variable</span></span>

* <span data-ttu-id="f6b9a-334">**Dziennik stdout modułu ASP.NET Core:** Plik dziennika jest tworzony, ale pusty lub tworzony przy użyciu zwykłych wpisów do momentu awarii aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-334">**ASP.NET Core Module stdout Log:** The log file is created but empty or created with normal entries until the point of the app failing.</span></span>

* <span data-ttu-id="f6b9a-335">**Dziennik debugowania modułu ASP.NET Core:** Zmienna</span><span class="sxs-lookup"><span data-stu-id="f6b9a-335">**ASP.NET Core Module Debug Log:** Variable</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="f6b9a-336">**Przeglądarka:** Błąd HTTP 502,5 — niepowodzenie procesu</span><span class="sxs-lookup"><span data-stu-id="f6b9a-336">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="f6b9a-337">**Dziennik aplikacji:** Aplikacja "MACHINE/WEBROOT/APPHOST/{ASSEMBLY}" z fizycznym głównym katalogiem "C: \{PATH} \' została utworzona przy użyciu wiersza polecenia" "C: \{PATH} \{ASSEMBLY}. {exe | dll} "", ale wystąpiła awaria lub nie nasłuchuje na danym porcie "{PORT}", ErrorCode = "{ERROR}"</span><span class="sxs-lookup"><span data-stu-id="f6b9a-337">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' created process with commandline '"C:\{PATH}\{ASSEMBLY}.{exe|dll}" ' but either crashed or did not respond or did not listen on the given port '{PORT}', ErrorCode = '{ERROR CODE}'</span></span>

* <span data-ttu-id="f6b9a-338">**Dziennik stdout modułu ASP.NET Core:** Plik dziennika jest tworzony, ale pusty.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-338">**ASP.NET Core Module stdout Log:** The log file is created but empty.</span></span>

::: moniker-end

<span data-ttu-id="f6b9a-339">Rozwiązywanie problemów:</span><span class="sxs-lookup"><span data-stu-id="f6b9a-339">Troubleshooting:</span></span>

<span data-ttu-id="f6b9a-340">Nie można uruchomić procesu, prawdopodobnie z powodu problemu z konfiguracją lub programowaniem aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f6b9a-340">The process failed to start, most likely due to an app configuration or programming issue.</span></span>

<span data-ttu-id="f6b9a-341">Więcej informacji znajduje się w następujących tematach:</span><span class="sxs-lookup"><span data-stu-id="f6b9a-341">For more information, see the following topics:</span></span>

* <xref:test/troubleshoot-azure-iis>
* <xref:test/troubleshoot>
