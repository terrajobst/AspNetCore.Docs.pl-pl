---
title: Konfiguracja Core SignalR platformy ASP.NET
author: bradygaster
description: Informacje o sposobie konfigurowania aplikacji ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 09/06/2018
uid: signalr/configuration
ms.openlocfilehash: bb18ba242584afa7181dcc19a5295f86996aeaa3
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2019
ms.locfileid: "54837523"
---
# <a name="aspnet-core-signalr-configuration"></a><span data-ttu-id="4dac2-103">Konfiguracja Core SignalR platformy ASP.NET</span><span class="sxs-lookup"><span data-stu-id="4dac2-103">ASP.NET Core SignalR configuration</span></span>

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="4dac2-104">Opcje serializacji JSON/MessagePack</span><span class="sxs-lookup"><span data-stu-id="4dac2-104">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="4dac2-105">SignalR platformy ASP.NET Core obsługuje dwa protokoły, kodowania wiadomości: [JSON](https://www.json.org/) i [MessagePack](https://msgpack.org/index.html).</span><span class="sxs-lookup"><span data-stu-id="4dac2-105">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="4dac2-106">Dla każdego protokołu zawiera opcje konfiguracji serializacji.</span><span class="sxs-lookup"><span data-stu-id="4dac2-106">Each protocol has serialization configuration options.</span></span>

<span data-ttu-id="4dac2-107">Serializacja kodu JSON można skonfigurować na serwerze za pomocą [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) metodę rozszerzenia, które mogą być dodawane po [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) w swojej `Startup.ConfigureServices` metody.</span><span class="sxs-lookup"><span data-stu-id="4dac2-107">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method, which can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in your `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="4dac2-108">`AddJsonProtocol` Metoda przyjmuje obiekt delegowany, który odbiera `options` obiektu.</span><span class="sxs-lookup"><span data-stu-id="4dac2-108">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="4dac2-109">[PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) właściwość obiektu, na którym jest na składnik JSON.NET `JsonSerializerSettings` obiektu, który może służyć do konfigurowania serializacji argumenty i zwracać wartości.</span><span class="sxs-lookup"><span data-stu-id="4dac2-109">The [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) property on that object is a JSON.NET `JsonSerializerSettings` object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="4dac2-110">Zobacz [dokumentacji na składnik JSON.NET](https://www.newtonsoft.com/json/help/html/Introduction.htm) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="4dac2-110">See the [JSON.NET Documentation](https://www.newtonsoft.com/json/help/html/Introduction.htm) for more details.</span></span>

<span data-ttu-id="4dac2-111">Na przykład aby skonfigurować serializator do użycia nazwy właściwości "PascalCase", zamiast domyślnej nazwy "camelCase", użyj poniższego kodu:</span><span class="sxs-lookup"><span data-stu-id="4dac2-111">As an example, to configure the serializer to use "PascalCase" property names, instead of the default "camelCase" names, use the following code:</span></span>

```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver = 
        new DefaultContractResolver();
    });
```

<span data-ttu-id="4dac2-112">W kliencie programu .NET, takie same `AddJsonProtocol` — metoda rozszerzenia istnieje na [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="4dac2-112">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="4dac2-113">`Microsoft.Extensions.DependencyInjection` Przestrzeni nazw, należy zaimportować resolve — metoda rozszerzenia:</span><span class="sxs-lookup"><span data-stu-id="4dac2-113">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

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
> <span data-ttu-id="4dac2-114">Nie jest możliwe do skonfigurowania serializacji JSON w kliencie JavaScript w tej chwili.</span><span class="sxs-lookup"><span data-stu-id="4dac2-114">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="4dac2-115">Opcje serializacji MessagePack</span><span class="sxs-lookup"><span data-stu-id="4dac2-115">MessagePack serialization options</span></span>

<span data-ttu-id="4dac2-116">Serializacja MessagePack można skonfigurować poprzez dostarczenie delegata do [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) wywołania.</span><span class="sxs-lookup"><span data-stu-id="4dac2-116">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="4dac2-117">Zobacz [MessagePack w SignalR](xref:signalr/messagepackhubprotocol) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="4dac2-117">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="4dac2-118">Nie jest możliwe do skonfigurowania MessagePack serializacji w kliencie JavaScript w tej chwili.</span><span class="sxs-lookup"><span data-stu-id="4dac2-118">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="4dac2-119">Konfigurowanie opcji serwera</span><span class="sxs-lookup"><span data-stu-id="4dac2-119">Configure server options</span></span>

<span data-ttu-id="4dac2-120">W poniższej tabeli opisano opcje dotyczące konfigurowania centrów SignalR:</span><span class="sxs-lookup"><span data-stu-id="4dac2-120">The following table describes options for configuring SignalR hubs:</span></span>

| <span data-ttu-id="4dac2-121">Opcja</span><span class="sxs-lookup"><span data-stu-id="4dac2-121">Option</span></span> | <span data-ttu-id="4dac2-122">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="4dac2-122">Default Value</span></span> | <span data-ttu-id="4dac2-123">Opis</span><span class="sxs-lookup"><span data-stu-id="4dac2-123">Description</span></span> |
| ------ | ------------- | ----------- |
| `HandshakeTimeout` | <span data-ttu-id="4dac2-124">15 sekund</span><span class="sxs-lookup"><span data-stu-id="4dac2-124">15 seconds</span></span> | <span data-ttu-id="4dac2-125">Jeśli klient nie wysyła komunikat uzgadniania połączenia początkowego, w tym przedziale czasu, połączenie jest zamknięte.</span><span class="sxs-lookup"><span data-stu-id="4dac2-125">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="4dac2-126">To ustawienie Zaawansowane, które powinny być modyfikowane tylko, jeśli występują błędy przekroczenia limitu czasu uzgadnianie ze względu na opóźnienie sieci poważne.</span><span class="sxs-lookup"><span data-stu-id="4dac2-126">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="4dac2-127">Aby uzyskać więcej szczegółów na temat procesu uzgadniania, zobacz [specyfikacji protokołu Centrum SignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="4dac2-127">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="4dac2-128">15 sekund</span><span class="sxs-lookup"><span data-stu-id="4dac2-128">15 seconds</span></span> | <span data-ttu-id="4dac2-129">Jeśli serwer nie wysłał wiadomości, w tym przedziale czasu, wiadomość ping są wysyłane automatycznie do utrzymanie otwartego połączenia.</span><span class="sxs-lookup"><span data-stu-id="4dac2-129">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="4dac2-130">Po zmianie `KeepAliveInterval`, zmień `ServerTimeout` / `serverTimeoutInMilliseconds` ustawienie na komputerze klienckim.</span><span class="sxs-lookup"><span data-stu-id="4dac2-130">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="4dac2-131">Zalecanym `ServerTimeout` / `serverTimeoutInMilliseconds` wartość double `KeepAliveInterval` wartość.</span><span class="sxs-lookup"><span data-stu-id="4dac2-131">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="4dac2-132">Wszystkie zainstalowane protokołów</span><span class="sxs-lookup"><span data-stu-id="4dac2-132">All installed protocols</span></span> | <span data-ttu-id="4dac2-133">Protokoły obsługiwane przez tego koncentratora.</span><span class="sxs-lookup"><span data-stu-id="4dac2-133">Protocols supported by this hub.</span></span> <span data-ttu-id="4dac2-134">Domyślnie są dozwolone wszystkie protokoły zarejestrowany na serwerze, ale protokoły mogą być usunięte z tej listy, aby wyłączyć określone protokoły dla poszczególnych centrów.</span><span class="sxs-lookup"><span data-stu-id="4dac2-134">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="4dac2-135">Jeśli `true`, szczegółowe komunikaty o wyjątkach są zwracane do klientów, gdy wyjątek jest zgłaszany w przypadku metody koncentratora.</span><span class="sxs-lookup"><span data-stu-id="4dac2-135">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="4dac2-136">Wartość domyślna to `false`, jak te komunikaty o wyjątkach mogą zawierać poufne informacje.</span><span class="sxs-lookup"><span data-stu-id="4dac2-136">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

<span data-ttu-id="4dac2-137">Można skonfigurować opcje do wszystkich centrów, zapewniając delegat opcje do `AddSignalR` wywołania `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="4dac2-137">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

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

<span data-ttu-id="4dac2-138">Opcje dla jednego centrum zastępują opcje globalne w `AddSignalR` i może być konfigurowana przy użyciu [AddHubOptions\<T >](/dotnet/api/microsoft.extensions.dependencyinjection.huboptionsdependencyinjectionextensions.addhuboptions):</span><span class="sxs-lookup"><span data-stu-id="4dac2-138">Options for a single hub override the global options provided in `AddSignalR` and can be configured using [AddHubOptions\<T>](/dotnet/api/microsoft.extensions.dependencyinjection.huboptionsdependencyinjectionextensions.addhuboptions):</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

<span data-ttu-id="4dac2-139">Użyj `HttpConnectionDispatcherOptions` skonfigurować zaawansowane ustawienia powiązane z transportami i Zarządzanie buforem pamięci.</span><span class="sxs-lookup"><span data-stu-id="4dac2-139">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="4dac2-140">Te opcje są konfigurowane przez przekazanie delegat [MapHub\<T >](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub).</span><span class="sxs-lookup"><span data-stu-id="4dac2-140">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub).</span></span>

| <span data-ttu-id="4dac2-141">Opcja</span><span class="sxs-lookup"><span data-stu-id="4dac2-141">Option</span></span> | <span data-ttu-id="4dac2-142">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="4dac2-142">Default Value</span></span> | <span data-ttu-id="4dac2-143">Opis</span><span class="sxs-lookup"><span data-stu-id="4dac2-143">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="4dac2-144">32 KB</span><span class="sxs-lookup"><span data-stu-id="4dac2-144">32 KB</span></span> | <span data-ttu-id="4dac2-145">Maksymalna liczba bajtów odebranych od klienta, bufory serwera.</span><span class="sxs-lookup"><span data-stu-id="4dac2-145">The maximum number of bytes received from the client that the server buffers.</span></span> <span data-ttu-id="4dac2-146">Zwiększenie tej wartości umożliwia serwer do odbierania komunikatów większy, ale może mieć negatywny wpływ na zużycie pamięci.</span><span class="sxs-lookup"><span data-stu-id="4dac2-146">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="4dac2-147">Dane są automatycznie zbierane z `Authorize` atrybuty stosowane do klasy koncentratora.</span><span class="sxs-lookup"><span data-stu-id="4dac2-147">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="4dac2-148">Lista [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) obiekty używane do określenia, czy klient jest autoryzowany do podłączać do koncentratora.</span><span class="sxs-lookup"><span data-stu-id="4dac2-148">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="4dac2-149">32 KB</span><span class="sxs-lookup"><span data-stu-id="4dac2-149">32 KB</span></span> | <span data-ttu-id="4dac2-150">Maksymalna liczba bajtów wysłanych przez aplikację, bufory serwera.</span><span class="sxs-lookup"><span data-stu-id="4dac2-150">The maximum number of bytes sent by the app that the server buffers.</span></span> <span data-ttu-id="4dac2-151">Zwiększenie tej wartości umożliwia serwerowi na wysyłanie wiadomości większych, ale może mieć negatywny wpływ na zużycie pamięci.</span><span class="sxs-lookup"><span data-stu-id="4dac2-151">Increasing this value allows the server to send larger messages, but can negatively impact memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="4dac2-152">Wszystkich transportów są włączone.</span><span class="sxs-lookup"><span data-stu-id="4dac2-152">All Transports are enabled.</span></span> | <span data-ttu-id="4dac2-153">Maska bitów `HttpTransportType` wartości, które mogą ograniczyć transportów klienta można się połączyć.</span><span class="sxs-lookup"><span data-stu-id="4dac2-153">A bitmask of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="4dac2-154">Zobacz poniżej.</span><span class="sxs-lookup"><span data-stu-id="4dac2-154">See below.</span></span> | <span data-ttu-id="4dac2-155">Dodatkowe opcje specyficzne dla transportu sondowania długie.</span><span class="sxs-lookup"><span data-stu-id="4dac2-155">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="4dac2-156">Zobacz poniżej.</span><span class="sxs-lookup"><span data-stu-id="4dac2-156">See below.</span></span> | <span data-ttu-id="4dac2-157">Dodatkowe opcje specyficzne dla transportu funkcji WebSockets.</span><span class="sxs-lookup"><span data-stu-id="4dac2-157">Additional options specific to the WebSockets transport.</span></span> |

<span data-ttu-id="4dac2-158">Transport sondowania długie ma dodatkowe opcje, które można skonfigurować przy użyciu `LongPolling` właściwości:</span><span class="sxs-lookup"><span data-stu-id="4dac2-158">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="4dac2-159">Opcja</span><span class="sxs-lookup"><span data-stu-id="4dac2-159">Option</span></span> | <span data-ttu-id="4dac2-160">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="4dac2-160">Default Value</span></span> | <span data-ttu-id="4dac2-161">Opis</span><span class="sxs-lookup"><span data-stu-id="4dac2-161">Description</span></span> |
| ------ | ------------- | ----------- |
| `PollTimeout` | <span data-ttu-id="4dac2-162">90 sekund</span><span class="sxs-lookup"><span data-stu-id="4dac2-162">90 seconds</span></span> | <span data-ttu-id="4dac2-163">Maksymalna ilość czasu serwer oczekuje na komunikat do wysłania do klienta przed zakończeniem żądania sondowania pojedynczego.</span><span class="sxs-lookup"><span data-stu-id="4dac2-163">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="4dac2-164">Zmniejszenie tej wartości powoduje, że klient wystawiania nowego żądania sondowania częściej.</span><span class="sxs-lookup"><span data-stu-id="4dac2-164">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> |

<span data-ttu-id="4dac2-165">WebSocket transport ma dodatkowe opcje, które można skonfigurować przy użyciu `WebSockets` właściwości:</span><span class="sxs-lookup"><span data-stu-id="4dac2-165">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="4dac2-166">Opcja</span><span class="sxs-lookup"><span data-stu-id="4dac2-166">Option</span></span> | <span data-ttu-id="4dac2-167">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="4dac2-167">Default Value</span></span> | <span data-ttu-id="4dac2-168">Opis</span><span class="sxs-lookup"><span data-stu-id="4dac2-168">Description</span></span> |
| ------ | ------------- | ----------- |
| `CloseTimeout` | <span data-ttu-id="4dac2-169">5 sekund</span><span class="sxs-lookup"><span data-stu-id="4dac2-169">5 seconds</span></span> | <span data-ttu-id="4dac2-170">Po zamknięciu serwera, jeśli klient nie może zamknąć w tym przedziale czasu, połączenie zostanie przerwane.</span><span class="sxs-lookup"><span data-stu-id="4dac2-170">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | `null` | <span data-ttu-id="4dac2-171">Delegat, który może służyć do ustawiania `Sec-WebSocket-Protocol` nagłówka niestandardowej wartości.</span><span class="sxs-lookup"><span data-stu-id="4dac2-171">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="4dac2-172">Delegat otrzymuje wartości żądanego przez klienta jako dane wejściowe i powinien zwrócić na żądaną wartość.</span><span class="sxs-lookup"><span data-stu-id="4dac2-172">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="4dac2-173">Skonfiguruj opcje klienta</span><span class="sxs-lookup"><span data-stu-id="4dac2-173">Configure client options</span></span>

<span data-ttu-id="4dac2-174">Opcje klienta można skonfigurować na `HubConnectionBuilder` typu (dostępne w klientach .NET i języka JavaScript) oraz stanu na `HubConnection` sam.</span><span class="sxs-lookup"><span data-stu-id="4dac2-174">Client options can be configured on the `HubConnectionBuilder` type (available in both .NET and JavaScript clients), as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="4dac2-175">Konfigurowanie rejestrowania</span><span class="sxs-lookup"><span data-stu-id="4dac2-175">Configure logging</span></span>

<span data-ttu-id="4dac2-176">Rejestrowania skonfigurowano w kliencie programu .NET przy użyciu `ConfigureLogging` metody.</span><span class="sxs-lookup"><span data-stu-id="4dac2-176">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="4dac2-177">Rejestrowanie dostawcy i filtrów można zarejestrować w taki sam sposób jak znajdują się na serwerze.</span><span class="sxs-lookup"><span data-stu-id="4dac2-177">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="4dac2-178">Zobacz [rejestrowania w programie ASP.NET Core](xref:fundamentals/logging/index) dokumentacji, aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="4dac2-178">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="4dac2-179">Aby można było zarejestrować dostawców logowania, należy zainstalować wymagane pakiety.</span><span class="sxs-lookup"><span data-stu-id="4dac2-179">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="4dac2-180">Zobacz [wbudowane funkcje rejestrowania dostawców](xref:fundamentals/logging/index#built-in-logging-providers) sekcji dokumentacji, aby uzyskać pełną listę.</span><span class="sxs-lookup"><span data-stu-id="4dac2-180">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="4dac2-181">Na przykład, aby włączyć rejestrowanie konsoli, należy zainstalować `Microsoft.Extensions.Logging.Console` pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="4dac2-181">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="4dac2-182">Wywołaj `AddConsole` — metoda rozszerzenia:</span><span class="sxs-lookup"><span data-stu-id="4dac2-182">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="4dac2-183">W kliencie JavaScript podobny `configureLogging` istnieje metoda.</span><span class="sxs-lookup"><span data-stu-id="4dac2-183">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="4dac2-184">Podaj `LogLevel` wartość wskazującą, minimalny poziom komunikaty dziennika do produkcji.</span><span class="sxs-lookup"><span data-stu-id="4dac2-184">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="4dac2-185">Dzienniki są zapisywane do okna konsoli przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="4dac2-185">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

> [!NOTE]
> <span data-ttu-id="4dac2-186">Aby wyłączyć rejestrowanie w całości, należy określić `signalR.LogLevel.None` w `configureLogging` metody.</span><span class="sxs-lookup"><span data-stu-id="4dac2-186">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="4dac2-187">Poniżej przedstawiono dostępne dla klienta JavaScript poziomy dziennika.</span><span class="sxs-lookup"><span data-stu-id="4dac2-187">Log levels available to the JavaScript client are listed below.</span></span> <span data-ttu-id="4dac2-188">Ustawienie poziomu dziennika na jedną z następujących wartości umożliwia rejestrowanie komunikatów na **lub nowszej** tego poziomu.</span><span class="sxs-lookup"><span data-stu-id="4dac2-188">Setting the log level to one of these values enables logging of messages at **or above** that level.</span></span>

| <span data-ttu-id="4dac2-189">Poziom</span><span class="sxs-lookup"><span data-stu-id="4dac2-189">Level</span></span> | <span data-ttu-id="4dac2-190">Opis</span><span class="sxs-lookup"><span data-stu-id="4dac2-190">Description</span></span> |
| ----- | ----------- |
| `None` | <span data-ttu-id="4dac2-191">Żadne komunikaty są rejestrowane.</span><span class="sxs-lookup"><span data-stu-id="4dac2-191">No messages are logged.</span></span> |
| `Critical` | <span data-ttu-id="4dac2-192">Komunikaty, które wskazują błędy w całej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4dac2-192">Messages that indicate a failure in the entire app.</span></span> |
| `Error` | <span data-ttu-id="4dac2-193">Komunikaty, które wskazują błędy w bieżącej operacji.</span><span class="sxs-lookup"><span data-stu-id="4dac2-193">Messages that indicate a failure in the current operation.</span></span> |
| `Warning` | <span data-ttu-id="4dac2-194">Komunikaty, które wskazują na problem krytyczny.</span><span class="sxs-lookup"><span data-stu-id="4dac2-194">Messages that indicate a non-fatal problem.</span></span> |
| `Information` | <span data-ttu-id="4dac2-195">Komunikaty informacyjne.</span><span class="sxs-lookup"><span data-stu-id="4dac2-195">Informational messages.</span></span> |
| `Debug` | <span data-ttu-id="4dac2-196">Komunikaty diagnostyczne przydatne podczas debugowania.</span><span class="sxs-lookup"><span data-stu-id="4dac2-196">Diagnostic messages useful for debugging.</span></span> |
| `Trace` | <span data-ttu-id="4dac2-197">Bardzo szczegółowe komunikaty diagnostyczne przeznaczone dla diagnozowanie konkretnych problemów.</span><span class="sxs-lookup"><span data-stu-id="4dac2-197">Very detailed diagnostic messages designed for diagnosing specific issues.</span></span> |

### <a name="configure-allowed-transports"></a><span data-ttu-id="4dac2-198">Konfigurowanie transportów dozwolone</span><span class="sxs-lookup"><span data-stu-id="4dac2-198">Configure allowed transports</span></span>

<span data-ttu-id="4dac2-199">Transporty używane przez element SignalR można skonfigurować w `WithUrl` wywołania (`withUrl` w języku JavaScript).</span><span class="sxs-lookup"><span data-stu-id="4dac2-199">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="4dac2-200">Bitowe OR dla wartości z `HttpTransportType` może służyć do ograniczania klienta do użycia tylko określonych transportów.</span><span class="sxs-lookup"><span data-stu-id="4dac2-200">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="4dac2-201">Wszystkich transportów są domyślnie włączone.</span><span class="sxs-lookup"><span data-stu-id="4dac2-201">All transports are enabled by default.</span></span>

<span data-ttu-id="4dac2-202">Na przykład aby wyłączyć transportu Server-Sent zdarzenia, ale zezwalaj na WebSockets długie sondowania połączenia i:</span><span class="sxs-lookup"><span data-stu-id="4dac2-202">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="4dac2-203">W kliencie JavaScript transportów są skonfigurowane, ustawiając `transport` pola w obiekcie opcje udostępniane `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="4dac2-203">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

### <a name="configure-bearer-authentication"></a><span data-ttu-id="4dac2-204">Konfigurowanie uwierzytelniania elementu nośnego</span><span class="sxs-lookup"><span data-stu-id="4dac2-204">Configure bearer authentication</span></span>

<span data-ttu-id="4dac2-205">Aby przekazać dane uwierzytelniania wraz z SignalR żądania, użyj `AccessTokenProvider` opcji (`accessTokenFactory` w języku JavaScript), aby określić funkcję, która zwraca token żądanego dostępu.</span><span class="sxs-lookup"><span data-stu-id="4dac2-205">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="4dac2-206">W kliencie programu .NET ten token dostępu jest przekazywany jako "Uwierzytelniania elementu nośnego" HTTP tokenu (używanie `Authorization` nagłówka o typie `Bearer`).</span><span class="sxs-lookup"><span data-stu-id="4dac2-206">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="4dac2-207">W kliencie JavaScript token dostępu jest używany jako token elementu nośnego **z wyjątkiem** w niektórych przypadkach, gdy przeglądarka interfejsów API ograniczyć możliwość stosowania nagłówki (w szczególności w żądaniach Server-Sent zdarzeń i technologia WebSockets).</span><span class="sxs-lookup"><span data-stu-id="4dac2-207">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="4dac2-208">W takich przypadkach token dostępu jest oferowana jako wartość ciągu zapytania `access_token`.</span><span class="sxs-lookup"><span data-stu-id="4dac2-208">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="4dac2-209">W kliencie programu .NET `AccessTokenProvider` opcja może być określona za pomocą opcji delegata `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="4dac2-209">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

<span data-ttu-id="4dac2-210">W kliencie JavaScript token dostępu jest skonfigurowana, ustawiając `accessTokenFactory` pola w obiekcie opcje w `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="4dac2-210">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

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

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="4dac2-211">Konfigurowanie limitu czasu i opcje keep-alive</span><span class="sxs-lookup"><span data-stu-id="4dac2-211">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="4dac2-212">Dodatkowe opcje dotyczące konfigurowania limitu czasu i zachowanie keep-alive są dostępne na `HubConnection` samego obiektu:</span><span class="sxs-lookup"><span data-stu-id="4dac2-212">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>

| <span data-ttu-id="4dac2-213">.NET — opcja</span><span class="sxs-lookup"><span data-stu-id="4dac2-213">.NET Option</span></span> | <span data-ttu-id="4dac2-214">JavaScript Option</span><span class="sxs-lookup"><span data-stu-id="4dac2-214">JavaScript Option</span></span> | <span data-ttu-id="4dac2-215">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="4dac2-215">Default Value</span></span> | <span data-ttu-id="4dac2-216">Opis</span><span class="sxs-lookup"><span data-stu-id="4dac2-216">Description</span></span> |
| ----------- | ----------------- | ------------- | ----------- |
| `ServerTimeout` | `serverTimeoutInMilliseconds` | <span data-ttu-id="4dac2-217">30 sekund (ponad 30 000 MS)</span><span class="sxs-lookup"><span data-stu-id="4dac2-217">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="4dac2-218">Limit czasu aktywności serwera.</span><span class="sxs-lookup"><span data-stu-id="4dac2-218">Timeout for server activity.</span></span> <span data-ttu-id="4dac2-219">Jeśli serwer nie wysłał wiadomości, w tym interwał, klient traktuje serwera odłączona i wyzwalacze `Closed` zdarzeń (`onclose` w języku JavaScript).</span><span class="sxs-lookup"><span data-stu-id="4dac2-219">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="4dac2-220">Ta wartość musi być wystarczająco duży dla komunikat ping do wysłania z serwera **i** odebranych przez klienta w ciągu interwału limitu czasu.</span><span class="sxs-lookup"><span data-stu-id="4dac2-220">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="4dac2-221">Zalecana wartość to co najmniej dwukrotnie serwera numeru `KeepAliveInterval` wartość, aby dać czas na polecenia ping do odbierania.</span><span class="sxs-lookup"><span data-stu-id="4dac2-221">The recommended value is a number at least double the server's `KeepAliveInterval` value, to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="4dac2-222">Nie można konfigurować</span><span class="sxs-lookup"><span data-stu-id="4dac2-222">Not configurable</span></span> | <span data-ttu-id="4dac2-223">15 sekund</span><span class="sxs-lookup"><span data-stu-id="4dac2-223">15 seconds</span></span> | <span data-ttu-id="4dac2-224">Limit czasu dla serwera początkowego uzgadniania.</span><span class="sxs-lookup"><span data-stu-id="4dac2-224">Timeout for initial server handshake.</span></span> <span data-ttu-id="4dac2-225">Jeśli serwer nie wysyłać odpowiedzi uzgadniania, w tym interwał, klient anuluje uzgadnianiu i wyzwalacze `Closed` zdarzeń (`onclose` w języku JavaScript).</span><span class="sxs-lookup"><span data-stu-id="4dac2-225">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="4dac2-226">To ustawienie Zaawansowane, które powinny być modyfikowane tylko, jeśli występują błędy przekroczenia limitu czasu uzgadnianie ze względu na opóźnienie sieci poważne.</span><span class="sxs-lookup"><span data-stu-id="4dac2-226">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="4dac2-227">Aby uzyskać więcej szczegółów na temat procesu uzgadniania, zobacz [specyfikacji protokołu Centrum SignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="4dac2-227">For more detail on the Handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

<span data-ttu-id="4dac2-228">W kliencie programu .NET wartości limitu czasu są określane jako `TimeSpan` wartości.</span><span class="sxs-lookup"><span data-stu-id="4dac2-228">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span> <span data-ttu-id="4dac2-229">W kliencie JavaScript wartości limitu czasu są określane jako liczba wskazująca czas trwania (w milisekundach).</span><span class="sxs-lookup"><span data-stu-id="4dac2-229">In the JavaScript client, timeout values are specified as a number indicating the duration in milliseconds.</span></span>

### <a name="configure-additional-options"></a><span data-ttu-id="4dac2-230">Konfigurowanie opcji dodatkowych</span><span class="sxs-lookup"><span data-stu-id="4dac2-230">Configure additional options</span></span>

<span data-ttu-id="4dac2-231">Dodatkowe opcje można skonfigurować w `WithUrl` (`withUrl` w języku JavaScript) metody `HubConnectionBuilder`:</span><span class="sxs-lookup"><span data-stu-id="4dac2-231">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder`:</span></span>

| <span data-ttu-id="4dac2-232">.NET — opcja</span><span class="sxs-lookup"><span data-stu-id="4dac2-232">.NET Option</span></span> | <span data-ttu-id="4dac2-233">JavaScript Option</span><span class="sxs-lookup"><span data-stu-id="4dac2-233">JavaScript Option</span></span> | <span data-ttu-id="4dac2-234">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="4dac2-234">Default Value</span></span> | <span data-ttu-id="4dac2-235">Opis</span><span class="sxs-lookup"><span data-stu-id="4dac2-235">Description</span></span> |
| ----------- | ----------------- | ------------- | ----------- |
| `AccessTokenProvider` | `accessTokenFactory` | `null` | <span data-ttu-id="4dac2-236">Funkcja zwraca ciąg, który jest dostarczana jako token uwierzytelniania elementu nośnego w żądaniach HTTP.</span><span class="sxs-lookup"><span data-stu-id="4dac2-236">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `skipNegotiation` | `false` | <span data-ttu-id="4dac2-237">Ustaw tę opcję na `true` Aby pominąć krok negocjacji.</span><span class="sxs-lookup"><span data-stu-id="4dac2-237">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="4dac2-238">**Obsługiwane tylko w przypadku transportu WebSockets jest tylko transportu włączone**.</span><span class="sxs-lookup"><span data-stu-id="4dac2-238">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="4dac2-239">Nie można włączyć to ustawienie, korzystając z usługi Azure SignalR Service.</span><span class="sxs-lookup"><span data-stu-id="4dac2-239">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="4dac2-240">Nie można skonfigurować \*</span><span class="sxs-lookup"><span data-stu-id="4dac2-240">Not configurable \*</span></span> | <span data-ttu-id="4dac2-241">Pusty</span><span class="sxs-lookup"><span data-stu-id="4dac2-241">Empty</span></span> | <span data-ttu-id="4dac2-242">Kolekcja certyfikaty protokołu TLS do wysłania do uwierzytelniania żądań.</span><span class="sxs-lookup"><span data-stu-id="4dac2-242">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="4dac2-243">Nie można skonfigurować \*</span><span class="sxs-lookup"><span data-stu-id="4dac2-243">Not configurable \*</span></span> | <span data-ttu-id="4dac2-244">Pusty</span><span class="sxs-lookup"><span data-stu-id="4dac2-244">Empty</span></span> | <span data-ttu-id="4dac2-245">Kolekcja plików cookie protokołu HTTP ma wysłać z każdym żądaniem HTTP.</span><span class="sxs-lookup"><span data-stu-id="4dac2-245">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="4dac2-246">Nie można skonfigurować \*</span><span class="sxs-lookup"><span data-stu-id="4dac2-246">Not configurable \*</span></span> | <span data-ttu-id="4dac2-247">Pusty</span><span class="sxs-lookup"><span data-stu-id="4dac2-247">Empty</span></span> | <span data-ttu-id="4dac2-248">Poświadczenia, aby wysłać z każdym żądaniem HTTP.</span><span class="sxs-lookup"><span data-stu-id="4dac2-248">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="4dac2-249">Nie można skonfigurować \*</span><span class="sxs-lookup"><span data-stu-id="4dac2-249">Not configurable \*</span></span> | <span data-ttu-id="4dac2-250">5 sekund</span><span class="sxs-lookup"><span data-stu-id="4dac2-250">5 seconds</span></span> | <span data-ttu-id="4dac2-251">Tylko WebSockets.</span><span class="sxs-lookup"><span data-stu-id="4dac2-251">WebSockets only.</span></span> <span data-ttu-id="4dac2-252">Maksymalna ilość czasu klient odczekuje po zamknięciu dla serwera potwierdzić żądanie zamknięcia.</span><span class="sxs-lookup"><span data-stu-id="4dac2-252">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="4dac2-253">Jeśli serwer nie potwierdzenia zamknięcia w tej chwili, klient odłączy się.</span><span class="sxs-lookup"><span data-stu-id="4dac2-253">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="4dac2-254">Nie można skonfigurować \*</span><span class="sxs-lookup"><span data-stu-id="4dac2-254">Not configurable \*</span></span> | <span data-ttu-id="4dac2-255">Pusty</span><span class="sxs-lookup"><span data-stu-id="4dac2-255">Empty</span></span> | <span data-ttu-id="4dac2-256">Słownik zawierający dodatkowe nagłówki HTTP ma wysłać z każdym żądaniem HTTP.</span><span class="sxs-lookup"><span data-stu-id="4dac2-256">A dictionary of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | <span data-ttu-id="4dac2-257">Nie można skonfigurować \*</span><span class="sxs-lookup"><span data-stu-id="4dac2-257">Not configurable \*</span></span> | `null` | <span data-ttu-id="4dac2-258">Delegat, który może służyć do konfigurowania lub Zastąp `HttpMessageHandler` używane do wysyłania żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="4dac2-258">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="4dac2-259">Nie są używane dla połączeń protokołu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="4dac2-259">Not used for WebSocket connections.</span></span> <span data-ttu-id="4dac2-260">Ten delegat musi zwracać wartość inną niż null i otrzymuje wartość domyślna, jako parametr.</span><span class="sxs-lookup"><span data-stu-id="4dac2-260">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="4dac2-261">Albo zmodyfikować ustawienia na tej wartości domyślne i przywrócić go albo zwraca nową `HttpMessageHandler` wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="4dac2-261">Either modify settings on that default value and return it, or return a new `HttpMessageHandler` instance.</span></span> <span data-ttu-id="4dac2-262">**Podczas zastępowania programu obsługi upewnij się, że Skopiuj ustawienia które mają być przechowywane z podanego programu obsługi, w przeciwnym razie skonfigurowanych opcji (takich jak pliki cookie i nagłówki) nie zostaną zastosowane do nowego programu obsługi.**</span><span class="sxs-lookup"><span data-stu-id="4dac2-262">**When replacing the handler make sure to copy the settings you want to keep from the provided handler, otherwise, the configured options (such as Cookies and Headers) won't apply to the new handler.**</span></span> |
| `Proxy` | <span data-ttu-id="4dac2-263">Nie można skonfigurować \*</span><span class="sxs-lookup"><span data-stu-id="4dac2-263">Not configurable \*</span></span> | `null` | <span data-ttu-id="4dac2-264">Serwer proxy HTTP do użycia podczas wysyłania żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="4dac2-264">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | <span data-ttu-id="4dac2-265">Nie można skonfigurować \*</span><span class="sxs-lookup"><span data-stu-id="4dac2-265">Not configurable \*</span></span> | `false` | <span data-ttu-id="4dac2-266">Ustaw ten atrybut typu wartość logiczna do wysyłania poświadczeń domyślnych dla żądań HTTP i Websocket.</span><span class="sxs-lookup"><span data-stu-id="4dac2-266">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="4dac2-267">Umożliwia to korzystanie z uwierzytelniania Windows.</span><span class="sxs-lookup"><span data-stu-id="4dac2-267">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | <span data-ttu-id="4dac2-268">Nie można skonfigurować \*</span><span class="sxs-lookup"><span data-stu-id="4dac2-268">Not configurable \*</span></span> | `null` | <span data-ttu-id="4dac2-269">Delegat, który można skonfigurować dodatkowe opcje protokołu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="4dac2-269">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="4dac2-270">Otrzymuje wystąpienie elementu [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) można skonfigurować opcje.</span><span class="sxs-lookup"><span data-stu-id="4dac2-270">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

<span data-ttu-id="4dac2-271">Nie można skonfigurować w klienta JavaScript, ze względu na ograniczenia w przeglądarce interfejsów API są opcje oznaczone gwiazdką (\*).</span><span class="sxs-lookup"><span data-stu-id="4dac2-271">Options marked with an asterisk (\*) aren't configurable in the JavaScript client, due to limitations in browser APIs.</span></span>

<span data-ttu-id="4dac2-272">W kliencie programu .NET, te opcje mogą być modyfikowane przez delegata opcje udostępnionego `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="4dac2-272">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="4dac2-273">W kliencie JavaScript, te opcje można podać w obiekcie JavaScript udostępniane `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="4dac2-273">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

## <a name="additional-resources"></a><span data-ttu-id="4dac2-274">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="4dac2-274">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>
