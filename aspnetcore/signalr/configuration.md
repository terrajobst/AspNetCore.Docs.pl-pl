---
title: Konfiguracja Core SignalR platformy ASP.NET
author: tdykstra
description: Informacje o sposobie konfigurowania aplikacji ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/30/2018
uid: signalr/configuration
ms.openlocfilehash: fac0226c939f4cf446c876b1c0b359d6c5b9dfd3
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095405"
---
# <a name="aspnet-core-signalr-configuration"></a><span data-ttu-id="cd2e5-103">Konfiguracja Core SignalR platformy ASP.NET</span><span class="sxs-lookup"><span data-stu-id="cd2e5-103">ASP.NET Core SignalR configuration</span></span>

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="cd2e5-104">Opcje serializacji JSON/MessagePack</span><span class="sxs-lookup"><span data-stu-id="cd2e5-104">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="cd2e5-105">Biblioteki SignalR platformy ASP.NET Core obsługuje dwa protokoły kodowania wiadomości: [JSON](https://www.json.org/) i [MessagePack](https://msgpack.org/index.html).</span><span class="sxs-lookup"><span data-stu-id="cd2e5-105">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="cd2e5-106">Dla każdego protokołu zawiera opcje konfiguracji serializacji.</span><span class="sxs-lookup"><span data-stu-id="cd2e5-106">Each protocol has serialization configuration options.</span></span>

<span data-ttu-id="cd2e5-107">Serializacja kodu JSON można skonfigurować na serwerze za pomocą [ `AddJsonProtocol` ](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) metodę rozszerzenia, które mogą być dodawane po [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) w swojej `Startup.ConfigureServices` metody.</span><span class="sxs-lookup"><span data-stu-id="cd2e5-107">JSON serialization can be configured on the server using the [`AddJsonProtocol`](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method, which can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in your `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="cd2e5-108">`AddJsonProtocol` Metoda przyjmuje obiekt delegowany, który odbiera `options` obiektu.</span><span class="sxs-lookup"><span data-stu-id="cd2e5-108">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="cd2e5-109">[ `PayloadSerializerSettings` ](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) Właściwość obiektu, na którym jest na składnik JSON.NET `JsonSerializerSettings` obiektu, który może służyć do konfigurowania serializacji argumenty i zwracać wartości.</span><span class="sxs-lookup"><span data-stu-id="cd2e5-109">The [`PayloadSerializerSettings`](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) property on that object is a JSON.NET `JsonSerializerSettings` object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="cd2e5-110">Zobacz [dokumentacji na składnik JSON.NET](https://www.newtonsoft.com/json/help/html/Introduction.htm) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="cd2e5-110">See the [JSON.NET Documentation](https://www.newtonsoft.com/json/help/html/Introduction.htm) for more details.</span></span>

<span data-ttu-id="cd2e5-111">Na przykład aby skonfigurować serializator do użycia nazwy właściwości "PascalCase", zamiast domyślnej nazwy "camelCase", użyj poniższego kodu:</span><span class="sxs-lookup"><span data-stu-id="cd2e5-111">As an example, to configure the serializer to use "PascalCase" property names, instead of the default "camelCase" names, use the following code:</span></span>

```csharp
services.AddSignalR()
    .AddJsonHubProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver = 
        new DefaultContractResolver();
    });
```

<span data-ttu-id="cd2e5-112">W kliencie programu .NET, takie same `AddJsonHubProtocol` — metoda rozszerzenia istnieje na [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="cd2e5-112">In the .NET client, the same `AddJsonHubProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="cd2e5-113">`Microsoft.Extensions.DependencyInjection` Przestrzeni nazw, należy zaimportować resolve — metoda rozszerzenia:</span><span class="sxs-lookup"><span data-stu-id="cd2e5-113">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

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
> <span data-ttu-id="cd2e5-114">Nie jest możliwe do skonfigurowania serializacji JSON w kliencie JavaScript w tej chwili.</span><span class="sxs-lookup"><span data-stu-id="cd2e5-114">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="cd2e5-115">Opcje serializacji MessagePack</span><span class="sxs-lookup"><span data-stu-id="cd2e5-115">MessagePack serialization options</span></span>

<span data-ttu-id="cd2e5-116">Serializacja MessagePack można skonfigurować poprzez dostarczenie delegata do [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) wywołania.</span><span class="sxs-lookup"><span data-stu-id="cd2e5-116">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="cd2e5-117">Zobacz [MessagePack w SignalR](xref:signalr/messagepackhubprotocol) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="cd2e5-117">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="cd2e5-118">Nie jest możliwe do skonfigurowania MessagePack serializacji w kliencie JavaScript w tej chwili.</span><span class="sxs-lookup"><span data-stu-id="cd2e5-118">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="cd2e5-119">Konfigurowanie opcji serwera</span><span class="sxs-lookup"><span data-stu-id="cd2e5-119">Configure server options</span></span>

<span data-ttu-id="cd2e5-120">W poniższej tabeli opisano opcje dotyczące konfigurowania centrów SignalR:</span><span class="sxs-lookup"><span data-stu-id="cd2e5-120">The following table describes options for configuring SignalR hubs:</span></span>

| <span data-ttu-id="cd2e5-121">Opcja</span><span class="sxs-lookup"><span data-stu-id="cd2e5-121">Option</span></span> | <span data-ttu-id="cd2e5-122">Opis</span><span class="sxs-lookup"><span data-stu-id="cd2e5-122">Description</span></span> |
| ------ | ----------- |
| `HandshakeTimeout` | <span data-ttu-id="cd2e5-123">Jeśli klient nie wysyła komunikat uzgadniania połączenia początkowego, w tym przedziale czasu, połączenie jest zamknięte.</span><span class="sxs-lookup"><span data-stu-id="cd2e5-123">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="cd2e5-124">Jeśli serwer nie wysłał wiadomości, w tym przedziale czasu, wiadomość ping są wysyłane automatycznie do utrzymanie otwartego połączenia.</span><span class="sxs-lookup"><span data-stu-id="cd2e5-124">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> |
| `SupportedProtocols` | <span data-ttu-id="cd2e5-125">Protokoły obsługiwane przez tego koncentratora.</span><span class="sxs-lookup"><span data-stu-id="cd2e5-125">Protocols supported by this hub.</span></span> <span data-ttu-id="cd2e5-126">Domyślnie są dozwolone wszystkie protokoły zarejestrowany na serwerze, ale protokoły mogą być usunięte z tej listy, aby wyłączyć określone protokoły dla poszczególnych centrów.</span><span class="sxs-lookup"><span data-stu-id="cd2e5-126">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | <span data-ttu-id="cd2e5-127">Jeśli `true`, szczegółowe komunikaty o wyjątkach są zwracane do klientów, gdy wyjątek jest zgłaszany w przypadku metody koncentratora.</span><span class="sxs-lookup"><span data-stu-id="cd2e5-127">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="cd2e5-128">Wartość domyślna to `false`, jak te komunikaty o wyjątkach mogą zawierać poufne informacje.</span><span class="sxs-lookup"><span data-stu-id="cd2e5-128">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

<span data-ttu-id="cd2e5-129">Można skonfigurować opcje do wszystkich centrów, zapewniając delegat opcje do `AddSignalR` wywołania `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="cd2e5-129">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

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

<span data-ttu-id="cd2e5-130">Opcje dla jednego centrum zastępują opcje globalne w `AddSignalR` i może być konfigurowana przy użyciu [ `AddHubOptions<T>` ](/dotnet/api/microsoft.extensions.dependencyinjection.huboptionsdependencyinjectionextensions.addhuboptions):</span><span class="sxs-lookup"><span data-stu-id="cd2e5-130">Options for a single hub override the global options provided in `AddSignalR` and can be configured using [`AddHubOptions<T>`](/dotnet/api/microsoft.extensions.dependencyinjection.huboptionsdependencyinjectionextensions.addhuboptions):</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
}
```

<span data-ttu-id="cd2e5-131">Użyj `HttpConnectionDispatcherOptions` skonfigurować zaawansowane ustawienia powiązane z transportami i Zarządzanie buforem pamięci.</span><span class="sxs-lookup"><span data-stu-id="cd2e5-131">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="cd2e5-132">Te opcje są konfigurowane przez przekazanie delegat [ `MapHub<T>` ](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub).</span><span class="sxs-lookup"><span data-stu-id="cd2e5-132">These options are configured by passing a delegate to [`MapHub<T>`](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub).</span></span>

| <span data-ttu-id="cd2e5-133">Opcja</span><span class="sxs-lookup"><span data-stu-id="cd2e5-133">Option</span></span> | <span data-ttu-id="cd2e5-134">Opis</span><span class="sxs-lookup"><span data-stu-id="cd2e5-134">Description</span></span> |
| ------ | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="cd2e5-135">Maksymalna liczba bajtów odebranych od klienta, bufory serwera.</span><span class="sxs-lookup"><span data-stu-id="cd2e5-135">The maximum number of bytes received from the client that the server buffers.</span></span> <span data-ttu-id="cd2e5-136">Zwiększenie tej wartości umożliwia serwer do odbierania komunikatów większy, ale może mieć negatywny wpływ na zużycie pamięci.</span><span class="sxs-lookup"><span data-stu-id="cd2e5-136">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> <span data-ttu-id="cd2e5-137">Wartość domyślna to 32KB.</span><span class="sxs-lookup"><span data-stu-id="cd2e5-137">The default value is 32KB.</span></span> |
| `AuthorizationData` | <span data-ttu-id="cd2e5-138">Lista [ `IAuthorizeData` ](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) obiekty używane do określenia, czy klient jest autoryzowany do podłączać do koncentratora.</span><span class="sxs-lookup"><span data-stu-id="cd2e5-138">A list of [`IAuthorizeData`](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> <span data-ttu-id="cd2e5-139">Domyślnie jest to wypełnione wartościami z `Authorize` atrybuty stosowane do klasy koncentratora.</span><span class="sxs-lookup"><span data-stu-id="cd2e5-139">By default, this is populated with values from the `Authorize` attributes applied to the Hub class.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="cd2e5-140">Maksymalna liczba bajtów wysłanych przez aplikację, bufory serwera.</span><span class="sxs-lookup"><span data-stu-id="cd2e5-140">The maximum number of bytes sent by the app that the server buffers.</span></span> <span data-ttu-id="cd2e5-141">Zwiększenie tej wartości umożliwia serwerowi na wysyłanie wiadomości większych, ale może mieć negatywny wpływ na zużycie pamięci.</span><span class="sxs-lookup"><span data-stu-id="cd2e5-141">Increasing this value allows the server to send larger messages, but can negatively impact memory consumption.</span></span> <span data-ttu-id="cd2e5-142">Wartość domyślna to 32KB.</span><span class="sxs-lookup"><span data-stu-id="cd2e5-142">The default value is 32KB.</span></span> |
| `Transports` | <span data-ttu-id="cd2e5-143">Maska bitów `HttpTransportType` wartości, które mogą ograniczyć transportów klienta można się połączyć.</span><span class="sxs-lookup"><span data-stu-id="cd2e5-143">A bitmask of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> <span data-ttu-id="cd2e5-144">Wszystkich transportów są domyślnie włączone.</span><span class="sxs-lookup"><span data-stu-id="cd2e5-144">All transports are enabled by default.</span></span> |
| `LongPolling` | <span data-ttu-id="cd2e5-145">Dodatkowe opcje specyficzne dla transportu sondowania długie.</span><span class="sxs-lookup"><span data-stu-id="cd2e5-145">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="cd2e5-146">Dodatkowe opcje specyficzne dla transportu funkcji WebSockets.</span><span class="sxs-lookup"><span data-stu-id="cd2e5-146">Additional options specific to the WebSockets transport.</span></span> |

<span data-ttu-id="cd2e5-147">Transport sondowania długie ma dodatkowe opcje, które można skonfigurować przy użyciu `LongPolling` właściwości:</span><span class="sxs-lookup"><span data-stu-id="cd2e5-147">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="cd2e5-148">Opcja</span><span class="sxs-lookup"><span data-stu-id="cd2e5-148">Option</span></span> | <span data-ttu-id="cd2e5-149">Opis</span><span class="sxs-lookup"><span data-stu-id="cd2e5-149">Description</span></span> |
| ------ | ----------- |
| `PollTimeout` | <span data-ttu-id="cd2e5-150">Maksymalna ilość czasu serwer oczekuje na komunikat do wysłania do klienta przed zakończeniem żądania sondowania pojedynczego.</span><span class="sxs-lookup"><span data-stu-id="cd2e5-150">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="cd2e5-151">Zmniejszenie tej wartości powoduje, że klient wystawiania nowego żądania sondowania częściej.</span><span class="sxs-lookup"><span data-stu-id="cd2e5-151">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> <span data-ttu-id="cd2e5-152">Wartość domyślna to 90 sekund.</span><span class="sxs-lookup"><span data-stu-id="cd2e5-152">The default value is 90 seconds.</span></span> |

<span data-ttu-id="cd2e5-153">WebSocket transport ma dodatkowe opcje, które można skonfigurować przy użyciu `WebSockets` właściwości:</span><span class="sxs-lookup"><span data-stu-id="cd2e5-153">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="cd2e5-154">Opcja</span><span class="sxs-lookup"><span data-stu-id="cd2e5-154">Option</span></span> | <span data-ttu-id="cd2e5-155">Opis</span><span class="sxs-lookup"><span data-stu-id="cd2e5-155">Description</span></span> |
| ------ | ----------- |
| `CloseTimeout` | <span data-ttu-id="cd2e5-156">Po zamknięciu serwera, jeśli klient nie może zamknąć w tym przedziale czasu, połączenie zostanie przerwane.</span><span class="sxs-lookup"><span data-stu-id="cd2e5-156">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | <span data-ttu-id="cd2e5-157">Delegat, który może służyć do ustawiania `Sec-WebSocket-Protocol` nagłówka niestandardowej wartości.</span><span class="sxs-lookup"><span data-stu-id="cd2e5-157">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="cd2e5-158">Delegat otrzymuje wartości żądanego przez klienta jako dane wejściowe i powinien zwrócić na żądaną wartość.</span><span class="sxs-lookup"><span data-stu-id="cd2e5-158">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="cd2e5-159">Skonfiguruj opcje klienta</span><span class="sxs-lookup"><span data-stu-id="cd2e5-159">Configure client options</span></span>

<span data-ttu-id="cd2e5-160">Opcje klienta można skonfigurować na `HubConnectionBuilder` typu (dostępne w klientach .NET i języka JavaScript) oraz stanu na `HubConnection` sam.</span><span class="sxs-lookup"><span data-stu-id="cd2e5-160">Client options can be configured on the `HubConnectionBuilder` type (available in both .NET and JavaScript clients), as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="cd2e5-161">Konfigurowanie rejestrowania</span><span class="sxs-lookup"><span data-stu-id="cd2e5-161">Configure logging</span></span>

<span data-ttu-id="cd2e5-162">Rejestrowania skonfigurowano w kliencie programu .NET przy użyciu `ConfigureLogging` metody.</span><span class="sxs-lookup"><span data-stu-id="cd2e5-162">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="cd2e5-163">Rejestrowanie dostawcy i filtrów można zarejestrować w taki sam sposób jak znajdują się na serwerze.</span><span class="sxs-lookup"><span data-stu-id="cd2e5-163">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="cd2e5-164">Zobacz [rejestrowania w programie ASP.NET Core](xref:fundamentals/logging/index#how-to-add-providers) dokumentacji, aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="cd2e5-164">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index#how-to-add-providers) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="cd2e5-165">Aby można było zarejestrować dostawców logowania, należy zainstalować wymagane pakiety.</span><span class="sxs-lookup"><span data-stu-id="cd2e5-165">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="cd2e5-166">Zobacz [wbudowane funkcje rejestrowania dostawców](xref:fundamentals/logging/index#built-in-logging-providers) sekcji dokumentacji, aby uzyskać pełną listę.</span><span class="sxs-lookup"><span data-stu-id="cd2e5-166">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="cd2e5-167">Na przykład, aby włączyć rejestrowanie konsoli, należy zainstalować `Microsoft.Extensions.Logging.Console` pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="cd2e5-167">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="cd2e5-168">Wywołaj `AddConsole` — metoda rozszerzenia:</span><span class="sxs-lookup"><span data-stu-id="cd2e5-168">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="cd2e5-169">W kliencie JavaScript podobny `configureLogging` istnieje metoda.</span><span class="sxs-lookup"><span data-stu-id="cd2e5-169">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="cd2e5-170">Podaj `LogLevel` wartość wskazującą, minimalny poziom komunikaty dziennika do produkcji.</span><span class="sxs-lookup"><span data-stu-id="cd2e5-170">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="cd2e5-171">Dzienniki są zapisywane do okna konsoli przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="cd2e5-171">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

> [!NOTE]
> <span data-ttu-id="cd2e5-172">Aby wyłączyć rejestrowanie w całości, należy określić `signalR.LogLevel.None` w `configureLogging` metody.</span><span class="sxs-lookup"><span data-stu-id="cd2e5-172">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="cd2e5-173">Poniżej przedstawiono dostępne dla klienta JavaScript poziomy dziennika.</span><span class="sxs-lookup"><span data-stu-id="cd2e5-173">Log levels available to the JavaScript client are listed below.</span></span> <span data-ttu-id="cd2e5-174">Ustawienie poziomu dziennika na jedną z następujących wartości umożliwia rejestrowanie komunikatów na **lub nowszej** tego poziomu.</span><span class="sxs-lookup"><span data-stu-id="cd2e5-174">Setting the log level to one of these values enables logging of messages at **or above** that level.</span></span>

| <span data-ttu-id="cd2e5-175">Poziom</span><span class="sxs-lookup"><span data-stu-id="cd2e5-175">Level</span></span> | <span data-ttu-id="cd2e5-176">Opis</span><span class="sxs-lookup"><span data-stu-id="cd2e5-176">Description</span></span> |
| ----- | ----------- |
| `None` | <span data-ttu-id="cd2e5-177">Żadne komunikaty są rejestrowane.</span><span class="sxs-lookup"><span data-stu-id="cd2e5-177">No messages are logged.</span></span> |
| `Critical` | <span data-ttu-id="cd2e5-178">Komunikaty, które wskazują błędy w całej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="cd2e5-178">Messages that indicate a failure in the entire app.</span></span> |
| `Error` | <span data-ttu-id="cd2e5-179">Komunikaty, które wskazują błędy w bieżącej operacji.</span><span class="sxs-lookup"><span data-stu-id="cd2e5-179">Messages that indicate a failure in the current operation.</span></span> |
| `Warning` | <span data-ttu-id="cd2e5-180">Komunikaty, które wskazują na problem krytyczny.</span><span class="sxs-lookup"><span data-stu-id="cd2e5-180">Messages that indicate a non-fatal problem.</span></span> |
| `Information` | <span data-ttu-id="cd2e5-181">Komunikaty informacyjne.</span><span class="sxs-lookup"><span data-stu-id="cd2e5-181">Informational messages.</span></span> |
| `Debug` | <span data-ttu-id="cd2e5-182">Komunikaty diagnostyczne przydatne podczas debugowania.</span><span class="sxs-lookup"><span data-stu-id="cd2e5-182">Diagnostic messages useful for debugging.</span></span> |
| `Trace` | <span data-ttu-id="cd2e5-183">Bardzo szczegółowe komunikaty diagnostyczne przeznaczone dla diagnozowanie konkretnych problemów.</span><span class="sxs-lookup"><span data-stu-id="cd2e5-183">Very detailed diagnostic messages designed for diagnosing specific issues.</span></span> |

### <a name="configure-allowed-transports"></a><span data-ttu-id="cd2e5-184">Konfigurowanie transportów dozwolone</span><span class="sxs-lookup"><span data-stu-id="cd2e5-184">Configure allowed transports</span></span>

<span data-ttu-id="cd2e5-185">Transporty używane przez element SignalR można skonfigurować w `WithUrl` wywołania (`withUrl` w języku JavaScript).</span><span class="sxs-lookup"><span data-stu-id="cd2e5-185">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="cd2e5-186">Bitowe OR dla wartości z `HttpTransportType` może służyć do ograniczania klienta do użycia tylko określonych transportów.</span><span class="sxs-lookup"><span data-stu-id="cd2e5-186">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="cd2e5-187">Wszystkich transportów są domyślnie włączone.</span><span class="sxs-lookup"><span data-stu-id="cd2e5-187">All transports are enabled by default.</span></span>

<span data-ttu-id="cd2e5-188">Na przykład aby wyłączyć transportu Server-Sent zdarzenia, ale zezwalaj na WebSockets długie sondowania połączenia i:</span><span class="sxs-lookup"><span data-stu-id="cd2e5-188">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="cd2e5-189">W kliencie JavaScript transportów są skonfigurowane, ustawiając `transport` pola w obiekcie opcje udostępniane `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="cd2e5-189">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

### <a name="configure-bearer-authentication"></a><span data-ttu-id="cd2e5-190">Konfigurowanie uwierzytelniania elementu nośnego</span><span class="sxs-lookup"><span data-stu-id="cd2e5-190">Configure bearer authentication</span></span>

<span data-ttu-id="cd2e5-191">Aby przekazać dane uwierzytelniania wraz z SignalR żądania, użyj `AccessTokenProvider` opcji (`accessTokenFactory` w języku JavaScript), aby określić funkcję, która zwraca token żądanego dostępu.</span><span class="sxs-lookup"><span data-stu-id="cd2e5-191">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="cd2e5-192">W kliencie programu .NET ten token dostępu jest przekazywany jako "Uwierzytelniania elementu nośnego" HTTP tokenu (używanie `Authorization` nagłówka o typie `Bearer`).</span><span class="sxs-lookup"><span data-stu-id="cd2e5-192">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="cd2e5-193">W kliencie JavaScript token dostępu jest używany jako token elementu nośnego **z wyjątkiem** w niektórych przypadkach, gdy przeglądarka interfejsów API ograniczyć możliwość stosowania nagłówki (w szczególności w żądaniach Server-Sent zdarzeń i technologia WebSockets).</span><span class="sxs-lookup"><span data-stu-id="cd2e5-193">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="cd2e5-194">W takich przypadkach token dostępu jest oferowana jako wartość ciągu zapytania `access_token`.</span><span class="sxs-lookup"><span data-stu-id="cd2e5-194">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="cd2e5-195">W kliencie programu .NET `AccessTokenProvider` opcja może być określona za pomocą opcji delegata `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="cd2e5-195">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        }
    })
    .Build();
```

<span data-ttu-id="cd2e5-196">W kliencie JavaScript token dostępu jest skonfigurowana, ustawiając `accessTokenFactory` pola w obiekcie opcje w `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="cd2e5-196">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

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

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="cd2e5-197">Konfigurowanie limitu czasu i opcje keep-alive</span><span class="sxs-lookup"><span data-stu-id="cd2e5-197">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="cd2e5-198">Dodatkowe opcje dotyczące konfigurowania limitu czasu i zachowanie keep-alive są dostępne na `HubConnection` samego obiektu:</span><span class="sxs-lookup"><span data-stu-id="cd2e5-198">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>

| <span data-ttu-id="cd2e5-199">.NET — opcja</span><span class="sxs-lookup"><span data-stu-id="cd2e5-199">.NET Option</span></span> | <span data-ttu-id="cd2e5-200">Opcja języka JavaScript</span><span class="sxs-lookup"><span data-stu-id="cd2e5-200">JavaScript Option</span></span> | <span data-ttu-id="cd2e5-201">Opis</span><span class="sxs-lookup"><span data-stu-id="cd2e5-201">Description</span></span> |
| ----------- | ----------------- | ----------- |
| `ServerTimeout` | `serverTimeoutInMilliseconds` | <span data-ttu-id="cd2e5-202">Limit czasu aktywności serwera.</span><span class="sxs-lookup"><span data-stu-id="cd2e5-202">Timeout for server activity.</span></span> <span data-ttu-id="cd2e5-203">Jeśli serwer nie wysłał żadnego komunikatu w tym interwał, klient traktuje serwera odłączona i wyzwalacza `Closed` zdarzeń (`onclose` w języku JavaScript).</span><span class="sxs-lookup"><span data-stu-id="cd2e5-203">If the server hasn't sent any message in this interval, the client considers the server disconnected and trigger the `Closed` event (`onclose` in JavaScript).</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="cd2e5-204">Nie można konfigurować</span><span class="sxs-lookup"><span data-stu-id="cd2e5-204">Not configurable</span></span> | <span data-ttu-id="cd2e5-205">Limit czasu dla serwera początkowego uzgadniania.</span><span class="sxs-lookup"><span data-stu-id="cd2e5-205">Timeout for initial server handshake.</span></span> <span data-ttu-id="cd2e5-206">Jeśli serwer nie wysyłać odpowiedzi uzgadniania, w tym interwał, klient anuluje uzgadnianiu i wyzwalacza `Closed` zdarzeń (`onclose` w języku JavaScript).</span><span class="sxs-lookup"><span data-stu-id="cd2e5-206">If the server doesn't send a handshake response in this interval, the client cancels the handshake and trigger the `Closed` event (`onclose` in JavaScript).</span></span> |

<span data-ttu-id="cd2e5-207">W kliencie programu .NET wartości limitu czasu są określane jako `TimeSpan` wartości.</span><span class="sxs-lookup"><span data-stu-id="cd2e5-207">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span> <span data-ttu-id="cd2e5-208">W kliencie JavaScript wartości limitu czasu są określone jako liczby.</span><span class="sxs-lookup"><span data-stu-id="cd2e5-208">In the JavaScript client, timeout values are specified as numbers.</span></span> <span data-ttu-id="cd2e5-209">Liczby reprezentują wartości czasu w milisekundach.</span><span class="sxs-lookup"><span data-stu-id="cd2e5-209">The numbers represent time values in milliseconds.</span></span>

### <a name="configure-additional-options"></a><span data-ttu-id="cd2e5-210">Konfigurowanie opcji dodatkowych</span><span class="sxs-lookup"><span data-stu-id="cd2e5-210">Configure additional options</span></span>

<span data-ttu-id="cd2e5-211">Dodatkowe opcje można skonfigurować w `WithUrl` (`withUrl` w języku JavaScript) metody `HubConnectionBuilder`:</span><span class="sxs-lookup"><span data-stu-id="cd2e5-211">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder`:</span></span>

| <span data-ttu-id="cd2e5-212">.NET — opcja</span><span class="sxs-lookup"><span data-stu-id="cd2e5-212">.NET Option</span></span> | <span data-ttu-id="cd2e5-213">Opcja języka JavaScript</span><span class="sxs-lookup"><span data-stu-id="cd2e5-213">JavaScript Option</span></span> | <span data-ttu-id="cd2e5-214">Opis</span><span class="sxs-lookup"><span data-stu-id="cd2e5-214">Description</span></span> |
| ----------- | ----------------- | ----------- |
| `AccessTokenProvider` | `accessTokenFactory` | <span data-ttu-id="cd2e5-215">Funkcja zwraca ciąg, który jest dostarczana jako token uwierzytelniania elementu nośnego w żądaniach HTTP.</span><span class="sxs-lookup"><span data-stu-id="cd2e5-215">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `skipNegotiation` | <span data-ttu-id="cd2e5-216">Ustaw tę opcję na `true` Aby pominąć krok negocjacji.</span><span class="sxs-lookup"><span data-stu-id="cd2e5-216">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="cd2e5-217">**Obsługiwane tylko w przypadku transportu WebSockets jest tylko transportu włączone**.</span><span class="sxs-lookup"><span data-stu-id="cd2e5-217">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="cd2e5-218">Nie można włączyć to ustawienie, korzystając z usługi Azure SignalR Service.</span><span class="sxs-lookup"><span data-stu-id="cd2e5-218">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="cd2e5-219">Nie można skonfigurować \*</span><span class="sxs-lookup"><span data-stu-id="cd2e5-219">Not configurable \*</span></span> | <span data-ttu-id="cd2e5-220">Kolekcja certyfikaty protokołu TLS do wysłania do uwierzytelniania żądań.</span><span class="sxs-lookup"><span data-stu-id="cd2e5-220">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="cd2e5-221">Nie można skonfigurować \*</span><span class="sxs-lookup"><span data-stu-id="cd2e5-221">Not configurable \*</span></span> | <span data-ttu-id="cd2e5-222">Kolekcja plików cookie protokołu HTTP ma wysłać z każdym żądaniem HTTP.</span><span class="sxs-lookup"><span data-stu-id="cd2e5-222">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="cd2e5-223">Nie można skonfigurować \*</span><span class="sxs-lookup"><span data-stu-id="cd2e5-223">Not configurable \*</span></span> | <span data-ttu-id="cd2e5-224">Poświadczenia, aby wysłać z każdym żądaniem HTTP.</span><span class="sxs-lookup"><span data-stu-id="cd2e5-224">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="cd2e5-225">Nie można skonfigurować \*</span><span class="sxs-lookup"><span data-stu-id="cd2e5-225">Not configurable \*</span></span> | <span data-ttu-id="cd2e5-226">Tylko WebSockets.</span><span class="sxs-lookup"><span data-stu-id="cd2e5-226">WebSockets only.</span></span> <span data-ttu-id="cd2e5-227">Maksymalna ilość czasu klient odczekuje po zamknięciu dla serwera potwierdzić żądanie zamknięcia.</span><span class="sxs-lookup"><span data-stu-id="cd2e5-227">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="cd2e5-228">Jeśli serwer nie potwierdzenia zamknięcia w tej chwili, klient odłączy się.</span><span class="sxs-lookup"><span data-stu-id="cd2e5-228">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="cd2e5-229">Nie można skonfigurować \*</span><span class="sxs-lookup"><span data-stu-id="cd2e5-229">Not configurable \*</span></span> | <span data-ttu-id="cd2e5-230">Słownik zawierający dodatkowe nagłówki HTTP ma wysłać z każdym żądaniem HTTP.</span><span class="sxs-lookup"><span data-stu-id="cd2e5-230">A dictionary of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | <span data-ttu-id="cd2e5-231">Nie można skonfigurować \*</span><span class="sxs-lookup"><span data-stu-id="cd2e5-231">Not configurable \*</span></span> | <span data-ttu-id="cd2e5-232">Delegat, który może służyć do konfigurowania lub Zastąp `HttpMessageHandler` używane do wysyłania żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="cd2e5-232">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="cd2e5-233">Nie są używane dla połączeń protokołu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="cd2e5-233">Not used for WebSocket connections.</span></span> <span data-ttu-id="cd2e5-234">Ten delegat musi zwracać wartość inną niż null i otrzymuje wartość domyślna, jako parametr.</span><span class="sxs-lookup"><span data-stu-id="cd2e5-234">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="cd2e5-235">Zmodyfikować ustawienia na tej wartości domyślne i przywrócić go lub zwróć zupełnie nowe `HttpMessageHandler` wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="cd2e5-235">Either modify settings on that default value and return it, or return a completely new `HttpMessageHandler` instance.</span></span> |
| `Proxy` | <span data-ttu-id="cd2e5-236">Nie można skonfigurować \*</span><span class="sxs-lookup"><span data-stu-id="cd2e5-236">Not configurable \*</span></span> | <span data-ttu-id="cd2e5-237">Serwer proxy HTTP do użycia podczas wysyłania żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="cd2e5-237">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | <span data-ttu-id="cd2e5-238">Nie można skonfigurować \*</span><span class="sxs-lookup"><span data-stu-id="cd2e5-238">Not configurable \*</span></span> | <span data-ttu-id="cd2e5-239">Ustaw ten atrybut typu wartość logiczna do wysyłania poświadczeń domyślnych dla żądań HTTP i Websocket.</span><span class="sxs-lookup"><span data-stu-id="cd2e5-239">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="cd2e5-240">Umożliwia to korzystanie z uwierzytelniania Windows.</span><span class="sxs-lookup"><span data-stu-id="cd2e5-240">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | <span data-ttu-id="cd2e5-241">Nie można skonfigurować \*</span><span class="sxs-lookup"><span data-stu-id="cd2e5-241">Not configurable \*</span></span> | <span data-ttu-id="cd2e5-242">Delegat, który można skonfigurować dodatkowe opcje protokołu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="cd2e5-242">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="cd2e5-243">Otrzymuje wystąpienie elementu [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) można skonfigurować opcje.</span><span class="sxs-lookup"><span data-stu-id="cd2e5-243">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

<span data-ttu-id="cd2e5-244">Nie można skonfigurować w klienta JavaScript, ze względu na ograniczenia w przeglądarce interfejsów API są opcje oznaczone gwiazdką (\*).</span><span class="sxs-lookup"><span data-stu-id="cd2e5-244">Options marked with an asterisk (\*) aren't configurable in the JavaScript client, due to limitations in browser APIs.</span></span>

<span data-ttu-id="cd2e5-245">W kliencie programu .NET, te opcje mogą być modyfikowane przez delegata opcje udostępnionego `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="cd2e5-245">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="cd2e5-246">W kliencie JavaScript, te opcje można podać w obiekcie JavaScript udostępniane `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="cd2e5-246">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    });
    .build();
```

## <a name="additional-resources"></a><span data-ttu-id="cd2e5-247">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="cd2e5-247">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>
