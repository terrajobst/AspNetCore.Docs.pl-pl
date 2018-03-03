---
title: "Typowe błędy odwołania dla usługi Azure App Service i IIS z platformy ASP.NET Core"
author: guardrex
description: "Rozróżnianie typowe błędy hosting aplikacji platformy ASP.NET Core w usłudze aplikacji Azure i usług IIS."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 03/13/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/azure-iis-errors-reference
ms.openlocfilehash: cd9f8fc310ba0258477db51aa416c03debadeffe
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/02/2018
---
# <a name="common-errors-reference-for-azure-app-service-and-iis-with-aspnet-core"></a><span data-ttu-id="32e7d-103">Typowe błędy odwołania dla usługi Azure App Service i IIS z platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="32e7d-103">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>

<span data-ttu-id="32e7d-104">Przez [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="32e7d-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="32e7d-105">Następujące nie znajduje się pełna lista błędów.</span><span class="sxs-lookup"><span data-stu-id="32e7d-105">The following isn't a complete list of errors.</span></span> <span data-ttu-id="32e7d-106">Jeśli wystąpi błąd niewymienione w tym [Otwórz nowy problem](https://github.com/aspnet/Docs/issues/new) z szczegółowe instrukcje dotyczące odtwarzania błędu.</span><span class="sxs-lookup"><span data-stu-id="32e7d-106">If you encounter an error not listed here, [open a new issue](https://github.com/aspnet/Docs/issues/new) with detailed instructions to reproduce the error.</span></span>

<span data-ttu-id="32e7d-107">Zbierz wymienione poniżej informacje.</span><span class="sxs-lookup"><span data-stu-id="32e7d-107">Collect the following information:</span></span>

* <span data-ttu-id="32e7d-108">Zachowanie przeglądarki</span><span class="sxs-lookup"><span data-stu-id="32e7d-108">Browser behavior</span></span>
* <span data-ttu-id="32e7d-109">Wpisy w dzienniku zdarzeń aplikacji</span><span class="sxs-lookup"><span data-stu-id="32e7d-109">Application Event Log entries</span></span>
* <span data-ttu-id="32e7d-110">Wpisy dziennika stdout Module głównym programu ASP.NET</span><span class="sxs-lookup"><span data-stu-id="32e7d-110">ASP.NET Core Module stdout log entries</span></span>

<span data-ttu-id="32e7d-111">Porównywanie informacji do poniższych typowych błędów.</span><span class="sxs-lookup"><span data-stu-id="32e7d-111">Compare the information to the following common errors.</span></span> <span data-ttu-id="32e7d-112">Jeśli zostanie znaleziony dopasowanie, wykonaj porady dotyczące rozwiązywania problemów.</span><span class="sxs-lookup"><span data-stu-id="32e7d-112">If a match is found, follow the troubleshooting advice.</span></span>

[!INCLUDE[Azure App Service Preview Notice](../includes/azure-apps-preview-notice.md)]

## <a name="installer-unable-to-obtain-vc-redistributable"></a><span data-ttu-id="32e7d-113">Nie można uzyskać VC ++ pakiet redystrybucyjny Instalatora</span><span class="sxs-lookup"><span data-stu-id="32e7d-113">Installer unable to obtain VC++ Redistributable</span></span>

* <span data-ttu-id="32e7d-114">**Instalator wyjątek:** 0x80072efd lub 0x80072f76 — nieokreślony błąd</span><span class="sxs-lookup"><span data-stu-id="32e7d-114">**Installer Exception:** 0x80072efd or 0x80072f76 - Unspecified error</span></span>

* <span data-ttu-id="32e7d-115">**Wyjątek dziennika Instalatora &#8224;:** błąd 0x80072efd lub 0x80072f76: nie można wykonać EXE pakietu</span><span class="sxs-lookup"><span data-stu-id="32e7d-115">**Installer Log Exception&#8224;:** Error 0x80072efd or 0x80072f76: Failed to execute EXE package</span></span>

  <span data-ttu-id="32e7d-116">&#8224; Dziennik znajduje się pod adresem C:\Users\\{USER}\AppData\Local\Temp\dd_DotNetCoreWinSvrHosting__{timestamp}.log.</span><span class="sxs-lookup"><span data-stu-id="32e7d-116">&#8224;The log is located at C:\Users\\{USER}\AppData\Local\Temp\dd_DotNetCoreWinSvrHosting__{timestamp}.log.</span></span>

<span data-ttu-id="32e7d-117">Rozwiązywanie problemów:</span><span class="sxs-lookup"><span data-stu-id="32e7d-117">Troubleshooting:</span></span>

* <span data-ttu-id="32e7d-118">Jeśli system nie ma dostępu do Internetu, podczas instalowania serwera obsługującego pakietu, ten wyjątek występuje, gdy Instalator będzie mógł uzyskiwania *Microsoft Visual C++ 2015 Redistributable*.</span><span class="sxs-lookup"><span data-stu-id="32e7d-118">If the system doesn't have Internet access while installing the server hosting bundle, this exception occurs when the installer is prevented from obtaining the *Microsoft Visual C++ 2015 Redistributable*.</span></span> <span data-ttu-id="32e7d-119">Uzyskać Instalatora z [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840).</span><span class="sxs-lookup"><span data-stu-id="32e7d-119">Obtain an installer from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840).</span></span> <span data-ttu-id="32e7d-120">Jeśli Instalator zakończy się niepowodzeniem, serwer nie może otrzymywać środowiska uruchomieniowego .NET Core wymaganego do obsługi wdrożenia framework zależne (stacje).</span><span class="sxs-lookup"><span data-stu-id="32e7d-120">If the installer fails, the server may not receive the .NET Core runtime required to host a framework-dependent deployment (FDD).</span></span> <span data-ttu-id="32e7d-121">Jeśli hosting Dyskietki, upewnij się, że środowisko wykonawcze jest instalowana w programach &amp; funkcji.</span><span class="sxs-lookup"><span data-stu-id="32e7d-121">If hosting an FDD, confirm that the runtime is installed in Programs &amp; Features.</span></span> <span data-ttu-id="32e7d-122">W razie potrzeby Uzyskaj Instalatora środowiska uruchomieniowego, z [pobiera .NET](https://www.microsoft.com/net/download/core).</span><span class="sxs-lookup"><span data-stu-id="32e7d-122">If needed, obtain a runtime installer from [.NET Downloads](https://www.microsoft.com/net/download/core).</span></span> <span data-ttu-id="32e7d-123">Po zainstalowaniu środowiska uruchomieniowego, ponownie uruchom system, lub ponownego uruchomienia usług IIS, wykonując **net stop została /y** następuje **net start w3svc** z wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="32e7d-123">After installing the runtime, restart the system or restart IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

## <a name="os-upgrade-removed-the-32-bit-aspnet-core-module"></a><span data-ttu-id="32e7d-124">Uaktualnienie systemu operacyjnego usunąć moduł 32-bitowej platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="32e7d-124">OS upgrade removed the 32-bit ASP.NET Core Module</span></span>

* <span data-ttu-id="32e7d-125">**Dziennik aplikacji:** modułu DLL **C:\WINDOWS\system32\inetsrv\aspnetcore.dll** nie można załadować.</span><span class="sxs-lookup"><span data-stu-id="32e7d-125">**Application Log:** The Module DLL **C:\WINDOWS\system32\inetsrv\aspnetcore.dll** failed to load.</span></span> <span data-ttu-id="32e7d-126">Dane to kod błędu.</span><span class="sxs-lookup"><span data-stu-id="32e7d-126">The data is the error.</span></span>

<span data-ttu-id="32e7d-127">Rozwiązywanie problemów:</span><span class="sxs-lookup"><span data-stu-id="32e7d-127">Troubleshooting:</span></span>

* <span data-ttu-id="32e7d-128">Pliki systemu operacyjnego bez **C:\Windows\SysWOW64\inetsrv** katalogu nie są zachowywane podczas OS uaktualnienia.</span><span class="sxs-lookup"><span data-stu-id="32e7d-128">Non-OS files in the **C:\Windows\SysWOW64\inetsrv** directory aren't preserved during an OS upgrade.</span></span> <span data-ttu-id="32e7d-129">Jeśli zainstalowano modułu platformy ASP.NET Core przed uaktualnienia systemu operacyjnego, a następnie wszystkie puli aplikacji jest uruchomiony w trybie 32-bitowej, po uaktualnieniu systemu operacyjnego, ten problem.</span><span class="sxs-lookup"><span data-stu-id="32e7d-129">If the ASP.NET Core Module is installed prior to an OS upgrade and then any AppPool is run in 32-bit mode after an OS upgrade, this issue is encountered.</span></span> <span data-ttu-id="32e7d-130">Po uaktualnieniu systemu operacyjnego Napraw moduł platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="32e7d-130">After an OS upgrade, repair the ASP.NET Core Module.</span></span> <span data-ttu-id="32e7d-131">Zobacz [instalacji pakietu .NET Core systemu Windows serwer obsługujący](xref:host-and-deploy/iis/index#install-the-net-core-windows-server-hosting-bundle).</span><span class="sxs-lookup"><span data-stu-id="32e7d-131">See [Install the .NET Core Windows Server Hosting bundle](xref:host-and-deploy/iis/index#install-the-net-core-windows-server-hosting-bundle).</span></span> <span data-ttu-id="32e7d-132">Wybierz **naprawy** po uruchomieniu Instalatora.</span><span class="sxs-lookup"><span data-stu-id="32e7d-132">Select **Repair** when the installer is run.</span></span>

## <a name="platform-conflicts-with-rid"></a><span data-ttu-id="32e7d-133">Platforma w konflikcie z identyfikatorów RID</span><span class="sxs-lookup"><span data-stu-id="32e7d-133">Platform conflicts with RID</span></span>

* <span data-ttu-id="32e7d-134">**Przeglądarki:** błąd HTTP 502.5 — błąd procesu</span><span class="sxs-lookup"><span data-stu-id="32e7d-134">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="32e7d-135">**Dziennik aplikacji:** aplikacji "MACHINE/WEBROOT/APPHOST / {zestawu}" z fizyczny katalog główny "C:\{ścieżki}\' nie można uruchomić procesu z wiersza polecenia" "C:\\{PATH} {zestawu}. { exe | dll} "", kod błędu = "0x80004005: ff.</span><span class="sxs-lookup"><span data-stu-id="32e7d-135">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' failed to start process with commandline '"C:\\{PATH}{assembly}.{exe|dll}" ', ErrorCode = '0x80004005 : ff.</span></span>

* <span data-ttu-id="32e7d-136">**Dziennik modułu platformy ASP.NET Core:** nieobsługiwany wyjątek: System.BadImageFormatException: nie można załadować pliku lub zestawu "dll {zestawu}".</span><span class="sxs-lookup"><span data-stu-id="32e7d-136">**ASP.NET Core Module Log:** Unhandled Exception: System.BadImageFormatException: Could not load file or assembly '{assembly}.dll'.</span></span> <span data-ttu-id="32e7d-137">Próbowano załadować program w niepoprawnym formacie.</span><span class="sxs-lookup"><span data-stu-id="32e7d-137">An attempt was made to load a program with an incorrect format.</span></span>

<span data-ttu-id="32e7d-138">Rozwiązywanie problemów:</span><span class="sxs-lookup"><span data-stu-id="32e7d-138">Troubleshooting:</span></span>

* <span data-ttu-id="32e7d-139">Upewnij się, że aplikacja działa lokalnie na Kestrel.</span><span class="sxs-lookup"><span data-stu-id="32e7d-139">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="32e7d-140">Błąd procesu może być wynikiem problemu w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="32e7d-140">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="32e7d-141">Aby uzyskać więcej informacji, zobacz [Rozwiązywanie problemów](xref:host-and-deploy/iis/troubleshoot).</span><span class="sxs-lookup"><span data-stu-id="32e7d-141">For more information, see [Troubleshooting](xref:host-and-deploy/iis/troubleshoot).</span></span>

* <span data-ttu-id="32e7d-142">Upewnij się, że `<PlatformTarget>` w *.csproj* nie koliduje to z RID.</span><span class="sxs-lookup"><span data-stu-id="32e7d-142">Confirm that the `<PlatformTarget>` in the *.csproj* doesn't conflict with the RID.</span></span> <span data-ttu-id="32e7d-143">Na przykład określić nie `<PlatformTarget>` z `x86` i publikowanie za pomocą RID `win10-x64`, przy użyciu *dotnet publikowania - c - r wersji win10-x64* lub przez ustawienie `<RuntimeIdentifiers>` w *.csproj*  do `win10-x64`.</span><span class="sxs-lookup"><span data-stu-id="32e7d-143">For example, don't specify a `<PlatformTarget>` of `x86` and publish with an RID of `win10-x64`, either by using *dotnet publish -c Release -r win10-x64* or by setting the `<RuntimeIdentifiers>` in the *.csproj* to `win10-x64`.</span></span> <span data-ttu-id="32e7d-144">Projekt publikuje bez ostrzeżenia lub błędu, ale kończy się niepowodzeniem z powyższych wyjątki zarejestrowane w systemie.</span><span class="sxs-lookup"><span data-stu-id="32e7d-144">The project publishes without warning or error but fails with the above logged exceptions on the system.</span></span>

* <span data-ttu-id="32e7d-145">Jeśli ten wyjątek występuje wdrożenia aplikacji Azure, podczas uaktualniania aplikacji i wdrożenie nowszej zestawy, ręcznie usuń wszystkie pliki z poprzedniego wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="32e7d-145">If this exception occurs for an Azure Apps deployment when upgrading an app and deploying newer assemblies, manually delete all files from the prior deployment.</span></span> <span data-ttu-id="32e7d-146">Pokutujące niezgodne zestawy może spowodować `System.BadImageFormatException` wyjątków w przypadku wdrażania uaktualnionego aplikacji.</span><span class="sxs-lookup"><span data-stu-id="32e7d-146">Lingering incompatible assemblies can result in a `System.BadImageFormatException` exception when deploying an upgraded app.</span></span>

## <a name="uri-endpoint-wrong-or-stopped-website"></a><span data-ttu-id="32e7d-147">Identyfikator URI punktu końcowego nieprawidłowe lub zatrzymania witryny sieci Web</span><span class="sxs-lookup"><span data-stu-id="32e7d-147">URI endpoint wrong or stopped website</span></span>

* <span data-ttu-id="32e7d-148">**Przeglądarki:** ERR_CONNECTION_REFUSED</span><span class="sxs-lookup"><span data-stu-id="32e7d-148">**Browser:** ERR_CONNECTION_REFUSED</span></span>

* <span data-ttu-id="32e7d-149">**Dziennik aplikacji:** wpisu</span><span class="sxs-lookup"><span data-stu-id="32e7d-149">**Application Log:** No entry</span></span>

* <span data-ttu-id="32e7d-150">**Dziennik modułu platformy ASP.NET Core:** nie utworzono pliku dziennika</span><span class="sxs-lookup"><span data-stu-id="32e7d-150">**ASP.NET Core Module Log:** Log file not created</span></span>

<span data-ttu-id="32e7d-151">Rozwiązywanie problemów:</span><span class="sxs-lookup"><span data-stu-id="32e7d-151">Troubleshooting:</span></span>

* <span data-ttu-id="32e7d-152">Potwierdź, że poprawne identyfikatora URI punktu końcowego dla aplikacji jest używany.</span><span class="sxs-lookup"><span data-stu-id="32e7d-152">Confirm the correct URI endpoint for the app is being used.</span></span> <span data-ttu-id="32e7d-153">Sprawdź powiązania.</span><span class="sxs-lookup"><span data-stu-id="32e7d-153">Check the bindings.</span></span>

* <span data-ttu-id="32e7d-154">Upewnij się, że witryna sieci Web usług IIS nie jest w *zatrzymane* stanu.</span><span class="sxs-lookup"><span data-stu-id="32e7d-154">Confirm that the IIS website isn't in the *Stopped* state.</span></span>

## <a name="corewebengine-or-w3svc-server-features-disabled"></a><span data-ttu-id="32e7d-155">Serwer CoreWebEngine lub W3SVC funkcje wyłączone</span><span class="sxs-lookup"><span data-stu-id="32e7d-155">CoreWebEngine or W3SVC server features disabled</span></span>

* <span data-ttu-id="32e7d-156">**Wyjątek systemu operacyjnego:** funkcje usług IIS 7.0 CoreWebEngine i W3SVC musi być zainstalowany, aby użyć modułu programu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="32e7d-156">**OS Exception:** The IIS 7.0 CoreWebEngine and W3SVC features must be installed to use the ASP.NET Core Module.</span></span>

<span data-ttu-id="32e7d-157">Rozwiązywanie problemów:</span><span class="sxs-lookup"><span data-stu-id="32e7d-157">Troubleshooting:</span></span>

* <span data-ttu-id="32e7d-158">Upewnij się, że odpowiednie role i funkcje są włączone.</span><span class="sxs-lookup"><span data-stu-id="32e7d-158">Confirm that the proper role and features are enabled.</span></span> <span data-ttu-id="32e7d-159">Zobacz [konfiguracji usług IIS](xref:host-and-deploy/iis/index#iis-configuration).</span><span class="sxs-lookup"><span data-stu-id="32e7d-159">See [IIS Configuration](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

## <a name="incorrect-website-physical-path-or-app-missing"></a><span data-ttu-id="32e7d-160">Ścieżka fizyczna witryny sieci Web nieprawidłowe lub brakujące aplikacji</span><span class="sxs-lookup"><span data-stu-id="32e7d-160">Incorrect website physical path or app missing</span></span>

* <span data-ttu-id="32e7d-161">**Przeglądarki:** 403 Zabroniony — odmowa dostępu **--lub--** zabroniony 403.14 — serwer sieci Web jest skonfigurowana do nie listy zawartości tego katalogu.</span><span class="sxs-lookup"><span data-stu-id="32e7d-161">**Browser:** 403 Forbidden - Access is denied **--OR--** 403.14 Forbidden - The Web server is configured to not list the contents of this directory.</span></span>

* <span data-ttu-id="32e7d-162">**Dziennik aplikacji:** wpisu</span><span class="sxs-lookup"><span data-stu-id="32e7d-162">**Application Log:** No entry</span></span>

* <span data-ttu-id="32e7d-163">**Dziennik modułu platformy ASP.NET Core:** nie utworzono pliku dziennika</span><span class="sxs-lookup"><span data-stu-id="32e7d-163">**ASP.NET Core Module Log:** Log file not created</span></span>

<span data-ttu-id="32e7d-164">Rozwiązywanie problemów:</span><span class="sxs-lookup"><span data-stu-id="32e7d-164">Troubleshooting:</span></span>

* <span data-ttu-id="32e7d-165">Witrynie sieci Web usług IIS **podstawowych ustawień** i folder aplikacji fizycznych.</span><span class="sxs-lookup"><span data-stu-id="32e7d-165">Check the IIS website **Basic Settings** and the physical app folder.</span></span> <span data-ttu-id="32e7d-166">Upewnij się, że aplikacja znajduje się w folderze w witrynie sieci Web usług IIS **ścieżka fizyczna**.</span><span class="sxs-lookup"><span data-stu-id="32e7d-166">Confirm that the app is in the folder at the IIS website **Physical path**.</span></span>

## <a name="incorrect-role-module-not-installed-or-incorrect-permissions"></a><span data-ttu-id="32e7d-167">Niepoprawne roli, nie jest zainstalowany moduł lub nieprawidłowe uprawnienia</span><span class="sxs-lookup"><span data-stu-id="32e7d-167">Incorrect role, module not installed, or incorrect permissions</span></span>

* <span data-ttu-id="32e7d-168">**Przeglądarki:** 500.19 wewnętrzny błąd serwera — nie można uzyskać dostępu do żądanej strony, ponieważ jej odpowiednie dane konfiguracyjne dla strony jest nieprawidłowy.</span><span class="sxs-lookup"><span data-stu-id="32e7d-168">**Browser:** 500.19 Internal Server Error - The requested page cannot be accessed because the related configuration data for the page is invalid.</span></span>

* <span data-ttu-id="32e7d-169">**Dziennik aplikacji:** wpisu</span><span class="sxs-lookup"><span data-stu-id="32e7d-169">**Application Log:** No entry</span></span>

* <span data-ttu-id="32e7d-170">**Dziennik modułu platformy ASP.NET Core:** nie utworzono pliku dziennika</span><span class="sxs-lookup"><span data-stu-id="32e7d-170">**ASP.NET Core Module Log:** Log file not created</span></span>

<span data-ttu-id="32e7d-171">Rozwiązywanie problemów:</span><span class="sxs-lookup"><span data-stu-id="32e7d-171">Troubleshooting:</span></span>

* <span data-ttu-id="32e7d-172">Upewnij się, że właściwej roli jest włączona.</span><span class="sxs-lookup"><span data-stu-id="32e7d-172">Confirm that the proper role is enabled.</span></span> <span data-ttu-id="32e7d-173">Zobacz [konfiguracji usług IIS](xref:host-and-deploy/iis/index#iis-configuration).</span><span class="sxs-lookup"><span data-stu-id="32e7d-173">See [IIS Configuration](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

* <span data-ttu-id="32e7d-174">Sprawdź **programy &amp; funkcje** i upewnij się, że **modułu programu Microsoft ASP.NET Core** został zainstalowany.</span><span class="sxs-lookup"><span data-stu-id="32e7d-174">Check **Programs &amp; Features** and confirm that the **Microsoft ASP.NET Core Module** has been installed.</span></span> <span data-ttu-id="32e7d-175">Jeśli **modułu programu Microsoft ASP.NET Core** nie występuje na liście zainstalowanych programów, instalowania modułu.</span><span class="sxs-lookup"><span data-stu-id="32e7d-175">If the **Microsoft ASP.NET Core Module** isn't present in the list of installed programs, install the module.</span></span> <span data-ttu-id="32e7d-176">Zobacz [instalacji pakietu .NET Core systemu Windows serwer obsługujący](xref:host-and-deploy/iis/index#install-the-net-core-windows-server-hosting-bundle).</span><span class="sxs-lookup"><span data-stu-id="32e7d-176">See [Install the .NET Core Windows Server Hosting bundle](xref:host-and-deploy/iis/index#install-the-net-core-windows-server-hosting-bundle).</span></span>

* <span data-ttu-id="32e7d-177">Upewnij się, że **puli aplikacji** > **Model procesu** > **tożsamości** ustawiono **puli** lub tożsamość niestandardowa ma odpowiednie uprawnienia dostępu do folderu wdrożenia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="32e7d-177">Make sure that the **Application Pool** > **Process Model** > **Identity** is set to **ApplicationPoolIdentity** or the custom identity has the correct permissions to access the app's deployment folder.</span></span>

## <a name="incorrect-processpath-missing-path-variable-hosting-bundle-not-installed-systemiis-not-restarted-vc-redistributable-not-installed-or-dotnetexe-access-violation"></a><span data-ttu-id="32e7d-178">Niepoprawne processPath, Brak zmiennej PATH, pakiet hostingu nie jest zainstalowany, system/IIS ponowne uruchomienie nie, VC ++ Redistributable nie jest zainstalowany lub dotnet.exe naruszenie zasad dostępu</span><span class="sxs-lookup"><span data-stu-id="32e7d-178">Incorrect processPath, missing PATH variable, hosting bundle not installed, system/IIS not restarted, VC++ Redistributable not installed, or dotnet.exe access violation</span></span>

* <span data-ttu-id="32e7d-179">**Przeglądarki:** błąd HTTP 502.5 — błąd procesu</span><span class="sxs-lookup"><span data-stu-id="32e7d-179">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="32e7d-180">**Dziennik aplikacji:** aplikacji "MACHINE/WEBROOT/APPHOST / {zestawu}" z fizyczny katalog główny "C:\\{PATH}\' nie można uruchomić procesu z wiersza polecenia" ".\{ .exe zestawu}"", kod błędu = "0x80070002: 0.</span><span class="sxs-lookup"><span data-stu-id="32e7d-180">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\\{PATH}\' failed to start process with commandline '".\{assembly}.exe" ', ErrorCode = '0x80070002 : 0.</span></span>

* <span data-ttu-id="32e7d-181">**Dziennik modułu platformy ASP.NET Core:** utworzony plik dziennika, ale pusty</span><span class="sxs-lookup"><span data-stu-id="32e7d-181">**ASP.NET Core Module Log:** Log file created but empty</span></span>

<span data-ttu-id="32e7d-182">Rozwiązywanie problemów:</span><span class="sxs-lookup"><span data-stu-id="32e7d-182">Troubleshooting:</span></span>

* <span data-ttu-id="32e7d-183">Upewnij się, że aplikacja działa lokalnie na Kestrel.</span><span class="sxs-lookup"><span data-stu-id="32e7d-183">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="32e7d-184">Błąd procesu może być wynikiem problemu w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="32e7d-184">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="32e7d-185">Aby uzyskać więcej informacji, zobacz [Rozwiązywanie problemów](xref:host-and-deploy/iis/troubleshoot).</span><span class="sxs-lookup"><span data-stu-id="32e7d-185">For more information, see [Troubleshooting](xref:host-and-deploy/iis/troubleshoot).</span></span>

* <span data-ttu-id="32e7d-186">Sprawdź *processPath* atrybutu `<aspNetCore>` element *web.config* aby upewnić się, że jest *dotnet* wdrożenia framework zależne (stacje) lub *. \{zestawu} .exe* niezależne wdrożenia (SCD).</span><span class="sxs-lookup"><span data-stu-id="32e7d-186">Check the *processPath* attribute on the `<aspNetCore>` element in *web.config* to confirm that it's *dotnet* for a framework-dependent deployment (FDD) or *.\{assembly}.exe* for a self-contained deployment (SCD).</span></span>

* <span data-ttu-id="32e7d-187">Dla Dyskietki *dotnet.exe* może nie być dostępny za pośrednictwem ustawienia ścieżki.</span><span class="sxs-lookup"><span data-stu-id="32e7d-187">For an FDD, *dotnet.exe* might not be accessible via the PATH settings.</span></span> <span data-ttu-id="32e7d-188">Upewnij się, że * C:\Program Files\dotnet\* istnieje w ŚCIEŻCE systemowej ustawienia.</span><span class="sxs-lookup"><span data-stu-id="32e7d-188">Confirm that *C:\Program Files\dotnet\* exists in the System PATH settings.</span></span>

* <span data-ttu-id="32e7d-189">Dla Dyskietki *dotnet.exe* mogą nie być dostępne dla tożsamości puli aplikacji.</span><span class="sxs-lookup"><span data-stu-id="32e7d-189">For an FDD, *dotnet.exe* might not be accessible for the user identity of the Application Pool.</span></span> <span data-ttu-id="32e7d-190">Upewnij się, że tożsamość puli aplikacji ma dostęp do *C:\Program Files\dotnet* katalogu.</span><span class="sxs-lookup"><span data-stu-id="32e7d-190">Confirm that the AppPool user identity has access to the *C:\Program Files\dotnet* directory.</span></span> <span data-ttu-id="32e7d-191">Upewnij się, że nie istnieją żadne reguły odmowy skonfigurowane dla tożsamości użytkownika puli aplikacji na *C:\Program Files\dotnet* i katalogów aplikacji.</span><span class="sxs-lookup"><span data-stu-id="32e7d-191">Confirm that there are no deny rules configured for the AppPool user identity on the *C:\Program Files\dotnet* and app directories.</span></span>

* <span data-ttu-id="32e7d-192">STACJE zostały wdrożone, oraz .NET Core zainstalowany bez ponownego uruchomienia usług IIS.</span><span class="sxs-lookup"><span data-stu-id="32e7d-192">An FDD may have been deployed and .NET Core installed without restarting IIS.</span></span> <span data-ttu-id="32e7d-193">Uruchom ponownie serwer lub ponownego uruchomienia usług IIS, wykonując **net stop została /y** następuje **net start w3svc** z wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="32e7d-193">Either restart the server or restart IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

* <span data-ttu-id="32e7d-194">STACJE mogą wdrożyć bez instalowania środowiska uruchomieniowego .NET Core przez system operacyjny.</span><span class="sxs-lookup"><span data-stu-id="32e7d-194">An FDD may have been deployed without installing the .NET Core runtime on the hosting system.</span></span> <span data-ttu-id="32e7d-195">Jeśli nie zainstalowano środowiska wykonawczego platformy .NET Core, uruchom **.NET Core systemu Windows serwer obsługujący Instalatora pakietu** w systemie.</span><span class="sxs-lookup"><span data-stu-id="32e7d-195">If the .NET Core runtime hasn't been installed, run the **.NET Core Windows Server Hosting bundle installer** on the system.</span></span> <span data-ttu-id="32e7d-196">Zobacz [instalacji pakietu .NET Core systemu Windows serwer obsługujący](xref:host-and-deploy/iis/index#install-the-net-core-windows-server-hosting-bundle).</span><span class="sxs-lookup"><span data-stu-id="32e7d-196">See [Install the .NET Core Windows Server Hosting bundle](xref:host-and-deploy/iis/index#install-the-net-core-windows-server-hosting-bundle).</span></span> <span data-ttu-id="32e7d-197">Jeśli próba instalowanie środowiska uruchomieniowego .NET Core w systemie bez połączenia z Internetem, uzyskać środowiska uruchomieniowego z [pobiera .NET](https://www.microsoft.com/net/download/core) i uruchom Instalatora pakietu hostingu, aby zainstalować moduł platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="32e7d-197">If attempting to install the .NET Core runtime on a system without an Internet connection, obtain the runtime from [.NET Downloads](https://www.microsoft.com/net/download/core) and run the hosting bundle installer to install the ASP.NET Core Module.</span></span> <span data-ttu-id="32e7d-198">Ukończenie instalacji przez ponowne uruchomienie systemu lub ponowne uruchomienie usług IIS, wykonując **net stop została /y** następuje **net start w3svc** z wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="32e7d-198">Complete the installation by restarting the system or restarting IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

* <span data-ttu-id="32e7d-199">STACJE zostały wdrożone i *Microsoft Visual C++ 2015 Redistributable (x64)* nie jest zainstalowany w systemie.</span><span class="sxs-lookup"><span data-stu-id="32e7d-199">An FDD may have been deployed and the *Microsoft Visual C++ 2015 Redistributable (x64)* isn't installed on the system.</span></span> <span data-ttu-id="32e7d-200">Uzyskać Instalatora z [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840).</span><span class="sxs-lookup"><span data-stu-id="32e7d-200">Obtain an installer from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840).</span></span>

## <a name="incorrect-arguments-of-aspnetcore-element"></a><span data-ttu-id="32e7d-201">Nieprawidłowe argumenty \<aspNetCore\> — element</span><span class="sxs-lookup"><span data-stu-id="32e7d-201">Incorrect arguments of \<aspNetCore\> element</span></span>

* <span data-ttu-id="32e7d-202">**Przeglądarki:** błąd HTTP 502.5 — błąd procesu</span><span class="sxs-lookup"><span data-stu-id="32e7d-202">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="32e7d-203">**Dziennik aplikacji:** aplikacji "MACHINE/WEBROOT/APPHOST / {zestawu}" z fizyczny katalog główny "C:\\{PATH}\' nie można uruchomić procesu z wiersza polecenia" "dotnet".\{ DLL zestawu} ", kod błędu =" 0x80004005: 80008081.</span><span class="sxs-lookup"><span data-stu-id="32e7d-203">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\\{PATH}\' failed to start process with commandline '"dotnet" .\{assembly}.dll', ErrorCode = '0x80004005 : 80008081.</span></span>

* <span data-ttu-id="32e7d-204">**Program ASP.NET Core modułu dziennika:** wykonywanie aplikacji nie istnieje: "ŚCIEŻKA\{zestawu} .dll"</span><span class="sxs-lookup"><span data-stu-id="32e7d-204">**ASP.NET Core Module Log:** The application to execute does not exist: 'PATH\{assembly}.dll'</span></span>

<span data-ttu-id="32e7d-205">Rozwiązywanie problemów:</span><span class="sxs-lookup"><span data-stu-id="32e7d-205">Troubleshooting:</span></span>

* <span data-ttu-id="32e7d-206">Upewnij się, że aplikacja działa lokalnie na Kestrel.</span><span class="sxs-lookup"><span data-stu-id="32e7d-206">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="32e7d-207">Błąd procesu może być wynikiem problemu w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="32e7d-207">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="32e7d-208">Aby uzyskać więcej informacji, zobacz [Rozwiązywanie problemów](xref:host-and-deploy/iis/troubleshoot).</span><span class="sxs-lookup"><span data-stu-id="32e7d-208">For more information, see [Troubleshooting](xref:host-and-deploy/iis/troubleshoot).</span></span>

* <span data-ttu-id="32e7d-209">Sprawdź *argumenty* atrybutu `<aspNetCore>` element *web.config* aby upewnić się, że jest ona () *.\{ DLL zestawu}* we wdrożeniu (stacje); zależne od framework lub brak pustego ciągu (b) (*argumenty = ""*), lub listę argumentów aplikacji (*argumenty = "arg1, arg2..."*) Samodzielne wdrożenia (SCD).</span><span class="sxs-lookup"><span data-stu-id="32e7d-209">Examine the *arguments* attribute on the `<aspNetCore>` element in *web.config* to confirm that it's either (a) *.\{assembly}.dll* for a framework-dependent deployment (FDD); or (b) not present, an empty string (*arguments=""*), or a list of the app's arguments (*arguments="arg1, arg2, ..."*) for a self-contained deployment (SCD).</span></span>

## <a name="missing-net-framework-version"></a><span data-ttu-id="32e7d-210">Brak wersji .NET Framework</span><span class="sxs-lookup"><span data-stu-id="32e7d-210">Missing .NET Framework version</span></span>

* <span data-ttu-id="32e7d-211">**Przeglądarki:** 502.3 Zła brama — wystąpił błąd połączenia podczas próby trasy żądania.</span><span class="sxs-lookup"><span data-stu-id="32e7d-211">**Browser:** 502.3 Bad Gateway - There was a connection error while trying to route the request.</span></span>

* <span data-ttu-id="32e7d-212">**Dziennik aplikacji:** ErrorCode = aplikacji "MACHINE/WEBROOT/APPHOST / {zestawu}" z fizyczny katalog główny "C:\\{PATH}\' nie można uruchomić procesu z wiersza polecenia" "dotnet".\{ DLL zestawu} ", kod błędu =" 0x80004005: 80008081.</span><span class="sxs-lookup"><span data-stu-id="32e7d-212">**Application Log:** ErrorCode = Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\\{PATH}\' failed to start process with commandline '"dotnet" .\{assembly}.dll', ErrorCode = '0x80004005 : 80008081.</span></span>

* <span data-ttu-id="32e7d-213">**Dziennik modułu platformy ASP.NET Core:** Brak metody, pliku lub zestawu wyjątku.</span><span class="sxs-lookup"><span data-stu-id="32e7d-213">**ASP.NET Core Module Log:** Missing method, file, or assembly exception.</span></span> <span data-ttu-id="32e7d-214">Metoda, pliku lub zestawu określonego w wyjątek jest metoda, pliku lub zestawu .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="32e7d-214">The method, file, or assembly specified in the exception is a .NET Framework method, file, or assembly.</span></span>

<span data-ttu-id="32e7d-215">Rozwiązywanie problemów:</span><span class="sxs-lookup"><span data-stu-id="32e7d-215">Troubleshooting:</span></span>

* <span data-ttu-id="32e7d-216">Zainstaluj wersję .NET Framework w systemie.</span><span class="sxs-lookup"><span data-stu-id="32e7d-216">Install the .NET Framework version missing from the system.</span></span>

* <span data-ttu-id="32e7d-217">Zależne od framework wdrożenia (stacje) Potwierdź, że poprawne środowiska uruchomieniowego zainstalowane w systemie.</span><span class="sxs-lookup"><span data-stu-id="32e7d-217">For a framework-dependent deployment (FDD), confirm that the correct runtime installed on the system.</span></span> <span data-ttu-id="32e7d-218">Jeśli projekt jest uaktualniana 1.1 2.0 wdrożony system obsługujący i wyniki tego wyjątku, upewnij się, że 2.0 framework jest przez system operacyjny.</span><span class="sxs-lookup"><span data-stu-id="32e7d-218">If the project is upgraded from 1.1 to 2.0, deployed to the hosting system, and this exception results, ensure that the 2.0 framework is on the hosting system.</span></span>

## <a name="stopped-application-pool"></a><span data-ttu-id="32e7d-219">Zatrzymanej puli aplikacji</span><span class="sxs-lookup"><span data-stu-id="32e7d-219">Stopped Application Pool</span></span>

* <span data-ttu-id="32e7d-220">**Przeglądarki:** 503 Usługa jest niedostępna</span><span class="sxs-lookup"><span data-stu-id="32e7d-220">**Browser:** 503 Service Unavailable</span></span>

* <span data-ttu-id="32e7d-221">**Dziennik aplikacji:** wpisu</span><span class="sxs-lookup"><span data-stu-id="32e7d-221">**Application Log:** No entry</span></span>

* <span data-ttu-id="32e7d-222">**Dziennik modułu platformy ASP.NET Core:** nie utworzono pliku dziennika</span><span class="sxs-lookup"><span data-stu-id="32e7d-222">**ASP.NET Core Module Log:** Log file not created</span></span>

<span data-ttu-id="32e7d-223">Rozwiązywanie problemów</span><span class="sxs-lookup"><span data-stu-id="32e7d-223">Troubleshooting</span></span>

* <span data-ttu-id="32e7d-224">Upewnij się, że pula aplikacji nie znajduje się w *zatrzymane* stanu.</span><span class="sxs-lookup"><span data-stu-id="32e7d-224">Confirm that the Application Pool isn't in the *Stopped* state.</span></span>

## <a name="iis-integration-middleware-not-implemented"></a><span data-ttu-id="32e7d-225">Oprogramowanie pośredniczące integracji usług IIS nie jest zaimplementowana</span><span class="sxs-lookup"><span data-stu-id="32e7d-225">IIS Integration middleware not implemented</span></span>

* <span data-ttu-id="32e7d-226">**Przeglądarki:** błąd HTTP 502.5 — błąd procesu</span><span class="sxs-lookup"><span data-stu-id="32e7d-226">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="32e7d-227">**Dziennik aplikacji:** aplikacji "MACHINE/WEBROOT/APPHOST / {zestawu}" z fizyczny katalog główny "C:\\{PATH}\' utworzyć procesu z wiersza polecenia" "C:\\{PATH}\{zestawu}. { exe | dll} "" ale awaria lub odpowiedzi nie albo Nasłuchuj na danego portu "{PORT}", kod błędu = "0x800705b4"</span><span class="sxs-lookup"><span data-stu-id="32e7d-227">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\\{PATH}\' created process with commandline '"C:\\{PATH}\{assembly}.{exe|dll}" ' but either crashed or did not reponse or did not listen on the given port '{PORT}', ErrorCode = '0x800705b4'</span></span>

* <span data-ttu-id="32e7d-228">**Dziennik modułu platformy ASP.NET Core:** plik dziennika utworzony i przedstawia normalnego działania.</span><span class="sxs-lookup"><span data-stu-id="32e7d-228">**ASP.NET Core Module Log:** Log file created and shows normal operation.</span></span>

<span data-ttu-id="32e7d-229">Rozwiązywanie problemów</span><span class="sxs-lookup"><span data-stu-id="32e7d-229">Troubleshooting</span></span>

* <span data-ttu-id="32e7d-230">Upewnij się, że aplikacja działa lokalnie na Kestrel.</span><span class="sxs-lookup"><span data-stu-id="32e7d-230">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="32e7d-231">Błąd procesu może być wynikiem problemu w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="32e7d-231">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="32e7d-232">Aby uzyskać więcej informacji, zobacz [Rozwiązywanie problemów](xref:host-and-deploy/iis/troubleshoot).</span><span class="sxs-lookup"><span data-stu-id="32e7d-232">For more information, see [Troubleshooting](xref:host-and-deploy/iis/troubleshoot).</span></span>

* <span data-ttu-id="32e7d-233">Upewnij się, że to:</span><span class="sxs-lookup"><span data-stu-id="32e7d-233">Confirm that either:</span></span>
  * <span data-ttu-id="32e7d-234">Oprogramowanie pośredniczące integracji usług IIS jest referencedby wywoływania `UseIISIntegration` metody w aplikacji `WebHostBuilder` (platformy ASP.NET Core 1.x)</span><span class="sxs-lookup"><span data-stu-id="32e7d-234">The IIS Integration middleware is referencedby calling the `UseIISIntegration` method on the app's `WebHostBuilder` (ASP.NET Core 1.x)</span></span>
  * <span data-ttu-id="32e7d-235">Używa aplikacji `CreateDefaultBuilder` — metoda (platformy ASP.NET Core 2.x).</span><span class="sxs-lookup"><span data-stu-id="32e7d-235">The apps uses the `CreateDefaultBuilder` method (ASP.NET Core 2.x).</span></span>
  
  <span data-ttu-id="32e7d-236">Zobacz [Hosting w ASP.NET Core](xref:fundamentals/hosting) szczegółowe informacje.</span><span class="sxs-lookup"><span data-stu-id="32e7d-236">See [Hosting in ASP.NET Core](xref:fundamentals/hosting) for details.</span></span>

## <a name="sub-application-includes-a-handlers-section"></a><span data-ttu-id="32e7d-237">Podrzędne aplikacja zawiera \<obsługi\> sekcji</span><span class="sxs-lookup"><span data-stu-id="32e7d-237">Sub-application includes a \<handlers\> section</span></span>

* <span data-ttu-id="32e7d-238">**Przeglądarki:** błąd HTTP 500.19 — wewnętrzny błąd serwera</span><span class="sxs-lookup"><span data-stu-id="32e7d-238">**Browser:** HTTP Error 500.19 - Internal Server Error</span></span>

* <span data-ttu-id="32e7d-239">**Dziennik aplikacji:** wpisu</span><span class="sxs-lookup"><span data-stu-id="32e7d-239">**Application Log:** No entry</span></span>

* <span data-ttu-id="32e7d-240">**Dziennik modułu platformy ASP.NET Core:** plik dziennika utworzony i pokazuje normalnego działania głównego aplikacji.</span><span class="sxs-lookup"><span data-stu-id="32e7d-240">**ASP.NET Core Module Log:** Log file created and shows normal operation for the root app.</span></span> <span data-ttu-id="32e7d-241">Plik dziennika nie został utworzony dla aplikacji sub.</span><span class="sxs-lookup"><span data-stu-id="32e7d-241">Log file not created for the sub-app.</span></span>

<span data-ttu-id="32e7d-242">Rozwiązywanie problemów</span><span class="sxs-lookup"><span data-stu-id="32e7d-242">Troubleshooting</span></span>

* <span data-ttu-id="32e7d-243">Upewnij się, że aplikacja sub *web.config* nie zawiera pliku `<handlers>` sekcji.</span><span class="sxs-lookup"><span data-stu-id="32e7d-243">Confirm that the sub-app's *web.config* file doesn't include a `<handlers>` section.</span></span>

## <a name="application-configuration-general-issue"></a><span data-ttu-id="32e7d-244">Ogólne problem z konfiguracją aplikacji</span><span class="sxs-lookup"><span data-stu-id="32e7d-244">Application configuration general issue</span></span>

* <span data-ttu-id="32e7d-245">**Przeglądarki:** błąd HTTP 502.5 — błąd procesu</span><span class="sxs-lookup"><span data-stu-id="32e7d-245">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="32e7d-246">**Dziennik aplikacji:** aplikacji "MACHINE/WEBROOT/APPHOST / {zestawu}" z fizyczny katalog główny "C:\\{PATH}\' utworzyć procesu z wiersza polecenia" "C:\\{PATH}\{zestawu}. { exe | dll} "" ale awaria lub odpowiedzi nie albo Nasłuchuj na danego portu "{PORT}", kod błędu = "0x800705b4"</span><span class="sxs-lookup"><span data-stu-id="32e7d-246">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\\{PATH}\' created process with commandline '"C:\\{PATH}\{assembly}.{exe|dll}" ' but either crashed or did not reponse or did not listen on the given port '{PORT}', ErrorCode = '0x800705b4'</span></span>

* <span data-ttu-id="32e7d-247">**Dziennik modułu platformy ASP.NET Core:** utworzony plik dziennika, ale pusty</span><span class="sxs-lookup"><span data-stu-id="32e7d-247">**ASP.NET Core Module Log:** Log file created but empty</span></span>

<span data-ttu-id="32e7d-248">Rozwiązywanie problemów</span><span class="sxs-lookup"><span data-stu-id="32e7d-248">Troubleshooting</span></span>

* <span data-ttu-id="32e7d-249">Ten wyjątek ogólny wskazuje, że proces nie mógł uruchomić najprawdopodobniej ze względu na problem z konfiguracją aplikacji.</span><span class="sxs-lookup"><span data-stu-id="32e7d-249">This general exception indicates that the process failed to start, most likely due to an app configuration issue.</span></span> <span data-ttu-id="32e7d-250">Odwołanie do [struktury katalogów](xref:host-and-deploy/directory-structure), upewnij się, że aplikacja wdrożona pliki i foldery są odpowiednie i że pliki konfiguracji aplikacji są obecne i zawiera poprawne ustawienia dla aplikacji i środowiska.</span><span class="sxs-lookup"><span data-stu-id="32e7d-250">Referring to [Directory Structure](xref:host-and-deploy/directory-structure), confirm that the app's deployed files and folders are appropriate and that the app's configuration files are present and contain the correct settings for the app and environment.</span></span> <span data-ttu-id="32e7d-251">Aby uzyskać więcej informacji, zobacz [Rozwiązywanie problemów](xref:host-and-deploy/iis/troubleshoot).</span><span class="sxs-lookup"><span data-stu-id="32e7d-251">For more information, see [Troubleshooting](xref:host-and-deploy/iis/troubleshoot).</span></span>
