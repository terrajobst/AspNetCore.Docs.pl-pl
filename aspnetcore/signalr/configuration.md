---
title: Konfiguracja SignalR ASP.NET Core
author: bradygaster
description: Dowiedz się, jak skonfigurować aplikacje SignalR ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- SignalR
uid: signalr/configuration
ms.openlocfilehash: 682cc36216a4dc9b38c87b4f67100ab565a70e5c
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/12/2019
ms.locfileid: "73963983"
---
# <a name="aspnet-core-opno-locsignalr-configuration"></a>Konfiguracja SignalR ASP.NET Core

## <a name="jsonmessagepack-serialization-options"></a>Opcje serializacji JSON/MessagePack

ASP.NET Core SignalR obsługuje dwa protokoły dla komunikatów kodowania: [JSON](https://www.json.org/) i [MessagePack](https://msgpack.org/index.html). Każdy protokół ma opcje konfiguracji serializacji.

::: moniker range=">= aspnetcore-3.0"

Serializacji JSON można skonfigurować na serwerze przy użyciu metody rozszerzenia [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) . `AddJsonProtocol` można dodać po elemencie [Addsignaler](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) w `Startup.ConfigureServices`. Metoda `AddJsonProtocol` przyjmuje delegata, który odbiera obiekt `options`. Właściwość [PayloadSerializerOptions](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializeroptions) tego obiektu jest obiektem <xref:System.Text.Json.JsonSerializerOptions> `System.Text.Json`, którego można użyć do skonfigurowania serializacji argumentów i zwracanych wartości. Aby uzyskać więcej informacji, zobacz [dokumentację system. Text. JSON](/dotnet/api/system.text.json).

Przykładowo, aby skonfigurować Serializator, aby nie zmieniać wielkości liter nazw właściwości zamiast domyślnych nazw "camelCase", należy użyć następującego kodu w `Startup.ConfigureServices`:

```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerOptions.PropertyNamingPolicy = null
    });
```

W kliencie .NET ta sama Metoda rozszerzenia `AddJsonProtocol` istnieje w [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder). Aby rozwiązać metodę rozszerzenia, należy zaimportować przestrzeń nazw `Microsoft.Extensions.DependencyInjection`:

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

> [!NOTE]
> W tej chwili nie można skonfigurować serializacji JSON w kliencie JavaScript.

### <a name="switch-to-newtonsoftjson"></a>Przejdź do pliku Newtonsoft. JSON

Jeśli potrzebujesz funkcji `Newtonsoft.Json`, które nie są obsługiwane w `System.Text.Json`, zobacz [Switch to Newtonsoft. JSON](xref:migration/22-to-30#switch-to-newtonsoftjson).

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

Serializacji JSON można skonfigurować na serwerze przy użyciu metody rozszerzenia [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) , która może zostać dodana po metodzie [addsygnalizująca](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) w `Startup.ConfigureServices`. Metoda `AddJsonProtocol` przyjmuje delegata, który odbiera obiekt `options`. Właściwość [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) tego obiektu jest obiektem JSON.NET `JsonSerializerSettings`, którego można użyć do skonfigurowania serializacji argumentów i zwracanych wartości. Aby uzyskać więcej informacji, zapoznaj się z [dokumentacją JSON.NET](https://www.newtonsoft.com/json/help/html/Introduction.htm).
 
Przykładowo, aby skonfigurować serializator do używania nazw właściwości "PascalCase" zamiast domyślnych nazw "camelCase", należy użyć następującego kodu w `Startup.ConfigureServices`:
 
```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver =
            new DefaultContractResolver();
    });
```

W kliencie .NET ta sama Metoda rozszerzenia `AddJsonProtocol` istnieje w [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder). Aby rozwiązać metodę rozszerzenia, należy zaimportować przestrzeń nazw `Microsoft.Extensions.DependencyInjection`:

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

> [!NOTE]
> W tej chwili nie można skonfigurować serializacji JSON w kliencie JavaScript.

::: moniker-end

### <a name="messagepack-serialization-options"></a>Opcje serializacji MessagePack

Serializacji MessagePack można skonfigurować, dostarczając delegata do wywołania [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) . Aby uzyskać więcej informacji, zobacz [MessagePack w SignalR](xref:signalr/messagepackhubprotocol) .

> [!NOTE]
> W tej chwili nie można skonfigurować serializacji MessagePack w kliencie JavaScript.

## <a name="configure-server-options"></a>Konfigurowanie opcji serwera

W poniższej tabeli opisano opcje konfigurowania centrów SignalR:

::: moniker range=">= aspnetcore-3.0"

| Opcja | Wartość domyślna | Opis |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | 30 sekund | Serwer Rozważ odłączenie klienta, jeśli nie odebrano komunikatu (w tym w tym interwale). Może upłynąć dłużej niż ten interwał limitu czasu, aby klient mógł zostać oznaczony jako odłączony, ze względu na to, jak jest to zaimplementowane. Zalecaną wartością jest podwójna wartość `KeepAliveInterval`.|
| `HandshakeTimeout` | 15 sekund | Jeśli klient nie wyśle początkowego komunikatu uzgadniania w tym przedziale czasu, połączenie zostanie zamknięte. Jest to ustawienie zaawansowane, które należy zmodyfikować tylko w przypadku wystąpienia błędów limitu czasu uzgadniania z powodu poważnego opóźnienia sieci. Aby uzyskać więcej szczegółów na temat procesu uzgadniania, zobacz [Specyfikacja protokołuSignalR Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md). |
| `KeepAliveInterval` | 15 sekund | Jeśli serwer nie wysłał komunikatu w tym interwale, zostanie automatycznie wysłana wiadomość ping w celu pozostawienia otwartego połączenia. Podczas zmiany `KeepAliveInterval`Zmień ustawienie `ServerTimeout`/`serverTimeoutInMilliseconds` na kliencie. Zalecana wartość `ServerTimeout`/`serverTimeoutInMilliseconds` jest podwójna `KeepAliveInterval` wartość.  |
| `SupportedProtocols` | Wszystkie zainstalowane protokoły | Protokoły obsługiwane przez to centrum. Domyślnie wszystkie protokoły zarejestrowane na serwerze są dozwolone, ale protokoły można usunąć z tej listy, aby wyłączyć określone protokoły dla poszczególnych centrów. |
| `EnableDetailedErrors` | `false` | Jeśli `true`, szczegółowe komunikaty o wyjątkach są zwracane do klientów, gdy wyjątek jest zgłaszany w metodzie centrum. Wartość domyślna to `false`, ponieważ te komunikaty o wyjątkach mogą zawierać informacje poufne. |
| `StreamBufferCapacity` | `10` | Maksymalna liczba elementów, które mogą być buforowane dla strumieni przekazywania klientów. Po osiągnięciu tego limitu przetwarzanie wywołań jest blokowane, dopóki serwer nie przetwarza elementów strumienia.|
| `MaximumReceiveMessageSize` | 32 KB | Maksymalny rozmiar pojedynczego przychodzącego komunikatu centrum. |

::: moniker-end

::: moniker range="= aspnetcore-2.2"

| Opcja | Wartość domyślna | Opis |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | 30 sekund | Serwer Rozważ odłączenie klienta, jeśli nie odebrano komunikatu (w tym w tym interwale). Może upłynąć dłużej niż ten interwał limitu czasu, aby klient mógł zostać oznaczony jako odłączony, ze względu na to, jak jest to zaimplementowane. Zalecaną wartością jest podwójna wartość `KeepAliveInterval`.|
| `HandshakeTimeout` | 15 sekund | Jeśli klient nie wyśle początkowego komunikatu uzgadniania w tym przedziale czasu, połączenie zostanie zamknięte. Jest to ustawienie zaawansowane, które należy zmodyfikować tylko w przypadku wystąpienia błędów limitu czasu uzgadniania z powodu poważnego opóźnienia sieci. Aby uzyskać więcej szczegółów na temat procesu uzgadniania, zobacz [Specyfikacja protokołuSignalR Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md). |
| `KeepAliveInterval` | 15 sekund | Jeśli serwer nie wysłał komunikatu w tym interwale, zostanie automatycznie wysłana wiadomość ping w celu pozostawienia otwartego połączenia. Podczas zmiany `KeepAliveInterval`Zmień ustawienie `ServerTimeout`/`serverTimeoutInMilliseconds` na kliencie. Zalecana wartość `ServerTimeout`/`serverTimeoutInMilliseconds` jest podwójna `KeepAliveInterval` wartość.  |
| `SupportedProtocols` | Wszystkie zainstalowane protokoły | Protokoły obsługiwane przez to centrum. Domyślnie wszystkie protokoły zarejestrowane na serwerze są dozwolone, ale protokoły można usunąć z tej listy, aby wyłączyć określone protokoły dla poszczególnych centrów. |
| `EnableDetailedErrors` | `false` | Jeśli `true`, szczegółowe komunikaty o wyjątkach są zwracane do klientów, gdy wyjątek jest zgłaszany w metodzie centrum. Wartość domyślna to `false`, ponieważ te komunikaty o wyjątkach mogą zawierać informacje poufne. |

::: moniker-end

::: moniker range="= aspnetcore-2.1"

| Opcja | Wartość domyślna | Opis |
| ------ | ------------- | ----------- |
| `HandshakeTimeout` | 15 sekund | Jeśli klient nie wyśle początkowego komunikatu uzgadniania w tym przedziale czasu, połączenie zostanie zamknięte. Jest to ustawienie zaawansowane, które należy zmodyfikować tylko w przypadku wystąpienia błędów limitu czasu uzgadniania z powodu poważnego opóźnienia sieci. Aby uzyskać więcej szczegółów na temat procesu uzgadniania, zobacz [Specyfikacja protokołuSignalR Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md). |
| `KeepAliveInterval` | 15 sekund | Jeśli serwer nie wysłał komunikatu w tym interwale, zostanie automatycznie wysłana wiadomość ping w celu pozostawienia otwartego połączenia. Podczas zmiany `KeepAliveInterval`Zmień ustawienie `ServerTimeout`/`serverTimeoutInMilliseconds` na kliencie. Zalecana wartość `ServerTimeout`/`serverTimeoutInMilliseconds` jest podwójna `KeepAliveInterval` wartość.  |
| `SupportedProtocols` | Wszystkie zainstalowane protokoły | Protokoły obsługiwane przez to centrum. Domyślnie wszystkie protokoły zarejestrowane na serwerze są dozwolone, ale protokoły można usunąć z tej listy, aby wyłączyć określone protokoły dla poszczególnych centrów. |
| `EnableDetailedErrors` | `false` | Jeśli `true`, szczegółowe komunikaty o wyjątkach są zwracane do klientów, gdy wyjątek jest zgłaszany w metodzie centrum. Wartość domyślna to `false`, ponieważ te komunikaty o wyjątkach mogą zawierać informacje poufne. |

::: moniker-end

Opcje można skonfigurować dla wszystkich centrów, dostarczając opcje delegata do wywołania `AddSignalR` w `Startup.ConfigureServices`.

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

Opcje jednego centrum przesłaniają opcje globalne dostępne w `AddSignalR` i można je skonfigurować za pomocą <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

### <a name="advanced-http-configuration-options"></a>Zaawansowane opcje konfiguracji HTTP

::: moniker range=">= aspnetcore-3.0"

Użyj `HttpConnectionDispatcherOptions`, aby skonfigurować zaawansowane ustawienia związane z transportami i zarządzaniem buforem pamięci. Te opcje są konfigurowane przez przekazanie delegata do [MapHub\<t >](/dotnet/api/microsoft.aspnetcore.builder.hubendpointroutebuilderextensions.maphub) w `Startup.Configure`.

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

Użyj `HttpConnectionDispatcherOptions`, aby skonfigurować zaawansowane ustawienia związane z transportami i zarządzaniem buforem pamięci. Te opcje są konfigurowane przez przekazanie delegata do [MapHub\<t >](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) w `Startup.Configure`.

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

W poniższej tabeli opisano opcje konfigurowania zaawansowanych opcji protokołu HTTP w programie ASP.NET Core SignalR:

::: moniker range=">= aspnetcore-3.0"

| Opcja | Wartość domyślna | Opis |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | 32 KB | Maksymalna liczba bajtów odebranych od klienta, które buforują serwer przed zastosowaniem nadużycia. Zwiększenie tej wartości umożliwia serwerowi otrzymywanie większych komunikatów szybciej bez naciskania, ale może zwiększyć zużycie pamięci. |
| `AuthorizationData` | Dane zbierane automatycznie z `Authorize` atrybuty zastosowane do klasy centrum. | Lista obiektów [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) służąca do określenia, czy klient jest autoryzowany do łączenia się z centrum. |
| `TransportMaxBufferSize` | 32 KB | Maksymalna liczba bajtów wysłanych przez aplikację, które buforuje serwer przed zaobserwowaniem presji. Zwiększenie tej wartości umożliwia serwerowi przebuforowanie większych komunikatów szybciej bez czekania na obciążenie, ale może zwiększyć zużycie pamięci. |
| `Transports` | Wszystkie transporty są włączone. | Wyliczenie flag bitowych wartości `HttpTransportType`, które mogą ograniczyć transporty, z których może korzystać klient do nawiązywania połączeń. |
| `LongPolling` | Zobacz poniżej. | Dodatkowe opcje specyficzne dla długiego transportu sondowania. |
| `WebSockets` | Zobacz poniżej. | Dodatkowe opcje specyficzne dla transportu obiektów WebSockets. |

::: moniker-end

::: moniker range="< aspnetcore-3.0"

| Opcja | Wartość domyślna | Opis |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | 32 KB | Maksymalna liczba bajtów odebranych od klienta, które buforuje serwer. Zwiększenie tej wartości umożliwia serwerowi otrzymywanie większych komunikatów, ale może mieć negatywny wpływ na użycie pamięci. |
| `AuthorizationData` | Dane zbierane automatycznie z `Authorize` atrybuty zastosowane do klasy centrum. | Lista obiektów [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) służąca do określenia, czy klient jest autoryzowany do łączenia się z centrum. |
| `TransportMaxBufferSize` | 32 KB | Maksymalna liczba bajtów wysłanych przez aplikację, które buforują serwer. Zwiększenie tej wartości umożliwia serwerowi wysyłanie większych komunikatów, ale może mieć negatywny wpływ na użycie pamięci. |
| `Transports` | Wszystkie transporty są włączone. | Wyliczenie flag bitowych wartości `HttpTransportType`, które mogą ograniczyć transporty, z których może korzystać klient do nawiązywania połączeń. |
| `LongPolling` | Zobacz poniżej. | Dodatkowe opcje specyficzne dla długiego transportu sondowania. |
| `WebSockets` | Zobacz poniżej. | Dodatkowe opcje specyficzne dla transportu obiektów WebSockets. |

::: moniker-end

W przypadku długiego transportu sondowania dostępne są dodatkowe opcje, które można skonfigurować przy użyciu właściwości `LongPolling`:

| Opcja | Wartość domyślna | Opis |
| ------ | ------------- | ----------- |
| `PollTimeout` | 90 sekund | Maksymalny czas, przez jaki serwer czeka na wysłanie wiadomości do klienta przed zakończeniem jednego żądania sondowania. Zmniejszenie tej wartości powoduje, że klient wystawia nowe żądania sondy częściej. |

Transport WebSocket ma dodatkowe opcje, które można skonfigurować za pomocą właściwości `WebSockets`:

| Opcja | Wartość domyślna | Opis |
| ------ | ------------- | ----------- |
| `CloseTimeout` | 5 sekund | Gdy serwer zostanie zamknięty, jeśli klient nie zostanie zamknięty w tym okresie, połączenie zostanie przerwane. |
| `SubProtocolSelector` | `null` | Delegat, którego można użyć do ustawienia nagłówka `Sec-WebSocket-Protocol` na wartość niestandardową. Delegat otrzymuje wartości żądane przez klienta jako dane wejściowe i oczekuje na zwrócenie żądanej wartości. |

## <a name="configure-client-options"></a>Konfigurowanie opcji klienta

Opcje klienta można skonfigurować dla typu `HubConnectionBuilder` (dostępnego w klientach .NET i JavaScript). Jest ona również dostępna w kliencie Java, ale podklasa `HttpHubConnectionBuilder` zawiera opcje konfiguracji konstruktora, a także `HubConnection` samego siebie.

### <a name="configure-logging"></a>Konfigurowanie rejestrowania

Rejestrowanie jest konfigurowane w kliencie .NET przy użyciu metody `ConfigureLogging`. Dostawcy i filtry rejestrowania mogą być rejestrowane w taki sam sposób, w jaki znajdują się na serwerze. Aby uzyskać więcej informacji, zobacz dokumentację dotyczącą [rejestrowania w ASP.NET Core](xref:fundamentals/logging/index) .

> [!NOTE]
> Aby zarejestrować dostawców rejestrowania, należy zainstalować wymagane pakiety. Aby zapoznać się z pełną listą, zobacz sekcję [wbudowane dostawcy rejestrowania](xref:fundamentals/logging/index#built-in-logging-providers) w witrynie docs.

Aby na przykład włączyć rejestrowanie konsoli, należy zainstalować pakiet NuGet `Microsoft.Extensions.Logging.Console`. Wywołaj metodę rozszerzenia `AddConsole`:

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

Zamiast wartości `LogLevel` można również podać wartość `string` reprezentującą nazwę poziomu dziennika. Jest to przydatne podczas konfigurowania rejestrowania SignalR w środowiskach, w których nie masz dostępu do stałych `LogLevel`.

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging("warn")
    .build();
```

Poniższa tabela zawiera listę dostępnych poziomów dzienników. Wybrana wartość `configureLogging` ustawia **minimalny** poziom dziennika, który zostanie zarejestrowany. Komunikaty zarejestrowane na tym poziomie **lub poziomy wymienione po nim w tabeli**zostaną zarejestrowane.

| String                      | LogLevel               |
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
> Aby całkowicie wyłączyć rejestrowanie, określ `signalR.LogLevel.None` w `configureLogging` metodzie.

Aby uzyskać więcej informacji na temat rejestrowania, zobacz [dokumentację diagnostykiSignalR](xref:signalr/diagnostics).

Klient języka Java SignalR używa biblioteki [SLF4J](https://www.slf4j.org/) do rejestrowania. Jest to interfejs API rejestrowania wysokiego poziomu, który umożliwia użytkownikom biblioteki wybranie własnych implementacji rejestrowania przez nałożenie określonej zależności rejestrowania. Poniższy fragment kodu przedstawia sposób korzystania z `java.util.logging` z klientem SignalR Java.

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

Transporty używane przez SignalR można skonfigurować w wywołaniu `WithUrl` (`withUrl` w języku JavaScript). Wartości `HttpTransportType` mogą być używane w celu ograniczenia klienta do używania tylko określonych transportów. Wszystkie transporty są domyślnie włączone.

Na przykład, aby wyłączyć transport zdarzeń wysłanych przez serwer, ale zezwolić na połączenia z usługą WebSockets i długie sondowanie:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

W kliencie JavaScript transporty są konfigurowane przez ustawienie pola `transport` w obiekcie Options udostępnianym `withUrl`:

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

::: moniker range=">= aspnetcore-2.2"

W tej wersji elementu WebSockets klienta Java jest jedynym dostępnym transportem.

::: moniker-end

::: moniker range="= aspnetcore-3.0"

W kliencie Java, transport jest wybierany z metodą `withTransport`ową `HttpHubConnectionBuilder`. Klient Java domyślnie używa transportu usługi WebSockets.

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withTransport(TransportEnum.WEBSOCKETS)
    .build();
```

> [!NOTE]
> Klient języka Java SignalR nie obsługuje jeszcze rezerwy transportowej.

::: moniker-end

### <a name="configure-bearer-authentication"></a>Konfigurowanie uwierzytelniania okaziciela

Aby zapewnić dane uwierzytelniania wraz z SignalR żądaniami, użyj opcji `AccessTokenProvider` (`accessTokenFactory` w języku JavaScript), aby określić funkcję, która zwraca żądany token dostępu. W kliencie .NET token dostępu jest przenoszona jako token "uwierzytelnianie okaziciela" protokołu HTTP (przy użyciu nagłówka `Authorization` z typem `Bearer`). W kliencie JavaScript token dostępu jest używany jako token okaziciela, **z wyjątkiem** kilku przypadków, w których interfejsy API przeglądarki ograniczają możliwość zastosowania nagłówków (w konkretnym przypadku zdarzeń wysyłanych przez serwer i żądań WebSockets). W takich przypadkach token dostępu jest dostarczany jako wartość ciągu zapytania `access_token`.

W kliencie .NET opcja `AccessTokenProvider` można określić za pomocą delegata opcji w `WithUrl`:

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

W kliencie SignalR Java można skonfigurować token okaziciela do użycia na potrzeby uwierzytelniania, dostarczając fabrykę tokenów dostępu do [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java). Użyj [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) , aby podać [](https://github.com/ReactiveX/RxJava) [\<pojedynczego ciągu](https://reactivex.io/documentation/single.html). Wywołanie metody [Single. Ustąp](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-)umożliwia zapisanie logiki w celu utworzenia tokenów dostępu dla klienta.

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

### <a name="configure-timeout-and-keep-alive-options"></a>Konfigurowanie opcji limitu czasu i utrzymywania aktywności

W celu skonfigurowania zachowania przekroczenia limitu czasu i utrzymywania aktywności są dostępne dodatkowe opcje dla samego obiektu `HubConnection`:


::: moniker range=">= aspnetcore-2.2"

# <a name="nettabdotnet"></a>[.NET](#tab/dotnet)

| Opcja | Wartość domyślna | Opis |
| ------ | ------------- | ----------- |
| `ServerTimeout` | 30 sekund (30 000 MS) | Limit czasu dla działania serwera. Jeśli serwer nie wysłał komunikatu w tym interwale, Klient uważa, że serwer odłączył się i wyzwala zdarzenie `Closed` (`onclose` w języku JavaScript). Ta wartość musi być wystarczająca do wysłania wiadomości ping z serwera **i** odebrana przez klienta w ramach przedziału czasu. Zalecana wartość to liczba z co najmniej podwójną wartością `KeepAliveInterval` serwera, aby umożliwić czas na nadejście poleceń ping. |
| `HandshakeTimeout` | 15 sekund | Limit czasu początkowej uzgadniania z serwerem. Jeśli serwer nie wyśle odpowiedzi uzgadniania w tym interwale, klient anuluje uzgadnianie i wyzwala zdarzenie `Closed` (`onclose` w języku JavaScript). Jest to ustawienie zaawansowane, które należy zmodyfikować tylko w przypadku wystąpienia błędów limitu czasu uzgadniania z powodu poważnego opóźnienia sieci. Aby uzyskać więcej szczegółów na temat procesu uzgadniania, zobacz [Specyfikacja protokołuSignalR Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md). |
| `KeepAliveInterval` | 15 sekund | Określa interwał wysyłania komunikatów ping przez klienta. Wysłanie wszelkich komunikatów z klienta powoduje zresetowanie czasomierza do początku interwału. Jeśli klient nie wyśle komunikatu w `ClientTimeoutInterval` zestawie na serwerze, serwer traktuje klienta jako odłączony. |

W kliencie .NET wartości limitu czasu są określane jako wartości `TimeSpan`.

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

| Opcja | Wartość domyślna | Opis |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | 30 sekund (30 000 MS) | Limit czasu dla działania serwera. Jeśli serwer nie wysłał komunikatu w tym interwale, Klient uważa, że serwer odłączył się i wyzwala zdarzenie `onclose`. Ta wartość musi być wystarczająca do wysłania wiadomości ping z serwera **i** odebrana przez klienta w ramach przedziału czasu. Zalecana wartość to liczba z co najmniej podwójną wartością `KeepAliveInterval` serwera, aby umożliwić czas na nadejście poleceń ping. |
| `keepAliveIntervalInMilliseconds` | 15 sekund (15 000 milisekund) | Określa interwał wysyłania komunikatów ping przez klienta. Wysłanie wszelkich komunikatów z klienta powoduje zresetowanie czasomierza do początku interwału. Jeśli klient nie wyśle komunikatu w `ClientTimeoutInterval` zestawie na serwerze, serwer traktuje klienta jako odłączony. |

# <a name="javatabjava"></a>[Java](#tab/java)

| Opcja | Wartość domyślna | Opis |
| ------ | ------------- | ----------- |
| `getServerTimeout` / `setServerTimeout` | 30 sekund (30 000 MS) | Limit czasu dla działania serwera. Jeśli serwer nie wysłał komunikatu w tym interwale, Klient uważa, że serwer odłączył się i wyzwala zdarzenie `onClose`. Ta wartość musi być wystarczająca do wysłania wiadomości ping z serwera **i** odebrana przez klienta w ramach przedziału czasu. Zalecana wartość to liczba z co najmniej podwójną wartością `KeepAliveInterval` serwera, aby umożliwić czas na nadejście poleceń ping. |
| `withHandshakeResponseTimeout` | 15 sekund | Limit czasu początkowej uzgadniania z serwerem. Jeśli serwer nie wyśle odpowiedzi uzgadniania w tym interwale, klient anuluje uzgadnianie i wyzwala zdarzenie `onClose`. Jest to ustawienie zaawansowane, które należy zmodyfikować tylko w przypadku wystąpienia błędów limitu czasu uzgadniania z powodu poważnego opóźnienia sieci. Aby uzyskać więcej szczegółów na temat procesu uzgadniania, zobacz [Specyfikacja protokołuSignalR Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md). |
| `getKeepAliveInterval` / `setKeepAliveInterval` | 15 sekund (15 000 MS) | Określa interwał wysyłania komunikatów ping przez klienta. Wysłanie wszelkich komunikatów z klienta powoduje zresetowanie czasomierza do początku interwału. Jeśli klient nie wyśle komunikatu w `ClientTimeoutInterval` zestawie na serwerze, serwer traktuje klienta jako odłączony. |

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="nettabdotnet"></a>[.NET](#tab/dotnet)

| Opcja | Wartość domyślna | Opis |
| ------ | ------------- | ----------- |
| `ServerTimeout` | 30 sekund (30 000 MS) | Limit czasu dla działania serwera. Jeśli serwer nie wysłał komunikatu w tym interwale, Klient uważa, że serwer odłączył się i wyzwala zdarzenie `Closed` (`onclose` w języku JavaScript). Ta wartość musi być wystarczająca do wysłania wiadomości ping z serwera **i** odebrana przez klienta w ramach przedziału czasu. Zalecana wartość to liczba z co najmniej podwójną wartością `KeepAliveInterval` serwera, aby umożliwić czas na nadejście poleceń ping. |
| `HandshakeTimeout` | 15 sekund | Limit czasu początkowej uzgadniania z serwerem. Jeśli serwer nie wyśle odpowiedzi uzgadniania w tym interwale, klient anuluje uzgadnianie i wyzwala zdarzenie `Closed` (`onclose` w języku JavaScript). Jest to ustawienie zaawansowane, które należy zmodyfikować tylko w przypadku wystąpienia błędów limitu czasu uzgadniania z powodu poważnego opóźnienia sieci. Aby uzyskać więcej szczegółów na temat procesu uzgadniania, zobacz [Specyfikacja protokołuSignalR Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md). |

W kliencie .NET wartości limitu czasu są określane jako wartości `TimeSpan`.

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

| Opcja | Wartość domyślna | Opis |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | 30 sekund (30 000 MS) | Limit czasu dla działania serwera. Jeśli serwer nie wysłał komunikatu w tym interwale, Klient uważa, że serwer odłączył się i wyzwala zdarzenie `onclose`. Ta wartość musi być wystarczająca do wysłania wiadomości ping z serwera **i** odebrana przez klienta w ramach przedziału czasu. Zalecana wartość to liczba z co najmniej podwójną wartością `KeepAliveInterval` serwera, aby umożliwić czas na nadejście poleceń ping. |

# <a name="javatabjava"></a>[Java](#tab/java)

| Opcja | Wartość domyślna | Opis |
| ------ | ------------- | ----------- |
| `getServerTimeout` / `setServerTimeout` | 30 sekund (30 000 MS) | Limit czasu dla działania serwera. Jeśli serwer nie wysłał komunikatu w tym interwale, Klient uważa, że serwer odłączył się i wyzwala zdarzenie `onClose`. Ta wartość musi być wystarczająca do wysłania wiadomości ping z serwera **i** odebrana przez klienta w ramach przedziału czasu. Zalecana wartość to liczba z co najmniej podwójną wartością `KeepAliveInterval` serwera, aby umożliwić czas na nadejście poleceń ping. |
| `withHandshakeResponseTimeout` | 15 sekund | Limit czasu początkowej uzgadniania z serwerem. Jeśli serwer nie wyśle odpowiedzi uzgadniania w tym interwale, klient anuluje uzgadnianie i wyzwala zdarzenie `onClose`. Jest to ustawienie zaawansowane, które należy zmodyfikować tylko w przypadku wystąpienia błędów limitu czasu uzgadniania z powodu poważnego opóźnienia sieci. Aby uzyskać więcej szczegółów na temat procesu uzgadniania, zobacz [Specyfikacja protokołuSignalR Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md). |

::: moniker-end

---

### <a name="configure-additional-options"></a>Skonfiguruj dodatkowe opcje

Dodatkowe opcje można skonfigurować w metodzie `WithUrl` (`withUrl` w języku JavaScript) na `HubConnectionBuilder` lub w różnych interfejsach API konfiguracji na `HttpHubConnectionBuilder` klienta Java:

# <a name="nettabdotnet"></a>[.NET](#tab/dotnet)

| Opcja .NET |  Wartość domyślna | Opis |
| ----------- | -------------- | ----------- |
| `AccessTokenProvider` | `null` | Funkcja zwracająca ciąg, który jest dostarczany jako token uwierzytelniania okaziciela w żądaniach HTTP. |
| `SkipNegotiation` | `false` | Ustaw tę wartość na `true`, aby pominąć etap negocjacji. **Obsługiwane tylko wtedy, gdy transport Sockets jest jedynym włączonym transportem**. Nie można włączyć tego ustawienia w przypadku korzystania z usługi Azure SignalR. |
| `ClientCertificates` | Pusty | Kolekcja certyfikatów TLS do wysłania w celu uwierzytelniania żądań. |
| `Cookies` | Pusty | Kolekcja plików cookie protokołu HTTP do wysłania w każdym żądaniu HTTP. |
| `Credentials` | Pusty | Poświadczenia do wysyłania przy każdym żądaniu HTTP. |
| `CloseTimeout` | 5 sekund | Tylko obiekty WebSockets. Maksymalny czas oczekiwania klienta po zamknięciu serwera w celu potwierdzenia żądania zamknięcia. Jeśli serwer nie potwierdzi zamknięcia w tym czasie, klient zostanie rozłączony. |
| `Headers` | Pusty | Mapa dodatkowych nagłówków HTTP do wysłania w każdym żądaniu HTTP. |
| `HttpMessageHandlerFactory` | `null` | Delegat, który może służyć do konfigurowania lub zastępowania `HttpMessageHandler` używany do wysyłania żądań HTTP. Nieużywane na potrzeby połączeń protokołu WebSocket. Ten delegat musi zwracać wartość różną od null i otrzymuje wartość domyślną jako parametr. Zmodyfikuj ustawienia tej wartości domyślnej i zwróć ją lub Zwróć nowe wystąpienie `HttpMessageHandler`. **Podczas zastępowania programu obsługi należy skopiować ustawienia, które mają być zachowane z podanej procedury obsługi. w przeciwnym razie skonfigurowane opcje (takie jak pliki cookie i nagłówki) nie będą miały zastosowania do nowego programu obsługi.** |
| `Proxy` | `null` | Serwer proxy HTTP, który ma być używany podczas wysyłania żądań HTTP. |
| `UseDefaultCredentials` | `false` | Ustaw tę wartość logiczną, aby wysyłać domyślne poświadczenia dla żądań HTTP i WebSockets. Umożliwia to korzystanie z uwierzytelniania systemu Windows. |
| `WebSocketConfiguration` | `null` | Delegat, który może służyć do konfigurowania dodatkowych opcji protokołu WebSocket. Odbiera wystąpienie elementu [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) , którego można użyć do skonfigurowania opcji. |

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

| Opcja języka JavaScript | Wartość domyślna | Opis |
| ----------------- | ------------- | ----------- |
| `accessTokenFactory` | `null` | Funkcja zwracająca ciąg, który jest dostarczany jako token uwierzytelniania okaziciela w żądaniach HTTP. |
| `skipNegotiation` | `false` | Ustaw tę wartość na `true`, aby pominąć etap negocjacji. **Obsługiwane tylko wtedy, gdy transport Sockets jest jedynym włączonym transportem**. Nie można włączyć tego ustawienia w przypadku korzystania z usługi Azure SignalR. |

# <a name="javatabjava"></a>[Java](#tab/java)

| Opcja Java | Wartość domyślna | Opis |
| ----------- | ------------- | ----------- |
| `withAccessTokenProvider` | `null` | Funkcja zwracająca ciąg, który jest dostarczany jako token uwierzytelniania okaziciela w żądaniach HTTP. |
| `shouldSkipNegotiate` | `false` | Ustaw tę wartość na `true`, aby pominąć etap negocjacji. **Obsługiwane tylko wtedy, gdy transport Sockets jest jedynym włączonym transportem**. Nie można włączyć tego ustawienia w przypadku korzystania z usługi Azure SignalR. |
| `withHeader``withHeaders` | Pusty | Mapa dodatkowych nagłówków HTTP do wysłania w każdym żądaniu HTTP. |

---

W kliencie .NET opcje te można modyfikować za pomocą delegata opcji udostępnianego do `WithUrl`:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

W kliencie JavaScript te opcje można podać w obiekcie JavaScript udostępnionym do `withUrl`:

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

W kliencie Java Opcje te można skonfigurować przy użyciu metod na `HttpHubConnectionBuilder` zwracanych z `HubConnectionBuilder.create("HUB URL")`

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
        .withHeader("Foo", "Bar")
        .shouldSkipNegotiate(true)
        .withHandshakeResponseTimeout(30*1000)
        .build();
```

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>
