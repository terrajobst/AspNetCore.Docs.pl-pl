---
title: "Praca z wieloma środowiska w programie ASP.NET Core"
author: rick-anderson
description: "Dowiedz się, jak platformy ASP.NET Core umożliwia kontrolowanie zachowania aplikacji w wielu środowiskach."
manager: wpickett
ms.author: riande
ms.date: 12/25/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/environments
ms.openlocfilehash: f2e074e1e19bb79453319c5b72e6c3872cd96ead
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/02/2018
---
# <a name="working-with-multiple-environments"></a><span data-ttu-id="4ac88-103">Praca w środowiskach wielu</span><span class="sxs-lookup"><span data-stu-id="4ac88-103">Working with multiple environments</span></span>

<span data-ttu-id="4ac88-104">przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="4ac88-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="4ac88-105">Platformy ASP.NET Core obsługuje ustawienie aplikacji w czasie wykonywania zmiennych środowiskowych.</span><span class="sxs-lookup"><span data-stu-id="4ac88-105">ASP.NET Core provides support for setting application behavior at runtime with environment variables.</span></span>

<span data-ttu-id="4ac88-106">[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="4ac88-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="environments"></a><span data-ttu-id="4ac88-107">Środowisk</span><span class="sxs-lookup"><span data-stu-id="4ac88-107">Environments</span></span>

<span data-ttu-id="4ac88-108">Zmienna środowiskowa odczytuje platformy ASP.NET Core `ASPNETCORE_ENVIRONMENT` przy uruchamianiu aplikacji i magazyny wartość w [IHostingEnvironment.EnvironmentName](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName).</span><span class="sxs-lookup"><span data-stu-id="4ac88-108">ASP.NET Core reads the environment variable `ASPNETCORE_ENVIRONMENT` at application startup and stores that value in [IHostingEnvironment.EnvironmentName](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName).</span></span> <span data-ttu-id="4ac88-109">`ASPNETCORE_ENVIRONMENT` można ustawić dowolną wartość, ale [trzy wartości](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname?view=aspnetcore-2.0) są obsługiwane przez platformę: [programowanie](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development?view=aspnetcore-2.0), [przemieszczania](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging?view=aspnetcore-2.0), i [produkcji](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production?view=aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="4ac88-109">`ASPNETCORE_ENVIRONMENT` can be set to any value, but [three values](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname?view=aspnetcore-2.0) are supported by the framework: [Development](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development?view=aspnetcore-2.0), [Staging](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging?view=aspnetcore-2.0), and [Production](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production?view=aspnetcore-2.0).</span></span> <span data-ttu-id="4ac88-110">Jeśli `ASPNETCORE_ENVIRONMENT` nie jest ustawiona, domyślnie zostanie użyta `Production`.</span><span class="sxs-lookup"><span data-stu-id="4ac88-110">If `ASPNETCORE_ENVIRONMENT` isn't set, it will default to `Production`.</span></span>

[!code-csharp[](environments/sample/WebApp1/Startup.cs?name=snippet)]

<span data-ttu-id="4ac88-111">Poprzedni kod:</span><span class="sxs-lookup"><span data-stu-id="4ac88-111">The preceding code:</span></span>

* <span data-ttu-id="4ac88-112">Wywołania [UseDeveloperExceptionPage](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_DeveloperExceptionPageExtensions_UseDeveloperExceptionPage_Microsoft_AspNetCore_Builder_IApplicationBuilder_) i [UseBrowserLink](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_BrowserLinkExtensions_UseBrowserLink_Microsoft_AspNetCore_Builder_IApplicationBuilder_) podczas `ASPNETCORE_ENVIRONMENT` ma ustawioną wartość `Development`.</span><span class="sxs-lookup"><span data-stu-id="4ac88-112">Calls [UseDeveloperExceptionPage](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_DeveloperExceptionPageExtensions_UseDeveloperExceptionPage_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [UseBrowserLink](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_BrowserLinkExtensions_UseBrowserLink_Microsoft_AspNetCore_Builder_IApplicationBuilder_) when `ASPNETCORE_ENVIRONMENT` is set to `Development`.</span></span>
* <span data-ttu-id="4ac88-113">Wywołania [UseExceptionHandler](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ExceptionHandlerExtensions_UseExceptionHandler_Microsoft_AspNetCore_Builder_IApplicationBuilder_) podczas wartość `ASPNETCORE_ENVIRONMENT` ustawiono jedną z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="4ac88-113">Calls [UseExceptionHandler](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ExceptionHandlerExtensions_UseExceptionHandler_Microsoft_AspNetCore_Builder_IApplicationBuilder_) when the value of `ASPNETCORE_ENVIRONMENT` is set one of the following:</span></span>

    * `Staging`
    * `Production`
    * `Staging_2`

<span data-ttu-id="4ac88-114">[Pomocnika Tag środowiska ](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) używa wartości `IHostingEnvironment.EnvironmentName` do dołączania lub wykluczania znaczników w elemencie:</span><span class="sxs-lookup"><span data-stu-id="4ac88-114">The [Environment Tag Helper ](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) uses the value of `IHostingEnvironment.EnvironmentName` to include or exclude markup in the element:</span></span>

[!code-html[](environments/sample/WebApp1/Pages/About.cshtml)]

<span data-ttu-id="4ac88-115">Uwaga: W systemach Windows i macOS zmienne środowiskowe i wartości nie są z uwzględnieniem wielkości liter.</span><span class="sxs-lookup"><span data-stu-id="4ac88-115">Note: On Windows and macOS, environment variables and values are not case sensitive.</span></span> <span data-ttu-id="4ac88-116">Zmienne środowiskowe systemu Linux i wartości są **z uwzględnieniem wielkości liter** domyślnie.</span><span class="sxs-lookup"><span data-stu-id="4ac88-116">Linux environment variables and values are **case sensitive** by default.</span></span>

### <a name="development"></a><span data-ttu-id="4ac88-117">Tworzenie</span><span class="sxs-lookup"><span data-stu-id="4ac88-117">Development</span></span>

<span data-ttu-id="4ac88-118">Środowisko projektowe można włączyć funkcje, które nie powinny być dostępne w środowisku produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="4ac88-118">The development environment can enable features that shouldn't be exposed in production.</span></span> <span data-ttu-id="4ac88-119">Na przykład włączyć szablonów platformy ASP.NET Core [developer wyjątku strony](xref:fundamentals/error-handling#the-developer-exception-page) w środowisku programistycznym.</span><span class="sxs-lookup"><span data-stu-id="4ac88-119">For example, the ASP.NET Core templates enable the [developer exception page](xref:fundamentals/error-handling#the-developer-exception-page) in the development environment.</span></span>

<span data-ttu-id="4ac88-120">Środowisko rozwoju lokalnych komputera można skonfigurować *Properties\launchSettings.json* pliku projektu.</span><span class="sxs-lookup"><span data-stu-id="4ac88-120">The environment for local machine development can be set in the *Properties\launchSettings.json* file of the project.</span></span> <span data-ttu-id="4ac88-121">Ustaw wartości środowiska *launchSettings.json* przesłaniają wartości w środowisku systemu.</span><span class="sxs-lookup"><span data-stu-id="4ac88-121">Environment values set in *launchSettings.json* override values set in the system environment.</span></span>

<span data-ttu-id="4ac88-122">Następujące JSON zawiera trzy profile z *launchSettings.json* pliku:</span><span class="sxs-lookup"><span data-stu-id="4ac88-122">The following JSON shows three profiles from a *launchSettings.json* file:</span></span>

[!code-json[](environments/sample/WebApp1/Properties/launchSettings.json?highlight=10,11,18,26)]

<span data-ttu-id="4ac88-123">Gdy aplikacja jest uruchamiana z [dotnet Uruchom](/dotnet/core/tools/dotnet-run), pierwszy profil z `"commandName": "Project"` będą używane.</span><span class="sxs-lookup"><span data-stu-id="4ac88-123">When the application is launched with [dotnet run](/dotnet/core/tools/dotnet-run), the first profile with `"commandName": "Project"` will be used.</span></span> <span data-ttu-id="4ac88-124">Wartość `commandName` Określa serwer sieci web do uruchomienia.</span><span class="sxs-lookup"><span data-stu-id="4ac88-124">The value of `commandName` specifies the web server to launch.</span></span> <span data-ttu-id="4ac88-125">`commandName` może to być jedna z:</span><span class="sxs-lookup"><span data-stu-id="4ac88-125">`commandName` can be one of :</span></span>

* <span data-ttu-id="4ac88-126">Usługi IIS Express</span><span class="sxs-lookup"><span data-stu-id="4ac88-126">IIS Express</span></span>
* <span data-ttu-id="4ac88-127">IIS</span><span class="sxs-lookup"><span data-stu-id="4ac88-127">IIS</span></span>
* <span data-ttu-id="4ac88-128">Projekt (który uruchamia Kestrel)</span><span class="sxs-lookup"><span data-stu-id="4ac88-128">Project (which launches Kestrel)</span></span>

<span data-ttu-id="4ac88-129">Gdy aplikacja jest uruchamiana z [dotnet Uruchom](/dotnet/core/tools/dotnet-run):</span><span class="sxs-lookup"><span data-stu-id="4ac88-129">When an app is launched with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>

* <span data-ttu-id="4ac88-130">*launchSettings.json* jest do odczytu. Jeśli jest dostępna.</span><span class="sxs-lookup"><span data-stu-id="4ac88-130">*launchSettings.json* is read if available.</span></span> <span data-ttu-id="4ac88-131">`environmentVariables` ustawienia w *launchSettings.json* zastąpienia zmiennych środowiskowych.</span><span class="sxs-lookup"><span data-stu-id="4ac88-131">`environmentVariables` settings in *launchSettings.json* override environment variables.</span></span>
* <span data-ttu-id="4ac88-132">Środowisko macierzyste są wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="4ac88-132">The hosting environment is displayed.</span></span>


<span data-ttu-id="4ac88-133">Następujące dane wyjściowe zawiera wprowadzenie do aplikacji [dotnet Uruchom](/dotnet/core/tools/dotnet-run):</span><span class="sxs-lookup"><span data-stu-id="4ac88-133">The following output shows an app started with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>
```bash
PS C:\Webs\WebApp1> dotnet run
Using launch settings from C:\Webs\WebApp1\Properties\launchSettings.json...
Hosting environment: Staging
Content root path: C:\Webs\WebApp1
Now listening on: http://localhost:54340
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="4ac88-134">Visual Studio **debugowania** karta zawiera graficzny interfejs użytkownika do edycji *launchSettings.json* pliku:</span><span class="sxs-lookup"><span data-stu-id="4ac88-134">The Visual Studio **Debug** tab provides a GUI to edit the *launchSettings.json* file:</span></span>

![Zmienne środowiskowe ustawienie właściwości projektu](environments/_static/project-properties-debug.png)

<span data-ttu-id="4ac88-136">Zmiany wprowadzone do profilów projektu mogą nie zostać zastosowane do czasu ponownego uruchomienia serwera sieci web.</span><span class="sxs-lookup"><span data-stu-id="4ac88-136">Changes made to project profiles may not take effect until the web server is restarted.</span></span> <span data-ttu-id="4ac88-137">Przed wykryje zmiany wprowadzone w jego środowisku, należy ponownie uruchomić kestrel.</span><span class="sxs-lookup"><span data-stu-id="4ac88-137">Kestrel must be restarted before it will detect changes made to its environment.</span></span>

>[!WARNING]
> <span data-ttu-id="4ac88-138">*launchSettings.json* nie należy przechowywać kluczy tajnych.</span><span class="sxs-lookup"><span data-stu-id="4ac88-138">*launchSettings.json* shouldn't store secrets.</span></span> <span data-ttu-id="4ac88-139">[Narzędzie Menedżer klucz tajny](xref:security/app-secrets) może służyć do przechowywania kluczy tajnych dla rozwoju lokalnych.</span><span class="sxs-lookup"><span data-stu-id="4ac88-139">The [Secret Manager tool](xref:security/app-secrets) can be used to store secrets for local development.</span></span>

### <a name="production"></a><span data-ttu-id="4ac88-140">Produkcji</span><span class="sxs-lookup"><span data-stu-id="4ac88-140">Production</span></span>

<span data-ttu-id="4ac88-141">Aby zmaksymalizować zabezpieczeń, wydajności i niezawodności aplikacji należy skonfigurować środowiska produkcyjnego.</span><span class="sxs-lookup"><span data-stu-id="4ac88-141">The production environment should be configured to maximize security, performance, and application robustness.</span></span> <span data-ttu-id="4ac88-142">Niektóre typowe ustawienia, które różnią się od rozwoju obejmują:</span><span class="sxs-lookup"><span data-stu-id="4ac88-142">Some common settings that differ from development include:</span></span>

* <span data-ttu-id="4ac88-143">Buforowanie.</span><span class="sxs-lookup"><span data-stu-id="4ac88-143">Caching.</span></span>
* <span data-ttu-id="4ac88-144">Zasoby po stronie klienta są powiązane, zminimalizowane i potencjalnie pochodzący z sieci CDN.</span><span class="sxs-lookup"><span data-stu-id="4ac88-144">Client-side resources are bundled, minified, and potentially served from a CDN.</span></span>
* <span data-ttu-id="4ac88-145">Strony błędów diagnostycznych wyłączone.</span><span class="sxs-lookup"><span data-stu-id="4ac88-145">Diagnostic error pages disabled.</span></span>
* <span data-ttu-id="4ac88-146">Strony błędów przyjazną włączone.</span><span class="sxs-lookup"><span data-stu-id="4ac88-146">Friendly error pages enabled.</span></span>
* <span data-ttu-id="4ac88-147">Rejestrowanie produkcyjnych i włączone monitorowanie.</span><span class="sxs-lookup"><span data-stu-id="4ac88-147">Production logging and monitoring enabled.</span></span> <span data-ttu-id="4ac88-148">Na przykład [usługi Application Insights](/azure/application-insights/app-insights-asp-net-core).</span><span class="sxs-lookup"><span data-stu-id="4ac88-148">For example, [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="setting-the-environment"></a><span data-ttu-id="4ac88-149">Ustawienia środowiska</span><span class="sxs-lookup"><span data-stu-id="4ac88-149">Setting the environment</span></span>

<span data-ttu-id="4ac88-150">Często jest to przydatne ustawić w określonym środowisku do testowania.</span><span class="sxs-lookup"><span data-stu-id="4ac88-150">It's often useful to set a specific environment for testing.</span></span> <span data-ttu-id="4ac88-151">Jeśli środowisko nie jest ustawiona, domyślnie zostanie użyta `Production` która wyłącza większość funkcji debugowania.</span><span class="sxs-lookup"><span data-stu-id="4ac88-151">If the environment isn't set, it will default to `Production` which disables most debugging features.</span></span>

<span data-ttu-id="4ac88-152">Metoda do ustawiania środowiska zależy od systemu operacyjnego.</span><span class="sxs-lookup"><span data-stu-id="4ac88-152">The method for setting the environment depends on the operating system.</span></span>

### <a name="azure"></a><span data-ttu-id="4ac88-153">Azure</span><span class="sxs-lookup"><span data-stu-id="4ac88-153">Azure</span></span>

<span data-ttu-id="4ac88-154">Usługi aplikacji Azure:</span><span class="sxs-lookup"><span data-stu-id="4ac88-154">For Azure app service:</span></span>

* <span data-ttu-id="4ac88-155">Wybierz **ustawienia aplikacji** bloku.</span><span class="sxs-lookup"><span data-stu-id="4ac88-155">Select the **Application settings** blade.</span></span>
* <span data-ttu-id="4ac88-156">Dodaj klucz i wartość w **ustawień aplikacji**.</span><span class="sxs-lookup"><span data-stu-id="4ac88-156">Add the key and value in **App settings**.</span></span>


### <a name="windows"></a><span data-ttu-id="4ac88-157">Windows</span><span class="sxs-lookup"><span data-stu-id="4ac88-157">Windows</span></span>
<span data-ttu-id="4ac88-158">Aby ustawić `ASPNETCORE_ENVIRONMENT` dla bieżącej sesji, jeśli aplikacja zostanie uruchomiona przy użyciu [dotnet Uruchom](/dotnet/core/tools/dotnet-run), są używane następujące polecenia</span><span class="sxs-lookup"><span data-stu-id="4ac88-158">To set the `ASPNETCORE_ENVIRONMENT` for the current session, if the app is started using [dotnet run](/dotnet/core/tools/dotnet-run), the following commands are used</span></span>

<span data-ttu-id="4ac88-159">**Wiersz polecenia**</span><span class="sxs-lookup"><span data-stu-id="4ac88-159">**Command line**</span></span>
```
set ASPNETCORE_ENVIRONMENT=Development
```
<span data-ttu-id="4ac88-160">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="4ac88-160">**PowerShell**</span></span>
```
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

<span data-ttu-id="4ac88-161">Te polecenia brane pod uwagę tylko dla bieżącego okna.</span><span class="sxs-lookup"><span data-stu-id="4ac88-161">These commands take effect only for the current window.</span></span> <span data-ttu-id="4ac88-162">Po zamknięciu okna ustawienie ASPNETCORE_ENVIRONMENT Przywraca wartości domyślne ustawienie lub komputera.</span><span class="sxs-lookup"><span data-stu-id="4ac88-162">When the window is closed, the ASPNETCORE_ENVIRONMENT setting reverts to the default setting or machine value.</span></span> <span data-ttu-id="4ac88-163">Aby można było ustawić wartość globalnie dla systemu Windows otwórz **Panelu sterowania** > **systemu** > **Zaawansowane ustawienia systemu** i Dodaj lub Edytuj `ASPNETCORE_ENVIRONMENT` wartość.</span><span class="sxs-lookup"><span data-stu-id="4ac88-163">In order to set the value globally on Windows open the **Control Panel** > **System** > **Advanced system settings** and add or edit the `ASPNETCORE_ENVIRONMENT` value.</span></span>

![Zaawansowane właściwości systemu](environments/_static/systemsetting_environment.png)

![Zmienna środowiskowa Core ASPNET](environments/_static/windows_aspnetcore_environment.png)


<span data-ttu-id="4ac88-166">**web.config**</span><span class="sxs-lookup"><span data-stu-id="4ac88-166">**web.config**</span></span>

<span data-ttu-id="4ac88-167">Zobacz *ustawienia zmiennych środowiskowych* sekcji [odwołania konfiguracji platformy ASP.NET Core modułu](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) tematu.</span><span class="sxs-lookup"><span data-stu-id="4ac88-167">See the *Setting environment variables* section of the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) topic.</span></span>

<span data-ttu-id="4ac88-168">**Dla każdej puli aplikacji usług IIS**</span><span class="sxs-lookup"><span data-stu-id="4ac88-168">**Per IIS Application Pool**</span></span>

<span data-ttu-id="4ac88-169">Aby ustawić zmienne środowiskowe dla poszczególnych aplikacji uruchomionych w izolowanej pulach aplikacji (obsługiwane w usługach IIS 10.0 +), zobacz *polecenia AppCmd.exe* sekcji [zmiennych środowiskowych \< environmentVariables >](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) tematu.</span><span class="sxs-lookup"><span data-stu-id="4ac88-169">To set environment variables for individual apps running in isolated Application Pools (supported on IIS 10.0+), see the *AppCmd.exe command* section of the [Environment Variables \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic.</span></span>

### <a name="macos"></a><span data-ttu-id="4ac88-170">macOS</span><span class="sxs-lookup"><span data-stu-id="4ac88-170">macOS</span></span>
<span data-ttu-id="4ac88-171">Ustawianie bieżącego środowiska pod kątem macOS może odbywać się w wierszu przy uruchamianiu aplikacji;</span><span class="sxs-lookup"><span data-stu-id="4ac88-171">Setting the current environment for macOS can be done in-line when running the application;</span></span>

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```
<span data-ttu-id="4ac88-172">lub przy użyciu `export` ustaw go przed uruchomieniem aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4ac88-172">or using `export` to set it prior to running the app.</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```
<span data-ttu-id="4ac88-173">Zmienne środowiskowe poziomu są ustawiane w *.bashrc* lub *.bash_profile* pliku.</span><span class="sxs-lookup"><span data-stu-id="4ac88-173">Machine level environment variables are set in the *.bashrc* or *.bash_profile* file.</span></span> <span data-ttu-id="4ac88-174">Przeprowadź edycję pliku za pomocą dowolnego edytora tekstu, a następnie dodaj następującą instrukcję.</span><span class="sxs-lookup"><span data-stu-id="4ac88-174">Edit the file using any text editor and add the following statment.</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

### <a name="linux"></a><span data-ttu-id="4ac88-175">Linux</span><span class="sxs-lookup"><span data-stu-id="4ac88-175">Linux</span></span>
<span data-ttu-id="4ac88-176">Dystrybucjach systemu Linux, można użyć `export` polecenie w wierszu polecenia dla zmiennej ustawieniami opartymi na sesji i *bash_profile* w pliku ustawień z poziomu środowiska maszyny.</span><span class="sxs-lookup"><span data-stu-id="4ac88-176">For Linux distros, use the `export` command at the command line for session based variable settings and *bash_profile* file for machine level environment settings.</span></span>

### <a name="configuration-by-environment"></a><span data-ttu-id="4ac88-177">Konfiguracja przez środowisko</span><span class="sxs-lookup"><span data-stu-id="4ac88-177">Configuration by environment</span></span>

<span data-ttu-id="4ac88-178">Zobacz [konfiguracji przez środowisko](xref:fundamentals/configuration/index#configuration-by-environment) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="4ac88-178">See [Configuration by environment](xref:fundamentals/configuration/index#configuration-by-environment) for more information.</span></span>

<a name="startup-conventions"></a>
## <a name="environment-based-startup-class-and-methods"></a><span data-ttu-id="4ac88-179">Klasa początkowa i metod opartych na środowisku</span><span class="sxs-lookup"><span data-stu-id="4ac88-179">Environment based Startup class and methods</span></span>

<span data-ttu-id="4ac88-180">Po uruchomieniu aplikacji platformy ASP.NET Core [Klasa początkowa](xref:fundamentals/startup) używa do ładowania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4ac88-180">When an ASP.NET Core app starts, the [Startup class](xref:fundamentals/startup) bootstraps the app.</span></span> <span data-ttu-id="4ac88-181">Jeśli klasa `Startup{EnvironmentName}` istnieje, że klasa zostanie wywołana dla tej `EnvironmentName`:</span><span class="sxs-lookup"><span data-stu-id="4ac88-181">If a class `Startup{EnvironmentName}` exists, that class will be called for that `EnvironmentName`:</span></span>

[!code-csharp[](environments/sample/WebApp1/StartupDev.cs?name=snippet&highlight=1)]

<span data-ttu-id="4ac88-182">Uwaga: Wywołanie [WebHostBuilder.UseStartup<TStartup> ](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) zastępuje sekcji konfiguracyjnych.</span><span class="sxs-lookup"><span data-stu-id="4ac88-182">Note: Calling [WebHostBuilder.UseStartup<TStartup>](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) overrides configuration sections.</span></span>

<span data-ttu-id="4ac88-183">[Skonfiguruj](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_StartupBase_Configure_Microsoft_AspNetCore_Builder_IApplicationBuilder_) i [ConfigureServices](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices?view=aspnetcore-2.0) obsługuje środowisko określonych wersji formularza `Configure{EnvironmentName}` i `Configure{EnvironmentName}Services`:</span><span class="sxs-lookup"><span data-stu-id="4ac88-183">[Configure](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_StartupBase_Configure_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [ConfigureServices](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices?view=aspnetcore-2.0) support environment specific versions of the form `Configure{EnvironmentName}` and `Configure{EnvironmentName}Services`:</span></span>

[!code-csharp[](environments/sample/WebApp1/Startup.cs?name=snippet_all&highlight=15,37)]

## <a name="additional-resources"></a><span data-ttu-id="4ac88-184">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="4ac88-184">Additional resources</span></span>

* [<span data-ttu-id="4ac88-185">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="4ac88-185">Application startup</span></span>](xref:fundamentals/startup)
* [<span data-ttu-id="4ac88-186">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="4ac88-186">Configuration</span></span>](xref:fundamentals/configuration/index)
* [<span data-ttu-id="4ac88-187">IHostingEnvironment.EnvironmentName</span><span class="sxs-lookup"><span data-stu-id="4ac88-187">IHostingEnvironment.EnvironmentName</span></span>](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName)
