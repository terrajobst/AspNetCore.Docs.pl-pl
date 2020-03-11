---
title: Konfiguracja SignalR ASP.NET Core
author: bradygaster
description: Dowiedz się, jak skonfigurować aplikacje SignalR ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 12/10/2019
no-loc:
- SignalR
uid: signalr/configuration
ms.openlocfilehash: c225ff88110dc17185a430ac1c422d2433306115
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78658635"
---
# <a name="aspnet-core-signalr-configuration"></a><span data-ttu-id="53e69-103">Konfiguracja sygnałów ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="53e69-103">ASP.NET Core SignalR configuration</span></span>

::: moniker range=">= aspnetcore-3.0"

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="53e69-104">Opcje serializacji JSON/MessagePack</span><span class="sxs-lookup"><span data-stu-id="53e69-104">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="53e69-105">ASP.NET Core sygnalizujący obsługuje dwa protokoły dla komunikatów kodowania: [JSON](https://www.json.org/) i [MessagePack](https://msgpack.org/index.html).</span><span class="sxs-lookup"><span data-stu-id="53e69-105">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="53e69-106">Każdy protokół ma opcje konfiguracji serializacji.</span><span class="sxs-lookup"><span data-stu-id="53e69-106">Each protocol has serialization configuration options.</span></span>

<span data-ttu-id="53e69-107">Serializacji JSON można skonfigurować na serwerze przy użyciu metody rozszerzenia [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) .</span><span class="sxs-lookup"><span data-stu-id="53e69-107">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method.</span></span> <span data-ttu-id="53e69-108">`AddJsonProtocol` można dodać po elemencie [Addsignaler](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="53e69-108">`AddJsonProtocol` can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="53e69-109">Metoda `AddJsonProtocol` przyjmuje delegata, który odbiera obiekt `options`.</span><span class="sxs-lookup"><span data-stu-id="53e69-109">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="53e69-110">Właściwość [PayloadSerializerOptions](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializeroptions) tego obiektu jest obiektem <xref:System.Text.Json.JsonSerializerOptions> `System.Text.Json`, którego można użyć do skonfigurowania serializacji argumentów i zwracanych wartości.</span><span class="sxs-lookup"><span data-stu-id="53e69-110">The [PayloadSerializerOptions](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializeroptions) property on that object is a `System.Text.Json` <xref:System.Text.Json.JsonSerializerOptions> object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="53e69-111">Aby uzyskać więcej informacji, zobacz [dokumentację system. Text. JSON](/dotnet/api/system.text.json).</span><span class="sxs-lookup"><span data-stu-id="53e69-111">For more information, see the [System.Text.Json documentation](/dotnet/api/system.text.json).</span></span>

<span data-ttu-id="53e69-112">Przykładowo, aby skonfigurować Serializator, aby nie zmieniać wielkości liter nazw właściwości zamiast domyślnych nazw "camelCase", należy użyć następującego kodu w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="53e69-112">As an example, to configure the serializer to not change the casing of property names, instead of the default "camelCase" names, use the following code in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerOptions.PropertyNamingPolicy = null
    });
```

<span data-ttu-id="53e69-113">W kliencie .NET ta sama Metoda rozszerzenia `AddJsonProtocol` istnieje w [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="53e69-113">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="53e69-114">Aby rozwiązać metodę rozszerzenia, należy zaimportować przestrzeń nazw `Microsoft.Extensions.DependencyInjection`:</span><span class="sxs-lookup"><span data-stu-id="53e69-114">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

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
> <span data-ttu-id="53e69-115">W tej chwili nie można skonfigurować serializacji JSON w kliencie JavaScript.</span><span class="sxs-lookup"><span data-stu-id="53e69-115">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="switch-to-newtonsoftjson"></a><span data-ttu-id="53e69-116">Przejdź do pliku Newtonsoft. JSON</span><span class="sxs-lookup"><span data-stu-id="53e69-116">Switch to Newtonsoft.Json</span></span>

<span data-ttu-id="53e69-117">Jeśli potrzebujesz funkcji `Newtonsoft.Json`, które nie są obsługiwane w `System.Text.Json`, zobacz [Switch to Newtonsoft. JSON](xref:migration/22-to-30#switch-to-newtonsoftjson).</span><span class="sxs-lookup"><span data-stu-id="53e69-117">If you need features of `Newtonsoft.Json` that aren't supported in `System.Text.Json`, See [Switch to Newtonsoft.Json](xref:migration/22-to-30#switch-to-newtonsoftjson).</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="53e69-118">Opcje serializacji MessagePack</span><span class="sxs-lookup"><span data-stu-id="53e69-118">MessagePack serialization options</span></span>

<span data-ttu-id="53e69-119">Serializacji MessagePack można skonfigurować, dostarczając delegata do wywołania [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) .</span><span class="sxs-lookup"><span data-stu-id="53e69-119">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="53e69-120">Aby uzyskać więcej informacji, zobacz [MessagePack w sygnalizacji](xref:signalr/messagepackhubprotocol) .</span><span class="sxs-lookup"><span data-stu-id="53e69-120">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="53e69-121">W tej chwili nie można skonfigurować serializacji MessagePack w kliencie JavaScript.</span><span class="sxs-lookup"><span data-stu-id="53e69-121">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="53e69-122">Konfigurowanie opcji serwera</span><span class="sxs-lookup"><span data-stu-id="53e69-122">Configure server options</span></span>

<span data-ttu-id="53e69-123">W poniższej tabeli opisano opcje konfigurowania centrów sygnałów:</span><span class="sxs-lookup"><span data-stu-id="53e69-123">The following table describes options for configuring SignalR hubs:</span></span>

| <span data-ttu-id="53e69-124">Opcja</span><span class="sxs-lookup"><span data-stu-id="53e69-124">Option</span></span> | <span data-ttu-id="53e69-125">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="53e69-125">Default Value</span></span> | <span data-ttu-id="53e69-126">Opis</span><span class="sxs-lookup"><span data-stu-id="53e69-126">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="53e69-127">30 sekund</span><span class="sxs-lookup"><span data-stu-id="53e69-127">30 seconds</span></span> | <span data-ttu-id="53e69-128">Serwer Rozważ odłączenie klienta, jeśli nie odebrano komunikatu (w tym w tym interwale).</span><span class="sxs-lookup"><span data-stu-id="53e69-128">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="53e69-129">Może upłynąć dłużej niż ten interwał limitu czasu, aby klient mógł zostać oznaczony jako odłączony, ze względu na to, jak jest to zaimplementowane.</span><span class="sxs-lookup"><span data-stu-id="53e69-129">It could take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="53e69-130">Zalecaną wartością jest podwójna wartość `KeepAliveInterval`.</span><span class="sxs-lookup"><span data-stu-id="53e69-130">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="53e69-131">15 sekund</span><span class="sxs-lookup"><span data-stu-id="53e69-131">15 seconds</span></span> | <span data-ttu-id="53e69-132">Jeśli klient nie wyśle początkowego komunikatu uzgadniania w tym przedziale czasu, połączenie zostanie zamknięte.</span><span class="sxs-lookup"><span data-stu-id="53e69-132">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="53e69-133">Jest to ustawienie zaawansowane, które należy zmodyfikować tylko w przypadku wystąpienia błędów limitu czasu uzgadniania z powodu poważnego opóźnienia sieci.</span><span class="sxs-lookup"><span data-stu-id="53e69-133">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="53e69-134">Aby uzyskać więcej szczegółów na temat procesu uzgadniania, zobacz [specyfikację protokołu centrum sygnału](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="53e69-134">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="53e69-135">15 sekund</span><span class="sxs-lookup"><span data-stu-id="53e69-135">15 seconds</span></span> | <span data-ttu-id="53e69-136">Jeśli serwer nie wysłał komunikatu w tym interwale, zostanie automatycznie wysłana wiadomość ping w celu pozostawienia otwartego połączenia.</span><span class="sxs-lookup"><span data-stu-id="53e69-136">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="53e69-137">Podczas zmiany `KeepAliveInterval`Zmień ustawienie `ServerTimeout`/`serverTimeoutInMilliseconds` na kliencie.</span><span class="sxs-lookup"><span data-stu-id="53e69-137">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="53e69-138">Zalecana wartość `ServerTimeout`/`serverTimeoutInMilliseconds` jest podwójna `KeepAliveInterval` wartość.</span><span class="sxs-lookup"><span data-stu-id="53e69-138">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="53e69-139">Wszystkie zainstalowane protokoły</span><span class="sxs-lookup"><span data-stu-id="53e69-139">All installed protocols</span></span> | <span data-ttu-id="53e69-140">Protokoły obsługiwane przez to centrum.</span><span class="sxs-lookup"><span data-stu-id="53e69-140">Protocols supported by this hub.</span></span> <span data-ttu-id="53e69-141">Domyślnie wszystkie protokoły zarejestrowane na serwerze są dozwolone, ale protokoły można usunąć z tej listy, aby wyłączyć określone protokoły dla poszczególnych centrów.</span><span class="sxs-lookup"><span data-stu-id="53e69-141">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="53e69-142">Jeśli `true`, szczegółowe komunikaty o wyjątkach są zwracane do klientów, gdy wyjątek jest zgłaszany w metodzie centrum.</span><span class="sxs-lookup"><span data-stu-id="53e69-142">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="53e69-143">Wartość domyślna to `false`, ponieważ te komunikaty o wyjątkach mogą zawierać informacje poufne.</span><span class="sxs-lookup"><span data-stu-id="53e69-143">The default is `false`, as these exception messages can contain sensitive information.</span></span> |
| `StreamBufferCapacity` | `10` | <span data-ttu-id="53e69-144">Maksymalna liczba elementów, które mogą być buforowane dla strumieni przekazywania klientów.</span><span class="sxs-lookup"><span data-stu-id="53e69-144">The maximum number of items that can be buffered for client upload streams.</span></span> <span data-ttu-id="53e69-145">Po osiągnięciu tego limitu przetwarzanie wywołań jest blokowane, dopóki serwer nie przetwarza elementów strumienia.</span><span class="sxs-lookup"><span data-stu-id="53e69-145">If this limit is reached, the processing of invocations is blocked until the server processes stream items.</span></span>|
| `MaximumReceiveMessageSize` | <span data-ttu-id="53e69-146">32 KB</span><span class="sxs-lookup"><span data-stu-id="53e69-146">32 KB</span></span> | <span data-ttu-id="53e69-147">Maksymalny rozmiar pojedynczego przychodzącego komunikatu centrum.</span><span class="sxs-lookup"><span data-stu-id="53e69-147">Maximum size of a single incoming hub message.</span></span> |

<span data-ttu-id="53e69-148">Opcje można skonfigurować dla wszystkich centrów, dostarczając opcje delegata do wywołania `AddSignalR` w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="53e69-148">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

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

<span data-ttu-id="53e69-149">Opcje jednego centrum przesłaniają opcje globalne dostępne w `AddSignalR` i można je skonfigurować za pomocą <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span><span class="sxs-lookup"><span data-stu-id="53e69-149">Options for a single hub override the global options provided in `AddSignalR` and can be configured using <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

### <a name="advanced-http-configuration-options"></a><span data-ttu-id="53e69-150">Zaawansowane opcje konfiguracji HTTP</span><span class="sxs-lookup"><span data-stu-id="53e69-150">Advanced HTTP configuration options</span></span>

<span data-ttu-id="53e69-151">Użyj `HttpConnectionDispatcherOptions`, aby skonfigurować zaawansowane ustawienia związane z transportami i zarządzaniem buforem pamięci.</span><span class="sxs-lookup"><span data-stu-id="53e69-151">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="53e69-152">Te opcje są konfigurowane przez przekazanie delegata do [MapHub\<t >](/dotnet/api/microsoft.aspnetcore.builder.hubendpointroutebuilderextensions.maphub) w `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="53e69-152">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.builder.hubendpointroutebuilderextensions.maphub) in `Startup.Configure`.</span></span>

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

<span data-ttu-id="53e69-153">W poniższej tabeli opisano opcje konfigurowania zaawansowanych opcji protokołu HTTP w programie ASP.NET Core Signal:</span><span class="sxs-lookup"><span data-stu-id="53e69-153">The following table describes options for configuring ASP.NET Core SignalR's advanced HTTP options:</span></span>

| <span data-ttu-id="53e69-154">Opcja</span><span class="sxs-lookup"><span data-stu-id="53e69-154">Option</span></span> | <span data-ttu-id="53e69-155">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="53e69-155">Default Value</span></span> | <span data-ttu-id="53e69-156">Opis</span><span class="sxs-lookup"><span data-stu-id="53e69-156">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="53e69-157">32 KB</span><span class="sxs-lookup"><span data-stu-id="53e69-157">32 KB</span></span> | <span data-ttu-id="53e69-158">Maksymalna liczba bajtów odebranych od klienta, które buforują serwer przed zastosowaniem nadużycia.</span><span class="sxs-lookup"><span data-stu-id="53e69-158">The maximum number of bytes received from the client that the server buffers before applying backpressure.</span></span> <span data-ttu-id="53e69-159">Zwiększenie tej wartości umożliwia serwerowi otrzymywanie większych komunikatów szybciej bez naciskania, ale może zwiększyć zużycie pamięci.</span><span class="sxs-lookup"><span data-stu-id="53e69-159">Increasing this value allows the server to receive larger messages more quickly without applying backpressure, but can increase memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="53e69-160">Dane zbierane automatycznie z `Authorize` atrybuty zastosowane do klasy centrum.</span><span class="sxs-lookup"><span data-stu-id="53e69-160">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="53e69-161">Lista obiektów [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) służąca do określenia, czy klient jest autoryzowany do łączenia się z centrum.</span><span class="sxs-lookup"><span data-stu-id="53e69-161">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="53e69-162">32 KB</span><span class="sxs-lookup"><span data-stu-id="53e69-162">32 KB</span></span> | <span data-ttu-id="53e69-163">Maksymalna liczba bajtów wysłanych przez aplikację, które buforuje serwer przed zaobserwowaniem presji.</span><span class="sxs-lookup"><span data-stu-id="53e69-163">The maximum number of bytes sent by the app that the server buffers before observing backpressure.</span></span> <span data-ttu-id="53e69-164">Zwiększenie tej wartości umożliwia serwerowi przebuforowanie większych komunikatów szybciej bez czekania na obciążenie, ale może zwiększyć zużycie pamięci.</span><span class="sxs-lookup"><span data-stu-id="53e69-164">Increasing this value allows the server to buffer larger messages more quickly without awaiting backpressure, but can increase memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="53e69-165">Wszystkie transporty są włączone.</span><span class="sxs-lookup"><span data-stu-id="53e69-165">All Transports are enabled.</span></span> | <span data-ttu-id="53e69-166">Wyliczenie flag bitowych wartości `HttpTransportType`, które mogą ograniczyć transporty, z których może korzystać klient do nawiązywania połączeń.</span><span class="sxs-lookup"><span data-stu-id="53e69-166">A bit flags enum of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="53e69-167">Zobacz poniżej.</span><span class="sxs-lookup"><span data-stu-id="53e69-167">See below.</span></span> | <span data-ttu-id="53e69-168">Dodatkowe opcje specyficzne dla długiego transportu sondowania.</span><span class="sxs-lookup"><span data-stu-id="53e69-168">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="53e69-169">Zobacz poniżej.</span><span class="sxs-lookup"><span data-stu-id="53e69-169">See below.</span></span> | <span data-ttu-id="53e69-170">Dodatkowe opcje specyficzne dla transportu obiektów WebSockets.</span><span class="sxs-lookup"><span data-stu-id="53e69-170">Additional options specific to the WebSockets transport.</span></span> |

<span data-ttu-id="53e69-171">W przypadku długiego transportu sondowania dostępne są dodatkowe opcje, które można skonfigurować przy użyciu właściwości `LongPolling`:</span><span class="sxs-lookup"><span data-stu-id="53e69-171">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="53e69-172">Opcja</span><span class="sxs-lookup"><span data-stu-id="53e69-172">Option</span></span> | <span data-ttu-id="53e69-173">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="53e69-173">Default Value</span></span> | <span data-ttu-id="53e69-174">Opis</span><span class="sxs-lookup"><span data-stu-id="53e69-174">Description</span></span> |
| ------ | ------------- | ----------- |
| `PollTimeout` | <span data-ttu-id="53e69-175">90 sekund</span><span class="sxs-lookup"><span data-stu-id="53e69-175">90 seconds</span></span> | <span data-ttu-id="53e69-176">Maksymalny czas, przez jaki serwer czeka na wysłanie wiadomości do klienta przed zakończeniem jednego żądania sondowania.</span><span class="sxs-lookup"><span data-stu-id="53e69-176">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="53e69-177">Zmniejszenie tej wartości powoduje, że klient wystawia nowe żądania sondy częściej.</span><span class="sxs-lookup"><span data-stu-id="53e69-177">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> |

<span data-ttu-id="53e69-178">Transport WebSocket ma dodatkowe opcje, które można skonfigurować za pomocą właściwości `WebSockets`:</span><span class="sxs-lookup"><span data-stu-id="53e69-178">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="53e69-179">Opcja</span><span class="sxs-lookup"><span data-stu-id="53e69-179">Option</span></span> | <span data-ttu-id="53e69-180">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="53e69-180">Default Value</span></span> | <span data-ttu-id="53e69-181">Opis</span><span class="sxs-lookup"><span data-stu-id="53e69-181">Description</span></span> |
| ------ | ------------- | ----------- |
| `CloseTimeout` | <span data-ttu-id="53e69-182">5 sekund</span><span class="sxs-lookup"><span data-stu-id="53e69-182">5 seconds</span></span> | <span data-ttu-id="53e69-183">Gdy serwer zostanie zamknięty, jeśli klient nie zostanie zamknięty w tym okresie, połączenie zostanie przerwane.</span><span class="sxs-lookup"><span data-stu-id="53e69-183">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | `null` | <span data-ttu-id="53e69-184">Delegat, którego można użyć do ustawienia nagłówka `Sec-WebSocket-Protocol` na wartość niestandardową.</span><span class="sxs-lookup"><span data-stu-id="53e69-184">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="53e69-185">Delegat otrzymuje wartości żądane przez klienta jako dane wejściowe i oczekuje na zwrócenie żądanej wartości.</span><span class="sxs-lookup"><span data-stu-id="53e69-185">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="53e69-186">Konfigurowanie opcji klienta</span><span class="sxs-lookup"><span data-stu-id="53e69-186">Configure client options</span></span>

<span data-ttu-id="53e69-187">Opcje klienta można skonfigurować dla typu `HubConnectionBuilder` (dostępnego w klientach .NET i JavaScript).</span><span class="sxs-lookup"><span data-stu-id="53e69-187">Client options can be configured on the `HubConnectionBuilder` type (available in the .NET and JavaScript clients).</span></span> <span data-ttu-id="53e69-188">Jest ona również dostępna w kliencie Java, ale podklasa `HttpHubConnectionBuilder` zawiera opcje konfiguracji konstruktora, a także `HubConnection` samego siebie.</span><span class="sxs-lookup"><span data-stu-id="53e69-188">It's also available in the Java client, but the `HttpHubConnectionBuilder` subclass is what contains the builder configuration options, as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="53e69-189">Konfigurowanie rejestrowania</span><span class="sxs-lookup"><span data-stu-id="53e69-189">Configure logging</span></span>

<span data-ttu-id="53e69-190">Rejestrowanie jest konfigurowane w kliencie .NET przy użyciu metody `ConfigureLogging`.</span><span class="sxs-lookup"><span data-stu-id="53e69-190">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="53e69-191">Dostawcy i filtry rejestrowania mogą być rejestrowane w taki sam sposób, w jaki znajdują się na serwerze.</span><span class="sxs-lookup"><span data-stu-id="53e69-191">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="53e69-192">Aby uzyskać więcej informacji, zobacz dokumentację dotyczącą [rejestrowania w ASP.NET Core](xref:fundamentals/logging/index) .</span><span class="sxs-lookup"><span data-stu-id="53e69-192">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="53e69-193">Aby zarejestrować dostawców rejestrowania, należy zainstalować wymagane pakiety.</span><span class="sxs-lookup"><span data-stu-id="53e69-193">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="53e69-194">Aby zapoznać się z pełną listą, zobacz sekcję [wbudowane dostawcy rejestrowania](xref:fundamentals/logging/index#built-in-logging-providers) w witrynie docs.</span><span class="sxs-lookup"><span data-stu-id="53e69-194">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="53e69-195">Aby na przykład włączyć rejestrowanie konsoli, należy zainstalować pakiet NuGet `Microsoft.Extensions.Logging.Console`.</span><span class="sxs-lookup"><span data-stu-id="53e69-195">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="53e69-196">Wywołaj metodę rozszerzenia `AddConsole`:</span><span class="sxs-lookup"><span data-stu-id="53e69-196">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="53e69-197">W kliencie JavaScript istnieje podobna Metoda `configureLogging`.</span><span class="sxs-lookup"><span data-stu-id="53e69-197">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="53e69-198">Podaj wartość `LogLevel` wskazującą minimalny poziom komunikatów dziennika do wygenerowania.</span><span class="sxs-lookup"><span data-stu-id="53e69-198">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="53e69-199">Dzienniki są zapisywane w oknie konsoli przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="53e69-199">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

<span data-ttu-id="53e69-200">Zamiast wartości `LogLevel` można również podać wartość `string` reprezentującą nazwę poziomu dziennika.</span><span class="sxs-lookup"><span data-stu-id="53e69-200">Instead of a `LogLevel` value, you can also provide a `string` value representing a log level name.</span></span> <span data-ttu-id="53e69-201">Jest to przydatne podczas konfigurowania rejestrowania sygnałów w środowiskach, w których nie masz dostępu do stałych `LogLevel`.</span><span class="sxs-lookup"><span data-stu-id="53e69-201">This is useful when configuring SignalR logging in environments where you don't have access to the `LogLevel` constants.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging("warn")
    .build();
```

<span data-ttu-id="53e69-202">Poniższa tabela zawiera listę dostępnych poziomów dzienników.</span><span class="sxs-lookup"><span data-stu-id="53e69-202">The following table lists the available log levels.</span></span> <span data-ttu-id="53e69-203">Wybrana wartość `configureLogging` ustawia **minimalny** poziom dziennika, który zostanie zarejestrowany.</span><span class="sxs-lookup"><span data-stu-id="53e69-203">The value you provide to `configureLogging` sets the **minimum** log level that will be logged.</span></span> <span data-ttu-id="53e69-204">Komunikaty zarejestrowane na tym poziomie **lub poziomy wymienione po nim w tabeli**zostaną zarejestrowane.</span><span class="sxs-lookup"><span data-stu-id="53e69-204">Messages logged at this level, **or the levels listed after it in the table**, will be logged.</span></span>

| <span data-ttu-id="53e69-205">Ciąg</span><span class="sxs-lookup"><span data-stu-id="53e69-205">String</span></span>                      | <span data-ttu-id="53e69-206">LogLevel</span><span class="sxs-lookup"><span data-stu-id="53e69-206">LogLevel</span></span>               |
| --------------------------- | ---------------------- |
| `trace`                     | `LogLevel.Trace`       |
| `debug`                     | `LogLevel.Debug`       |
| <span data-ttu-id="53e69-207">`info` **lub** `information`</span><span class="sxs-lookup"><span data-stu-id="53e69-207">`info` **or** `information`</span></span> | `LogLevel.Information` |
| <span data-ttu-id="53e69-208">`warn` **lub** `warning`</span><span class="sxs-lookup"><span data-stu-id="53e69-208">`warn` **or** `warning`</span></span>     | `LogLevel.Warning`     |
| `error`                     | `LogLevel.Error`       |
| `critical`                  | `LogLevel.Critical`    |
| `none`                      | `LogLevel.None`        |

> [!NOTE]
> <span data-ttu-id="53e69-209">Aby całkowicie wyłączyć rejestrowanie, określ `signalR.LogLevel.None` w `configureLogging` metodzie.</span><span class="sxs-lookup"><span data-stu-id="53e69-209">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="53e69-210">Aby uzyskać więcej informacji na temat rejestrowania, zobacz [dokumentację diagnostyki sygnału](xref:signalr/diagnostics).</span><span class="sxs-lookup"><span data-stu-id="53e69-210">For more information on logging, see the [SignalR Diagnostics documentation](xref:signalr/diagnostics).</span></span>

<span data-ttu-id="53e69-211">Klient Java sygnalizujący używa biblioteki [SLF4J](https://www.slf4j.org/) do rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="53e69-211">The SignalR Java client uses the [SLF4J](https://www.slf4j.org/) library for logging.</span></span> <span data-ttu-id="53e69-212">Jest to interfejs API rejestrowania wysokiego poziomu, który umożliwia użytkownikom biblioteki wybranie własnych implementacji rejestrowania przez nałożenie określonej zależności rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="53e69-212">It's a high-level logging API that allows users of the library to chose their own specific logging implementation by bringing in a specific logging dependency.</span></span> <span data-ttu-id="53e69-213">Poniższy fragment kodu przedstawia sposób użycia `java.util.logging` z klientem języka Java sygnalizującego.</span><span class="sxs-lookup"><span data-stu-id="53e69-213">The following code snippet shows how to use `java.util.logging` with the SignalR Java client.</span></span>

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

<span data-ttu-id="53e69-214">Jeśli nie skonfigurujesz rejestrowania w zależnościach, SLF4J ładuje domyślny Rejestrator braku operacji z następującym komunikatem ostrzegawczym:</span><span class="sxs-lookup"><span data-stu-id="53e69-214">If you don't configure logging in your dependencies, SLF4J loads a default no-operation logger with the following warning message:</span></span>

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

<span data-ttu-id="53e69-215">Można je bezpiecznie zignorować.</span><span class="sxs-lookup"><span data-stu-id="53e69-215">This can safely be ignored.</span></span>

### <a name="configure-allowed-transports"></a><span data-ttu-id="53e69-216">Skonfiguruj dozwolone transporty</span><span class="sxs-lookup"><span data-stu-id="53e69-216">Configure allowed transports</span></span>

<span data-ttu-id="53e69-217">Transporty używane przez sygnalizującego można skonfigurować w wywołaniu `WithUrl` (`withUrl` w języku JavaScript).</span><span class="sxs-lookup"><span data-stu-id="53e69-217">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="53e69-218">Wartości `HttpTransportType` mogą być używane w celu ograniczenia klienta do używania tylko określonych transportów.</span><span class="sxs-lookup"><span data-stu-id="53e69-218">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="53e69-219">Wszystkie transporty są domyślnie włączone.</span><span class="sxs-lookup"><span data-stu-id="53e69-219">All transports are enabled by default.</span></span>

<span data-ttu-id="53e69-220">Na przykład, aby wyłączyć transport zdarzeń wysłanych przez serwer, ale zezwolić na połączenia z usługą WebSockets i długie sondowanie:</span><span class="sxs-lookup"><span data-stu-id="53e69-220">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="53e69-221">W kliencie JavaScript transporty są konfigurowane przez ustawienie pola `transport` w obiekcie Options udostępnianym `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="53e69-221">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

<span data-ttu-id="53e69-222">W tej wersji elementu WebSockets klienta Java jest jedynym dostępnym transportem.</span><span class="sxs-lookup"><span data-stu-id="53e69-222">In this version of the Java client websockets is the only available transport.</span></span>

<span data-ttu-id="53e69-223">W kliencie Java, transport jest wybierany z metodą `withTransport`ową `HttpHubConnectionBuilder`.</span><span class="sxs-lookup"><span data-stu-id="53e69-223">In the Java client, the transport is selected with the `withTransport` method on the `HttpHubConnectionBuilder`.</span></span> <span data-ttu-id="53e69-224">Klient Java domyślnie używa transportu usługi WebSockets.</span><span class="sxs-lookup"><span data-stu-id="53e69-224">The Java client defaults to using the WebSockets transport.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withTransport(TransportEnum.WEBSOCKETS)
    .build();
```

> [!NOTE]
> <span data-ttu-id="53e69-225">Klient Java sygnalizujący nie obsługuje jeszcze rezerwy transportowej.</span><span class="sxs-lookup"><span data-stu-id="53e69-225">The SignalR Java client doesn't support transport fallback yet.</span></span>

### <a name="configure-bearer-authentication"></a><span data-ttu-id="53e69-226">Konfigurowanie uwierzytelniania okaziciela</span><span class="sxs-lookup"><span data-stu-id="53e69-226">Configure bearer authentication</span></span>

<span data-ttu-id="53e69-227">Aby zapewnić dane uwierzytelniania wraz z żądaniami sygnałów, użyj opcji `AccessTokenProvider` (`accessTokenFactory` w języku JavaScript), aby określić funkcję, która zwraca żądany token dostępu.</span><span class="sxs-lookup"><span data-stu-id="53e69-227">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="53e69-228">W kliencie .NET token dostępu jest przenoszona jako token "uwierzytelnianie okaziciela" protokołu HTTP (przy użyciu nagłówka `Authorization` z typem `Bearer`).</span><span class="sxs-lookup"><span data-stu-id="53e69-228">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="53e69-229">W kliencie JavaScript token dostępu jest używany jako token okaziciela, **z wyjątkiem** kilku przypadków, w których interfejsy API przeglądarki ograniczają możliwość zastosowania nagłówków (w konkretnym przypadku zdarzeń wysyłanych przez serwer i żądań WebSockets).</span><span class="sxs-lookup"><span data-stu-id="53e69-229">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="53e69-230">W takich przypadkach token dostępu jest dostarczany jako wartość ciągu zapytania `access_token`.</span><span class="sxs-lookup"><span data-stu-id="53e69-230">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="53e69-231">W kliencie .NET opcja `AccessTokenProvider` można określić za pomocą delegata opcji w `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="53e69-231">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

<span data-ttu-id="53e69-232">W kliencie JavaScript token dostępu jest konfigurowany przez ustawienie pola `accessTokenFactory` w obiekcie Options w `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="53e69-232">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

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

<span data-ttu-id="53e69-233">W kliencie Java programu sygnalizującego można skonfigurować token okaziciela do użycia na potrzeby uwierzytelniania, dostarczając fabrykę tokenów dostępu do [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span><span class="sxs-lookup"><span data-stu-id="53e69-233">In the SignalR Java client, you can configure a bearer token to use for authentication by providing an access token factory to the [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span></span> <span data-ttu-id="53e69-234">Użyj [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) , aby podać [](https://github.com/ReactiveX/RxJava) [>\<pojedynczego ciągu](https://reactivex.io/documentation/single.html).</span><span class="sxs-lookup"><span data-stu-id="53e69-234">Use [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) to provide an [RxJava](https://github.com/ReactiveX/RxJava) [Single\<String>](https://reactivex.io/documentation/single.html).</span></span> <span data-ttu-id="53e69-235">Wywołanie metody [Single. Ustąp](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-)umożliwia zapisanie logiki w celu utworzenia tokenów dostępu dla klienta.</span><span class="sxs-lookup"><span data-stu-id="53e69-235">With a call to [Single.defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), you can write logic to produce access tokens for your client.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="53e69-236">Konfigurowanie opcji limitu czasu i utrzymywania aktywności</span><span class="sxs-lookup"><span data-stu-id="53e69-236">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="53e69-237">W celu skonfigurowania zachowania przekroczenia limitu czasu i utrzymywania aktywności są dostępne dodatkowe opcje dla samego obiektu `HubConnection`:</span><span class="sxs-lookup"><span data-stu-id="53e69-237">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>

# <a name="net"></a>[<span data-ttu-id="53e69-238">.NET</span><span class="sxs-lookup"><span data-stu-id="53e69-238">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="53e69-239">Opcja</span><span class="sxs-lookup"><span data-stu-id="53e69-239">Option</span></span> | <span data-ttu-id="53e69-240">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="53e69-240">Default value</span></span> | <span data-ttu-id="53e69-241">Opis</span><span class="sxs-lookup"><span data-stu-id="53e69-241">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="53e69-242">30 sekund (30 000 MS)</span><span class="sxs-lookup"><span data-stu-id="53e69-242">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="53e69-243">Limit czasu dla działania serwera.</span><span class="sxs-lookup"><span data-stu-id="53e69-243">Timeout for server activity.</span></span> <span data-ttu-id="53e69-244">Jeśli serwer nie wysłał komunikatu w tym interwale, Klient uważa, że serwer odłączył się i wyzwala zdarzenie `Closed` (`onclose` w języku JavaScript).</span><span class="sxs-lookup"><span data-stu-id="53e69-244">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="53e69-245">Ta wartość musi być wystarczająca do wysłania wiadomości ping z serwera **i** odebrana przez klienta w ramach przedziału czasu.</span><span class="sxs-lookup"><span data-stu-id="53e69-245">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="53e69-246">Zalecana wartość to liczba z co najmniej podwójną wartością `KeepAliveInterval` serwera, aby umożliwić czas na nadejście poleceń ping.</span><span class="sxs-lookup"><span data-stu-id="53e69-246">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="53e69-247">15 sekund</span><span class="sxs-lookup"><span data-stu-id="53e69-247">15 seconds</span></span> | <span data-ttu-id="53e69-248">Limit czasu początkowej uzgadniania z serwerem.</span><span class="sxs-lookup"><span data-stu-id="53e69-248">Timeout for initial server handshake.</span></span> <span data-ttu-id="53e69-249">Jeśli serwer nie wyśle odpowiedzi uzgadniania w tym interwale, klient anuluje uzgadnianie i wyzwala zdarzenie `Closed` (`onclose` w języku JavaScript).</span><span class="sxs-lookup"><span data-stu-id="53e69-249">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="53e69-250">Jest to ustawienie zaawansowane, które należy zmodyfikować tylko w przypadku wystąpienia błędów limitu czasu uzgadniania z powodu poważnego opóźnienia sieci.</span><span class="sxs-lookup"><span data-stu-id="53e69-250">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="53e69-251">Aby uzyskać więcej szczegółów na temat procesu uzgadniania, zobacz [specyfikację protokołu centrum sygnału](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="53e69-251">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="53e69-252">15 sekund</span><span class="sxs-lookup"><span data-stu-id="53e69-252">15 seconds</span></span> | <span data-ttu-id="53e69-253">Określa interwał wysyłania komunikatów ping przez klienta.</span><span class="sxs-lookup"><span data-stu-id="53e69-253">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="53e69-254">Wysłanie wszelkich komunikatów z klienta powoduje zresetowanie czasomierza do początku interwału.</span><span class="sxs-lookup"><span data-stu-id="53e69-254">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="53e69-255">Jeśli klient nie wyśle komunikatu w `ClientTimeoutInterval` zestawie na serwerze, serwer traktuje klienta jako odłączony.</span><span class="sxs-lookup"><span data-stu-id="53e69-255">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

<span data-ttu-id="53e69-256">W kliencie .NET wartości limitu czasu są określane jako wartości `TimeSpan`.</span><span class="sxs-lookup"><span data-stu-id="53e69-256">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascript"></a>[<span data-ttu-id="53e69-257">JavaScript</span><span class="sxs-lookup"><span data-stu-id="53e69-257">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="53e69-258">Opcja</span><span class="sxs-lookup"><span data-stu-id="53e69-258">Option</span></span> | <span data-ttu-id="53e69-259">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="53e69-259">Default value</span></span> | <span data-ttu-id="53e69-260">Opis</span><span class="sxs-lookup"><span data-stu-id="53e69-260">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="53e69-261">30 sekund (30 000 MS)</span><span class="sxs-lookup"><span data-stu-id="53e69-261">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="53e69-262">Limit czasu dla działania serwera.</span><span class="sxs-lookup"><span data-stu-id="53e69-262">Timeout for server activity.</span></span> <span data-ttu-id="53e69-263">Jeśli serwer nie wysłał komunikatu w tym interwale, Klient uważa, że serwer odłączył się i wyzwala zdarzenie `onclose`.</span><span class="sxs-lookup"><span data-stu-id="53e69-263">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="53e69-264">Ta wartość musi być wystarczająca do wysłania wiadomości ping z serwera **i** odebrana przez klienta w ramach przedziału czasu.</span><span class="sxs-lookup"><span data-stu-id="53e69-264">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="53e69-265">Zalecana wartość to liczba z co najmniej podwójną wartością `KeepAliveInterval` serwera, aby umożliwić czas na nadejście poleceń ping.</span><span class="sxs-lookup"><span data-stu-id="53e69-265">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `keepAliveIntervalInMilliseconds` | <span data-ttu-id="53e69-266">15 sekund (15 000 MS)</span><span class="sxs-lookup"><span data-stu-id="53e69-266">15 seconds (15,000 milliseconds)</span></span> | <span data-ttu-id="53e69-267">Określa interwał wysyłania komunikatów ping przez klienta.</span><span class="sxs-lookup"><span data-stu-id="53e69-267">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="53e69-268">Wysłanie wszelkich komunikatów z klienta powoduje zresetowanie czasomierza do początku interwału.</span><span class="sxs-lookup"><span data-stu-id="53e69-268">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="53e69-269">Jeśli klient nie wyśle komunikatu w `ClientTimeoutInterval` zestawie na serwerze, serwer traktuje klienta jako odłączony.</span><span class="sxs-lookup"><span data-stu-id="53e69-269">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

# <a name="java"></a>[<span data-ttu-id="53e69-270">Java</span><span class="sxs-lookup"><span data-stu-id="53e69-270">Java</span></span>](#tab/java)

| <span data-ttu-id="53e69-271">Opcja</span><span class="sxs-lookup"><span data-stu-id="53e69-271">Option</span></span> | <span data-ttu-id="53e69-272">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="53e69-272">Default value</span></span> | <span data-ttu-id="53e69-273">Opis</span><span class="sxs-lookup"><span data-stu-id="53e69-273">Description</span></span> |
| ------ | ------------- | ----------- |
| <span data-ttu-id="53e69-274">`getServerTimeout` / `setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="53e69-274">`getServerTimeout` / `setServerTimeout`</span></span> | <span data-ttu-id="53e69-275">30 sekund (30 000 MS)</span><span class="sxs-lookup"><span data-stu-id="53e69-275">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="53e69-276">Limit czasu dla działania serwera.</span><span class="sxs-lookup"><span data-stu-id="53e69-276">Timeout for server activity.</span></span> <span data-ttu-id="53e69-277">Jeśli serwer nie wysłał komunikatu w tym interwale, Klient uważa, że serwer odłączył się i wyzwala zdarzenie `onClose`.</span><span class="sxs-lookup"><span data-stu-id="53e69-277">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="53e69-278">Ta wartość musi być wystarczająca do wysłania wiadomości ping z serwera **i** odebrana przez klienta w ramach przedziału czasu.</span><span class="sxs-lookup"><span data-stu-id="53e69-278">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="53e69-279">Zalecana wartość to liczba z co najmniej podwójną wartością `KeepAliveInterval` serwera, aby umożliwić czas na nadejście poleceń ping.</span><span class="sxs-lookup"><span data-stu-id="53e69-279">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="53e69-280">15 sekund</span><span class="sxs-lookup"><span data-stu-id="53e69-280">15 seconds</span></span> | <span data-ttu-id="53e69-281">Limit czasu początkowej uzgadniania z serwerem.</span><span class="sxs-lookup"><span data-stu-id="53e69-281">Timeout for initial server handshake.</span></span> <span data-ttu-id="53e69-282">Jeśli serwer nie wyśle odpowiedzi uzgadniania w tym interwale, klient anuluje uzgadnianie i wyzwala zdarzenie `onClose`.</span><span class="sxs-lookup"><span data-stu-id="53e69-282">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="53e69-283">Jest to ustawienie zaawansowane, które należy zmodyfikować tylko w przypadku wystąpienia błędów limitu czasu uzgadniania z powodu poważnego opóźnienia sieci.</span><span class="sxs-lookup"><span data-stu-id="53e69-283">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="53e69-284">Aby uzyskać więcej szczegółów na temat procesu uzgadniania, zobacz [specyfikację protokołu centrum sygnału](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="53e69-284">For more detail on the Handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| <span data-ttu-id="53e69-285">`getKeepAliveInterval` / `setKeepAliveInterval`</span><span class="sxs-lookup"><span data-stu-id="53e69-285">`getKeepAliveInterval` / `setKeepAliveInterval`</span></span> | <span data-ttu-id="53e69-286">15 sekund (15 000 MS)</span><span class="sxs-lookup"><span data-stu-id="53e69-286">15 seconds (15,000 milliseconds)</span></span> | <span data-ttu-id="53e69-287">Określa interwał wysyłania komunikatów ping przez klienta.</span><span class="sxs-lookup"><span data-stu-id="53e69-287">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="53e69-288">Wysłanie wszelkich komunikatów z klienta powoduje zresetowanie czasomierza do początku interwału.</span><span class="sxs-lookup"><span data-stu-id="53e69-288">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="53e69-289">Jeśli klient nie wyśle komunikatu w `ClientTimeoutInterval` zestawie na serwerze, serwer traktuje klienta jako odłączony.</span><span class="sxs-lookup"><span data-stu-id="53e69-289">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

---

### <a name="configure-additional-options"></a><span data-ttu-id="53e69-290">Skonfiguruj dodatkowe opcje</span><span class="sxs-lookup"><span data-stu-id="53e69-290">Configure additional options</span></span>

<span data-ttu-id="53e69-291">Dodatkowe opcje można skonfigurować w metodzie `WithUrl` (`withUrl` w języku JavaScript) na `HubConnectionBuilder` lub w różnych interfejsach API konfiguracji na `HttpHubConnectionBuilder` klienta Java:</span><span class="sxs-lookup"><span data-stu-id="53e69-291">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder` or on the various configuration APIs on the `HttpHubConnectionBuilder` in the Java client:</span></span>

# <a name="net"></a>[<span data-ttu-id="53e69-292">.NET</span><span class="sxs-lookup"><span data-stu-id="53e69-292">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="53e69-293">Opcja .NET</span><span class="sxs-lookup"><span data-stu-id="53e69-293">.NET Option</span></span> |  <span data-ttu-id="53e69-294">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="53e69-294">Default value</span></span> | <span data-ttu-id="53e69-295">Opis</span><span class="sxs-lookup"><span data-stu-id="53e69-295">Description</span></span> |
| ----------- | -------------- | ----------- |
| `AccessTokenProvider` | `null` | <span data-ttu-id="53e69-296">Funkcja zwracająca ciąg, który jest dostarczany jako token uwierzytelniania okaziciela w żądaniach HTTP.</span><span class="sxs-lookup"><span data-stu-id="53e69-296">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `false` | <span data-ttu-id="53e69-297">Ustaw tę wartość na `true`, aby pominąć etap negocjacji.</span><span class="sxs-lookup"><span data-stu-id="53e69-297">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="53e69-298">**Obsługiwane tylko wtedy, gdy transport Sockets jest jedynym włączonym transportem**.</span><span class="sxs-lookup"><span data-stu-id="53e69-298">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="53e69-299">Nie można włączyć tego ustawienia w przypadku korzystania z usługi Azure Signal Service.</span><span class="sxs-lookup"><span data-stu-id="53e69-299">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="53e69-300">Pusty</span><span class="sxs-lookup"><span data-stu-id="53e69-300">Empty</span></span> | <span data-ttu-id="53e69-301">Kolekcja certyfikatów TLS do wysłania w celu uwierzytelniania żądań.</span><span class="sxs-lookup"><span data-stu-id="53e69-301">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="53e69-302">Pusty</span><span class="sxs-lookup"><span data-stu-id="53e69-302">Empty</span></span> | <span data-ttu-id="53e69-303">Kolekcja plików cookie protokołu HTTP do wysłania w każdym żądaniu HTTP.</span><span class="sxs-lookup"><span data-stu-id="53e69-303">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="53e69-304">Pusty</span><span class="sxs-lookup"><span data-stu-id="53e69-304">Empty</span></span> | <span data-ttu-id="53e69-305">Poświadczenia do wysyłania przy każdym żądaniu HTTP.</span><span class="sxs-lookup"><span data-stu-id="53e69-305">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="53e69-306">5 sekund</span><span class="sxs-lookup"><span data-stu-id="53e69-306">5 seconds</span></span> | <span data-ttu-id="53e69-307">Tylko obiekty WebSockets.</span><span class="sxs-lookup"><span data-stu-id="53e69-307">WebSockets only.</span></span> <span data-ttu-id="53e69-308">Maksymalny czas oczekiwania klienta po zamknięciu serwera w celu potwierdzenia żądania zamknięcia.</span><span class="sxs-lookup"><span data-stu-id="53e69-308">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="53e69-309">Jeśli serwer nie potwierdzi zamknięcia w tym czasie, klient zostanie rozłączony.</span><span class="sxs-lookup"><span data-stu-id="53e69-309">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="53e69-310">Pusty</span><span class="sxs-lookup"><span data-stu-id="53e69-310">Empty</span></span> | <span data-ttu-id="53e69-311">Mapa dodatkowych nagłówków HTTP do wysłania w każdym żądaniu HTTP.</span><span class="sxs-lookup"><span data-stu-id="53e69-311">A Map of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | `null` | <span data-ttu-id="53e69-312">Delegat, który może służyć do konfigurowania lub zastępowania `HttpMessageHandler` używany do wysyłania żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="53e69-312">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="53e69-313">Nieużywane na potrzeby połączeń protokołu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="53e69-313">Not used for WebSocket connections.</span></span> <span data-ttu-id="53e69-314">Ten delegat musi zwracać wartość różną od null i otrzymuje wartość domyślną jako parametr.</span><span class="sxs-lookup"><span data-stu-id="53e69-314">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="53e69-315">Zmodyfikuj ustawienia tej wartości domyślnej i zwróć ją lub Zwróć nowe wystąpienie `HttpMessageHandler`.</span><span class="sxs-lookup"><span data-stu-id="53e69-315">Either modify settings on that default value and return it, or return a new `HttpMessageHandler` instance.</span></span> <span data-ttu-id="53e69-316">**Podczas zastępowania programu obsługi należy skopiować ustawienia, które mają być zachowane z podanej procedury obsługi. w przeciwnym razie skonfigurowane opcje (takie jak pliki cookie i nagłówki) nie będą miały zastosowania do nowego programu obsługi.**</span><span class="sxs-lookup"><span data-stu-id="53e69-316">**When replacing the handler make sure to copy the settings you want to keep from the provided handler, otherwise, the configured options (such as Cookies and Headers) won't apply to the new handler.**</span></span> |
| `Proxy` | `null` | <span data-ttu-id="53e69-317">Serwer proxy HTTP, który ma być używany podczas wysyłania żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="53e69-317">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | `false` | <span data-ttu-id="53e69-318">Ustaw tę wartość logiczną, aby wysyłać domyślne poświadczenia dla żądań HTTP i WebSockets.</span><span class="sxs-lookup"><span data-stu-id="53e69-318">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="53e69-319">Umożliwia to korzystanie z uwierzytelniania systemu Windows.</span><span class="sxs-lookup"><span data-stu-id="53e69-319">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | `null` | <span data-ttu-id="53e69-320">Delegat, który może służyć do konfigurowania dodatkowych opcji protokołu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="53e69-320">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="53e69-321">Odbiera wystąpienie elementu [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) , którego można użyć do skonfigurowania opcji.</span><span class="sxs-lookup"><span data-stu-id="53e69-321">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

# <a name="javascript"></a>[<span data-ttu-id="53e69-322">JavaScript</span><span class="sxs-lookup"><span data-stu-id="53e69-322">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="53e69-323">Opcja języka JavaScript</span><span class="sxs-lookup"><span data-stu-id="53e69-323">JavaScript Option</span></span> | <span data-ttu-id="53e69-324">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="53e69-324">Default Value</span></span> | <span data-ttu-id="53e69-325">Opis</span><span class="sxs-lookup"><span data-stu-id="53e69-325">Description</span></span> |
| ----------------- | ------------- | ----------- |
| `accessTokenFactory` | `null` | <span data-ttu-id="53e69-326">Funkcja zwracająca ciąg, który jest dostarczany jako token uwierzytelniania okaziciela w żądaniach HTTP.</span><span class="sxs-lookup"><span data-stu-id="53e69-326">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `skipNegotiation` | `false` | <span data-ttu-id="53e69-327">Ustaw tę wartość na `true`, aby pominąć etap negocjacji.</span><span class="sxs-lookup"><span data-stu-id="53e69-327">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="53e69-328">**Obsługiwane tylko wtedy, gdy transport Sockets jest jedynym włączonym transportem**.</span><span class="sxs-lookup"><span data-stu-id="53e69-328">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="53e69-329">Nie można włączyć tego ustawienia w przypadku korzystania z usługi Azure Signal Service.</span><span class="sxs-lookup"><span data-stu-id="53e69-329">This setting can't be enabled when using the Azure SignalR Service.</span></span> |

# <a name="java"></a>[<span data-ttu-id="53e69-330">Java</span><span class="sxs-lookup"><span data-stu-id="53e69-330">Java</span></span>](#tab/java)

| <span data-ttu-id="53e69-331">Opcja Java</span><span class="sxs-lookup"><span data-stu-id="53e69-331">Java Option</span></span> | <span data-ttu-id="53e69-332">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="53e69-332">Default Value</span></span> | <span data-ttu-id="53e69-333">Opis</span><span class="sxs-lookup"><span data-stu-id="53e69-333">Description</span></span> |
| ----------- | ------------- | ----------- |
| `withAccessTokenProvider` | `null` | <span data-ttu-id="53e69-334">Funkcja zwracająca ciąg, który jest dostarczany jako token uwierzytelniania okaziciela w żądaniach HTTP.</span><span class="sxs-lookup"><span data-stu-id="53e69-334">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `shouldSkipNegotiate` | `false` | <span data-ttu-id="53e69-335">Ustaw tę wartość na `true`, aby pominąć etap negocjacji.</span><span class="sxs-lookup"><span data-stu-id="53e69-335">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="53e69-336">**Obsługiwane tylko wtedy, gdy transport Sockets jest jedynym włączonym transportem**.</span><span class="sxs-lookup"><span data-stu-id="53e69-336">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="53e69-337">Nie można włączyć tego ustawienia w przypadku korzystania z usługi Azure Signal Service.</span><span class="sxs-lookup"><span data-stu-id="53e69-337">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| <span data-ttu-id="53e69-338">`withHeader` `withHeaders`</span><span class="sxs-lookup"><span data-stu-id="53e69-338">`withHeader` `withHeaders`</span></span> | <span data-ttu-id="53e69-339">Pusty</span><span class="sxs-lookup"><span data-stu-id="53e69-339">Empty</span></span> | <span data-ttu-id="53e69-340">Mapa dodatkowych nagłówków HTTP do wysłania w każdym żądaniu HTTP.</span><span class="sxs-lookup"><span data-stu-id="53e69-340">A Map of additional HTTP headers to send with every HTTP request.</span></span> |

---

<span data-ttu-id="53e69-341">W kliencie .NET opcje te można modyfikować za pomocą delegata opcji udostępnianego do `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="53e69-341">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="53e69-342">W kliencie JavaScript te opcje można podać w obiekcie JavaScript udostępnionym do `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="53e69-342">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

<span data-ttu-id="53e69-343">W kliencie Java Opcje te można skonfigurować przy użyciu metod na `HttpHubConnectionBuilder` zwracanych z `HubConnectionBuilder.create("HUB URL")`</span><span class="sxs-lookup"><span data-stu-id="53e69-343">In the Java client, these options can be configured with the methods on the `HttpHubConnectionBuilder` returned from the `HubConnectionBuilder.create("HUB URL")`</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
        .withHeader("Foo", "Bar")
        .shouldSkipNegotiate(true)
        .withHandshakeResponseTimeout(30*1000)
        .build();
```

## <a name="additional-resources"></a><span data-ttu-id="53e69-344">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="53e69-344">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>

::: moniker-end
::: moniker range="= aspnetcore-2.2"

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="53e69-345">Opcje serializacji JSON/MessagePack</span><span class="sxs-lookup"><span data-stu-id="53e69-345">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="53e69-346">ASP.NET Core sygnalizujący obsługuje dwa protokoły dla komunikatów kodowania: [JSON](https://www.json.org/) i [MessagePack](https://msgpack.org/index.html).</span><span class="sxs-lookup"><span data-stu-id="53e69-346">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="53e69-347">Każdy protokół ma opcje konfiguracji serializacji.</span><span class="sxs-lookup"><span data-stu-id="53e69-347">Each protocol has serialization configuration options.</span></span>

<span data-ttu-id="53e69-348">Serializacji JSON można skonfigurować na serwerze przy użyciu metody rozszerzenia [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) , która może zostać dodana po metodzie [addsygnalizująca](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="53e69-348">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method, which can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in your `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="53e69-349">Metoda `AddJsonProtocol` przyjmuje delegata, który odbiera obiekt `options`.</span><span class="sxs-lookup"><span data-stu-id="53e69-349">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="53e69-350">Właściwość [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) tego obiektu jest obiektem JSON.NET `JsonSerializerSettings`, którego można użyć do skonfigurowania serializacji argumentów i zwracanych wartości.</span><span class="sxs-lookup"><span data-stu-id="53e69-350">The [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) property on that object is a JSON.NET `JsonSerializerSettings` object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="53e69-351">Aby uzyskać więcej informacji, zapoznaj się z [dokumentacją JSON.NET](https://www.newtonsoft.com/json/help/html/Introduction.htm).</span><span class="sxs-lookup"><span data-stu-id="53e69-351">For more information, see the [JSON.NET documentation](https://www.newtonsoft.com/json/help/html/Introduction.htm).</span></span>
 
<span data-ttu-id="53e69-352">Przykładowo, aby skonfigurować serializator do używania nazw właściwości "PascalCase" zamiast domyślnych nazw "camelCase", należy użyć następującego kodu w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="53e69-352">As an example, to configure the serializer to use "PascalCase" property names, instead of the default "camelCase" names, use the following code in `Startup.ConfigureServices`:</span></span>
 
```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver =
            new DefaultContractResolver();
    });
```

<span data-ttu-id="53e69-353">W kliencie .NET ta sama Metoda rozszerzenia `AddJsonProtocol` istnieje w [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="53e69-353">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="53e69-354">Aby rozwiązać metodę rozszerzenia, należy zaimportować przestrzeń nazw `Microsoft.Extensions.DependencyInjection`:</span><span class="sxs-lookup"><span data-stu-id="53e69-354">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

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
> <span data-ttu-id="53e69-355">W tej chwili nie można skonfigurować serializacji JSON w kliencie JavaScript.</span><span class="sxs-lookup"><span data-stu-id="53e69-355">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="53e69-356">Opcje serializacji MessagePack</span><span class="sxs-lookup"><span data-stu-id="53e69-356">MessagePack serialization options</span></span>

<span data-ttu-id="53e69-357">Serializacji MessagePack można skonfigurować, dostarczając delegata do wywołania [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) .</span><span class="sxs-lookup"><span data-stu-id="53e69-357">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="53e69-358">Aby uzyskać więcej informacji, zobacz [MessagePack w sygnalizacji](xref:signalr/messagepackhubprotocol) .</span><span class="sxs-lookup"><span data-stu-id="53e69-358">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="53e69-359">W tej chwili nie można skonfigurować serializacji MessagePack w kliencie JavaScript.</span><span class="sxs-lookup"><span data-stu-id="53e69-359">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="53e69-360">Konfigurowanie opcji serwera</span><span class="sxs-lookup"><span data-stu-id="53e69-360">Configure server options</span></span>

<span data-ttu-id="53e69-361">W poniższej tabeli opisano opcje konfigurowania centrów sygnałów:</span><span class="sxs-lookup"><span data-stu-id="53e69-361">The following table describes options for configuring SignalR hubs:</span></span>

| <span data-ttu-id="53e69-362">Opcja</span><span class="sxs-lookup"><span data-stu-id="53e69-362">Option</span></span> | <span data-ttu-id="53e69-363">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="53e69-363">Default Value</span></span> | <span data-ttu-id="53e69-364">Opis</span><span class="sxs-lookup"><span data-stu-id="53e69-364">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="53e69-365">30 sekund</span><span class="sxs-lookup"><span data-stu-id="53e69-365">30 seconds</span></span> | <span data-ttu-id="53e69-366">Serwer Rozważ odłączenie klienta, jeśli nie odebrano komunikatu (w tym w tym interwale).</span><span class="sxs-lookup"><span data-stu-id="53e69-366">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="53e69-367">Może upłynąć dłużej niż ten interwał limitu czasu, aby klient mógł zostać oznaczony jako odłączony, ze względu na to, jak jest to zaimplementowane.</span><span class="sxs-lookup"><span data-stu-id="53e69-367">It could take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="53e69-368">Zalecaną wartością jest podwójna wartość `KeepAliveInterval`.</span><span class="sxs-lookup"><span data-stu-id="53e69-368">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="53e69-369">15 sekund</span><span class="sxs-lookup"><span data-stu-id="53e69-369">15 seconds</span></span> | <span data-ttu-id="53e69-370">Jeśli klient nie wyśle początkowego komunikatu uzgadniania w tym przedziale czasu, połączenie zostanie zamknięte.</span><span class="sxs-lookup"><span data-stu-id="53e69-370">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="53e69-371">Jest to ustawienie zaawansowane, które należy zmodyfikować tylko w przypadku wystąpienia błędów limitu czasu uzgadniania z powodu poważnego opóźnienia sieci.</span><span class="sxs-lookup"><span data-stu-id="53e69-371">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="53e69-372">Aby uzyskać więcej szczegółów na temat procesu uzgadniania, zobacz [specyfikację protokołu centrum sygnału](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="53e69-372">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="53e69-373">15 sekund</span><span class="sxs-lookup"><span data-stu-id="53e69-373">15 seconds</span></span> | <span data-ttu-id="53e69-374">Jeśli serwer nie wysłał komunikatu w tym interwale, zostanie automatycznie wysłana wiadomość ping w celu pozostawienia otwartego połączenia.</span><span class="sxs-lookup"><span data-stu-id="53e69-374">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="53e69-375">Podczas zmiany `KeepAliveInterval`Zmień ustawienie `ServerTimeout`/`serverTimeoutInMilliseconds` na kliencie.</span><span class="sxs-lookup"><span data-stu-id="53e69-375">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="53e69-376">Zalecana wartość `ServerTimeout`/`serverTimeoutInMilliseconds` jest podwójna `KeepAliveInterval` wartość.</span><span class="sxs-lookup"><span data-stu-id="53e69-376">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="53e69-377">Wszystkie zainstalowane protokoły</span><span class="sxs-lookup"><span data-stu-id="53e69-377">All installed protocols</span></span> | <span data-ttu-id="53e69-378">Protokoły obsługiwane przez to centrum.</span><span class="sxs-lookup"><span data-stu-id="53e69-378">Protocols supported by this hub.</span></span> <span data-ttu-id="53e69-379">Domyślnie wszystkie protokoły zarejestrowane na serwerze są dozwolone, ale protokoły można usunąć z tej listy, aby wyłączyć określone protokoły dla poszczególnych centrów.</span><span class="sxs-lookup"><span data-stu-id="53e69-379">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="53e69-380">Jeśli `true`, szczegółowe komunikaty o wyjątkach są zwracane do klientów, gdy wyjątek jest zgłaszany w metodzie centrum.</span><span class="sxs-lookup"><span data-stu-id="53e69-380">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="53e69-381">Wartość domyślna to `false`, ponieważ te komunikaty o wyjątkach mogą zawierać informacje poufne.</span><span class="sxs-lookup"><span data-stu-id="53e69-381">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

<span data-ttu-id="53e69-382">Opcje można skonfigurować dla wszystkich centrów, dostarczając opcje delegata do wywołania `AddSignalR` w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="53e69-382">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

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

<span data-ttu-id="53e69-383">Opcje jednego centrum przesłaniają opcje globalne dostępne w `AddSignalR` i można je skonfigurować za pomocą <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span><span class="sxs-lookup"><span data-stu-id="53e69-383">Options for a single hub override the global options provided in `AddSignalR` and can be configured using <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

### <a name="advanced-http-configuration-options"></a><span data-ttu-id="53e69-384">Zaawansowane opcje konfiguracji HTTP</span><span class="sxs-lookup"><span data-stu-id="53e69-384">Advanced HTTP configuration options</span></span>

<span data-ttu-id="53e69-385">Użyj `HttpConnectionDispatcherOptions`, aby skonfigurować zaawansowane ustawienia związane z transportami i zarządzaniem buforem pamięci.</span><span class="sxs-lookup"><span data-stu-id="53e69-385">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="53e69-386">Te opcje są konfigurowane przez przekazanie delegata do [MapHub\<t >](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) w `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="53e69-386">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) in `Startup.Configure`.</span></span>

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

<span data-ttu-id="53e69-387">W poniższej tabeli opisano opcje konfigurowania zaawansowanych opcji protokołu HTTP w programie ASP.NET Core Signal:</span><span class="sxs-lookup"><span data-stu-id="53e69-387">The following table describes options for configuring ASP.NET Core SignalR's advanced HTTP options:</span></span>

| <span data-ttu-id="53e69-388">Opcja</span><span class="sxs-lookup"><span data-stu-id="53e69-388">Option</span></span> | <span data-ttu-id="53e69-389">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="53e69-389">Default Value</span></span> | <span data-ttu-id="53e69-390">Opis</span><span class="sxs-lookup"><span data-stu-id="53e69-390">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="53e69-391">32 KB</span><span class="sxs-lookup"><span data-stu-id="53e69-391">32 KB</span></span> | <span data-ttu-id="53e69-392">Maksymalna liczba bajtów odebranych od klienta, które buforuje serwer.</span><span class="sxs-lookup"><span data-stu-id="53e69-392">The maximum number of bytes received from the client that the server buffers.</span></span> <span data-ttu-id="53e69-393">Zwiększenie tej wartości umożliwia serwerowi otrzymywanie większych komunikatów, ale może mieć negatywny wpływ na użycie pamięci.</span><span class="sxs-lookup"><span data-stu-id="53e69-393">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="53e69-394">Dane zbierane automatycznie z `Authorize` atrybuty zastosowane do klasy centrum.</span><span class="sxs-lookup"><span data-stu-id="53e69-394">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="53e69-395">Lista obiektów [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) służąca do określenia, czy klient jest autoryzowany do łączenia się z centrum.</span><span class="sxs-lookup"><span data-stu-id="53e69-395">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="53e69-396">32 KB</span><span class="sxs-lookup"><span data-stu-id="53e69-396">32 KB</span></span> | <span data-ttu-id="53e69-397">Maksymalna liczba bajtów wysłanych przez aplikację, które buforują serwer.</span><span class="sxs-lookup"><span data-stu-id="53e69-397">The maximum number of bytes sent by the app that the server buffers.</span></span> <span data-ttu-id="53e69-398">Zwiększenie tej wartości umożliwia serwerowi wysyłanie większych komunikatów, ale może mieć negatywny wpływ na użycie pamięci.</span><span class="sxs-lookup"><span data-stu-id="53e69-398">Increasing this value allows the server to send larger messages, but can negatively impact memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="53e69-399">Wszystkie transporty są włączone.</span><span class="sxs-lookup"><span data-stu-id="53e69-399">All Transports are enabled.</span></span> | <span data-ttu-id="53e69-400">Wyliczenie flag bitowych wartości `HttpTransportType`, które mogą ograniczyć transporty, z których może korzystać klient do nawiązywania połączeń.</span><span class="sxs-lookup"><span data-stu-id="53e69-400">A bit flags enum of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="53e69-401">Zobacz poniżej.</span><span class="sxs-lookup"><span data-stu-id="53e69-401">See below.</span></span> | <span data-ttu-id="53e69-402">Dodatkowe opcje specyficzne dla długiego transportu sondowania.</span><span class="sxs-lookup"><span data-stu-id="53e69-402">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="53e69-403">Zobacz poniżej.</span><span class="sxs-lookup"><span data-stu-id="53e69-403">See below.</span></span> | <span data-ttu-id="53e69-404">Dodatkowe opcje specyficzne dla transportu obiektów WebSockets.</span><span class="sxs-lookup"><span data-stu-id="53e69-404">Additional options specific to the WebSockets transport.</span></span> |

<span data-ttu-id="53e69-405">W przypadku długiego transportu sondowania dostępne są dodatkowe opcje, które można skonfigurować przy użyciu właściwości `LongPolling`:</span><span class="sxs-lookup"><span data-stu-id="53e69-405">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="53e69-406">Opcja</span><span class="sxs-lookup"><span data-stu-id="53e69-406">Option</span></span> | <span data-ttu-id="53e69-407">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="53e69-407">Default Value</span></span> | <span data-ttu-id="53e69-408">Opis</span><span class="sxs-lookup"><span data-stu-id="53e69-408">Description</span></span> |
| ------ | ------------- | ----------- |
| `PollTimeout` | <span data-ttu-id="53e69-409">90 sekund</span><span class="sxs-lookup"><span data-stu-id="53e69-409">90 seconds</span></span> | <span data-ttu-id="53e69-410">Maksymalny czas, przez jaki serwer czeka na wysłanie wiadomości do klienta przed zakończeniem jednego żądania sondowania.</span><span class="sxs-lookup"><span data-stu-id="53e69-410">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="53e69-411">Zmniejszenie tej wartości powoduje, że klient wystawia nowe żądania sondy częściej.</span><span class="sxs-lookup"><span data-stu-id="53e69-411">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> |

<span data-ttu-id="53e69-412">Transport WebSocket ma dodatkowe opcje, które można skonfigurować za pomocą właściwości `WebSockets`:</span><span class="sxs-lookup"><span data-stu-id="53e69-412">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="53e69-413">Opcja</span><span class="sxs-lookup"><span data-stu-id="53e69-413">Option</span></span> | <span data-ttu-id="53e69-414">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="53e69-414">Default Value</span></span> | <span data-ttu-id="53e69-415">Opis</span><span class="sxs-lookup"><span data-stu-id="53e69-415">Description</span></span> |
| ------ | ------------- | ----------- |
| `CloseTimeout` | <span data-ttu-id="53e69-416">5 sekund</span><span class="sxs-lookup"><span data-stu-id="53e69-416">5 seconds</span></span> | <span data-ttu-id="53e69-417">Gdy serwer zostanie zamknięty, jeśli klient nie zostanie zamknięty w tym okresie, połączenie zostanie przerwane.</span><span class="sxs-lookup"><span data-stu-id="53e69-417">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | `null` | <span data-ttu-id="53e69-418">Delegat, którego można użyć do ustawienia nagłówka `Sec-WebSocket-Protocol` na wartość niestandardową.</span><span class="sxs-lookup"><span data-stu-id="53e69-418">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="53e69-419">Delegat otrzymuje wartości żądane przez klienta jako dane wejściowe i oczekuje na zwrócenie żądanej wartości.</span><span class="sxs-lookup"><span data-stu-id="53e69-419">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="53e69-420">Konfigurowanie opcji klienta</span><span class="sxs-lookup"><span data-stu-id="53e69-420">Configure client options</span></span>

<span data-ttu-id="53e69-421">Opcje klienta można skonfigurować dla typu `HubConnectionBuilder` (dostępnego w klientach .NET i JavaScript).</span><span class="sxs-lookup"><span data-stu-id="53e69-421">Client options can be configured on the `HubConnectionBuilder` type (available in the .NET and JavaScript clients).</span></span> <span data-ttu-id="53e69-422">Jest ona również dostępna w kliencie Java, ale podklasa `HttpHubConnectionBuilder` zawiera opcje konfiguracji konstruktora, a także `HubConnection` samego siebie.</span><span class="sxs-lookup"><span data-stu-id="53e69-422">It's also available in the Java client, but the `HttpHubConnectionBuilder` subclass is what contains the builder configuration options, as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="53e69-423">Konfigurowanie rejestrowania</span><span class="sxs-lookup"><span data-stu-id="53e69-423">Configure logging</span></span>

<span data-ttu-id="53e69-424">Rejestrowanie jest konfigurowane w kliencie .NET przy użyciu metody `ConfigureLogging`.</span><span class="sxs-lookup"><span data-stu-id="53e69-424">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="53e69-425">Dostawcy i filtry rejestrowania mogą być rejestrowane w taki sam sposób, w jaki znajdują się na serwerze.</span><span class="sxs-lookup"><span data-stu-id="53e69-425">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="53e69-426">Aby uzyskać więcej informacji, zobacz dokumentację dotyczącą [rejestrowania w ASP.NET Core](xref:fundamentals/logging/index) .</span><span class="sxs-lookup"><span data-stu-id="53e69-426">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="53e69-427">Aby zarejestrować dostawców rejestrowania, należy zainstalować wymagane pakiety.</span><span class="sxs-lookup"><span data-stu-id="53e69-427">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="53e69-428">Aby zapoznać się z pełną listą, zobacz sekcję [wbudowane dostawcy rejestrowania](xref:fundamentals/logging/index#built-in-logging-providers) w witrynie docs.</span><span class="sxs-lookup"><span data-stu-id="53e69-428">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="53e69-429">Aby na przykład włączyć rejestrowanie konsoli, należy zainstalować pakiet NuGet `Microsoft.Extensions.Logging.Console`.</span><span class="sxs-lookup"><span data-stu-id="53e69-429">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="53e69-430">Wywołaj metodę rozszerzenia `AddConsole`:</span><span class="sxs-lookup"><span data-stu-id="53e69-430">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="53e69-431">W kliencie JavaScript istnieje podobna Metoda `configureLogging`.</span><span class="sxs-lookup"><span data-stu-id="53e69-431">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="53e69-432">Podaj wartość `LogLevel` wskazującą minimalny poziom komunikatów dziennika do wygenerowania.</span><span class="sxs-lookup"><span data-stu-id="53e69-432">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="53e69-433">Dzienniki są zapisywane w oknie konsoli przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="53e69-433">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

> [!NOTE]
> <span data-ttu-id="53e69-434">Aby całkowicie wyłączyć rejestrowanie, określ `signalR.LogLevel.None` w `configureLogging` metodzie.</span><span class="sxs-lookup"><span data-stu-id="53e69-434">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="53e69-435">Aby uzyskać więcej informacji na temat rejestrowania, zobacz [dokumentację diagnostyki sygnału](xref:signalr/diagnostics).</span><span class="sxs-lookup"><span data-stu-id="53e69-435">For more information on logging, see the [SignalR Diagnostics documentation](xref:signalr/diagnostics).</span></span>

<span data-ttu-id="53e69-436">Klient Java sygnalizujący używa biblioteki [SLF4J](https://www.slf4j.org/) do rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="53e69-436">The SignalR Java client uses the [SLF4J](https://www.slf4j.org/) library for logging.</span></span> <span data-ttu-id="53e69-437">Jest to interfejs API rejestrowania wysokiego poziomu, który umożliwia użytkownikom biblioteki wybranie własnych implementacji rejestrowania przez nałożenie określonej zależności rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="53e69-437">It's a high-level logging API that allows users of the library to chose their own specific logging implementation by bringing in a specific logging dependency.</span></span> <span data-ttu-id="53e69-438">Poniższy fragment kodu przedstawia sposób użycia `java.util.logging` z klientem języka Java sygnalizującego.</span><span class="sxs-lookup"><span data-stu-id="53e69-438">The following code snippet shows how to use `java.util.logging` with the SignalR Java client.</span></span>

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

<span data-ttu-id="53e69-439">Jeśli nie skonfigurujesz rejestrowania w zależnościach, SLF4J ładuje domyślny Rejestrator braku operacji z następującym komunikatem ostrzegawczym:</span><span class="sxs-lookup"><span data-stu-id="53e69-439">If you don't configure logging in your dependencies, SLF4J loads a default no-operation logger with the following warning message:</span></span>

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

<span data-ttu-id="53e69-440">Można je bezpiecznie zignorować.</span><span class="sxs-lookup"><span data-stu-id="53e69-440">This can safely be ignored.</span></span>

### <a name="configure-allowed-transports"></a><span data-ttu-id="53e69-441">Skonfiguruj dozwolone transporty</span><span class="sxs-lookup"><span data-stu-id="53e69-441">Configure allowed transports</span></span>

<span data-ttu-id="53e69-442">Transporty używane przez sygnalizującego można skonfigurować w wywołaniu `WithUrl` (`withUrl` w języku JavaScript).</span><span class="sxs-lookup"><span data-stu-id="53e69-442">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="53e69-443">Wartości `HttpTransportType` mogą być używane w celu ograniczenia klienta do używania tylko określonych transportów.</span><span class="sxs-lookup"><span data-stu-id="53e69-443">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="53e69-444">Wszystkie transporty są domyślnie włączone.</span><span class="sxs-lookup"><span data-stu-id="53e69-444">All transports are enabled by default.</span></span>

<span data-ttu-id="53e69-445">Na przykład, aby wyłączyć transport zdarzeń wysłanych przez serwer, ale zezwolić na połączenia z usługą WebSockets i długie sondowanie:</span><span class="sxs-lookup"><span data-stu-id="53e69-445">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="53e69-446">W kliencie JavaScript transporty są konfigurowane przez ustawienie pola `transport` w obiekcie Options udostępnianym `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="53e69-446">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

<span data-ttu-id="53e69-447">W tej wersji elementu WebSockets klienta Java jest jedynym dostępnym transportem.</span><span class="sxs-lookup"><span data-stu-id="53e69-447">In this version of the Java client websockets is the only available transport.</span></span>

### <a name="configure-bearer-authentication"></a><span data-ttu-id="53e69-448">Konfigurowanie uwierzytelniania okaziciela</span><span class="sxs-lookup"><span data-stu-id="53e69-448">Configure bearer authentication</span></span>

<span data-ttu-id="53e69-449">Aby zapewnić dane uwierzytelniania wraz z żądaniami sygnałów, użyj opcji `AccessTokenProvider` (`accessTokenFactory` w języku JavaScript), aby określić funkcję, która zwraca żądany token dostępu.</span><span class="sxs-lookup"><span data-stu-id="53e69-449">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="53e69-450">W kliencie .NET token dostępu jest przenoszona jako token "uwierzytelnianie okaziciela" protokołu HTTP (przy użyciu nagłówka `Authorization` z typem `Bearer`).</span><span class="sxs-lookup"><span data-stu-id="53e69-450">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="53e69-451">W kliencie JavaScript token dostępu jest używany jako token okaziciela, **z wyjątkiem** kilku przypadków, w których interfejsy API przeglądarki ograniczają możliwość zastosowania nagłówków (w konkretnym przypadku zdarzeń wysyłanych przez serwer i żądań WebSockets).</span><span class="sxs-lookup"><span data-stu-id="53e69-451">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="53e69-452">W takich przypadkach token dostępu jest dostarczany jako wartość ciągu zapytania `access_token`.</span><span class="sxs-lookup"><span data-stu-id="53e69-452">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="53e69-453">W kliencie .NET opcja `AccessTokenProvider` można określić za pomocą delegata opcji w `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="53e69-453">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

<span data-ttu-id="53e69-454">W kliencie JavaScript token dostępu jest konfigurowany przez ustawienie pola `accessTokenFactory` w obiekcie Options w `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="53e69-454">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

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

<span data-ttu-id="53e69-455">W kliencie Java programu sygnalizującego można skonfigurować token okaziciela do użycia na potrzeby uwierzytelniania, dostarczając fabrykę tokenów dostępu do [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span><span class="sxs-lookup"><span data-stu-id="53e69-455">In the SignalR Java client, you can configure a bearer token to use for authentication by providing an access token factory to the [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span></span> <span data-ttu-id="53e69-456">Użyj [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) , aby podać [](https://github.com/ReactiveX/RxJava) [>\<pojedynczego ciągu](https://reactivex.io/documentation/single.html).</span><span class="sxs-lookup"><span data-stu-id="53e69-456">Use [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) to provide an [RxJava](https://github.com/ReactiveX/RxJava) [Single\<String>](https://reactivex.io/documentation/single.html).</span></span> <span data-ttu-id="53e69-457">Wywołanie metody [Single. Ustąp](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-)umożliwia zapisanie logiki w celu utworzenia tokenów dostępu dla klienta.</span><span class="sxs-lookup"><span data-stu-id="53e69-457">With a call to [Single.defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), you can write logic to produce access tokens for your client.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="53e69-458">Konfigurowanie opcji limitu czasu i utrzymywania aktywności</span><span class="sxs-lookup"><span data-stu-id="53e69-458">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="53e69-459">W celu skonfigurowania zachowania przekroczenia limitu czasu i utrzymywania aktywności są dostępne dodatkowe opcje dla samego obiektu `HubConnection`:</span><span class="sxs-lookup"><span data-stu-id="53e69-459">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>

# <a name="net"></a>[<span data-ttu-id="53e69-460">.NET</span><span class="sxs-lookup"><span data-stu-id="53e69-460">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="53e69-461">Opcja</span><span class="sxs-lookup"><span data-stu-id="53e69-461">Option</span></span> | <span data-ttu-id="53e69-462">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="53e69-462">Default value</span></span> | <span data-ttu-id="53e69-463">Opis</span><span class="sxs-lookup"><span data-stu-id="53e69-463">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="53e69-464">30 sekund (30 000 MS)</span><span class="sxs-lookup"><span data-stu-id="53e69-464">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="53e69-465">Limit czasu dla działania serwera.</span><span class="sxs-lookup"><span data-stu-id="53e69-465">Timeout for server activity.</span></span> <span data-ttu-id="53e69-466">Jeśli serwer nie wysłał komunikatu w tym interwale, Klient uważa, że serwer odłączył się i wyzwala zdarzenie `Closed` (`onclose` w języku JavaScript).</span><span class="sxs-lookup"><span data-stu-id="53e69-466">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="53e69-467">Ta wartość musi być wystarczająca do wysłania wiadomości ping z serwera **i** odebrana przez klienta w ramach przedziału czasu.</span><span class="sxs-lookup"><span data-stu-id="53e69-467">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="53e69-468">Zalecana wartość to liczba z co najmniej podwójną wartością `KeepAliveInterval` serwera, aby umożliwić czas na nadejście poleceń ping.</span><span class="sxs-lookup"><span data-stu-id="53e69-468">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="53e69-469">15 sekund</span><span class="sxs-lookup"><span data-stu-id="53e69-469">15 seconds</span></span> | <span data-ttu-id="53e69-470">Limit czasu początkowej uzgadniania z serwerem.</span><span class="sxs-lookup"><span data-stu-id="53e69-470">Timeout for initial server handshake.</span></span> <span data-ttu-id="53e69-471">Jeśli serwer nie wyśle odpowiedzi uzgadniania w tym interwale, klient anuluje uzgadnianie i wyzwala zdarzenie `Closed` (`onclose` w języku JavaScript).</span><span class="sxs-lookup"><span data-stu-id="53e69-471">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="53e69-472">Jest to ustawienie zaawansowane, które należy zmodyfikować tylko w przypadku wystąpienia błędów limitu czasu uzgadniania z powodu poważnego opóźnienia sieci.</span><span class="sxs-lookup"><span data-stu-id="53e69-472">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="53e69-473">Aby uzyskać więcej szczegółów na temat procesu uzgadniania, zobacz [specyfikację protokołu centrum sygnału](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="53e69-473">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="53e69-474">15 sekund</span><span class="sxs-lookup"><span data-stu-id="53e69-474">15 seconds</span></span> | <span data-ttu-id="53e69-475">Określa interwał wysyłania komunikatów ping przez klienta.</span><span class="sxs-lookup"><span data-stu-id="53e69-475">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="53e69-476">Wysłanie wszelkich komunikatów z klienta powoduje zresetowanie czasomierza do początku interwału.</span><span class="sxs-lookup"><span data-stu-id="53e69-476">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="53e69-477">Jeśli klient nie wyśle komunikatu w `ClientTimeoutInterval` zestawie na serwerze, serwer traktuje klienta jako odłączony.</span><span class="sxs-lookup"><span data-stu-id="53e69-477">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

<span data-ttu-id="53e69-478">W kliencie .NET wartości limitu czasu są określane jako wartości `TimeSpan`.</span><span class="sxs-lookup"><span data-stu-id="53e69-478">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascript"></a>[<span data-ttu-id="53e69-479">JavaScript</span><span class="sxs-lookup"><span data-stu-id="53e69-479">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="53e69-480">Opcja</span><span class="sxs-lookup"><span data-stu-id="53e69-480">Option</span></span> | <span data-ttu-id="53e69-481">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="53e69-481">Default value</span></span> | <span data-ttu-id="53e69-482">Opis</span><span class="sxs-lookup"><span data-stu-id="53e69-482">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="53e69-483">30 sekund (30 000 MS)</span><span class="sxs-lookup"><span data-stu-id="53e69-483">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="53e69-484">Limit czasu dla działania serwera.</span><span class="sxs-lookup"><span data-stu-id="53e69-484">Timeout for server activity.</span></span> <span data-ttu-id="53e69-485">Jeśli serwer nie wysłał komunikatu w tym interwale, Klient uważa, że serwer odłączył się i wyzwala zdarzenie `onclose`.</span><span class="sxs-lookup"><span data-stu-id="53e69-485">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="53e69-486">Ta wartość musi być wystarczająca do wysłania wiadomości ping z serwera **i** odebrana przez klienta w ramach przedziału czasu.</span><span class="sxs-lookup"><span data-stu-id="53e69-486">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="53e69-487">Zalecana wartość to liczba z co najmniej podwójną wartością `KeepAliveInterval` serwera, aby umożliwić czas na nadejście poleceń ping.</span><span class="sxs-lookup"><span data-stu-id="53e69-487">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `keepAliveIntervalInMilliseconds` | <span data-ttu-id="53e69-488">15 sekund (15 000 MS)</span><span class="sxs-lookup"><span data-stu-id="53e69-488">15 seconds (15,000 milliseconds)</span></span> | <span data-ttu-id="53e69-489">Określa interwał wysyłania komunikatów ping przez klienta.</span><span class="sxs-lookup"><span data-stu-id="53e69-489">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="53e69-490">Wysłanie wszelkich komunikatów z klienta powoduje zresetowanie czasomierza do początku interwału.</span><span class="sxs-lookup"><span data-stu-id="53e69-490">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="53e69-491">Jeśli klient nie wyśle komunikatu w `ClientTimeoutInterval` zestawie na serwerze, serwer traktuje klienta jako odłączony.</span><span class="sxs-lookup"><span data-stu-id="53e69-491">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

# <a name="java"></a>[<span data-ttu-id="53e69-492">Java</span><span class="sxs-lookup"><span data-stu-id="53e69-492">Java</span></span>](#tab/java)

| <span data-ttu-id="53e69-493">Opcja</span><span class="sxs-lookup"><span data-stu-id="53e69-493">Option</span></span> | <span data-ttu-id="53e69-494">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="53e69-494">Default value</span></span> | <span data-ttu-id="53e69-495">Opis</span><span class="sxs-lookup"><span data-stu-id="53e69-495">Description</span></span> |
| ------ | ------------- | ----------- |
| <span data-ttu-id="53e69-496">`getServerTimeout` / `setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="53e69-496">`getServerTimeout` / `setServerTimeout`</span></span> | <span data-ttu-id="53e69-497">30 sekund (30 000 MS)</span><span class="sxs-lookup"><span data-stu-id="53e69-497">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="53e69-498">Limit czasu dla działania serwera.</span><span class="sxs-lookup"><span data-stu-id="53e69-498">Timeout for server activity.</span></span> <span data-ttu-id="53e69-499">Jeśli serwer nie wysłał komunikatu w tym interwale, Klient uważa, że serwer odłączył się i wyzwala zdarzenie `onClose`.</span><span class="sxs-lookup"><span data-stu-id="53e69-499">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="53e69-500">Ta wartość musi być wystarczająca do wysłania wiadomości ping z serwera **i** odebrana przez klienta w ramach przedziału czasu.</span><span class="sxs-lookup"><span data-stu-id="53e69-500">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="53e69-501">Zalecana wartość to liczba z co najmniej podwójną wartością `KeepAliveInterval` serwera, aby umożliwić czas na nadejście poleceń ping.</span><span class="sxs-lookup"><span data-stu-id="53e69-501">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="53e69-502">15 sekund</span><span class="sxs-lookup"><span data-stu-id="53e69-502">15 seconds</span></span> | <span data-ttu-id="53e69-503">Limit czasu początkowej uzgadniania z serwerem.</span><span class="sxs-lookup"><span data-stu-id="53e69-503">Timeout for initial server handshake.</span></span> <span data-ttu-id="53e69-504">Jeśli serwer nie wyśle odpowiedzi uzgadniania w tym interwale, klient anuluje uzgadnianie i wyzwala zdarzenie `onClose`.</span><span class="sxs-lookup"><span data-stu-id="53e69-504">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="53e69-505">Jest to ustawienie zaawansowane, które należy zmodyfikować tylko w przypadku wystąpienia błędów limitu czasu uzgadniania z powodu poważnego opóźnienia sieci.</span><span class="sxs-lookup"><span data-stu-id="53e69-505">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="53e69-506">Aby uzyskać więcej szczegółów na temat procesu uzgadniania, zobacz [specyfikację protokołu centrum sygnału](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="53e69-506">For more detail on the Handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| <span data-ttu-id="53e69-507">`getKeepAliveInterval` / `setKeepAliveInterval`</span><span class="sxs-lookup"><span data-stu-id="53e69-507">`getKeepAliveInterval` / `setKeepAliveInterval`</span></span> | <span data-ttu-id="53e69-508">15 sekund (15 000 MS)</span><span class="sxs-lookup"><span data-stu-id="53e69-508">15 seconds (15,000 milliseconds)</span></span> | <span data-ttu-id="53e69-509">Określa interwał wysyłania komunikatów ping przez klienta.</span><span class="sxs-lookup"><span data-stu-id="53e69-509">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="53e69-510">Wysłanie wszelkich komunikatów z klienta powoduje zresetowanie czasomierza do początku interwału.</span><span class="sxs-lookup"><span data-stu-id="53e69-510">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="53e69-511">Jeśli klient nie wyśle komunikatu w `ClientTimeoutInterval` zestawie na serwerze, serwer traktuje klienta jako odłączony.</span><span class="sxs-lookup"><span data-stu-id="53e69-511">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

---

### <a name="configure-additional-options"></a><span data-ttu-id="53e69-512">Skonfiguruj dodatkowe opcje</span><span class="sxs-lookup"><span data-stu-id="53e69-512">Configure additional options</span></span>

<span data-ttu-id="53e69-513">Dodatkowe opcje można skonfigurować w metodzie `WithUrl` (`withUrl` w języku JavaScript) na `HubConnectionBuilder` lub w różnych interfejsach API konfiguracji na `HttpHubConnectionBuilder` klienta Java:</span><span class="sxs-lookup"><span data-stu-id="53e69-513">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder` or on the various configuration APIs on the `HttpHubConnectionBuilder` in the Java client:</span></span>

# <a name="net"></a>[<span data-ttu-id="53e69-514">.NET</span><span class="sxs-lookup"><span data-stu-id="53e69-514">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="53e69-515">Opcja .NET</span><span class="sxs-lookup"><span data-stu-id="53e69-515">.NET Option</span></span> |  <span data-ttu-id="53e69-516">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="53e69-516">Default value</span></span> | <span data-ttu-id="53e69-517">Opis</span><span class="sxs-lookup"><span data-stu-id="53e69-517">Description</span></span> |
| ----------- | -------------- | ----------- |
| `AccessTokenProvider` | `null` | <span data-ttu-id="53e69-518">Funkcja zwracająca ciąg, który jest dostarczany jako token uwierzytelniania okaziciela w żądaniach HTTP.</span><span class="sxs-lookup"><span data-stu-id="53e69-518">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `false` | <span data-ttu-id="53e69-519">Ustaw tę wartość na `true`, aby pominąć etap negocjacji.</span><span class="sxs-lookup"><span data-stu-id="53e69-519">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="53e69-520">**Obsługiwane tylko wtedy, gdy transport Sockets jest jedynym włączonym transportem**.</span><span class="sxs-lookup"><span data-stu-id="53e69-520">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="53e69-521">Nie można włączyć tego ustawienia w przypadku korzystania z usługi Azure Signal Service.</span><span class="sxs-lookup"><span data-stu-id="53e69-521">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="53e69-522">Pusty</span><span class="sxs-lookup"><span data-stu-id="53e69-522">Empty</span></span> | <span data-ttu-id="53e69-523">Kolekcja certyfikatów TLS do wysłania w celu uwierzytelniania żądań.</span><span class="sxs-lookup"><span data-stu-id="53e69-523">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="53e69-524">Pusty</span><span class="sxs-lookup"><span data-stu-id="53e69-524">Empty</span></span> | <span data-ttu-id="53e69-525">Kolekcja plików cookie protokołu HTTP do wysłania w każdym żądaniu HTTP.</span><span class="sxs-lookup"><span data-stu-id="53e69-525">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="53e69-526">Pusty</span><span class="sxs-lookup"><span data-stu-id="53e69-526">Empty</span></span> | <span data-ttu-id="53e69-527">Poświadczenia do wysyłania przy każdym żądaniu HTTP.</span><span class="sxs-lookup"><span data-stu-id="53e69-527">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="53e69-528">5 sekund</span><span class="sxs-lookup"><span data-stu-id="53e69-528">5 seconds</span></span> | <span data-ttu-id="53e69-529">Tylko obiekty WebSockets.</span><span class="sxs-lookup"><span data-stu-id="53e69-529">WebSockets only.</span></span> <span data-ttu-id="53e69-530">Maksymalny czas oczekiwania klienta po zamknięciu serwera w celu potwierdzenia żądania zamknięcia.</span><span class="sxs-lookup"><span data-stu-id="53e69-530">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="53e69-531">Jeśli serwer nie potwierdzi zamknięcia w tym czasie, klient zostanie rozłączony.</span><span class="sxs-lookup"><span data-stu-id="53e69-531">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="53e69-532">Pusty</span><span class="sxs-lookup"><span data-stu-id="53e69-532">Empty</span></span> | <span data-ttu-id="53e69-533">Mapa dodatkowych nagłówków HTTP do wysłania w każdym żądaniu HTTP.</span><span class="sxs-lookup"><span data-stu-id="53e69-533">A Map of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | `null` | <span data-ttu-id="53e69-534">Delegat, który może służyć do konfigurowania lub zastępowania `HttpMessageHandler` używany do wysyłania żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="53e69-534">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="53e69-535">Nieużywane na potrzeby połączeń protokołu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="53e69-535">Not used for WebSocket connections.</span></span> <span data-ttu-id="53e69-536">Ten delegat musi zwracać wartość różną od null i otrzymuje wartość domyślną jako parametr.</span><span class="sxs-lookup"><span data-stu-id="53e69-536">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="53e69-537">Zmodyfikuj ustawienia tej wartości domyślnej i zwróć ją lub Zwróć nowe wystąpienie `HttpMessageHandler`.</span><span class="sxs-lookup"><span data-stu-id="53e69-537">Either modify settings on that default value and return it, or return a new `HttpMessageHandler` instance.</span></span> <span data-ttu-id="53e69-538">**Podczas zastępowania programu obsługi należy skopiować ustawienia, które mają być zachowane z podanej procedury obsługi. w przeciwnym razie skonfigurowane opcje (takie jak pliki cookie i nagłówki) nie będą miały zastosowania do nowego programu obsługi.**</span><span class="sxs-lookup"><span data-stu-id="53e69-538">**When replacing the handler make sure to copy the settings you want to keep from the provided handler, otherwise, the configured options (such as Cookies and Headers) won't apply to the new handler.**</span></span> |
| `Proxy` | `null` | <span data-ttu-id="53e69-539">Serwer proxy HTTP, który ma być używany podczas wysyłania żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="53e69-539">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | `false` | <span data-ttu-id="53e69-540">Ustaw tę wartość logiczną, aby wysyłać domyślne poświadczenia dla żądań HTTP i WebSockets.</span><span class="sxs-lookup"><span data-stu-id="53e69-540">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="53e69-541">Umożliwia to korzystanie z uwierzytelniania systemu Windows.</span><span class="sxs-lookup"><span data-stu-id="53e69-541">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | `null` | <span data-ttu-id="53e69-542">Delegat, który może służyć do konfigurowania dodatkowych opcji protokołu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="53e69-542">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="53e69-543">Odbiera wystąpienie elementu [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) , którego można użyć do skonfigurowania opcji.</span><span class="sxs-lookup"><span data-stu-id="53e69-543">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

# <a name="javascript"></a>[<span data-ttu-id="53e69-544">JavaScript</span><span class="sxs-lookup"><span data-stu-id="53e69-544">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="53e69-545">Opcja języka JavaScript</span><span class="sxs-lookup"><span data-stu-id="53e69-545">JavaScript Option</span></span> | <span data-ttu-id="53e69-546">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="53e69-546">Default Value</span></span> | <span data-ttu-id="53e69-547">Opis</span><span class="sxs-lookup"><span data-stu-id="53e69-547">Description</span></span> |
| ----------------- | ------------- | ----------- |
| `accessTokenFactory` | `null` | <span data-ttu-id="53e69-548">Funkcja zwracająca ciąg, który jest dostarczany jako token uwierzytelniania okaziciela w żądaniach HTTP.</span><span class="sxs-lookup"><span data-stu-id="53e69-548">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `skipNegotiation` | `false` | <span data-ttu-id="53e69-549">Ustaw tę wartość na `true`, aby pominąć etap negocjacji.</span><span class="sxs-lookup"><span data-stu-id="53e69-549">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="53e69-550">**Obsługiwane tylko wtedy, gdy transport Sockets jest jedynym włączonym transportem**.</span><span class="sxs-lookup"><span data-stu-id="53e69-550">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="53e69-551">Nie można włączyć tego ustawienia w przypadku korzystania z usługi Azure Signal Service.</span><span class="sxs-lookup"><span data-stu-id="53e69-551">This setting can't be enabled when using the Azure SignalR Service.</span></span> |

# <a name="java"></a>[<span data-ttu-id="53e69-552">Java</span><span class="sxs-lookup"><span data-stu-id="53e69-552">Java</span></span>](#tab/java)

| <span data-ttu-id="53e69-553">Opcja Java</span><span class="sxs-lookup"><span data-stu-id="53e69-553">Java Option</span></span> | <span data-ttu-id="53e69-554">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="53e69-554">Default Value</span></span> | <span data-ttu-id="53e69-555">Opis</span><span class="sxs-lookup"><span data-stu-id="53e69-555">Description</span></span> |
| ----------- | ------------- | ----------- |
| `withAccessTokenProvider` | `null` | <span data-ttu-id="53e69-556">Funkcja zwracająca ciąg, który jest dostarczany jako token uwierzytelniania okaziciela w żądaniach HTTP.</span><span class="sxs-lookup"><span data-stu-id="53e69-556">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `shouldSkipNegotiate` | `false` | <span data-ttu-id="53e69-557">Ustaw tę wartość na `true`, aby pominąć etap negocjacji.</span><span class="sxs-lookup"><span data-stu-id="53e69-557">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="53e69-558">**Obsługiwane tylko wtedy, gdy transport Sockets jest jedynym włączonym transportem**.</span><span class="sxs-lookup"><span data-stu-id="53e69-558">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="53e69-559">Nie można włączyć tego ustawienia w przypadku korzystania z usługi Azure Signal Service.</span><span class="sxs-lookup"><span data-stu-id="53e69-559">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| <span data-ttu-id="53e69-560">`withHeader` `withHeaders`</span><span class="sxs-lookup"><span data-stu-id="53e69-560">`withHeader` `withHeaders`</span></span> | <span data-ttu-id="53e69-561">Pusty</span><span class="sxs-lookup"><span data-stu-id="53e69-561">Empty</span></span> | <span data-ttu-id="53e69-562">Mapa dodatkowych nagłówków HTTP do wysłania w każdym żądaniu HTTP.</span><span class="sxs-lookup"><span data-stu-id="53e69-562">A Map of additional HTTP headers to send with every HTTP request.</span></span> |

---

<span data-ttu-id="53e69-563">W kliencie .NET opcje te można modyfikować za pomocą delegata opcji udostępnianego do `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="53e69-563">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="53e69-564">W kliencie JavaScript te opcje można podać w obiekcie JavaScript udostępnionym do `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="53e69-564">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

<span data-ttu-id="53e69-565">W kliencie Java Opcje te można skonfigurować przy użyciu metod na `HttpHubConnectionBuilder` zwracanych z `HubConnectionBuilder.create("HUB URL")`</span><span class="sxs-lookup"><span data-stu-id="53e69-565">In the Java client, these options can be configured with the methods on the `HttpHubConnectionBuilder` returned from the `HubConnectionBuilder.create("HUB URL")`</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
        .withHeader("Foo", "Bar")
        .shouldSkipNegotiate(true)
        .withHandshakeResponseTimeout(30*1000)
        .build();
```

## <a name="additional-resources"></a><span data-ttu-id="53e69-566">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="53e69-566">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>

::: moniker-end
::: moniker range="< aspnetcore-2.2"

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="53e69-567">Opcje serializacji JSON/MessagePack</span><span class="sxs-lookup"><span data-stu-id="53e69-567">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="53e69-568">ASP.NET Core sygnalizujący obsługuje dwa protokoły dla komunikatów kodowania: [JSON](https://www.json.org/) i [MessagePack](https://msgpack.org/index.html).</span><span class="sxs-lookup"><span data-stu-id="53e69-568">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="53e69-569">Każdy protokół ma opcje konfiguracji serializacji.</span><span class="sxs-lookup"><span data-stu-id="53e69-569">Each protocol has serialization configuration options.</span></span>

<span data-ttu-id="53e69-570">Serializacji JSON można skonfigurować na serwerze przy użyciu metody rozszerzenia [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) , która może zostać dodana po metodzie [addsygnalizująca](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="53e69-570">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method, which can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in your `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="53e69-571">Metoda `AddJsonProtocol` przyjmuje delegata, który odbiera obiekt `options`.</span><span class="sxs-lookup"><span data-stu-id="53e69-571">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="53e69-572">Właściwość [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) tego obiektu jest obiektem JSON.NET `JsonSerializerSettings`, którego można użyć do skonfigurowania serializacji argumentów i zwracanych wartości.</span><span class="sxs-lookup"><span data-stu-id="53e69-572">The [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) property on that object is a JSON.NET `JsonSerializerSettings` object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="53e69-573">Aby uzyskać więcej informacji, zapoznaj się z [dokumentacją JSON.NET](https://www.newtonsoft.com/json/help/html/Introduction.htm).</span><span class="sxs-lookup"><span data-stu-id="53e69-573">For more information, see the [JSON.NET documentation](https://www.newtonsoft.com/json/help/html/Introduction.htm).</span></span>
 
<span data-ttu-id="53e69-574">Przykładowo, aby skonfigurować serializator do używania nazw właściwości "PascalCase" zamiast domyślnych nazw "camelCase", należy użyć następującego kodu w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="53e69-574">As an example, to configure the serializer to use "PascalCase" property names, instead of the default "camelCase" names, use the following code in `Startup.ConfigureServices`:</span></span>
 
```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver =
            new DefaultContractResolver();
    });
```

<span data-ttu-id="53e69-575">W kliencie .NET ta sama Metoda rozszerzenia `AddJsonProtocol` istnieje w [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="53e69-575">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="53e69-576">Aby rozwiązać metodę rozszerzenia, należy zaimportować przestrzeń nazw `Microsoft.Extensions.DependencyInjection`:</span><span class="sxs-lookup"><span data-stu-id="53e69-576">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

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
> <span data-ttu-id="53e69-577">W tej chwili nie można skonfigurować serializacji JSON w kliencie JavaScript.</span><span class="sxs-lookup"><span data-stu-id="53e69-577">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="53e69-578">Opcje serializacji MessagePack</span><span class="sxs-lookup"><span data-stu-id="53e69-578">MessagePack serialization options</span></span>

<span data-ttu-id="53e69-579">Serializacji MessagePack można skonfigurować, dostarczając delegata do wywołania [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) .</span><span class="sxs-lookup"><span data-stu-id="53e69-579">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="53e69-580">Aby uzyskać więcej informacji, zobacz [MessagePack w sygnalizacji](xref:signalr/messagepackhubprotocol) .</span><span class="sxs-lookup"><span data-stu-id="53e69-580">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="53e69-581">W tej chwili nie można skonfigurować serializacji MessagePack w kliencie JavaScript.</span><span class="sxs-lookup"><span data-stu-id="53e69-581">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="53e69-582">Konfigurowanie opcji serwera</span><span class="sxs-lookup"><span data-stu-id="53e69-582">Configure server options</span></span>

<span data-ttu-id="53e69-583">W poniższej tabeli opisano opcje konfigurowania centrów sygnałów:</span><span class="sxs-lookup"><span data-stu-id="53e69-583">The following table describes options for configuring SignalR hubs:</span></span>

| <span data-ttu-id="53e69-584">Opcja</span><span class="sxs-lookup"><span data-stu-id="53e69-584">Option</span></span> | <span data-ttu-id="53e69-585">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="53e69-585">Default Value</span></span> | <span data-ttu-id="53e69-586">Opis</span><span class="sxs-lookup"><span data-stu-id="53e69-586">Description</span></span> |
| ------ | ------------- | ----------- |
| `HandshakeTimeout` | <span data-ttu-id="53e69-587">15 sekund</span><span class="sxs-lookup"><span data-stu-id="53e69-587">15 seconds</span></span> | <span data-ttu-id="53e69-588">Jeśli klient nie wyśle początkowego komunikatu uzgadniania w tym przedziale czasu, połączenie zostanie zamknięte.</span><span class="sxs-lookup"><span data-stu-id="53e69-588">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="53e69-589">Jest to ustawienie zaawansowane, które należy zmodyfikować tylko w przypadku wystąpienia błędów limitu czasu uzgadniania z powodu poważnego opóźnienia sieci.</span><span class="sxs-lookup"><span data-stu-id="53e69-589">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="53e69-590">Aby uzyskać więcej szczegółów na temat procesu uzgadniania, zobacz [specyfikację protokołu centrum sygnału](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="53e69-590">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="53e69-591">15 sekund</span><span class="sxs-lookup"><span data-stu-id="53e69-591">15 seconds</span></span> | <span data-ttu-id="53e69-592">Jeśli serwer nie wysłał komunikatu w tym interwale, zostanie automatycznie wysłana wiadomość ping w celu pozostawienia otwartego połączenia.</span><span class="sxs-lookup"><span data-stu-id="53e69-592">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="53e69-593">Podczas zmiany `KeepAliveInterval`Zmień ustawienie `ServerTimeout`/`serverTimeoutInMilliseconds` na kliencie.</span><span class="sxs-lookup"><span data-stu-id="53e69-593">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="53e69-594">Zalecana wartość `ServerTimeout`/`serverTimeoutInMilliseconds` jest podwójna `KeepAliveInterval` wartość.</span><span class="sxs-lookup"><span data-stu-id="53e69-594">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="53e69-595">Wszystkie zainstalowane protokoły</span><span class="sxs-lookup"><span data-stu-id="53e69-595">All installed protocols</span></span> | <span data-ttu-id="53e69-596">Protokoły obsługiwane przez to centrum.</span><span class="sxs-lookup"><span data-stu-id="53e69-596">Protocols supported by this hub.</span></span> <span data-ttu-id="53e69-597">Domyślnie wszystkie protokoły zarejestrowane na serwerze są dozwolone, ale protokoły można usunąć z tej listy, aby wyłączyć określone protokoły dla poszczególnych centrów.</span><span class="sxs-lookup"><span data-stu-id="53e69-597">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="53e69-598">Jeśli `true`, szczegółowe komunikaty o wyjątkach są zwracane do klientów, gdy wyjątek jest zgłaszany w metodzie centrum.</span><span class="sxs-lookup"><span data-stu-id="53e69-598">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="53e69-599">Wartość domyślna to `false`, ponieważ te komunikaty o wyjątkach mogą zawierać informacje poufne.</span><span class="sxs-lookup"><span data-stu-id="53e69-599">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

<span data-ttu-id="53e69-600">Opcje można skonfigurować dla wszystkich centrów, dostarczając opcje delegata do wywołania `AddSignalR` w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="53e69-600">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

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

<span data-ttu-id="53e69-601">Opcje jednego centrum przesłaniają opcje globalne dostępne w `AddSignalR` i można je skonfigurować za pomocą <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span><span class="sxs-lookup"><span data-stu-id="53e69-601">Options for a single hub override the global options provided in `AddSignalR` and can be configured using <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

### <a name="advanced-http-configuration-options"></a><span data-ttu-id="53e69-602">Zaawansowane opcje konfiguracji HTTP</span><span class="sxs-lookup"><span data-stu-id="53e69-602">Advanced HTTP configuration options</span></span>

<span data-ttu-id="53e69-603">Użyj `HttpConnectionDispatcherOptions`, aby skonfigurować zaawansowane ustawienia związane z transportami i zarządzaniem buforem pamięci.</span><span class="sxs-lookup"><span data-stu-id="53e69-603">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="53e69-604">Te opcje są konfigurowane przez przekazanie delegata do [MapHub\<t >](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) w `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="53e69-604">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) in `Startup.Configure`.</span></span>

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

<span data-ttu-id="53e69-605">W poniższej tabeli opisano opcje konfigurowania zaawansowanych opcji protokołu HTTP w programie ASP.NET Core Signal:</span><span class="sxs-lookup"><span data-stu-id="53e69-605">The following table describes options for configuring ASP.NET Core SignalR's advanced HTTP options:</span></span>

| <span data-ttu-id="53e69-606">Opcja</span><span class="sxs-lookup"><span data-stu-id="53e69-606">Option</span></span> | <span data-ttu-id="53e69-607">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="53e69-607">Default Value</span></span> | <span data-ttu-id="53e69-608">Opis</span><span class="sxs-lookup"><span data-stu-id="53e69-608">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="53e69-609">32 KB</span><span class="sxs-lookup"><span data-stu-id="53e69-609">32 KB</span></span> | <span data-ttu-id="53e69-610">Maksymalna liczba bajtów odebranych od klienta, które buforuje serwer.</span><span class="sxs-lookup"><span data-stu-id="53e69-610">The maximum number of bytes received from the client that the server buffers.</span></span> <span data-ttu-id="53e69-611">Zwiększenie tej wartości umożliwia serwerowi otrzymywanie większych komunikatów, ale może mieć negatywny wpływ na użycie pamięci.</span><span class="sxs-lookup"><span data-stu-id="53e69-611">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="53e69-612">Dane zbierane automatycznie z `Authorize` atrybuty zastosowane do klasy centrum.</span><span class="sxs-lookup"><span data-stu-id="53e69-612">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="53e69-613">Lista obiektów [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) służąca do określenia, czy klient jest autoryzowany do łączenia się z centrum.</span><span class="sxs-lookup"><span data-stu-id="53e69-613">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="53e69-614">32 KB</span><span class="sxs-lookup"><span data-stu-id="53e69-614">32 KB</span></span> | <span data-ttu-id="53e69-615">Maksymalna liczba bajtów wysłanych przez aplikację, które buforują serwer.</span><span class="sxs-lookup"><span data-stu-id="53e69-615">The maximum number of bytes sent by the app that the server buffers.</span></span> <span data-ttu-id="53e69-616">Zwiększenie tej wartości umożliwia serwerowi wysyłanie większych komunikatów, ale może mieć negatywny wpływ na użycie pamięci.</span><span class="sxs-lookup"><span data-stu-id="53e69-616">Increasing this value allows the server to send larger messages, but can negatively impact memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="53e69-617">Wszystkie transporty są włączone.</span><span class="sxs-lookup"><span data-stu-id="53e69-617">All Transports are enabled.</span></span> | <span data-ttu-id="53e69-618">Wyliczenie flag bitowych wartości `HttpTransportType`, które mogą ograniczyć transporty, z których może korzystać klient do nawiązywania połączeń.</span><span class="sxs-lookup"><span data-stu-id="53e69-618">A bit flags enum of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="53e69-619">Zobacz poniżej.</span><span class="sxs-lookup"><span data-stu-id="53e69-619">See below.</span></span> | <span data-ttu-id="53e69-620">Dodatkowe opcje specyficzne dla długiego transportu sondowania.</span><span class="sxs-lookup"><span data-stu-id="53e69-620">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="53e69-621">Zobacz poniżej.</span><span class="sxs-lookup"><span data-stu-id="53e69-621">See below.</span></span> | <span data-ttu-id="53e69-622">Dodatkowe opcje specyficzne dla transportu obiektów WebSockets.</span><span class="sxs-lookup"><span data-stu-id="53e69-622">Additional options specific to the WebSockets transport.</span></span> |

<span data-ttu-id="53e69-623">W przypadku długiego transportu sondowania dostępne są dodatkowe opcje, które można skonfigurować przy użyciu właściwości `LongPolling`:</span><span class="sxs-lookup"><span data-stu-id="53e69-623">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="53e69-624">Opcja</span><span class="sxs-lookup"><span data-stu-id="53e69-624">Option</span></span> | <span data-ttu-id="53e69-625">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="53e69-625">Default Value</span></span> | <span data-ttu-id="53e69-626">Opis</span><span class="sxs-lookup"><span data-stu-id="53e69-626">Description</span></span> |
| ------ | ------------- | ----------- |
| `PollTimeout` | <span data-ttu-id="53e69-627">90 sekund</span><span class="sxs-lookup"><span data-stu-id="53e69-627">90 seconds</span></span> | <span data-ttu-id="53e69-628">Maksymalny czas, przez jaki serwer czeka na wysłanie wiadomości do klienta przed zakończeniem jednego żądania sondowania.</span><span class="sxs-lookup"><span data-stu-id="53e69-628">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="53e69-629">Zmniejszenie tej wartości powoduje, że klient wystawia nowe żądania sondy częściej.</span><span class="sxs-lookup"><span data-stu-id="53e69-629">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> |

<span data-ttu-id="53e69-630">Transport WebSocket ma dodatkowe opcje, które można skonfigurować za pomocą właściwości `WebSockets`:</span><span class="sxs-lookup"><span data-stu-id="53e69-630">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="53e69-631">Opcja</span><span class="sxs-lookup"><span data-stu-id="53e69-631">Option</span></span> | <span data-ttu-id="53e69-632">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="53e69-632">Default Value</span></span> | <span data-ttu-id="53e69-633">Opis</span><span class="sxs-lookup"><span data-stu-id="53e69-633">Description</span></span> |
| ------ | ------------- | ----------- |
| `CloseTimeout` | <span data-ttu-id="53e69-634">5 sekund</span><span class="sxs-lookup"><span data-stu-id="53e69-634">5 seconds</span></span> | <span data-ttu-id="53e69-635">Gdy serwer zostanie zamknięty, jeśli klient nie zostanie zamknięty w tym okresie, połączenie zostanie przerwane.</span><span class="sxs-lookup"><span data-stu-id="53e69-635">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | `null` | <span data-ttu-id="53e69-636">Delegat, którego można użyć do ustawienia nagłówka `Sec-WebSocket-Protocol` na wartość niestandardową.</span><span class="sxs-lookup"><span data-stu-id="53e69-636">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="53e69-637">Delegat otrzymuje wartości żądane przez klienta jako dane wejściowe i oczekuje na zwrócenie żądanej wartości.</span><span class="sxs-lookup"><span data-stu-id="53e69-637">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="53e69-638">Konfigurowanie opcji klienta</span><span class="sxs-lookup"><span data-stu-id="53e69-638">Configure client options</span></span>

<span data-ttu-id="53e69-639">Opcje klienta można skonfigurować dla typu `HubConnectionBuilder` (dostępnego w klientach .NET i JavaScript).</span><span class="sxs-lookup"><span data-stu-id="53e69-639">Client options can be configured on the `HubConnectionBuilder` type (available in the .NET and JavaScript clients).</span></span> <span data-ttu-id="53e69-640">Jest ona również dostępna w kliencie Java, ale podklasa `HttpHubConnectionBuilder` zawiera opcje konfiguracji konstruktora, a także `HubConnection` samego siebie.</span><span class="sxs-lookup"><span data-stu-id="53e69-640">It's also available in the Java client, but the `HttpHubConnectionBuilder` subclass is what contains the builder configuration options, as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="53e69-641">Konfigurowanie rejestrowania</span><span class="sxs-lookup"><span data-stu-id="53e69-641">Configure logging</span></span>

<span data-ttu-id="53e69-642">Rejestrowanie jest konfigurowane w kliencie .NET przy użyciu metody `ConfigureLogging`.</span><span class="sxs-lookup"><span data-stu-id="53e69-642">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="53e69-643">Dostawcy i filtry rejestrowania mogą być rejestrowane w taki sam sposób, w jaki znajdują się na serwerze.</span><span class="sxs-lookup"><span data-stu-id="53e69-643">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="53e69-644">Aby uzyskać więcej informacji, zobacz dokumentację dotyczącą [rejestrowania w ASP.NET Core](xref:fundamentals/logging/index) .</span><span class="sxs-lookup"><span data-stu-id="53e69-644">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="53e69-645">Aby zarejestrować dostawców rejestrowania, należy zainstalować wymagane pakiety.</span><span class="sxs-lookup"><span data-stu-id="53e69-645">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="53e69-646">Aby zapoznać się z pełną listą, zobacz sekcję [wbudowane dostawcy rejestrowania](xref:fundamentals/logging/index#built-in-logging-providers) w witrynie docs.</span><span class="sxs-lookup"><span data-stu-id="53e69-646">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="53e69-647">Aby na przykład włączyć rejestrowanie konsoli, należy zainstalować pakiet NuGet `Microsoft.Extensions.Logging.Console`.</span><span class="sxs-lookup"><span data-stu-id="53e69-647">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="53e69-648">Wywołaj metodę rozszerzenia `AddConsole`:</span><span class="sxs-lookup"><span data-stu-id="53e69-648">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="53e69-649">W kliencie JavaScript istnieje podobna Metoda `configureLogging`.</span><span class="sxs-lookup"><span data-stu-id="53e69-649">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="53e69-650">Podaj wartość `LogLevel` wskazującą minimalny poziom komunikatów dziennika do wygenerowania.</span><span class="sxs-lookup"><span data-stu-id="53e69-650">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="53e69-651">Dzienniki są zapisywane w oknie konsoli przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="53e69-651">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

> [!NOTE]
> <span data-ttu-id="53e69-652">Aby całkowicie wyłączyć rejestrowanie, określ `signalR.LogLevel.None` w `configureLogging` metodzie.</span><span class="sxs-lookup"><span data-stu-id="53e69-652">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="53e69-653">Aby uzyskać więcej informacji na temat rejestrowania, zobacz [dokumentację diagnostyki sygnału](xref:signalr/diagnostics).</span><span class="sxs-lookup"><span data-stu-id="53e69-653">For more information on logging, see the [SignalR Diagnostics documentation](xref:signalr/diagnostics).</span></span>

<span data-ttu-id="53e69-654">Klient Java sygnalizujący używa biblioteki [SLF4J](https://www.slf4j.org/) do rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="53e69-654">The SignalR Java client uses the [SLF4J](https://www.slf4j.org/) library for logging.</span></span> <span data-ttu-id="53e69-655">Jest to interfejs API rejestrowania wysokiego poziomu, który umożliwia użytkownikom biblioteki wybranie własnych implementacji rejestrowania przez nałożenie określonej zależności rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="53e69-655">It's a high-level logging API that allows users of the library to chose their own specific logging implementation by bringing in a specific logging dependency.</span></span> <span data-ttu-id="53e69-656">Poniższy fragment kodu przedstawia sposób użycia `java.util.logging` z klientem języka Java sygnalizującego.</span><span class="sxs-lookup"><span data-stu-id="53e69-656">The following code snippet shows how to use `java.util.logging` with the SignalR Java client.</span></span>

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

<span data-ttu-id="53e69-657">Jeśli nie skonfigurujesz rejestrowania w zależnościach, SLF4J ładuje domyślny Rejestrator braku operacji z następującym komunikatem ostrzegawczym:</span><span class="sxs-lookup"><span data-stu-id="53e69-657">If you don't configure logging in your dependencies, SLF4J loads a default no-operation logger with the following warning message:</span></span>

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

<span data-ttu-id="53e69-658">Można je bezpiecznie zignorować.</span><span class="sxs-lookup"><span data-stu-id="53e69-658">This can safely be ignored.</span></span>

### <a name="configure-allowed-transports"></a><span data-ttu-id="53e69-659">Skonfiguruj dozwolone transporty</span><span class="sxs-lookup"><span data-stu-id="53e69-659">Configure allowed transports</span></span>

<span data-ttu-id="53e69-660">Transporty używane przez sygnalizującego można skonfigurować w wywołaniu `WithUrl` (`withUrl` w języku JavaScript).</span><span class="sxs-lookup"><span data-stu-id="53e69-660">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="53e69-661">Wartości `HttpTransportType` mogą być używane w celu ograniczenia klienta do używania tylko określonych transportów.</span><span class="sxs-lookup"><span data-stu-id="53e69-661">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="53e69-662">Wszystkie transporty są domyślnie włączone.</span><span class="sxs-lookup"><span data-stu-id="53e69-662">All transports are enabled by default.</span></span>

<span data-ttu-id="53e69-663">Na przykład, aby wyłączyć transport zdarzeń wysłanych przez serwer, ale zezwolić na połączenia z usługą WebSockets i długie sondowanie:</span><span class="sxs-lookup"><span data-stu-id="53e69-663">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="53e69-664">W kliencie JavaScript transporty są konfigurowane przez ustawienie pola `transport` w obiekcie Options udostępnianym `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="53e69-664">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

### <a name="configure-bearer-authentication"></a><span data-ttu-id="53e69-665">Konfigurowanie uwierzytelniania okaziciela</span><span class="sxs-lookup"><span data-stu-id="53e69-665">Configure bearer authentication</span></span>

<span data-ttu-id="53e69-666">Aby zapewnić dane uwierzytelniania wraz z żądaniami sygnałów, użyj opcji `AccessTokenProvider` (`accessTokenFactory` w języku JavaScript), aby określić funkcję, która zwraca żądany token dostępu.</span><span class="sxs-lookup"><span data-stu-id="53e69-666">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="53e69-667">W kliencie .NET token dostępu jest przenoszona jako token "uwierzytelnianie okaziciela" protokołu HTTP (przy użyciu nagłówka `Authorization` z typem `Bearer`).</span><span class="sxs-lookup"><span data-stu-id="53e69-667">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="53e69-668">W kliencie JavaScript token dostępu jest używany jako token okaziciela, **z wyjątkiem** kilku przypadków, w których interfejsy API przeglądarki ograniczają możliwość zastosowania nagłówków (w konkretnym przypadku zdarzeń wysyłanych przez serwer i żądań WebSockets).</span><span class="sxs-lookup"><span data-stu-id="53e69-668">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="53e69-669">W takich przypadkach token dostępu jest dostarczany jako wartość ciągu zapytania `access_token`.</span><span class="sxs-lookup"><span data-stu-id="53e69-669">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="53e69-670">W kliencie .NET opcja `AccessTokenProvider` można określić za pomocą delegata opcji w `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="53e69-670">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

<span data-ttu-id="53e69-671">W kliencie JavaScript token dostępu jest konfigurowany przez ustawienie pola `accessTokenFactory` w obiekcie Options w `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="53e69-671">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

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

<span data-ttu-id="53e69-672">W kliencie Java programu sygnalizującego można skonfigurować token okaziciela do użycia na potrzeby uwierzytelniania, dostarczając fabrykę tokenów dostępu do [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span><span class="sxs-lookup"><span data-stu-id="53e69-672">In the SignalR Java client, you can configure a bearer token to use for authentication by providing an access token factory to the [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span></span> <span data-ttu-id="53e69-673">Użyj [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) , aby podać [](https://github.com/ReactiveX/RxJava) [>\<pojedynczego ciągu](https://reactivex.io/documentation/single.html).</span><span class="sxs-lookup"><span data-stu-id="53e69-673">Use [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) to provide an [RxJava](https://github.com/ReactiveX/RxJava) [Single\<String>](https://reactivex.io/documentation/single.html).</span></span> <span data-ttu-id="53e69-674">Wywołanie metody [Single. Ustąp](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-)umożliwia zapisanie logiki w celu utworzenia tokenów dostępu dla klienta.</span><span class="sxs-lookup"><span data-stu-id="53e69-674">With a call to [Single.defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), you can write logic to produce access tokens for your client.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="53e69-675">Konfigurowanie opcji limitu czasu i utrzymywania aktywności</span><span class="sxs-lookup"><span data-stu-id="53e69-675">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="53e69-676">W celu skonfigurowania zachowania przekroczenia limitu czasu i utrzymywania aktywności są dostępne dodatkowe opcje dla samego obiektu `HubConnection`:</span><span class="sxs-lookup"><span data-stu-id="53e69-676">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>

# <a name="net"></a>[<span data-ttu-id="53e69-677">.NET</span><span class="sxs-lookup"><span data-stu-id="53e69-677">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="53e69-678">Opcja</span><span class="sxs-lookup"><span data-stu-id="53e69-678">Option</span></span> | <span data-ttu-id="53e69-679">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="53e69-679">Default value</span></span> | <span data-ttu-id="53e69-680">Opis</span><span class="sxs-lookup"><span data-stu-id="53e69-680">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="53e69-681">30 sekund (30 000 MS)</span><span class="sxs-lookup"><span data-stu-id="53e69-681">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="53e69-682">Limit czasu dla działania serwera.</span><span class="sxs-lookup"><span data-stu-id="53e69-682">Timeout for server activity.</span></span> <span data-ttu-id="53e69-683">Jeśli serwer nie wysłał komunikatu w tym interwale, Klient uważa, że serwer odłączył się i wyzwala zdarzenie `Closed` (`onclose` w języku JavaScript).</span><span class="sxs-lookup"><span data-stu-id="53e69-683">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="53e69-684">Ta wartość musi być wystarczająca do wysłania wiadomości ping z serwera **i** odebrana przez klienta w ramach przedziału czasu.</span><span class="sxs-lookup"><span data-stu-id="53e69-684">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="53e69-685">Zalecana wartość to liczba z co najmniej podwójną wartością `KeepAliveInterval` serwera, aby umożliwić czas na nadejście poleceń ping.</span><span class="sxs-lookup"><span data-stu-id="53e69-685">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="53e69-686">15 sekund</span><span class="sxs-lookup"><span data-stu-id="53e69-686">15 seconds</span></span> | <span data-ttu-id="53e69-687">Limit czasu początkowej uzgadniania z serwerem.</span><span class="sxs-lookup"><span data-stu-id="53e69-687">Timeout for initial server handshake.</span></span> <span data-ttu-id="53e69-688">Jeśli serwer nie wyśle odpowiedzi uzgadniania w tym interwale, klient anuluje uzgadnianie i wyzwala zdarzenie `Closed` (`onclose` w języku JavaScript).</span><span class="sxs-lookup"><span data-stu-id="53e69-688">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="53e69-689">Jest to ustawienie zaawansowane, które należy zmodyfikować tylko w przypadku wystąpienia błędów limitu czasu uzgadniania z powodu poważnego opóźnienia sieci.</span><span class="sxs-lookup"><span data-stu-id="53e69-689">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="53e69-690">Aby uzyskać więcej szczegółów na temat procesu uzgadniania, zobacz [specyfikację protokołu centrum sygnału](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="53e69-690">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

<span data-ttu-id="53e69-691">W kliencie .NET wartości limitu czasu są określane jako wartości `TimeSpan`.</span><span class="sxs-lookup"><span data-stu-id="53e69-691">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascript"></a>[<span data-ttu-id="53e69-692">JavaScript</span><span class="sxs-lookup"><span data-stu-id="53e69-692">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="53e69-693">Opcja</span><span class="sxs-lookup"><span data-stu-id="53e69-693">Option</span></span> | <span data-ttu-id="53e69-694">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="53e69-694">Default value</span></span> | <span data-ttu-id="53e69-695">Opis</span><span class="sxs-lookup"><span data-stu-id="53e69-695">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="53e69-696">30 sekund (30 000 MS)</span><span class="sxs-lookup"><span data-stu-id="53e69-696">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="53e69-697">Limit czasu dla działania serwera.</span><span class="sxs-lookup"><span data-stu-id="53e69-697">Timeout for server activity.</span></span> <span data-ttu-id="53e69-698">Jeśli serwer nie wysłał komunikatu w tym interwale, Klient uważa, że serwer odłączył się i wyzwala zdarzenie `onclose`.</span><span class="sxs-lookup"><span data-stu-id="53e69-698">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="53e69-699">Ta wartość musi być wystarczająca do wysłania wiadomości ping z serwera **i** odebrana przez klienta w ramach przedziału czasu.</span><span class="sxs-lookup"><span data-stu-id="53e69-699">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="53e69-700">Zalecana wartość to liczba z co najmniej podwójną wartością `KeepAliveInterval` serwera, aby umożliwić czas na nadejście poleceń ping.</span><span class="sxs-lookup"><span data-stu-id="53e69-700">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |

# <a name="java"></a>[<span data-ttu-id="53e69-701">Java</span><span class="sxs-lookup"><span data-stu-id="53e69-701">Java</span></span>](#tab/java)

| <span data-ttu-id="53e69-702">Opcja</span><span class="sxs-lookup"><span data-stu-id="53e69-702">Option</span></span> | <span data-ttu-id="53e69-703">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="53e69-703">Default value</span></span> | <span data-ttu-id="53e69-704">Opis</span><span class="sxs-lookup"><span data-stu-id="53e69-704">Description</span></span> |
| ------ | ------------- | ----------- |
| <span data-ttu-id="53e69-705">`getServerTimeout` / `setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="53e69-705">`getServerTimeout` / `setServerTimeout`</span></span> | <span data-ttu-id="53e69-706">30 sekund (30 000 MS)</span><span class="sxs-lookup"><span data-stu-id="53e69-706">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="53e69-707">Limit czasu dla działania serwera.</span><span class="sxs-lookup"><span data-stu-id="53e69-707">Timeout for server activity.</span></span> <span data-ttu-id="53e69-708">Jeśli serwer nie wysłał komunikatu w tym interwale, Klient uważa, że serwer odłączył się i wyzwala zdarzenie `onClose`.</span><span class="sxs-lookup"><span data-stu-id="53e69-708">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="53e69-709">Ta wartość musi być wystarczająca do wysłania wiadomości ping z serwera **i** odebrana przez klienta w ramach przedziału czasu.</span><span class="sxs-lookup"><span data-stu-id="53e69-709">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="53e69-710">Zalecana wartość to liczba z co najmniej podwójną wartością `KeepAliveInterval` serwera, aby umożliwić czas na nadejście poleceń ping.</span><span class="sxs-lookup"><span data-stu-id="53e69-710">The recommended value is a number at least double the server's `KeepAliveInterval` value, to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="53e69-711">15 sekund</span><span class="sxs-lookup"><span data-stu-id="53e69-711">15 seconds</span></span> | <span data-ttu-id="53e69-712">Limit czasu początkowej uzgadniania z serwerem.</span><span class="sxs-lookup"><span data-stu-id="53e69-712">Timeout for initial server handshake.</span></span> <span data-ttu-id="53e69-713">Jeśli serwer nie wyśle odpowiedzi uzgadniania w tym interwale, klient anuluje uzgadnianie i wyzwala zdarzenie `onClose`.</span><span class="sxs-lookup"><span data-stu-id="53e69-713">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="53e69-714">Jest to ustawienie zaawansowane, które należy zmodyfikować tylko w przypadku wystąpienia błędów limitu czasu uzgadniania z powodu poważnego opóźnienia sieci.</span><span class="sxs-lookup"><span data-stu-id="53e69-714">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="53e69-715">Aby uzyskać więcej szczegółów na temat procesu uzgadniania, zobacz [specyfikację protokołu centrum sygnału](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="53e69-715">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

---

### <a name="configure-additional-options"></a><span data-ttu-id="53e69-716">Skonfiguruj dodatkowe opcje</span><span class="sxs-lookup"><span data-stu-id="53e69-716">Configure additional options</span></span>

<span data-ttu-id="53e69-717">Dodatkowe opcje można skonfigurować w metodzie `WithUrl` (`withUrl` w języku JavaScript) na `HubConnectionBuilder` lub w różnych interfejsach API konfiguracji na `HttpHubConnectionBuilder` klienta Java:</span><span class="sxs-lookup"><span data-stu-id="53e69-717">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder` or on the various configuration APIs on the `HttpHubConnectionBuilder` in the Java client:</span></span>

# <a name="net"></a>[<span data-ttu-id="53e69-718">.NET</span><span class="sxs-lookup"><span data-stu-id="53e69-718">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="53e69-719">Opcja .NET</span><span class="sxs-lookup"><span data-stu-id="53e69-719">.NET Option</span></span> |  <span data-ttu-id="53e69-720">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="53e69-720">Default value</span></span> | <span data-ttu-id="53e69-721">Opis</span><span class="sxs-lookup"><span data-stu-id="53e69-721">Description</span></span> |
| ----------- | -------------- | ----------- |
| `AccessTokenProvider` | `null` | <span data-ttu-id="53e69-722">Funkcja zwracająca ciąg, który jest dostarczany jako token uwierzytelniania okaziciela w żądaniach HTTP.</span><span class="sxs-lookup"><span data-stu-id="53e69-722">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `false` | <span data-ttu-id="53e69-723">Ustaw tę wartość na `true`, aby pominąć etap negocjacji.</span><span class="sxs-lookup"><span data-stu-id="53e69-723">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="53e69-724">**Obsługiwane tylko wtedy, gdy transport Sockets jest jedynym włączonym transportem**.</span><span class="sxs-lookup"><span data-stu-id="53e69-724">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="53e69-725">Nie można włączyć tego ustawienia w przypadku korzystania z usługi Azure Signal Service.</span><span class="sxs-lookup"><span data-stu-id="53e69-725">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="53e69-726">Pusty</span><span class="sxs-lookup"><span data-stu-id="53e69-726">Empty</span></span> | <span data-ttu-id="53e69-727">Kolekcja certyfikatów TLS do wysłania w celu uwierzytelniania żądań.</span><span class="sxs-lookup"><span data-stu-id="53e69-727">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="53e69-728">Pusty</span><span class="sxs-lookup"><span data-stu-id="53e69-728">Empty</span></span> | <span data-ttu-id="53e69-729">Kolekcja plików cookie protokołu HTTP do wysłania w każdym żądaniu HTTP.</span><span class="sxs-lookup"><span data-stu-id="53e69-729">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="53e69-730">Pusty</span><span class="sxs-lookup"><span data-stu-id="53e69-730">Empty</span></span> | <span data-ttu-id="53e69-731">Poświadczenia do wysyłania przy każdym żądaniu HTTP.</span><span class="sxs-lookup"><span data-stu-id="53e69-731">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="53e69-732">5 sekund</span><span class="sxs-lookup"><span data-stu-id="53e69-732">5 seconds</span></span> | <span data-ttu-id="53e69-733">Tylko obiekty WebSockets.</span><span class="sxs-lookup"><span data-stu-id="53e69-733">WebSockets only.</span></span> <span data-ttu-id="53e69-734">Maksymalny czas oczekiwania klienta po zamknięciu serwera w celu potwierdzenia żądania zamknięcia.</span><span class="sxs-lookup"><span data-stu-id="53e69-734">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="53e69-735">Jeśli serwer nie potwierdzi zamknięcia w tym czasie, klient zostanie rozłączony.</span><span class="sxs-lookup"><span data-stu-id="53e69-735">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="53e69-736">Pusty</span><span class="sxs-lookup"><span data-stu-id="53e69-736">Empty</span></span> | <span data-ttu-id="53e69-737">Mapa dodatkowych nagłówków HTTP do wysłania w każdym żądaniu HTTP.</span><span class="sxs-lookup"><span data-stu-id="53e69-737">A Map of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | `null` | <span data-ttu-id="53e69-738">Delegat, który może służyć do konfigurowania lub zastępowania `HttpMessageHandler` używany do wysyłania żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="53e69-738">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="53e69-739">Nieużywane na potrzeby połączeń protokołu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="53e69-739">Not used for WebSocket connections.</span></span> <span data-ttu-id="53e69-740">Ten delegat musi zwracać wartość różną od null i otrzymuje wartość domyślną jako parametr.</span><span class="sxs-lookup"><span data-stu-id="53e69-740">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="53e69-741">Zmodyfikuj ustawienia tej wartości domyślnej i zwróć ją lub Zwróć nowe wystąpienie `HttpMessageHandler`.</span><span class="sxs-lookup"><span data-stu-id="53e69-741">Either modify settings on that default value and return it, or return a new `HttpMessageHandler` instance.</span></span> <span data-ttu-id="53e69-742">**Podczas zastępowania programu obsługi należy skopiować ustawienia, które mają być zachowane z podanej procedury obsługi. w przeciwnym razie skonfigurowane opcje (takie jak pliki cookie i nagłówki) nie będą miały zastosowania do nowego programu obsługi.**</span><span class="sxs-lookup"><span data-stu-id="53e69-742">**When replacing the handler make sure to copy the settings you want to keep from the provided handler, otherwise, the configured options (such as Cookies and Headers) won't apply to the new handler.**</span></span> |
| `Proxy` | `null` | <span data-ttu-id="53e69-743">Serwer proxy HTTP, który ma być używany podczas wysyłania żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="53e69-743">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | `false` | <span data-ttu-id="53e69-744">Ustaw tę wartość logiczną, aby wysyłać domyślne poświadczenia dla żądań HTTP i WebSockets.</span><span class="sxs-lookup"><span data-stu-id="53e69-744">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="53e69-745">Umożliwia to korzystanie z uwierzytelniania systemu Windows.</span><span class="sxs-lookup"><span data-stu-id="53e69-745">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | `null` | <span data-ttu-id="53e69-746">Delegat, który może służyć do konfigurowania dodatkowych opcji protokołu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="53e69-746">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="53e69-747">Odbiera wystąpienie elementu [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) , którego można użyć do skonfigurowania opcji.</span><span class="sxs-lookup"><span data-stu-id="53e69-747">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

# <a name="javascript"></a>[<span data-ttu-id="53e69-748">JavaScript</span><span class="sxs-lookup"><span data-stu-id="53e69-748">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="53e69-749">Opcja języka JavaScript</span><span class="sxs-lookup"><span data-stu-id="53e69-749">JavaScript Option</span></span> | <span data-ttu-id="53e69-750">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="53e69-750">Default Value</span></span> | <span data-ttu-id="53e69-751">Opis</span><span class="sxs-lookup"><span data-stu-id="53e69-751">Description</span></span> |
| ----------------- | ------------- | ----------- |
| `accessTokenFactory` | `null` | <span data-ttu-id="53e69-752">Funkcja zwracająca ciąg, który jest dostarczany jako token uwierzytelniania okaziciela w żądaniach HTTP.</span><span class="sxs-lookup"><span data-stu-id="53e69-752">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `skipNegotiation` | `false` | <span data-ttu-id="53e69-753">Ustaw tę wartość na `true`, aby pominąć etap negocjacji.</span><span class="sxs-lookup"><span data-stu-id="53e69-753">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="53e69-754">**Obsługiwane tylko wtedy, gdy transport Sockets jest jedynym włączonym transportem**.</span><span class="sxs-lookup"><span data-stu-id="53e69-754">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="53e69-755">Nie można włączyć tego ustawienia w przypadku korzystania z usługi Azure Signal Service.</span><span class="sxs-lookup"><span data-stu-id="53e69-755">This setting can't be enabled when using the Azure SignalR Service.</span></span> |

# <a name="java"></a>[<span data-ttu-id="53e69-756">Java</span><span class="sxs-lookup"><span data-stu-id="53e69-756">Java</span></span>](#tab/java)

| <span data-ttu-id="53e69-757">Opcja Java</span><span class="sxs-lookup"><span data-stu-id="53e69-757">Java Option</span></span> | <span data-ttu-id="53e69-758">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="53e69-758">Default Value</span></span> | <span data-ttu-id="53e69-759">Opis</span><span class="sxs-lookup"><span data-stu-id="53e69-759">Description</span></span> |
| ----------- | ------------- | ----------- |
| `withAccessTokenProvider` | `null` | <span data-ttu-id="53e69-760">Funkcja zwracająca ciąg, który jest dostarczany jako token uwierzytelniania okaziciela w żądaniach HTTP.</span><span class="sxs-lookup"><span data-stu-id="53e69-760">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `shouldSkipNegotiate` | `false` | <span data-ttu-id="53e69-761">Ustaw tę wartość na `true`, aby pominąć etap negocjacji.</span><span class="sxs-lookup"><span data-stu-id="53e69-761">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="53e69-762">**Obsługiwane tylko wtedy, gdy transport Sockets jest jedynym włączonym transportem**.</span><span class="sxs-lookup"><span data-stu-id="53e69-762">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="53e69-763">Nie można włączyć tego ustawienia w przypadku korzystania z usługi Azure Signal Service.</span><span class="sxs-lookup"><span data-stu-id="53e69-763">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| <span data-ttu-id="53e69-764">`withHeader` `withHeaders`</span><span class="sxs-lookup"><span data-stu-id="53e69-764">`withHeader` `withHeaders`</span></span> | <span data-ttu-id="53e69-765">Pusty</span><span class="sxs-lookup"><span data-stu-id="53e69-765">Empty</span></span> | <span data-ttu-id="53e69-766">Mapa dodatkowych nagłówków HTTP do wysłania w każdym żądaniu HTTP.</span><span class="sxs-lookup"><span data-stu-id="53e69-766">A Map of additional HTTP headers to send with every HTTP request.</span></span> |

---

<span data-ttu-id="53e69-767">W kliencie .NET opcje te można modyfikować za pomocą delegata opcji udostępnianego do `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="53e69-767">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="53e69-768">W kliencie JavaScript te opcje można podać w obiekcie JavaScript udostępnionym do `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="53e69-768">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

<span data-ttu-id="53e69-769">W kliencie Java Opcje te można skonfigurować przy użyciu metod na `HttpHubConnectionBuilder` zwracanych z `HubConnectionBuilder.create("HUB URL")`</span><span class="sxs-lookup"><span data-stu-id="53e69-769">In the Java client, these options can be configured with the methods on the `HttpHubConnectionBuilder` returned from the `HubConnectionBuilder.create("HUB URL")`</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
        .withHeader("Foo", "Bar")
        .shouldSkipNegotiate(true)
        .withHandshakeResponseTimeout(30*1000)
        .build();
```

## <a name="additional-resources"></a><span data-ttu-id="53e69-770">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="53e69-770">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>

::: moniker-end