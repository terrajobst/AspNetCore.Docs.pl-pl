---
title: Używanie wielu środowisk w ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak kontrolować zachowanie aplikacji w wielu środowiskach w aplikacjach ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/17/2019
uid: fundamentals/environments
ms.openlocfilehash: b0218b2c77c283c0849dca9491046534b88c5a77
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78656220"
---
# <a name="use-multiple-environments-in-aspnet-core"></a>Używanie wielu środowisk w ASP.NET Core

::: moniker range=">= aspnetcore-3.0"

Autor: [Rick Anderson](https://twitter.com/RickAndMSFT)

ASP.NET Core konfiguruje zachowanie aplikacji na podstawie środowiska uruchomieniowego przy użyciu zmiennej środowiskowej.

[Wyświetl lub pobierz przykładowy kod](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([jak pobrać](xref:index#how-to-download-a-sample))

## <a name="environments"></a>Środowiska

ASP.NET Core odczytuje zmienną środowiskową `ASPNETCORE_ENVIRONMENT` podczas uruchamiania aplikacji i zapisuje wartość w [IWebHostEnvironment. EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.EnvironmentName). `ASPNETCORE_ENVIRONMENT` można ustawić na dowolną wartość, ale w strukturze są dostarczane trzy wartości:

* <xref:Microsoft.Extensions.Hosting.Environments.Development>
* <xref:Microsoft.Extensions.Hosting.Environments.Staging>
* <xref:Microsoft.Extensions.Hosting.Environments.Production> (wartość domyślna)

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet)]

Powyższy kod:

* Wywołuje [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) , gdy `ASPNETCORE_ENVIRONMENT` jest ustawiony na `Development`.
* Wywołuje [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler) , gdy wartość `ASPNETCORE_ENVIRONMENT` jest ustawiona na jedną z następujących wartości:

  * `Staging`
  * `Production`
  * `Staging_2`

[Pomocnik tagu środowiska](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) używa wartości `IHostingEnvironment.EnvironmentName` do dołączania lub wykluczania znaczników w elemencie:

[!code-cshtml[](environments/sample-snapshot/EnvironmentsSample/Pages/About.cshtml)]

W systemach Windows i macOS, zmienne środowiskowe i wartości nie uwzględniają wielkości liter. Zmienne środowiskowe systemu Linux i wartości domyślnie **uwzględniają wielkość** liter.

### <a name="development"></a>Opracowywanie zawartości

Środowisko programistyczne może włączyć funkcje, które nie powinny być ujawnione w środowisku produkcyjnym. Na przykład szablony ASP.NET Core umożliwiają [stronie wyjątków dla deweloperów](xref:fundamentals/error-handling#developer-exception-page) w środowisku deweloperskim.

Środowisko do tworzenia maszyn lokalnych można ustawić w pliku *Properties\launchSettings.JSON* projektu. Wartości środowiskowe ustawione w wartościach przesłonięcia *profilu launchsettings. JSON* ustawione w środowisku systemowym.

Poniższy kod JSON przedstawia trzy profile z pliku *profilu launchsettings. JSON* :

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
> Właściwość `applicationUrl` w pliku *profilu launchsettings. JSON* może określać listę adresów URL serwera. Użyj średnika między adresami URL na liście:
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

Gdy aplikacja jest [uruchamiana z uruchomieniem dotnet](/dotnet/core/tools/dotnet-run), zostanie użyty pierwszy profil z `"commandName": "Project"`. Wartość `commandName` Określa serwer sieci Web do uruchomienia. `commandName` może być jedną z następujących:

* `IISExpress`
* `IIS`
* `Project` (co spowoduje uruchomienie Kestrel)

Gdy aplikacja jest uruchamiana z [uruchomieniem dotnet](/dotnet/core/tools/dotnet-run):

* plik *profilu launchsettings. JSON* jest odczytywany, jeśli jest dostępny. `environmentVariables` ustawienia w zmiennych środowiskowych *profilu launchsettings. JSON* .
* Zostanie wyświetlone środowisko hostingu.

Następujące dane wyjściowe pokazują aplikację uruchomioną z [uruchomieniem dotnet](/dotnet/core/tools/dotnet-run):

```bash
PS C:\Websites\EnvironmentsSample> dotnet run
Using launch settings from C:\Websites\EnvironmentsSample\Properties\launchSettings.json...
Hosting environment: Staging
Content root path: C:\Websites\EnvironmentsSample
Now listening on: http://localhost:54340
Application started. Press Ctrl+C to shut down.
```

Karta **debugowanie** właściwości projektu programu Visual Studio udostępnia graficzny interfejs użytkownika służący do edytowania pliku *profilu launchsettings. JSON* :

![Ustawienia właściwości projektu zmienne środowiskowe](environments/_static/project-properties-debug.png)

Zmiany wprowadzone w profilach projektu mogą zacząć obowiązywać dopiero po ponownym uruchomieniu serwera sieci Web. Kestrel musi zostać uruchomiona ponownie, zanim będzie mógł wykryć zmiany wprowadzone w środowisku.

> [!WARNING]
> plik *profilu launchsettings. JSON* nie powinien przechowywać wpisów tajnych. [Narzędzia do zarządzania kluczami tajnymi](xref:security/app-secrets) można używać do przechowywania wpisów tajnych na potrzeby lokalnego tworzenia oprogramowania.

W przypadku korzystania z [Visual Studio Code](https://code.visualstudio.com/)zmienne środowiskowe można ustawić w pliku *. programu vscode/Launch. JSON* . Poniższy przykład ustawia środowisko na `Development`:

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

Plik *. programu vscode/Launch. JSON* w projekcie nie jest odczytywany podczas uruchamiania aplikacji przy użyciu `dotnet run` w taki sam sposób, jak *właściwości/profilu launchsettings. JSON*. Podczas uruchamiania aplikacji w środowisku deweloperskim, która nie ma pliku *profilu launchsettings. JSON* , należy ustawić środowisko przy użyciu zmiennej środowiskowej lub argumentu wiersza polecenia do polecenia `dotnet run`.

### <a name="production"></a>Produkcja

Środowisko produkcyjne powinno być skonfigurowane w celu zmaksymalizowania zabezpieczeń, wydajności i niezawodności aplikacji. Niektóre typowe ustawienia, które różnią się od programowania, to m.in.:

* Pamięć.
* Zasoby po stronie klienta są powiązane, zminimalizowanego i mogą być obsługiwane przez sieć CDN.
* Wyłączono strony błędów diagnostyki.
* Włączono przyjazne strony błędów.
* Włączono rejestrowanie i monitorowanie produkcji. Na przykład [Application Insights](/azure/application-insights/app-insights-asp-net-core).

## <a name="set-the-environment"></a>Ustawianie środowiska

Często warto ustawić określone środowisko do testowania przy użyciu zmiennej środowiskowej lub ustawienia platformy. Jeśli środowisko nie jest ustawione, domyślnie `Production`, co spowoduje wyłączenie większości funkcji debugowania. Metoda ustawiania środowiska zależy od systemu operacyjnego.

Po skompilowaniu hosta ostatnie ustawienie środowiska odczytywane przez aplikację określa środowisko aplikacji. Nie można zmienić środowiska aplikacji, gdy aplikacja jest uruchomiona.

### <a name="environment-variable-or-platform-setting"></a>Zmienna środowiskowa lub ustawienie platformy

#### <a name="azure-app-service"></a>Azure App Service

Aby ustawić środowisko w [Azure App Service](https://azure.microsoft.com/services/app-service/), wykonaj następujące czynności:

1. Wybierz aplikację z bloku **App Services** .
1. W grupie **Ustawienia** wybierz blok **Konfiguracja** .
1. Na karcie **Ustawienia aplikacji** wybierz pozycję **nowe ustawienie aplikacji**.
1. W oknie **Dodawanie/Edytowanie ustawienia aplikacji** Podaj `ASPNETCORE_ENVIRONMENT` dla **nazwy**. W polu **wartość**Podaj środowisko (na przykład `Staging`).
1. Zaznacz pole wyboru **ustawienie miejsca wdrożenia** , jeśli chcesz, aby ustawienie środowiska pozostawało w bieżącym gnieździe w przypadku wymiany miejsc wdrożenia. Aby uzyskać więcej informacji, zobacz [Konfigurowanie środowisk przejściowych w Azure App Service](/azure/app-service/web-sites-staged-publishing) w dokumentacji platformy Azure.
1. Wybierz **przycisk OK** , aby zamknąć okno **Dodawanie/Edytowanie ustawienia aplikacji** .
1. Wybierz pozycję **Zapisz** w górnej części bloku **Konfiguracja** .

Azure App Service automatycznie uruchamia ponownie aplikację po dodaniu, zmianie lub usunięciu ustawienia aplikacji (zmienna środowiskowa) w Azure Portal.

#### <a name="windows"></a>System Windows

Aby ustawić `ASPNETCORE_ENVIRONMENT` bieżącej sesji, gdy aplikacja zostanie uruchomiona przy użyciu [uruchomienia dotnet](/dotnet/core/tools/dotnet-run), są używane następujące polecenia:

**Wiersz polecenia**

```console
set ASPNETCORE_ENVIRONMENT=Development
```

**PowerShell**

```powershell
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

Te polecenia zaczną obowiązywać tylko dla bieżącego okna. Po zamknięciu okna ustawienie `ASPNETCORE_ENVIRONMENT` przywraca ustawienie domyślne lub wartość maszyny.

Aby ustawić wartość globalnie w systemie Windows, użyj jednej z następujących metod:

* Otwórz **Panel sterowania** > **system** > **Zaawansowane ustawienia systemowe** i Dodaj lub Edytuj wartość `ASPNETCORE_ENVIRONMENT`:

  ![Zaawansowane właściwości systemu](environments/_static/systemsetting_environment.png)

  ![Zmienna środowiskowa ASPNET Core](environments/_static/windows_aspnetcore_environment.png)

* Otwórz wiersz polecenia z uprawnieniami administracyjnymi i użyj polecenia `setx` lub Otwórz wiersz polecenia administracyjnych programu PowerShell i użyj `[Environment]::SetEnvironmentVariable`:

  **Wiersz polecenia**

  ```console
  setx ASPNETCORE_ENVIRONMENT Development /M
  ```

  Przełącznik `/M` wskazuje, aby ustawić zmienną środowiskową na poziomie systemu. Jeśli przełącznik `/M` nie jest używany, zmienna środowiskowa jest ustawiana dla konta użytkownika.

  **PowerShell**

  ```powershell
  [Environment]::SetEnvironmentVariable("ASPNETCORE_ENVIRONMENT", "Development", "Machine")
  ```

  Opcja `Machine` wartość wskazuje, aby ustawić zmienną środowiskową na poziomie systemu. Jeśli wartość opcji zostanie zmieniona na `User`, zmienna środowiskowa jest ustawiana dla konta użytkownika.

Gdy zmienna środowiskowa `ASPNETCORE_ENVIRONMENT` jest ustawiona globalnie, zacznie obowiązywać `dotnet run` w każdym oknie polecenia otwartym po ustawieniu wartości.

**plik Web. config**

Aby ustawić zmienną środowiskową `ASPNETCORE_ENVIRONMENT` za pomocą *pliku Web. config*, zobacz sekcję *Ustawianie zmiennych środowiskowych* w programie <xref:host-and-deploy/aspnet-core-module#setting-environment-variables>.

**Plik projektu lub profil publikacji**

**W przypadku wdrożeń usług Windows IIS:** Uwzględnij Właściwość `<EnvironmentName>` w [profilu publikowania (pubxml)](xref:host-and-deploy/visual-studio-publish-profiles) lub pliku projektu. To podejście ustawia środowisko w *pliku Web. config* po opublikowaniu projektu:

```xml
<PropertyGroup>
  <EnvironmentName>Development</EnvironmentName>
</PropertyGroup>
```

**Na pulę aplikacji IIS**

Aby ustawić zmienną środowiskową `ASPNETCORE_ENVIRONMENT` dla aplikacji uruchomionej w izolowanej puli aplikacji (obsługiwanej przez usługi IIS 10,0 lub nowszej), zobacz sekcję *polecenie Appcmd. exe* w temacie [zmienne środowiskowe &lt;environmentVariables&gt;](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) temat. Gdy zmienna środowiskowa `ASPNETCORE_ENVIRONMENT` jest ustawiona dla puli aplikacji, jej wartość zastępuje ustawienie na poziomie systemu.

> [!IMPORTANT]
> Podczas hostowania aplikacji w usługach IIS i dodawania lub zmieniania zmiennej środowiskowej `ASPNETCORE_ENVIRONMENT` należy użyć jednego z poniższych metod, aby nowa wartość była pobierana przez aplikacje:
>
> * Wykonaj `net stop was /y` a następnie `net start w3svc` z wiersza polecenia.
> * Uruchom ponownie serwer.

#### <a name="macos"></a>macOS

Ustawienie bieżącego środowiska dla macOS można wykonać w trybie online podczas uruchamiania aplikacji:

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```

Alternatywnie można ustawić środowisko z `export` przed uruchomieniem aplikacji:

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

Zmienne środowiskowe na poziomie komputera są ustawiane w pliku *. bashrc* lub *. bash_profile* . Edytuj plik przy użyciu dowolnego edytora tekstu. Dodaj następującą instrukcję:

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

#### <a name="linux"></a>Linux

W przypadku systemu Linux dystrybucje Użyj polecenia `export` w wierszu polecenia dla ustawień zmiennych opartych na sesji i pliku *bash_profile* dla ustawień środowiska na poziomie komputera.

### <a name="set-the-environment-in-code"></a>Ustawianie środowiska w kodzie

Wywołaj <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseEnvironment*> podczas kompilowania hosta. Zobacz: <xref:fundamentals/host/generic-host#environmentname>.


### <a name="configuration-by-environment"></a>Konfiguracja według środowiska

Aby załadować konfigurację według środowiska, zalecamy:

* pliki *AppSettings* (*appSettings. { Environment}. JSON*). Zobacz: <xref:fundamentals/configuration/index#json-configuration-provider>.
* Zmienne środowiskowe (ustawiane dla każdego systemu, w którym jest hostowana aplikacja). Zobacz <xref:fundamentals/host/generic-host#environmentname> i <xref:security/app-secrets#environment-variables>.
* Secret Manager (tylko w środowisku programistycznym). Zobacz: <xref:security/app-secrets>.

## <a name="environment-based-startup-class-and-methods"></a>Klasy i metody uruchamiania oparte na środowisku

### <a name="inject-iwebhostenvironment-into-startupconfigure"></a>Wsuń IWebHostEnvironment do uruchamiania. Skonfiguruj

Wsuń <xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment> do `Startup.Configure`. Takie podejście jest przydatne, gdy aplikacja wymaga tylko dostosowania `Startup.Configure` w kilku środowiskach z minimalnymi różnicami kodu na środowisko.

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

### <a name="inject-iwebhostenvironment-into-the-startup-class"></a>Wsuń IWebHostEnvironment do klasy startowej

Wsuń <xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment> do konstruktora `Startup`. Takie podejście jest przydatne, gdy aplikacja wymaga skonfigurowania `Startup` dla kilku środowisk z minimalnymi różnicami kodu na środowisko.

W poniższym przykładzie:

* Środowisko jest przechowywane w polu `_env`.
* `_env` jest używany w `ConfigureServices` i `Configure` do zastosowania konfiguracji uruchamiania na podstawie środowiska aplikacji.

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
### <a name="startup-class-conventions"></a>Konwencje klasy uruchamiania

Po uruchomieniu aplikacji ASP.NET Core, [Klasa startowa](xref:fundamentals/startup) uruchamia aplikację. Aplikacja może definiować oddzielne klasy `Startup` dla różnych środowisk (na przykład `StartupDevelopment`). Odpowiednia Klasa `Startup` jest wybierana w czasie wykonywania. Kategoria, której sufiks nazwy jest zgodny z bieżącym środowiskiem, ma priorytet. Jeśli nie odnaleziono zgodnej klasy `Startup{EnvironmentName}`, używana jest Klasa `Startup`. Takie podejście jest przydatne, gdy aplikacja wymaga skonfigurowania uruchamiania dla kilku środowisk z wieloma różnicami kodu na środowisko.

Aby zaimplementować klasy `Startup` oparte na środowisku, należy utworzyć klasę `Startup{EnvironmentName}` dla każdego używanego środowiska i klasy rezerwowej `Startup`:

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

[!INCLUDE[about the series](~/includes/code-comments-loc.md)]

Użyj przeciążenia [UseStartup (IWebHostBuilder, String)](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usestartup) , które akceptuje nazwę zestawu:

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

### <a name="startup-method-conventions"></a>Konwencje metody uruchamiania

[Skonfiguruj](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) i [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) wersje `Configure<EnvironmentName>` i `Configure<EnvironmentName>Services`dla poszczególnych środowisk. Takie podejście jest przydatne, gdy aplikacja wymaga skonfigurowania uruchamiania dla kilku środowisk z wieloma różnicami kodu na środowisko.

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet_all&highlight=15,42)]

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:fundamentals/startup>
* <xref:fundamentals/configuration/index>
::: moniker-end

::: moniker range="< aspnetcore-3.0"

Autor: [Rick Anderson](https://twitter.com/RickAndMSFT)

ASP.NET Core konfiguruje zachowanie aplikacji na podstawie środowiska uruchomieniowego przy użyciu zmiennej środowiskowej.

[Wyświetl lub pobierz przykładowy kod](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([jak pobrać](xref:index#how-to-download-a-sample))

## <a name="environments"></a>Środowiska

ASP.NET Core odczytuje zmienną środowiskową `ASPNETCORE_ENVIRONMENT` podczas uruchamiania aplikacji i zapisuje wartość w [IHostingEnvironment. EnvironmentName](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName). `ASPNETCORE_ENVIRONMENT` można ustawić na dowolną wartość, ale w strukturze są dostarczane trzy wartości:

* <xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Development>
* <xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Staging>
* <xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Production> (wartość domyślna)

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet)]

Powyższy kod:

* Wywołuje [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) , gdy `ASPNETCORE_ENVIRONMENT` jest ustawiony na `Development`.
* Wywołuje [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler) , gdy wartość `ASPNETCORE_ENVIRONMENT` jest ustawiona na jedną z następujących wartości:

  * `Staging`
  * `Production`
  * `Staging_2`

[Pomocnik tagu środowiska](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) używa wartości `IHostingEnvironment.EnvironmentName` do dołączania lub wykluczania znaczników w elemencie:

[!code-cshtml[](environments/sample-snapshot/EnvironmentsSample/Pages/About.cshtml)]

W systemach Windows i macOS, zmienne środowiskowe i wartości nie uwzględniają wielkości liter. Zmienne środowiskowe systemu Linux i wartości domyślnie **uwzględniają wielkość** liter.

### <a name="development"></a>Opracowywanie zawartości

Środowisko programistyczne może włączyć funkcje, które nie powinny być ujawnione w środowisku produkcyjnym. Na przykład szablony ASP.NET Core umożliwiają [stronie wyjątków dla deweloperów](xref:fundamentals/error-handling#developer-exception-page) w środowisku deweloperskim.

Środowisko do tworzenia maszyn lokalnych można ustawić w pliku *Properties\launchSettings.JSON* projektu. Wartości środowiskowe ustawione w wartościach przesłonięcia *profilu launchsettings. JSON* ustawione w środowisku systemowym.

Poniższy kod JSON przedstawia trzy profile z pliku *profilu launchsettings. JSON* :

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
> Właściwość `applicationUrl` w pliku *profilu launchsettings. JSON* może określać listę adresów URL serwera. Użyj średnika między adresami URL na liście:
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

Gdy aplikacja jest [uruchamiana z uruchomieniem dotnet](/dotnet/core/tools/dotnet-run), zostanie użyty pierwszy profil z `"commandName": "Project"`. Wartość `commandName` Określa serwer sieci Web do uruchomienia. `commandName` może być jedną z następujących:

* `IISExpress`
* `IIS`
* `Project` (co spowoduje uruchomienie Kestrel)

Gdy aplikacja jest uruchamiana z [uruchomieniem dotnet](/dotnet/core/tools/dotnet-run):

* plik *profilu launchsettings. JSON* jest odczytywany, jeśli jest dostępny. `environmentVariables` ustawienia w zmiennych środowiskowych *profilu launchsettings. JSON* .
* Zostanie wyświetlone środowisko hostingu.

Następujące dane wyjściowe pokazują aplikację uruchomioną z [uruchomieniem dotnet](/dotnet/core/tools/dotnet-run):

```bash
PS C:\Websites\EnvironmentsSample> dotnet run
Using launch settings from C:\Websites\EnvironmentsSample\Properties\launchSettings.json...
Hosting environment: Staging
Content root path: C:\Websites\EnvironmentsSample
Now listening on: http://localhost:54340
Application started. Press Ctrl+C to shut down.
```

Karta **debugowanie** właściwości projektu programu Visual Studio udostępnia graficzny interfejs użytkownika służący do edytowania pliku *profilu launchsettings. JSON* :

![Ustawienia właściwości projektu zmienne środowiskowe](environments/_static/project-properties-debug.png)

Zmiany wprowadzone w profilach projektu mogą zacząć obowiązywać dopiero po ponownym uruchomieniu serwera sieci Web. Kestrel musi zostać uruchomiona ponownie, zanim będzie mógł wykryć zmiany wprowadzone w środowisku.

> [!WARNING]
> plik *profilu launchsettings. JSON* nie powinien przechowywać wpisów tajnych. [Narzędzia do zarządzania kluczami tajnymi](xref:security/app-secrets) można używać do przechowywania wpisów tajnych na potrzeby lokalnego tworzenia oprogramowania.

W przypadku korzystania z [Visual Studio Code](https://code.visualstudio.com/)zmienne środowiskowe można ustawić w pliku *. programu vscode/Launch. JSON* . Poniższy przykład ustawia środowisko na `Development`:

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

Plik *. programu vscode/Launch. JSON* w projekcie nie jest odczytywany podczas uruchamiania aplikacji przy użyciu `dotnet run` w taki sam sposób, jak *właściwości/profilu launchsettings. JSON*. Podczas uruchamiania aplikacji w środowisku deweloperskim, która nie ma pliku *profilu launchsettings. JSON* , należy ustawić środowisko przy użyciu zmiennej środowiskowej lub argumentu wiersza polecenia do polecenia `dotnet run`.

### <a name="production"></a>Produkcja

Środowisko produkcyjne powinno być skonfigurowane w celu zmaksymalizowania zabezpieczeń, wydajności i niezawodności aplikacji. Niektóre typowe ustawienia, które różnią się od programowania, to m.in.:

* Pamięć.
* Zasoby po stronie klienta są powiązane, zminimalizowanego i mogą być obsługiwane przez sieć CDN.
* Wyłączono strony błędów diagnostyki.
* Włączono przyjazne strony błędów.
* Włączono rejestrowanie i monitorowanie produkcji. Na przykład [Application Insights](/azure/application-insights/app-insights-asp-net-core).

## <a name="set-the-environment"></a>Ustawianie środowiska

Często warto ustawić określone środowisko do testowania przy użyciu zmiennej środowiskowej lub ustawienia platformy. Jeśli środowisko nie jest ustawione, domyślnie `Production`, co spowoduje wyłączenie większości funkcji debugowania. Metoda ustawiania środowiska zależy od systemu operacyjnego.

Po skompilowaniu hosta ostatnie ustawienie środowiska odczytywane przez aplikację określa środowisko aplikacji. Nie można zmienić środowiska aplikacji, gdy aplikacja jest uruchomiona.

### <a name="environment-variable-or-platform-setting"></a>Zmienna środowiskowa lub ustawienie platformy

#### <a name="azure-app-service"></a>Azure App Service

Aby ustawić środowisko w [Azure App Service](https://azure.microsoft.com/services/app-service/), wykonaj następujące czynności:

1. Wybierz aplikację z bloku **App Services** .
1. W grupie **Ustawienia** wybierz blok **Konfiguracja** .
1. Na karcie **Ustawienia aplikacji** wybierz pozycję **nowe ustawienie aplikacji**.
1. W oknie **Dodawanie/Edytowanie ustawienia aplikacji** Podaj `ASPNETCORE_ENVIRONMENT` dla **nazwy**. W polu **wartość**Podaj środowisko (na przykład `Staging`).
1. Zaznacz pole wyboru **ustawienie miejsca wdrożenia** , jeśli chcesz, aby ustawienie środowiska pozostawało w bieżącym gnieździe w przypadku wymiany miejsc wdrożenia. Aby uzyskać więcej informacji, zobacz [Konfigurowanie środowisk przejściowych w Azure App Service](/azure/app-service/web-sites-staged-publishing) w dokumentacji platformy Azure.
1. Wybierz **przycisk OK** , aby zamknąć okno **Dodawanie/Edytowanie ustawienia aplikacji** .
1. Wybierz pozycję **Zapisz** w górnej części bloku **Konfiguracja** .

Azure App Service automatycznie uruchamia ponownie aplikację po dodaniu, zmianie lub usunięciu ustawienia aplikacji (zmienna środowiskowa) w Azure Portal.

#### <a name="windows"></a>System Windows

Aby ustawić `ASPNETCORE_ENVIRONMENT` bieżącej sesji, gdy aplikacja zostanie uruchomiona przy użyciu [uruchomienia dotnet](/dotnet/core/tools/dotnet-run), są używane następujące polecenia:

**Wiersz polecenia**

```console
set ASPNETCORE_ENVIRONMENT=Development
```

**PowerShell**

```powershell
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

Te polecenia zaczną obowiązywać tylko dla bieżącego okna. Po zamknięciu okna ustawienie `ASPNETCORE_ENVIRONMENT` przywraca ustawienie domyślne lub wartość maszyny.

Aby ustawić wartość globalnie w systemie Windows, użyj jednej z następujących metod:

* Otwórz **Panel sterowania** > **system** > **Zaawansowane ustawienia systemowe** i Dodaj lub Edytuj wartość `ASPNETCORE_ENVIRONMENT`:

  ![Zaawansowane właściwości systemu](environments/_static/systemsetting_environment.png)

  ![Zmienna środowiskowa ASPNET Core](environments/_static/windows_aspnetcore_environment.png)

* Otwórz wiersz polecenia z uprawnieniami administracyjnymi i użyj polecenia `setx` lub Otwórz wiersz polecenia administracyjnych programu PowerShell i użyj `[Environment]::SetEnvironmentVariable`:

  **Wiersz polecenia**

  ```console
  setx ASPNETCORE_ENVIRONMENT Development /M
  ```

  Przełącznik `/M` wskazuje, aby ustawić zmienną środowiskową na poziomie systemu. Jeśli przełącznik `/M` nie jest używany, zmienna środowiskowa jest ustawiana dla konta użytkownika.

  **PowerShell**

  ```powershell
  [Environment]::SetEnvironmentVariable("ASPNETCORE_ENVIRONMENT", "Development", "Machine")
  ```

  Opcja `Machine` wartość wskazuje, aby ustawić zmienną środowiskową na poziomie systemu. Jeśli wartość opcji zostanie zmieniona na `User`, zmienna środowiskowa jest ustawiana dla konta użytkownika.

Gdy zmienna środowiskowa `ASPNETCORE_ENVIRONMENT` jest ustawiona globalnie, zacznie obowiązywać `dotnet run` w każdym oknie polecenia otwartym po ustawieniu wartości.

**plik Web. config**

Aby ustawić zmienną środowiskową `ASPNETCORE_ENVIRONMENT` za pomocą *pliku Web. config*, zobacz sekcję *Ustawianie zmiennych środowiskowych* w programie <xref:host-and-deploy/aspnet-core-module#setting-environment-variables>.

**Plik projektu lub profil publikacji**

**W przypadku wdrożeń usług Windows IIS:** Uwzględnij Właściwość `<EnvironmentName>` w [profilu publikowania (pubxml)](xref:host-and-deploy/visual-studio-publish-profiles) lub pliku projektu. To podejście ustawia środowisko w *pliku Web. config* po opublikowaniu projektu:

```xml
<PropertyGroup>
  <EnvironmentName>Development</EnvironmentName>
</PropertyGroup>
```

**Na pulę aplikacji IIS**

Aby ustawić zmienną środowiskową `ASPNETCORE_ENVIRONMENT` dla aplikacji uruchomionej w izolowanej puli aplikacji (obsługiwanej przez usługi IIS 10,0 lub nowszej), zobacz sekcję *polecenie Appcmd. exe* w temacie [zmienne środowiskowe &lt;environmentVariables&gt;](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) temat. Gdy zmienna środowiskowa `ASPNETCORE_ENVIRONMENT` jest ustawiona dla puli aplikacji, jej wartość zastępuje ustawienie na poziomie systemu.

> [!IMPORTANT]
> Podczas hostowania aplikacji w usługach IIS i dodawania lub zmieniania zmiennej środowiskowej `ASPNETCORE_ENVIRONMENT` należy użyć jednego z poniższych metod, aby nowa wartość była pobierana przez aplikacje:
>
> * Wykonaj `net stop was /y` a następnie `net start w3svc` z wiersza polecenia.
> * Uruchom ponownie serwer.

#### <a name="macos"></a>macOS

Ustawienie bieżącego środowiska dla macOS można wykonać w trybie online podczas uruchamiania aplikacji:

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```

Alternatywnie można ustawić środowisko z `export` przed uruchomieniem aplikacji:

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

Zmienne środowiskowe na poziomie komputera są ustawiane w pliku *. bashrc* lub *. bash_profile* . Edytuj plik przy użyciu dowolnego edytora tekstu. Dodaj następującą instrukcję:

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

#### <a name="linux"></a>Linux

W przypadku systemu Linux dystrybucje Użyj polecenia `export` w wierszu polecenia dla ustawień zmiennych opartych na sesji i pliku *bash_profile* dla ustawień środowiska na poziomie komputera.

### <a name="set-the-environment-in-code"></a>Ustawianie środowiska w kodzie

Wywołaj <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseEnvironment*> podczas kompilowania hosta. Zobacz: <xref:fundamentals/host/web-host#environment>.

### <a name="configuration-by-environment"></a>Konfiguracja według środowiska

Aby załadować konfigurację według środowiska, zalecamy:

* pliki *AppSettings* (*appSettings. { Environment}. JSON*). Zobacz: <xref:fundamentals/configuration/index#json-configuration-provider>.
* Zmienne środowiskowe (ustawiane dla każdego systemu, w którym jest hostowana aplikacja). Zobacz <xref:fundamentals/host/web-host#environment> i <xref:security/app-secrets#environment-variables>.
* Secret Manager (tylko w środowisku programistycznym). Zobacz: <xref:security/app-secrets>.

## <a name="environment-based-startup-class-and-methods"></a>Klasy i metody uruchamiania oparte na środowisku

### <a name="inject-ihostingenvironment-into-startupconfigure"></a>Wsuń IHostingEnvironment do uruchamiania. Skonfiguruj

Wsuń <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> do `Startup.Configure`. Takie podejście jest przydatne, gdy aplikacja wymaga tylko konfigurowania `Startup.Configure` tylko dla kilku środowisk z minimalnymi różnicami w kodzie dla danego środowiska.

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

### <a name="inject-ihostingenvironment-into-the-startup-class"></a>Wsuń IHostingEnvironment do klasy startowej

Wsuń <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> do konstruktora `Startup` i przypisz usługę do pola do użycia w całej klasie `Startup`. Takie podejście jest przydatne, gdy aplikacja wymaga konfiguracji uruchamiania tylko dla kilku środowisk z minimalnymi różnicami w kodzie dla danego środowiska.

W poniższym przykładzie:

* Środowisko jest przechowywane w polu `_env`.
* `_env` jest używany w `ConfigureServices` i `Configure` do zastosowania konfiguracji uruchamiania na podstawie środowiska aplikacji.

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

### <a name="startup-class-conventions"></a>Konwencje klasy uruchamiania

Po uruchomieniu aplikacji ASP.NET Core, [Klasa startowa](xref:fundamentals/startup) uruchamia aplikację. Aplikacja może definiować oddzielne klasy `Startup` dla różnych środowisk (na przykład `StartupDevelopment`). Odpowiednia Klasa `Startup` jest wybierana w czasie wykonywania. Kategoria, której sufiks nazwy jest zgodny z bieżącym środowiskiem, ma priorytet. Jeśli nie odnaleziono zgodnej klasy `Startup{EnvironmentName}`, używana jest Klasa `Startup`. Takie podejście jest przydatne, gdy aplikacja wymaga skonfigurowania uruchamiania dla kilku środowisk z wieloma różnicami kodu na środowisko.

Aby zaimplementować klasy `Startup` oparte na środowisku, należy utworzyć klasę `Startup{EnvironmentName}` dla każdego używanego środowiska i klasy rezerwowej `Startup`:

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

Użyj przeciążenia [UseStartup (IWebHostBuilder, String)](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usestartup) , które akceptuje nazwę zestawu:

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

### <a name="startup-method-conventions"></a>Konwencje metody uruchamiania

[Skonfiguruj](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) i [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) wersje `Configure<EnvironmentName>` i `Configure<EnvironmentName>Services`dla poszczególnych środowisk. Takie podejście jest przydatne, gdy aplikacja wymaga skonfigurowania uruchamiania dla kilku środowisk z wieloma różnicami kodu na środowisko.

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet_all&highlight=15,42)]

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:fundamentals/startup>
* <xref:fundamentals/configuration/index>

::: moniker-end
