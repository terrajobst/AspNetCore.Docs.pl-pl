---
title: Użyj wiele środowisk w ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak platformy ASP.NET Core umożliwia kontrolowanie zachowania aplikacji w wielu środowiskach.
ms.author: riande
ms.date: 12/25/2017
uid: fundamentals/environments
ms.openlocfilehash: 5a4caeeba045cb93dec9c73c931dae8a352bede9
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276935"
---
# <a name="use-multiple-environments-in-aspnet-core"></a><span data-ttu-id="1acd6-103">Użyj wiele środowisk w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1acd6-103">Use multiple environments in ASP.NET Core</span></span>

<span data-ttu-id="1acd6-104">przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="1acd6-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="1acd6-105">Platformy ASP.NET Core obsługuje ustawienie aplikacji w czasie wykonywania zmiennych środowiskowych.</span><span class="sxs-lookup"><span data-stu-id="1acd6-105">ASP.NET Core provides support for setting application behavior at runtime with environment variables.</span></span>

<span data-ttu-id="1acd6-106">[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="1acd6-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="environments"></a><span data-ttu-id="1acd6-107">Środowisk</span><span class="sxs-lookup"><span data-stu-id="1acd6-107">Environments</span></span>

<span data-ttu-id="1acd6-108">Zmienna środowiskowa odczytuje platformy ASP.NET Core `ASPNETCORE_ENVIRONMENT` przy uruchamianiu aplikacji i magazyny wartość w [IHostingEnvironment.EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName).</span><span class="sxs-lookup"><span data-stu-id="1acd6-108">ASP.NET Core reads the environment variable `ASPNETCORE_ENVIRONMENT` at application startup and stores that value in [IHostingEnvironment.EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName).</span></span> <span data-ttu-id="1acd6-109">`ASPNETCORE_ENVIRONMENT` można ustawić dowolną wartość, ale [trzy wartości](/dotnet/api/microsoft.aspnetcore.hosting.environmentname?view=aspnetcore-2.0) są obsługiwane przez platformę: [programowanie](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development?view=aspnetcore-2.0), [przemieszczania](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging?view=aspnetcore-2.0), i [produkcji](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production?view=aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="1acd6-109">`ASPNETCORE_ENVIRONMENT` can be set to any value, but [three values](/dotnet/api/microsoft.aspnetcore.hosting.environmentname?view=aspnetcore-2.0) are supported by the framework: [Development](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development?view=aspnetcore-2.0), [Staging](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging?view=aspnetcore-2.0), and [Production](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production?view=aspnetcore-2.0).</span></span> <span data-ttu-id="1acd6-110">Jeśli `ASPNETCORE_ENVIRONMENT` nie jest ustawiona, domyślnie zostanie użyta `Production`.</span><span class="sxs-lookup"><span data-stu-id="1acd6-110">If `ASPNETCORE_ENVIRONMENT` isn't set, it will default to `Production`.</span></span>

[!code-csharp[](environments/sample/WebApp1/Startup.cs?name=snippet)]

<span data-ttu-id="1acd6-111">Poprzedni kod:</span><span class="sxs-lookup"><span data-stu-id="1acd6-111">The preceding code:</span></span>

* <span data-ttu-id="1acd6-112">Wywołania [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_DeveloperExceptionPageExtensions_UseDeveloperExceptionPage_Microsoft_AspNetCore_Builder_IApplicationBuilder_) i [UseBrowserLink](/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_BrowserLinkExtensions_UseBrowserLink_Microsoft_AspNetCore_Builder_IApplicationBuilder_) podczas `ASPNETCORE_ENVIRONMENT` ma ustawioną wartość `Development`.</span><span class="sxs-lookup"><span data-stu-id="1acd6-112">Calls [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_DeveloperExceptionPageExtensions_UseDeveloperExceptionPage_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [UseBrowserLink](/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_BrowserLinkExtensions_UseBrowserLink_Microsoft_AspNetCore_Builder_IApplicationBuilder_) when `ASPNETCORE_ENVIRONMENT` is set to `Development`.</span></span>
* <span data-ttu-id="1acd6-113">Wywołania [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ExceptionHandlerExtensions_UseExceptionHandler_Microsoft_AspNetCore_Builder_IApplicationBuilder_) podczas wartość `ASPNETCORE_ENVIRONMENT` ustawiono jedną z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="1acd6-113">Calls [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ExceptionHandlerExtensions_UseExceptionHandler_Microsoft_AspNetCore_Builder_IApplicationBuilder_) when the value of `ASPNETCORE_ENVIRONMENT` is set one of the following:</span></span>

    * `Staging`
    * `Production`
    * `Staging_2`

<span data-ttu-id="1acd6-114">[Pomocnika Tag środowiska ](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) używa wartości `IHostingEnvironment.EnvironmentName` do dołączania lub wykluczania znaczników w elemencie:</span><span class="sxs-lookup"><span data-stu-id="1acd6-114">The [Environment Tag Helper ](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) uses the value of `IHostingEnvironment.EnvironmentName` to include or exclude markup in the element:</span></span>

[!code-html[](environments/sample/WebApp1/Pages/About.cshtml)]

<span data-ttu-id="1acd6-115">Uwaga: W systemach Windows i macOS zmienne środowiskowe i wartości nie są z uwzględnieniem wielkości liter.</span><span class="sxs-lookup"><span data-stu-id="1acd6-115">Note: On Windows and macOS, environment variables and values are not case sensitive.</span></span> <span data-ttu-id="1acd6-116">Zmienne środowiskowe systemu Linux i wartości są **z uwzględnieniem wielkości liter** domyślnie.</span><span class="sxs-lookup"><span data-stu-id="1acd6-116">Linux environment variables and values are **case sensitive** by default.</span></span>

### <a name="development"></a><span data-ttu-id="1acd6-117">Tworzenie</span><span class="sxs-lookup"><span data-stu-id="1acd6-117">Development</span></span>

<span data-ttu-id="1acd6-118">Środowisko projektowe można włączyć funkcje, które nie powinny być dostępne w środowisku produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="1acd6-118">The development environment can enable features that shouldn't be exposed in production.</span></span> <span data-ttu-id="1acd6-119">Na przykład włączyć szablonów platformy ASP.NET Core [developer wyjątku strony](xref:fundamentals/error-handling#the-developer-exception-page) w środowisku programistycznym.</span><span class="sxs-lookup"><span data-stu-id="1acd6-119">For example, the ASP.NET Core templates enable the [developer exception page](xref:fundamentals/error-handling#the-developer-exception-page) in the development environment.</span></span>

<span data-ttu-id="1acd6-120">Środowisko rozwoju lokalnych komputera można skonfigurować *Properties\launchSettings.json* pliku projektu.</span><span class="sxs-lookup"><span data-stu-id="1acd6-120">The environment for local machine development can be set in the *Properties\launchSettings.json* file of the project.</span></span> <span data-ttu-id="1acd6-121">Ustaw wartości środowiska *launchSettings.json* przesłaniają wartości w środowisku systemu.</span><span class="sxs-lookup"><span data-stu-id="1acd6-121">Environment values set in *launchSettings.json* override values set in the system environment.</span></span>

<span data-ttu-id="1acd6-122">Następujące JSON zawiera trzy profile z *launchSettings.json* pliku:</span><span class="sxs-lookup"><span data-stu-id="1acd6-122">The following JSON shows three profiles from a *launchSettings.json* file:</span></span>

[!code-json[](environments/sample/WebApp1/Properties/launchSettings.json?highlight=10,11,18,26)]

::: moniker range=">= aspnetcore-2.1"
> [!NOTE]
> <span data-ttu-id="1acd6-123">`applicationUrl` Właściwości w *launchSettings.json* można określić listę adresów URL serwera.</span><span class="sxs-lookup"><span data-stu-id="1acd6-123">The `applicationUrl` property in *launchSettings.json* can specify a list of server URLs.</span></span> <span data-ttu-id="1acd6-124">Użyj średnika między adresami URL na liście:</span><span class="sxs-lookup"><span data-stu-id="1acd6-124">Use a semicolon between the URLs in the list:</span></span>
>
> ```json
> "WebApplication1": {
>    "commandName": "Project",
>    "launchBrowser": true,
>    "applicationUrl": "https://localhost:5001;http://localhost:5000",
>    "environmentVariables": {
>      "ASPNETCORE_ENVIRONMENT": "Development"
>    }
> }
> ```
::: moniker-end

<span data-ttu-id="1acd6-125">Gdy aplikacja jest uruchamiana z [dotnet Uruchom](/dotnet/core/tools/dotnet-run), pierwszy profil z `"commandName": "Project"` będą używane.</span><span class="sxs-lookup"><span data-stu-id="1acd6-125">When the application is launched with [dotnet run](/dotnet/core/tools/dotnet-run), the first profile with `"commandName": "Project"` will be used.</span></span> <span data-ttu-id="1acd6-126">Wartość `commandName` Określa serwer sieci web do uruchomienia.</span><span class="sxs-lookup"><span data-stu-id="1acd6-126">The value of `commandName` specifies the web server to launch.</span></span> <span data-ttu-id="1acd6-127">`commandName` może to być jedna z:</span><span class="sxs-lookup"><span data-stu-id="1acd6-127">`commandName` can be one of :</span></span>

* <span data-ttu-id="1acd6-128">IIS Express</span><span class="sxs-lookup"><span data-stu-id="1acd6-128">IIS Express</span></span>
* <span data-ttu-id="1acd6-129">IIS</span><span class="sxs-lookup"><span data-stu-id="1acd6-129">IIS</span></span>
* <span data-ttu-id="1acd6-130">Projekt (który uruchamia Kestrel)</span><span class="sxs-lookup"><span data-stu-id="1acd6-130">Project (which launches Kestrel)</span></span>

<span data-ttu-id="1acd6-131">Gdy aplikacja jest uruchamiana z [dotnet Uruchom](/dotnet/core/tools/dotnet-run):</span><span class="sxs-lookup"><span data-stu-id="1acd6-131">When an app is launched with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>

* <span data-ttu-id="1acd6-132">*launchSettings.json* jest do odczytu. Jeśli jest dostępna.</span><span class="sxs-lookup"><span data-stu-id="1acd6-132">*launchSettings.json* is read if available.</span></span> <span data-ttu-id="1acd6-133">`environmentVariables` ustawienia w *launchSettings.json* zastąpienia zmiennych środowiskowych.</span><span class="sxs-lookup"><span data-stu-id="1acd6-133">`environmentVariables` settings in *launchSettings.json* override environment variables.</span></span>
* <span data-ttu-id="1acd6-134">Środowisko macierzyste są wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="1acd6-134">The hosting environment is displayed.</span></span>

<span data-ttu-id="1acd6-135">Następujące dane wyjściowe zawiera wprowadzenie do aplikacji [dotnet Uruchom](/dotnet/core/tools/dotnet-run):</span><span class="sxs-lookup"><span data-stu-id="1acd6-135">The following output shows an app started with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>

```bash
PS C:\Webs\WebApp1> dotnet run
Using launch settings from C:\Webs\WebApp1\Properties\launchSettings.json...
Hosting environment: Staging
Content root path: C:\Webs\WebApp1
Now listening on: http://localhost:54340
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="1acd6-136">Visual Studio **debugowania** karta zawiera graficzny interfejs użytkownika do edycji *launchSettings.json* pliku:</span><span class="sxs-lookup"><span data-stu-id="1acd6-136">The Visual Studio **Debug** tab provides a GUI to edit the *launchSettings.json* file:</span></span>

![Zmienne środowiskowe ustawienie właściwości projektu](environments/_static/project-properties-debug.png)

<span data-ttu-id="1acd6-138">Zmiany wprowadzone do profilów projektu mogą nie zostać zastosowane do czasu ponownego uruchomienia serwera sieci web.</span><span class="sxs-lookup"><span data-stu-id="1acd6-138">Changes made to project profiles may not take effect until the web server is restarted.</span></span> <span data-ttu-id="1acd6-139">Przed wykryje zmiany wprowadzone w jego środowisku, należy ponownie uruchomić kestrel.</span><span class="sxs-lookup"><span data-stu-id="1acd6-139">Kestrel must be restarted before it will detect changes made to its environment.</span></span>

> [!WARNING]
> <span data-ttu-id="1acd6-140">*launchSettings.json* nie należy przechowywać kluczy tajnych.</span><span class="sxs-lookup"><span data-stu-id="1acd6-140">*launchSettings.json* shouldn't store secrets.</span></span> <span data-ttu-id="1acd6-141">[Narzędzie Menedżer klucz tajny](xref:security/app-secrets) może służyć do przechowywania kluczy tajnych dla rozwoju lokalnych.</span><span class="sxs-lookup"><span data-stu-id="1acd6-141">The [Secret Manager tool](xref:security/app-secrets) can be used to store secrets for local development.</span></span>

<span data-ttu-id="1acd6-142">Korzystając z [Visual Studio Code](https://code.visualstudio.com/), można ustawić zmienne środowiskowe w *.vscode/launch.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="1acd6-142">When using [Visual Studio Code](https://code.visualstudio.com/), environment variables can be set in the *.vscode/launch.json* file.</span></span> <span data-ttu-id="1acd6-143">Poniższy przykład przedstawia środowiska `Development`:</span><span class="sxs-lookup"><span data-stu-id="1acd6-143">The following example sets the environment to `Development`:</span></span>

```json
{
   "version": "0.2.0",
   "configurations": [
        {
            "name": ".NET Core Launch (web)",

            ... additional VS Code configuration settings ...

            "env": {
                "ASPNETCORE_ENVIRONMENT": "Development"
            }
        }
    ]
}
```

<span data-ttu-id="1acd6-144">A *.vscode/launch.json* plik w projekcie nie jest do odczytu podczas uruchamiania aplikacji z `dotnet run` w taki sam sposób jak *Properties/launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="1acd6-144">A *.vscode/launch.json* file in the project isn't read when starting the app with `dotnet run` in the same way as *Properties/launchSettings.json*.</span></span> <span data-ttu-id="1acd6-145">Podczas uruchamiania aplikacji w projektowania, które nie ma *launchSettings.json* pliku, należy ustawić środowisko zmienną środowiskową lub argumentu wiersza polecenia do `dotnet run` polecenia.</span><span class="sxs-lookup"><span data-stu-id="1acd6-145">When launching an app in development that doesn't have a *launchSettings.json* file, either set the environment with an environment variable or a command-line argument to the `dotnet run` command.</span></span>

### <a name="production"></a><span data-ttu-id="1acd6-146">Produkcji</span><span class="sxs-lookup"><span data-stu-id="1acd6-146">Production</span></span>

<span data-ttu-id="1acd6-147">Aby zmaksymalizować zabezpieczeń, wydajności i niezawodności aplikacji należy skonfigurować środowiska produkcyjnego.</span><span class="sxs-lookup"><span data-stu-id="1acd6-147">The production environment should be configured to maximize security, performance, and application robustness.</span></span> <span data-ttu-id="1acd6-148">Niektóre typowe ustawienia, które różnią się od rozwoju obejmują:</span><span class="sxs-lookup"><span data-stu-id="1acd6-148">Some common settings that differ from development include:</span></span>

* <span data-ttu-id="1acd6-149">Buforowanie.</span><span class="sxs-lookup"><span data-stu-id="1acd6-149">Caching.</span></span>
* <span data-ttu-id="1acd6-150">Zasoby po stronie klienta są powiązane, zminimalizowane i potencjalnie pochodzący z sieci CDN.</span><span class="sxs-lookup"><span data-stu-id="1acd6-150">Client-side resources are bundled, minified, and potentially served from a CDN.</span></span>
* <span data-ttu-id="1acd6-151">Strony błędów diagnostycznych wyłączone.</span><span class="sxs-lookup"><span data-stu-id="1acd6-151">Diagnostic error pages disabled.</span></span>
* <span data-ttu-id="1acd6-152">Strony błędów przyjazną włączone.</span><span class="sxs-lookup"><span data-stu-id="1acd6-152">Friendly error pages enabled.</span></span>
* <span data-ttu-id="1acd6-153">Rejestrowanie produkcyjnych i włączone monitorowanie.</span><span class="sxs-lookup"><span data-stu-id="1acd6-153">Production logging and monitoring enabled.</span></span> <span data-ttu-id="1acd6-154">Na przykład [usługi Application Insights](/azure/application-insights/app-insights-asp-net-core).</span><span class="sxs-lookup"><span data-stu-id="1acd6-154">For example, [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="setting-the-environment"></a><span data-ttu-id="1acd6-155">Ustawienia środowiska</span><span class="sxs-lookup"><span data-stu-id="1acd6-155">Setting the environment</span></span>

<span data-ttu-id="1acd6-156">Często jest to przydatne ustawić w określonym środowisku do testowania.</span><span class="sxs-lookup"><span data-stu-id="1acd6-156">It's often useful to set a specific environment for testing.</span></span> <span data-ttu-id="1acd6-157">Jeśli środowisko nie jest ustawiona, domyślnie zostanie użyta `Production` która wyłącza większość funkcji debugowania.</span><span class="sxs-lookup"><span data-stu-id="1acd6-157">If the environment isn't set, it will default to `Production` which disables most debugging features.</span></span>

<span data-ttu-id="1acd6-158">Metoda do ustawiania środowiska zależy od systemu operacyjnego.</span><span class="sxs-lookup"><span data-stu-id="1acd6-158">The method for setting the environment depends on the operating system.</span></span>

### <a name="azure"></a><span data-ttu-id="1acd6-159">Azure</span><span class="sxs-lookup"><span data-stu-id="1acd6-159">Azure</span></span>

<span data-ttu-id="1acd6-160">Usługi aplikacji Azure:</span><span class="sxs-lookup"><span data-stu-id="1acd6-160">For Azure app service:</span></span>

* <span data-ttu-id="1acd6-161">Wybierz **ustawienia aplikacji** bloku.</span><span class="sxs-lookup"><span data-stu-id="1acd6-161">Select the **Application settings** blade.</span></span>
* <span data-ttu-id="1acd6-162">Dodaj klucz i wartość w **ustawień aplikacji**.</span><span class="sxs-lookup"><span data-stu-id="1acd6-162">Add the key and value in **App settings**.</span></span>


### <a name="windows"></a><span data-ttu-id="1acd6-163">Windows</span><span class="sxs-lookup"><span data-stu-id="1acd6-163">Windows</span></span>
<span data-ttu-id="1acd6-164">Aby ustawić `ASPNETCORE_ENVIRONMENT` dla bieżącej sesji, jeśli aplikacja zostanie uruchomiona przy użyciu [dotnet Uruchom](/dotnet/core/tools/dotnet-run), są używane następujące polecenia</span><span class="sxs-lookup"><span data-stu-id="1acd6-164">To set the `ASPNETCORE_ENVIRONMENT` for the current session, if the app is started using [dotnet run](/dotnet/core/tools/dotnet-run), the following commands are used</span></span>

<span data-ttu-id="1acd6-165">**Wiersz polecenia**</span><span class="sxs-lookup"><span data-stu-id="1acd6-165">**Command line**</span></span>
```
set ASPNETCORE_ENVIRONMENT=Development
```
<span data-ttu-id="1acd6-166">**Środowiska PowerShell**</span><span class="sxs-lookup"><span data-stu-id="1acd6-166">**PowerShell**</span></span>
```
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

<span data-ttu-id="1acd6-167">Te polecenia brane pod uwagę tylko dla bieżącego okna.</span><span class="sxs-lookup"><span data-stu-id="1acd6-167">These commands take effect only for the current window.</span></span> <span data-ttu-id="1acd6-168">Po zamknięciu okna ustawienie ASPNETCORE_ENVIRONMENT Przywraca wartości domyślne ustawienie lub komputera.</span><span class="sxs-lookup"><span data-stu-id="1acd6-168">When the window is closed, the ASPNETCORE_ENVIRONMENT setting reverts to the default setting or machine value.</span></span> <span data-ttu-id="1acd6-169">Aby można było ustawić wartość globalnie dla systemu Windows otwórz **Panelu sterowania** > **systemu** > **Zaawansowane ustawienia systemu** i Dodaj lub Edytuj `ASPNETCORE_ENVIRONMENT` wartość.</span><span class="sxs-lookup"><span data-stu-id="1acd6-169">In order to set the value globally on Windows open the **Control Panel** > **System** > **Advanced system settings** and add or edit the `ASPNETCORE_ENVIRONMENT` value.</span></span>

![Zaawansowane właściwości systemu](environments/_static/systemsetting_environment.png)

![Zmienna środowiskowa Core ASPNET](environments/_static/windows_aspnetcore_environment.png)


<span data-ttu-id="1acd6-172">**web.config**</span><span class="sxs-lookup"><span data-stu-id="1acd6-172">**web.config**</span></span>

<span data-ttu-id="1acd6-173">Zobacz *ustawienia zmiennych środowiskowych* sekcji [odwołania konfiguracji platformy ASP.NET Core modułu](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) tematu.</span><span class="sxs-lookup"><span data-stu-id="1acd6-173">See the *Setting environment variables* section of the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) topic.</span></span>

<span data-ttu-id="1acd6-174">**Dla każdej puli aplikacji usług IIS**</span><span class="sxs-lookup"><span data-stu-id="1acd6-174">**Per IIS Application Pool**</span></span>

<span data-ttu-id="1acd6-175">Aby ustawić zmienne środowiskowe dla poszczególnych aplikacji uruchomionych w izolowanej pulach aplikacji (obsługiwane w usługach IIS 10.0 +), zobacz *polecenia AppCmd.exe* sekcji [zmiennych środowiskowych \< environmentVariables >](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) tematu.</span><span class="sxs-lookup"><span data-stu-id="1acd6-175">To set environment variables for individual apps running in isolated Application Pools (supported on IIS 10.0+), see the *AppCmd.exe command* section of the [Environment Variables \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic.</span></span>

### <a name="macos"></a><span data-ttu-id="1acd6-176">macOS</span><span class="sxs-lookup"><span data-stu-id="1acd6-176">macOS</span></span>
<span data-ttu-id="1acd6-177">Ustawianie bieżącego środowiska pod kątem macOS może odbywać się w wierszu przy uruchamianiu aplikacji;</span><span class="sxs-lookup"><span data-stu-id="1acd6-177">Setting the current environment for macOS can be done in-line when running the application;</span></span>

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```
<span data-ttu-id="1acd6-178">lub przy użyciu `export` ustaw go przed uruchomieniem aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1acd6-178">or using `export` to set it prior to running the app.</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```
<span data-ttu-id="1acd6-179">Zmienne środowiskowe poziomu są ustawiane w *.bashrc* lub *.bash_profile* pliku.</span><span class="sxs-lookup"><span data-stu-id="1acd6-179">Machine level environment variables are set in the *.bashrc* or *.bash_profile* file.</span></span> <span data-ttu-id="1acd6-180">Przeprowadź edycję pliku za pomocą dowolnego edytora tekstu, a następnie dodaj następującą instrukcję.</span><span class="sxs-lookup"><span data-stu-id="1acd6-180">Edit the file using any text editor and add the following statment.</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

### <a name="linux"></a><span data-ttu-id="1acd6-181">Linux</span><span class="sxs-lookup"><span data-stu-id="1acd6-181">Linux</span></span>
<span data-ttu-id="1acd6-182">Dystrybucjach systemu Linux, można użyć `export` polecenie w wierszu polecenia dla zmiennej ustawieniami opartymi na sesji i *bash_profile* w pliku ustawień z poziomu środowiska maszyny.</span><span class="sxs-lookup"><span data-stu-id="1acd6-182">For Linux distros, use the `export` command at the command line for session based variable settings and *bash_profile* file for machine level environment settings.</span></span>

### <a name="configuration-by-environment"></a><span data-ttu-id="1acd6-183">Konfiguracja przez środowisko</span><span class="sxs-lookup"><span data-stu-id="1acd6-183">Configuration by environment</span></span>

<span data-ttu-id="1acd6-184">Zobacz [konfiguracji przez środowisko](xref:fundamentals/configuration/index#configuration-by-environment) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="1acd6-184">See [Configuration by environment](xref:fundamentals/configuration/index#configuration-by-environment) for more information.</span></span>

<a name="startup-conventions"></a>
## <a name="environment-based-startup-class-and-methods"></a><span data-ttu-id="1acd6-185">Klasa początkowa i metod opartych na środowisku</span><span class="sxs-lookup"><span data-stu-id="1acd6-185">Environment based Startup class and methods</span></span>

<span data-ttu-id="1acd6-186">Po uruchomieniu aplikacji platformy ASP.NET Core [Klasa początkowa](xref:fundamentals/startup) używa do ładowania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1acd6-186">When an ASP.NET Core app starts, the [Startup class](xref:fundamentals/startup) bootstraps the app.</span></span> <span data-ttu-id="1acd6-187">Jeśli klasa `Startup{EnvironmentName}` istnieje, że klasa zostanie wywołana dla tej `EnvironmentName`:</span><span class="sxs-lookup"><span data-stu-id="1acd6-187">If a class `Startup{EnvironmentName}` exists, that class will be called for that `EnvironmentName`:</span></span>

[!code-csharp[](environments/sample/WebApp1/StartupDev.cs?name=snippet&highlight=1)]

<span data-ttu-id="1acd6-188">Uwaga: Wywołanie [WebHostBuilder.UseStartup<TStartup> ](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) zastępuje sekcji konfiguracyjnych.</span><span class="sxs-lookup"><span data-stu-id="1acd6-188">Note: Calling [WebHostBuilder.UseStartup<TStartup>](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) overrides configuration sections.</span></span>

<span data-ttu-id="1acd6-189">[Skonfiguruj](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_StartupBase_Configure_Microsoft_AspNetCore_Builder_IApplicationBuilder_) i [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices?view=aspnetcore-2.0) obsługuje środowisko określonych wersji formularza `Configure{EnvironmentName}` i `Configure{EnvironmentName}Services`:</span><span class="sxs-lookup"><span data-stu-id="1acd6-189">[Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_StartupBase_Configure_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices?view=aspnetcore-2.0) support environment specific versions of the form `Configure{EnvironmentName}` and `Configure{EnvironmentName}Services`:</span></span>

[!code-csharp[](environments/sample/WebApp1/Startup.cs?name=snippet_all&highlight=15,37)]

## <a name="additional-resources"></a><span data-ttu-id="1acd6-190">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="1acd6-190">Additional resources</span></span>

* [<span data-ttu-id="1acd6-191">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="1acd6-191">Application startup</span></span>](xref:fundamentals/startup)
* [<span data-ttu-id="1acd6-192">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="1acd6-192">Configuration</span></span>](xref:fundamentals/configuration/index)
* [<span data-ttu-id="1acd6-193">IHostingEnvironment.EnvironmentName</span><span class="sxs-lookup"><span data-stu-id="1acd6-193">IHostingEnvironment.EnvironmentName</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName)
