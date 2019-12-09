---
title: Używanie wielu środowisk w ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak kontrolować zachowanie aplikacji w wielu środowiskach w aplikacjach ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 11/07/2019
uid: fundamentals/environments
ms.openlocfilehash: affbb95273c91fe5bf452e0e1ebefa669297304c
ms.sourcegitcommit: 851b921080fe8d719f54871770ccf6f78052584e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/09/2019
ms.locfileid: "74944324"
---
# <a name="use-multiple-environments-in-aspnet-core"></a><span data-ttu-id="23488-103">Używanie wielu środowisk w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="23488-103">Use multiple environments in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="23488-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="23488-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="23488-105">ASP.NET Core konfiguruje zachowanie aplikacji na podstawie środowiska uruchomieniowego przy użyciu zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="23488-105">ASP.NET Core configures app behavior based on the runtime environment using an environment variable.</span></span>

<span data-ttu-id="23488-106">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="23488-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="environments"></a><span data-ttu-id="23488-107">Środowiska</span><span class="sxs-lookup"><span data-stu-id="23488-107">Environments</span></span>

<span data-ttu-id="23488-108">ASP.NET Core odczytuje zmienną środowiskową `ASPNETCORE_ENVIRONMENT` podczas uruchamiania aplikacji i zapisuje wartość w [IWebHostEnvironment. EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.EnvironmentName).</span><span class="sxs-lookup"><span data-stu-id="23488-108">ASP.NET Core reads the environment variable `ASPNETCORE_ENVIRONMENT` at app startup and stores the value in [IWebHostEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.EnvironmentName).</span></span> <span data-ttu-id="23488-109">`ASPNETCORE_ENVIRONMENT` można ustawić na dowolną wartość, ale w strukturze są dostarczane trzy wartości:</span><span class="sxs-lookup"><span data-stu-id="23488-109">`ASPNETCORE_ENVIRONMENT` can be set to any value, but three values are provided by the framework:</span></span>

* <xref:Microsoft.Extensions.Hosting.Environments.Development>
* <xref:Microsoft.Extensions.Hosting.Environments.Staging>
* <span data-ttu-id="23488-110"><xref:Microsoft.Extensions.Hosting.Environments.Production> (wartość domyślna)</span><span class="sxs-lookup"><span data-stu-id="23488-110"><xref:Microsoft.Extensions.Hosting.Environments.Production> (default)</span></span>

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet)]

<span data-ttu-id="23488-111">Powyższy kod:</span><span class="sxs-lookup"><span data-stu-id="23488-111">The preceding code:</span></span>

* <span data-ttu-id="23488-112">Wywołuje [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) , gdy `ASPNETCORE_ENVIRONMENT` jest ustawiony na `Development`.</span><span class="sxs-lookup"><span data-stu-id="23488-112">Calls [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) when `ASPNETCORE_ENVIRONMENT` is set to `Development`.</span></span>
* <span data-ttu-id="23488-113">Wywołuje [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler) , gdy wartość `ASPNETCORE_ENVIRONMENT` jest ustawiona na jedną z następujących wartości:</span><span class="sxs-lookup"><span data-stu-id="23488-113">Calls [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler) when the value of `ASPNETCORE_ENVIRONMENT` is set one of the following:</span></span>

  * `Staging`
  * `Production`
  * `Staging_2`

<span data-ttu-id="23488-114">[Pomocnik tagu środowiska](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) używa wartości `IHostingEnvironment.EnvironmentName` do dołączania lub wykluczania znaczników w elemencie:</span><span class="sxs-lookup"><span data-stu-id="23488-114">The [Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) uses the value of `IHostingEnvironment.EnvironmentName` to include or exclude markup in the element:</span></span>

[!code-cshtml[](environments/sample-snapshot/EnvironmentsSample/Pages/About.cshtml)]

<span data-ttu-id="23488-115">W systemach Windows i macOS, zmienne środowiskowe i wartości nie uwzględniają wielkości liter.</span><span class="sxs-lookup"><span data-stu-id="23488-115">On Windows and macOS, environment variables and values aren't case sensitive.</span></span> <span data-ttu-id="23488-116">Zmienne środowiskowe systemu Linux i wartości domyślnie **uwzględniają wielkość** liter.</span><span class="sxs-lookup"><span data-stu-id="23488-116">Linux environment variables and values are **case sensitive** by default.</span></span>

### <a name="development"></a><span data-ttu-id="23488-117">Projektowanie</span><span class="sxs-lookup"><span data-stu-id="23488-117">Development</span></span>

<span data-ttu-id="23488-118">Środowisko programistyczne może włączyć funkcje, które nie powinny być ujawnione w środowisku produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="23488-118">The development environment can enable features that shouldn't be exposed in production.</span></span> <span data-ttu-id="23488-119">Na przykład szablony ASP.NET Core umożliwiają [stronie wyjątków dla deweloperów](xref:fundamentals/error-handling#developer-exception-page) w środowisku deweloperskim.</span><span class="sxs-lookup"><span data-stu-id="23488-119">For example, the ASP.NET Core templates enable the [Developer Exception Page](xref:fundamentals/error-handling#developer-exception-page) in the development environment.</span></span>

<span data-ttu-id="23488-120">Środowisko do tworzenia maszyn lokalnych można ustawić w pliku *Properties\launchSettings.JSON* projektu.</span><span class="sxs-lookup"><span data-stu-id="23488-120">The environment for local machine development can be set in the *Properties\launchSettings.json* file of the project.</span></span> <span data-ttu-id="23488-121">Wartości środowiskowe ustawione w wartościach przesłonięcia *profilu launchsettings. JSON* ustawione w środowisku systemowym.</span><span class="sxs-lookup"><span data-stu-id="23488-121">Environment values set in *launchSettings.json* override values set in the system environment.</span></span>

<span data-ttu-id="23488-122">Poniższy kod JSON przedstawia trzy profile z pliku *profilu launchsettings. JSON* :</span><span class="sxs-lookup"><span data-stu-id="23488-122">The following JSON shows three profiles from a *launchSettings.json* file:</span></span>

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
> <span data-ttu-id="23488-123">Właściwość `applicationUrl` w pliku *profilu launchsettings. JSON* może określać listę adresów URL serwera.</span><span class="sxs-lookup"><span data-stu-id="23488-123">The `applicationUrl` property in *launchSettings.json* can specify a list of server URLs.</span></span> <span data-ttu-id="23488-124">Użyj średnika między adresami URL na liście:</span><span class="sxs-lookup"><span data-stu-id="23488-124">Use a semicolon between the URLs in the list:</span></span>
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

<span data-ttu-id="23488-125">Gdy aplikacja jest [uruchamiana z uruchomieniem dotnet](/dotnet/core/tools/dotnet-run), zostanie użyty pierwszy profil z `"commandName": "Project"`.</span><span class="sxs-lookup"><span data-stu-id="23488-125">When the app is launched with [dotnet run](/dotnet/core/tools/dotnet-run), the first profile with `"commandName": "Project"` is used.</span></span> <span data-ttu-id="23488-126">Wartość `commandName` Określa serwer sieci Web do uruchomienia.</span><span class="sxs-lookup"><span data-stu-id="23488-126">The value of `commandName` specifies the web server to launch.</span></span> <span data-ttu-id="23488-127">`commandName` może być jedną z następujących:</span><span class="sxs-lookup"><span data-stu-id="23488-127">`commandName` can be any one of the following:</span></span>

* `IISExpress`
* `IIS`
* <span data-ttu-id="23488-128">`Project` (co spowoduje uruchomienie Kestrel)</span><span class="sxs-lookup"><span data-stu-id="23488-128">`Project` (which launches Kestrel)</span></span>

<span data-ttu-id="23488-129">Gdy aplikacja jest uruchamiana z [uruchomieniem dotnet](/dotnet/core/tools/dotnet-run):</span><span class="sxs-lookup"><span data-stu-id="23488-129">When an app is launched with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>

* <span data-ttu-id="23488-130">plik *profilu launchsettings. JSON* jest odczytywany, jeśli jest dostępny.</span><span class="sxs-lookup"><span data-stu-id="23488-130">*launchSettings.json* is read if available.</span></span> <span data-ttu-id="23488-131">`environmentVariables` ustawienia w zmiennych środowiskowych *profilu launchsettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="23488-131">`environmentVariables` settings in *launchSettings.json* override environment variables.</span></span>
* <span data-ttu-id="23488-132">Zostanie wyświetlone środowisko hostingu.</span><span class="sxs-lookup"><span data-stu-id="23488-132">The hosting environment is displayed.</span></span>

<span data-ttu-id="23488-133">Następujące dane wyjściowe pokazują aplikację uruchomioną z [uruchomieniem dotnet](/dotnet/core/tools/dotnet-run):</span><span class="sxs-lookup"><span data-stu-id="23488-133">The following output shows an app started with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>

```bash
PS C:\Websites\EnvironmentsSample> dotnet run
Using launch settings from C:\Websites\EnvironmentsSample\Properties\launchSettings.json...
Hosting environment: Staging
Content root path: C:\Websites\EnvironmentsSample
Now listening on: http://localhost:54340
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="23488-134">Karta **debugowanie** właściwości projektu programu Visual Studio udostępnia graficzny interfejs użytkownika służący do edytowania pliku *profilu launchsettings. JSON* :</span><span class="sxs-lookup"><span data-stu-id="23488-134">The Visual Studio project properties **Debug** tab provides a GUI to edit the *launchSettings.json* file:</span></span>

![Ustawienia właściwości projektu zmienne środowiskowe](environments/_static/project-properties-debug.png)

<span data-ttu-id="23488-136">Zmiany wprowadzone w profilach projektu mogą zacząć obowiązywać dopiero po ponownym uruchomieniu serwera sieci Web.</span><span class="sxs-lookup"><span data-stu-id="23488-136">Changes made to project profiles may not take effect until the web server is restarted.</span></span> <span data-ttu-id="23488-137">Kestrel musi zostać uruchomiona ponownie, zanim będzie mógł wykryć zmiany wprowadzone w środowisku.</span><span class="sxs-lookup"><span data-stu-id="23488-137">Kestrel must be restarted before it can detect changes made to its environment.</span></span>

> [!WARNING]
> <span data-ttu-id="23488-138">plik *profilu launchsettings. JSON* nie powinien przechowywać wpisów tajnych.</span><span class="sxs-lookup"><span data-stu-id="23488-138">*launchSettings.json* shouldn't store secrets.</span></span> <span data-ttu-id="23488-139">[Narzędzia do zarządzania kluczami tajnymi](xref:security/app-secrets) można używać do przechowywania wpisów tajnych na potrzeby lokalnego tworzenia oprogramowania.</span><span class="sxs-lookup"><span data-stu-id="23488-139">The [Secret Manager tool](xref:security/app-secrets) can be used to store secrets for local development.</span></span>

<span data-ttu-id="23488-140">W przypadku korzystania z [Visual Studio Code](https://code.visualstudio.com/)zmienne środowiskowe można ustawić w pliku *. programu vscode/Launch. JSON* .</span><span class="sxs-lookup"><span data-stu-id="23488-140">When using [Visual Studio Code](https://code.visualstudio.com/), environment variables can be set in the *.vscode/launch.json* file.</span></span> <span data-ttu-id="23488-141">Poniższy przykład ustawia środowisko na `Development`:</span><span class="sxs-lookup"><span data-stu-id="23488-141">The following example sets the environment to `Development`:</span></span>

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

<span data-ttu-id="23488-142">Plik *. programu vscode/Launch. JSON* w projekcie nie jest odczytywany podczas uruchamiania aplikacji przy użyciu `dotnet run` w taki sam sposób, jak *właściwości/profilu launchsettings. JSON*.</span><span class="sxs-lookup"><span data-stu-id="23488-142">A *.vscode/launch.json* file in the project isn't read when starting the app with `dotnet run` in the same way as *Properties/launchSettings.json*.</span></span> <span data-ttu-id="23488-143">Podczas uruchamiania aplikacji w środowisku deweloperskim, która nie ma pliku *profilu launchsettings. JSON* , należy ustawić środowisko przy użyciu zmiennej środowiskowej lub argumentu wiersza polecenia do polecenia `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="23488-143">When launching an app in development that doesn't have a *launchSettings.json* file, either set the environment with an environment variable or a command-line argument to the `dotnet run` command.</span></span>

### <a name="production"></a><span data-ttu-id="23488-144">Produkcja</span><span class="sxs-lookup"><span data-stu-id="23488-144">Production</span></span>

<span data-ttu-id="23488-145">Środowisko produkcyjne powinno być skonfigurowane w celu zmaksymalizowania zabezpieczeń, wydajności i niezawodności aplikacji.</span><span class="sxs-lookup"><span data-stu-id="23488-145">The production environment should be configured to maximize security, performance, and app robustness.</span></span> <span data-ttu-id="23488-146">Niektóre typowe ustawienia, które różnią się od programowania, to m.in.:</span><span class="sxs-lookup"><span data-stu-id="23488-146">Some common settings that differ from development include:</span></span>

* <span data-ttu-id="23488-147">Pamięć.</span><span class="sxs-lookup"><span data-stu-id="23488-147">Caching.</span></span>
* <span data-ttu-id="23488-148">Zasoby po stronie klienta są powiązane, zminimalizowanego i mogą być obsługiwane przez sieć CDN.</span><span class="sxs-lookup"><span data-stu-id="23488-148">Client-side resources are bundled, minified, and potentially served from a CDN.</span></span>
* <span data-ttu-id="23488-149">Wyłączono strony błędów diagnostyki.</span><span class="sxs-lookup"><span data-stu-id="23488-149">Diagnostic error pages disabled.</span></span>
* <span data-ttu-id="23488-150">Włączono przyjazne strony błędów.</span><span class="sxs-lookup"><span data-stu-id="23488-150">Friendly error pages enabled.</span></span>
* <span data-ttu-id="23488-151">Włączono rejestrowanie i monitorowanie produkcji.</span><span class="sxs-lookup"><span data-stu-id="23488-151">Production logging and monitoring enabled.</span></span> <span data-ttu-id="23488-152">Na przykład [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span><span class="sxs-lookup"><span data-stu-id="23488-152">For example, [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="set-the-environment"></a><span data-ttu-id="23488-153">Ustawianie środowiska</span><span class="sxs-lookup"><span data-stu-id="23488-153">Set the environment</span></span>

<span data-ttu-id="23488-154">Często warto ustawić określone środowisko do testowania przy użyciu zmiennej środowiskowej lub ustawienia platformy.</span><span class="sxs-lookup"><span data-stu-id="23488-154">It's often useful to set a specific environment for testing with an environment variable or platform setting.</span></span> <span data-ttu-id="23488-155">Jeśli środowisko nie jest ustawione, domyślnie `Production`, co spowoduje wyłączenie większości funkcji debugowania.</span><span class="sxs-lookup"><span data-stu-id="23488-155">If the environment isn't set, it defaults to `Production`, which disables most debugging features.</span></span> <span data-ttu-id="23488-156">Metoda ustawiania środowiska zależy od systemu operacyjnego.</span><span class="sxs-lookup"><span data-stu-id="23488-156">The method for setting the environment depends on the operating system.</span></span>

<span data-ttu-id="23488-157">Po skompilowaniu hosta ostatnie ustawienie środowiska odczytywane przez aplikację określa środowisko aplikacji.</span><span class="sxs-lookup"><span data-stu-id="23488-157">When the host is built, the last environment setting read by the app determines the app's environment.</span></span> <span data-ttu-id="23488-158">Nie można zmienić środowiska aplikacji, gdy aplikacja jest uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="23488-158">The app's environment can't be changed while the app is running.</span></span>

### <a name="environment-variable-or-platform-setting"></a><span data-ttu-id="23488-159">Zmienna środowiskowa lub ustawienie platformy</span><span class="sxs-lookup"><span data-stu-id="23488-159">Environment variable or platform setting</span></span>

#### <a name="azure-app-service"></a><span data-ttu-id="23488-160">Usługa Azure App Service</span><span class="sxs-lookup"><span data-stu-id="23488-160">Azure App Service</span></span>

<span data-ttu-id="23488-161">Aby ustawić środowisko w [Azure App Service](https://azure.microsoft.com/services/app-service/), wykonaj następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="23488-161">To set the environment in [Azure App Service](https://azure.microsoft.com/services/app-service/), perform the following steps:</span></span>

1. <span data-ttu-id="23488-162">Wybierz aplikację z bloku **App Services** .</span><span class="sxs-lookup"><span data-stu-id="23488-162">Select the app from the **App Services** blade.</span></span>
1. <span data-ttu-id="23488-163">W grupie **Ustawienia** wybierz blok **Ustawienia aplikacji** .</span><span class="sxs-lookup"><span data-stu-id="23488-163">In the **SETTINGS** group, select the **Application settings** blade.</span></span>
1. <span data-ttu-id="23488-164">W obszarze **Ustawienia aplikacji** wybierz pozycję **Dodaj nowe ustawienie**.</span><span class="sxs-lookup"><span data-stu-id="23488-164">In the **Application settings** area, select **Add new setting**.</span></span>
1. <span data-ttu-id="23488-165">W obszarze **Wprowadź nazwę**Podaj `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="23488-165">For **Enter a name**, provide `ASPNETCORE_ENVIRONMENT`.</span></span> <span data-ttu-id="23488-166">**Wprowadź wartość**, aby podać środowisko (na przykład `Staging`).</span><span class="sxs-lookup"><span data-stu-id="23488-166">For **Enter a value**, provide the environment (for example, `Staging`).</span></span>
1. <span data-ttu-id="23488-167">Zaznacz pole wyboru **ustawienie miejsca** , jeśli chcesz, aby ustawienie środowiska pozostawało w bieżącym gnieździe w przypadku wymiany miejsc wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="23488-167">Select the **Slot Setting** check box if you wish the environment setting to remain with the current slot when deployment slots are swapped.</span></span> <span data-ttu-id="23488-168">Aby uzyskać więcej informacji, zobacz [dokumentację platformy Azure: które ustawienia są wymieniane?](/azure/app-service/web-sites-staged-publishing).</span><span class="sxs-lookup"><span data-stu-id="23488-168">For more information, see [Azure Documentation: Which settings are swapped?](/azure/app-service/web-sites-staged-publishing).</span></span>
1. <span data-ttu-id="23488-169">Wybierz pozycję **Zapisz** w górnej części bloku.</span><span class="sxs-lookup"><span data-stu-id="23488-169">Select **Save** at the top of the blade.</span></span>

<span data-ttu-id="23488-170">Azure App Service automatycznie uruchamia ponownie aplikację po dodaniu, zmianie lub usunięciu ustawienia aplikacji (zmienna środowiskowa) w Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="23488-170">Azure App Service automatically restarts the app after an app setting (environment variable) is added, changed, or deleted in the Azure portal.</span></span>

#### <a name="windows"></a><span data-ttu-id="23488-171">Windows</span><span class="sxs-lookup"><span data-stu-id="23488-171">Windows</span></span>

<span data-ttu-id="23488-172">Aby ustawić `ASPNETCORE_ENVIRONMENT` bieżącej sesji, gdy aplikacja zostanie uruchomiona przy użyciu [uruchomienia dotnet](/dotnet/core/tools/dotnet-run), są używane następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="23488-172">To set the `ASPNETCORE_ENVIRONMENT` for the current session when the app is started using [dotnet run](/dotnet/core/tools/dotnet-run), the following commands are used:</span></span>

<span data-ttu-id="23488-173">**Wiersz polecenia**</span><span class="sxs-lookup"><span data-stu-id="23488-173">**Command prompt**</span></span>

```console
set ASPNETCORE_ENVIRONMENT=Development
```

<span data-ttu-id="23488-174">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="23488-174">**PowerShell**</span></span>

```powershell
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

<span data-ttu-id="23488-175">Te polecenia zaczną obowiązywać tylko dla bieżącego okna.</span><span class="sxs-lookup"><span data-stu-id="23488-175">These commands only take effect for the current window.</span></span> <span data-ttu-id="23488-176">Po zamknięciu okna ustawienie `ASPNETCORE_ENVIRONMENT` przywraca ustawienie domyślne lub wartość maszyny.</span><span class="sxs-lookup"><span data-stu-id="23488-176">When the window is closed, the `ASPNETCORE_ENVIRONMENT` setting reverts to the default setting or machine value.</span></span>

<span data-ttu-id="23488-177">Aby ustawić wartość globalnie w systemie Windows, użyj jednej z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="23488-177">To set the value globally in Windows, use either of the following approaches:</span></span>

* <span data-ttu-id="23488-178">Otwórz **Panel sterowania** > **system** > **Zaawansowane ustawienia systemowe** i Dodaj lub Edytuj wartość `ASPNETCORE_ENVIRONMENT`:</span><span class="sxs-lookup"><span data-stu-id="23488-178">Open the **Control Panel** > **System** > **Advanced system settings** and add or edit the `ASPNETCORE_ENVIRONMENT` value:</span></span>

  ![Zaawansowane właściwości systemu](environments/_static/systemsetting_environment.png)

  ![Zmienna środowiskowa ASPNET Core](environments/_static/windows_aspnetcore_environment.png)

* <span data-ttu-id="23488-181">Otwórz wiersz polecenia z uprawnieniami administracyjnymi i użyj polecenia `setx` lub Otwórz wiersz polecenia administracyjnych programu PowerShell i użyj `[Environment]::SetEnvironmentVariable`:</span><span class="sxs-lookup"><span data-stu-id="23488-181">Open an administrative command prompt and use the `setx` command or open an administrative PowerShell command prompt and use `[Environment]::SetEnvironmentVariable`:</span></span>

  <span data-ttu-id="23488-182">**Wiersz polecenia**</span><span class="sxs-lookup"><span data-stu-id="23488-182">**Command prompt**</span></span>

  ```console
  setx ASPNETCORE_ENVIRONMENT Development /M
  ```

  <span data-ttu-id="23488-183">Przełącznik `/M` wskazuje, aby ustawić zmienną środowiskową na poziomie systemu.</span><span class="sxs-lookup"><span data-stu-id="23488-183">The `/M` switch indicates to set the environment variable at the system level.</span></span> <span data-ttu-id="23488-184">Jeśli przełącznik `/M` nie jest używany, zmienna środowiskowa jest ustawiana dla konta użytkownika.</span><span class="sxs-lookup"><span data-stu-id="23488-184">If the `/M` switch isn't used, the environment variable is set for the user account.</span></span>

  <span data-ttu-id="23488-185">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="23488-185">**PowerShell**</span></span>

  ```powershell
  [Environment]::SetEnvironmentVariable("ASPNETCORE_ENVIRONMENT", "Development", "Machine")
  ```

  <span data-ttu-id="23488-186">Opcja `Machine` wartość wskazuje, aby ustawić zmienną środowiskową na poziomie systemu.</span><span class="sxs-lookup"><span data-stu-id="23488-186">The `Machine` option value indicates to set the environment variable at the system level.</span></span> <span data-ttu-id="23488-187">Jeśli wartość opcji zostanie zmieniona na `User`, zmienna środowiskowa jest ustawiana dla konta użytkownika.</span><span class="sxs-lookup"><span data-stu-id="23488-187">If the option value is changed to `User`, the environment variable is set for the user account.</span></span>

<span data-ttu-id="23488-188">Gdy zmienna środowiskowa `ASPNETCORE_ENVIRONMENT` jest ustawiona globalnie, zacznie obowiązywać `dotnet run` w każdym oknie polecenia otwartym po ustawieniu wartości.</span><span class="sxs-lookup"><span data-stu-id="23488-188">When the `ASPNETCORE_ENVIRONMENT` environment variable is set globally, it takes effect for `dotnet run` in any command window opened after the value is set.</span></span>

<span data-ttu-id="23488-189">**web.config**</span><span class="sxs-lookup"><span data-stu-id="23488-189">**web.config**</span></span>

<span data-ttu-id="23488-190">Aby ustawić zmienną środowiskową `ASPNETCORE_ENVIRONMENT` za pomocą *pliku Web. config*, zobacz sekcję *Ustawianie zmiennych środowiskowych* w programie <xref:host-and-deploy/aspnet-core-module#setting-environment-variables>.</span><span class="sxs-lookup"><span data-stu-id="23488-190">To set the `ASPNETCORE_ENVIRONMENT` environment variable with *web.config*, see the *Setting environment variables* section of <xref:host-and-deploy/aspnet-core-module#setting-environment-variables>.</span></span>

<span data-ttu-id="23488-191">**Plik projektu lub profil publikacji**</span><span class="sxs-lookup"><span data-stu-id="23488-191">**Project file or publish profile**</span></span>

<span data-ttu-id="23488-192">**W przypadku wdrożeń usług Windows IIS:** Uwzględnij Właściwość `<EnvironmentName>` w [profilu publikowania (pubxml)](xref:host-and-deploy/visual-studio-publish-profiles) lub pliku projektu.</span><span class="sxs-lookup"><span data-stu-id="23488-192">**For Windows IIS deployments:** Include the `<EnvironmentName>` property in the [publish profile (.pubxml)](xref:host-and-deploy/visual-studio-publish-profiles) or project file.</span></span> <span data-ttu-id="23488-193">To podejście ustawia środowisko w *pliku Web. config* po opublikowaniu projektu:</span><span class="sxs-lookup"><span data-stu-id="23488-193">This approach sets the environment in *web.config* when the project is published:</span></span>

```xml
<PropertyGroup>
  <EnvironmentName>Development</EnvironmentName>
</PropertyGroup>
```

<span data-ttu-id="23488-194">**Na pulę aplikacji IIS**</span><span class="sxs-lookup"><span data-stu-id="23488-194">**Per IIS Application Pool**</span></span>

<span data-ttu-id="23488-195">Aby ustawić zmienną środowiskową `ASPNETCORE_ENVIRONMENT` dla aplikacji uruchomionej w izolowanej puli aplikacji (obsługiwanej przez usługi IIS 10,0 lub nowszej), zobacz sekcję *polecenie Appcmd. exe* w temacie [zmienne środowiskowe &lt;environmentVariables&gt;](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) temat.</span><span class="sxs-lookup"><span data-stu-id="23488-195">To set the `ASPNETCORE_ENVIRONMENT` environment variable for an app running in an isolated Application Pool (supported on IIS 10.0 or later), see the *AppCmd.exe command* section of the [Environment Variables &lt;environmentVariables&gt;](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic.</span></span> <span data-ttu-id="23488-196">Gdy zmienna środowiskowa `ASPNETCORE_ENVIRONMENT` jest ustawiona dla puli aplikacji, jej wartość zastępuje ustawienie na poziomie systemu.</span><span class="sxs-lookup"><span data-stu-id="23488-196">When the `ASPNETCORE_ENVIRONMENT` environment variable is set for an app pool, its value overrides a setting at the system level.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="23488-197">Podczas hostowania aplikacji w usługach IIS i dodawania lub zmieniania zmiennej środowiskowej `ASPNETCORE_ENVIRONMENT` należy użyć jednego z poniższych metod, aby nowa wartość była pobierana przez aplikacje:</span><span class="sxs-lookup"><span data-stu-id="23488-197">When hosting an app in IIS and adding or changing the `ASPNETCORE_ENVIRONMENT` environment variable, use any one of the following approaches to have the new value picked up by apps:</span></span>
>
> * <span data-ttu-id="23488-198">Wykonaj `net stop was /y` a następnie `net start w3svc` z wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="23488-198">Execute `net stop was /y` followed by `net start w3svc` from a command prompt.</span></span>
> * <span data-ttu-id="23488-199">Uruchom ponownie serwer.</span><span class="sxs-lookup"><span data-stu-id="23488-199">Restart the server.</span></span>

#### <a name="macos"></a><span data-ttu-id="23488-200">macOS</span><span class="sxs-lookup"><span data-stu-id="23488-200">macOS</span></span>

<span data-ttu-id="23488-201">Ustawienie bieżącego środowiska dla macOS można wykonać w trybie online podczas uruchamiania aplikacji:</span><span class="sxs-lookup"><span data-stu-id="23488-201">Setting the current environment for macOS can be performed in-line when running the app:</span></span>

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```

<span data-ttu-id="23488-202">Alternatywnie można ustawić środowisko z `export` przed uruchomieniem aplikacji:</span><span class="sxs-lookup"><span data-stu-id="23488-202">Alternatively, set the environment with `export` prior to running the app:</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

<span data-ttu-id="23488-203">Zmienne środowiskowe na poziomie komputera są ustawiane w pliku *. bashrc* lub *. bash_profile* .</span><span class="sxs-lookup"><span data-stu-id="23488-203">Machine-level environment variables are set in the *.bashrc* or *.bash_profile* file.</span></span> <span data-ttu-id="23488-204">Edytuj plik przy użyciu dowolnego edytora tekstu.</span><span class="sxs-lookup"><span data-stu-id="23488-204">Edit the file using any text editor.</span></span> <span data-ttu-id="23488-205">Dodaj następującą instrukcję:</span><span class="sxs-lookup"><span data-stu-id="23488-205">Add the following statement:</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

#### <a name="linux"></a><span data-ttu-id="23488-206">Linux</span><span class="sxs-lookup"><span data-stu-id="23488-206">Linux</span></span>

<span data-ttu-id="23488-207">W przypadku systemu Linux dystrybucje Użyj polecenia `export` w wierszu polecenia dla ustawień zmiennych opartych na sesji i pliku *bash_profile* dla ustawień środowiska na poziomie komputera.</span><span class="sxs-lookup"><span data-stu-id="23488-207">For Linux distros, use the `export` command at a command prompt for session-based variable settings and *bash_profile* file for machine-level environment settings.</span></span>

### <a name="set-the-environment-in-code"></a><span data-ttu-id="23488-208">Ustawianie środowiska w kodzie</span><span class="sxs-lookup"><span data-stu-id="23488-208">Set the environment in code</span></span>

<span data-ttu-id="23488-209">Wywołaj <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseEnvironment*> podczas kompilowania hosta.</span><span class="sxs-lookup"><span data-stu-id="23488-209">Call <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseEnvironment*> when building the host.</span></span> <span data-ttu-id="23488-210">Zobacz <xref:fundamentals/host/generic-host#environmentname>.</span><span class="sxs-lookup"><span data-stu-id="23488-210">See <xref:fundamentals/host/generic-host#environmentname>.</span></span>


### <a name="configuration-by-environment"></a><span data-ttu-id="23488-211">Konfiguracja według środowiska</span><span class="sxs-lookup"><span data-stu-id="23488-211">Configuration by environment</span></span>

<span data-ttu-id="23488-212">Aby załadować konfigurację według środowiska, zalecamy:</span><span class="sxs-lookup"><span data-stu-id="23488-212">To load configuration by environment, we recommend:</span></span>

* <span data-ttu-id="23488-213">pliki *AppSettings* (*appSettings. { Environment}. JSON*).</span><span class="sxs-lookup"><span data-stu-id="23488-213">*appsettings* files (*appsettings.{Environment}.json*).</span></span> <span data-ttu-id="23488-214">Zobacz <xref:fundamentals/configuration/index#json-configuration-provider>.</span><span class="sxs-lookup"><span data-stu-id="23488-214">See <xref:fundamentals/configuration/index#json-configuration-provider>.</span></span>
* <span data-ttu-id="23488-215">Zmienne środowiskowe (ustawiane dla każdego systemu, w którym jest hostowana aplikacja).</span><span class="sxs-lookup"><span data-stu-id="23488-215">Environment variables (set on each system where the app is hosted).</span></span> <span data-ttu-id="23488-216">Zobacz <xref:fundamentals/host/generic-host#environmentname> i <xref:security/app-secrets#environment-variables>.</span><span class="sxs-lookup"><span data-stu-id="23488-216">See <xref:fundamentals/host/generic-host#environmentname> and <xref:security/app-secrets#environment-variables>.</span></span>
* <span data-ttu-id="23488-217">Secret Manager (tylko w środowisku programistycznym).</span><span class="sxs-lookup"><span data-stu-id="23488-217">Secret Manager (in the Development environment only).</span></span> <span data-ttu-id="23488-218">Zobacz <xref:security/app-secrets>.</span><span class="sxs-lookup"><span data-stu-id="23488-218">See <xref:security/app-secrets>.</span></span>

## <a name="environment-based-startup-class-and-methods"></a><span data-ttu-id="23488-219">Klasy i metody uruchamiania oparte na środowisku</span><span class="sxs-lookup"><span data-stu-id="23488-219">Environment-based Startup class and methods</span></span>

### <a name="inject-iwebhostenvironment-into-startupconfigure"></a><span data-ttu-id="23488-220">Wsuń IWebHostEnvironment do uruchamiania. Skonfiguruj</span><span class="sxs-lookup"><span data-stu-id="23488-220">Inject IWebHostEnvironment into Startup.Configure</span></span>

<span data-ttu-id="23488-221">Wsuń <xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment> do `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="23488-221">Inject <xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment> into `Startup.Configure`.</span></span> <span data-ttu-id="23488-222">Takie podejście jest przydatne, gdy aplikacja wymaga tylko dostosowania `Startup.Configure` w kilku środowiskach z minimalnymi różnicami kodu na środowisko.</span><span class="sxs-lookup"><span data-stu-id="23488-222">This approach is useful when the app only requires adjusting `Startup.Configure` for a few environments with minimal code differences per environment.</span></span>

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

### <a name="inject-iwebhostenvironment-into-the-startup-class"></a><span data-ttu-id="23488-223">Wsuń IWebHostEnvironment do klasy startowej</span><span class="sxs-lookup"><span data-stu-id="23488-223">Inject IWebHostEnvironment into the Startup class</span></span>

<span data-ttu-id="23488-224">Wsuń <xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment> do konstruktora `Startup`.</span><span class="sxs-lookup"><span data-stu-id="23488-224">Inject <xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment> into the `Startup` constructor.</span></span> <span data-ttu-id="23488-225">Takie podejście jest przydatne, gdy aplikacja wymaga skonfigurowania `Startup` dla kilku środowisk z minimalnymi różnicami kodu na środowisko.</span><span class="sxs-lookup"><span data-stu-id="23488-225">This approach is useful when the app requires configuring `Startup` for only a few environments with minimal code differences per environment.</span></span>

<span data-ttu-id="23488-226">W poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="23488-226">In the following example:</span></span>

* <span data-ttu-id="23488-227">Środowisko jest przechowywane w polu `_env`.</span><span class="sxs-lookup"><span data-stu-id="23488-227">The environment is held in the `_env` field.</span></span>
* <span data-ttu-id="23488-228">`_env` jest używany w `ConfigureServices` i `Configure` do zastosowania konfiguracji uruchamiania na podstawie środowiska aplikacji.</span><span class="sxs-lookup"><span data-stu-id="23488-228">`_env` is used in `ConfigureServices` and `Configure` to apply startup configuration based on the app's environment.</span></span>

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
### <a name="startup-class-conventions"></a><span data-ttu-id="23488-229">Konwencje klasy uruchamiania</span><span class="sxs-lookup"><span data-stu-id="23488-229">Startup class conventions</span></span>

<span data-ttu-id="23488-230">Po uruchomieniu aplikacji ASP.NET Core, [Klasa startowa](xref:fundamentals/startup) uruchamia aplikację.</span><span class="sxs-lookup"><span data-stu-id="23488-230">When an ASP.NET Core app starts, the [Startup class](xref:fundamentals/startup) bootstraps the app.</span></span> <span data-ttu-id="23488-231">Aplikacja może definiować oddzielne klasy `Startup` dla różnych środowisk (na przykład `StartupDevelopment`).</span><span class="sxs-lookup"><span data-stu-id="23488-231">The app can define separate `Startup` classes for different environments (for example, `StartupDevelopment`).</span></span> <span data-ttu-id="23488-232">Odpowiednia Klasa `Startup` jest wybierana w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="23488-232">The appropriate `Startup` class is selected at runtime.</span></span> <span data-ttu-id="23488-233">Kategoria, której sufiks nazwy jest zgodny z bieżącym środowiskiem, ma priorytet.</span><span class="sxs-lookup"><span data-stu-id="23488-233">The class whose name suffix matches the current environment is prioritized.</span></span> <span data-ttu-id="23488-234">Jeśli nie odnaleziono zgodnej klasy `Startup{EnvironmentName}`, używana jest Klasa `Startup`.</span><span class="sxs-lookup"><span data-stu-id="23488-234">If a matching `Startup{EnvironmentName}` class isn't found, the `Startup` class is used.</span></span> <span data-ttu-id="23488-235">Takie podejście jest przydatne, gdy aplikacja wymaga skonfigurowania uruchamiania dla kilku środowisk z wieloma różnicami kodu na środowisko.</span><span class="sxs-lookup"><span data-stu-id="23488-235">This approach is useful when the app requires configuring startup for several environments with many code differences per environment.</span></span>

<span data-ttu-id="23488-236">Aby zaimplementować klasy `Startup` oparte na środowisku, należy utworzyć klasę `Startup{EnvironmentName}` dla każdego używanego środowiska i klasy rezerwowej `Startup`:</span><span class="sxs-lookup"><span data-stu-id="23488-236">To implement environment-based `Startup` classes, create a `Startup{EnvironmentName}` class for each environment in use and a fallback `Startup` class:</span></span>

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

<span data-ttu-id="23488-237">Użyj przeciążenia [UseStartup (IWebHostBuilder, String)](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usestartup) , które akceptuje nazwę zestawu:</span><span class="sxs-lookup"><span data-stu-id="23488-237">Use the [UseStartup(IWebHostBuilder, String)](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usestartup) overload that accepts an assembly name:</span></span>

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

### <a name="startup-method-conventions"></a><span data-ttu-id="23488-238">Konwencje metody uruchamiania</span><span class="sxs-lookup"><span data-stu-id="23488-238">Startup method conventions</span></span>

<span data-ttu-id="23488-239">[Skonfiguruj](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) i [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) wersje `Configure<EnvironmentName>` i `Configure<EnvironmentName>Services`dla poszczególnych środowisk.</span><span class="sxs-lookup"><span data-stu-id="23488-239">[Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) support environment-specific versions of the form `Configure<EnvironmentName>` and `Configure<EnvironmentName>Services`.</span></span> <span data-ttu-id="23488-240">Takie podejście jest przydatne, gdy aplikacja wymaga skonfigurowania uruchamiania dla kilku środowisk z wieloma różnicami kodu na środowisko.</span><span class="sxs-lookup"><span data-stu-id="23488-240">This approach is useful when the app requires configuring startup for several environments with many code differences per environment.</span></span>

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet_all&highlight=15,42)]

## <a name="additional-resources"></a><span data-ttu-id="23488-241">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="23488-241">Additional resources</span></span>

* <xref:fundamentals/startup>
* <xref:fundamentals/configuration/index>
::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="23488-242">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="23488-242">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="23488-243">ASP.NET Core konfiguruje zachowanie aplikacji na podstawie środowiska uruchomieniowego przy użyciu zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="23488-243">ASP.NET Core configures app behavior based on the runtime environment using an environment variable.</span></span>

<span data-ttu-id="23488-244">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="23488-244">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="environments"></a><span data-ttu-id="23488-245">Środowiska</span><span class="sxs-lookup"><span data-stu-id="23488-245">Environments</span></span>

<span data-ttu-id="23488-246">ASP.NET Core odczytuje zmienną środowiskową `ASPNETCORE_ENVIRONMENT` podczas uruchamiania aplikacji i zapisuje wartość w [IHostingEnvironment. EnvironmentName](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName).</span><span class="sxs-lookup"><span data-stu-id="23488-246">ASP.NET Core reads the environment variable `ASPNETCORE_ENVIRONMENT` at app startup and stores the value in [IHostingEnvironment.EnvironmentName](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName).</span></span> <span data-ttu-id="23488-247">`ASPNETCORE_ENVIRONMENT` można ustawić na dowolną wartość, ale w strukturze są dostarczane trzy wartości:</span><span class="sxs-lookup"><span data-stu-id="23488-247">`ASPNETCORE_ENVIRONMENT` can be set to any value, but three values are provided by the framework:</span></span>

* <xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Development>
* <xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Staging>
* <span data-ttu-id="23488-248"><xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Production> (wartość domyślna)</span><span class="sxs-lookup"><span data-stu-id="23488-248"><xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Production> (default)</span></span>

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet)]

<span data-ttu-id="23488-249">Powyższy kod:</span><span class="sxs-lookup"><span data-stu-id="23488-249">The preceding code:</span></span>

* <span data-ttu-id="23488-250">Wywołuje [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) , gdy `ASPNETCORE_ENVIRONMENT` jest ustawiony na `Development`.</span><span class="sxs-lookup"><span data-stu-id="23488-250">Calls [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) when `ASPNETCORE_ENVIRONMENT` is set to `Development`.</span></span>
* <span data-ttu-id="23488-251">Wywołuje [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler) , gdy wartość `ASPNETCORE_ENVIRONMENT` jest ustawiona na jedną z następujących wartości:</span><span class="sxs-lookup"><span data-stu-id="23488-251">Calls [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler) when the value of `ASPNETCORE_ENVIRONMENT` is set one of the following:</span></span>

  * `Staging`
  * `Production`
  * `Staging_2`

<span data-ttu-id="23488-252">[Pomocnik tagu środowiska](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) używa wartości `IHostingEnvironment.EnvironmentName` do dołączania lub wykluczania znaczników w elemencie:</span><span class="sxs-lookup"><span data-stu-id="23488-252">The [Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) uses the value of `IHostingEnvironment.EnvironmentName` to include or exclude markup in the element:</span></span>

[!code-cshtml[](environments/sample-snapshot/EnvironmentsSample/Pages/About.cshtml)]

<span data-ttu-id="23488-253">W systemach Windows i macOS, zmienne środowiskowe i wartości nie uwzględniają wielkości liter.</span><span class="sxs-lookup"><span data-stu-id="23488-253">On Windows and macOS, environment variables and values aren't case sensitive.</span></span> <span data-ttu-id="23488-254">Zmienne środowiskowe systemu Linux i wartości domyślnie **uwzględniają wielkość** liter.</span><span class="sxs-lookup"><span data-stu-id="23488-254">Linux environment variables and values are **case sensitive** by default.</span></span>

### <a name="development"></a><span data-ttu-id="23488-255">Projektowanie</span><span class="sxs-lookup"><span data-stu-id="23488-255">Development</span></span>

<span data-ttu-id="23488-256">Środowisko programistyczne może włączyć funkcje, które nie powinny być ujawnione w środowisku produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="23488-256">The development environment can enable features that shouldn't be exposed in production.</span></span> <span data-ttu-id="23488-257">Na przykład szablony ASP.NET Core umożliwiają [stronie wyjątków dla deweloperów](xref:fundamentals/error-handling#developer-exception-page) w środowisku deweloperskim.</span><span class="sxs-lookup"><span data-stu-id="23488-257">For example, the ASP.NET Core templates enable the [Developer Exception Page](xref:fundamentals/error-handling#developer-exception-page) in the development environment.</span></span>

<span data-ttu-id="23488-258">Środowisko do tworzenia maszyn lokalnych można ustawić w pliku *Properties\launchSettings.JSON* projektu.</span><span class="sxs-lookup"><span data-stu-id="23488-258">The environment for local machine development can be set in the *Properties\launchSettings.json* file of the project.</span></span> <span data-ttu-id="23488-259">Wartości środowiskowe ustawione w wartościach przesłonięcia *profilu launchsettings. JSON* ustawione w środowisku systemowym.</span><span class="sxs-lookup"><span data-stu-id="23488-259">Environment values set in *launchSettings.json* override values set in the system environment.</span></span>

<span data-ttu-id="23488-260">Poniższy kod JSON przedstawia trzy profile z pliku *profilu launchsettings. JSON* :</span><span class="sxs-lookup"><span data-stu-id="23488-260">The following JSON shows three profiles from a *launchSettings.json* file:</span></span>

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
> <span data-ttu-id="23488-261">Właściwość `applicationUrl` w pliku *profilu launchsettings. JSON* może określać listę adresów URL serwera.</span><span class="sxs-lookup"><span data-stu-id="23488-261">The `applicationUrl` property in *launchSettings.json* can specify a list of server URLs.</span></span> <span data-ttu-id="23488-262">Użyj średnika między adresami URL na liście:</span><span class="sxs-lookup"><span data-stu-id="23488-262">Use a semicolon between the URLs in the list:</span></span>
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

<span data-ttu-id="23488-263">Gdy aplikacja jest [uruchamiana z uruchomieniem dotnet](/dotnet/core/tools/dotnet-run), zostanie użyty pierwszy profil z `"commandName": "Project"`.</span><span class="sxs-lookup"><span data-stu-id="23488-263">When the app is launched with [dotnet run](/dotnet/core/tools/dotnet-run), the first profile with `"commandName": "Project"` is used.</span></span> <span data-ttu-id="23488-264">Wartość `commandName` Określa serwer sieci Web do uruchomienia.</span><span class="sxs-lookup"><span data-stu-id="23488-264">The value of `commandName` specifies the web server to launch.</span></span> <span data-ttu-id="23488-265">`commandName` może być jedną z następujących:</span><span class="sxs-lookup"><span data-stu-id="23488-265">`commandName` can be any one of the following:</span></span>

* `IISExpress`
* `IIS`
* <span data-ttu-id="23488-266">`Project` (co spowoduje uruchomienie Kestrel)</span><span class="sxs-lookup"><span data-stu-id="23488-266">`Project` (which launches Kestrel)</span></span>

<span data-ttu-id="23488-267">Gdy aplikacja jest uruchamiana z [uruchomieniem dotnet](/dotnet/core/tools/dotnet-run):</span><span class="sxs-lookup"><span data-stu-id="23488-267">When an app is launched with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>

* <span data-ttu-id="23488-268">plik *profilu launchsettings. JSON* jest odczytywany, jeśli jest dostępny.</span><span class="sxs-lookup"><span data-stu-id="23488-268">*launchSettings.json* is read if available.</span></span> <span data-ttu-id="23488-269">`environmentVariables` ustawienia w zmiennych środowiskowych *profilu launchsettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="23488-269">`environmentVariables` settings in *launchSettings.json* override environment variables.</span></span>
* <span data-ttu-id="23488-270">Zostanie wyświetlone środowisko hostingu.</span><span class="sxs-lookup"><span data-stu-id="23488-270">The hosting environment is displayed.</span></span>

<span data-ttu-id="23488-271">Następujące dane wyjściowe pokazują aplikację uruchomioną z [uruchomieniem dotnet](/dotnet/core/tools/dotnet-run):</span><span class="sxs-lookup"><span data-stu-id="23488-271">The following output shows an app started with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>

```bash
PS C:\Websites\EnvironmentsSample> dotnet run
Using launch settings from C:\Websites\EnvironmentsSample\Properties\launchSettings.json...
Hosting environment: Staging
Content root path: C:\Websites\EnvironmentsSample
Now listening on: http://localhost:54340
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="23488-272">Karta **debugowanie** właściwości projektu programu Visual Studio udostępnia graficzny interfejs użytkownika służący do edytowania pliku *profilu launchsettings. JSON* :</span><span class="sxs-lookup"><span data-stu-id="23488-272">The Visual Studio project properties **Debug** tab provides a GUI to edit the *launchSettings.json* file:</span></span>

![Ustawienia właściwości projektu zmienne środowiskowe](environments/_static/project-properties-debug.png)

<span data-ttu-id="23488-274">Zmiany wprowadzone w profilach projektu mogą zacząć obowiązywać dopiero po ponownym uruchomieniu serwera sieci Web.</span><span class="sxs-lookup"><span data-stu-id="23488-274">Changes made to project profiles may not take effect until the web server is restarted.</span></span> <span data-ttu-id="23488-275">Kestrel musi zostać uruchomiona ponownie, zanim będzie mógł wykryć zmiany wprowadzone w środowisku.</span><span class="sxs-lookup"><span data-stu-id="23488-275">Kestrel must be restarted before it can detect changes made to its environment.</span></span>

> [!WARNING]
> <span data-ttu-id="23488-276">plik *profilu launchsettings. JSON* nie powinien przechowywać wpisów tajnych.</span><span class="sxs-lookup"><span data-stu-id="23488-276">*launchSettings.json* shouldn't store secrets.</span></span> <span data-ttu-id="23488-277">[Narzędzia do zarządzania kluczami tajnymi](xref:security/app-secrets) można używać do przechowywania wpisów tajnych na potrzeby lokalnego tworzenia oprogramowania.</span><span class="sxs-lookup"><span data-stu-id="23488-277">The [Secret Manager tool](xref:security/app-secrets) can be used to store secrets for local development.</span></span>

<span data-ttu-id="23488-278">W przypadku korzystania z [Visual Studio Code](https://code.visualstudio.com/)zmienne środowiskowe można ustawić w pliku *. programu vscode/Launch. JSON* .</span><span class="sxs-lookup"><span data-stu-id="23488-278">When using [Visual Studio Code](https://code.visualstudio.com/), environment variables can be set in the *.vscode/launch.json* file.</span></span> <span data-ttu-id="23488-279">Poniższy przykład ustawia środowisko na `Development`:</span><span class="sxs-lookup"><span data-stu-id="23488-279">The following example sets the environment to `Development`:</span></span>

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

<span data-ttu-id="23488-280">Plik *. programu vscode/Launch. JSON* w projekcie nie jest odczytywany podczas uruchamiania aplikacji przy użyciu `dotnet run` w taki sam sposób, jak *właściwości/profilu launchsettings. JSON*.</span><span class="sxs-lookup"><span data-stu-id="23488-280">A *.vscode/launch.json* file in the project isn't read when starting the app with `dotnet run` in the same way as *Properties/launchSettings.json*.</span></span> <span data-ttu-id="23488-281">Podczas uruchamiania aplikacji w środowisku deweloperskim, która nie ma pliku *profilu launchsettings. JSON* , należy ustawić środowisko przy użyciu zmiennej środowiskowej lub argumentu wiersza polecenia do polecenia `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="23488-281">When launching an app in development that doesn't have a *launchSettings.json* file, either set the environment with an environment variable or a command-line argument to the `dotnet run` command.</span></span>

### <a name="production"></a><span data-ttu-id="23488-282">Produkcja</span><span class="sxs-lookup"><span data-stu-id="23488-282">Production</span></span>

<span data-ttu-id="23488-283">Środowisko produkcyjne powinno być skonfigurowane w celu zmaksymalizowania zabezpieczeń, wydajności i niezawodności aplikacji.</span><span class="sxs-lookup"><span data-stu-id="23488-283">The production environment should be configured to maximize security, performance, and app robustness.</span></span> <span data-ttu-id="23488-284">Niektóre typowe ustawienia, które różnią się od programowania, to m.in.:</span><span class="sxs-lookup"><span data-stu-id="23488-284">Some common settings that differ from development include:</span></span>

* <span data-ttu-id="23488-285">Pamięć.</span><span class="sxs-lookup"><span data-stu-id="23488-285">Caching.</span></span>
* <span data-ttu-id="23488-286">Zasoby po stronie klienta są powiązane, zminimalizowanego i mogą być obsługiwane przez sieć CDN.</span><span class="sxs-lookup"><span data-stu-id="23488-286">Client-side resources are bundled, minified, and potentially served from a CDN.</span></span>
* <span data-ttu-id="23488-287">Wyłączono strony błędów diagnostyki.</span><span class="sxs-lookup"><span data-stu-id="23488-287">Diagnostic error pages disabled.</span></span>
* <span data-ttu-id="23488-288">Włączono przyjazne strony błędów.</span><span class="sxs-lookup"><span data-stu-id="23488-288">Friendly error pages enabled.</span></span>
* <span data-ttu-id="23488-289">Włączono rejestrowanie i monitorowanie produkcji.</span><span class="sxs-lookup"><span data-stu-id="23488-289">Production logging and monitoring enabled.</span></span> <span data-ttu-id="23488-290">Na przykład [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span><span class="sxs-lookup"><span data-stu-id="23488-290">For example, [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="set-the-environment"></a><span data-ttu-id="23488-291">Ustawianie środowiska</span><span class="sxs-lookup"><span data-stu-id="23488-291">Set the environment</span></span>

<span data-ttu-id="23488-292">Często warto ustawić określone środowisko do testowania przy użyciu zmiennej środowiskowej lub ustawienia platformy.</span><span class="sxs-lookup"><span data-stu-id="23488-292">It's often useful to set a specific environment for testing with an environment variable or platform setting.</span></span> <span data-ttu-id="23488-293">Jeśli środowisko nie jest ustawione, domyślnie `Production`, co spowoduje wyłączenie większości funkcji debugowania.</span><span class="sxs-lookup"><span data-stu-id="23488-293">If the environment isn't set, it defaults to `Production`, which disables most debugging features.</span></span> <span data-ttu-id="23488-294">Metoda ustawiania środowiska zależy od systemu operacyjnego.</span><span class="sxs-lookup"><span data-stu-id="23488-294">The method for setting the environment depends on the operating system.</span></span>

<span data-ttu-id="23488-295">Po skompilowaniu hosta ostatnie ustawienie środowiska odczytywane przez aplikację określa środowisko aplikacji.</span><span class="sxs-lookup"><span data-stu-id="23488-295">When the host is built, the last environment setting read by the app determines the app's environment.</span></span> <span data-ttu-id="23488-296">Nie można zmienić środowiska aplikacji, gdy aplikacja jest uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="23488-296">The app's environment can't be changed while the app is running.</span></span>

### <a name="environment-variable-or-platform-setting"></a><span data-ttu-id="23488-297">Zmienna środowiskowa lub ustawienie platformy</span><span class="sxs-lookup"><span data-stu-id="23488-297">Environment variable or platform setting</span></span>

#### <a name="azure-app-service"></a><span data-ttu-id="23488-298">Usługa Azure App Service</span><span class="sxs-lookup"><span data-stu-id="23488-298">Azure App Service</span></span>

<span data-ttu-id="23488-299">Aby ustawić środowisko w [Azure App Service](https://azure.microsoft.com/services/app-service/), wykonaj następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="23488-299">To set the environment in [Azure App Service](https://azure.microsoft.com/services/app-service/), perform the following steps:</span></span>

1. <span data-ttu-id="23488-300">Wybierz aplikację z bloku **App Services** .</span><span class="sxs-lookup"><span data-stu-id="23488-300">Select the app from the **App Services** blade.</span></span>
1. <span data-ttu-id="23488-301">W grupie **Ustawienia** wybierz blok **Ustawienia aplikacji** .</span><span class="sxs-lookup"><span data-stu-id="23488-301">In the **SETTINGS** group, select the **Application settings** blade.</span></span>
1. <span data-ttu-id="23488-302">W obszarze **Ustawienia aplikacji** wybierz pozycję **Dodaj nowe ustawienie**.</span><span class="sxs-lookup"><span data-stu-id="23488-302">In the **Application settings** area, select **Add new setting**.</span></span>
1. <span data-ttu-id="23488-303">W obszarze **Wprowadź nazwę**Podaj `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="23488-303">For **Enter a name**, provide `ASPNETCORE_ENVIRONMENT`.</span></span> <span data-ttu-id="23488-304">**Wprowadź wartość**, aby podać środowisko (na przykład `Staging`).</span><span class="sxs-lookup"><span data-stu-id="23488-304">For **Enter a value**, provide the environment (for example, `Staging`).</span></span>
1. <span data-ttu-id="23488-305">Zaznacz pole wyboru **ustawienie miejsca** , jeśli chcesz, aby ustawienie środowiska pozostawało w bieżącym gnieździe w przypadku wymiany miejsc wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="23488-305">Select the **Slot Setting** check box if you wish the environment setting to remain with the current slot when deployment slots are swapped.</span></span> <span data-ttu-id="23488-306">Aby uzyskać więcej informacji, zobacz [dokumentację platformy Azure: które ustawienia są wymieniane?](/azure/app-service/web-sites-staged-publishing).</span><span class="sxs-lookup"><span data-stu-id="23488-306">For more information, see [Azure Documentation: Which settings are swapped?](/azure/app-service/web-sites-staged-publishing).</span></span>
1. <span data-ttu-id="23488-307">Wybierz pozycję **Zapisz** w górnej części bloku.</span><span class="sxs-lookup"><span data-stu-id="23488-307">Select **Save** at the top of the blade.</span></span>

<span data-ttu-id="23488-308">Azure App Service automatycznie uruchamia ponownie aplikację po dodaniu, zmianie lub usunięciu ustawienia aplikacji (zmienna środowiskowa) w Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="23488-308">Azure App Service automatically restarts the app after an app setting (environment variable) is added, changed, or deleted in the Azure portal.</span></span>

#### <a name="windows"></a><span data-ttu-id="23488-309">Windows</span><span class="sxs-lookup"><span data-stu-id="23488-309">Windows</span></span>

<span data-ttu-id="23488-310">Aby ustawić `ASPNETCORE_ENVIRONMENT` bieżącej sesji, gdy aplikacja zostanie uruchomiona przy użyciu [uruchomienia dotnet](/dotnet/core/tools/dotnet-run), są używane następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="23488-310">To set the `ASPNETCORE_ENVIRONMENT` for the current session when the app is started using [dotnet run](/dotnet/core/tools/dotnet-run), the following commands are used:</span></span>

<span data-ttu-id="23488-311">**Wiersz polecenia**</span><span class="sxs-lookup"><span data-stu-id="23488-311">**Command prompt**</span></span>

```console
set ASPNETCORE_ENVIRONMENT=Development
```

<span data-ttu-id="23488-312">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="23488-312">**PowerShell**</span></span>

```powershell
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

<span data-ttu-id="23488-313">Te polecenia zaczną obowiązywać tylko dla bieżącego okna.</span><span class="sxs-lookup"><span data-stu-id="23488-313">These commands only take effect for the current window.</span></span> <span data-ttu-id="23488-314">Po zamknięciu okna ustawienie `ASPNETCORE_ENVIRONMENT` przywraca ustawienie domyślne lub wartość maszyny.</span><span class="sxs-lookup"><span data-stu-id="23488-314">When the window is closed, the `ASPNETCORE_ENVIRONMENT` setting reverts to the default setting or machine value.</span></span>

<span data-ttu-id="23488-315">Aby ustawić wartość globalnie w systemie Windows, użyj jednej z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="23488-315">To set the value globally in Windows, use either of the following approaches:</span></span>

* <span data-ttu-id="23488-316">Otwórz **Panel sterowania** > **system** > **Zaawansowane ustawienia systemowe** i Dodaj lub Edytuj wartość `ASPNETCORE_ENVIRONMENT`:</span><span class="sxs-lookup"><span data-stu-id="23488-316">Open the **Control Panel** > **System** > **Advanced system settings** and add or edit the `ASPNETCORE_ENVIRONMENT` value:</span></span>

  ![Zaawansowane właściwości systemu](environments/_static/systemsetting_environment.png)

  ![Zmienna środowiskowa ASPNET Core](environments/_static/windows_aspnetcore_environment.png)

* <span data-ttu-id="23488-319">Otwórz wiersz polecenia z uprawnieniami administracyjnymi i użyj polecenia `setx` lub Otwórz wiersz polecenia administracyjnych programu PowerShell i użyj `[Environment]::SetEnvironmentVariable`:</span><span class="sxs-lookup"><span data-stu-id="23488-319">Open an administrative command prompt and use the `setx` command or open an administrative PowerShell command prompt and use `[Environment]::SetEnvironmentVariable`:</span></span>

  <span data-ttu-id="23488-320">**Wiersz polecenia**</span><span class="sxs-lookup"><span data-stu-id="23488-320">**Command prompt**</span></span>

  ```console
  setx ASPNETCORE_ENVIRONMENT Development /M
  ```

  <span data-ttu-id="23488-321">Przełącznik `/M` wskazuje, aby ustawić zmienną środowiskową na poziomie systemu.</span><span class="sxs-lookup"><span data-stu-id="23488-321">The `/M` switch indicates to set the environment variable at the system level.</span></span> <span data-ttu-id="23488-322">Jeśli przełącznik `/M` nie jest używany, zmienna środowiskowa jest ustawiana dla konta użytkownika.</span><span class="sxs-lookup"><span data-stu-id="23488-322">If the `/M` switch isn't used, the environment variable is set for the user account.</span></span>

  <span data-ttu-id="23488-323">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="23488-323">**PowerShell**</span></span>

  ```powershell
  [Environment]::SetEnvironmentVariable("ASPNETCORE_ENVIRONMENT", "Development", "Machine")
  ```

  <span data-ttu-id="23488-324">Opcja `Machine` wartość wskazuje, aby ustawić zmienną środowiskową na poziomie systemu.</span><span class="sxs-lookup"><span data-stu-id="23488-324">The `Machine` option value indicates to set the environment variable at the system level.</span></span> <span data-ttu-id="23488-325">Jeśli wartość opcji zostanie zmieniona na `User`, zmienna środowiskowa jest ustawiana dla konta użytkownika.</span><span class="sxs-lookup"><span data-stu-id="23488-325">If the option value is changed to `User`, the environment variable is set for the user account.</span></span>

<span data-ttu-id="23488-326">Gdy zmienna środowiskowa `ASPNETCORE_ENVIRONMENT` jest ustawiona globalnie, zacznie obowiązywać `dotnet run` w każdym oknie polecenia otwartym po ustawieniu wartości.</span><span class="sxs-lookup"><span data-stu-id="23488-326">When the `ASPNETCORE_ENVIRONMENT` environment variable is set globally, it takes effect for `dotnet run` in any command window opened after the value is set.</span></span>

<span data-ttu-id="23488-327">**web.config**</span><span class="sxs-lookup"><span data-stu-id="23488-327">**web.config**</span></span>

<span data-ttu-id="23488-328">Aby ustawić zmienną środowiskową `ASPNETCORE_ENVIRONMENT` za pomocą *pliku Web. config*, zobacz sekcję *Ustawianie zmiennych środowiskowych* w programie <xref:host-and-deploy/aspnet-core-module#setting-environment-variables>.</span><span class="sxs-lookup"><span data-stu-id="23488-328">To set the `ASPNETCORE_ENVIRONMENT` environment variable with *web.config*, see the *Setting environment variables* section of <xref:host-and-deploy/aspnet-core-module#setting-environment-variables>.</span></span>

<span data-ttu-id="23488-329">**Plik projektu lub profil publikacji**</span><span class="sxs-lookup"><span data-stu-id="23488-329">**Project file or publish profile**</span></span>

<span data-ttu-id="23488-330">**W przypadku wdrożeń usług Windows IIS:** Uwzględnij Właściwość `<EnvironmentName>` w [profilu publikowania (pubxml)](xref:host-and-deploy/visual-studio-publish-profiles) lub pliku projektu.</span><span class="sxs-lookup"><span data-stu-id="23488-330">**For Windows IIS deployments:** Include the `<EnvironmentName>` property in the [publish profile (.pubxml)](xref:host-and-deploy/visual-studio-publish-profiles) or project file.</span></span> <span data-ttu-id="23488-331">To podejście ustawia środowisko w *pliku Web. config* po opublikowaniu projektu:</span><span class="sxs-lookup"><span data-stu-id="23488-331">This approach sets the environment in *web.config* when the project is published:</span></span>

```xml
<PropertyGroup>
  <EnvironmentName>Development</EnvironmentName>
</PropertyGroup>
```

<span data-ttu-id="23488-332">**Na pulę aplikacji IIS**</span><span class="sxs-lookup"><span data-stu-id="23488-332">**Per IIS Application Pool**</span></span>

<span data-ttu-id="23488-333">Aby ustawić zmienną środowiskową `ASPNETCORE_ENVIRONMENT` dla aplikacji uruchomionej w izolowanej puli aplikacji (obsługiwanej przez usługi IIS 10,0 lub nowszej), zobacz sekcję *polecenie Appcmd. exe* w temacie [zmienne środowiskowe &lt;environmentVariables&gt;](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) temat.</span><span class="sxs-lookup"><span data-stu-id="23488-333">To set the `ASPNETCORE_ENVIRONMENT` environment variable for an app running in an isolated Application Pool (supported on IIS 10.0 or later), see the *AppCmd.exe command* section of the [Environment Variables &lt;environmentVariables&gt;](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic.</span></span> <span data-ttu-id="23488-334">Gdy zmienna środowiskowa `ASPNETCORE_ENVIRONMENT` jest ustawiona dla puli aplikacji, jej wartość zastępuje ustawienie na poziomie systemu.</span><span class="sxs-lookup"><span data-stu-id="23488-334">When the `ASPNETCORE_ENVIRONMENT` environment variable is set for an app pool, its value overrides a setting at the system level.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="23488-335">Podczas hostowania aplikacji w usługach IIS i dodawania lub zmieniania zmiennej środowiskowej `ASPNETCORE_ENVIRONMENT` należy użyć jednego z poniższych metod, aby nowa wartość była pobierana przez aplikacje:</span><span class="sxs-lookup"><span data-stu-id="23488-335">When hosting an app in IIS and adding or changing the `ASPNETCORE_ENVIRONMENT` environment variable, use any one of the following approaches to have the new value picked up by apps:</span></span>
>
> * <span data-ttu-id="23488-336">Wykonaj `net stop was /y` a następnie `net start w3svc` z wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="23488-336">Execute `net stop was /y` followed by `net start w3svc` from a command prompt.</span></span>
> * <span data-ttu-id="23488-337">Uruchom ponownie serwer.</span><span class="sxs-lookup"><span data-stu-id="23488-337">Restart the server.</span></span>

#### <a name="macos"></a><span data-ttu-id="23488-338">macOS</span><span class="sxs-lookup"><span data-stu-id="23488-338">macOS</span></span>

<span data-ttu-id="23488-339">Ustawienie bieżącego środowiska dla macOS można wykonać w trybie online podczas uruchamiania aplikacji:</span><span class="sxs-lookup"><span data-stu-id="23488-339">Setting the current environment for macOS can be performed in-line when running the app:</span></span>

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```

<span data-ttu-id="23488-340">Alternatywnie można ustawić środowisko z `export` przed uruchomieniem aplikacji:</span><span class="sxs-lookup"><span data-stu-id="23488-340">Alternatively, set the environment with `export` prior to running the app:</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

<span data-ttu-id="23488-341">Zmienne środowiskowe na poziomie komputera są ustawiane w pliku *. bashrc* lub *. bash_profile* .</span><span class="sxs-lookup"><span data-stu-id="23488-341">Machine-level environment variables are set in the *.bashrc* or *.bash_profile* file.</span></span> <span data-ttu-id="23488-342">Edytuj plik przy użyciu dowolnego edytora tekstu.</span><span class="sxs-lookup"><span data-stu-id="23488-342">Edit the file using any text editor.</span></span> <span data-ttu-id="23488-343">Dodaj następującą instrukcję:</span><span class="sxs-lookup"><span data-stu-id="23488-343">Add the following statement:</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

#### <a name="linux"></a><span data-ttu-id="23488-344">Linux</span><span class="sxs-lookup"><span data-stu-id="23488-344">Linux</span></span>

<span data-ttu-id="23488-345">W przypadku systemu Linux dystrybucje Użyj polecenia `export` w wierszu polecenia dla ustawień zmiennych opartych na sesji i pliku *bash_profile* dla ustawień środowiska na poziomie komputera.</span><span class="sxs-lookup"><span data-stu-id="23488-345">For Linux distros, use the `export` command at a command prompt for session-based variable settings and *bash_profile* file for machine-level environment settings.</span></span>

### <a name="set-the-environment-in-code"></a><span data-ttu-id="23488-346">Ustawianie środowiska w kodzie</span><span class="sxs-lookup"><span data-stu-id="23488-346">Set the environment in code</span></span>

<span data-ttu-id="23488-347">Wywołaj <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseEnvironment*> podczas kompilowania hosta.</span><span class="sxs-lookup"><span data-stu-id="23488-347">Call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseEnvironment*> when building the host.</span></span> <span data-ttu-id="23488-348">Zobacz <xref:fundamentals/host/web-host#environment>.</span><span class="sxs-lookup"><span data-stu-id="23488-348">See <xref:fundamentals/host/web-host#environment>.</span></span>

### <a name="configuration-by-environment"></a><span data-ttu-id="23488-349">Konfiguracja według środowiska</span><span class="sxs-lookup"><span data-stu-id="23488-349">Configuration by environment</span></span>

<span data-ttu-id="23488-350">Aby załadować konfigurację według środowiska, zalecamy:</span><span class="sxs-lookup"><span data-stu-id="23488-350">To load configuration by environment, we recommend:</span></span>

* <span data-ttu-id="23488-351">pliki *AppSettings* (*appSettings. { Environment}. JSON*).</span><span class="sxs-lookup"><span data-stu-id="23488-351">*appsettings* files (*appsettings.{Environment}.json*).</span></span> <span data-ttu-id="23488-352">Zobacz <xref:fundamentals/configuration/index#json-configuration-provider>.</span><span class="sxs-lookup"><span data-stu-id="23488-352">See <xref:fundamentals/configuration/index#json-configuration-provider>.</span></span>
* <span data-ttu-id="23488-353">Zmienne środowiskowe (ustawiane dla każdego systemu, w którym jest hostowana aplikacja).</span><span class="sxs-lookup"><span data-stu-id="23488-353">Environment variables (set on each system where the app is hosted).</span></span> <span data-ttu-id="23488-354">Zobacz <xref:fundamentals/host/web-host#environment> i <xref:security/app-secrets#environment-variables>.</span><span class="sxs-lookup"><span data-stu-id="23488-354">See <xref:fundamentals/host/web-host#environment> and <xref:security/app-secrets#environment-variables>.</span></span>
* <span data-ttu-id="23488-355">Secret Manager (tylko w środowisku programistycznym).</span><span class="sxs-lookup"><span data-stu-id="23488-355">Secret Manager (in the Development environment only).</span></span> <span data-ttu-id="23488-356">Zobacz <xref:security/app-secrets>.</span><span class="sxs-lookup"><span data-stu-id="23488-356">See <xref:security/app-secrets>.</span></span>

## <a name="environment-based-startup-class-and-methods"></a><span data-ttu-id="23488-357">Klasy i metody uruchamiania oparte na środowisku</span><span class="sxs-lookup"><span data-stu-id="23488-357">Environment-based Startup class and methods</span></span>

### <a name="inject-ihostingenvironment-into-startupconfigure"></a><span data-ttu-id="23488-358">Wsuń IHostingEnvironment do uruchamiania. Skonfiguruj</span><span class="sxs-lookup"><span data-stu-id="23488-358">Inject IHostingEnvironment into Startup.Configure</span></span>

<span data-ttu-id="23488-359">Wsuń <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> do `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="23488-359">Inject <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> into `Startup.Configure`.</span></span> <span data-ttu-id="23488-360">Takie podejście jest przydatne, gdy aplikacja wymaga tylko konfigurowania `Startup.Configure` tylko dla kilku środowisk z minimalnymi różnicami w kodzie dla danego środowiska.</span><span class="sxs-lookup"><span data-stu-id="23488-360">This approach is useful when the app only requires configuring `Startup.Configure` for only a few environments with minimal code differences per environment.</span></span>

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

### <a name="inject-ihostingenvironment-into-the-startup-class"></a><span data-ttu-id="23488-361">Wsuń IHostingEnvironment do klasy startowej</span><span class="sxs-lookup"><span data-stu-id="23488-361">Inject IHostingEnvironment into the Startup class</span></span>

<span data-ttu-id="23488-362">Wsuń <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> do konstruktora `Startup` i przypisz usługę do pola do użycia w całej klasie `Startup`.</span><span class="sxs-lookup"><span data-stu-id="23488-362">Inject <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> into the `Startup` constructor and assign the service to a field for use throughout the `Startup` class.</span></span> <span data-ttu-id="23488-363">Takie podejście jest przydatne, gdy aplikacja wymaga konfiguracji uruchamiania tylko dla kilku środowisk z minimalnymi różnicami w kodzie dla danego środowiska.</span><span class="sxs-lookup"><span data-stu-id="23488-363">This approach is useful when the app requires configuring startup for only a few environments with minimal code differences per environment.</span></span>

<span data-ttu-id="23488-364">W poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="23488-364">In the following example:</span></span>

* <span data-ttu-id="23488-365">Środowisko jest przechowywane w polu `_env`.</span><span class="sxs-lookup"><span data-stu-id="23488-365">The environment is held in the `_env` field.</span></span>
* <span data-ttu-id="23488-366">`_env` jest używany w `ConfigureServices` i `Configure` do zastosowania konfiguracji uruchamiania na podstawie środowiska aplikacji.</span><span class="sxs-lookup"><span data-stu-id="23488-366">`_env` is used in `ConfigureServices` and `Configure` to apply startup configuration based on the app's environment.</span></span>

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

### <a name="startup-class-conventions"></a><span data-ttu-id="23488-367">Konwencje klasy uruchamiania</span><span class="sxs-lookup"><span data-stu-id="23488-367">Startup class conventions</span></span>

<span data-ttu-id="23488-368">Po uruchomieniu aplikacji ASP.NET Core, [Klasa startowa](xref:fundamentals/startup) uruchamia aplikację.</span><span class="sxs-lookup"><span data-stu-id="23488-368">When an ASP.NET Core app starts, the [Startup class](xref:fundamentals/startup) bootstraps the app.</span></span> <span data-ttu-id="23488-369">Aplikacja może definiować oddzielne klasy `Startup` dla różnych środowisk (na przykład `StartupDevelopment`).</span><span class="sxs-lookup"><span data-stu-id="23488-369">The app can define separate `Startup` classes for different environments (for example, `StartupDevelopment`).</span></span> <span data-ttu-id="23488-370">Odpowiednia Klasa `Startup` jest wybierana w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="23488-370">The appropriate `Startup` class is selected at runtime.</span></span> <span data-ttu-id="23488-371">Kategoria, której sufiks nazwy jest zgodny z bieżącym środowiskiem, ma priorytet.</span><span class="sxs-lookup"><span data-stu-id="23488-371">The class whose name suffix matches the current environment is prioritized.</span></span> <span data-ttu-id="23488-372">Jeśli nie odnaleziono zgodnej klasy `Startup{EnvironmentName}`, używana jest Klasa `Startup`.</span><span class="sxs-lookup"><span data-stu-id="23488-372">If a matching `Startup{EnvironmentName}` class isn't found, the `Startup` class is used.</span></span> <span data-ttu-id="23488-373">Takie podejście jest przydatne, gdy aplikacja wymaga skonfigurowania uruchamiania dla kilku środowisk z wieloma różnicami kodu na środowisko.</span><span class="sxs-lookup"><span data-stu-id="23488-373">This approach is useful when the app requires configuring startup for several environments with many code differences per environment.</span></span>

<span data-ttu-id="23488-374">Aby zaimplementować klasy `Startup` oparte na środowisku, należy utworzyć klasę `Startup{EnvironmentName}` dla każdego używanego środowiska i klasy rezerwowej `Startup`:</span><span class="sxs-lookup"><span data-stu-id="23488-374">To implement environment-based `Startup` classes, create a `Startup{EnvironmentName}` class for each environment in use and a fallback `Startup` class:</span></span>

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

<span data-ttu-id="23488-375">Użyj przeciążenia [UseStartup (IWebHostBuilder, String)](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usestartup) , które akceptuje nazwę zestawu:</span><span class="sxs-lookup"><span data-stu-id="23488-375">Use the [UseStartup(IWebHostBuilder, String)](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usestartup) overload that accepts an assembly name:</span></span>

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

### <a name="startup-method-conventions"></a><span data-ttu-id="23488-376">Konwencje metody uruchamiania</span><span class="sxs-lookup"><span data-stu-id="23488-376">Startup method conventions</span></span>

<span data-ttu-id="23488-377">[Skonfiguruj](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) i [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) wersje `Configure<EnvironmentName>` i `Configure<EnvironmentName>Services`dla poszczególnych środowisk.</span><span class="sxs-lookup"><span data-stu-id="23488-377">[Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) support environment-specific versions of the form `Configure<EnvironmentName>` and `Configure<EnvironmentName>Services`.</span></span> <span data-ttu-id="23488-378">Takie podejście jest przydatne, gdy aplikacja wymaga skonfigurowania uruchamiania dla kilku środowisk z wieloma różnicami kodu na środowisko.</span><span class="sxs-lookup"><span data-stu-id="23488-378">This approach is useful when the app requires configuring startup for several environments with many code differences per environment.</span></span>

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet_all&highlight=15,42)]

## <a name="additional-resources"></a><span data-ttu-id="23488-379">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="23488-379">Additional resources</span></span>

* <xref:fundamentals/startup>
* <xref:fundamentals/configuration/index>

::: moniker-end