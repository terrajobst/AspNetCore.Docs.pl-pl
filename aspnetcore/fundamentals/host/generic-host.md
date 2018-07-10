---
title: Ogólny hosta platformy .NET
author: guardrex
description: Więcej informacji na temat ogólnych hosta na platformie .NET, który jest odpowiedzialny za zarządzanie uruchamiania i czasu życia aplikacji.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/16/2018
uid: fundamentals/host/generic-host
ms.openlocfilehash: 879f31a5916646a4d63f9f503173dc9ff4c53434
ms.sourcegitcommit: ea7ec8d47f94cfb8e008d771f647f86bbb4baa44
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/06/2018
ms.locfileid: "37894156"
---
# <a name="net-generic-host"></a>Ogólny hosta platformy .NET

Przez [Luke Latham](https://github.com/guardrex)

Konfigurowanie aplikacji platformy .NET i uruchamiania *hosta*. Host jest odpowiedzialny za zarządzanie uruchamiania i czasu życia aplikacji. W tym temacie omówiono Host rodzajowego Core ASP.NET ([HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder)), co jest przydatne do hostowania aplikacji, które nie przetwarzają żądania HTTP. Pokrycia hosta sieci Web ([WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)), zobacz <xref:fundamentals/host/web-host>.

Celem ogólnego hosta jest rozdzielenie potoku HTTP z hosta internetowego interfejsu API, umożliwiające szersze gamę scenariuszy hosta. Komunikaty, zadania w tle i innych obciążeń innych niż HTTP oparte na korzyść ogólnego hosta z przekrojowe możliwości, takich jak konfiguracja, wstrzykiwanie zależności (DI) i rejestrowania.

Ogólny hosta jest nowa w programie ASP.NET Core 2.1 i nie jest odpowiednie w scenariuszach hostingu w sieci web. W przypadku scenariuszy hostingu w sieci web, użyj [hosta sieci Web](xref:fundamentals/host/web-host). Ogólny Host jest w fazie projektowania w celu zastąpienia hosta sieci Web w przyszłym wydaniu i pełnić rolę hosta podstawowego interfejsu API zarówno w przypadku protokołu HTTP, jak i scenariuszy innych niż HTTP.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

Podczas uruchamiania przykładowej aplikacji [programu Visual Studio Code](https://code.visualstudio.com/), użyj *zewnętrznych lub w zintegrowanym terminalu*. Nie, uruchom przykład w `internalConsole`.

Aby ustawić konsoli w programie Visual Studio Code:

1. Otwórz *.vscode/launch.json* pliku.
1. W **.NET Core uruchamianie (Konsola)** konfiguracji, zlokalizuj **konsoli** wpisu. Ustaw wartość na wartość `externalTerminal` lub `integratedTerminal`.

## <a name="introduction"></a>Wprowadzenie

Biblioteka ogólnego hosta jest dostępna w [Microsoft.Extensions.Hosting przestrzeni nazw](/dotnet/api/microsoft.extensions.hosting) i udostępnionych przez [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/) pakietu. `Microsoft.Extensions.Hosting` Pakietu znajduje się w [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) (platformy ASP.NET Core 2.1 lub nowszej).

[Pomocą interfejsu IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) jest punktem wejścia do wykonania kodu. Każdy `IHostedService` implementacja jest wykonywany w kolejności od [usługi rejestracji w ConfigureServices](#configureservices). [StartAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) jest wywoływana w każdej `IHostedService` podczas uruchamiania hosta, a [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) jest wywoływana w kolejności odwrotnej rejestracji, gdy kończy pracę bez problemu zmieniała hosta.

## <a name="set-up-a-host"></a>Konfigurowanie hosta

[IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) jest głównym składnikiem, który bibliotek i aplikacji umożliwia inicjowanie, tworzeniu i uruchamianiu hosta:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_HostBuilder)]

## <a name="host-configuration"></a>Konfiguracja hosta

[HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder) opiera się na następujących podejść można ustawić wartości konfiguracji hostów:

* Konstruktor konfiguracji
* Konfiguracja metody rozszerzenia

### <a name="configuration-builder"></a>Konstruktor konfiguracji

Konfiguracja Konstruktora hosta jest tworzony przez wywołanie [ConfigureHostConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configurehostconfiguration) na [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) implementacji. `ConfigureHostConfiguration` używa [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) utworzyć [wartości IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) dla hosta. Inicjuje konstruktora konfiguracji [IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) do użycia w procesie kompilacji aplikacji.

Zmienne konfiguracji środowiska nie jest dodawany domyślnie. Wywołaj [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) na Konstruktor hosta, aby skonfigurować hosta ze zmiennych środowiskowych. `AddEnvironmentVariables` akceptuje opcjonalny prefiks zdefiniowany przez użytkownika. Przykładowa aplikacja korzysta z prefiksem `PREFIX_`. Prefiks jest usuwany, gdy zmienne środowiskowe są odczytywane. Po skonfigurowaniu hostów przykładową aplikację, wartość zmiennej środowiskowej, aby uzyskać `PREFIX_ENVIRONMENT` staje się wartość konfiguracji hosta `environment` klucza.

Podczas programowania, korzystając z [programu Visual Studio](https://www.visualstudio.com/) lub uruchamianie aplikacji za pomocą `dotnet run`, zmienne środowiskowe, może być ustawiona w *Properties/launchSettings.json* pliku. W [programu Visual Studio Code](https://code.visualstudio.com/), zmienne środowiskowe, może być ustawiona w *.vscode/launch.json* pliku podczas programowania. Aby uzyskać więcej informacji, zobacz <xref:fundamentals/environments>.

`ConfigureHostConfiguration` można wywołać wiele razy z wynikami dodatku. Host używa jednego z tych opcji ustawia wartość ostatniego.

*hostsettings.JSON*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/hostsettings.json)]

Przykład `HostBuilder` konfiguracji przy użyciu `ConfigureHostConfiguration`:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureHostConfiguration)]

> [!NOTE]
> [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) metody rozszerzenia nie jest obecnie zdolne do analizowania sekcji konfiguracji, zwracany przez [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (na przykład `.AddConfiguration(Configuration.GetSection("section"))`. `GetSection` Metoda klucze konfiguracji do sekcji żądane filtry, ale klucze nie powoduje usunięcia nazwy sekcji (na przykład `section:environment`). `AddConfiguration` Metoda oczekuje kluczy, aby dopasować `HostBuilder` kluczy (na przykład `environment`). Obecność nazwa sekcji na kluczach zapobiega wartości w sekcji Konfigurowanie hosta. Ten problem zostanie rozwiązany w kolejnej wersji. Aby uzyskać więcej informacji i rozwiązania problemu, zobacz [przekazywanie sekcję konfiguracji do WebHostBuilder.UseConfiguration korzysta z kluczami pełną](https://github.com/aspnet/Hosting/issues/839).

### <a name="extension-method-configuration"></a>Konfiguracja metody rozszerzenia

Metody rozszerzenia są wywoływane na `IHostBuilder` implementacji, aby skonfigurować zawartość katalogu głównego i środowiska.

#### <a name="application-key-name"></a>Klucz aplikacji (nazwa)

[IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) właściwość ma wartość z konfiguracji hosta podczas konstruowania hosta. Aby jawnie ustawić wartość, użyj [HostDefaults.ApplicationKey](/dotnet/api/microsoft.extensions.hosting.hostdefaults.applicationkey):

**Klucz**: applicationName  
**Typ**: *ciągu*  
**Domyślne**: Nazwa zestawu zawierającego punkt wejścia aplikacji.  
**Można ustawić przy użyciu**: `UseSetting`  
**Zmienna środowiskowa**: `<PREFIX_>APPLICATIONKEY` (`<PREFIX_>` jest [opcjonalne i zdefiniowane przez użytkownika](#configuration-builder))

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.ApplicationKey, "CustomApplicationName")
```

#### <a name="content-root"></a>Zawartość katalogu głównego

To ustawienie określa, gdzie hosta rozpoczyna się wyszukiwanie plików zawartości.

**Klucz**: contentRoot  
**Typ**: *ciągu*  
**Domyślne**: wartość domyślna to folder, w którym znajduje się zestaw aplikacji.  
**Można ustawić przy użyciu**: `UseContentRoot`  
**Zmienna środowiskowa**: `<PREFIX_>CONTENTROOT` (`<PREFIX_>` jest [opcjonalne i zdefiniowane przez użytkownika](#configuration-builder))

Jeśli ścieżka nie istnieje, host nie można uruchomić.

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseContentRoot)]

#### <a name="environment"></a>Środowisko

Ustawia aplikacji [środowiska](xref:fundamentals/environments).

**Klucz**: środowisko  
**Typ**: *ciągu*  
**Domyślne**: produkcji  
**Można ustawić przy użyciu**: `UseEnvironment`  
**Zmienna środowiskowa**: `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` jest [opcjonalne i zdefiniowane przez użytkownika](#configuration-builder))

Środowisko można ustawić dowolną wartość. Wartości zdefiniowane w ramach obejmują `Development`, `Staging`, i `Production`. Wartości nie są z uwzględnieniem wielkości liter.

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseEnvironment)]

## <a name="configureappconfiguration"></a>ConfigureAppConfiguration

Konfiguracja Konstruktora aplikacji jest tworzony przez wywołanie [ConfigureAppConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configureappconfiguration) na [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) implementacji. `ConfigureAppConfiguration` używa [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) utworzyć [wartości IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) dla aplikacji. `ConfigureAppConfiguration` można wywołać wiele razy z wynikami dodatku. Aplikacja używa jednego z tych opcji ustawia wartość ostatniego. Konfiguracji utworzonej przez `ConfigureAppConfiguration` znajduje się w temacie [HostBuilderContext.Configuration](/dotnet/api/microsoft.extensions.hosting.hostbuildercontext.configuration) dla kolejnych operacji i w [usług](/dotnet/api/microsoft.extensions.hosting.ihost.services).

Przykład korzystającą configuration `ConfigureAppConfiguration`:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureAppConfiguration)]

*appSettings.JSON*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.json)]

*appSettings. Development.JSON*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Development.json)]

*appSettings. Production.JSON*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Production.json)]

> [!NOTE]
> [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) metody rozszerzenia nie jest obecnie zdolne do analizowania sekcji konfiguracji, zwracany przez [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (na przykład `.AddConfiguration(Configuration.GetSection("section"))`. `GetSection` Metoda klucze konfiguracji do sekcji żądane filtry, ale klucze nie powoduje usunięcia nazwy sekcji (na przykład `section:Logging:LogLevel:Default`). `AddConfiguration` Metoda oczekuje dokładnie dopasowany do klucze konfiguracji (na przykład `Logging:LogLevel:Default`). Obecność nazwa sekcji na kluczach zapobiega wartości sekcji konfigurowania aplikacji. Ten problem zostanie rozwiązany w kolejnej wersji. Aby uzyskać więcej informacji i rozwiązania problemu, zobacz [przekazywanie sekcję konfiguracji do WebHostBuilder.UseConfiguration korzysta z kluczami pełną](https://github.com/aspnet/Hosting/issues/839).

Aby przenieść pliki ustawień do katalogu wyjściowego, określ pliki ustawień jako [elementów projektu programu MSBuild](/visualstudio/msbuild/common-msbuild-project-items) w pliku projektu. Przykładowa aplikacja przenosi jego pliki ustawień aplikacji w formacie JSON i *hostsettings.json* następującym **&lt;zawartości:&gt;** elementu:

```xml
<ItemGroup>
  <Content Include="**\*.json" CopyToOutputDirectory="PreserveNewest" />
</ItemGroup>
```

## <a name="configureservices"></a>ConfigureServices

[ConfigureServices](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configureservices) dodaje usług do aplikacji [wstrzykiwanie zależności](xref:fundamentals/dependency-injection) kontenera. `ConfigureServices` można wywołać wiele razy z wynikami dodatku.

Usługa hostowana jest klasą z logiką zadań tła, który implementuje [pomocą interfejsu IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) interfejsu. Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/hosted-services>.

[Przykładową aplikację](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) używa `AddHostedService` metodę rozszerzenia, aby dodać usługę do zdarzenia okresu istnienia `LifetimeEventsHostedService`i zadania w tle czasu `TimedHostedService`, do aplikacji:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureServices)]

## <a name="configurelogging"></a>ConfigureLogging

[ConfigureLogging](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configurelogging) dodaje delegata do konfigurowania podane [ILoggingBuilder](/dotnet/api/microsoft.extensions.logging.iloggingbuilder). `ConfigureLogging` może zostać wywołana wiele razy z wynikami dodatku.

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureLogging)]

### <a name="useconsolelifetime"></a>UseConsoleLifetime

[UseConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.useconsolelifetime) nasłuchuje `Ctrl+C`SIGTERM lub wywołań i /SIGINT [StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) można uruchomić procesu zamykania. `UseConsoleLifetime` Odblokowuje rozszerzeń, takich jak [RunAsync](#runasync) i [WaitForShutdownAsync](#waitforshutdownasync). [ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) wstępnie jest zarejestrowany jako domyślna implementacja okresu istnienia. Ostatniego okresu istnienia zarejestrowany jest używany.

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseConsoleLifetime)]

## <a name="container-configuration"></a>Konfiguracja kontenera

Aby zapewnić obsługę podłączania innych kontenerów, host może akceptować [IServiceProviderFactory](/dotnet/api/microsoft.extensions.dependencyinjection.iserviceproviderfactory-1). Zapewnianie fabrykę nie jest częścią DI rejestracja kontenera jest jednak wewnętrzne hosta używany do tworzenia konkretnych kontenera DI. [UseServiceProviderFactory (IServiceProviderFactory&lt;TContainerBuilder&gt;)](/dotnet/api/microsoft.extensions.hosting.hostbuilder.useserviceproviderfactory) zastępuje domyślną fabrykę użyty do utworzenia dostawcy usługi app service.

Konfiguracja kontenera niestandardowego jest zarządzana przez [ConfigureContainer](/dotnet/api/microsoft.extensions.hosting.hostbuilder.configurecontainer) metody. `ConfigureContainer` udostępnia silnie typizowane środowisko na potrzeby konfigurowania kontenera na podstawie odpowiedniego hosta interfejsu API. `ConfigureContainer` można wywołać wiele razy z wynikami dodatku.

Tworzenie kontenera usług dla aplikacji:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainer.cs)]

Podaj fabryki kontenera usług:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainerFactory.cs)]

Używaj fabryki i konfigurowanie kontenera niestandardowego usługi dla aplikacji:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ContainerConfiguration)]

## <a name="extensibility"></a>Rozszerzalność

Rozszerzalność hosta odbywa się za pomocą metod rozszerzenia na `IHostBuilder`. W poniższym przykładzie pokazano, jak metoda rozszerzenia rozszerza `IHostBuilder` implementację [RabbitMQ](https://www.rabbitmq.com/). Metoda rozszerzenia (gdzie indziej w aplikacji) rejestruje RabbitMQ `IHostedService`:

```csharp
// UseRabbitMq is an extension method that sets up RabbitMQ to handle incoming
// messages.
var host = new HostBuilder()
    .UseRabbitMq<MyMessageHandler>()
    .Build();

await host.StartAsync();
```

## <a name="manage-the-host"></a>Zarządzać hosta

[IHost](/dotnet/api/microsoft.extensions.hosting.ihost) implementacja jest odpowiedzialna za uruchamianie i zatrzymywanie `IHostedService` implementacji, które są zarejestrowane w usłudze service container.

### <a name="run"></a>Uruchom

[Uruchom](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.run) uruchamia aplikację i blokuje wątek wywołujący, aż zamknie hosta:

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

[RunAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.runasync) uruchamia aplikację i zwraca `Task` który zostaje ukończony po wyzwoleniu token anulowania lub zamknięcia:

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

[RunConsoleAsync](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.runconsoleasync) umożliwia obsługę konsoli, tworzy i uruchamia hosta i czeka, aż `Ctrl+C`/SIGINT lub SIGTERM, aby zamknąć.

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

[Rozpocznij](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.start) synchronicznie uruchamia hosta.

[StopAsync(TimeSpan)](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.stopasync) podejmuje próby zatrzymania hosta w ciągu podanego limitu czasu.

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

[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync) uruchamia aplikację.

[StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync) zatrzymuje aplikację.

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

[WaitForShutdown](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdown) jest wyzwalany za pośrednictwem [IHostLifetime](/dotnet/api/microsoft.extensions.hosting.ihostlifetime), takich jak [ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) (nasłuchuje `Ctrl+C`/SIGINT lub SIGTERM). `WaitForShutdown` wywołania [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).

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

[WaitForShutdownAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdownasync) zwraca `Task` który zostaje ukończony po wyzwoleniu zamknięcie za pośrednictwem danego tokenu i wywołania [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).

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

[IHostLifetime.WaitForStartAsync](/dotnet/api/microsoft.extensions.hosting.ihostlifetime.waitforstartasync) nosi nazwę na początku [StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync), który powoduje oczekiwanie, dopóki nie zostanie ukończone przed kontynuowaniem. Może to służyć do opóźnienie uruchamiania, dopóki nie zasygnalizowane przez zdarzenie zewnętrzne.

## <a name="ihostingenvironment-interface"></a>Interfejs IHostingEnvironment

[IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) dostarcza informacji na temat aplikacji w środowisku hostingu. Użyj [iniekcji konstruktora](xref:fundamentals/dependency-injection) uzyskać `IHostingEnvironment` aby można było używać jej właściwości i metody rozszerzenia:

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

[IApplicationLifetime](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime) umożliwia działań po uruchamiania i zamykania, w tym łagodne zamykanie żądania. Trzy właściwości w interfejsie są tokenów anulowania, używane do rejestrowania `Action` metod, które definiują zdarzenia uruchamiania i zamykania.

| Token anulowania | Wyzwalane, gdy&#8230; |
| ------------------ | --------------------- |
| [ApplicationStarted](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstarted) | Host pełni została uruchomiona. |
| [ApplicationStopped](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopped) | Host jest kończonych łagodne zamykanie. Wszystkie żądania powinny zostać przetworzone. Blokuje zamknięcia, aż do zakończenia tego zdarzenia. |
| [ApplicationStopping](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopping) | Wykonuje łagodne zamykanie hosta. Żądania mogą nadal być przetwarzane. Blokuje zamknięcia, aż do zakończenia tego zdarzenia. |

Wstrzykiwanie Konstruktor `IApplicationLifetime` usługi do każdej klasy. [Przykładową aplikację](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) używa iniekcji konstruktora do `LifetimeEventsHostedService` klasy ( `IHostedService` implementacji) do rejestrowania zdarzeń.

*LifetimeEventsHostedService.cs*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/LifetimeEventsHostedService.cs?name=snippet1)]

[StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) żądania zakończenia aplikacji. Następujące klasy używa `StopApplication` bezpiecznie zamknąć aplikację po klasy `Shutdown` metoda jest wywoływana:

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
