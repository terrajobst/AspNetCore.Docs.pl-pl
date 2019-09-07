---
title: Konfiguracja sygnałów ASP.NET Core
author: bradygaster
description: Dowiedz się, jak skonfigurować aplikacje sygnalizujące ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 08/05/2019
uid: signalr/configuration
ms.openlocfilehash: 156ffac83fbdf61fd88ad8acc307c2c701c46bca
ms.sourcegitcommit: f65d8765e4b7c894481db9b37aa6969abc625a48
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/06/2019
ms.locfileid: "70773928"
---
# <a name="aspnet-core-signalr-configuration"></a><span data-ttu-id="48641-103">Konfiguracja sygnałów ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="48641-103">ASP.NET Core SignalR configuration</span></span>

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="48641-104">Opcje serializacji JSON/MessagePack</span><span class="sxs-lookup"><span data-stu-id="48641-104">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="48641-105">ASP.NET Core sygnalizujący obsługuje dwa protokoły dla komunikatów kodowania: [JSON](https://www.json.org/) i [MessagePack](https://msgpack.org/index.html).</span><span class="sxs-lookup"><span data-stu-id="48641-105">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="48641-106">Każdy protokół ma opcje konfiguracji serializacji.</span><span class="sxs-lookup"><span data-stu-id="48641-106">Each protocol has serialization configuration options.</span></span>

<span data-ttu-id="48641-107">Serializacji JSON można skonfigurować na serwerze przy użyciu metody rozszerzenia [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) , która może zostać dodana po `Startup.ConfigureServices` metodzie [addsygnalizująca](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) w ramach metody.</span><span class="sxs-lookup"><span data-stu-id="48641-107">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method, which can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in your `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="48641-108">Metoda przyjmuje delegata, który `options` odbiera obiekt. `AddJsonProtocol`</span><span class="sxs-lookup"><span data-stu-id="48641-108">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="48641-109">Właściwość [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) tego obiektu jest obiektem JSON.NET `JsonSerializerSettings` , którego można użyć do skonfigurowania serializacji argumentów i zwracanych wartości.</span><span class="sxs-lookup"><span data-stu-id="48641-109">The [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) property on that object is a JSON.NET `JsonSerializerSettings` object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="48641-110">Aby uzyskać więcej informacji, zobacz [dokumentację JSON.NET](https://www.newtonsoft.com/json/help/html/Introduction.htm) .</span><span class="sxs-lookup"><span data-stu-id="48641-110">See the [JSON.NET Documentation](https://www.newtonsoft.com/json/help/html/Introduction.htm) for more details.</span></span>

<span data-ttu-id="48641-111">Przykładowo, aby skonfigurować serializator do używania nazw właściwości "PascalCase" zamiast domyślnych nazw "camelCase", należy użyć następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="48641-111">As an example, to configure the serializer to use "PascalCase" property names, instead of the default "camelCase" names, use the following code:</span></span>

```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver =
        new DefaultContractResolver();
    });
```

<span data-ttu-id="48641-112">W kliencie .NET ta sama `AddJsonProtocol` Metoda rozszerzająca istnieje w [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="48641-112">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="48641-113">`Microsoft.Extensions.DependencyInjection` Przestrzeń nazw musi zostać zaimportowana, aby można było rozpoznać metodę rozszerzenia:</span><span class="sxs-lookup"><span data-stu-id="48641-113">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

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
> <span data-ttu-id="48641-114">W tej chwili nie można skonfigurować serializacji JSON w kliencie JavaScript.</span><span class="sxs-lookup"><span data-stu-id="48641-114">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="48641-115">Opcje serializacji MessagePack</span><span class="sxs-lookup"><span data-stu-id="48641-115">MessagePack serialization options</span></span>

<span data-ttu-id="48641-116">Serializacji MessagePack można skonfigurować, dostarczając delegata do wywołania [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) .</span><span class="sxs-lookup"><span data-stu-id="48641-116">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="48641-117">Aby uzyskać więcej informacji, zobacz [MessagePack w sygnalizacji](xref:signalr/messagepackhubprotocol) .</span><span class="sxs-lookup"><span data-stu-id="48641-117">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="48641-118">W tej chwili nie można skonfigurować serializacji MessagePack w kliencie JavaScript.</span><span class="sxs-lookup"><span data-stu-id="48641-118">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="48641-119">Konfigurowanie opcji serwera</span><span class="sxs-lookup"><span data-stu-id="48641-119">Configure server options</span></span>

<span data-ttu-id="48641-120">W poniższej tabeli opisano opcje konfigurowania centrów sygnałów:</span><span class="sxs-lookup"><span data-stu-id="48641-120">The following table describes options for configuring SignalR hubs:</span></span>

::: moniker range=">= aspnetcore-3.0"

| <span data-ttu-id="48641-121">Opcja</span><span class="sxs-lookup"><span data-stu-id="48641-121">Option</span></span> | <span data-ttu-id="48641-122">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="48641-122">Default Value</span></span> | <span data-ttu-id="48641-123">Opis</span><span class="sxs-lookup"><span data-stu-id="48641-123">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="48641-124">30 sekund</span><span class="sxs-lookup"><span data-stu-id="48641-124">30 seconds</span></span> | <span data-ttu-id="48641-125">Serwer Rozważ odłączenie klienta, jeśli nie odebrano komunikatu (w tym w tym interwale).</span><span class="sxs-lookup"><span data-stu-id="48641-125">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="48641-126">Może upłynąć dłużej niż ten interwał limitu czasu, aby klient mógł zostać oznaczony jako odłączony, ze względu na to, jak jest to zaimplementowane.</span><span class="sxs-lookup"><span data-stu-id="48641-126">It could take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="48641-127">Zalecaną wartością jest podwójna `KeepAliveInterval` wartość.</span><span class="sxs-lookup"><span data-stu-id="48641-127">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="48641-128">15 sekund</span><span class="sxs-lookup"><span data-stu-id="48641-128">15 seconds</span></span> | <span data-ttu-id="48641-129">Jeśli klient nie wyśle początkowego komunikatu uzgadniania w tym przedziale czasu, połączenie zostanie zamknięte.</span><span class="sxs-lookup"><span data-stu-id="48641-129">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="48641-130">Jest to ustawienie zaawansowane, które należy zmodyfikować tylko w przypadku wystąpienia błędów limitu czasu uzgadniania z powodu poważnego opóźnienia sieci.</span><span class="sxs-lookup"><span data-stu-id="48641-130">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="48641-131">Aby uzyskać więcej szczegółów na temat procesu uzgadniania, zobacz [specyfikację protokołu centrum sygnału](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="48641-131">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="48641-132">15 sekund</span><span class="sxs-lookup"><span data-stu-id="48641-132">15 seconds</span></span> | <span data-ttu-id="48641-133">Jeśli serwer nie wysłał komunikatu w tym interwale, zostanie automatycznie wysłana wiadomość ping w celu pozostawienia otwartego połączenia.</span><span class="sxs-lookup"><span data-stu-id="48641-133">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="48641-134">W przypadku `KeepAliveInterval`zmiany należy `ServerTimeout` / zmienić`serverTimeoutInMilliseconds` ustawienie na kliencie.</span><span class="sxs-lookup"><span data-stu-id="48641-134">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="48641-135">Zalecaną `ServerTimeout` / wartościąjest`KeepAliveInterval` podwójna wartość. `serverTimeoutInMilliseconds`</span><span class="sxs-lookup"><span data-stu-id="48641-135">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="48641-136">Wszystkie zainstalowane protokoły</span><span class="sxs-lookup"><span data-stu-id="48641-136">All installed protocols</span></span> | <span data-ttu-id="48641-137">Protokoły obsługiwane przez to centrum.</span><span class="sxs-lookup"><span data-stu-id="48641-137">Protocols supported by this hub.</span></span> <span data-ttu-id="48641-138">Domyślnie wszystkie protokoły zarejestrowane na serwerze są dozwolone, ale protokoły można usunąć z tej listy, aby wyłączyć określone protokoły dla poszczególnych centrów.</span><span class="sxs-lookup"><span data-stu-id="48641-138">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="48641-139">Jeśli `true`szczegółowe komunikaty o wyjątkach są zwracane do klientów, gdy wyjątek jest zgłaszany w metodzie centrum.</span><span class="sxs-lookup"><span data-stu-id="48641-139">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="48641-140">Wartość domyślna to `false`, ponieważ te komunikaty o wyjątkach mogą zawierać informacje poufne.</span><span class="sxs-lookup"><span data-stu-id="48641-140">The default is `false`, as these exception messages can contain sensitive information.</span></span> |
| `StreamBufferCapacity` | `10` | <span data-ttu-id="48641-141">Maksymalna liczba elementów, które mogą być buforowane dla strumieni przekazywania klientów.</span><span class="sxs-lookup"><span data-stu-id="48641-141">The maximum number of items that can be buffered for client upload streams.</span></span> <span data-ttu-id="48641-142">Po osiągnięciu tego limitu przetwarzanie wywołań jest blokowane, dopóki serwer nie przetwarza elementów strumienia.</span><span class="sxs-lookup"><span data-stu-id="48641-142">If this limit is reached, the processing of invocations is blocked until the server processes stream items.</span></span>|
| `MaximumReceiveMessageSize` | <span data-ttu-id="48641-143">32 KB</span><span class="sxs-lookup"><span data-stu-id="48641-143">32 KB</span></span> | <span data-ttu-id="48641-144">Maksymalny rozmiar pojedynczego przychodzącego komunikatu centrum.</span><span class="sxs-lookup"><span data-stu-id="48641-144">Maximum size of a single incoming hub message.</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-2.2"

| <span data-ttu-id="48641-145">Opcja</span><span class="sxs-lookup"><span data-stu-id="48641-145">Option</span></span> | <span data-ttu-id="48641-146">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="48641-146">Default Value</span></span> | <span data-ttu-id="48641-147">Opis</span><span class="sxs-lookup"><span data-stu-id="48641-147">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="48641-148">30 sekund</span><span class="sxs-lookup"><span data-stu-id="48641-148">30 seconds</span></span> | <span data-ttu-id="48641-149">Serwer Rozważ odłączenie klienta, jeśli nie odebrano komunikatu (w tym w tym interwale).</span><span class="sxs-lookup"><span data-stu-id="48641-149">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="48641-150">Może upłynąć dłużej niż ten interwał limitu czasu, aby klient mógł zostać oznaczony jako odłączony, ze względu na to, jak jest to zaimplementowane.</span><span class="sxs-lookup"><span data-stu-id="48641-150">It could take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="48641-151">Zalecaną wartością jest podwójna `KeepAliveInterval` wartość.</span><span class="sxs-lookup"><span data-stu-id="48641-151">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="48641-152">15 sekund</span><span class="sxs-lookup"><span data-stu-id="48641-152">15 seconds</span></span> | <span data-ttu-id="48641-153">Jeśli klient nie wyśle początkowego komunikatu uzgadniania w tym przedziale czasu, połączenie zostanie zamknięte.</span><span class="sxs-lookup"><span data-stu-id="48641-153">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="48641-154">Jest to ustawienie zaawansowane, które należy zmodyfikować tylko w przypadku wystąpienia błędów limitu czasu uzgadniania z powodu poważnego opóźnienia sieci.</span><span class="sxs-lookup"><span data-stu-id="48641-154">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="48641-155">Aby uzyskać więcej szczegółów na temat procesu uzgadniania, zobacz [specyfikację protokołu centrum sygnału](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="48641-155">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="48641-156">15 sekund</span><span class="sxs-lookup"><span data-stu-id="48641-156">15 seconds</span></span> | <span data-ttu-id="48641-157">Jeśli serwer nie wysłał komunikatu w tym interwale, zostanie automatycznie wysłana wiadomość ping w celu pozostawienia otwartego połączenia.</span><span class="sxs-lookup"><span data-stu-id="48641-157">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="48641-158">W przypadku `KeepAliveInterval`zmiany należy `ServerTimeout` / zmienić`serverTimeoutInMilliseconds` ustawienie na kliencie.</span><span class="sxs-lookup"><span data-stu-id="48641-158">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="48641-159">Zalecaną `ServerTimeout` / wartościąjest`KeepAliveInterval` podwójna wartość. `serverTimeoutInMilliseconds`</span><span class="sxs-lookup"><span data-stu-id="48641-159">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="48641-160">Wszystkie zainstalowane protokoły</span><span class="sxs-lookup"><span data-stu-id="48641-160">All installed protocols</span></span> | <span data-ttu-id="48641-161">Protokoły obsługiwane przez to centrum.</span><span class="sxs-lookup"><span data-stu-id="48641-161">Protocols supported by this hub.</span></span> <span data-ttu-id="48641-162">Domyślnie wszystkie protokoły zarejestrowane na serwerze są dozwolone, ale protokoły można usunąć z tej listy, aby wyłączyć określone protokoły dla poszczególnych centrów.</span><span class="sxs-lookup"><span data-stu-id="48641-162">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="48641-163">Jeśli `true`szczegółowe komunikaty o wyjątkach są zwracane do klientów, gdy wyjątek jest zgłaszany w metodzie centrum.</span><span class="sxs-lookup"><span data-stu-id="48641-163">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="48641-164">Wartość domyślna to `false`, ponieważ te komunikaty o wyjątkach mogą zawierać informacje poufne.</span><span class="sxs-lookup"><span data-stu-id="48641-164">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-2.1"

| <span data-ttu-id="48641-165">Opcja</span><span class="sxs-lookup"><span data-stu-id="48641-165">Option</span></span> | <span data-ttu-id="48641-166">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="48641-166">Default Value</span></span> | <span data-ttu-id="48641-167">Opis</span><span class="sxs-lookup"><span data-stu-id="48641-167">Description</span></span> |
| ------ | ------------- | ----------- |
| `HandshakeTimeout` | <span data-ttu-id="48641-168">15 sekund</span><span class="sxs-lookup"><span data-stu-id="48641-168">15 seconds</span></span> | <span data-ttu-id="48641-169">Jeśli klient nie wyśle początkowego komunikatu uzgadniania w tym przedziale czasu, połączenie zostanie zamknięte.</span><span class="sxs-lookup"><span data-stu-id="48641-169">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="48641-170">Jest to ustawienie zaawansowane, które należy zmodyfikować tylko w przypadku wystąpienia błędów limitu czasu uzgadniania z powodu poważnego opóźnienia sieci.</span><span class="sxs-lookup"><span data-stu-id="48641-170">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="48641-171">Aby uzyskać więcej szczegółów na temat procesu uzgadniania, zobacz [specyfikację protokołu centrum sygnału](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="48641-171">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="48641-172">15 sekund</span><span class="sxs-lookup"><span data-stu-id="48641-172">15 seconds</span></span> | <span data-ttu-id="48641-173">Jeśli serwer nie wysłał komunikatu w tym interwale, zostanie automatycznie wysłana wiadomość ping w celu pozostawienia otwartego połączenia.</span><span class="sxs-lookup"><span data-stu-id="48641-173">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="48641-174">W przypadku `KeepAliveInterval`zmiany należy `ServerTimeout` / zmienić`serverTimeoutInMilliseconds` ustawienie na kliencie.</span><span class="sxs-lookup"><span data-stu-id="48641-174">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="48641-175">Zalecaną `ServerTimeout` / wartościąjest`KeepAliveInterval` podwójna wartość. `serverTimeoutInMilliseconds`</span><span class="sxs-lookup"><span data-stu-id="48641-175">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="48641-176">Wszystkie zainstalowane protokoły</span><span class="sxs-lookup"><span data-stu-id="48641-176">All installed protocols</span></span> | <span data-ttu-id="48641-177">Protokoły obsługiwane przez to centrum.</span><span class="sxs-lookup"><span data-stu-id="48641-177">Protocols supported by this hub.</span></span> <span data-ttu-id="48641-178">Domyślnie wszystkie protokoły zarejestrowane na serwerze są dozwolone, ale protokoły można usunąć z tej listy, aby wyłączyć określone protokoły dla poszczególnych centrów.</span><span class="sxs-lookup"><span data-stu-id="48641-178">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="48641-179">Jeśli `true`szczegółowe komunikaty o wyjątkach są zwracane do klientów, gdy wyjątek jest zgłaszany w metodzie centrum.</span><span class="sxs-lookup"><span data-stu-id="48641-179">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="48641-180">Wartość domyślna to `false`, ponieważ te komunikaty o wyjątkach mogą zawierać informacje poufne.</span><span class="sxs-lookup"><span data-stu-id="48641-180">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

::: moniker-end

<span data-ttu-id="48641-181">Opcje można skonfigurować dla wszystkich centrów, dostarczając opcje delegata do `AddSignalR` wywołania w. `Startup.ConfigureServices`</span><span class="sxs-lookup"><span data-stu-id="48641-181">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

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

<span data-ttu-id="48641-182">Opcje jednego centrum przesłaniają opcje globalne podane w `AddSignalR` i można je skonfigurować przy użyciu: <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*></span><span class="sxs-lookup"><span data-stu-id="48641-182">Options for a single hub override the global options provided in `AddSignalR` and can be configured using <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

### <a name="advanced-http-configuration-options"></a><span data-ttu-id="48641-183">Zaawansowane opcje konfiguracji HTTP</span><span class="sxs-lookup"><span data-stu-id="48641-183">Advanced HTTP configuration options</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="48641-184">Służy `HttpConnectionDispatcherOptions` do konfigurowania ustawień zaawansowanych związanych z transportami i zarządzaniem buforem pamięci.</span><span class="sxs-lookup"><span data-stu-id="48641-184">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="48641-185">Te opcje są konfigurowane przez przekazanie delegata [do\<MapHub T >](/dotnet/api/microsoft.aspnetcore.builder.hubendpointroutebuilderextensions.maphub) w. `Startup.Configure`</span><span class="sxs-lookup"><span data-stu-id="48641-185">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.builder.hubendpointroutebuilderextensions.maphub) in `Startup.Configure`.</span></span>

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

<span data-ttu-id="48641-186">Służy `HttpConnectionDispatcherOptions` do konfigurowania ustawień zaawansowanych związanych z transportami i zarządzaniem buforem pamięci.</span><span class="sxs-lookup"><span data-stu-id="48641-186">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="48641-187">Te opcje są konfigurowane przez przekazanie delegata [do\<MapHub T >](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) w. `Startup.Configure`</span><span class="sxs-lookup"><span data-stu-id="48641-187">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) in `Startup.Configure`.</span></span>

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

<span data-ttu-id="48641-188">W poniższej tabeli opisano opcje konfigurowania zaawansowanych opcji protokołu HTTP w programie ASP.NET Core Signal:</span><span class="sxs-lookup"><span data-stu-id="48641-188">The following table describes options for configuring ASP.NET Core SignalR's advanced HTTP options:</span></span>

::: moniker range=">= aspnetcore-3.0"

| <span data-ttu-id="48641-189">Opcja</span><span class="sxs-lookup"><span data-stu-id="48641-189">Option</span></span> | <span data-ttu-id="48641-190">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="48641-190">Default Value</span></span> | <span data-ttu-id="48641-191">Opis</span><span class="sxs-lookup"><span data-stu-id="48641-191">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="48641-192">32 KB</span><span class="sxs-lookup"><span data-stu-id="48641-192">32 KB</span></span> | <span data-ttu-id="48641-193">Maksymalna liczba bajtów odebranych od klienta, które buforują serwer przed zastosowaniem nadużycia.</span><span class="sxs-lookup"><span data-stu-id="48641-193">The maximum number of bytes received from the client that the server buffers before applying backpressure.</span></span> <span data-ttu-id="48641-194">Zwiększenie tej wartości umożliwia serwerowi otrzymywanie większych komunikatów szybciej bez naciskania, ale może zwiększyć zużycie pamięci.</span><span class="sxs-lookup"><span data-stu-id="48641-194">Increasing this value allows the server to receive larger messages more quickly without applying backpressure, but can increase memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="48641-195">Dane zbierane automatycznie z `Authorize` atrybutów zastosowanych do klasy centrum.</span><span class="sxs-lookup"><span data-stu-id="48641-195">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="48641-196">Lista obiektów [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) służąca do określenia, czy klient jest autoryzowany do łączenia się z centrum.</span><span class="sxs-lookup"><span data-stu-id="48641-196">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="48641-197">32 KB</span><span class="sxs-lookup"><span data-stu-id="48641-197">32 KB</span></span> | <span data-ttu-id="48641-198">Maksymalna liczba bajtów wysłanych przez aplikację, które buforuje serwer przed zaobserwowaniem presji.</span><span class="sxs-lookup"><span data-stu-id="48641-198">The maximum number of bytes sent by the app that the server buffers before observing backpressure.</span></span> <span data-ttu-id="48641-199">Zwiększenie tej wartości umożliwia serwerowi przebuforowanie większych komunikatów szybciej bez czekania na obciążenie, ale może zwiększyć zużycie pamięci.</span><span class="sxs-lookup"><span data-stu-id="48641-199">Increasing this value allows the server to buffer larger messages more quickly without awaiting backpressure, but can increase memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="48641-200">Wszystkie transporty są włączone.</span><span class="sxs-lookup"><span data-stu-id="48641-200">All Transports are enabled.</span></span> | <span data-ttu-id="48641-201">Wyliczenie flag bitowych `HttpTransportType` wartości, które mogą ograniczyć transporty, których klient może użyć do nawiązania połączenia.</span><span class="sxs-lookup"><span data-stu-id="48641-201">A bit flags enum of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="48641-202">Zobacz poniżej.</span><span class="sxs-lookup"><span data-stu-id="48641-202">See below.</span></span> | <span data-ttu-id="48641-203">Dodatkowe opcje specyficzne dla długiego transportu sondowania.</span><span class="sxs-lookup"><span data-stu-id="48641-203">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="48641-204">Zobacz poniżej.</span><span class="sxs-lookup"><span data-stu-id="48641-204">See below.</span></span> | <span data-ttu-id="48641-205">Dodatkowe opcje specyficzne dla transportu obiektów WebSockets.</span><span class="sxs-lookup"><span data-stu-id="48641-205">Additional options specific to the WebSockets transport.</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-3.0"

| <span data-ttu-id="48641-206">Opcja</span><span class="sxs-lookup"><span data-stu-id="48641-206">Option</span></span> | <span data-ttu-id="48641-207">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="48641-207">Default Value</span></span> | <span data-ttu-id="48641-208">Opis</span><span class="sxs-lookup"><span data-stu-id="48641-208">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="48641-209">32 KB</span><span class="sxs-lookup"><span data-stu-id="48641-209">32 KB</span></span> | <span data-ttu-id="48641-210">Maksymalna liczba bajtów odebranych od klienta, które buforuje serwer.</span><span class="sxs-lookup"><span data-stu-id="48641-210">The maximum number of bytes received from the client that the server buffers.</span></span> <span data-ttu-id="48641-211">Zwiększenie tej wartości umożliwia serwerowi otrzymywanie większych komunikatów, ale może mieć negatywny wpływ na użycie pamięci.</span><span class="sxs-lookup"><span data-stu-id="48641-211">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="48641-212">Dane zbierane automatycznie z `Authorize` atrybutów zastosowanych do klasy centrum.</span><span class="sxs-lookup"><span data-stu-id="48641-212">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="48641-213">Lista obiektów [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) służąca do określenia, czy klient jest autoryzowany do łączenia się z centrum.</span><span class="sxs-lookup"><span data-stu-id="48641-213">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="48641-214">32 KB</span><span class="sxs-lookup"><span data-stu-id="48641-214">32 KB</span></span> | <span data-ttu-id="48641-215">Maksymalna liczba bajtów wysłanych przez aplikację, które buforują serwer.</span><span class="sxs-lookup"><span data-stu-id="48641-215">The maximum number of bytes sent by the app that the server buffers.</span></span> <span data-ttu-id="48641-216">Zwiększenie tej wartości umożliwia serwerowi wysyłanie większych komunikatów, ale może mieć negatywny wpływ na użycie pamięci.</span><span class="sxs-lookup"><span data-stu-id="48641-216">Increasing this value allows the server to send larger messages, but can negatively impact memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="48641-217">Wszystkie transporty są włączone.</span><span class="sxs-lookup"><span data-stu-id="48641-217">All Transports are enabled.</span></span> | <span data-ttu-id="48641-218">Wyliczenie flag bitowych `HttpTransportType` wartości, które mogą ograniczyć transporty, których klient może użyć do nawiązania połączenia.</span><span class="sxs-lookup"><span data-stu-id="48641-218">A bit flags enum of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="48641-219">Zobacz poniżej.</span><span class="sxs-lookup"><span data-stu-id="48641-219">See below.</span></span> | <span data-ttu-id="48641-220">Dodatkowe opcje specyficzne dla długiego transportu sondowania.</span><span class="sxs-lookup"><span data-stu-id="48641-220">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="48641-221">Zobacz poniżej.</span><span class="sxs-lookup"><span data-stu-id="48641-221">See below.</span></span> | <span data-ttu-id="48641-222">Dodatkowe opcje specyficzne dla transportu obiektów WebSockets.</span><span class="sxs-lookup"><span data-stu-id="48641-222">Additional options specific to the WebSockets transport.</span></span> |

::: moniker-end

<span data-ttu-id="48641-223">W przypadku długiego transportu sondowania dostępne są dodatkowe opcje, które można `LongPolling` skonfigurować przy użyciu właściwości:</span><span class="sxs-lookup"><span data-stu-id="48641-223">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="48641-224">Opcja</span><span class="sxs-lookup"><span data-stu-id="48641-224">Option</span></span> | <span data-ttu-id="48641-225">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="48641-225">Default Value</span></span> | <span data-ttu-id="48641-226">Opis</span><span class="sxs-lookup"><span data-stu-id="48641-226">Description</span></span> |
| ------ | ------------- | ----------- |
| `PollTimeout` | <span data-ttu-id="48641-227">90 sekund</span><span class="sxs-lookup"><span data-stu-id="48641-227">90 seconds</span></span> | <span data-ttu-id="48641-228">Maksymalny czas, przez jaki serwer czeka na wysłanie wiadomości do klienta przed zakończeniem jednego żądania sondowania.</span><span class="sxs-lookup"><span data-stu-id="48641-228">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="48641-229">Zmniejszenie tej wartości powoduje, że klient wystawia nowe żądania sondy częściej.</span><span class="sxs-lookup"><span data-stu-id="48641-229">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> |

<span data-ttu-id="48641-230">Transport WebSocket ma dodatkowe opcje, które można skonfigurować przy użyciu `WebSockets` właściwości:</span><span class="sxs-lookup"><span data-stu-id="48641-230">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="48641-231">Opcja</span><span class="sxs-lookup"><span data-stu-id="48641-231">Option</span></span> | <span data-ttu-id="48641-232">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="48641-232">Default Value</span></span> | <span data-ttu-id="48641-233">Opis</span><span class="sxs-lookup"><span data-stu-id="48641-233">Description</span></span> |
| ------ | ------------- | ----------- |
| `CloseTimeout` | <span data-ttu-id="48641-234">5 sekund</span><span class="sxs-lookup"><span data-stu-id="48641-234">5 seconds</span></span> | <span data-ttu-id="48641-235">Gdy serwer zostanie zamknięty, jeśli klient nie zostanie zamknięty w tym okresie, połączenie zostanie przerwane.</span><span class="sxs-lookup"><span data-stu-id="48641-235">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | `null` | <span data-ttu-id="48641-236">Delegat, którego można użyć do ustawienia `Sec-WebSocket-Protocol` nagłówka na wartość niestandardową.</span><span class="sxs-lookup"><span data-stu-id="48641-236">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="48641-237">Delegat otrzymuje wartości żądane przez klienta jako dane wejściowe i oczekuje na zwrócenie żądanej wartości.</span><span class="sxs-lookup"><span data-stu-id="48641-237">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="48641-238">Konfigurowanie opcji klienta</span><span class="sxs-lookup"><span data-stu-id="48641-238">Configure client options</span></span>

<span data-ttu-id="48641-239">Opcje klienta można skonfigurować dla `HubConnectionBuilder` typu (dostępnego w klientach .NET i JavaScript).</span><span class="sxs-lookup"><span data-stu-id="48641-239">Client options can be configured on the `HubConnectionBuilder` type (available in the .NET and JavaScript clients).</span></span> <span data-ttu-id="48641-240">Jest ona również dostępna w kliencie Java, ale `HttpHubConnectionBuilder` podklasa zawiera opcje konfiguracji konstruktora, a także `HubConnection` samego siebie.</span><span class="sxs-lookup"><span data-stu-id="48641-240">It's also available in the Java client, but the `HttpHubConnectionBuilder` subclass is what contains the builder configuration options, as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="48641-241">Konfigurowanie rejestrowania</span><span class="sxs-lookup"><span data-stu-id="48641-241">Configure logging</span></span>

<span data-ttu-id="48641-242">Rejestrowanie jest konfigurowane w kliencie .NET przy użyciu `ConfigureLogging` metody.</span><span class="sxs-lookup"><span data-stu-id="48641-242">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="48641-243">Dostawcy i filtry rejestrowania mogą być rejestrowane w taki sam sposób, w jaki znajdują się na serwerze.</span><span class="sxs-lookup"><span data-stu-id="48641-243">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="48641-244">Aby uzyskać więcej informacji, zobacz dokumentację dotyczącą [rejestrowania w ASP.NET Core](xref:fundamentals/logging/index) .</span><span class="sxs-lookup"><span data-stu-id="48641-244">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="48641-245">Aby zarejestrować dostawców rejestrowania, należy zainstalować wymagane pakiety.</span><span class="sxs-lookup"><span data-stu-id="48641-245">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="48641-246">Aby zapoznać się z pełną listą, zobacz sekcję [wbudowane dostawcy rejestrowania](xref:fundamentals/logging/index#built-in-logging-providers) w witrynie docs.</span><span class="sxs-lookup"><span data-stu-id="48641-246">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="48641-247">Na przykład aby włączyć rejestrowanie konsoli, zainstaluj `Microsoft.Extensions.Logging.Console` pakiet NuGet.</span><span class="sxs-lookup"><span data-stu-id="48641-247">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="48641-248">Wywołaj `AddConsole` metodę rozszerzenia:</span><span class="sxs-lookup"><span data-stu-id="48641-248">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="48641-249">W kliencie JavaScript istnieje podobna `configureLogging` Metoda.</span><span class="sxs-lookup"><span data-stu-id="48641-249">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="48641-250">`LogLevel` Podaj wartość wskazującą minimalny poziom komunikatów dziennika do wygenerowania.</span><span class="sxs-lookup"><span data-stu-id="48641-250">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="48641-251">Dzienniki są zapisywane w oknie konsoli przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="48641-251">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="48641-252">Zamiast wartości można także podać wartość reprezentującą nazwę poziomu dziennika. `string` `LogLevel`</span><span class="sxs-lookup"><span data-stu-id="48641-252">Instead of a `LogLevel` value, you can also provide a `string` value representing a log level name.</span></span> <span data-ttu-id="48641-253">Jest to przydatne podczas konfigurowania rejestrowania sygnałów w środowiskach, w których nie masz dostępu do `LogLevel` stałych.</span><span class="sxs-lookup"><span data-stu-id="48641-253">This is useful when configuring SignalR logging in environments where you don't have access to the `LogLevel` constants.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging("warn")
    .build();
```

<span data-ttu-id="48641-254">Poniższa tabela zawiera listę dostępnych poziomów dzienników.</span><span class="sxs-lookup"><span data-stu-id="48641-254">The following table lists the available log levels.</span></span> <span data-ttu-id="48641-255">Wartość określana przez użytkownika `configureLogging` w celu ustawienia **minimalnego** poziomu dziennika, który zostanie zarejestrowany.</span><span class="sxs-lookup"><span data-stu-id="48641-255">The value you provide to `configureLogging` sets the **minimum** log level that will be logged.</span></span> <span data-ttu-id="48641-256">Komunikaty zarejestrowane na tym poziomie **lub poziomy wymienione po nim w tabeli**zostaną zarejestrowane.</span><span class="sxs-lookup"><span data-stu-id="48641-256">Messages logged at this level, **or the levels listed after it in the table**, will be logged.</span></span>

| <span data-ttu-id="48641-257">String</span><span class="sxs-lookup"><span data-stu-id="48641-257">String</span></span>                      | <span data-ttu-id="48641-258">LogLevel</span><span class="sxs-lookup"><span data-stu-id="48641-258">LogLevel</span></span>               |
| --------------------------- | ---------------------- |
| `trace`                     | `LogLevel.Trace`       |
| `debug`                     | `LogLevel.Debug`       |
| <span data-ttu-id="48641-259">`info`**lub**`information`</span><span class="sxs-lookup"><span data-stu-id="48641-259">`info` **or** `information`</span></span> | `LogLevel.Information` |
| <span data-ttu-id="48641-260">`warn`**lub**`warning`</span><span class="sxs-lookup"><span data-stu-id="48641-260">`warn` **or** `warning`</span></span>     | `LogLevel.Warning`     |
| `error`                     | `LogLevel.Error`       |
| `critical`                  | `LogLevel.Critical`    |
| `none`                      | `LogLevel.None`        |

::: moniker-end

> [!NOTE]
> <span data-ttu-id="48641-261">Aby całkowicie wyłączyć rejestrowanie, określ `signalR.LogLevel.None` `configureLogging` w metodzie.</span><span class="sxs-lookup"><span data-stu-id="48641-261">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="48641-262">Aby uzyskać więcej informacji na temat rejestrowania, zobacz [dokumentację diagnostyki sygnału](xref:signalr/diagnostics).</span><span class="sxs-lookup"><span data-stu-id="48641-262">For more information on logging, see the [SignalR Diagnostics documentation](xref:signalr/diagnostics).</span></span>

<span data-ttu-id="48641-263">Klient Java sygnalizujący używa biblioteki [SLF4J](https://www.slf4j.org/) do rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="48641-263">The SignalR Java client uses the [SLF4J](https://www.slf4j.org/) library for logging.</span></span> <span data-ttu-id="48641-264">Jest to interfejs API rejestrowania wysokiego poziomu, który umożliwia użytkownikom biblioteki wybranie własnych implementacji rejestrowania przez nałożenie określonej zależności rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="48641-264">It's a high-level logging API that allows users of the library to chose their own specific logging implementation by bringing in a specific logging dependency.</span></span> <span data-ttu-id="48641-265">Poniższy fragment kodu przedstawia sposób użycia `java.util.logging` z klientem programu sygnalizującego środowisko Java.</span><span class="sxs-lookup"><span data-stu-id="48641-265">The following code snippet shows how to use `java.util.logging` with the SignalR Java client.</span></span>

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

<span data-ttu-id="48641-266">Jeśli nie skonfigurujesz rejestrowania w zależnościach, SLF4J ładuje domyślny Rejestrator braku operacji z następującym komunikatem ostrzegawczym:</span><span class="sxs-lookup"><span data-stu-id="48641-266">If you don't configure logging in your dependencies, SLF4J loads a default no-operation logger with the following warning message:</span></span>

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

<span data-ttu-id="48641-267">Można je bezpiecznie zignorować.</span><span class="sxs-lookup"><span data-stu-id="48641-267">This can safely be ignored.</span></span>

### <a name="configure-allowed-transports"></a><span data-ttu-id="48641-268">Skonfiguruj dozwolone transporty</span><span class="sxs-lookup"><span data-stu-id="48641-268">Configure allowed transports</span></span>

<span data-ttu-id="48641-269">Transporty używane przez sygnalizującego można skonfigurować w `WithUrl` wywołaniu (`withUrl` w języku JavaScript).</span><span class="sxs-lookup"><span data-stu-id="48641-269">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="48641-270">Wartości bitowe i `HttpTransportType` można użyć, aby ograniczyć klient do używania tylko określonych transportów.</span><span class="sxs-lookup"><span data-stu-id="48641-270">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="48641-271">Wszystkie transporty są domyślnie włączone.</span><span class="sxs-lookup"><span data-stu-id="48641-271">All transports are enabled by default.</span></span>

<span data-ttu-id="48641-272">Na przykład, aby wyłączyć transport zdarzeń wysłanych przez serwer, ale zezwolić na połączenia z usługą WebSockets i długie sondowanie:</span><span class="sxs-lookup"><span data-stu-id="48641-272">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="48641-273">W kliencie JavaScript transporty są konfigurowane przez ustawienie `transport` pola w obiekcie Options dostarczonego do: `withUrl`</span><span class="sxs-lookup"><span data-stu-id="48641-273">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="48641-274">W tej wersji elementu WebSockets klienta Java jest jedynym dostępnym transportem.</span><span class="sxs-lookup"><span data-stu-id="48641-274">In this version of the Java client websockets is the only available transport.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-3.0"

<span data-ttu-id="48641-275">W kliencie Java, transport jest wybierany przy użyciu `withTransport` metody `HttpHubConnectionBuilder`w.</span><span class="sxs-lookup"><span data-stu-id="48641-275">In the Java client, the transport is selected with the `withTransport` method on the `HttpHubConnectionBuilder`.</span></span> <span data-ttu-id="48641-276">Klient Java domyślnie używa transportu usługi WebSockets.</span><span class="sxs-lookup"><span data-stu-id="48641-276">The Java client defaults to using the WebSockets transport.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withTransport(TransportEnum.WEBSOCKETS)
    .build();
```

> [!NOTE]
> <span data-ttu-id="48641-277">Klient Java sygnalizujący nie obsługuje jeszcze rezerwy transportowej.</span><span class="sxs-lookup"><span data-stu-id="48641-277">The SignalR Java client doesn't support transport fallback yet.</span></span>

::: moniker-end

### <a name="configure-bearer-authentication"></a><span data-ttu-id="48641-278">Konfigurowanie uwierzytelniania okaziciela</span><span class="sxs-lookup"><span data-stu-id="48641-278">Configure bearer authentication</span></span>

<span data-ttu-id="48641-279">Aby zapewnić dane uwierzytelniania wraz z żądaniami sygnałów, użyj `AccessTokenProvider` opcji (`accessTokenFactory` w języku JavaScript), aby określić funkcję, która zwraca żądany token dostępu.</span><span class="sxs-lookup"><span data-stu-id="48641-279">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="48641-280">W kliencie .NET token dostępu jest przenoszona jako token "uwierzytelnianie okaziciela" protokołu HTTP (przy użyciu `Authorization` nagłówka z `Bearer`typem).</span><span class="sxs-lookup"><span data-stu-id="48641-280">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="48641-281">W kliencie JavaScript token dostępu jest używany jako token okaziciela, **z wyjątkiem** kilku przypadków, w których interfejsy API przeglądarki ograniczają możliwość zastosowania nagłówków (w konkretnym przypadku zdarzeń wysyłanych przez serwer i żądań WebSockets).</span><span class="sxs-lookup"><span data-stu-id="48641-281">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="48641-282">W takich przypadkach token dostępu jest podawany jako wartość `access_token`ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="48641-282">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="48641-283">W kliencie `AccessTokenProvider` .NET opcji można określić za pomocą delegata opcji w `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="48641-283">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

<span data-ttu-id="48641-284">W kliencie JavaScript token dostępu jest konfigurowany przez ustawienie `accessTokenFactory` pola w obiekcie Options w: `withUrl`</span><span class="sxs-lookup"><span data-stu-id="48641-284">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

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

<span data-ttu-id="48641-285">W kliencie Java programu sygnalizującego można skonfigurować token okaziciela do użycia na potrzeby uwierzytelniania, dostarczając fabrykę tokenów dostępu do [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span><span class="sxs-lookup"><span data-stu-id="48641-285">In the SignalR Java client, you can configure a bearer token to use for authentication by providing an access token factory to the [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span></span> <span data-ttu-id="48641-286">Użyj [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) , aby dostarczyć [RxJava](https://github.com/ReactiveX/RxJava) [pojedynczej\<>](https://reactivex.io/documentation/single.html).</span><span class="sxs-lookup"><span data-stu-id="48641-286">Use [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) to provide an [RxJava](https://github.com/ReactiveX/RxJava) [Single\<String>](https://reactivex.io/documentation/single.html).</span></span> <span data-ttu-id="48641-287">Wywołanie metody [Single. Ustąp](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-)umożliwia zapisanie logiki w celu utworzenia tokenów dostępu dla klienta.</span><span class="sxs-lookup"><span data-stu-id="48641-287">With a call to [Single.defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), you can write logic to produce access tokens for your client.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="48641-288">Konfigurowanie opcji limitu czasu i utrzymywania aktywności</span><span class="sxs-lookup"><span data-stu-id="48641-288">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="48641-289">W `HubConnection` samym obiekcie są dostępne dodatkowe opcje konfigurowania trybu przekroczenia limitu czasu i zachowania utrzymywania aktywności:</span><span class="sxs-lookup"><span data-stu-id="48641-289">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>


::: moniker range=">= aspnetcore-2.2"

# <a name="nettabdotnet"></a>[<span data-ttu-id="48641-290">.NET</span><span class="sxs-lookup"><span data-stu-id="48641-290">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="48641-291">Opcja</span><span class="sxs-lookup"><span data-stu-id="48641-291">Option</span></span> | <span data-ttu-id="48641-292">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="48641-292">Default value</span></span> | <span data-ttu-id="48641-293">Opis</span><span class="sxs-lookup"><span data-stu-id="48641-293">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="48641-294">30 sekund (30 000 MS)</span><span class="sxs-lookup"><span data-stu-id="48641-294">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="48641-295">Limit czasu dla działania serwera.</span><span class="sxs-lookup"><span data-stu-id="48641-295">Timeout for server activity.</span></span> <span data-ttu-id="48641-296">Jeśli serwer nie wysłał komunikatu w tym interwale, Klient uważa, że serwer odłączył się i wyzwala `Closed` zdarzenie (`onclose` w języku JavaScript).</span><span class="sxs-lookup"><span data-stu-id="48641-296">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="48641-297">Ta wartość musi być wystarczająca do wysłania wiadomości ping z serwera **i** odebrana przez klienta w ramach przedziału czasu.</span><span class="sxs-lookup"><span data-stu-id="48641-297">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="48641-298">Zalecana wartość to liczba z co najmniej podwójną `KeepAliveInterval` wartością serwera, aby umożliwić czas na nadejście poleceń ping.</span><span class="sxs-lookup"><span data-stu-id="48641-298">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="48641-299">15 sekund</span><span class="sxs-lookup"><span data-stu-id="48641-299">15 seconds</span></span> | <span data-ttu-id="48641-300">Limit czasu początkowej uzgadniania z serwerem.</span><span class="sxs-lookup"><span data-stu-id="48641-300">Timeout for initial server handshake.</span></span> <span data-ttu-id="48641-301">Jeśli serwer nie wyśle odpowiedzi uzgadniania w tym interwale, klient anuluje uzgadnianie i wyzwala `Closed` zdarzenie (`onclose` w języku JavaScript).</span><span class="sxs-lookup"><span data-stu-id="48641-301">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="48641-302">Jest to ustawienie zaawansowane, które należy zmodyfikować tylko w przypadku wystąpienia błędów limitu czasu uzgadniania z powodu poważnego opóźnienia sieci.</span><span class="sxs-lookup"><span data-stu-id="48641-302">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="48641-303">Aby uzyskać więcej szczegółów na temat procesu uzgadniania, zobacz [specyfikację protokołu centrum sygnału](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="48641-303">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="48641-304">15 sekund</span><span class="sxs-lookup"><span data-stu-id="48641-304">15 seconds</span></span> | <span data-ttu-id="48641-305">Określa interwał wysyłania komunikatów ping przez klienta.</span><span class="sxs-lookup"><span data-stu-id="48641-305">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="48641-306">Wysłanie wszelkich komunikatów z klienta powoduje zresetowanie czasomierza do początku interwału.</span><span class="sxs-lookup"><span data-stu-id="48641-306">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="48641-307">Jeśli klient nie wysłał komunikatu w `ClientTimeoutInterval` zestawie na serwerze, serwer traktuje klienta jako odłączony.</span><span class="sxs-lookup"><span data-stu-id="48641-307">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

<span data-ttu-id="48641-308">W kliencie .NET wartości limitu czasu są określone jako `TimeSpan` wartości.</span><span class="sxs-lookup"><span data-stu-id="48641-308">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="48641-309">JavaScript</span><span class="sxs-lookup"><span data-stu-id="48641-309">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="48641-310">Opcja</span><span class="sxs-lookup"><span data-stu-id="48641-310">Option</span></span> | <span data-ttu-id="48641-311">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="48641-311">Default value</span></span> | <span data-ttu-id="48641-312">Opis</span><span class="sxs-lookup"><span data-stu-id="48641-312">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="48641-313">30 sekund (30 000 MS)</span><span class="sxs-lookup"><span data-stu-id="48641-313">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="48641-314">Limit czasu dla działania serwera.</span><span class="sxs-lookup"><span data-stu-id="48641-314">Timeout for server activity.</span></span> <span data-ttu-id="48641-315">Jeśli serwer nie wysłał komunikatu w tym interwale, Klient uważa, że serwer odłączył się i wyzwala `onclose` zdarzenie.</span><span class="sxs-lookup"><span data-stu-id="48641-315">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="48641-316">Ta wartość musi być wystarczająca do wysłania wiadomości ping z serwera **i** odebrana przez klienta w ramach przedziału czasu.</span><span class="sxs-lookup"><span data-stu-id="48641-316">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="48641-317">Zalecana wartość to liczba z co najmniej podwójną `KeepAliveInterval` wartością serwera, aby umożliwić czas na nadejście poleceń ping.</span><span class="sxs-lookup"><span data-stu-id="48641-317">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `keepAliveIntervalInMilliseconds` | <span data-ttu-id="48641-318">15 sekund (15 000 milisekund)</span><span class="sxs-lookup"><span data-stu-id="48641-318">15 seconds (15,000 miliseconds)</span></span> | <span data-ttu-id="48641-319">Określa interwał wysyłania komunikatów ping przez klienta.</span><span class="sxs-lookup"><span data-stu-id="48641-319">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="48641-320">Wysłanie wszelkich komunikatów z klienta powoduje zresetowanie czasomierza do początku interwału.</span><span class="sxs-lookup"><span data-stu-id="48641-320">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="48641-321">Jeśli klient nie wysłał komunikatu w `ClientTimeoutInterval` zestawie na serwerze, serwer traktuje klienta jako odłączony.</span><span class="sxs-lookup"><span data-stu-id="48641-321">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="48641-322">Java</span><span class="sxs-lookup"><span data-stu-id="48641-322">Java</span></span>](#tab/java)

| <span data-ttu-id="48641-323">Opcja</span><span class="sxs-lookup"><span data-stu-id="48641-323">Option</span></span> | <span data-ttu-id="48641-324">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="48641-324">Default value</span></span> | <span data-ttu-id="48641-325">Opis</span><span class="sxs-lookup"><span data-stu-id="48641-325">Description</span></span> |
| ------ | ------------- | ----------- |
| <span data-ttu-id="48641-326">`getServerTimeout` / `setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="48641-326">`getServerTimeout` / `setServerTimeout`</span></span> | <span data-ttu-id="48641-327">30 sekund (30 000 MS)</span><span class="sxs-lookup"><span data-stu-id="48641-327">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="48641-328">Limit czasu dla działania serwera.</span><span class="sxs-lookup"><span data-stu-id="48641-328">Timeout for server activity.</span></span> <span data-ttu-id="48641-329">Jeśli serwer nie wysłał komunikatu w tym interwale, Klient uważa, że serwer odłączył się i wyzwala `onClose` zdarzenie.</span><span class="sxs-lookup"><span data-stu-id="48641-329">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="48641-330">Ta wartość musi być wystarczająca do wysłania wiadomości ping z serwera **i** odebrana przez klienta w ramach przedziału czasu.</span><span class="sxs-lookup"><span data-stu-id="48641-330">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="48641-331">Zalecana wartość to liczba z co najmniej podwójną `KeepAliveInterval` wartością serwera, aby umożliwić czas na nadejście poleceń ping.</span><span class="sxs-lookup"><span data-stu-id="48641-331">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="48641-332">15 sekund</span><span class="sxs-lookup"><span data-stu-id="48641-332">15 seconds</span></span> | <span data-ttu-id="48641-333">Limit czasu początkowej uzgadniania z serwerem.</span><span class="sxs-lookup"><span data-stu-id="48641-333">Timeout for initial server handshake.</span></span> <span data-ttu-id="48641-334">Jeśli serwer nie wyśle odpowiedzi uzgadniania w tym interwale, klient anuluje uzgadnianie i wyzwala `onClose` zdarzenie.</span><span class="sxs-lookup"><span data-stu-id="48641-334">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="48641-335">Jest to ustawienie zaawansowane, które należy zmodyfikować tylko w przypadku wystąpienia błędów limitu czasu uzgadniania z powodu poważnego opóźnienia sieci.</span><span class="sxs-lookup"><span data-stu-id="48641-335">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="48641-336">Aby uzyskać więcej szczegółów na temat procesu uzgadniania, zobacz [specyfikację protokołu centrum sygnału](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="48641-336">For more detail on the Handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| <span data-ttu-id="48641-337">`getKeepAliveInterval` / `setKeepAliveInterval`</span><span class="sxs-lookup"><span data-stu-id="48641-337">`getKeepAliveInterval` / `setKeepAliveInterval`</span></span> | <span data-ttu-id="48641-338">15 sekund (15 000 MS)</span><span class="sxs-lookup"><span data-stu-id="48641-338">15 seconds (15,000 milliseconds)</span></span> | <span data-ttu-id="48641-339">Określa interwał wysyłania komunikatów ping przez klienta.</span><span class="sxs-lookup"><span data-stu-id="48641-339">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="48641-340">Wysłanie wszelkich komunikatów z klienta powoduje zresetowanie czasomierza do początku interwału.</span><span class="sxs-lookup"><span data-stu-id="48641-340">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="48641-341">Jeśli klient nie wysłał komunikatu w `ClientTimeoutInterval` zestawie na serwerze, serwer traktuje klienta jako odłączony.</span><span class="sxs-lookup"><span data-stu-id="48641-341">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="nettabdotnet"></a>[<span data-ttu-id="48641-342">.NET</span><span class="sxs-lookup"><span data-stu-id="48641-342">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="48641-343">Opcja</span><span class="sxs-lookup"><span data-stu-id="48641-343">Option</span></span> | <span data-ttu-id="48641-344">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="48641-344">Default value</span></span> | <span data-ttu-id="48641-345">Opis</span><span class="sxs-lookup"><span data-stu-id="48641-345">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="48641-346">30 sekund (30 000 MS)</span><span class="sxs-lookup"><span data-stu-id="48641-346">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="48641-347">Limit czasu dla działania serwera.</span><span class="sxs-lookup"><span data-stu-id="48641-347">Timeout for server activity.</span></span> <span data-ttu-id="48641-348">Jeśli serwer nie wysłał komunikatu w tym interwale, Klient uważa, że serwer odłączył się i wyzwala `Closed` zdarzenie (`onclose` w języku JavaScript).</span><span class="sxs-lookup"><span data-stu-id="48641-348">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="48641-349">Ta wartość musi być wystarczająca do wysłania wiadomości ping z serwera **i** odebrana przez klienta w ramach przedziału czasu.</span><span class="sxs-lookup"><span data-stu-id="48641-349">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="48641-350">Zalecana wartość to liczba z co najmniej podwójną `KeepAliveInterval` wartością serwera, aby umożliwić czas na nadejście poleceń ping.</span><span class="sxs-lookup"><span data-stu-id="48641-350">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="48641-351">15 sekund</span><span class="sxs-lookup"><span data-stu-id="48641-351">15 seconds</span></span> | <span data-ttu-id="48641-352">Limit czasu początkowej uzgadniania z serwerem.</span><span class="sxs-lookup"><span data-stu-id="48641-352">Timeout for initial server handshake.</span></span> <span data-ttu-id="48641-353">Jeśli serwer nie wyśle odpowiedzi uzgadniania w tym interwale, klient anuluje uzgadnianie i wyzwala `Closed` zdarzenie (`onclose` w języku JavaScript).</span><span class="sxs-lookup"><span data-stu-id="48641-353">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="48641-354">Jest to ustawienie zaawansowane, które należy zmodyfikować tylko w przypadku wystąpienia błędów limitu czasu uzgadniania z powodu poważnego opóźnienia sieci.</span><span class="sxs-lookup"><span data-stu-id="48641-354">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="48641-355">Aby uzyskać więcej szczegółów na temat procesu uzgadniania, zobacz [specyfikację protokołu centrum sygnału](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="48641-355">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

<span data-ttu-id="48641-356">W kliencie .NET wartości limitu czasu są określone jako `TimeSpan` wartości.</span><span class="sxs-lookup"><span data-stu-id="48641-356">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="48641-357">JavaScript</span><span class="sxs-lookup"><span data-stu-id="48641-357">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="48641-358">Opcja</span><span class="sxs-lookup"><span data-stu-id="48641-358">Option</span></span> | <span data-ttu-id="48641-359">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="48641-359">Default value</span></span> | <span data-ttu-id="48641-360">Opis</span><span class="sxs-lookup"><span data-stu-id="48641-360">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="48641-361">30 sekund (30 000 MS)</span><span class="sxs-lookup"><span data-stu-id="48641-361">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="48641-362">Limit czasu dla działania serwera.</span><span class="sxs-lookup"><span data-stu-id="48641-362">Timeout for server activity.</span></span> <span data-ttu-id="48641-363">Jeśli serwer nie wysłał komunikatu w tym interwale, Klient uważa, że serwer odłączył się i wyzwala `onclose` zdarzenie.</span><span class="sxs-lookup"><span data-stu-id="48641-363">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="48641-364">Ta wartość musi być wystarczająca do wysłania wiadomości ping z serwera **i** odebrana przez klienta w ramach przedziału czasu.</span><span class="sxs-lookup"><span data-stu-id="48641-364">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="48641-365">Zalecana wartość to liczba z co najmniej podwójną `KeepAliveInterval` wartością serwera, aby umożliwić czas na nadejście poleceń ping.</span><span class="sxs-lookup"><span data-stu-id="48641-365">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="48641-366">Java</span><span class="sxs-lookup"><span data-stu-id="48641-366">Java</span></span>](#tab/java)

| <span data-ttu-id="48641-367">Opcja</span><span class="sxs-lookup"><span data-stu-id="48641-367">Option</span></span> | <span data-ttu-id="48641-368">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="48641-368">Default value</span></span> | <span data-ttu-id="48641-369">Opis</span><span class="sxs-lookup"><span data-stu-id="48641-369">Description</span></span> |
| ------ | ------------- | ----------- |
| <span data-ttu-id="48641-370">`getServerTimeout` / `setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="48641-370">`getServerTimeout` / `setServerTimeout`</span></span> | <span data-ttu-id="48641-371">30 sekund (30 000 MS)</span><span class="sxs-lookup"><span data-stu-id="48641-371">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="48641-372">Limit czasu dla działania serwera.</span><span class="sxs-lookup"><span data-stu-id="48641-372">Timeout for server activity.</span></span> <span data-ttu-id="48641-373">Jeśli serwer nie wysłał komunikatu w tym interwale, Klient uważa, że serwer odłączył się i wyzwala `onClose` zdarzenie.</span><span class="sxs-lookup"><span data-stu-id="48641-373">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="48641-374">Ta wartość musi być wystarczająca do wysłania wiadomości ping z serwera **i** odebrana przez klienta w ramach przedziału czasu.</span><span class="sxs-lookup"><span data-stu-id="48641-374">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="48641-375">Zalecana wartość to liczba z co najmniej podwójną `KeepAliveInterval` wartością serwera, aby umożliwić czas na nadejście poleceń ping.</span><span class="sxs-lookup"><span data-stu-id="48641-375">The recommended value is a number at least double the server's `KeepAliveInterval` value, to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="48641-376">15 sekund</span><span class="sxs-lookup"><span data-stu-id="48641-376">15 seconds</span></span> | <span data-ttu-id="48641-377">Limit czasu początkowej uzgadniania z serwerem.</span><span class="sxs-lookup"><span data-stu-id="48641-377">Timeout for initial server handshake.</span></span> <span data-ttu-id="48641-378">Jeśli serwer nie wyśle odpowiedzi uzgadniania w tym interwale, klient anuluje uzgadnianie i wyzwala `onClose` zdarzenie.</span><span class="sxs-lookup"><span data-stu-id="48641-378">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="48641-379">Jest to ustawienie zaawansowane, które należy zmodyfikować tylko w przypadku wystąpienia błędów limitu czasu uzgadniania z powodu poważnego opóźnienia sieci.</span><span class="sxs-lookup"><span data-stu-id="48641-379">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="48641-380">Aby uzyskać więcej szczegółów na temat procesu uzgadniania, zobacz [specyfikację protokołu centrum sygnału](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="48641-380">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

::: moniker-end

---

### <a name="configure-additional-options"></a><span data-ttu-id="48641-381">Skonfiguruj dodatkowe opcje</span><span class="sxs-lookup"><span data-stu-id="48641-381">Configure additional options</span></span>

<span data-ttu-id="48641-382">Dodatkowe opcje `WithUrl` można skonfigurować w metodzie (`withUrl` w języku JavaScript) dla `HubConnectionBuilder` `HttpHubConnectionBuilder` lub w różnych interfejsach API konfiguracji w kliencie Java:</span><span class="sxs-lookup"><span data-stu-id="48641-382">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder` or on the various configuration APIs on the `HttpHubConnectionBuilder` in the Java client:</span></span>

# <a name="nettabdotnet"></a>[<span data-ttu-id="48641-383">.NET</span><span class="sxs-lookup"><span data-stu-id="48641-383">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="48641-384">Opcja .NET</span><span class="sxs-lookup"><span data-stu-id="48641-384">.NET Option</span></span> |  <span data-ttu-id="48641-385">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="48641-385">Default value</span></span> | <span data-ttu-id="48641-386">Opis</span><span class="sxs-lookup"><span data-stu-id="48641-386">Description</span></span> |
| ----------- | -------------- | ----------- |
| `AccessTokenProvider` | `null` | <span data-ttu-id="48641-387">Funkcja zwracająca ciąg, który jest dostarczany jako token uwierzytelniania okaziciela w żądaniach HTTP.</span><span class="sxs-lookup"><span data-stu-id="48641-387">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `false` | <span data-ttu-id="48641-388">Ustaw tę `true` wartość na, aby pominąć krok negocjowania.</span><span class="sxs-lookup"><span data-stu-id="48641-388">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="48641-389">**Obsługiwane tylko wtedy, gdy transport Sockets jest jedynym włączonym transportem**.</span><span class="sxs-lookup"><span data-stu-id="48641-389">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="48641-390">Nie można włączyć tego ustawienia w przypadku korzystania z usługi Azure Signal Service.</span><span class="sxs-lookup"><span data-stu-id="48641-390">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="48641-391">Pusty</span><span class="sxs-lookup"><span data-stu-id="48641-391">Empty</span></span> | <span data-ttu-id="48641-392">Kolekcja certyfikatów TLS do wysłania w celu uwierzytelniania żądań.</span><span class="sxs-lookup"><span data-stu-id="48641-392">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="48641-393">Pusty</span><span class="sxs-lookup"><span data-stu-id="48641-393">Empty</span></span> | <span data-ttu-id="48641-394">Kolekcja plików cookie protokołu HTTP do wysłania w każdym żądaniu HTTP.</span><span class="sxs-lookup"><span data-stu-id="48641-394">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="48641-395">Pusty</span><span class="sxs-lookup"><span data-stu-id="48641-395">Empty</span></span> | <span data-ttu-id="48641-396">Poświadczenia do wysyłania przy każdym żądaniu HTTP.</span><span class="sxs-lookup"><span data-stu-id="48641-396">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="48641-397">5 sekund</span><span class="sxs-lookup"><span data-stu-id="48641-397">5 seconds</span></span> | <span data-ttu-id="48641-398">Tylko obiekty WebSockets.</span><span class="sxs-lookup"><span data-stu-id="48641-398">WebSockets only.</span></span> <span data-ttu-id="48641-399">Maksymalny czas oczekiwania klienta po zamknięciu serwera w celu potwierdzenia żądania zamknięcia.</span><span class="sxs-lookup"><span data-stu-id="48641-399">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="48641-400">Jeśli serwer nie potwierdzi zamknięcia w tym czasie, klient zostanie rozłączony.</span><span class="sxs-lookup"><span data-stu-id="48641-400">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="48641-401">Pusty</span><span class="sxs-lookup"><span data-stu-id="48641-401">Empty</span></span> | <span data-ttu-id="48641-402">Mapa dodatkowych nagłówków HTTP do wysłania w każdym żądaniu HTTP.</span><span class="sxs-lookup"><span data-stu-id="48641-402">A Map of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | `null` | <span data-ttu-id="48641-403">Delegat, który może służyć do konfigurowania lub zastępowania `HttpMessageHandler` używanych do wysyłania żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="48641-403">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="48641-404">Nieużywane na potrzeby połączeń protokołu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="48641-404">Not used for WebSocket connections.</span></span> <span data-ttu-id="48641-405">Ten delegat musi zwracać wartość różną od null i otrzymuje wartość domyślną jako parametr.</span><span class="sxs-lookup"><span data-stu-id="48641-405">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="48641-406">Zmodyfikuj ustawienia tej wartości domyślnej i zwróć ją lub Zwróć nowe `HttpMessageHandler` wystąpienie.</span><span class="sxs-lookup"><span data-stu-id="48641-406">Either modify settings on that default value and return it, or return a new `HttpMessageHandler` instance.</span></span> <span data-ttu-id="48641-407">**Podczas zastępowania programu obsługi należy skopiować ustawienia, które mają być zachowane z podanej procedury obsługi. w przeciwnym razie skonfigurowane opcje (takie jak pliki cookie i nagłówki) nie będą miały zastosowania do nowego programu obsługi.**</span><span class="sxs-lookup"><span data-stu-id="48641-407">**When replacing the handler make sure to copy the settings you want to keep from the provided handler, otherwise, the configured options (such as Cookies and Headers) won't apply to the new handler.**</span></span> |
| `Proxy` | `null` | <span data-ttu-id="48641-408">Serwer proxy HTTP, który ma być używany podczas wysyłania żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="48641-408">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | `false` | <span data-ttu-id="48641-409">Ustaw tę wartość logiczną, aby wysyłać domyślne poświadczenia dla żądań HTTP i WebSockets.</span><span class="sxs-lookup"><span data-stu-id="48641-409">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="48641-410">Umożliwia to korzystanie z uwierzytelniania systemu Windows.</span><span class="sxs-lookup"><span data-stu-id="48641-410">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | `null` | <span data-ttu-id="48641-411">Delegat, który może służyć do konfigurowania dodatkowych opcji protokołu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="48641-411">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="48641-412">Odbiera wystąpienie elementu [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) , którego można użyć do skonfigurowania opcji.</span><span class="sxs-lookup"><span data-stu-id="48641-412">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="48641-413">JavaScript</span><span class="sxs-lookup"><span data-stu-id="48641-413">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="48641-414">Opcja języka JavaScript</span><span class="sxs-lookup"><span data-stu-id="48641-414">JavaScript Option</span></span> | <span data-ttu-id="48641-415">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="48641-415">Default Value</span></span> | <span data-ttu-id="48641-416">Opis</span><span class="sxs-lookup"><span data-stu-id="48641-416">Description</span></span> |
| ----------------- | ------------- | ----------- |
| `accessTokenFactory` | `null` | <span data-ttu-id="48641-417">Funkcja zwracająca ciąg, który jest dostarczany jako token uwierzytelniania okaziciela w żądaniach HTTP.</span><span class="sxs-lookup"><span data-stu-id="48641-417">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `skipNegotiation` | `false` | <span data-ttu-id="48641-418">Ustaw tę `true` wartość na, aby pominąć krok negocjowania.</span><span class="sxs-lookup"><span data-stu-id="48641-418">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="48641-419">**Obsługiwane tylko wtedy, gdy transport Sockets jest jedynym włączonym transportem**.</span><span class="sxs-lookup"><span data-stu-id="48641-419">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="48641-420">Nie można włączyć tego ustawienia w przypadku korzystania z usługi Azure Signal Service.</span><span class="sxs-lookup"><span data-stu-id="48641-420">This setting can't be enabled when using the Azure SignalR Service.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="48641-421">Java</span><span class="sxs-lookup"><span data-stu-id="48641-421">Java</span></span>](#tab/java)

| <span data-ttu-id="48641-422">Opcja Java</span><span class="sxs-lookup"><span data-stu-id="48641-422">Java Option</span></span> | <span data-ttu-id="48641-423">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="48641-423">Default Value</span></span> | <span data-ttu-id="48641-424">Opis</span><span class="sxs-lookup"><span data-stu-id="48641-424">Description</span></span> |
| ----------- | ------------- | ----------- |
| `withAccessTokenProvider` | `null` | <span data-ttu-id="48641-425">Funkcja zwracająca ciąg, który jest dostarczany jako token uwierzytelniania okaziciela w żądaniach HTTP.</span><span class="sxs-lookup"><span data-stu-id="48641-425">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `shouldSkipNegotiate` | `false` | <span data-ttu-id="48641-426">Ustaw tę `true` wartość na, aby pominąć krok negocjowania.</span><span class="sxs-lookup"><span data-stu-id="48641-426">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="48641-427">**Obsługiwane tylko wtedy, gdy transport Sockets jest jedynym włączonym transportem**.</span><span class="sxs-lookup"><span data-stu-id="48641-427">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="48641-428">Nie można włączyć tego ustawienia w przypadku korzystania z usługi Azure Signal Service.</span><span class="sxs-lookup"><span data-stu-id="48641-428">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| <span data-ttu-id="48641-429">`withHeader``withHeaders`</span><span class="sxs-lookup"><span data-stu-id="48641-429">`withHeader` `withHeaders`</span></span> | <span data-ttu-id="48641-430">Pusty</span><span class="sxs-lookup"><span data-stu-id="48641-430">Empty</span></span> | <span data-ttu-id="48641-431">Mapa dodatkowych nagłówków HTTP do wysłania w każdym żądaniu HTTP.</span><span class="sxs-lookup"><span data-stu-id="48641-431">A Map of additional HTTP headers to send with every HTTP request.</span></span> |

---

<span data-ttu-id="48641-432">W kliencie platformy .NET te opcje można modyfikować za pomocą delegata opcji udostępnianego dla `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="48641-432">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="48641-433">W kliencie JavaScript te opcje można podać w obiekcie JavaScript udostępnionym `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="48641-433">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

<span data-ttu-id="48641-434">W kliencie Java Opcje te można skonfigurować przy użyciu metod `HttpHubConnectionBuilder` zwróconych z`HubConnectionBuilder.create("HUB URL")`</span><span class="sxs-lookup"><span data-stu-id="48641-434">In the Java client, these options can be configured with the methods on the `HttpHubConnectionBuilder` returned from the `HubConnectionBuilder.create("HUB URL")`</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
        .withHeader("Foo", "Bar")
        .shouldSkipNegotiate(true)
        .withHandshakeResponseTimeout(30*1000)
        .build();
```

## <a name="additional-resources"></a><span data-ttu-id="48641-435">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="48641-435">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>
