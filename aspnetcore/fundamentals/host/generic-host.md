---
title: Ogólny hosta platformy .NET
author: tdykstra
description: Dowiedz się więcej o hosta rodzajowego Core .NET który jest odpowiedzialny za zarządzanie uruchamiania i okres istnienia aplikacji.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 07/01/2019
uid: fundamentals/host/generic-host
ms.openlocfilehash: d787559eaecd6d4d6cfe01e37baf28774a90c5c3
ms.sourcegitcommit: bee530454ae2b3c25dc7ffebf93536f479a14460
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/10/2019
ms.locfileid: "67724428"
---
# <a name="net-generic-host"></a>Ogólny hosta platformy .NET

::: moniker range=">= aspnetcore-3.0"

W tym artykule przedstawiono hosta rodzajowego Core .NET (<xref:Microsoft.Extensions.Hosting.HostBuilder>) oraz wskazówki dotyczące sposobu używania go.

## <a name="whats-a-host"></a>Co to jest hostem?

A *hosta* jest obiektem, który hermetyzuje zasobów aplikacji, takich jak:

* Wstrzykiwanie zależności (DI)
* Rejestrowanie
* Konfiguracja
* `IHostedService` Implementacje

Po uruchomieniu hosta wywoływanych przez nią `IHostedService.StartAsync` każdego wykonania <xref:Microsoft.Extensions.Hosting.IHostedService> znalezione w kontenerze DI. W aplikacji sieci web, jeden z `IHostedService` implementacje jest usługą sieci web, który rozpoczyna się [implementację serwera HTTP](xref:fundamentals/index#servers).

Głównym powodem wraz ze wszystkimi zasobami współzależne aplikacji w jeden obiekt jest zarządzanie okresem istnienia: kontrolę nad uruchamianiem aplikacji i łagodne zamykanie.

W wersjach starszych niż 3.0, platformy ASP.NET Core [hosta sieci Web](xref:fundamentals/host/web-host) jest używany w przypadku obciążeń HTTP. Host sieci Web nie jest już zalecany dla aplikacji sieci web i pozostają dostępne tylko dla zgodności z poprzednimi wersjami.

## <a name="set-up-a-host"></a>Konfigurowanie hosta

Host jest skonfigurowany zwykle, skompilowane i wykonywania przez kod w `Program` klasy. `Main` Metody:

* Wywołania `CreateHostBuilder` metodę, aby utworzyć i skonfigurować obiekt konstruktora.
* Wywołania `Build` i `Run` metody do konstruktora obiektu.

Oto *Program.cs* kodu w przypadku obciążeń innych niż HTTP, za pomocą jednego `IHostedService` dodany do kontenera DI implementacji. 

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

W przypadku obciążeń HTTP `Main` metody jest tym samym ale `CreateHostBuilder` wywołania `ConfigureWebHostDefaults`:

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.UseStartup<Startup>();
        });
```

Jeśli aplikacja korzysta z platformy Entity Framework Core, nie należy zmieniać nazwy lub podpis `CreateHostBuilder` metody. [Narzędzia Entity Framework Core](/ef/core/miscellaneous/cli/) poszukiwać `CreateHostBuilder` metodę, która umożliwia skonfigurowanie hosta bez uruchamiania aplikacji. Aby uzyskać więcej informacji, zobacz [Tworzenie typu DbContext w czasie projektowania](/ef/core/miscellaneous/cli/dbcontext-creation).

## <a name="default-builder-settings"></a>Domyślne ustawienia konstruktora 

<xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> Metody:

* Ustawia zawartość katalogu głównego ścieżka zwrócona przez element <xref:System.IO.Directory.GetCurrentDirectory*>.
* Konfiguracja hosta obciążeń z:
  * Zmienne środowiskowe z prefiksem "DOTNET_".
  * Argumenty wiersza polecenia.
* Konfiguracja aplikacji obciążeń z:
  * *appsettings.json*.
  * *appsettings.{Environment}.json*.
  * [Klucz tajny Menedżera](xref:security/app-secrets) uruchamiania aplikacji `Development` środowiska.
  * Zmienne środowiskowe.
  * Argumenty wiersza polecenia.
* Dodaje następujące [rejestrowania](xref:fundamentals/logging/index) dostawców:
  * Konsola
  * Debugowanie
  * EventSource
  * Dziennik zdarzeń (tylko wtedy, gdy systemem Windows)
* Włącza [zakresu walidacji](xref:fundamentals/dependency-injection#scope-validation) i [weryfikacji zależności](xref:Microsoft.Extensions.DependencyInjection.ServiceProviderOptions.ValidateOnBuild) po środowisko programowania.

`ConfigureWebHostDefaults` Metody:

* Obciążenia hostów konfiguracji ze zmiennych środowiskowych z prefiksem "ASPNETCORE_".
* Zestawy [Kestrel](xref:fundamentals/servers/kestrel) serwera jako serwera sieci web i konfiguruje go za pomocą dostawcy usług konfiguracji hosta aplikacji. Opcjach domyślny serwer Kestrel, zobacz temat <xref:fundamentals/servers/kestrel#kestrel-options>.
* Dodaje [filtrowanie hosta oprogramowania pośredniczącego](xref:fundamentals/servers/kestrel#host-filtering).
* Dodaje [oprogramowanie pośredniczące przekazane nagłówki](xref:host-and-deploy/proxy-load-balancer#forwarded-headers) Jeśli ASPNETCORE_FORWARDEDHEADERS_ENABLED = true.
* Umożliwia integrację usług IIS. Opcjach domyślny usług IIS, zobacz temat <xref:host-and-deploy/iis/index#iis-options>.

[Ustawienia dla wszystkich typów aplikacji](#settings-for-all-app-types) i [ustawień aplikacji sieci web](#settings-for-web-apps) sekcje w dalszej części tego artykułu opisano sposób zastępują ustawienia konstruktora domyślnego.

## <a name="framework-provided-services"></a>Dostarczone do struktury usługi

Usługi, które są automatycznie rejestrowane są następujące:

* [IHostApplicationLifetime](#ihostapplicationlifetime)
* [IHostLifetime](#ihostlifetime)
* [IHostEnvironment / IWebHostEnvironment](#ihostenvironment)

Aby uzyskać listę wszystkich usług dostarczone przez framework, zobacz <xref:fundamentals/dependency-injection#framework-provided-services>.

## <a name="ihostapplicationlifetime"></a>IHostApplicationLifetime

Wstrzykiwanie <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime> (dawniej `IApplicationLifetime`) usługi do każdej klasy do obsługi zadań uruchamiania po i łagodne zamykanie. Trzy właściwości w interfejsie są tokenów anulowania, używane do rejestrowania początkowego aplikacji i metody obsługi zdarzeń zatrzymania aplikacji. Interfejs zawiera również `StopApplication` metody.

Poniższy przykład jest `IHostedService` implementację, która rejestruje `IApplicationLifetime` zdarzenia:

[!code-csharp[](generic-host/samples-snapshot/3.x/LifetimeEventsHostedService.cs?name=snippet_LifetimeEvents)]

## <a name="ihostlifetime"></a>IHostLifetime

<xref:Microsoft.Extensions.Hosting.IHostLifetime> Implementacji kontroluje podczas uruchamiania hosta i po zatrzymaniu. Ostatniego wykonania zarejestrowany jest używany.

<xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime> Wartość domyślna to `IHostLifetime` implementacji. `ConsoleLifetime`:

* nasłuchuje sygnał SIGTERM lub wywołań i Ctrl + C/SIGINT <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> można uruchomić procesu zamykania.
* Odblokowuje rozszerzeń, takich jak [RunAsync](#runasync) i [WaitForShutdownAsync](#waitforshutdownasync).

## <a name="ihostenvironment"></a>IHostEnvironment

Wstrzykiwanie <xref:Microsoft.Extensions.Hosting.IHostEnvironment> usługi do klasy, aby uzyskać informacje o następujących czynności:

* [ApplicationName](#applicationname)
* [EnvironmentName](#environmentname)
* [ContentRootPath](#contentrootpath)

Wdrożenie aplikacji sieci Web `IWebHostEnvironment` interfejs, który dziedziczy `IHostEnvironment` i dodaje:

* [WebRootPath](#webroot)

## <a name="host-configuration"></a>Konfiguracja hosta

Konfiguracja hosta jest używana do właściwości <xref:Microsoft.Extensions.Hosting.IHostEnvironment> implementacji.

Konfiguracja hosta jest dostępny z [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration) wewnątrz <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>. Po `ConfigureAppConfiguration`, `HostBuilderContext.Configuration` zostaje zastąpiona opcją konfiguracji aplikacji.

Aby dodać konfigurację hosta, należy wywołać <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> na `IHostBuilder`. `ConfigureHostConfiguration` można wywołać wiele razy z wynikami dodatku. Host używa jednego z tych opcji ustawia wartość ostatniego dla danego klucza.

Dostawca zmiennej środowiska z prefiksem `DOTNET_` i argumenty wiersza polecenia są uwzględniane przy CreateDefaultBuilder. W przypadku aplikacji sieci web dostawcy zmiennej środowiska z prefiksem `ASPNETCORE_` zostanie dodany. Prefiks jest usuwany, gdy zmienne środowiskowe są odczytywane. Na przykład tej wartości zmiennej środowiskowej dla `ASPNETCORE_ENVIRONMENT` staje się wartość konfiguracji hosta `environment` klucza.

Poniższy przykład umożliwia utworzenie konfiguracji hosta:

[!code-csharp[](generic-host/samples-snapshot/3.x/Program.cs?name=snippet_HostConfig)]

## <a name="app-configuration"></a>Konfiguracja aplikacji

Konfiguracja aplikacji jest tworzony przez wywołanie <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> na `IHostBuilder`. `ConfigureAppConfiguration` można wywołać wiele razy z wynikami dodatku. Aplikacja używa jednego z tych opcji ustawia wartość ostatniego dla danego klucza. 

Konfiguracji utworzonej przez `ConfigureAppConfiguration` znajduje się w temacie [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) dla kolejnych operacji i as a service firmy DI. Konfiguracja hosta jest także dodawane do konfiguracji aplikacji.

Aby uzyskać więcej informacji, zobacz [konfiguracji w programie ASP.NET Core](xref:fundamentals/configuration/index#configureappconfiguration).

## <a name="settings-for-all-app-types"></a>Ustawienia dla wszystkich typów aplikacji

W tej sekcji przedstawiono ustawienia hosta, które są stosowane do protokołów HTTP, jak i obciążeń innych niż HTTP. Domyślnie zmienne środowiskowe używane, aby skonfigurować te ustawienia mogą mieć `DOTNET_` lub `ASPNETCORE_` prefiks.

### <a name="applicationname"></a>ApplicationName

[IHostEnvironment.ApplicationName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ApplicationName*) właściwość ma wartość z konfiguracji hosta podczas konstruowania hosta.

**Klucz**: applicationName  
**Typ**: *ciągu*  
**Domyślne**: Nazwa zestawu, który zawiera wpis aplikacji punktu.
**Zmienna środowiskowa**: `<PREFIX_>APPLICATIONNAME`

Aby ustawić tę wartość, należy użyć zmiennej środowiskowej. 

### <a name="contentrootpath"></a>ContentRootPath

[IHostEnvironment.ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath*) właściwość określa, gdzie hosta rozpoczyna się wyszukiwanie plików zawartości. Jeśli ścieżka nie istnieje, host nie można uruchomić.

**Klucz**: contentRoot  
**Typ**: *ciągu*  
**Domyślne**: Folder, w którym znajduje się zestaw aplikacji.  
**Zmienna środowiskowa**: `<PREFIX_>CONTENTROOT`

Aby ustawić tę wartość, należy użyć zmiennej środowiskowej lub wywołanie `UseContentRoot` na `IHostBuilder`:

```csharp
Host.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\content-root")
    //...
```

### <a name="environmentname"></a>EnvironmentName

[IHostEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.EnvironmentName*) właściwość można ustawić dowolną wartość. Wartości zdefiniowane w ramach obejmują `Development`, `Staging`, i `Production`. Wartości nie są z uwzględnieniem wielkości liter.

**Klucz**: środowisko  
**Typ**: *ciągu*  
**Domyślne**: Produkcji  
**Zmienna środowiskowa**: `<PREFIX_>ENVIRONMENT`

Aby ustawić tę wartość, należy użyć zmiennej środowiskowej lub wywołanie `UseEnvironment` na `IHostBuilder`:

```csharp
Host.CreateDefaultBuilder(args)
    .UseEnvironment("Development")
    //...
```

### <a name="shutdowntimeout"></a>ShutdownTimeout

[HostOptions.ShutdownTimeout](xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*) ustawiono limit czasu <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>. Wartość domyślna to 5 sekund.  W trakcie okresu czasu hosta:

* Wyzwalacze [IHostApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).
* Podejmuje próby zatrzymania usług hostowanych, rejestrowanie błędów, których nie można zatrzymać usługi.

Jeśli upłynie limit czasu przed wszystkimi zatrzymania usług hostowanych, wszystkie pozostałe usługi active zostaną zatrzymane podczas zamykania aplikacji. Usługi zatrzymania nawet wtedy, gdy jeszcze nie zakończyło się przetwarzanie. Jeśli usługi wymagają dodatkowego czasu, aby zatrzymać, zwiększ limit czasu.

**Klucz**: shutdownTimeoutSeconds  
**Typ**: *int*  
**Domyślne**: 5 sekund **zmiennej środowiskowej**: `<PREFIX_>SHUTDOWNTIMEOUTSECONDS`

Aby ustawić tę wartość, należy użyć zmiennej środowiskowej lub skonfigurować `HostOptions`. W poniższym przykładzie ustawiono limit czasu na 20 sekund:

[!code-csharp[](generic-host/samples-snapshot/3.x/Program.cs?name=snippet_HostOptions)]

## <a name="settings-for-web-apps"></a>Ustawienia dla aplikacji sieci web

Niektóre ustawienia hosta dotyczą tylko obciążenia HTTP. Domyślnie zmienne środowiskowe używane, aby skonfigurować te ustawienia mogą mieć `DOTNET_` lub `ASPNETCORE_` prefiks.

Metody rozszerzenia na `IWebHostBuilder` są dostępne dla tych ustawień. Przyjęto założenie, przykłady kodu, które pokazują sposób wywołania metody rozszerzenia `webBuilder` jest wystąpieniem `IWebHostBuilder`, jak w poniższym przykładzie:

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

Gdy `false`, błędy podczas uruchamiania wynik na hoście, kończenie. Gdy `true`, host przechwytuje wyjątki podczas uruchamiania i próbuje uruchomić serwer.

**Klucz**: captureStartupErrors  
**Typ**: *bool* (`true` lub `1`)  
**Domyślne**: Wartość domyślna to `false` chyba, że aplikacja jest uruchamiana z Kestrel za usług IIS, w którym domyślnie są `true`.  
**Zmienna środowiskowa**: `<PREFIX_>CAPTURESTARTUPERRORS`

Aby ustawić tę wartość, należy użyć konfiguracji lub wywołanie `CaptureStartupErrors`:

```csharp
webBuilder.CaptureStartupErrors(true);
```

### <a name="detailederrors"></a>DetailedErrors

Po włączeniu lub w przypadku, gdy środowisko jest `Development`, aplikacja przechwytuje szczegółowe informacje o błędach.

**Klucz**: detailedErrors  
**Typ**: *bool* (`true` lub `1`)  
**Domyślne**: false  
**Zmienna środowiskowa**: `<PREFIX_>_DETAILEDERRORS`

Aby ustawić tę wartość, należy użyć konfiguracji lub wywołanie `UseSetting`:

```csharp
webBuilder.UseSetting(WebHostDefaults.DetailedErrorsKey, "true");
```

### <a name="hostingstartupassemblies"></a>HostingStartupAssemblies

Rozdzielana średnikami ciąg hostingu zestawy startowe załadować podczas uruchamiania. Mimo że konfiguracja ma domyślnie wartość pustego ciągu, hostingu zestawy startowe zawsze zawierać zestaw aplikacji. Hostingu zestawy startowe są udostępniane, są dodawane do zestawu aplikacji dotyczące ładowania, gdy aplikacja tworzy swoich usług wspólne podczas uruchamiania.

**Klucz**: hostingStartupAssemblies  
**Typ**: *ciągu*  
**Domyślne**: Pusty ciąg  
**Zmienna środowiskowa**: `<PREFIX_>_HOSTINGSTARTUPASSEMBLIES`

Aby ustawić tę wartość, należy użyć konfiguracji lub wywołanie `UseSetting`:

```csharp
webBuilder.UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2");
```

### <a name="hostingstartupexcludeassemblies"></a>HostingStartupExcludeAssemblies

Rozdzielana średnikami ciąg hostingu uruchamiania zestawów, które mają zostać wykluczone podczas uruchamiania.

**Klucz**: hostingStartupExcludeAssemblies  
**Typ**: *ciągu*  
**Domyślne**: Pusty ciąg  
**Zmienna środowiskowa**: `<PREFIX_>_HOSTINGSTARTUPEXCLUDEASSEMBLIES`

Aby ustawić tę wartość, należy użyć konfiguracji lub wywołanie `UseSetting`:

```csharp
webBuilder.UseSetting(WebHostDefaults.HostingStartupExcludeAssembliesKey, "assembly1;assembly2");
```

### <a name="httpsport"></a>HTTPS_Port

HTTPS przekierowania portu. Używane w [Wymuszanie protokołu HTTPS](xref:security/enforcing-ssl).

**Klucz**: https_port **typu**: *ciąg*
**domyślne**: Nie ustawiono wartość domyślną.
**Zmienna środowiskowa**: `<PREFIX_>HTTPS_PORT`

Aby ustawić tę wartość, należy użyć konfiguracji lub wywołanie `UseSetting`:

```csharp
webBuilder.UseSetting("https_port", "8080");
```

### <a name="preferhostingurls"></a>PreferHostingUrls

Wskazuje, czy host powinien nasłuchiwać adresy URL skonfigurowano `IWebHostBuilder` zamiast konfigurowane przy użyciu `IServer` implementacji.

**Klucz**: preferHostingUrls  
**Typ**: *bool* (`true` lub `1`)  
**Domyślne**: true  
**Zmienna środowiskowa**: `<PREFIX_>_PREFERHOSTINGURLS`

Aby ustawić tę wartość, należy użyć zmiennej środowiskowej lub wywołanie `PreferHostingUrls`:

```csharp
webBuilder.PreferHostingUrls(false);
```

### <a name="preventhostingstartup"></a>PreventHostingStartup

Uniemożliwia automatyczne ładowanie obsługi zestawów uruchamiania, w tym hosting zestawy startowe skonfigurowany przez zestaw aplikacji. Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/platform-specific-configuration>.

**Klucz**: preventHostingStartup  
**Typ**: *bool* (`true` lub `1`)  
**Domyślne**: false  
**Zmienna środowiskowa**: `<PREFIX_>_PREVENTHOSTINGSTARTUP`

Aby ustawić tę wartość, należy użyć zmiennej środowiskowej lub wywołanie `UseSetting` :

```csharp
webBuilder.UseSetting(WebHostDefaults.PreventHostingStartupKey, "true");
```

### <a name="startupassembly"></a>StartupAssembly

Zestaw, aby wyszukać `Startup` klasy.

**Klucz**: startupAssembly **typu**: *ciągu*  
**Domyślne**: Zestaw aplikacji  
**Zmienna środowiskowa**: `<PREFIX_>STARTUPASSEMBLY`

Aby ustawić tę wartość, należy użyć zmiennej środowiskowej lub wywołanie `UseStartup`. `UseStartup` może to nazwa zestawu (`string`) lub typu (`TStartup`). Jeśli wiele `UseStartup` metody są wywoływane, ostatni z nich ma pierwszeństwo.

```csharp
webBuilder.UseStartup("StartupAssemblyName");
```

```csharp
webBuilder.UseStartup<Startup>();
```

### <a name="urls"></a>adresy URL

Rozdzielana średnikami lista adresów IP lub adresów hosta, portów i protokołów, które serwer powinien nasłuchiwać żądań protokołu. Na przykład `http://localhost:123`. Użyj "\*" Aby wskazać, że serwer powinien nasłuchiwać żądań adresy IP lub nazwa hosta przy użyciu określonego portu i protokołu (na przykład `http://*:5000`). Protokół (`http://` lub `https://`) muszą być dołączone do każdego adresu URL. Obsługiwane formaty różnią się między serwerami.

**Klucz**: adresy URL  
**Typ**: *ciągu*  
**Domyślne**: `http://localhost:5000` i `https://localhost:5001` 
 **zmiennej środowiskowej**: `<PREFIX_>URLS`

Aby ustawić tę wartość, należy użyć zmiennej środowiskowej lub wywołanie `UseUrls`:

```csharp
webBuilder.UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002");
```

Kestrel ma swój własny konfiguracji punktu końcowego interfejsu API. Aby uzyskać więcej informacji, zobacz <xref:fundamentals/servers/kestrel#endpoint-configuration>.

### <a name="webroot"></a>WebRoot

Ścieżka względna do statycznych zasobów aplikacji.

**Klucz**: webroot  
**Typ**: *ciągu*  
**Domyślne**: *(Zawartość katalogu głównego) / wwwroot*, jeśli ścieżka istnieje. Jeśli ścieżka nie istnieje, jest używany dostawca pliku pusta.  
**Zmienna środowiskowa**: `<PREFIX_>WEBROOT`

Aby ustawić tę wartość, należy użyć zmiennej środowiskowej lub wywołanie `UseWebRoot`:

```csharp
webBuilder.UseWebRoot("public");
```

## <a name="manage-the-host-lifetime"></a>Zarządzanie okresem istnienia hosta

Wywoływanie metod na wbudowanych <xref:Microsoft.Extensions.Hosting.IHost> implementacji, uruchamianie i zatrzymywanie aplikacji. Te metody mają wpływ na wszystkie <xref:Microsoft.Extensions.Hosting.IHostedService> implementacji, które są zarejestrowane w usłudze service container.

### <a name="run"></a>Uruchom

<xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> Uruchamia aplikację i blokuje wątek wywołujący, aż host zostanie zamknięta.

### <a name="runasync"></a>RunAsync

<xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> Uruchamia aplikację i zwraca <xref:System.Threading.Tasks.Task> który zostaje ukończony po wyzwoleniu token anulowania lub zamknięcia systemu.

### <a name="runconsoleasync"></a>RunConsoleAsync

<xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> Włączenie obsługi konsoli, kompilacje uruchamia hosta i czeka na klawisze Ctrl + C/SIGINT lub SIGTERM zamknąć.

### <a name="start"></a>Uruchamianie

<xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> Uruchamia hosta synchronicznie.

### <a name="startasync"></a>StartAsync

<xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> Uruchamia hosta i zwraca <xref:System.Threading.Tasks.Task> który zostaje ukończony po wyzwoleniu token anulowania lub zamknięcia systemu. 

<xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> nosi nazwę na początku `StartAsync`, który powoduje oczekiwanie, dopóki nie zostanie ukończone przed kontynuowaniem. Może to służyć do opóźnienie uruchamiania, dopóki nie zasygnalizowane przez zdarzenie zewnętrzne.

### <a name="stopasync"></a>StopAsync

<xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> podejmuje próby zatrzymania hosta w ciągu podanego limitu czasu.

### <a name="waitforshutdown"></a>WaitForShutdown

<xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> blokuje wątek wywołujący, aż zamknięcie jest wyzwalany przez IHostLifetime, takie jak za pomocą klawiszy Ctrl + C/SIGINT lub SIGTERM.

### <a name="waitforshutdownasync"></a>WaitForShutdownAsync

<xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> Zwraca <xref:System.Threading.Tasks.Task> który zostaje ukończony po wyzwoleniu zamknięcie za pośrednictwem danego tokenu i wywołania <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.

### <a name="external-control"></a>Zewnętrznej kontroli

Bezpośrednia kontrola nad okresem istnienia hosta można osiągnąć przy użyciu metod, które mogą być wywoływane zewnętrznie:

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

Aplikacje platformy ASP.NET Core, konfigurowanie i uruchamiania hosta. Host jest odpowiedzialny za zarządzanie uruchamiania i czasu życia aplikacji.

W tym artykule opisano Host rodzajowego Core ASP.NET (<xref:Microsoft.Extensions.Hosting.HostBuilder>), który jest używany w przypadku aplikacji, które nie przetwarzają żądania HTTP.

Ogólny hosta ma na celu rozdzielenie potoku HTTP z hosta internetowego interfejsu API, umożliwiające szersze gamę scenariuszy hosta. Komunikaty, zadania w tle i innych obciążeń innych niż HTTP oparte na ogólnych hosta korzyści z możliwości przekrojowe, takie jak konfiguracja, wstrzykiwanie zależności (DI) i rejestrowania.

Ogólny hosta jest nowa w programie ASP.NET Core 2.1 i nie jest odpowiednie w scenariuszach hostingu w sieci web. W przypadku scenariuszy hostingu w sieci web, użyj [hosta sieci Web](xref:fundamentals/host/web-host). Ogólny Host będzie Zastąp hosta sieci Web w przyszłym wydaniu i pełnić rolę hosta podstawowego interfejsu API zarówno w przypadku protokołu HTTP, jak i scenariuszy innych niż HTTP.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([sposobu pobierania](xref:index#how-to-download-a-sample))

Podczas uruchamiania przykładowej aplikacji [programu Visual Studio Code](https://code.visualstudio.com/), użyj *zewnętrznych lub w zintegrowanym terminalu*. Nie, uruchom przykład w `internalConsole`.

Aby ustawić konsoli w programie Visual Studio Code:

1. Otwórz *.vscode/launch.json* pliku.
1. W **.NET Core uruchamianie (Konsola)** konfiguracji, zlokalizuj **konsoli** wpisu. Ustaw wartość na wartość `externalTerminal` lub `integratedTerminal`.

## <a name="introduction"></a>Wprowadzenie

Biblioteka ogólnego hosta jest dostępna w <xref:Microsoft.Extensions.Hosting> przestrzeni nazw i udostępnionych przez [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/) pakietu. `Microsoft.Extensions.Hosting` Pakietu znajduje się w [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) (platformy ASP.NET Core 2.1 lub nowszej).

<xref:Microsoft.Extensions.Hosting.IHostedService> jest punktem wejścia do wykonania kodu. Każdy <xref:Microsoft.Extensions.Hosting.IHostedService> implementacja jest wykonywany w kolejności od [usługi rejestracji w ConfigureServices](#configureservices). <xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*> jest wywoływana w każdej <xref:Microsoft.Extensions.Hosting.IHostedService> podczas uruchamiania hosta, a <xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*> jest wywoływana w kolejności odwrotnej rejestracji, gdy kończy pracę bez problemu zmieniała hosta.

## <a name="set-up-a-host"></a>Konfigurowanie hosta

<xref:Microsoft.Extensions.Hosting.IHostBuilder> jest to główny składnik, używanego przez aplikacje i biblioteki do zainicjowania, tworzeniu i uruchamianiu hosta:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_HostBuilder)]

## <a name="options"></a>Opcje

<xref:Microsoft.Extensions.Hosting.HostOptions> Skonfiguruj opcje <xref:Microsoft.Extensions.Hosting.IHost>.

### <a name="shutdown-timeout"></a>Limit czasu zamykania

<xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> Ustawia limit czasu dla <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>. Wartość domyślna to 5 sekund.

Następująca Konfiguracja opcji w `Program.Main` zwiększa domyślna pięć drugi zamykania wartość limitu czasu na 20 sekund:

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
**Domyślne**: Nazwa zestawu zawierającego wpis aplikacji punktu.  
**Można ustawić przy użyciu**: `HostBuilderContext.HostingEnvironment.ApplicationName`  
**Zmienna środowiskowa**: `<PREFIX_>APPLICATIONNAME` (`<PREFIX_>` jest [opcjonalne i zdefiniowane przez użytkownika](#configurehostconfiguration))

### <a name="content-root"></a>Zawartość katalogu głównego

To ustawienie określa, gdzie hosta rozpoczyna się wyszukiwanie plików zawartości.

**Klucz**: contentRoot  
**Typ**: *ciągu*  
**Domyślne**: Wartość domyślna to folder, w którym znajduje się zestaw aplikacji.  
**Można ustawić przy użyciu**: `UseContentRoot`  
**Zmienna środowiskowa**: `<PREFIX_>CONTENTROOT` (`<PREFIX_>` jest [opcjonalne i zdefiniowane przez użytkownika](#configurehostconfiguration))

Jeśli ścieżka nie istnieje, host nie można uruchomić.

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseContentRoot)]

### <a name="environment"></a>Środowisko

Ustawia aplikacji [środowiska](xref:fundamentals/environments).

**Klucz**: środowisko  
**Typ**: *ciągu*  
**Domyślne**: Produkcji  
**Można ustawić przy użyciu**: `UseEnvironment`  
**Zmienna środowiskowa**: `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` jest [opcjonalne i zdefiniowane przez użytkownika](#configurehostconfiguration))

Środowisko można ustawić dowolną wartość. Wartości zdefiniowane w ramach obejmują `Development`, `Staging`, i `Production`. Wartości nie są z uwzględnieniem wielkości liter.

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseEnvironment)]

### <a name="configurehostconfiguration"></a>ConfigureHostConfiguration

<xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> używa <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> utworzyć <xref:Microsoft.Extensions.Configuration.IConfiguration> dla hosta. Konfiguracja hosta jest wykorzystywany do inicjacji <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> do użycia w procesie kompilacji aplikacji.

<xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> można wywołać wiele razy z wynikami dodatku. Host używa jednego z tych opcji ustawia wartość ostatniego dla danego klucza.

Brak dostawców są domyślnie dołączone. Należy jawnie określić niezależnie od dostawcy konfiguracji aplikacji wymaga w <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>, w tym:

* Plik konfiguracji (na przykład z *hostsettings.json* pliku).
* Zmienne konfiguracji środowiska.
* Konfiguracja argumentu wiersza polecenia.
* Innych dostawców wymaganej konfiguracji.

Plik konfiguracji hosta jest włączona, określając ścieżki podstawowej aplikacji za pomocą `SetBasePath` następuje wywołanie jednej z [pliku dostawcy konfiguracji](xref:fundamentals/configuration/index#file-configuration-provider). Przykładowa aplikacja używa pliku JSON, *hostsettings.json*i wywołuje <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> korzystanie z ustawień konfiguracji hosta tego pliku.

Aby dodać [zmiennych konfiguracji środowiska](xref:fundamentals/configuration/index#environment-variables-configuration-provider) hosta, należy wywołać <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> w Konstruktorze hosta. `AddEnvironmentVariables` akceptuje opcjonalny prefiks zdefiniowany przez użytkownika. Przykładowa aplikacja korzysta z prefiksem `PREFIX_`. Prefiks jest usuwany, gdy zmienne środowiskowe są odczytywane. Po skonfigurowaniu hostów przykładową aplikację, wartość zmiennej środowiskowej, aby uzyskać `PREFIX_ENVIRONMENT` staje się wartość konfiguracji hosta `environment` klucza.

Podczas programowania, korzystając z [programu Visual Studio](https://visualstudio.microsoft.com) lub uruchamianie aplikacji za pomocą `dotnet run`, zmienne środowiskowe, może być ustawiona w *Properties/launchSettings.json* pliku. W [programu Visual Studio Code](https://code.visualstudio.com/), zmienne środowiskowe, może być ustawiona w *.vscode/launch.json* pliku podczas programowania. Aby uzyskać więcej informacji, zobacz <xref:fundamentals/environments>.

[Wiersza polecenia konfiguracji](xref:fundamentals/configuration/index#command-line-configuration-provider) jest dodawany przez wywołanie <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>. Konfiguracja wiersza polecenia zostanie dodany ostatnio pozwalającą na argumenty wiersza polecenia, aby zastąpić konfiguracji udostępnianych przez starszych dostawców konfiguracji.

*hostsettings.json*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/hostsettings.json)]

Dodatkowa konfiguracja może być wyposażone w [applicationName](#application-key-name) i [contentRoot](#content-root) kluczy.

Przykład `HostBuilder` konfiguracji przy użyciu <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureHostConfiguration)]

## <a name="configureappconfiguration"></a>ConfigureAppConfiguration

Konfiguracja aplikacji jest tworzony przez wywołanie <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> na <xref:Microsoft.Extensions.Hosting.IHostBuilder> implementacji. <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> używa <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> utworzyć <xref:Microsoft.Extensions.Configuration.IConfiguration> dla aplikacji. <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> można wywołać wiele razy z wynikami dodatku. Aplikacja używa jednego z tych opcji ustawia wartość ostatniego dla danego klucza. Konfiguracji utworzonej przez <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> znajduje się w temacie [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) dla kolejnych operacji i w <xref:Microsoft.Extensions.Hosting.IHost.Services*>.

Konfiguracja aplikacji automatycznie otrzymuje konfigurację hosta, dostarczone przez [ConfigureHostConfiguration](#configurehostconfiguration).

Przykład korzystającą configuration <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureAppConfiguration)]

*appsettings.json*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.json)]

*appsettings.Development.json*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Development.json)]

*appsettings.Production.json*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Production.json)]

Aby przenieść pliki ustawień do katalogu wyjściowego, określ pliki ustawień jako [elementów projektu programu MSBuild](/visualstudio/msbuild/common-msbuild-project-items) w pliku projektu. Przykładowa aplikacja przenosi jego pliki ustawień aplikacji w formacie JSON i *hostsettings.json* następującym `<Content>` elementu:

```xml
<ItemGroup>
  <Content Include="**\*.json" Exclude="bin\**\*;obj\**\*" 
      CopyToOutputDirectory="PreserveNewest" />
</ItemGroup>
```

> [!NOTE]
> Metody rozszerzenia konfiguracji, takich jak <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> i <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> wymagają dodatkowych pakietów NuGet, takich jak [Microsoft.Extensions.Configuration.Json](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Json) i [ Microsoft.Extensions.Configuration.EnvironmentVariables](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.EnvironmentVariables). Chyba że aplikacja korzysta z [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app), te pakiety muszą zostać dodane do projektu, oprócz podstawowe [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration) pakietu. Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/index>.

## <a name="configureservices"></a>ConfigureServices

<xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> dodaje usług do aplikacji [wstrzykiwanie zależności](xref:fundamentals/dependency-injection) kontenera. <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> można wywołać wiele razy z wynikami dodatku.

Usługa hostowana jest klasą z logiką zadań tła, który implementuje <xref:Microsoft.Extensions.Hosting.IHostedService> interfejsu. Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/hosted-services>.

[Przykładową aplikację](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) używa `AddHostedService` metodę rozszerzenia, aby dodać usługę do zdarzenia okresu istnienia `LifetimeEventsHostedService`i zadania w tle czasu `TimedHostedService`, do aplikacji:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureServices)]

## <a name="configurelogging"></a>ConfigureLogging

<xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> dodaje delegata do konfigurowania podane <xref:Microsoft.Extensions.Logging.ILoggingBuilder>. <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> może zostać wywołana wiele razy z wynikami dodatku.

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureLogging)]

### <a name="useconsolelifetime"></a>UseConsoleLifetime

<xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> nasłuchuje sygnał SIGTERM lub wywołań i Ctrl + C/SIGINT <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> można uruchomić procesu zamykania. <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> Odblokowuje rozszerzeń, takich jak [RunAsync](#runasync) i [WaitForShutdownAsync](#waitforshutdownasync). <xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime> wstępnie jest zarejestrowany jako domyślna implementacja okresu istnienia. Ostatniego okresu istnienia zarejestrowany jest używany.

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseConsoleLifetime)]

## <a name="container-configuration"></a>Konfiguracja kontenera

Aby zapewnić obsługę podłączania innych kontenerów, host może akceptować <xref:Microsoft.Extensions.DependencyInjection.IServiceProviderFactory%601>. Zapewnianie fabrykę nie jest częścią DI rejestracja kontenera jest jednak wewnętrzne hosta używany do tworzenia konkretnych kontenera DI. [UseServiceProviderFactory (IServiceProviderFactory&lt;TContainerBuilder&gt;)](xref:Microsoft.Extensions.Hosting.HostBuilder.UseServiceProviderFactory*) zastępuje domyślną fabrykę użyty do utworzenia dostawcy usługi app service.

Konfiguracja kontenera niestandardowego jest zarządzana przez <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> metody. <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> udostępnia silnie typizowane środowisko na potrzeby konfigurowania kontenera na podstawie odpowiedniego hosta interfejsu API. <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> można wywołać wiele razy z wynikami dodatku.

Tworzenie kontenera usług dla aplikacji:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainer.cs)]

Podaj fabryki kontenera usług:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainerFactory.cs)]

Używaj fabryki i konfigurowanie kontenera niestandardowego usługi dla aplikacji:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ContainerConfiguration)]

## <a name="extensibility"></a>Rozszerzalność

Rozszerzalność hosta odbywa się za pomocą metod rozszerzenia na <xref:Microsoft.Extensions.Hosting.IHostBuilder>. W poniższym przykładzie pokazano, jak metoda rozszerzenia rozszerza <xref:Microsoft.Extensions.Hosting.IHostBuilder> implementację [TimedHostedService](xref:fundamentals/host/hosted-services#timed-background-tasks) przykład przedstawiona w <xref:fundamentals/host/hosted-services>.

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

<xref:Microsoft.Extensions.Hosting.IHost> Implementacja jest odpowiedzialna za uruchamianie i zatrzymywanie <xref:Microsoft.Extensions.Hosting.IHostedService> implementacji, które są zarejestrowane w usłudze service container.

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

<xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> Uruchamia aplikację i zwraca <xref:System.Threading.Tasks.Task> który zostaje ukończony po wyzwoleniu token anulowania lub zamknięcia:

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

<xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> Włączenie obsługi konsoli, kompilacje uruchamia hosta i czeka na klawisze Ctrl + C/SIGINT lub SIGTERM zamknąć.

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

<xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> jest wyzwalany za pośrednictwem <xref:Microsoft.Extensions.Hosting.IHostLifetime>, takich jak <xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime> (nasłuchuje klawisze Ctrl + C/SIGINT lub SIGTERM). <xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> wywołania <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.

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

<xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> Zwraca <xref:System.Threading.Tasks.Task> który zostaje ukończony po wyzwoleniu zamknięcie za pośrednictwem danego tokenu i wywołania <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.

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

<xref:Microsoft.Extensions.Hosting.IHostingEnvironment> zawiera informacje o aplikacji w środowisku hostingu. Użyj [iniekcji konstruktora](xref:fundamentals/dependency-injection) uzyskać <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> aby można było używać jej właściwości i metody rozszerzenia:

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

<xref:Microsoft.Extensions.Hosting.IApplicationLifetime> Umożliwia działań po uruchamiania i zamykania, w tym łagodne zamykanie żądania. Trzy właściwości w interfejsie są tokenów anulowania, używane do rejestrowania <xref:System.Action> metod, które definiują zdarzenia uruchamiania i zamykania.

| Token anulowania | Wyzwalane, gdy&#8230; |
| ------------------ | --------------------- |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStarted*> | Host pełni została uruchomiona. |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStopped*> | Host jest kończonych łagodne zamykanie. Wszystkie żądania powinny zostać przetworzone. Blokuje zamknięcia, aż do zakończenia tego zdarzenia. |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStopping*> | Wykonuje łagodne zamykanie hosta. Żądania mogą nadal być przetwarzane. Blokuje zamknięcia, aż do zakończenia tego zdarzenia. |

Wstrzykiwanie Konstruktor <xref:Microsoft.Extensions.Hosting.IApplicationLifetime> usługi do każdej klasy. [Przykładową aplikację](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) używa iniekcji konstruktora do `LifetimeEventsHostedService` klasy ( <xref:Microsoft.Extensions.Hosting.IHostedService> implementacji) do rejestrowania zdarzeń.

*LifetimeEventsHostedService.cs*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/LifetimeEventsHostedService.cs?name=snippet1)]

<xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> żądania zakończenia aplikacji. Następujące klasy używa <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> bezpiecznie zamknąć aplikację po klasy `Shutdown` metoda jest wywoływana:

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
