---
title: Host ogólny .NET
author: rick-anderson
description: Dowiedz się więcej o hoście ogólnym programu .NET Core, który jest odpowiedzialny za uruchamianie aplikacji i zarządzanie okresem istnienia.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/23/2020
uid: fundamentals/host/generic-host
ms.openlocfilehash: 0f8f03dabf65f2cbfe4c41d36b02a25d7902cefb
ms.sourcegitcommit: 91dc1dd3d055b4c7d7298420927b3fd161067c64
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/24/2020
ms.locfileid: "80219223"
---
# <a name="net-generic-host"></a>Host ogólny .NET

::: moniker range=">= aspnetcore-5.0"

W tym artykule przedstawiono hosta ogólnego platformy .NET Core (<xref:Microsoft.Extensions.Hosting.HostBuilder>) i przedstawiono wskazówki dotyczące korzystania z niego.

## <a name="whats-a-host"></a>Co to jest host?

*Host* to obiekt, który hermetyzuje zasoby aplikacji, takie jak:

* Iniekcja zależności (DI)
* Rejestrowanie
* Konfiguracja
* implementacje `IHostedService`

Po uruchomieniu hosta wywołuje `IHostedService.StartAsync` przy każdej implementacji <xref:Microsoft.Extensions.Hosting.IHostedService>, która znajduje się w kontenerze DI. W aplikacji sieci Web jedna z implementacji `IHostedService` jest usługą sieci Web, która uruchamia [implementację serwera http](xref:fundamentals/index#servers).

Główną przyczyną uwzględnienia wszystkich zasobów zależnych od aplikacji w jednym obiekcie jest zarządzanie okresem istnienia: Kontrola uruchamiania aplikacji i bezpieczne zamykanie.

W wersjach ASP.NET Core wcześniejszych niż 3,0 [host sieci Web](xref:fundamentals/host/web-host) jest używany do obciążeń http. Host sieci Web nie jest już zalecany dla aplikacji sieci Web i pozostaje dostępny tylko w celu zapewnienia zgodności z poprzednimi wersjami.

## <a name="set-up-a-host"></a>Konfigurowanie hosta

Host jest zazwyczaj konfigurowany, zbudowany i uruchamiany przez kod w klasie `Program`. Metoda `Main`:

* Wywołuje metodę `CreateHostBuilder`, aby utworzyć i skonfigurować obiekt konstruktora.
* Wywołuje metody `Build` i `Run` w obiekcie konstruktora.

Oto kod *program.cs* dla obciążenia innego niż HTTP z jedną implementacją `IHostedService` dodaną do kontenera di. 

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateHostBuilder(args).Build().Run();
    }

    public static IHostBuilder CreateHostBuilder(string[] args) =>
        Host.CreateDefaultBuilder(args)
            .ConfigureServices((hostContext, services) =>
            {
               services.AddHostedService<Worker>();
            });
}
```

W przypadku obciążeń HTTP Metoda `Main` jest taka sama, ale `CreateHostBuilder` wywołuje `ConfigureWebHostDefaults`:

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.UseStartup<Startup>();
        });
```

Jeśli aplikacja używa Entity Framework Core, nie zmieniaj nazwy ani podpisu metody `CreateHostBuilder`. [Narzędzia Entity Framework Core](/ef/core/miscellaneous/cli/) oczekują na znalezienie `CreateHostBuilder`j metody, która konfiguruje hosta bez uruchamiania aplikacji. Aby uzyskać więcej informacji, zobacz [Tworzenie DbContext w czasie projektowania](/ef/core/miscellaneous/cli/dbcontext-creation).

## <a name="default-builder-settings"></a>Ustawienia domyślnego konstruktora

Metoda <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*>:

* Ustawia [katalog główny zawartości](xref:fundamentals/index#content-root) na ścieżkę zwracaną przez <xref:System.IO.Directory.GetCurrentDirectory*>.
* Ładuje konfigurację hosta z:
  * Zmienne środowiskowe poprzedzone prefiksem `DOTNET_`.
  * Argumenty wiersza polecenia.
* Ładuje konfigurację aplikacji z:
  * *appSettings. JSON*.
  * *appSettings. {Environment}. JSON*.
  * [Secret Manager](xref:security/app-secrets) , gdy aplikacja jest uruchamiana w środowisku `Development`owym.
  * Zmienne środowiskowe.
  * Argumenty wiersza polecenia.
* Dodaje następujących dostawców [rejestrowania](xref:fundamentals/logging/index) :
  * Konsola
  * Debugowanie
  * EventSource
  * EventLog (tylko w przypadku uruchamiania w systemie Windows)
* Umożliwia [weryfikację zakresu](xref:fundamentals/dependency-injection#scope-validation) i [Sprawdzanie poprawności zależności](xref:Microsoft.Extensions.DependencyInjection.ServiceProviderOptions.ValidateOnBuild) , gdy środowisko jest opracowywane.

Metoda `ConfigureWebHostDefaults`:

* Ładuje konfigurację hosta ze zmiennych środowiskowych z prefiksem `ASPNETCORE_`.
* Ustawia serwer [Kestrel](xref:fundamentals/servers/kestrel) jako serwer sieci Web i konfiguruje go przy użyciu dostawców konfiguracji hostingu aplikacji. Aby poznać domyślne opcje serwera Kestrel, zobacz <xref:fundamentals/servers/kestrel#kestrel-options>.
* Dodaje [oprogramowanie pośredniczące do filtrowania hosta](xref:fundamentals/servers/kestrel#host-filtering).
* Dodaje [przekazane nagłówki pośredniczące](xref:host-and-deploy/proxy-load-balancer#forwarded-headers) , jeśli `ASPNETCORE_FORWARDEDHEADERS_ENABLED` jest równe `true`.
* Włącza integrację usług IIS. Aby poznać domyślne opcje usług IIS, zobacz <xref:host-and-deploy/iis/index#iis-options>.

[Ustawienia dla wszystkich typów aplikacji](#settings-for-all-app-types) i [Ustawienia dla usługi Web Apps](#settings-for-web-apps) w dalszej części tego artykułu pokazują, jak zastąpić ustawienia domyślnego konstruktora.

## <a name="framework-provided-services"></a>Usługi udostępniane przez platformę

Następujące usługi są rejestrowane automatycznie:

* [IHostApplicationLifetime](#ihostapplicationlifetime)
* [IHostLifetime](#ihostlifetime)
* [IHostEnvironment / IWebHostEnvironment](#ihostenvironment)

Aby uzyskać więcej informacji na temat usług udostępnianych przez platformę, zobacz <xref:fundamentals/dependency-injection#framework-provided-services>.

## <a name="ihostapplicationlifetime"></a>IHostApplicationLifetime

Wstrzyknąć usługę <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime> (dawniej `IApplicationLifetime`) do dowolnej klasy w celu obsługi zadań po uruchomieniu i bezpiecznego zamykania. Trzy właściwości interfejsu to tokeny anulowania używane do rejestrowania metod obsługi zdarzeń uruchamiania i zatrzymywania aplikacji. Interfejs zawiera również metodę `StopApplication`.

Poniższy przykład jest implementacją `IHostedService`, która rejestruje zdarzenia `IHostApplicationLifetime`:

[!code-csharp[](generic-host/samples-snapshot/3.x/LifetimeEventsHostedService.cs?name=snippet_LifetimeEvents)]

## <a name="ihostlifetime"></a>IHostLifetime

Implementacja <xref:Microsoft.Extensions.Hosting.IHostLifetime> kontroluje, gdy host zostanie uruchomiony i gdy zostanie zatrzymana. Używana jest Ostatnia zarejestrowana implementacja.

`Microsoft.Extensions.Hosting.Internal.ConsoleLifetime` jest domyślną implementacją `IHostLifetime`. `ConsoleLifetime`:

* Nasłuchuje <kbd>kombinacji klawiszy Ctrl</kbd>+<kbd>C</kbd>/SIGINT lub SIGTERM i wywołuje <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime.StopApplication*>, aby rozpocząć proces zamykania.
* Odblokowuje rozszerzenia, takie jak [RunAsync](#runasync) i [WaitForShutdownAsync](#waitforshutdownasync).

## <a name="ihostenvironment"></a>IHostEnvironment

Wsuń usługę <xref:Microsoft.Extensions.Hosting.IHostEnvironment> do klasy, aby uzyskać informacje o następujących ustawieniach:

* [ApplicationName](#applicationname)
* [EnvironmentName](#environmentname)
* [ContentRootPath](#contentrootpath)

Aplikacje sieci Web implementują interfejs `IWebHostEnvironment`, który dziedziczy `IHostEnvironment` i dodaje [WebRootPath](#webroot).

## <a name="host-configuration"></a>Konfiguracja hosta

Konfiguracja hosta jest używana we właściwościach implementacji <xref:Microsoft.Extensions.Hosting.IHostEnvironment>.

Konfiguracja hosta jest dostępna z [HostBuilderContext. Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration) w <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>. Po `ConfigureAppConfiguration``HostBuilderContext.Configuration` jest zastępowana konfiguracją aplikacji.

Aby dodać konfigurację hosta, wywołaj <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> w `IHostBuilder`. `ConfigureHostConfiguration` może być wywoływana wiele razy z wynikami. Host używa dowolnej opcji ustawia wartość ostatnią dla danego klucza.

Dostawca zmiennych środowiskowych z prefiksami `DOTNET_` i argumenty wiersza polecenia są uwzględniane przez `CreateDefaultBuilder`. W przypadku aplikacji sieci Web zostanie dodany dostawca zmiennych środowiskowych z prefiksem `ASPNETCORE_`. Prefiks jest usuwany, gdy są odczytywane zmienne środowiskowe. Na przykład wartość zmiennej środowiskowej dla `ASPNETCORE_ENVIRONMENT` ma wartość Konfiguracja hosta dla klucza `environment`.

Poniższy przykład umożliwia utworzenie konfiguracji hosta:

[!code-csharp[](generic-host/samples-snapshot/3.x/Program.cs?name=snippet_HostConfig)]

## <a name="app-configuration"></a>Konfiguracja aplikacji

Konfiguracja aplikacji jest tworzona przez wywołanie <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> w `IHostBuilder`. `ConfigureAppConfiguration` może być wywoływana wiele razy z wynikami. Aplikacja używa dowolnej opcji ustawia wartość ostatnią dla danego klucza. 

Konfiguracja utworzona przez `ConfigureAppConfiguration` jest dostępna pod adresem [HostBuilderContext. Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) dla kolejnych operacji i jako usługa z elementu di. Konfiguracja hosta jest również dodawana do konfiguracji aplikacji.

Aby uzyskać więcej informacji, zobacz [Konfiguracja w ASP.NET Core](xref:fundamentals/configuration/index#configureappconfiguration).

## <a name="settings-for-all-app-types"></a>Ustawienia dla wszystkich typów aplikacji

Ta sekcja zawiera listę ustawień hosta, które dotyczą zarówno obciążeń HTTP, jak i innych niż HTTP. Domyślnie zmienne środowiskowe używane do konfigurowania tych ustawień mogą mieć prefiks `DOTNET_` lub `ASPNETCORE_`.

<!-- In the following sections, two spaces at end of line are used to force line breaks in the rendered page. -->

### <a name="applicationname"></a>ApplicationName

Właściwość [IHostEnvironment. ApplicationName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ApplicationName*) jest ustawiana na podstawie konfiguracji hosta podczas konstruowania hosta.

**Klucz**: `applicationName`  
**Typ**: `string`  
**Wartość domyślna**: Nazwa zestawu, który zawiera punkt wejścia aplikacji.  
**Zmienna środowiskowa**: `<PREFIX_>APPLICATIONNAME`

Aby ustawić tę wartość, należy użyć zmiennej środowiskowej. 

### <a name="contentrootpath"></a>ContentRootPath

Właściwość [IHostEnvironment. ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath*) określa, gdzie host rozpoczyna wyszukiwanie plików zawartości. Jeśli ścieżka nie istnieje, uruchomienie hosta nie powiedzie się.

**Klucz**: `contentRoot`  
**Typ**: `string`  
**Domyślnie**: folder, w którym znajduje się zestaw aplikacji.  
**Zmienna środowiskowa**: `<PREFIX_>CONTENTROOT`

Aby ustawić tę wartość, użyj zmiennej środowiskowej lub wywołaj `UseContentRoot` na `IHostBuilder`:

```csharp
Host.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\content-root")
    //...
```

Aby uzyskać więcej informacji, zobacz:

* [Podstawy: zawartość główna](xref:fundamentals/index#content-root)
* [WebRoot](#webroot)

### <a name="environmentname"></a>EnvironmentName

Dla właściwości [IHostEnvironment. EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.EnvironmentName*) można ustawić dowolną wartość. Wartości zdefiniowane przez platformę obejmują `Development`, `Staging`i `Production`. W wartościach nie jest rozróżniana wielkość liter.

**Klucz**: `environment`  
**Typ**: `string`  
**Wartość domyślna**: `Production`  
**Zmienna środowiskowa**: `<PREFIX_>ENVIRONMENT`

Aby ustawić tę wartość, użyj zmiennej środowiskowej lub wywołaj `UseEnvironment` na `IHostBuilder`:

```csharp
Host.CreateDefaultBuilder(args)
    .UseEnvironment("Development")
    //...
```

### <a name="shutdowntimeout"></a>ShutdownTimeout

[HostOptions. shutdownTimeout](xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*) ustawia limit czasu dla <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>. Wartość domyślna to pięć sekund.  Podczas okresu przekroczenia limitu czasu Host:

* Wyzwala [IHostApplicationLifetime. ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.ihostapplicationlifetime.applicationstopping).
* Próbuje zatrzymać usługi hostowane, rejestrowanie błędów dla usług, których zatrzymanie nie powiodło się.

Jeśli limit czasu upłynie przed zatrzymaniem wszystkich usług hostowanych, wszystkie pozostałe aktywne usługi zostaną zatrzymane po zamknięciu aplikacji. Usługi są zatrzymane nawet wtedy, gdy nie zakończyły przetwarzania. Jeśli usługi wymagają dodatkowego czasu na zatrzymanie, zwiększ limit czasu.

**Klucz**: `shutdownTimeoutSeconds`  
**Typ**: `int`  
**Wartość domyślna**: 5 sekund  
**Zmienna środowiskowa**: `<PREFIX_>SHUTDOWNTIMEOUTSECONDS`

Aby ustawić tę wartość, użyj zmiennej środowiskowej lub skonfiguruj `HostOptions`. Poniższy przykład ustawia limit czasu na 20 sekund:

[!code-csharp[](generic-host/samples-snapshot/3.x/Program.cs?name=snippet_HostOptions)]

### <a name="disable-app-configuration-reload-on-change"></a>Wyłącz ponowne ładowanie konfiguracji aplikacji przy zmianie

[Domyślnie](xref:fundamentals/configuration/index#default)plik *appSettings. JSON* i *appSettings. { Środowisko}. kod JSON* jest ponownie ładowany, gdy plik ulegnie zmianie. Aby wyłączyć to zachowanie ponownego ładowania w ASP.NET Core 5,0 w wersji zapoznawczej 3 lub nowszej, Ustaw klucz `hostBuilder:reloadConfigOnChange` na `false`.

**Klucz**: `hostBuilder:reloadConfigOnChange`  
**Typ**: `bool` (`true` lub `1`)  
**Wartość domyślna**: `true`  
**Argument wiersza polecenia**: `hostBuilder:reloadConfigOnChange`  
**Zmienna środowiskowa**: `<PREFIX_>hostBuilder:reloadConfigOnChange`

> [!WARNING]
> Separator dwukropek (`:`) nie działa ze zmiennymi kluczy hierarchicznych na wszystkich platformach. Aby uzyskać więcej informacji, zobacz [zmienne środowiskowe](xref:fundamentals/configuration/index#environment-variables).

## <a name="settings-for-web-apps"></a>Ustawienia usługi Web Apps

Niektóre ustawienia hosta mają zastosowanie tylko do obciążeń protokołu HTTP. Domyślnie zmienne środowiskowe używane do konfigurowania tych ustawień mogą mieć prefiks `DOTNET_` lub `ASPNETCORE_`.

Metody rozszerzające w `IWebHostBuilder` są dostępne dla tych ustawień. Przykłady kodu, które pokazują, jak wywoływać metody rozszerzenia Załóżmy, że `webBuilder` jest wystąpieniem `IWebHostBuilder`, jak w poniższym przykładzie:

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.CaptureStartupErrors(true);
            webBuilder.UseStartup<Startup>();
        });
```

### <a name="capturestartuperrors"></a>CaptureStartupErrors

Gdy `false`, błędy podczas uruchamiania, kończenie działania hosta. Gdy `true`, Host przechwytuje wyjątki podczas uruchamiania i próbuje uruchomić serwer.

**Klucz**: `captureStartupErrors`  
**Typ**: `bool` (`true` lub `1`)  
**Domyślnie**: wartość domyślna to `false`, chyba że aplikacja zostanie uruchomiona z Kestrel za pomocą usług IIS, gdzie wartość domyślna to `true`.  
**Zmienna środowiskowa**: `<PREFIX_>CAPTURESTARTUPERRORS`

Aby ustawić tę wartość, użyj konfiguracji lub `CaptureStartupErrors`wywołań:

```csharp
webBuilder.CaptureStartupErrors(true);
```

### <a name="detailederrors"></a>DetailedErrors

Po włączeniu lub gdy środowisko jest `Development`, aplikacja przechwytuje szczegółowe błędy.

**Klucz**: `detailedErrors`  
**Typ**: `bool` (`true` lub `1`)  
**Wartość domyślna**: `false`  
**Zmienna środowiskowa**: `<PREFIX_>_DETAILEDERRORS`

Aby ustawić tę wartość, użyj konfiguracji lub `UseSetting`wywołań:

```csharp
webBuilder.UseSetting(WebHostDefaults.DetailedErrorsKey, "true");
```

### <a name="hostingstartupassemblies"></a>HostingStartupAssemblies

Rozdzielany średnikami ciąg początkowych zestawów startowych do załadowania podczas uruchamiania. Mimo że wartość konfiguracji jest domyślnie pustym ciągiem, zestaw startowy obsługujący zawsze zawiera zestaw aplikacji. W przypadku udostępniania zestawów startowych są one dodawane do zestawu aplikacji do załadowania, gdy aplikacja kompiluje swoje popularne usługi podczas uruchamiania.

**Klucz**: `hostingStartupAssemblies`  
**Typ**: `string`  
**Wartość domyślna**: pusty ciąg  
**Zmienna środowiskowa**: `<PREFIX_>_HOSTINGSTARTUPASSEMBLIES`

Aby ustawić tę wartość, użyj konfiguracji lub `UseSetting`wywołań:

```csharp
webBuilder.UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2");
```

### <a name="hostingstartupexcludeassemblies"></a>HostingStartupExcludeAssemblies

Rozdzielany średnikami ciąg początkowych zestawów uruchamiania, który ma zostać wykluczony podczas uruchamiania.

**Klucz**: `hostingStartupExcludeAssemblies`  
**Typ**: `string`  
**Wartość domyślna**: pusty ciąg  
**Zmienna środowiskowa**: `<PREFIX_>_HOSTINGSTARTUPEXCLUDEASSEMBLIES`

Aby ustawić tę wartość, użyj konfiguracji lub `UseSetting`wywołań:

```csharp
webBuilder.UseSetting(WebHostDefaults.HostingStartupExcludeAssembliesKey, "assembly1;assembly2");
```

### <a name="https_port"></a>HTTPS_Port

Port przekierowania protokołu HTTPS. Używany do [wymuszania protokołu HTTPS](xref:security/enforcing-ssl).

**Klucz**: `https_port`  
**Typ**: `string`  
Wartość **Domyślna**: nie ustawiono wartości domyślnej.  
**Zmienna środowiskowa**: `<PREFIX_>HTTPS_PORT`

Aby ustawić tę wartość, użyj konfiguracji lub `UseSetting`wywołań:

```csharp
webBuilder.UseSetting("https_port", "8080");
```

### <a name="preferhostingurls"></a>PreferHostingUrls

Wskazuje, czy host powinien nasłuchiwać adresów URL skonfigurowanych przy użyciu `IWebHostBuilder` zamiast adresów URL skonfigurowanych przy użyciu implementacji `IServer`.

**Klucz**: `preferHostingUrls`  
**Typ**: `bool` (`true` lub `1`)  
**Wartość domyślna**: `true`  
**Zmienna środowiskowa**: `<PREFIX_>_PREFERHOSTINGURLS`

Aby ustawić tę wartość, użyj zmiennej środowiskowej lub `PreferHostingUrls`wywołań:

```csharp
webBuilder.PreferHostingUrls(false);
```

### <a name="preventhostingstartup"></a>PreventHostingStartup

Zapobiega automatycznemu ładowaniu zestawów startowych hostingu, w tym hostingu zestawów startowych skonfigurowanych przez zestaw aplikacji. Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/platform-specific-configuration>.

**Klucz**: `preventHostingStartup`  
**Typ**: `bool` (`true` lub `1`)  
**Wartość domyślna**: `false`  
**Zmienna środowiskowa**: `<PREFIX_>_PREVENTHOSTINGSTARTUP`

Aby ustawić tę wartość, użyj zmiennej środowiskowej lub `UseSetting` wywołań:

```csharp
webBuilder.UseSetting(WebHostDefaults.PreventHostingStartupKey, "true");
```

### <a name="startupassembly"></a>StartupAssembly

Zestaw do wyszukiwania klasy `Startup`.

**Klucz**: `startupAssembly`  
**Typ**: `string`  
**Domyślnie**: zestaw aplikacji  
**Zmienna środowiskowa**: `<PREFIX_>STARTUPASSEMBLY`

Aby ustawić tę wartość, użyj zmiennej środowiskowej lub `UseStartup`wywołań. `UseStartup` może przyjmować nazwę zestawu (`string`) lub typ (`TStartup`). Jeśli wywoływana jest wiele metod `UseStartup`, pierwszeństwo ma Ostatnia.

```csharp
webBuilder.UseStartup("StartupAssemblyName");
```

```csharp
webBuilder.UseStartup<Startup>();
```

### <a name="urls"></a>Adresy URL

Rozdzielana średnikami lista adresów IP lub adresów hostów z portami i protokołami, na których serwer powinien nasłuchiwać żądań. Na przykład `http://localhost:123`. Użyj "\*", aby wskazać, że serwer powinien nasłuchiwać żądań na dowolnym adresie IP lub nazwie hosta przy użyciu określonego portu i protokołu (na przykład `http://*:5000`). Protokół (`http://` lub `https://`) musi być dołączony do każdego adresu URL. Obsługiwane formaty różnią się między serwerami.

**Klucz**: `urls`  
**Typ**: `string`  
**Wartość domyślna**: `http://localhost:5000` i `https://localhost:5001`  
**Zmienna środowiskowa**: `<PREFIX_>URLS`

Aby ustawić tę wartość, użyj zmiennej środowiskowej lub `UseUrls`wywołań:

```csharp
webBuilder.UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002");
```

Kestrel ma własny interfejs API konfiguracji punktu końcowego. Aby uzyskać więcej informacji, zobacz <xref:fundamentals/servers/kestrel#endpoint-configuration>.

### <a name="webroot"></a>WebRoot

Ścieżka względna do statycznych zasobów aplikacji.

**Klucz**: `webroot`  
**Typ**: `string`  
**Wartość domyślna**: wartość domyślna to `wwwroot`. Ścieżka do *elementu {content root}/wwwroot* musi istnieć. Jeśli ścieżka nie istnieje, jest używany dostawca plików No-op.  
**Zmienna środowiskowa**: `<PREFIX_>WEBROOT`

Aby ustawić tę wartość, użyj zmiennej środowiskowej lub `UseWebRoot`wywołań:

```csharp
webBuilder.UseWebRoot("public");
```

Aby uzyskać więcej informacji, zobacz:

* [Podstawy: katalog główny sieci Web](xref:fundamentals/index#web-root)
* [ContentRootPath](#contentrootpath)

## <a name="manage-the-host-lifetime"></a>Zarządzanie okresem istnienia hosta

Wywołaj metody na skompilowanej implementacji <xref:Microsoft.Extensions.Hosting.IHost>, aby uruchomić i zatrzymać aplikację. Te metody wpływają na wszystkie implementacje <xref:Microsoft.Extensions.Hosting.IHostedService>, które są zarejestrowane w kontenerze usługi.

### <a name="run"></a>Run

<xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> uruchamia aplikację i blokuje wątek wywołujący do momentu wyłączenia hosta.

### <a name="runasync"></a>RunAsync

<xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> uruchamia aplikację i zwraca <xref:System.Threading.Tasks.Task>, która kończy się w momencie wyzwolenia tokenu anulowania lub zamknięcia.

### <a name="runconsoleasync"></a>RunConsoleAsync

<xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> umożliwia obsługę konsoli, kompiluje i uruchamia hosta oraz czeka na zamknięcie programu <kbd>Ctrl</kbd>+<kbd>C</kbd>/SIGINT lub SIGTERM.

### <a name="start"></a>Uruchamianie

<xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> uruchamia hosta synchronicznie.

### <a name="startasync"></a>StartAsync

<xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> uruchamia hosta i zwraca <xref:System.Threading.Tasks.Task>, który kończy się w momencie wyzwolenia tokenu anulowania lub zamknięcia. 

<xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> jest wywoływana na początku `StartAsync`, który czeka na zakończenie przed kontynuowaniem. Może to służyć do opóźnienia uruchamiania do momentu zasygnalizowania zdarzenia zewnętrznego.

### <a name="stopasync"></a>StopAsync

<xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> próbuje zatrzymać hosta w ramach podanego limitu czasu.

### <a name="waitforshutdown"></a>WaitForShutdown

<xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> blokuje wątek wywołujący do momentu wyzwolenia przez IHostLifetime, na przykład za pośrednictwem <kbd>kombinacji klawiszy Ctrl</kbd>+<kbd>C</kbd>/SIGINT lub SIGTERM.

### <a name="waitforshutdownasync"></a>WaitForShutdownAsync

<xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> zwraca <xref:System.Threading.Tasks.Task>, który kończy się po wyzwoleniu zamknięcia za pośrednictwem danego tokenu i wywołań <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.

### <a name="external-control"></a>Zewnętrzna kontrola

Bezpośrednią kontrolę nad okresem istnienia hosta można uzyskać przy użyciu metod, które mogą być wywoływane zewnętrznie:

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

::: moniker-end

::: moniker range=">= aspnetcore-3.0 <= aspnetcore-3.1"

W tym artykule przedstawiono hosta ogólnego platformy .NET Core (<xref:Microsoft.Extensions.Hosting.HostBuilder>) i przedstawiono wskazówki dotyczące korzystania z niego.

## <a name="whats-a-host"></a>Co to jest host?

*Host* to obiekt, który hermetyzuje zasoby aplikacji, takie jak:

* Iniekcja zależności (DI)
* Rejestrowanie
* Konfiguracja
* implementacje `IHostedService`

Po uruchomieniu hosta wywołuje `IHostedService.StartAsync` przy każdej implementacji <xref:Microsoft.Extensions.Hosting.IHostedService>, która znajduje się w kontenerze DI. W aplikacji sieci Web jedna z implementacji `IHostedService` jest usługą sieci Web, która uruchamia [implementację serwera http](xref:fundamentals/index#servers).

Główną przyczyną uwzględnienia wszystkich zasobów zależnych od aplikacji w jednym obiekcie jest zarządzanie okresem istnienia: Kontrola uruchamiania aplikacji i bezpieczne zamykanie.

W wersjach ASP.NET Core wcześniejszych niż 3,0 [host sieci Web](xref:fundamentals/host/web-host) jest używany do obciążeń http. Host sieci Web nie jest już zalecany dla aplikacji sieci Web i pozostaje dostępny tylko w celu zapewnienia zgodności z poprzednimi wersjami.

## <a name="set-up-a-host"></a>Konfigurowanie hosta

Host jest zazwyczaj konfigurowany, zbudowany i uruchamiany przez kod w klasie `Program`. Metoda `Main`:

* Wywołuje metodę `CreateHostBuilder`, aby utworzyć i skonfigurować obiekt konstruktora.
* Wywołuje metody `Build` i `Run` w obiekcie konstruktora.

Oto kod *program.cs* dla obciążenia innego niż HTTP z jedną implementacją `IHostedService` dodaną do kontenera di. 

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateHostBuilder(args).Build().Run();
    }

    public static IHostBuilder CreateHostBuilder(string[] args) =>
        Host.CreateDefaultBuilder(args)
            .ConfigureServices((hostContext, services) =>
            {
               services.AddHostedService<Worker>();
            });
}
```

W przypadku obciążeń HTTP Metoda `Main` jest taka sama, ale `CreateHostBuilder` wywołuje `ConfigureWebHostDefaults`:

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.UseStartup<Startup>();
        });
```

Jeśli aplikacja używa Entity Framework Core, nie zmieniaj nazwy ani podpisu metody `CreateHostBuilder`. [Narzędzia Entity Framework Core](/ef/core/miscellaneous/cli/) oczekują na znalezienie `CreateHostBuilder`j metody, która konfiguruje hosta bez uruchamiania aplikacji. Aby uzyskać więcej informacji, zobacz [Tworzenie DbContext w czasie projektowania](/ef/core/miscellaneous/cli/dbcontext-creation).

## <a name="default-builder-settings"></a>Ustawienia domyślnego konstruktora

Metoda <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*>:

* Ustawia [katalog główny zawartości](xref:fundamentals/index#content-root) na ścieżkę zwracaną przez <xref:System.IO.Directory.GetCurrentDirectory*>.
* Ładuje konfigurację hosta z:
  * Zmienne środowiskowe poprzedzone prefiksem `DOTNET_`.
  * Argumenty wiersza polecenia.
* Ładuje konfigurację aplikacji z:
  * *appSettings. JSON*.
  * *appSettings. {Environment}. JSON*.
  * [Secret Manager](xref:security/app-secrets) , gdy aplikacja jest uruchamiana w środowisku `Development`owym.
  * Zmienne środowiskowe.
  * Argumenty wiersza polecenia.
* Dodaje następujących dostawców [rejestrowania](xref:fundamentals/logging/index) :
  * Konsola
  * Debugowanie
  * EventSource
  * EventLog (tylko w przypadku uruchamiania w systemie Windows)
* Umożliwia [weryfikację zakresu](xref:fundamentals/dependency-injection#scope-validation) i [Sprawdzanie poprawności zależności](xref:Microsoft.Extensions.DependencyInjection.ServiceProviderOptions.ValidateOnBuild) , gdy środowisko jest opracowywane.

Metoda `ConfigureWebHostDefaults`:

* Ładuje konfigurację hosta ze zmiennych środowiskowych z prefiksem `ASPNETCORE_`.
* Ustawia serwer [Kestrel](xref:fundamentals/servers/kestrel) jako serwer sieci Web i konfiguruje go przy użyciu dostawców konfiguracji hostingu aplikacji. Aby poznać domyślne opcje serwera Kestrel, zobacz <xref:fundamentals/servers/kestrel#kestrel-options>.
* Dodaje [oprogramowanie pośredniczące do filtrowania hosta](xref:fundamentals/servers/kestrel#host-filtering).
* Dodaje [przekazane nagłówki pośredniczące](xref:host-and-deploy/proxy-load-balancer#forwarded-headers) , jeśli `ASPNETCORE_FORWARDEDHEADERS_ENABLED` jest równe `true`.
* Włącza integrację usług IIS. Aby poznać domyślne opcje usług IIS, zobacz <xref:host-and-deploy/iis/index#iis-options>.

[Ustawienia dla wszystkich typów aplikacji](#settings-for-all-app-types) i [Ustawienia dla usługi Web Apps](#settings-for-web-apps) w dalszej części tego artykułu pokazują, jak zastąpić ustawienia domyślnego konstruktora.

## <a name="framework-provided-services"></a>Usługi udostępniane przez platformę

Następujące usługi są rejestrowane automatycznie:

* [IHostApplicationLifetime](#ihostapplicationlifetime)
* [IHostLifetime](#ihostlifetime)
* [IHostEnvironment / IWebHostEnvironment](#ihostenvironment)

Aby uzyskać więcej informacji na temat usług udostępnianych przez platformę, zobacz <xref:fundamentals/dependency-injection#framework-provided-services>.

## <a name="ihostapplicationlifetime"></a>IHostApplicationLifetime

Wstrzyknąć usługę <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime> (dawniej `IApplicationLifetime`) do dowolnej klasy w celu obsługi zadań po uruchomieniu i bezpiecznego zamykania. Trzy właściwości interfejsu to tokeny anulowania używane do rejestrowania metod obsługi zdarzeń uruchamiania i zatrzymywania aplikacji. Interfejs zawiera również metodę `StopApplication`.

Poniższy przykład jest implementacją `IHostedService`, która rejestruje zdarzenia `IHostApplicationLifetime`:

[!code-csharp[](generic-host/samples-snapshot/3.x/LifetimeEventsHostedService.cs?name=snippet_LifetimeEvents)]

## <a name="ihostlifetime"></a>IHostLifetime

Implementacja <xref:Microsoft.Extensions.Hosting.IHostLifetime> kontroluje, gdy host zostanie uruchomiony i gdy zostanie zatrzymana. Używana jest Ostatnia zarejestrowana implementacja.

`Microsoft.Extensions.Hosting.Internal.ConsoleLifetime` jest domyślną implementacją `IHostLifetime`. `ConsoleLifetime`:

* Nasłuchuje <kbd>kombinacji klawiszy Ctrl</kbd>+<kbd>C</kbd>/SIGINT lub SIGTERM i wywołuje <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime.StopApplication*>, aby rozpocząć proces zamykania.
* Odblokowuje rozszerzenia, takie jak [RunAsync](#runasync) i [WaitForShutdownAsync](#waitforshutdownasync).

## <a name="ihostenvironment"></a>IHostEnvironment

Wsuń usługę <xref:Microsoft.Extensions.Hosting.IHostEnvironment> do klasy, aby uzyskać informacje o następujących ustawieniach:

* [ApplicationName](#applicationname)
* [EnvironmentName](#environmentname)
* [ContentRootPath](#contentrootpath)

Aplikacje sieci Web implementują interfejs `IWebHostEnvironment`, który dziedziczy `IHostEnvironment` i dodaje [WebRootPath](#webroot).

## <a name="host-configuration"></a>Konfiguracja hosta

Konfiguracja hosta jest używana we właściwościach implementacji <xref:Microsoft.Extensions.Hosting.IHostEnvironment>.

Konfiguracja hosta jest dostępna z [HostBuilderContext. Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration) w <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>. Po `ConfigureAppConfiguration``HostBuilderContext.Configuration` jest zastępowana konfiguracją aplikacji.

Aby dodać konfigurację hosta, wywołaj <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> w `IHostBuilder`. `ConfigureHostConfiguration` może być wywoływana wiele razy z wynikami. Host używa dowolnej opcji ustawia wartość ostatnią dla danego klucza.

Dostawca zmiennych środowiskowych z prefiksami `DOTNET_` i argumenty wiersza polecenia są uwzględniane przez `CreateDefaultBuilder`. W przypadku aplikacji sieci Web zostanie dodany dostawca zmiennych środowiskowych z prefiksem `ASPNETCORE_`. Prefiks jest usuwany, gdy są odczytywane zmienne środowiskowe. Na przykład wartość zmiennej środowiskowej dla `ASPNETCORE_ENVIRONMENT` ma wartość Konfiguracja hosta dla klucza `environment`.

Poniższy przykład umożliwia utworzenie konfiguracji hosta:

[!code-csharp[](generic-host/samples-snapshot/3.x/Program.cs?name=snippet_HostConfig)]

## <a name="app-configuration"></a>Konfiguracja aplikacji

Konfiguracja aplikacji jest tworzona przez wywołanie <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> w `IHostBuilder`. `ConfigureAppConfiguration` może być wywoływana wiele razy z wynikami. Aplikacja używa dowolnej opcji ustawia wartość ostatnią dla danego klucza. 

Konfiguracja utworzona przez `ConfigureAppConfiguration` jest dostępna pod adresem [HostBuilderContext. Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) dla kolejnych operacji i jako usługa z elementu di. Konfiguracja hosta jest również dodawana do konfiguracji aplikacji.

Aby uzyskać więcej informacji, zobacz [Konfiguracja w ASP.NET Core](xref:fundamentals/configuration/index#configureappconfiguration).

## <a name="settings-for-all-app-types"></a>Ustawienia dla wszystkich typów aplikacji

Ta sekcja zawiera listę ustawień hosta, które dotyczą zarówno obciążeń HTTP, jak i innych niż HTTP. Domyślnie zmienne środowiskowe używane do konfigurowania tych ustawień mogą mieć prefiks `DOTNET_` lub `ASPNETCORE_`.

<!-- In the following sections, two spaces at end of line are used to force line breaks in the rendered page. -->

### <a name="applicationname"></a>ApplicationName

Właściwość [IHostEnvironment. ApplicationName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ApplicationName*) jest ustawiana na podstawie konfiguracji hosta podczas konstruowania hosta.

**Klucz**: `applicationName`  
**Typ**: `string`  
**Wartość domyślna**: Nazwa zestawu, który zawiera punkt wejścia aplikacji.  
**Zmienna środowiskowa**: `<PREFIX_>APPLICATIONNAME`

Aby ustawić tę wartość, należy użyć zmiennej środowiskowej. 

### <a name="contentrootpath"></a>ContentRootPath

Właściwość [IHostEnvironment. ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath*) określa, gdzie host rozpoczyna wyszukiwanie plików zawartości. Jeśli ścieżka nie istnieje, uruchomienie hosta nie powiedzie się.

**Klucz**: `contentRoot`  
**Typ**: `string`  
**Domyślnie**: folder, w którym znajduje się zestaw aplikacji.  
**Zmienna środowiskowa**: `<PREFIX_>CONTENTROOT`

Aby ustawić tę wartość, użyj zmiennej środowiskowej lub wywołaj `UseContentRoot` na `IHostBuilder`:

```csharp
Host.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\content-root")
    //...
```

Aby uzyskać więcej informacji, zobacz:

* [Podstawy: zawartość główna](xref:fundamentals/index#content-root)
* [WebRoot](#webroot)

### <a name="environmentname"></a>EnvironmentName

Dla właściwości [IHostEnvironment. EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.EnvironmentName*) można ustawić dowolną wartość. Wartości zdefiniowane przez platformę obejmują `Development`, `Staging`i `Production`. W wartościach nie jest rozróżniana wielkość liter.

**Klucz**: `environment`  
**Typ**: `string`  
**Wartość domyślna**: `Production`  
**Zmienna środowiskowa**: `<PREFIX_>ENVIRONMENT`

Aby ustawić tę wartość, użyj zmiennej środowiskowej lub wywołaj `UseEnvironment` na `IHostBuilder`:

```csharp
Host.CreateDefaultBuilder(args)
    .UseEnvironment("Development")
    //...
```

### <a name="shutdowntimeout"></a>ShutdownTimeout

[HostOptions. shutdownTimeout](xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*) ustawia limit czasu dla <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>. Wartość domyślna to pięć sekund.  Podczas okresu przekroczenia limitu czasu Host:

* Wyzwala [IHostApplicationLifetime. ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.ihostapplicationlifetime.applicationstopping).
* Próbuje zatrzymać usługi hostowane, rejestrowanie błędów dla usług, których zatrzymanie nie powiodło się.

Jeśli limit czasu upłynie przed zatrzymaniem wszystkich usług hostowanych, wszystkie pozostałe aktywne usługi zostaną zatrzymane po zamknięciu aplikacji. Usługi są zatrzymane nawet wtedy, gdy nie zakończyły przetwarzania. Jeśli usługi wymagają dodatkowego czasu na zatrzymanie, zwiększ limit czasu.

**Klucz**: `shutdownTimeoutSeconds`  
**Typ**: `int`  
**Wartość domyślna**: 5 sekund  
**Zmienna środowiskowa**: `<PREFIX_>SHUTDOWNTIMEOUTSECONDS`

Aby ustawić tę wartość, użyj zmiennej środowiskowej lub skonfiguruj `HostOptions`. Poniższy przykład ustawia limit czasu na 20 sekund:

[!code-csharp[](generic-host/samples-snapshot/3.x/Program.cs?name=snippet_HostOptions)]

## <a name="settings-for-web-apps"></a>Ustawienia usługi Web Apps

Niektóre ustawienia hosta mają zastosowanie tylko do obciążeń protokołu HTTP. Domyślnie zmienne środowiskowe używane do konfigurowania tych ustawień mogą mieć prefiks `DOTNET_` lub `ASPNETCORE_`.

Metody rozszerzające w `IWebHostBuilder` są dostępne dla tych ustawień. Przykłady kodu, które pokazują, jak wywoływać metody rozszerzenia Załóżmy, że `webBuilder` jest wystąpieniem `IWebHostBuilder`, jak w poniższym przykładzie:

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.CaptureStartupErrors(true);
            webBuilder.UseStartup<Startup>();
        });
```

### <a name="capturestartuperrors"></a>CaptureStartupErrors

Gdy `false`, błędy podczas uruchamiania, kończenie działania hosta. Gdy `true`, Host przechwytuje wyjątki podczas uruchamiania i próbuje uruchomić serwer.

**Klucz**: `captureStartupErrors`  
**Typ**: `bool` (`true` lub `1`)  
**Domyślnie**: wartość domyślna to `false`, chyba że aplikacja zostanie uruchomiona z Kestrel za pomocą usług IIS, gdzie wartość domyślna to `true`.  
**Zmienna środowiskowa**: `<PREFIX_>CAPTURESTARTUPERRORS`

Aby ustawić tę wartość, użyj konfiguracji lub `CaptureStartupErrors`wywołań:

```csharp
webBuilder.CaptureStartupErrors(true);
```

### <a name="detailederrors"></a>DetailedErrors

Po włączeniu lub gdy środowisko jest `Development`, aplikacja przechwytuje szczegółowe błędy.

**Klucz**: `detailedErrors`  
**Typ**: `bool` (`true` lub `1`)  
**Wartość domyślna**: `false`  
**Zmienna środowiskowa**: `<PREFIX_>_DETAILEDERRORS`

Aby ustawić tę wartość, użyj konfiguracji lub `UseSetting`wywołań:

```csharp
webBuilder.UseSetting(WebHostDefaults.DetailedErrorsKey, "true");
```

### <a name="hostingstartupassemblies"></a>HostingStartupAssemblies

Rozdzielany średnikami ciąg początkowych zestawów startowych do załadowania podczas uruchamiania. Mimo że wartość konfiguracji jest domyślnie pustym ciągiem, zestaw startowy obsługujący zawsze zawiera zestaw aplikacji. W przypadku udostępniania zestawów startowych są one dodawane do zestawu aplikacji do załadowania, gdy aplikacja kompiluje swoje popularne usługi podczas uruchamiania.

**Klucz**: `hostingStartupAssemblies`  
**Typ**: `string`  
**Wartość domyślna**: pusty ciąg  
**Zmienna środowiskowa**: `<PREFIX_>_HOSTINGSTARTUPASSEMBLIES`

Aby ustawić tę wartość, użyj konfiguracji lub `UseSetting`wywołań:

```csharp
webBuilder.UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2");
```

### <a name="hostingstartupexcludeassemblies"></a>HostingStartupExcludeAssemblies

Rozdzielany średnikami ciąg początkowych zestawów uruchamiania, który ma zostać wykluczony podczas uruchamiania.

**Klucz**: `hostingStartupExcludeAssemblies`  
**Typ**: `string`  
**Wartość domyślna**: pusty ciąg  
**Zmienna środowiskowa**: `<PREFIX_>_HOSTINGSTARTUPEXCLUDEASSEMBLIES`

Aby ustawić tę wartość, użyj konfiguracji lub `UseSetting`wywołań:

```csharp
webBuilder.UseSetting(WebHostDefaults.HostingStartupExcludeAssembliesKey, "assembly1;assembly2");
```

### <a name="https_port"></a>HTTPS_Port

Port przekierowania protokołu HTTPS. Używany do [wymuszania protokołu HTTPS](xref:security/enforcing-ssl).

**Klucz**: `https_port`  
**Typ**: `string`  
Wartość **Domyślna**: nie ustawiono wartości domyślnej.  
**Zmienna środowiskowa**: `<PREFIX_>HTTPS_PORT`

Aby ustawić tę wartość, użyj konfiguracji lub `UseSetting`wywołań:

```csharp
webBuilder.UseSetting("https_port", "8080");
```

### <a name="preferhostingurls"></a>PreferHostingUrls

Wskazuje, czy host powinien nasłuchiwać adresów URL skonfigurowanych przy użyciu `IWebHostBuilder` zamiast adresów URL skonfigurowanych przy użyciu implementacji `IServer`.

**Klucz**: `preferHostingUrls`  
**Typ**: `bool` (`true` lub `1`)  
**Wartość domyślna**: `true`  
**Zmienna środowiskowa**: `<PREFIX_>_PREFERHOSTINGURLS`

Aby ustawić tę wartość, użyj zmiennej środowiskowej lub `PreferHostingUrls`wywołań:

```csharp
webBuilder.PreferHostingUrls(false);
```

### <a name="preventhostingstartup"></a>PreventHostingStartup

Zapobiega automatycznemu ładowaniu zestawów startowych hostingu, w tym hostingu zestawów startowych skonfigurowanych przez zestaw aplikacji. Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/platform-specific-configuration>.

**Klucz**: `preventHostingStartup`  
**Typ**: `bool` (`true` lub `1`)  
**Wartość domyślna**: `false`  
**Zmienna środowiskowa**: `<PREFIX_>_PREVENTHOSTINGSTARTUP`

Aby ustawić tę wartość, użyj zmiennej środowiskowej lub `UseSetting` wywołań:

```csharp
webBuilder.UseSetting(WebHostDefaults.PreventHostingStartupKey, "true");
```

### <a name="startupassembly"></a>StartupAssembly

Zestaw do wyszukiwania klasy `Startup`.

**Klucz**: `startupAssembly`  
**Typ**: `string`  
**Domyślnie**: zestaw aplikacji  
**Zmienna środowiskowa**: `<PREFIX_>STARTUPASSEMBLY`

Aby ustawić tę wartość, użyj zmiennej środowiskowej lub `UseStartup`wywołań. `UseStartup` może przyjmować nazwę zestawu (`string`) lub typ (`TStartup`). Jeśli wywoływana jest wiele metod `UseStartup`, pierwszeństwo ma Ostatnia.

```csharp
webBuilder.UseStartup("StartupAssemblyName");
```

```csharp
webBuilder.UseStartup<Startup>();
```

### <a name="urls"></a>Adresy URL

Rozdzielana średnikami lista adresów IP lub adresów hostów z portami i protokołami, na których serwer powinien nasłuchiwać żądań. Na przykład `http://localhost:123`. Użyj "\*", aby wskazać, że serwer powinien nasłuchiwać żądań na dowolnym adresie IP lub nazwie hosta przy użyciu określonego portu i protokołu (na przykład `http://*:5000`). Protokół (`http://` lub `https://`) musi być dołączony do każdego adresu URL. Obsługiwane formaty różnią się między serwerami.

**Klucz**: `urls`  
**Typ**: `string`  
**Wartość domyślna**: `http://localhost:5000` i `https://localhost:5001`  
**Zmienna środowiskowa**: `<PREFIX_>URLS`

Aby ustawić tę wartość, użyj zmiennej środowiskowej lub `UseUrls`wywołań:

```csharp
webBuilder.UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002");
```

Kestrel ma własny interfejs API konfiguracji punktu końcowego. Aby uzyskać więcej informacji, zobacz <xref:fundamentals/servers/kestrel#endpoint-configuration>.

### <a name="webroot"></a>WebRoot

Ścieżka względna do statycznych zasobów aplikacji.

**Klucz**: `webroot`  
**Typ**: `string`  
**Wartość domyślna**: wartość domyślna to `wwwroot`. Ścieżka do *elementu {content root}/wwwroot* musi istnieć. Jeśli ścieżka nie istnieje, jest używany dostawca plików No-op.  
**Zmienna środowiskowa**: `<PREFIX_>WEBROOT`

Aby ustawić tę wartość, użyj zmiennej środowiskowej lub `UseWebRoot`wywołań:

```csharp
webBuilder.UseWebRoot("public");
```

Aby uzyskać więcej informacji, zobacz:

* [Podstawy: katalog główny sieci Web](xref:fundamentals/index#web-root)
* [ContentRootPath](#contentrootpath)

## <a name="manage-the-host-lifetime"></a>Zarządzanie okresem istnienia hosta

Wywołaj metody na skompilowanej implementacji <xref:Microsoft.Extensions.Hosting.IHost>, aby uruchomić i zatrzymać aplikację. Te metody wpływają na wszystkie implementacje <xref:Microsoft.Extensions.Hosting.IHostedService>, które są zarejestrowane w kontenerze usługi.

### <a name="run"></a>Run

<xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> uruchamia aplikację i blokuje wątek wywołujący do momentu wyłączenia hosta.

### <a name="runasync"></a>RunAsync

<xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> uruchamia aplikację i zwraca <xref:System.Threading.Tasks.Task>, która kończy się w momencie wyzwolenia tokenu anulowania lub zamknięcia.

### <a name="runconsoleasync"></a>RunConsoleAsync

<xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> umożliwia obsługę konsoli, kompiluje i uruchamia hosta oraz czeka na zamknięcie programu <kbd>Ctrl</kbd>+<kbd>C</kbd>/SIGINT lub SIGTERM.

### <a name="start"></a>Uruchamianie

<xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> uruchamia hosta synchronicznie.

### <a name="startasync"></a>StartAsync

<xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> uruchamia hosta i zwraca <xref:System.Threading.Tasks.Task>, który kończy się w momencie wyzwolenia tokenu anulowania lub zamknięcia. 

<xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> jest wywoływana na początku `StartAsync`, który czeka na zakończenie przed kontynuowaniem. Może to służyć do opóźnienia uruchamiania do momentu zasygnalizowania zdarzenia zewnętrznego.

### <a name="stopasync"></a>StopAsync

<xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> próbuje zatrzymać hosta w ramach podanego limitu czasu.

### <a name="waitforshutdown"></a>WaitForShutdown

<xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> blokuje wątek wywołujący do momentu wyzwolenia przez IHostLifetime, na przykład za pośrednictwem <kbd>kombinacji klawiszy Ctrl</kbd>+<kbd>C</kbd>/SIGINT lub SIGTERM.

### <a name="waitforshutdownasync"></a>WaitForShutdownAsync

<xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> zwraca <xref:System.Threading.Tasks.Task>, który kończy się po wyzwoleniu zamknięcia za pośrednictwem danego tokenu i wywołań <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.

### <a name="external-control"></a>Zewnętrzna kontrola

Bezpośrednią kontrolę nad okresem istnienia hosta można uzyskać przy użyciu metod, które mogą być wywoływane zewnętrznie:

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

::: moniker-end

::: moniker range="< aspnetcore-3.0"

ASP.NET Core aplikacje konfigurują i uruchamiają hosta. Host jest odpowiedzialny za zarządzanie uruchamiania i czasu życia aplikacji.

W tym artykule opisano ASP.NET Core hosta ogólnego (<xref:Microsoft.Extensions.Hosting.HostBuilder>), który jest używany w przypadku aplikacji, które nie przetwarzają żądań HTTP.

Przeznaczeniem hosta ogólnego jest oddzielenie potoku HTTP od interfejsu API hosta sieci Web w celu umożliwienia szerszej tablicy scenariuszy hostów. Obsługa komunikatów, zadań w tle i innych obciążeń innych niż HTTP na podstawie ogólnej korzyści z hosta, takich jak konfiguracja, iniekcja zależności (DI) i rejestrowanie.

Host ogólny jest nowy w ASP.NET Core 2,1 i nie jest odpowiedni dla scenariuszy hostingu w sieci Web. W przypadku scenariuszy hostingu w sieci Web należy użyć [hosta sieci Web](xref:fundamentals/host/web-host). Host ogólny zastąpi hosta sieci Web w przyszłej wersji i będzie pełnić rolę podstawowego interfejsu API hosta zarówno w scenariuszach HTTP, jak i innych niż HTTP.

[Wyświetl lub pobierz przykładowy kod](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([jak pobrać](xref:index#how-to-download-a-sample))

Podczas uruchamiania przykładowej aplikacji w [Visual Studio Code](https://code.visualstudio.com/)należy użyć *zewnętrznego lub zintegrowanego terminalu*. Nie uruchamiaj przykładu w `internalConsole`.

Aby ustawić konsolę w Visual Studio Code:

1. Otwórz plik *. programu vscode/Launch. JSON* .
1. W konfiguracji **uruchamiania programu .NET Core (konsoli)** zlokalizuj wpis **konsoli** . Ustaw wartość na `externalTerminal` lub `integratedTerminal`.

## <a name="introduction"></a>Wprowadzenie

Ogólna Biblioteka hostów jest dostępna w przestrzeni nazw <xref:Microsoft.Extensions.Hosting> i udostępniana przez pakiet [Microsoft. Extensions. hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/) . Pakiet `Microsoft.Extensions.Hosting` jest zawarty w [pakiecie Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app) (ASP.NET Core 2,1 lub nowszym).

<xref:Microsoft.Extensions.Hosting.IHostedService> jest punktem wejścia do wykonania kodu. Każda implementacja <xref:Microsoft.Extensions.Hosting.IHostedService> jest wykonywana w kolejności [rejestracji usługi w ConfigureServices](#configureservices). <xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*> jest wywoływana dla każdego <xref:Microsoft.Extensions.Hosting.IHostedService> podczas uruchamiania hosta, a <xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*> jest wywoływana w odwrotnej kolejności rejestracji, gdy host zostanie bezpiecznie zamknięty.

## <a name="set-up-a-host"></a>Konfigurowanie hosta

<xref:Microsoft.Extensions.Hosting.IHostBuilder> jest głównym składnikiem używanym przez biblioteki i aplikacje do inicjowania, kompilowania i uruchamiania hosta:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_HostBuilder)]

## <a name="options"></a>Opcje

<xref:Microsoft.Extensions.Hosting.HostOptions> skonfigurować opcje <xref:Microsoft.Extensions.Hosting.IHost>.

### <a name="shutdown-timeout"></a>Limit czasu zamykania

<xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> ustawia limit czasu dla <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>. Wartość domyślna to pięć sekund.

Następująca konfiguracja opcji w `Program.Main` powoduje zwiększenie domyślnego limitu czasu zamykania pięciu sekund:

```csharp
var host = new HostBuilder()
    .ConfigureServices((hostContext, services) =>
    {
        services.Configure<HostOptions>(option =>
        {
            option.ShutdownTimeout = System.TimeSpan.FromSeconds(20);
        });
    })
    .Build();
```

## <a name="default-services"></a>Usługi domyślne

Podczas inicjowania hosta zarejestrowano następujące usługi:

* [Środowisko](xref:fundamentals/environments) (<xref:Microsoft.Extensions.Hosting.IHostingEnvironment>)
* <xref:Microsoft.Extensions.Hosting.HostBuilderContext>
* [Konfiguracja](xref:fundamentals/configuration/index) (<xref:Microsoft.Extensions.Configuration.IConfiguration>)
* <xref:Microsoft.Extensions.Hosting.IApplicationLifetime> (`Microsoft.Extensions.Hosting.Internal.ApplicationLifetime`)
* <xref:Microsoft.Extensions.Hosting.IHostLifetime> (`Microsoft.Extensions.Hosting.Internal.ConsoleLifetime`)
* <xref:Microsoft.Extensions.Hosting.IHost>
* [Opcje](xref:fundamentals/configuration/options) (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.AddOptions*>)
* [Rejestrowanie](xref:fundamentals/logging/index) (<xref:Microsoft.Extensions.DependencyInjection.LoggingServiceCollectionExtensions.AddLogging*>)

## <a name="host-configuration"></a>Konfiguracja hosta

Konfiguracja hosta jest tworzona przez:

* Wywoływanie metod rozszerzających <xref:Microsoft.Extensions.Hosting.IHostBuilder> w celu ustawienia [katalogu głównego zawartości](#content-root) i [środowiska](#environment).
* Odczytywanie konfiguracji od dostawców konfiguracji w <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>.

### <a name="extension-methods"></a>Metody rozszerzające

### <a name="application-key-name"></a>Klucz aplikacji (nazwa)

Właściwość [IHostingEnvironment. ApplicationName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ApplicationName*) jest ustawiana na podstawie konfiguracji hosta podczas konstruowania hosta. Aby jawnie ustawić wartość, użyj [HostDefaults. ApplicationKey](xref:Microsoft.Extensions.Hosting.HostDefaults.ApplicationKey):

**Klucz**: `applicationName`  
**Typ**: `string`  
**Wartość domyślna**: Nazwa zestawu zawierającego punkt wejścia aplikacji.  
**Ustaw przy użyciu**: `HostBuilderContext.HostingEnvironment.ApplicationName`  
**Zmienna środowiskowa**: `<PREFIX_>APPLICATIONNAME` (`<PREFIX_>` jest [opcjonalny i zdefiniowany przez użytkownika](#configurehostconfiguration))

### <a name="content-root"></a>Katalog główny zawartości

To ustawienie określa, gdzie host rozpoczyna wyszukiwanie plików zawartości.

**Klucz**: `contentRoot`  
**Typ**: `string`  
**Domyślnie**: Domyślnie folder, w którym znajduje się zestaw aplikacji.  
**Ustaw przy użyciu**: `UseContentRoot`  
**Zmienna środowiskowa**: `<PREFIX_>CONTENTROOT` (`<PREFIX_>` jest [opcjonalny i zdefiniowany przez użytkownika](#configurehostconfiguration))

Jeśli ścieżka nie istnieje, uruchomienie hosta nie powiedzie się.

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseContentRoot)]

Aby uzyskać więcej informacji, zobacz temat [podstawy: zawartość główna](xref:fundamentals/index#content-root).

### <a name="environment"></a>Środowisko

Ustawia [środowisko](xref:fundamentals/environments)aplikacji.

**Klucz**: `environment`  
**Typ**: `string`  
**Wartość domyślna**: `Production`  
**Ustaw przy użyciu**: `UseEnvironment`  
**Zmienna środowiskowa**: `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` jest [opcjonalny i zdefiniowany przez użytkownika](#configurehostconfiguration))

Dla środowiska można ustawić dowolną wartość. Wartości zdefiniowane przez platformę obejmują `Development`, `Staging`i `Production`. W wartościach nie jest rozróżniana wielkość liter.

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseEnvironment)]

### <a name="configurehostconfiguration"></a>ConfigureHostConfiguration

<xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> używa <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> do tworzenia <xref:Microsoft.Extensions.Configuration.IConfiguration> dla hosta. Konfiguracja hosta służy do inicjowania <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> do użycia w procesie kompilacji aplikacji.

<xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> może być wywoływana wiele razy z wynikami. Host używa dowolnej opcji ustawia wartość ostatnią dla danego klucza.

Żaden dostawca nie jest domyślnie uwzględniany. Należy jawnie określić dostawców konfiguracji, których wymaga aplikacja w <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>, w tym:

* Konfiguracja pliku (na przykład z pliku *HostSettings. JSON* ).
* Konfiguracja zmiennej środowiskowej.
* Konfiguracja argumentu wiersza polecenia.
* Każdy inny wymagany dostawca konfiguracji.

Konfiguracja pliku hosta jest włączana przez określenie ścieżki podstawowej aplikacji z `SetBasePath`, a następnie wywołanie jednego z [dostawców konfiguracji plików](xref:fundamentals/configuration/index#file-configuration-provider). Przykładowa aplikacja używa pliku JSON, *HostSettings. JSON*i wywołuje <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*>, aby użyć ustawień konfiguracji hosta pliku.

Aby dodać [konfigurację zmiennej środowiskowej](xref:fundamentals/configuration/index#environment-variables-configuration-provider) hosta, wywołaj <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> na konstruktorze hosta. `AddEnvironmentVariables` akceptuje opcjonalnego prefiksu zdefiniowanego przez użytkownika. Przykładowa aplikacja używa prefiksu `PREFIX_`. Prefiks jest usuwany, gdy są odczytywane zmienne środowiskowe. Po skonfigurowaniu hosta przykładowej aplikacji wartość zmiennej środowiskowej dla `PREFIX_ENVIRONMENT` będzie wartością konfiguracji hosta dla klucza `environment`.

Podczas programowania podczas korzystania z [programu Visual Studio](https://visualstudio.microsoft.com) lub uruchamiania aplikacji z `dotnet run`, zmienne środowiskowe można ustawić w pliku *Properties/profilu launchsettings. JSON* . W [Visual Studio Code](https://code.visualstudio.com/)zmienne środowiskowe mogą być ustawiane w pliku *. programu vscode/Launch. JSON* podczas tworzenia. Aby uzyskać więcej informacji, zobacz <xref:fundamentals/environments>.

[Konfiguracja wiersza polecenia](xref:fundamentals/configuration/index#command-line-configuration-provider) jest dodawana przez wywołanie <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>. Konfiguracja wiersza polecenia jest dodawana jako Ostatnia, aby zezwolić na argumenty wiersza polecenia w celu przesłonięcia konfiguracji udostępnionej przez wcześniejszych dostawców konfiguracji.

*HostSettings. JSON*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/hostsettings.json)]

Dodatkową konfigurację można uzyskać za pomocą programu [ApplicationName](#application-key-name) i kluczy [contentRoot](#content-root) .

Przykład konfiguracji `HostBuilder` przy użyciu <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureHostConfiguration)]

## <a name="configureappconfiguration"></a>ConfigureAppConfiguration

Konfiguracja aplikacji jest tworzona przez wywołanie <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> w implementacji <xref:Microsoft.Extensions.Hosting.IHostBuilder>. <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> używa <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> do tworzenia <xref:Microsoft.Extensions.Configuration.IConfiguration> dla aplikacji. <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> może być wywoływana wiele razy z wynikami. Aplikacja używa dowolnej opcji ustawia wartość ostatnią dla danego klucza. Konfiguracja utworzona przez <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> jest dostępna pod adresem [HostBuilderContext. Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) dla kolejnych operacji i w <xref:Microsoft.Extensions.Hosting.IHost.Services*>.

Konfiguracja aplikacji automatycznie otrzymuje konfigurację hosta dostarczoną przez [ConfigureHostConfiguration](#configurehostconfiguration).

Przykładowa konfiguracja aplikacji przy użyciu <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureAppConfiguration)]

*appSettings. JSON*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.json)]

*appSettings. Plik Development. JSON*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Development.json)]

*appSettings. Production. JSON*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Production.json)]

Aby przenieść pliki ustawień do katalogu wyjściowego, określ pliki ustawień jako [elementy projektu MSBuild](/visualstudio/msbuild/common-msbuild-project-items) w pliku projektu. Aplikacja Przykładowa przenosi pliki ustawień aplikacji JSON i *HostSettings. JSON* z następującym `<Content>` elementem:

```xml
<ItemGroup>
  <Content Include="**\*.json" Exclude="bin\**\*;obj\**\*" 
      CopyToOutputDirectory="PreserveNewest" />
</ItemGroup>
```

> [!NOTE]
> Metody rozszerzenia konfiguracji, takie jak <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> i <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> wymagają dodatkowych pakietów NuGet, takich jak [Microsoft. Extensions. Configuration. JSON](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Json) i [Microsoft. Extensions. Configuration. EnvironmentVariables](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.EnvironmentVariables). Jeśli aplikacja nie używa [pakietu Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app), należy dodać te pakiety do projektu oprócz podstawowego pakietu [Microsoft. Extensions. Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration) . Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/index>.

## <a name="configureservices"></a>ConfigureServices

<xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> dodaje usługi do kontenera [iniekcji zależności](xref:fundamentals/dependency-injection) aplikacji. <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> może być wywoływana wiele razy z wynikami.

Usługa hostowana jest klasą z logiką zadań w tle, która implementuje interfejs <xref:Microsoft.Extensions.Hosting.IHostedService>. Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/hosted-services>.

[Przykładowa aplikacja](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) używa metody rozszerzenia `AddHostedService`, aby dodać usługę dla zdarzeń okresu istnienia, `LifetimeEventsHostedService`i zadania w tle z przekroczeniem czasu, `TimedHostedService`do aplikacji:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureServices)]

## <a name="configurelogging"></a>ConfigureLogging

<xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> dodaje delegata do konfigurowania podanej <xref:Microsoft.Extensions.Logging.ILoggingBuilder>. <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> mogą być wywoływane wiele razy z wynikami.

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureLogging)]

### <a name="useconsolelifetime"></a>UseConsoleLifetime

<xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> nasłuchuje <kbd>kombinacji klawiszy Ctrl</kbd>+<kbd>C</kbd>/SIGINT lub SIGTERM i wywołuje <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> w celu uruchomienia procesu zamykania. <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> odblokowywanie rozszerzeń, takich jak [RunAsync](#runasync) i [WaitForShutdownAsync](#waitforshutdownasync). `Microsoft.Extensions.Hosting.Internal.ConsoleLifetime` jest wstępnie zarejestrowany jako domyślna implementacja okresu istnienia. Używany jest ostatni zarejestrowany okres istnienia.

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseConsoleLifetime)]

## <a name="container-configuration"></a>Konfiguracja kontenera

Aby zapewnić obsługę podłączania w innych kontenerach, host może akceptować <xref:Microsoft.Extensions.DependencyInjection.IServiceProviderFactory%601>. Dostarczenie fabryki nie jest częścią rejestracji typu DI Container, ale zamiast tego jest wewnętrznym hostem używanym do tworzenia konkretnych kontenerów DI. [UseServiceProviderFactory (IServiceProviderFactory&lt;TContainerBuilder&gt;)](xref:Microsoft.Extensions.Hosting.HostBuilder.UseServiceProviderFactory*) zastępuje domyślną fabrykę używaną do tworzenia dostawcy usług aplikacji.

Niestandardowa konfiguracja kontenera jest zarządzana przez metodę <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*>. <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> zapewnia silnie określone środowisko do konfigurowania kontenera na podstawie podstawowego interfejsu API hosta. <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> może być wywoływana wiele razy z wynikami.

Utwórz kontener usługi dla aplikacji:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainer.cs)]

Podaj fabrykę kontenera usługi:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainerFactory.cs)]

Użyj fabryki i skonfiguruj niestandardowy kontener usługi dla aplikacji:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ContainerConfiguration)]

## <a name="extensibility"></a>Rozszerzalność

Rozszerzalność hosta jest wykonywana przy użyciu metod rozszerzających na <xref:Microsoft.Extensions.Hosting.IHostBuilder>. Poniższy przykład pokazuje, jak Metoda rozszerzania rozszerza implementację <xref:Microsoft.Extensions.Hosting.IHostBuilder> z przykładem [TimedHostedService](xref:fundamentals/host/hosted-services#timed-background-tasks) przedstawionym w <xref:fundamentals/host/hosted-services>.

```csharp
var host = new HostBuilder()
    .UseHostedService<TimedHostedService>()
    .Build();

await host.StartAsync();
```

Aplikacja ustanawia `UseHostedService` metodę rozszerzenia w celu zarejestrowania usługi hostowanej przekazaną w `T`:

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

## <a name="manage-the-host"></a>Zarządzanie hostem

Implementacja <xref:Microsoft.Extensions.Hosting.IHost> jest odpowiedzialna za uruchamianie i zatrzymywanie implementacji <xref:Microsoft.Extensions.Hosting.IHostedService> zarejestrowanych w kontenerze usługi.

### <a name="run"></a>Run

<xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> uruchamia aplikację i blokuje wątek wywołujący do momentu wyłączenia hosta:

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

<xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> uruchamia aplikację i zwraca <xref:System.Threading.Tasks.Task>, która kończy się w momencie wyzwolenia tokenu anulowania lub zamknięcia:

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

<xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> umożliwia obsługę konsoli, kompiluje i uruchamia hosta oraz czeka na zamknięcie programu <kbd>Ctrl</kbd>+<kbd>C</kbd>/SIGINT lub SIGTERM.

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

### <a name="start-and-stopasync"></a>Uruchom i StopAsync

<xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> uruchamia hosta synchronicznie.

<xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> próbuje zatrzymać hosta w ramach podanego limitu czasu.

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

<xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> uruchamia aplikację.

<xref:Microsoft.Extensions.Hosting.IHost.StopAsync*> zatrzymać aplikację.

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

<xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> jest wyzwalane za pośrednictwem <xref:Microsoft.Extensions.Hosting.IHostLifetime>, takich jak `Microsoft.Extensions.Hosting.Internal.ConsoleLifetime` (nasłuchuje na <kbd>Ctrl</kbd>+<kbd>C</kbd>/SIGINT lub SIGTERM). <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> wywołań.

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

<xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> zwraca <xref:System.Threading.Tasks.Task>, który kończy się po wyzwoleniu zamknięcia za pośrednictwem danego tokenu i wywołań <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.

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

### <a name="external-control"></a>Zewnętrzna kontrola

Kontrolę zewnętrzną hosta można osiągnąć przy użyciu metod, które mogą być wywoływane zewnętrznie:

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

<xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> jest wywoływana na początku <xref:Microsoft.Extensions.Hosting.IHost.StartAsync*>, który czeka na zakończenie przed kontynuowaniem. Może to służyć do opóźnienia uruchamiania do momentu zasygnalizowania zdarzenia zewnętrznego.

## <a name="ihostingenvironment-interface"></a>IHostingEnvironment, interfejs

<xref:Microsoft.Extensions.Hosting.IHostingEnvironment> zawiera informacje o środowisku hostingu aplikacji. Użyj [iniekcji konstruktora](xref:fundamentals/dependency-injection) , aby uzyskać <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> w celu użycia jej właściwości i metod rozszerzania:

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

## <a name="iapplicationlifetime-interface"></a>IApplicationLifetime, interfejs

<xref:Microsoft.Extensions.Hosting.IApplicationLifetime> pozwala na działania po uruchomieniu i zamknięcia, w tym bezpieczne żądania zamknięcia. Trzy właściwości interfejsu są tokenami anulowania używanymi do rejestrowania metod <xref:System.Action>, które definiują zdarzenia uruchamiania i zamykania.

| Token anulowania | Wyzwalane, gdy&#8230; |
| ------------------ | --------------------- |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStarted*> | Host został w pełni uruchomiony. |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStopped*> | Host kończy bezpieczne zamknięcie. Wszystkie żądania powinny być przetwarzane. Bloki zamknięcia do momentu zakończenia tego zdarzenia. |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStopping*> | Host wykonuje bezpieczne zamknięcie. Żądania mogą nadal być przetwarzane. Bloki zamknięcia do momentu zakończenia tego zdarzenia. |

Konstruktor — wstrzyknąć usługę <xref:Microsoft.Extensions.Hosting.IApplicationLifetime> do dowolnej klasy. [Przykładowa aplikacja](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) używa iniekcji konstruktora do klasy `LifetimeEventsHostedService` (implementacja <xref:Microsoft.Extensions.Hosting.IHostedService>), aby zarejestrować zdarzenia.

*LifetimeEventsHostedService.cs*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/LifetimeEventsHostedService.cs?name=snippet1)]

<xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> żądania zakończenia aplikacji. Następująca Klasa używa <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*>, aby bezpiecznie zamknąć aplikację w przypadku wywołania metody `Shutdown` klasy:

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

::: moniker-end

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:fundamentals/host/hosted-services>
