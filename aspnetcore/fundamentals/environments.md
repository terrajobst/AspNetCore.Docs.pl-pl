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
# <a name="use-multiple-environments-in-aspnet-core"></a>Użyj wiele środowisk w ASP.NET Core

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

Platformy ASP.NET Core konfiguruje zachowanie aplikacji opartych na środowisku środowiska uruchomieniowego za pomocą zmiennej środowiskowej.

[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

## <a name="environments"></a>Środowisk

Zmienna środowiskowa odczytuje platformy ASP.NET Core `ASPNETCORE_ENVIRONMENT` przy uruchamianiu aplikacji i przechowuje wartość [IHostingEnvironment.EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname). Można ustawić `ASPNETCORE_ENVIRONMENT` do żadnej wartości, ale [trzy wartości](/dotnet/api/microsoft.aspnetcore.hosting.environmentname) są obsługiwane przez platformę: [programowanie](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development), [przemieszczania](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging), i [produkcji](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production). Jeśli `ASPNETCORE_ENVIRONMENT` nie jest ustawiona domyślnie `Production`.

[!code-csharp[](environments/sample/WebApp1/Startup.cs?name=snippet)]

Poprzedni kod:

* Wywołania [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) i [UseBrowserLink](/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink) podczas `ASPNETCORE_ENVIRONMENT` ma ustawioną wartość `Development`.
* Wywołania [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler) podczas wartość `ASPNETCORE_ENVIRONMENT` ustawiono jedną z następujących czynności:

    * `Staging`
    * `Production`
    * `Staging_2`

[Pomocnika Tag środowiska](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) używa wartości `IHostingEnvironment.EnvironmentName` do dołączania lub wykluczania znaczników w elemencie:

[!code-cshtml[](environments/sample-snapshot/WebApp1/Pages/About.cshtml)]

W systemach Windows i macOS zmienne środowiskowe i wartości nie jest uwzględniana wielkość liter. Zmienne środowiskowe systemu Linux i wartości są **z uwzględnieniem wielkości liter** domyślnie.

### <a name="development"></a>Tworzenie

Środowisko projektowe można włączyć funkcje, które nie powinny być dostępne w środowisku produkcyjnym. Na przykład włączyć szablonów platformy ASP.NET Core [developer wyjątku strony](xref:fundamentals/error-handling#the-developer-exception-page) w środowisku programistycznym.

Środowisko rozwoju lokalnych komputera można skonfigurować *Properties\launchSettings.json* pliku projektu. Ustaw wartości środowiska *launchSettings.json* przesłaniają wartości w środowisku systemu.

Następujące JSON zawiera trzy profile z *launchSettings.json* pliku:

[!code-json[](environments/sample/WebApp1/Properties/launchSettings.json?highlight=10,11,18,26)]

::: moniker range=">= aspnetcore-2.1"

> [!NOTE]
> `applicationUrl` Właściwości w *launchSettings.json* można określić listę adresów URL serwera. Użyj średnika między adresami URL na liście:
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

Gdy aplikacja jest uruchamiana z [dotnet Uruchom](/dotnet/core/tools/dotnet-run), pierwszy profil z `"commandName": "Project"` jest używany. Wartość `commandName` Określa serwer sieci web do uruchomienia. `commandName` może być jednym z następujących czynności:

* IIS Express
* IIS
* Projekt (który uruchamia Kestrel)

Gdy aplikacja jest uruchamiana z [dotnet Uruchom](/dotnet/core/tools/dotnet-run):

* *launchSettings.json* jest do odczytu. Jeśli jest dostępna. `environmentVariables` ustawienia w *launchSettings.json* zastąpienia zmiennych środowiskowych.
* Środowisko macierzyste są wyświetlane.

Następujące dane wyjściowe zawiera wprowadzenie do aplikacji [dotnet Uruchom](/dotnet/core/tools/dotnet-run):

```bash
PS C:\Webs\WebApp1> dotnet run
Using launch settings from C:\Webs\WebApp1\Properties\launchSettings.json...
Hosting environment: Staging
Content root path: C:\Webs\WebApp1
Now listening on: http://localhost:54340
Application started. Press Ctrl+C to shut down.
```

Visual Studio **debugowania** karta zawiera graficzny interfejs użytkownika do edycji *launchSettings.json* pliku:

![Zmienne środowiskowe ustawienie właściwości projektu](environments/_static/project-properties-debug.png)

Zmiany wprowadzone do profilów projektu mogą nie zostać zastosowane do czasu ponownego uruchomienia serwera sieci web. Aby zmiany wprowadzone w jego środowisku może wykryć, należy ponownie uruchomić kestrel.

> [!WARNING]
> *launchSettings.json* nie należy przechowywać kluczy tajnych. [Narzędzie Menedżer klucz tajny](xref:security/app-secrets) może służyć do przechowywania kluczy tajnych dla rozwoju lokalnych.

Korzystając z [Visual Studio Code](https://code.visualstudio.com/), można ustawić zmienne środowiskowe w *.vscode/launch.json* pliku. Poniższy przykład przedstawia środowiska `Development`:

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

A *.vscode/launch.json* plik w projekcie nie jest do odczytu podczas uruchamiania aplikacji z `dotnet run` w taki sam sposób jak *Properties/launchSettings.json*. Podczas uruchamiania aplikacji w projektowania, które nie ma *launchSettings.json* pliku, należy ustawić środowisko zmienną środowiskową lub argumentu wiersza polecenia do `dotnet run` polecenia.

### <a name="production"></a>Produkcji

Aby zmaksymalizować zabezpieczeń, wydajności i niezawodności aplikacji należy skonfigurować w środowisku produkcyjnym. Niektóre typowe ustawienia, które różnią się od rozwoju obejmują:

* Buforowanie.
* Zasoby po stronie klienta są powiązane, zminimalizowane i potencjalnie pochodzący z sieci CDN.
* Strony błędów diagnostycznych wyłączone.
* Strony błędów przyjazną włączone.
* Rejestrowanie produkcyjnych i włączone monitorowanie. Na przykład [usługi Application Insights](/azure/application-insights/app-insights-asp-net-core).

## <a name="setting-the-environment"></a>Ustawienia środowiska

Często jest to przydatne ustawić w określonym środowisku do testowania. Jeśli środowisko nie jest ustawiona, domyślne `Production`, która wyłącza większość funkcji debugowania. Metoda do ustawiania środowiska zależy od systemu operacyjnego.

### <a name="azure-app-service"></a>Usługa aplikacji Azure

Aby ustawić środowiska w [usłudze Azure App Service](https://azure.microsoft.com/services/app-service/), wykonaj następujące czynności:

1. Wybierz aplikację z **usługi aplikacji** bloku.
1. W **ustawienia** grupy wybierz **ustawienia aplikacji** bloku.
1. W **ustawienia aplikacji** wybierz opcję **Dodaj nowe ustawienie**.
1. Aby uzyskać **wprowadź nazwę**, podaj `ASPNETCORE_ENVIRONMENT`. Dla **wprowadź wartość**, podaj środowisku (na przykład `Staging`).
1. Wybierz **ustawienie miejsca** pole wyboru, jeśli chcesz, aby ustawienia środowiska do bieżącego miejsca po są zamienione miejsc wdrożenia. Aby uzyskać więcej informacji, zobacz [dokumentacji platformy Azure: ustawienia, które są zamienione?](/azure/app-service/web-sites-staged-publishing).
1. Wybierz **zapisać** w górnej części bloku.

Usługa aplikacji Azure automatyczne ponowne uruchomienie aplikacji po ustawienie aplikacji (zmienną środowiskową) jest dodać, zmienić lub usunąć w portalu Azure.

### <a name="windows"></a>Windows

Aby ustawić `ASPNETCORE_ENVIRONMENT` dla bieżącej sesji, po uruchomieniu aplikacji przy użyciu [dotnet Uruchom](/dotnet/core/tools/dotnet-run), są używane następujące polecenia:

**Wiersz polecenia**

```console
set ASPNETCORE_ENVIRONMENT=Development
```

**Środowiska PowerShell**

```powershell
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

Te polecenia tylko obowiązywać dla bieżącego okna. Po zamknięciu okna `ASPNETCORE_ENVIRONMENT` ustawienie powraca do wartości domyślne ustawienie lub komputera. Aby ustawić wartość globalnie w systemie Windows, otwórz **Panelu sterowania** > **systemu** > **Zaawansowane ustawienia systemu** i Dodaj lub Edytuj `ASPNETCORE_ENVIRONMENT`wartość:

![Zaawansowane właściwości systemu](environments/_static/systemsetting_environment.png)

![Zmienna środowiskowa Core ASPNET](environments/_static/windows_aspnetcore_environment.png)

**web.config**

Zobacz *ustawienia zmiennych środowiskowych* sekcji [odwołania konfiguracji platformy ASP.NET Core modułu](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) tematu.

**Dla każdej puli aplikacji usług IIS**

Aby ustawić zmienne środowiskowe dla poszczególnych aplikacji uruchomionych w izolowanej pulach aplikacji (obsługiwane w usługach IIS 10.0 +), zobacz *polecenia AppCmd.exe* sekcji [zmiennych środowiskowych &lt; environmentVariables&gt; ](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) tematu.

### <a name="macos"></a>macOS

Ustawienie w bieżącym środowisku macOS mogą być wykonywane w wierszu, podczas uruchamiania aplikacji:

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```

Możesz również ustawić środowisko `export` przed uruchomieniem aplikacji:

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

Zmienne środowiskowe poziomie komputera są ustawiane *.bashrc* lub *.bash_profile* pliku. Przeprowadź edycję pliku za pomocą dowolnego edytora tekstu. Dodaj następującą instrukcję:

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

### <a name="linux"></a>Linux

Dystrybucjach systemu Linux, można użyć `export` polecenie w wierszu polecenia dla ustawienia zmiennej oparte na sesji i *bash_profile* ustawienia środowiska na poziomie komputera w pliku.

### <a name="configuration-by-environment"></a>Konfiguracja przez środowisko

Zobacz [konfiguracji przez środowisko](xref:fundamentals/configuration/index#configuration-by-environment) Aby uzyskać więcej informacji.

## <a name="environment-based-startup-class-and-methods"></a>Na podstawie środowiska uruchamiania klasy i metody

Po uruchomieniu aplikacji platformy ASP.NET Core [Klasa początkowa](xref:fundamentals/startup) używa do ładowania aplikacji. Jeśli `Startup{EnvironmentName}` istnieje klasy, nosi nazwę klasy w tym `EnvironmentName`:

[!code-csharp[](environments/sample/WebApp1/StartupDev.cs?name=snippet&highlight=1)]

[WebHostBuilder.UseStartup<TStartup> ](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) zastępuje sekcji konfiguracyjnych.

[Skonfiguruj](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) i [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) obsługuje wersji określonego środowiska formularza `Configure{EnvironmentName}` i `Configure{EnvironmentName}Services`:

[!code-csharp[](environments/sample/WebApp1/Startup.cs?name=snippet_all&highlight=15,37)]

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Uruchamianie aplikacji](xref:fundamentals/startup)
* [Konfiguracja](xref:fundamentals/configuration/index)
* [IHostingEnvironment.EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname)
