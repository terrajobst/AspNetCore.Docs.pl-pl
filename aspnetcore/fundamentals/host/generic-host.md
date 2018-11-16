---
title: Ogólny hosta platformy .NET
author: guardrex
description: Więcej informacji na temat ogólnych hosta na platformie .NET, który jest odpowiedzialny za zarządzanie uruchamiania i czasu życia aplikacji.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/30/2018
uid: fundamentals/host/generic-host
ms.openlocfilehash: cac5ccdea7838d26b7468f9bf1ab8d317b444b46
ms.sourcegitcommit: 09bcda59a58019fdf47b2db5259fe87acf19dd38
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/15/2018
ms.locfileid: "51708520"
---
# <a name="net-generic-host"></a>Ogólny hosta platformy .NET

Przez [Luke Latham](https://github.com/guardrex)

Aplikacje platformy .NET core, konfigurowanie i uruchamianie *hosta*. Host jest odpowiedzialny za zarządzanie uruchamiania i czasu życia aplikacji. W tym temacie omówiono Host rodzajowego Core ASP.NET (<xref:Microsoft.Extensions.Hosting.HostBuilder>), co jest przydatne do hostowania aplikacji, które nie przetwarzają żądania HTTP. Pokrycia hosta sieci Web (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>), zobacz <xref:fundamentals/host/web-host>.

Celem ogólnego hosta jest rozdzielenie potoku HTTP z hosta internetowego interfejsu API, umożliwiające szersze gamę scenariuszy hosta. Komunikaty, zadania w tle i innych obciążeń innych niż HTTP oparte na korzyść ogólnego hosta z przekrojowe możliwości, takich jak konfiguracja, wstrzykiwanie zależności (DI) i rejestrowania.

Ogólny hosta jest nowa w programie ASP.NET Core 2.1 i nie jest odpowiednie w scenariuszach hostingu w sieci web. W przypadku scenariuszy hostingu w sieci web, użyj [hosta sieci Web](xref:fundamentals/host/web-host). Ogólny Host jest w fazie projektowania w celu zastąpienia hosta sieci Web w przyszłym wydaniu i pełnić rolę hosta podstawowego interfejsu API zarówno w przypadku protokołu HTTP, jak i scenariuszy innych niż HTTP.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([sposobu pobierania](xref:index#how-to-download-a-sample))

Podczas uruchamiania przykładowej aplikacji [programu Visual Studio Code](https://code.visualstudio.com/), użyj *zewnętrznych lub w zintegrowanym terminalu*. Nie, uruchom przykład w `internalConsole`.

Aby ustawić konsoli w programie Visual Studio Code:

1. Otwórz *.vscode/launch.json* pliku.
1. W **.NET Core uruchamianie (Konsola)** konfiguracji, zlokalizuj **konsoli** wpisu. Ustaw wartość na wartość `externalTerminal` lub `integratedTerminal`.

## <a name="introduction"></a>Wprowadzenie

Biblioteka ogólnego hosta jest dostępna w <xref:Microsoft.Extensions.Hosting> przestrzeni nazw i udostępnionych przez [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/) pakietu. `Microsoft.Extensions.Hosting` Pakietu znajduje się w [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) (platformy ASP.NET Core 2.1 lub nowszej).

<xref:Microsoft.Extensions.Hosting.IHostedService> jest punktem wejścia do wykonania kodu. Każdy `IHostedService` implementacja jest wykonywany w kolejności od [usługi rejestracji w ConfigureServices](#configureservices). <xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*> jest wywoływana w każdej `IHostedService` podczas uruchamiania hosta, a <xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*> jest wywoływana w kolejności odwrotnej rejestracji, gdy kończy pracę bez problemu zmieniała hosta.

## <a name="set-up-a-host"></a>Konfigurowanie hosta

<xref:Microsoft.Extensions.Hosting.IHostBuilder> jest to główny składnik, używanego przez aplikacje i biblioteki do zainicjowania, tworzeniu i uruchamianiu hosta:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_HostBuilder)]

## <a name="default-services"></a>Usług domyślnych

Następujące usługi są rejestrowane podczas inicjowania hosta:

* [Środowisko](xref:fundamentals/environments) (<xref:Microsoft.Extensions.Hosting.IHostingEnvironment>)
* <xref:Microsoft.Extensions.Hosting.HostBuilderContext>
* [Konfiguracja](xref:fundamentals/configuration/index) (<xref:Microsoft.Extensions.Configuration.IConfiguration>)
* <xref:Microsoft.Extensions.Hosting.IApplicationLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ApplicationLifetime>)
* <xref:Microsoft.Extensions.Hosting.IHostLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime>)
* <xref:Microsoft.Extensions.Hosting.IHost>
* [Opcje](xref:fundamentals/configuration/options) (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.AddOptions*>)
* [Rejestrowanie](xref:fundamentals/logging/index) (<xref:Microsoft.Extensions.DependencyInjection.LoggingServiceCollectionExtensions.AddLogging*>)

## <a name="host-configuration"></a>Konfiguracja hosta

Konfiguracja hosta jest tworzony przez:

* Wywoływanie metody rozszerzenia na <xref:Microsoft.Extensions.Hosting.IHostBuilder> można ustawić [zawartości głównego](#content-root) i [środowiska](#environment).
* Odczytywanie konfiguracji od dostawców konfiguracji w <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>.

### <a name="extension-methods"></a>Metody rozszerzenia

### <a name="application-key-name"></a>Klucz aplikacji (nazwa)

[IHostingEnvironment.ApplicationName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ApplicationName*) właściwość ma wartość z konfiguracji hosta podczas konstruowania hosta. Aby jawnie ustawić wartość, użyj [HostDefaults.ApplicationKey](xref:Microsoft.Extensions.Hosting.HostDefaults.ApplicationKey):

**Klucz**: applicationName  
**Typ**: *ciągu*  
**Domyślne**: Nazwa zestawu zawierającego punkt wejścia aplikacji.  
**Można ustawić przy użyciu**: `HostBuilderContext.HostingEnvironment.ApplicationName`  
**Zmienna środowiskowa**: `<PREFIX_>APPLICATIONNAME` (`<PREFIX_>` jest [opcjonalne i zdefiniowane przez użytkownika](#configurehostconfiguration))

### <a name="content-root"></a>Zawartość katalogu głównego

To ustawienie określa, gdzie hosta rozpoczyna się wyszukiwanie plików zawartości.

**Klucz**: contentRoot  
**Typ**: *ciągu*  
**Domyślne**: wartość domyślna to folder, w którym znajduje się zestaw aplikacji.  
**Można ustawić przy użyciu**: `UseContentRoot`  
**Zmienna środowiskowa**: `<PREFIX_>CONTENTROOT` (`<PREFIX_>` jest [opcjonalne i zdefiniowane przez użytkownika](#configurehostconfiguration))

Jeśli ścieżka nie istnieje, host nie można uruchomić.

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseContentRoot)]

### <a name="environment"></a>Środowisko

Ustawia aplikacji [środowiska](xref:fundamentals/environments).

**Klucz**: środowisko  
**Typ**: *ciągu*  
**Domyślne**: produkcji  
**Można ustawić przy użyciu**: `UseEnvironment`  
**Zmienna środowiskowa**: `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` jest [opcjonalne i zdefiniowane przez użytkownika](#configurehostconfiguration))

Środowisko można ustawić dowolną wartość. Wartości zdefiniowane w ramach obejmują `Development`, `Staging`, i `Production`. Wartości nie są z uwzględnieniem wielkości liter.

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseEnvironment)]

### <a name="configurehostconfiguration"></a>ConfigureHostConfiguration

`ConfigureHostConfiguration` używa <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> utworzyć <xref:Microsoft.Extensions.Configuration.IConfiguration> dla hosta. Konfiguracja hosta jest wykorzystywany do inicjacji <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> do użycia w procesie kompilacji aplikacji.

`ConfigureHostConfiguration` można wywołać wiele razy z wynikami dodatku. Host używa jednego z tych opcji ustawia wartość ostatniego dla danego klucza.

Konfiguracja hosta są automatycznie przekazywane do konfiguracji aplikacji ([ConfigureAppConfiguration](#configureappconfiguration) i pozostałej części aplikacji).

Brak dostawców są domyślnie dołączone. Należy jawnie określić niezależnie od dostawcy konfiguracji aplikacji wymaga w `ConfigureHostConfiguration`, w tym:

* Plik konfiguracji (na przykład z *hostsettings.json* pliku).
* Zmienne konfiguracji środowiska.
* Konfiguracja argumentu wiersza polecenia.
* Innych dostawców wymaganej konfiguracji.

Plik konfiguracji hosta jest włączona, określając ścieżki podstawowej aplikacji za pomocą `SetBasePath` następuje wywołanie jednej z [pliku dostawcy konfiguracji](xref:fundamentals/configuration/index#file-configuration-provider). Przykładowa aplikacja używa pliku JSON, *hostsettings.json*i wywołuje <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> korzystanie z ustawień konfiguracji hosta tego pliku.

Aby dodać [zmiennych konfiguracji środowiska](xref:fundamentals/configuration/index#environment-variables-configuration-provider) hosta, należy wywołać <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> w Konstruktorze hosta. `AddEnvironmentVariables` akceptuje opcjonalny prefiks zdefiniowany przez użytkownika. Przykładowa aplikacja korzysta z prefiksem `PREFIX_`. Prefiks jest usuwany, gdy zmienne środowiskowe są odczytywane. Po skonfigurowaniu hostów przykładową aplikację, wartość zmiennej środowiskowej, aby uzyskać `PREFIX_ENVIRONMENT` staje się wartość konfiguracji hosta `environment` klucza.

Podczas programowania, korzystając z [programu Visual Studio](https://www.visualstudio.com/) lub uruchamianie aplikacji za pomocą `dotnet run`, zmienne środowiskowe, może być ustawiona w *Properties/launchSettings.json* pliku. W [programu Visual Studio Code](https://code.visualstudio.com/), zmienne środowiskowe, może być ustawiona w *.vscode/launch.json* pliku podczas programowania. Aby uzyskać więcej informacji, zobacz <xref:fundamentals/environments>.

[Wiersza polecenia konfiguracji](xref:fundamentals/configuration/index#command-line-configuration-provider) jest dodawany przez wywołanie <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>. Konfiguracja wiersza polecenia zostanie dodany ostatnio pozwalającą na argumenty wiersza polecenia, aby zastąpić konfiguracji udostępnianych przez starszych dostawców konfiguracji.

*hostsettings.JSON*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/hostsettings.json)]

Dodatkowa konfiguracja może być wyposażone w [applicationName](#application-key-name) i [contentRoot](#content-root) kluczy.

Przykład `HostBuilder` konfiguracji przy użyciu `ConfigureHostConfiguration`:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureHostConfiguration)]

## <a name="configureappconfiguration"></a>ConfigureAppConfiguration

Konfiguracja aplikacji jest tworzony przez wywołanie <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> na <xref:Microsoft.Extensions.Hosting.IHostBuilder> implementacji. `ConfigureAppConfiguration` używa <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> utworzyć <xref:Microsoft.Extensions.Configuration.IConfiguration> dla aplikacji. `ConfigureAppConfiguration` można wywołać wiele razy z wynikami dodatku. Aplikacja używa jednego z tych opcji ustawia wartość ostatniego dla danego klucza. Konfiguracji utworzonej przez `ConfigureAppConfiguration` znajduje się w temacie [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) dla kolejnych operacji i w <xref:Microsoft.Extensions.Hosting.IHost.Services*>.

Konfiguracja aplikacji automatycznie otrzymuje konfigurację hosta, dostarczone przez [ConfigureHostConfiguration](#configurehostconfiguration).

Przykład korzystającą configuration `ConfigureAppConfiguration`:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureAppConfiguration)]

*appSettings.JSON*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.json)]

*appSettings. Development.JSON*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Development.json)]

*appSettings. Production.JSON*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Production.json)]

Aby przenieść pliki ustawień do katalogu wyjściowego, określ pliki ustawień jako [elementów projektu programu MSBuild](/visualstudio/msbuild/common-msbuild-project-items) w pliku projektu. Przykładowa aplikacja przenosi jego pliki ustawień aplikacji w formacie JSON i *hostsettings.json* następującym `<Content>` elementu:

```xml
<ItemGroup>
  <Content Include="**\*.json" Exclude="bin\**\*;obj\**\*" CopyToOutputDirectory="PreserveNewest" />
</ItemGroup>
```

## <a name="configureservices"></a>ConfigureServices

<xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> dodaje usług do aplikacji [wstrzykiwanie zależności](xref:fundamentals/dependency-injection) kontenera. `ConfigureServices` można wywołać wiele razy z wynikami dodatku.

Usługa hostowana jest klasą z logiką zadań tła, który implementuje <xref:Microsoft.Extensions.Hosting.IHostedService> interfejsu. Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/hosted-services>.

[Przykładową aplikację](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) używa `AddHostedService` metodę rozszerzenia, aby dodać usługę do zdarzenia okresu istnienia `LifetimeEventsHostedService`i zadania w tle czasu `TimedHostedService`, do aplikacji:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureServices)]

## <a name="configurelogging"></a>ConfigureLogging

<xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> dodaje delegata do konfigurowania podane <xref:Microsoft.Extensions.Logging.ILoggingBuilder>. `ConfigureLogging` może zostać wywołana wiele razy z wynikami dodatku.

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureLogging)]

### <a name="useconsolelifetime"></a>UseConsoleLifetime

<xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> nasłuchuje `Ctrl+C`SIGTERM lub wywołań i /SIGINT <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> można uruchomić procesu zamykania. `UseConsoleLifetime` Odblokowuje rozszerzeń, takich jak [RunAsync](#runasync) i [WaitForShutdownAsync](#waitforshutdownasync). <xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime> wstępnie jest zarejestrowany jako domyślna implementacja okresu istnienia. Ostatniego okresu istnienia zarejestrowany jest używany.

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseConsoleLifetime)]

## <a name="container-configuration"></a>Konfiguracja kontenera

Aby zapewnić obsługę podłączania innych kontenerów, host może akceptować <xref:Microsoft.Extensions.DependencyInjection.IServiceProviderFactory`1>. Zapewnianie fabrykę nie jest częścią DI rejestracja kontenera jest jednak wewnętrzne hosta używany do tworzenia konkretnych kontenera DI. [UseServiceProviderFactory (IServiceProviderFactory&lt;TContainerBuilder&gt;)](xref:Microsoft.Extensions.Hosting.HostBuilder.UseServiceProviderFactory*) zastępuje domyślną fabrykę użyty do utworzenia dostawcy usługi app service.

Konfiguracja kontenera niestandardowego jest zarządzana przez <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> metody. `ConfigureContainer` udostępnia silnie typizowane środowisko na potrzeby konfigurowania kontenera na podstawie odpowiedniego hosta interfejsu API. `ConfigureContainer` można wywołać wiele razy z wynikami dodatku.

Tworzenie kontenera usług dla aplikacji:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainer.cs)]

Podaj fabryki kontenera usług:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainerFactory.cs)]

Używaj fabryki i konfigurowanie kontenera niestandardowego usługi dla aplikacji:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ContainerConfiguration)]

## <a name="extensibility"></a>Rozszerzalność

Rozszerzalność hosta odbywa się za pomocą metod rozszerzenia na `IHostBuilder`. W poniższym przykładzie pokazano, jak metoda rozszerzenia rozszerza `IHostBuilder` implementację [TimedHostedService](xref:fundamentals/host/hosted-services#timed-background-tasks) przykład przedstawiona w <xref:fundamentals/host/hosted-services>.

```csharp
var host = new HostBuilder()
    .UseHostedService<TimedHostedService>()
    .Build();

await host.StartAsync();
```

Aplikacja ustanawia `UseHostedService` metodę rozszerzenia, aby zarejestrować usługi hostowanej przekazanej `T`:

```csharp
using System;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Hosting;

public static class Extensions
{
    public static IHostBuilder UseHostedService<T>(this IHostBuilder hostBuilder)
        where T : class, IHostedService, IDisposable
    {
        return hostBuilder.ConfigureServices(services =>
            services.AddHostedService<T>());
    }
}
```

## <a name="manage-the-host"></a>Zarządzać hosta

<xref:Microsoft.Extensions.Hosting.IHost> Implementacja jest odpowiedzialna za uruchamianie i zatrzymywanie `IHostedService` implementacji, które są zarejestrowane w usłudze service container.

### <a name="run"></a>Uruchom

<xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> Uruchamia aplikację i blokuje wątek wywołujący, aż zamknie hosta:

```csharp
public class Program
{
    public void Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        host.Run();
    }
}
```

### <a name="runasync"></a>RunAsync

<xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> Uruchamia aplikację i zwraca `Task` który zostaje ukończony po wyzwoleniu token anulowania lub zamknięcia:

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        await host.RunAsync();
    }
}
```

### <a name="runconsoleasync"></a>RunConsoleAsync

<xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> Umożliwia obsługę konsoli, tworzy i uruchamia hosta i czeka, aż `Ctrl+C`/SIGINT lub SIGTERM, aby zamknąć.

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var hostBuilder = new HostBuilder();

        await hostBuilder.RunConsoleAsync();
    }
}
```

### <a name="start-and-stopasync"></a>Początkowy i StopAsync

<xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> Uruchamia hosta synchronicznie.

<xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> podejmuje próby zatrzymania hosta w ciągu podanego limitu czasu.

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        using (host)
        {
            host.Start();

            await host.StopAsync(TimeSpan.FromSeconds(5));
        }
    }
}
```

### <a name="startasync-and-stopasync"></a>StartAsync i StopAsync

<xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> Uruchamia aplikację.

<xref:Microsoft.Extensions.Hosting.IHost.StopAsync*> Zatrzymuje aplikację.

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        using (host)
        {
            await host.StartAsync();

            await host.StopAsync();
        }
    }
}
```

### <a name="waitforshutdown"></a>WaitForShutdown

<xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> jest wyzwalany za pośrednictwem <xref:Microsoft.Extensions.Hosting.IHostLifetime>, takich jak <xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime> (nasłuchuje `Ctrl+C`/SIGINT lub SIGTERM). `WaitForShutdown` wywołania <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.

```csharp
public class Program
{
    public void Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        using (host)
        {
            host.Start();

            host.WaitForShutdown();
        }
    }
}
```

### <a name="waitforshutdownasync"></a>WaitForShutdownAsync

<xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> Zwraca `Task` który zostaje ukończony po wyzwoleniu zamknięcie za pośrednictwem danego tokenu i wywołania <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        using (host)
        {
            await host.StartAsync();

            await host.WaitForShutdownAsync();
        }

    }
}
```

### <a name="external-control"></a>Zewnętrznej kontroli

Zewnętrznej kontroli hosta można osiągnąć przy użyciu metod, które mogą być wywoływane zewnętrznie:

```csharp
public class Program
{
    private IHost _host;

    public Program()
    {
        _host = new HostBuilder()
            .Build();
    }

    public async Task StartAsync()
    {
        _host.StartAsync();
    }

    public async Task StopAsync()
    {
        using (_host)
        {
            await _host.StopAsync(TimeSpan.FromSeconds(5));
        }
    }
}
```

<xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> nosi nazwę na początku <xref:Microsoft.Extensions.Hosting.IHost.StartAsync*>, który powoduje oczekiwanie, dopóki nie zostanie ukończone przed kontynuowaniem. Może to służyć do opóźnienie uruchamiania, dopóki nie zasygnalizowane przez zdarzenie zewnętrzne.

## <a name="ihostingenvironment-interface"></a>Interfejs IHostingEnvironment

<xref:Microsoft.Extensions.Hosting.IHostingEnvironment> zawiera informacje o aplikacji w środowisku hostingu. Użyj [iniekcji konstruktora](xref:fundamentals/dependency-injection) uzyskać `IHostingEnvironment` aby można było używać jej właściwości i metody rozszerzenia:

```csharp
public class MyClass
{
    private readonly IHostingEnvironment _env;

    public MyClass(IHostingEnvironment env)
    {
        _env = env;
    }

    public void DoSomething()
    {
        var environmentName = _env.EnvironmentName;
    }
}
```

Aby uzyskać więcej informacji, zobacz <xref:fundamentals/environments>.

## <a name="iapplicationlifetime-interface"></a>Interfejs IApplicationLifetime

<xref:Microsoft.Extensions.Hosting.IApplicationLifetime> Umożliwia działań po uruchamiania i zamykania, w tym łagodne zamykanie żądania. Trzy właściwości w interfejsie są tokenów anulowania, używane do rejestrowania `Action` metod, które definiują zdarzenia uruchamiania i zamykania.

| Token anulowania | Wyzwalane, gdy&#8230; |
| ------------------ | --------------------- |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStarted*> | Host pełni została uruchomiona. |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStopped*> | Host jest kończonych łagodne zamykanie. Wszystkie żądania powinny zostać przetworzone. Blokuje zamknięcia, aż do zakończenia tego zdarzenia. |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStopping*> | Wykonuje łagodne zamykanie hosta. Żądania mogą nadal być przetwarzane. Blokuje zamknięcia, aż do zakończenia tego zdarzenia. |

Wstrzykiwanie Konstruktor `IApplicationLifetime` usługi do każdej klasy. [Przykładową aplikację](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) używa iniekcji konstruktora do `LifetimeEventsHostedService` klasy ( `IHostedService` implementacji) do rejestrowania zdarzeń.

*LifetimeEventsHostedService.cs*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/LifetimeEventsHostedService.cs?name=snippet1)]

<xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> żądania zakończenia aplikacji. Następujące klasy używa `StopApplication` bezpiecznie zamknąć aplikację po klasy `Shutdown` metoda jest wywoływana:

```csharp
public class MyClass
{
    private readonly IApplicationLifetime _appLifetime;

    public MyClass(IApplicationLifetime appLifetime)
    {
        _appLifetime = appLifetime;
    }

    public void Shutdown()
    {
        _appLifetime.StopApplication();
    }
}
```

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:fundamentals/host/hosted-services>
* [Hosting repozytorium przykładów w witrynie GitHub](https://github.com/aspnet/Hosting/tree/release/2.1/samples)
