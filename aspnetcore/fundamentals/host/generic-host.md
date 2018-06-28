---
title: .NET rodzajowego hosta
author: guardrex
description: Więcej informacji na temat ogólnych hosta w .NET, który jest odpowiedzialny za zarządzanie uruchamiania i okresem istnienia aplikacji.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/16/2018
uid: fundamentals/host/generic-host
ms.openlocfilehash: 40d297257895a4defeb89cef9c5ec6deea64a985
ms.sourcegitcommit: 7003d27b607e529642ded0400aa48ae692a0e666
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37033358"
---
# <a name="net-generic-host"></a>.NET rodzajowego hosta

Przez [Luke Latham](https://github.com/guardrex)

Aplikacje .NET skonfigurować i uruchomić *hosta*. Host jest odpowiedzialny za zarządzanie uruchamiania i okresem istnienia aplikacji. W tym temacie omówiono Host ogólnego ASP.NET Core ([HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder)), co jest przydatne do hostowania aplikacji, które nie przetwarzają żądań HTTP. Pokrycia hosta sieci Web ([WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)), zobacz [hosta sieci Web](xref:fundamentals/host/web-host) tematu.

Celem rodzajowego hosta jest oddzielana potoku HTTP z interfejsu API hosta sieci Web, aby włączyć szerszego scenariuszy hosta. Do obsługi komunikatów, zadania w tle i innych obciążeń innych niż HTTP na podstawie ogólnego hosta korzyści z kompleksowymi możliwości, takie jak konfiguracja, iniekcji zależności (Podpisane) i rejestrowania.

Ogólny hosta jest nowa w programie ASP.NET Core 2.1 i nie jest odpowiedni dla scenariuszy hostingu w sieci web. W scenariuszach hostingu sieci web, użyj [hosta sieci Web](xref:fundamentals/host/web-host). Ogólny hosta jest w fazie projektowania hosta sieci Web w przyszłych wydaniach i działa jako podstawowy hosta interfejsu API w protokołów HTTP i scenariusze innych niż HTTP.

[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

Podczas uruchamiania przykładową aplikację [Visual Studio Code](https://code.visualstudio.com/), użyj *terminal zewnętrznego lub zintegrowane*. Nie uruchamiaj próbki `internalConsole`.

Aby ustawić konsoli w programie Visual Studio Code:

1. Otwórz *.vscode/launch.json* pliku.
1. W **.NET Core uruchom (Konsola)** konfiguracji, zlokalizuj **konsoli** wpisu. Ustaw wartość na jedną `externalTerminal` lub `integratedTerminal`.

## <a name="introduction"></a>Wprowadzenie

Biblioteka rodzajowego hosta jest dostępna w [przestrzeni nazw Microsoft.Extensions.Hosting](/dotnet/api/microsoft.extensions.hosting) i udostępnionych przez [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/) pakietu. `Microsoft.Extensions.Hosting` Pakietu znajduje się w [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (platformy ASP.NET Core 2.1 lub nowszej).

[IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) jest punkt wejścia do wykonania kodu. Każdy `IHostedService` implementacji jest wykonywany w celu z [usługi rejestracji w ConfigureServices](#configureservices). [StartAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) jest wywoływana w każdym `IHostedService` po uruchomieniu hosta i [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) jest wywoływane w kolejności odwrotnej rejestracji, gdy host zamyka bezpieczne.

## <a name="set-up-a-host"></a>Konfigurowanie hosta

[IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) jest głównym składnikiem korzystające z biblioteki i aplikacji do zainicjowania, tworzenie i uruchamianie hosta:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_HostBuilder)]

## <a name="host-configuration"></a>Konfiguracja hosta

[HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder) zależy od następujących metod można ustawić wartości konfiguracji hosta:

* Konstruktor konfiguracji
* Konfiguracja — metoda rozszerzenia

### <a name="configuration-builder"></a>Konstruktor konfiguracji

Konfiguracja Konstruktora hosta jest tworzony przez wywołanie metody [ConfigureHostConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configurehostconfiguration) na [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) implementacji. `ConfigureHostConfiguration` używa [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) utworzyć [wartości IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) dla hosta. Inicjuje konstruktora konfiguracji [IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) do użycia w procesie kompilacji.

Domyślnie nie jest dodawany konfiguracji zmiennej środowiskowej. Wywołanie [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) na Konstruktor hosta, aby skonfigurować hosta z zmiennych środowiskowych. `AddEnvironmentVariables` akceptuje opcjonalny prefiks zdefiniowany przez użytkownika. Przykładowa aplikacja korzysta z prefiksem `PREFIX_`. Prefiks jest usuwany przeczytaniu zmiennych środowiskowych. Jeśli została skonfigurowana Przykładowa aplikacja hosta, wartość zmiennej środowiskowej dla `PREFIX_ENVIRONMENT` staje się wartością konfiguracji hosta dla `environment` klucza.

Podczas tworzenia przy użyciu [programu Visual Studio](https://www.visualstudio.com/) lub uruchamiania aplikacji z `dotnet run`, zmienne środowiskowe może być ustawiona w *Properties/launchSettings.json* pliku. W [Visual Studio Code](https://code.visualstudio.com/), zmienne środowiskowe może być ustawiona w *.vscode/launch.json* plik w czasie projektowania. Aby uzyskać więcej informacji, zobacz [używać wiele środowisk](xref:fundamentals/environments).

`ConfigureHostConfiguration` może być wywołana wiele razy z dodatku wyników. Host używa jednego z tych opcji ustawia wartość ostatnio.

*hostsettings.JSON*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/hostsettings.json)]

Przykład `HostBuilder` konfiguracji przy użyciu `ConfigureHostConfiguration`:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureHostConfiguration)]

> [!NOTE]
> [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) — metoda rozszerzenia nie jest obecnie stanie podczas analizowania sekcji konfiguracji zwrócony przez [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (na przykład `.AddConfiguration(Configuration.GetSection("section"))`. `GetSection` — Metoda filtruje klucze konfiguracji do sekcji żądanie, jednak nie pozostawia nazwy sekcji kluczy (na przykład `section:environment`). `AddConfiguration` Metoda oczekuje klucze do dopasowania `HostBuilder` kluczy (na przykład `environment`). Występowanie nazwy sekcji kluczy uniemożliwia wartości w sekcji Konfigurowanie hosta. Ten problem zostanie rozwiązany w kolejnej wersji. Aby uzyskać więcej informacji i rozwiązania problemu, zobacz [przekazywanie Sekcja konfiguracyjna do WebHostBuilder.UseConfiguration używa kluczy pełna](https://github.com/aspnet/Hosting/issues/839).

### <a name="extension-method-configuration"></a>Konfiguracja — metoda rozszerzenia

Metody rozszerzenia są wywoływane na `IHostBuilder` implementacji, aby skonfigurować główny zawartości i środowiska.

#### <a name="content-root"></a>Główny zawartości

To ustawienie określa, gdzie hosta rozpocznie się wyszukiwanie plików zawartości.

**Klucz**: contentRoot  
**Typ**: *ciągu*  
**Domyślna**: domyślne do folderu, w którym znajduje się zestaw aplikacji.  
**Ustawić za pomocą**: `UseContentRoot`  
**Zmienna środowiskowa**: `<PREFIX_>CONTENTROOT` (`<PREFIX_>` jest [opcjonalne i zdefiniowanych przez użytkownika](#configuration-builder))

Jeśli ścieżka nie istnieje, host nie powiedzie się.

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseContentRoot)]

#### <a name="environment"></a>Środowisko

Ustawia aplikacji [środowiska](xref:fundamentals/environments).

**Klucz**: środowiska  
**Typ**: *ciągu*  
**Domyślna**: produkcji  
**Ustawić za pomocą**: `UseEnvironment`  
**Zmienna środowiskowa**: `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` jest [opcjonalne i zdefiniowanych przez użytkownika](#configuration-builder))

Środowisko można ustawić dowolną wartość. Wartości zdefiniowane w ramach obejmują `Development`, `Staging`, i `Production`. Wartości nie jest uwzględniana wielkość liter.

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseEnvironment)]

## <a name="configureappconfiguration"></a>ConfigureAppConfiguration

Konfiguracja Konstruktora aplikacji jest tworzony przez wywołanie metody [ConfigureAppConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configureappconfiguration) na [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) implementacji. `ConfigureAppConfiguration` używa [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) utworzyć [wartości IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) dla aplikacji. `ConfigureAppConfiguration` może być wywołana wiele razy z dodatku wyników. Aplikacja korzysta z jednego z tych opcji ustawia wartość ostatnio. Konfiguracji utworzonej przez `ConfigureAppConfiguration` znajduje się w temacie [HostBuilderContext.Configuration](/dotnet/api/microsoft.extensions.hosting.hostbuildercontext.configuration) dla kolejnych operacji i w [usług](/dotnet/api/microsoft.extensions.hosting.ihost.services).

Przykład aplikacji konfiguracji przy użyciu `ConfigureAppConfiguration`:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureAppConfiguration)]

*appSettings.JSON*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.json)]

*appSettings. Development.JSON*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Development.json)]

*appSettings. Production.JSON*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Production.json)]

> [!NOTE]
> [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) — metoda rozszerzenia nie jest obecnie stanie podczas analizowania sekcji konfiguracji zwrócony przez [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (na przykład `.AddConfiguration(Configuration.GetSection("section"))`. `GetSection` — Metoda filtruje klucze konfiguracji do sekcji żądanie, jednak nie pozostawia nazwy sekcji kluczy (na przykład `section:Logging:LogLevel:Default`). `AddConfiguration` Metoda oczekuje dokładnego dopasowania do konfiguracji kluczy (na przykład `Logging:LogLevel:Default`). Występowanie nazwy sekcji kluczy uniemożliwia wartości w sekcji Konfigurowanie aplikacji. Ten problem zostanie rozwiązany w kolejnej wersji. Aby uzyskać więcej informacji i rozwiązania problemu, zobacz [przekazywanie Sekcja konfiguracyjna do WebHostBuilder.UseConfiguration używa kluczy pełna](https://github.com/aspnet/Hosting/issues/839).

## <a name="configureservices"></a>ConfigureServices

[ConfigureServices](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configureservices) dodaje usług do aplikacji [iniekcji zależności](xref:fundamentals/dependency-injection) kontenera. `ConfigureServices` może być wywołana wiele razy z dodatku wyników.

Hostowana usługa jest klasa z logiką zadania tła, który implementuje [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) interfejsu. Aby uzyskać więcej informacji, zobacz [zadania związane z usług hostowanych w tle](xref:fundamentals/host/hosted-services) tematu.

[Przykładowa aplikacja](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) używa `AddHostedService` — metoda rozszerzenia, aby dodać usługę dla zdarzenia okresu istnienia `LifetimeEventsHostedService`i zadania w tle czasu, `TimedHostedService`, do aplikacji:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureServices)]

## <a name="configurelogging"></a>ConfigureLogging

[ConfigureLogging](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configurelogging) dodaje delegata do konfigurowania udostępnionych [ILoggingBuilder](/dotnet/api/microsoft.extensions.logging.iloggingbuilder). `ConfigureLogging` może zostać wywołana wiele razy z wynikami dodatku.

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureLogging)]

### <a name="useconsolelifetime"></a>UseConsoleLifetime

[UseConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.useconsolelifetime) nasłuchuje `Ctrl+C`/SIGINT lub sigterm — i wywołania [StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) można uruchomić procesu zamykania. `UseConsoleLifetime` Odblokowuje rozszerzenia takich jak [RunAsync](#runasync) i [WaitForShutdownAsync](#waitforshutdownasync). [ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) wstępnie jest zarejestrowany jako domyślna implementacja okres istnienia. Okres istnienia ostatniego zarejestrowany jest używany.

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseConsoleLifetime)]

## <a name="container-configuration"></a>Kontenera konfiguracji

Aby zapewnić obsługę podłączania innych kontenerów, host może zaakceptować [IServiceProviderFactory](/dotnet/api/microsoft.extensions.dependencyinjection.iserviceproviderfactory-1). Udostępnia fabrykę nie jest częścią rejestracji kontenera Podpisane, ale zamiast tego jest używany do tworzenia konkretnych kontenera Podpisane hosta — wewnętrzne. [UseServiceProviderFactory (IServiceProviderFactory&lt;TContainerBuilder&gt;)](/dotnet/api/microsoft.extensions.hosting.hostbuilder.useserviceproviderfactory) zastępuje domyślną fabrykę używany do tworzenia aplikacji usługodawcy.

Niestandardowe kontenera konfiguracji jest zarządzana przez [ConfigureContainer](/dotnet/api/microsoft.extensions.hosting.hostbuilder.configurecontainer) metody. `ConfigureContainer` zapewnia obsługę jednoznacznie konfigurowania kontenera na górze odpowiedniego hosta interfejsu API. `ConfigureContainer` może być wywołana wiele razy z dodatku wyników.

Utwórz kontener usługi dla aplikacji:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainer.cs)]

Podaj fabryki kontenera usług:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainerFactory.cs)]

Użyj fabryka i skonfiguruj kontener usług niestandardowych dla aplikacji:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ContainerConfiguration)]

## <a name="extensibility"></a>Rozszerzalność

Rozszerzalność hosta odbywa się za pomocą metod rozszerzenia na `IHostBuilder`. W poniższym przykładzie pokazano, jak rozszerza metodę rozszerzenia `IHostBuilder` implementację [RabbitMQ](https://www.rabbitmq.com/). Metoda rozszerzenia (w aplikacji) rejestruje RabbitMQ `IHostedService`:

```csharp
// UseRabbitMq is an extension method that sets up RabbitMQ to handle incoming
// messages.
var host = new HostBuilder()
    .UseRabbitMq<MyMessageHandler>()
    .Build();

await host.StartAsync();
```

## <a name="manage-the-host"></a>Zarządzanie hosta

[IHost](/dotnet/api/microsoft.extensions.hosting.ihost) implementacja jest odpowiedzialna za uruchamianie i zatrzymywanie `IHostedService` implementacji, które są zarejestrowane w kontenerze usług.

### <a name="run"></a>Uruchom

[Uruchom](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.run) uruchamia aplikację i blokuje wątek wywołujący, dopóki nie zostanie zamknięta hosta:

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

[RunConsoleAsync](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.runconsoleasync) włącza obsługę konsoli, kompiluje i uruchamia hosta i czeka na `Ctrl+C`/SIGINT lub sigterm — w celu zamknięcia.

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

### <a name="start-and-stopasync"></a>Start i StopAsync

[Uruchom](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.start) synchronicznie uruchamia hosta.

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

[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync) uruchomienia aplikacji.

[StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync) zatrzymuje aplikacji.

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

[WaitForShutdown](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdown) zostanie wywołany za pomocą [IHostLifetime](/dotnet/api/microsoft.extensions.hosting.ihostlifetime), takich jak [ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) (nasłuchuje `Ctrl+C`/SIGINT lub sigterm —). `WaitForShutdown` wywołania [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).

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

[WaitForShutdownAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdownasync) zwraca `Task` który zostaje ukończony po wyzwoleniu zamknięcia za pomocą danego tokenu i wywołania [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).

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

### <a name="external-control"></a>Zewnętrzny

Formant zewnętrznego hosta można osiągnąć przy użyciu metod, które mogą być wywoływane zewnętrznie:

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

[IHostLifetime.WaitForStartAsync](/dotnet/api/microsoft.extensions.hosting.ihostlifetime.waitforstartasync) nazywa się na początku [StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync), który oczekuje jego zakończenie przed kontynuowaniem. Może to służyć do opóźnienia uruchomienia aż zgłoszony przez zdarzenie zewnętrzne.

## <a name="ihostingenvironment-interface"></a>Interfejs IHostingEnvironment

[IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) zapewnia środowiska macierzystego informacji o aplikacji. Użyj [iniekcji konstruktora](xref:fundamentals/dependency-injection) uzyskanie `IHostingEnvironment` aby można było używać ich właściwości i metody rozszerzenia:

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

Aby uzyskać więcej informacji, zobacz [używać wiele środowisk](xref:fundamentals/environments).

## <a name="iapplicationlifetime-interface"></a>Interfejs IApplicationLifetime

[IApplicationLifetime](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime) umożliwia po uruchamiania i wyłączania działań, włącznie z żądaniami łagodne zamykanie. Anulowanie tokenów używany do rejestrowania są trzy właściwości w interfejsie `Action` metod, które definiują zdarzenia uruchamiania i wyłączania.

| Token anulowania | Wyzwalane, gdy&#8230; |
| ------------------ | --------------------- |
| [ApplicationStarted](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstarted) | Host pełni została uruchomiona. |
| [ApplicationStopped](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopped) | Host jest kończonych łagodne zamykanie. Wszystkie żądania powinna zostać przetworzona. Bloki zamknięcia aż do zakończenia tego zdarzenia. |
| [ApplicationStopping](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopping) | Host wykonuje łagodne zamykanie. Żądania mogą nadal być przetwarzane. Bloki zamknięcia aż do zakończenia tego zdarzenia. |

Wstaw konstruktora `IApplicationLifetime` usługi do żadnej klasy. [Przykładowa aplikacja](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) używa iniekcji konstruktora do `LifetimeEventsHostedService` klasy ( `IHostedService` implementacji) można zarejestrować zdarzenia.

*LifetimeEventsHostedService.cs*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/LifetimeEventsHostedService.cs?name=snippet1)]

[StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) żąda zakończenia aplikacji. Następujące klasy używa `StopApplication` można bezpiecznie zamknąć aplikację po klasy `Shutdown` metoda jest wywoływana:

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

* [Zadania w tle z usługami hostowanymi](xref:fundamentals/host/hosted-services)
* [Hosting przykłady repozytorium w witrynie GitHub](https://github.com/aspnet/Hosting/tree/release/2.1/samples)
