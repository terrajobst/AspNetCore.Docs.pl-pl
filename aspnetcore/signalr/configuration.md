---
title: Konfiguracja sygnałów ASP.NET Core
author: bradygaster
description: Dowiedz się, jak skonfigurować aplikacje sygnalizujące ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 08/05/2019
uid: signalr/configuration
ms.openlocfilehash: 4706a1e2774fa9f6fb40085da944e8a82476ef05
ms.sourcegitcommit: 2eb605f4f20ac4dd9de6c3b3e3453e108a357a21
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/06/2019
ms.locfileid: "68820046"
---
# <a name="aspnet-core-signalr-configuration"></a>Konfiguracja sygnałów ASP.NET Core

## <a name="jsonmessagepack-serialization-options"></a>Opcje serializacji JSON/MessagePack

ASP.NET Core sygnalizujący obsługuje dwa protokoły dla komunikatów kodowania: [JSON](https://www.json.org/) i [MessagePack](https://msgpack.org/index.html). Każdy protokół ma opcje konfiguracji serializacji.

Serializacji JSON można skonfigurować na serwerze przy użyciu metody rozszerzenia [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) , która może zostać dodana po `Startup.ConfigureServices` metodzie [addsygnalizująca](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) w ramach metody. Metoda przyjmuje delegata, który `options` odbiera obiekt. `AddJsonProtocol` Właściwość [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) tego obiektu jest obiektem JSON.NET `JsonSerializerSettings` , którego można użyć do skonfigurowania serializacji argumentów i zwracanych wartości. Aby uzyskać więcej informacji, zobacz [dokumentację JSON.NET](https://www.newtonsoft.com/json/help/html/Introduction.htm) .

Przykładowo, aby skonfigurować serializator do używania nazw właściwości "PascalCase" zamiast domyślnych nazw "camelCase", należy użyć następującego kodu:

```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver =
        new DefaultContractResolver();
    });
```

W kliencie .NET ta sama `AddJsonProtocol` Metoda rozszerzająca istnieje w [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder). `Microsoft.Extensions.DependencyInjection` Przestrzeń nazw musi zostać zaimportowana, aby można było rozpoznać metodę rozszerzenia:

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

### <a name="messagepack-serialization-options"></a>Opcje serializacji MessagePack

Serializacji MessagePack można skonfigurować, dostarczając delegata do wywołania [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) . Aby uzyskać więcej informacji, zobacz [MessagePack w sygnalizacji](xref:signalr/messagepackhubprotocol) .

> [!NOTE]
> W tej chwili nie można skonfigurować serializacji MessagePack w kliencie JavaScript.

## <a name="configure-server-options"></a>Konfigurowanie opcji serwera

W poniższej tabeli opisano opcje konfigurowania centrów sygnałów:

::: moniker range=">= aspnetcore-3.0"

| Opcja | Wartość domyślna | Opis |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | 30 sekund | Serwer Rozważ odłączenie klienta, jeśli nie odebrano komunikatu (w tym w tym interwale). Może upłynąć dłużej niż ten interwał limitu czasu, aby klient mógł zostać oznaczony jako odłączony, ze względu na to, jak jest to zaimplementowane. Zalecaną wartością jest podwójna `KeepAliveInterval` wartość.|
| `HandshakeTimeout` | 15 sekund | Jeśli klient nie wyśle początkowego komunikatu uzgadniania w tym przedziale czasu, połączenie zostanie zamknięte. Jest to ustawienie zaawansowane, które należy zmodyfikować tylko w przypadku wystąpienia błędów limitu czasu uzgadniania z powodu poważnego opóźnienia sieci. Aby uzyskać więcej szczegółów na temat procesu uzgadniania, zobacz [specyfikację protokołu centrum sygnału](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md). |
| `KeepAliveInterval` | 15 sekund | Jeśli serwer nie wysłał komunikatu w tym interwale, zostanie automatycznie wysłana wiadomość ping w celu pozostawienia otwartego połączenia. W przypadku `KeepAliveInterval`zmiany należy `ServerTimeout` / zmienić`serverTimeoutInMilliseconds` ustawienie na kliencie. Zalecaną `ServerTimeout` / wartościąjest`KeepAliveInterval` podwójna wartość. `serverTimeoutInMilliseconds`  |
| `SupportedProtocols` | Wszystkie zainstalowane protokoły | Protokoły obsługiwane przez to centrum. Domyślnie wszystkie protokoły zarejestrowane na serwerze są dozwolone, ale protokoły można usunąć z tej listy, aby wyłączyć określone protokoły dla poszczególnych centrów. |
| `EnableDetailedErrors` | `false` | Jeśli `true`szczegółowe komunikaty o wyjątkach są zwracane do klientów, gdy wyjątek jest zgłaszany w metodzie centrum. Wartość domyślna to `false`, ponieważ te komunikaty o wyjątkach mogą zawierać informacje poufne. |
| `StreamBufferCapacity` | `10` | Maksymalna liczba elementów, które mogą być buforowane dla strumieni przekazywania klientów. Po osiągnięciu tego limitu przetwarzanie wywołań jest blokowane, dopóki serwer nie przetwarza elementów strumienia.|

::: moniker-end

::: moniker range="= aspnetcore-2.2"

| Opcja | Wartość domyślna | Opis |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | 30 sekund | Serwer Rozważ odłączenie klienta, jeśli nie odebrano komunikatu (w tym w tym interwale). Może upłynąć dłużej niż ten interwał limitu czasu, aby klient mógł zostać oznaczony jako odłączony, ze względu na to, jak jest to zaimplementowane. Zalecaną wartością jest podwójna `KeepAliveInterval` wartość.|
| `HandshakeTimeout` | 15 sekund | Jeśli klient nie wyśle początkowego komunikatu uzgadniania w tym przedziale czasu, połączenie zostanie zamknięte. Jest to ustawienie zaawansowane, które należy zmodyfikować tylko w przypadku wystąpienia błędów limitu czasu uzgadniania z powodu poważnego opóźnienia sieci. Aby uzyskać więcej szczegółów na temat procesu uzgadniania, zobacz [specyfikację protokołu centrum sygnału](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md). |
| `KeepAliveInterval` | 15 sekund | Jeśli serwer nie wysłał komunikatu w tym interwale, zostanie automatycznie wysłana wiadomość ping w celu pozostawienia otwartego połączenia. W przypadku `KeepAliveInterval`zmiany należy `ServerTimeout` / zmienić`serverTimeoutInMilliseconds` ustawienie na kliencie. Zalecaną `ServerTimeout` / wartościąjest`KeepAliveInterval` podwójna wartość. `serverTimeoutInMilliseconds`  |
| `SupportedProtocols` | Wszystkie zainstalowane protokoły | Protokoły obsługiwane przez to centrum. Domyślnie wszystkie protokoły zarejestrowane na serwerze są dozwolone, ale protokoły można usunąć z tej listy, aby wyłączyć określone protokoły dla poszczególnych centrów. |
| `EnableDetailedErrors` | `false` | Jeśli `true`szczegółowe komunikaty o wyjątkach są zwracane do klientów, gdy wyjątek jest zgłaszany w metodzie centrum. Wartość domyślna to `false`, ponieważ te komunikaty o wyjątkach mogą zawierać informacje poufne. |

::: moniker-end

::: moniker range="= aspnetcore-2.1"

| Opcja | Wartość domyślna | Opis |
| ------ | ------------- | ----------- |
| `HandshakeTimeout` | 15 sekund | Jeśli klient nie wyśle początkowego komunikatu uzgadniania w tym przedziale czasu, połączenie zostanie zamknięte. Jest to ustawienie zaawansowane, które należy zmodyfikować tylko w przypadku wystąpienia błędów limitu czasu uzgadniania z powodu poważnego opóźnienia sieci. Aby uzyskać więcej szczegółów na temat procesu uzgadniania, zobacz [specyfikację protokołu centrum sygnału](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md). |
| `KeepAliveInterval` | 15 sekund | Jeśli serwer nie wysłał komunikatu w tym interwale, zostanie automatycznie wysłana wiadomość ping w celu pozostawienia otwartego połączenia. W przypadku `KeepAliveInterval`zmiany należy `ServerTimeout` / zmienić`serverTimeoutInMilliseconds` ustawienie na kliencie. Zalecaną `ServerTimeout` / wartościąjest`KeepAliveInterval` podwójna wartość. `serverTimeoutInMilliseconds`  |
| `SupportedProtocols` | Wszystkie zainstalowane protokoły | Protokoły obsługiwane przez to centrum. Domyślnie wszystkie protokoły zarejestrowane na serwerze są dozwolone, ale protokoły można usunąć z tej listy, aby wyłączyć określone protokoły dla poszczególnych centrów. |
| `EnableDetailedErrors` | `false` | Jeśli `true`szczegółowe komunikaty o wyjątkach są zwracane do klientów, gdy wyjątek jest zgłaszany w metodzie centrum. Wartość domyślna to `false`, ponieważ te komunikaty o wyjątkach mogą zawierać informacje poufne. |

::: moniker-end

Opcje można skonfigurować dla wszystkich centrów, dostarczając opcje delegata do `AddSignalR` wywołania w. `Startup.ConfigureServices`

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

Opcje jednego centrum przesłaniają opcje globalne podane w `AddSignalR` i można je skonfigurować przy użyciu: <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

### <a name="advanced-http-configuration-options"></a>Zaawansowane opcje konfiguracji HTTP

Służy `HttpConnectionDispatcherOptions` do konfigurowania ustawień zaawansowanych związanych z transportami i zarządzaniem buforem pamięci. Te opcje są konfigurowane przez przekazanie delegata [do\<MapHub T >](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) w. `Startup.Configure`

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

W poniższej tabeli opisano opcje konfigurowania zaawansowanych opcji protokołu HTTP w programie ASP.NET Core Signal:

| Opcja | Wartość domyślna | Opis |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | 32 KB | Maksymalna liczba bajtów odebranych od klienta, które buforuje serwer. Zwiększenie tej wartości umożliwia serwerowi otrzymywanie większych komunikatów, ale może mieć negatywny wpływ na użycie pamięci. |
| `AuthorizationData` | Dane zbierane automatycznie z `Authorize` atrybutów zastosowanych do klasy centrum. | Lista obiektów [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) służąca do określenia, czy klient jest autoryzowany do łączenia się z centrum. |
| `TransportMaxBufferSize` | 32 KB | Maksymalna liczba bajtów wysłanych przez aplikację, które buforują serwer. Zwiększenie tej wartości umożliwia serwerowi wysyłanie większych komunikatów, ale może mieć negatywny wpływ na użycie pamięci. |
| `Transports` | Wszystkie transporty są włączone. | Maska bitów `HttpTransportType` wartości, która może ograniczyć transporty, z których może korzystać klient do nawiązywania połączeń. |
| `LongPolling` | Zobacz poniżej. | Dodatkowe opcje specyficzne dla długiego transportu sondowania. |
| `WebSockets` | Zobacz poniżej. | Dodatkowe opcje specyficzne dla transportu obiektów WebSockets. |

W przypadku długiego transportu sondowania dostępne są dodatkowe opcje, które można `LongPolling` skonfigurować przy użyciu właściwości:

| Opcja | Wartość domyślna | Opis |
| ------ | ------------- | ----------- |
| `PollTimeout` | 90 sekund | Maksymalny czas, przez jaki serwer czeka na wysłanie wiadomości do klienta przed zakończeniem jednego żądania sondowania. Zmniejszenie tej wartości powoduje, że klient wystawia nowe żądania sondy częściej. |

Transport WebSocket ma dodatkowe opcje, które można skonfigurować przy użyciu `WebSockets` właściwości:

| Opcja | Wartość domyślna | Opis |
| ------ | ------------- | ----------- |
| `CloseTimeout` | 5 sekund | Gdy serwer zostanie zamknięty, jeśli klient nie zostanie zamknięty w tym okresie, połączenie zostanie przerwane. |
| `SubProtocolSelector` | `null` | Delegat, którego można użyć do ustawienia `Sec-WebSocket-Protocol` nagłówka na wartość niestandardową. Delegat otrzymuje wartości żądane przez klienta jako dane wejściowe i oczekuje na zwrócenie żądanej wartości. |

## <a name="configure-client-options"></a>Konfigurowanie opcji klienta

Opcje klienta można skonfigurować dla `HubConnectionBuilder` typu (dostępnego w klientach .NET i JavaScript). Jest ona również dostępna w kliencie Java, ale `HttpHubConnectionBuilder` podklasa zawiera opcje konfiguracji konstruktora, a także `HubConnection` samego siebie.

### <a name="configure-logging"></a>Konfigurowanie rejestrowania

Rejestrowanie jest konfigurowane w kliencie .NET przy użyciu `ConfigureLogging` metody. Dostawcy i filtry rejestrowania mogą być rejestrowane w taki sam sposób, w jaki znajdują się na serwerze. Aby uzyskać więcej informacji, zobacz dokumentację dotyczącą [rejestrowania w ASP.NET Core](xref:fundamentals/logging/index) .

> [!NOTE]
> Aby zarejestrować dostawców rejestrowania, należy zainstalować wymagane pakiety. Aby zapoznać się z pełną listą, zobacz sekcję [wbudowane dostawcy rejestrowania](xref:fundamentals/logging/index#built-in-logging-providers) w witrynie docs.

Na przykład aby włączyć rejestrowanie konsoli, zainstaluj `Microsoft.Extensions.Logging.Console` pakiet NuGet. Wywołaj `AddConsole` metodę rozszerzenia:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

W kliencie JavaScript istnieje podobna `configureLogging` Metoda. `LogLevel` Podaj wartość wskazującą minimalny poziom komunikatów dziennika do wygenerowania. Dzienniki są zapisywane w oknie konsoli przeglądarki.

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

::: moniker range=">= aspnetcore-3.0"

Zamiast wartości można także podać wartość reprezentującą nazwę poziomu dziennika. `string` `LogLevel` Jest to przydatne podczas konfigurowania rejestrowania sygnałów w środowiskach, w których nie masz dostępu do `LogLevel` stałych.

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging("warn")
    .build();
```

Poniższa tabela zawiera listę dostępnych poziomów dzienników. Wartość określana przez użytkownika `configureLogging` w celu ustawienia minimalnego poziomu dziennika, który zostanie zarejestrowany. Komunikaty zarejestrowane na tym poziomie **lub poziomy wymienione po nim w tabeli**zostaną zarejestrowane.

| String                      | LogLevel               |
| --------------------------- | ---------------------- |
| `trace`                     | `LogLevel.Trace`       |
| `debug`                     | `LogLevel.Debug`       |
| `info`**lub**`information` | `LogLevel.Information` |
| `warn`**lub**`warning`     | `LogLevel.Warning`     |
| `error`                     | `LogLevel.Error`       |
| `critical`                  | `LogLevel.Critical`    |
| `none`                      | `LogLevel.None`        |

::: moniker-end

> [!NOTE]
> Aby całkowicie wyłączyć rejestrowanie, określ `signalR.LogLevel.None` `configureLogging` w metodzie.

Aby uzyskać więcej informacji na temat rejestrowania, zobacz [dokumentację diagnostyki sygnału](xref:signalr/diagnostics).

Klient Java sygnalizujący używa biblioteki [SLF4J](https://www.slf4j.org/) do rejestrowania. Jest to interfejs API rejestrowania wysokiego poziomu, który umożliwia użytkownikom biblioteki wybranie własnych implementacji rejestrowania przez nałożenie określonej zależności rejestrowania. Poniższy fragment kodu przedstawia sposób użycia `java.util.logging` z klientem programu sygnalizującego środowisko Java.

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

Transporty używane przez sygnalizującego można skonfigurować w `WithUrl` wywołaniu (`withUrl` w języku JavaScript). Wartości bitowe i `HttpTransportType` można użyć, aby ograniczyć klient do używania tylko określonych transportów. Wszystkie transporty są domyślnie włączone.

Na przykład, aby wyłączyć transport zdarzeń wysłanych przez serwer, ale zezwolić na połączenia z usługą WebSockets i długie sondowanie:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

W kliencie JavaScript transporty są konfigurowane przez ustawienie `transport` pola w obiekcie Options dostarczonego do: `withUrl`

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

::: moniker range=">= aspnetcore-2.2"

W tej wersji elementu WebSockets klienta Java jest jedynym dostępnym transportem.

::: moniker-end

::: moniker range="= aspnetcore-3.0"

W kliencie Java, transport jest wybierany przy użyciu `withTransport` metody `HttpHubConnectionBuilder`w. Klient Java domyślnie używa transportu usługi WebSockets.

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withTransport(TransportEnum.WEBSOCKETS)
    .build();
```

> [!NOTE]
> Klient Java sygnalizujący nie obsługuje jeszcze rezerwy transportowej.

::: moniker-end

### <a name="configure-bearer-authentication"></a>Konfigurowanie uwierzytelniania okaziciela

Aby zapewnić dane uwierzytelniania wraz z żądaniami sygnałów, użyj `AccessTokenProvider` opcji (`accessTokenFactory` w języku JavaScript), aby określić funkcję, która zwraca żądany token dostępu. W kliencie .NET token dostępu jest przenoszona jako token "uwierzytelnianie okaziciela" protokołu HTTP (przy użyciu `Authorization` nagłówka z `Bearer`typem). W kliencie JavaScript token dostępu jest używany jako token okaziciela, **z wyjątkiem** kilku przypadków, w których interfejsy API przeglądarki ograniczają możliwość zastosowania nagłówków (w konkretnym przypadku zdarzeń wysyłanych przez serwer i żądań WebSockets). W takich przypadkach token dostępu jest podawany jako wartość `access_token`ciągu zapytania.

W kliencie `AccessTokenProvider` .NET opcji można określić za pomocą delegata opcji w `WithUrl`:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

W kliencie JavaScript token dostępu jest konfigurowany przez ustawienie `accessTokenFactory` pola w obiekcie Options w: `withUrl`

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

W kliencie Java programu sygnalizującego można skonfigurować token okaziciela do użycia na potrzeby uwierzytelniania, dostarczając fabrykę tokenów dostępu do [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java). Użyj [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) , aby dostarczyć [RxJava](https://github.com/ReactiveX/RxJava) [pojedynczej\<>](https://reactivex.io/documentation/single.html). Wywołanie metody [Single. Ustąp](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-)umożliwia zapisanie logiki w celu utworzenia tokenów dostępu dla klienta.

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

### <a name="configure-timeout-and-keep-alive-options"></a>Konfigurowanie opcji limitu czasu i utrzymywania aktywności

W `HubConnection` samym obiekcie są dostępne dodatkowe opcje konfigurowania trybu przekroczenia limitu czasu i zachowania utrzymywania aktywności:


::: moniker range=">= aspnetcore-2.2"

# <a name="nettabdotnet"></a>[.NET](#tab/dotnet)

| Opcja | Wartość domyślna | Opis |
| ------ | ------------- | ----------- |
| `ServerTimeout` | 30 sekund (30 000 MS) | Limit czasu dla działania serwera. Jeśli serwer nie wysłał komunikatu w tym interwale, Klient uważa, że serwer odłączył się i wyzwala `Closed` zdarzenie (`onclose` w języku JavaScript). Ta wartość musi być wystarczająca do wysłania wiadomości ping z serwera **i** odebrana przez klienta w ramach przedziału czasu. Zalecana wartość to liczba z co najmniej podwójną `KeepAliveInterval` wartością serwera, aby umożliwić czas na nadejście poleceń ping. |
| `HandshakeTimeout` | 15 sekund | Limit czasu początkowej uzgadniania z serwerem. Jeśli serwer nie wyśle odpowiedzi uzgadniania w tym interwale, klient anuluje uzgadnianie i wyzwala `Closed` zdarzenie (`onclose` w języku JavaScript). Jest to ustawienie zaawansowane, które należy zmodyfikować tylko w przypadku wystąpienia błędów limitu czasu uzgadniania z powodu poważnego opóźnienia sieci. Aby uzyskać więcej szczegółów na temat procesu uzgadniania, zobacz [specyfikację protokołu centrum sygnału](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md). |
| `KeepAliveInterval` | 15 sekund | Określa interwał wysyłania komunikatów ping przez klienta. Wysłanie wszelkich komunikatów z klienta powoduje zresetowanie czasomierza do początku interwału. Jeśli klient nie wysłał komunikatu w `ClientTimeoutInterval` zestawie na serwerze, serwer traktuje klienta jako odłączony. |

W kliencie .NET wartości limitu czasu są określone jako `TimeSpan` wartości.

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

| Opcja | Wartość domyślna | Opis |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | 30 sekund (30 000 MS) | Limit czasu dla działania serwera. Jeśli serwer nie wysłał komunikatu w tym interwale, Klient uważa, że serwer odłączył się i wyzwala `onclose` zdarzenie. Ta wartość musi być wystarczająca do wysłania wiadomości ping z serwera **i** odebrana przez klienta w ramach przedziału czasu. Zalecana wartość to liczba z co najmniej podwójną `KeepAliveInterval` wartością serwera, aby umożliwić czas na nadejście poleceń ping. |
| `keepAliveIntervalInMilliseconds` | 15 sekund (15 000 milisekund) | Określa interwał wysyłania komunikatów ping przez klienta. Wysłanie wszelkich komunikatów z klienta powoduje zresetowanie czasomierza do początku interwału. Jeśli klient nie wysłał komunikatu w `ClientTimeoutInterval` zestawie na serwerze, serwer traktuje klienta jako odłączony. |

# <a name="javatabjava"></a>[Java](#tab/java)

| Opcja | Wartość domyślna | Opis |
| ------ | ------------- | ----------- |
| `getServerTimeout` / `setServerTimeout` | 30 sekund (30 000 MS) | Limit czasu dla działania serwera. Jeśli serwer nie wysłał komunikatu w tym interwale, Klient uważa, że serwer odłączył się i wyzwala `onClose` zdarzenie. Ta wartość musi być wystarczająca do wysłania wiadomości ping z serwera **i** odebrana przez klienta w ramach przedziału czasu. Zalecana wartość to liczba z co najmniej podwójną `KeepAliveInterval` wartością serwera, aby umożliwić czas na nadejście poleceń ping. |
| `withHandshakeResponseTimeout` | 15 sekund | Limit czasu początkowej uzgadniania z serwerem. Jeśli serwer nie wyśle odpowiedzi uzgadniania w tym interwale, klient anuluje uzgadnianie i wyzwala `onClose` zdarzenie. Jest to ustawienie zaawansowane, które należy zmodyfikować tylko w przypadku wystąpienia błędów limitu czasu uzgadniania z powodu poważnego opóźnienia sieci. Aby uzyskać więcej szczegółów na temat procesu uzgadniania, zobacz [specyfikację protokołu centrum sygnału](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md). |
| `getKeepAliveInterval` / `setKeepAliveInterval` | 15 sekund (15 000 MS) | Określa interwał wysyłania komunikatów ping przez klienta. Wysłanie wszelkich komunikatów z klienta powoduje zresetowanie czasomierza do początku interwału. Jeśli klient nie wysłał komunikatu w `ClientTimeoutInterval` zestawie na serwerze, serwer traktuje klienta jako odłączony. |

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="nettabdotnet"></a>[.NET](#tab/dotnet)

| Opcja | Wartość domyślna | Opis |
| ------ | ------------- | ----------- |
| `ServerTimeout` | 30 sekund (30 000 MS) | Limit czasu dla działania serwera. Jeśli serwer nie wysłał komunikatu w tym interwale, Klient uważa, że serwer odłączył się i wyzwala `Closed` zdarzenie (`onclose` w języku JavaScript). Ta wartość musi być wystarczająca do wysłania wiadomości ping z serwera **i** odebrana przez klienta w ramach przedziału czasu. Zalecana wartość to liczba z co najmniej podwójną `KeepAliveInterval` wartością serwera, aby umożliwić czas na nadejście poleceń ping. |
| `HandshakeTimeout` | 15 sekund | Limit czasu początkowej uzgadniania z serwerem. Jeśli serwer nie wyśle odpowiedzi uzgadniania w tym interwale, klient anuluje uzgadnianie i wyzwala `Closed` zdarzenie (`onclose` w języku JavaScript). Jest to ustawienie zaawansowane, które należy zmodyfikować tylko w przypadku wystąpienia błędów limitu czasu uzgadniania z powodu poważnego opóźnienia sieci. Aby uzyskać więcej szczegółów na temat procesu uzgadniania, zobacz [specyfikację protokołu centrum sygnału](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md). |

W kliencie .NET wartości limitu czasu są określone jako `TimeSpan` wartości.

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

| Opcja | Wartość domyślna | Opis |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | 30 sekund (30 000 MS) | Limit czasu dla działania serwera. Jeśli serwer nie wysłał komunikatu w tym interwale, Klient uważa, że serwer odłączył się i wyzwala `onclose` zdarzenie. Ta wartość musi być wystarczająca do wysłania wiadomości ping z serwera **i** odebrana przez klienta w ramach przedziału czasu. Zalecana wartość to liczba z co najmniej podwójną `KeepAliveInterval` wartością serwera, aby umożliwić czas na nadejście poleceń ping. |

# <a name="javatabjava"></a>[Java](#tab/java)

| Opcja | Wartość domyślna | Opis |
| ------ | ------------- | ----------- |
| `getServerTimeout` / `setServerTimeout` | 30 sekund (30 000 MS) | Limit czasu dla działania serwera. Jeśli serwer nie wysłał komunikatu w tym interwale, Klient uważa, że serwer odłączył się i wyzwala `onClose` zdarzenie. Ta wartość musi być wystarczająca do wysłania wiadomości ping z serwera **i** odebrana przez klienta w ramach przedziału czasu. Zalecana wartość to liczba z co najmniej podwójną `KeepAliveInterval` wartością serwera, aby umożliwić czas na nadejście poleceń ping. |
| `withHandshakeResponseTimeout` | 15 sekund | Limit czasu początkowej uzgadniania z serwerem. Jeśli serwer nie wyśle odpowiedzi uzgadniania w tym interwale, klient anuluje uzgadnianie i wyzwala `onClose` zdarzenie. Jest to ustawienie zaawansowane, które należy zmodyfikować tylko w przypadku wystąpienia błędów limitu czasu uzgadniania z powodu poważnego opóźnienia sieci. Aby uzyskać więcej szczegółów na temat procesu uzgadniania, zobacz [specyfikację protokołu centrum sygnału](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md). |

::: moniker-end

---

### <a name="configure-additional-options"></a>Skonfiguruj dodatkowe opcje

Dodatkowe opcje `WithUrl` można skonfigurować w metodzie (`withUrl` w języku JavaScript) dla `HubConnectionBuilder` `HttpHubConnectionBuilder` lub w różnych interfejsach API konfiguracji w kliencie Java:

# <a name="nettabdotnet"></a>[.NET](#tab/dotnet)

| Opcja .NET |  Wartość domyślna | Opis |
| ----------- | -------------- | ----------- |
| `AccessTokenProvider` | `null` | Funkcja zwracająca ciąg, który jest dostarczany jako token uwierzytelniania okaziciela w żądaniach HTTP. |
| `SkipNegotiation` | `false` | Ustaw tę `true` wartość na, aby pominąć krok negocjowania. **Obsługiwane tylko wtedy, gdy transport Sockets jest jedynym włączonym**transportem. Nie można włączyć tego ustawienia w przypadku korzystania z usługi Azure Signal Service. |
| `ClientCertificates` | Pusty | Kolekcja certyfikatów TLS do wysłania w celu uwierzytelniania żądań. |
| `Cookies` | Pusty | Kolekcja plików cookie protokołu HTTP do wysłania w każdym żądaniu HTTP. |
| `Credentials` | Pusty | Poświadczenia do wysyłania przy każdym żądaniu HTTP. |
| `CloseTimeout` | 5 sekund | Tylko obiekty WebSockets. Maksymalny czas oczekiwania klienta po zamknięciu serwera w celu potwierdzenia żądania zamknięcia. Jeśli serwer nie potwierdzi zamknięcia w tym czasie, klient zostanie rozłączony. |
| `Headers` | Pusty | Mapa dodatkowych nagłówków HTTP do wysłania w każdym żądaniu HTTP. |
| `HttpMessageHandlerFactory` | `null` | Delegat, który może służyć do konfigurowania lub zastępowania `HttpMessageHandler` używanych do wysyłania żądań HTTP. Nieużywane na potrzeby połączeń protokołu WebSocket. Ten delegat musi zwracać wartość różną od null i otrzymuje wartość domyślną jako parametr. Zmodyfikuj ustawienia tej wartości domyślnej i zwróć ją lub Zwróć nowe `HttpMessageHandler` wystąpienie. **Podczas zastępowania programu obsługi należy skopiować ustawienia, które mają być zachowane z podanej procedury obsługi. w przeciwnym razie skonfigurowane opcje (takie jak pliki cookie i nagłówki) nie będą miały zastosowania do nowego programu obsługi.** |
| `Proxy` | `null` | Serwer proxy HTTP, który ma być używany podczas wysyłania żądań HTTP. |
| `UseDefaultCredentials` | `false` | Ustaw tę wartość logiczną, aby wysyłać domyślne poświadczenia dla żądań HTTP i WebSockets. Umożliwia to korzystanie z uwierzytelniania systemu Windows. |
| `WebSocketConfiguration` | `null` | Delegat, który może służyć do konfigurowania dodatkowych opcji protokołu WebSocket. Odbiera wystąpienie elementu [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) , którego można użyć do skonfigurowania opcji. |

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

| Opcja języka JavaScript | Wartość domyślna | Opis |
| ----------------- | ------------- | ----------- |
| `accessTokenFactory` | `null` | Funkcja zwracająca ciąg, który jest dostarczany jako token uwierzytelniania okaziciela w żądaniach HTTP. |
| `skipNegotiation` | `false` | Ustaw tę `true` wartość na, aby pominąć krok negocjowania. **Obsługiwane tylko wtedy, gdy transport Sockets jest jedynym włączonym**transportem. Nie można włączyć tego ustawienia w przypadku korzystania z usługi Azure Signal Service. |

# <a name="javatabjava"></a>[Java](#tab/java)

| Opcja Java | Wartość domyślna | Opis |
| ----------- | ------------- | ----------- |
| `withAccessTokenProvider` | `null` | Funkcja zwracająca ciąg, który jest dostarczany jako token uwierzytelniania okaziciela w żądaniach HTTP. |
| `shouldSkipNegotiate` | `false` | Ustaw tę `true` wartość na, aby pominąć krok negocjowania. **Obsługiwane tylko wtedy, gdy transport Sockets jest jedynym włączonym**transportem. Nie można włączyć tego ustawienia w przypadku korzystania z usługi Azure Signal Service. |
| `withHeader``withHeaders` | Pusty | Mapa dodatkowych nagłówków HTTP do wysłania w każdym żądaniu HTTP. |

---

W kliencie platformy .NET te opcje można modyfikować za pomocą delegata opcji udostępnianego dla `WithUrl`:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

W kliencie JavaScript te opcje można podać w obiekcie JavaScript udostępnionym `withUrl`:

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

W kliencie Java Opcje te można skonfigurować przy użyciu metod `HttpHubConnectionBuilder` zwróconych z`HubConnectionBuilder.create("HUB URL")`

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
