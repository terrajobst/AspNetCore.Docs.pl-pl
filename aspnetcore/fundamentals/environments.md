---
title: Używanie wielu środowisk w ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak kontrolować zachowanie aplikacji w wielu środowiskach w aplikacjach ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/17/2019
uid: fundamentals/environments
ms.openlocfilehash: 30e2771c0a24fcbf6490d08c7028566314b6c011
ms.sourcegitcommit: 2cb857f0de774df421e35289662ba92cfe56ffd1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/25/2019
ms.locfileid: "75358724"
---
# <a name="use-multiple-environments-in-aspnet-core"></a><span data-ttu-id="c0995-103">Używanie wielu środowisk w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c0995-103">Use multiple environments in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="c0995-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="c0995-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="c0995-105">ASP.NET Core konfiguruje zachowanie aplikacji na podstawie środowiska uruchomieniowego przy użyciu zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="c0995-105">ASP.NET Core configures app behavior based on the runtime environment using an environment variable.</span></span>

<span data-ttu-id="c0995-106">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c0995-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="environments"></a><span data-ttu-id="c0995-107">Środowiska</span><span class="sxs-lookup"><span data-stu-id="c0995-107">Environments</span></span>

<span data-ttu-id="c0995-108">ASP.NET Core odczytuje zmienną środowiskową `ASPNETCORE_ENVIRONMENT` podczas uruchamiania aplikacji i zapisuje wartość w [IWebHostEnvironment. EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.EnvironmentName).</span><span class="sxs-lookup"><span data-stu-id="c0995-108">ASP.NET Core reads the environment variable `ASPNETCORE_ENVIRONMENT` at app startup and stores the value in [IWebHostEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.EnvironmentName).</span></span> <span data-ttu-id="c0995-109">`ASPNETCORE_ENVIRONMENT` można ustawić na dowolną wartość, ale w strukturze są dostarczane trzy wartości:</span><span class="sxs-lookup"><span data-stu-id="c0995-109">`ASPNETCORE_ENVIRONMENT` can be set to any value, but three values are provided by the framework:</span></span>

* <xref:Microsoft.Extensions.Hosting.Environments.Development>
* <xref:Microsoft.Extensions.Hosting.Environments.Staging>
* <span data-ttu-id="c0995-110"><xref:Microsoft.Extensions.Hosting.Environments.Production> (wartość domyślna)</span><span class="sxs-lookup"><span data-stu-id="c0995-110"><xref:Microsoft.Extensions.Hosting.Environments.Production> (default)</span></span>

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet)]

<span data-ttu-id="c0995-111">Powyższy kod:</span><span class="sxs-lookup"><span data-stu-id="c0995-111">The preceding code:</span></span>

* <span data-ttu-id="c0995-112">Wywołuje [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) , gdy `ASPNETCORE_ENVIRONMENT` jest ustawiony na `Development`.</span><span class="sxs-lookup"><span data-stu-id="c0995-112">Calls [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) when `ASPNETCORE_ENVIRONMENT` is set to `Development`.</span></span>
* <span data-ttu-id="c0995-113">Wywołuje [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler) , gdy wartość `ASPNETCORE_ENVIRONMENT` jest ustawiona na jedną z następujących wartości:</span><span class="sxs-lookup"><span data-stu-id="c0995-113">Calls [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler) when the value of `ASPNETCORE_ENVIRONMENT` is set one of the following:</span></span>

  * `Staging`
  * `Production`
  * `Staging_2`

<span data-ttu-id="c0995-114">[Pomocnik tagu środowiska](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) używa wartości `IHostingEnvironment.EnvironmentName` do dołączania lub wykluczania znaczników w elemencie:</span><span class="sxs-lookup"><span data-stu-id="c0995-114">The [Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) uses the value of `IHostingEnvironment.EnvironmentName` to include or exclude markup in the element:</span></span>

[!code-cshtml[](environments/sample-snapshot/EnvironmentsSample/Pages/About.cshtml)]

<span data-ttu-id="c0995-115">W systemach Windows i macOS, zmienne środowiskowe i wartości nie uwzględniają wielkości liter.</span><span class="sxs-lookup"><span data-stu-id="c0995-115">On Windows and macOS, environment variables and values aren't case sensitive.</span></span> <span data-ttu-id="c0995-116">Zmienne środowiskowe systemu Linux i wartości domyślnie **uwzględniają wielkość** liter.</span><span class="sxs-lookup"><span data-stu-id="c0995-116">Linux environment variables and values are **case sensitive** by default.</span></span>

### <a name="development"></a><span data-ttu-id="c0995-117">Projektowanie</span><span class="sxs-lookup"><span data-stu-id="c0995-117">Development</span></span>

<span data-ttu-id="c0995-118">Środowisko programistyczne może włączyć funkcje, które nie powinny być ujawnione w środowisku produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="c0995-118">The development environment can enable features that shouldn't be exposed in production.</span></span> <span data-ttu-id="c0995-119">Na przykład szablony ASP.NET Core umożliwiają [stronie wyjątków dla deweloperów](xref:fundamentals/error-handling#developer-exception-page) w środowisku deweloperskim.</span><span class="sxs-lookup"><span data-stu-id="c0995-119">For example, the ASP.NET Core templates enable the [Developer Exception Page](xref:fundamentals/error-handling#developer-exception-page) in the development environment.</span></span>

<span data-ttu-id="c0995-120">Środowisko do tworzenia maszyn lokalnych można ustawić w pliku *Properties\launchSettings.JSON* projektu.</span><span class="sxs-lookup"><span data-stu-id="c0995-120">The environment for local machine development can be set in the *Properties\launchSettings.json* file of the project.</span></span> <span data-ttu-id="c0995-121">Wartości środowiskowe ustawione w wartościach przesłonięcia *profilu launchsettings. JSON* ustawione w środowisku systemowym.</span><span class="sxs-lookup"><span data-stu-id="c0995-121">Environment values set in *launchSettings.json* override values set in the system environment.</span></span>

<span data-ttu-id="c0995-122">Poniższy kod JSON przedstawia trzy profile z pliku *profilu launchsettings. JSON* :</span><span class="sxs-lookup"><span data-stu-id="c0995-122">The following JSON shows three profiles from a *launchSettings.json* file:</span></span>

```json
{
  "iisSettings": {
    "windowsAuthentication": false,
    "anonymousAuthentication": true,
    "iisExpress": {
      "applicationUrl": "http://localhost:54339/",
      "sslPort": 0
    }
  },
  "profiles": {
    "IIS Express": {
      "commandName": "IISExpress",
      "launchBrowser": true,
      "environmentVariables": {
        "ASPNETCORE_My_Environment": "1",
        "ASPNETCORE_DETAILEDERRORS": "1",
        "ASPNETCORE_ENVIRONMENT": "Staging"
      }
    },
    "EnvironmentsSample": {
      "commandName": "Project",
      "launchBrowser": true,
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Staging"
      },
      "applicationUrl": "http://localhost:54340/"
    },
    "Kestrel Staging": {
      "commandName": "Project",
      "launchBrowser": true,
      "environmentVariables": {
        "ASPNETCORE_My_Environment": "1",
        "ASPNETCORE_DETAILEDERRORS": "1",
        "ASPNETCORE_ENVIRONMENT": "Staging"
      },
      "applicationUrl": "http://localhost:51997/"
    }
  }
}
```

> [!NOTE]
> <span data-ttu-id="c0995-123">Właściwość `applicationUrl` w pliku *profilu launchsettings. JSON* może określać listę adresów URL serwera.</span><span class="sxs-lookup"><span data-stu-id="c0995-123">The `applicationUrl` property in *launchSettings.json* can specify a list of server URLs.</span></span> <span data-ttu-id="c0995-124">Użyj średnika między adresami URL na liście:</span><span class="sxs-lookup"><span data-stu-id="c0995-124">Use a semicolon between the URLs in the list:</span></span>
>
> ```json
> "EnvironmentsSample": {
>    "commandName": "Project",
>    "launchBrowser": true,
>    "applicationUrl": "https://localhost:5001;http://localhost:5000",
>    "environmentVariables": {
>      "ASPNETCORE_ENVIRONMENT": "Development"
>    }
> }
> ```

<span data-ttu-id="c0995-125">Gdy aplikacja jest [uruchamiana z uruchomieniem dotnet](/dotnet/core/tools/dotnet-run), zostanie użyty pierwszy profil z `"commandName": "Project"`.</span><span class="sxs-lookup"><span data-stu-id="c0995-125">When the app is launched with [dotnet run](/dotnet/core/tools/dotnet-run), the first profile with `"commandName": "Project"` is used.</span></span> <span data-ttu-id="c0995-126">Wartość `commandName` Określa serwer sieci Web do uruchomienia.</span><span class="sxs-lookup"><span data-stu-id="c0995-126">The value of `commandName` specifies the web server to launch.</span></span> <span data-ttu-id="c0995-127">`commandName` może być jedną z następujących:</span><span class="sxs-lookup"><span data-stu-id="c0995-127">`commandName` can be any one of the following:</span></span>

* `IISExpress`
* `IIS`
* <span data-ttu-id="c0995-128">`Project` (co spowoduje uruchomienie Kestrel)</span><span class="sxs-lookup"><span data-stu-id="c0995-128">`Project` (which launches Kestrel)</span></span>

<span data-ttu-id="c0995-129">Gdy aplikacja jest uruchamiana z [uruchomieniem dotnet](/dotnet/core/tools/dotnet-run):</span><span class="sxs-lookup"><span data-stu-id="c0995-129">When an app is launched with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>

* <span data-ttu-id="c0995-130">plik *profilu launchsettings. JSON* jest odczytywany, jeśli jest dostępny.</span><span class="sxs-lookup"><span data-stu-id="c0995-130">*launchSettings.json* is read if available.</span></span> <span data-ttu-id="c0995-131">`environmentVariables` ustawienia w zmiennych środowiskowych *profilu launchsettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="c0995-131">`environmentVariables` settings in *launchSettings.json* override environment variables.</span></span>
* <span data-ttu-id="c0995-132">Zostanie wyświetlone środowisko hostingu.</span><span class="sxs-lookup"><span data-stu-id="c0995-132">The hosting environment is displayed.</span></span>

<span data-ttu-id="c0995-133">Następujące dane wyjściowe pokazują aplikację uruchomioną z [uruchomieniem dotnet](/dotnet/core/tools/dotnet-run):</span><span class="sxs-lookup"><span data-stu-id="c0995-133">The following output shows an app started with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>

```bash
PS C:\Websites\EnvironmentsSample> dotnet run
Using launch settings from C:\Websites\EnvironmentsSample\Properties\launchSettings.json...
Hosting environment: Staging
Content root path: C:\Websites\EnvironmentsSample
Now listening on: http://localhost:54340
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="c0995-134">Karta **debugowanie** właściwości projektu programu Visual Studio udostępnia graficzny interfejs użytkownika służący do edytowania pliku *profilu launchsettings. JSON* :</span><span class="sxs-lookup"><span data-stu-id="c0995-134">The Visual Studio project properties **Debug** tab provides a GUI to edit the *launchSettings.json* file:</span></span>

![Ustawienia właściwości projektu zmienne środowiskowe](environments/_static/project-properties-debug.png)

<span data-ttu-id="c0995-136">Zmiany wprowadzone w profilach projektu mogą zacząć obowiązywać dopiero po ponownym uruchomieniu serwera sieci Web.</span><span class="sxs-lookup"><span data-stu-id="c0995-136">Changes made to project profiles may not take effect until the web server is restarted.</span></span> <span data-ttu-id="c0995-137">Kestrel musi zostać uruchomiona ponownie, zanim będzie mógł wykryć zmiany wprowadzone w środowisku.</span><span class="sxs-lookup"><span data-stu-id="c0995-137">Kestrel must be restarted before it can detect changes made to its environment.</span></span>

> [!WARNING]
> <span data-ttu-id="c0995-138">plik *profilu launchsettings. JSON* nie powinien przechowywać wpisów tajnych.</span><span class="sxs-lookup"><span data-stu-id="c0995-138">*launchSettings.json* shouldn't store secrets.</span></span> <span data-ttu-id="c0995-139">[Narzędzia do zarządzania kluczami tajnymi](xref:security/app-secrets) można używać do przechowywania wpisów tajnych na potrzeby lokalnego tworzenia oprogramowania.</span><span class="sxs-lookup"><span data-stu-id="c0995-139">The [Secret Manager tool](xref:security/app-secrets) can be used to store secrets for local development.</span></span>

<span data-ttu-id="c0995-140">W przypadku korzystania z [Visual Studio Code](https://code.visualstudio.com/)zmienne środowiskowe można ustawić w pliku *. programu vscode/Launch. JSON* .</span><span class="sxs-lookup"><span data-stu-id="c0995-140">When using [Visual Studio Code](https://code.visualstudio.com/), environment variables can be set in the *.vscode/launch.json* file.</span></span> <span data-ttu-id="c0995-141">Poniższy przykład ustawia środowisko na `Development`:</span><span class="sxs-lookup"><span data-stu-id="c0995-141">The following example sets the environment to `Development`:</span></span>

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

<span data-ttu-id="c0995-142">Plik *. programu vscode/Launch. JSON* w projekcie nie jest odczytywany podczas uruchamiania aplikacji przy użyciu `dotnet run` w taki sam sposób, jak *właściwości/profilu launchsettings. JSON*.</span><span class="sxs-lookup"><span data-stu-id="c0995-142">A *.vscode/launch.json* file in the project isn't read when starting the app with `dotnet run` in the same way as *Properties/launchSettings.json*.</span></span> <span data-ttu-id="c0995-143">Podczas uruchamiania aplikacji w środowisku deweloperskim, która nie ma pliku *profilu launchsettings. JSON* , należy ustawić środowisko przy użyciu zmiennej środowiskowej lub argumentu wiersza polecenia do polecenia `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="c0995-143">When launching an app in development that doesn't have a *launchSettings.json* file, either set the environment with an environment variable or a command-line argument to the `dotnet run` command.</span></span>

### <a name="production"></a><span data-ttu-id="c0995-144">Produkcja</span><span class="sxs-lookup"><span data-stu-id="c0995-144">Production</span></span>

<span data-ttu-id="c0995-145">Środowisko produkcyjne powinno być skonfigurowane w celu zmaksymalizowania zabezpieczeń, wydajności i niezawodności aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c0995-145">The production environment should be configured to maximize security, performance, and app robustness.</span></span> <span data-ttu-id="c0995-146">Niektóre typowe ustawienia, które różnią się od programowania, to m.in.:</span><span class="sxs-lookup"><span data-stu-id="c0995-146">Some common settings that differ from development include:</span></span>

* <span data-ttu-id="c0995-147">Pamięć.</span><span class="sxs-lookup"><span data-stu-id="c0995-147">Caching.</span></span>
* <span data-ttu-id="c0995-148">Zasoby po stronie klienta są powiązane, zminimalizowanego i mogą być obsługiwane przez sieć CDN.</span><span class="sxs-lookup"><span data-stu-id="c0995-148">Client-side resources are bundled, minified, and potentially served from a CDN.</span></span>
* <span data-ttu-id="c0995-149">Wyłączono strony błędów diagnostyki.</span><span class="sxs-lookup"><span data-stu-id="c0995-149">Diagnostic error pages disabled.</span></span>
* <span data-ttu-id="c0995-150">Włączono przyjazne strony błędów.</span><span class="sxs-lookup"><span data-stu-id="c0995-150">Friendly error pages enabled.</span></span>
* <span data-ttu-id="c0995-151">Włączono rejestrowanie i monitorowanie produkcji.</span><span class="sxs-lookup"><span data-stu-id="c0995-151">Production logging and monitoring enabled.</span></span> <span data-ttu-id="c0995-152">Na przykład [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span><span class="sxs-lookup"><span data-stu-id="c0995-152">For example, [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="set-the-environment"></a><span data-ttu-id="c0995-153">Ustawianie środowiska</span><span class="sxs-lookup"><span data-stu-id="c0995-153">Set the environment</span></span>

<span data-ttu-id="c0995-154">Często warto ustawić określone środowisko do testowania przy użyciu zmiennej środowiskowej lub ustawienia platformy.</span><span class="sxs-lookup"><span data-stu-id="c0995-154">It's often useful to set a specific environment for testing with an environment variable or platform setting.</span></span> <span data-ttu-id="c0995-155">Jeśli środowisko nie jest ustawione, domyślnie `Production`, co spowoduje wyłączenie większości funkcji debugowania.</span><span class="sxs-lookup"><span data-stu-id="c0995-155">If the environment isn't set, it defaults to `Production`, which disables most debugging features.</span></span> <span data-ttu-id="c0995-156">Metoda ustawiania środowiska zależy od systemu operacyjnego.</span><span class="sxs-lookup"><span data-stu-id="c0995-156">The method for setting the environment depends on the operating system.</span></span>

<span data-ttu-id="c0995-157">Po skompilowaniu hosta ostatnie ustawienie środowiska odczytywane przez aplikację określa środowisko aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c0995-157">When the host is built, the last environment setting read by the app determines the app's environment.</span></span> <span data-ttu-id="c0995-158">Nie można zmienić środowiska aplikacji, gdy aplikacja jest uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="c0995-158">The app's environment can't be changed while the app is running.</span></span>

### <a name="environment-variable-or-platform-setting"></a><span data-ttu-id="c0995-159">Zmienna środowiskowa lub ustawienie platformy</span><span class="sxs-lookup"><span data-stu-id="c0995-159">Environment variable or platform setting</span></span>

#### <a name="azure-app-service"></a><span data-ttu-id="c0995-160">Usługa Azure App Service</span><span class="sxs-lookup"><span data-stu-id="c0995-160">Azure App Service</span></span>

<span data-ttu-id="c0995-161">Aby ustawić środowisko w [Azure App Service](https://azure.microsoft.com/services/app-service/), wykonaj następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="c0995-161">To set the environment in [Azure App Service](https://azure.microsoft.com/services/app-service/), perform the following steps:</span></span>

1. <span data-ttu-id="c0995-162">Wybierz aplikację z bloku **App Services** .</span><span class="sxs-lookup"><span data-stu-id="c0995-162">Select the app from the **App Services** blade.</span></span>
1. <span data-ttu-id="c0995-163">W grupie **Ustawienia** wybierz blok **Konfiguracja** .</span><span class="sxs-lookup"><span data-stu-id="c0995-163">In the **Settings** group, select the **Configuration** blade.</span></span>
1. <span data-ttu-id="c0995-164">Na karcie **Ustawienia aplikacji** wybierz pozycję **nowe ustawienie aplikacji**.</span><span class="sxs-lookup"><span data-stu-id="c0995-164">In the **Application settings** tab, select **New application setting**.</span></span>
1. <span data-ttu-id="c0995-165">W oknie **Dodawanie/Edytowanie ustawienia aplikacji** Podaj `ASPNETCORE_ENVIRONMENT` dla **nazwy**.</span><span class="sxs-lookup"><span data-stu-id="c0995-165">In the **Add/Edit application setting** window, provide `ASPNETCORE_ENVIRONMENT` for the **Name**.</span></span> <span data-ttu-id="c0995-166">W polu **wartość**Podaj środowisko (na przykład `Staging`).</span><span class="sxs-lookup"><span data-stu-id="c0995-166">For **Value**, provide the environment (for example, `Staging`).</span></span>
1. <span data-ttu-id="c0995-167">Zaznacz pole wyboru **ustawienie miejsca wdrożenia** , jeśli chcesz, aby ustawienie środowiska pozostawało w bieżącym gnieździe w przypadku wymiany miejsc wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="c0995-167">Select the **Deployment slot setting** check box if you wish the environment setting to remain with the current slot when deployment slots are swapped.</span></span> <span data-ttu-id="c0995-168">Aby uzyskać więcej informacji, zobacz [Konfigurowanie środowisk przejściowych w Azure App Service](/azure/app-service/web-sites-staged-publishing) w dokumentacji platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="c0995-168">For more information, see [Set up staging environments in Azure App Service](/azure/app-service/web-sites-staged-publishing) in the Azure documentation.</span></span>
1. <span data-ttu-id="c0995-169">Wybierz **przycisk OK** , aby zamknąć okno **Dodawanie/Edytowanie ustawienia aplikacji** .</span><span class="sxs-lookup"><span data-stu-id="c0995-169">Select **OK** to close the **Add/Edit application setting** window.</span></span>
1. <span data-ttu-id="c0995-170">Wybierz pozycję **Zapisz** w górnej części bloku **Konfiguracja** .</span><span class="sxs-lookup"><span data-stu-id="c0995-170">Select **Save** at the top of the **Configuration** blade.</span></span>

<span data-ttu-id="c0995-171">Azure App Service automatycznie uruchamia ponownie aplikację po dodaniu, zmianie lub usunięciu ustawienia aplikacji (zmienna środowiskowa) w Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="c0995-171">Azure App Service automatically restarts the app after an app setting (environment variable) is added, changed, or deleted in the Azure portal.</span></span>

#### <a name="windows"></a><span data-ttu-id="c0995-172">Windows</span><span class="sxs-lookup"><span data-stu-id="c0995-172">Windows</span></span>

<span data-ttu-id="c0995-173">Aby ustawić `ASPNETCORE_ENVIRONMENT` bieżącej sesji, gdy aplikacja zostanie uruchomiona przy użyciu [uruchomienia dotnet](/dotnet/core/tools/dotnet-run), są używane następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="c0995-173">To set the `ASPNETCORE_ENVIRONMENT` for the current session when the app is started using [dotnet run](/dotnet/core/tools/dotnet-run), the following commands are used:</span></span>

<span data-ttu-id="c0995-174">**Wiersz polecenia**</span><span class="sxs-lookup"><span data-stu-id="c0995-174">**Command prompt**</span></span>

```console
set ASPNETCORE_ENVIRONMENT=Development
```

<span data-ttu-id="c0995-175">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="c0995-175">**PowerShell**</span></span>

```powershell
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

<span data-ttu-id="c0995-176">Te polecenia zaczną obowiązywać tylko dla bieżącego okna.</span><span class="sxs-lookup"><span data-stu-id="c0995-176">These commands only take effect for the current window.</span></span> <span data-ttu-id="c0995-177">Po zamknięciu okna ustawienie `ASPNETCORE_ENVIRONMENT` przywraca ustawienie domyślne lub wartość maszyny.</span><span class="sxs-lookup"><span data-stu-id="c0995-177">When the window is closed, the `ASPNETCORE_ENVIRONMENT` setting reverts to the default setting or machine value.</span></span>

<span data-ttu-id="c0995-178">Aby ustawić wartość globalnie w systemie Windows, użyj jednej z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="c0995-178">To set the value globally in Windows, use either of the following approaches:</span></span>

* <span data-ttu-id="c0995-179">Otwórz **Panel sterowania** > **system** > **Zaawansowane ustawienia systemowe** i Dodaj lub Edytuj wartość `ASPNETCORE_ENVIRONMENT`:</span><span class="sxs-lookup"><span data-stu-id="c0995-179">Open the **Control Panel** > **System** > **Advanced system settings** and add or edit the `ASPNETCORE_ENVIRONMENT` value:</span></span>

  ![Zaawansowane właściwości systemu](environments/_static/systemsetting_environment.png)

  ![Zmienna środowiskowa ASPNET Core](environments/_static/windows_aspnetcore_environment.png)

* <span data-ttu-id="c0995-182">Otwórz wiersz polecenia z uprawnieniami administracyjnymi i użyj polecenia `setx` lub Otwórz wiersz polecenia administracyjnych programu PowerShell i użyj `[Environment]::SetEnvironmentVariable`:</span><span class="sxs-lookup"><span data-stu-id="c0995-182">Open an administrative command prompt and use the `setx` command or open an administrative PowerShell command prompt and use `[Environment]::SetEnvironmentVariable`:</span></span>

  <span data-ttu-id="c0995-183">**Wiersz polecenia**</span><span class="sxs-lookup"><span data-stu-id="c0995-183">**Command prompt**</span></span>

  ```console
  setx ASPNETCORE_ENVIRONMENT Development /M
  ```

  <span data-ttu-id="c0995-184">Przełącznik `/M` wskazuje, aby ustawić zmienną środowiskową na poziomie systemu.</span><span class="sxs-lookup"><span data-stu-id="c0995-184">The `/M` switch indicates to set the environment variable at the system level.</span></span> <span data-ttu-id="c0995-185">Jeśli przełącznik `/M` nie jest używany, zmienna środowiskowa jest ustawiana dla konta użytkownika.</span><span class="sxs-lookup"><span data-stu-id="c0995-185">If the `/M` switch isn't used, the environment variable is set for the user account.</span></span>

  <span data-ttu-id="c0995-186">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="c0995-186">**PowerShell**</span></span>

  ```powershell
  [Environment]::SetEnvironmentVariable("ASPNETCORE_ENVIRONMENT", "Development", "Machine")
  ```

  <span data-ttu-id="c0995-187">Opcja `Machine` wartość wskazuje, aby ustawić zmienną środowiskową na poziomie systemu.</span><span class="sxs-lookup"><span data-stu-id="c0995-187">The `Machine` option value indicates to set the environment variable at the system level.</span></span> <span data-ttu-id="c0995-188">Jeśli wartość opcji zostanie zmieniona na `User`, zmienna środowiskowa jest ustawiana dla konta użytkownika.</span><span class="sxs-lookup"><span data-stu-id="c0995-188">If the option value is changed to `User`, the environment variable is set for the user account.</span></span>

<span data-ttu-id="c0995-189">Gdy zmienna środowiskowa `ASPNETCORE_ENVIRONMENT` jest ustawiona globalnie, zacznie obowiązywać `dotnet run` w każdym oknie polecenia otwartym po ustawieniu wartości.</span><span class="sxs-lookup"><span data-stu-id="c0995-189">When the `ASPNETCORE_ENVIRONMENT` environment variable is set globally, it takes effect for `dotnet run` in any command window opened after the value is set.</span></span>

<span data-ttu-id="c0995-190">**web.config**</span><span class="sxs-lookup"><span data-stu-id="c0995-190">**web.config**</span></span>

<span data-ttu-id="c0995-191">Aby ustawić zmienną środowiskową `ASPNETCORE_ENVIRONMENT` za pomocą *pliku Web. config*, zobacz sekcję *Ustawianie zmiennych środowiskowych* w programie <xref:host-and-deploy/aspnet-core-module#setting-environment-variables>.</span><span class="sxs-lookup"><span data-stu-id="c0995-191">To set the `ASPNETCORE_ENVIRONMENT` environment variable with *web.config*, see the *Setting environment variables* section of <xref:host-and-deploy/aspnet-core-module#setting-environment-variables>.</span></span>

<span data-ttu-id="c0995-192">**Plik projektu lub profil publikacji**</span><span class="sxs-lookup"><span data-stu-id="c0995-192">**Project file or publish profile**</span></span>

<span data-ttu-id="c0995-193">**W przypadku wdrożeń usług Windows IIS:** Uwzględnij Właściwość `<EnvironmentName>` w [profilu publikowania (pubxml)](xref:host-and-deploy/visual-studio-publish-profiles) lub pliku projektu.</span><span class="sxs-lookup"><span data-stu-id="c0995-193">**For Windows IIS deployments:** Include the `<EnvironmentName>` property in the [publish profile (.pubxml)](xref:host-and-deploy/visual-studio-publish-profiles) or project file.</span></span> <span data-ttu-id="c0995-194">To podejście ustawia środowisko w *pliku Web. config* po opublikowaniu projektu:</span><span class="sxs-lookup"><span data-stu-id="c0995-194">This approach sets the environment in *web.config* when the project is published:</span></span>

```xml
<PropertyGroup>
  <EnvironmentName>Development</EnvironmentName>
</PropertyGroup>
```

<span data-ttu-id="c0995-195">**Na pulę aplikacji IIS**</span><span class="sxs-lookup"><span data-stu-id="c0995-195">**Per IIS Application Pool**</span></span>

<span data-ttu-id="c0995-196">Aby ustawić zmienną środowiskową `ASPNETCORE_ENVIRONMENT` dla aplikacji uruchomionej w izolowanej puli aplikacji (obsługiwanej przez usługi IIS 10,0 lub nowszej), zobacz sekcję *polecenie Appcmd. exe* w temacie [zmienne środowiskowe &lt;environmentVariables&gt;](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) temat.</span><span class="sxs-lookup"><span data-stu-id="c0995-196">To set the `ASPNETCORE_ENVIRONMENT` environment variable for an app running in an isolated Application Pool (supported on IIS 10.0 or later), see the *AppCmd.exe command* section of the [Environment Variables &lt;environmentVariables&gt;](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic.</span></span> <span data-ttu-id="c0995-197">Gdy zmienna środowiskowa `ASPNETCORE_ENVIRONMENT` jest ustawiona dla puli aplikacji, jej wartość zastępuje ustawienie na poziomie systemu.</span><span class="sxs-lookup"><span data-stu-id="c0995-197">When the `ASPNETCORE_ENVIRONMENT` environment variable is set for an app pool, its value overrides a setting at the system level.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c0995-198">Podczas hostowania aplikacji w usługach IIS i dodawania lub zmieniania zmiennej środowiskowej `ASPNETCORE_ENVIRONMENT` należy użyć jednego z poniższych metod, aby nowa wartość była pobierana przez aplikacje:</span><span class="sxs-lookup"><span data-stu-id="c0995-198">When hosting an app in IIS and adding or changing the `ASPNETCORE_ENVIRONMENT` environment variable, use any one of the following approaches to have the new value picked up by apps:</span></span>
>
> * <span data-ttu-id="c0995-199">Wykonaj `net stop was /y` a następnie `net start w3svc` z wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="c0995-199">Execute `net stop was /y` followed by `net start w3svc` from a command prompt.</span></span>
> * <span data-ttu-id="c0995-200">Uruchom ponownie serwer.</span><span class="sxs-lookup"><span data-stu-id="c0995-200">Restart the server.</span></span>

#### <a name="macos"></a><span data-ttu-id="c0995-201">macOS</span><span class="sxs-lookup"><span data-stu-id="c0995-201">macOS</span></span>

<span data-ttu-id="c0995-202">Ustawienie bieżącego środowiska dla macOS można wykonać w trybie online podczas uruchamiania aplikacji:</span><span class="sxs-lookup"><span data-stu-id="c0995-202">Setting the current environment for macOS can be performed in-line when running the app:</span></span>

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```

<span data-ttu-id="c0995-203">Alternatywnie można ustawić środowisko z `export` przed uruchomieniem aplikacji:</span><span class="sxs-lookup"><span data-stu-id="c0995-203">Alternatively, set the environment with `export` prior to running the app:</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

<span data-ttu-id="c0995-204">Zmienne środowiskowe na poziomie komputera są ustawiane w pliku *. bashrc* lub *. bash_profile* .</span><span class="sxs-lookup"><span data-stu-id="c0995-204">Machine-level environment variables are set in the *.bashrc* or *.bash_profile* file.</span></span> <span data-ttu-id="c0995-205">Edytuj plik przy użyciu dowolnego edytora tekstu.</span><span class="sxs-lookup"><span data-stu-id="c0995-205">Edit the file using any text editor.</span></span> <span data-ttu-id="c0995-206">Dodaj następującą instrukcję:</span><span class="sxs-lookup"><span data-stu-id="c0995-206">Add the following statement:</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

#### <a name="linux"></a><span data-ttu-id="c0995-207">Linux</span><span class="sxs-lookup"><span data-stu-id="c0995-207">Linux</span></span>

<span data-ttu-id="c0995-208">W przypadku systemu Linux dystrybucje Użyj polecenia `export` w wierszu polecenia dla ustawień zmiennych opartych na sesji i pliku *bash_profile* dla ustawień środowiska na poziomie komputera.</span><span class="sxs-lookup"><span data-stu-id="c0995-208">For Linux distros, use the `export` command at a command prompt for session-based variable settings and *bash_profile* file for machine-level environment settings.</span></span>

### <a name="set-the-environment-in-code"></a><span data-ttu-id="c0995-209">Ustawianie środowiska w kodzie</span><span class="sxs-lookup"><span data-stu-id="c0995-209">Set the environment in code</span></span>

<span data-ttu-id="c0995-210">Wywołaj <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseEnvironment*> podczas kompilowania hosta.</span><span class="sxs-lookup"><span data-stu-id="c0995-210">Call <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseEnvironment*> when building the host.</span></span> <span data-ttu-id="c0995-211">Zobacz <xref:fundamentals/host/generic-host#environmentname>.</span><span class="sxs-lookup"><span data-stu-id="c0995-211">See <xref:fundamentals/host/generic-host#environmentname>.</span></span>


### <a name="configuration-by-environment"></a><span data-ttu-id="c0995-212">Konfiguracja według środowiska</span><span class="sxs-lookup"><span data-stu-id="c0995-212">Configuration by environment</span></span>

<span data-ttu-id="c0995-213">Aby załadować konfigurację według środowiska, zalecamy:</span><span class="sxs-lookup"><span data-stu-id="c0995-213">To load configuration by environment, we recommend:</span></span>

* <span data-ttu-id="c0995-214">pliki *AppSettings* (*appSettings. { Environment}. JSON*).</span><span class="sxs-lookup"><span data-stu-id="c0995-214">*appsettings* files (*appsettings.{Environment}.json*).</span></span> <span data-ttu-id="c0995-215">Zobacz <xref:fundamentals/configuration/index#json-configuration-provider>.</span><span class="sxs-lookup"><span data-stu-id="c0995-215">See <xref:fundamentals/configuration/index#json-configuration-provider>.</span></span>
* <span data-ttu-id="c0995-216">Zmienne środowiskowe (ustawiane dla każdego systemu, w którym jest hostowana aplikacja).</span><span class="sxs-lookup"><span data-stu-id="c0995-216">Environment variables (set on each system where the app is hosted).</span></span> <span data-ttu-id="c0995-217">Zobacz <xref:fundamentals/host/generic-host#environmentname> i <xref:security/app-secrets#environment-variables>.</span><span class="sxs-lookup"><span data-stu-id="c0995-217">See <xref:fundamentals/host/generic-host#environmentname> and <xref:security/app-secrets#environment-variables>.</span></span>
* <span data-ttu-id="c0995-218">Secret Manager (tylko w środowisku programistycznym).</span><span class="sxs-lookup"><span data-stu-id="c0995-218">Secret Manager (in the Development environment only).</span></span> <span data-ttu-id="c0995-219">Zobacz <xref:security/app-secrets>.</span><span class="sxs-lookup"><span data-stu-id="c0995-219">See <xref:security/app-secrets>.</span></span>

## <a name="environment-based-startup-class-and-methods"></a><span data-ttu-id="c0995-220">Klasy i metody uruchamiania oparte na środowisku</span><span class="sxs-lookup"><span data-stu-id="c0995-220">Environment-based Startup class and methods</span></span>

### <a name="inject-iwebhostenvironment-into-startupconfigure"></a><span data-ttu-id="c0995-221">Wsuń IWebHostEnvironment do uruchamiania. Skonfiguruj</span><span class="sxs-lookup"><span data-stu-id="c0995-221">Inject IWebHostEnvironment into Startup.Configure</span></span>

<span data-ttu-id="c0995-222">Wsuń <xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment> do `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="c0995-222">Inject <xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment> into `Startup.Configure`.</span></span> <span data-ttu-id="c0995-223">Takie podejście jest przydatne, gdy aplikacja wymaga tylko dostosowania `Startup.Configure` w kilku środowiskach z minimalnymi różnicami kodu na środowisko.</span><span class="sxs-lookup"><span data-stu-id="c0995-223">This approach is useful when the app only requires adjusting `Startup.Configure` for a few environments with minimal code differences per environment.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    if (env.IsDevelopment())
    {
        // Development environment code
    }
    else
    {
        // Code for all other environments
    }
}
```

### <a name="inject-iwebhostenvironment-into-the-startup-class"></a><span data-ttu-id="c0995-224">Wsuń IWebHostEnvironment do klasy startowej</span><span class="sxs-lookup"><span data-stu-id="c0995-224">Inject IWebHostEnvironment into the Startup class</span></span>

<span data-ttu-id="c0995-225">Wsuń <xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment> do konstruktora `Startup`.</span><span class="sxs-lookup"><span data-stu-id="c0995-225">Inject <xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment> into the `Startup` constructor.</span></span> <span data-ttu-id="c0995-226">Takie podejście jest przydatne, gdy aplikacja wymaga skonfigurowania `Startup` dla kilku środowisk z minimalnymi różnicami kodu na środowisko.</span><span class="sxs-lookup"><span data-stu-id="c0995-226">This approach is useful when the app requires configuring `Startup` for only a few environments with minimal code differences per environment.</span></span>

<span data-ttu-id="c0995-227">W poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="c0995-227">In the following example:</span></span>

* <span data-ttu-id="c0995-228">Środowisko jest przechowywane w polu `_env`.</span><span class="sxs-lookup"><span data-stu-id="c0995-228">The environment is held in the `_env` field.</span></span>
* <span data-ttu-id="c0995-229">`_env` jest używany w `ConfigureServices` i `Configure` do zastosowania konfiguracji uruchamiania na podstawie środowiska aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c0995-229">`_env` is used in `ConfigureServices` and `Configure` to apply startup configuration based on the app's environment.</span></span>

```csharp
public class Startup
{
    private readonly IWebHostEnvironment _env;

    public Startup(IWebHostEnvironment env)
    {
        _env = env;
    }

    public void ConfigureServices(IServiceCollection services)
    {
        if (_env.IsDevelopment())
        {
            // Development environment code
        }
        else if (_env.IsStaging())
        {
            // Staging environment code
        }
        else
        {
            // Code for all other environments
        }
    }

    public void Configure(IApplicationBuilder app)
    {
        if (_env.IsDevelopment())
        {
            // Development environment code
        }
        else
        {
            // Code for all other environments
        }
    }
}
```
### <a name="startup-class-conventions"></a><span data-ttu-id="c0995-230">Konwencje klasy uruchamiania</span><span class="sxs-lookup"><span data-stu-id="c0995-230">Startup class conventions</span></span>

<span data-ttu-id="c0995-231">Po uruchomieniu aplikacji ASP.NET Core, [Klasa startowa](xref:fundamentals/startup) uruchamia aplikację.</span><span class="sxs-lookup"><span data-stu-id="c0995-231">When an ASP.NET Core app starts, the [Startup class](xref:fundamentals/startup) bootstraps the app.</span></span> <span data-ttu-id="c0995-232">Aplikacja może definiować oddzielne klasy `Startup` dla różnych środowisk (na przykład `StartupDevelopment`).</span><span class="sxs-lookup"><span data-stu-id="c0995-232">The app can define separate `Startup` classes for different environments (for example, `StartupDevelopment`).</span></span> <span data-ttu-id="c0995-233">Odpowiednia Klasa `Startup` jest wybierana w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="c0995-233">The appropriate `Startup` class is selected at runtime.</span></span> <span data-ttu-id="c0995-234">Kategoria, której sufiks nazwy jest zgodny z bieżącym środowiskiem, ma priorytet.</span><span class="sxs-lookup"><span data-stu-id="c0995-234">The class whose name suffix matches the current environment is prioritized.</span></span> <span data-ttu-id="c0995-235">Jeśli nie odnaleziono zgodnej klasy `Startup{EnvironmentName}`, używana jest Klasa `Startup`.</span><span class="sxs-lookup"><span data-stu-id="c0995-235">If a matching `Startup{EnvironmentName}` class isn't found, the `Startup` class is used.</span></span> <span data-ttu-id="c0995-236">Takie podejście jest przydatne, gdy aplikacja wymaga skonfigurowania uruchamiania dla kilku środowisk z wieloma różnicami kodu na środowisko.</span><span class="sxs-lookup"><span data-stu-id="c0995-236">This approach is useful when the app requires configuring startup for several environments with many code differences per environment.</span></span>

<span data-ttu-id="c0995-237">Aby zaimplementować klasy `Startup` oparte na środowisku, należy utworzyć klasę `Startup{EnvironmentName}` dla każdego używanego środowiska i klasy rezerwowej `Startup`:</span><span class="sxs-lookup"><span data-stu-id="c0995-237">To implement environment-based `Startup` classes, create a `Startup{EnvironmentName}` class for each environment in use and a fallback `Startup` class:</span></span>

```csharp
// Startup class to use in the Development environment
public class StartupDevelopment
{
    public void ConfigureServices(IServiceCollection services)
    {
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
    }
}

// Startup class to use in the Production environment
public class StartupProduction
{
    public void ConfigureServices(IServiceCollection services)
    {
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
    }
}

// Fallback Startup class
// Selected if the environment doesn't match a Startup{EnvironmentName} class
public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
    }
}
```

<span data-ttu-id="c0995-238">Użyj przeciążenia [UseStartup (IWebHostBuilder, String)](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usestartup) , które akceptuje nazwę zestawu:</span><span class="sxs-lookup"><span data-stu-id="c0995-238">Use the [UseStartup(IWebHostBuilder, String)](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usestartup) overload that accepts an assembly name:</span></span>

```csharp
public static void Main(string[] args)
{
    CreateWebHostBuilder(args).Build().Run();
}

public static IWebHostBuilder CreateWebHostBuilder(string[] args)
{
    var assemblyName = typeof(Startup).GetTypeInfo().Assembly.FullName;

    return WebHost.CreateDefaultBuilder(args)
        .UseStartup(assemblyName);
}
```

### <a name="startup-method-conventions"></a><span data-ttu-id="c0995-239">Konwencje metody uruchamiania</span><span class="sxs-lookup"><span data-stu-id="c0995-239">Startup method conventions</span></span>

<span data-ttu-id="c0995-240">[Skonfiguruj](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) i [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) wersje `Configure<EnvironmentName>` i `Configure<EnvironmentName>Services`dla poszczególnych środowisk.</span><span class="sxs-lookup"><span data-stu-id="c0995-240">[Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) support environment-specific versions of the form `Configure<EnvironmentName>` and `Configure<EnvironmentName>Services`.</span></span> <span data-ttu-id="c0995-241">Takie podejście jest przydatne, gdy aplikacja wymaga skonfigurowania uruchamiania dla kilku środowisk z wieloma różnicami kodu na środowisko.</span><span class="sxs-lookup"><span data-stu-id="c0995-241">This approach is useful when the app requires configuring startup for several environments with many code differences per environment.</span></span>

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet_all&highlight=15,42)]

## <a name="additional-resources"></a><span data-ttu-id="c0995-242">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="c0995-242">Additional resources</span></span>

* <xref:fundamentals/startup>
* <xref:fundamentals/configuration/index>
::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="c0995-243">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="c0995-243">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="c0995-244">ASP.NET Core konfiguruje zachowanie aplikacji na podstawie środowiska uruchomieniowego przy użyciu zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="c0995-244">ASP.NET Core configures app behavior based on the runtime environment using an environment variable.</span></span>

<span data-ttu-id="c0995-245">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c0995-245">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="environments"></a><span data-ttu-id="c0995-246">Środowiska</span><span class="sxs-lookup"><span data-stu-id="c0995-246">Environments</span></span>

<span data-ttu-id="c0995-247">ASP.NET Core odczytuje zmienną środowiskową `ASPNETCORE_ENVIRONMENT` podczas uruchamiania aplikacji i zapisuje wartość w [IHostingEnvironment. EnvironmentName](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName).</span><span class="sxs-lookup"><span data-stu-id="c0995-247">ASP.NET Core reads the environment variable `ASPNETCORE_ENVIRONMENT` at app startup and stores the value in [IHostingEnvironment.EnvironmentName](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName).</span></span> <span data-ttu-id="c0995-248">`ASPNETCORE_ENVIRONMENT` można ustawić na dowolną wartość, ale w strukturze są dostarczane trzy wartości:</span><span class="sxs-lookup"><span data-stu-id="c0995-248">`ASPNETCORE_ENVIRONMENT` can be set to any value, but three values are provided by the framework:</span></span>

* <xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Development>
* <xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Staging>
* <span data-ttu-id="c0995-249"><xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Production> (wartość domyślna)</span><span class="sxs-lookup"><span data-stu-id="c0995-249"><xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Production> (default)</span></span>

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet)]

<span data-ttu-id="c0995-250">Powyższy kod:</span><span class="sxs-lookup"><span data-stu-id="c0995-250">The preceding code:</span></span>

* <span data-ttu-id="c0995-251">Wywołuje [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) , gdy `ASPNETCORE_ENVIRONMENT` jest ustawiony na `Development`.</span><span class="sxs-lookup"><span data-stu-id="c0995-251">Calls [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) when `ASPNETCORE_ENVIRONMENT` is set to `Development`.</span></span>
* <span data-ttu-id="c0995-252">Wywołuje [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler) , gdy wartość `ASPNETCORE_ENVIRONMENT` jest ustawiona na jedną z następujących wartości:</span><span class="sxs-lookup"><span data-stu-id="c0995-252">Calls [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler) when the value of `ASPNETCORE_ENVIRONMENT` is set one of the following:</span></span>

  * `Staging`
  * `Production`
  * `Staging_2`

<span data-ttu-id="c0995-253">[Pomocnik tagu środowiska](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) używa wartości `IHostingEnvironment.EnvironmentName` do dołączania lub wykluczania znaczników w elemencie:</span><span class="sxs-lookup"><span data-stu-id="c0995-253">The [Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) uses the value of `IHostingEnvironment.EnvironmentName` to include or exclude markup in the element:</span></span>

[!code-cshtml[](environments/sample-snapshot/EnvironmentsSample/Pages/About.cshtml)]

<span data-ttu-id="c0995-254">W systemach Windows i macOS, zmienne środowiskowe i wartości nie uwzględniają wielkości liter.</span><span class="sxs-lookup"><span data-stu-id="c0995-254">On Windows and macOS, environment variables and values aren't case sensitive.</span></span> <span data-ttu-id="c0995-255">Zmienne środowiskowe systemu Linux i wartości domyślnie **uwzględniają wielkość** liter.</span><span class="sxs-lookup"><span data-stu-id="c0995-255">Linux environment variables and values are **case sensitive** by default.</span></span>

### <a name="development"></a><span data-ttu-id="c0995-256">Projektowanie</span><span class="sxs-lookup"><span data-stu-id="c0995-256">Development</span></span>

<span data-ttu-id="c0995-257">Środowisko programistyczne może włączyć funkcje, które nie powinny być ujawnione w środowisku produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="c0995-257">The development environment can enable features that shouldn't be exposed in production.</span></span> <span data-ttu-id="c0995-258">Na przykład szablony ASP.NET Core umożliwiają [stronie wyjątków dla deweloperów](xref:fundamentals/error-handling#developer-exception-page) w środowisku deweloperskim.</span><span class="sxs-lookup"><span data-stu-id="c0995-258">For example, the ASP.NET Core templates enable the [Developer Exception Page](xref:fundamentals/error-handling#developer-exception-page) in the development environment.</span></span>

<span data-ttu-id="c0995-259">Środowisko do tworzenia maszyn lokalnych można ustawić w pliku *Properties\launchSettings.JSON* projektu.</span><span class="sxs-lookup"><span data-stu-id="c0995-259">The environment for local machine development can be set in the *Properties\launchSettings.json* file of the project.</span></span> <span data-ttu-id="c0995-260">Wartości środowiskowe ustawione w wartościach przesłonięcia *profilu launchsettings. JSON* ustawione w środowisku systemowym.</span><span class="sxs-lookup"><span data-stu-id="c0995-260">Environment values set in *launchSettings.json* override values set in the system environment.</span></span>

<span data-ttu-id="c0995-261">Poniższy kod JSON przedstawia trzy profile z pliku *profilu launchsettings. JSON* :</span><span class="sxs-lookup"><span data-stu-id="c0995-261">The following JSON shows three profiles from a *launchSettings.json* file:</span></span>

```json
{
  "iisSettings": {
    "windowsAuthentication": false,
    "anonymousAuthentication": true,
    "iisExpress": {
      "applicationUrl": "http://localhost:54339/",
      "sslPort": 0
    }
  },
  "profiles": {
    "IIS Express": {
      "commandName": "IISExpress",
      "launchBrowser": true,
      "environmentVariables": {
        "ASPNETCORE_My_Environment": "1",
        "ASPNETCORE_DETAILEDERRORS": "1",
        "ASPNETCORE_ENVIRONMENT": "Staging"
      }
    },
    "EnvironmentsSample": {
      "commandName": "Project",
      "launchBrowser": true,
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Staging"
      },
      "applicationUrl": "http://localhost:54340/"
    },
    "Kestrel Staging": {
      "commandName": "Project",
      "launchBrowser": true,
      "environmentVariables": {
        "ASPNETCORE_My_Environment": "1",
        "ASPNETCORE_DETAILEDERRORS": "1",
        "ASPNETCORE_ENVIRONMENT": "Staging"
      },
      "applicationUrl": "http://localhost:51997/"
    }
  }
}
```

> [!NOTE]
> <span data-ttu-id="c0995-262">Właściwość `applicationUrl` w pliku *profilu launchsettings. JSON* może określać listę adresów URL serwera.</span><span class="sxs-lookup"><span data-stu-id="c0995-262">The `applicationUrl` property in *launchSettings.json* can specify a list of server URLs.</span></span> <span data-ttu-id="c0995-263">Użyj średnika między adresami URL na liście:</span><span class="sxs-lookup"><span data-stu-id="c0995-263">Use a semicolon between the URLs in the list:</span></span>
>
> ```json
> "EnvironmentsSample": {
>    "commandName": "Project",
>    "launchBrowser": true,
>    "applicationUrl": "https://localhost:5001;http://localhost:5000",
>    "environmentVariables": {
>      "ASPNETCORE_ENVIRONMENT": "Development"
>    }
> }
> ```

<span data-ttu-id="c0995-264">Gdy aplikacja jest [uruchamiana z uruchomieniem dotnet](/dotnet/core/tools/dotnet-run), zostanie użyty pierwszy profil z `"commandName": "Project"`.</span><span class="sxs-lookup"><span data-stu-id="c0995-264">When the app is launched with [dotnet run](/dotnet/core/tools/dotnet-run), the first profile with `"commandName": "Project"` is used.</span></span> <span data-ttu-id="c0995-265">Wartość `commandName` Określa serwer sieci Web do uruchomienia.</span><span class="sxs-lookup"><span data-stu-id="c0995-265">The value of `commandName` specifies the web server to launch.</span></span> <span data-ttu-id="c0995-266">`commandName` może być jedną z następujących:</span><span class="sxs-lookup"><span data-stu-id="c0995-266">`commandName` can be any one of the following:</span></span>

* `IISExpress`
* `IIS`
* <span data-ttu-id="c0995-267">`Project` (co spowoduje uruchomienie Kestrel)</span><span class="sxs-lookup"><span data-stu-id="c0995-267">`Project` (which launches Kestrel)</span></span>

<span data-ttu-id="c0995-268">Gdy aplikacja jest uruchamiana z [uruchomieniem dotnet](/dotnet/core/tools/dotnet-run):</span><span class="sxs-lookup"><span data-stu-id="c0995-268">When an app is launched with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>

* <span data-ttu-id="c0995-269">plik *profilu launchsettings. JSON* jest odczytywany, jeśli jest dostępny.</span><span class="sxs-lookup"><span data-stu-id="c0995-269">*launchSettings.json* is read if available.</span></span> <span data-ttu-id="c0995-270">`environmentVariables` ustawienia w zmiennych środowiskowych *profilu launchsettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="c0995-270">`environmentVariables` settings in *launchSettings.json* override environment variables.</span></span>
* <span data-ttu-id="c0995-271">Zostanie wyświetlone środowisko hostingu.</span><span class="sxs-lookup"><span data-stu-id="c0995-271">The hosting environment is displayed.</span></span>

<span data-ttu-id="c0995-272">Następujące dane wyjściowe pokazują aplikację uruchomioną z [uruchomieniem dotnet](/dotnet/core/tools/dotnet-run):</span><span class="sxs-lookup"><span data-stu-id="c0995-272">The following output shows an app started with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>

```bash
PS C:\Websites\EnvironmentsSample> dotnet run
Using launch settings from C:\Websites\EnvironmentsSample\Properties\launchSettings.json...
Hosting environment: Staging
Content root path: C:\Websites\EnvironmentsSample
Now listening on: http://localhost:54340
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="c0995-273">Karta **debugowanie** właściwości projektu programu Visual Studio udostępnia graficzny interfejs użytkownika służący do edytowania pliku *profilu launchsettings. JSON* :</span><span class="sxs-lookup"><span data-stu-id="c0995-273">The Visual Studio project properties **Debug** tab provides a GUI to edit the *launchSettings.json* file:</span></span>

![Ustawienia właściwości projektu zmienne środowiskowe](environments/_static/project-properties-debug.png)

<span data-ttu-id="c0995-275">Zmiany wprowadzone w profilach projektu mogą zacząć obowiązywać dopiero po ponownym uruchomieniu serwera sieci Web.</span><span class="sxs-lookup"><span data-stu-id="c0995-275">Changes made to project profiles may not take effect until the web server is restarted.</span></span> <span data-ttu-id="c0995-276">Kestrel musi zostać uruchomiona ponownie, zanim będzie mógł wykryć zmiany wprowadzone w środowisku.</span><span class="sxs-lookup"><span data-stu-id="c0995-276">Kestrel must be restarted before it can detect changes made to its environment.</span></span>

> [!WARNING]
> <span data-ttu-id="c0995-277">plik *profilu launchsettings. JSON* nie powinien przechowywać wpisów tajnych.</span><span class="sxs-lookup"><span data-stu-id="c0995-277">*launchSettings.json* shouldn't store secrets.</span></span> <span data-ttu-id="c0995-278">[Narzędzia do zarządzania kluczami tajnymi](xref:security/app-secrets) można używać do przechowywania wpisów tajnych na potrzeby lokalnego tworzenia oprogramowania.</span><span class="sxs-lookup"><span data-stu-id="c0995-278">The [Secret Manager tool](xref:security/app-secrets) can be used to store secrets for local development.</span></span>

<span data-ttu-id="c0995-279">W przypadku korzystania z [Visual Studio Code](https://code.visualstudio.com/)zmienne środowiskowe można ustawić w pliku *. programu vscode/Launch. JSON* .</span><span class="sxs-lookup"><span data-stu-id="c0995-279">When using [Visual Studio Code](https://code.visualstudio.com/), environment variables can be set in the *.vscode/launch.json* file.</span></span> <span data-ttu-id="c0995-280">Poniższy przykład ustawia środowisko na `Development`:</span><span class="sxs-lookup"><span data-stu-id="c0995-280">The following example sets the environment to `Development`:</span></span>

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

<span data-ttu-id="c0995-281">Plik *. programu vscode/Launch. JSON* w projekcie nie jest odczytywany podczas uruchamiania aplikacji przy użyciu `dotnet run` w taki sam sposób, jak *właściwości/profilu launchsettings. JSON*.</span><span class="sxs-lookup"><span data-stu-id="c0995-281">A *.vscode/launch.json* file in the project isn't read when starting the app with `dotnet run` in the same way as *Properties/launchSettings.json*.</span></span> <span data-ttu-id="c0995-282">Podczas uruchamiania aplikacji w środowisku deweloperskim, która nie ma pliku *profilu launchsettings. JSON* , należy ustawić środowisko przy użyciu zmiennej środowiskowej lub argumentu wiersza polecenia do polecenia `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="c0995-282">When launching an app in development that doesn't have a *launchSettings.json* file, either set the environment with an environment variable or a command-line argument to the `dotnet run` command.</span></span>

### <a name="production"></a><span data-ttu-id="c0995-283">Produkcja</span><span class="sxs-lookup"><span data-stu-id="c0995-283">Production</span></span>

<span data-ttu-id="c0995-284">Środowisko produkcyjne powinno być skonfigurowane w celu zmaksymalizowania zabezpieczeń, wydajności i niezawodności aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c0995-284">The production environment should be configured to maximize security, performance, and app robustness.</span></span> <span data-ttu-id="c0995-285">Niektóre typowe ustawienia, które różnią się od programowania, to m.in.:</span><span class="sxs-lookup"><span data-stu-id="c0995-285">Some common settings that differ from development include:</span></span>

* <span data-ttu-id="c0995-286">Pamięć.</span><span class="sxs-lookup"><span data-stu-id="c0995-286">Caching.</span></span>
* <span data-ttu-id="c0995-287">Zasoby po stronie klienta są powiązane, zminimalizowanego i mogą być obsługiwane przez sieć CDN.</span><span class="sxs-lookup"><span data-stu-id="c0995-287">Client-side resources are bundled, minified, and potentially served from a CDN.</span></span>
* <span data-ttu-id="c0995-288">Wyłączono strony błędów diagnostyki.</span><span class="sxs-lookup"><span data-stu-id="c0995-288">Diagnostic error pages disabled.</span></span>
* <span data-ttu-id="c0995-289">Włączono przyjazne strony błędów.</span><span class="sxs-lookup"><span data-stu-id="c0995-289">Friendly error pages enabled.</span></span>
* <span data-ttu-id="c0995-290">Włączono rejestrowanie i monitorowanie produkcji.</span><span class="sxs-lookup"><span data-stu-id="c0995-290">Production logging and monitoring enabled.</span></span> <span data-ttu-id="c0995-291">Na przykład [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span><span class="sxs-lookup"><span data-stu-id="c0995-291">For example, [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="set-the-environment"></a><span data-ttu-id="c0995-292">Ustawianie środowiska</span><span class="sxs-lookup"><span data-stu-id="c0995-292">Set the environment</span></span>

<span data-ttu-id="c0995-293">Często warto ustawić określone środowisko do testowania przy użyciu zmiennej środowiskowej lub ustawienia platformy.</span><span class="sxs-lookup"><span data-stu-id="c0995-293">It's often useful to set a specific environment for testing with an environment variable or platform setting.</span></span> <span data-ttu-id="c0995-294">Jeśli środowisko nie jest ustawione, domyślnie `Production`, co spowoduje wyłączenie większości funkcji debugowania.</span><span class="sxs-lookup"><span data-stu-id="c0995-294">If the environment isn't set, it defaults to `Production`, which disables most debugging features.</span></span> <span data-ttu-id="c0995-295">Metoda ustawiania środowiska zależy od systemu operacyjnego.</span><span class="sxs-lookup"><span data-stu-id="c0995-295">The method for setting the environment depends on the operating system.</span></span>

<span data-ttu-id="c0995-296">Po skompilowaniu hosta ostatnie ustawienie środowiska odczytywane przez aplikację określa środowisko aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c0995-296">When the host is built, the last environment setting read by the app determines the app's environment.</span></span> <span data-ttu-id="c0995-297">Nie można zmienić środowiska aplikacji, gdy aplikacja jest uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="c0995-297">The app's environment can't be changed while the app is running.</span></span>

### <a name="environment-variable-or-platform-setting"></a><span data-ttu-id="c0995-298">Zmienna środowiskowa lub ustawienie platformy</span><span class="sxs-lookup"><span data-stu-id="c0995-298">Environment variable or platform setting</span></span>

#### <a name="azure-app-service"></a><span data-ttu-id="c0995-299">Usługa Azure App Service</span><span class="sxs-lookup"><span data-stu-id="c0995-299">Azure App Service</span></span>

<span data-ttu-id="c0995-300">Aby ustawić środowisko w [Azure App Service](https://azure.microsoft.com/services/app-service/), wykonaj następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="c0995-300">To set the environment in [Azure App Service](https://azure.microsoft.com/services/app-service/), perform the following steps:</span></span>

1. <span data-ttu-id="c0995-301">Wybierz aplikację z bloku **App Services** .</span><span class="sxs-lookup"><span data-stu-id="c0995-301">Select the app from the **App Services** blade.</span></span>
1. <span data-ttu-id="c0995-302">W grupie **Ustawienia** wybierz blok **Konfiguracja** .</span><span class="sxs-lookup"><span data-stu-id="c0995-302">In the **Settings** group, select the **Configuration** blade.</span></span>
1. <span data-ttu-id="c0995-303">Na karcie **Ustawienia aplikacji** wybierz pozycję **nowe ustawienie aplikacji**.</span><span class="sxs-lookup"><span data-stu-id="c0995-303">In the **Application settings** tab, select **New application setting**.</span></span>
1. <span data-ttu-id="c0995-304">W oknie **Dodawanie/Edytowanie ustawienia aplikacji** Podaj `ASPNETCORE_ENVIRONMENT` dla **nazwy**.</span><span class="sxs-lookup"><span data-stu-id="c0995-304">In the **Add/Edit application setting** window, provide `ASPNETCORE_ENVIRONMENT` for the **Name**.</span></span> <span data-ttu-id="c0995-305">W polu **wartość**Podaj środowisko (na przykład `Staging`).</span><span class="sxs-lookup"><span data-stu-id="c0995-305">For **Value**, provide the environment (for example, `Staging`).</span></span>
1. <span data-ttu-id="c0995-306">Zaznacz pole wyboru **ustawienie miejsca wdrożenia** , jeśli chcesz, aby ustawienie środowiska pozostawało w bieżącym gnieździe w przypadku wymiany miejsc wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="c0995-306">Select the **Deployment slot setting** check box if you wish the environment setting to remain with the current slot when deployment slots are swapped.</span></span> <span data-ttu-id="c0995-307">Aby uzyskać więcej informacji, zobacz [Konfigurowanie środowisk przejściowych w Azure App Service](/azure/app-service/web-sites-staged-publishing) w dokumentacji platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="c0995-307">For more information, see [Set up staging environments in Azure App Service](/azure/app-service/web-sites-staged-publishing) in the Azure documentation.</span></span>
1. <span data-ttu-id="c0995-308">Wybierz **przycisk OK** , aby zamknąć okno **Dodawanie/Edytowanie ustawienia aplikacji** .</span><span class="sxs-lookup"><span data-stu-id="c0995-308">Select **OK** to close the **Add/Edit application setting** window.</span></span>
1. <span data-ttu-id="c0995-309">Wybierz pozycję **Zapisz** w górnej części bloku **Konfiguracja** .</span><span class="sxs-lookup"><span data-stu-id="c0995-309">Select **Save** at the top of the **Configuration** blade.</span></span>

<span data-ttu-id="c0995-310">Azure App Service automatycznie uruchamia ponownie aplikację po dodaniu, zmianie lub usunięciu ustawienia aplikacji (zmienna środowiskowa) w Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="c0995-310">Azure App Service automatically restarts the app after an app setting (environment variable) is added, changed, or deleted in the Azure portal.</span></span>

#### <a name="windows"></a><span data-ttu-id="c0995-311">Windows</span><span class="sxs-lookup"><span data-stu-id="c0995-311">Windows</span></span>

<span data-ttu-id="c0995-312">Aby ustawić `ASPNETCORE_ENVIRONMENT` bieżącej sesji, gdy aplikacja zostanie uruchomiona przy użyciu [uruchomienia dotnet](/dotnet/core/tools/dotnet-run), są używane następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="c0995-312">To set the `ASPNETCORE_ENVIRONMENT` for the current session when the app is started using [dotnet run](/dotnet/core/tools/dotnet-run), the following commands are used:</span></span>

<span data-ttu-id="c0995-313">**Wiersz polecenia**</span><span class="sxs-lookup"><span data-stu-id="c0995-313">**Command prompt**</span></span>

```console
set ASPNETCORE_ENVIRONMENT=Development
```

<span data-ttu-id="c0995-314">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="c0995-314">**PowerShell**</span></span>

```powershell
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

<span data-ttu-id="c0995-315">Te polecenia zaczną obowiązywać tylko dla bieżącego okna.</span><span class="sxs-lookup"><span data-stu-id="c0995-315">These commands only take effect for the current window.</span></span> <span data-ttu-id="c0995-316">Po zamknięciu okna ustawienie `ASPNETCORE_ENVIRONMENT` przywraca ustawienie domyślne lub wartość maszyny.</span><span class="sxs-lookup"><span data-stu-id="c0995-316">When the window is closed, the `ASPNETCORE_ENVIRONMENT` setting reverts to the default setting or machine value.</span></span>

<span data-ttu-id="c0995-317">Aby ustawić wartość globalnie w systemie Windows, użyj jednej z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="c0995-317">To set the value globally in Windows, use either of the following approaches:</span></span>

* <span data-ttu-id="c0995-318">Otwórz **Panel sterowania** > **system** > **Zaawansowane ustawienia systemowe** i Dodaj lub Edytuj wartość `ASPNETCORE_ENVIRONMENT`:</span><span class="sxs-lookup"><span data-stu-id="c0995-318">Open the **Control Panel** > **System** > **Advanced system settings** and add or edit the `ASPNETCORE_ENVIRONMENT` value:</span></span>

  ![Zaawansowane właściwości systemu](environments/_static/systemsetting_environment.png)

  ![Zmienna środowiskowa ASPNET Core](environments/_static/windows_aspnetcore_environment.png)

* <span data-ttu-id="c0995-321">Otwórz wiersz polecenia z uprawnieniami administracyjnymi i użyj polecenia `setx` lub Otwórz wiersz polecenia administracyjnych programu PowerShell i użyj `[Environment]::SetEnvironmentVariable`:</span><span class="sxs-lookup"><span data-stu-id="c0995-321">Open an administrative command prompt and use the `setx` command or open an administrative PowerShell command prompt and use `[Environment]::SetEnvironmentVariable`:</span></span>

  <span data-ttu-id="c0995-322">**Wiersz polecenia**</span><span class="sxs-lookup"><span data-stu-id="c0995-322">**Command prompt**</span></span>

  ```console
  setx ASPNETCORE_ENVIRONMENT Development /M
  ```

  <span data-ttu-id="c0995-323">Przełącznik `/M` wskazuje, aby ustawić zmienną środowiskową na poziomie systemu.</span><span class="sxs-lookup"><span data-stu-id="c0995-323">The `/M` switch indicates to set the environment variable at the system level.</span></span> <span data-ttu-id="c0995-324">Jeśli przełącznik `/M` nie jest używany, zmienna środowiskowa jest ustawiana dla konta użytkownika.</span><span class="sxs-lookup"><span data-stu-id="c0995-324">If the `/M` switch isn't used, the environment variable is set for the user account.</span></span>

  <span data-ttu-id="c0995-325">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="c0995-325">**PowerShell**</span></span>

  ```powershell
  [Environment]::SetEnvironmentVariable("ASPNETCORE_ENVIRONMENT", "Development", "Machine")
  ```

  <span data-ttu-id="c0995-326">Opcja `Machine` wartość wskazuje, aby ustawić zmienną środowiskową na poziomie systemu.</span><span class="sxs-lookup"><span data-stu-id="c0995-326">The `Machine` option value indicates to set the environment variable at the system level.</span></span> <span data-ttu-id="c0995-327">Jeśli wartość opcji zostanie zmieniona na `User`, zmienna środowiskowa jest ustawiana dla konta użytkownika.</span><span class="sxs-lookup"><span data-stu-id="c0995-327">If the option value is changed to `User`, the environment variable is set for the user account.</span></span>

<span data-ttu-id="c0995-328">Gdy zmienna środowiskowa `ASPNETCORE_ENVIRONMENT` jest ustawiona globalnie, zacznie obowiązywać `dotnet run` w każdym oknie polecenia otwartym po ustawieniu wartości.</span><span class="sxs-lookup"><span data-stu-id="c0995-328">When the `ASPNETCORE_ENVIRONMENT` environment variable is set globally, it takes effect for `dotnet run` in any command window opened after the value is set.</span></span>

<span data-ttu-id="c0995-329">**web.config**</span><span class="sxs-lookup"><span data-stu-id="c0995-329">**web.config**</span></span>

<span data-ttu-id="c0995-330">Aby ustawić zmienną środowiskową `ASPNETCORE_ENVIRONMENT` za pomocą *pliku Web. config*, zobacz sekcję *Ustawianie zmiennych środowiskowych* w programie <xref:host-and-deploy/aspnet-core-module#setting-environment-variables>.</span><span class="sxs-lookup"><span data-stu-id="c0995-330">To set the `ASPNETCORE_ENVIRONMENT` environment variable with *web.config*, see the *Setting environment variables* section of <xref:host-and-deploy/aspnet-core-module#setting-environment-variables>.</span></span>

<span data-ttu-id="c0995-331">**Plik projektu lub profil publikacji**</span><span class="sxs-lookup"><span data-stu-id="c0995-331">**Project file or publish profile**</span></span>

<span data-ttu-id="c0995-332">**W przypadku wdrożeń usług Windows IIS:** Uwzględnij Właściwość `<EnvironmentName>` w [profilu publikowania (pubxml)](xref:host-and-deploy/visual-studio-publish-profiles) lub pliku projektu.</span><span class="sxs-lookup"><span data-stu-id="c0995-332">**For Windows IIS deployments:** Include the `<EnvironmentName>` property in the [publish profile (.pubxml)](xref:host-and-deploy/visual-studio-publish-profiles) or project file.</span></span> <span data-ttu-id="c0995-333">To podejście ustawia środowisko w *pliku Web. config* po opublikowaniu projektu:</span><span class="sxs-lookup"><span data-stu-id="c0995-333">This approach sets the environment in *web.config* when the project is published:</span></span>

```xml
<PropertyGroup>
  <EnvironmentName>Development</EnvironmentName>
</PropertyGroup>
```

<span data-ttu-id="c0995-334">**Na pulę aplikacji IIS**</span><span class="sxs-lookup"><span data-stu-id="c0995-334">**Per IIS Application Pool**</span></span>

<span data-ttu-id="c0995-335">Aby ustawić zmienną środowiskową `ASPNETCORE_ENVIRONMENT` dla aplikacji uruchomionej w izolowanej puli aplikacji (obsługiwanej przez usługi IIS 10,0 lub nowszej), zobacz sekcję *polecenie Appcmd. exe* w temacie [zmienne środowiskowe &lt;environmentVariables&gt;](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) temat.</span><span class="sxs-lookup"><span data-stu-id="c0995-335">To set the `ASPNETCORE_ENVIRONMENT` environment variable for an app running in an isolated Application Pool (supported on IIS 10.0 or later), see the *AppCmd.exe command* section of the [Environment Variables &lt;environmentVariables&gt;](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic.</span></span> <span data-ttu-id="c0995-336">Gdy zmienna środowiskowa `ASPNETCORE_ENVIRONMENT` jest ustawiona dla puli aplikacji, jej wartość zastępuje ustawienie na poziomie systemu.</span><span class="sxs-lookup"><span data-stu-id="c0995-336">When the `ASPNETCORE_ENVIRONMENT` environment variable is set for an app pool, its value overrides a setting at the system level.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c0995-337">Podczas hostowania aplikacji w usługach IIS i dodawania lub zmieniania zmiennej środowiskowej `ASPNETCORE_ENVIRONMENT` należy użyć jednego z poniższych metod, aby nowa wartość była pobierana przez aplikacje:</span><span class="sxs-lookup"><span data-stu-id="c0995-337">When hosting an app in IIS and adding or changing the `ASPNETCORE_ENVIRONMENT` environment variable, use any one of the following approaches to have the new value picked up by apps:</span></span>
>
> * <span data-ttu-id="c0995-338">Wykonaj `net stop was /y` a następnie `net start w3svc` z wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="c0995-338">Execute `net stop was /y` followed by `net start w3svc` from a command prompt.</span></span>
> * <span data-ttu-id="c0995-339">Uruchom ponownie serwer.</span><span class="sxs-lookup"><span data-stu-id="c0995-339">Restart the server.</span></span>

#### <a name="macos"></a><span data-ttu-id="c0995-340">macOS</span><span class="sxs-lookup"><span data-stu-id="c0995-340">macOS</span></span>

<span data-ttu-id="c0995-341">Ustawienie bieżącego środowiska dla macOS można wykonać w trybie online podczas uruchamiania aplikacji:</span><span class="sxs-lookup"><span data-stu-id="c0995-341">Setting the current environment for macOS can be performed in-line when running the app:</span></span>

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```

<span data-ttu-id="c0995-342">Alternatywnie można ustawić środowisko z `export` przed uruchomieniem aplikacji:</span><span class="sxs-lookup"><span data-stu-id="c0995-342">Alternatively, set the environment with `export` prior to running the app:</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

<span data-ttu-id="c0995-343">Zmienne środowiskowe na poziomie komputera są ustawiane w pliku *. bashrc* lub *. bash_profile* .</span><span class="sxs-lookup"><span data-stu-id="c0995-343">Machine-level environment variables are set in the *.bashrc* or *.bash_profile* file.</span></span> <span data-ttu-id="c0995-344">Edytuj plik przy użyciu dowolnego edytora tekstu.</span><span class="sxs-lookup"><span data-stu-id="c0995-344">Edit the file using any text editor.</span></span> <span data-ttu-id="c0995-345">Dodaj następującą instrukcję:</span><span class="sxs-lookup"><span data-stu-id="c0995-345">Add the following statement:</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

#### <a name="linux"></a><span data-ttu-id="c0995-346">Linux</span><span class="sxs-lookup"><span data-stu-id="c0995-346">Linux</span></span>

<span data-ttu-id="c0995-347">W przypadku systemu Linux dystrybucje Użyj polecenia `export` w wierszu polecenia dla ustawień zmiennych opartych na sesji i pliku *bash_profile* dla ustawień środowiska na poziomie komputera.</span><span class="sxs-lookup"><span data-stu-id="c0995-347">For Linux distros, use the `export` command at a command prompt for session-based variable settings and *bash_profile* file for machine-level environment settings.</span></span>

### <a name="set-the-environment-in-code"></a><span data-ttu-id="c0995-348">Ustawianie środowiska w kodzie</span><span class="sxs-lookup"><span data-stu-id="c0995-348">Set the environment in code</span></span>

<span data-ttu-id="c0995-349">Wywołaj <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseEnvironment*> podczas kompilowania hosta.</span><span class="sxs-lookup"><span data-stu-id="c0995-349">Call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseEnvironment*> when building the host.</span></span> <span data-ttu-id="c0995-350">Zobacz <xref:fundamentals/host/web-host#environment>.</span><span class="sxs-lookup"><span data-stu-id="c0995-350">See <xref:fundamentals/host/web-host#environment>.</span></span>

### <a name="configuration-by-environment"></a><span data-ttu-id="c0995-351">Konfiguracja według środowiska</span><span class="sxs-lookup"><span data-stu-id="c0995-351">Configuration by environment</span></span>

<span data-ttu-id="c0995-352">Aby załadować konfigurację według środowiska, zalecamy:</span><span class="sxs-lookup"><span data-stu-id="c0995-352">To load configuration by environment, we recommend:</span></span>

* <span data-ttu-id="c0995-353">pliki *AppSettings* (*appSettings. { Environment}. JSON*).</span><span class="sxs-lookup"><span data-stu-id="c0995-353">*appsettings* files (*appsettings.{Environment}.json*).</span></span> <span data-ttu-id="c0995-354">Zobacz <xref:fundamentals/configuration/index#json-configuration-provider>.</span><span class="sxs-lookup"><span data-stu-id="c0995-354">See <xref:fundamentals/configuration/index#json-configuration-provider>.</span></span>
* <span data-ttu-id="c0995-355">Zmienne środowiskowe (ustawiane dla każdego systemu, w którym jest hostowana aplikacja).</span><span class="sxs-lookup"><span data-stu-id="c0995-355">Environment variables (set on each system where the app is hosted).</span></span> <span data-ttu-id="c0995-356">Zobacz <xref:fundamentals/host/web-host#environment> i <xref:security/app-secrets#environment-variables>.</span><span class="sxs-lookup"><span data-stu-id="c0995-356">See <xref:fundamentals/host/web-host#environment> and <xref:security/app-secrets#environment-variables>.</span></span>
* <span data-ttu-id="c0995-357">Secret Manager (tylko w środowisku programistycznym).</span><span class="sxs-lookup"><span data-stu-id="c0995-357">Secret Manager (in the Development environment only).</span></span> <span data-ttu-id="c0995-358">Zobacz <xref:security/app-secrets>.</span><span class="sxs-lookup"><span data-stu-id="c0995-358">See <xref:security/app-secrets>.</span></span>

## <a name="environment-based-startup-class-and-methods"></a><span data-ttu-id="c0995-359">Klasy i metody uruchamiania oparte na środowisku</span><span class="sxs-lookup"><span data-stu-id="c0995-359">Environment-based Startup class and methods</span></span>

### <a name="inject-ihostingenvironment-into-startupconfigure"></a><span data-ttu-id="c0995-360">Wsuń IHostingEnvironment do uruchamiania. Skonfiguruj</span><span class="sxs-lookup"><span data-stu-id="c0995-360">Inject IHostingEnvironment into Startup.Configure</span></span>

<span data-ttu-id="c0995-361">Wsuń <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> do `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="c0995-361">Inject <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> into `Startup.Configure`.</span></span> <span data-ttu-id="c0995-362">Takie podejście jest przydatne, gdy aplikacja wymaga tylko konfigurowania `Startup.Configure` tylko dla kilku środowisk z minimalnymi różnicami w kodzie dla danego środowiska.</span><span class="sxs-lookup"><span data-stu-id="c0995-362">This approach is useful when the app only requires configuring `Startup.Configure` for only a few environments with minimal code differences per environment.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        // Development environment code
    }
    else
    {
        // Code for all other environments
    }
}
```

### <a name="inject-ihostingenvironment-into-the-startup-class"></a><span data-ttu-id="c0995-363">Wsuń IHostingEnvironment do klasy startowej</span><span class="sxs-lookup"><span data-stu-id="c0995-363">Inject IHostingEnvironment into the Startup class</span></span>

<span data-ttu-id="c0995-364">Wsuń <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> do konstruktora `Startup` i przypisz usługę do pola do użycia w całej klasie `Startup`.</span><span class="sxs-lookup"><span data-stu-id="c0995-364">Inject <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> into the `Startup` constructor and assign the service to a field for use throughout the `Startup` class.</span></span> <span data-ttu-id="c0995-365">Takie podejście jest przydatne, gdy aplikacja wymaga konfiguracji uruchamiania tylko dla kilku środowisk z minimalnymi różnicami w kodzie dla danego środowiska.</span><span class="sxs-lookup"><span data-stu-id="c0995-365">This approach is useful when the app requires configuring startup for only a few environments with minimal code differences per environment.</span></span>

<span data-ttu-id="c0995-366">W poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="c0995-366">In the following example:</span></span>

* <span data-ttu-id="c0995-367">Środowisko jest przechowywane w polu `_env`.</span><span class="sxs-lookup"><span data-stu-id="c0995-367">The environment is held in the `_env` field.</span></span>
* <span data-ttu-id="c0995-368">`_env` jest używany w `ConfigureServices` i `Configure` do zastosowania konfiguracji uruchamiania na podstawie środowiska aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c0995-368">`_env` is used in `ConfigureServices` and `Configure` to apply startup configuration based on the app's environment.</span></span>

```csharp
public class Startup
{
    private readonly IHostingEnvironment _env;

    public Startup(IHostingEnvironment env)
    {
        _env = env;
    }

    public void ConfigureServices(IServiceCollection services)
    {
        if (_env.IsDevelopment())
        {
            // Development environment code
        }
        else if (_env.IsStaging())
        {
            // Staging environment code
        }
        else
        {
            // Code for all other environments
        }
    }

    public void Configure(IApplicationBuilder app)
    {
        if (_env.IsDevelopment())
        {
            // Development environment code
        }
        else
        {
            // Code for all other environments
        }
    }
}
```

### <a name="startup-class-conventions"></a><span data-ttu-id="c0995-369">Konwencje klasy uruchamiania</span><span class="sxs-lookup"><span data-stu-id="c0995-369">Startup class conventions</span></span>

<span data-ttu-id="c0995-370">Po uruchomieniu aplikacji ASP.NET Core, [Klasa startowa](xref:fundamentals/startup) uruchamia aplikację.</span><span class="sxs-lookup"><span data-stu-id="c0995-370">When an ASP.NET Core app starts, the [Startup class](xref:fundamentals/startup) bootstraps the app.</span></span> <span data-ttu-id="c0995-371">Aplikacja może definiować oddzielne klasy `Startup` dla różnych środowisk (na przykład `StartupDevelopment`).</span><span class="sxs-lookup"><span data-stu-id="c0995-371">The app can define separate `Startup` classes for different environments (for example, `StartupDevelopment`).</span></span> <span data-ttu-id="c0995-372">Odpowiednia Klasa `Startup` jest wybierana w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="c0995-372">The appropriate `Startup` class is selected at runtime.</span></span> <span data-ttu-id="c0995-373">Kategoria, której sufiks nazwy jest zgodny z bieżącym środowiskiem, ma priorytet.</span><span class="sxs-lookup"><span data-stu-id="c0995-373">The class whose name suffix matches the current environment is prioritized.</span></span> <span data-ttu-id="c0995-374">Jeśli nie odnaleziono zgodnej klasy `Startup{EnvironmentName}`, używana jest Klasa `Startup`.</span><span class="sxs-lookup"><span data-stu-id="c0995-374">If a matching `Startup{EnvironmentName}` class isn't found, the `Startup` class is used.</span></span> <span data-ttu-id="c0995-375">Takie podejście jest przydatne, gdy aplikacja wymaga skonfigurowania uruchamiania dla kilku środowisk z wieloma różnicami kodu na środowisko.</span><span class="sxs-lookup"><span data-stu-id="c0995-375">This approach is useful when the app requires configuring startup for several environments with many code differences per environment.</span></span>

<span data-ttu-id="c0995-376">Aby zaimplementować klasy `Startup` oparte na środowisku, należy utworzyć klasę `Startup{EnvironmentName}` dla każdego używanego środowiska i klasy rezerwowej `Startup`:</span><span class="sxs-lookup"><span data-stu-id="c0995-376">To implement environment-based `Startup` classes, create a `Startup{EnvironmentName}` class for each environment in use and a fallback `Startup` class:</span></span>

```csharp
// Startup class to use in the Development environment
public class StartupDevelopment
{
    public void ConfigureServices(IServiceCollection services)
    {
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
    }
}

// Startup class to use in the Production environment
public class StartupProduction
{
    public void ConfigureServices(IServiceCollection services)
    {
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
    }
}

// Fallback Startup class
// Selected if the environment doesn't match a Startup{EnvironmentName} class
public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
    }
}
```

<span data-ttu-id="c0995-377">Użyj przeciążenia [UseStartup (IWebHostBuilder, String)](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usestartup) , które akceptuje nazwę zestawu:</span><span class="sxs-lookup"><span data-stu-id="c0995-377">Use the [UseStartup(IWebHostBuilder, String)](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usestartup) overload that accepts an assembly name:</span></span>

```csharp
public static void Main(string[] args)
{
    CreateWebHostBuilder(args).Build().Run();
}

public static IWebHostBuilder CreateWebHostBuilder(string[] args)
{
    var assemblyName = typeof(Startup).GetTypeInfo().Assembly.FullName;

    return WebHost.CreateDefaultBuilder(args)
        .UseStartup(assemblyName);
}
```

### <a name="startup-method-conventions"></a><span data-ttu-id="c0995-378">Konwencje metody uruchamiania</span><span class="sxs-lookup"><span data-stu-id="c0995-378">Startup method conventions</span></span>

<span data-ttu-id="c0995-379">[Skonfiguruj](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) i [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) wersje `Configure<EnvironmentName>` i `Configure<EnvironmentName>Services`dla poszczególnych środowisk.</span><span class="sxs-lookup"><span data-stu-id="c0995-379">[Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) support environment-specific versions of the form `Configure<EnvironmentName>` and `Configure<EnvironmentName>Services`.</span></span> <span data-ttu-id="c0995-380">Takie podejście jest przydatne, gdy aplikacja wymaga skonfigurowania uruchamiania dla kilku środowisk z wieloma różnicami kodu na środowisko.</span><span class="sxs-lookup"><span data-stu-id="c0995-380">This approach is useful when the app requires configuring startup for several environments with many code differences per environment.</span></span>

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet_all&highlight=15,42)]

## <a name="additional-resources"></a><span data-ttu-id="c0995-381">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="c0995-381">Additional resources</span></span>

* <xref:fundamentals/startup>
* <xref:fundamentals/configuration/index>

::: moniker-end
