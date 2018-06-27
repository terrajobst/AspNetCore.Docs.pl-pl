---
title: Konfiguracja programu ASP.NET Core SignalR
author: rachelappel
description: Informacje o sposobie konfigurowania aplikacji ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 06/30/2018
uid: signalr/configuration
ms.openlocfilehash: 167b8828efd093d755aeb1c91e080dbd22b5d485
ms.sourcegitcommit: 356c8d394aaf384c834e9c90cabab43bfe36e063
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/26/2018
ms.locfileid: "36962460"
---
# <a name="aspnet-core-signalr-configuration"></a>Konfiguracja programu ASP.NET Core SignalR

## <a name="jsonmessagepack-serialization-options"></a>Opcje serializacji JSON/MessagePack

ASP.NET Core SignalR obsługuje dwa protokoły kodowania wiadomości: [JSON](https://www.json.org/) i [MessagePack](https://msgpack.org/index.html). Każdy protokół ma opcji konfiguracji serializacji.

Serializacja kodu JSON można skonfigurować na serwerze przy użyciu [ `AddJsonProtocol` ](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) — metoda rozszerzenia, które mogą być dodawane po [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) w Twojej `Startup.ConfigureServices` metody. `AddJsonProtocol` Metoda przyjmuje delegata, który odbiera `options` obiektu. [ `PayloadSerializerSettings` ](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) Właściwość obiektu, na którym jest JSON.NET `JsonSerializerSettings` obiektu, który może służyć do konfigurowania serializacji argumenty i zwracać wartości. Zobacz [dokumentacji struktury JSON.NET](https://www.newtonsoft.com/json/help/html/Introduction.htm) więcej szczegółów.

Na przykład aby skonfigurować serializator do użycia zamiast domyślnej nazwy "(camelcase)", "PascalCase" nazwy właściwości, użyć poniższego kodu:

```csharp
services.AddSignalR()
    .AddJsonHubProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver = 
        new DefaultContractResolver();
    });
```

W kliencie programu .NET, takie same `AddJsonHubProtocol` — metoda rozszerzenia istnieje na [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder). `Microsoft.Extensions.DependencyInjection` Przestrzeni nazw, należy zaimportować do rozpoznania — metoda rozszerzenia:

```csharp
// At the top of the file:
using Microsoft.Extensions.DependencyInjection;

// When constructing your connection:
var connection = new HubConnectionBuilder()
.AddJsonHubProtocol(options => {
    options.PayloadSerializerSettings.ContractResolver = 
        new DefaultContractResolver();
});
```

> [!NOTE]
> Nie jest możliwe Konfigurowanie serializacji JSON w kliencie JavaScript w tej chwili.

### <a name="messagepack-serialization-options"></a>MessagePack szeregowanie opcje

Można skonfigurować MessagePack szeregowanie zapewniając pełnomocnika, aby [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) wywołania. Zobacz [MessagePack w SignalR](xref:signalr/messagepackhubprotocol) więcej szczegółów.

> [!NOTE]
> Nie jest możliwe skonfigurować MessagePack szeregowanie w kliencie JavaScript w tej chwili.

## <a name="configure-server-options"></a>Konfigurowanie opcji serwera

W poniższej tabeli opisano opcje konfigurowania koncentratorów SignalR:

| Opcja | Opis |
| ------ | ----------- |
| `HandshakeTimeout` | Klient nie wysłania komunikatu uzgadniania początkowej w tym przedziale czasu, połączenie jest zamknięte. |
| `KeepAliveInterval` | Jeśli serwer nie wysłał wiadomość, w tym przedziale czasu, wiadomość ping są wysyłane automatycznie, aby utrzymać otwarte połączenie. |
| `SupportedProtocols` | Protokoły obsługiwane przez tego koncentratora. Domyślnie wszystkie protokoły w zarejestrowany na serwerze są dozwolone, ale protokoły można usunąć z tej listy, aby wyłączyć określone protokoły dla poszczególnych koncentratorów. |
| `EnableDetailedErrors` | Jeśli `true`, szczegółowe komunikaty o wyjątkach są zwracane do klientów, gdy wyjątek w metodzie koncentratora. Wartość domyślna to `false`, jak te komunikaty o wyjątkach mogą zawierać poufne informacje. |

Można skonfigurować opcje dla wszystkich koncentratorów zapewniając delegata opcje do `AddSignalR` wywołanie w `Startup.ConfigureServices`.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSignalR(hubOptions =>
    {
        hubOptions.EnableDetailedErrors = true;
        hubOptions.KeepAliveInterval = TimeSpan.FromMinutes(1);
    })
}
```

Opcje dotyczące pojedynczego koncentratora zastąpienia globalnych opcji dostępnych w `AddSignalR` i można je skonfigurować przy użyciu [ `AddHubOptions<T>` ](/dotnet/api/microsoft.extensions.dependencyinjection.huboptionsdependencyinjectionextensions.addhuboptions):

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
}
```

Użyj `HttpConnectionDispatcherOptions` Aby skonfigurować ustawienia zaawansowane powiązane z transportami i Zarządzanie buforem pamięci. Te opcje są konfigurowane przez przekazywanie delegata do [ `MapHub<T>` ](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub).

| Opcja | Opis |
| ------ | ----------- |
| `ApplicationMaxBufferSize` | Maksymalna liczba bajtów odebranych z klienta który bufory serwera. Zwiększenie tej wartości umożliwia serwer do odbierania wiadomości większych, ale może niekorzystnie wpłynąć na zmniejszenie zużycia pamięci. Wartość domyślna to 32KB. |
| `AuthorizationData` | Lista [ `IAuthorizeData` ](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) obiekty używane do określenia, czy klient jest autoryzowany do nawiązania połączenia z koncentratorem. Domyślnie jest to wypełnione wartościami z `Authorize` atrybuty stosowane do klasy koncentratora. |
| `TransportMaxBufferSize` | Maksymalna liczba bajtów wysłanych przez aplikację który bufory serwera. Zwiększenie tej wartości umożliwia serwerowi na wysyłanie wiadomości większych, ale może niekorzystnie wpłynąć na zmniejszenie zużycia pamięci. Wartość domyślna to 32KB. |
| `Transports` | Maska bitowa `HttpTransportType` wartości, które mogą ograniczyć transportów klienta można się połączyć. Wszystkich transportów są domyślnie włączone. |
| `LongPolling` | Dodatkowe opcje specyficzne dla transportu sondowania długo. |
| `WebSockets` | Dodatkowe opcje specyficzne dla transportu protokołu WebSockets. |

Transport sondowania długi ma dodatkowe opcje, które można skonfigurować za pomocą `LongPolling` właściwości:

| Opcja | Opis |
| ------ | ----------- |
| `PollTimeout` | Maksymalna ilość czasu oczekiwania przez serwer komunikat do wysłania do klienta przed zakończeniem żądanie sondowania pojedynczego. Zmniejszenie tej wartości powoduje, że klientowi wygenerowanie nowego żądania sondowania częściej. Wartość domyślna wynosi 90 s. |

WebSocket transport ma dodatkowe opcje, które można skonfigurować za pomocą `WebSockets` właściwości:

| Opcja | Opis |
| ------ | ----------- |
| `CloseTimeout` | Po zamknięciu serwera, jeśli klient nie może zamknąć w tym przedziale czasu, połączenie zostało zakończone. |
| `SubProtocolSelector` | Delegata, który można używać do konfigurowania `Sec-WebSocket-Protocol` nagłówka do niestandardowej wartości. Delegat odbiera wartości żądany przez klienta jako dane wejściowe i powinien zwrócić żądaną wartość. |

## <a name="configure-client-options"></a>Skonfiguruj opcje klienta

Opcje klientów można skonfigurować na `HubConnectionBuilder` typu (dostępne w klientach .NET i JavaScript) również na dzień `HubConnection` samej siebie.

### <a name="configure-logging"></a>Konfigurowanie rejestrowania

Rejestrowanie skonfigurowano w kliencie programu .NET przy użyciu `ConfigureLogging` metody. Rejestrowanie dostawcy i filtrów można zarejestrować w taki sam sposób jak na serwerze. Zobacz [logowanie do platformy ASP.NET Core](xref:fundamentals/logging/index#how-to-add-providers) dokumentacji, aby uzyskać więcej informacji.

> [!NOTE]
> Aby zarejestrować dostawców rejestrowanie, musisz zainstalować wymaganych pakietów. Zobacz [dostawców wbudowanych rejestrowania](xref:fundamentals/logging/index#built-in-logging-providers) części dokumentów, aby uzyskać pełną listę.

Na przykład, aby włączyć rejestrowanie konsoli, należy zainstalować `Microsoft.Extensions.Logging.Console` pakietu NuGet. Wywołanie `AddConsole` — metoda rozszerzenia:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

W kliencie JavaScript, podobne `configureLogging` istnieje metoda. Podaj `LogLevel` wartość wskazującą, minimalny poziom dziennika komunikatów do produkcji. Dzienniki są zapisywane w oknie konsoli przeglądarki.

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

> [!NOTE]
> Aby wyłączyć rejestrowanie całkowicie, określ `signalR.LogLevel.None` w `configureLogging` metody.

Poniżej przedstawiono dostępne dla klienta JavaScript poziomy dziennika. Ustawienie poziomu dziennika na jedną z następujących wartości umożliwia rejestrowanie komunikatów na **lub nowszej** tego poziomu.

| Poziom | Opis |
| ----- | ----------- |
| `None` | Żadne komunikaty są rejestrowane. |
| `Critical` | Komunikaty wskazujących na niepowodzenie w całej aplikacji. |
| `Error` | Komunikaty wskazujących na niepowodzenie w bieżącej operacji. |
| `Warning` | Komunikaty, które wskazują na problem z systemem innym niż krytyczny. |
| `Information` | Komunikaty informacyjne. |
| `Debug` | Komunikaty diagnostyczne przydatne w przypadku debugowania. |
| `Trace` | Bardzo szczegółowe komunikaty diagnostyczne przeznaczone do diagnozowania określonych problemów. |

### <a name="configure-allowed-transports"></a>Konfigurowanie transportów dozwolone

Transporty używane przez SignalR można skonfigurować w `WithUrl` wywołania (`withUrl` w języku JavaScript). Alternatywy wartości `HttpTransportType` może służyć do ograniczania klienta można używać tylko określonych transportów. Wszystkich transportów są domyślnie włączone.

Na przykład aby wyłączyć transportu Server-Sent zdarzeń, ale zezwalaj na połączenia Websocket i długi sondowania:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

W kliencie JavaScript transportu są konfigurowane przez ustawienie `transport` na obiekt opcje przekazane do `withUrl`:

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

### <a name="configure-bearer-authentication"></a>Konfigurowanie uwierzytelniania elementu nośnego

Aby dostarczyć dane uwierzytelniania wraz z SignalR żądania, użyj `AccessTokenProvider` opcji (`accessTokenFactory` w języku JavaScript) aby określić funkcję, która zwraca token żądany dostęp. W kliencie programu .NET ten token dostępu jest przekazywany jako "Uwierzytelniania elementu nośnego" HTTP tokenu (używanie `Authorization` nagłówka o typie `Bearer`). W kliencie JavaScript token dostępu jest używany jako tokenu elementu nośnego, **z wyjątkiem** w niektórych przypadkach, gdy przeglądarki interfejsów API ograniczyć możliwość stosowania nagłówków (w szczególności w żądaniach Server-Sent zdarzeń i technologia WebSockets). W takich przypadkach token dostępu jest podana jako wartość ciągu kwerendy `access_token`.

W kliencie programu .NET `AccessTokenProvider` opcję można określić za pomocą opcji delegata `WithUrl`:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        }
    })
    .Build();
```

W kliencie JavaScript token dostępu jest skonfigurowana przez ustawienie `accessTokenFactory` na obiekt opcje w `withUrl`:

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

### <a name="configure-timeout-and-keep-alive-options"></a>Skonfiguruj opcje keep-alive i limit czasu

Dodatkowe opcje dotyczące konfigurowania limitu czasu i zachowanie keep-alive są dostępne na `HubConnection` samego obiektu:

| Opcja .NET | Opcja JavaScript | Opis |
| ----------- | ----------------- | ----------- |
| `ServerTimeout` | `serverTimeoutInMilliseconds` | Limit czasu dla działania serwera. Jeśli serwer nie wysłał wiadomości w tym interwał, klient traktuje serwera rozłączona i wyzwalacza `Closed` zdarzenia (`onclose` w języku JavaScript). |
| `HandshakeTimeout` | Nie można skonfigurować | Limit czasu dla serwera początkowego uzgadniania. Jeśli serwer nie wysłał odpowiedź uzgadniania w tym interwał, klient anuluje uzgadnianiu i wyzwalacza `Closed` zdarzenia (`onclose` w języku JavaScript). |

W kliencie programu .NET wartości limitu czasu są określane jako `TimeSpan` wartości. W kliencie JavaScript wartości limitu czasu są określane jako liczby. Liczby reprezentują wartości czasu w milisekundach.

### <a name="configure-additional-options"></a>Konfigurowanie dodatkowych opcji

Można skonfigurować dodatkowe opcje w `WithUrl` (`withUrl` w języku JavaScript) Metoda `HubConnectionBuilder`:

| Opcja .NET | Opcja JavaScript | Opis |
| ----------- | ----------------- | ----------- |
| `AccessTokenProvider` | `accessTokenFactory` | Funkcja zwraca ciąg, który jest dostępna jako token uwierzytelniania elementu nośnego w żądania HTTP. |
| `SkipNegotiation` | `skipNegotiation` | Ustaw tę wartość na `true` Aby pominąć krok negocjacji. **Obsługiwana tylko w przypadku transportu Websocket jest tylko włączone transportowej**. Nie można włączyć to ustawienie, jeśli przy użyciu usługi Azure SignalR. |
| `ClientCertificates` | Nie można skonfigurować * | Kolekcja certyfikaty protokołu TLS do wysłania do uwierzytelniania żądań. |
| `Cookies` | Nie można skonfigurować * | Kolekcja plików cookie protokołu HTTP do wysłania z każdego żądania HTTP. |
| `Credentials` | Nie można skonfigurować * | Poświadczenia, aby wysłać z każdym żądaniem HTTP. |
| `CloseTimeout` | Nie można skonfigurować * | Tylko Websocket. Maksymalna ilość czasu oczekiwania przez klienta po zamknięciu dla serwera, aby potwierdzić żądania zamknięcia. Jeśli serwer nie potwierdzić zamknięcia w tym czasie, klient odłączy się. |
| `Headers` | Nie można skonfigurować * | Słownik zawierający dodatkowe nagłówki HTTP do wysłania z każdego żądania HTTP. |
| `HttpMessageHandlerFactory` | Nie można skonfigurować * | Delegata, który może służyć do konfigurowania lub Zastąp `HttpMessageHandler` używane do wysyłania żądań HTTP. Nie są używane dla połączeń protokołu WebSocket. Ten delegat musi zwracać wartość inną niż null i otrzymuje wartość domyślna jako parametr. Zmodyfikuj ustawienia na tej wartości domyślnej i przywrócić go lub powrócić całkowicie nowe `HttpMessageHandler` wystąpienia. |
| `Proxy` | Nie można skonfigurować * | Serwer proxy HTTP do użycia podczas wysyłania żądań HTTP. |
| `UseDefaultCredentials` | Nie można skonfigurować * | Ustaw to boolean, można wysłać poświadczeń domyślnych dla żądań HTTP i technologia WebSockets. Umożliwia to korzystanie z uwierzytelniania systemu Windows. |
| `WebSocketConfiguration` | Nie można skonfigurować * | Delegat, który można skonfigurować dodatkowe opcje protokołu WebSocket. Otrzymuje wystąpienie elementu [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) można skonfigurować opcje. |

Nie można skonfigurować w kliencie JavaScript, ze względu na ograniczenia w przeglądarce interfejsy API są opcje oznaczone gwiazdką (*).

W kliencie programu .NET, te opcje mogą być modyfikowane przez delegata opcje przekazane do `WithUrl`:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

W kliencie JavaScript, te opcje można podać w obiekcie JavaScript do `withUrl`:

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    });
    .build();
```

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>
