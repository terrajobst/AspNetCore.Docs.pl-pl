---
title: Używanie wielu środowisk w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak kontrolować zachowanie aplikacji w wielu środowiskach, w aplikacji platformy ASP.NET Core.
ms.author: riande
ms.date: 07/03/2018
uid: fundamentals/environments
ms.openlocfilehash: b0e001b50ada85a183590fbee1ad1f3b895004d5
ms.sourcegitcommit: 661d30492d5ef7bbca4f7e709f40d8f3309d2dac
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37938436"
---
# <a name="use-multiple-environments-in-aspnet-core"></a><span data-ttu-id="00908-103">Używanie wielu środowisk w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="00908-103">Use multiple environments in ASP.NET Core</span></span>

<span data-ttu-id="00908-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="00908-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="00908-105">Platforma ASP.NET Core konfiguruje zachowanie aplikacji, w zależności od środowiska czasu wykonywania przy użyciu zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="00908-105">ASP.NET Core configures app behavior based on the runtime environment using an environment variable.</span></span>

<span data-ttu-id="00908-106">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="00908-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="environments"></a><span data-ttu-id="00908-107">Środowiska</span><span class="sxs-lookup"><span data-stu-id="00908-107">Environments</span></span>

<span data-ttu-id="00908-108">Platforma ASP.NET Core odczytuje zmiennej środowiskowej `ASPNETCORE_ENVIRONMENT` przy uruchamianiu aplikacji i przechowuje wartość w [IHostingEnvironment.EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname).</span><span class="sxs-lookup"><span data-stu-id="00908-108">ASP.NET Core reads the environment variable `ASPNETCORE_ENVIRONMENT` at app startup and stores the value in [IHostingEnvironment.EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname).</span></span> <span data-ttu-id="00908-109">Możesz ustawić `ASPNETCORE_ENVIRONMENT` dowolną wartość, ale [trzech wartości](/dotnet/api/microsoft.aspnetcore.hosting.environmentname) są obsługiwane przez platformę: [rozwoju](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development), [przemieszczania](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging), i [wśrodowiskuprodukcyjnym](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production).</span><span class="sxs-lookup"><span data-stu-id="00908-109">You can set `ASPNETCORE_ENVIRONMENT` to any value, but [three values](/dotnet/api/microsoft.aspnetcore.hosting.environmentname) are supported by the framework: [Development](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development), [Staging](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging), and [Production](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production).</span></span> <span data-ttu-id="00908-110">Jeśli `ASPNETCORE_ENVIRONMENT` nie jest ustawiony, jego wartość domyślna to `Production`.</span><span class="sxs-lookup"><span data-stu-id="00908-110">If `ASPNETCORE_ENVIRONMENT` isn't set, it defaults to `Production`.</span></span>

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet)]

<span data-ttu-id="00908-111">Powyższy kod:</span><span class="sxs-lookup"><span data-stu-id="00908-111">The preceding code:</span></span>

* <span data-ttu-id="00908-112">Wywołania [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) i [UseBrowserLink](/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink) podczas `ASPNETCORE_ENVIRONMENT` ustawiono `Development`.</span><span class="sxs-lookup"><span data-stu-id="00908-112">Calls [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) and [UseBrowserLink](/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink) when `ASPNETCORE_ENVIRONMENT` is set to `Development`.</span></span>
* <span data-ttu-id="00908-113">Wywołania [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler) podczas wartość `ASPNETCORE_ENVIRONMENT` ustawiono jeden z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="00908-113">Calls [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler) when the value of `ASPNETCORE_ENVIRONMENT` is set one of the following:</span></span>

    * `Staging`
    * `Production`
    * `Staging_2`

<span data-ttu-id="00908-114">[Pomocnik tagu środowiska](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) używa wartości `IHostingEnvironment.EnvironmentName` do dołączania lub wykluczania znaczników w elemencie:</span><span class="sxs-lookup"><span data-stu-id="00908-114">The [Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) uses the value of `IHostingEnvironment.EnvironmentName` to include or exclude markup in the element:</span></span>

[!code-cshtml[](environments/sample-snapshot/EnvironmentsSample/Pages/About.cshtml)]

<span data-ttu-id="00908-115">W systemie Windows i macOS wartości i zmienne środowiskowe nie są z uwzględnieniem wielkości liter.</span><span class="sxs-lookup"><span data-stu-id="00908-115">On Windows and macOS, environment variables and values aren't case sensitive.</span></span> <span data-ttu-id="00908-116">Zmienne środowiskowe systemu Linux i wartości są **z uwzględnieniem wielkości liter** domyślnie.</span><span class="sxs-lookup"><span data-stu-id="00908-116">Linux environment variables and values are **case sensitive** by default.</span></span>

### <a name="development"></a><span data-ttu-id="00908-117">Tworzenie</span><span class="sxs-lookup"><span data-stu-id="00908-117">Development</span></span>

<span data-ttu-id="00908-118">Środowisko projektowe można włączyć funkcje, które nie powinny być dostępne w środowisku produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="00908-118">The development environment can enable features that shouldn't be exposed in production.</span></span> <span data-ttu-id="00908-119">Na przykład włączyć szablony ASP.NET Core [stronie wyjątków deweloperów](xref:fundamentals/error-handling#the-developer-exception-page) w środowisku programistycznym.</span><span class="sxs-lookup"><span data-stu-id="00908-119">For example, the ASP.NET Core templates enable the [developer exception page](xref:fundamentals/error-handling#the-developer-exception-page) in the development environment.</span></span>

<span data-ttu-id="00908-120">Środowisko do tworzenia aplikacji z komputera lokalnego można ustawić w *Properties\launchSettings.json* pliku projektu.</span><span class="sxs-lookup"><span data-stu-id="00908-120">The environment for local machine development can be set in the *Properties\launchSettings.json* file of the project.</span></span> <span data-ttu-id="00908-121">Wartości środowiska ustawione w *launchSettings.json* zastąpienie wartości ustawionych w środowisku systemowym.</span><span class="sxs-lookup"><span data-stu-id="00908-121">Environment values set in *launchSettings.json* override values set in the system environment.</span></span>

<span data-ttu-id="00908-122">Następujący kod JSON zawiera trzy profile z *launchSettings.json* pliku:</span><span class="sxs-lookup"><span data-stu-id="00908-122">The following JSON shows three profiles from a *launchSettings.json* file:</span></span>

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

::: moniker range=">= aspnetcore-2.1"

> [!NOTE]
> <span data-ttu-id="00908-123">`applicationUrl` Właściwość *launchSettings.json* można określić listę adresów URL serwera.</span><span class="sxs-lookup"><span data-stu-id="00908-123">The `applicationUrl` property in *launchSettings.json* can specify a list of server URLs.</span></span> <span data-ttu-id="00908-124">Użyj średnika między adresami URL na liście:</span><span class="sxs-lookup"><span data-stu-id="00908-124">Use a semicolon between the URLs in the list:</span></span>
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

::: moniker-end

<span data-ttu-id="00908-125">Gdy aplikacja jest uruchamiana z [dotnet, uruchom](/dotnet/core/tools/dotnet-run), pierwszy profil z `"commandName": "Project"` jest używany.</span><span class="sxs-lookup"><span data-stu-id="00908-125">When the app is launched with [dotnet run](/dotnet/core/tools/dotnet-run), the first profile with `"commandName": "Project"` is used.</span></span> <span data-ttu-id="00908-126">Wartość `commandName` Określa serwer sieci web, aby uruchomić.</span><span class="sxs-lookup"><span data-stu-id="00908-126">The value of `commandName` specifies the web server to launch.</span></span> <span data-ttu-id="00908-127">`commandName` może być jednym z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="00908-127">`commandName` can be any one of the following:</span></span>

* <span data-ttu-id="00908-128">IIS Express</span><span class="sxs-lookup"><span data-stu-id="00908-128">IIS Express</span></span>
* <span data-ttu-id="00908-129">IIS</span><span class="sxs-lookup"><span data-stu-id="00908-129">IIS</span></span>
* <span data-ttu-id="00908-130">Projekt (który spowoduje uruchomienie Kestrel)</span><span class="sxs-lookup"><span data-stu-id="00908-130">Project (which launches Kestrel)</span></span>

<span data-ttu-id="00908-131">Gdy aplikacja jest uruchamiana z [dotnet, uruchom](/dotnet/core/tools/dotnet-run):</span><span class="sxs-lookup"><span data-stu-id="00908-131">When an app is launched with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>

* <span data-ttu-id="00908-132">*launchSettings.json* jest do odczytu. Jeśli jest dostępny.</span><span class="sxs-lookup"><span data-stu-id="00908-132">*launchSettings.json* is read if available.</span></span> <span data-ttu-id="00908-133">`environmentVariables` ustawienia w *launchSettings.json* zastępują zmienne środowiskowe.</span><span class="sxs-lookup"><span data-stu-id="00908-133">`environmentVariables` settings in *launchSettings.json* override environment variables.</span></span>
* <span data-ttu-id="00908-134">Środowisko hostingu jest wyświetlany.</span><span class="sxs-lookup"><span data-stu-id="00908-134">The hosting environment is displayed.</span></span>

<span data-ttu-id="00908-135">Następujące dane wyjściowe zawierają aplikację wprowadzenie [dotnet, uruchom](/dotnet/core/tools/dotnet-run):</span><span class="sxs-lookup"><span data-stu-id="00908-135">The following output shows an app started with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>

```bash
PS C:\Websites\EnvironmentsSample> dotnet run
Using launch settings from C:\Websites\EnvironmentsSample\Properties\launchSettings.json...
Hosting environment: Staging
Content root path: C:\Websites\EnvironmentsSample
Now listening on: http://localhost:54340
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="00908-136">Właściwości projektu programu Visual Studio **debugowania** karta zawiera graficzny interfejs użytkownika do edycji *launchSettings.json* pliku:</span><span class="sxs-lookup"><span data-stu-id="00908-136">The Visual Studio project properties **Debug** tab provides a GUI to edit the *launchSettings.json* file:</span></span>

![Zmienne środowiskowe ustawienie właściwości projektu](environments/_static/project-properties-debug.png)

<span data-ttu-id="00908-138">Zmiany wprowadzone do profilów projektu nie mogą zostać wprowadzone do czasu ponownego uruchomienia serwera sieci web.</span><span class="sxs-lookup"><span data-stu-id="00908-138">Changes made to project profiles may not take effect until the web server is restarted.</span></span> <span data-ttu-id="00908-139">Kestrel musi być dopiero po ponownym uruchomieniu potrafi wykrywać zmiany wprowadzone w swoim środowisku.</span><span class="sxs-lookup"><span data-stu-id="00908-139">Kestrel must be restarted before it can detect changes made to its environment.</span></span>

> [!WARNING]
> <span data-ttu-id="00908-140">*launchSettings.json* nie należy przechowywać wpisy tajne.</span><span class="sxs-lookup"><span data-stu-id="00908-140">*launchSettings.json* shouldn't store secrets.</span></span> <span data-ttu-id="00908-141">[Narzędzie Menedżer klucz tajny](xref:security/app-secrets) może służyć do przechowywania kluczy tajnych na potrzeby lokalnego programowania.</span><span class="sxs-lookup"><span data-stu-id="00908-141">The [Secret Manager tool](xref:security/app-secrets) can be used to store secrets for local development.</span></span>

<span data-ttu-id="00908-142">Korzystając z [programu Visual Studio Code](https://code.visualstudio.com/), zmienne środowiskowe można ustawić w *.vscode/launch.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="00908-142">When using [Visual Studio Code](https://code.visualstudio.com/), environment variables can be set in the *.vscode/launch.json* file.</span></span> <span data-ttu-id="00908-143">Poniższy przykład ustawia środowisko `Development`:</span><span class="sxs-lookup"><span data-stu-id="00908-143">The following example sets the environment to `Development`:</span></span>

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

<span data-ttu-id="00908-144">A *.vscode/launch.json* plik w projekcie nie jest do odczytu podczas uruchamiania aplikacji przy użyciu `dotnet run` w taki sam sposób jak *Properties/launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="00908-144">A *.vscode/launch.json* file in the project isn't read when starting the app with `dotnet run` in the same way as *Properties/launchSettings.json*.</span></span> <span data-ttu-id="00908-145">Podczas uruchamiania aplikacji w trakcie programowania, który nie ma *launchSettings.json* pliku, należy ustawić środowisko przy użyciu zmiennej środowiskowej lub argument wiersza polecenia, aby `dotnet run` polecenia.</span><span class="sxs-lookup"><span data-stu-id="00908-145">When launching an app in development that doesn't have a *launchSettings.json* file, either set the environment with an environment variable or a command-line argument to the `dotnet run` command.</span></span>

### <a name="production"></a><span data-ttu-id="00908-146">Produkcji</span><span class="sxs-lookup"><span data-stu-id="00908-146">Production</span></span>

<span data-ttu-id="00908-147">W środowisku produkcyjnym należy skonfigurować tak, aby zwiększyć poziom bezpieczeństwa, wydajności i niezawodności aplikacji.</span><span class="sxs-lookup"><span data-stu-id="00908-147">The production environment should be configured to maximize security, performance, and app robustness.</span></span> <span data-ttu-id="00908-148">Niektóre typowe ustawienia, które różnią się od etapu programowania obejmują:</span><span class="sxs-lookup"><span data-stu-id="00908-148">Some common settings that differ from development include:</span></span>

* <span data-ttu-id="00908-149">Pamięć podręczna.</span><span class="sxs-lookup"><span data-stu-id="00908-149">Caching.</span></span>
* <span data-ttu-id="00908-150">Zasoby po stronie klienta są powiązane, zminimalizowany i potencjalnie udostępnianej przez usługę CDN.</span><span class="sxs-lookup"><span data-stu-id="00908-150">Client-side resources are bundled, minified, and potentially served from a CDN.</span></span>
* <span data-ttu-id="00908-151">Błąd diagnostyki strony wyłączone.</span><span class="sxs-lookup"><span data-stu-id="00908-151">Diagnostic error pages disabled.</span></span>
* <span data-ttu-id="00908-152">Strony błędów przyjazna, włączone.</span><span class="sxs-lookup"><span data-stu-id="00908-152">Friendly error pages enabled.</span></span>
* <span data-ttu-id="00908-153">Rejestrowanie produkcji i monitorowanie jest włączone.</span><span class="sxs-lookup"><span data-stu-id="00908-153">Production logging and monitoring enabled.</span></span> <span data-ttu-id="00908-154">Na przykład [usługi Application Insights](/azure/application-insights/app-insights-asp-net-core).</span><span class="sxs-lookup"><span data-stu-id="00908-154">For example, [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="set-the-environment"></a><span data-ttu-id="00908-155">Ustaw środowisko</span><span class="sxs-lookup"><span data-stu-id="00908-155">Set the environment</span></span>

<span data-ttu-id="00908-156">Często jest to przydatne do zestawu określonego środowiska na potrzeby testowania.</span><span class="sxs-lookup"><span data-stu-id="00908-156">It's often useful to set a specific environment for testing.</span></span> <span data-ttu-id="00908-157">Jeśli środowisko nie jest ustawiona, wartość domyślna to `Production`, która wyłącza większości funkcji debugowania.</span><span class="sxs-lookup"><span data-stu-id="00908-157">If the environment isn't set, it defaults to `Production`, which disables most debugging features.</span></span> <span data-ttu-id="00908-158">Metoda do ustawiania środowiska zależy od systemu operacyjnego.</span><span class="sxs-lookup"><span data-stu-id="00908-158">The method for setting the environment depends on the operating system.</span></span>

### <a name="azure-app-service"></a><span data-ttu-id="00908-159">Usługa Azure App Service</span><span class="sxs-lookup"><span data-stu-id="00908-159">Azure App Service</span></span>

<span data-ttu-id="00908-160">Aby ustawić środowiska w [usługi Azure App Service](https://azure.microsoft.com/services/app-service/), wykonaj następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="00908-160">To set the environment in [Azure App Service](https://azure.microsoft.com/services/app-service/), perform the following steps:</span></span>

1. <span data-ttu-id="00908-161">Wybierz aplikację z **App Services** bloku.</span><span class="sxs-lookup"><span data-stu-id="00908-161">Select the app from the **App Services** blade.</span></span>
1. <span data-ttu-id="00908-162">W **ustawienia** grupy, wybierz opcję **ustawienia aplikacji** bloku.</span><span class="sxs-lookup"><span data-stu-id="00908-162">In the **SETTINGS** group, select the **Application settings** blade.</span></span>
1. <span data-ttu-id="00908-163">W **ustawienia aplikacji** wybierz opcję **Dodaj nowe ustawienie**.</span><span class="sxs-lookup"><span data-stu-id="00908-163">In the **Application settings** area, select **Add new setting**.</span></span>
1. <span data-ttu-id="00908-164">Aby uzyskać **wprowadź nazwę**, podaj `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="00908-164">For **Enter a name**, provide `ASPNETCORE_ENVIRONMENT`.</span></span> <span data-ttu-id="00908-165">Aby uzyskać **wprowadź wartość**, zapewniają środowiska (na przykład `Staging`).</span><span class="sxs-lookup"><span data-stu-id="00908-165">For **Enter a value**, provide the environment (for example, `Staging`).</span></span>
1. <span data-ttu-id="00908-166">Wybierz **ustawienie miejsca** pole wyboru, jeśli chcesz, aby ustawienie środowiska, aby pozostać z bieżącym gnieździe, gdy zostały zamienione miejscami wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="00908-166">Select the **Slot Setting** check box if you wish the environment setting to remain with the current slot when deployment slots are swapped.</span></span> <span data-ttu-id="00908-167">Aby uzyskać więcej informacji, zobacz [dokumentacji platformy Azure: ustawienia, które zostały zamienione?](/azure/app-service/web-sites-staged-publishing).</span><span class="sxs-lookup"><span data-stu-id="00908-167">For more information, see [Azure Documentation: Which settings are swapped?](/azure/app-service/web-sites-staged-publishing).</span></span>
1. <span data-ttu-id="00908-168">Wybierz **Zapisz** w górnej części bloku.</span><span class="sxs-lookup"><span data-stu-id="00908-168">Select **Save** at the top of the blade.</span></span>

<span data-ttu-id="00908-169">Usługa Azure App Service zostanie automatycznie uruchomiony ponownie aplikację po ustawienia aplikacji (zmienną środowiskową) jest dodanie, zmianę lub usunąć w witrynie Azure portal.</span><span class="sxs-lookup"><span data-stu-id="00908-169">Azure App Service automatically restarts the app after an app setting (environment variable) is added, changed, or deleted in the Azure portal.</span></span>

### <a name="windows"></a><span data-ttu-id="00908-170">Windows</span><span class="sxs-lookup"><span data-stu-id="00908-170">Windows</span></span>

<span data-ttu-id="00908-171">Aby ustawić `ASPNETCORE_ENVIRONMENT` dla bieżącej sesji, podczas uruchamiania aplikacji przy użyciu [dotnet, uruchom](/dotnet/core/tools/dotnet-run), służą następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="00908-171">To set the `ASPNETCORE_ENVIRONMENT` for the current session when the app is started using [dotnet run](/dotnet/core/tools/dotnet-run), the following commands are used:</span></span>

<span data-ttu-id="00908-172">**Wiersz polecenia**</span><span class="sxs-lookup"><span data-stu-id="00908-172">**Command prompt**</span></span>

```console
set ASPNETCORE_ENVIRONMENT=Development
```

<span data-ttu-id="00908-173">**Program PowerShell**</span><span class="sxs-lookup"><span data-stu-id="00908-173">**PowerShell**</span></span>

```powershell
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

<span data-ttu-id="00908-174">Te polecenia tylko obowiązywać w przypadku bieżącego okna.</span><span class="sxs-lookup"><span data-stu-id="00908-174">These commands only take effect for the current window.</span></span> <span data-ttu-id="00908-175">Po zamknięciu okna `ASPNETCORE_ENVIRONMENT` przywraca ustawienie domyślne ustawienie lub wartość maszyny.</span><span class="sxs-lookup"><span data-stu-id="00908-175">When the window is closed, the `ASPNETCORE_ENVIRONMENT` setting reverts to the default setting or machine value.</span></span> <span data-ttu-id="00908-176">Aby ustawić wartość globalnie w Windows, otwórz **Panelu sterowania** > **systemu** > **Zaawansowane ustawienia systemu** i Dodaj lub Edytuj `ASPNETCORE_ENVIRONMENT`wartość:</span><span class="sxs-lookup"><span data-stu-id="00908-176">To set the value globally in Windows, open the **Control Panel** > **System** > **Advanced system settings** and add or edit the `ASPNETCORE_ENVIRONMENT` value:</span></span>

![Zaawansowane właściwości systemu](environments/_static/systemsetting_environment.png)

![Zmienna środowiskowa Core ASPNET](environments/_static/windows_aspnetcore_environment.png)

<span data-ttu-id="00908-179">**web.config**</span><span class="sxs-lookup"><span data-stu-id="00908-179">**web.config**</span></span>

<span data-ttu-id="00908-180">Zobacz *Ustawianie zmiennych środowiskowych* części [informacje o konfiguracji modułu ASP.NET Core](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) tematu.</span><span class="sxs-lookup"><span data-stu-id="00908-180">See the *Setting environment variables* section of the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) topic.</span></span>

<span data-ttu-id="00908-181">**Na pulę aplikacji usług IIS**</span><span class="sxs-lookup"><span data-stu-id="00908-181">**Per IIS Application Pool**</span></span>

<span data-ttu-id="00908-182">Aby ustawić zmienne środowiskowe dla poszczególnych aplikacji działających w izolowanej pulach aplikacji (obsługiwane w usługach IIS 10.0 +), zobacz *polecenia AppCmd.exe* części [zmienne środowiskowe &lt; environmentVariables&gt; ](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) tematu.</span><span class="sxs-lookup"><span data-stu-id="00908-182">To set environment variables for individual apps running in isolated Application Pools (supported on IIS 10.0+), see the *AppCmd.exe command* section of the [Environment Variables &lt;environmentVariables&gt;](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic.</span></span>

### <a name="macos"></a><span data-ttu-id="00908-183">macOS</span><span class="sxs-lookup"><span data-stu-id="00908-183">macOS</span></span>

<span data-ttu-id="00908-184">Ustawienie bieżącego środowiska, dla systemu macOS może być wykonywane w tekście, podczas uruchamiania aplikacji:</span><span class="sxs-lookup"><span data-stu-id="00908-184">Setting the current environment for macOS can be performed in-line when running the app:</span></span>

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```

<span data-ttu-id="00908-185">Alternatywnie należy ustawić środowisko z `export` przed uruchomieniem aplikacji:</span><span class="sxs-lookup"><span data-stu-id="00908-185">Alternatively, set the environment with `export` prior to running the app:</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

<span data-ttu-id="00908-186">Zmienne środowiskowe na poziomie komputera są ustawiane w *.bashrc* lub *.bash_profile* pliku.</span><span class="sxs-lookup"><span data-stu-id="00908-186">Machine-level environment variables are set in the *.bashrc* or *.bash_profile* file.</span></span> <span data-ttu-id="00908-187">Edytuj plik za pomocą dowolnego edytora tekstów.</span><span class="sxs-lookup"><span data-stu-id="00908-187">Edit the file using any text editor.</span></span> <span data-ttu-id="00908-188">Dodaj następującą instrukcję:</span><span class="sxs-lookup"><span data-stu-id="00908-188">Add the following statement:</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

### <a name="linux"></a><span data-ttu-id="00908-189">Linux</span><span class="sxs-lookup"><span data-stu-id="00908-189">Linux</span></span>

<span data-ttu-id="00908-190">Dystrybucje systemu Linux, można użyć `export` polecenie w wierszu polecenia dla ustawień zmiennych oparte na sesji i *bash_profile* ustawienia maszyn na poziomie środowiska w pliku.</span><span class="sxs-lookup"><span data-stu-id="00908-190">For Linux distros, use the `export` command at a command prompt for session-based variable settings and *bash_profile* file for machine-level environment settings.</span></span>

### <a name="configuration-by-environment"></a><span data-ttu-id="00908-191">Konfiguracja według środowiska</span><span class="sxs-lookup"><span data-stu-id="00908-191">Configuration by environment</span></span>

<span data-ttu-id="00908-192">Zobacz [konfiguracji przez środowisko](xref:fundamentals/configuration/index#configuration-by-environment) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="00908-192">See [Configuration by environment](xref:fundamentals/configuration/index#configuration-by-environment) for more information.</span></span>

## <a name="environment-based-startup-class-and-methods"></a><span data-ttu-id="00908-193">Klasa początkowa oparte na środowisku i metody</span><span class="sxs-lookup"><span data-stu-id="00908-193">Environment-based Startup class and methods</span></span>

### <a name="startup-class-conventions"></a><span data-ttu-id="00908-194">Konwencje klasy uruchamiania</span><span class="sxs-lookup"><span data-stu-id="00908-194">Startup class conventions</span></span>

<span data-ttu-id="00908-195">Po uruchomieniu aplikacji ASP.NET Core [Klasa początkowa](xref:fundamentals/startup) używa do ładowania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="00908-195">When an ASP.NET Core app starts, the [Startup class](xref:fundamentals/startup) bootstraps the app.</span></span> <span data-ttu-id="00908-196">Aplikację można zdefiniować w oddzielnym `Startup` klas w różnych środowiskach (na przykład `StartupDevelopment`), a odpowiedni `Startup` klasy jest zaznaczona w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="00908-196">The app can define separate `Startup` classes for different environments (for example, `StartupDevelopment`), and the appropriate `Startup` class is selected at runtime.</span></span> <span data-ttu-id="00908-197">Klasy, w których sufiks nazwy pasuje do bieżącego środowiska jest podzielony na priorytety.</span><span class="sxs-lookup"><span data-stu-id="00908-197">The class whose name suffix matches the current environment is prioritized.</span></span> <span data-ttu-id="00908-198">Jeśli pasujący obiekt typu `Startup{EnvironmentName}` klasy nie zostanie znaleziona, `Startup` klasa jest używana.</span><span class="sxs-lookup"><span data-stu-id="00908-198">If a matching `Startup{EnvironmentName}` class isn't found, the `Startup` class is used.</span></span>

<span data-ttu-id="00908-199">Aby zaimplementować oparte na środowisku `Startup` Tworzenie klas, `Startup{EnvironmentName}` klasy dla każdego środowiska w użyciu i rezerwowe `Startup` klasy:</span><span class="sxs-lookup"><span data-stu-id="00908-199">To implement environment-based `Startup` classes, create a `Startup{EnvironmentName}` class for each environment in use and a fallback `Startup` class:</span></span>

```csharp
// Startup class to use in the Development environment
public class StartupDevelopment
{
    public void ConfigureServices(IServiceCollection services)
    {
        ...
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
        ...
    }
}

// Startup class to use in the Production environment
public class StartupProduction
{
    public void ConfigureServices(IServiceCollection services)
    {
        ...
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
        ...
    }
}

// Fallback Startup class
// Selected if the environment doesn't match a Startup{EnvironmentName} class
public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
        ...
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
        ...
    }
}
```

<span data-ttu-id="00908-200">Użyj [UseStartup (IWebHostBuilder, ciąg)](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usestartup) przeciążenie, które akceptuje to nazwa zestawu:</span><span class="sxs-lookup"><span data-stu-id="00908-200">Use the [UseStartup(IWebHostBuilder, String)](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usestartup) overload that accepts an assembly name:</span></span>

::: moniker range=">= aspnetcore-2.1"

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

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```csharp
public static void Main(string[] args)
{
    CreateWebHost(args).Run();
}

public static IWebHost CreateWebHost(string[] args)
{
    var assemblyName = typeof(Startup).GetTypeInfo().Assembly.FullName;

    return WebHost.CreateDefaultBuilder(args)
        .UseStartup(assemblyName)
        .Build();
}
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        var assemblyName = typeof(Startup).GetTypeInfo().Assembly.FullName;

        var host = new WebHostBuilder()
            .UseStartup(assemblyName)
            .Build();

        host.Run();
    }
}
```

::: moniker-end

### <a name="startup-method-conventions"></a><span data-ttu-id="00908-201">Konwencje metod uruchamiania</span><span class="sxs-lookup"><span data-stu-id="00908-201">Startup method conventions</span></span>

<span data-ttu-id="00908-202">[Konfigurowanie](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) i [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) obsługuje specyficznymi dla środowiska wersji formularza `Configure<EnvironmentName>` i `Configure<EnvironmentName>Services`:</span><span class="sxs-lookup"><span data-stu-id="00908-202">[Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) support environment-specific versions of the form `Configure<EnvironmentName>` and `Configure<EnvironmentName>Services`:</span></span>

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet_all&highlight=15,51)]

## <a name="additional-resources"></a><span data-ttu-id="00908-203">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="00908-203">Additional resources</span></span>

* <xref:fundamentals/startup>
* <xref:fundamentals/configuration/index>
* [<span data-ttu-id="00908-204">IHostingEnvironment.EnvironmentName</span><span class="sxs-lookup"><span data-stu-id="00908-204">IHostingEnvironment.EnvironmentName</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname)
