---
title: Użyj wiele środowisk w ASP.NET Core
author: rick-anderson
description: Informacje o sposobie kontrolowania zachowania aplikacji w wielu środowiskach, w aplikacji platformy ASP.NET Core.
ms.author: riande
ms.date: 06/21/2018
uid: fundamentals/environments
ms.openlocfilehash: 505f19d8b4df6e476b46a1fe7c49872d3c4acc1a
ms.sourcegitcommit: e22097b84d26a812cd1380a6b2d12c93e522c125
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/22/2018
ms.locfileid: "36314107"
---
# <a name="use-multiple-environments-in-aspnet-core"></a><span data-ttu-id="16519-103">Użyj wiele środowisk w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="16519-103">Use multiple environments in ASP.NET Core</span></span>

<span data-ttu-id="16519-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="16519-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="16519-105">Platformy ASP.NET Core konfiguruje zachowanie aplikacji opartych na środowisku środowiska uruchomieniowego za pomocą zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="16519-105">ASP.NET Core configures app behavior based on the runtime environment using an environment variable.</span></span>

<span data-ttu-id="16519-106">[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="16519-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="environments"></a><span data-ttu-id="16519-107">Środowisk</span><span class="sxs-lookup"><span data-stu-id="16519-107">Environments</span></span>

<span data-ttu-id="16519-108">Zmienna środowiskowa odczytuje platformy ASP.NET Core `ASPNETCORE_ENVIRONMENT` przy uruchamianiu aplikacji i przechowuje wartość [IHostingEnvironment.EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname).</span><span class="sxs-lookup"><span data-stu-id="16519-108">ASP.NET Core reads the environment variable `ASPNETCORE_ENVIRONMENT` at app startup and stores the value in [IHostingEnvironment.EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname).</span></span> <span data-ttu-id="16519-109">Można ustawić `ASPNETCORE_ENVIRONMENT` do żadnej wartości, ale [trzy wartości](/dotnet/api/microsoft.aspnetcore.hosting.environmentname) są obsługiwane przez platformę: [programowanie](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development), [przemieszczania](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging), i [produkcji](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production).</span><span class="sxs-lookup"><span data-stu-id="16519-109">You can set `ASPNETCORE_ENVIRONMENT` to any value, but [three values](/dotnet/api/microsoft.aspnetcore.hosting.environmentname) are supported by the framework: [Development](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development), [Staging](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging), and [Production](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production).</span></span> <span data-ttu-id="16519-110">Jeśli `ASPNETCORE_ENVIRONMENT` nie jest ustawiona domyślnie `Production`.</span><span class="sxs-lookup"><span data-stu-id="16519-110">If `ASPNETCORE_ENVIRONMENT` isn't set, it defaults to `Production`.</span></span>

[!code-csharp[](environments/sample/WebApp1/Startup.cs?name=snippet)]

<span data-ttu-id="16519-111">Poprzedni kod:</span><span class="sxs-lookup"><span data-stu-id="16519-111">The preceding code:</span></span>

* <span data-ttu-id="16519-112">Wywołania [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) i [UseBrowserLink](/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink) podczas `ASPNETCORE_ENVIRONMENT` ma ustawioną wartość `Development`.</span><span class="sxs-lookup"><span data-stu-id="16519-112">Calls [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) and [UseBrowserLink](/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink) when `ASPNETCORE_ENVIRONMENT` is set to `Development`.</span></span>
* <span data-ttu-id="16519-113">Wywołania [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler) podczas wartość `ASPNETCORE_ENVIRONMENT` ustawiono jedną z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="16519-113">Calls [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler) when the value of `ASPNETCORE_ENVIRONMENT` is set one of the following:</span></span>

    * `Staging`
    * `Production`
    * `Staging_2`

<span data-ttu-id="16519-114">[Pomocnika Tag środowiska](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) używa wartości `IHostingEnvironment.EnvironmentName` do dołączania lub wykluczania znaczników w elemencie:</span><span class="sxs-lookup"><span data-stu-id="16519-114">The [Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) uses the value of `IHostingEnvironment.EnvironmentName` to include or exclude markup in the element:</span></span>

[!code-cshtml[](environments/sample-snapshot/WebApp1/Pages/About.cshtml)]

<span data-ttu-id="16519-115">W systemach Windows i macOS zmienne środowiskowe i wartości nie jest uwzględniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="16519-115">On Windows and macOS, environment variables and values aren't case sensitive.</span></span> <span data-ttu-id="16519-116">Zmienne środowiskowe systemu Linux i wartości są **z uwzględnieniem wielkości liter** domyślnie.</span><span class="sxs-lookup"><span data-stu-id="16519-116">Linux environment variables and values are **case sensitive** by default.</span></span>

### <a name="development"></a><span data-ttu-id="16519-117">Tworzenie</span><span class="sxs-lookup"><span data-stu-id="16519-117">Development</span></span>

<span data-ttu-id="16519-118">Środowisko projektowe można włączyć funkcje, które nie powinny być dostępne w środowisku produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="16519-118">The development environment can enable features that shouldn't be exposed in production.</span></span> <span data-ttu-id="16519-119">Na przykład włączyć szablonów platformy ASP.NET Core [developer wyjątku strony](xref:fundamentals/error-handling#the-developer-exception-page) w środowisku programistycznym.</span><span class="sxs-lookup"><span data-stu-id="16519-119">For example, the ASP.NET Core templates enable the [developer exception page](xref:fundamentals/error-handling#the-developer-exception-page) in the development environment.</span></span>

<span data-ttu-id="16519-120">Środowisko rozwoju lokalnych komputera można skonfigurować *Properties\launchSettings.json* pliku projektu.</span><span class="sxs-lookup"><span data-stu-id="16519-120">The environment for local machine development can be set in the *Properties\launchSettings.json* file of the project.</span></span> <span data-ttu-id="16519-121">Ustaw wartości środowiska *launchSettings.json* przesłaniają wartości w środowisku systemu.</span><span class="sxs-lookup"><span data-stu-id="16519-121">Environment values set in *launchSettings.json* override values set in the system environment.</span></span>

<span data-ttu-id="16519-122">Następujące JSON zawiera trzy profile z *launchSettings.json* pliku:</span><span class="sxs-lookup"><span data-stu-id="16519-122">The following JSON shows three profiles from a *launchSettings.json* file:</span></span>

[!code-json[](environments/sample/WebApp1/Properties/launchSettings.json?highlight=10,11,18,26)]

::: moniker range=">= aspnetcore-2.1"

> [!NOTE]
> <span data-ttu-id="16519-123">`applicationUrl` Właściwości w *launchSettings.json* można określić listę adresów URL serwera.</span><span class="sxs-lookup"><span data-stu-id="16519-123">The `applicationUrl` property in *launchSettings.json* can specify a list of server URLs.</span></span> <span data-ttu-id="16519-124">Użyj średnika między adresami URL na liście:</span><span class="sxs-lookup"><span data-stu-id="16519-124">Use a semicolon between the URLs in the list:</span></span>
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

<span data-ttu-id="16519-125">Gdy aplikacja jest uruchamiana z [dotnet Uruchom](/dotnet/core/tools/dotnet-run), pierwszy profil z `"commandName": "Project"` jest używany.</span><span class="sxs-lookup"><span data-stu-id="16519-125">When the app is launched with [dotnet run](/dotnet/core/tools/dotnet-run), the first profile with `"commandName": "Project"` is used.</span></span> <span data-ttu-id="16519-126">Wartość `commandName` Określa serwer sieci web do uruchomienia.</span><span class="sxs-lookup"><span data-stu-id="16519-126">The value of `commandName` specifies the web server to launch.</span></span> <span data-ttu-id="16519-127">`commandName` może być jednym z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="16519-127">`commandName` can be any one of the following:</span></span>

* <span data-ttu-id="16519-128">IIS Express</span><span class="sxs-lookup"><span data-stu-id="16519-128">IIS Express</span></span>
* <span data-ttu-id="16519-129">IIS</span><span class="sxs-lookup"><span data-stu-id="16519-129">IIS</span></span>
* <span data-ttu-id="16519-130">Projekt (który uruchamia Kestrel)</span><span class="sxs-lookup"><span data-stu-id="16519-130">Project (which launches Kestrel)</span></span>

<span data-ttu-id="16519-131">Gdy aplikacja jest uruchamiana z [dotnet Uruchom](/dotnet/core/tools/dotnet-run):</span><span class="sxs-lookup"><span data-stu-id="16519-131">When an app is launched with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>

* <span data-ttu-id="16519-132">*launchSettings.json* jest do odczytu. Jeśli jest dostępna.</span><span class="sxs-lookup"><span data-stu-id="16519-132">*launchSettings.json* is read if available.</span></span> <span data-ttu-id="16519-133">`environmentVariables` ustawienia w *launchSettings.json* zastąpienia zmiennych środowiskowych.</span><span class="sxs-lookup"><span data-stu-id="16519-133">`environmentVariables` settings in *launchSettings.json* override environment variables.</span></span>
* <span data-ttu-id="16519-134">Środowisko macierzyste są wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="16519-134">The hosting environment is displayed.</span></span>

<span data-ttu-id="16519-135">Następujące dane wyjściowe zawiera wprowadzenie do aplikacji [dotnet Uruchom](/dotnet/core/tools/dotnet-run):</span><span class="sxs-lookup"><span data-stu-id="16519-135">The following output shows an app started with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>

```bash
PS C:\Webs\WebApp1> dotnet run
Using launch settings from C:\Webs\WebApp1\Properties\launchSettings.json...
Hosting environment: Staging
Content root path: C:\Webs\WebApp1
Now listening on: http://localhost:54340
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="16519-136">Visual Studio **debugowania** karta zawiera graficzny interfejs użytkownika do edycji *launchSettings.json* pliku:</span><span class="sxs-lookup"><span data-stu-id="16519-136">The Visual Studio **Debug** tab provides a GUI to edit the *launchSettings.json* file:</span></span>

![Zmienne środowiskowe ustawienie właściwości projektu](environments/_static/project-properties-debug.png)

<span data-ttu-id="16519-138">Zmiany wprowadzone do profilów projektu mogą nie zostać zastosowane do czasu ponownego uruchomienia serwera sieci web.</span><span class="sxs-lookup"><span data-stu-id="16519-138">Changes made to project profiles may not take effect until the web server is restarted.</span></span> <span data-ttu-id="16519-139">Aby zmiany wprowadzone w jego środowisku może wykryć, należy ponownie uruchomić kestrel.</span><span class="sxs-lookup"><span data-stu-id="16519-139">Kestrel must be restarted before it can detect changes made to its environment.</span></span>

> [!WARNING]
> <span data-ttu-id="16519-140">*launchSettings.json* nie należy przechowywać kluczy tajnych.</span><span class="sxs-lookup"><span data-stu-id="16519-140">*launchSettings.json* shouldn't store secrets.</span></span> <span data-ttu-id="16519-141">[Narzędzie Menedżer klucz tajny](xref:security/app-secrets) może służyć do przechowywania kluczy tajnych dla rozwoju lokalnych.</span><span class="sxs-lookup"><span data-stu-id="16519-141">The [Secret Manager tool](xref:security/app-secrets) can be used to store secrets for local development.</span></span>

<span data-ttu-id="16519-142">Korzystając z [Visual Studio Code](https://code.visualstudio.com/), można ustawić zmienne środowiskowe w *.vscode/launch.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="16519-142">When using [Visual Studio Code](https://code.visualstudio.com/), environment variables can be set in the *.vscode/launch.json* file.</span></span> <span data-ttu-id="16519-143">Poniższy przykład przedstawia środowiska `Development`:</span><span class="sxs-lookup"><span data-stu-id="16519-143">The following example sets the environment to `Development`:</span></span>

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

<span data-ttu-id="16519-144">A *.vscode/launch.json* plik w projekcie nie jest do odczytu podczas uruchamiania aplikacji z `dotnet run` w taki sam sposób jak *Properties/launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="16519-144">A *.vscode/launch.json* file in the project isn't read when starting the app with `dotnet run` in the same way as *Properties/launchSettings.json*.</span></span> <span data-ttu-id="16519-145">Podczas uruchamiania aplikacji w projektowania, które nie ma *launchSettings.json* pliku, należy ustawić środowisko zmienną środowiskową lub argumentu wiersza polecenia do `dotnet run` polecenia.</span><span class="sxs-lookup"><span data-stu-id="16519-145">When launching an app in development that doesn't have a *launchSettings.json* file, either set the environment with an environment variable or a command-line argument to the `dotnet run` command.</span></span>

### <a name="production"></a><span data-ttu-id="16519-146">Produkcji</span><span class="sxs-lookup"><span data-stu-id="16519-146">Production</span></span>

<span data-ttu-id="16519-147">Aby zmaksymalizować zabezpieczeń, wydajności i niezawodności aplikacji należy skonfigurować w środowisku produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="16519-147">The production environment should be configured to maximize security, performance, and app robustness.</span></span> <span data-ttu-id="16519-148">Niektóre typowe ustawienia, które różnią się od rozwoju obejmują:</span><span class="sxs-lookup"><span data-stu-id="16519-148">Some common settings that differ from development include:</span></span>

* <span data-ttu-id="16519-149">Buforowanie.</span><span class="sxs-lookup"><span data-stu-id="16519-149">Caching.</span></span>
* <span data-ttu-id="16519-150">Zasoby po stronie klienta są powiązane, zminimalizowane i potencjalnie pochodzący z sieci CDN.</span><span class="sxs-lookup"><span data-stu-id="16519-150">Client-side resources are bundled, minified, and potentially served from a CDN.</span></span>
* <span data-ttu-id="16519-151">Strony błędów diagnostycznych wyłączone.</span><span class="sxs-lookup"><span data-stu-id="16519-151">Diagnostic error pages disabled.</span></span>
* <span data-ttu-id="16519-152">Strony błędów przyjazną włączone.</span><span class="sxs-lookup"><span data-stu-id="16519-152">Friendly error pages enabled.</span></span>
* <span data-ttu-id="16519-153">Rejestrowanie produkcyjnych i włączone monitorowanie.</span><span class="sxs-lookup"><span data-stu-id="16519-153">Production logging and monitoring enabled.</span></span> <span data-ttu-id="16519-154">Na przykład [usługi Application Insights](/azure/application-insights/app-insights-asp-net-core).</span><span class="sxs-lookup"><span data-stu-id="16519-154">For example, [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="setting-the-environment"></a><span data-ttu-id="16519-155">Ustawienia środowiska</span><span class="sxs-lookup"><span data-stu-id="16519-155">Setting the environment</span></span>

<span data-ttu-id="16519-156">Często jest to przydatne ustawić w określonym środowisku do testowania.</span><span class="sxs-lookup"><span data-stu-id="16519-156">It's often useful to set a specific environment for testing.</span></span> <span data-ttu-id="16519-157">Jeśli środowisko nie jest ustawiona, domyślne `Production`, która wyłącza większość funkcji debugowania.</span><span class="sxs-lookup"><span data-stu-id="16519-157">If the environment isn't set, it defaults to `Production`, which disables most debugging features.</span></span> <span data-ttu-id="16519-158">Metoda do ustawiania środowiska zależy od systemu operacyjnego.</span><span class="sxs-lookup"><span data-stu-id="16519-158">The method for setting the environment depends on the operating system.</span></span>

### <a name="azure-app-service"></a><span data-ttu-id="16519-159">Usługa aplikacji Azure</span><span class="sxs-lookup"><span data-stu-id="16519-159">Azure App Service</span></span>

<span data-ttu-id="16519-160">Aby ustawić środowiska w [usłudze Azure App Service](https://azure.microsoft.com/services/app-service/), wykonaj następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="16519-160">To set the environment in [Azure App Service](https://azure.microsoft.com/services/app-service/), perform the following steps:</span></span>

1. <span data-ttu-id="16519-161">Wybierz aplikację z **usługi aplikacji** bloku.</span><span class="sxs-lookup"><span data-stu-id="16519-161">Select the app from the **App Services** blade.</span></span>
1. <span data-ttu-id="16519-162">W **ustawienia** grupy wybierz **ustawienia aplikacji** bloku.</span><span class="sxs-lookup"><span data-stu-id="16519-162">In the **SETTINGS** group, select the **Application settings** blade.</span></span>
1. <span data-ttu-id="16519-163">W **ustawienia aplikacji** wybierz opcję **Dodaj nowe ustawienie**.</span><span class="sxs-lookup"><span data-stu-id="16519-163">In the **Application settings** area, select **Add new setting**.</span></span>
1. <span data-ttu-id="16519-164">Aby uzyskać **wprowadź nazwę**, podaj `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="16519-164">For **Enter a name**, provide `ASPNETCORE_ENVIRONMENT`.</span></span> <span data-ttu-id="16519-165">Dla **wprowadź wartość**, podaj środowisku (na przykład `Staging`).</span><span class="sxs-lookup"><span data-stu-id="16519-165">For **Enter a value**, provide the environment (for example, `Staging`).</span></span>
1. <span data-ttu-id="16519-166">Wybierz **ustawienie miejsca** pole wyboru, jeśli chcesz, aby ustawienia środowiska do bieżącego miejsca po są zamienione miejsc wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="16519-166">Select the **Slot Setting** check box if you wish the environment setting to remain with the current slot when deployment slots are swapped.</span></span> <span data-ttu-id="16519-167">Aby uzyskać więcej informacji, zobacz [dokumentacji platformy Azure: ustawienia, które są zamienione?](/azure/app-service/web-sites-staged-publishing).</span><span class="sxs-lookup"><span data-stu-id="16519-167">For more information, see [Azure Documentation: Which settings are swapped?](/azure/app-service/web-sites-staged-publishing).</span></span>
1. <span data-ttu-id="16519-168">Wybierz **zapisać** w górnej części bloku.</span><span class="sxs-lookup"><span data-stu-id="16519-168">Select **Save** at the top of the blade.</span></span>

<span data-ttu-id="16519-169">Usługa aplikacji Azure automatyczne ponowne uruchomienie aplikacji po ustawienie aplikacji (zmienną środowiskową) jest dodać, zmienić lub usunąć w portalu Azure.</span><span class="sxs-lookup"><span data-stu-id="16519-169">Azure App Service automatically restarts the app after an app setting (environment variable) is added, changed, or deleted in the Azure portal.</span></span>

### <a name="windows"></a><span data-ttu-id="16519-170">Windows</span><span class="sxs-lookup"><span data-stu-id="16519-170">Windows</span></span>

<span data-ttu-id="16519-171">Aby ustawić `ASPNETCORE_ENVIRONMENT` dla bieżącej sesji, po uruchomieniu aplikacji przy użyciu [dotnet Uruchom](/dotnet/core/tools/dotnet-run), są używane następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="16519-171">To set the `ASPNETCORE_ENVIRONMENT` for the current session when the app is started using [dotnet run](/dotnet/core/tools/dotnet-run), the following commands are used:</span></span>

<span data-ttu-id="16519-172">**Wiersz polecenia**</span><span class="sxs-lookup"><span data-stu-id="16519-172">**Command prompt**</span></span>

```console
set ASPNETCORE_ENVIRONMENT=Development
```

<span data-ttu-id="16519-173">**Środowiska PowerShell**</span><span class="sxs-lookup"><span data-stu-id="16519-173">**PowerShell**</span></span>

```powershell
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

<span data-ttu-id="16519-174">Te polecenia tylko obowiązywać dla bieżącego okna.</span><span class="sxs-lookup"><span data-stu-id="16519-174">These commands only take effect for the current window.</span></span> <span data-ttu-id="16519-175">Po zamknięciu okna `ASPNETCORE_ENVIRONMENT` ustawienie powraca do wartości domyślne ustawienie lub komputera.</span><span class="sxs-lookup"><span data-stu-id="16519-175">When the window is closed, the `ASPNETCORE_ENVIRONMENT` setting reverts to the default setting or machine value.</span></span> <span data-ttu-id="16519-176">Aby ustawić wartość globalnie w systemie Windows, otwórz **Panelu sterowania** > **systemu** > **Zaawansowane ustawienia systemu** i Dodaj lub Edytuj `ASPNETCORE_ENVIRONMENT`wartość:</span><span class="sxs-lookup"><span data-stu-id="16519-176">To set the value globally in Windows, open the **Control Panel** > **System** > **Advanced system settings** and add or edit the `ASPNETCORE_ENVIRONMENT` value:</span></span>

![Zaawansowane właściwości systemu](environments/_static/systemsetting_environment.png)

![Zmienna środowiskowa Core ASPNET](environments/_static/windows_aspnetcore_environment.png)

<span data-ttu-id="16519-179">**web.config**</span><span class="sxs-lookup"><span data-stu-id="16519-179">**web.config**</span></span>

<span data-ttu-id="16519-180">Zobacz *ustawienia zmiennych środowiskowych* sekcji [odwołania konfiguracji platformy ASP.NET Core modułu](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) tematu.</span><span class="sxs-lookup"><span data-stu-id="16519-180">See the *Setting environment variables* section of the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) topic.</span></span>

<span data-ttu-id="16519-181">**Dla każdej puli aplikacji usług IIS**</span><span class="sxs-lookup"><span data-stu-id="16519-181">**Per IIS Application Pool**</span></span>

<span data-ttu-id="16519-182">Aby ustawić zmienne środowiskowe dla poszczególnych aplikacji uruchomionych w izolowanej pulach aplikacji (obsługiwane w usługach IIS 10.0 +), zobacz *polecenia AppCmd.exe* sekcji [zmiennych środowiskowych &lt; environmentVariables&gt; ](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) tematu.</span><span class="sxs-lookup"><span data-stu-id="16519-182">To set environment variables for individual apps running in isolated Application Pools (supported on IIS 10.0+), see the *AppCmd.exe command* section of the [Environment Variables &lt;environmentVariables&gt;](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic.</span></span>

### <a name="macos"></a><span data-ttu-id="16519-183">macOS</span><span class="sxs-lookup"><span data-stu-id="16519-183">macOS</span></span>

<span data-ttu-id="16519-184">Ustawienie w bieżącym środowisku macOS mogą być wykonywane w wierszu, podczas uruchamiania aplikacji:</span><span class="sxs-lookup"><span data-stu-id="16519-184">Setting the current environment for macOS can be performed in-line when running the app:</span></span>

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```

<span data-ttu-id="16519-185">Możesz również ustawić środowisko `export` przed uruchomieniem aplikacji:</span><span class="sxs-lookup"><span data-stu-id="16519-185">Alternatively, set the environment with `export` prior to running the app:</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

<span data-ttu-id="16519-186">Zmienne środowiskowe poziomie komputera są ustawiane *.bashrc* lub *.bash_profile* pliku.</span><span class="sxs-lookup"><span data-stu-id="16519-186">Machine-level environment variables are set in the *.bashrc* or *.bash_profile* file.</span></span> <span data-ttu-id="16519-187">Przeprowadź edycję pliku za pomocą dowolnego edytora tekstu.</span><span class="sxs-lookup"><span data-stu-id="16519-187">Edit the file using any text editor.</span></span> <span data-ttu-id="16519-188">Dodaj następującą instrukcję:</span><span class="sxs-lookup"><span data-stu-id="16519-188">Add the following statement:</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

### <a name="linux"></a><span data-ttu-id="16519-189">Linux</span><span class="sxs-lookup"><span data-stu-id="16519-189">Linux</span></span>

<span data-ttu-id="16519-190">Dystrybucjach systemu Linux, można użyć `export` polecenie w wierszu polecenia dla ustawienia zmiennej oparte na sesji i *bash_profile* ustawienia środowiska na poziomie komputera w pliku.</span><span class="sxs-lookup"><span data-stu-id="16519-190">For Linux distros, use the `export` command at a command prompt for session-based variable settings and *bash_profile* file for machine-level environment settings.</span></span>

### <a name="configuration-by-environment"></a><span data-ttu-id="16519-191">Konfiguracja przez środowisko</span><span class="sxs-lookup"><span data-stu-id="16519-191">Configuration by environment</span></span>

<span data-ttu-id="16519-192">Zobacz [konfiguracji przez środowisko](xref:fundamentals/configuration/index#configuration-by-environment) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="16519-192">See [Configuration by environment](xref:fundamentals/configuration/index#configuration-by-environment) for more information.</span></span>

## <a name="environment-based-startup-class-and-methods"></a><span data-ttu-id="16519-193">Na podstawie środowiska uruchamiania klasy i metody</span><span class="sxs-lookup"><span data-stu-id="16519-193">Environment-based Startup class and methods</span></span>

<span data-ttu-id="16519-194">Po uruchomieniu aplikacji platformy ASP.NET Core [Klasa początkowa](xref:fundamentals/startup) używa do ładowania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="16519-194">When an ASP.NET Core app starts, the [Startup class](xref:fundamentals/startup) bootstraps the app.</span></span> <span data-ttu-id="16519-195">Jeśli `Startup{EnvironmentName}` istnieje klasy, nosi nazwę klasy w tym `EnvironmentName`:</span><span class="sxs-lookup"><span data-stu-id="16519-195">If a `Startup{EnvironmentName}` class exists, the class is called for that `EnvironmentName`:</span></span>

[!code-csharp[](environments/sample/WebApp1/StartupDev.cs?name=snippet&highlight=1)]

<span data-ttu-id="16519-196">[WebHostBuilder.UseStartup<TStartup> ](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) zastępuje sekcji konfiguracyjnych.</span><span class="sxs-lookup"><span data-stu-id="16519-196">[WebHostBuilder.UseStartup<TStartup>](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) overrides configuration sections.</span></span>

<span data-ttu-id="16519-197">[Skonfiguruj](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) i [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) obsługuje wersji określonego środowiska formularza `Configure{EnvironmentName}` i `Configure{EnvironmentName}Services`:</span><span class="sxs-lookup"><span data-stu-id="16519-197">[Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) support environment-specific versions of the form `Configure{EnvironmentName}` and `Configure{EnvironmentName}Services`:</span></span>

[!code-csharp[](environments/sample/WebApp1/Startup.cs?name=snippet_all&highlight=15,37)]

## <a name="additional-resources"></a><span data-ttu-id="16519-198">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="16519-198">Additional resources</span></span>

* [<span data-ttu-id="16519-199">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="16519-199">Application startup</span></span>](xref:fundamentals/startup)
* [<span data-ttu-id="16519-200">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="16519-200">Configuration</span></span>](xref:fundamentals/configuration/index)
* [<span data-ttu-id="16519-201">IHostingEnvironment.EnvironmentName</span><span class="sxs-lookup"><span data-stu-id="16519-201">IHostingEnvironment.EnvironmentName</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname)
