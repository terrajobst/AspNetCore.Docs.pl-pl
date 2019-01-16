---
title: Używanie wielu środowisk w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak kontrolować zachowanie aplikacji w wielu środowiskach, w aplikacji platformy ASP.NET Core.
ms.author: riande
ms.date: 07/03/2018
uid: fundamentals/environments
ms.openlocfilehash: 642af9e8f9e322e3624dad46bb1463f6525f5c9e
ms.sourcegitcommit: 42a8164b8aba21f322ffefacb92301bdfb4d3c2d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/16/2019
ms.locfileid: "54341670"
---
# <a name="use-multiple-environments-in-aspnet-core"></a><span data-ttu-id="63c7c-103">Używanie wielu środowisk w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="63c7c-103">Use multiple environments in ASP.NET Core</span></span>

<span data-ttu-id="63c7c-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="63c7c-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="63c7c-105">Platforma ASP.NET Core konfiguruje zachowanie aplikacji, w zależności od środowiska czasu wykonywania przy użyciu zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="63c7c-105">ASP.NET Core configures app behavior based on the runtime environment using an environment variable.</span></span>

<span data-ttu-id="63c7c-106">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="63c7c-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="environments"></a><span data-ttu-id="63c7c-107">Środowiska</span><span class="sxs-lookup"><span data-stu-id="63c7c-107">Environments</span></span>

<span data-ttu-id="63c7c-108">Platforma ASP.NET Core odczytuje zmiennej środowiskowej `ASPNETCORE_ENVIRONMENT` przy uruchamianiu aplikacji i przechowuje wartość w [IHostingEnvironment.EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname).</span><span class="sxs-lookup"><span data-stu-id="63c7c-108">ASP.NET Core reads the environment variable `ASPNETCORE_ENVIRONMENT` at app startup and stores the value in [IHostingEnvironment.EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname).</span></span> <span data-ttu-id="63c7c-109">Możesz ustawić `ASPNETCORE_ENVIRONMENT` dowolną wartość, ale [trzech wartości](/dotnet/api/microsoft.aspnetcore.hosting.environmentname) są obsługiwane przez platformę: [Programowanie](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development), [przemieszczania](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging), i [produkcji](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production).</span><span class="sxs-lookup"><span data-stu-id="63c7c-109">You can set `ASPNETCORE_ENVIRONMENT` to any value, but [three values](/dotnet/api/microsoft.aspnetcore.hosting.environmentname) are supported by the framework: [Development](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development), [Staging](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging), and [Production](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production).</span></span> <span data-ttu-id="63c7c-110">Jeśli `ASPNETCORE_ENVIRONMENT` nie jest ustawiony, jego wartość domyślna to `Production`.</span><span class="sxs-lookup"><span data-stu-id="63c7c-110">If `ASPNETCORE_ENVIRONMENT` isn't set, it defaults to `Production`.</span></span>

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet)]

<span data-ttu-id="63c7c-111">Powyższy kod:</span><span class="sxs-lookup"><span data-stu-id="63c7c-111">The preceding code:</span></span>

* <span data-ttu-id="63c7c-112">Wywołania [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) podczas `ASPNETCORE_ENVIRONMENT` ustawiono `Development`.</span><span class="sxs-lookup"><span data-stu-id="63c7c-112">Calls [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) when `ASPNETCORE_ENVIRONMENT` is set to `Development`.</span></span>
* <span data-ttu-id="63c7c-113">Wywołania [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler) podczas wartość `ASPNETCORE_ENVIRONMENT` ustawiono jeden z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="63c7c-113">Calls [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler) when the value of `ASPNETCORE_ENVIRONMENT` is set one of the following:</span></span>

    * `Staging`
    * `Production`
    * `Staging_2`

<span data-ttu-id="63c7c-114">[Pomocnik tagu środowiska](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) używa wartości `IHostingEnvironment.EnvironmentName` do dołączania lub wykluczania znaczników w elemencie:</span><span class="sxs-lookup"><span data-stu-id="63c7c-114">The [Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) uses the value of `IHostingEnvironment.EnvironmentName` to include or exclude markup in the element:</span></span>

[!code-cshtml[](environments/sample-snapshot/EnvironmentsSample/Pages/About.cshtml)]

<span data-ttu-id="63c7c-115">W systemie Windows i macOS wartości i zmienne środowiskowe nie są z uwzględnieniem wielkości liter.</span><span class="sxs-lookup"><span data-stu-id="63c7c-115">On Windows and macOS, environment variables and values aren't case sensitive.</span></span> <span data-ttu-id="63c7c-116">Zmienne środowiskowe systemu Linux i wartości są **z uwzględnieniem wielkości liter** domyślnie.</span><span class="sxs-lookup"><span data-stu-id="63c7c-116">Linux environment variables and values are **case sensitive** by default.</span></span>

### <a name="development"></a><span data-ttu-id="63c7c-117">Tworzenie</span><span class="sxs-lookup"><span data-stu-id="63c7c-117">Development</span></span>

<span data-ttu-id="63c7c-118">Środowisko projektowe można włączyć funkcje, które nie powinny być dostępne w środowisku produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="63c7c-118">The development environment can enable features that shouldn't be exposed in production.</span></span> <span data-ttu-id="63c7c-119">Na przykład włączyć szablony ASP.NET Core [stronie wyjątków deweloperów](xref:fundamentals/error-handling#the-developer-exception-page) w środowisku programistycznym.</span><span class="sxs-lookup"><span data-stu-id="63c7c-119">For example, the ASP.NET Core templates enable the [developer exception page](xref:fundamentals/error-handling#the-developer-exception-page) in the development environment.</span></span>

<span data-ttu-id="63c7c-120">Środowisko do tworzenia aplikacji z komputera lokalnego można ustawić w *Properties\launchSettings.json* pliku projektu.</span><span class="sxs-lookup"><span data-stu-id="63c7c-120">The environment for local machine development can be set in the *Properties\launchSettings.json* file of the project.</span></span> <span data-ttu-id="63c7c-121">Wartości środowiska ustawione w *launchSettings.json* zastąpienie wartości ustawionych w środowisku systemowym.</span><span class="sxs-lookup"><span data-stu-id="63c7c-121">Environment values set in *launchSettings.json* override values set in the system environment.</span></span>

<span data-ttu-id="63c7c-122">Następujący kod JSON zawiera trzy profile z *launchSettings.json* pliku:</span><span class="sxs-lookup"><span data-stu-id="63c7c-122">The following JSON shows three profiles from a *launchSettings.json* file:</span></span>

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
> <span data-ttu-id="63c7c-123">`applicationUrl` Właściwość *launchSettings.json* można określić listę adresów URL serwera.</span><span class="sxs-lookup"><span data-stu-id="63c7c-123">The `applicationUrl` property in *launchSettings.json* can specify a list of server URLs.</span></span> <span data-ttu-id="63c7c-124">Użyj średnika między adresami URL na liście:</span><span class="sxs-lookup"><span data-stu-id="63c7c-124">Use a semicolon between the URLs in the list:</span></span>
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

<span data-ttu-id="63c7c-125">Gdy aplikacja jest uruchamiana z [dotnet, uruchom](/dotnet/core/tools/dotnet-run), pierwszy profil z `"commandName": "Project"` jest używany.</span><span class="sxs-lookup"><span data-stu-id="63c7c-125">When the app is launched with [dotnet run](/dotnet/core/tools/dotnet-run), the first profile with `"commandName": "Project"` is used.</span></span> <span data-ttu-id="63c7c-126">Wartość `commandName` Określa serwer sieci web, aby uruchomić.</span><span class="sxs-lookup"><span data-stu-id="63c7c-126">The value of `commandName` specifies the web server to launch.</span></span> <span data-ttu-id="63c7c-127">`commandName` może być jednym z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="63c7c-127">`commandName` can be any one of the following:</span></span>

* `IISExpress`
* `IIS`
* <span data-ttu-id="63c7c-128">`Project` (który spowoduje uruchomienie Kestrel)</span><span class="sxs-lookup"><span data-stu-id="63c7c-128">`Project` (which launches Kestrel)</span></span>

<span data-ttu-id="63c7c-129">Gdy aplikacja jest uruchamiana z [dotnet, uruchom](/dotnet/core/tools/dotnet-run):</span><span class="sxs-lookup"><span data-stu-id="63c7c-129">When an app is launched with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>

* <span data-ttu-id="63c7c-130">*launchSettings.json* jest do odczytu. Jeśli jest dostępny.</span><span class="sxs-lookup"><span data-stu-id="63c7c-130">*launchSettings.json* is read if available.</span></span> <span data-ttu-id="63c7c-131">`environmentVariables` ustawienia w *launchSettings.json* zastępują zmienne środowiskowe.</span><span class="sxs-lookup"><span data-stu-id="63c7c-131">`environmentVariables` settings in *launchSettings.json* override environment variables.</span></span>
* <span data-ttu-id="63c7c-132">Środowisko hostingu jest wyświetlany.</span><span class="sxs-lookup"><span data-stu-id="63c7c-132">The hosting environment is displayed.</span></span>

<span data-ttu-id="63c7c-133">Następujące dane wyjściowe zawierają aplikację wprowadzenie [dotnet, uruchom](/dotnet/core/tools/dotnet-run):</span><span class="sxs-lookup"><span data-stu-id="63c7c-133">The following output shows an app started with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>

```bash
PS C:\Websites\EnvironmentsSample> dotnet run
Using launch settings from C:\Websites\EnvironmentsSample\Properties\launchSettings.json...
Hosting environment: Staging
Content root path: C:\Websites\EnvironmentsSample
Now listening on: http://localhost:54340
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="63c7c-134">Właściwości projektu programu Visual Studio **debugowania** karta zawiera graficzny interfejs użytkownika do edycji *launchSettings.json* pliku:</span><span class="sxs-lookup"><span data-stu-id="63c7c-134">The Visual Studio project properties **Debug** tab provides a GUI to edit the *launchSettings.json* file:</span></span>

![Zmienne środowiskowe ustawienie właściwości projektu](environments/_static/project-properties-debug.png)

<span data-ttu-id="63c7c-136">Zmiany wprowadzone do profilów projektu nie mogą zostać wprowadzone do czasu ponownego uruchomienia serwera sieci web.</span><span class="sxs-lookup"><span data-stu-id="63c7c-136">Changes made to project profiles may not take effect until the web server is restarted.</span></span> <span data-ttu-id="63c7c-137">Kestrel musi być dopiero po ponownym uruchomieniu potrafi wykrywać zmiany wprowadzone w swoim środowisku.</span><span class="sxs-lookup"><span data-stu-id="63c7c-137">Kestrel must be restarted before it can detect changes made to its environment.</span></span>

> [!WARNING]
> <span data-ttu-id="63c7c-138">*launchSettings.json* nie należy przechowywać wpisy tajne.</span><span class="sxs-lookup"><span data-stu-id="63c7c-138">*launchSettings.json* shouldn't store secrets.</span></span> <span data-ttu-id="63c7c-139">[Narzędzie Menedżer klucz tajny](xref:security/app-secrets) może służyć do przechowywania kluczy tajnych na potrzeby lokalnego programowania.</span><span class="sxs-lookup"><span data-stu-id="63c7c-139">The [Secret Manager tool](xref:security/app-secrets) can be used to store secrets for local development.</span></span>

<span data-ttu-id="63c7c-140">Korzystając z [programu Visual Studio Code](https://code.visualstudio.com/), zmienne środowiskowe można ustawić w *.vscode/launch.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="63c7c-140">When using [Visual Studio Code](https://code.visualstudio.com/), environment variables can be set in the *.vscode/launch.json* file.</span></span> <span data-ttu-id="63c7c-141">Poniższy przykład ustawia środowisko `Development`:</span><span class="sxs-lookup"><span data-stu-id="63c7c-141">The following example sets the environment to `Development`:</span></span>

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

<span data-ttu-id="63c7c-142">A *.vscode/launch.json* plik w projekcie nie jest do odczytu podczas uruchamiania aplikacji przy użyciu `dotnet run` w taki sam sposób jak *Properties/launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="63c7c-142">A *.vscode/launch.json* file in the project isn't read when starting the app with `dotnet run` in the same way as *Properties/launchSettings.json*.</span></span> <span data-ttu-id="63c7c-143">Podczas uruchamiania aplikacji w trakcie programowania, który nie ma *launchSettings.json* pliku, należy ustawić środowisko przy użyciu zmiennej środowiskowej lub argument wiersza polecenia, aby `dotnet run` polecenia.</span><span class="sxs-lookup"><span data-stu-id="63c7c-143">When launching an app in development that doesn't have a *launchSettings.json* file, either set the environment with an environment variable or a command-line argument to the `dotnet run` command.</span></span>

### <a name="production"></a><span data-ttu-id="63c7c-144">Produkcji</span><span class="sxs-lookup"><span data-stu-id="63c7c-144">Production</span></span>

<span data-ttu-id="63c7c-145">W środowisku produkcyjnym należy skonfigurować tak, aby zwiększyć poziom bezpieczeństwa, wydajności i niezawodności aplikacji.</span><span class="sxs-lookup"><span data-stu-id="63c7c-145">The production environment should be configured to maximize security, performance, and app robustness.</span></span> <span data-ttu-id="63c7c-146">Niektóre typowe ustawienia, które różnią się od etapu programowania obejmują:</span><span class="sxs-lookup"><span data-stu-id="63c7c-146">Some common settings that differ from development include:</span></span>

* <span data-ttu-id="63c7c-147">Pamięć podręczna.</span><span class="sxs-lookup"><span data-stu-id="63c7c-147">Caching.</span></span>
* <span data-ttu-id="63c7c-148">Zasoby po stronie klienta są powiązane, zminimalizowany i potencjalnie udostępnianej przez usługę CDN.</span><span class="sxs-lookup"><span data-stu-id="63c7c-148">Client-side resources are bundled, minified, and potentially served from a CDN.</span></span>
* <span data-ttu-id="63c7c-149">Błąd diagnostyki strony wyłączone.</span><span class="sxs-lookup"><span data-stu-id="63c7c-149">Diagnostic error pages disabled.</span></span>
* <span data-ttu-id="63c7c-150">Strony błędów przyjazna, włączone.</span><span class="sxs-lookup"><span data-stu-id="63c7c-150">Friendly error pages enabled.</span></span>
* <span data-ttu-id="63c7c-151">Rejestrowanie produkcji i monitorowanie jest włączone.</span><span class="sxs-lookup"><span data-stu-id="63c7c-151">Production logging and monitoring enabled.</span></span> <span data-ttu-id="63c7c-152">Na przykład [usługi Application Insights](/azure/application-insights/app-insights-asp-net-core).</span><span class="sxs-lookup"><span data-stu-id="63c7c-152">For example, [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="set-the-environment"></a><span data-ttu-id="63c7c-153">Ustaw środowisko</span><span class="sxs-lookup"><span data-stu-id="63c7c-153">Set the environment</span></span>

<span data-ttu-id="63c7c-154">Często jest to przydatne do zestawu określonego środowiska na potrzeby testowania.</span><span class="sxs-lookup"><span data-stu-id="63c7c-154">It's often useful to set a specific environment for testing.</span></span> <span data-ttu-id="63c7c-155">Jeśli środowisko nie jest ustawiona, wartość domyślna to `Production`, która wyłącza większości funkcji debugowania.</span><span class="sxs-lookup"><span data-stu-id="63c7c-155">If the environment isn't set, it defaults to `Production`, which disables most debugging features.</span></span> <span data-ttu-id="63c7c-156">Metoda do ustawiania środowiska zależy od systemu operacyjnego.</span><span class="sxs-lookup"><span data-stu-id="63c7c-156">The method for setting the environment depends on the operating system.</span></span>

### <a name="azure-app-service"></a><span data-ttu-id="63c7c-157">Usługa Azure App Service</span><span class="sxs-lookup"><span data-stu-id="63c7c-157">Azure App Service</span></span>

<span data-ttu-id="63c7c-158">Aby ustawić środowiska w [usługi Azure App Service](https://azure.microsoft.com/services/app-service/), wykonaj następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="63c7c-158">To set the environment in [Azure App Service](https://azure.microsoft.com/services/app-service/), perform the following steps:</span></span>

1. <span data-ttu-id="63c7c-159">Wybierz aplikację z **App Services** bloku.</span><span class="sxs-lookup"><span data-stu-id="63c7c-159">Select the app from the **App Services** blade.</span></span>
1. <span data-ttu-id="63c7c-160">W **ustawienia** grupy, wybierz opcję **ustawienia aplikacji** bloku.</span><span class="sxs-lookup"><span data-stu-id="63c7c-160">In the **SETTINGS** group, select the **Application settings** blade.</span></span>
1. <span data-ttu-id="63c7c-161">W **ustawienia aplikacji** wybierz opcję **Dodaj nowe ustawienie**.</span><span class="sxs-lookup"><span data-stu-id="63c7c-161">In the **Application settings** area, select **Add new setting**.</span></span>
1. <span data-ttu-id="63c7c-162">Aby uzyskać **wprowadź nazwę**, podaj `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="63c7c-162">For **Enter a name**, provide `ASPNETCORE_ENVIRONMENT`.</span></span> <span data-ttu-id="63c7c-163">Aby uzyskać **wprowadź wartość**, zapewniają środowiska (na przykład `Staging`).</span><span class="sxs-lookup"><span data-stu-id="63c7c-163">For **Enter a value**, provide the environment (for example, `Staging`).</span></span>
1. <span data-ttu-id="63c7c-164">Wybierz **ustawienie miejsca** pole wyboru, jeśli chcesz, aby ustawienie środowiska, aby pozostać z bieżącym gnieździe, gdy zostały zamienione miejscami wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="63c7c-164">Select the **Slot Setting** check box if you wish the environment setting to remain with the current slot when deployment slots are swapped.</span></span> <span data-ttu-id="63c7c-165">Aby uzyskać więcej informacji, zobacz [dokumentacji platformy Azure: Ustawienia, które zostały zamienione? ](/azure/app-service/web-sites-staged-publishing).</span><span class="sxs-lookup"><span data-stu-id="63c7c-165">For more information, see [Azure Documentation: Which settings are swapped?](/azure/app-service/web-sites-staged-publishing).</span></span>
1. <span data-ttu-id="63c7c-166">Wybierz **Zapisz** w górnej części bloku.</span><span class="sxs-lookup"><span data-stu-id="63c7c-166">Select **Save** at the top of the blade.</span></span>

<span data-ttu-id="63c7c-167">Usługa Azure App Service zostanie automatycznie uruchomiony ponownie aplikację po ustawienia aplikacji (zmienną środowiskową) jest dodanie, zmianę lub usunąć w witrynie Azure portal.</span><span class="sxs-lookup"><span data-stu-id="63c7c-167">Azure App Service automatically restarts the app after an app setting (environment variable) is added, changed, or deleted in the Azure portal.</span></span>

### <a name="windows"></a><span data-ttu-id="63c7c-168">Windows</span><span class="sxs-lookup"><span data-stu-id="63c7c-168">Windows</span></span>

<span data-ttu-id="63c7c-169">Aby ustawić `ASPNETCORE_ENVIRONMENT` dla bieżącej sesji, podczas uruchamiania aplikacji przy użyciu [dotnet, uruchom](/dotnet/core/tools/dotnet-run), służą następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="63c7c-169">To set the `ASPNETCORE_ENVIRONMENT` for the current session when the app is started using [dotnet run](/dotnet/core/tools/dotnet-run), the following commands are used:</span></span>

<span data-ttu-id="63c7c-170">**Wiersz polecenia**</span><span class="sxs-lookup"><span data-stu-id="63c7c-170">**Command prompt**</span></span>

```console
set ASPNETCORE_ENVIRONMENT=Development
```

<span data-ttu-id="63c7c-171">**Program PowerShell**</span><span class="sxs-lookup"><span data-stu-id="63c7c-171">**PowerShell**</span></span>

```powershell
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

<span data-ttu-id="63c7c-172">Te polecenia tylko obowiązywać w przypadku bieżącego okna.</span><span class="sxs-lookup"><span data-stu-id="63c7c-172">These commands only take effect for the current window.</span></span> <span data-ttu-id="63c7c-173">Po zamknięciu okna `ASPNETCORE_ENVIRONMENT` przywraca ustawienie domyślne ustawienie lub wartość maszyny.</span><span class="sxs-lookup"><span data-stu-id="63c7c-173">When the window is closed, the `ASPNETCORE_ENVIRONMENT` setting reverts to the default setting or machine value.</span></span>

<span data-ttu-id="63c7c-174">Aby ustawić wartość globalnie w Windows, użyj jednej z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="63c7c-174">To set the value globally in Windows, use either of the following approaches:</span></span>

* <span data-ttu-id="63c7c-175">Otwórz **Panelu sterowania** > **systemu** > **Zaawansowane ustawienia systemu** i Dodaj lub Edytuj `ASPNETCORE_ENVIRONMENT` wartość:</span><span class="sxs-lookup"><span data-stu-id="63c7c-175">Open the **Control Panel** > **System** > **Advanced system settings** and add or edit the `ASPNETCORE_ENVIRONMENT` value:</span></span>

  ![Zaawansowane właściwości systemu](environments/_static/systemsetting_environment.png)

  ![Zmienna środowiskowa Core ASPNET](environments/_static/windows_aspnetcore_environment.png)

* <span data-ttu-id="63c7c-178">Otwórz administracyjny wiersz polecenia i użyj `setx` polecenia lub Otwórz administracyjny wiersz polecenia programu PowerShell i użyj `[Environment]::SetEnvironmentVariable`:</span><span class="sxs-lookup"><span data-stu-id="63c7c-178">Open an administrative command prompt and use the `setx` command or open an administrative PowerShell command prompt and use `[Environment]::SetEnvironmentVariable`:</span></span>

  <span data-ttu-id="63c7c-179">**Wiersz polecenia**</span><span class="sxs-lookup"><span data-stu-id="63c7c-179">**Command prompt**</span></span>

  ```console
  setx ASPNETCORE_ENVIRONMENT Development /M
  ```

  <span data-ttu-id="63c7c-180">`/M` Przełącznika wskazuje, aby ustawić zmienną środowiskową na poziomie systemu.</span><span class="sxs-lookup"><span data-stu-id="63c7c-180">The `/M` switch indicates to set the environment variable at the system level.</span></span> <span data-ttu-id="63c7c-181">Jeśli `/M` przełącznik nie jest używany, zmienna środowiskowa jest ustawiona dla konta użytkownika.</span><span class="sxs-lookup"><span data-stu-id="63c7c-181">If the `/M` switch isn't used, the environment variable is set for the user account.</span></span>

  <span data-ttu-id="63c7c-182">**Program PowerShell**</span><span class="sxs-lookup"><span data-stu-id="63c7c-182">**PowerShell**</span></span>

  ```powershell
  [Environment]::SetEnvironmentVariable("ASPNETCORE_ENVIRONMENT", "Development", "Machine")
  ```

  <span data-ttu-id="63c7c-183">`Machine` Wskazuje wartość opcji, aby ustawić zmienną środowiskową na poziomie systemu.</span><span class="sxs-lookup"><span data-stu-id="63c7c-183">The `Machine` option value indicates to set the environment variable at the system level.</span></span> <span data-ttu-id="63c7c-184">Jeżeli wartość opcji została zmieniona na `User`, zmienna środowiskowa jest ustawiona dla konta użytkownika.</span><span class="sxs-lookup"><span data-stu-id="63c7c-184">If the option value is changed to `User`, the environment variable is set for the user account.</span></span>

<span data-ttu-id="63c7c-185">Gdy `ASPNETCORE_ENVIRONMENT` zmienna środowiskowa jest ustawiona globalnie, wprowadzone `dotnet run` w dowolnym oknie polecenia otwarte po ustawieniu wartości.</span><span class="sxs-lookup"><span data-stu-id="63c7c-185">When the `ASPNETCORE_ENVIRONMENT` environment variable is set globally, it takes effect for `dotnet run` in any command window opened after the value is set.</span></span>

<span data-ttu-id="63c7c-186">**web.config**</span><span class="sxs-lookup"><span data-stu-id="63c7c-186">**web.config**</span></span>

<span data-ttu-id="63c7c-187">Aby ustawić `ASPNETCORE_ENVIRONMENT` zmiennej środowiskowej *web.config*, zobacz *Ustawianie zmiennych środowiskowych* części <xref:host-and-deploy/aspnet-core-module#setting-environment-variables>.</span><span class="sxs-lookup"><span data-stu-id="63c7c-187">To set the `ASPNETCORE_ENVIRONMENT` environment variable with *web.config*, see the *Setting environment variables* section of <xref:host-and-deploy/aspnet-core-module#setting-environment-variables>.</span></span> <span data-ttu-id="63c7c-188">Gdy `ASPNETCORE_ENVIRONMENT` zmienna środowiskowa została ustawiona za pomocą *web.config*, jego wartość zastępuje ustawienie poziomie systemu.</span><span class="sxs-lookup"><span data-stu-id="63c7c-188">When the `ASPNETCORE_ENVIRONMENT` environment variable is set with *web.config*, its value overrides a setting at the system level.</span></span>

<span data-ttu-id="63c7c-189">**Na pulę aplikacji usług IIS**</span><span class="sxs-lookup"><span data-stu-id="63c7c-189">**Per IIS Application Pool**</span></span>

<span data-ttu-id="63c7c-190">Aby ustawić `ASPNETCORE_ENVIRONMENT` zmienną środowiskową dla aplikacji uruchomionej w izolowanej puli aplikacji (obsługiwane w usługach IIS 10.0 lub nowszy), zobacz *polecenia AppCmd.exe* części [zmienne środowiskowe &lt; environmentVariables&gt; ](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) tematu.</span><span class="sxs-lookup"><span data-stu-id="63c7c-190">To set the `ASPNETCORE_ENVIRONMENT` environment variable for an app running in an isolated Application Pool (supported on IIS 10.0 or later), see the *AppCmd.exe command* section of the [Environment Variables &lt;environmentVariables&gt;](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic.</span></span> <span data-ttu-id="63c7c-191">Gdy `ASPNETCORE_ENVIRONMENT` zmienna środowiskowa jest ustawiona dla puli aplikacji, jego wartość zastępuje ustawienie poziomie systemu.</span><span class="sxs-lookup"><span data-stu-id="63c7c-191">When the `ASPNETCORE_ENVIRONMENT` environment variable is set for an app pool, its value overrides a setting at the system level.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="63c7c-192">Hostowanie aplikacji w usługach IIS i dodawania lub zmieniania `ASPNETCORE_ENVIRONMENT` środowiska, użycie zmiennej zbliża się jedną z następujących ma nowej wartości, które są pobierane przez aplikacje:</span><span class="sxs-lookup"><span data-stu-id="63c7c-192">When hosting an app in IIS and adding or changing the `ASPNETCORE_ENVIRONMENT` environment variable, use any one of the following approaches to have the new value picked up by apps:</span></span>
>
> * <span data-ttu-id="63c7c-193">Wykonaj `net stop was /y` następuje `net start w3svc` z poziomu wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="63c7c-193">Execute `net stop was /y` followed by `net start w3svc` from a command prompt.</span></span>
> * <span data-ttu-id="63c7c-194">Uruchom ponownie serwer.</span><span class="sxs-lookup"><span data-stu-id="63c7c-194">Restart the server.</span></span>

### <a name="macos"></a><span data-ttu-id="63c7c-195">macOS</span><span class="sxs-lookup"><span data-stu-id="63c7c-195">macOS</span></span>

<span data-ttu-id="63c7c-196">Ustawienie bieżącego środowiska, dla systemu macOS może być wykonywane w tekście, podczas uruchamiania aplikacji:</span><span class="sxs-lookup"><span data-stu-id="63c7c-196">Setting the current environment for macOS can be performed in-line when running the app:</span></span>

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```

<span data-ttu-id="63c7c-197">Alternatywnie należy ustawić środowisko z `export` przed uruchomieniem aplikacji:</span><span class="sxs-lookup"><span data-stu-id="63c7c-197">Alternatively, set the environment with `export` prior to running the app:</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

<span data-ttu-id="63c7c-198">Zmienne środowiskowe na poziomie komputera są ustawiane w *.bashrc* lub *.bash_profile* pliku.</span><span class="sxs-lookup"><span data-stu-id="63c7c-198">Machine-level environment variables are set in the *.bashrc* or *.bash_profile* file.</span></span> <span data-ttu-id="63c7c-199">Edytuj plik za pomocą dowolnego edytora tekstów.</span><span class="sxs-lookup"><span data-stu-id="63c7c-199">Edit the file using any text editor.</span></span> <span data-ttu-id="63c7c-200">Dodaj następującą instrukcję:</span><span class="sxs-lookup"><span data-stu-id="63c7c-200">Add the following statement:</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

### <a name="linux"></a><span data-ttu-id="63c7c-201">Linux</span><span class="sxs-lookup"><span data-stu-id="63c7c-201">Linux</span></span>

<span data-ttu-id="63c7c-202">Dystrybucje systemu Linux, można użyć `export` polecenie w wierszu polecenia dla ustawień zmiennych oparte na sesji i *bash_profile* ustawienia maszyn na poziomie środowiska w pliku.</span><span class="sxs-lookup"><span data-stu-id="63c7c-202">For Linux distros, use the `export` command at a command prompt for session-based variable settings and *bash_profile* file for machine-level environment settings.</span></span>

### <a name="configuration-by-environment"></a><span data-ttu-id="63c7c-203">Konfiguracja według środowiska</span><span class="sxs-lookup"><span data-stu-id="63c7c-203">Configuration by environment</span></span>

<span data-ttu-id="63c7c-204">Aby załadować konfiguracji przez środowisko, zalecamy:</span><span class="sxs-lookup"><span data-stu-id="63c7c-204">To load configuration by environment, we recommend:</span></span>

* <span data-ttu-id="63c7c-205">*appSettings* pliki (\* appsettings.&lt; <Environment> &gt;JSON).</span><span class="sxs-lookup"><span data-stu-id="63c7c-205">*appsettings* files (\*appsettings.&lt;<Environment>&gt;.json).</span></span> <span data-ttu-id="63c7c-206">Zobacz [konfiguracji: Dostawca konfiguracji pliku](xref:fundamentals/configuration/index#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="63c7c-206">See [Configuration: File configuration provider](xref:fundamentals/configuration/index#file-configuration-provider).</span></span>
* <span data-ttu-id="63c7c-207">zmienne środowiskowe (ustawiona w każdym systemie którym hostowana jest aplikacja).</span><span class="sxs-lookup"><span data-stu-id="63c7c-207">environment variables (set on each system where the app is hosted).</span></span> <span data-ttu-id="63c7c-208">Zobacz [konfiguracji: Dostawca konfiguracji pliku](xref:fundamentals/configuration/index#file-configuration-provider) i [bezpieczne przechowywanie kluczy tajnych aplikacji w trakcie opracowywania: Zmienne środowiskowe](xref:security/app-secrets#environment-variables).</span><span class="sxs-lookup"><span data-stu-id="63c7c-208">See [Configuration: File configuration provider](xref:fundamentals/configuration/index#file-configuration-provider) and [Safe storage of app secrets in development: Environment variables](xref:security/app-secrets#environment-variables).</span></span>
* <span data-ttu-id="63c7c-209">Klucz tajny Menedżer (w środowisku programistycznym tylko).</span><span class="sxs-lookup"><span data-stu-id="63c7c-209">Secret Manager (in the Development environment only).</span></span> <span data-ttu-id="63c7c-210">Zobacz <xref:security/app-secrets>.</span><span class="sxs-lookup"><span data-stu-id="63c7c-210">See <xref:security/app-secrets>.</span></span>

## <a name="environment-based-startup-class-and-methods"></a><span data-ttu-id="63c7c-211">Klasa początkowa oparte na środowisku i metody</span><span class="sxs-lookup"><span data-stu-id="63c7c-211">Environment-based Startup class and methods</span></span>

### <a name="startup-class-conventions"></a><span data-ttu-id="63c7c-212">Konwencje klasy uruchamiania</span><span class="sxs-lookup"><span data-stu-id="63c7c-212">Startup class conventions</span></span>

<span data-ttu-id="63c7c-213">Po uruchomieniu aplikacji ASP.NET Core [Klasa początkowa](xref:fundamentals/startup) używa do ładowania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="63c7c-213">When an ASP.NET Core app starts, the [Startup class](xref:fundamentals/startup) bootstraps the app.</span></span> <span data-ttu-id="63c7c-214">Aplikację można zdefiniować w oddzielnym `Startup` klas w różnych środowiskach (na przykład `StartupDevelopment`), a odpowiedni `Startup` klasy jest zaznaczona w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="63c7c-214">The app can define separate `Startup` classes for different environments (for example, `StartupDevelopment`), and the appropriate `Startup` class is selected at runtime.</span></span> <span data-ttu-id="63c7c-215">Klasy, w których sufiks nazwy pasuje do bieżącego środowiska jest podzielony na priorytety.</span><span class="sxs-lookup"><span data-stu-id="63c7c-215">The class whose name suffix matches the current environment is prioritized.</span></span> <span data-ttu-id="63c7c-216">Jeśli pasujący obiekt typu `Startup{EnvironmentName}` klasy nie zostanie znaleziona, `Startup` klasa jest używana.</span><span class="sxs-lookup"><span data-stu-id="63c7c-216">If a matching `Startup{EnvironmentName}` class isn't found, the `Startup` class is used.</span></span>

<span data-ttu-id="63c7c-217">Aby zaimplementować oparte na środowisku `Startup` Tworzenie klas, `Startup{EnvironmentName}` klasy dla każdego środowiska w użyciu i rezerwowe `Startup` klasy:</span><span class="sxs-lookup"><span data-stu-id="63c7c-217">To implement environment-based `Startup` classes, create a `Startup{EnvironmentName}` class for each environment in use and a fallback `Startup` class:</span></span>

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

<span data-ttu-id="63c7c-218">Użyj [UseStartup (IWebHostBuilder, ciąg)](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usestartup) przeciążenie, które akceptuje to nazwa zestawu:</span><span class="sxs-lookup"><span data-stu-id="63c7c-218">Use the [UseStartup(IWebHostBuilder, String)](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usestartup) overload that accepts an assembly name:</span></span>

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

### <a name="startup-method-conventions"></a><span data-ttu-id="63c7c-219">Konwencje metod uruchamiania</span><span class="sxs-lookup"><span data-stu-id="63c7c-219">Startup method conventions</span></span>

<span data-ttu-id="63c7c-220">[Konfigurowanie](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) i [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) obsługuje specyficznymi dla środowiska wersji formularza `Configure<EnvironmentName>` i `Configure<EnvironmentName>Services`:</span><span class="sxs-lookup"><span data-stu-id="63c7c-220">[Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) support environment-specific versions of the form `Configure<EnvironmentName>` and `Configure<EnvironmentName>Services`:</span></span>

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet_all&highlight=15,51)]

## <a name="additional-resources"></a><span data-ttu-id="63c7c-221">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="63c7c-221">Additional resources</span></span>

* <xref:fundamentals/startup>
* <xref:fundamentals/configuration/index>
* [<span data-ttu-id="63c7c-222">IHostingEnvironment.EnvironmentName</span><span class="sxs-lookup"><span data-stu-id="63c7c-222">IHostingEnvironment.EnvironmentName</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname)
