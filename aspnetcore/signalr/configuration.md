---
title: Konfiguracja sygnałów ASP.NET Core
author: bradygaster
description: Dowiedz się, jak skonfigurować aplikacje sygnalizujące ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 08/05/2019
uid: signalr/configuration
ms.openlocfilehash: 66f274fcda27392091de6b4be8c7221bc87b7585
ms.sourcegitcommit: c452e6af92e130413106c4863193f377cde4cd9c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/10/2019
ms.locfileid: "72246492"
---
# <a name="aspnet-core-signalr-configuration"></a>Konfiguracja sygnałów ASP.NET Core

## <a name="jsonmessagepack-serialization-options"></a>Opcje serializacji JSON/MessagePack

ASP.NET Core sygnalizujący obsługuje dwa protokoły dla komunikatów kodowania: [JSON](https://www.json.org/) i [MessagePack](https://msgpack.org/index.html). Każdy protokół ma opcje konfiguracji serializacji.

::: moniker range=">= aspnetcore-3.0"

Serializacji JSON można skonfigurować na serwerze przy użyciu metody rozszerzenia [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) . `AddJsonProtocol` można dodać po [Addsignaler](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) w `Startup.ConfigureServices`. Metoda `AddJsonProtocol` przyjmuje delegata, który odbiera obiekt `options`. Właściwość [PayloadSerializerOptions](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializeroptions) dla tego obiektu jest obiektem `System.Text.Json` <xref:System.Text.Json.JsonSerializerOptions>, którego można użyć do skonfigurowania serializacji argumentów i zwracanych wartości. Aby uzyskać więcej informacji, zobacz [dokumentację system. Text. JSON](/dotnet/api/system.text.json).

Przykładowo, aby skonfigurować Serializator, aby nie zmieniać wielkości liter nazw właściwości zamiast domyślnych nazw "camelCase", należy użyć następującego kodu w `Startup.ConfigureServices`:

```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerOptions.PropertyNamingPolicy = null
    });
```

W kliencie .NET ta sama Metoda rozszerzenia `AddJsonProtocol` istnieje w [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder). Aby można było rozpoznać metodę rozszerzenia, należy zaimportować przestrzeń nazw `Microsoft.Extensions.DependencyInjection`:

```csharp
// At the top of the file:
using Microsoft.Extensions.DependencyInjection;

// When constructing your connection:
var connection = new HubConnectionBuilder()
    .AddJsonProtocol(options => {
        options.PayloadSerializerOptions.PropertyNamingPolicy = null;
    })
    .Build();
```

### <a name="switch-to-newtonsoftjson"></a>Przejdź do pliku Newtonsoft. JSON

Jeśli potrzebujesz funkcji `Newtonsoft.Json`, które nie są obsługiwane w `System.Text.Json`, zobacz [Switch to Newtonsoft. JSON](xref:migration/22-to-30#switch-to-newtonsoftjson).

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

Serializacji JSON można skonfigurować na serwerze przy użyciu metody rozszerzenia [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) , która może zostać dodana po [addsignaler](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) w metodzie `Startup.ConfigureServices`. Metoda `AddJsonProtocol` przyjmuje delegata, który odbiera obiekt `options`. Właściwość [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) tego obiektu jest obiektem JSON.NET `JsonSerializerSettings`, którego można użyć do skonfigurowania serializacji argumentów i zwracanych wartości. Aby uzyskać więcej informacji, zapoznaj się z [dokumentacją JSON.NET](https://www.newtonsoft.com/json/help/html/Introduction.htm).
 
Przykładowo, aby skonfigurować serializator do używania nazw właściwości "PascalCase" zamiast domyślnych nazw "camelCase", należy użyć następującego kodu w `Startup.ConfigureServices`:
 
```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver =
            new DefaultContractResolver();
    });
```

W kliencie .NET ta sama Metoda rozszerzenia `AddJsonProtocol` istnieje w [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder). Aby można było rozpoznać metodę rozszerzenia, należy zaimportować przestrzeń nazw `Microsoft.Extensions.DependencyInjection`:

```csharp
// At the top of the file:
using Microsoft.Extensions.DependencyInjection;
 
// When constructing your connection:
var connection = new HubConnectionBuilder()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver =
            new DefaultContractResolver();
    })
    .Build();
```

::: moniker-end

> [!NOTE]
> W tej chwili nie można skonfigurować serializacji JSON w kliencie JavaScript.

### <a name="messagepack-serialization-options"></a>Opcje serializacji MessagePack

Serializacji MessagePack można skonfigurować, dostarczając delegata do wywołania [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) . Aby uzyskać więcej informacji, zobacz [MessagePack w sygnalizacji](xref:signalr/messagepackhubprotocol) .

> [!NOTE]
> W tej chwili nie można skonfigurować serializacji MessagePack w kliencie JavaScript.

## <a name="configure-server-options"></a>Konfigurowanie opcji serwera

W poniższej tabeli opisano opcje konfigurowania centrów sygnałów:

::: moniker range=">= aspnetcore-3.0"

| Opcja | Wartość domyślna | Opis |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | 30 sekund | Serwer Rozważ odłączenie klienta, jeśli nie odebrano komunikatu (w tym w tym interwale). Może upłynąć dłużej niż ten interwał limitu czasu, aby klient mógł zostać oznaczony jako odłączony, ze względu na to, jak jest to zaimplementowane. Zalecaną wartością jest podwójna wartość `KeepAliveInterval`.|
| `HandshakeTimeout` | 15 sekund | Jeśli klient nie wyśle początkowego komunikatu uzgadniania w tym przedziale czasu, połączenie zostanie zamknięte. Jest to ustawienie zaawansowane, które należy zmodyfikować tylko w przypadku wystąpienia błędów limitu czasu uzgadniania z powodu poważnego opóźnienia sieci. Aby uzyskać więcej szczegółów na temat procesu uzgadniania, zobacz [specyfikację protokołu centrum sygnału](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md). |
| `KeepAliveInterval` | 15 sekund | Jeśli serwer nie wysłał komunikatu w tym interwale, zostanie automatycznie wysłana wiadomość ping w celu pozostawienia otwartego połączenia. Podczas zmiany `KeepAliveInterval` Zmień ustawienie `ServerTimeout` @ no__t-2 @ no__t-3 na kliencie. Zalecana wartość `ServerTimeout` @ no__t-1 @ no__t-2 jest podwójnie wartością `KeepAliveInterval`.  |
| `SupportedProtocols` | Wszystkie zainstalowane protokoły | Protokoły obsługiwane przez to centrum. Domyślnie wszystkie protokoły zarejestrowane na serwerze są dozwolone, ale protokoły można usunąć z tej listy, aby wyłączyć określone protokoły dla poszczególnych centrów. |
| `EnableDetailedErrors` | `false` | Jeśli `true`, szczegółowe komunikaty o wyjątkach są zwracane do klientów, gdy wyjątek jest zgłaszany w metodzie centrum. Wartość domyślna to `false`, ponieważ te komunikaty o wyjątkach mogą zawierać informacje poufne. |
| `StreamBufferCapacity` | `10` | Maksymalna liczba elementów, które mogą być buforowane dla strumieni przekazywania klientów. Po osiągnięciu tego limitu przetwarzanie wywołań jest blokowane, dopóki serwer nie przetwarza elementów strumienia.|
| `MaximumReceiveMessageSize` | 32 KB | Maksymalny rozmiar pojedynczego przychodzącego komunikatu centrum. |

::: moniker-end

::: moniker range="= aspnetcore-2.2"

| Opcja | Wartość domyślna | Opis |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | 30 sekund | Serwer Rozważ odłączenie klienta, jeśli nie odebrano komunikatu (w tym w tym interwale). Może upłynąć dłużej niż ten interwał limitu czasu, aby klient mógł zostać oznaczony jako odłączony, ze względu na to, jak jest to zaimplementowane. Zalecaną wartością jest podwójna wartość `KeepAliveInterval`.|
| `HandshakeTimeout` | 15 sekund | Jeśli klient nie wyśle początkowego komunikatu uzgadniania w tym przedziale czasu, połączenie zostanie zamknięte. Jest to ustawienie zaawansowane, które należy zmodyfikować tylko w przypadku wystąpienia błędów limitu czasu uzgadniania z powodu poważnego opóźnienia sieci. Aby uzyskać więcej szczegółów na temat procesu uzgadniania, zobacz [specyfikację protokołu centrum sygnału](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md). |
| `KeepAliveInterval` | 15 sekund | Jeśli serwer nie wysłał komunikatu w tym interwale, zostanie automatycznie wysłana wiadomość ping w celu pozostawienia otwartego połączenia. Podczas zmiany `KeepAliveInterval` Zmień ustawienie `ServerTimeout` @ no__t-2 @ no__t-3 na kliencie. Zalecana wartość `ServerTimeout` @ no__t-1 @ no__t-2 jest podwójnie wartością `KeepAliveInterval`.  |
| `SupportedProtocols` | Wszystkie zainstalowane protokoły | Protokoły obsługiwane przez to centrum. Domyślnie wszystkie protokoły zarejestrowane na serwerze są dozwolone, ale protokoły można usunąć z tej listy, aby wyłączyć określone protokoły dla poszczególnych centrów. |
| `EnableDetailedErrors` | `false` | Jeśli `true`, szczegółowe komunikaty o wyjątkach są zwracane do klientów, gdy wyjątek jest zgłaszany w metodzie centrum. Wartość domyślna to `false`, ponieważ te komunikaty o wyjątkach mogą zawierać informacje poufne. |

::: moniker-end

::: moniker range="= aspnetcore-2.1"

| Opcja | Wartość domyślna | Opis |
| ------ | ------------- | ----------- |
| `HandshakeTimeout` | 15 sekund | Jeśli klient nie wyśle początkowego komunikatu uzgadniania w tym przedziale czasu, połączenie zostanie zamknięte. Jest to ustawienie zaawansowane, które należy zmodyfikować tylko w przypadku wystąpienia błędów limitu czasu uzgadniania z powodu poważnego opóźnienia sieci. Aby uzyskać więcej szczegółów na temat procesu uzgadniania, zobacz [specyfikację protokołu centrum sygnału](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md). |
| `KeepAliveInterval` | 15 sekund | Jeśli serwer nie wysłał komunikatu w tym interwale, zostanie automatycznie wysłana wiadomość ping w celu pozostawienia otwartego połączenia. Podczas zmiany `KeepAliveInterval` Zmień ustawienie `ServerTimeout` @ no__t-2 @ no__t-3 na kliencie. Zalecana wartość `ServerTimeout` @ no__t-1 @ no__t-2 jest podwójnie wartością `KeepAliveInterval`.  |
| `SupportedProtocols` | Wszystkie zainstalowane protokoły | Protokoły obsługiwane przez to centrum. Domyślnie wszystkie protokoły zarejestrowane na serwerze są dozwolone, ale protokoły można usunąć z tej listy, aby wyłączyć określone protokoły dla poszczególnych centrów. |
| `EnableDetailedErrors` | `false` | Jeśli `true`, szczegółowe komunikaty o wyjątkach są zwracane do klientów, gdy wyjątek jest zgłaszany w metodzie centrum. Wartość domyślna to `false`, ponieważ te komunikaty o wyjątkach mogą zawierać informacje poufne. |

::: moniker-end

Opcje można skonfigurować dla wszystkich centrów, dostarczając opcje delegatów wywołania `AddSignalR` w `Startup.ConfigureServices`.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSignalR(hubOptions =>
    {
        hubOptions.EnableDetailedErrors = true;
        hubOptions.KeepAliveInterval = TimeSpan.FromMinutes(1);
    });
}
```

Opcje jednego centrum przesłaniają opcje globalne podane w `AddSignalR` i można je skonfigurować przy użyciu <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

### <a name="advanced-http-configuration-options"></a>Zaawansowane opcje konfiguracji HTTP

::: moniker range=">= aspnetcore-3.0"

Użyj `HttpConnectionDispatcherOptions`, aby skonfigurować zaawansowane ustawienia związane z transportami i zarządzaniem buforem pamięci. Te opcje są konfigurowane przez przekazanie delegata do [MapHub @ no__t-1T >](/dotnet/api/microsoft.aspnetcore.builder.hubendpointroutebuilderextensions.maphub) w `Startup.Configure`.

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseRouting();

    app.UseEndpoints(endpoints =>
    {
        endpoints.MapHub<MyHub>("/myhub", options =>
        {
            options.Transports =
                HttpTransportType.WebSockets |
                HttpTransportType.LongPolling;
        });
    });
}
```
::: moniker-end

::: moniker range="<= aspnetcore-2.2"

Użyj `HttpConnectionDispatcherOptions`, aby skonfigurować zaawansowane ustawienia związane z transportami i zarządzaniem buforem pamięci. Te opcje są konfigurowane przez przekazanie delegata do [MapHub @ no__t-1T >](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) w `Startup.Configure`.

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseSignalR((configure) =>
    {
        var desiredTransports =
            HttpTransportType.WebSockets |
            HttpTransportType.LongPolling;

        configure.MapHub<MyHub>("/myhub", (options) =>
        {
            options.Transports = desiredTransports;
        });
    });
}
```

::: moniker-end

W poniższej tabeli opisano opcje konfigurowania zaawansowanych opcji protokołu HTTP w programie ASP.NET Core Signal:

::: moniker range=">= aspnetcore-3.0"

| Opcja | Wartość domyślna | Opis |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | 32 KB | Maksymalna liczba bajtów odebranych od klienta, które buforują serwer przed zastosowaniem nadużycia. Zwiększenie tej wartości umożliwia serwerowi otrzymywanie większych komunikatów szybciej bez naciskania, ale może zwiększyć zużycie pamięci. |
| `AuthorizationData` | Dane zbierane automatycznie z atrybutów `Authorize` zastosowanych do klasy centrum. | Lista obiektów [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) służąca do określenia, czy klient jest autoryzowany do łączenia się z centrum. |
| `TransportMaxBufferSize` | 32 KB | Maksymalna liczba bajtów wysłanych przez aplikację, które buforuje serwer przed zaobserwowaniem presji. Zwiększenie tej wartości umożliwia serwerowi przebuforowanie większych komunikatów szybciej bez czekania na obciążenie, ale może zwiększyć zużycie pamięci. |
| `Transports` | Wszystkie transporty są włączone. | Wyliczenia flag bitowych wartości `HttpTransportType`, które mogą ograniczyć transporty, których klient może użyć do nawiązania połączenia. |
| `LongPolling` | Zobacz poniżej. | Dodatkowe opcje specyficzne dla długiego transportu sondowania. |
| `WebSockets` | Zobacz poniżej. | Dodatkowe opcje specyficzne dla transportu obiektów WebSockets. |

::: moniker-end

::: moniker range="< aspnetcore-3.0"

| Opcja | Wartość domyślna | Opis |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | 32 KB | Maksymalna liczba bajtów odebranych od klienta, które buforuje serwer. Zwiększenie tej wartości umożliwia serwerowi otrzymywanie większych komunikatów, ale może mieć negatywny wpływ na użycie pamięci. |
| `AuthorizationData` | Dane zbierane automatycznie z atrybutów `Authorize` zastosowanych do klasy centrum. | Lista obiektów [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) służąca do określenia, czy klient jest autoryzowany do łączenia się z centrum. |
| `TransportMaxBufferSize` | 32 KB | Maksymalna liczba bajtów wysłanych przez aplikację, które buforują serwer. Zwiększenie tej wartości umożliwia serwerowi wysyłanie większych komunikatów, ale może mieć negatywny wpływ na użycie pamięci. |
| `Transports` | Wszystkie transporty są włączone. | Wyliczenia flag bitowych wartości `HttpTransportType`, które mogą ograniczyć transporty, których klient może użyć do nawiązania połączenia. |
| `LongPolling` | Zobacz poniżej. | Dodatkowe opcje specyficzne dla długiego transportu sondowania. |
| `WebSockets` | Zobacz poniżej. | Dodatkowe opcje specyficzne dla transportu obiektów WebSockets. |

::: moniker-end

W przypadku długiego transportu sondowania dostępne są dodatkowe opcje, które można skonfigurować za pomocą właściwości `LongPolling`:

| Opcja | Wartość domyślna | Opis |
| ------ | ------------- | ----------- |
| `PollTimeout` | 90 sekund | Maksymalny czas, przez jaki serwer czeka na wysłanie wiadomości do klienta przed zakończeniem jednego żądania sondowania. Zmniejszenie tej wartości powoduje, że klient wystawia nowe żądania sondy częściej. |

Transport WebSocket ma dodatkowe opcje, które można skonfigurować za pomocą właściwości `WebSockets`:

| Opcja | Wartość domyślna | Opis |
| ------ | ------------- | ----------- |
| `CloseTimeout` | 5 sekund | Gdy serwer zostanie zamknięty, jeśli klient nie zostanie zamknięty w tym okresie, połączenie zostanie przerwane. |
| `SubProtocolSelector` | `null` | Delegat, którego można użyć do ustawienia nagłówka `Sec-WebSocket-Protocol` na wartość niestandardową. Delegat otrzymuje wartości żądane przez klienta jako dane wejściowe i oczekuje na zwrócenie żądanej wartości. |

## <a name="configure-client-options"></a>Konfigurowanie opcji klienta

Opcje klienta można skonfigurować dla typu `HubConnectionBuilder` (dostępnego w klientach .NET i JavaScript). Jest ona również dostępna w kliencie Java, ale podklasa `HttpHubConnectionBuilder` zawiera opcje konfiguracji konstruktora, a także `HubConnection`.

### <a name="configure-logging"></a>Konfigurowanie rejestrowania

Rejestrowanie jest konfigurowane w kliencie .NET przy użyciu metody `ConfigureLogging`. Dostawcy i filtry rejestrowania mogą być rejestrowane w taki sam sposób, w jaki znajdują się na serwerze. Aby uzyskać więcej informacji, zobacz dokumentację dotyczącą [rejestrowania w ASP.NET Core](xref:fundamentals/logging/index) .

> [!NOTE]
> Aby zarejestrować dostawców rejestrowania, należy zainstalować wymagane pakiety. Aby zapoznać się z pełną listą, zobacz sekcję [wbudowane dostawcy rejestrowania](xref:fundamentals/logging/index#built-in-logging-providers) w witrynie docs.

Aby na przykład włączyć rejestrowanie konsoli, zainstaluj pakiet NuGet `Microsoft.Extensions.Logging.Console`. Wywołaj metodę rozszerzenia `AddConsole`:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

W kliencie JavaScript istnieje podobna Metoda `configureLogging`. Podaj wartość `LogLevel` wskazującą minimalny poziom komunikatów dziennika do wygenerowania. Dzienniki są zapisywane w oknie konsoli przeglądarki.

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

::: moniker range=">= aspnetcore-3.0"

Zamiast wartości `LogLevel` można także podać wartość `string` reprezentującą nazwę poziomu dziennika. Jest to przydatne podczas konfigurowania rejestrowania sygnałów w środowiskach, w których nie masz dostępu do stałych `LogLevel`.

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging("warn")
    .build();
```

Poniższa tabela zawiera listę dostępnych poziomów dzienników. Wartość `configureLogging` ustawia **minimalny** poziom rejestrowania, który zostanie zarejestrowany. Komunikaty zarejestrowane na tym poziomie **lub poziomy wymienione po nim w tabeli**zostaną zarejestrowane.

| Ciąg                      | logLevel               |
| --------------------------- | ---------------------- |
| `trace`                     | `LogLevel.Trace`       |
| `debug`                     | `LogLevel.Debug`       |
| `info` **lub** `information` | `LogLevel.Information` |
| `warn` **lub** `warning`     | `LogLevel.Warning`     |
| `error`                     | `LogLevel.Error`       |
| `critical`                  | `LogLevel.Critical`    |
| `none`                      | `LogLevel.None`        |

::: moniker-end

> [!NOTE]
> Aby całkowicie wyłączyć rejestrowanie, określ `signalR.LogLevel.None` w metodzie `configureLogging`.

Aby uzyskać więcej informacji na temat rejestrowania, zobacz [dokumentację diagnostyki sygnału](xref:signalr/diagnostics).

Klient Java sygnalizujący używa biblioteki [SLF4J](https://www.slf4j.org/) do rejestrowania. Jest to interfejs API rejestrowania wysokiego poziomu, który umożliwia użytkownikom biblioteki wybranie własnych implementacji rejestrowania przez nałożenie określonej zależności rejestrowania. Poniższy fragment kodu przedstawia sposób używania `java.util.logging` z klientem Java sygnalizującego.

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

Jeśli nie skonfigurujesz rejestrowania w zależnościach, SLF4J ładuje domyślny Rejestrator braku operacji z następującym komunikatem ostrzegawczym:

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

Można je bezpiecznie zignorować.

### <a name="configure-allowed-transports"></a>Skonfiguruj dozwolone transporty

Transporty używane przez sygnalizującego można skonfigurować w wywołaniu `WithUrl` (`withUrl` w języku JavaScript). Wartości `HttpTransportType` można używać w celu ograniczenia liczby klientów do używania tylko określonych transportów. Wszystkie transporty są domyślnie włączone.

Na przykład, aby wyłączyć transport zdarzeń wysłanych przez serwer, ale zezwolić na połączenia z usługą WebSockets i długie sondowanie:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

W kliencie JavaScript transporty są konfigurowane przez ustawienie pola `transport` w obiekcie Options udostępnionym do `withUrl`:

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

::: moniker range=">= aspnetcore-2.2"

W tej wersji elementu WebSockets klienta Java jest jedynym dostępnym transportem.

::: moniker-end

::: moniker range="= aspnetcore-3.0"

W kliencie Java, transport jest wybierany z metodą `withTransport` na `HttpHubConnectionBuilder`. Klient Java domyślnie używa transportu usługi WebSockets.

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withTransport(TransportEnum.WEBSOCKETS)
    .build();
```

> [!NOTE]
> Klient Java sygnalizujący nie obsługuje jeszcze rezerwy transportowej.

::: moniker-end

### <a name="configure-bearer-authentication"></a>Konfigurowanie uwierzytelniania okaziciela

Aby zapewnić dane uwierzytelniania wraz z żądaniami sygnałów, użyj opcji `AccessTokenProvider` (`accessTokenFactory` w języku JavaScript), aby określić funkcję, która zwraca żądany token dostępu. W kliencie .NET token dostępu jest przenoszona jako token "uwierzytelnianie okaziciela" protokołu HTTP (przy użyciu nagłówka `Authorization` z typem `Bearer`). W kliencie JavaScript token dostępu jest używany jako token okaziciela, **z wyjątkiem** kilku przypadków, w których interfejsy API przeglądarki ograniczają możliwość zastosowania nagłówków (w konkretnym przypadku zdarzeń wysyłanych przez serwer i żądań WebSockets). W takich przypadkach token dostępu jest podawany jako wartość ciągu zapytania `access_token`.

W kliencie .NET opcja `AccessTokenProvider` może być określona za pomocą delegata opcji w `WithUrl`:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

W kliencie JavaScript token dostępu jest konfigurowany przez ustawienie pola `accessTokenFactory` w obiekcie Options w `withUrl`:

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        accessTokenFactory: () => {
            // Get and return the access token.
            // This function can return a JavaScript Promise if asynchronous
            // logic is required to retrieve the access token.
        }
    })
    .build();
```

W kliencie Java programu sygnalizującego można skonfigurować token okaziciela do użycia na potrzeby uwierzytelniania, dostarczając fabrykę tokenów dostępu do [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java). Użyj [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) , aby dostarczyć [RxJava](https://github.com/ReactiveX/RxJava) [pojedyncze @ no__t-> 3String](https://reactivex.io/documentation/single.html). Wywołanie metody [Single. Ustąp](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-)umożliwia zapisanie logiki w celu utworzenia tokenów dostępu dla klienta.

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

### <a name="configure-timeout-and-keep-alive-options"></a>Konfigurowanie opcji limitu czasu i utrzymywania aktywności

W samym obiekcie `HubConnection` dostępne są dodatkowe opcje konfigurowania trybu przekroczenia limitu czasu i zachowania utrzymywania aktywności:


::: moniker range=">= aspnetcore-2.2"

# <a name="nettabdotnet"></a>[.NET](#tab/dotnet)

| Opcja | Wartość domyślna | Opis |
| ------ | ------------- | ----------- |
| `ServerTimeout` | 30 sekund (30 000 MS) | Limit czasu dla działania serwera. Jeśli serwer nie wysłał komunikatu w tym interwale, Klient uważa, że serwer odłączył się i wyzwala zdarzenie `Closed` (`onclose` w języku JavaScript). Ta wartość musi być wystarczająca do wysłania wiadomości ping z serwera **i** odebrana przez klienta w ramach przedziału czasu. Zalecana wartość to liczba równa co najmniej podwójnej wartości @no__t na serwerze, aby umożliwić czas na nadejście poleceń ping. |
| `HandshakeTimeout` | 15 sekund | Limit czasu początkowej uzgadniania z serwerem. Jeśli serwer nie wyśle odpowiedzi uzgadniania w tym interwale, klient anuluje uzgadnianie i wyzwala zdarzenie `Closed` (`onclose` w języku JavaScript). Jest to ustawienie zaawansowane, które należy zmodyfikować tylko w przypadku wystąpienia błędów limitu czasu uzgadniania z powodu poważnego opóźnienia sieci. Aby uzyskać więcej szczegółów na temat procesu uzgadniania, zobacz [specyfikację protokołu centrum sygnału](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md). |
| `KeepAliveInterval` | 15 sekund | Określa interwał wysyłania komunikatów ping przez klienta. Wysłanie wszelkich komunikatów z klienta powoduje zresetowanie czasomierza do początku interwału. Jeśli klient nie wysłał komunikatu w `ClientTimeoutInterval` ustawionym na serwerze, serwer traktuje klienta jako odłączony. |

W kliencie .NET wartości limitu czasu są określane jako wartości `TimeSpan`.

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

| Opcja | Wartość domyślna | Opis |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | 30 sekund (30 000 MS) | Limit czasu dla działania serwera. Jeśli serwer nie wysłał komunikatu w tym interwale, Klient uważa, że serwer odłączył się i wyzwala zdarzenie `onclose`. Ta wartość musi być wystarczająca do wysłania wiadomości ping z serwera **i** odebrana przez klienta w ramach przedziału czasu. Zalecana wartość to liczba równa co najmniej podwójnej wartości @no__t na serwerze, aby umożliwić czas na nadejście poleceń ping. |
| `keepAliveIntervalInMilliseconds` | 15 sekund (15 000 milisekund) | Określa interwał wysyłania komunikatów ping przez klienta. Wysłanie wszelkich komunikatów z klienta powoduje zresetowanie czasomierza do początku interwału. Jeśli klient nie wysłał komunikatu w `ClientTimeoutInterval` ustawionym na serwerze, serwer traktuje klienta jako odłączony. |

# <a name="javatabjava"></a>[Java](#tab/java)

| Opcja | Wartość domyślna | Opis |
| ------ | ------------- | ----------- |
| `getServerTimeout` / `setServerTimeout` | 30 sekund (30 000 MS) | Limit czasu dla działania serwera. Jeśli serwer nie wysłał komunikatu w tym interwale, Klient uważa, że serwer odłączył się i wyzwala zdarzenie `onClose`. Ta wartość musi być wystarczająca do wysłania wiadomości ping z serwera **i** odebrana przez klienta w ramach przedziału czasu. Zalecana wartość to liczba równa co najmniej podwójnej wartości @no__t na serwerze, aby umożliwić czas na nadejście poleceń ping. |
| `withHandshakeResponseTimeout` | 15 sekund | Limit czasu początkowej uzgadniania z serwerem. Jeśli serwer nie wyśle odpowiedzi uzgadniania w tym interwale, klient anuluje uzgadnianie i wyzwala zdarzenie `onClose`. Jest to ustawienie zaawansowane, które należy zmodyfikować tylko w przypadku wystąpienia błędów limitu czasu uzgadniania z powodu poważnego opóźnienia sieci. Aby uzyskać więcej szczegółów na temat procesu uzgadniania, zobacz [specyfikację protokołu centrum sygnału](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md). |
| `getKeepAliveInterval` / `setKeepAliveInterval` | 15 sekund (15 000 MS) | Określa interwał wysyłania komunikatów ping przez klienta. Wysłanie wszelkich komunikatów z klienta powoduje zresetowanie czasomierza do początku interwału. Jeśli klient nie wysłał komunikatu w `ClientTimeoutInterval` ustawionym na serwerze, serwer traktuje klienta jako odłączony. |

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="nettabdotnet"></a>[.NET](#tab/dotnet)

| Opcja | Wartość domyślna | Opis |
| ------ | ------------- | ----------- |
| `ServerTimeout` | 30 sekund (30 000 MS) | Limit czasu dla działania serwera. Jeśli serwer nie wysłał komunikatu w tym interwale, Klient uważa, że serwer odłączył się i wyzwala zdarzenie `Closed` (`onclose` w języku JavaScript). Ta wartość musi być wystarczająca do wysłania wiadomości ping z serwera **i** odebrana przez klienta w ramach przedziału czasu. Zalecana wartość to liczba równa co najmniej podwójnej wartości @no__t na serwerze, aby umożliwić czas na nadejście poleceń ping. |
| `HandshakeTimeout` | 15 sekund | Limit czasu początkowej uzgadniania z serwerem. Jeśli serwer nie wyśle odpowiedzi uzgadniania w tym interwale, klient anuluje uzgadnianie i wyzwala zdarzenie `Closed` (`onclose` w języku JavaScript). Jest to ustawienie zaawansowane, które należy zmodyfikować tylko w przypadku wystąpienia błędów limitu czasu uzgadniania z powodu poważnego opóźnienia sieci. Aby uzyskać więcej szczegółów na temat procesu uzgadniania, zobacz [specyfikację protokołu centrum sygnału](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md). |

W kliencie .NET wartości limitu czasu są określane jako wartości `TimeSpan`.

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

| Opcja | Wartość domyślna | Opis |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | 30 sekund (30 000 MS) | Limit czasu dla działania serwera. Jeśli serwer nie wysłał komunikatu w tym interwale, Klient uważa, że serwer odłączył się i wyzwala zdarzenie `onclose`. Ta wartość musi być wystarczająca do wysłania wiadomości ping z serwera **i** odebrana przez klienta w ramach przedziału czasu. Zalecana wartość to liczba równa co najmniej podwójnej wartości @no__t na serwerze, aby umożliwić czas na nadejście poleceń ping. |

# <a name="javatabjava"></a>[Java](#tab/java)

| Opcja | Wartość domyślna | Opis |
| ------ | ------------- | ----------- |
| `getServerTimeout` / `setServerTimeout` | 30 sekund (30 000 MS) | Limit czasu dla działania serwera. Jeśli serwer nie wysłał komunikatu w tym interwale, Klient uważa, że serwer odłączył się i wyzwala zdarzenie `onClose`. Ta wartość musi być wystarczająca do wysłania wiadomości ping z serwera **i** odebrana przez klienta w ramach przedziału czasu. Zalecana wartość to liczba równa co najmniej podwójnej wartości `KeepAliveInterval` serwera, aby umożliwić czas na nadejście poleceń ping. |
| `withHandshakeResponseTimeout` | 15 sekund | Limit czasu początkowej uzgadniania z serwerem. Jeśli serwer nie wyśle odpowiedzi uzgadniania w tym interwale, klient anuluje uzgadnianie i wyzwala zdarzenie `onClose`. Jest to ustawienie zaawansowane, które należy zmodyfikować tylko w przypadku wystąpienia błędów limitu czasu uzgadniania z powodu poważnego opóźnienia sieci. Aby uzyskać więcej szczegółów na temat procesu uzgadniania, zobacz [specyfikację protokołu centrum sygnału](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md). |

::: moniker-end

---

### <a name="configure-additional-options"></a>Skonfiguruj dodatkowe opcje

Dodatkowe opcje można skonfigurować w metodzie `WithUrl` (`withUrl` w języku JavaScript) na `HubConnectionBuilder` lub w różnych interfejsach API konfiguracji na `HttpHubConnectionBuilder` w kliencie Java:

# <a name="nettabdotnet"></a>[.NET](#tab/dotnet)

| Opcja .NET |  Wartość domyślna | Opis |
| ----------- | -------------- | ----------- |
| `AccessTokenProvider` | `null` | Funkcja zwracająca ciąg, który jest dostarczany jako token uwierzytelniania okaziciela w żądaniach HTTP. |
| `SkipNegotiation` | `false` | Ustaw tę wartość na `true`, aby pominąć etap negocjacji. **Obsługiwane tylko wtedy, gdy transport Sockets jest jedynym włączonym transportem**. Nie można włączyć tego ustawienia w przypadku korzystania z usługi Azure Signal Service. |
| `ClientCertificates` | Pusty | Kolekcja certyfikatów TLS do wysłania w celu uwierzytelniania żądań. |
| `Cookies` | Pusty | Kolekcja plików cookie protokołu HTTP do wysłania w każdym żądaniu HTTP. |
| `Credentials` | Pusty | Poświadczenia do wysyłania przy każdym żądaniu HTTP. |
| `CloseTimeout` | 5 sekund | Tylko obiekty WebSockets. Maksymalny czas oczekiwania klienta po zamknięciu serwera w celu potwierdzenia żądania zamknięcia. Jeśli serwer nie potwierdzi zamknięcia w tym czasie, klient zostanie rozłączony. |
| `Headers` | Pusty | Mapa dodatkowych nagłówków HTTP do wysłania w każdym żądaniu HTTP. |
| `HttpMessageHandlerFactory` | `null` | Delegat, który może służyć do konfigurowania lub zastępowania `HttpMessageHandler` używanych do wysyłania żądań HTTP. Nieużywane na potrzeby połączeń protokołu WebSocket. Ten delegat musi zwracać wartość różną od null i otrzymuje wartość domyślną jako parametr. Zmodyfikuj ustawienia tej wartości domyślnej i zwróć ją lub Zwróć nowe wystąpienie `HttpMessageHandler`. **Podczas zastępowania programu obsługi należy skopiować ustawienia, które mają być zachowane z podanej procedury obsługi. w przeciwnym razie skonfigurowane opcje (takie jak pliki cookie i nagłówki) nie będą miały zastosowania do nowego programu obsługi.** |
| `Proxy` | `null` | Serwer proxy HTTP, który ma być używany podczas wysyłania żądań HTTP. |
| `UseDefaultCredentials` | `false` | Ustaw tę wartość logiczną, aby wysyłać domyślne poświadczenia dla żądań HTTP i WebSockets. Umożliwia to korzystanie z uwierzytelniania systemu Windows. |
| `WebSocketConfiguration` | `null` | Delegat, który może służyć do konfigurowania dodatkowych opcji protokołu WebSocket. Odbiera wystąpienie elementu [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) , którego można użyć do skonfigurowania opcji. |

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

| Opcja języka JavaScript | Wartość domyślna | Opis |
| ----------------- | ------------- | ----------- |
| `accessTokenFactory` | `null` | Funkcja zwracająca ciąg, który jest dostarczany jako token uwierzytelniania okaziciela w żądaniach HTTP. |
| `skipNegotiation` | `false` | Ustaw tę wartość na `true`, aby pominąć etap negocjacji. **Obsługiwane tylko wtedy, gdy transport Sockets jest jedynym włączonym transportem**. Nie można włączyć tego ustawienia w przypadku korzystania z usługi Azure Signal Service. |

# <a name="javatabjava"></a>[Java](#tab/java)

| Opcja Java | Wartość domyślna | Opis |
| ----------- | ------------- | ----------- |
| `withAccessTokenProvider` | `null` | Funkcja zwracająca ciąg, który jest dostarczany jako token uwierzytelniania okaziciela w żądaniach HTTP. |
| `shouldSkipNegotiate` | `false` | Ustaw tę wartość na `true`, aby pominąć etap negocjacji. **Obsługiwane tylko wtedy, gdy transport Sockets jest jedynym włączonym transportem**. Nie można włączyć tego ustawienia w przypadku korzystania z usługi Azure Signal Service. |
| `withHeader` `withHeaders` | Pusty | Mapa dodatkowych nagłówków HTTP do wysłania w każdym żądaniu HTTP. |

---

W kliencie .NET opcje te mogą być modyfikowane przez delegata opcji udostępnionych dla `WithUrl`:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

W kliencie JavaScript te opcje można podać w obiekcie JavaScript dostarczonym do `withUrl`:

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

W kliencie Java Opcje te można skonfigurować przy użyciu metod w `HttpHubConnectionBuilder` zwracanych z `HubConnectionBuilder.create("HUB URL")`

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
        .withHeader("Foo", "Bar")
        .shouldSkipNegotiate(true)
        .withHandshakeResponseTimeout(30*1000)
        .build();
```

## <a name="additional-resources"></a>Zasoby dodatkowe

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>
