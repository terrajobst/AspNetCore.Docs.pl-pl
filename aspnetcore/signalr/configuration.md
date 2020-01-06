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
ms.sourcegitcommit: 2cb857f0de774df421e35289662ba92cfe56ffd1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/25/2019
ms.locfileid: "75358115"
---
# <a name="aspnet-core-opno-locsignalr-configuration"></a><span data-ttu-id="3b5f4-103">Konfiguracja SignalR ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3b5f4-103">ASP.NET Core SignalR configuration</span></span>

::: moniker range=">= aspnetcore-3.0"

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="3b5f4-104">Opcje serializacji JSON/MessagePack</span><span class="sxs-lookup"><span data-stu-id="3b5f4-104">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="3b5f4-105">ASP.NET Core SignalR obsługuje dwa protokoły dla komunikatów kodowania: [JSON](https://www.json.org/) i [MessagePack](https://msgpack.org/index.html).</span><span class="sxs-lookup"><span data-stu-id="3b5f4-105">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="3b5f4-106">Każdy protokół ma opcje konfiguracji serializacji.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-106">Each protocol has serialization configuration options.</span></span>

<span data-ttu-id="3b5f4-107">Serializacji JSON można skonfigurować na serwerze przy użyciu metody rozszerzenia [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) .</span><span class="sxs-lookup"><span data-stu-id="3b5f4-107">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method.</span></span> <span data-ttu-id="3b5f4-108">`AddJsonProtocol` można dodać po elemencie [Addsignaler](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-108">`AddJsonProtocol` can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="3b5f4-109">Metoda `AddJsonProtocol` przyjmuje delegata, który odbiera obiekt `options`.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-109">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="3b5f4-110">Właściwość [PayloadSerializerOptions](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializeroptions) tego obiektu jest obiektem <xref:System.Text.Json.JsonSerializerOptions> `System.Text.Json`, którego można użyć do skonfigurowania serializacji argumentów i zwracanych wartości.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-110">The [PayloadSerializerOptions](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializeroptions) property on that object is a `System.Text.Json` <xref:System.Text.Json.JsonSerializerOptions> object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="3b5f4-111">Aby uzyskać więcej informacji, zobacz [dokumentację system. Text. JSON](/dotnet/api/system.text.json).</span><span class="sxs-lookup"><span data-stu-id="3b5f4-111">For more information, see the [System.Text.Json documentation](/dotnet/api/system.text.json).</span></span>

<span data-ttu-id="3b5f4-112">Przykładowo, aby skonfigurować Serializator, aby nie zmieniać wielkości liter nazw właściwości zamiast domyślnych nazw "camelCase", należy użyć następującego kodu w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="3b5f4-112">As an example, to configure the serializer to not change the casing of property names, instead of the default "camelCase" names, use the following code in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerOptions.PropertyNamingPolicy = null
    });
```

<span data-ttu-id="3b5f4-113">W kliencie .NET ta sama Metoda rozszerzenia `AddJsonProtocol` istnieje w [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="3b5f4-113">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="3b5f4-114">Aby rozwiązać metodę rozszerzenia, należy zaimportować przestrzeń nazw `Microsoft.Extensions.DependencyInjection`:</span><span class="sxs-lookup"><span data-stu-id="3b5f4-114">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

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
> <span data-ttu-id="3b5f4-115">W tej chwili nie można skonfigurować serializacji JSON w kliencie JavaScript.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-115">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="switch-to-newtonsoftjson"></a><span data-ttu-id="3b5f4-116">Przejdź do pliku Newtonsoft. JSON</span><span class="sxs-lookup"><span data-stu-id="3b5f4-116">Switch to Newtonsoft.Json</span></span>

<span data-ttu-id="3b5f4-117">Jeśli potrzebujesz funkcji `Newtonsoft.Json`, które nie są obsługiwane w `System.Text.Json`, zobacz [Switch to Newtonsoft. JSON](xref:migration/22-to-30#switch-to-newtonsoftjson).</span><span class="sxs-lookup"><span data-stu-id="3b5f4-117">If you need features of `Newtonsoft.Json` that aren't supported in `System.Text.Json`, See [Switch to Newtonsoft.Json](xref:migration/22-to-30#switch-to-newtonsoftjson).</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="3b5f4-118">Opcje serializacji MessagePack</span><span class="sxs-lookup"><span data-stu-id="3b5f4-118">MessagePack serialization options</span></span>

<span data-ttu-id="3b5f4-119">Serializacji MessagePack można skonfigurować, dostarczając delegata do wywołania [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) .</span><span class="sxs-lookup"><span data-stu-id="3b5f4-119">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="3b5f4-120">Aby uzyskać więcej informacji, zobacz [MessagePack w SignalR](xref:signalr/messagepackhubprotocol) .</span><span class="sxs-lookup"><span data-stu-id="3b5f4-120">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="3b5f4-121">W tej chwili nie można skonfigurować serializacji MessagePack w kliencie JavaScript.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-121">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="3b5f4-122">Konfigurowanie opcji serwera</span><span class="sxs-lookup"><span data-stu-id="3b5f4-122">Configure server options</span></span>

<span data-ttu-id="3b5f4-123">W poniższej tabeli opisano opcje konfigurowania centrów SignalR:</span><span class="sxs-lookup"><span data-stu-id="3b5f4-123">The following table describes options for configuring SignalR hubs:</span></span>

| <span data-ttu-id="3b5f4-124">Opcja</span><span class="sxs-lookup"><span data-stu-id="3b5f4-124">Option</span></span> | <span data-ttu-id="3b5f4-125">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="3b5f4-125">Default Value</span></span> | <span data-ttu-id="3b5f4-126">Opis</span><span class="sxs-lookup"><span data-stu-id="3b5f4-126">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="3b5f4-127">30 sekund</span><span class="sxs-lookup"><span data-stu-id="3b5f4-127">30 seconds</span></span> | <span data-ttu-id="3b5f4-128">Serwer Rozważ odłączenie klienta, jeśli nie odebrano komunikatu (w tym w tym interwale).</span><span class="sxs-lookup"><span data-stu-id="3b5f4-128">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="3b5f4-129">Może upłynąć dłużej niż ten interwał limitu czasu, aby klient mógł zostać oznaczony jako odłączony, ze względu na to, jak jest to zaimplementowane.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-129">It could take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="3b5f4-130">Zalecaną wartością jest podwójna wartość `KeepAliveInterval`.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-130">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="3b5f4-131">15 sekund</span><span class="sxs-lookup"><span data-stu-id="3b5f4-131">15 seconds</span></span> | <span data-ttu-id="3b5f4-132">Jeśli klient nie wyśle początkowego komunikatu uzgadniania w tym przedziale czasu, połączenie zostanie zamknięte.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-132">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="3b5f4-133">Jest to ustawienie zaawansowane, które należy zmodyfikować tylko w przypadku wystąpienia błędów limitu czasu uzgadniania z powodu poważnego opóźnienia sieci.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-133">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="3b5f4-134">Aby uzyskać więcej szczegółów na temat procesu uzgadniania, zobacz [Specyfikacja protokołuSignalR Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="3b5f4-134">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="3b5f4-135">15 sekund</span><span class="sxs-lookup"><span data-stu-id="3b5f4-135">15 seconds</span></span> | <span data-ttu-id="3b5f4-136">Jeśli serwer nie wysłał komunikatu w tym interwale, zostanie automatycznie wysłana wiadomość ping w celu pozostawienia otwartego połączenia.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-136">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="3b5f4-137">Podczas zmiany `KeepAliveInterval`Zmień ustawienie `ServerTimeout`/`serverTimeoutInMilliseconds` na kliencie.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-137">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="3b5f4-138">Zalecana wartość `ServerTimeout`/`serverTimeoutInMilliseconds` jest podwójna `KeepAliveInterval` wartość.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-138">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="3b5f4-139">Wszystkie zainstalowane protokoły</span><span class="sxs-lookup"><span data-stu-id="3b5f4-139">All installed protocols</span></span> | <span data-ttu-id="3b5f4-140">Protokoły obsługiwane przez to centrum.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-140">Protocols supported by this hub.</span></span> <span data-ttu-id="3b5f4-141">Domyślnie wszystkie protokoły zarejestrowane na serwerze są dozwolone, ale protokoły można usunąć z tej listy, aby wyłączyć określone protokoły dla poszczególnych centrów.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-141">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="3b5f4-142">Jeśli `true`, szczegółowe komunikaty o wyjątkach są zwracane do klientów, gdy wyjątek jest zgłaszany w metodzie centrum.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-142">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="3b5f4-143">Wartość domyślna to `false`, ponieważ te komunikaty o wyjątkach mogą zawierać informacje poufne.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-143">The default is `false`, as these exception messages can contain sensitive information.</span></span> |
| `StreamBufferCapacity` | `10` | <span data-ttu-id="3b5f4-144">Maksymalna liczba elementów, które mogą być buforowane dla strumieni przekazywania klientów.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-144">The maximum number of items that can be buffered for client upload streams.</span></span> <span data-ttu-id="3b5f4-145">Po osiągnięciu tego limitu przetwarzanie wywołań jest blokowane, dopóki serwer nie przetwarza elementów strumienia.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-145">If this limit is reached, the processing of invocations is blocked until the server processes stream items.</span></span>|
| `MaximumReceiveMessageSize` | <span data-ttu-id="3b5f4-146">32 KB</span><span class="sxs-lookup"><span data-stu-id="3b5f4-146">32 KB</span></span> | <span data-ttu-id="3b5f4-147">Maksymalny rozmiar pojedynczego przychodzącego komunikatu centrum.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-147">Maximum size of a single incoming hub message.</span></span> |

<span data-ttu-id="3b5f4-148">Opcje można skonfigurować dla wszystkich centrów, dostarczając opcje delegata do wywołania `AddSignalR` w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-148">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

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

<span data-ttu-id="3b5f4-149">Opcje jednego centrum przesłaniają opcje globalne dostępne w `AddSignalR` i można je skonfigurować za pomocą <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span><span class="sxs-lookup"><span data-stu-id="3b5f4-149">Options for a single hub override the global options provided in `AddSignalR` and can be configured using <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

### <a name="advanced-http-configuration-options"></a><span data-ttu-id="3b5f4-150">Zaawansowane opcje konfiguracji HTTP</span><span class="sxs-lookup"><span data-stu-id="3b5f4-150">Advanced HTTP configuration options</span></span>

<span data-ttu-id="3b5f4-151">Użyj `HttpConnectionDispatcherOptions`, aby skonfigurować zaawansowane ustawienia związane z transportami i zarządzaniem buforem pamięci.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-151">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="3b5f4-152">Te opcje są konfigurowane przez przekazanie delegata do [MapHub\<t >](/dotnet/api/microsoft.aspnetcore.builder.hubendpointroutebuilderextensions.maphub) w `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-152">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.builder.hubendpointroutebuilderextensions.maphub) in `Startup.Configure`.</span></span>

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

<span data-ttu-id="3b5f4-153">W poniższej tabeli opisano opcje konfigurowania zaawansowanych opcji protokołu HTTP w programie ASP.NET Core SignalR:</span><span class="sxs-lookup"><span data-stu-id="3b5f4-153">The following table describes options for configuring ASP.NET Core SignalR's advanced HTTP options:</span></span>

| <span data-ttu-id="3b5f4-154">Opcja</span><span class="sxs-lookup"><span data-stu-id="3b5f4-154">Option</span></span> | <span data-ttu-id="3b5f4-155">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="3b5f4-155">Default Value</span></span> | <span data-ttu-id="3b5f4-156">Opis</span><span class="sxs-lookup"><span data-stu-id="3b5f4-156">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="3b5f4-157">32 KB</span><span class="sxs-lookup"><span data-stu-id="3b5f4-157">32 KB</span></span> | <span data-ttu-id="3b5f4-158">Maksymalna liczba bajtów odebranych od klienta, które buforują serwer przed zastosowaniem nadużycia.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-158">The maximum number of bytes received from the client that the server buffers before applying backpressure.</span></span> <span data-ttu-id="3b5f4-159">Zwiększenie tej wartości umożliwia serwerowi otrzymywanie większych komunikatów szybciej bez naciskania, ale może zwiększyć zużycie pamięci.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-159">Increasing this value allows the server to receive larger messages more quickly without applying backpressure, but can increase memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="3b5f4-160">Dane zbierane automatycznie z `Authorize` atrybuty zastosowane do klasy centrum.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-160">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="3b5f4-161">Lista obiektów [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) służąca do określenia, czy klient jest autoryzowany do łączenia się z centrum.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-161">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="3b5f4-162">32 KB</span><span class="sxs-lookup"><span data-stu-id="3b5f4-162">32 KB</span></span> | <span data-ttu-id="3b5f4-163">Maksymalna liczba bajtów wysłanych przez aplikację, które buforuje serwer przed zaobserwowaniem presji.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-163">The maximum number of bytes sent by the app that the server buffers before observing backpressure.</span></span> <span data-ttu-id="3b5f4-164">Zwiększenie tej wartości umożliwia serwerowi przebuforowanie większych komunikatów szybciej bez czekania na obciążenie, ale może zwiększyć zużycie pamięci.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-164">Increasing this value allows the server to buffer larger messages more quickly without awaiting backpressure, but can increase memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="3b5f4-165">Wszystkie transporty są włączone.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-165">All Transports are enabled.</span></span> | <span data-ttu-id="3b5f4-166">Wyliczenie flag bitowych wartości `HttpTransportType`, które mogą ograniczyć transporty, z których może korzystać klient do nawiązywania połączeń.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-166">A bit flags enum of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="3b5f4-167">Zobacz poniżej.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-167">See below.</span></span> | <span data-ttu-id="3b5f4-168">Dodatkowe opcje specyficzne dla długiego transportu sondowania.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-168">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="3b5f4-169">Zobacz poniżej.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-169">See below.</span></span> | <span data-ttu-id="3b5f4-170">Dodatkowe opcje specyficzne dla transportu obiektów WebSockets.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-170">Additional options specific to the WebSockets transport.</span></span> |

<span data-ttu-id="3b5f4-171">W przypadku długiego transportu sondowania dostępne są dodatkowe opcje, które można skonfigurować przy użyciu właściwości `LongPolling`:</span><span class="sxs-lookup"><span data-stu-id="3b5f4-171">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="3b5f4-172">Opcja</span><span class="sxs-lookup"><span data-stu-id="3b5f4-172">Option</span></span> | <span data-ttu-id="3b5f4-173">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="3b5f4-173">Default Value</span></span> | <span data-ttu-id="3b5f4-174">Opis</span><span class="sxs-lookup"><span data-stu-id="3b5f4-174">Description</span></span> |
| ------ | ------------- | ----------- |
| `PollTimeout` | <span data-ttu-id="3b5f4-175">90 sekund</span><span class="sxs-lookup"><span data-stu-id="3b5f4-175">90 seconds</span></span> | <span data-ttu-id="3b5f4-176">Maksymalny czas, przez jaki serwer czeka na wysłanie wiadomości do klienta przed zakończeniem jednego żądania sondowania.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-176">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="3b5f4-177">Zmniejszenie tej wartości powoduje, że klient wystawia nowe żądania sondy częściej.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-177">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> |

<span data-ttu-id="3b5f4-178">Transport WebSocket ma dodatkowe opcje, które można skonfigurować za pomocą właściwości `WebSockets`:</span><span class="sxs-lookup"><span data-stu-id="3b5f4-178">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="3b5f4-179">Opcja</span><span class="sxs-lookup"><span data-stu-id="3b5f4-179">Option</span></span> | <span data-ttu-id="3b5f4-180">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="3b5f4-180">Default Value</span></span> | <span data-ttu-id="3b5f4-181">Opis</span><span class="sxs-lookup"><span data-stu-id="3b5f4-181">Description</span></span> |
| ------ | ------------- | ----------- |
| `CloseTimeout` | <span data-ttu-id="3b5f4-182">5 sekund</span><span class="sxs-lookup"><span data-stu-id="3b5f4-182">5 seconds</span></span> | <span data-ttu-id="3b5f4-183">Gdy serwer zostanie zamknięty, jeśli klient nie zostanie zamknięty w tym okresie, połączenie zostanie przerwane.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-183">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | `null` | <span data-ttu-id="3b5f4-184">Delegat, którego można użyć do ustawienia nagłówka `Sec-WebSocket-Protocol` na wartość niestandardową.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-184">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="3b5f4-185">Delegat otrzymuje wartości żądane przez klienta jako dane wejściowe i oczekuje na zwrócenie żądanej wartości.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-185">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="3b5f4-186">Konfigurowanie opcji klienta</span><span class="sxs-lookup"><span data-stu-id="3b5f4-186">Configure client options</span></span>

<span data-ttu-id="3b5f4-187">Opcje klienta można skonfigurować dla typu `HubConnectionBuilder` (dostępnego w klientach .NET i JavaScript).</span><span class="sxs-lookup"><span data-stu-id="3b5f4-187">Client options can be configured on the `HubConnectionBuilder` type (available in the .NET and JavaScript clients).</span></span> <span data-ttu-id="3b5f4-188">Jest ona również dostępna w kliencie Java, ale podklasa `HttpHubConnectionBuilder` zawiera opcje konfiguracji konstruktora, a także `HubConnection` samego siebie.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-188">It's also available in the Java client, but the `HttpHubConnectionBuilder` subclass is what contains the builder configuration options, as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="3b5f4-189">Konfigurowanie rejestrowania</span><span class="sxs-lookup"><span data-stu-id="3b5f4-189">Configure logging</span></span>

<span data-ttu-id="3b5f4-190">Rejestrowanie jest konfigurowane w kliencie .NET przy użyciu metody `ConfigureLogging`.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-190">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="3b5f4-191">Dostawcy i filtry rejestrowania mogą być rejestrowane w taki sam sposób, w jaki znajdują się na serwerze.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-191">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="3b5f4-192">Aby uzyskać więcej informacji, zobacz dokumentację dotyczącą [rejestrowania w ASP.NET Core](xref:fundamentals/logging/index) .</span><span class="sxs-lookup"><span data-stu-id="3b5f4-192">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="3b5f4-193">Aby zarejestrować dostawców rejestrowania, należy zainstalować wymagane pakiety.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-193">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="3b5f4-194">Aby zapoznać się z pełną listą, zobacz sekcję [wbudowane dostawcy rejestrowania](xref:fundamentals/logging/index#built-in-logging-providers) w witrynie docs.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-194">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="3b5f4-195">Aby na przykład włączyć rejestrowanie konsoli, należy zainstalować pakiet NuGet `Microsoft.Extensions.Logging.Console`.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-195">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="3b5f4-196">Wywołaj metodę rozszerzenia `AddConsole`:</span><span class="sxs-lookup"><span data-stu-id="3b5f4-196">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="3b5f4-197">W kliencie JavaScript istnieje podobna Metoda `configureLogging`.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-197">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="3b5f4-198">Podaj wartość `LogLevel` wskazującą minimalny poziom komunikatów dziennika do wygenerowania.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-198">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="3b5f4-199">Dzienniki są zapisywane w oknie konsoli przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-199">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

<span data-ttu-id="3b5f4-200">Zamiast wartości `LogLevel` można również podać wartość `string` reprezentującą nazwę poziomu dziennika.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-200">Instead of a `LogLevel` value, you can also provide a `string` value representing a log level name.</span></span> <span data-ttu-id="3b5f4-201">Jest to przydatne podczas konfigurowania rejestrowania SignalR w środowiskach, w których nie masz dostępu do stałych `LogLevel`.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-201">This is useful when configuring SignalR logging in environments where you don't have access to the `LogLevel` constants.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging("warn")
    .build();
```

<span data-ttu-id="3b5f4-202">Poniższa tabela zawiera listę dostępnych poziomów dzienników.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-202">The following table lists the available log levels.</span></span> <span data-ttu-id="3b5f4-203">Wybrana wartość `configureLogging` ustawia **minimalny** poziom dziennika, który zostanie zarejestrowany.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-203">The value you provide to `configureLogging` sets the **minimum** log level that will be logged.</span></span> <span data-ttu-id="3b5f4-204">Komunikaty zarejestrowane na tym poziomie **lub poziomy wymienione po nim w tabeli**zostaną zarejestrowane.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-204">Messages logged at this level, **or the levels listed after it in the table**, will be logged.</span></span>

| <span data-ttu-id="3b5f4-205">String</span><span class="sxs-lookup"><span data-stu-id="3b5f4-205">String</span></span>                      | <span data-ttu-id="3b5f4-206">LogLevel</span><span class="sxs-lookup"><span data-stu-id="3b5f4-206">LogLevel</span></span>               |
| --------------------------- | ---------------------- |
| `trace`                     | `LogLevel.Trace`       |
| `debug`                     | `LogLevel.Debug`       |
| <span data-ttu-id="3b5f4-207">`info` **lub** `information`</span><span class="sxs-lookup"><span data-stu-id="3b5f4-207">`info` **or** `information`</span></span> | `LogLevel.Information` |
| <span data-ttu-id="3b5f4-208">`warn` **lub** `warning`</span><span class="sxs-lookup"><span data-stu-id="3b5f4-208">`warn` **or** `warning`</span></span>     | `LogLevel.Warning`     |
| `error`                     | `LogLevel.Error`       |
| `critical`                  | `LogLevel.Critical`    |
| `none`                      | `LogLevel.None`        |

> [!NOTE]
> <span data-ttu-id="3b5f4-209">Aby całkowicie wyłączyć rejestrowanie, określ `signalR.LogLevel.None` w `configureLogging` metodzie.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-209">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="3b5f4-210">Aby uzyskać więcej informacji na temat rejestrowania, zobacz [dokumentację diagnostykiSignalR](xref:signalr/diagnostics).</span><span class="sxs-lookup"><span data-stu-id="3b5f4-210">For more information on logging, see the [SignalR Diagnostics documentation](xref:signalr/diagnostics).</span></span>

<span data-ttu-id="3b5f4-211">Klient języka Java SignalR używa biblioteki [SLF4J](https://www.slf4j.org/) do rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-211">The SignalR Java client uses the [SLF4J](https://www.slf4j.org/) library for logging.</span></span> <span data-ttu-id="3b5f4-212">Jest to interfejs API rejestrowania wysokiego poziomu, który umożliwia użytkownikom biblioteki wybranie własnych implementacji rejestrowania przez nałożenie określonej zależności rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-212">It's a high-level logging API that allows users of the library to chose their own specific logging implementation by bringing in a specific logging dependency.</span></span> <span data-ttu-id="3b5f4-213">Poniższy fragment kodu przedstawia sposób korzystania z `java.util.logging` z klientem SignalR Java.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-213">The following code snippet shows how to use `java.util.logging` with the SignalR Java client.</span></span>

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

<span data-ttu-id="3b5f4-214">Jeśli nie skonfigurujesz rejestrowania w zależnościach, SLF4J ładuje domyślny Rejestrator braku operacji z następującym komunikatem ostrzegawczym:</span><span class="sxs-lookup"><span data-stu-id="3b5f4-214">If you don't configure logging in your dependencies, SLF4J loads a default no-operation logger with the following warning message:</span></span>

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

<span data-ttu-id="3b5f4-215">Można je bezpiecznie zignorować.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-215">This can safely be ignored.</span></span>

### <a name="configure-allowed-transports"></a><span data-ttu-id="3b5f4-216">Skonfiguruj dozwolone transporty</span><span class="sxs-lookup"><span data-stu-id="3b5f4-216">Configure allowed transports</span></span>

<span data-ttu-id="3b5f4-217">Transporty używane przez SignalR można skonfigurować w wywołaniu `WithUrl` (`withUrl` w języku JavaScript).</span><span class="sxs-lookup"><span data-stu-id="3b5f4-217">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="3b5f4-218">Wartości `HttpTransportType` mogą być używane w celu ograniczenia klienta do używania tylko określonych transportów.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-218">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="3b5f4-219">Wszystkie transporty są domyślnie włączone.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-219">All transports are enabled by default.</span></span>

<span data-ttu-id="3b5f4-220">Na przykład, aby wyłączyć transport zdarzeń wysłanych przez serwer, ale zezwolić na połączenia z usługą WebSockets i długie sondowanie:</span><span class="sxs-lookup"><span data-stu-id="3b5f4-220">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="3b5f4-221">W kliencie JavaScript transporty są konfigurowane przez ustawienie pola `transport` w obiekcie Options udostępnianym `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="3b5f4-221">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

<span data-ttu-id="3b5f4-222">W tej wersji elementu WebSockets klienta Java jest jedynym dostępnym transportem.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-222">In this version of the Java client websockets is the only available transport.</span></span>

<span data-ttu-id="3b5f4-223">W kliencie Java, transport jest wybierany z metodą `withTransport`ową `HttpHubConnectionBuilder`.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-223">In the Java client, the transport is selected with the `withTransport` method on the `HttpHubConnectionBuilder`.</span></span> <span data-ttu-id="3b5f4-224">Klient Java domyślnie używa transportu usługi WebSockets.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-224">The Java client defaults to using the WebSockets transport.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withTransport(TransportEnum.WEBSOCKETS)
    .build();
```

> [!NOTE]
> <span data-ttu-id="3b5f4-225">Klient języka Java SignalR nie obsługuje jeszcze rezerwy transportowej.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-225">The SignalR Java client doesn't support transport fallback yet.</span></span>

### <a name="configure-bearer-authentication"></a><span data-ttu-id="3b5f4-226">Konfigurowanie uwierzytelniania okaziciela</span><span class="sxs-lookup"><span data-stu-id="3b5f4-226">Configure bearer authentication</span></span>

<span data-ttu-id="3b5f4-227">Aby zapewnić dane uwierzytelniania wraz z SignalR żądaniami, użyj opcji `AccessTokenProvider` (`accessTokenFactory` w języku JavaScript), aby określić funkcję, która zwraca żądany token dostępu.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-227">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="3b5f4-228">W kliencie .NET token dostępu jest przenoszona jako token "uwierzytelnianie okaziciela" protokołu HTTP (przy użyciu nagłówka `Authorization` z typem `Bearer`).</span><span class="sxs-lookup"><span data-stu-id="3b5f4-228">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="3b5f4-229">W kliencie JavaScript token dostępu jest używany jako token okaziciela, **z wyjątkiem** kilku przypadków, w których interfejsy API przeglądarki ograniczają możliwość zastosowania nagłówków (w konkretnym przypadku zdarzeń wysyłanych przez serwer i żądań WebSockets).</span><span class="sxs-lookup"><span data-stu-id="3b5f4-229">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="3b5f4-230">W takich przypadkach token dostępu jest dostarczany jako wartość ciągu zapytania `access_token`.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-230">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="3b5f4-231">W kliencie .NET opcja `AccessTokenProvider` można określić za pomocą delegata opcji w `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="3b5f4-231">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

<span data-ttu-id="3b5f4-232">W kliencie JavaScript token dostępu jest konfigurowany przez ustawienie pola `accessTokenFactory` w obiekcie Options w `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="3b5f4-232">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

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

<span data-ttu-id="3b5f4-233">W kliencie SignalR Java można skonfigurować token okaziciela do użycia na potrzeby uwierzytelniania, dostarczając fabrykę tokenów dostępu do [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span><span class="sxs-lookup"><span data-stu-id="3b5f4-233">In the SignalR Java client, you can configure a bearer token to use for authentication by providing an access token factory to the [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span></span> <span data-ttu-id="3b5f4-234">Użyj [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) , aby podać [](https://github.com/ReactiveX/RxJava) [\<pojedynczego ciągu](https://reactivex.io/documentation/single.html).</span><span class="sxs-lookup"><span data-stu-id="3b5f4-234">Use [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) to provide an [RxJava](https://github.com/ReactiveX/RxJava) [Single\<String>](https://reactivex.io/documentation/single.html).</span></span> <span data-ttu-id="3b5f4-235">Wywołanie metody [Single. Ustąp](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-)umożliwia zapisanie logiki w celu utworzenia tokenów dostępu dla klienta.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-235">With a call to [Single.defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), you can write logic to produce access tokens for your client.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="3b5f4-236">Konfigurowanie opcji limitu czasu i utrzymywania aktywności</span><span class="sxs-lookup"><span data-stu-id="3b5f4-236">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="3b5f4-237">W celu skonfigurowania zachowania przekroczenia limitu czasu i utrzymywania aktywności są dostępne dodatkowe opcje dla samego obiektu `HubConnection`:</span><span class="sxs-lookup"><span data-stu-id="3b5f4-237">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>

# <a name="nettabdotnet"></a>[<span data-ttu-id="3b5f4-238">.NET</span><span class="sxs-lookup"><span data-stu-id="3b5f4-238">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="3b5f4-239">Opcja</span><span class="sxs-lookup"><span data-stu-id="3b5f4-239">Option</span></span> | <span data-ttu-id="3b5f4-240">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="3b5f4-240">Default value</span></span> | <span data-ttu-id="3b5f4-241">Opis</span><span class="sxs-lookup"><span data-stu-id="3b5f4-241">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="3b5f4-242">30 sekund (30 000 MS)</span><span class="sxs-lookup"><span data-stu-id="3b5f4-242">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="3b5f4-243">Limit czasu dla działania serwera.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-243">Timeout for server activity.</span></span> <span data-ttu-id="3b5f4-244">Jeśli serwer nie wysłał komunikatu w tym interwale, Klient uważa, że serwer odłączył się i wyzwala zdarzenie `Closed` (`onclose` w języku JavaScript).</span><span class="sxs-lookup"><span data-stu-id="3b5f4-244">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="3b5f4-245">Ta wartość musi być wystarczająca do wysłania wiadomości ping z serwera **i** odebrana przez klienta w ramach przedziału czasu.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-245">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="3b5f4-246">Zalecana wartość to liczba z co najmniej podwójną wartością `KeepAliveInterval` serwera, aby umożliwić czas na nadejście poleceń ping.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-246">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="3b5f4-247">15 sekund</span><span class="sxs-lookup"><span data-stu-id="3b5f4-247">15 seconds</span></span> | <span data-ttu-id="3b5f4-248">Limit czasu początkowej uzgadniania z serwerem.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-248">Timeout for initial server handshake.</span></span> <span data-ttu-id="3b5f4-249">Jeśli serwer nie wyśle odpowiedzi uzgadniania w tym interwale, klient anuluje uzgadnianie i wyzwala zdarzenie `Closed` (`onclose` w języku JavaScript).</span><span class="sxs-lookup"><span data-stu-id="3b5f4-249">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="3b5f4-250">Jest to ustawienie zaawansowane, które należy zmodyfikować tylko w przypadku wystąpienia błędów limitu czasu uzgadniania z powodu poważnego opóźnienia sieci.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-250">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="3b5f4-251">Aby uzyskać więcej szczegółów na temat procesu uzgadniania, zobacz [Specyfikacja protokołuSignalR Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="3b5f4-251">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="3b5f4-252">15 sekund</span><span class="sxs-lookup"><span data-stu-id="3b5f4-252">15 seconds</span></span> | <span data-ttu-id="3b5f4-253">Określa interwał wysyłania komunikatów ping przez klienta.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-253">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="3b5f4-254">Wysłanie wszelkich komunikatów z klienta powoduje zresetowanie czasomierza do początku interwału.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-254">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="3b5f4-255">Jeśli klient nie wyśle komunikatu w `ClientTimeoutInterval` zestawie na serwerze, serwer traktuje klienta jako odłączony.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-255">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

<span data-ttu-id="3b5f4-256">W kliencie .NET wartości limitu czasu są określane jako wartości `TimeSpan`.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-256">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="3b5f4-257">JavaScript</span><span class="sxs-lookup"><span data-stu-id="3b5f4-257">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="3b5f4-258">Opcja</span><span class="sxs-lookup"><span data-stu-id="3b5f4-258">Option</span></span> | <span data-ttu-id="3b5f4-259">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="3b5f4-259">Default value</span></span> | <span data-ttu-id="3b5f4-260">Opis</span><span class="sxs-lookup"><span data-stu-id="3b5f4-260">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="3b5f4-261">30 sekund (30 000 MS)</span><span class="sxs-lookup"><span data-stu-id="3b5f4-261">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="3b5f4-262">Limit czasu dla działania serwera.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-262">Timeout for server activity.</span></span> <span data-ttu-id="3b5f4-263">Jeśli serwer nie wysłał komunikatu w tym interwale, Klient uważa, że serwer odłączył się i wyzwala zdarzenie `onclose`.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-263">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="3b5f4-264">Ta wartość musi być wystarczająca do wysłania wiadomości ping z serwera **i** odebrana przez klienta w ramach przedziału czasu.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-264">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="3b5f4-265">Zalecana wartość to liczba z co najmniej podwójną wartością `KeepAliveInterval` serwera, aby umożliwić czas na nadejście poleceń ping.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-265">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `keepAliveIntervalInMilliseconds` | <span data-ttu-id="3b5f4-266">15 sekund (15 000 MS)</span><span class="sxs-lookup"><span data-stu-id="3b5f4-266">15 seconds (15,000 milliseconds)</span></span> | <span data-ttu-id="3b5f4-267">Określa interwał wysyłania komunikatów ping przez klienta.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-267">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="3b5f4-268">Wysłanie wszelkich komunikatów z klienta powoduje zresetowanie czasomierza do początku interwału.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-268">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="3b5f4-269">Jeśli klient nie wyśle komunikatu w `ClientTimeoutInterval` zestawie na serwerze, serwer traktuje klienta jako odłączony.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-269">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="3b5f4-270">Java</span><span class="sxs-lookup"><span data-stu-id="3b5f4-270">Java</span></span>](#tab/java)

| <span data-ttu-id="3b5f4-271">Opcja</span><span class="sxs-lookup"><span data-stu-id="3b5f4-271">Option</span></span> | <span data-ttu-id="3b5f4-272">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="3b5f4-272">Default value</span></span> | <span data-ttu-id="3b5f4-273">Opis</span><span class="sxs-lookup"><span data-stu-id="3b5f4-273">Description</span></span> |
| ------ | ------------- | ----------- |
| <span data-ttu-id="3b5f4-274">`getServerTimeout` / `setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="3b5f4-274">`getServerTimeout` / `setServerTimeout`</span></span> | <span data-ttu-id="3b5f4-275">30 sekund (30 000 MS)</span><span class="sxs-lookup"><span data-stu-id="3b5f4-275">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="3b5f4-276">Limit czasu dla działania serwera.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-276">Timeout for server activity.</span></span> <span data-ttu-id="3b5f4-277">Jeśli serwer nie wysłał komunikatu w tym interwale, Klient uważa, że serwer odłączył się i wyzwala zdarzenie `onClose`.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-277">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="3b5f4-278">Ta wartość musi być wystarczająca do wysłania wiadomości ping z serwera **i** odebrana przez klienta w ramach przedziału czasu.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-278">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="3b5f4-279">Zalecana wartość to liczba z co najmniej podwójną wartością `KeepAliveInterval` serwera, aby umożliwić czas na nadejście poleceń ping.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-279">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="3b5f4-280">15 sekund</span><span class="sxs-lookup"><span data-stu-id="3b5f4-280">15 seconds</span></span> | <span data-ttu-id="3b5f4-281">Limit czasu początkowej uzgadniania z serwerem.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-281">Timeout for initial server handshake.</span></span> <span data-ttu-id="3b5f4-282">Jeśli serwer nie wyśle odpowiedzi uzgadniania w tym interwale, klient anuluje uzgadnianie i wyzwala zdarzenie `onClose`.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-282">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="3b5f4-283">Jest to ustawienie zaawansowane, które należy zmodyfikować tylko w przypadku wystąpienia błędów limitu czasu uzgadniania z powodu poważnego opóźnienia sieci.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-283">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="3b5f4-284">Aby uzyskać więcej szczegółów na temat procesu uzgadniania, zobacz [Specyfikacja protokołuSignalR Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="3b5f4-284">For more detail on the Handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| <span data-ttu-id="3b5f4-285">`getKeepAliveInterval` / `setKeepAliveInterval`</span><span class="sxs-lookup"><span data-stu-id="3b5f4-285">`getKeepAliveInterval` / `setKeepAliveInterval`</span></span> | <span data-ttu-id="3b5f4-286">15 sekund (15 000 MS)</span><span class="sxs-lookup"><span data-stu-id="3b5f4-286">15 seconds (15,000 milliseconds)</span></span> | <span data-ttu-id="3b5f4-287">Określa interwał wysyłania komunikatów ping przez klienta.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-287">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="3b5f4-288">Wysłanie wszelkich komunikatów z klienta powoduje zresetowanie czasomierza do początku interwału.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-288">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="3b5f4-289">Jeśli klient nie wyśle komunikatu w `ClientTimeoutInterval` zestawie na serwerze, serwer traktuje klienta jako odłączony.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-289">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

---

### <a name="configure-additional-options"></a><span data-ttu-id="3b5f4-290">Konfigurowanie opcji dodatkowych</span><span class="sxs-lookup"><span data-stu-id="3b5f4-290">Configure additional options</span></span>

<span data-ttu-id="3b5f4-291">Dodatkowe opcje można skonfigurować w metodzie `WithUrl` (`withUrl` w języku JavaScript) na `HubConnectionBuilder` lub w różnych interfejsach API konfiguracji na `HttpHubConnectionBuilder` klienta Java:</span><span class="sxs-lookup"><span data-stu-id="3b5f4-291">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder` or on the various configuration APIs on the `HttpHubConnectionBuilder` in the Java client:</span></span>

# <a name="nettabdotnet"></a>[<span data-ttu-id="3b5f4-292">.NET</span><span class="sxs-lookup"><span data-stu-id="3b5f4-292">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="3b5f4-293">Opcja .NET</span><span class="sxs-lookup"><span data-stu-id="3b5f4-293">.NET Option</span></span> |  <span data-ttu-id="3b5f4-294">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="3b5f4-294">Default value</span></span> | <span data-ttu-id="3b5f4-295">Opis</span><span class="sxs-lookup"><span data-stu-id="3b5f4-295">Description</span></span> |
| ----------- | -------------- | ----------- |
| `AccessTokenProvider` | `null` | <span data-ttu-id="3b5f4-296">Funkcja zwracająca ciąg, który jest dostarczany jako token uwierzytelniania okaziciela w żądaniach HTTP.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-296">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `false` | <span data-ttu-id="3b5f4-297">Ustaw tę wartość na `true`, aby pominąć etap negocjacji.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-297">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="3b5f4-298">**Obsługiwane tylko wtedy, gdy transport Sockets jest jedynym włączonym transportem**.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-298">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="3b5f4-299">Nie można włączyć tego ustawienia w przypadku korzystania z usługi Azure SignalR.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-299">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="3b5f4-300">Puste</span><span class="sxs-lookup"><span data-stu-id="3b5f4-300">Empty</span></span> | <span data-ttu-id="3b5f4-301">Kolekcja certyfikatów TLS do wysłania w celu uwierzytelniania żądań.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-301">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="3b5f4-302">Puste</span><span class="sxs-lookup"><span data-stu-id="3b5f4-302">Empty</span></span> | <span data-ttu-id="3b5f4-303">Kolekcja plików cookie protokołu HTTP do wysłania w każdym żądaniu HTTP.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-303">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="3b5f4-304">Puste</span><span class="sxs-lookup"><span data-stu-id="3b5f4-304">Empty</span></span> | <span data-ttu-id="3b5f4-305">Poświadczenia do wysyłania przy każdym żądaniu HTTP.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-305">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="3b5f4-306">5 sekund</span><span class="sxs-lookup"><span data-stu-id="3b5f4-306">5 seconds</span></span> | <span data-ttu-id="3b5f4-307">Tylko obiekty WebSockets.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-307">WebSockets only.</span></span> <span data-ttu-id="3b5f4-308">Maksymalny czas oczekiwania klienta po zamknięciu serwera w celu potwierdzenia żądania zamknięcia.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-308">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="3b5f4-309">Jeśli serwer nie potwierdzi zamknięcia w tym czasie, klient zostanie rozłączony.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-309">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="3b5f4-310">Puste</span><span class="sxs-lookup"><span data-stu-id="3b5f4-310">Empty</span></span> | <span data-ttu-id="3b5f4-311">Mapa dodatkowych nagłówków HTTP do wysłania w każdym żądaniu HTTP.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-311">A Map of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | `null` | <span data-ttu-id="3b5f4-312">Delegat, który może służyć do konfigurowania lub zastępowania `HttpMessageHandler` używany do wysyłania żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-312">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="3b5f4-313">Nieużywane na potrzeby połączeń protokołu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-313">Not used for WebSocket connections.</span></span> <span data-ttu-id="3b5f4-314">Ten delegat musi zwracać wartość różną od null i otrzymuje wartość domyślną jako parametr.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-314">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="3b5f4-315">Zmodyfikuj ustawienia tej wartości domyślnej i zwróć ją lub Zwróć nowe wystąpienie `HttpMessageHandler`.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-315">Either modify settings on that default value and return it, or return a new `HttpMessageHandler` instance.</span></span> <span data-ttu-id="3b5f4-316">**Podczas zastępowania programu obsługi należy skopiować ustawienia, które mają być zachowane z podanej procedury obsługi. w przeciwnym razie skonfigurowane opcje (takie jak pliki cookie i nagłówki) nie będą miały zastosowania do nowego programu obsługi.**</span><span class="sxs-lookup"><span data-stu-id="3b5f4-316">**When replacing the handler make sure to copy the settings you want to keep from the provided handler, otherwise, the configured options (such as Cookies and Headers) won't apply to the new handler.**</span></span> |
| `Proxy` | `null` | <span data-ttu-id="3b5f4-317">Serwer proxy HTTP, który ma być używany podczas wysyłania żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-317">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | `false` | <span data-ttu-id="3b5f4-318">Ustaw tę wartość logiczną, aby wysyłać domyślne poświadczenia dla żądań HTTP i WebSockets.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-318">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="3b5f4-319">Umożliwia to korzystanie z uwierzytelniania systemu Windows.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-319">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | `null` | <span data-ttu-id="3b5f4-320">Delegat, który może służyć do konfigurowania dodatkowych opcji protokołu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-320">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="3b5f4-321">Odbiera wystąpienie elementu [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) , którego można użyć do skonfigurowania opcji.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-321">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="3b5f4-322">JavaScript</span><span class="sxs-lookup"><span data-stu-id="3b5f4-322">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="3b5f4-323">Opcja języka JavaScript</span><span class="sxs-lookup"><span data-stu-id="3b5f4-323">JavaScript Option</span></span> | <span data-ttu-id="3b5f4-324">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="3b5f4-324">Default Value</span></span> | <span data-ttu-id="3b5f4-325">Opis</span><span class="sxs-lookup"><span data-stu-id="3b5f4-325">Description</span></span> |
| ----------------- | ------------- | ----------- |
| `accessTokenFactory` | `null` | <span data-ttu-id="3b5f4-326">Funkcja zwracająca ciąg, który jest dostarczany jako token uwierzytelniania okaziciela w żądaniach HTTP.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-326">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `skipNegotiation` | `false` | <span data-ttu-id="3b5f4-327">Ustaw tę wartość na `true`, aby pominąć etap negocjacji.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-327">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="3b5f4-328">**Obsługiwane tylko wtedy, gdy transport Sockets jest jedynym włączonym transportem**.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-328">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="3b5f4-329">Nie można włączyć tego ustawienia w przypadku korzystania z usługi Azure SignalR.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-329">This setting can't be enabled when using the Azure SignalR Service.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="3b5f4-330">Java</span><span class="sxs-lookup"><span data-stu-id="3b5f4-330">Java</span></span>](#tab/java)

| <span data-ttu-id="3b5f4-331">Opcja Java</span><span class="sxs-lookup"><span data-stu-id="3b5f4-331">Java Option</span></span> | <span data-ttu-id="3b5f4-332">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="3b5f4-332">Default Value</span></span> | <span data-ttu-id="3b5f4-333">Opis</span><span class="sxs-lookup"><span data-stu-id="3b5f4-333">Description</span></span> |
| ----------- | ------------- | ----------- |
| `withAccessTokenProvider` | `null` | <span data-ttu-id="3b5f4-334">Funkcja zwracająca ciąg, który jest dostarczany jako token uwierzytelniania okaziciela w żądaniach HTTP.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-334">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `shouldSkipNegotiate` | `false` | <span data-ttu-id="3b5f4-335">Ustaw tę wartość na `true`, aby pominąć etap negocjacji.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-335">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="3b5f4-336">**Obsługiwane tylko wtedy, gdy transport Sockets jest jedynym włączonym transportem**.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-336">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="3b5f4-337">Nie można włączyć tego ustawienia w przypadku korzystania z usługi Azure SignalR.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-337">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| <span data-ttu-id="3b5f4-338">`withHeader``withHeaders`</span><span class="sxs-lookup"><span data-stu-id="3b5f4-338">`withHeader` `withHeaders`</span></span> | <span data-ttu-id="3b5f4-339">Puste</span><span class="sxs-lookup"><span data-stu-id="3b5f4-339">Empty</span></span> | <span data-ttu-id="3b5f4-340">Mapa dodatkowych nagłówków HTTP do wysłania w każdym żądaniu HTTP.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-340">A Map of additional HTTP headers to send with every HTTP request.</span></span> |

---

<span data-ttu-id="3b5f4-341">W kliencie .NET opcje te można modyfikować za pomocą delegata opcji udostępnianego do `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="3b5f4-341">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="3b5f4-342">W kliencie JavaScript te opcje można podać w obiekcie JavaScript udostępnionym do `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="3b5f4-342">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

<span data-ttu-id="3b5f4-343">W kliencie Java Opcje te można skonfigurować przy użyciu metod na `HttpHubConnectionBuilder` zwracanych z `HubConnectionBuilder.create("HUB URL")`</span><span class="sxs-lookup"><span data-stu-id="3b5f4-343">In the Java client, these options can be configured with the methods on the `HttpHubConnectionBuilder` returned from the `HubConnectionBuilder.create("HUB URL")`</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
        .withHeader("Foo", "Bar")
        .shouldSkipNegotiate(true)
        .withHandshakeResponseTimeout(30*1000)
        .build();
```

## <a name="additional-resources"></a><span data-ttu-id="3b5f4-344">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="3b5f4-344">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>

::: moniker-end
::: moniker range="= aspnetcore-2.2"

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="3b5f4-345">Opcje serializacji JSON/MessagePack</span><span class="sxs-lookup"><span data-stu-id="3b5f4-345">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="3b5f4-346">ASP.NET Core SignalR obsługuje dwa protokoły dla komunikatów kodowania: [JSON](https://www.json.org/) i [MessagePack](https://msgpack.org/index.html).</span><span class="sxs-lookup"><span data-stu-id="3b5f4-346">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="3b5f4-347">Każdy protokół ma opcje konfiguracji serializacji.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-347">Each protocol has serialization configuration options.</span></span>

<span data-ttu-id="3b5f4-348">Serializacji JSON można skonfigurować na serwerze przy użyciu metody rozszerzenia [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) , która może zostać dodana po metodzie [addsygnalizująca](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-348">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method, which can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in your `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="3b5f4-349">Metoda `AddJsonProtocol` przyjmuje delegata, który odbiera obiekt `options`.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-349">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="3b5f4-350">Właściwość [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) tego obiektu jest obiektem JSON.NET `JsonSerializerSettings`, którego można użyć do skonfigurowania serializacji argumentów i zwracanych wartości.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-350">The [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) property on that object is a JSON.NET `JsonSerializerSettings` object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="3b5f4-351">Aby uzyskać więcej informacji, zapoznaj się z [dokumentacją JSON.NET](https://www.newtonsoft.com/json/help/html/Introduction.htm).</span><span class="sxs-lookup"><span data-stu-id="3b5f4-351">For more information, see the [JSON.NET documentation](https://www.newtonsoft.com/json/help/html/Introduction.htm).</span></span>
 
<span data-ttu-id="3b5f4-352">Przykładowo, aby skonfigurować serializator do używania nazw właściwości "PascalCase" zamiast domyślnych nazw "camelCase", należy użyć następującego kodu w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="3b5f4-352">As an example, to configure the serializer to use "PascalCase" property names, instead of the default "camelCase" names, use the following code in `Startup.ConfigureServices`:</span></span>
 
```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver =
            new DefaultContractResolver();
    });
```

<span data-ttu-id="3b5f4-353">W kliencie .NET ta sama Metoda rozszerzenia `AddJsonProtocol` istnieje w [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="3b5f4-353">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="3b5f4-354">Aby rozwiązać metodę rozszerzenia, należy zaimportować przestrzeń nazw `Microsoft.Extensions.DependencyInjection`:</span><span class="sxs-lookup"><span data-stu-id="3b5f4-354">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

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
> <span data-ttu-id="3b5f4-355">W tej chwili nie można skonfigurować serializacji JSON w kliencie JavaScript.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-355">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="3b5f4-356">Opcje serializacji MessagePack</span><span class="sxs-lookup"><span data-stu-id="3b5f4-356">MessagePack serialization options</span></span>

<span data-ttu-id="3b5f4-357">Serializacji MessagePack można skonfigurować, dostarczając delegata do wywołania [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) .</span><span class="sxs-lookup"><span data-stu-id="3b5f4-357">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="3b5f4-358">Aby uzyskać więcej informacji, zobacz [MessagePack w SignalR](xref:signalr/messagepackhubprotocol) .</span><span class="sxs-lookup"><span data-stu-id="3b5f4-358">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="3b5f4-359">W tej chwili nie można skonfigurować serializacji MessagePack w kliencie JavaScript.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-359">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="3b5f4-360">Konfigurowanie opcji serwera</span><span class="sxs-lookup"><span data-stu-id="3b5f4-360">Configure server options</span></span>

<span data-ttu-id="3b5f4-361">W poniższej tabeli opisano opcje konfigurowania centrów SignalR:</span><span class="sxs-lookup"><span data-stu-id="3b5f4-361">The following table describes options for configuring SignalR hubs:</span></span>

| <span data-ttu-id="3b5f4-362">Opcja</span><span class="sxs-lookup"><span data-stu-id="3b5f4-362">Option</span></span> | <span data-ttu-id="3b5f4-363">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="3b5f4-363">Default Value</span></span> | <span data-ttu-id="3b5f4-364">Opis</span><span class="sxs-lookup"><span data-stu-id="3b5f4-364">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="3b5f4-365">30 sekund</span><span class="sxs-lookup"><span data-stu-id="3b5f4-365">30 seconds</span></span> | <span data-ttu-id="3b5f4-366">Serwer Rozważ odłączenie klienta, jeśli nie odebrano komunikatu (w tym w tym interwale).</span><span class="sxs-lookup"><span data-stu-id="3b5f4-366">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="3b5f4-367">Może upłynąć dłużej niż ten interwał limitu czasu, aby klient mógł zostać oznaczony jako odłączony, ze względu na to, jak jest to zaimplementowane.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-367">It could take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="3b5f4-368">Zalecaną wartością jest podwójna wartość `KeepAliveInterval`.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-368">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="3b5f4-369">15 sekund</span><span class="sxs-lookup"><span data-stu-id="3b5f4-369">15 seconds</span></span> | <span data-ttu-id="3b5f4-370">Jeśli klient nie wyśle początkowego komunikatu uzgadniania w tym przedziale czasu, połączenie zostanie zamknięte.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-370">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="3b5f4-371">Jest to ustawienie zaawansowane, które należy zmodyfikować tylko w przypadku wystąpienia błędów limitu czasu uzgadniania z powodu poważnego opóźnienia sieci.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-371">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="3b5f4-372">Aby uzyskać więcej szczegółów na temat procesu uzgadniania, zobacz [Specyfikacja protokołuSignalR Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="3b5f4-372">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="3b5f4-373">15 sekund</span><span class="sxs-lookup"><span data-stu-id="3b5f4-373">15 seconds</span></span> | <span data-ttu-id="3b5f4-374">Jeśli serwer nie wysłał komunikatu w tym interwale, zostanie automatycznie wysłana wiadomość ping w celu pozostawienia otwartego połączenia.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-374">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="3b5f4-375">Podczas zmiany `KeepAliveInterval`Zmień ustawienie `ServerTimeout`/`serverTimeoutInMilliseconds` na kliencie.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-375">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="3b5f4-376">Zalecana wartość `ServerTimeout`/`serverTimeoutInMilliseconds` jest podwójna `KeepAliveInterval` wartość.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-376">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="3b5f4-377">Wszystkie zainstalowane protokoły</span><span class="sxs-lookup"><span data-stu-id="3b5f4-377">All installed protocols</span></span> | <span data-ttu-id="3b5f4-378">Protokoły obsługiwane przez to centrum.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-378">Protocols supported by this hub.</span></span> <span data-ttu-id="3b5f4-379">Domyślnie wszystkie protokoły zarejestrowane na serwerze są dozwolone, ale protokoły można usunąć z tej listy, aby wyłączyć określone protokoły dla poszczególnych centrów.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-379">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="3b5f4-380">Jeśli `true`, szczegółowe komunikaty o wyjątkach są zwracane do klientów, gdy wyjątek jest zgłaszany w metodzie centrum.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-380">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="3b5f4-381">Wartość domyślna to `false`, ponieważ te komunikaty o wyjątkach mogą zawierać informacje poufne.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-381">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

<span data-ttu-id="3b5f4-382">Opcje można skonfigurować dla wszystkich centrów, dostarczając opcje delegata do wywołania `AddSignalR` w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-382">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

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

<span data-ttu-id="3b5f4-383">Opcje jednego centrum przesłaniają opcje globalne dostępne w `AddSignalR` i można je skonfigurować za pomocą <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span><span class="sxs-lookup"><span data-stu-id="3b5f4-383">Options for a single hub override the global options provided in `AddSignalR` and can be configured using <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

### <a name="advanced-http-configuration-options"></a><span data-ttu-id="3b5f4-384">Zaawansowane opcje konfiguracji HTTP</span><span class="sxs-lookup"><span data-stu-id="3b5f4-384">Advanced HTTP configuration options</span></span>

<span data-ttu-id="3b5f4-385">Użyj `HttpConnectionDispatcherOptions`, aby skonfigurować zaawansowane ustawienia związane z transportami i zarządzaniem buforem pamięci.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-385">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="3b5f4-386">Te opcje są konfigurowane przez przekazanie delegata do [MapHub\<t >](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) w `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-386">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) in `Startup.Configure`.</span></span>

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

<span data-ttu-id="3b5f4-387">W poniższej tabeli opisano opcje konfigurowania zaawansowanych opcji protokołu HTTP w programie ASP.NET Core SignalR:</span><span class="sxs-lookup"><span data-stu-id="3b5f4-387">The following table describes options for configuring ASP.NET Core SignalR's advanced HTTP options:</span></span>

| <span data-ttu-id="3b5f4-388">Opcja</span><span class="sxs-lookup"><span data-stu-id="3b5f4-388">Option</span></span> | <span data-ttu-id="3b5f4-389">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="3b5f4-389">Default Value</span></span> | <span data-ttu-id="3b5f4-390">Opis</span><span class="sxs-lookup"><span data-stu-id="3b5f4-390">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="3b5f4-391">32 KB</span><span class="sxs-lookup"><span data-stu-id="3b5f4-391">32 KB</span></span> | <span data-ttu-id="3b5f4-392">Maksymalna liczba bajtów odebranych od klienta, które buforuje serwer.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-392">The maximum number of bytes received from the client that the server buffers.</span></span> <span data-ttu-id="3b5f4-393">Zwiększenie tej wartości umożliwia serwerowi otrzymywanie większych komunikatów, ale może mieć negatywny wpływ na użycie pamięci.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-393">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="3b5f4-394">Dane zbierane automatycznie z `Authorize` atrybuty zastosowane do klasy centrum.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-394">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="3b5f4-395">Lista obiektów [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) służąca do określenia, czy klient jest autoryzowany do łączenia się z centrum.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-395">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="3b5f4-396">32 KB</span><span class="sxs-lookup"><span data-stu-id="3b5f4-396">32 KB</span></span> | <span data-ttu-id="3b5f4-397">Maksymalna liczba bajtów wysłanych przez aplikację, które buforują serwer.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-397">The maximum number of bytes sent by the app that the server buffers.</span></span> <span data-ttu-id="3b5f4-398">Zwiększenie tej wartości umożliwia serwerowi wysyłanie większych komunikatów, ale może mieć negatywny wpływ na użycie pamięci.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-398">Increasing this value allows the server to send larger messages, but can negatively impact memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="3b5f4-399">Wszystkie transporty są włączone.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-399">All Transports are enabled.</span></span> | <span data-ttu-id="3b5f4-400">Wyliczenie flag bitowych wartości `HttpTransportType`, które mogą ograniczyć transporty, z których może korzystać klient do nawiązywania połączeń.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-400">A bit flags enum of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="3b5f4-401">Zobacz poniżej.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-401">See below.</span></span> | <span data-ttu-id="3b5f4-402">Dodatkowe opcje specyficzne dla długiego transportu sondowania.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-402">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="3b5f4-403">Zobacz poniżej.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-403">See below.</span></span> | <span data-ttu-id="3b5f4-404">Dodatkowe opcje specyficzne dla transportu obiektów WebSockets.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-404">Additional options specific to the WebSockets transport.</span></span> |

<span data-ttu-id="3b5f4-405">W przypadku długiego transportu sondowania dostępne są dodatkowe opcje, które można skonfigurować przy użyciu właściwości `LongPolling`:</span><span class="sxs-lookup"><span data-stu-id="3b5f4-405">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="3b5f4-406">Opcja</span><span class="sxs-lookup"><span data-stu-id="3b5f4-406">Option</span></span> | <span data-ttu-id="3b5f4-407">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="3b5f4-407">Default Value</span></span> | <span data-ttu-id="3b5f4-408">Opis</span><span class="sxs-lookup"><span data-stu-id="3b5f4-408">Description</span></span> |
| ------ | ------------- | ----------- |
| `PollTimeout` | <span data-ttu-id="3b5f4-409">90 sekund</span><span class="sxs-lookup"><span data-stu-id="3b5f4-409">90 seconds</span></span> | <span data-ttu-id="3b5f4-410">Maksymalny czas, przez jaki serwer czeka na wysłanie wiadomości do klienta przed zakończeniem jednego żądania sondowania.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-410">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="3b5f4-411">Zmniejszenie tej wartości powoduje, że klient wystawia nowe żądania sondy częściej.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-411">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> |

<span data-ttu-id="3b5f4-412">Transport WebSocket ma dodatkowe opcje, które można skonfigurować za pomocą właściwości `WebSockets`:</span><span class="sxs-lookup"><span data-stu-id="3b5f4-412">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="3b5f4-413">Opcja</span><span class="sxs-lookup"><span data-stu-id="3b5f4-413">Option</span></span> | <span data-ttu-id="3b5f4-414">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="3b5f4-414">Default Value</span></span> | <span data-ttu-id="3b5f4-415">Opis</span><span class="sxs-lookup"><span data-stu-id="3b5f4-415">Description</span></span> |
| ------ | ------------- | ----------- |
| `CloseTimeout` | <span data-ttu-id="3b5f4-416">5 sekund</span><span class="sxs-lookup"><span data-stu-id="3b5f4-416">5 seconds</span></span> | <span data-ttu-id="3b5f4-417">Gdy serwer zostanie zamknięty, jeśli klient nie zostanie zamknięty w tym okresie, połączenie zostanie przerwane.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-417">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | `null` | <span data-ttu-id="3b5f4-418">Delegat, którego można użyć do ustawienia nagłówka `Sec-WebSocket-Protocol` na wartość niestandardową.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-418">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="3b5f4-419">Delegat otrzymuje wartości żądane przez klienta jako dane wejściowe i oczekuje na zwrócenie żądanej wartości.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-419">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="3b5f4-420">Konfigurowanie opcji klienta</span><span class="sxs-lookup"><span data-stu-id="3b5f4-420">Configure client options</span></span>

<span data-ttu-id="3b5f4-421">Opcje klienta można skonfigurować dla typu `HubConnectionBuilder` (dostępnego w klientach .NET i JavaScript).</span><span class="sxs-lookup"><span data-stu-id="3b5f4-421">Client options can be configured on the `HubConnectionBuilder` type (available in the .NET and JavaScript clients).</span></span> <span data-ttu-id="3b5f4-422">Jest ona również dostępna w kliencie Java, ale podklasa `HttpHubConnectionBuilder` zawiera opcje konfiguracji konstruktora, a także `HubConnection` samego siebie.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-422">It's also available in the Java client, but the `HttpHubConnectionBuilder` subclass is what contains the builder configuration options, as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="3b5f4-423">Konfigurowanie rejestrowania</span><span class="sxs-lookup"><span data-stu-id="3b5f4-423">Configure logging</span></span>

<span data-ttu-id="3b5f4-424">Rejestrowanie jest konfigurowane w kliencie .NET przy użyciu metody `ConfigureLogging`.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-424">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="3b5f4-425">Dostawcy i filtry rejestrowania mogą być rejestrowane w taki sam sposób, w jaki znajdują się na serwerze.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-425">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="3b5f4-426">Aby uzyskać więcej informacji, zobacz dokumentację dotyczącą [rejestrowania w ASP.NET Core](xref:fundamentals/logging/index) .</span><span class="sxs-lookup"><span data-stu-id="3b5f4-426">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="3b5f4-427">Aby zarejestrować dostawców rejestrowania, należy zainstalować wymagane pakiety.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-427">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="3b5f4-428">Aby zapoznać się z pełną listą, zobacz sekcję [wbudowane dostawcy rejestrowania](xref:fundamentals/logging/index#built-in-logging-providers) w witrynie docs.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-428">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="3b5f4-429">Aby na przykład włączyć rejestrowanie konsoli, należy zainstalować pakiet NuGet `Microsoft.Extensions.Logging.Console`.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-429">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="3b5f4-430">Wywołaj metodę rozszerzenia `AddConsole`:</span><span class="sxs-lookup"><span data-stu-id="3b5f4-430">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="3b5f4-431">W kliencie JavaScript istnieje podobna Metoda `configureLogging`.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-431">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="3b5f4-432">Podaj wartość `LogLevel` wskazującą minimalny poziom komunikatów dziennika do wygenerowania.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-432">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="3b5f4-433">Dzienniki są zapisywane w oknie konsoli przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-433">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

> [!NOTE]
> <span data-ttu-id="3b5f4-434">Aby całkowicie wyłączyć rejestrowanie, określ `signalR.LogLevel.None` w `configureLogging` metodzie.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-434">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="3b5f4-435">Aby uzyskać więcej informacji na temat rejestrowania, zobacz [dokumentację diagnostykiSignalR](xref:signalr/diagnostics).</span><span class="sxs-lookup"><span data-stu-id="3b5f4-435">For more information on logging, see the [SignalR Diagnostics documentation](xref:signalr/diagnostics).</span></span>

<span data-ttu-id="3b5f4-436">Klient języka Java SignalR używa biblioteki [SLF4J](https://www.slf4j.org/) do rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-436">The SignalR Java client uses the [SLF4J](https://www.slf4j.org/) library for logging.</span></span> <span data-ttu-id="3b5f4-437">Jest to interfejs API rejestrowania wysokiego poziomu, który umożliwia użytkownikom biblioteki wybranie własnych implementacji rejestrowania przez nałożenie określonej zależności rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-437">It's a high-level logging API that allows users of the library to chose their own specific logging implementation by bringing in a specific logging dependency.</span></span> <span data-ttu-id="3b5f4-438">Poniższy fragment kodu przedstawia sposób korzystania z `java.util.logging` z klientem SignalR Java.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-438">The following code snippet shows how to use `java.util.logging` with the SignalR Java client.</span></span>

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

<span data-ttu-id="3b5f4-439">Jeśli nie skonfigurujesz rejestrowania w zależnościach, SLF4J ładuje domyślny Rejestrator braku operacji z następującym komunikatem ostrzegawczym:</span><span class="sxs-lookup"><span data-stu-id="3b5f4-439">If you don't configure logging in your dependencies, SLF4J loads a default no-operation logger with the following warning message:</span></span>

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

<span data-ttu-id="3b5f4-440">Można je bezpiecznie zignorować.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-440">This can safely be ignored.</span></span>

### <a name="configure-allowed-transports"></a><span data-ttu-id="3b5f4-441">Skonfiguruj dozwolone transporty</span><span class="sxs-lookup"><span data-stu-id="3b5f4-441">Configure allowed transports</span></span>

<span data-ttu-id="3b5f4-442">Transporty używane przez SignalR można skonfigurować w wywołaniu `WithUrl` (`withUrl` w języku JavaScript).</span><span class="sxs-lookup"><span data-stu-id="3b5f4-442">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="3b5f4-443">Wartości `HttpTransportType` mogą być używane w celu ograniczenia klienta do używania tylko określonych transportów.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-443">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="3b5f4-444">Wszystkie transporty są domyślnie włączone.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-444">All transports are enabled by default.</span></span>

<span data-ttu-id="3b5f4-445">Na przykład, aby wyłączyć transport zdarzeń wysłanych przez serwer, ale zezwolić na połączenia z usługą WebSockets i długie sondowanie:</span><span class="sxs-lookup"><span data-stu-id="3b5f4-445">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="3b5f4-446">W kliencie JavaScript transporty są konfigurowane przez ustawienie pola `transport` w obiekcie Options udostępnianym `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="3b5f4-446">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

<span data-ttu-id="3b5f4-447">W tej wersji elementu WebSockets klienta Java jest jedynym dostępnym transportem.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-447">In this version of the Java client websockets is the only available transport.</span></span>

### <a name="configure-bearer-authentication"></a><span data-ttu-id="3b5f4-448">Konfigurowanie uwierzytelniania okaziciela</span><span class="sxs-lookup"><span data-stu-id="3b5f4-448">Configure bearer authentication</span></span>

<span data-ttu-id="3b5f4-449">Aby zapewnić dane uwierzytelniania wraz z SignalR żądaniami, użyj opcji `AccessTokenProvider` (`accessTokenFactory` w języku JavaScript), aby określić funkcję, która zwraca żądany token dostępu.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-449">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="3b5f4-450">W kliencie .NET token dostępu jest przenoszona jako token "uwierzytelnianie okaziciela" protokołu HTTP (przy użyciu nagłówka `Authorization` z typem `Bearer`).</span><span class="sxs-lookup"><span data-stu-id="3b5f4-450">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="3b5f4-451">W kliencie JavaScript token dostępu jest używany jako token okaziciela, **z wyjątkiem** kilku przypadków, w których interfejsy API przeglądarki ograniczają możliwość zastosowania nagłówków (w konkretnym przypadku zdarzeń wysyłanych przez serwer i żądań WebSockets).</span><span class="sxs-lookup"><span data-stu-id="3b5f4-451">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="3b5f4-452">W takich przypadkach token dostępu jest dostarczany jako wartość ciągu zapytania `access_token`.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-452">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="3b5f4-453">W kliencie .NET opcja `AccessTokenProvider` można określić za pomocą delegata opcji w `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="3b5f4-453">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

<span data-ttu-id="3b5f4-454">W kliencie JavaScript token dostępu jest konfigurowany przez ustawienie pola `accessTokenFactory` w obiekcie Options w `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="3b5f4-454">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

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

<span data-ttu-id="3b5f4-455">W kliencie SignalR Java można skonfigurować token okaziciela do użycia na potrzeby uwierzytelniania, dostarczając fabrykę tokenów dostępu do [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span><span class="sxs-lookup"><span data-stu-id="3b5f4-455">In the SignalR Java client, you can configure a bearer token to use for authentication by providing an access token factory to the [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span></span> <span data-ttu-id="3b5f4-456">Użyj [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) , aby podać [](https://github.com/ReactiveX/RxJava) [\<pojedynczego ciągu](https://reactivex.io/documentation/single.html).</span><span class="sxs-lookup"><span data-stu-id="3b5f4-456">Use [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) to provide an [RxJava](https://github.com/ReactiveX/RxJava) [Single\<String>](https://reactivex.io/documentation/single.html).</span></span> <span data-ttu-id="3b5f4-457">Wywołanie metody [Single. Ustąp](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-)umożliwia zapisanie logiki w celu utworzenia tokenów dostępu dla klienta.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-457">With a call to [Single.defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), you can write logic to produce access tokens for your client.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="3b5f4-458">Konfigurowanie opcji limitu czasu i utrzymywania aktywności</span><span class="sxs-lookup"><span data-stu-id="3b5f4-458">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="3b5f4-459">W celu skonfigurowania zachowania przekroczenia limitu czasu i utrzymywania aktywności są dostępne dodatkowe opcje dla samego obiektu `HubConnection`:</span><span class="sxs-lookup"><span data-stu-id="3b5f4-459">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>

# <a name="nettabdotnet"></a>[<span data-ttu-id="3b5f4-460">.NET</span><span class="sxs-lookup"><span data-stu-id="3b5f4-460">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="3b5f4-461">Opcja</span><span class="sxs-lookup"><span data-stu-id="3b5f4-461">Option</span></span> | <span data-ttu-id="3b5f4-462">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="3b5f4-462">Default value</span></span> | <span data-ttu-id="3b5f4-463">Opis</span><span class="sxs-lookup"><span data-stu-id="3b5f4-463">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="3b5f4-464">30 sekund (30 000 MS)</span><span class="sxs-lookup"><span data-stu-id="3b5f4-464">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="3b5f4-465">Limit czasu dla działania serwera.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-465">Timeout for server activity.</span></span> <span data-ttu-id="3b5f4-466">Jeśli serwer nie wysłał komunikatu w tym interwale, Klient uważa, że serwer odłączył się i wyzwala zdarzenie `Closed` (`onclose` w języku JavaScript).</span><span class="sxs-lookup"><span data-stu-id="3b5f4-466">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="3b5f4-467">Ta wartość musi być wystarczająca do wysłania wiadomości ping z serwera **i** odebrana przez klienta w ramach przedziału czasu.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-467">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="3b5f4-468">Zalecana wartość to liczba z co najmniej podwójną wartością `KeepAliveInterval` serwera, aby umożliwić czas na nadejście poleceń ping.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-468">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="3b5f4-469">15 sekund</span><span class="sxs-lookup"><span data-stu-id="3b5f4-469">15 seconds</span></span> | <span data-ttu-id="3b5f4-470">Limit czasu początkowej uzgadniania z serwerem.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-470">Timeout for initial server handshake.</span></span> <span data-ttu-id="3b5f4-471">Jeśli serwer nie wyśle odpowiedzi uzgadniania w tym interwale, klient anuluje uzgadnianie i wyzwala zdarzenie `Closed` (`onclose` w języku JavaScript).</span><span class="sxs-lookup"><span data-stu-id="3b5f4-471">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="3b5f4-472">Jest to ustawienie zaawansowane, które należy zmodyfikować tylko w przypadku wystąpienia błędów limitu czasu uzgadniania z powodu poważnego opóźnienia sieci.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-472">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="3b5f4-473">Aby uzyskać więcej szczegółów na temat procesu uzgadniania, zobacz [Specyfikacja protokołuSignalR Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="3b5f4-473">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="3b5f4-474">15 sekund</span><span class="sxs-lookup"><span data-stu-id="3b5f4-474">15 seconds</span></span> | <span data-ttu-id="3b5f4-475">Określa interwał wysyłania komunikatów ping przez klienta.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-475">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="3b5f4-476">Wysłanie wszelkich komunikatów z klienta powoduje zresetowanie czasomierza do początku interwału.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-476">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="3b5f4-477">Jeśli klient nie wyśle komunikatu w `ClientTimeoutInterval` zestawie na serwerze, serwer traktuje klienta jako odłączony.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-477">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

<span data-ttu-id="3b5f4-478">W kliencie .NET wartości limitu czasu są określane jako wartości `TimeSpan`.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-478">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="3b5f4-479">JavaScript</span><span class="sxs-lookup"><span data-stu-id="3b5f4-479">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="3b5f4-480">Opcja</span><span class="sxs-lookup"><span data-stu-id="3b5f4-480">Option</span></span> | <span data-ttu-id="3b5f4-481">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="3b5f4-481">Default value</span></span> | <span data-ttu-id="3b5f4-482">Opis</span><span class="sxs-lookup"><span data-stu-id="3b5f4-482">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="3b5f4-483">30 sekund (30 000 MS)</span><span class="sxs-lookup"><span data-stu-id="3b5f4-483">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="3b5f4-484">Limit czasu dla działania serwera.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-484">Timeout for server activity.</span></span> <span data-ttu-id="3b5f4-485">Jeśli serwer nie wysłał komunikatu w tym interwale, Klient uważa, że serwer odłączył się i wyzwala zdarzenie `onclose`.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-485">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="3b5f4-486">Ta wartość musi być wystarczająca do wysłania wiadomości ping z serwera **i** odebrana przez klienta w ramach przedziału czasu.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-486">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="3b5f4-487">Zalecana wartość to liczba z co najmniej podwójną wartością `KeepAliveInterval` serwera, aby umożliwić czas na nadejście poleceń ping.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-487">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `keepAliveIntervalInMilliseconds` | <span data-ttu-id="3b5f4-488">15 sekund (15 000 MS)</span><span class="sxs-lookup"><span data-stu-id="3b5f4-488">15 seconds (15,000 milliseconds)</span></span> | <span data-ttu-id="3b5f4-489">Określa interwał wysyłania komunikatów ping przez klienta.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-489">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="3b5f4-490">Wysłanie wszelkich komunikatów z klienta powoduje zresetowanie czasomierza do początku interwału.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-490">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="3b5f4-491">Jeśli klient nie wyśle komunikatu w `ClientTimeoutInterval` zestawie na serwerze, serwer traktuje klienta jako odłączony.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-491">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="3b5f4-492">Java</span><span class="sxs-lookup"><span data-stu-id="3b5f4-492">Java</span></span>](#tab/java)

| <span data-ttu-id="3b5f4-493">Opcja</span><span class="sxs-lookup"><span data-stu-id="3b5f4-493">Option</span></span> | <span data-ttu-id="3b5f4-494">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="3b5f4-494">Default value</span></span> | <span data-ttu-id="3b5f4-495">Opis</span><span class="sxs-lookup"><span data-stu-id="3b5f4-495">Description</span></span> |
| ------ | ------------- | ----------- |
| <span data-ttu-id="3b5f4-496">`getServerTimeout` / `setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="3b5f4-496">`getServerTimeout` / `setServerTimeout`</span></span> | <span data-ttu-id="3b5f4-497">30 sekund (30 000 MS)</span><span class="sxs-lookup"><span data-stu-id="3b5f4-497">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="3b5f4-498">Limit czasu dla działania serwera.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-498">Timeout for server activity.</span></span> <span data-ttu-id="3b5f4-499">Jeśli serwer nie wysłał komunikatu w tym interwale, Klient uważa, że serwer odłączył się i wyzwala zdarzenie `onClose`.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-499">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="3b5f4-500">Ta wartość musi być wystarczająca do wysłania wiadomości ping z serwera **i** odebrana przez klienta w ramach przedziału czasu.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-500">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="3b5f4-501">Zalecana wartość to liczba z co najmniej podwójną wartością `KeepAliveInterval` serwera, aby umożliwić czas na nadejście poleceń ping.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-501">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="3b5f4-502">15 sekund</span><span class="sxs-lookup"><span data-stu-id="3b5f4-502">15 seconds</span></span> | <span data-ttu-id="3b5f4-503">Limit czasu początkowej uzgadniania z serwerem.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-503">Timeout for initial server handshake.</span></span> <span data-ttu-id="3b5f4-504">Jeśli serwer nie wyśle odpowiedzi uzgadniania w tym interwale, klient anuluje uzgadnianie i wyzwala zdarzenie `onClose`.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-504">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="3b5f4-505">Jest to ustawienie zaawansowane, które należy zmodyfikować tylko w przypadku wystąpienia błędów limitu czasu uzgadniania z powodu poważnego opóźnienia sieci.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-505">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="3b5f4-506">Aby uzyskać więcej szczegółów na temat procesu uzgadniania, zobacz [Specyfikacja protokołuSignalR Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="3b5f4-506">For more detail on the Handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| <span data-ttu-id="3b5f4-507">`getKeepAliveInterval` / `setKeepAliveInterval`</span><span class="sxs-lookup"><span data-stu-id="3b5f4-507">`getKeepAliveInterval` / `setKeepAliveInterval`</span></span> | <span data-ttu-id="3b5f4-508">15 sekund (15 000 MS)</span><span class="sxs-lookup"><span data-stu-id="3b5f4-508">15 seconds (15,000 milliseconds)</span></span> | <span data-ttu-id="3b5f4-509">Określa interwał wysyłania komunikatów ping przez klienta.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-509">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="3b5f4-510">Wysłanie wszelkich komunikatów z klienta powoduje zresetowanie czasomierza do początku interwału.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-510">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="3b5f4-511">Jeśli klient nie wyśle komunikatu w `ClientTimeoutInterval` zestawie na serwerze, serwer traktuje klienta jako odłączony.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-511">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

---

### <a name="configure-additional-options"></a><span data-ttu-id="3b5f4-512">Konfigurowanie opcji dodatkowych</span><span class="sxs-lookup"><span data-stu-id="3b5f4-512">Configure additional options</span></span>

<span data-ttu-id="3b5f4-513">Dodatkowe opcje można skonfigurować w metodzie `WithUrl` (`withUrl` w języku JavaScript) na `HubConnectionBuilder` lub w różnych interfejsach API konfiguracji na `HttpHubConnectionBuilder` klienta Java:</span><span class="sxs-lookup"><span data-stu-id="3b5f4-513">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder` or on the various configuration APIs on the `HttpHubConnectionBuilder` in the Java client:</span></span>

# <a name="nettabdotnet"></a>[<span data-ttu-id="3b5f4-514">.NET</span><span class="sxs-lookup"><span data-stu-id="3b5f4-514">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="3b5f4-515">Opcja .NET</span><span class="sxs-lookup"><span data-stu-id="3b5f4-515">.NET Option</span></span> |  <span data-ttu-id="3b5f4-516">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="3b5f4-516">Default value</span></span> | <span data-ttu-id="3b5f4-517">Opis</span><span class="sxs-lookup"><span data-stu-id="3b5f4-517">Description</span></span> |
| ----------- | -------------- | ----------- |
| `AccessTokenProvider` | `null` | <span data-ttu-id="3b5f4-518">Funkcja zwracająca ciąg, który jest dostarczany jako token uwierzytelniania okaziciela w żądaniach HTTP.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-518">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `false` | <span data-ttu-id="3b5f4-519">Ustaw tę wartość na `true`, aby pominąć etap negocjacji.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-519">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="3b5f4-520">**Obsługiwane tylko wtedy, gdy transport Sockets jest jedynym włączonym transportem**.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-520">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="3b5f4-521">Nie można włączyć tego ustawienia w przypadku korzystania z usługi Azure SignalR.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-521">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="3b5f4-522">Puste</span><span class="sxs-lookup"><span data-stu-id="3b5f4-522">Empty</span></span> | <span data-ttu-id="3b5f4-523">Kolekcja certyfikatów TLS do wysłania w celu uwierzytelniania żądań.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-523">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="3b5f4-524">Puste</span><span class="sxs-lookup"><span data-stu-id="3b5f4-524">Empty</span></span> | <span data-ttu-id="3b5f4-525">Kolekcja plików cookie protokołu HTTP do wysłania w każdym żądaniu HTTP.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-525">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="3b5f4-526">Puste</span><span class="sxs-lookup"><span data-stu-id="3b5f4-526">Empty</span></span> | <span data-ttu-id="3b5f4-527">Poświadczenia do wysyłania przy każdym żądaniu HTTP.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-527">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="3b5f4-528">5 sekund</span><span class="sxs-lookup"><span data-stu-id="3b5f4-528">5 seconds</span></span> | <span data-ttu-id="3b5f4-529">Tylko obiekty WebSockets.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-529">WebSockets only.</span></span> <span data-ttu-id="3b5f4-530">Maksymalny czas oczekiwania klienta po zamknięciu serwera w celu potwierdzenia żądania zamknięcia.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-530">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="3b5f4-531">Jeśli serwer nie potwierdzi zamknięcia w tym czasie, klient zostanie rozłączony.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-531">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="3b5f4-532">Puste</span><span class="sxs-lookup"><span data-stu-id="3b5f4-532">Empty</span></span> | <span data-ttu-id="3b5f4-533">Mapa dodatkowych nagłówków HTTP do wysłania w każdym żądaniu HTTP.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-533">A Map of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | `null` | <span data-ttu-id="3b5f4-534">Delegat, który może służyć do konfigurowania lub zastępowania `HttpMessageHandler` używany do wysyłania żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-534">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="3b5f4-535">Nieużywane na potrzeby połączeń protokołu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-535">Not used for WebSocket connections.</span></span> <span data-ttu-id="3b5f4-536">Ten delegat musi zwracać wartość różną od null i otrzymuje wartość domyślną jako parametr.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-536">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="3b5f4-537">Zmodyfikuj ustawienia tej wartości domyślnej i zwróć ją lub Zwróć nowe wystąpienie `HttpMessageHandler`.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-537">Either modify settings on that default value and return it, or return a new `HttpMessageHandler` instance.</span></span> <span data-ttu-id="3b5f4-538">**Podczas zastępowania programu obsługi należy skopiować ustawienia, które mają być zachowane z podanej procedury obsługi. w przeciwnym razie skonfigurowane opcje (takie jak pliki cookie i nagłówki) nie będą miały zastosowania do nowego programu obsługi.**</span><span class="sxs-lookup"><span data-stu-id="3b5f4-538">**When replacing the handler make sure to copy the settings you want to keep from the provided handler, otherwise, the configured options (such as Cookies and Headers) won't apply to the new handler.**</span></span> |
| `Proxy` | `null` | <span data-ttu-id="3b5f4-539">Serwer proxy HTTP, który ma być używany podczas wysyłania żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-539">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | `false` | <span data-ttu-id="3b5f4-540">Ustaw tę wartość logiczną, aby wysyłać domyślne poświadczenia dla żądań HTTP i WebSockets.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-540">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="3b5f4-541">Umożliwia to korzystanie z uwierzytelniania systemu Windows.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-541">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | `null` | <span data-ttu-id="3b5f4-542">Delegat, który może służyć do konfigurowania dodatkowych opcji protokołu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-542">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="3b5f4-543">Odbiera wystąpienie elementu [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) , którego można użyć do skonfigurowania opcji.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-543">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="3b5f4-544">JavaScript</span><span class="sxs-lookup"><span data-stu-id="3b5f4-544">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="3b5f4-545">Opcja języka JavaScript</span><span class="sxs-lookup"><span data-stu-id="3b5f4-545">JavaScript Option</span></span> | <span data-ttu-id="3b5f4-546">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="3b5f4-546">Default Value</span></span> | <span data-ttu-id="3b5f4-547">Opis</span><span class="sxs-lookup"><span data-stu-id="3b5f4-547">Description</span></span> |
| ----------------- | ------------- | ----------- |
| `accessTokenFactory` | `null` | <span data-ttu-id="3b5f4-548">Funkcja zwracająca ciąg, który jest dostarczany jako token uwierzytelniania okaziciela w żądaniach HTTP.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-548">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `skipNegotiation` | `false` | <span data-ttu-id="3b5f4-549">Ustaw tę wartość na `true`, aby pominąć etap negocjacji.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-549">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="3b5f4-550">**Obsługiwane tylko wtedy, gdy transport Sockets jest jedynym włączonym transportem**.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-550">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="3b5f4-551">Nie można włączyć tego ustawienia w przypadku korzystania z usługi Azure SignalR.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-551">This setting can't be enabled when using the Azure SignalR Service.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="3b5f4-552">Java</span><span class="sxs-lookup"><span data-stu-id="3b5f4-552">Java</span></span>](#tab/java)

| <span data-ttu-id="3b5f4-553">Opcja Java</span><span class="sxs-lookup"><span data-stu-id="3b5f4-553">Java Option</span></span> | <span data-ttu-id="3b5f4-554">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="3b5f4-554">Default Value</span></span> | <span data-ttu-id="3b5f4-555">Opis</span><span class="sxs-lookup"><span data-stu-id="3b5f4-555">Description</span></span> |
| ----------- | ------------- | ----------- |
| `withAccessTokenProvider` | `null` | <span data-ttu-id="3b5f4-556">Funkcja zwracająca ciąg, który jest dostarczany jako token uwierzytelniania okaziciela w żądaniach HTTP.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-556">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `shouldSkipNegotiate` | `false` | <span data-ttu-id="3b5f4-557">Ustaw tę wartość na `true`, aby pominąć etap negocjacji.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-557">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="3b5f4-558">**Obsługiwane tylko wtedy, gdy transport Sockets jest jedynym włączonym transportem**.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-558">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="3b5f4-559">Nie można włączyć tego ustawienia w przypadku korzystania z usługi Azure SignalR.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-559">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| <span data-ttu-id="3b5f4-560">`withHeader``withHeaders`</span><span class="sxs-lookup"><span data-stu-id="3b5f4-560">`withHeader` `withHeaders`</span></span> | <span data-ttu-id="3b5f4-561">Puste</span><span class="sxs-lookup"><span data-stu-id="3b5f4-561">Empty</span></span> | <span data-ttu-id="3b5f4-562">Mapa dodatkowych nagłówków HTTP do wysłania w każdym żądaniu HTTP.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-562">A Map of additional HTTP headers to send with every HTTP request.</span></span> |

---

<span data-ttu-id="3b5f4-563">W kliencie .NET opcje te można modyfikować za pomocą delegata opcji udostępnianego do `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="3b5f4-563">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="3b5f4-564">W kliencie JavaScript te opcje można podać w obiekcie JavaScript udostępnionym do `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="3b5f4-564">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

<span data-ttu-id="3b5f4-565">W kliencie Java Opcje te można skonfigurować przy użyciu metod na `HttpHubConnectionBuilder` zwracanych z `HubConnectionBuilder.create("HUB URL")`</span><span class="sxs-lookup"><span data-stu-id="3b5f4-565">In the Java client, these options can be configured with the methods on the `HttpHubConnectionBuilder` returned from the `HubConnectionBuilder.create("HUB URL")`</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
        .withHeader("Foo", "Bar")
        .shouldSkipNegotiate(true)
        .withHandshakeResponseTimeout(30*1000)
        .build();
```

## <a name="additional-resources"></a><span data-ttu-id="3b5f4-566">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="3b5f4-566">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>

::: moniker-end
::: moniker range="< aspnetcore-2.2"

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="3b5f4-567">Opcje serializacji JSON/MessagePack</span><span class="sxs-lookup"><span data-stu-id="3b5f4-567">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="3b5f4-568">ASP.NET Core SignalR obsługuje dwa protokoły dla komunikatów kodowania: [JSON](https://www.json.org/) i [MessagePack](https://msgpack.org/index.html).</span><span class="sxs-lookup"><span data-stu-id="3b5f4-568">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="3b5f4-569">Każdy protokół ma opcje konfiguracji serializacji.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-569">Each protocol has serialization configuration options.</span></span>

<span data-ttu-id="3b5f4-570">Serializacji JSON można skonfigurować na serwerze przy użyciu metody rozszerzenia [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) , która może zostać dodana po metodzie [addsygnalizująca](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-570">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method, which can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in your `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="3b5f4-571">Metoda `AddJsonProtocol` przyjmuje delegata, który odbiera obiekt `options`.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-571">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="3b5f4-572">Właściwość [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) tego obiektu jest obiektem JSON.NET `JsonSerializerSettings`, którego można użyć do skonfigurowania serializacji argumentów i zwracanych wartości.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-572">The [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) property on that object is a JSON.NET `JsonSerializerSettings` object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="3b5f4-573">Aby uzyskać więcej informacji, zapoznaj się z [dokumentacją JSON.NET](https://www.newtonsoft.com/json/help/html/Introduction.htm).</span><span class="sxs-lookup"><span data-stu-id="3b5f4-573">For more information, see the [JSON.NET documentation](https://www.newtonsoft.com/json/help/html/Introduction.htm).</span></span>
 
<span data-ttu-id="3b5f4-574">Przykładowo, aby skonfigurować serializator do używania nazw właściwości "PascalCase" zamiast domyślnych nazw "camelCase", należy użyć następującego kodu w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="3b5f4-574">As an example, to configure the serializer to use "PascalCase" property names, instead of the default "camelCase" names, use the following code in `Startup.ConfigureServices`:</span></span>
 
```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver =
            new DefaultContractResolver();
    });
```

<span data-ttu-id="3b5f4-575">W kliencie .NET ta sama Metoda rozszerzenia `AddJsonProtocol` istnieje w [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="3b5f4-575">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="3b5f4-576">Aby rozwiązać metodę rozszerzenia, należy zaimportować przestrzeń nazw `Microsoft.Extensions.DependencyInjection`:</span><span class="sxs-lookup"><span data-stu-id="3b5f4-576">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

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
> <span data-ttu-id="3b5f4-577">W tej chwili nie można skonfigurować serializacji JSON w kliencie JavaScript.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-577">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="3b5f4-578">Opcje serializacji MessagePack</span><span class="sxs-lookup"><span data-stu-id="3b5f4-578">MessagePack serialization options</span></span>

<span data-ttu-id="3b5f4-579">Serializacji MessagePack można skonfigurować, dostarczając delegata do wywołania [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) .</span><span class="sxs-lookup"><span data-stu-id="3b5f4-579">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="3b5f4-580">Aby uzyskać więcej informacji, zobacz [MessagePack w SignalR](xref:signalr/messagepackhubprotocol) .</span><span class="sxs-lookup"><span data-stu-id="3b5f4-580">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="3b5f4-581">W tej chwili nie można skonfigurować serializacji MessagePack w kliencie JavaScript.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-581">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="3b5f4-582">Konfigurowanie opcji serwera</span><span class="sxs-lookup"><span data-stu-id="3b5f4-582">Configure server options</span></span>

<span data-ttu-id="3b5f4-583">W poniższej tabeli opisano opcje konfigurowania centrów SignalR:</span><span class="sxs-lookup"><span data-stu-id="3b5f4-583">The following table describes options for configuring SignalR hubs:</span></span>

| <span data-ttu-id="3b5f4-584">Opcja</span><span class="sxs-lookup"><span data-stu-id="3b5f4-584">Option</span></span> | <span data-ttu-id="3b5f4-585">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="3b5f4-585">Default Value</span></span> | <span data-ttu-id="3b5f4-586">Opis</span><span class="sxs-lookup"><span data-stu-id="3b5f4-586">Description</span></span> |
| ------ | ------------- | ----------- |
| `HandshakeTimeout` | <span data-ttu-id="3b5f4-587">15 sekund</span><span class="sxs-lookup"><span data-stu-id="3b5f4-587">15 seconds</span></span> | <span data-ttu-id="3b5f4-588">Jeśli klient nie wyśle początkowego komunikatu uzgadniania w tym przedziale czasu, połączenie zostanie zamknięte.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-588">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="3b5f4-589">Jest to ustawienie zaawansowane, które należy zmodyfikować tylko w przypadku wystąpienia błędów limitu czasu uzgadniania z powodu poważnego opóźnienia sieci.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-589">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="3b5f4-590">Aby uzyskać więcej szczegółów na temat procesu uzgadniania, zobacz [Specyfikacja protokołuSignalR Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="3b5f4-590">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="3b5f4-591">15 sekund</span><span class="sxs-lookup"><span data-stu-id="3b5f4-591">15 seconds</span></span> | <span data-ttu-id="3b5f4-592">Jeśli serwer nie wysłał komunikatu w tym interwale, zostanie automatycznie wysłana wiadomość ping w celu pozostawienia otwartego połączenia.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-592">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="3b5f4-593">Podczas zmiany `KeepAliveInterval`Zmień ustawienie `ServerTimeout`/`serverTimeoutInMilliseconds` na kliencie.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-593">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="3b5f4-594">Zalecana wartość `ServerTimeout`/`serverTimeoutInMilliseconds` jest podwójna `KeepAliveInterval` wartość.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-594">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="3b5f4-595">Wszystkie zainstalowane protokoły</span><span class="sxs-lookup"><span data-stu-id="3b5f4-595">All installed protocols</span></span> | <span data-ttu-id="3b5f4-596">Protokoły obsługiwane przez to centrum.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-596">Protocols supported by this hub.</span></span> <span data-ttu-id="3b5f4-597">Domyślnie wszystkie protokoły zarejestrowane na serwerze są dozwolone, ale protokoły można usunąć z tej listy, aby wyłączyć określone protokoły dla poszczególnych centrów.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-597">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="3b5f4-598">Jeśli `true`, szczegółowe komunikaty o wyjątkach są zwracane do klientów, gdy wyjątek jest zgłaszany w metodzie centrum.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-598">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="3b5f4-599">Wartość domyślna to `false`, ponieważ te komunikaty o wyjątkach mogą zawierać informacje poufne.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-599">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

<span data-ttu-id="3b5f4-600">Opcje można skonfigurować dla wszystkich centrów, dostarczając opcje delegata do wywołania `AddSignalR` w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-600">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

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

<span data-ttu-id="3b5f4-601">Opcje jednego centrum przesłaniają opcje globalne dostępne w `AddSignalR` i można je skonfigurować za pomocą <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span><span class="sxs-lookup"><span data-stu-id="3b5f4-601">Options for a single hub override the global options provided in `AddSignalR` and can be configured using <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

### <a name="advanced-http-configuration-options"></a><span data-ttu-id="3b5f4-602">Zaawansowane opcje konfiguracji HTTP</span><span class="sxs-lookup"><span data-stu-id="3b5f4-602">Advanced HTTP configuration options</span></span>

<span data-ttu-id="3b5f4-603">Użyj `HttpConnectionDispatcherOptions`, aby skonfigurować zaawansowane ustawienia związane z transportami i zarządzaniem buforem pamięci.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-603">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="3b5f4-604">Te opcje są konfigurowane przez przekazanie delegata do [MapHub\<t >](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) w `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-604">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) in `Startup.Configure`.</span></span>

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

<span data-ttu-id="3b5f4-605">W poniższej tabeli opisano opcje konfigurowania zaawansowanych opcji protokołu HTTP w programie ASP.NET Core SignalR:</span><span class="sxs-lookup"><span data-stu-id="3b5f4-605">The following table describes options for configuring ASP.NET Core SignalR's advanced HTTP options:</span></span>

| <span data-ttu-id="3b5f4-606">Opcja</span><span class="sxs-lookup"><span data-stu-id="3b5f4-606">Option</span></span> | <span data-ttu-id="3b5f4-607">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="3b5f4-607">Default Value</span></span> | <span data-ttu-id="3b5f4-608">Opis</span><span class="sxs-lookup"><span data-stu-id="3b5f4-608">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="3b5f4-609">32 KB</span><span class="sxs-lookup"><span data-stu-id="3b5f4-609">32 KB</span></span> | <span data-ttu-id="3b5f4-610">Maksymalna liczba bajtów odebranych od klienta, które buforuje serwer.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-610">The maximum number of bytes received from the client that the server buffers.</span></span> <span data-ttu-id="3b5f4-611">Zwiększenie tej wartości umożliwia serwerowi otrzymywanie większych komunikatów, ale może mieć negatywny wpływ na użycie pamięci.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-611">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="3b5f4-612">Dane zbierane automatycznie z `Authorize` atrybuty zastosowane do klasy centrum.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-612">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="3b5f4-613">Lista obiektów [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) służąca do określenia, czy klient jest autoryzowany do łączenia się z centrum.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-613">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="3b5f4-614">32 KB</span><span class="sxs-lookup"><span data-stu-id="3b5f4-614">32 KB</span></span> | <span data-ttu-id="3b5f4-615">Maksymalna liczba bajtów wysłanych przez aplikację, które buforują serwer.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-615">The maximum number of bytes sent by the app that the server buffers.</span></span> <span data-ttu-id="3b5f4-616">Zwiększenie tej wartości umożliwia serwerowi wysyłanie większych komunikatów, ale może mieć negatywny wpływ na użycie pamięci.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-616">Increasing this value allows the server to send larger messages, but can negatively impact memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="3b5f4-617">Wszystkie transporty są włączone.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-617">All Transports are enabled.</span></span> | <span data-ttu-id="3b5f4-618">Wyliczenie flag bitowych wartości `HttpTransportType`, które mogą ograniczyć transporty, z których może korzystać klient do nawiązywania połączeń.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-618">A bit flags enum of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="3b5f4-619">Zobacz poniżej.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-619">See below.</span></span> | <span data-ttu-id="3b5f4-620">Dodatkowe opcje specyficzne dla długiego transportu sondowania.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-620">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="3b5f4-621">Zobacz poniżej.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-621">See below.</span></span> | <span data-ttu-id="3b5f4-622">Dodatkowe opcje specyficzne dla transportu obiektów WebSockets.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-622">Additional options specific to the WebSockets transport.</span></span> |

<span data-ttu-id="3b5f4-623">W przypadku długiego transportu sondowania dostępne są dodatkowe opcje, które można skonfigurować przy użyciu właściwości `LongPolling`:</span><span class="sxs-lookup"><span data-stu-id="3b5f4-623">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="3b5f4-624">Opcja</span><span class="sxs-lookup"><span data-stu-id="3b5f4-624">Option</span></span> | <span data-ttu-id="3b5f4-625">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="3b5f4-625">Default Value</span></span> | <span data-ttu-id="3b5f4-626">Opis</span><span class="sxs-lookup"><span data-stu-id="3b5f4-626">Description</span></span> |
| ------ | ------------- | ----------- |
| `PollTimeout` | <span data-ttu-id="3b5f4-627">90 sekund</span><span class="sxs-lookup"><span data-stu-id="3b5f4-627">90 seconds</span></span> | <span data-ttu-id="3b5f4-628">Maksymalny czas, przez jaki serwer czeka na wysłanie wiadomości do klienta przed zakończeniem jednego żądania sondowania.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-628">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="3b5f4-629">Zmniejszenie tej wartości powoduje, że klient wystawia nowe żądania sondy częściej.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-629">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> |

<span data-ttu-id="3b5f4-630">Transport WebSocket ma dodatkowe opcje, które można skonfigurować za pomocą właściwości `WebSockets`:</span><span class="sxs-lookup"><span data-stu-id="3b5f4-630">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="3b5f4-631">Opcja</span><span class="sxs-lookup"><span data-stu-id="3b5f4-631">Option</span></span> | <span data-ttu-id="3b5f4-632">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="3b5f4-632">Default Value</span></span> | <span data-ttu-id="3b5f4-633">Opis</span><span class="sxs-lookup"><span data-stu-id="3b5f4-633">Description</span></span> |
| ------ | ------------- | ----------- |
| `CloseTimeout` | <span data-ttu-id="3b5f4-634">5 sekund</span><span class="sxs-lookup"><span data-stu-id="3b5f4-634">5 seconds</span></span> | <span data-ttu-id="3b5f4-635">Gdy serwer zostanie zamknięty, jeśli klient nie zostanie zamknięty w tym okresie, połączenie zostanie przerwane.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-635">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | `null` | <span data-ttu-id="3b5f4-636">Delegat, którego można użyć do ustawienia nagłówka `Sec-WebSocket-Protocol` na wartość niestandardową.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-636">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="3b5f4-637">Delegat otrzymuje wartości żądane przez klienta jako dane wejściowe i oczekuje na zwrócenie żądanej wartości.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-637">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="3b5f4-638">Konfigurowanie opcji klienta</span><span class="sxs-lookup"><span data-stu-id="3b5f4-638">Configure client options</span></span>

<span data-ttu-id="3b5f4-639">Opcje klienta można skonfigurować dla typu `HubConnectionBuilder` (dostępnego w klientach .NET i JavaScript).</span><span class="sxs-lookup"><span data-stu-id="3b5f4-639">Client options can be configured on the `HubConnectionBuilder` type (available in the .NET and JavaScript clients).</span></span> <span data-ttu-id="3b5f4-640">Jest ona również dostępna w kliencie Java, ale podklasa `HttpHubConnectionBuilder` zawiera opcje konfiguracji konstruktora, a także `HubConnection` samego siebie.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-640">It's also available in the Java client, but the `HttpHubConnectionBuilder` subclass is what contains the builder configuration options, as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="3b5f4-641">Konfigurowanie rejestrowania</span><span class="sxs-lookup"><span data-stu-id="3b5f4-641">Configure logging</span></span>

<span data-ttu-id="3b5f4-642">Rejestrowanie jest konfigurowane w kliencie .NET przy użyciu metody `ConfigureLogging`.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-642">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="3b5f4-643">Dostawcy i filtry rejestrowania mogą być rejestrowane w taki sam sposób, w jaki znajdują się na serwerze.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-643">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="3b5f4-644">Aby uzyskać więcej informacji, zobacz dokumentację dotyczącą [rejestrowania w ASP.NET Core](xref:fundamentals/logging/index) .</span><span class="sxs-lookup"><span data-stu-id="3b5f4-644">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="3b5f4-645">Aby zarejestrować dostawców rejestrowania, należy zainstalować wymagane pakiety.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-645">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="3b5f4-646">Aby zapoznać się z pełną listą, zobacz sekcję [wbudowane dostawcy rejestrowania](xref:fundamentals/logging/index#built-in-logging-providers) w witrynie docs.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-646">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="3b5f4-647">Aby na przykład włączyć rejestrowanie konsoli, należy zainstalować pakiet NuGet `Microsoft.Extensions.Logging.Console`.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-647">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="3b5f4-648">Wywołaj metodę rozszerzenia `AddConsole`:</span><span class="sxs-lookup"><span data-stu-id="3b5f4-648">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="3b5f4-649">W kliencie JavaScript istnieje podobna Metoda `configureLogging`.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-649">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="3b5f4-650">Podaj wartość `LogLevel` wskazującą minimalny poziom komunikatów dziennika do wygenerowania.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-650">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="3b5f4-651">Dzienniki są zapisywane w oknie konsoli przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-651">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

> [!NOTE]
> <span data-ttu-id="3b5f4-652">Aby całkowicie wyłączyć rejestrowanie, określ `signalR.LogLevel.None` w `configureLogging` metodzie.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-652">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="3b5f4-653">Aby uzyskać więcej informacji na temat rejestrowania, zobacz [dokumentację diagnostykiSignalR](xref:signalr/diagnostics).</span><span class="sxs-lookup"><span data-stu-id="3b5f4-653">For more information on logging, see the [SignalR Diagnostics documentation](xref:signalr/diagnostics).</span></span>

<span data-ttu-id="3b5f4-654">Klient języka Java SignalR używa biblioteki [SLF4J](https://www.slf4j.org/) do rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-654">The SignalR Java client uses the [SLF4J](https://www.slf4j.org/) library for logging.</span></span> <span data-ttu-id="3b5f4-655">Jest to interfejs API rejestrowania wysokiego poziomu, który umożliwia użytkownikom biblioteki wybranie własnych implementacji rejestrowania przez nałożenie określonej zależności rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-655">It's a high-level logging API that allows users of the library to chose their own specific logging implementation by bringing in a specific logging dependency.</span></span> <span data-ttu-id="3b5f4-656">Poniższy fragment kodu przedstawia sposób korzystania z `java.util.logging` z klientem SignalR Java.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-656">The following code snippet shows how to use `java.util.logging` with the SignalR Java client.</span></span>

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

<span data-ttu-id="3b5f4-657">Jeśli nie skonfigurujesz rejestrowania w zależnościach, SLF4J ładuje domyślny Rejestrator braku operacji z następującym komunikatem ostrzegawczym:</span><span class="sxs-lookup"><span data-stu-id="3b5f4-657">If you don't configure logging in your dependencies, SLF4J loads a default no-operation logger with the following warning message:</span></span>

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

<span data-ttu-id="3b5f4-658">Można je bezpiecznie zignorować.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-658">This can safely be ignored.</span></span>

### <a name="configure-allowed-transports"></a><span data-ttu-id="3b5f4-659">Skonfiguruj dozwolone transporty</span><span class="sxs-lookup"><span data-stu-id="3b5f4-659">Configure allowed transports</span></span>

<span data-ttu-id="3b5f4-660">Transporty używane przez SignalR można skonfigurować w wywołaniu `WithUrl` (`withUrl` w języku JavaScript).</span><span class="sxs-lookup"><span data-stu-id="3b5f4-660">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="3b5f4-661">Wartości `HttpTransportType` mogą być używane w celu ograniczenia klienta do używania tylko określonych transportów.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-661">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="3b5f4-662">Wszystkie transporty są domyślnie włączone.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-662">All transports are enabled by default.</span></span>

<span data-ttu-id="3b5f4-663">Na przykład, aby wyłączyć transport zdarzeń wysłanych przez serwer, ale zezwolić na połączenia z usługą WebSockets i długie sondowanie:</span><span class="sxs-lookup"><span data-stu-id="3b5f4-663">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="3b5f4-664">W kliencie JavaScript transporty są konfigurowane przez ustawienie pola `transport` w obiekcie Options udostępnianym `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="3b5f4-664">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

### <a name="configure-bearer-authentication"></a><span data-ttu-id="3b5f4-665">Konfigurowanie uwierzytelniania okaziciela</span><span class="sxs-lookup"><span data-stu-id="3b5f4-665">Configure bearer authentication</span></span>

<span data-ttu-id="3b5f4-666">Aby zapewnić dane uwierzytelniania wraz z SignalR żądaniami, użyj opcji `AccessTokenProvider` (`accessTokenFactory` w języku JavaScript), aby określić funkcję, która zwraca żądany token dostępu.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-666">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="3b5f4-667">W kliencie .NET token dostępu jest przenoszona jako token "uwierzytelnianie okaziciela" protokołu HTTP (przy użyciu nagłówka `Authorization` z typem `Bearer`).</span><span class="sxs-lookup"><span data-stu-id="3b5f4-667">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="3b5f4-668">W kliencie JavaScript token dostępu jest używany jako token okaziciela, **z wyjątkiem** kilku przypadków, w których interfejsy API przeglądarki ograniczają możliwość zastosowania nagłówków (w konkretnym przypadku zdarzeń wysyłanych przez serwer i żądań WebSockets).</span><span class="sxs-lookup"><span data-stu-id="3b5f4-668">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="3b5f4-669">W takich przypadkach token dostępu jest dostarczany jako wartość ciągu zapytania `access_token`.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-669">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="3b5f4-670">W kliencie .NET opcja `AccessTokenProvider` można określić za pomocą delegata opcji w `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="3b5f4-670">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

<span data-ttu-id="3b5f4-671">W kliencie JavaScript token dostępu jest konfigurowany przez ustawienie pola `accessTokenFactory` w obiekcie Options w `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="3b5f4-671">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

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

<span data-ttu-id="3b5f4-672">W kliencie SignalR Java można skonfigurować token okaziciela do użycia na potrzeby uwierzytelniania, dostarczając fabrykę tokenów dostępu do [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span><span class="sxs-lookup"><span data-stu-id="3b5f4-672">In the SignalR Java client, you can configure a bearer token to use for authentication by providing an access token factory to the [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span></span> <span data-ttu-id="3b5f4-673">Użyj [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) , aby podać [](https://github.com/ReactiveX/RxJava) [\<pojedynczego ciągu](https://reactivex.io/documentation/single.html).</span><span class="sxs-lookup"><span data-stu-id="3b5f4-673">Use [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) to provide an [RxJava](https://github.com/ReactiveX/RxJava) [Single\<String>](https://reactivex.io/documentation/single.html).</span></span> <span data-ttu-id="3b5f4-674">Wywołanie metody [Single. Ustąp](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-)umożliwia zapisanie logiki w celu utworzenia tokenów dostępu dla klienta.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-674">With a call to [Single.defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), you can write logic to produce access tokens for your client.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="3b5f4-675">Konfigurowanie opcji limitu czasu i utrzymywania aktywności</span><span class="sxs-lookup"><span data-stu-id="3b5f4-675">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="3b5f4-676">W celu skonfigurowania zachowania przekroczenia limitu czasu i utrzymywania aktywności są dostępne dodatkowe opcje dla samego obiektu `HubConnection`:</span><span class="sxs-lookup"><span data-stu-id="3b5f4-676">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>

# <a name="nettabdotnet"></a>[<span data-ttu-id="3b5f4-677">.NET</span><span class="sxs-lookup"><span data-stu-id="3b5f4-677">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="3b5f4-678">Opcja</span><span class="sxs-lookup"><span data-stu-id="3b5f4-678">Option</span></span> | <span data-ttu-id="3b5f4-679">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="3b5f4-679">Default value</span></span> | <span data-ttu-id="3b5f4-680">Opis</span><span class="sxs-lookup"><span data-stu-id="3b5f4-680">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="3b5f4-681">30 sekund (30 000 MS)</span><span class="sxs-lookup"><span data-stu-id="3b5f4-681">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="3b5f4-682">Limit czasu dla działania serwera.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-682">Timeout for server activity.</span></span> <span data-ttu-id="3b5f4-683">Jeśli serwer nie wysłał komunikatu w tym interwale, Klient uważa, że serwer odłączył się i wyzwala zdarzenie `Closed` (`onclose` w języku JavaScript).</span><span class="sxs-lookup"><span data-stu-id="3b5f4-683">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="3b5f4-684">Ta wartość musi być wystarczająca do wysłania wiadomości ping z serwera **i** odebrana przez klienta w ramach przedziału czasu.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-684">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="3b5f4-685">Zalecana wartość to liczba z co najmniej podwójną wartością `KeepAliveInterval` serwera, aby umożliwić czas na nadejście poleceń ping.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-685">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="3b5f4-686">15 sekund</span><span class="sxs-lookup"><span data-stu-id="3b5f4-686">15 seconds</span></span> | <span data-ttu-id="3b5f4-687">Limit czasu początkowej uzgadniania z serwerem.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-687">Timeout for initial server handshake.</span></span> <span data-ttu-id="3b5f4-688">Jeśli serwer nie wyśle odpowiedzi uzgadniania w tym interwale, klient anuluje uzgadnianie i wyzwala zdarzenie `Closed` (`onclose` w języku JavaScript).</span><span class="sxs-lookup"><span data-stu-id="3b5f4-688">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="3b5f4-689">Jest to ustawienie zaawansowane, które należy zmodyfikować tylko w przypadku wystąpienia błędów limitu czasu uzgadniania z powodu poważnego opóźnienia sieci.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-689">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="3b5f4-690">Aby uzyskać więcej szczegółów na temat procesu uzgadniania, zobacz [Specyfikacja protokołuSignalR Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="3b5f4-690">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

<span data-ttu-id="3b5f4-691">W kliencie .NET wartości limitu czasu są określane jako wartości `TimeSpan`.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-691">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="3b5f4-692">JavaScript</span><span class="sxs-lookup"><span data-stu-id="3b5f4-692">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="3b5f4-693">Opcja</span><span class="sxs-lookup"><span data-stu-id="3b5f4-693">Option</span></span> | <span data-ttu-id="3b5f4-694">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="3b5f4-694">Default value</span></span> | <span data-ttu-id="3b5f4-695">Opis</span><span class="sxs-lookup"><span data-stu-id="3b5f4-695">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="3b5f4-696">30 sekund (30 000 MS)</span><span class="sxs-lookup"><span data-stu-id="3b5f4-696">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="3b5f4-697">Limit czasu dla działania serwera.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-697">Timeout for server activity.</span></span> <span data-ttu-id="3b5f4-698">Jeśli serwer nie wysłał komunikatu w tym interwale, Klient uważa, że serwer odłączył się i wyzwala zdarzenie `onclose`.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-698">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="3b5f4-699">Ta wartość musi być wystarczająca do wysłania wiadomości ping z serwera **i** odebrana przez klienta w ramach przedziału czasu.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-699">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="3b5f4-700">Zalecana wartość to liczba z co najmniej podwójną wartością `KeepAliveInterval` serwera, aby umożliwić czas na nadejście poleceń ping.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-700">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="3b5f4-701">Java</span><span class="sxs-lookup"><span data-stu-id="3b5f4-701">Java</span></span>](#tab/java)

| <span data-ttu-id="3b5f4-702">Opcja</span><span class="sxs-lookup"><span data-stu-id="3b5f4-702">Option</span></span> | <span data-ttu-id="3b5f4-703">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="3b5f4-703">Default value</span></span> | <span data-ttu-id="3b5f4-704">Opis</span><span class="sxs-lookup"><span data-stu-id="3b5f4-704">Description</span></span> |
| ------ | ------------- | ----------- |
| <span data-ttu-id="3b5f4-705">`getServerTimeout` / `setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="3b5f4-705">`getServerTimeout` / `setServerTimeout`</span></span> | <span data-ttu-id="3b5f4-706">30 sekund (30 000 MS)</span><span class="sxs-lookup"><span data-stu-id="3b5f4-706">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="3b5f4-707">Limit czasu dla działania serwera.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-707">Timeout for server activity.</span></span> <span data-ttu-id="3b5f4-708">Jeśli serwer nie wysłał komunikatu w tym interwale, Klient uważa, że serwer odłączył się i wyzwala zdarzenie `onClose`.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-708">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="3b5f4-709">Ta wartość musi być wystarczająca do wysłania wiadomości ping z serwera **i** odebrana przez klienta w ramach przedziału czasu.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-709">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="3b5f4-710">Zalecana wartość to liczba z co najmniej podwójną wartością `KeepAliveInterval` serwera, aby umożliwić czas na nadejście poleceń ping.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-710">The recommended value is a number at least double the server's `KeepAliveInterval` value, to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="3b5f4-711">15 sekund</span><span class="sxs-lookup"><span data-stu-id="3b5f4-711">15 seconds</span></span> | <span data-ttu-id="3b5f4-712">Limit czasu początkowej uzgadniania z serwerem.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-712">Timeout for initial server handshake.</span></span> <span data-ttu-id="3b5f4-713">Jeśli serwer nie wyśle odpowiedzi uzgadniania w tym interwale, klient anuluje uzgadnianie i wyzwala zdarzenie `onClose`.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-713">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="3b5f4-714">Jest to ustawienie zaawansowane, które należy zmodyfikować tylko w przypadku wystąpienia błędów limitu czasu uzgadniania z powodu poważnego opóźnienia sieci.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-714">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="3b5f4-715">Aby uzyskać więcej szczegółów na temat procesu uzgadniania, zobacz [Specyfikacja protokołuSignalR Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="3b5f4-715">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

---

### <a name="configure-additional-options"></a><span data-ttu-id="3b5f4-716">Konfigurowanie opcji dodatkowych</span><span class="sxs-lookup"><span data-stu-id="3b5f4-716">Configure additional options</span></span>

<span data-ttu-id="3b5f4-717">Dodatkowe opcje można skonfigurować w metodzie `WithUrl` (`withUrl` w języku JavaScript) na `HubConnectionBuilder` lub w różnych interfejsach API konfiguracji na `HttpHubConnectionBuilder` klienta Java:</span><span class="sxs-lookup"><span data-stu-id="3b5f4-717">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder` or on the various configuration APIs on the `HttpHubConnectionBuilder` in the Java client:</span></span>

# <a name="nettabdotnet"></a>[<span data-ttu-id="3b5f4-718">.NET</span><span class="sxs-lookup"><span data-stu-id="3b5f4-718">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="3b5f4-719">Opcja .NET</span><span class="sxs-lookup"><span data-stu-id="3b5f4-719">.NET Option</span></span> |  <span data-ttu-id="3b5f4-720">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="3b5f4-720">Default value</span></span> | <span data-ttu-id="3b5f4-721">Opis</span><span class="sxs-lookup"><span data-stu-id="3b5f4-721">Description</span></span> |
| ----------- | -------------- | ----------- |
| `AccessTokenProvider` | `null` | <span data-ttu-id="3b5f4-722">Funkcja zwracająca ciąg, który jest dostarczany jako token uwierzytelniania okaziciela w żądaniach HTTP.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-722">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `false` | <span data-ttu-id="3b5f4-723">Ustaw tę wartość na `true`, aby pominąć etap negocjacji.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-723">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="3b5f4-724">**Obsługiwane tylko wtedy, gdy transport Sockets jest jedynym włączonym transportem**.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-724">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="3b5f4-725">Nie można włączyć tego ustawienia w przypadku korzystania z usługi Azure SignalR.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-725">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="3b5f4-726">Puste</span><span class="sxs-lookup"><span data-stu-id="3b5f4-726">Empty</span></span> | <span data-ttu-id="3b5f4-727">Kolekcja certyfikatów TLS do wysłania w celu uwierzytelniania żądań.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-727">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="3b5f4-728">Puste</span><span class="sxs-lookup"><span data-stu-id="3b5f4-728">Empty</span></span> | <span data-ttu-id="3b5f4-729">Kolekcja plików cookie protokołu HTTP do wysłania w każdym żądaniu HTTP.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-729">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="3b5f4-730">Puste</span><span class="sxs-lookup"><span data-stu-id="3b5f4-730">Empty</span></span> | <span data-ttu-id="3b5f4-731">Poświadczenia do wysyłania przy każdym żądaniu HTTP.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-731">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="3b5f4-732">5 sekund</span><span class="sxs-lookup"><span data-stu-id="3b5f4-732">5 seconds</span></span> | <span data-ttu-id="3b5f4-733">Tylko obiekty WebSockets.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-733">WebSockets only.</span></span> <span data-ttu-id="3b5f4-734">Maksymalny czas oczekiwania klienta po zamknięciu serwera w celu potwierdzenia żądania zamknięcia.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-734">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="3b5f4-735">Jeśli serwer nie potwierdzi zamknięcia w tym czasie, klient zostanie rozłączony.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-735">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="3b5f4-736">Puste</span><span class="sxs-lookup"><span data-stu-id="3b5f4-736">Empty</span></span> | <span data-ttu-id="3b5f4-737">Mapa dodatkowych nagłówków HTTP do wysłania w każdym żądaniu HTTP.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-737">A Map of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | `null` | <span data-ttu-id="3b5f4-738">Delegat, który może służyć do konfigurowania lub zastępowania `HttpMessageHandler` używany do wysyłania żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-738">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="3b5f4-739">Nieużywane na potrzeby połączeń protokołu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-739">Not used for WebSocket connections.</span></span> <span data-ttu-id="3b5f4-740">Ten delegat musi zwracać wartość różną od null i otrzymuje wartość domyślną jako parametr.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-740">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="3b5f4-741">Zmodyfikuj ustawienia tej wartości domyślnej i zwróć ją lub Zwróć nowe wystąpienie `HttpMessageHandler`.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-741">Either modify settings on that default value and return it, or return a new `HttpMessageHandler` instance.</span></span> <span data-ttu-id="3b5f4-742">**Podczas zastępowania programu obsługi należy skopiować ustawienia, które mają być zachowane z podanej procedury obsługi. w przeciwnym razie skonfigurowane opcje (takie jak pliki cookie i nagłówki) nie będą miały zastosowania do nowego programu obsługi.**</span><span class="sxs-lookup"><span data-stu-id="3b5f4-742">**When replacing the handler make sure to copy the settings you want to keep from the provided handler, otherwise, the configured options (such as Cookies and Headers) won't apply to the new handler.**</span></span> |
| `Proxy` | `null` | <span data-ttu-id="3b5f4-743">Serwer proxy HTTP, który ma być używany podczas wysyłania żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-743">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | `false` | <span data-ttu-id="3b5f4-744">Ustaw tę wartość logiczną, aby wysyłać domyślne poświadczenia dla żądań HTTP i WebSockets.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-744">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="3b5f4-745">Umożliwia to korzystanie z uwierzytelniania systemu Windows.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-745">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | `null` | <span data-ttu-id="3b5f4-746">Delegat, który może służyć do konfigurowania dodatkowych opcji protokołu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-746">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="3b5f4-747">Odbiera wystąpienie elementu [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) , którego można użyć do skonfigurowania opcji.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-747">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="3b5f4-748">JavaScript</span><span class="sxs-lookup"><span data-stu-id="3b5f4-748">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="3b5f4-749">Opcja języka JavaScript</span><span class="sxs-lookup"><span data-stu-id="3b5f4-749">JavaScript Option</span></span> | <span data-ttu-id="3b5f4-750">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="3b5f4-750">Default Value</span></span> | <span data-ttu-id="3b5f4-751">Opis</span><span class="sxs-lookup"><span data-stu-id="3b5f4-751">Description</span></span> |
| ----------------- | ------------- | ----------- |
| `accessTokenFactory` | `null` | <span data-ttu-id="3b5f4-752">Funkcja zwracająca ciąg, który jest dostarczany jako token uwierzytelniania okaziciela w żądaniach HTTP.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-752">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `skipNegotiation` | `false` | <span data-ttu-id="3b5f4-753">Ustaw tę wartość na `true`, aby pominąć etap negocjacji.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-753">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="3b5f4-754">**Obsługiwane tylko wtedy, gdy transport Sockets jest jedynym włączonym transportem**.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-754">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="3b5f4-755">Nie można włączyć tego ustawienia w przypadku korzystania z usługi Azure SignalR.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-755">This setting can't be enabled when using the Azure SignalR Service.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="3b5f4-756">Java</span><span class="sxs-lookup"><span data-stu-id="3b5f4-756">Java</span></span>](#tab/java)

| <span data-ttu-id="3b5f4-757">Opcja Java</span><span class="sxs-lookup"><span data-stu-id="3b5f4-757">Java Option</span></span> | <span data-ttu-id="3b5f4-758">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="3b5f4-758">Default Value</span></span> | <span data-ttu-id="3b5f4-759">Opis</span><span class="sxs-lookup"><span data-stu-id="3b5f4-759">Description</span></span> |
| ----------- | ------------- | ----------- |
| `withAccessTokenProvider` | `null` | <span data-ttu-id="3b5f4-760">Funkcja zwracająca ciąg, który jest dostarczany jako token uwierzytelniania okaziciela w żądaniach HTTP.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-760">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `shouldSkipNegotiate` | `false` | <span data-ttu-id="3b5f4-761">Ustaw tę wartość na `true`, aby pominąć etap negocjacji.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-761">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="3b5f4-762">**Obsługiwane tylko wtedy, gdy transport Sockets jest jedynym włączonym transportem**.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-762">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="3b5f4-763">Nie można włączyć tego ustawienia w przypadku korzystania z usługi Azure SignalR.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-763">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| <span data-ttu-id="3b5f4-764">`withHeader``withHeaders`</span><span class="sxs-lookup"><span data-stu-id="3b5f4-764">`withHeader` `withHeaders`</span></span> | <span data-ttu-id="3b5f4-765">Puste</span><span class="sxs-lookup"><span data-stu-id="3b5f4-765">Empty</span></span> | <span data-ttu-id="3b5f4-766">Mapa dodatkowych nagłówków HTTP do wysłania w każdym żądaniu HTTP.</span><span class="sxs-lookup"><span data-stu-id="3b5f4-766">A Map of additional HTTP headers to send with every HTTP request.</span></span> |

---

<span data-ttu-id="3b5f4-767">W kliencie .NET opcje te można modyfikować za pomocą delegata opcji udostępnianego do `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="3b5f4-767">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="3b5f4-768">W kliencie JavaScript te opcje można podać w obiekcie JavaScript udostępnionym do `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="3b5f4-768">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

<span data-ttu-id="3b5f4-769">W kliencie Java Opcje te można skonfigurować przy użyciu metod na `HttpHubConnectionBuilder` zwracanych z `HubConnectionBuilder.create("HUB URL")`</span><span class="sxs-lookup"><span data-stu-id="3b5f4-769">In the Java client, these options can be configured with the methods on the `HttpHubConnectionBuilder` returned from the `HubConnectionBuilder.create("HUB URL")`</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
        .withHeader("Foo", "Bar")
        .shouldSkipNegotiate(true)
        .withHandshakeResponseTimeout(30*1000)
        .build();
```

## <a name="additional-resources"></a><span data-ttu-id="3b5f4-770">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="3b5f4-770">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>

::: moniker-end