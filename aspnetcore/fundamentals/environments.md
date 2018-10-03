---
title: Używanie wielu środowisk w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak kontrolować zachowanie aplikacji w wielu środowiskach, w aplikacji platformy ASP.NET Core.
ms.author: riande
ms.date: 07/03/2018
uid: fundamentals/environments
ms.openlocfilehash: 0786b55ee133b98aae1bfac688e7d81d23c8c9a4
ms.sourcegitcommit: 13940eb53c68664b11a2d685ee17c78faab1945d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/01/2018
ms.locfileid: "47860982"
---
# <a name="use-multiple-environments-in-aspnet-core"></a>Używanie wielu środowisk w programie ASP.NET Core

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

Platforma ASP.NET Core konfiguruje zachowanie aplikacji, w zależności od środowiska czasu wykonywania przy użyciu zmiennej środowiskowej.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

## <a name="environments"></a>Środowiska

Platforma ASP.NET Core odczytuje zmiennej środowiskowej `ASPNETCORE_ENVIRONMENT` przy uruchamianiu aplikacji i przechowuje wartość w [IHostingEnvironment.EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname). Możesz ustawić `ASPNETCORE_ENVIRONMENT` dowolną wartość, ale [trzech wartości](/dotnet/api/microsoft.aspnetcore.hosting.environmentname) są obsługiwane przez platformę: [rozwoju](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development), [przemieszczania](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging), i [wśrodowiskuprodukcyjnym](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production). Jeśli `ASPNETCORE_ENVIRONMENT` nie jest ustawiony, jego wartość domyślna to `Production`.

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet)]

Powyższy kod:

* Wywołania [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) podczas `ASPNETCORE_ENVIRONMENT` ustawiono `Development`.
* Wywołania [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler) podczas wartość `ASPNETCORE_ENVIRONMENT` ustawiono jeden z następujących czynności:

    * `Staging`
    * `Production`
    * `Staging_2`

[Pomocnik tagu środowiska](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) używa wartości `IHostingEnvironment.EnvironmentName` do dołączania lub wykluczania znaczników w elemencie:

[!code-cshtml[](environments/sample-snapshot/EnvironmentsSample/Pages/About.cshtml)]

W systemie Windows i macOS wartości i zmienne środowiskowe nie są z uwzględnieniem wielkości liter. Zmienne środowiskowe systemu Linux i wartości są **z uwzględnieniem wielkości liter** domyślnie.

### <a name="development"></a>Tworzenie

Środowisko projektowe można włączyć funkcje, które nie powinny być dostępne w środowisku produkcyjnym. Na przykład włączyć szablony ASP.NET Core [stronie wyjątków deweloperów](xref:fundamentals/error-handling#the-developer-exception-page) w środowisku programistycznym.

Środowisko do tworzenia aplikacji z komputera lokalnego można ustawić w *Properties\launchSettings.json* pliku projektu. Wartości środowiska ustawione w *launchSettings.json* zastąpienie wartości ustawionych w środowisku systemowym.

Następujący kod JSON zawiera trzy profile z *launchSettings.json* pliku:

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
> `applicationUrl` Właściwość *launchSettings.json* można określić listę adresów URL serwera. Użyj średnika między adresami URL na liście:
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

Gdy aplikacja jest uruchamiana z [dotnet, uruchom](/dotnet/core/tools/dotnet-run), pierwszy profil z `"commandName": "Project"` jest używany. Wartość `commandName` Określa serwer sieci web, aby uruchomić. `commandName` może być jednym z następujących czynności:

* IIS Express
* IIS
* Projekt (który spowoduje uruchomienie Kestrel)

Gdy aplikacja jest uruchamiana z [dotnet, uruchom](/dotnet/core/tools/dotnet-run):

* *launchSettings.json* jest do odczytu. Jeśli jest dostępny. `environmentVariables` ustawienia w *launchSettings.json* zastępują zmienne środowiskowe.
* Środowisko hostingu jest wyświetlany.

Następujące dane wyjściowe zawierają aplikację wprowadzenie [dotnet, uruchom](/dotnet/core/tools/dotnet-run):

```bash
PS C:\Websites\EnvironmentsSample> dotnet run
Using launch settings from C:\Websites\EnvironmentsSample\Properties\launchSettings.json...
Hosting environment: Staging
Content root path: C:\Websites\EnvironmentsSample
Now listening on: http://localhost:54340
Application started. Press Ctrl+C to shut down.
```

Właściwości projektu programu Visual Studio **debugowania** karta zawiera graficzny interfejs użytkownika do edycji *launchSettings.json* pliku:

![Zmienne środowiskowe ustawienie właściwości projektu](environments/_static/project-properties-debug.png)

Zmiany wprowadzone do profilów projektu nie mogą zostać wprowadzone do czasu ponownego uruchomienia serwera sieci web. Kestrel musi być dopiero po ponownym uruchomieniu potrafi wykrywać zmiany wprowadzone w swoim środowisku.

> [!WARNING]
> *launchSettings.json* nie należy przechowywać wpisy tajne. [Narzędzie Menedżer klucz tajny](xref:security/app-secrets) może służyć do przechowywania kluczy tajnych na potrzeby lokalnego programowania.

Korzystając z [programu Visual Studio Code](https://code.visualstudio.com/), zmienne środowiskowe można ustawić w *.vscode/launch.json* pliku. Poniższy przykład ustawia środowisko `Development`:

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

A *.vscode/launch.json* plik w projekcie nie jest do odczytu podczas uruchamiania aplikacji przy użyciu `dotnet run` w taki sam sposób jak *Properties/launchSettings.json*. Podczas uruchamiania aplikacji w trakcie programowania, który nie ma *launchSettings.json* pliku, należy ustawić środowisko przy użyciu zmiennej środowiskowej lub argument wiersza polecenia, aby `dotnet run` polecenia.

### <a name="production"></a>Produkcji

W środowisku produkcyjnym należy skonfigurować tak, aby zwiększyć poziom bezpieczeństwa, wydajności i niezawodności aplikacji. Niektóre typowe ustawienia, które różnią się od etapu programowania obejmują:

* Pamięć podręczna.
* Zasoby po stronie klienta są powiązane, zminimalizowany i potencjalnie udostępnianej przez usługę CDN.
* Błąd diagnostyki strony wyłączone.
* Strony błędów przyjazna, włączone.
* Rejestrowanie produkcji i monitorowanie jest włączone. Na przykład [usługi Application Insights](/azure/application-insights/app-insights-asp-net-core).

## <a name="set-the-environment"></a>Ustaw środowisko

Często jest to przydatne do zestawu określonego środowiska na potrzeby testowania. Jeśli środowisko nie jest ustawiona, wartość domyślna to `Production`, która wyłącza większości funkcji debugowania. Metoda do ustawiania środowiska zależy od systemu operacyjnego.

### <a name="azure-app-service"></a>Usługa Azure App Service

Aby ustawić środowiska w [usługi Azure App Service](https://azure.microsoft.com/services/app-service/), wykonaj następujące czynności:

1. Wybierz aplikację z **App Services** bloku.
1. W **ustawienia** grupy, wybierz opcję **ustawienia aplikacji** bloku.
1. W **ustawienia aplikacji** wybierz opcję **Dodaj nowe ustawienie**.
1. Aby uzyskać **wprowadź nazwę**, podaj `ASPNETCORE_ENVIRONMENT`. Aby uzyskać **wprowadź wartość**, zapewniają środowiska (na przykład `Staging`).
1. Wybierz **ustawienie miejsca** pole wyboru, jeśli chcesz, aby ustawienie środowiska, aby pozostać z bieżącym gnieździe, gdy zostały zamienione miejscami wdrożenia. Aby uzyskać więcej informacji, zobacz [dokumentacji platformy Azure: ustawienia, które zostały zamienione?](/azure/app-service/web-sites-staged-publishing).
1. Wybierz **Zapisz** w górnej części bloku.

Usługa Azure App Service zostanie automatycznie uruchomiony ponownie aplikację po ustawienia aplikacji (zmienną środowiskową) jest dodanie, zmianę lub usunąć w witrynie Azure portal.

### <a name="windows"></a>Windows

Aby ustawić `ASPNETCORE_ENVIRONMENT` dla bieżącej sesji, podczas uruchamiania aplikacji przy użyciu [dotnet, uruchom](/dotnet/core/tools/dotnet-run), służą następujące polecenia:

**Wiersz polecenia**

```console
set ASPNETCORE_ENVIRONMENT=Development
```

**Program PowerShell**

```powershell
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

Te polecenia tylko obowiązywać w przypadku bieżącego okna. Po zamknięciu okna `ASPNETCORE_ENVIRONMENT` przywraca ustawienie domyślne ustawienie lub wartość maszyny.

Aby ustawić wartość globalnie w Windows, użyj jednej z następujących metod:

* Otwórz **Panelu sterowania** > **systemu** > **Zaawansowane ustawienia systemu** i Dodaj lub Edytuj `ASPNETCORE_ENVIRONMENT` wartość:

  ![Zaawansowane właściwości systemu](environments/_static/systemsetting_environment.png)

  ![Zmienna środowiskowa Core ASPNET](environments/_static/windows_aspnetcore_environment.png)

* Otwórz administracyjny wiersz polecenia i użyj `setx` polecenia lub Otwórz administracyjny wiersz polecenia programu PowerShell i użyj `[Environment]::SetEnvironmentVariable`:

  **Wiersz polecenia**

  ```console
  setx ASPNETCORE_ENVIRONMENT Development /M
  ```

  `/M` Przełącznika wskazuje, aby ustawić zmienną środowiskową na poziomie systemu. Jeśli `/M` przełącznik nie jest używany, zmienna środowiskowa jest ustawiona dla konta użytkownika.

  **Program PowerShell**

  ```powershell
  [Environment]::SetEnvironmentVariable("ASPNETCORE_ENVIRONMENT", "Development", "Machine")
  ```

  `Machine` Wskazuje wartość opcji, aby ustawić zmienną środowiskową na poziomie systemu. Jeżeli wartość opcji została zmieniona na `User`, zmienna środowiskowa jest ustawiona dla konta użytkownika.

Gdy `ASPNETCORE_ENVIRONMENT` zmienna środowiskowa jest ustawiona globalnie, wprowadzone `dotnet run` w dowolnym oknie polecenia otwarte po ustawieniu wartości.

**web.config**

Aby ustawić `ASPNETCORE_ENVIRONMENT` zmiennej środowiskowej *web.config*, zobacz *Ustawianie zmiennych środowiskowych* części <xref:host-and-deploy/aspnet-core-module#setting-environment-variables>. Gdy `ASPNETCORE_ENVIRONMENT` zmienna środowiskowa została ustawiona za pomocą *web.config*, jego wartość zastępuje ustawienie poziomie systemu.

**Na pulę aplikacji usług IIS**

Aby ustawić `ASPNETCORE_ENVIRONMENT` zmienną środowiskową dla aplikacji uruchomionej w izolowanej puli aplikacji (obsługiwane w usługach IIS 10.0 lub nowszy), zobacz *polecenia AppCmd.exe* części [zmienne środowiskowe &lt; environmentVariables&gt; ](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) tematu. Gdy `ASPNETCORE_ENVIRONMENT` zmienna środowiskowa jest ustawiona dla puli aplikacji, jego wartość zastępuje ustawienie poziomie systemu.

> [!IMPORTANT]
> Hostowanie aplikacji w usługach IIS i dodawania lub zmieniania `ASPNETCORE_ENVIRONMENT` środowiska, użycie zmiennej zbliża się jedną z następujących ma nowej wartości, które są pobierane przez aplikacje:
>
> * Ponowne uruchomienie puli aplikacji w aplikacji.
> * Wykonaj `net stop was /y` następuje `net start w3svc` z poziomu wiersza polecenia.
> * Uruchom ponownie serwer.

### <a name="macos"></a>macOS

Ustawienie bieżącego środowiska, dla systemu macOS może być wykonywane w tekście, podczas uruchamiania aplikacji:

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```

Alternatywnie należy ustawić środowisko z `export` przed uruchomieniem aplikacji:

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

Zmienne środowiskowe na poziomie komputera są ustawiane w *.bashrc* lub *.bash_profile* pliku. Edytuj plik za pomocą dowolnego edytora tekstów. Dodaj następującą instrukcję:

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

### <a name="linux"></a>Linux

Dystrybucje systemu Linux, można użyć `export` polecenie w wierszu polecenia dla ustawień zmiennych oparte na sesji i *bash_profile* ustawienia maszyn na poziomie środowiska w pliku.

### <a name="configuration-by-environment"></a>Konfiguracja według środowiska

Aby załadować konfiguracji przez środowisko, zalecamy:

* *appSettings* pliki (* appsettings.&lt; <Environment> &gt;JSON). Zobacz [konfiguracji: Dostawca konfiguracji pliku](xref:fundamentals/configuration/index#file-configuration-provider).
* zmienne środowiskowe (ustawiona w każdym systemie którym hostowana jest aplikacja). Zobacz [konfiguracji: Dostawca konfiguracji pliku](xref:fundamentals/configuration/index#file-configuration-provider) i [bezpieczne przechowywanie kluczy tajnych aplikacji w trakcie opracowywania: zmienne środowiskowe](xref:security/app-secrets#environment-variables).
* Klucz tajny Menedżer (w środowisku programistycznym tylko). Zobacz <xref:security/app-secrets>.

## <a name="environment-based-startup-class-and-methods"></a>Klasa początkowa oparte na środowisku i metody

### <a name="startup-class-conventions"></a>Konwencje klasy uruchamiania

Po uruchomieniu aplikacji ASP.NET Core [Klasa początkowa](xref:fundamentals/startup) używa do ładowania aplikacji. Aplikację można zdefiniować w oddzielnym `Startup` klas w różnych środowiskach (na przykład `StartupDevelopment`), a odpowiedni `Startup` klasy jest zaznaczona w czasie wykonywania. Klasy, w których sufiks nazwy pasuje do bieżącego środowiska jest podzielony na priorytety. Jeśli pasujący obiekt typu `Startup{EnvironmentName}` klasy nie zostanie znaleziona, `Startup` klasa jest używana.

Aby zaimplementować oparte na środowisku `Startup` Tworzenie klas, `Startup{EnvironmentName}` klasy dla każdego środowiska w użyciu i rezerwowe `Startup` klasy:

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

Użyj [UseStartup (IWebHostBuilder, ciąg)](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usestartup) przeciążenie, które akceptuje to nazwa zestawu:

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

### <a name="startup-method-conventions"></a>Konwencje metod uruchamiania

[Konfigurowanie](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) i [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) obsługuje specyficznymi dla środowiska wersji formularza `Configure<EnvironmentName>` i `Configure<EnvironmentName>Services`:

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet_all&highlight=15,51)]

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:fundamentals/startup>
* <xref:fundamentals/configuration/index>
* [IHostingEnvironment.EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname)
