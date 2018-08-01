---
title: Konfiguracja Core SignalR platformy ASP.NET
author: tdykstra
description: Informacje o sposobie konfigurowania aplikacji ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 07/31/2018
uid: signalr/configuration
ms.openlocfilehash: 32c0ad94fba09fa099c2ab4a6b1d6d79a5542d7f
ms.sourcegitcommit: a25b572eaed21791230c85416f449f66a405ec19
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/01/2018
ms.locfileid: "39396065"
---
# <a name="aspnet-core-signalr-configuration"></a>Konfiguracja Core SignalR platformy ASP.NET

## <a name="jsonmessagepack-serialization-options"></a>Opcje serializacji JSON/MessagePack

Biblioteki SignalR platformy ASP.NET Core obsługuje dwa protokoły kodowania wiadomości: [JSON](https://www.json.org/) i [MessagePack](https://msgpack.org/index.html). Dla każdego protokołu zawiera opcje konfiguracji serializacji.

Serializacja kodu JSON można skonfigurować na serwerze za pomocą [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) metodę rozszerzenia, które mogą być dodawane po [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) w swojej `Startup.ConfigureServices` metody. `AddJsonProtocol` Metoda przyjmuje obiekt delegowany, który odbiera `options` obiektu. [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) właściwość obiektu, na którym jest na składnik JSON.NET `JsonSerializerSettings` obiektu, który może służyć do konfigurowania serializacji argumenty i zwracać wartości. Zobacz [dokumentacji na składnik JSON.NET](https://www.newtonsoft.com/json/help/html/Introduction.htm) Aby uzyskać więcej informacji.

Na przykład aby skonfigurować serializator do użycia nazwy właściwości "PascalCase", zamiast domyślnej nazwy "camelCase", użyj poniższego kodu:

```csharp
services.AddSignalR()
    .AddJsonHubProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver = 
        new DefaultContractResolver();
    });
```

W kliencie programu .NET, takie same `AddJsonHubProtocol` — metoda rozszerzenia istnieje na [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder). `Microsoft.Extensions.DependencyInjection` Przestrzeni nazw, należy zaimportować resolve — metoda rozszerzenia:

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
> Nie jest możliwe do skonfigurowania serializacji JSON w kliencie JavaScript w tej chwili.

### <a name="messagepack-serialization-options"></a>Opcje serializacji MessagePack

Serializacja MessagePack można skonfigurować poprzez dostarczenie delegata do [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) wywołania. Zobacz [MessagePack w SignalR](xref:signalr/messagepackhubprotocol) Aby uzyskać więcej informacji.

> [!NOTE]
> Nie jest możliwe do skonfigurowania MessagePack serializacji w kliencie JavaScript w tej chwili.

## <a name="configure-server-options"></a>Konfigurowanie opcji serwera

W poniższej tabeli opisano opcje dotyczące konfigurowania centrów SignalR:

| Opcja | Wartość domyślna | Opis |
| ------ | ------------- | ----------- |
| `HandshakeTimeout` | 15 sekund | Jeśli klient nie wysyła komunikat uzgadniania połączenia początkowego, w tym przedziale czasu, połączenie jest zamknięte. To ustawienie Zaawansowane, które powinny być modyfikowane tylko, jeśli występują błędy przekroczenia limitu czasu uzgadnianie ze względu na opóźnienie sieci poważne. Aby uzyskać więcej szczegółów na temat procesu uzgadniania, zobacz [specyfikacji protokołu Centrum SignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md). |
| `KeepAliveInterval` | 15 sekund | Jeśli serwer nie wysłał wiadomości, w tym przedziale czasu, wiadomość ping są wysyłane automatycznie do utrzymanie otwartego połączenia. |
| `SupportedProtocols` | Wszystkie zainstalowane protokołów | Protokoły obsługiwane przez tego koncentratora. Domyślnie są dozwolone wszystkie protokoły zarejestrowany na serwerze, ale protokoły mogą być usunięte z tej listy, aby wyłączyć określone protokoły dla poszczególnych centrów. |
| `EnableDetailedErrors` | `false` | Jeśli `true`, szczegółowe komunikaty o wyjątkach są zwracane do klientów, gdy wyjątek jest zgłaszany w przypadku metody koncentratora. Wartość domyślna to `false`, jak te komunikaty o wyjątkach mogą zawierać poufne informacje. |

Można skonfigurować opcje do wszystkich centrów, zapewniając delegat opcje do `AddSignalR` wywołania `Startup.ConfigureServices`.

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

Opcje dla jednego centrum zastępują opcje globalne w `AddSignalR` i może być konfigurowana przy użyciu [AddHubOptions\<T >](/dotnet/api/microsoft.extensions.dependencyinjection.huboptionsdependencyinjectionextensions.addhuboptions):

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
}
```

Użyj `HttpConnectionDispatcherOptions` skonfigurować zaawansowane ustawienia powiązane z transportami i Zarządzanie buforem pamięci. Te opcje są konfigurowane przez przekazanie delegat [MapHub\<T >](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub).

| Opcja | Wartość domyślna | Opis |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | 32 KB. | Maksymalna liczba bajtów odebranych od klienta, bufory serwera. Zwiększenie tej wartości umożliwia serwer do odbierania komunikatów większy, ale może mieć negatywny wpływ na zużycie pamięci. |
| `AuthorizationData` | Dane są automatycznie zbierane z `Authorize` atrybuty stosowane do klasy koncentratora. | Lista [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) obiekty używane do określenia, czy klient jest autoryzowany do podłączać do koncentratora. |
| `TransportMaxBufferSize` | 32 KB. | Maksymalna liczba bajtów wysłanych przez aplikację, bufory serwera. Zwiększenie tej wartości umożliwia serwerowi na wysyłanie wiadomości większych, ale może mieć negatywny wpływ na zużycie pamięci. |
| `Transports` | Wszystkich transportów są włączone. | Maska bitów `HttpTransportType` wartości, które mogą ograniczyć transportów klienta można się połączyć. |
| `LongPolling` | Zobacz poniżej. | Dodatkowe opcje specyficzne dla transportu sondowania długie. |
| `WebSockets` | Zobacz poniżej. | Dodatkowe opcje specyficzne dla transportu funkcji WebSockets. |

Transport sondowania długie ma dodatkowe opcje, które można skonfigurować przy użyciu `LongPolling` właściwości:

| Opcja | Wartość domyślna | Opis |
| ------ | ------------- | ----------- |
| `PollTimeout` | 90 sekund | Maksymalna ilość czasu serwer oczekuje na komunikat do wysłania do klienta przed zakończeniem żądania sondowania pojedynczego. Zmniejszenie tej wartości powoduje, że klient wystawiania nowego żądania sondowania częściej. |

WebSocket transport ma dodatkowe opcje, które można skonfigurować przy użyciu `WebSockets` właściwości:

| Opcja | Wartość domyślna | Opis |
| ------ | ------------- | ----------- |
| `CloseTimeout` | 5 sekund | Po zamknięciu serwera, jeśli klient nie może zamknąć w tym przedziale czasu, połączenie zostanie przerwane. |
| `SubProtocolSelector` | `null` | Delegat, który może służyć do ustawiania `Sec-WebSocket-Protocol` nagłówka niestandardowej wartości. Delegat otrzymuje wartości żądanego przez klienta jako dane wejściowe i powinien zwrócić na żądaną wartość. |

## <a name="configure-client-options"></a>Skonfiguruj opcje klienta

Opcje klienta można skonfigurować na `HubConnectionBuilder` typu (dostępne w klientach .NET i języka JavaScript) oraz stanu na `HubConnection` sam.

### <a name="configure-logging"></a>Konfigurowanie rejestrowania

Rejestrowania skonfigurowano w kliencie programu .NET przy użyciu `ConfigureLogging` metody. Rejestrowanie dostawcy i filtrów można zarejestrować w taki sam sposób jak znajdują się na serwerze. Zobacz [rejestrowania w programie ASP.NET Core](xref:fundamentals/logging/index#how-to-add-providers) dokumentacji, aby uzyskać więcej informacji.

> [!NOTE]
> Aby można było zarejestrować dostawców logowania, należy zainstalować wymagane pakiety. Zobacz [wbudowane funkcje rejestrowania dostawców](xref:fundamentals/logging/index#built-in-logging-providers) sekcji dokumentacji, aby uzyskać pełną listę.

Na przykład, aby włączyć rejestrowanie konsoli, należy zainstalować `Microsoft.Extensions.Logging.Console` pakietu NuGet. Wywołaj `AddConsole` — metoda rozszerzenia:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

W kliencie JavaScript podobny `configureLogging` istnieje metoda. Podaj `LogLevel` wartość wskazującą, minimalny poziom komunikaty dziennika do produkcji. Dzienniki są zapisywane do okna konsoli przeglądarki.

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

> [!NOTE]
> Aby wyłączyć rejestrowanie w całości, należy określić `signalR.LogLevel.None` w `configureLogging` metody.

Poniżej przedstawiono dostępne dla klienta JavaScript poziomy dziennika. Ustawienie poziomu dziennika na jedną z następujących wartości umożliwia rejestrowanie komunikatów na **lub nowszej** tego poziomu.

| Poziom | Opis |
| ----- | ----------- |
| `None` | Żadne komunikaty są rejestrowane. |
| `Critical` | Komunikaty, które wskazują błędy w całej aplikacji. |
| `Error` | Komunikaty, które wskazują błędy w bieżącej operacji. |
| `Warning` | Komunikaty, które wskazują na problem krytyczny. |
| `Information` | Komunikaty informacyjne. |
| `Debug` | Komunikaty diagnostyczne przydatne podczas debugowania. |
| `Trace` | Bardzo szczegółowe komunikaty diagnostyczne przeznaczone dla diagnozowanie konkretnych problemów. |

### <a name="configure-allowed-transports"></a>Konfigurowanie transportów dozwolone

Transporty używane przez element SignalR można skonfigurować w `WithUrl` wywołania (`withUrl` w języku JavaScript). Bitowe OR dla wartości z `HttpTransportType` może służyć do ograniczania klienta do użycia tylko określonych transportów. Wszystkich transportów są domyślnie włączone.

Na przykład aby wyłączyć transportu Server-Sent zdarzenia, ale zezwalaj na WebSockets długie sondowania połączenia i:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

W kliencie JavaScript transportów są skonfigurowane, ustawiając `transport` pola w obiekcie opcje udostępniane `withUrl`:

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

### <a name="configure-bearer-authentication"></a>Konfigurowanie uwierzytelniania elementu nośnego

Aby przekazać dane uwierzytelniania wraz z SignalR żądania, użyj `AccessTokenProvider` opcji (`accessTokenFactory` w języku JavaScript), aby określić funkcję, która zwraca token żądanego dostępu. W kliencie programu .NET ten token dostępu jest przekazywany jako "Uwierzytelniania elementu nośnego" HTTP tokenu (używanie `Authorization` nagłówka o typie `Bearer`). W kliencie JavaScript token dostępu jest używany jako token elementu nośnego **z wyjątkiem** w niektórych przypadkach, gdy przeglądarka interfejsów API ograniczyć możliwość stosowania nagłówki (w szczególności w żądaniach Server-Sent zdarzeń i technologia WebSockets). W takich przypadkach token dostępu jest oferowana jako wartość ciągu zapytania `access_token`.

W kliencie programu .NET `AccessTokenProvider` opcja może być określona za pomocą opcji delegata `WithUrl`:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        }
    })
    .Build();
```

W kliencie JavaScript token dostępu jest skonfigurowana, ustawiając `accessTokenFactory` pola w obiekcie opcje w `withUrl`:

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

### <a name="configure-timeout-and-keep-alive-options"></a>Konfigurowanie limitu czasu i opcje keep-alive

Dodatkowe opcje dotyczące konfigurowania limitu czasu i zachowanie keep-alive są dostępne na `HubConnection` samego obiektu:

| .NET — opcja | Opcja języka JavaScript | Wartość domyślna | Opis |
| ----------- | ----------------- | ------------- | ----------- |
| `ServerTimeout` | `serverTimeoutInMilliseconds` | 30 sekund (ponad 30 000 MS) | Limit czasu aktywności serwera. Jeśli serwer nie wysłał wiadomości, w tym interwał, klient traktuje serwera odłączona i wyzwalacze `Closed` zdarzeń (`onclose` w języku JavaScript). |
| `HandshakeTimeout` | Nie można konfigurować | 15 sekund | Limit czasu dla serwera początkowego uzgadniania. Jeśli serwer nie wysyłać odpowiedzi uzgadniania, w tym interwał, klient anuluje uzgadnianiu i wyzwalacze `Closed` zdarzeń (`onclose` w języku JavaScript). To ustawienie Zaawansowane, które powinny być modyfikowane tylko, jeśli występują błędy przekroczenia limitu czasu uzgadnianie ze względu na opóźnienie sieci poważne. Aby uzyskać więcej szczegółów na temat procesu uzgadniania, zobacz [specyfikacji protokołu Centrum SignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md). |

W kliencie programu .NET wartości limitu czasu są określane jako `TimeSpan` wartości. W kliencie JavaScript wartości limitu czasu są określane jako liczba wskazująca czas trwania (w milisekundach).

### <a name="configure-additional-options"></a>Konfigurowanie opcji dodatkowych

Dodatkowe opcje można skonfigurować w `WithUrl` (`withUrl` w języku JavaScript) metody `HubConnectionBuilder`:

| .NET — opcja | Opcja języka JavaScript | Wartość domyślna | Opis |
| ----------- | ----------------- | ------------- | ----------- |
| `AccessTokenProvider` | `accessTokenFactory` | `null` | Funkcja zwraca ciąg, który jest dostarczana jako token uwierzytelniania elementu nośnego w żądaniach HTTP. |
| `SkipNegotiation` | `skipNegotiation` | `false` | Ustaw tę opcję na `true` Aby pominąć krok negocjacji. **Obsługiwane tylko w przypadku transportu WebSockets jest tylko transportu włączone**. Nie można włączyć to ustawienie, korzystając z usługi Azure SignalR Service. |
| `ClientCertificates` | Nie można skonfigurować * | Pusty | Kolekcja certyfikaty protokołu TLS do wysłania do uwierzytelniania żądań. |
| `Cookies` | Nie można skonfigurować * | Pusty | Kolekcja plików cookie protokołu HTTP ma wysłać z każdym żądaniem HTTP. |
| `Credentials` | Nie można skonfigurować * | Pusty | Poświadczenia, aby wysłać z każdym żądaniem HTTP. |
| `CloseTimeout` | Nie można skonfigurować * | 5 sekund | Tylko WebSockets. Maksymalna ilość czasu klient odczekuje po zamknięciu dla serwera potwierdzić żądanie zamknięcia. Jeśli serwer nie potwierdzenia zamknięcia w tej chwili, klient odłączy się. |
| `Headers` | Nie można skonfigurować * | Pusty | Słownik zawierający dodatkowe nagłówki HTTP ma wysłać z każdym żądaniem HTTP. |
| `HttpMessageHandlerFactory` | Nie można skonfigurować * | `null` | Delegat, który może służyć do konfigurowania lub Zastąp `HttpMessageHandler` używane do wysyłania żądań HTTP. Nie są używane dla połączeń protokołu WebSocket. Ten delegat musi zwracać wartość inną niż null i otrzymuje wartość domyślna, jako parametr. Zmodyfikować ustawienia na tej wartości domyślne i przywrócić go lub zwróć zupełnie nowe `HttpMessageHandler` wystąpienia. |
| `Proxy` | Nie można skonfigurować * | `null` | Serwer proxy HTTP do użycia podczas wysyłania żądań HTTP. |
| `UseDefaultCredentials` | Nie można skonfigurować * | `false` | Ustaw ten atrybut typu wartość logiczna do wysyłania poświadczeń domyślnych dla żądań HTTP i Websocket. Umożliwia to korzystanie z uwierzytelniania Windows. |
| `WebSocketConfiguration` | Nie można skonfigurować * | `null` | Delegat, który można skonfigurować dodatkowe opcje protokołu WebSocket. Otrzymuje wystąpienie elementu [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) można skonfigurować opcje. |

Nie można skonfigurować w klienta JavaScript, ze względu na ograniczenia w przeglądarce interfejsów API są opcje oznaczone gwiazdką (*).

W kliencie programu .NET, te opcje mogą być modyfikowane przez delegata opcje udostępnionego `WithUrl`:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

W kliencie JavaScript, te opcje można podać w obiekcie JavaScript udostępniane `withUrl`:

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
