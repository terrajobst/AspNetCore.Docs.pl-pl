---
title: Używanie wielu środowisk w ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak kontrolować zachowanie aplikacji w wielu środowiskach w aplikacjach ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 11/07/2019
uid: fundamentals/environments
ms.openlocfilehash: 7e49499e94fb9ea82a0ba17e4e9de05c6a2d4e98
ms.sourcegitcommit: 67116718dc33a7a01696d41af38590fdbb58e014
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/07/2019
ms.locfileid: "73799315"
---
# <a name="use-multiple-environments-in-aspnet-core"></a><span data-ttu-id="078c1-103">Używanie wielu środowisk w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="078c1-103">Use multiple environments in ASP.NET Core</span></span>

<span data-ttu-id="078c1-104">Autor [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="078c1-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="078c1-105">ASP.NET Core konfiguruje zachowanie aplikacji na podstawie środowiska uruchomieniowego przy użyciu zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="078c1-105">ASP.NET Core configures app behavior based on the runtime environment using an environment variable.</span></span>

<span data-ttu-id="078c1-106">[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="078c1-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="environments"></a><span data-ttu-id="078c1-107">Wiejski</span><span class="sxs-lookup"><span data-stu-id="078c1-107">Environments</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="078c1-108">ASP.NET Core odczytuje zmienną środowiskową `ASPNETCORE_ENVIRONMENT` podczas uruchamiania aplikacji i zapisuje wartość w [IWebHostEnvironment. EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.EnvironmentName).</span><span class="sxs-lookup"><span data-stu-id="078c1-108">ASP.NET Core reads the environment variable `ASPNETCORE_ENVIRONMENT` at app startup and stores the value in [IWebHostEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.EnvironmentName).</span></span> <span data-ttu-id="078c1-109">`ASPNETCORE_ENVIRONMENT` można ustawić na dowolną wartość, ale w strukturze są dostarczane trzy wartości:</span><span class="sxs-lookup"><span data-stu-id="078c1-109">`ASPNETCORE_ENVIRONMENT` can be set to any value, but three values are provided by the framework:</span></span>

* <xref:Microsoft.Extensions.Hosting.Environments.Development>
* <xref:Microsoft.Extensions.Hosting.Environments.Staging>
* <span data-ttu-id="078c1-110"><xref:Microsoft.Extensions.Hosting.Environments.Production> (wartość domyślna)</span><span class="sxs-lookup"><span data-stu-id="078c1-110"><xref:Microsoft.Extensions.Hosting.Environments.Production> (default)</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="078c1-111">ASP.NET Core odczytuje zmienną środowiskową `ASPNETCORE_ENVIRONMENT` podczas uruchamiania aplikacji i zapisuje wartość w [IHostingEnvironment. EnvironmentName](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName).</span><span class="sxs-lookup"><span data-stu-id="078c1-111">ASP.NET Core reads the environment variable `ASPNETCORE_ENVIRONMENT` at app startup and stores the value in [IHostingEnvironment.EnvironmentName](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName).</span></span> <span data-ttu-id="078c1-112">`ASPNETCORE_ENVIRONMENT` można ustawić na dowolną wartość, ale w strukturze są dostarczane trzy wartości:</span><span class="sxs-lookup"><span data-stu-id="078c1-112">`ASPNETCORE_ENVIRONMENT` can be set to any value, but three values are provided by the framework:</span></span>

* <xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Development>
* <xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Staging>
* <span data-ttu-id="078c1-113"><xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Production> (wartość domyślna)</span><span class="sxs-lookup"><span data-stu-id="078c1-113"><xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Production> (default)</span></span>

::: moniker-end

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet)]

<span data-ttu-id="078c1-114">Poprzedni kod:</span><span class="sxs-lookup"><span data-stu-id="078c1-114">The preceding code:</span></span>

* <span data-ttu-id="078c1-115">Wywołuje [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) , gdy `ASPNETCORE_ENVIRONMENT` jest ustawiony na `Development`.</span><span class="sxs-lookup"><span data-stu-id="078c1-115">Calls [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) when `ASPNETCORE_ENVIRONMENT` is set to `Development`.</span></span>
* <span data-ttu-id="078c1-116">Wywołuje [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler) , gdy wartość `ASPNETCORE_ENVIRONMENT` jest ustawiona na jedną z następujących wartości:</span><span class="sxs-lookup"><span data-stu-id="078c1-116">Calls [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler) when the value of `ASPNETCORE_ENVIRONMENT` is set one of the following:</span></span>

  * `Staging`
  * `Production`
  * `Staging_2`

<span data-ttu-id="078c1-117">[Pomocnik tagu środowiska](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) używa wartości `IHostingEnvironment.EnvironmentName` do dołączania lub wykluczania znaczników w elemencie:</span><span class="sxs-lookup"><span data-stu-id="078c1-117">The [Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) uses the value of `IHostingEnvironment.EnvironmentName` to include or exclude markup in the element:</span></span>

[!code-cshtml[](environments/sample-snapshot/EnvironmentsSample/Pages/About.cshtml)]

<span data-ttu-id="078c1-118">W systemach Windows i macOS, zmienne środowiskowe i wartości nie uwzględniają wielkości liter.</span><span class="sxs-lookup"><span data-stu-id="078c1-118">On Windows and macOS, environment variables and values aren't case sensitive.</span></span> <span data-ttu-id="078c1-119">Zmienne środowiskowe systemu Linux i wartości domyślnie **uwzględniają wielkość** liter.</span><span class="sxs-lookup"><span data-stu-id="078c1-119">Linux environment variables and values are **case sensitive** by default.</span></span>

### <a name="development"></a><span data-ttu-id="078c1-120">Tworzenie</span><span class="sxs-lookup"><span data-stu-id="078c1-120">Development</span></span>

<span data-ttu-id="078c1-121">Środowisko programistyczne może włączyć funkcje, które nie powinny być ujawnione w środowisku produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="078c1-121">The development environment can enable features that shouldn't be exposed in production.</span></span> <span data-ttu-id="078c1-122">Na przykład szablony ASP.NET Core umożliwiają [stronie wyjątków dla deweloperów](xref:fundamentals/error-handling#developer-exception-page) w środowisku deweloperskim.</span><span class="sxs-lookup"><span data-stu-id="078c1-122">For example, the ASP.NET Core templates enable the [Developer Exception Page](xref:fundamentals/error-handling#developer-exception-page) in the development environment.</span></span>

<span data-ttu-id="078c1-123">Środowisko do tworzenia maszyn lokalnych można ustawić w pliku *Properties\launchSettings.JSON* projektu.</span><span class="sxs-lookup"><span data-stu-id="078c1-123">The environment for local machine development can be set in the *Properties\launchSettings.json* file of the project.</span></span> <span data-ttu-id="078c1-124">Wartości środowiskowe ustawione w wartościach przesłonięcia *profilu launchsettings. JSON* ustawione w środowisku systemowym.</span><span class="sxs-lookup"><span data-stu-id="078c1-124">Environment values set in *launchSettings.json* override values set in the system environment.</span></span>

<span data-ttu-id="078c1-125">Poniższy kod JSON przedstawia trzy profile z pliku *profilu launchsettings. JSON* :</span><span class="sxs-lookup"><span data-stu-id="078c1-125">The following JSON shows three profiles from a *launchSettings.json* file:</span></span>

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
> <span data-ttu-id="078c1-126">Właściwość `applicationUrl` w pliku *profilu launchsettings. JSON* może określać listę adresów URL serwera.</span><span class="sxs-lookup"><span data-stu-id="078c1-126">The `applicationUrl` property in *launchSettings.json* can specify a list of server URLs.</span></span> <span data-ttu-id="078c1-127">Użyj średnika między adresami URL na liście:</span><span class="sxs-lookup"><span data-stu-id="078c1-127">Use a semicolon between the URLs in the list:</span></span>
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

<span data-ttu-id="078c1-128">Gdy aplikacja jest [uruchamiana z uruchomieniem dotnet](/dotnet/core/tools/dotnet-run), zostanie użyty pierwszy profil z `"commandName": "Project"`.</span><span class="sxs-lookup"><span data-stu-id="078c1-128">When the app is launched with [dotnet run](/dotnet/core/tools/dotnet-run), the first profile with `"commandName": "Project"` is used.</span></span> <span data-ttu-id="078c1-129">Wartość `commandName` Określa serwer sieci Web do uruchomienia.</span><span class="sxs-lookup"><span data-stu-id="078c1-129">The value of `commandName` specifies the web server to launch.</span></span> <span data-ttu-id="078c1-130">`commandName` może być jedną z następujących:</span><span class="sxs-lookup"><span data-stu-id="078c1-130">`commandName` can be any one of the following:</span></span>

* `IISExpress`
* `IIS`
* <span data-ttu-id="078c1-131">`Project` (co spowoduje uruchomienie Kestrel)</span><span class="sxs-lookup"><span data-stu-id="078c1-131">`Project` (which launches Kestrel)</span></span>

<span data-ttu-id="078c1-132">Gdy aplikacja jest uruchamiana z [uruchomieniem dotnet](/dotnet/core/tools/dotnet-run):</span><span class="sxs-lookup"><span data-stu-id="078c1-132">When an app is launched with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>

* <span data-ttu-id="078c1-133">plik *profilu launchsettings. JSON* jest odczytywany, jeśli jest dostępny.</span><span class="sxs-lookup"><span data-stu-id="078c1-133">*launchSettings.json* is read if available.</span></span> <span data-ttu-id="078c1-134">`environmentVariables` ustawienia w zmiennych środowiskowych *profilu launchsettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="078c1-134">`environmentVariables` settings in *launchSettings.json* override environment variables.</span></span>
* <span data-ttu-id="078c1-135">Zostanie wyświetlone środowisko hostingu.</span><span class="sxs-lookup"><span data-stu-id="078c1-135">The hosting environment is displayed.</span></span>

<span data-ttu-id="078c1-136">Następujące dane wyjściowe pokazują aplikację uruchomioną z [uruchomieniem dotnet](/dotnet/core/tools/dotnet-run):</span><span class="sxs-lookup"><span data-stu-id="078c1-136">The following output shows an app started with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>

```bash
PS C:\Websites\EnvironmentsSample> dotnet run
Using launch settings from C:\Websites\EnvironmentsSample\Properties\launchSettings.json...
Hosting environment: Staging
Content root path: C:\Websites\EnvironmentsSample
Now listening on: http://localhost:54340
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="078c1-137">Karta **debugowanie** właściwości projektu programu Visual Studio udostępnia graficzny interfejs użytkownika służący do edytowania pliku *profilu launchsettings. JSON* :</span><span class="sxs-lookup"><span data-stu-id="078c1-137">The Visual Studio project properties **Debug** tab provides a GUI to edit the *launchSettings.json* file:</span></span>

![Ustawienia właściwości projektu zmienne środowiskowe](environments/_static/project-properties-debug.png)

<span data-ttu-id="078c1-139">Zmiany wprowadzone w profilach projektu mogą zacząć obowiązywać dopiero po ponownym uruchomieniu serwera sieci Web.</span><span class="sxs-lookup"><span data-stu-id="078c1-139">Changes made to project profiles may not take effect until the web server is restarted.</span></span> <span data-ttu-id="078c1-140">Kestrel musi zostać uruchomiona ponownie, zanim będzie mógł wykryć zmiany wprowadzone w środowisku.</span><span class="sxs-lookup"><span data-stu-id="078c1-140">Kestrel must be restarted before it can detect changes made to its environment.</span></span>

> [!WARNING]
> <span data-ttu-id="078c1-141">plik *profilu launchsettings. JSON* nie powinien przechowywać wpisów tajnych.</span><span class="sxs-lookup"><span data-stu-id="078c1-141">*launchSettings.json* shouldn't store secrets.</span></span> <span data-ttu-id="078c1-142">[Narzędzia do zarządzania kluczami tajnymi](xref:security/app-secrets) można używać do przechowywania wpisów tajnych na potrzeby lokalnego tworzenia oprogramowania.</span><span class="sxs-lookup"><span data-stu-id="078c1-142">The [Secret Manager tool](xref:security/app-secrets) can be used to store secrets for local development.</span></span>

<span data-ttu-id="078c1-143">W przypadku korzystania z [Visual Studio Code](https://code.visualstudio.com/)zmienne środowiskowe można ustawić w pliku *. programu vscode/Launch. JSON* .</span><span class="sxs-lookup"><span data-stu-id="078c1-143">When using [Visual Studio Code](https://code.visualstudio.com/), environment variables can be set in the *.vscode/launch.json* file.</span></span> <span data-ttu-id="078c1-144">Poniższy przykład ustawia środowisko na `Development`:</span><span class="sxs-lookup"><span data-stu-id="078c1-144">The following example sets the environment to `Development`:</span></span>

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

<span data-ttu-id="078c1-145">Plik *. programu vscode/Launch. JSON* w projekcie nie jest odczytywany podczas uruchamiania aplikacji przy użyciu `dotnet run` w taki sam sposób, jak *właściwości/profilu launchsettings. JSON*.</span><span class="sxs-lookup"><span data-stu-id="078c1-145">A *.vscode/launch.json* file in the project isn't read when starting the app with `dotnet run` in the same way as *Properties/launchSettings.json*.</span></span> <span data-ttu-id="078c1-146">Podczas uruchamiania aplikacji w środowisku deweloperskim, która nie ma pliku *profilu launchsettings. JSON* , należy ustawić środowisko przy użyciu zmiennej środowiskowej lub argumentu wiersza polecenia do polecenia `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="078c1-146">When launching an app in development that doesn't have a *launchSettings.json* file, either set the environment with an environment variable or a command-line argument to the `dotnet run` command.</span></span>

### <a name="production"></a><span data-ttu-id="078c1-147">Narzędzi</span><span class="sxs-lookup"><span data-stu-id="078c1-147">Production</span></span>

<span data-ttu-id="078c1-148">Środowisko produkcyjne powinno być skonfigurowane w celu zmaksymalizowania zabezpieczeń, wydajności i niezawodności aplikacji.</span><span class="sxs-lookup"><span data-stu-id="078c1-148">The production environment should be configured to maximize security, performance, and app robustness.</span></span> <span data-ttu-id="078c1-149">Niektóre typowe ustawienia, które różnią się od programowania, to m.in.:</span><span class="sxs-lookup"><span data-stu-id="078c1-149">Some common settings that differ from development include:</span></span>

* <span data-ttu-id="078c1-150">Pamięć.</span><span class="sxs-lookup"><span data-stu-id="078c1-150">Caching.</span></span>
* <span data-ttu-id="078c1-151">Zasoby po stronie klienta są powiązane, zminimalizowanego i mogą być obsługiwane przez sieć CDN.</span><span class="sxs-lookup"><span data-stu-id="078c1-151">Client-side resources are bundled, minified, and potentially served from a CDN.</span></span>
* <span data-ttu-id="078c1-152">Wyłączono strony błędów diagnostyki.</span><span class="sxs-lookup"><span data-stu-id="078c1-152">Diagnostic error pages disabled.</span></span>
* <span data-ttu-id="078c1-153">Włączono przyjazne strony błędów.</span><span class="sxs-lookup"><span data-stu-id="078c1-153">Friendly error pages enabled.</span></span>
* <span data-ttu-id="078c1-154">Włączono rejestrowanie i monitorowanie produkcji.</span><span class="sxs-lookup"><span data-stu-id="078c1-154">Production logging and monitoring enabled.</span></span> <span data-ttu-id="078c1-155">Na przykład [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span><span class="sxs-lookup"><span data-stu-id="078c1-155">For example, [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="set-the-environment"></a><span data-ttu-id="078c1-156">Ustawianie środowiska</span><span class="sxs-lookup"><span data-stu-id="078c1-156">Set the environment</span></span>

<span data-ttu-id="078c1-157">Często warto ustawić określone środowisko do testowania przy użyciu zmiennej środowiskowej lub ustawienia platformy.</span><span class="sxs-lookup"><span data-stu-id="078c1-157">It's often useful to set a specific environment for testing with an environment variable or platform setting.</span></span> <span data-ttu-id="078c1-158">Jeśli środowisko nie jest ustawione, domyślnie `Production`, co spowoduje wyłączenie większości funkcji debugowania.</span><span class="sxs-lookup"><span data-stu-id="078c1-158">If the environment isn't set, it defaults to `Production`, which disables most debugging features.</span></span> <span data-ttu-id="078c1-159">Metoda ustawiania środowiska zależy od systemu operacyjnego.</span><span class="sxs-lookup"><span data-stu-id="078c1-159">The method for setting the environment depends on the operating system.</span></span>

<span data-ttu-id="078c1-160">Po skompilowaniu hosta ostatnie ustawienie środowiska odczytywane przez aplikację określa środowisko aplikacji.</span><span class="sxs-lookup"><span data-stu-id="078c1-160">When the host is built, the last environment setting read by the app determines the app's environment.</span></span> <span data-ttu-id="078c1-161">Nie można zmienić środowiska aplikacji, gdy aplikacja jest uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="078c1-161">The app's environment can't be changed while the app is running.</span></span>

### <a name="environment-variable-or-platform-setting"></a><span data-ttu-id="078c1-162">Zmienna środowiskowa lub ustawienie platformy</span><span class="sxs-lookup"><span data-stu-id="078c1-162">Environment variable or platform setting</span></span>

#### <a name="azure-app-service"></a><span data-ttu-id="078c1-163">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="078c1-163">Azure App Service</span></span>

<span data-ttu-id="078c1-164">Aby ustawić środowisko w [Azure App Service](https://azure.microsoft.com/services/app-service/), wykonaj następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="078c1-164">To set the environment in [Azure App Service](https://azure.microsoft.com/services/app-service/), perform the following steps:</span></span>

1. <span data-ttu-id="078c1-165">Wybierz aplikację z bloku **App Services** .</span><span class="sxs-lookup"><span data-stu-id="078c1-165">Select the app from the **App Services** blade.</span></span>
1. <span data-ttu-id="078c1-166">W grupie **Ustawienia** wybierz blok **Ustawienia aplikacji** .</span><span class="sxs-lookup"><span data-stu-id="078c1-166">In the **SETTINGS** group, select the **Application settings** blade.</span></span>
1. <span data-ttu-id="078c1-167">W obszarze **Ustawienia aplikacji** wybierz pozycję **Dodaj nowe ustawienie**.</span><span class="sxs-lookup"><span data-stu-id="078c1-167">In the **Application settings** area, select **Add new setting**.</span></span>
1. <span data-ttu-id="078c1-168">W obszarze **Wprowadź nazwę**Podaj `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="078c1-168">For **Enter a name**, provide `ASPNETCORE_ENVIRONMENT`.</span></span> <span data-ttu-id="078c1-169">**Wprowadź wartość**, aby podać środowisko (na przykład `Staging`).</span><span class="sxs-lookup"><span data-stu-id="078c1-169">For **Enter a value**, provide the environment (for example, `Staging`).</span></span>
1. <span data-ttu-id="078c1-170">Zaznacz pole wyboru **ustawienie miejsca** , jeśli chcesz, aby ustawienie środowiska pozostawało w bieżącym gnieździe w przypadku wymiany miejsc wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="078c1-170">Select the **Slot Setting** check box if you wish the environment setting to remain with the current slot when deployment slots are swapped.</span></span> <span data-ttu-id="078c1-171">Aby uzyskać więcej informacji, zobacz [dokumentację platformy Azure: które ustawienia są wymieniane?](/azure/app-service/web-sites-staged-publishing).</span><span class="sxs-lookup"><span data-stu-id="078c1-171">For more information, see [Azure Documentation: Which settings are swapped?](/azure/app-service/web-sites-staged-publishing).</span></span>
1. <span data-ttu-id="078c1-172">Wybierz pozycję **Zapisz** w górnej części bloku.</span><span class="sxs-lookup"><span data-stu-id="078c1-172">Select **Save** at the top of the blade.</span></span>

<span data-ttu-id="078c1-173">Azure App Service automatycznie uruchamia ponownie aplikację po dodaniu, zmianie lub usunięciu ustawienia aplikacji (zmienna środowiskowa) w Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="078c1-173">Azure App Service automatically restarts the app after an app setting (environment variable) is added, changed, or deleted in the Azure portal.</span></span>

#### <a name="windows"></a><span data-ttu-id="078c1-174">Windows</span><span class="sxs-lookup"><span data-stu-id="078c1-174">Windows</span></span>

<span data-ttu-id="078c1-175">Aby ustawić `ASPNETCORE_ENVIRONMENT` bieżącej sesji, gdy aplikacja zostanie uruchomiona przy użyciu [uruchomienia dotnet](/dotnet/core/tools/dotnet-run), są używane następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="078c1-175">To set the `ASPNETCORE_ENVIRONMENT` for the current session when the app is started using [dotnet run](/dotnet/core/tools/dotnet-run), the following commands are used:</span></span>

<span data-ttu-id="078c1-176">**Wiersz polecenia**</span><span class="sxs-lookup"><span data-stu-id="078c1-176">**Command prompt**</span></span>

```console
set ASPNETCORE_ENVIRONMENT=Development
```

<span data-ttu-id="078c1-177">**Narzędzia**</span><span class="sxs-lookup"><span data-stu-id="078c1-177">**PowerShell**</span></span>

```powershell
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

<span data-ttu-id="078c1-178">Te polecenia zaczną obowiązywać tylko dla bieżącego okna.</span><span class="sxs-lookup"><span data-stu-id="078c1-178">These commands only take effect for the current window.</span></span> <span data-ttu-id="078c1-179">Po zamknięciu okna ustawienie `ASPNETCORE_ENVIRONMENT` przywraca ustawienie domyślne lub wartość maszyny.</span><span class="sxs-lookup"><span data-stu-id="078c1-179">When the window is closed, the `ASPNETCORE_ENVIRONMENT` setting reverts to the default setting or machine value.</span></span>

<span data-ttu-id="078c1-180">Aby ustawić wartość globalnie w systemie Windows, użyj jednej z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="078c1-180">To set the value globally in Windows, use either of the following approaches:</span></span>

* <span data-ttu-id="078c1-181">Otwórz **Panel sterowania** > **system** > **Zaawansowane ustawienia systemowe** i Dodaj lub Edytuj wartość `ASPNETCORE_ENVIRONMENT`:</span><span class="sxs-lookup"><span data-stu-id="078c1-181">Open the **Control Panel** > **System** > **Advanced system settings** and add or edit the `ASPNETCORE_ENVIRONMENT` value:</span></span>

  ![Zaawansowane właściwości systemu](environments/_static/systemsetting_environment.png)

  ![Zmienna środowiskowa ASPNET Core](environments/_static/windows_aspnetcore_environment.png)

* <span data-ttu-id="078c1-184">Otwórz wiersz polecenia z uprawnieniami administracyjnymi i użyj polecenia `setx` lub Otwórz wiersz polecenia administracyjnych programu PowerShell i użyj `[Environment]::SetEnvironmentVariable`:</span><span class="sxs-lookup"><span data-stu-id="078c1-184">Open an administrative command prompt and use the `setx` command or open an administrative PowerShell command prompt and use `[Environment]::SetEnvironmentVariable`:</span></span>

  <span data-ttu-id="078c1-185">**Wiersz polecenia**</span><span class="sxs-lookup"><span data-stu-id="078c1-185">**Command prompt**</span></span>

  ```console
  setx ASPNETCORE_ENVIRONMENT Development /M
  ```

  <span data-ttu-id="078c1-186">Przełącznik `/M` wskazuje, aby ustawić zmienną środowiskową na poziomie systemu.</span><span class="sxs-lookup"><span data-stu-id="078c1-186">The `/M` switch indicates to set the environment variable at the system level.</span></span> <span data-ttu-id="078c1-187">Jeśli przełącznik `/M` nie jest używany, zmienna środowiskowa jest ustawiana dla konta użytkownika.</span><span class="sxs-lookup"><span data-stu-id="078c1-187">If the `/M` switch isn't used, the environment variable is set for the user account.</span></span>

  <span data-ttu-id="078c1-188">**Narzędzia**</span><span class="sxs-lookup"><span data-stu-id="078c1-188">**PowerShell**</span></span>

  ```powershell
  [Environment]::SetEnvironmentVariable("ASPNETCORE_ENVIRONMENT", "Development", "Machine")
  ```

  <span data-ttu-id="078c1-189">Opcja `Machine` wartość wskazuje, aby ustawić zmienną środowiskową na poziomie systemu.</span><span class="sxs-lookup"><span data-stu-id="078c1-189">The `Machine` option value indicates to set the environment variable at the system level.</span></span> <span data-ttu-id="078c1-190">Jeśli wartość opcji zostanie zmieniona na `User`, zmienna środowiskowa jest ustawiana dla konta użytkownika.</span><span class="sxs-lookup"><span data-stu-id="078c1-190">If the option value is changed to `User`, the environment variable is set for the user account.</span></span>

<span data-ttu-id="078c1-191">Gdy zmienna środowiskowa `ASPNETCORE_ENVIRONMENT` jest ustawiona globalnie, zacznie obowiązywać `dotnet run` w każdym oknie polecenia otwartym po ustawieniu wartości.</span><span class="sxs-lookup"><span data-stu-id="078c1-191">When the `ASPNETCORE_ENVIRONMENT` environment variable is set globally, it takes effect for `dotnet run` in any command window opened after the value is set.</span></span>

<span data-ttu-id="078c1-192">**plik Web. config**</span><span class="sxs-lookup"><span data-stu-id="078c1-192">**web.config**</span></span>

<span data-ttu-id="078c1-193">Aby ustawić zmienną środowiskową `ASPNETCORE_ENVIRONMENT` za pomocą *pliku Web. config*, zobacz sekcję *Ustawianie zmiennych środowiskowych* w programie <xref:host-and-deploy/aspnet-core-module#setting-environment-variables>.</span><span class="sxs-lookup"><span data-stu-id="078c1-193">To set the `ASPNETCORE_ENVIRONMENT` environment variable with *web.config*, see the *Setting environment variables* section of <xref:host-and-deploy/aspnet-core-module#setting-environment-variables>.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="078c1-194">**Plik projektu lub profil publikacji**</span><span class="sxs-lookup"><span data-stu-id="078c1-194">**Project file or publish profile**</span></span>

<span data-ttu-id="078c1-195">**W przypadku wdrożeń usług Windows IIS:** Uwzględnij Właściwość `<EnvironmentName>` w [profilu publikowania (pubxml)](xref:host-and-deploy/visual-studio-publish-profiles) lub pliku projektu.</span><span class="sxs-lookup"><span data-stu-id="078c1-195">**For Windows IIS deployments:** Include the `<EnvironmentName>` property in the [publish profile (.pubxml)](xref:host-and-deploy/visual-studio-publish-profiles) or project file.</span></span> <span data-ttu-id="078c1-196">To podejście ustawia środowisko w *pliku Web. config* po opublikowaniu projektu:</span><span class="sxs-lookup"><span data-stu-id="078c1-196">This approach sets the environment in *web.config* when the project is published:</span></span>

```xml
<PropertyGroup>
  <EnvironmentName>Development</EnvironmentName>
</PropertyGroup>
```

::: moniker-end

<span data-ttu-id="078c1-197">**Na pulę aplikacji IIS**</span><span class="sxs-lookup"><span data-stu-id="078c1-197">**Per IIS Application Pool**</span></span>

<span data-ttu-id="078c1-198">Aby ustawić zmienną środowiskową `ASPNETCORE_ENVIRONMENT` dla aplikacji uruchomionej w izolowanej puli aplikacji (obsługiwanej przez usługi IIS 10,0 lub nowszej), zobacz sekcję *polecenie Appcmd. exe* w temacie [zmienne środowiskowe &lt;environmentVariables&gt;](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) temat.</span><span class="sxs-lookup"><span data-stu-id="078c1-198">To set the `ASPNETCORE_ENVIRONMENT` environment variable for an app running in an isolated Application Pool (supported on IIS 10.0 or later), see the *AppCmd.exe command* section of the [Environment Variables &lt;environmentVariables&gt;](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic.</span></span> <span data-ttu-id="078c1-199">Gdy zmienna środowiskowa `ASPNETCORE_ENVIRONMENT` jest ustawiona dla puli aplikacji, jej wartość zastępuje ustawienie na poziomie systemu.</span><span class="sxs-lookup"><span data-stu-id="078c1-199">When the `ASPNETCORE_ENVIRONMENT` environment variable is set for an app pool, its value overrides a setting at the system level.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="078c1-200">Podczas hostowania aplikacji w usługach IIS i dodawania lub zmieniania zmiennej środowiskowej `ASPNETCORE_ENVIRONMENT` należy użyć jednego z poniższych metod, aby nowa wartość była pobierana przez aplikacje:</span><span class="sxs-lookup"><span data-stu-id="078c1-200">When hosting an app in IIS and adding or changing the `ASPNETCORE_ENVIRONMENT` environment variable, use any one of the following approaches to have the new value picked up by apps:</span></span>
>
> * <span data-ttu-id="078c1-201">Wykonaj `net stop was /y` a następnie `net start w3svc` z wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="078c1-201">Execute `net stop was /y` followed by `net start w3svc` from a command prompt.</span></span>
> * <span data-ttu-id="078c1-202">Uruchom ponownie serwer.</span><span class="sxs-lookup"><span data-stu-id="078c1-202">Restart the server.</span></span>

#### <a name="macos"></a><span data-ttu-id="078c1-203">macOS</span><span class="sxs-lookup"><span data-stu-id="078c1-203">macOS</span></span>

<span data-ttu-id="078c1-204">Ustawienie bieżącego środowiska dla macOS można wykonać w trybie online podczas uruchamiania aplikacji:</span><span class="sxs-lookup"><span data-stu-id="078c1-204">Setting the current environment for macOS can be performed in-line when running the app:</span></span>

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```

<span data-ttu-id="078c1-205">Alternatywnie można ustawić środowisko z `export` przed uruchomieniem aplikacji:</span><span class="sxs-lookup"><span data-stu-id="078c1-205">Alternatively, set the environment with `export` prior to running the app:</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

<span data-ttu-id="078c1-206">Zmienne środowiskowe na poziomie komputera są ustawiane w pliku *. bashrc* lub *. bash_profile* .</span><span class="sxs-lookup"><span data-stu-id="078c1-206">Machine-level environment variables are set in the *.bashrc* or *.bash_profile* file.</span></span> <span data-ttu-id="078c1-207">Edytuj plik przy użyciu dowolnego edytora tekstu.</span><span class="sxs-lookup"><span data-stu-id="078c1-207">Edit the file using any text editor.</span></span> <span data-ttu-id="078c1-208">Dodaj następującą instrukcję:</span><span class="sxs-lookup"><span data-stu-id="078c1-208">Add the following statement:</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

#### <a name="linux"></a><span data-ttu-id="078c1-209">Linux</span><span class="sxs-lookup"><span data-stu-id="078c1-209">Linux</span></span>

<span data-ttu-id="078c1-210">W przypadku systemu Linux dystrybucje Użyj polecenia `export` w wierszu polecenia dla ustawień zmiennych opartych na sesji i pliku *bash_profile* dla ustawień środowiska na poziomie komputera.</span><span class="sxs-lookup"><span data-stu-id="078c1-210">For Linux distros, use the `export` command at a command prompt for session-based variable settings and *bash_profile* file for machine-level environment settings.</span></span>

### <a name="set-the-environment-in-code"></a><span data-ttu-id="078c1-211">Ustawianie środowiska w kodzie</span><span class="sxs-lookup"><span data-stu-id="078c1-211">Set the environment in code</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="078c1-212">Wywołaj <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseEnvironment*> podczas kompilowania hosta.</span><span class="sxs-lookup"><span data-stu-id="078c1-212">Call <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseEnvironment*> when building the host.</span></span> <span data-ttu-id="078c1-213">Zobacz <xref:fundamentals/host/generic-host#environmentname>.</span><span class="sxs-lookup"><span data-stu-id="078c1-213">See <xref:fundamentals/host/generic-host#environmentname>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="078c1-214">Wywołaj <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseEnvironment*> podczas kompilowania hosta.</span><span class="sxs-lookup"><span data-stu-id="078c1-214">Call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseEnvironment*> when building the host.</span></span> <span data-ttu-id="078c1-215">Zobacz <xref:fundamentals/host/web-host#environment>.</span><span class="sxs-lookup"><span data-stu-id="078c1-215">See <xref:fundamentals/host/web-host#environment>.</span></span>

::: moniker-end

### <a name="configuration-by-environment"></a><span data-ttu-id="078c1-216">Konfiguracja według środowiska</span><span class="sxs-lookup"><span data-stu-id="078c1-216">Configuration by environment</span></span>

<span data-ttu-id="078c1-217">Aby załadować konfigurację według środowiska, zalecamy:</span><span class="sxs-lookup"><span data-stu-id="078c1-217">To load configuration by environment, we recommend:</span></span>

::: moniker range=">= aspnetcore-3.0"

* <span data-ttu-id="078c1-218">pliki *AppSettings* (*appSettings. { Environment}. JSON*).</span><span class="sxs-lookup"><span data-stu-id="078c1-218">*appsettings* files (*appsettings.{Environment}.json*).</span></span> <span data-ttu-id="078c1-219">Zobacz <xref:fundamentals/configuration/index#json-configuration-provider>.</span><span class="sxs-lookup"><span data-stu-id="078c1-219">See <xref:fundamentals/configuration/index#json-configuration-provider>.</span></span>
* <span data-ttu-id="078c1-220">Zmienne środowiskowe (ustawiane dla każdego systemu, w którym jest hostowana aplikacja).</span><span class="sxs-lookup"><span data-stu-id="078c1-220">Environment variables (set on each system where the app is hosted).</span></span> <span data-ttu-id="078c1-221">Zobacz <xref:fundamentals/host/generic-host#environmentname> i <xref:security/app-secrets#environment-variables>.</span><span class="sxs-lookup"><span data-stu-id="078c1-221">See <xref:fundamentals/host/generic-host#environmentname> and <xref:security/app-secrets#environment-variables>.</span></span>
* <span data-ttu-id="078c1-222">Secret Manager (tylko w środowisku programistycznym).</span><span class="sxs-lookup"><span data-stu-id="078c1-222">Secret Manager (in the Development environment only).</span></span> <span data-ttu-id="078c1-223">Zobacz <xref:security/app-secrets>.</span><span class="sxs-lookup"><span data-stu-id="078c1-223">See <xref:security/app-secrets>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* <span data-ttu-id="078c1-224">pliki *AppSettings* (*appSettings. { Environment}. JSON*).</span><span class="sxs-lookup"><span data-stu-id="078c1-224">*appsettings* files (*appsettings.{Environment}.json*).</span></span> <span data-ttu-id="078c1-225">Zobacz <xref:fundamentals/configuration/index#json-configuration-provider>.</span><span class="sxs-lookup"><span data-stu-id="078c1-225">See <xref:fundamentals/configuration/index#json-configuration-provider>.</span></span>
* <span data-ttu-id="078c1-226">Zmienne środowiskowe (ustawiane dla każdego systemu, w którym jest hostowana aplikacja).</span><span class="sxs-lookup"><span data-stu-id="078c1-226">Environment variables (set on each system where the app is hosted).</span></span> <span data-ttu-id="078c1-227">Zobacz <xref:fundamentals/host/web-host#environment> i <xref:security/app-secrets#environment-variables>.</span><span class="sxs-lookup"><span data-stu-id="078c1-227">See <xref:fundamentals/host/web-host#environment> and <xref:security/app-secrets#environment-variables>.</span></span>
* <span data-ttu-id="078c1-228">Secret Manager (tylko w środowisku programistycznym).</span><span class="sxs-lookup"><span data-stu-id="078c1-228">Secret Manager (in the Development environment only).</span></span> <span data-ttu-id="078c1-229">Zobacz <xref:security/app-secrets>.</span><span class="sxs-lookup"><span data-stu-id="078c1-229">See <xref:security/app-secrets>.</span></span>

::: moniker-end

## <a name="environment-based-startup-class-and-methods"></a><span data-ttu-id="078c1-230">Klasy i metody uruchamiania oparte na środowisku</span><span class="sxs-lookup"><span data-stu-id="078c1-230">Environment-based Startup class and methods</span></span>

::: moniker range=">= aspnetcore-3.0"

### <a name="inject-iwebhostenvironment-into-startupconfigure"></a><span data-ttu-id="078c1-231">Wsuń IWebHostEnvironment do uruchamiania. Skonfiguruj</span><span class="sxs-lookup"><span data-stu-id="078c1-231">Inject IWebHostEnvironment into Startup.Configure</span></span>

<span data-ttu-id="078c1-232">Wsuń <xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment> do `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="078c1-232">Inject <xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment> into `Startup.Configure`.</span></span> <span data-ttu-id="078c1-233">Takie podejście jest przydatne, gdy aplikacja wymaga tylko dostosowania `Startup.Configure` w kilku środowiskach z minimalnymi różnicami kodu na środowisko.</span><span class="sxs-lookup"><span data-stu-id="078c1-233">This approach is useful when the app only requires adjusting `Startup.Configure` for a few environments with minimal code differences per environment.</span></span>

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

### <a name="inject-iwebhostenvironment-into-the-startup-class"></a><span data-ttu-id="078c1-234">Wsuń IWebHostEnvironment do klasy startowej</span><span class="sxs-lookup"><span data-stu-id="078c1-234">Inject IWebHostEnvironment into the Startup class</span></span>

<span data-ttu-id="078c1-235">Wsuń <xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment> do konstruktora `Startup`.</span><span class="sxs-lookup"><span data-stu-id="078c1-235">Inject <xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment> into the `Startup` constructor.</span></span> <span data-ttu-id="078c1-236">Takie podejście jest przydatne, gdy aplikacja wymaga skonfigurowania `Startup` dla kilku środowisk z minimalnymi różnicami kodu na środowisko.</span><span class="sxs-lookup"><span data-stu-id="078c1-236">This approach is useful when the app requires configuring `Startup` for only a few environments with minimal code differences per environment.</span></span>

<span data-ttu-id="078c1-237">W poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="078c1-237">In the following example:</span></span>

* <span data-ttu-id="078c1-238">Środowisko jest przechowywane w polu `_env`.</span><span class="sxs-lookup"><span data-stu-id="078c1-238">The environment is held in the `_env` field.</span></span>
* <span data-ttu-id="078c1-239">`_env` jest używany w `ConfigureServices` i `Configure` do zastosowania konfiguracji uruchamiania na podstawie środowiska aplikacji.</span><span class="sxs-lookup"><span data-stu-id="078c1-239">`_env` is used in `ConfigureServices` and `Configure` to apply startup configuration based on the app's environment.</span></span>

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

::: moniker-end

::: moniker range="< aspnetcore-3.0"

### <a name="inject-ihostingenvironment-into-startupconfigure"></a><span data-ttu-id="078c1-240">Wsuń IHostingEnvironment do uruchamiania. Skonfiguruj</span><span class="sxs-lookup"><span data-stu-id="078c1-240">Inject IHostingEnvironment into Startup.Configure</span></span>

<span data-ttu-id="078c1-241">Wsuń <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> do `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="078c1-241">Inject <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> into `Startup.Configure`.</span></span> <span data-ttu-id="078c1-242">Takie podejście jest przydatne, gdy aplikacja wymaga tylko konfigurowania `Startup.Configure` tylko dla kilku środowisk z minimalnymi różnicami w kodzie dla danego środowiska.</span><span class="sxs-lookup"><span data-stu-id="078c1-242">This approach is useful when the app only requires configuring `Startup.Configure` for only a few environments with minimal code differences per environment.</span></span>

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

### <a name="inject-ihostingenvironment-into-the-startup-class"></a><span data-ttu-id="078c1-243">Wsuń IHostingEnvironment do klasy startowej</span><span class="sxs-lookup"><span data-stu-id="078c1-243">Inject IHostingEnvironment into the Startup class</span></span>

<span data-ttu-id="078c1-244">Wsuń <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> do konstruktora `Startup` i przypisz usługę do pola do użycia w całej klasie `Startup`.</span><span class="sxs-lookup"><span data-stu-id="078c1-244">Inject <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> into the `Startup` constructor and assign the service to a field for use throughout the `Startup` class.</span></span> <span data-ttu-id="078c1-245">Takie podejście jest przydatne, gdy aplikacja wymaga konfiguracji uruchamiania tylko dla kilku środowisk z minimalnymi różnicami w kodzie dla danego środowiska.</span><span class="sxs-lookup"><span data-stu-id="078c1-245">This approach is useful when the app requires configuring startup for only a few environments with minimal code differences per environment.</span></span>

<span data-ttu-id="078c1-246">W poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="078c1-246">In the following example:</span></span>

* <span data-ttu-id="078c1-247">Środowisko jest przechowywane w polu `_env`.</span><span class="sxs-lookup"><span data-stu-id="078c1-247">The environment is held in the `_env` field.</span></span>
* <span data-ttu-id="078c1-248">`_env` jest używany w `ConfigureServices` i `Configure` do zastosowania konfiguracji uruchamiania na podstawie środowiska aplikacji.</span><span class="sxs-lookup"><span data-stu-id="078c1-248">`_env` is used in `ConfigureServices` and `Configure` to apply startup configuration based on the app's environment.</span></span>

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

::: moniker-end

### <a name="startup-class-conventions"></a><span data-ttu-id="078c1-249">Konwencje klasy uruchamiania</span><span class="sxs-lookup"><span data-stu-id="078c1-249">Startup class conventions</span></span>

<span data-ttu-id="078c1-250">Po uruchomieniu aplikacji ASP.NET Core, [Klasa startowa](xref:fundamentals/startup) uruchamia aplikację.</span><span class="sxs-lookup"><span data-stu-id="078c1-250">When an ASP.NET Core app starts, the [Startup class](xref:fundamentals/startup) bootstraps the app.</span></span> <span data-ttu-id="078c1-251">Aplikacja może definiować oddzielne klasy `Startup` dla różnych środowisk (na przykład `StartupDevelopment`).</span><span class="sxs-lookup"><span data-stu-id="078c1-251">The app can define separate `Startup` classes for different environments (for example, `StartupDevelopment`).</span></span> <span data-ttu-id="078c1-252">Odpowiednia Klasa `Startup` jest wybierana w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="078c1-252">The appropriate `Startup` class is selected at runtime.</span></span> <span data-ttu-id="078c1-253">Kategoria, której sufiks nazwy jest zgodny z bieżącym środowiskiem, ma priorytet.</span><span class="sxs-lookup"><span data-stu-id="078c1-253">The class whose name suffix matches the current environment is prioritized.</span></span> <span data-ttu-id="078c1-254">Jeśli nie odnaleziono zgodnej klasy `Startup{EnvironmentName}`, używana jest Klasa `Startup`.</span><span class="sxs-lookup"><span data-stu-id="078c1-254">If a matching `Startup{EnvironmentName}` class isn't found, the `Startup` class is used.</span></span> <span data-ttu-id="078c1-255">Takie podejście jest przydatne, gdy aplikacja wymaga skonfigurowania uruchamiania dla kilku środowisk z wieloma różnicami kodu na środowisko.</span><span class="sxs-lookup"><span data-stu-id="078c1-255">This approach is useful when the app requires configuring startup for several environments with many code differences per environment.</span></span>

<span data-ttu-id="078c1-256">Aby zaimplementować klasy `Startup` oparte na środowisku, należy utworzyć klasę `Startup{EnvironmentName}` dla każdego używanego środowiska i klasy rezerwowej `Startup`:</span><span class="sxs-lookup"><span data-stu-id="078c1-256">To implement environment-based `Startup` classes, create a `Startup{EnvironmentName}` class for each environment in use and a fallback `Startup` class:</span></span>

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

<span data-ttu-id="078c1-257">Użyj przeciążenia [UseStartup (IWebHostBuilder, String)](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usestartup) , które akceptuje nazwę zestawu:</span><span class="sxs-lookup"><span data-stu-id="078c1-257">Use the [UseStartup(IWebHostBuilder, String)](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usestartup) overload that accepts an assembly name:</span></span>

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

### <a name="startup-method-conventions"></a><span data-ttu-id="078c1-258">Konwencje metody uruchamiania</span><span class="sxs-lookup"><span data-stu-id="078c1-258">Startup method conventions</span></span>

<span data-ttu-id="078c1-259">[Skonfiguruj](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) i [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) wersje `Configure<EnvironmentName>` i `Configure<EnvironmentName>Services`dla poszczególnych środowisk.</span><span class="sxs-lookup"><span data-stu-id="078c1-259">[Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) support environment-specific versions of the form `Configure<EnvironmentName>` and `Configure<EnvironmentName>Services`.</span></span> <span data-ttu-id="078c1-260">Takie podejście jest przydatne, gdy aplikacja wymaga skonfigurowania uruchamiania dla kilku środowisk z wieloma różnicami kodu na środowisko.</span><span class="sxs-lookup"><span data-stu-id="078c1-260">This approach is useful when the app requires configuring startup for several environments with many code differences per environment.</span></span>

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet_all&highlight=15,42)]

## <a name="additional-resources"></a><span data-ttu-id="078c1-261">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="078c1-261">Additional resources</span></span>

* <xref:fundamentals/startup>
* <xref:fundamentals/configuration/index>
