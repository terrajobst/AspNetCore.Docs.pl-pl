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
# <a name="aspnet-core-signalr-configuration"></a><span data-ttu-id="33565-103">Konfiguracja programu ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="33565-103">ASP.NET Core SignalR configuration</span></span>

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="33565-104">Opcje serializacji JSON/MessagePack</span><span class="sxs-lookup"><span data-stu-id="33565-104">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="33565-105">ASP.NET Core SignalR obsługuje dwa protokoły kodowania wiadomości: [JSON](https://www.json.org/) i [MessagePack](https://msgpack.org/index.html).</span><span class="sxs-lookup"><span data-stu-id="33565-105">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="33565-106">Każdy protokół ma opcji konfiguracji serializacji.</span><span class="sxs-lookup"><span data-stu-id="33565-106">Each protocol has serialization configuration options.</span></span>

<span data-ttu-id="33565-107">Serializacja kodu JSON można skonfigurować na serwerze przy użyciu [ `AddJsonProtocol` ](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) — metoda rozszerzenia, które mogą być dodawane po [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) w Twojej `Startup.ConfigureServices` metody.</span><span class="sxs-lookup"><span data-stu-id="33565-107">JSON serialization can be configured on the server using the [`AddJsonProtocol`](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method, which can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in your `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="33565-108">`AddJsonProtocol` Metoda przyjmuje delegata, który odbiera `options` obiektu.</span><span class="sxs-lookup"><span data-stu-id="33565-108">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="33565-109">[ `PayloadSerializerSettings` ](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) Właściwość obiektu, na którym jest JSON.NET `JsonSerializerSettings` obiektu, który może służyć do konfigurowania serializacji argumenty i zwracać wartości.</span><span class="sxs-lookup"><span data-stu-id="33565-109">The [`PayloadSerializerSettings`](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) property on that object is a JSON.NET `JsonSerializerSettings` object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="33565-110">Zobacz [dokumentacji struktury JSON.NET](https://www.newtonsoft.com/json/help/html/Introduction.htm) więcej szczegółów.</span><span class="sxs-lookup"><span data-stu-id="33565-110">See the [JSON.NET Documentation](https://www.newtonsoft.com/json/help/html/Introduction.htm) for more details.</span></span>

<span data-ttu-id="33565-111">Na przykład aby skonfigurować serializator do użycia zamiast domyślnej nazwy "(camelcase)", "PascalCase" nazwy właściwości, użyć poniższego kodu:</span><span class="sxs-lookup"><span data-stu-id="33565-111">As an example, to configure the serializer to use "PascalCase" property names, instead of the default "camelCase" names, use the following code:</span></span>

```csharp
services.AddSignalR()
    .AddJsonHubProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver = 
        new DefaultContractResolver();
    });
```

<span data-ttu-id="33565-112">W kliencie programu .NET, takie same `AddJsonHubProtocol` — metoda rozszerzenia istnieje na [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="33565-112">In the .NET client, the same `AddJsonHubProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="33565-113">`Microsoft.Extensions.DependencyInjection` Przestrzeni nazw, należy zaimportować do rozpoznania — metoda rozszerzenia:</span><span class="sxs-lookup"><span data-stu-id="33565-113">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

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
> <span data-ttu-id="33565-114">Nie jest możliwe Konfigurowanie serializacji JSON w kliencie JavaScript w tej chwili.</span><span class="sxs-lookup"><span data-stu-id="33565-114">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="33565-115">MessagePack szeregowanie opcje</span><span class="sxs-lookup"><span data-stu-id="33565-115">MessagePack serialization options</span></span>

<span data-ttu-id="33565-116">Można skonfigurować MessagePack szeregowanie zapewniając pełnomocnika, aby [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) wywołania.</span><span class="sxs-lookup"><span data-stu-id="33565-116">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="33565-117">Zobacz [MessagePack w SignalR](xref:signalr/messagepackhubprotocol) więcej szczegółów.</span><span class="sxs-lookup"><span data-stu-id="33565-117">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="33565-118">Nie jest możliwe skonfigurować MessagePack szeregowanie w kliencie JavaScript w tej chwili.</span><span class="sxs-lookup"><span data-stu-id="33565-118">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="33565-119">Konfigurowanie opcji serwera</span><span class="sxs-lookup"><span data-stu-id="33565-119">Configure server options</span></span>

<span data-ttu-id="33565-120">W poniższej tabeli opisano opcje konfigurowania koncentratorów SignalR:</span><span class="sxs-lookup"><span data-stu-id="33565-120">The following table describes options for configuring SignalR hubs:</span></span>

| <span data-ttu-id="33565-121">Opcja</span><span class="sxs-lookup"><span data-stu-id="33565-121">Option</span></span> | <span data-ttu-id="33565-122">Opis</span><span class="sxs-lookup"><span data-stu-id="33565-122">Description</span></span> |
| ------ | ----------- |
| `HandshakeTimeout` | <span data-ttu-id="33565-123">Klient nie wysłania komunikatu uzgadniania początkowej w tym przedziale czasu, połączenie jest zamknięte.</span><span class="sxs-lookup"><span data-stu-id="33565-123">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="33565-124">Jeśli serwer nie wysłał wiadomość, w tym przedziale czasu, wiadomość ping są wysyłane automatycznie, aby utrzymać otwarte połączenie.</span><span class="sxs-lookup"><span data-stu-id="33565-124">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> |
| `SupportedProtocols` | <span data-ttu-id="33565-125">Protokoły obsługiwane przez tego koncentratora.</span><span class="sxs-lookup"><span data-stu-id="33565-125">Protocols supported by this hub.</span></span> <span data-ttu-id="33565-126">Domyślnie wszystkie protokoły w zarejestrowany na serwerze są dozwolone, ale protokoły można usunąć z tej listy, aby wyłączyć określone protokoły dla poszczególnych koncentratorów.</span><span class="sxs-lookup"><span data-stu-id="33565-126">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | <span data-ttu-id="33565-127">Jeśli `true`, szczegółowe komunikaty o wyjątkach są zwracane do klientów, gdy wyjątek w metodzie koncentratora.</span><span class="sxs-lookup"><span data-stu-id="33565-127">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="33565-128">Wartość domyślna to `false`, jak te komunikaty o wyjątkach mogą zawierać poufne informacje.</span><span class="sxs-lookup"><span data-stu-id="33565-128">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

<span data-ttu-id="33565-129">Można skonfigurować opcje dla wszystkich koncentratorów zapewniając delegata opcje do `AddSignalR` wywołanie w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="33565-129">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

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

<span data-ttu-id="33565-130">Opcje dotyczące pojedynczego koncentratora zastąpienia globalnych opcji dostępnych w `AddSignalR` i można je skonfigurować przy użyciu [ `AddHubOptions<T>` ](/dotnet/api/microsoft.extensions.dependencyinjection.huboptionsdependencyinjectionextensions.addhuboptions):</span><span class="sxs-lookup"><span data-stu-id="33565-130">Options for a single hub override the global options provided in `AddSignalR` and can be configured using [`AddHubOptions<T>`](/dotnet/api/microsoft.extensions.dependencyinjection.huboptionsdependencyinjectionextensions.addhuboptions):</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
}
```

<span data-ttu-id="33565-131">Użyj `HttpConnectionDispatcherOptions` Aby skonfigurować ustawienia zaawansowane powiązane z transportami i Zarządzanie buforem pamięci.</span><span class="sxs-lookup"><span data-stu-id="33565-131">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="33565-132">Te opcje są konfigurowane przez przekazywanie delegata do [ `MapHub<T>` ](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub).</span><span class="sxs-lookup"><span data-stu-id="33565-132">These options are configured by passing a delegate to [`MapHub<T>`](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub).</span></span>

| <span data-ttu-id="33565-133">Opcja</span><span class="sxs-lookup"><span data-stu-id="33565-133">Option</span></span> | <span data-ttu-id="33565-134">Opis</span><span class="sxs-lookup"><span data-stu-id="33565-134">Description</span></span> |
| ------ | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="33565-135">Maksymalna liczba bajtów odebranych z klienta który bufory serwera.</span><span class="sxs-lookup"><span data-stu-id="33565-135">The maximum number of bytes received from the client that the server buffers.</span></span> <span data-ttu-id="33565-136">Zwiększenie tej wartości umożliwia serwer do odbierania wiadomości większych, ale może niekorzystnie wpłynąć na zmniejszenie zużycia pamięci.</span><span class="sxs-lookup"><span data-stu-id="33565-136">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> <span data-ttu-id="33565-137">Wartość domyślna to 32KB.</span><span class="sxs-lookup"><span data-stu-id="33565-137">The default value is 32KB.</span></span> |
| `AuthorizationData` | <span data-ttu-id="33565-138">Lista [ `IAuthorizeData` ](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) obiekty używane do określenia, czy klient jest autoryzowany do nawiązania połączenia z koncentratorem.</span><span class="sxs-lookup"><span data-stu-id="33565-138">A list of [`IAuthorizeData`](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> <span data-ttu-id="33565-139">Domyślnie jest to wypełnione wartościami z `Authorize` atrybuty stosowane do klasy koncentratora.</span><span class="sxs-lookup"><span data-stu-id="33565-139">By default, this is populated with values from the `Authorize` attributes applied to the Hub class.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="33565-140">Maksymalna liczba bajtów wysłanych przez aplikację który bufory serwera.</span><span class="sxs-lookup"><span data-stu-id="33565-140">The maximum number of bytes sent by the app that the server buffers.</span></span> <span data-ttu-id="33565-141">Zwiększenie tej wartości umożliwia serwerowi na wysyłanie wiadomości większych, ale może niekorzystnie wpłynąć na zmniejszenie zużycia pamięci.</span><span class="sxs-lookup"><span data-stu-id="33565-141">Increasing this value allows the server to send larger messages, but can negatively impact memory consumption.</span></span> <span data-ttu-id="33565-142">Wartość domyślna to 32KB.</span><span class="sxs-lookup"><span data-stu-id="33565-142">The default value is 32KB.</span></span> |
| `Transports` | <span data-ttu-id="33565-143">Maska bitowa `HttpTransportType` wartości, które mogą ograniczyć transportów klienta można się połączyć.</span><span class="sxs-lookup"><span data-stu-id="33565-143">A bitmask of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> <span data-ttu-id="33565-144">Wszystkich transportów są domyślnie włączone.</span><span class="sxs-lookup"><span data-stu-id="33565-144">All transports are enabled by default.</span></span> |
| `LongPolling` | <span data-ttu-id="33565-145">Dodatkowe opcje specyficzne dla transportu sondowania długo.</span><span class="sxs-lookup"><span data-stu-id="33565-145">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="33565-146">Dodatkowe opcje specyficzne dla transportu protokołu WebSockets.</span><span class="sxs-lookup"><span data-stu-id="33565-146">Additional options specific to the WebSockets transport.</span></span> |

<span data-ttu-id="33565-147">Transport sondowania długi ma dodatkowe opcje, które można skonfigurować za pomocą `LongPolling` właściwości:</span><span class="sxs-lookup"><span data-stu-id="33565-147">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="33565-148">Opcja</span><span class="sxs-lookup"><span data-stu-id="33565-148">Option</span></span> | <span data-ttu-id="33565-149">Opis</span><span class="sxs-lookup"><span data-stu-id="33565-149">Description</span></span> |
| ------ | ----------- |
| `PollTimeout` | <span data-ttu-id="33565-150">Maksymalna ilość czasu oczekiwania przez serwer komunikat do wysłania do klienta przed zakończeniem żądanie sondowania pojedynczego.</span><span class="sxs-lookup"><span data-stu-id="33565-150">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="33565-151">Zmniejszenie tej wartości powoduje, że klientowi wygenerowanie nowego żądania sondowania częściej.</span><span class="sxs-lookup"><span data-stu-id="33565-151">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> <span data-ttu-id="33565-152">Wartość domyślna wynosi 90 s.</span><span class="sxs-lookup"><span data-stu-id="33565-152">The default value is 90 seconds.</span></span> |

<span data-ttu-id="33565-153">WebSocket transport ma dodatkowe opcje, które można skonfigurować za pomocą `WebSockets` właściwości:</span><span class="sxs-lookup"><span data-stu-id="33565-153">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="33565-154">Opcja</span><span class="sxs-lookup"><span data-stu-id="33565-154">Option</span></span> | <span data-ttu-id="33565-155">Opis</span><span class="sxs-lookup"><span data-stu-id="33565-155">Description</span></span> |
| ------ | ----------- |
| `CloseTimeout` | <span data-ttu-id="33565-156">Po zamknięciu serwera, jeśli klient nie może zamknąć w tym przedziale czasu, połączenie zostało zakończone.</span><span class="sxs-lookup"><span data-stu-id="33565-156">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | <span data-ttu-id="33565-157">Delegata, który można używać do konfigurowania `Sec-WebSocket-Protocol` nagłówka do niestandardowej wartości.</span><span class="sxs-lookup"><span data-stu-id="33565-157">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="33565-158">Delegat odbiera wartości żądany przez klienta jako dane wejściowe i powinien zwrócić żądaną wartość.</span><span class="sxs-lookup"><span data-stu-id="33565-158">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="33565-159">Skonfiguruj opcje klienta</span><span class="sxs-lookup"><span data-stu-id="33565-159">Configure client options</span></span>

<span data-ttu-id="33565-160">Opcje klientów można skonfigurować na `HubConnectionBuilder` typu (dostępne w klientach .NET i JavaScript) również na dzień `HubConnection` samej siebie.</span><span class="sxs-lookup"><span data-stu-id="33565-160">Client options can be configured on the `HubConnectionBuilder` type (available in both .NET and JavaScript clients), as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="33565-161">Konfigurowanie rejestrowania</span><span class="sxs-lookup"><span data-stu-id="33565-161">Configure logging</span></span>

<span data-ttu-id="33565-162">Rejestrowanie skonfigurowano w kliencie programu .NET przy użyciu `ConfigureLogging` metody.</span><span class="sxs-lookup"><span data-stu-id="33565-162">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="33565-163">Rejestrowanie dostawcy i filtrów można zarejestrować w taki sam sposób jak na serwerze.</span><span class="sxs-lookup"><span data-stu-id="33565-163">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="33565-164">Zobacz [logowanie do platformy ASP.NET Core](xref:fundamentals/logging/index#how-to-add-providers) dokumentacji, aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="33565-164">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index#how-to-add-providers) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="33565-165">Aby zarejestrować dostawców rejestrowanie, musisz zainstalować wymaganych pakietów.</span><span class="sxs-lookup"><span data-stu-id="33565-165">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="33565-166">Zobacz [dostawców wbudowanych rejestrowania](xref:fundamentals/logging/index#built-in-logging-providers) części dokumentów, aby uzyskać pełną listę.</span><span class="sxs-lookup"><span data-stu-id="33565-166">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="33565-167">Na przykład, aby włączyć rejestrowanie konsoli, należy zainstalować `Microsoft.Extensions.Logging.Console` pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="33565-167">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="33565-168">Wywołanie `AddConsole` — metoda rozszerzenia:</span><span class="sxs-lookup"><span data-stu-id="33565-168">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="33565-169">W kliencie JavaScript, podobne `configureLogging` istnieje metoda.</span><span class="sxs-lookup"><span data-stu-id="33565-169">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="33565-170">Podaj `LogLevel` wartość wskazującą, minimalny poziom dziennika komunikatów do produkcji.</span><span class="sxs-lookup"><span data-stu-id="33565-170">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="33565-171">Dzienniki są zapisywane w oknie konsoli przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="33565-171">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

> [!NOTE]
> <span data-ttu-id="33565-172">Aby wyłączyć rejestrowanie całkowicie, określ `signalR.LogLevel.None` w `configureLogging` metody.</span><span class="sxs-lookup"><span data-stu-id="33565-172">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="33565-173">Poniżej przedstawiono dostępne dla klienta JavaScript poziomy dziennika.</span><span class="sxs-lookup"><span data-stu-id="33565-173">Log levels available to the JavaScript client are listed below.</span></span> <span data-ttu-id="33565-174">Ustawienie poziomu dziennika na jedną z następujących wartości umożliwia rejestrowanie komunikatów na **lub nowszej** tego poziomu.</span><span class="sxs-lookup"><span data-stu-id="33565-174">Setting the log level to one of these values enables logging of messages at **or above** that level.</span></span>

| <span data-ttu-id="33565-175">Poziom</span><span class="sxs-lookup"><span data-stu-id="33565-175">Level</span></span> | <span data-ttu-id="33565-176">Opis</span><span class="sxs-lookup"><span data-stu-id="33565-176">Description</span></span> |
| ----- | ----------- |
| `None` | <span data-ttu-id="33565-177">Żadne komunikaty są rejestrowane.</span><span class="sxs-lookup"><span data-stu-id="33565-177">No messages are logged.</span></span> |
| `Critical` | <span data-ttu-id="33565-178">Komunikaty wskazujących na niepowodzenie w całej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="33565-178">Messages that indicate a failure in the entire app.</span></span> |
| `Error` | <span data-ttu-id="33565-179">Komunikaty wskazujących na niepowodzenie w bieżącej operacji.</span><span class="sxs-lookup"><span data-stu-id="33565-179">Messages that indicate a failure in the current operation.</span></span> |
| `Warning` | <span data-ttu-id="33565-180">Komunikaty, które wskazują na problem z systemem innym niż krytyczny.</span><span class="sxs-lookup"><span data-stu-id="33565-180">Messages that indicate a non-fatal problem.</span></span> |
| `Information` | <span data-ttu-id="33565-181">Komunikaty informacyjne.</span><span class="sxs-lookup"><span data-stu-id="33565-181">Informational messages.</span></span> |
| `Debug` | <span data-ttu-id="33565-182">Komunikaty diagnostyczne przydatne w przypadku debugowania.</span><span class="sxs-lookup"><span data-stu-id="33565-182">Diagnostic messages useful for debugging.</span></span> |
| `Trace` | <span data-ttu-id="33565-183">Bardzo szczegółowe komunikaty diagnostyczne przeznaczone do diagnozowania określonych problemów.</span><span class="sxs-lookup"><span data-stu-id="33565-183">Very detailed diagnostic messages designed for diagnosing specific issues.</span></span> |

### <a name="configure-allowed-transports"></a><span data-ttu-id="33565-184">Konfigurowanie transportów dozwolone</span><span class="sxs-lookup"><span data-stu-id="33565-184">Configure allowed transports</span></span>

<span data-ttu-id="33565-185">Transporty używane przez SignalR można skonfigurować w `WithUrl` wywołania (`withUrl` w języku JavaScript).</span><span class="sxs-lookup"><span data-stu-id="33565-185">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="33565-186">Alternatywy wartości `HttpTransportType` może służyć do ograniczania klienta można używać tylko określonych transportów.</span><span class="sxs-lookup"><span data-stu-id="33565-186">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="33565-187">Wszystkich transportów są domyślnie włączone.</span><span class="sxs-lookup"><span data-stu-id="33565-187">All transports are enabled by default.</span></span>

<span data-ttu-id="33565-188">Na przykład aby wyłączyć transportu Server-Sent zdarzeń, ale zezwalaj na połączenia Websocket i długi sondowania:</span><span class="sxs-lookup"><span data-stu-id="33565-188">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="33565-189">W kliencie JavaScript transportu są konfigurowane przez ustawienie `transport` na obiekt opcje przekazane do `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="33565-189">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

### <a name="configure-bearer-authentication"></a><span data-ttu-id="33565-190">Konfigurowanie uwierzytelniania elementu nośnego</span><span class="sxs-lookup"><span data-stu-id="33565-190">Configure bearer authentication</span></span>

<span data-ttu-id="33565-191">Aby dostarczyć dane uwierzytelniania wraz z SignalR żądania, użyj `AccessTokenProvider` opcji (`accessTokenFactory` w języku JavaScript) aby określić funkcję, która zwraca token żądany dostęp.</span><span class="sxs-lookup"><span data-stu-id="33565-191">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="33565-192">W kliencie programu .NET ten token dostępu jest przekazywany jako "Uwierzytelniania elementu nośnego" HTTP tokenu (używanie `Authorization` nagłówka o typie `Bearer`).</span><span class="sxs-lookup"><span data-stu-id="33565-192">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="33565-193">W kliencie JavaScript token dostępu jest używany jako tokenu elementu nośnego, **z wyjątkiem** w niektórych przypadkach, gdy przeglądarki interfejsów API ograniczyć możliwość stosowania nagłówków (w szczególności w żądaniach Server-Sent zdarzeń i technologia WebSockets).</span><span class="sxs-lookup"><span data-stu-id="33565-193">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="33565-194">W takich przypadkach token dostępu jest podana jako wartość ciągu kwerendy `access_token`.</span><span class="sxs-lookup"><span data-stu-id="33565-194">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="33565-195">W kliencie programu .NET `AccessTokenProvider` opcję można określić za pomocą opcji delegata `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="33565-195">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        }
    })
    .Build();
```

<span data-ttu-id="33565-196">W kliencie JavaScript token dostępu jest skonfigurowana przez ustawienie `accessTokenFactory` na obiekt opcje w `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="33565-196">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

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

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="33565-197">Skonfiguruj opcje keep-alive i limit czasu</span><span class="sxs-lookup"><span data-stu-id="33565-197">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="33565-198">Dodatkowe opcje dotyczące konfigurowania limitu czasu i zachowanie keep-alive są dostępne na `HubConnection` samego obiektu:</span><span class="sxs-lookup"><span data-stu-id="33565-198">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>

| <span data-ttu-id="33565-199">Opcja .NET</span><span class="sxs-lookup"><span data-stu-id="33565-199">.NET Option</span></span> | <span data-ttu-id="33565-200">Opcja JavaScript</span><span class="sxs-lookup"><span data-stu-id="33565-200">JavaScript Option</span></span> | <span data-ttu-id="33565-201">Opis</span><span class="sxs-lookup"><span data-stu-id="33565-201">Description</span></span> |
| ----------- | ----------------- | ----------- |
| `ServerTimeout` | `serverTimeoutInMilliseconds` | <span data-ttu-id="33565-202">Limit czasu dla działania serwera.</span><span class="sxs-lookup"><span data-stu-id="33565-202">Timeout for server activity.</span></span> <span data-ttu-id="33565-203">Jeśli serwer nie wysłał wiadomości w tym interwał, klient traktuje serwera rozłączona i wyzwalacza `Closed` zdarzenia (`onclose` w języku JavaScript).</span><span class="sxs-lookup"><span data-stu-id="33565-203">If the server hasn't sent any message in this interval, the client considers the server disconnected and trigger the `Closed` event (`onclose` in JavaScript).</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="33565-204">Nie można skonfigurować</span><span class="sxs-lookup"><span data-stu-id="33565-204">Not configurable</span></span> | <span data-ttu-id="33565-205">Limit czasu dla serwera początkowego uzgadniania.</span><span class="sxs-lookup"><span data-stu-id="33565-205">Timeout for initial server handshake.</span></span> <span data-ttu-id="33565-206">Jeśli serwer nie wysłał odpowiedź uzgadniania w tym interwał, klient anuluje uzgadnianiu i wyzwalacza `Closed` zdarzenia (`onclose` w języku JavaScript).</span><span class="sxs-lookup"><span data-stu-id="33565-206">If the server doesn't send a handshake response in this interval, the client cancels the handshake and trigger the `Closed` event (`onclose` in JavaScript).</span></span> |

<span data-ttu-id="33565-207">W kliencie programu .NET wartości limitu czasu są określane jako `TimeSpan` wartości.</span><span class="sxs-lookup"><span data-stu-id="33565-207">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span> <span data-ttu-id="33565-208">W kliencie JavaScript wartości limitu czasu są określane jako liczby.</span><span class="sxs-lookup"><span data-stu-id="33565-208">In the JavaScript client, timeout values are specified as numbers.</span></span> <span data-ttu-id="33565-209">Liczby reprezentują wartości czasu w milisekundach.</span><span class="sxs-lookup"><span data-stu-id="33565-209">The numbers represent time values in milliseconds.</span></span>

### <a name="configure-additional-options"></a><span data-ttu-id="33565-210">Konfigurowanie dodatkowych opcji</span><span class="sxs-lookup"><span data-stu-id="33565-210">Configure additional options</span></span>

<span data-ttu-id="33565-211">Można skonfigurować dodatkowe opcje w `WithUrl` (`withUrl` w języku JavaScript) Metoda `HubConnectionBuilder`:</span><span class="sxs-lookup"><span data-stu-id="33565-211">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder`:</span></span>

| <span data-ttu-id="33565-212">Opcja .NET</span><span class="sxs-lookup"><span data-stu-id="33565-212">.NET Option</span></span> | <span data-ttu-id="33565-213">Opcja JavaScript</span><span class="sxs-lookup"><span data-stu-id="33565-213">JavaScript Option</span></span> | <span data-ttu-id="33565-214">Opis</span><span class="sxs-lookup"><span data-stu-id="33565-214">Description</span></span> |
| ----------- | ----------------- | ----------- |
| `AccessTokenProvider` | `accessTokenFactory` | <span data-ttu-id="33565-215">Funkcja zwraca ciąg, który jest dostępna jako token uwierzytelniania elementu nośnego w żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="33565-215">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `skipNegotiation` | <span data-ttu-id="33565-216">Ustaw tę wartość na `true` Aby pominąć krok negocjacji.</span><span class="sxs-lookup"><span data-stu-id="33565-216">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="33565-217">**Obsługiwana tylko w przypadku transportu Websocket jest tylko włączone transportowej**.</span><span class="sxs-lookup"><span data-stu-id="33565-217">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="33565-218">Nie można włączyć to ustawienie, jeśli przy użyciu usługi Azure SignalR.</span><span class="sxs-lookup"><span data-stu-id="33565-218">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="33565-219">Nie można skonfigurować \*</span><span class="sxs-lookup"><span data-stu-id="33565-219">Not configurable \*</span></span> | <span data-ttu-id="33565-220">Kolekcja certyfikaty protokołu TLS do wysłania do uwierzytelniania żądań.</span><span class="sxs-lookup"><span data-stu-id="33565-220">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="33565-221">Nie można skonfigurować \*</span><span class="sxs-lookup"><span data-stu-id="33565-221">Not configurable \*</span></span> | <span data-ttu-id="33565-222">Kolekcja plików cookie protokołu HTTP do wysłania z każdego żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="33565-222">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="33565-223">Nie można skonfigurować \*</span><span class="sxs-lookup"><span data-stu-id="33565-223">Not configurable \*</span></span> | <span data-ttu-id="33565-224">Poświadczenia, aby wysłać z każdym żądaniem HTTP.</span><span class="sxs-lookup"><span data-stu-id="33565-224">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="33565-225">Nie można skonfigurować \*</span><span class="sxs-lookup"><span data-stu-id="33565-225">Not configurable \*</span></span> | <span data-ttu-id="33565-226">Tylko Websocket.</span><span class="sxs-lookup"><span data-stu-id="33565-226">WebSockets only.</span></span> <span data-ttu-id="33565-227">Maksymalna ilość czasu oczekiwania przez klienta po zamknięciu dla serwera, aby potwierdzić żądania zamknięcia.</span><span class="sxs-lookup"><span data-stu-id="33565-227">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="33565-228">Jeśli serwer nie potwierdzić zamknięcia w tym czasie, klient odłączy się.</span><span class="sxs-lookup"><span data-stu-id="33565-228">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="33565-229">Nie można skonfigurować \*</span><span class="sxs-lookup"><span data-stu-id="33565-229">Not configurable \*</span></span> | <span data-ttu-id="33565-230">Słownik zawierający dodatkowe nagłówki HTTP do wysłania z każdego żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="33565-230">A dictionary of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | <span data-ttu-id="33565-231">Nie można skonfigurować \*</span><span class="sxs-lookup"><span data-stu-id="33565-231">Not configurable \*</span></span> | <span data-ttu-id="33565-232">Delegata, który może służyć do konfigurowania lub Zastąp `HttpMessageHandler` używane do wysyłania żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="33565-232">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="33565-233">Nie są używane dla połączeń protokołu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="33565-233">Not used for WebSocket connections.</span></span> <span data-ttu-id="33565-234">Ten delegat musi zwracać wartość inną niż null i otrzymuje wartość domyślna jako parametr.</span><span class="sxs-lookup"><span data-stu-id="33565-234">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="33565-235">Zmodyfikuj ustawienia na tej wartości domyślnej i przywrócić go lub powrócić całkowicie nowe `HttpMessageHandler` wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="33565-235">Either modify settings on that default value and return it, or return a completely new `HttpMessageHandler` instance.</span></span> |
| `Proxy` | <span data-ttu-id="33565-236">Nie można skonfigurować \*</span><span class="sxs-lookup"><span data-stu-id="33565-236">Not configurable \*</span></span> | <span data-ttu-id="33565-237">Serwer proxy HTTP do użycia podczas wysyłania żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="33565-237">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | <span data-ttu-id="33565-238">Nie można skonfigurować \*</span><span class="sxs-lookup"><span data-stu-id="33565-238">Not configurable \*</span></span> | <span data-ttu-id="33565-239">Ustaw to boolean, można wysłać poświadczeń domyślnych dla żądań HTTP i technologia WebSockets.</span><span class="sxs-lookup"><span data-stu-id="33565-239">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="33565-240">Umożliwia to korzystanie z uwierzytelniania systemu Windows.</span><span class="sxs-lookup"><span data-stu-id="33565-240">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | <span data-ttu-id="33565-241">Nie można skonfigurować \*</span><span class="sxs-lookup"><span data-stu-id="33565-241">Not configurable \*</span></span> | <span data-ttu-id="33565-242">Delegat, który można skonfigurować dodatkowe opcje protokołu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="33565-242">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="33565-243">Otrzymuje wystąpienie elementu [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) można skonfigurować opcje.</span><span class="sxs-lookup"><span data-stu-id="33565-243">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

<span data-ttu-id="33565-244">Nie można skonfigurować w kliencie JavaScript, ze względu na ograniczenia w przeglądarce interfejsy API są opcje oznaczone gwiazdką (\*).</span><span class="sxs-lookup"><span data-stu-id="33565-244">Options marked with an asterisk (\*) aren't configurable in the JavaScript client, due to limitations in browser APIs.</span></span>

<span data-ttu-id="33565-245">W kliencie programu .NET, te opcje mogą być modyfikowane przez delegata opcje przekazane do `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="33565-245">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="33565-246">W kliencie JavaScript, te opcje można podać w obiekcie JavaScript do `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="33565-246">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    });
    .build();
```

## <a name="additional-resources"></a><span data-ttu-id="33565-247">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="33565-247">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>
