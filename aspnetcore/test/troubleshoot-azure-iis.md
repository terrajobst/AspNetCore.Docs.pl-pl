---
title: Rozwiązywanie problemów ASP.NET Core na Azure App Service i usługach IIS
author: guardrex
description: Dowiedz się, jak zdiagnozować problemy z wdrożeniami Azure App Service i Internet Information Services (IIS) ASP.NET Core aplikacji.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/18/2020
uid: test/troubleshoot-azure-iis
ms.openlocfilehash: 071dba9e936351e201b7582b3d0667cd6fac54bb
ms.sourcegitcommit: f259889044d1fc0f0c7e3882df0008157ced4915
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/21/2020
ms.locfileid: "76294618"
---
# <a name="troubleshoot-aspnet-core-on-azure-app-service-and-iis"></a><span data-ttu-id="782e6-103">Rozwiązywanie problemów ASP.NET Core na Azure App Service i usługach IIS</span><span class="sxs-lookup"><span data-stu-id="782e6-103">Troubleshoot ASP.NET Core on Azure App Service and IIS</span></span>

<span data-ttu-id="782e6-104">Autorzy [Luke Latham](https://github.com/guardrex) i [Justin Kotalik](https://github.com/jkotalik)</span><span class="sxs-lookup"><span data-stu-id="782e6-104">By [Luke Latham](https://github.com/guardrex) and [Justin Kotalik](https://github.com/jkotalik)</span></span>

<span data-ttu-id="782e6-105">Ten artykuł zawiera informacje dotyczące typowych błędów uruchamiania aplikacji oraz instrukcje dotyczące sposobu diagnozowania błędów podczas wdrażania aplikacji w usłudze Azure App Service lub IIS:</span><span class="sxs-lookup"><span data-stu-id="782e6-105">This article provides information on common app startup errors and instructions on how to diagnose errors when an app is deployed to Azure App Service or IIS:</span></span>

[<span data-ttu-id="782e6-106">Błędy uruchamiania aplikacji</span><span class="sxs-lookup"><span data-stu-id="782e6-106">App startup errors</span></span>](#app-startup-errors)  
<span data-ttu-id="782e6-107">Objaśnia typowe scenariusze uruchamiania kodu stanu HTTP.</span><span class="sxs-lookup"><span data-stu-id="782e6-107">Explains common startup HTTP status code scenarios.</span></span>

[<span data-ttu-id="782e6-108">Rozwiązywanie problemów dotyczących Azure App Service</span><span class="sxs-lookup"><span data-stu-id="782e6-108">Troubleshoot on Azure App Service</span></span>](#troubleshoot-on-azure-app-service)  
<span data-ttu-id="782e6-109">Zawiera porady dotyczące rozwiązywania problemów z aplikacjami wdrożonymi w celu Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="782e6-109">Provides troubleshooting advice for apps deployed to Azure App Service.</span></span>

[<span data-ttu-id="782e6-110">Rozwiązywanie problemów w usługach IIS</span><span class="sxs-lookup"><span data-stu-id="782e6-110">Troubleshoot on IIS</span></span>](#troubleshoot-on-iis)  
<span data-ttu-id="782e6-111">Zawiera porady dotyczące rozwiązywania problemów z aplikacjami wdrożonymi w usługach IIS lub lokalnie uruchomionymi IIS Express.</span><span class="sxs-lookup"><span data-stu-id="782e6-111">Provides troubleshooting advice for apps deployed to IIS or running on IIS Express locally.</span></span> <span data-ttu-id="782e6-112">Wskazówki dotyczą zarówno wdrożeń systemu Windows Server, jak i pulpitu systemu Windows.</span><span class="sxs-lookup"><span data-stu-id="782e6-112">The guidance applies to both Windows Server and Windows desktop deployments.</span></span>

[<span data-ttu-id="782e6-113">Wyczyść pamięć podręczną pakietów</span><span class="sxs-lookup"><span data-stu-id="782e6-113">Clear package caches</span></span>](#clear-package-caches)  
<span data-ttu-id="782e6-114">Wyjaśnia, co należy zrobić, gdy niespójne pakiety przerywają działanie aplikacji podczas przeprowadzania uaktualnień głównych lub zmiany wersji pakietu.</span><span class="sxs-lookup"><span data-stu-id="782e6-114">Explains what to do when incoherent packages break an app when performing major upgrades or changing package versions.</span></span>

[<span data-ttu-id="782e6-115">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="782e6-115">Additional resources</span></span>](#additional-resources)  
<span data-ttu-id="782e6-116">Wyświetla listę dodatkowych tematów dotyczących rozwiązywania problemów.</span><span class="sxs-lookup"><span data-stu-id="782e6-116">Lists additional troubleshooting topics.</span></span>

## <a name="app-startup-errors"></a><span data-ttu-id="782e6-117">Błędy uruchamiania aplikacji</span><span class="sxs-lookup"><span data-stu-id="782e6-117">App startup errors</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="782e6-118">W programie Visual Studio ma domyślnie wartość projektu ASP.NET Core [usług IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hostingu podczas debugowania.</span><span class="sxs-lookup"><span data-stu-id="782e6-118">In Visual Studio, an ASP.NET Core project defaults to [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hosting during debugging.</span></span> <span data-ttu-id="782e6-119">*502,5 — błąd procesu* lub *błąd uruchomienia 500,30* , który występuje, gdy debugowanie lokalne można zdiagnozować przy użyciu porady w tym temacie.</span><span class="sxs-lookup"><span data-stu-id="782e6-119">A *502.5 - Process Failure* or a *500.30 - Start Failure* that occurs when debugging locally can be diagnosed using the advice in this topic.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="782e6-120">W programie Visual Studio ma domyślnie wartość projektu ASP.NET Core [usług IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hostingu podczas debugowania.</span><span class="sxs-lookup"><span data-stu-id="782e6-120">In Visual Studio, an ASP.NET Core project defaults to [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hosting during debugging.</span></span> <span data-ttu-id="782e6-121">*Błąd procesu 502,5* , który występuje, gdy debugowanie lokalne można zdiagnozować przy użyciu porady w tym temacie.</span><span class="sxs-lookup"><span data-stu-id="782e6-121">A *502.5 Process Failure* that occurs when debugging locally can be diagnosed using the advice in this topic.</span></span>

::: moniker-end

### <a name="40314-forbidden"></a><span data-ttu-id="782e6-122">403,14 zabronione</span><span class="sxs-lookup"><span data-stu-id="782e6-122">403.14 Forbidden</span></span>

<span data-ttu-id="782e6-123">Nie można uruchomić aplikacji.</span><span class="sxs-lookup"><span data-stu-id="782e6-123">The app fails to start.</span></span> <span data-ttu-id="782e6-124">Rejestrowany jest następujący błąd:</span><span class="sxs-lookup"><span data-stu-id="782e6-124">The following error is logged:</span></span>

```
The Web server is configured to not list the contents of this directory.
```

<span data-ttu-id="782e6-125">Ten błąd jest zwykle spowodowany przez uszkodzone wdrożenie w systemie hostingu, który obejmuje następujące scenariusze:</span><span class="sxs-lookup"><span data-stu-id="782e6-125">The error is usually caused by a broken deployment on the hosting system, which includes any of the following scenarios:</span></span>

* <span data-ttu-id="782e6-126">Aplikacja jest wdrażana w niewłaściwym folderze w systemie hostingu.</span><span class="sxs-lookup"><span data-stu-id="782e6-126">The app is deployed to the wrong folder on the hosting system.</span></span>
* <span data-ttu-id="782e6-127">W procesie wdrażania nie powiodło się przeniesienie wszystkich plików i folderów aplikacji do folderu wdrożenia w systemie hostingu.</span><span class="sxs-lookup"><span data-stu-id="782e6-127">The deployment process failed to move all of the app's files and folders to the deployment folder on the hosting system.</span></span>
* <span data-ttu-id="782e6-128">Brak pliku *Web. config* w wdrożeniu lub zawartość pliku *Web. config* jest nieprawidłowo sformułowana.</span><span class="sxs-lookup"><span data-stu-id="782e6-128">The *web.config* file is missing from the deployment, or the *web.config* file contents are malformed.</span></span>

<span data-ttu-id="782e6-129">Wykonaj poniższe czynności:</span><span class="sxs-lookup"><span data-stu-id="782e6-129">Perform the following steps:</span></span>

1. <span data-ttu-id="782e6-130">Usuń wszystkie pliki i foldery z folderu wdrożenia w systemie hostingu.</span><span class="sxs-lookup"><span data-stu-id="782e6-130">Delete all of the files and folders from the deployment folder on the hosting system.</span></span>
1. <span data-ttu-id="782e6-131">Wdróż ponownie zawartość folderu *publikowania* aplikacji w systemie hostingu przy użyciu zwykłej metody wdrażania, takiej jak Visual Studio, PowerShell lub wdrażanie ręczne:</span><span class="sxs-lookup"><span data-stu-id="782e6-131">Redeploy the contents of the app's *publish* folder to the hosting system using your normal method of deployment, such as Visual Studio, PowerShell, or manual deployment:</span></span>
   * <span data-ttu-id="782e6-132">Upewnij się, że plik *Web. config* znajduje się we wdrożeniu i że jego zawartość jest poprawna.</span><span class="sxs-lookup"><span data-stu-id="782e6-132">Confirm that the *web.config* file is present in the deployment and that its contents are correct.</span></span>
   * <span data-ttu-id="782e6-133">Podczas hostowania w Azure App Service upewnij się, że aplikacja została wdrożona w folderze `D:\home\site\wwwroot`.</span><span class="sxs-lookup"><span data-stu-id="782e6-133">When hosting on Azure App Service, confirm that the app is deployed to the `D:\home\site\wwwroot` folder.</span></span>
   * <span data-ttu-id="782e6-134">Jeśli aplikacja jest hostowana przez usługi IIS, upewnij się, że aplikacja jest wdrożona w **ścieżce fizycznej** usług IIS pokazanej w **ustawieniach podstawowych**w **Menedżerze usług IIS**.</span><span class="sxs-lookup"><span data-stu-id="782e6-134">When the app is hosted by IIS, confirm that the app is deployed to the IIS **Physical path** shown in **IIS Manager**'s **Basic Settings**.</span></span>
1. <span data-ttu-id="782e6-135">Upewnij się, że wszystkie pliki i foldery aplikacji zostały wdrożone, porównując wdrożenie w systemie hostingu z zawartością folderu *publikowania* projektu.</span><span class="sxs-lookup"><span data-stu-id="782e6-135">Confirm that all of the app's files and folders are deployed by comparing the deployment on the hosting system to the contents of the project's *publish* folder.</span></span>

<span data-ttu-id="782e6-136">Aby uzyskać więcej informacji na temat układu opublikowanej aplikacji ASP.NET Core, zobacz <xref:host-and-deploy/directory-structure>.</span><span class="sxs-lookup"><span data-stu-id="782e6-136">For more information on the layout of a published ASP.NET Core app, see <xref:host-and-deploy/directory-structure>.</span></span> <span data-ttu-id="782e6-137">Aby uzyskać więcej informacji na temat pliku *Web. config* , zobacz <xref:host-and-deploy/aspnet-core-module#configuration-with-webconfig>.</span><span class="sxs-lookup"><span data-stu-id="782e6-137">For more information on the *web.config* file, see <xref:host-and-deploy/aspnet-core-module#configuration-with-webconfig>.</span></span>

### <a name="500-internal-server-error"></a><span data-ttu-id="782e6-138">500 Wewnętrzny błąd serwera</span><span class="sxs-lookup"><span data-stu-id="782e6-138">500 Internal Server Error</span></span>

<span data-ttu-id="782e6-139">Uruchamia aplikację, ale błąd uniemożliwia spełnienie żądania przez serwer.</span><span class="sxs-lookup"><span data-stu-id="782e6-139">The app starts, but an error prevents the server from fulfilling the request.</span></span>

<span data-ttu-id="782e6-140">Ten błąd występuje w kodzie aplikacji, podczas uruchamiania lub podczas tworzenia odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="782e6-140">This error occurs within the app's code during startup or while creating a response.</span></span> <span data-ttu-id="782e6-141">Odpowiedź może zawierać żadnej zawartości lub odpowiedzi może być wyświetlana jako *500 Wewnętrzny błąd serwera* w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="782e6-141">The response may contain no content, or the response may appear as a *500 Internal Server Error* in the browser.</span></span> <span data-ttu-id="782e6-142">W dzienniku zdarzeń aplikacji stwierdza, zwykle uruchomiona aplikacja.</span><span class="sxs-lookup"><span data-stu-id="782e6-142">The Application Event Log usually states that the app started normally.</span></span> <span data-ttu-id="782e6-143">Z perspektywy serwera, który jest poprawna.</span><span class="sxs-lookup"><span data-stu-id="782e6-143">From the server's perspective, that's correct.</span></span> <span data-ttu-id="782e6-144">Aplikacja została uruchomiona, ale nie może wygenerować prawidłowej odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="782e6-144">The app did start, but it can't generate a valid response.</span></span> <span data-ttu-id="782e6-145">Uruchom aplikację w wierszu polecenia na serwerze lub Włącz dziennik stdout modułu ASP.NET Core, aby rozwiązać problem.</span><span class="sxs-lookup"><span data-stu-id="782e6-145">Run the app at a command prompt on the server or enable the ASP.NET Core Module stdout log to troubleshoot the problem.</span></span>

::: moniker range="= aspnetcore-2.2"

### <a name="5000-in-process-handler-load-failure"></a><span data-ttu-id="782e6-146">500.0 w procesie programu obsługi błędu ładowania</span><span class="sxs-lookup"><span data-stu-id="782e6-146">500.0 In-Process Handler Load Failure</span></span>

<span data-ttu-id="782e6-147">Proces roboczy kończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="782e6-147">The worker process fails.</span></span> <span data-ttu-id="782e6-148">Nie zaczyna się aplikacja.</span><span class="sxs-lookup"><span data-stu-id="782e6-148">The app doesn't start.</span></span>

<span data-ttu-id="782e6-149">[Moduł ASP.NET Core](xref:host-and-deploy/aspnet-core-module) nie może znaleźć platformy .NET Core CLR i znaleźć procedury obsługi żądań w procesie (*aspnetcorev2_inprocess. dll*).</span><span class="sxs-lookup"><span data-stu-id="782e6-149">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) fails to find the .NET Core CLR and find the in-process request handler (*aspnetcorev2_inprocess.dll*).</span></span> <span data-ttu-id="782e6-150">Sprawdź, czy:</span><span class="sxs-lookup"><span data-stu-id="782e6-150">Check that:</span></span>

* <span data-ttu-id="782e6-151">Aplikacja jest przeznaczona na albo [Microsoft.AspNetCore.Server.IIS](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IIS) pakietu NuGet lub [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="782e6-151">The app targets either the [Microsoft.AspNetCore.Server.IIS](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IIS) NuGet package or the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>
* <span data-ttu-id="782e6-152">Wersja udostępnionej platformy ASP.NET Core jest zainstalowanie aplikacji jest przeznaczony dla na komputerze docelowym.</span><span class="sxs-lookup"><span data-stu-id="782e6-152">The version of the ASP.NET Core shared framework that the app targets is installed on the target machine.</span></span>

### <a name="5000-out-of-process-handler-load-failure"></a><span data-ttu-id="782e6-153">500.0 Błąd ładowania poza procesem programu obsługi</span><span class="sxs-lookup"><span data-stu-id="782e6-153">500.0 Out-Of-Process Handler Load Failure</span></span>

<span data-ttu-id="782e6-154">Proces roboczy kończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="782e6-154">The worker process fails.</span></span> <span data-ttu-id="782e6-155">Nie zaczyna się aplikacja.</span><span class="sxs-lookup"><span data-stu-id="782e6-155">The app doesn't start.</span></span>

<span data-ttu-id="782e6-156">[Moduł ASP.NET Core](xref:host-and-deploy/aspnet-core-module) nie może odnaleźć procedury obsługi żądania hostingu poza procesem.</span><span class="sxs-lookup"><span data-stu-id="782e6-156">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) fails to find the out-of-process hosting request handler.</span></span> <span data-ttu-id="782e6-157">Upewnij się, że *aspnetcorev2_outofprocess.dll* znajduje się w podfolderze obok *aspnetcorev2.dll*.</span><span class="sxs-lookup"><span data-stu-id="782e6-157">Make sure the *aspnetcorev2_outofprocess.dll* is present in a subfolder next to *aspnetcorev2.dll*.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

### <a name="5000-in-process-handler-load-failure"></a><span data-ttu-id="782e6-158">500.0 w procesie programu obsługi błędu ładowania</span><span class="sxs-lookup"><span data-stu-id="782e6-158">500.0 In-Process Handler Load Failure</span></span>

<span data-ttu-id="782e6-159">Proces roboczy kończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="782e6-159">The worker process fails.</span></span> <span data-ttu-id="782e6-160">Nie zaczyna się aplikacja.</span><span class="sxs-lookup"><span data-stu-id="782e6-160">The app doesn't start.</span></span>

<span data-ttu-id="782e6-161">Wystąpił nieznany błąd podczas ładowania składników [modułu ASP.NET Core](xref:host-and-deploy/aspnet-core-module) .</span><span class="sxs-lookup"><span data-stu-id="782e6-161">An unknown error occurred loading [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) components.</span></span> <span data-ttu-id="782e6-162">Wykonaj jedną z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="782e6-162">Take one of the following actions:</span></span>

* <span data-ttu-id="782e6-163">[Pomoc techniczna firmy Microsoft](https://support.microsoft.com/oas/default.aspx?prid=15832) kontaktu (wybierz **Narzędzia deweloperskie** następnie **ASP.NET Core**).</span><span class="sxs-lookup"><span data-stu-id="782e6-163">Contact [Microsoft Support](https://support.microsoft.com/oas/default.aspx?prid=15832) (select **Developer Tools** then **ASP.NET Core**).</span></span>
* <span data-ttu-id="782e6-164">Zadawaj pytanie na Stack Overflow.</span><span class="sxs-lookup"><span data-stu-id="782e6-164">Ask a question on Stack Overflow.</span></span>
* <span data-ttu-id="782e6-165">Zajrzyj do problemu w naszym [repozytorium GitHub](https://github.com/dotnet/AspNetCore).</span><span class="sxs-lookup"><span data-stu-id="782e6-165">File an issue on our [GitHub repository](https://github.com/dotnet/AspNetCore).</span></span>

### <a name="50030-in-process-startup-failure"></a><span data-ttu-id="782e6-166">500.30 w procesie Niepowodzenie uruchamiania</span><span class="sxs-lookup"><span data-stu-id="782e6-166">500.30 In-Process Startup Failure</span></span>

<span data-ttu-id="782e6-167">Proces roboczy kończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="782e6-167">The worker process fails.</span></span> <span data-ttu-id="782e6-168">Nie zaczyna się aplikacja.</span><span class="sxs-lookup"><span data-stu-id="782e6-168">The app doesn't start.</span></span>

<span data-ttu-id="782e6-169">[Moduł ASP.NET Core](xref:host-and-deploy/aspnet-core-module) próbuje uruchomić program .NET Core CLR w procesie, ale nie można go uruchomić.</span><span class="sxs-lookup"><span data-stu-id="782e6-169">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) attempts to start the .NET Core CLR in-process, but it fails to start.</span></span> <span data-ttu-id="782e6-170">Przyczyna niepowodzenia uruchomienia procesu zwykle można ustalić na podstawie wpisów w dzienniku zdarzeń aplikacji i dzienniku modułu ASP.NET Core stdout.</span><span class="sxs-lookup"><span data-stu-id="782e6-170">The cause of a process startup failure can usually be determined from entries in the Application Event Log and the ASP.NET Core Module stdout log.</span></span>

<span data-ttu-id="782e6-171">Typowe warunki awarii:</span><span class="sxs-lookup"><span data-stu-id="782e6-171">Common failure conditions:</span></span>

* <span data-ttu-id="782e6-172">Aplikacja jest nieprawidłowo skonfigurowana z powodu docelowej wersji ASP.NET Core udostępnionej platformy, która nie istnieje.</span><span class="sxs-lookup"><span data-stu-id="782e6-172">The app is misconfigured due to targeting a version of the ASP.NET Core shared framework that isn't present.</span></span> <span data-ttu-id="782e6-173">Sprawdź, które wersje udostępnionej platformy ASP.NET Core są zainstalowane na komputerze docelowym.</span><span class="sxs-lookup"><span data-stu-id="782e6-173">Check which versions of the ASP.NET Core shared framework are installed on the target machine.</span></span>
* <span data-ttu-id="782e6-174">Za pomocą Azure Key Vault, brak uprawnień do Key Vault.</span><span class="sxs-lookup"><span data-stu-id="782e6-174">Using Azure Key Vault, lack of permissions to the Key Vault.</span></span> <span data-ttu-id="782e6-175">Sprawdź zasady dostępu w Key Vault celem, aby upewnić się, że udzielono odpowiednich uprawnień.</span><span class="sxs-lookup"><span data-stu-id="782e6-175">Check the access policies in the targeted Key Vault to ensure that the correct permissions are granted.</span></span>

### <a name="50031-ancm-failed-to-find-native-dependencies"></a><span data-ttu-id="782e6-176">500,31 ANCM nie może odnaleźć natywnych zależności</span><span class="sxs-lookup"><span data-stu-id="782e6-176">500.31 ANCM Failed to Find Native Dependencies</span></span>

<span data-ttu-id="782e6-177">Proces roboczy kończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="782e6-177">The worker process fails.</span></span> <span data-ttu-id="782e6-178">Nie zaczyna się aplikacja.</span><span class="sxs-lookup"><span data-stu-id="782e6-178">The app doesn't start.</span></span>

<span data-ttu-id="782e6-179">[Moduł ASP.NET Core](xref:host-and-deploy/aspnet-core-module) próbuje uruchomić środowisko uruchomieniowe programu .NET Core w procesie, ale nie może go uruchomić.</span><span class="sxs-lookup"><span data-stu-id="782e6-179">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) attempts to start the .NET Core runtime in-process, but it fails to start.</span></span> <span data-ttu-id="782e6-180">Najczęstszym powodem tego błędu uruchamiania jest to, kiedy `Microsoft.NETCore.App` lub środowisko uruchomieniowe `Microsoft.AspNetCore.App` nie jest zainstalowane.</span><span class="sxs-lookup"><span data-stu-id="782e6-180">The most common cause of this startup failure is when the `Microsoft.NETCore.App` or `Microsoft.AspNetCore.App` runtime isn't installed.</span></span> <span data-ttu-id="782e6-181">Jeśli aplikacja jest wdrożona w programie docelowym ASP.NET Core 3,0 i ta wersja nie istnieje na maszynie, wystąpi błąd.</span><span class="sxs-lookup"><span data-stu-id="782e6-181">If the app is deployed to target ASP.NET Core 3.0 and that version doesn't exist on the machine, this error occurs.</span></span> <span data-ttu-id="782e6-182">Poniżej znajduje się przykładowy komunikat o błędzie:</span><span class="sxs-lookup"><span data-stu-id="782e6-182">An example error message follows:</span></span>

```
The specified framework 'Microsoft.NETCore.App', version '3.0.0' was not found.
  - The following frameworks were found:
      2.2.1 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview5-27626-15 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview6-27713-13 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview6-27714-15 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview6-27723-08 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
```

<span data-ttu-id="782e6-183">W komunikacie o błędzie są wyświetlane wszystkie zainstalowane wersje programu .NET Core i wersja żądana przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="782e6-183">The error message lists all the installed .NET Core versions and the version requested by the app.</span></span> <span data-ttu-id="782e6-184">Aby naprawić ten błąd, należy:</span><span class="sxs-lookup"><span data-stu-id="782e6-184">To fix this error, either:</span></span>

* <span data-ttu-id="782e6-185">Zainstaluj na komputerze odpowiednią wersję programu .NET Core.</span><span class="sxs-lookup"><span data-stu-id="782e6-185">Install the appropriate version of .NET Core on the machine.</span></span>
* <span data-ttu-id="782e6-186">Zmień aplikację na wersję docelową platformy .NET Core, która jest obecna na maszynie.</span><span class="sxs-lookup"><span data-stu-id="782e6-186">Change the app to target a version of .NET Core that's present on the machine.</span></span>
* <span data-ttu-id="782e6-187">Publikowanie aplikacji jako [samodzielnego wdrożenia](/dotnet/core/deploying/#self-contained-deployments-scd).</span><span class="sxs-lookup"><span data-stu-id="782e6-187">Publish the app as a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd).</span></span>

<span data-ttu-id="782e6-188">Podczas pracy w środowisku deweloperskim (`ASPNETCORE_ENVIRONMENT` zmienna środowiskowa jest ustawiona na `Development`) określony błąd jest zapisywana w odpowiedzi HTTP.</span><span class="sxs-lookup"><span data-stu-id="782e6-188">When running in development (the `ASPNETCORE_ENVIRONMENT` environment variable is set to `Development`), the specific error is written to the HTTP response.</span></span> <span data-ttu-id="782e6-189">Przyczyna niepowodzenia uruchomienia procesu znajduje się również w dzienniku zdarzeń aplikacji.</span><span class="sxs-lookup"><span data-stu-id="782e6-189">The cause of a process startup failure is also found in the Application Event Log.</span></span>

### <a name="50032-ancm-failed-to-load-dll"></a><span data-ttu-id="782e6-190">500,32 ANCM nie może załadować biblioteki DLL</span><span class="sxs-lookup"><span data-stu-id="782e6-190">500.32 ANCM Failed to Load dll</span></span>

<span data-ttu-id="782e6-191">Proces roboczy kończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="782e6-191">The worker process fails.</span></span> <span data-ttu-id="782e6-192">Nie zaczyna się aplikacja.</span><span class="sxs-lookup"><span data-stu-id="782e6-192">The app doesn't start.</span></span>

<span data-ttu-id="782e6-193">Najbardziej typową przyczyną tego błędu jest to, że aplikacja jest publikowana dla niezgodnej architektury procesora.</span><span class="sxs-lookup"><span data-stu-id="782e6-193">The most common cause for this error is that the app is published for an incompatible processor architecture.</span></span> <span data-ttu-id="782e6-194">Jeśli proces roboczy jest uruchomiony jako aplikacja 32-bitowa, a aplikacja została opublikowana w docelowym 64-bitowym, ten błąd wystąpi.</span><span class="sxs-lookup"><span data-stu-id="782e6-194">If the worker process is running as a 32-bit app and the app was published to target 64-bit, this error occurs.</span></span>

<span data-ttu-id="782e6-195">Aby naprawić ten błąd, należy:</span><span class="sxs-lookup"><span data-stu-id="782e6-195">To fix this error, either:</span></span>

* <span data-ttu-id="782e6-196">Opublikuj ponownie aplikację dla tej samej architektury procesora co proces roboczy.</span><span class="sxs-lookup"><span data-stu-id="782e6-196">Republish the app for the same processor architecture as the worker process.</span></span>
* <span data-ttu-id="782e6-197">Opublikuj aplikację jako [wdrożenie zależne od platformy](/dotnet/core/deploying/#framework-dependent-executables-fde).</span><span class="sxs-lookup"><span data-stu-id="782e6-197">Publish the app as a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-executables-fde).</span></span>

### <a name="50033-ancm-request-handler-load-failure"></a><span data-ttu-id="782e6-198">500,33 błąd ładowania obsługi żądania ANCM</span><span class="sxs-lookup"><span data-stu-id="782e6-198">500.33 ANCM Request Handler Load Failure</span></span>

<span data-ttu-id="782e6-199">Proces roboczy kończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="782e6-199">The worker process fails.</span></span> <span data-ttu-id="782e6-200">Nie zaczyna się aplikacja.</span><span class="sxs-lookup"><span data-stu-id="782e6-200">The app doesn't start.</span></span>

<span data-ttu-id="782e6-201">Aplikacja nie odwołuje się do `Microsoft.AspNetCore.App` Framework.</span><span class="sxs-lookup"><span data-stu-id="782e6-201">The app didn't reference the `Microsoft.AspNetCore.App` framework.</span></span> <span data-ttu-id="782e6-202">Tylko aplikacje ukierunkowane na `Microsoft.AspNetCore.App` Framework mogą być hostowane przez [moduł ASP.NET Core](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="782e6-202">Only apps targeting the `Microsoft.AspNetCore.App` framework can be hosted by the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module).</span></span>

<span data-ttu-id="782e6-203">Aby naprawić ten błąd, upewnij się, że aplikacja jest ukierunkowana na strukturę `Microsoft.AspNetCore.App`.</span><span class="sxs-lookup"><span data-stu-id="782e6-203">To fix this error, confirm that the app is targeting the `Microsoft.AspNetCore.App` framework.</span></span> <span data-ttu-id="782e6-204">Sprawdź `.runtimeconfig.json`, aby sprawdzić, czy struktura jest przeznaczona dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="782e6-204">Check the `.runtimeconfig.json` to verify the framework targeted by the app.</span></span>

### <a name="50034-ancm-mixed-hosting-models-not-supported"></a><span data-ttu-id="782e6-205">500,34 ANCM mieszane modele hostingu nie są obsługiwane</span><span class="sxs-lookup"><span data-stu-id="782e6-205">500.34 ANCM Mixed Hosting Models Not Supported</span></span>

<span data-ttu-id="782e6-206">Proces roboczy nie może uruchomić zarówno aplikacji w procesie, jak i aplikacji pozaprocesowej w tym samym procesie.</span><span class="sxs-lookup"><span data-stu-id="782e6-206">The worker process can't run both an in-process app and an out-of-process app in the same process.</span></span>

<span data-ttu-id="782e6-207">Aby naprawić ten błąd, uruchom aplikacje w osobnych pulach aplikacji usług IIS.</span><span class="sxs-lookup"><span data-stu-id="782e6-207">To fix this error, run apps in separate IIS application pools.</span></span>

### <a name="50035-ancm-multiple-in-process-applications-in-same-process"></a><span data-ttu-id="782e6-208">500,35 ANCM wiele aplikacji w procesie w tym samym procesie</span><span class="sxs-lookup"><span data-stu-id="782e6-208">500.35 ANCM Multiple In-Process Applications in same Process</span></span>

<span data-ttu-id="782e6-209">Proces roboczy nie może uruchamiać wielu aplikacji w procesie w tym samym procesie.</span><span class="sxs-lookup"><span data-stu-id="782e6-209">The worker process can't run multiple in-process apps in the same process.</span></span>

<span data-ttu-id="782e6-210">Aby naprawić ten błąd, uruchom aplikacje w osobnych pulach aplikacji usług IIS.</span><span class="sxs-lookup"><span data-stu-id="782e6-210">To fix this error, run apps in separate IIS application pools.</span></span>

### <a name="50036-ancm-out-of-process-handler-load-failure"></a><span data-ttu-id="782e6-211">500,36 ANCM błąd ładowania procedury obsługi poza procesem</span><span class="sxs-lookup"><span data-stu-id="782e6-211">500.36 ANCM Out-Of-Process Handler Load Failure</span></span>

<span data-ttu-id="782e6-212">Procedura obsługi żądań out-of-process, *aspnetcorev2_outofprocess. dll*, nie jest obok pliku *aspnetcorev2. dll* .</span><span class="sxs-lookup"><span data-stu-id="782e6-212">The out-of-process request handler, *aspnetcorev2_outofprocess.dll*, isn't next to the *aspnetcorev2.dll* file.</span></span> <span data-ttu-id="782e6-213">Oznacza to uszkodzenie instalacji [modułu ASP.NET Core](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="782e6-213">This indicates a corrupted installation of the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module).</span></span>

<span data-ttu-id="782e6-214">Aby naprawić ten błąd, napraw instalację [pakietu hostingu platformy .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle) (dla usług IIS) lub programu Visual Studio (w przypadku IIS Express).</span><span class="sxs-lookup"><span data-stu-id="782e6-214">To fix this error, repair the installation of the [.NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle) (for IIS) or Visual Studio (for IIS Express).</span></span>

### <a name="50037-ancm-failed-to-start-within-startup-time-limit"></a><span data-ttu-id="782e6-215">Nie można uruchomić 500,37 ANCM w ramach limitu czasu uruchamiania</span><span class="sxs-lookup"><span data-stu-id="782e6-215">500.37 ANCM Failed to Start Within Startup Time Limit</span></span>

<span data-ttu-id="782e6-216">Nie można uruchomić ANCM w limicie czasu uruchamiania dostarczają.</span><span class="sxs-lookup"><span data-stu-id="782e6-216">ANCM failed to start within the provied startup time limit.</span></span> <span data-ttu-id="782e6-217">Domyślnie limit czasu wynosi 120 sekund.</span><span class="sxs-lookup"><span data-stu-id="782e6-217">By default, the timeout is 120 seconds.</span></span>

<span data-ttu-id="782e6-218">Ten błąd może wystąpić podczas uruchamiania dużej liczby aplikacji na tym samym komputerze.</span><span class="sxs-lookup"><span data-stu-id="782e6-218">This error can occur when starting a large number of apps on the same machine.</span></span> <span data-ttu-id="782e6-219">Podczas uruchamiania Sprawdź, czy na serwerze są naskoki użycie procesora CPU/pamięci.</span><span class="sxs-lookup"><span data-stu-id="782e6-219">Check for CPU/Memory usage spikes on the server during startup.</span></span> <span data-ttu-id="782e6-220">Może być konieczne rozłożenie procesu uruchamiania wielu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="782e6-220">You may need to stagger the startup process of multiple apps.</span></span>

::: moniker-end

### <a name="5025-process-failure"></a><span data-ttu-id="782e6-221">502.5 niepowodzenie procesu</span><span class="sxs-lookup"><span data-stu-id="782e6-221">502.5 Process Failure</span></span>

<span data-ttu-id="782e6-222">Proces roboczy kończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="782e6-222">The worker process fails.</span></span> <span data-ttu-id="782e6-223">Nie zaczyna się aplikacja.</span><span class="sxs-lookup"><span data-stu-id="782e6-223">The app doesn't start.</span></span>

<span data-ttu-id="782e6-224">[Moduł ASP.NET Core](xref:host-and-deploy/aspnet-core-module) próbuje uruchomić proces roboczy, ale jego uruchomienie nie powiedzie się.</span><span class="sxs-lookup"><span data-stu-id="782e6-224">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) attempts to start the worker process but it fails to start.</span></span> <span data-ttu-id="782e6-225">Przyczyna niepowodzenia uruchomienia procesu zwykle można ustalić na podstawie wpisów w dzienniku zdarzeń aplikacji i dzienniku modułu ASP.NET Core stdout.</span><span class="sxs-lookup"><span data-stu-id="782e6-225">The cause of a process startup failure can usually be determined from entries in the Application Event Log and the ASP.NET Core Module stdout log.</span></span>

<span data-ttu-id="782e6-226">Aplikacja jest błędnie skonfigurowane z powodu przeznaczony dla wersji udostępnionej platformy ASP.NET Core, która nie jest obecny jest jakiś wspólny warunek błędu.</span><span class="sxs-lookup"><span data-stu-id="782e6-226">A common failure condition is the app is misconfigured due to targeting a version of the ASP.NET Core shared framework that isn't present.</span></span> <span data-ttu-id="782e6-227">Sprawdź, które wersje udostępnionej platformy ASP.NET Core są zainstalowane na komputerze docelowym.</span><span class="sxs-lookup"><span data-stu-id="782e6-227">Check which versions of the ASP.NET Core shared framework are installed on the target machine.</span></span> <span data-ttu-id="782e6-228">*Platforma udostępniona* to zestaw zestawów (pliki *. dll* ), które są zainstalowane na komputerze i do których odwołuje się pakiet, taki jak `Microsoft.AspNetCore.App`.</span><span class="sxs-lookup"><span data-stu-id="782e6-228">The *shared framework* is the set of assemblies (*.dll* files) that are installed on the machine and referenced by a metapackage such as `Microsoft.AspNetCore.App`.</span></span> <span data-ttu-id="782e6-229">Odwołanie do pakietu nie może określać minimalnej wymaganej wersji.</span><span class="sxs-lookup"><span data-stu-id="782e6-229">The metapackage reference can specify a minimum required version.</span></span> <span data-ttu-id="782e6-230">Aby uzyskać więcej informacji, zobacz [udostępnioną strukturę](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/).</span><span class="sxs-lookup"><span data-stu-id="782e6-230">For more information, see [The shared framework](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/).</span></span>

<span data-ttu-id="782e6-231">*502.5 niepowodzenia procesu* strony błędu jest zwracany, jeśli aplikacji lub obsługującego błędnej konfiguracji powoduje niepowodzenie procesu roboczego:</span><span class="sxs-lookup"><span data-stu-id="782e6-231">The *502.5 Process Failure* error page is returned when a hosting or app misconfiguration causes the worker process to fail:</span></span>

### <a name="failed-to-start-application-errorcode-0x800700c1"></a><span data-ttu-id="782e6-232">Nie można uruchomić aplikację (kod błędu "0x800700c1")</span><span class="sxs-lookup"><span data-stu-id="782e6-232">Failed to start application (ErrorCode '0x800700c1')</span></span>

```
EventID: 1010
Source: IIS AspNetCore Module V2
Failed to start application '/LM/W3SVC/6/ROOT/', ErrorCode '0x800700c1'.
```

<span data-ttu-id="782e6-233">Aplikacji nie powiodło się, ponieważ zestaw aplikacji ( *.dll*) nie można go załadować.</span><span class="sxs-lookup"><span data-stu-id="782e6-233">The app failed to start because the app's assembly (*.dll*) couldn't be loaded.</span></span>

<span data-ttu-id="782e6-234">Ten błąd występuje, gdy występuje niezgodność liczby bitów opublikowanej aplikacji i procesu w3wp/programu iisexpress.</span><span class="sxs-lookup"><span data-stu-id="782e6-234">This error occurs when there's a bitness mismatch between the published app and the w3wp/iisexpress process.</span></span>

<span data-ttu-id="782e6-235">Upewnij się, że ustawienie 32-bitowych puli aplikacji jest prawidłowy:</span><span class="sxs-lookup"><span data-stu-id="782e6-235">Confirm that the app pool's 32-bit setting is correct:</span></span>

1. <span data-ttu-id="782e6-236">Wybierz pulę aplikacji w Menedżerze usług IIS w **pul aplikacji**.</span><span class="sxs-lookup"><span data-stu-id="782e6-236">Select the app pool in IIS Manager's **Application Pools**.</span></span>
1. <span data-ttu-id="782e6-237">Wybierz **Zaawansowane ustawienia** w obszarze **edytowanie puli aplikacji** w **akcje** panelu.</span><span class="sxs-lookup"><span data-stu-id="782e6-237">Select **Advanced Settings** under **Edit Application Pool** in the **Actions** panel.</span></span>
1. <span data-ttu-id="782e6-238">Ustaw **Włącz aplikacje 32-bitowe**:</span><span class="sxs-lookup"><span data-stu-id="782e6-238">Set **Enable 32-Bit Applications**:</span></span>
   * <span data-ttu-id="782e6-239">Jeśli wdrażanie (x86) 32-bitowych aplikacji, ustaw wartość `True`.</span><span class="sxs-lookup"><span data-stu-id="782e6-239">If deploying a 32-bit (x86) app, set the value to `True`.</span></span>
   * <span data-ttu-id="782e6-240">Jeśli wdrażanie (x64) 64-bitowych aplikacji, ustaw wartość `False`.</span><span class="sxs-lookup"><span data-stu-id="782e6-240">If deploying a 64-bit (x64) app, set the value to `False`.</span></span>

<span data-ttu-id="782e6-241">Upewnij się, że nie występuje konflikt między właściwością programu MSBuild `<Platform>` w pliku projektu a opublikowaną bitową ilością aplikacji.</span><span class="sxs-lookup"><span data-stu-id="782e6-241">Confirm that there isn't a conflict between a `<Platform>` MSBuild property in the project file and the published bitness of the app.</span></span>

### <a name="connection-reset"></a><span data-ttu-id="782e6-242">Resetowanie połączenia</span><span class="sxs-lookup"><span data-stu-id="782e6-242">Connection reset</span></span>

<span data-ttu-id="782e6-243">Jeśli błąd wystąpi po nagłówki są wysyłane, jest za późno serwera wysłać **500 Wewnętrzny błąd serwera** po wystąpieniu błędu.</span><span class="sxs-lookup"><span data-stu-id="782e6-243">If an error occurs after the headers are sent, it's too late for the server to send a **500 Internal Server Error** when an error occurs.</span></span> <span data-ttu-id="782e6-244">Dzieje się tak często, gdy wystąpi błąd podczas serializacji obiektów złożonych na odpowiedź.</span><span class="sxs-lookup"><span data-stu-id="782e6-244">This often happens when an error occurs during the serialization of complex objects for a response.</span></span> <span data-ttu-id="782e6-245">Tego typu błędu jest wyświetlany jako *resetowania połączenia* błąd na komputerze klienckim.</span><span class="sxs-lookup"><span data-stu-id="782e6-245">This type of error appears as a *connection reset* error on the client.</span></span> <span data-ttu-id="782e6-246">[Rejestrowanie aplikacji](xref:fundamentals/logging/index) mogą pomóc rozwiązać tego rodzaju błędów.</span><span class="sxs-lookup"><span data-stu-id="782e6-246">[Application logging](xref:fundamentals/logging/index) can help troubleshoot these types of errors.</span></span>

### <a name="default-startup-limits"></a><span data-ttu-id="782e6-247">Domyślne limity uruchamiania</span><span class="sxs-lookup"><span data-stu-id="782e6-247">Default startup limits</span></span>

<span data-ttu-id="782e6-248">[Moduł ASP.NET Core](xref:host-and-deploy/aspnet-core-module) jest skonfigurowany z domyślną *startupTimeLimitą* 120 sekund.</span><span class="sxs-lookup"><span data-stu-id="782e6-248">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) is configured with a default *startupTimeLimit* of 120 seconds.</span></span> <span data-ttu-id="782e6-249">Gdy pozostawić wartość domyślną, aplikacja może potrwać do dwóch minut przed moduł dzienniki awarii procesu.</span><span class="sxs-lookup"><span data-stu-id="782e6-249">When left at the default value, an app may take up to two minutes to start before the module logs a process failure.</span></span> <span data-ttu-id="782e6-250">Aby uzyskać informacje na temat konfigurowania modułu, zobacz [atrybuty elementu aspNetCore](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span><span class="sxs-lookup"><span data-stu-id="782e6-250">For information on configuring the module, see [Attributes of the aspNetCore element](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span></span>

## <a name="troubleshoot-on-azure-app-service"></a><span data-ttu-id="782e6-251">Rozwiązywanie problemów dotyczących Azure App Service</span><span class="sxs-lookup"><span data-stu-id="782e6-251">Troubleshoot on Azure App Service</span></span>

[!INCLUDE [Azure App Service Preview Notice](~/includes/azure-apps-preview-notice.md)]

### <a name="application-event-log-azure-app-service"></a><span data-ttu-id="782e6-252">Dziennik zdarzeń aplikacji (Azure App Service)</span><span class="sxs-lookup"><span data-stu-id="782e6-252">Application Event Log (Azure App Service)</span></span>

<span data-ttu-id="782e6-253">Aby uzyskać dostęp do dziennika zdarzeń aplikacji, użyj bloku **diagnozowanie i rozwiązywanie problemów** w Azure Portal:</span><span class="sxs-lookup"><span data-stu-id="782e6-253">To access the Application Event Log, use the **Diagnose and solve problems** blade in the Azure portal:</span></span>

1. <span data-ttu-id="782e6-254">W Azure Portal Otwórz aplikację w **App Services**.</span><span class="sxs-lookup"><span data-stu-id="782e6-254">In the Azure portal, open the app in **App Services**.</span></span>
1. <span data-ttu-id="782e6-255">Kliknij pozycję **Diagnozowanie i rozwiązywanie problemów**.</span><span class="sxs-lookup"><span data-stu-id="782e6-255">Select **Diagnose and solve problems**.</span></span>
1. <span data-ttu-id="782e6-256">Wybierz nagłówek **Narzędzia diagnostyczne** .</span><span class="sxs-lookup"><span data-stu-id="782e6-256">Select the **Diagnostic Tools** heading.</span></span>
1. <span data-ttu-id="782e6-257">W obszarze **Narzędzia obsługi**wybierz przycisk **zdarzenia aplikacji** .</span><span class="sxs-lookup"><span data-stu-id="782e6-257">Under **Support Tools**, select the **Application Events** button.</span></span>
1. <span data-ttu-id="782e6-258">Zapoznaj się z najnowszym błędem podanym w pozycji *AspNetCoreModule IIS* lub *IIS AspNetCoreModule v2* w kolumnie **Źródło** .</span><span class="sxs-lookup"><span data-stu-id="782e6-258">Examine the latest error provided by the *IIS AspNetCoreModule* or *IIS AspNetCoreModule V2* entry in the **Source** column.</span></span>

<span data-ttu-id="782e6-259">Alternatywą dla korzystania z bloku **diagnozowanie i rozwiązywanie problemów** jest przetestowanie pliku dziennika zdarzeń aplikacji bezpośrednio przy użyciu [kudu](https://github.com/projectkudu/kudu/wiki):</span><span class="sxs-lookup"><span data-stu-id="782e6-259">An alternative to using the **Diagnose and solve problems** blade is to examine the Application Event Log file directly using [Kudu](https://github.com/projectkudu/kudu/wiki):</span></span>

1. <span data-ttu-id="782e6-260">Otwórz **Narzędzia zaawansowane** w obszarze **Narzędzia programistyczne** .</span><span class="sxs-lookup"><span data-stu-id="782e6-260">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="782e6-261">Wybierz przycisk **przejdź&rarr;** .</span><span class="sxs-lookup"><span data-stu-id="782e6-261">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="782e6-262">Konsola kudu otwiera się w nowej karcie lub oknie przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="782e6-262">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="782e6-263">Korzystając z paska nawigacyjnego w górnej części strony, Otwórz **konsolę debugowanie** i wybierz polecenie **cmd**.</span><span class="sxs-lookup"><span data-stu-id="782e6-263">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="782e6-264">Otwórz folder **LogFiles** .</span><span class="sxs-lookup"><span data-stu-id="782e6-264">Open the **LogFiles** folder.</span></span>
1. <span data-ttu-id="782e6-265">Wybierz ikonę ołówka obok pliku *EventLog. XML* .</span><span class="sxs-lookup"><span data-stu-id="782e6-265">Select the pencil icon next to the *eventlog.xml* file.</span></span>
1. <span data-ttu-id="782e6-266">Przejrzyj dziennik.</span><span class="sxs-lookup"><span data-stu-id="782e6-266">Examine the log.</span></span> <span data-ttu-id="782e6-267">Przewiń w dół dziennika, aby zobaczyć najnowsze zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="782e6-267">Scroll to the bottom of the log to see the most recent events.</span></span>

### <a name="run-the-app-in-the-kudu-console"></a><span data-ttu-id="782e6-268">Uruchamianie aplikacji w konsoli kudu</span><span class="sxs-lookup"><span data-stu-id="782e6-268">Run the app in the Kudu console</span></span>

<span data-ttu-id="782e6-269">Wiele błędów uruchamiania przestaną generować przydatne informacje w dzienniku zdarzeń aplikacji.</span><span class="sxs-lookup"><span data-stu-id="782e6-269">Many startup errors don't produce useful information in the Application Event Log.</span></span> <span data-ttu-id="782e6-270">Możesz uruchomić aplikację w konsoli zdalnego wykonywania [kudu](https://github.com/projectkudu/kudu/wiki) , aby wykryć błąd:</span><span class="sxs-lookup"><span data-stu-id="782e6-270">You can run the app in the [Kudu](https://github.com/projectkudu/kudu/wiki) Remote Execution Console to discover the error:</span></span>

1. <span data-ttu-id="782e6-271">Otwórz **Narzędzia zaawansowane** w obszarze **Narzędzia programistyczne** .</span><span class="sxs-lookup"><span data-stu-id="782e6-271">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="782e6-272">Wybierz przycisk **przejdź&rarr;** .</span><span class="sxs-lookup"><span data-stu-id="782e6-272">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="782e6-273">Konsola kudu otwiera się w nowej karcie lub oknie przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="782e6-273">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="782e6-274">Korzystając z paska nawigacyjnego w górnej części strony, Otwórz **konsolę debugowanie** i wybierz polecenie **cmd**.</span><span class="sxs-lookup"><span data-stu-id="782e6-274">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>

#### <a name="test-a-32-bit-x86-app"></a><span data-ttu-id="782e6-275">Testowanie aplikacji 32-bitowej (x86)</span><span class="sxs-lookup"><span data-stu-id="782e6-275">Test a 32-bit (x86) app</span></span>

<span data-ttu-id="782e6-276">**Bieżąca wersja**</span><span class="sxs-lookup"><span data-stu-id="782e6-276">**Current release**</span></span>

1. `cd d:\home\site\wwwroot`
1. <span data-ttu-id="782e6-277">Uruchom aplikację:</span><span class="sxs-lookup"><span data-stu-id="782e6-277">Run the app:</span></span>
   * <span data-ttu-id="782e6-278">Jeśli aplikacja jest [wdrożenia zależny od struktury](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span><span class="sxs-lookup"><span data-stu-id="782e6-278">If the app is a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>

     ```dotnetcli
     dotnet .\{ASSEMBLY NAME}.dll
     ```

   * <span data-ttu-id="782e6-279">Jeśli aplikacja jest [niezależna wdrożenia](/dotnet/core/deploying/#self-contained-deployments-scd):</span><span class="sxs-lookup"><span data-stu-id="782e6-279">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>

     ```console
     {ASSEMBLY NAME}.exe
     ```

<span data-ttu-id="782e6-280">Dane wyjściowe konsoli z aplikacji, pokazujące wszystkie błędy, są przekazywane do konsoli Kudu.</span><span class="sxs-lookup"><span data-stu-id="782e6-280">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

<span data-ttu-id="782e6-281">**Wdrożenie zależne od platformy uruchomione w wersji zapoznawczej**</span><span class="sxs-lookup"><span data-stu-id="782e6-281">**Framework-dependent deployment running on a preview release**</span></span>

<span data-ttu-id="782e6-282">*Wymaga zainstalowania rozszerzenia witryny środowiska uruchomieniowego ASP.NET Core {VERSION} (x86).*</span><span class="sxs-lookup"><span data-stu-id="782e6-282">*Requires installing the ASP.NET Core {VERSION} (x86) Runtime site extension.*</span></span>

1. <span data-ttu-id="782e6-283">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x32` (`{X.Y}` to wersja środowiska uruchomieniowego)</span><span class="sxs-lookup"><span data-stu-id="782e6-283">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x32` (`{X.Y}` is the runtime version)</span></span>
1. <span data-ttu-id="782e6-284">Uruchom aplikację: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span><span class="sxs-lookup"><span data-stu-id="782e6-284">Run the app: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span></span>

<span data-ttu-id="782e6-285">Dane wyjściowe konsoli z aplikacji, pokazujące wszystkie błędy, są przekazywane do konsoli Kudu.</span><span class="sxs-lookup"><span data-stu-id="782e6-285">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

#### <a name="test-a-64-bit-x64-app"></a><span data-ttu-id="782e6-286">Testowanie aplikacji 64-bitowej (x64)</span><span class="sxs-lookup"><span data-stu-id="782e6-286">Test a 64-bit (x64) app</span></span>

<span data-ttu-id="782e6-287">**Bieżąca wersja**</span><span class="sxs-lookup"><span data-stu-id="782e6-287">**Current release**</span></span>

* <span data-ttu-id="782e6-288">Jeśli aplikacja jest wdrożeniem 64-bitowym (x64), [zależnym od platformy](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span><span class="sxs-lookup"><span data-stu-id="782e6-288">If the app is a 64-bit (x64) [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>
  1. `cd D:\Program Files\dotnet`
  1. <span data-ttu-id="782e6-289">Uruchom aplikację: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span><span class="sxs-lookup"><span data-stu-id="782e6-289">Run the app: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span></span>
* <span data-ttu-id="782e6-290">Jeśli aplikacja jest [niezależna wdrożenia](/dotnet/core/deploying/#self-contained-deployments-scd):</span><span class="sxs-lookup"><span data-stu-id="782e6-290">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>
  1. `cd D:\home\site\wwwroot`
  1. <span data-ttu-id="782e6-291">Uruchom aplikację: `{ASSEMBLY NAME}.exe`</span><span class="sxs-lookup"><span data-stu-id="782e6-291">Run the app: `{ASSEMBLY NAME}.exe`</span></span>

<span data-ttu-id="782e6-292">Dane wyjściowe konsoli z aplikacji, pokazujące wszystkie błędy, są przekazywane do konsoli Kudu.</span><span class="sxs-lookup"><span data-stu-id="782e6-292">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

<span data-ttu-id="782e6-293">**Wdrożenie zależne od platformy uruchomione w wersji zapoznawczej**</span><span class="sxs-lookup"><span data-stu-id="782e6-293">**Framework-dependent deployment running on a preview release**</span></span>

<span data-ttu-id="782e6-294">*Wymaga zainstalowania rozszerzenia witryny środowiska uruchomieniowego ASP.NET Core {VERSION} (x64).*</span><span class="sxs-lookup"><span data-stu-id="782e6-294">*Requires installing the ASP.NET Core {VERSION} (x64) Runtime site extension.*</span></span>

1. <span data-ttu-id="782e6-295">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x64` (`{X.Y}` to wersja środowiska uruchomieniowego)</span><span class="sxs-lookup"><span data-stu-id="782e6-295">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x64` (`{X.Y}` is the runtime version)</span></span>
1. <span data-ttu-id="782e6-296">Uruchom aplikację: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span><span class="sxs-lookup"><span data-stu-id="782e6-296">Run the app: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span></span>

<span data-ttu-id="782e6-297">Dane wyjściowe konsoli z aplikacji, pokazujące wszystkie błędy, są przekazywane do konsoli Kudu.</span><span class="sxs-lookup"><span data-stu-id="782e6-297">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

### <a name="aspnet-core-module-stdout-log-azure-app-service"></a><span data-ttu-id="782e6-298">Dziennik stdout modułu ASP.NET Core (Azure App Service)</span><span class="sxs-lookup"><span data-stu-id="782e6-298">ASP.NET Core Module stdout log (Azure App Service)</span></span>

<span data-ttu-id="782e6-299">Dziennik modułu ASP.NET Core stdout często rejestruje przydatne komunikaty o błędach, które nie są dostępne w dzienniku zdarzeń aplikacji.</span><span class="sxs-lookup"><span data-stu-id="782e6-299">The ASP.NET Core Module stdout log often records useful error messages not found in the Application Event Log.</span></span> <span data-ttu-id="782e6-300">Włączanie i wyświetlanie dzienników stdout:</span><span class="sxs-lookup"><span data-stu-id="782e6-300">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="782e6-301">Przejdź do bloku **diagnozowanie i rozwiązywanie problemów** w Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="782e6-301">Navigate to the **Diagnose and solve problems** blade in the Azure portal.</span></span>
1. <span data-ttu-id="782e6-302">W obszarze **Wybierz kategorię problemu**wybierz przycisk **aplikacji sieci Web w dół** .</span><span class="sxs-lookup"><span data-stu-id="782e6-302">Under **SELECT PROBLEM CATEGORY**, select the **Web App Down** button.</span></span>
1. <span data-ttu-id="782e6-303">W obszarze **sugerowane rozwiązania** > **włączyć przekierowywanie dziennika stdout**, wybierz przycisk, aby **otworzyć konsolę kudu, aby edytować plik Web. config**.</span><span class="sxs-lookup"><span data-stu-id="782e6-303">Under **Suggested Solutions** > **Enable Stdout Log Redirection**, select the button to **Open Kudu Console to edit Web.Config**.</span></span>
1. <span data-ttu-id="782e6-304">W **konsoli diagnostyki**kudu Otwórz foldery w **lokacji** ścieżki > **wwwroot**.</span><span class="sxs-lookup"><span data-stu-id="782e6-304">In the Kudu **Diagnostic Console**, open the folders to the path **site** > **wwwroot**.</span></span> <span data-ttu-id="782e6-305">Przewiń w dół, aby wyświetlić plik *Web. config* w dolnej części listy.</span><span class="sxs-lookup"><span data-stu-id="782e6-305">Scroll down to reveal the *web.config* file at the bottom of the list.</span></span>
1. <span data-ttu-id="782e6-306">Kliknij ikonę ołówka obok pliku *Web. config* .</span><span class="sxs-lookup"><span data-stu-id="782e6-306">Click the pencil icon next to the *web.config* file.</span></span>
1. <span data-ttu-id="782e6-307">Ustaw **stdoutLogEnabled** na `true` i zmień ścieżkę **stdoutLogFile** na: `\\?\%home%\LogFiles\stdout`.</span><span class="sxs-lookup"><span data-stu-id="782e6-307">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to: `\\?\%home%\LogFiles\stdout`.</span></span>
1. <span data-ttu-id="782e6-308">Wybierz pozycję **Zapisz** , aby zapisać zaktualizowany plik *Web. config* .</span><span class="sxs-lookup"><span data-stu-id="782e6-308">Select **Save** to save the updated *web.config* file.</span></span>
1. <span data-ttu-id="782e6-309">Wysłać żądanie do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="782e6-309">Make a request to the app.</span></span>
1. <span data-ttu-id="782e6-310">Wróć do witryny Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="782e6-310">Return to the Azure portal.</span></span> <span data-ttu-id="782e6-311">Wybierz blok **Narzędzia zaawansowane** w obszarze **Narzędzia programistyczne** .</span><span class="sxs-lookup"><span data-stu-id="782e6-311">Select the **Advanced Tools** blade in the **DEVELOPMENT TOOLS** area.</span></span> <span data-ttu-id="782e6-312">Wybierz przycisk **przejdź&rarr;** .</span><span class="sxs-lookup"><span data-stu-id="782e6-312">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="782e6-313">Konsola kudu otwiera się w nowej karcie lub oknie przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="782e6-313">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="782e6-314">Korzystając z paska nawigacyjnego w górnej części strony, Otwórz **konsolę debugowanie** i wybierz polecenie **cmd**.</span><span class="sxs-lookup"><span data-stu-id="782e6-314">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="782e6-315">Wybierz folder **LogFiles** .</span><span class="sxs-lookup"><span data-stu-id="782e6-315">Select the **LogFiles** folder.</span></span>
1. <span data-ttu-id="782e6-316">Sprawdź **zmodyfikowaną** kolumnę i wybierz ikonę ołówka, aby edytować dziennik stdout z datą ostatniej modyfikacji.</span><span class="sxs-lookup"><span data-stu-id="782e6-316">Inspect the **Modified** column and select the pencil icon to edit the stdout log with the latest modification date.</span></span>
1. <span data-ttu-id="782e6-317">Po otwarciu pliku dziennika zostanie wyświetlony komunikat o błędzie.</span><span class="sxs-lookup"><span data-stu-id="782e6-317">When the log file opens, the error is displayed.</span></span>

<span data-ttu-id="782e6-318">Wyłącz rejestrowanie stdout po zakończeniu rozwiązywania problemów:</span><span class="sxs-lookup"><span data-stu-id="782e6-318">Disable stdout logging when troubleshooting is complete:</span></span>

1. <span data-ttu-id="782e6-319">W **konsoli diagnostyki**kudu Wróć do **witryny** Path > **wwwroot** , aby wyświetlić plik *Web. config* .</span><span class="sxs-lookup"><span data-stu-id="782e6-319">In the Kudu **Diagnostic Console**, return to the path **site** > **wwwroot** to reveal the *web.config* file.</span></span> <span data-ttu-id="782e6-320">Otwórz plik **Web. config** ponownie, wybierając ikonę ołówka.</span><span class="sxs-lookup"><span data-stu-id="782e6-320">Open the **web.config** file again by selecting the pencil icon.</span></span>
1. <span data-ttu-id="782e6-321">Ustaw **stdoutLogEnabled** do `false`.</span><span class="sxs-lookup"><span data-stu-id="782e6-321">Set **stdoutLogEnabled** to `false`.</span></span>
1. <span data-ttu-id="782e6-322">Wybierz pozycję **Zapisz** , aby zapisać plik.</span><span class="sxs-lookup"><span data-stu-id="782e6-322">Select **Save** to save the file.</span></span>

<span data-ttu-id="782e6-323">Aby uzyskać więcej informacji, zobacz temat <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span><span class="sxs-lookup"><span data-stu-id="782e6-323">For more information, see <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span></span>

> [!WARNING]
> <span data-ttu-id="782e6-324">Nie można wyłączyć dziennika stdout może prowadzić do awarii aplikacji lub serwera.</span><span class="sxs-lookup"><span data-stu-id="782e6-324">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="782e6-325">Brak brak limitu rozmiaru pliku dziennika lub liczba pliki dziennika utworzone.</span><span class="sxs-lookup"><span data-stu-id="782e6-325">There's no limit on log file size or the number of log files created.</span></span> <span data-ttu-id="782e6-326">Rejestrowania stdout można używać tylko w celu rozwiązywania problemów z uruchamianiem aplikacji.</span><span class="sxs-lookup"><span data-stu-id="782e6-326">Only use stdout logging to troubleshoot app startup problems.</span></span>
>
> <span data-ttu-id="782e6-327">Aby uzyskać ogólne rejestrowanie w aplikacji ASP.NET Core po uruchomieniu, należy użyć biblioteki rejestrowania, która ogranicza rozmiar pliku dziennika i obraca dzienniki.</span><span class="sxs-lookup"><span data-stu-id="782e6-327">For general logging in an ASP.NET Core app after startup, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="782e6-328">Aby uzyskać więcej informacji, zobacz [rejestrowania innych dostawców](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="782e6-328">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

::: moniker range=">= aspnetcore-2.2"

### <a name="aspnet-core-module-debug-log-azure-app-service"></a><span data-ttu-id="782e6-329">Dziennik debugowania modułu ASP.NET Core (Azure App Service)</span><span class="sxs-lookup"><span data-stu-id="782e6-329">ASP.NET Core Module debug log (Azure App Service)</span></span>

<span data-ttu-id="782e6-330">Dziennik debugowania modułu ASP.NET Core zapewnia dodatkowe, dokładniejsze rejestrowanie z modułu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="782e6-330">The ASP.NET Core Module debug log provides additional, deeper logging from the ASP.NET Core Module.</span></span> <span data-ttu-id="782e6-331">Włączanie i wyświetlanie dzienników stdout:</span><span class="sxs-lookup"><span data-stu-id="782e6-331">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="782e6-332">Aby włączyć Rozszerzony Dziennik diagnostyczny, wykonaj jedną z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="782e6-332">To enable the enhanced diagnostic log, perform either of the following:</span></span>
   * <span data-ttu-id="782e6-333">Postępuj zgodnie z instrukcjami w temacie [udoskonalone dzienniki diagnostyczne](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) , aby skonfigurować aplikację do rozszerzonego rejestrowania diagnostycznego.</span><span class="sxs-lookup"><span data-stu-id="782e6-333">Follow the instructions in [Enhanced diagnostic logs](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) to configure the app for an enhanced diagnostic logging.</span></span> <span data-ttu-id="782e6-334">Wdróż ponownie aplikację.</span><span class="sxs-lookup"><span data-stu-id="782e6-334">Redeploy the app.</span></span>
   * <span data-ttu-id="782e6-335">Dodaj `<handlerSettings>` przedstawiony w [ulepszonych dziennikach diagnostycznych](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) do pliku *Web. config* aplikacji na żywo za pomocą konsoli kudu:</span><span class="sxs-lookup"><span data-stu-id="782e6-335">Add the `<handlerSettings>` shown in [Enhanced diagnostic logs](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) to the live app's *web.config* file using the Kudu console:</span></span>
     1. <span data-ttu-id="782e6-336">Otwórz **Narzędzia zaawansowane** w obszarze **Narzędzia programistyczne** .</span><span class="sxs-lookup"><span data-stu-id="782e6-336">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="782e6-337">Wybierz przycisk **przejdź&rarr;** .</span><span class="sxs-lookup"><span data-stu-id="782e6-337">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="782e6-338">Konsola kudu otwiera się w nowej karcie lub oknie przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="782e6-338">The Kudu console opens in a new browser tab or window.</span></span>
     1. <span data-ttu-id="782e6-339">Korzystając z paska nawigacyjnego w górnej części strony, Otwórz **konsolę debugowanie** i wybierz polecenie **cmd**.</span><span class="sxs-lookup"><span data-stu-id="782e6-339">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
     1. <span data-ttu-id="782e6-340">Otwórz foldery w **witrynie** Path > **wwwroot**.</span><span class="sxs-lookup"><span data-stu-id="782e6-340">Open the folders to the path **site** > **wwwroot**.</span></span> <span data-ttu-id="782e6-341">Edytuj plik *Web. config* , wybierając przycisk ołówka.</span><span class="sxs-lookup"><span data-stu-id="782e6-341">Edit the *web.config* file by selecting the pencil button.</span></span> <span data-ttu-id="782e6-342">Dodaj sekcję `<handlerSettings>`, jak pokazano w [udoskonalonych dziennikach diagnostycznych](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs).</span><span class="sxs-lookup"><span data-stu-id="782e6-342">Add the `<handlerSettings>` section as shown in [Enhanced diagnostic logs](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs).</span></span> <span data-ttu-id="782e6-343">Wybierz ikonę **Zapisz**.</span><span class="sxs-lookup"><span data-stu-id="782e6-343">Select the **Save** button.</span></span>
1. <span data-ttu-id="782e6-344">Otwórz **Narzędzia zaawansowane** w obszarze **Narzędzia programistyczne** .</span><span class="sxs-lookup"><span data-stu-id="782e6-344">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="782e6-345">Wybierz przycisk **przejdź&rarr;** .</span><span class="sxs-lookup"><span data-stu-id="782e6-345">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="782e6-346">Konsola kudu otwiera się w nowej karcie lub oknie przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="782e6-346">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="782e6-347">Korzystając z paska nawigacyjnego w górnej części strony, Otwórz **konsolę debugowanie** i wybierz polecenie **cmd**.</span><span class="sxs-lookup"><span data-stu-id="782e6-347">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="782e6-348">Otwórz foldery w **witrynie** Path > **wwwroot**.</span><span class="sxs-lookup"><span data-stu-id="782e6-348">Open the folders to the path **site** > **wwwroot**.</span></span> <span data-ttu-id="782e6-349">Jeśli nie podano ścieżki do pliku *aspnetcore-Debug. log* , plik zostanie wyświetlony na liście.</span><span class="sxs-lookup"><span data-stu-id="782e6-349">If you didn't supply a path for the *aspnetcore-debug.log* file, the file appears in the list.</span></span> <span data-ttu-id="782e6-350">Jeśli podano ścieżkę, przejdź do lokalizacji pliku dziennika.</span><span class="sxs-lookup"><span data-stu-id="782e6-350">If you supplied a path, navigate to the location of the log file.</span></span>
1. <span data-ttu-id="782e6-351">Otwórz plik dziennika z przyciskiem ołówek obok nazwy pliku.</span><span class="sxs-lookup"><span data-stu-id="782e6-351">Open the log file with the pencil button next to the file name.</span></span>

<span data-ttu-id="782e6-352">Wyłącz rejestrowanie debugowania po zakończeniu rozwiązywania problemów:</span><span class="sxs-lookup"><span data-stu-id="782e6-352">Disable debug logging when troubleshooting is complete:</span></span>

<span data-ttu-id="782e6-353">Aby wyłączyć rozszerzony Dziennik debugowania, wykonaj jedną z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="782e6-353">To disable the enhanced debug log, perform either of the following:</span></span>

* <span data-ttu-id="782e6-354">Usuń `<handlerSettings>` z pliku *Web. config* lokalnie i ponownie Wdróż aplikację.</span><span class="sxs-lookup"><span data-stu-id="782e6-354">Remove the `<handlerSettings>` from the *web.config* file locally and redeploy the app.</span></span>
* <span data-ttu-id="782e6-355">Za pomocą konsoli kudu Edytuj plik *Web. config* i usuń sekcję `<handlerSettings>`.</span><span class="sxs-lookup"><span data-stu-id="782e6-355">Use the Kudu console to edit the *web.config* file and remove the `<handlerSettings>` section.</span></span> <span data-ttu-id="782e6-356">Zapisz plik.</span><span class="sxs-lookup"><span data-stu-id="782e6-356">Save the file.</span></span>

<span data-ttu-id="782e6-357">Aby uzyskać więcej informacji, zobacz temat <xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs>.</span><span class="sxs-lookup"><span data-stu-id="782e6-357">For more information, see <xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs>.</span></span>

> [!WARNING]
> <span data-ttu-id="782e6-358">Niepowodzenie wyłączenia dziennika debugowania może prowadzić do awarii aplikacji lub serwera.</span><span class="sxs-lookup"><span data-stu-id="782e6-358">Failure to disable the debug log can lead to app or server failure.</span></span> <span data-ttu-id="782e6-359">Nie ma limitu rozmiaru pliku dziennika.</span><span class="sxs-lookup"><span data-stu-id="782e6-359">There's no limit on log file size.</span></span> <span data-ttu-id="782e6-360">Funkcja rejestrowania debugowania służy tylko do rozwiązywania problemów z uruchamianiem aplikacji.</span><span class="sxs-lookup"><span data-stu-id="782e6-360">Only use debug logging to troubleshoot app startup problems.</span></span>
>
> <span data-ttu-id="782e6-361">Aby uzyskać ogólne rejestrowanie w aplikacji ASP.NET Core po uruchomieniu, należy użyć biblioteki rejestrowania, która ogranicza rozmiar pliku dziennika i obraca dzienniki.</span><span class="sxs-lookup"><span data-stu-id="782e6-361">For general logging in an ASP.NET Core app after startup, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="782e6-362">Aby uzyskać więcej informacji, zobacz [rejestrowania innych dostawców](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="782e6-362">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

::: moniker-end

### <a name="slow-or-hanging-app-azure-app-service"></a><span data-ttu-id="782e6-363">Aplikacja wolna lub wysunięta (Azure App Service)</span><span class="sxs-lookup"><span data-stu-id="782e6-363">Slow or hanging app (Azure App Service)</span></span>

<span data-ttu-id="782e6-364">Gdy aplikacja reaguje powoli lub zawiesza się na żądanie, zobacz następujące artykuły:</span><span class="sxs-lookup"><span data-stu-id="782e6-364">When an app responds slowly or hangs on a request, see the following articles:</span></span>

* [<span data-ttu-id="782e6-365">Rozwiązywanie problemów związanych z niską wydajnością aplikacji internetowych w usłudze Azure App Service</span><span class="sxs-lookup"><span data-stu-id="782e6-365">Troubleshoot slow web app performance issues in Azure App Service</span></span>](/azure/app-service/app-service-web-troubleshoot-performance-degradation)
* [<span data-ttu-id="782e6-366">Użyj rozszerzenia witryny diagnostyki awarii, aby przechwycić zrzut dla sporadycznych problemów z wyjątkami lub problemów z wydajnością w usłudze Azure Web App</span><span class="sxs-lookup"><span data-stu-id="782e6-366">Use Crash Diagnoser Site Extension to Capture Dump for Intermittent Exception issues or performance issues on Azure Web App</span></span>](https://blogs.msdn.microsoft.com/asiatech/2015/12/28/use-crash-diagnoser-site-extension-to-capture-dump-for-intermittent-exception-issues-or-performance-issues-on-azure-web-app/)

### <a name="monitoring-blades"></a><span data-ttu-id="782e6-367">Bloki monitorowania</span><span class="sxs-lookup"><span data-stu-id="782e6-367">Monitoring blades</span></span>

<span data-ttu-id="782e6-368">Bloki monitorowania zapewniają alternatywne środowisko rozwiązywania problemów z metodami opisanymi wcześniej w temacie.</span><span class="sxs-lookup"><span data-stu-id="782e6-368">Monitoring blades provide an alternative troubleshooting experience to the methods described earlier in the topic.</span></span> <span data-ttu-id="782e6-369">Te bloki mogą służyć do diagnozowania błędów serii 500.</span><span class="sxs-lookup"><span data-stu-id="782e6-369">These blades can be used to diagnose 500-series errors.</span></span>

<span data-ttu-id="782e6-370">Upewnij się, że rozszerzenia ASP.NET Core są zainstalowane.</span><span class="sxs-lookup"><span data-stu-id="782e6-370">Confirm that the ASP.NET Core Extensions are installed.</span></span> <span data-ttu-id="782e6-371">Jeśli rozszerzenia nie są zainstalowane, zainstaluj je ręcznie:</span><span class="sxs-lookup"><span data-stu-id="782e6-371">If the extensions aren't installed, install them manually:</span></span>

1. <span data-ttu-id="782e6-372">W sekcji blok **narzędzi programistycznych** wybierz blok **rozszerzenia** .</span><span class="sxs-lookup"><span data-stu-id="782e6-372">In the **DEVELOPMENT TOOLS** blade section, select the **Extensions** blade.</span></span>
1. <span data-ttu-id="782e6-373">**Rozszerzenia ASP.NET Core** powinny znajdować się na liście.</span><span class="sxs-lookup"><span data-stu-id="782e6-373">The **ASP.NET Core Extensions** should appear in the list.</span></span>
1. <span data-ttu-id="782e6-374">Jeśli rozszerzenia nie są zainstalowane, wybierz przycisk **Dodaj** .</span><span class="sxs-lookup"><span data-stu-id="782e6-374">If the extensions aren't installed, select the **Add** button.</span></span>
1. <span data-ttu-id="782e6-375">Wybierz z listy **rozszerzenia ASP.NET Core** .</span><span class="sxs-lookup"><span data-stu-id="782e6-375">Choose the **ASP.NET Core Extensions** from the list.</span></span>
1. <span data-ttu-id="782e6-376">Wybierz **przycisk OK** , aby zaakceptować postanowienia prawne.</span><span class="sxs-lookup"><span data-stu-id="782e6-376">Select **OK** to accept the legal terms.</span></span>
1. <span data-ttu-id="782e6-377">W bloku **Dodaj rozszerzenie** wybierz pozycję **OK** .</span><span class="sxs-lookup"><span data-stu-id="782e6-377">Select **OK** on the **Add extension** blade.</span></span>
1. <span data-ttu-id="782e6-378">Komunikat podręczny informujący o pomyślnym zainstalowaniu rozszerzeń.</span><span class="sxs-lookup"><span data-stu-id="782e6-378">An informational pop-up message indicates when the extensions are successfully installed.</span></span>

<span data-ttu-id="782e6-379">Jeśli rejestrowanie stdout nie jest włączone, wykonaj następujące kroki:</span><span class="sxs-lookup"><span data-stu-id="782e6-379">If stdout logging isn't enabled, follow these steps:</span></span>

1. <span data-ttu-id="782e6-380">W Azure Portal wybierz blok **Narzędzia zaawansowane** w obszarze **Narzędzia programistyczne** .</span><span class="sxs-lookup"><span data-stu-id="782e6-380">In the Azure portal, select the **Advanced Tools** blade in the **DEVELOPMENT TOOLS** area.</span></span> <span data-ttu-id="782e6-381">Wybierz przycisk **przejdź&rarr;** .</span><span class="sxs-lookup"><span data-stu-id="782e6-381">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="782e6-382">Konsola kudu otwiera się w nowej karcie lub oknie przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="782e6-382">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="782e6-383">Korzystając z paska nawigacyjnego w górnej części strony, Otwórz **konsolę debugowanie** i wybierz polecenie **cmd**.</span><span class="sxs-lookup"><span data-stu-id="782e6-383">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="782e6-384">Otwórz foldery w **witrynie** Path > **wwwroot** i przewiń w dół, aby wyświetlić plik *Web. config* w dolnej części listy.</span><span class="sxs-lookup"><span data-stu-id="782e6-384">Open the folders to the path **site** > **wwwroot** and scroll down to reveal the *web.config* file at the bottom of the list.</span></span>
1. <span data-ttu-id="782e6-385">Kliknij ikonę ołówka obok pliku *Web. config* .</span><span class="sxs-lookup"><span data-stu-id="782e6-385">Click the pencil icon next to the *web.config* file.</span></span>
1. <span data-ttu-id="782e6-386">Ustaw **stdoutLogEnabled** na `true` i zmień ścieżkę **stdoutLogFile** na: `\\?\%home%\LogFiles\stdout`.</span><span class="sxs-lookup"><span data-stu-id="782e6-386">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to: `\\?\%home%\LogFiles\stdout`.</span></span>
1. <span data-ttu-id="782e6-387">Wybierz pozycję **Zapisz** , aby zapisać zaktualizowany plik *Web. config* .</span><span class="sxs-lookup"><span data-stu-id="782e6-387">Select **Save** to save the updated *web.config* file.</span></span>

<span data-ttu-id="782e6-388">Wykonaj aktywację rejestrowania diagnostycznego:</span><span class="sxs-lookup"><span data-stu-id="782e6-388">Proceed to activate diagnostic logging:</span></span>

1. <span data-ttu-id="782e6-389">W Azure Portal wybierz blok **dzienników diagnostycznych** .</span><span class="sxs-lookup"><span data-stu-id="782e6-389">In the Azure portal, select the **Diagnostics logs** blade.</span></span>
1. <span data-ttu-id="782e6-390">Wybierz pozycję **Włącz** , aby włączyć **Rejestrowanie aplikacji (system plików)** i **szczegółowe komunikaty o błędach**.</span><span class="sxs-lookup"><span data-stu-id="782e6-390">Select the **On** switch for **Application Logging (Filesystem)** and **Detailed error messages**.</span></span> <span data-ttu-id="782e6-391">Wybierz przycisk **Zapisz** znajdujący się u góry bloku.</span><span class="sxs-lookup"><span data-stu-id="782e6-391">Select the **Save** button at the top of the blade.</span></span>
1. <span data-ttu-id="782e6-392">Aby uwzględnić śledzenie nieudanych żądań, znane także jako rejestrowanie nieudanych żądań buforowania zdarzeń (FREB), wybierz **przełącznik dla** **śledzenia nieudanych żądań**.</span><span class="sxs-lookup"><span data-stu-id="782e6-392">To include failed request tracing, also known as Failed Request Event Buffering (FREB) logging, select the **On** switch for **Failed request tracing**.</span></span>
1. <span data-ttu-id="782e6-393">Wybierz blok **strumień dziennika** , który jest wyświetlany bezpośrednio w bloku **dzienników diagnostycznych** w portalu.</span><span class="sxs-lookup"><span data-stu-id="782e6-393">Select the **Log stream** blade, which is listed immediately under the **Diagnostics logs** blade in the portal.</span></span>
1. <span data-ttu-id="782e6-394">Wysłać żądanie do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="782e6-394">Make a request to the app.</span></span>
1. <span data-ttu-id="782e6-395">W danych strumienia dziennika jest wskazywana Przyczyna błędu.</span><span class="sxs-lookup"><span data-stu-id="782e6-395">Within the log stream data, the cause of the error is indicated.</span></span>

<span data-ttu-id="782e6-396">Należy pamiętać o wyłączeniu rejestrowania stdout po zakończeniu rozwiązywania problemów.</span><span class="sxs-lookup"><span data-stu-id="782e6-396">Be sure to disable stdout logging when troubleshooting is complete.</span></span>

<span data-ttu-id="782e6-397">Aby wyświetlić dzienniki śledzenia niepomyślnych żądań (dzienniki FREB):</span><span class="sxs-lookup"><span data-stu-id="782e6-397">To view the failed request tracing logs (FREB logs):</span></span>

1. <span data-ttu-id="782e6-398">Przejdź do bloku **diagnozowanie i rozwiązywanie problemów** w Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="782e6-398">Navigate to the **Diagnose and solve problems** blade in the Azure portal.</span></span>
1. <span data-ttu-id="782e6-399">Wybierz pozycję **dzienniki śledzenia niepomyślnych żądań** z obszaru **Narzędzia obsługi** na pasku bocznym.</span><span class="sxs-lookup"><span data-stu-id="782e6-399">Select **Failed Request Tracing Logs** from the **SUPPORT TOOLS** area of the sidebar.</span></span>

<span data-ttu-id="782e6-400">Zobacz [sekcję śledzenie niepomyślnych żądań w temacie Włączanie rejestrowania diagnostyki dla aplikacji sieci Web w programie Azure App Service](/azure/app-service/web-sites-enable-diagnostic-log#failed-request-traces) i informacje [o wydajności aplikacji dotyczące Web Apps na platformie Azure: Jak mogę włączyć śledzenie nieudanych żądań?](/azure/app-service/app-service-web-availability-performance-application-issues-faq#how-do-i-turn-on-failed-request-tracing) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="782e6-400">See [Failed request traces section of the Enable diagnostics logging for web apps in Azure App Service topic](/azure/app-service/web-sites-enable-diagnostic-log#failed-request-traces) and the [Application performance FAQs for Web Apps in Azure: How do I turn on failed request tracing?](/azure/app-service/app-service-web-availability-performance-application-issues-faq#how-do-i-turn-on-failed-request-tracing) for more information.</span></span>

<span data-ttu-id="782e6-401">Aby uzyskać więcej informacji, zobacz [Włączanie rejestrowania diagnostycznego dla aplikacji sieci Web w Azure App Service](/azure/app-service/web-sites-enable-diagnostic-log).</span><span class="sxs-lookup"><span data-stu-id="782e6-401">For more information, see [Enable diagnostics logging for web apps in Azure App Service](/azure/app-service/web-sites-enable-diagnostic-log).</span></span>

> [!WARNING]
> <span data-ttu-id="782e6-402">Nie można wyłączyć dziennika stdout może prowadzić do awarii aplikacji lub serwera.</span><span class="sxs-lookup"><span data-stu-id="782e6-402">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="782e6-403">Brak brak limitu rozmiaru pliku dziennika lub liczba pliki dziennika utworzone.</span><span class="sxs-lookup"><span data-stu-id="782e6-403">There's no limit on log file size or the number of log files created.</span></span>
>
> <span data-ttu-id="782e6-404">Rutynowe logujesz się w aplikacji ASP.NET Core, użytku bibliotekę rejestrowania, która ogranicza rozmiar pliku dziennika i obraca się loguje.</span><span class="sxs-lookup"><span data-stu-id="782e6-404">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="782e6-405">Aby uzyskać więcej informacji, zobacz [rejestrowania innych dostawców](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="782e6-405">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

## <a name="troubleshoot-on-iis"></a><span data-ttu-id="782e6-406">Rozwiązywanie problemów dotyczących usług IIS</span><span class="sxs-lookup"><span data-stu-id="782e6-406">Troubleshoot on IIS</span></span>

### <a name="application-event-log-iis"></a><span data-ttu-id="782e6-407">Dziennik zdarzeń aplikacji (IIS)</span><span class="sxs-lookup"><span data-stu-id="782e6-407">Application Event Log (IIS)</span></span>

<span data-ttu-id="782e6-408">Dostęp do dziennika zdarzeń aplikacji:</span><span class="sxs-lookup"><span data-stu-id="782e6-408">Access the Application Event Log:</span></span>

1. <span data-ttu-id="782e6-409">Otwórz menu Start, wyszukaj ciąg *Podgląd zdarzeń*i wybierz aplikację **Podgląd zdarzeń** .</span><span class="sxs-lookup"><span data-stu-id="782e6-409">Open the Start menu, search for *Event Viewer*, and select the **Event Viewer** app.</span></span>
1. <span data-ttu-id="782e6-410">W **Podgląd zdarzeń**, otwórz **Dzienniki Windows** węzła.</span><span class="sxs-lookup"><span data-stu-id="782e6-410">In **Event Viewer**, open the **Windows Logs** node.</span></span>
1. <span data-ttu-id="782e6-411">Wybierz **aplikacji** można otworzyć dziennika zdarzeń aplikacji.</span><span class="sxs-lookup"><span data-stu-id="782e6-411">Select **Application** to open the Application Event Log.</span></span>
1. <span data-ttu-id="782e6-412">Wyszukaj błędy skojarzone z aplikacją się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="782e6-412">Search for errors associated with the failing app.</span></span> <span data-ttu-id="782e6-413">Błędy mają wartość *moduł AspNetCore IIS* lub *usług IIS Express AspNetCore modułu* w *źródła* kolumny.</span><span class="sxs-lookup"><span data-stu-id="782e6-413">Errors have a value of *IIS AspNetCore Module* or *IIS Express AspNetCore Module* in the *Source* column.</span></span>

### <a name="run-the-app-at-a-command-prompt"></a><span data-ttu-id="782e6-414">Uruchamianie aplikacji w wierszu polecenia</span><span class="sxs-lookup"><span data-stu-id="782e6-414">Run the app at a command prompt</span></span>

<span data-ttu-id="782e6-415">Wiele błędów uruchamiania przestaną generować przydatne informacje w dzienniku zdarzeń aplikacji.</span><span class="sxs-lookup"><span data-stu-id="782e6-415">Many startup errors don't produce useful information in the Application Event Log.</span></span> <span data-ttu-id="782e6-416">Przyczyną niektórych błędów można znaleźć, uruchamiając aplikację w wierszu polecenia w systemie hostingu.</span><span class="sxs-lookup"><span data-stu-id="782e6-416">You can find the cause of some errors by running the app at a command prompt on the hosting system.</span></span>

#### <a name="framework-dependent-deployment"></a><span data-ttu-id="782e6-417">Wdrożenie zależny od struktury</span><span class="sxs-lookup"><span data-stu-id="782e6-417">Framework-dependent deployment</span></span>

<span data-ttu-id="782e6-418">Jeśli aplikacja jest [wdrożenia zależny od struktury](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span><span class="sxs-lookup"><span data-stu-id="782e6-418">If the app is a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>

1. <span data-ttu-id="782e6-419">W wierszu polecenia przejdź do folderu wdrożenia i uruchomienia aplikacji, wykonując zestaw aplikacji za pomocą *dotnet.exe*.</span><span class="sxs-lookup"><span data-stu-id="782e6-419">At a command prompt, navigate to the deployment folder and run the app by executing the app's assembly with *dotnet.exe*.</span></span> <span data-ttu-id="782e6-420">W poniższym poleceniu zastąp nazwę zestawu aplikacji dla \<assembly_name >: `dotnet .\<assembly_name>.dll`.</span><span class="sxs-lookup"><span data-stu-id="782e6-420">In the following command, substitute the name of the app's assembly for \<assembly_name>: `dotnet .\<assembly_name>.dll`.</span></span>
1. <span data-ttu-id="782e6-421">Dane wyjściowe z aplikacji, przedstawiający wszystkie błędy z konsoli są zapisywane w oknie konsoli.</span><span class="sxs-lookup"><span data-stu-id="782e6-421">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="782e6-422">Jeśli wystąpią błędy, gdy kieruje żądanie do aplikacji, należy wysłać żądanie do hosta i portu, na którym nasłuchuje Kestrel.</span><span class="sxs-lookup"><span data-stu-id="782e6-422">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="782e6-423">Przy użyciu domyślnego hosta i post, zgłosić wniosek o `http://localhost:5000/`.</span><span class="sxs-lookup"><span data-stu-id="782e6-423">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="782e6-424">Jeśli aplikacja reaguje, zwykle pod adresem punktu końcowego Kestrel, problem najprawdopodobniej związanych z konfiguracją hostingu i mniej prawdopodobne w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="782e6-424">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the hosting configuration and less likely within the app.</span></span>

#### <a name="self-contained-deployment"></a><span data-ttu-id="782e6-425">Niezależne wdrożenia</span><span class="sxs-lookup"><span data-stu-id="782e6-425">Self-contained deployment</span></span>

<span data-ttu-id="782e6-426">Jeśli aplikacja jest [niezależna wdrożenia](/dotnet/core/deploying/#self-contained-deployments-scd):</span><span class="sxs-lookup"><span data-stu-id="782e6-426">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>

1. <span data-ttu-id="782e6-427">W wierszu polecenia przejdź do folderu wdrożenia i uruchomienia pliku wykonywalnego aplikacji.</span><span class="sxs-lookup"><span data-stu-id="782e6-427">At a command prompt, navigate to the deployment folder and run the app's executable.</span></span> <span data-ttu-id="782e6-428">W poniższym poleceniu zastąp nazwę zestawu aplikacji dla \<assembly_name >: `<assembly_name>.exe`.</span><span class="sxs-lookup"><span data-stu-id="782e6-428">In the following command, substitute the name of the app's assembly for \<assembly_name>: `<assembly_name>.exe`.</span></span>
1. <span data-ttu-id="782e6-429">Dane wyjściowe z aplikacji, przedstawiający wszystkie błędy z konsoli są zapisywane w oknie konsoli.</span><span class="sxs-lookup"><span data-stu-id="782e6-429">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="782e6-430">Jeśli wystąpią błędy, gdy kieruje żądanie do aplikacji, należy wysłać żądanie do hosta i portu, na którym nasłuchuje Kestrel.</span><span class="sxs-lookup"><span data-stu-id="782e6-430">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="782e6-431">Przy użyciu domyślnego hosta i post, zgłosić wniosek o `http://localhost:5000/`.</span><span class="sxs-lookup"><span data-stu-id="782e6-431">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="782e6-432">Jeśli aplikacja reaguje, zwykle pod adresem punktu końcowego Kestrel, problem najprawdopodobniej związanych z konfiguracją hostingu i mniej prawdopodobne w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="782e6-432">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the hosting configuration and less likely within the app.</span></span>

### <a name="aspnet-core-module-stdout-log-iis"></a><span data-ttu-id="782e6-433">Dziennik stdout modułu ASP.NET Core (IIS)</span><span class="sxs-lookup"><span data-stu-id="782e6-433">ASP.NET Core Module stdout log (IIS)</span></span>

<span data-ttu-id="782e6-434">Włączanie i wyświetlanie dzienników stdout:</span><span class="sxs-lookup"><span data-stu-id="782e6-434">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="782e6-435">Przejdź do folderu wdrożenia witryny w systemie hostingu.</span><span class="sxs-lookup"><span data-stu-id="782e6-435">Navigate to the site's deployment folder on the hosting system.</span></span>
1. <span data-ttu-id="782e6-436">Jeśli *dzienniki* folder nie jest obecny, Utwórz folder.</span><span class="sxs-lookup"><span data-stu-id="782e6-436">If the *logs* folder isn't present, create the folder.</span></span> <span data-ttu-id="782e6-437">Aby uzyskać instrukcje dotyczące włączania MSBuild tworzenia *dzienniki* folderu we wdrożeniu automatycznie, zobacz [strukturę katalogów](xref:host-and-deploy/directory-structure) tematu.</span><span class="sxs-lookup"><span data-stu-id="782e6-437">For instructions on how to enable MSBuild to create the *logs* folder in the deployment automatically, see the [Directory structure](xref:host-and-deploy/directory-structure) topic.</span></span>
1. <span data-ttu-id="782e6-438">Edytuj *web.config* pliku.</span><span class="sxs-lookup"><span data-stu-id="782e6-438">Edit the *web.config* file.</span></span> <span data-ttu-id="782e6-439">Ustaw **stdoutLogEnabled** do `true` i zmień **stdoutLogFile** ścieżki, aby wskazywał *dzienniki* folderu (na przykład `.\logs\stdout`).</span><span class="sxs-lookup"><span data-stu-id="782e6-439">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to point to the *logs* folder (for example, `.\logs\stdout`).</span></span> <span data-ttu-id="782e6-440">`stdout` w ścieżce jest prefiks nazwy pliku dziennika.</span><span class="sxs-lookup"><span data-stu-id="782e6-440">`stdout` in the path is the log file name prefix.</span></span> <span data-ttu-id="782e6-441">Sygnatura czasowa, identyfikator procesu i rozszerzenie pliku są dodawane automatycznie, gdy zostanie utworzony dziennik.</span><span class="sxs-lookup"><span data-stu-id="782e6-441">A timestamp, process id, and file extension are added automatically when the log is created.</span></span> <span data-ttu-id="782e6-442">Za pomocą `stdout` jako prefiks nazwy pliku, plik dziennika typowe o nazwie *stdout_20180205184032_5412.log*.</span><span class="sxs-lookup"><span data-stu-id="782e6-442">Using `stdout` as the file name prefix, a typical log file is named *stdout_20180205184032_5412.log*.</span></span>
1. <span data-ttu-id="782e6-443">Upewnij się, tożsamość puli aplikacji ma uprawnienia do zapisu *dzienniki* folderu.</span><span class="sxs-lookup"><span data-stu-id="782e6-443">Ensure your application pool's identity has write permissions to the *logs* folder.</span></span>
1. <span data-ttu-id="782e6-444">Zapisz zaktualizowany *web.config* pliku.</span><span class="sxs-lookup"><span data-stu-id="782e6-444">Save the updated *web.config* file.</span></span>
1. <span data-ttu-id="782e6-445">Wysłać żądanie do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="782e6-445">Make a request to the app.</span></span>
1. <span data-ttu-id="782e6-446">Przejdź do *dzienniki* folderu.</span><span class="sxs-lookup"><span data-stu-id="782e6-446">Navigate to the *logs* folder.</span></span> <span data-ttu-id="782e6-447">Znajdowanie i otwieranie najnowszych dziennika stdout.</span><span class="sxs-lookup"><span data-stu-id="782e6-447">Find and open the most recent stdout log.</span></span>
1. <span data-ttu-id="782e6-448">Badanie w dzienniku błędów.</span><span class="sxs-lookup"><span data-stu-id="782e6-448">Study the log for errors.</span></span>

<span data-ttu-id="782e6-449">Wyłącz rejestrowanie stdout po zakończeniu rozwiązywania problemów:</span><span class="sxs-lookup"><span data-stu-id="782e6-449">Disable stdout logging when troubleshooting is complete:</span></span>

1. <span data-ttu-id="782e6-450">Edytuj *web.config* pliku.</span><span class="sxs-lookup"><span data-stu-id="782e6-450">Edit the *web.config* file.</span></span>
1. <span data-ttu-id="782e6-451">Ustaw **stdoutLogEnabled** do `false`.</span><span class="sxs-lookup"><span data-stu-id="782e6-451">Set **stdoutLogEnabled** to `false`.</span></span>
1. <span data-ttu-id="782e6-452">Zapisz plik.</span><span class="sxs-lookup"><span data-stu-id="782e6-452">Save the file.</span></span>

<span data-ttu-id="782e6-453">Aby uzyskać więcej informacji, zobacz temat <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span><span class="sxs-lookup"><span data-stu-id="782e6-453">For more information, see <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span></span>

> [!WARNING]
> <span data-ttu-id="782e6-454">Nie można wyłączyć dziennika stdout może prowadzić do awarii aplikacji lub serwera.</span><span class="sxs-lookup"><span data-stu-id="782e6-454">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="782e6-455">Brak brak limitu rozmiaru pliku dziennika lub liczba pliki dziennika utworzone.</span><span class="sxs-lookup"><span data-stu-id="782e6-455">There's no limit on log file size or the number of log files created.</span></span>
>
> <span data-ttu-id="782e6-456">Rutynowe logujesz się w aplikacji ASP.NET Core, użytku bibliotekę rejestrowania, która ogranicza rozmiar pliku dziennika i obraca się loguje.</span><span class="sxs-lookup"><span data-stu-id="782e6-456">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="782e6-457">Aby uzyskać więcej informacji, zobacz [rejestrowania innych dostawców](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="782e6-457">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

::: moniker range=">= aspnetcore-2.2"

### <a name="aspnet-core-module-debug-log-iis"></a><span data-ttu-id="782e6-458">Dziennik debugowania modułu ASP.NET Core (IIS)</span><span class="sxs-lookup"><span data-stu-id="782e6-458">ASP.NET Core Module debug log (IIS)</span></span>

<span data-ttu-id="782e6-459">Dodaj następujące ustawienia programu obsługi do pliku *Web. config* aplikacji, aby włączyć Dziennik debugowania modułu ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="782e6-459">Add the following handler settings to the app's *web.config* file to enable ASP.NET Core Module debug log:</span></span>

```xml
<aspNetCore ...>
  <handlerSettings>
    <handlerSetting name="debugLevel" value="file" />
    <handlerSetting name="debugFile" value="c:\temp\ancm.log" />
  </handlerSettings>
</aspNetCore>
```

<span data-ttu-id="782e6-460">Upewnij się, czy ścieżka określona dla dziennika istnieje i że tożsamość puli aplikacji ma uprawnienia do zapisu do lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="782e6-460">Confirm that the path specified for the log exists and that the app pool's identity has write permissions to the location.</span></span>

<span data-ttu-id="782e6-461">Aby uzyskać więcej informacji, zobacz temat <xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs>.</span><span class="sxs-lookup"><span data-stu-id="782e6-461">For more information, see <xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs>.</span></span>

::: moniker-end

### <a name="enable-the-developer-exception-page"></a><span data-ttu-id="782e6-462">Włącz na stronie wyjątków dla deweloperów</span><span class="sxs-lookup"><span data-stu-id="782e6-462">Enable the Developer Exception Page</span></span>

<span data-ttu-id="782e6-463">`ASPNETCORE_ENVIRONMENT` [Zmiennej środowiskowej, można dodać do pliku web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) do uruchomienia aplikacji w środowisku programistycznym.</span><span class="sxs-lookup"><span data-stu-id="782e6-463">The `ASPNETCORE_ENVIRONMENT` [environment variable can be added to web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) to run the app in the Development environment.</span></span> <span data-ttu-id="782e6-464">Tak długo, jak środowisko nie jest zastąpione przy uruchamianiu aplikacji przez `UseEnvironment` umożliwia ustawienie zmiennej środowiskowej w Konstruktorze hosta [stronie wyjątków deweloperów](xref:fundamentals/error-handling) się pojawiać po uruchomieniu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="782e6-464">As long as the environment isn't overridden in app startup by `UseEnvironment` on the host builder, setting the environment variable allows the [Developer Exception Page](xref:fundamentals/error-handling) to appear when the app is run.</span></span>

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

<span data-ttu-id="782e6-465">Ustawienie zmiennej środowiskowej, aby uzyskać `ASPNETCORE_ENVIRONMENT` jest zalecane tylko dla używane w przejściowym i testowania serwerów, które nie są połączone z Internetem.</span><span class="sxs-lookup"><span data-stu-id="782e6-465">Setting the environment variable for `ASPNETCORE_ENVIRONMENT` is only recommended for use on staging and testing servers that aren't exposed to the Internet.</span></span> <span data-ttu-id="782e6-466">Usuń zmienną środowiskową z *web.config* plik po rozwiązywania problemów.</span><span class="sxs-lookup"><span data-stu-id="782e6-466">Remove the environment variable from the *web.config* file after troubleshooting.</span></span> <span data-ttu-id="782e6-467">Aby uzyskać informacje na temat ustawiania zmiennych środowiskowych *web.config*, zobacz [environmentVariables element podrzędny elementu aspNetCore](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).</span><span class="sxs-lookup"><span data-stu-id="782e6-467">For information on setting environment variables in *web.config*, see [environmentVariables child element of aspNetCore](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).</span></span>

### <a name="obtain-data-from-an-app"></a><span data-ttu-id="782e6-468">Uzyskiwanie danych z aplikacji</span><span class="sxs-lookup"><span data-stu-id="782e6-468">Obtain data from an app</span></span>

<span data-ttu-id="782e6-469">Jeśli aplikacja jest w stanie odpowiadać na żądania, żądania, połączenia i dodatkowych danych można uzyskać z aplikację za pomocą oprogramowania pośredniczącego terminalu wbudowanego.</span><span class="sxs-lookup"><span data-stu-id="782e6-469">If an app is capable of responding to requests, obtain request, connection, and additional data from the app using terminal inline middleware.</span></span> <span data-ttu-id="782e6-470">Aby uzyskać więcej informacji i przykładowy kod, zobacz <xref:test/troubleshoot#obtain-data-from-an-app>.</span><span class="sxs-lookup"><span data-stu-id="782e6-470">For more information and sample code, see <xref:test/troubleshoot#obtain-data-from-an-app>.</span></span>

### <a name="slow-or-hanging-app-iis"></a><span data-ttu-id="782e6-471">Aplikacja wolna lub wysunięta (IIS)</span><span class="sxs-lookup"><span data-stu-id="782e6-471">Slow or hanging app (IIS)</span></span>

<span data-ttu-id="782e6-472">*Zrzut awaryjny* to migawka pamięci systemu, która może pomóc w ustaleniu przyczyny awarii aplikacji, awarii uruchamiania lub powolnej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="782e6-472">A *crash dump* is a snapshot of the system's memory and can help determine the cause of an app crash, startup failure, or slow app.</span></span>

#### <a name="app-crashes-or-encounters-an-exception"></a><span data-ttu-id="782e6-473">Awaria aplikacji lub napotka wyjątek</span><span class="sxs-lookup"><span data-stu-id="782e6-473">App crashes or encounters an exception</span></span>

<span data-ttu-id="782e6-474">Uzyskaj i Analizuj Zrzut z [raportowanie błędów systemu Windows (raportowanie błędów systemu Windows)](/windows/desktop/wer/windows-error-reporting):</span><span class="sxs-lookup"><span data-stu-id="782e6-474">Obtain and analyze a dump from [Windows Error Reporting (WER)](/windows/desktop/wer/windows-error-reporting):</span></span>

1. <span data-ttu-id="782e6-475">Utwórz folder do przechowywania plików zrzutu awaryjnego w `c:\dumps`.</span><span class="sxs-lookup"><span data-stu-id="782e6-475">Create a folder to hold crash dump files at `c:\dumps`.</span></span> <span data-ttu-id="782e6-476">Pula aplikacji musi mieć dostęp do zapisu w folderze.</span><span class="sxs-lookup"><span data-stu-id="782e6-476">The app pool must have write access to the folder.</span></span>
1. <span data-ttu-id="782e6-477">Uruchom [skrypt programu PowerShell](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/EnableDumps.ps1)w programie EnableDumps:</span><span class="sxs-lookup"><span data-stu-id="782e6-477">Run the [EnableDumps PowerShell script](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/EnableDumps.ps1):</span></span>
   * <span data-ttu-id="782e6-478">Jeśli aplikacja korzysta z [modelu hostingu w procesie](xref:host-and-deploy/iis/index#in-process-hosting-model), uruchom skrypt dla programu *w3wp. exe*:</span><span class="sxs-lookup"><span data-stu-id="782e6-478">If the app uses the [in-process hosting model](xref:host-and-deploy/iis/index#in-process-hosting-model), run the script for *w3wp.exe*:</span></span>

     ```console
     .\EnableDumps w3wp.exe c:\dumps
     ```

   * <span data-ttu-id="782e6-479">Jeśli aplikacja korzysta z [modelu hostingu poza procesem](xref:host-and-deploy/iis/index#out-of-process-hosting-model), uruchom skrypt dla programu *dotnet. exe*:</span><span class="sxs-lookup"><span data-stu-id="782e6-479">If the app uses the [out-of-process hosting model](xref:host-and-deploy/iis/index#out-of-process-hosting-model), run the script for *dotnet.exe*:</span></span>

     ```console
     .\EnableDumps dotnet.exe c:\dumps
     ```

1. <span data-ttu-id="782e6-480">Uruchom aplikację w warunkach, które powodują awarię.</span><span class="sxs-lookup"><span data-stu-id="782e6-480">Run the app under the conditions that cause the crash to occur.</span></span>
1. <span data-ttu-id="782e6-481">Po wystąpieniu awarii Uruchom [skrypt programu DisableDumps PowerShell](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/DisableDumps.ps1):</span><span class="sxs-lookup"><span data-stu-id="782e6-481">After the crash has occurred, run the [DisableDumps PowerShell script](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/DisableDumps.ps1):</span></span>
   * <span data-ttu-id="782e6-482">Jeśli aplikacja korzysta z [modelu hostingu w procesie](xref:host-and-deploy/iis/index#in-process-hosting-model), uruchom skrypt dla programu *w3wp. exe*:</span><span class="sxs-lookup"><span data-stu-id="782e6-482">If the app uses the [in-process hosting model](xref:host-and-deploy/iis/index#in-process-hosting-model), run the script for *w3wp.exe*:</span></span>

     ```console
     .\DisableDumps w3wp.exe
     ```

   * <span data-ttu-id="782e6-483">Jeśli aplikacja korzysta z [modelu hostingu poza procesem](xref:host-and-deploy/iis/index#out-of-process-hosting-model), uruchom skrypt dla programu *dotnet. exe*:</span><span class="sxs-lookup"><span data-stu-id="782e6-483">If the app uses the [out-of-process hosting model](xref:host-and-deploy/iis/index#out-of-process-hosting-model), run the script for *dotnet.exe*:</span></span>

     ```console
     .\DisableDumps dotnet.exe
     ```

<span data-ttu-id="782e6-484">Po awarii aplikacji i zakończeniu zbierania zrzutów aplikacja może zakończyć normalne działanie.</span><span class="sxs-lookup"><span data-stu-id="782e6-484">After an app crashes and dump collection is complete, the app is allowed to terminate normally.</span></span> <span data-ttu-id="782e6-485">Skrypt programu PowerShell konfiguruje raportowanie błędów systemu Windows w celu zebrania do pięciu zrzutów na aplikację.</span><span class="sxs-lookup"><span data-stu-id="782e6-485">The PowerShell script configures WER to collect up to five dumps per app.</span></span>

> [!WARNING]
> <span data-ttu-id="782e6-486">Zrzuty awaryjne mogą wymagać dużej ilości miejsca na dysku (do kilku gigabajtów).</span><span class="sxs-lookup"><span data-stu-id="782e6-486">Crash dumps might take up a large amount of disk space (up to several gigabytes each).</span></span>

#### <a name="app-hangs-fails-during-startup-or-runs-normally"></a><span data-ttu-id="782e6-487">Aplikacja zawiesza się, kończy się niepowodzeniem podczas uruchamiania lub działa normalnie</span><span class="sxs-lookup"><span data-stu-id="782e6-487">App hangs, fails during startup, or runs normally</span></span>

<span data-ttu-id="782e6-488">Gdy aplikacja *zawiesza* się (bez awarii), kończy się niepowodzeniem podczas uruchamiania lub działa normalnie, zobacz [pliki zrzutu w trybie użytkownika: wybór najlepszego narzędzia](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool) do wygenerowania zrzutu.</span><span class="sxs-lookup"><span data-stu-id="782e6-488">When an app *hangs* (stops responding but doesn't crash), fails during startup, or runs normally, see [User-Mode Dump Files: Choosing the Best Tool](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool) to select an appropriate tool to produce the dump.</span></span>

#### <a name="analyze-the-dump"></a><span data-ttu-id="782e6-489">Analizowanie zrzutu</span><span class="sxs-lookup"><span data-stu-id="782e6-489">Analyze the dump</span></span>

<span data-ttu-id="782e6-490">Zrzut można analizować przy użyciu kilku metod.</span><span class="sxs-lookup"><span data-stu-id="782e6-490">A dump can be analyzed using several approaches.</span></span> <span data-ttu-id="782e6-491">Aby uzyskać więcej informacji, zobacz [Analizowanie pliku zrzutu w trybie użytkownika](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file).</span><span class="sxs-lookup"><span data-stu-id="782e6-491">For more information, see [Analyzing a User-Mode Dump File](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file).</span></span>

## <a name="clear-package-caches"></a><span data-ttu-id="782e6-492">Wyczyść pamięć podręczną pakietów</span><span class="sxs-lookup"><span data-stu-id="782e6-492">Clear package caches</span></span>

<span data-ttu-id="782e6-493">Działająca aplikacja może zakończyć się niepowodzeniem natychmiast po uaktualnieniu zestaw .NET Core SDK na komputerze deweloperskim lub zmianie wersji pakietu w ramach aplikacji.</span><span class="sxs-lookup"><span data-stu-id="782e6-493">A functioning app may fail immediately after upgrading either the .NET Core SDK on the development machine or changing package versions within the app.</span></span> <span data-ttu-id="782e6-494">W niektórych przypadkach niespójne pakietów może spowodować uszkodzenie aplikacji podczas przeprowadzania uaktualnienia głównych.</span><span class="sxs-lookup"><span data-stu-id="782e6-494">In some cases, incoherent packages may break an app when performing major upgrades.</span></span> <span data-ttu-id="782e6-495">Większość z tych problemów można naprawić, wykonując następujące instrukcje:</span><span class="sxs-lookup"><span data-stu-id="782e6-495">Most of these issues can be fixed by following these instructions:</span></span>

1. <span data-ttu-id="782e6-496">Usuń *bin* i *obj* folderów.</span><span class="sxs-lookup"><span data-stu-id="782e6-496">Delete the *bin* and *obj* folders.</span></span>
1. <span data-ttu-id="782e6-497">Wyczyść pamięć podręczną pakietów, wykonując [wszystkie elementy lokalne usługi NuGet programu dotnet--Wyczyść](/dotnet/core/tools/dotnet-nuget-locals) z poziomu powłoki poleceń.</span><span class="sxs-lookup"><span data-stu-id="782e6-497">Clear the package caches by executing [dotnet nuget locals all --clear](/dotnet/core/tools/dotnet-nuget-locals) from a command shell.</span></span>

   <span data-ttu-id="782e6-498">Czyszczenie pamięci podręcznych pakietów można również wykonać za pomocą narzędzia [NuGet. exe](https://www.nuget.org/downloads) i wykonując polecenie `nuget locals all -clear`.</span><span class="sxs-lookup"><span data-stu-id="782e6-498">Clearing package caches can also be accomplished with the [nuget.exe](https://www.nuget.org/downloads) tool and executing the command `nuget locals all -clear`.</span></span> <span data-ttu-id="782e6-499">*nuget.exe* nie jest powiązane instalacji z pulpitu systemu operacyjnego Windows i należy uzyskać oddzielnie od [NuGet witryny sieci Web](https://www.nuget.org/downloads).</span><span class="sxs-lookup"><span data-stu-id="782e6-499">*nuget.exe* isn't a bundled install with the Windows desktop operating system and must be obtained separately from the [NuGet website](https://www.nuget.org/downloads).</span></span>

1. <span data-ttu-id="782e6-500">Przywróć i skompiluj ponownie projekt.</span><span class="sxs-lookup"><span data-stu-id="782e6-500">Restore and rebuild the project.</span></span>
1. <span data-ttu-id="782e6-501">Usuń wszystkie pliki z folderu wdrożenia na serwerze przed ponownym wdrożeniem aplikacji.</span><span class="sxs-lookup"><span data-stu-id="782e6-501">Delete all of the files in the deployment folder on the server prior to redeploying the app.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="782e6-502">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="782e6-502">Additional resources</span></span>

* <xref:test/troubleshoot>
* <xref:host-and-deploy/azure-iis-errors-reference>
* <xref:fundamentals/error-handling>
* <xref:host-and-deploy/aspnet-core-module>

### <a name="azure-documentation"></a><span data-ttu-id="782e6-503">Dokumentacja platformy Azure</span><span class="sxs-lookup"><span data-stu-id="782e6-503">Azure documentation</span></span>

* [<span data-ttu-id="782e6-504">Usługa Application Insights dla platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="782e6-504">Application Insights for ASP.NET Core</span></span>](/azure/application-insights/app-insights-asp-net-core)
* [<span data-ttu-id="782e6-505">Sekcja zdalne debugowanie aplikacji sieci Web Rozwiązywanie problemów z aplikacją sieci Web w Azure App Service przy użyciu programu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="782e6-505">Remote debugging web apps section of Troubleshoot a web app in Azure App Service using Visual Studio</span></span>](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio#remotedebug)
* [<span data-ttu-id="782e6-506">Omówienie diagnostyki Azure App Service</span><span class="sxs-lookup"><span data-stu-id="782e6-506">Azure App Service diagnostics overview</span></span>](/azure/app-service/app-service-diagnostics)
* [<span data-ttu-id="782e6-507">Jak monitorować aplikacje w usłudze Azure App Service</span><span class="sxs-lookup"><span data-stu-id="782e6-507">How to: Monitor Apps in Azure App Service</span></span>](/azure/app-service/web-sites-monitor)
* [<span data-ttu-id="782e6-508">Rozwiązywanie problemów z aplikacją sieci web w usłudze Azure App Service przy użyciu programu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="782e6-508">Troubleshoot a web app in Azure App Service using Visual Studio</span></span>](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio)
* [<span data-ttu-id="782e6-509">Rozwiązywanie problemów z błędami HTTP "502 złej Gateway" i "503 Usługa niedostępna" w usłudze Azure Web Apps</span><span class="sxs-lookup"><span data-stu-id="782e6-509">Troubleshoot HTTP errors of "502 bad gateway" and "503 service unavailable" in your Azure web apps</span></span>](/azure/app-service/app-service-web-troubleshoot-http-502-http-503)
* [<span data-ttu-id="782e6-510">Rozwiązywanie problemów związanych z niską wydajnością aplikacji internetowych w usłudze Azure App Service</span><span class="sxs-lookup"><span data-stu-id="782e6-510">Troubleshoot slow web app performance issues in Azure App Service</span></span>](/azure/app-service/app-service-web-troubleshoot-performance-degradation)
* [<span data-ttu-id="782e6-511">Często zadawane pytania dotyczące wydajności aplikacji dla Web Apps na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="782e6-511">Application performance FAQs for Web Apps in Azure</span></span>](/azure/app-service/app-service-web-availability-performance-application-issues-faq)
* [<span data-ttu-id="782e6-512">Piaskownica usługi Azure Web App (ograniczenia wykonywania App Service Runtime)</span><span class="sxs-lookup"><span data-stu-id="782e6-512">Azure Web App sandbox (App Service runtime execution limitations)</span></span>](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)
* [<span data-ttu-id="782e6-513">Piątek Azure: Azure App Service środowisko diagnostyczne i rozwiązywania problemów (wideo 12-minutowy)</span><span class="sxs-lookup"><span data-stu-id="782e6-513">Azure Friday: Azure App Service Diagnostic and Troubleshooting Experience (12-minute video)</span></span>](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)

### <a name="visual-studio-documentation"></a><span data-ttu-id="782e6-514">Dokumentacja programu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="782e6-514">Visual Studio documentation</span></span>

* [<span data-ttu-id="782e6-515">ASP.NET Core debugowania zdalnego na serwerze IIS na platformie Azure w programie Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="782e6-515">Remote Debug ASP.NET Core on IIS in Azure in Visual Studio 2017</span></span>](/visualstudio/debugger/remote-debugging-azure)
* [<span data-ttu-id="782e6-516">Zdalne debugowanie ASP.NET Core na zdalnym komputerze IIS w programie Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="782e6-516">Remote Debug ASP.NET Core on a Remote IIS Computer in Visual Studio 2017</span></span>](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer)
* [<span data-ttu-id="782e6-517">Naucz się debugować przy użyciu programu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="782e6-517">Learn to debug using Visual Studio</span></span>](/visualstudio/debugger/getting-started-with-the-debugger)

### <a name="visual-studio-code-documentation"></a><span data-ttu-id="782e6-518">Dokumentacja programu Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="782e6-518">Visual Studio Code documentation</span></span>

* [<span data-ttu-id="782e6-519">Debugowanie za pomocą programu Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="782e6-519">Debugging with Visual Studio Code</span></span>](https://code.visualstudio.com/docs/editor/debugging)
