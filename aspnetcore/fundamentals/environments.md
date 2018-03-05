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
# <a name="working-with-multiple-environments"></a>Praca w środowiskach wielu

przez [Rick Anderson](https://twitter.com/RickAndMSFT)

Platformy ASP.NET Core obsługuje ustawienie aplikacji w czasie wykonywania zmiennych środowiskowych.

[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

## <a name="environments"></a>Środowisk

Zmienna środowiskowa odczytuje platformy ASP.NET Core `ASPNETCORE_ENVIRONMENT` przy uruchamianiu aplikacji i magazyny wartość w [IHostingEnvironment.EnvironmentName](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName). `ASPNETCORE_ENVIRONMENT` można ustawić dowolną wartość, ale [trzy wartości](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname?view=aspnetcore-2.0) są obsługiwane przez platformę: [programowanie](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development?view=aspnetcore-2.0), [przemieszczania](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging?view=aspnetcore-2.0), i [produkcji](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production?view=aspnetcore-2.0). Jeśli `ASPNETCORE_ENVIRONMENT` nie jest ustawiona, domyślnie zostanie użyta `Production`.

[!code-csharp[](environments/sample/WebApp1/Startup.cs?name=snippet)]

Poprzedni kod:

* Wywołania [UseDeveloperExceptionPage](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_DeveloperExceptionPageExtensions_UseDeveloperExceptionPage_Microsoft_AspNetCore_Builder_IApplicationBuilder_) i [UseBrowserLink](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_BrowserLinkExtensions_UseBrowserLink_Microsoft_AspNetCore_Builder_IApplicationBuilder_) podczas `ASPNETCORE_ENVIRONMENT` ma ustawioną wartość `Development`.
* Wywołania [UseExceptionHandler](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ExceptionHandlerExtensions_UseExceptionHandler_Microsoft_AspNetCore_Builder_IApplicationBuilder_) podczas wartość `ASPNETCORE_ENVIRONMENT` ustawiono jedną z następujących czynności:

    * `Staging`
    * `Production`
    * `Staging_2`

[Pomocnika Tag środowiska ](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) używa wartości `IHostingEnvironment.EnvironmentName` do dołączania lub wykluczania znaczników w elemencie:

[!code-html[](environments/sample/WebApp1/Pages/About.cshtml)]

Uwaga: W systemach Windows i macOS zmienne środowiskowe i wartości nie są z uwzględnieniem wielkości liter. Zmienne środowiskowe systemu Linux i wartości są **z uwzględnieniem wielkości liter** domyślnie.

### <a name="development"></a>Tworzenie

Środowisko projektowe można włączyć funkcje, które nie powinny być dostępne w środowisku produkcyjnym. Na przykład włączyć szablonów platformy ASP.NET Core [developer wyjątku strony](xref:fundamentals/error-handling#the-developer-exception-page) w środowisku programistycznym.

Środowisko rozwoju lokalnych komputera można skonfigurować *Properties\launchSettings.json* pliku projektu. Ustaw wartości środowiska *launchSettings.json* przesłaniają wartości w środowisku systemu.

Następujące JSON zawiera trzy profile z *launchSettings.json* pliku:

[!code-json[](environments/sample/WebApp1/Properties/launchSettings.json?highlight=10,11,18,26)]

Gdy aplikacja jest uruchamiana z [dotnet Uruchom](/dotnet/core/tools/dotnet-run), pierwszy profil z `"commandName": "Project"` będą używane. Wartość `commandName` Określa serwer sieci web do uruchomienia. `commandName` może to być jedna z:

* Usługi IIS Express
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

Zmiany wprowadzone do profilów projektu mogą nie zostać zastosowane do czasu ponownego uruchomienia serwera sieci web. Przed wykryje zmiany wprowadzone w jego środowisku, należy ponownie uruchomić kestrel.

>[!WARNING]
> *launchSettings.json* nie należy przechowywać kluczy tajnych. [Narzędzie Menedżer klucz tajny](xref:security/app-secrets) może służyć do przechowywania kluczy tajnych dla rozwoju lokalnych.

### <a name="production"></a>Produkcji

Aby zmaksymalizować zabezpieczeń, wydajności i niezawodności aplikacji należy skonfigurować środowiska produkcyjnego. Niektóre typowe ustawienia, które różnią się od rozwoju obejmują:

* Buforowanie.
* Zasoby po stronie klienta są powiązane, zminimalizowane i potencjalnie pochodzący z sieci CDN.
* Strony błędów diagnostycznych wyłączone.
* Strony błędów przyjazną włączone.
* Rejestrowanie produkcyjnych i włączone monitorowanie. Na przykład [usługi Application Insights](/azure/application-insights/app-insights-asp-net-core).

## <a name="setting-the-environment"></a>Ustawienia środowiska

Często jest to przydatne ustawić w określonym środowisku do testowania. Jeśli środowisko nie jest ustawiona, domyślnie zostanie użyta `Production` która wyłącza większość funkcji debugowania.

Metoda do ustawiania środowiska zależy od systemu operacyjnego.

### <a name="azure"></a>Azure

Usługi aplikacji Azure:

* Wybierz **ustawienia aplikacji** bloku.
* Dodaj klucz i wartość w **ustawień aplikacji**.


### <a name="windows"></a>Windows
Aby ustawić `ASPNETCORE_ENVIRONMENT` dla bieżącej sesji, jeśli aplikacja zostanie uruchomiona przy użyciu [dotnet Uruchom](/dotnet/core/tools/dotnet-run), są używane następujące polecenia

**Wiersz polecenia**
```
set ASPNETCORE_ENVIRONMENT=Development
```
**PowerShell**
```
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

Te polecenia brane pod uwagę tylko dla bieżącego okna. Po zamknięciu okna ustawienie ASPNETCORE_ENVIRONMENT Przywraca wartości domyślne ustawienie lub komputera. Aby można było ustawić wartość globalnie dla systemu Windows otwórz **Panelu sterowania** > **systemu** > **Zaawansowane ustawienia systemu** i Dodaj lub Edytuj `ASPNETCORE_ENVIRONMENT` wartość.

![Zaawansowane właściwości systemu](environments/_static/systemsetting_environment.png)

![Zmienna środowiskowa Core ASPNET](environments/_static/windows_aspnetcore_environment.png)


**web.config**

Zobacz *ustawienia zmiennych środowiskowych* sekcji [odwołania konfiguracji platformy ASP.NET Core modułu](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) tematu.

**Dla każdej puli aplikacji usług IIS**

Aby ustawić zmienne środowiskowe dla poszczególnych aplikacji uruchomionych w izolowanej pulach aplikacji (obsługiwane w usługach IIS 10.0 +), zobacz *polecenia AppCmd.exe* sekcji [zmiennych środowiskowych \< environmentVariables >](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) tematu.

### <a name="macos"></a>macOS
Ustawianie bieżącego środowiska pod kątem macOS może odbywać się w wierszu przy uruchamianiu aplikacji;

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```
lub przy użyciu `export` ustaw go przed uruchomieniem aplikacji.

```bash
export ASPNETCORE_ENVIRONMENT=Development
```
Zmienne środowiskowe poziomu są ustawiane w *.bashrc* lub *.bash_profile* pliku. Przeprowadź edycję pliku za pomocą dowolnego edytora tekstu, a następnie dodaj następującą instrukcję.

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

### <a name="linux"></a>Linux
Dystrybucjach systemu Linux, można użyć `export` polecenie w wierszu polecenia dla zmiennej ustawieniami opartymi na sesji i *bash_profile* w pliku ustawień z poziomu środowiska maszyny.

### <a name="configuration-by-environment"></a>Konfiguracja przez środowisko

Zobacz [konfiguracji przez środowisko](xref:fundamentals/configuration/index#configuration-by-environment) Aby uzyskać więcej informacji.

<a name="startup-conventions"></a>
## <a name="environment-based-startup-class-and-methods"></a>Klasa początkowa i metod opartych na środowisku

Po uruchomieniu aplikacji platformy ASP.NET Core [Klasa początkowa](xref:fundamentals/startup) używa do ładowania aplikacji. Jeśli klasa `Startup{EnvironmentName}` istnieje, że klasa zostanie wywołana dla tej `EnvironmentName`:

[!code-csharp[](environments/sample/WebApp1/StartupDev.cs?name=snippet&highlight=1)]

Uwaga: Wywołanie [WebHostBuilder.UseStartup<TStartup> ](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) zastępuje sekcji konfiguracyjnych.

[Skonfiguruj](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_StartupBase_Configure_Microsoft_AspNetCore_Builder_IApplicationBuilder_) i [ConfigureServices](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices?view=aspnetcore-2.0) obsługuje środowisko określonych wersji formularza `Configure{EnvironmentName}` i `Configure{EnvironmentName}Services`:

[!code-csharp[](environments/sample/WebApp1/Startup.cs?name=snippet_all&highlight=15,37)]

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Uruchamianie aplikacji](xref:fundamentals/startup)
* [Konfiguracja](xref:fundamentals/configuration/index)
* [IHostingEnvironment.EnvironmentName](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName)
