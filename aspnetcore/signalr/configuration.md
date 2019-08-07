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
# <a name="aspnet-core-signalr-configuration"></a><span data-ttu-id="a41b6-103">Konfiguracja sygnałów ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a41b6-103">ASP.NET Core SignalR configuration</span></span>

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="a41b6-104">Opcje serializacji JSON/MessagePack</span><span class="sxs-lookup"><span data-stu-id="a41b6-104">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="a41b6-105">ASP.NET Core sygnalizujący obsługuje dwa protokoły dla komunikatów kodowania: [JSON](https://www.json.org/) i [MessagePack](https://msgpack.org/index.html).</span><span class="sxs-lookup"><span data-stu-id="a41b6-105">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="a41b6-106">Każdy protokół ma opcje konfiguracji serializacji.</span><span class="sxs-lookup"><span data-stu-id="a41b6-106">Each protocol has serialization configuration options.</span></span>

<span data-ttu-id="a41b6-107">Serializacji JSON można skonfigurować na serwerze przy użyciu metody rozszerzenia [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) , która może zostać dodana po `Startup.ConfigureServices` metodzie [addsygnalizująca](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) w ramach metody.</span><span class="sxs-lookup"><span data-stu-id="a41b6-107">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method, which can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in your `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="a41b6-108">Metoda przyjmuje delegata, który `options` odbiera obiekt. `AddJsonProtocol`</span><span class="sxs-lookup"><span data-stu-id="a41b6-108">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="a41b6-109">Właściwość [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) tego obiektu jest obiektem JSON.NET `JsonSerializerSettings` , którego można użyć do skonfigurowania serializacji argumentów i zwracanych wartości.</span><span class="sxs-lookup"><span data-stu-id="a41b6-109">The [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) property on that object is a JSON.NET `JsonSerializerSettings` object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="a41b6-110">Aby uzyskać więcej informacji, zobacz [dokumentację JSON.NET](https://www.newtonsoft.com/json/help/html/Introduction.htm) .</span><span class="sxs-lookup"><span data-stu-id="a41b6-110">See the [JSON.NET Documentation](https://www.newtonsoft.com/json/help/html/Introduction.htm) for more details.</span></span>

<span data-ttu-id="a41b6-111">Przykładowo, aby skonfigurować serializator do używania nazw właściwości "PascalCase" zamiast domyślnych nazw "camelCase", należy użyć następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="a41b6-111">As an example, to configure the serializer to use "PascalCase" property names, instead of the default "camelCase" names, use the following code:</span></span>

```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver =
        new DefaultContractResolver();
    });
```

<span data-ttu-id="a41b6-112">W kliencie .NET ta sama `AddJsonProtocol` Metoda rozszerzająca istnieje w [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="a41b6-112">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="a41b6-113">`Microsoft.Extensions.DependencyInjection` Przestrzeń nazw musi zostać zaimportowana, aby można było rozpoznać metodę rozszerzenia:</span><span class="sxs-lookup"><span data-stu-id="a41b6-113">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

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
> <span data-ttu-id="a41b6-114">W tej chwili nie można skonfigurować serializacji JSON w kliencie JavaScript.</span><span class="sxs-lookup"><span data-stu-id="a41b6-114">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="a41b6-115">Opcje serializacji MessagePack</span><span class="sxs-lookup"><span data-stu-id="a41b6-115">MessagePack serialization options</span></span>

<span data-ttu-id="a41b6-116">Serializacji MessagePack można skonfigurować, dostarczając delegata do wywołania [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) .</span><span class="sxs-lookup"><span data-stu-id="a41b6-116">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="a41b6-117">Aby uzyskać więcej informacji, zobacz [MessagePack w sygnalizacji](xref:signalr/messagepackhubprotocol) .</span><span class="sxs-lookup"><span data-stu-id="a41b6-117">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="a41b6-118">W tej chwili nie można skonfigurować serializacji MessagePack w kliencie JavaScript.</span><span class="sxs-lookup"><span data-stu-id="a41b6-118">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="a41b6-119">Konfigurowanie opcji serwera</span><span class="sxs-lookup"><span data-stu-id="a41b6-119">Configure server options</span></span>

<span data-ttu-id="a41b6-120">W poniższej tabeli opisano opcje konfigurowania centrów sygnałów:</span><span class="sxs-lookup"><span data-stu-id="a41b6-120">The following table describes options for configuring SignalR hubs:</span></span>

::: moniker range=">= aspnetcore-3.0"

| <span data-ttu-id="a41b6-121">Opcja</span><span class="sxs-lookup"><span data-stu-id="a41b6-121">Option</span></span> | <span data-ttu-id="a41b6-122">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="a41b6-122">Default Value</span></span> | <span data-ttu-id="a41b6-123">Opis</span><span class="sxs-lookup"><span data-stu-id="a41b6-123">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="a41b6-124">30 sekund</span><span class="sxs-lookup"><span data-stu-id="a41b6-124">30 seconds</span></span> | <span data-ttu-id="a41b6-125">Serwer Rozważ odłączenie klienta, jeśli nie odebrano komunikatu (w tym w tym interwale).</span><span class="sxs-lookup"><span data-stu-id="a41b6-125">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="a41b6-126">Może upłynąć dłużej niż ten interwał limitu czasu, aby klient mógł zostać oznaczony jako odłączony, ze względu na to, jak jest to zaimplementowane.</span><span class="sxs-lookup"><span data-stu-id="a41b6-126">It could take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="a41b6-127">Zalecaną wartością jest podwójna `KeepAliveInterval` wartość.</span><span class="sxs-lookup"><span data-stu-id="a41b6-127">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="a41b6-128">15 sekund</span><span class="sxs-lookup"><span data-stu-id="a41b6-128">15 seconds</span></span> | <span data-ttu-id="a41b6-129">Jeśli klient nie wyśle początkowego komunikatu uzgadniania w tym przedziale czasu, połączenie zostanie zamknięte.</span><span class="sxs-lookup"><span data-stu-id="a41b6-129">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="a41b6-130">Jest to ustawienie zaawansowane, które należy zmodyfikować tylko w przypadku wystąpienia błędów limitu czasu uzgadniania z powodu poważnego opóźnienia sieci.</span><span class="sxs-lookup"><span data-stu-id="a41b6-130">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="a41b6-131">Aby uzyskać więcej szczegółów na temat procesu uzgadniania, zobacz [specyfikację protokołu centrum sygnału](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="a41b6-131">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="a41b6-132">15 sekund</span><span class="sxs-lookup"><span data-stu-id="a41b6-132">15 seconds</span></span> | <span data-ttu-id="a41b6-133">Jeśli serwer nie wysłał komunikatu w tym interwale, zostanie automatycznie wysłana wiadomość ping w celu pozostawienia otwartego połączenia.</span><span class="sxs-lookup"><span data-stu-id="a41b6-133">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="a41b6-134">W przypadku `KeepAliveInterval`zmiany należy `ServerTimeout` / zmienić`serverTimeoutInMilliseconds` ustawienie na kliencie.</span><span class="sxs-lookup"><span data-stu-id="a41b6-134">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="a41b6-135">Zalecaną `ServerTimeout` / wartościąjest`KeepAliveInterval` podwójna wartość. `serverTimeoutInMilliseconds`</span><span class="sxs-lookup"><span data-stu-id="a41b6-135">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="a41b6-136">Wszystkie zainstalowane protokoły</span><span class="sxs-lookup"><span data-stu-id="a41b6-136">All installed protocols</span></span> | <span data-ttu-id="a41b6-137">Protokoły obsługiwane przez to centrum.</span><span class="sxs-lookup"><span data-stu-id="a41b6-137">Protocols supported by this hub.</span></span> <span data-ttu-id="a41b6-138">Domyślnie wszystkie protokoły zarejestrowane na serwerze są dozwolone, ale protokoły można usunąć z tej listy, aby wyłączyć określone protokoły dla poszczególnych centrów.</span><span class="sxs-lookup"><span data-stu-id="a41b6-138">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="a41b6-139">Jeśli `true`szczegółowe komunikaty o wyjątkach są zwracane do klientów, gdy wyjątek jest zgłaszany w metodzie centrum.</span><span class="sxs-lookup"><span data-stu-id="a41b6-139">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="a41b6-140">Wartość domyślna to `false`, ponieważ te komunikaty o wyjątkach mogą zawierać informacje poufne.</span><span class="sxs-lookup"><span data-stu-id="a41b6-140">The default is `false`, as these exception messages can contain sensitive information.</span></span> |
| `StreamBufferCapacity` | `10` | <span data-ttu-id="a41b6-141">Maksymalna liczba elementów, które mogą być buforowane dla strumieni przekazywania klientów.</span><span class="sxs-lookup"><span data-stu-id="a41b6-141">The maximum number of items that can be buffered for client upload streams.</span></span> <span data-ttu-id="a41b6-142">Po osiągnięciu tego limitu przetwarzanie wywołań jest blokowane, dopóki serwer nie przetwarza elementów strumienia.</span><span class="sxs-lookup"><span data-stu-id="a41b6-142">If this limit is reached, the processing of invocations is blocked until the server processes stream items.</span></span>|

::: moniker-end

::: moniker range="= aspnetcore-2.2"

| <span data-ttu-id="a41b6-143">Opcja</span><span class="sxs-lookup"><span data-stu-id="a41b6-143">Option</span></span> | <span data-ttu-id="a41b6-144">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="a41b6-144">Default Value</span></span> | <span data-ttu-id="a41b6-145">Opis</span><span class="sxs-lookup"><span data-stu-id="a41b6-145">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="a41b6-146">30 sekund</span><span class="sxs-lookup"><span data-stu-id="a41b6-146">30 seconds</span></span> | <span data-ttu-id="a41b6-147">Serwer Rozważ odłączenie klienta, jeśli nie odebrano komunikatu (w tym w tym interwale).</span><span class="sxs-lookup"><span data-stu-id="a41b6-147">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="a41b6-148">Może upłynąć dłużej niż ten interwał limitu czasu, aby klient mógł zostać oznaczony jako odłączony, ze względu na to, jak jest to zaimplementowane.</span><span class="sxs-lookup"><span data-stu-id="a41b6-148">It could take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="a41b6-149">Zalecaną wartością jest podwójna `KeepAliveInterval` wartość.</span><span class="sxs-lookup"><span data-stu-id="a41b6-149">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="a41b6-150">15 sekund</span><span class="sxs-lookup"><span data-stu-id="a41b6-150">15 seconds</span></span> | <span data-ttu-id="a41b6-151">Jeśli klient nie wyśle początkowego komunikatu uzgadniania w tym przedziale czasu, połączenie zostanie zamknięte.</span><span class="sxs-lookup"><span data-stu-id="a41b6-151">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="a41b6-152">Jest to ustawienie zaawansowane, które należy zmodyfikować tylko w przypadku wystąpienia błędów limitu czasu uzgadniania z powodu poważnego opóźnienia sieci.</span><span class="sxs-lookup"><span data-stu-id="a41b6-152">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="a41b6-153">Aby uzyskać więcej szczegółów na temat procesu uzgadniania, zobacz [specyfikację protokołu centrum sygnału](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="a41b6-153">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="a41b6-154">15 sekund</span><span class="sxs-lookup"><span data-stu-id="a41b6-154">15 seconds</span></span> | <span data-ttu-id="a41b6-155">Jeśli serwer nie wysłał komunikatu w tym interwale, zostanie automatycznie wysłana wiadomość ping w celu pozostawienia otwartego połączenia.</span><span class="sxs-lookup"><span data-stu-id="a41b6-155">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="a41b6-156">W przypadku `KeepAliveInterval`zmiany należy `ServerTimeout` / zmienić`serverTimeoutInMilliseconds` ustawienie na kliencie.</span><span class="sxs-lookup"><span data-stu-id="a41b6-156">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="a41b6-157">Zalecaną `ServerTimeout` / wartościąjest`KeepAliveInterval` podwójna wartość. `serverTimeoutInMilliseconds`</span><span class="sxs-lookup"><span data-stu-id="a41b6-157">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="a41b6-158">Wszystkie zainstalowane protokoły</span><span class="sxs-lookup"><span data-stu-id="a41b6-158">All installed protocols</span></span> | <span data-ttu-id="a41b6-159">Protokoły obsługiwane przez to centrum.</span><span class="sxs-lookup"><span data-stu-id="a41b6-159">Protocols supported by this hub.</span></span> <span data-ttu-id="a41b6-160">Domyślnie wszystkie protokoły zarejestrowane na serwerze są dozwolone, ale protokoły można usunąć z tej listy, aby wyłączyć określone protokoły dla poszczególnych centrów.</span><span class="sxs-lookup"><span data-stu-id="a41b6-160">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="a41b6-161">Jeśli `true`szczegółowe komunikaty o wyjątkach są zwracane do klientów, gdy wyjątek jest zgłaszany w metodzie centrum.</span><span class="sxs-lookup"><span data-stu-id="a41b6-161">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="a41b6-162">Wartość domyślna to `false`, ponieważ te komunikaty o wyjątkach mogą zawierać informacje poufne.</span><span class="sxs-lookup"><span data-stu-id="a41b6-162">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-2.1"

| <span data-ttu-id="a41b6-163">Opcja</span><span class="sxs-lookup"><span data-stu-id="a41b6-163">Option</span></span> | <span data-ttu-id="a41b6-164">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="a41b6-164">Default Value</span></span> | <span data-ttu-id="a41b6-165">Opis</span><span class="sxs-lookup"><span data-stu-id="a41b6-165">Description</span></span> |
| ------ | ------------- | ----------- |
| `HandshakeTimeout` | <span data-ttu-id="a41b6-166">15 sekund</span><span class="sxs-lookup"><span data-stu-id="a41b6-166">15 seconds</span></span> | <span data-ttu-id="a41b6-167">Jeśli klient nie wyśle początkowego komunikatu uzgadniania w tym przedziale czasu, połączenie zostanie zamknięte.</span><span class="sxs-lookup"><span data-stu-id="a41b6-167">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="a41b6-168">Jest to ustawienie zaawansowane, które należy zmodyfikować tylko w przypadku wystąpienia błędów limitu czasu uzgadniania z powodu poważnego opóźnienia sieci.</span><span class="sxs-lookup"><span data-stu-id="a41b6-168">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="a41b6-169">Aby uzyskać więcej szczegółów na temat procesu uzgadniania, zobacz [specyfikację protokołu centrum sygnału](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="a41b6-169">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="a41b6-170">15 sekund</span><span class="sxs-lookup"><span data-stu-id="a41b6-170">15 seconds</span></span> | <span data-ttu-id="a41b6-171">Jeśli serwer nie wysłał komunikatu w tym interwale, zostanie automatycznie wysłana wiadomość ping w celu pozostawienia otwartego połączenia.</span><span class="sxs-lookup"><span data-stu-id="a41b6-171">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="a41b6-172">W przypadku `KeepAliveInterval`zmiany należy `ServerTimeout` / zmienić`serverTimeoutInMilliseconds` ustawienie na kliencie.</span><span class="sxs-lookup"><span data-stu-id="a41b6-172">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="a41b6-173">Zalecaną `ServerTimeout` / wartościąjest`KeepAliveInterval` podwójna wartość. `serverTimeoutInMilliseconds`</span><span class="sxs-lookup"><span data-stu-id="a41b6-173">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="a41b6-174">Wszystkie zainstalowane protokoły</span><span class="sxs-lookup"><span data-stu-id="a41b6-174">All installed protocols</span></span> | <span data-ttu-id="a41b6-175">Protokoły obsługiwane przez to centrum.</span><span class="sxs-lookup"><span data-stu-id="a41b6-175">Protocols supported by this hub.</span></span> <span data-ttu-id="a41b6-176">Domyślnie wszystkie protokoły zarejestrowane na serwerze są dozwolone, ale protokoły można usunąć z tej listy, aby wyłączyć określone protokoły dla poszczególnych centrów.</span><span class="sxs-lookup"><span data-stu-id="a41b6-176">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="a41b6-177">Jeśli `true`szczegółowe komunikaty o wyjątkach są zwracane do klientów, gdy wyjątek jest zgłaszany w metodzie centrum.</span><span class="sxs-lookup"><span data-stu-id="a41b6-177">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="a41b6-178">Wartość domyślna to `false`, ponieważ te komunikaty o wyjątkach mogą zawierać informacje poufne.</span><span class="sxs-lookup"><span data-stu-id="a41b6-178">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

::: moniker-end

<span data-ttu-id="a41b6-179">Opcje można skonfigurować dla wszystkich centrów, dostarczając opcje delegata do `AddSignalR` wywołania w. `Startup.ConfigureServices`</span><span class="sxs-lookup"><span data-stu-id="a41b6-179">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

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

<span data-ttu-id="a41b6-180">Opcje jednego centrum przesłaniają opcje globalne podane w `AddSignalR` i można je skonfigurować przy użyciu: <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*></span><span class="sxs-lookup"><span data-stu-id="a41b6-180">Options for a single hub override the global options provided in `AddSignalR` and can be configured using <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

### <a name="advanced-http-configuration-options"></a><span data-ttu-id="a41b6-181">Zaawansowane opcje konfiguracji HTTP</span><span class="sxs-lookup"><span data-stu-id="a41b6-181">Advanced HTTP configuration options</span></span>

<span data-ttu-id="a41b6-182">Służy `HttpConnectionDispatcherOptions` do konfigurowania ustawień zaawansowanych związanych z transportami i zarządzaniem buforem pamięci.</span><span class="sxs-lookup"><span data-stu-id="a41b6-182">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="a41b6-183">Te opcje są konfigurowane przez przekazanie delegata [do\<MapHub T >](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) w. `Startup.Configure`</span><span class="sxs-lookup"><span data-stu-id="a41b6-183">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) in `Startup.Configure`.</span></span>

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

<span data-ttu-id="a41b6-184">W poniższej tabeli opisano opcje konfigurowania zaawansowanych opcji protokołu HTTP w programie ASP.NET Core Signal:</span><span class="sxs-lookup"><span data-stu-id="a41b6-184">The following table describes options for configuring ASP.NET Core SignalR's advanced HTTP options:</span></span>

| <span data-ttu-id="a41b6-185">Opcja</span><span class="sxs-lookup"><span data-stu-id="a41b6-185">Option</span></span> | <span data-ttu-id="a41b6-186">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="a41b6-186">Default Value</span></span> | <span data-ttu-id="a41b6-187">Opis</span><span class="sxs-lookup"><span data-stu-id="a41b6-187">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="a41b6-188">32 KB</span><span class="sxs-lookup"><span data-stu-id="a41b6-188">32 KB</span></span> | <span data-ttu-id="a41b6-189">Maksymalna liczba bajtów odebranych od klienta, które buforuje serwer.</span><span class="sxs-lookup"><span data-stu-id="a41b6-189">The maximum number of bytes received from the client that the server buffers.</span></span> <span data-ttu-id="a41b6-190">Zwiększenie tej wartości umożliwia serwerowi otrzymywanie większych komunikatów, ale może mieć negatywny wpływ na użycie pamięci.</span><span class="sxs-lookup"><span data-stu-id="a41b6-190">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="a41b6-191">Dane zbierane automatycznie z `Authorize` atrybutów zastosowanych do klasy centrum.</span><span class="sxs-lookup"><span data-stu-id="a41b6-191">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="a41b6-192">Lista obiektów [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) służąca do określenia, czy klient jest autoryzowany do łączenia się z centrum.</span><span class="sxs-lookup"><span data-stu-id="a41b6-192">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="a41b6-193">32 KB</span><span class="sxs-lookup"><span data-stu-id="a41b6-193">32 KB</span></span> | <span data-ttu-id="a41b6-194">Maksymalna liczba bajtów wysłanych przez aplikację, które buforują serwer.</span><span class="sxs-lookup"><span data-stu-id="a41b6-194">The maximum number of bytes sent by the app that the server buffers.</span></span> <span data-ttu-id="a41b6-195">Zwiększenie tej wartości umożliwia serwerowi wysyłanie większych komunikatów, ale może mieć negatywny wpływ na użycie pamięci.</span><span class="sxs-lookup"><span data-stu-id="a41b6-195">Increasing this value allows the server to send larger messages, but can negatively impact memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="a41b6-196">Wszystkie transporty są włączone.</span><span class="sxs-lookup"><span data-stu-id="a41b6-196">All Transports are enabled.</span></span> | <span data-ttu-id="a41b6-197">Maska bitów `HttpTransportType` wartości, która może ograniczyć transporty, z których może korzystać klient do nawiązywania połączeń.</span><span class="sxs-lookup"><span data-stu-id="a41b6-197">A bitmask of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="a41b6-198">Zobacz poniżej.</span><span class="sxs-lookup"><span data-stu-id="a41b6-198">See below.</span></span> | <span data-ttu-id="a41b6-199">Dodatkowe opcje specyficzne dla długiego transportu sondowania.</span><span class="sxs-lookup"><span data-stu-id="a41b6-199">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="a41b6-200">Zobacz poniżej.</span><span class="sxs-lookup"><span data-stu-id="a41b6-200">See below.</span></span> | <span data-ttu-id="a41b6-201">Dodatkowe opcje specyficzne dla transportu obiektów WebSockets.</span><span class="sxs-lookup"><span data-stu-id="a41b6-201">Additional options specific to the WebSockets transport.</span></span> |

<span data-ttu-id="a41b6-202">W przypadku długiego transportu sondowania dostępne są dodatkowe opcje, które można `LongPolling` skonfigurować przy użyciu właściwości:</span><span class="sxs-lookup"><span data-stu-id="a41b6-202">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="a41b6-203">Opcja</span><span class="sxs-lookup"><span data-stu-id="a41b6-203">Option</span></span> | <span data-ttu-id="a41b6-204">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="a41b6-204">Default Value</span></span> | <span data-ttu-id="a41b6-205">Opis</span><span class="sxs-lookup"><span data-stu-id="a41b6-205">Description</span></span> |
| ------ | ------------- | ----------- |
| `PollTimeout` | <span data-ttu-id="a41b6-206">90 sekund</span><span class="sxs-lookup"><span data-stu-id="a41b6-206">90 seconds</span></span> | <span data-ttu-id="a41b6-207">Maksymalny czas, przez jaki serwer czeka na wysłanie wiadomości do klienta przed zakończeniem jednego żądania sondowania.</span><span class="sxs-lookup"><span data-stu-id="a41b6-207">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="a41b6-208">Zmniejszenie tej wartości powoduje, że klient wystawia nowe żądania sondy częściej.</span><span class="sxs-lookup"><span data-stu-id="a41b6-208">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> |

<span data-ttu-id="a41b6-209">Transport WebSocket ma dodatkowe opcje, które można skonfigurować przy użyciu `WebSockets` właściwości:</span><span class="sxs-lookup"><span data-stu-id="a41b6-209">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="a41b6-210">Opcja</span><span class="sxs-lookup"><span data-stu-id="a41b6-210">Option</span></span> | <span data-ttu-id="a41b6-211">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="a41b6-211">Default Value</span></span> | <span data-ttu-id="a41b6-212">Opis</span><span class="sxs-lookup"><span data-stu-id="a41b6-212">Description</span></span> |
| ------ | ------------- | ----------- |
| `CloseTimeout` | <span data-ttu-id="a41b6-213">5 sekund</span><span class="sxs-lookup"><span data-stu-id="a41b6-213">5 seconds</span></span> | <span data-ttu-id="a41b6-214">Gdy serwer zostanie zamknięty, jeśli klient nie zostanie zamknięty w tym okresie, połączenie zostanie przerwane.</span><span class="sxs-lookup"><span data-stu-id="a41b6-214">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | `null` | <span data-ttu-id="a41b6-215">Delegat, którego można użyć do ustawienia `Sec-WebSocket-Protocol` nagłówka na wartość niestandardową.</span><span class="sxs-lookup"><span data-stu-id="a41b6-215">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="a41b6-216">Delegat otrzymuje wartości żądane przez klienta jako dane wejściowe i oczekuje na zwrócenie żądanej wartości.</span><span class="sxs-lookup"><span data-stu-id="a41b6-216">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="a41b6-217">Konfigurowanie opcji klienta</span><span class="sxs-lookup"><span data-stu-id="a41b6-217">Configure client options</span></span>

<span data-ttu-id="a41b6-218">Opcje klienta można skonfigurować dla `HubConnectionBuilder` typu (dostępnego w klientach .NET i JavaScript).</span><span class="sxs-lookup"><span data-stu-id="a41b6-218">Client options can be configured on the `HubConnectionBuilder` type (available in the .NET and JavaScript clients).</span></span> <span data-ttu-id="a41b6-219">Jest ona również dostępna w kliencie Java, ale `HttpHubConnectionBuilder` podklasa zawiera opcje konfiguracji konstruktora, a także `HubConnection` samego siebie.</span><span class="sxs-lookup"><span data-stu-id="a41b6-219">It's also available in the Java client, but the `HttpHubConnectionBuilder` subclass is what contains the builder configuration options, as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="a41b6-220">Konfigurowanie rejestrowania</span><span class="sxs-lookup"><span data-stu-id="a41b6-220">Configure logging</span></span>

<span data-ttu-id="a41b6-221">Rejestrowanie jest konfigurowane w kliencie .NET przy użyciu `ConfigureLogging` metody.</span><span class="sxs-lookup"><span data-stu-id="a41b6-221">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="a41b6-222">Dostawcy i filtry rejestrowania mogą być rejestrowane w taki sam sposób, w jaki znajdują się na serwerze.</span><span class="sxs-lookup"><span data-stu-id="a41b6-222">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="a41b6-223">Aby uzyskać więcej informacji, zobacz dokumentację dotyczącą [rejestrowania w ASP.NET Core](xref:fundamentals/logging/index) .</span><span class="sxs-lookup"><span data-stu-id="a41b6-223">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="a41b6-224">Aby zarejestrować dostawców rejestrowania, należy zainstalować wymagane pakiety.</span><span class="sxs-lookup"><span data-stu-id="a41b6-224">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="a41b6-225">Aby zapoznać się z pełną listą, zobacz sekcję [wbudowane dostawcy rejestrowania](xref:fundamentals/logging/index#built-in-logging-providers) w witrynie docs.</span><span class="sxs-lookup"><span data-stu-id="a41b6-225">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="a41b6-226">Na przykład aby włączyć rejestrowanie konsoli, zainstaluj `Microsoft.Extensions.Logging.Console` pakiet NuGet.</span><span class="sxs-lookup"><span data-stu-id="a41b6-226">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="a41b6-227">Wywołaj `AddConsole` metodę rozszerzenia:</span><span class="sxs-lookup"><span data-stu-id="a41b6-227">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="a41b6-228">W kliencie JavaScript istnieje podobna `configureLogging` Metoda.</span><span class="sxs-lookup"><span data-stu-id="a41b6-228">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="a41b6-229">`LogLevel` Podaj wartość wskazującą minimalny poziom komunikatów dziennika do wygenerowania.</span><span class="sxs-lookup"><span data-stu-id="a41b6-229">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="a41b6-230">Dzienniki są zapisywane w oknie konsoli przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="a41b6-230">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="a41b6-231">Zamiast wartości można także podać wartość reprezentującą nazwę poziomu dziennika. `string` `LogLevel`</span><span class="sxs-lookup"><span data-stu-id="a41b6-231">Instead of a `LogLevel` value, you can also provide a `string` value representing a log level name.</span></span> <span data-ttu-id="a41b6-232">Jest to przydatne podczas konfigurowania rejestrowania sygnałów w środowiskach, w których nie masz dostępu do `LogLevel` stałych.</span><span class="sxs-lookup"><span data-stu-id="a41b6-232">This is useful when configuring SignalR logging in environments where you don't have access to the `LogLevel` constants.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging("warn")
    .build();
```

<span data-ttu-id="a41b6-233">Poniższa tabela zawiera listę dostępnych poziomów dzienników.</span><span class="sxs-lookup"><span data-stu-id="a41b6-233">The following table lists the available log levels.</span></span> <span data-ttu-id="a41b6-234">Wartość określana przez użytkownika `configureLogging` w celu ustawienia minimalnego poziomu dziennika, który zostanie zarejestrowany.</span><span class="sxs-lookup"><span data-stu-id="a41b6-234">The value you provide to `configureLogging` sets the **minimum** log level that will be logged.</span></span> <span data-ttu-id="a41b6-235">Komunikaty zarejestrowane na tym poziomie **lub poziomy wymienione po nim w tabeli**zostaną zarejestrowane.</span><span class="sxs-lookup"><span data-stu-id="a41b6-235">Messages logged at this level, **or the levels listed after it in the table**, will be logged.</span></span>

| <span data-ttu-id="a41b6-236">String</span><span class="sxs-lookup"><span data-stu-id="a41b6-236">String</span></span>                      | <span data-ttu-id="a41b6-237">LogLevel</span><span class="sxs-lookup"><span data-stu-id="a41b6-237">LogLevel</span></span>               |
| --------------------------- | ---------------------- |
| `trace`                     | `LogLevel.Trace`       |
| `debug`                     | `LogLevel.Debug`       |
| <span data-ttu-id="a41b6-238">`info`**lub**`information`</span><span class="sxs-lookup"><span data-stu-id="a41b6-238">`info` **or** `information`</span></span> | `LogLevel.Information` |
| <span data-ttu-id="a41b6-239">`warn`**lub**`warning`</span><span class="sxs-lookup"><span data-stu-id="a41b6-239">`warn` **or** `warning`</span></span>     | `LogLevel.Warning`     |
| `error`                     | `LogLevel.Error`       |
| `critical`                  | `LogLevel.Critical`    |
| `none`                      | `LogLevel.None`        |

::: moniker-end

> [!NOTE]
> <span data-ttu-id="a41b6-240">Aby całkowicie wyłączyć rejestrowanie, określ `signalR.LogLevel.None` `configureLogging` w metodzie.</span><span class="sxs-lookup"><span data-stu-id="a41b6-240">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="a41b6-241">Aby uzyskać więcej informacji na temat rejestrowania, zobacz [dokumentację diagnostyki sygnału](xref:signalr/diagnostics).</span><span class="sxs-lookup"><span data-stu-id="a41b6-241">For more information on logging, see the [SignalR Diagnostics documentation](xref:signalr/diagnostics).</span></span>

<span data-ttu-id="a41b6-242">Klient Java sygnalizujący używa biblioteki [SLF4J](https://www.slf4j.org/) do rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="a41b6-242">The SignalR Java client uses the [SLF4J](https://www.slf4j.org/) library for logging.</span></span> <span data-ttu-id="a41b6-243">Jest to interfejs API rejestrowania wysokiego poziomu, który umożliwia użytkownikom biblioteki wybranie własnych implementacji rejestrowania przez nałożenie określonej zależności rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="a41b6-243">It's a high-level logging API that allows users of the library to chose their own specific logging implementation by bringing in a specific logging dependency.</span></span> <span data-ttu-id="a41b6-244">Poniższy fragment kodu przedstawia sposób użycia `java.util.logging` z klientem programu sygnalizującego środowisko Java.</span><span class="sxs-lookup"><span data-stu-id="a41b6-244">The following code snippet shows how to use `java.util.logging` with the SignalR Java client.</span></span>

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

<span data-ttu-id="a41b6-245">Jeśli nie skonfigurujesz rejestrowania w zależnościach, SLF4J ładuje domyślny Rejestrator braku operacji z następującym komunikatem ostrzegawczym:</span><span class="sxs-lookup"><span data-stu-id="a41b6-245">If you don't configure logging in your dependencies, SLF4J loads a default no-operation logger with the following warning message:</span></span>

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

<span data-ttu-id="a41b6-246">Można je bezpiecznie zignorować.</span><span class="sxs-lookup"><span data-stu-id="a41b6-246">This can safely be ignored.</span></span>

### <a name="configure-allowed-transports"></a><span data-ttu-id="a41b6-247">Skonfiguruj dozwolone transporty</span><span class="sxs-lookup"><span data-stu-id="a41b6-247">Configure allowed transports</span></span>

<span data-ttu-id="a41b6-248">Transporty używane przez sygnalizującego można skonfigurować w `WithUrl` wywołaniu (`withUrl` w języku JavaScript).</span><span class="sxs-lookup"><span data-stu-id="a41b6-248">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="a41b6-249">Wartości bitowe i `HttpTransportType` można użyć, aby ograniczyć klient do używania tylko określonych transportów.</span><span class="sxs-lookup"><span data-stu-id="a41b6-249">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="a41b6-250">Wszystkie transporty są domyślnie włączone.</span><span class="sxs-lookup"><span data-stu-id="a41b6-250">All transports are enabled by default.</span></span>

<span data-ttu-id="a41b6-251">Na przykład, aby wyłączyć transport zdarzeń wysłanych przez serwer, ale zezwolić na połączenia z usługą WebSockets i długie sondowanie:</span><span class="sxs-lookup"><span data-stu-id="a41b6-251">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="a41b6-252">W kliencie JavaScript transporty są konfigurowane przez ustawienie `transport` pola w obiekcie Options dostarczonego do: `withUrl`</span><span class="sxs-lookup"><span data-stu-id="a41b6-252">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="a41b6-253">W tej wersji elementu WebSockets klienta Java jest jedynym dostępnym transportem.</span><span class="sxs-lookup"><span data-stu-id="a41b6-253">In this version of the Java client websockets is the only available transport.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-3.0"

<span data-ttu-id="a41b6-254">W kliencie Java, transport jest wybierany przy użyciu `withTransport` metody `HttpHubConnectionBuilder`w.</span><span class="sxs-lookup"><span data-stu-id="a41b6-254">In the Java client, the transport is selected with the `withTransport` method on the `HttpHubConnectionBuilder`.</span></span> <span data-ttu-id="a41b6-255">Klient Java domyślnie używa transportu usługi WebSockets.</span><span class="sxs-lookup"><span data-stu-id="a41b6-255">The Java client defaults to using the WebSockets transport.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withTransport(TransportEnum.WEBSOCKETS)
    .build();
```

> [!NOTE]
> <span data-ttu-id="a41b6-256">Klient Java sygnalizujący nie obsługuje jeszcze rezerwy transportowej.</span><span class="sxs-lookup"><span data-stu-id="a41b6-256">The SignalR Java client doesn't support transport fallback yet.</span></span>

::: moniker-end

### <a name="configure-bearer-authentication"></a><span data-ttu-id="a41b6-257">Konfigurowanie uwierzytelniania okaziciela</span><span class="sxs-lookup"><span data-stu-id="a41b6-257">Configure bearer authentication</span></span>

<span data-ttu-id="a41b6-258">Aby zapewnić dane uwierzytelniania wraz z żądaniami sygnałów, użyj `AccessTokenProvider` opcji (`accessTokenFactory` w języku JavaScript), aby określić funkcję, która zwraca żądany token dostępu.</span><span class="sxs-lookup"><span data-stu-id="a41b6-258">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="a41b6-259">W kliencie .NET token dostępu jest przenoszona jako token "uwierzytelnianie okaziciela" protokołu HTTP (przy użyciu `Authorization` nagłówka z `Bearer`typem).</span><span class="sxs-lookup"><span data-stu-id="a41b6-259">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="a41b6-260">W kliencie JavaScript token dostępu jest używany jako token okaziciela, **z wyjątkiem** kilku przypadków, w których interfejsy API przeglądarki ograniczają możliwość zastosowania nagłówków (w konkretnym przypadku zdarzeń wysyłanych przez serwer i żądań WebSockets).</span><span class="sxs-lookup"><span data-stu-id="a41b6-260">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="a41b6-261">W takich przypadkach token dostępu jest podawany jako wartość `access_token`ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="a41b6-261">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="a41b6-262">W kliencie `AccessTokenProvider` .NET opcji można określić za pomocą delegata opcji w `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="a41b6-262">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

<span data-ttu-id="a41b6-263">W kliencie JavaScript token dostępu jest konfigurowany przez ustawienie `accessTokenFactory` pola w obiekcie Options w: `withUrl`</span><span class="sxs-lookup"><span data-stu-id="a41b6-263">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

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

<span data-ttu-id="a41b6-264">W kliencie Java programu sygnalizującego można skonfigurować token okaziciela do użycia na potrzeby uwierzytelniania, dostarczając fabrykę tokenów dostępu do [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span><span class="sxs-lookup"><span data-stu-id="a41b6-264">In the SignalR Java client, you can configure a bearer token to use for authentication by providing an access token factory to the [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span></span> <span data-ttu-id="a41b6-265">Użyj [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) , aby dostarczyć [RxJava](https://github.com/ReactiveX/RxJava) [pojedynczej\<>](https://reactivex.io/documentation/single.html).</span><span class="sxs-lookup"><span data-stu-id="a41b6-265">Use [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) to provide an [RxJava](https://github.com/ReactiveX/RxJava) [Single\<String>](https://reactivex.io/documentation/single.html).</span></span> <span data-ttu-id="a41b6-266">Wywołanie metody [Single. Ustąp](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-)umożliwia zapisanie logiki w celu utworzenia tokenów dostępu dla klienta.</span><span class="sxs-lookup"><span data-stu-id="a41b6-266">With a call to [Single.defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), you can write logic to produce access tokens for your client.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="a41b6-267">Konfigurowanie opcji limitu czasu i utrzymywania aktywności</span><span class="sxs-lookup"><span data-stu-id="a41b6-267">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="a41b6-268">W `HubConnection` samym obiekcie są dostępne dodatkowe opcje konfigurowania trybu przekroczenia limitu czasu i zachowania utrzymywania aktywności:</span><span class="sxs-lookup"><span data-stu-id="a41b6-268">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>


::: moniker range=">= aspnetcore-2.2"

# <a name="nettabdotnet"></a>[<span data-ttu-id="a41b6-269">.NET</span><span class="sxs-lookup"><span data-stu-id="a41b6-269">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="a41b6-270">Opcja</span><span class="sxs-lookup"><span data-stu-id="a41b6-270">Option</span></span> | <span data-ttu-id="a41b6-271">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="a41b6-271">Default value</span></span> | <span data-ttu-id="a41b6-272">Opis</span><span class="sxs-lookup"><span data-stu-id="a41b6-272">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="a41b6-273">30 sekund (30 000 MS)</span><span class="sxs-lookup"><span data-stu-id="a41b6-273">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="a41b6-274">Limit czasu dla działania serwera.</span><span class="sxs-lookup"><span data-stu-id="a41b6-274">Timeout for server activity.</span></span> <span data-ttu-id="a41b6-275">Jeśli serwer nie wysłał komunikatu w tym interwale, Klient uważa, że serwer odłączył się i wyzwala `Closed` zdarzenie (`onclose` w języku JavaScript).</span><span class="sxs-lookup"><span data-stu-id="a41b6-275">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="a41b6-276">Ta wartość musi być wystarczająca do wysłania wiadomości ping z serwera **i** odebrana przez klienta w ramach przedziału czasu.</span><span class="sxs-lookup"><span data-stu-id="a41b6-276">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="a41b6-277">Zalecana wartość to liczba z co najmniej podwójną `KeepAliveInterval` wartością serwera, aby umożliwić czas na nadejście poleceń ping.</span><span class="sxs-lookup"><span data-stu-id="a41b6-277">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="a41b6-278">15 sekund</span><span class="sxs-lookup"><span data-stu-id="a41b6-278">15 seconds</span></span> | <span data-ttu-id="a41b6-279">Limit czasu początkowej uzgadniania z serwerem.</span><span class="sxs-lookup"><span data-stu-id="a41b6-279">Timeout for initial server handshake.</span></span> <span data-ttu-id="a41b6-280">Jeśli serwer nie wyśle odpowiedzi uzgadniania w tym interwale, klient anuluje uzgadnianie i wyzwala `Closed` zdarzenie (`onclose` w języku JavaScript).</span><span class="sxs-lookup"><span data-stu-id="a41b6-280">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="a41b6-281">Jest to ustawienie zaawansowane, które należy zmodyfikować tylko w przypadku wystąpienia błędów limitu czasu uzgadniania z powodu poważnego opóźnienia sieci.</span><span class="sxs-lookup"><span data-stu-id="a41b6-281">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="a41b6-282">Aby uzyskać więcej szczegółów na temat procesu uzgadniania, zobacz [specyfikację protokołu centrum sygnału](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="a41b6-282">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="a41b6-283">15 sekund</span><span class="sxs-lookup"><span data-stu-id="a41b6-283">15 seconds</span></span> | <span data-ttu-id="a41b6-284">Określa interwał wysyłania komunikatów ping przez klienta.</span><span class="sxs-lookup"><span data-stu-id="a41b6-284">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="a41b6-285">Wysłanie wszelkich komunikatów z klienta powoduje zresetowanie czasomierza do początku interwału.</span><span class="sxs-lookup"><span data-stu-id="a41b6-285">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="a41b6-286">Jeśli klient nie wysłał komunikatu w `ClientTimeoutInterval` zestawie na serwerze, serwer traktuje klienta jako odłączony.</span><span class="sxs-lookup"><span data-stu-id="a41b6-286">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

<span data-ttu-id="a41b6-287">W kliencie .NET wartości limitu czasu są określone jako `TimeSpan` wartości.</span><span class="sxs-lookup"><span data-stu-id="a41b6-287">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="a41b6-288">JavaScript</span><span class="sxs-lookup"><span data-stu-id="a41b6-288">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="a41b6-289">Opcja</span><span class="sxs-lookup"><span data-stu-id="a41b6-289">Option</span></span> | <span data-ttu-id="a41b6-290">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="a41b6-290">Default value</span></span> | <span data-ttu-id="a41b6-291">Opis</span><span class="sxs-lookup"><span data-stu-id="a41b6-291">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="a41b6-292">30 sekund (30 000 MS)</span><span class="sxs-lookup"><span data-stu-id="a41b6-292">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="a41b6-293">Limit czasu dla działania serwera.</span><span class="sxs-lookup"><span data-stu-id="a41b6-293">Timeout for server activity.</span></span> <span data-ttu-id="a41b6-294">Jeśli serwer nie wysłał komunikatu w tym interwale, Klient uważa, że serwer odłączył się i wyzwala `onclose` zdarzenie.</span><span class="sxs-lookup"><span data-stu-id="a41b6-294">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="a41b6-295">Ta wartość musi być wystarczająca do wysłania wiadomości ping z serwera **i** odebrana przez klienta w ramach przedziału czasu.</span><span class="sxs-lookup"><span data-stu-id="a41b6-295">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="a41b6-296">Zalecana wartość to liczba z co najmniej podwójną `KeepAliveInterval` wartością serwera, aby umożliwić czas na nadejście poleceń ping.</span><span class="sxs-lookup"><span data-stu-id="a41b6-296">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `keepAliveIntervalInMilliseconds` | <span data-ttu-id="a41b6-297">15 sekund (15 000 milisekund)</span><span class="sxs-lookup"><span data-stu-id="a41b6-297">15 seconds (15,000 miliseconds)</span></span> | <span data-ttu-id="a41b6-298">Określa interwał wysyłania komunikatów ping przez klienta.</span><span class="sxs-lookup"><span data-stu-id="a41b6-298">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="a41b6-299">Wysłanie wszelkich komunikatów z klienta powoduje zresetowanie czasomierza do początku interwału.</span><span class="sxs-lookup"><span data-stu-id="a41b6-299">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="a41b6-300">Jeśli klient nie wysłał komunikatu w `ClientTimeoutInterval` zestawie na serwerze, serwer traktuje klienta jako odłączony.</span><span class="sxs-lookup"><span data-stu-id="a41b6-300">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="a41b6-301">Java</span><span class="sxs-lookup"><span data-stu-id="a41b6-301">Java</span></span>](#tab/java)

| <span data-ttu-id="a41b6-302">Opcja</span><span class="sxs-lookup"><span data-stu-id="a41b6-302">Option</span></span> | <span data-ttu-id="a41b6-303">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="a41b6-303">Default value</span></span> | <span data-ttu-id="a41b6-304">Opis</span><span class="sxs-lookup"><span data-stu-id="a41b6-304">Description</span></span> |
| ------ | ------------- | ----------- |
| <span data-ttu-id="a41b6-305">`getServerTimeout` / `setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="a41b6-305">`getServerTimeout` / `setServerTimeout`</span></span> | <span data-ttu-id="a41b6-306">30 sekund (30 000 MS)</span><span class="sxs-lookup"><span data-stu-id="a41b6-306">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="a41b6-307">Limit czasu dla działania serwera.</span><span class="sxs-lookup"><span data-stu-id="a41b6-307">Timeout for server activity.</span></span> <span data-ttu-id="a41b6-308">Jeśli serwer nie wysłał komunikatu w tym interwale, Klient uważa, że serwer odłączył się i wyzwala `onClose` zdarzenie.</span><span class="sxs-lookup"><span data-stu-id="a41b6-308">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="a41b6-309">Ta wartość musi być wystarczająca do wysłania wiadomości ping z serwera **i** odebrana przez klienta w ramach przedziału czasu.</span><span class="sxs-lookup"><span data-stu-id="a41b6-309">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="a41b6-310">Zalecana wartość to liczba z co najmniej podwójną `KeepAliveInterval` wartością serwera, aby umożliwić czas na nadejście poleceń ping.</span><span class="sxs-lookup"><span data-stu-id="a41b6-310">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="a41b6-311">15 sekund</span><span class="sxs-lookup"><span data-stu-id="a41b6-311">15 seconds</span></span> | <span data-ttu-id="a41b6-312">Limit czasu początkowej uzgadniania z serwerem.</span><span class="sxs-lookup"><span data-stu-id="a41b6-312">Timeout for initial server handshake.</span></span> <span data-ttu-id="a41b6-313">Jeśli serwer nie wyśle odpowiedzi uzgadniania w tym interwale, klient anuluje uzgadnianie i wyzwala `onClose` zdarzenie.</span><span class="sxs-lookup"><span data-stu-id="a41b6-313">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="a41b6-314">Jest to ustawienie zaawansowane, które należy zmodyfikować tylko w przypadku wystąpienia błędów limitu czasu uzgadniania z powodu poważnego opóźnienia sieci.</span><span class="sxs-lookup"><span data-stu-id="a41b6-314">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="a41b6-315">Aby uzyskać więcej szczegółów na temat procesu uzgadniania, zobacz [specyfikację protokołu centrum sygnału](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="a41b6-315">For more detail on the Handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| <span data-ttu-id="a41b6-316">`getKeepAliveInterval` / `setKeepAliveInterval`</span><span class="sxs-lookup"><span data-stu-id="a41b6-316">`getKeepAliveInterval` / `setKeepAliveInterval`</span></span> | <span data-ttu-id="a41b6-317">15 sekund (15 000 MS)</span><span class="sxs-lookup"><span data-stu-id="a41b6-317">15 seconds (15,000 milliseconds)</span></span> | <span data-ttu-id="a41b6-318">Określa interwał wysyłania komunikatów ping przez klienta.</span><span class="sxs-lookup"><span data-stu-id="a41b6-318">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="a41b6-319">Wysłanie wszelkich komunikatów z klienta powoduje zresetowanie czasomierza do początku interwału.</span><span class="sxs-lookup"><span data-stu-id="a41b6-319">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="a41b6-320">Jeśli klient nie wysłał komunikatu w `ClientTimeoutInterval` zestawie na serwerze, serwer traktuje klienta jako odłączony.</span><span class="sxs-lookup"><span data-stu-id="a41b6-320">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="nettabdotnet"></a>[<span data-ttu-id="a41b6-321">.NET</span><span class="sxs-lookup"><span data-stu-id="a41b6-321">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="a41b6-322">Opcja</span><span class="sxs-lookup"><span data-stu-id="a41b6-322">Option</span></span> | <span data-ttu-id="a41b6-323">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="a41b6-323">Default value</span></span> | <span data-ttu-id="a41b6-324">Opis</span><span class="sxs-lookup"><span data-stu-id="a41b6-324">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="a41b6-325">30 sekund (30 000 MS)</span><span class="sxs-lookup"><span data-stu-id="a41b6-325">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="a41b6-326">Limit czasu dla działania serwera.</span><span class="sxs-lookup"><span data-stu-id="a41b6-326">Timeout for server activity.</span></span> <span data-ttu-id="a41b6-327">Jeśli serwer nie wysłał komunikatu w tym interwale, Klient uważa, że serwer odłączył się i wyzwala `Closed` zdarzenie (`onclose` w języku JavaScript).</span><span class="sxs-lookup"><span data-stu-id="a41b6-327">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="a41b6-328">Ta wartość musi być wystarczająca do wysłania wiadomości ping z serwera **i** odebrana przez klienta w ramach przedziału czasu.</span><span class="sxs-lookup"><span data-stu-id="a41b6-328">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="a41b6-329">Zalecana wartość to liczba z co najmniej podwójną `KeepAliveInterval` wartością serwera, aby umożliwić czas na nadejście poleceń ping.</span><span class="sxs-lookup"><span data-stu-id="a41b6-329">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="a41b6-330">15 sekund</span><span class="sxs-lookup"><span data-stu-id="a41b6-330">15 seconds</span></span> | <span data-ttu-id="a41b6-331">Limit czasu początkowej uzgadniania z serwerem.</span><span class="sxs-lookup"><span data-stu-id="a41b6-331">Timeout for initial server handshake.</span></span> <span data-ttu-id="a41b6-332">Jeśli serwer nie wyśle odpowiedzi uzgadniania w tym interwale, klient anuluje uzgadnianie i wyzwala `Closed` zdarzenie (`onclose` w języku JavaScript).</span><span class="sxs-lookup"><span data-stu-id="a41b6-332">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="a41b6-333">Jest to ustawienie zaawansowane, które należy zmodyfikować tylko w przypadku wystąpienia błędów limitu czasu uzgadniania z powodu poważnego opóźnienia sieci.</span><span class="sxs-lookup"><span data-stu-id="a41b6-333">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="a41b6-334">Aby uzyskać więcej szczegółów na temat procesu uzgadniania, zobacz [specyfikację protokołu centrum sygnału](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="a41b6-334">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

<span data-ttu-id="a41b6-335">W kliencie .NET wartości limitu czasu są określone jako `TimeSpan` wartości.</span><span class="sxs-lookup"><span data-stu-id="a41b6-335">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="a41b6-336">JavaScript</span><span class="sxs-lookup"><span data-stu-id="a41b6-336">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="a41b6-337">Opcja</span><span class="sxs-lookup"><span data-stu-id="a41b6-337">Option</span></span> | <span data-ttu-id="a41b6-338">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="a41b6-338">Default value</span></span> | <span data-ttu-id="a41b6-339">Opis</span><span class="sxs-lookup"><span data-stu-id="a41b6-339">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="a41b6-340">30 sekund (30 000 MS)</span><span class="sxs-lookup"><span data-stu-id="a41b6-340">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="a41b6-341">Limit czasu dla działania serwera.</span><span class="sxs-lookup"><span data-stu-id="a41b6-341">Timeout for server activity.</span></span> <span data-ttu-id="a41b6-342">Jeśli serwer nie wysłał komunikatu w tym interwale, Klient uważa, że serwer odłączył się i wyzwala `onclose` zdarzenie.</span><span class="sxs-lookup"><span data-stu-id="a41b6-342">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="a41b6-343">Ta wartość musi być wystarczająca do wysłania wiadomości ping z serwera **i** odebrana przez klienta w ramach przedziału czasu.</span><span class="sxs-lookup"><span data-stu-id="a41b6-343">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="a41b6-344">Zalecana wartość to liczba z co najmniej podwójną `KeepAliveInterval` wartością serwera, aby umożliwić czas na nadejście poleceń ping.</span><span class="sxs-lookup"><span data-stu-id="a41b6-344">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="a41b6-345">Java</span><span class="sxs-lookup"><span data-stu-id="a41b6-345">Java</span></span>](#tab/java)

| <span data-ttu-id="a41b6-346">Opcja</span><span class="sxs-lookup"><span data-stu-id="a41b6-346">Option</span></span> | <span data-ttu-id="a41b6-347">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="a41b6-347">Default value</span></span> | <span data-ttu-id="a41b6-348">Opis</span><span class="sxs-lookup"><span data-stu-id="a41b6-348">Description</span></span> |
| ------ | ------------- | ----------- |
| <span data-ttu-id="a41b6-349">`getServerTimeout` / `setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="a41b6-349">`getServerTimeout` / `setServerTimeout`</span></span> | <span data-ttu-id="a41b6-350">30 sekund (30 000 MS)</span><span class="sxs-lookup"><span data-stu-id="a41b6-350">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="a41b6-351">Limit czasu dla działania serwera.</span><span class="sxs-lookup"><span data-stu-id="a41b6-351">Timeout for server activity.</span></span> <span data-ttu-id="a41b6-352">Jeśli serwer nie wysłał komunikatu w tym interwale, Klient uważa, że serwer odłączył się i wyzwala `onClose` zdarzenie.</span><span class="sxs-lookup"><span data-stu-id="a41b6-352">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="a41b6-353">Ta wartość musi być wystarczająca do wysłania wiadomości ping z serwera **i** odebrana przez klienta w ramach przedziału czasu.</span><span class="sxs-lookup"><span data-stu-id="a41b6-353">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="a41b6-354">Zalecana wartość to liczba z co najmniej podwójną `KeepAliveInterval` wartością serwera, aby umożliwić czas na nadejście poleceń ping.</span><span class="sxs-lookup"><span data-stu-id="a41b6-354">The recommended value is a number at least double the server's `KeepAliveInterval` value, to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="a41b6-355">15 sekund</span><span class="sxs-lookup"><span data-stu-id="a41b6-355">15 seconds</span></span> | <span data-ttu-id="a41b6-356">Limit czasu początkowej uzgadniania z serwerem.</span><span class="sxs-lookup"><span data-stu-id="a41b6-356">Timeout for initial server handshake.</span></span> <span data-ttu-id="a41b6-357">Jeśli serwer nie wyśle odpowiedzi uzgadniania w tym interwale, klient anuluje uzgadnianie i wyzwala `onClose` zdarzenie.</span><span class="sxs-lookup"><span data-stu-id="a41b6-357">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="a41b6-358">Jest to ustawienie zaawansowane, które należy zmodyfikować tylko w przypadku wystąpienia błędów limitu czasu uzgadniania z powodu poważnego opóźnienia sieci.</span><span class="sxs-lookup"><span data-stu-id="a41b6-358">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="a41b6-359">Aby uzyskać więcej szczegółów na temat procesu uzgadniania, zobacz [specyfikację protokołu centrum sygnału](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="a41b6-359">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

::: moniker-end

---

### <a name="configure-additional-options"></a><span data-ttu-id="a41b6-360">Skonfiguruj dodatkowe opcje</span><span class="sxs-lookup"><span data-stu-id="a41b6-360">Configure additional options</span></span>

<span data-ttu-id="a41b6-361">Dodatkowe opcje `WithUrl` można skonfigurować w metodzie (`withUrl` w języku JavaScript) dla `HubConnectionBuilder` `HttpHubConnectionBuilder` lub w różnych interfejsach API konfiguracji w kliencie Java:</span><span class="sxs-lookup"><span data-stu-id="a41b6-361">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder` or on the various configuration APIs on the `HttpHubConnectionBuilder` in the Java client:</span></span>

# <a name="nettabdotnet"></a>[<span data-ttu-id="a41b6-362">.NET</span><span class="sxs-lookup"><span data-stu-id="a41b6-362">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="a41b6-363">Opcja .NET</span><span class="sxs-lookup"><span data-stu-id="a41b6-363">.NET Option</span></span> |  <span data-ttu-id="a41b6-364">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="a41b6-364">Default value</span></span> | <span data-ttu-id="a41b6-365">Opis</span><span class="sxs-lookup"><span data-stu-id="a41b6-365">Description</span></span> |
| ----------- | -------------- | ----------- |
| `AccessTokenProvider` | `null` | <span data-ttu-id="a41b6-366">Funkcja zwracająca ciąg, który jest dostarczany jako token uwierzytelniania okaziciela w żądaniach HTTP.</span><span class="sxs-lookup"><span data-stu-id="a41b6-366">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `false` | <span data-ttu-id="a41b6-367">Ustaw tę `true` wartość na, aby pominąć krok negocjowania.</span><span class="sxs-lookup"><span data-stu-id="a41b6-367">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="a41b6-368">**Obsługiwane tylko wtedy, gdy transport Sockets jest jedynym włączonym**transportem.</span><span class="sxs-lookup"><span data-stu-id="a41b6-368">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="a41b6-369">Nie można włączyć tego ustawienia w przypadku korzystania z usługi Azure Signal Service.</span><span class="sxs-lookup"><span data-stu-id="a41b6-369">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="a41b6-370">Pusty</span><span class="sxs-lookup"><span data-stu-id="a41b6-370">Empty</span></span> | <span data-ttu-id="a41b6-371">Kolekcja certyfikatów TLS do wysłania w celu uwierzytelniania żądań.</span><span class="sxs-lookup"><span data-stu-id="a41b6-371">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="a41b6-372">Pusty</span><span class="sxs-lookup"><span data-stu-id="a41b6-372">Empty</span></span> | <span data-ttu-id="a41b6-373">Kolekcja plików cookie protokołu HTTP do wysłania w każdym żądaniu HTTP.</span><span class="sxs-lookup"><span data-stu-id="a41b6-373">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="a41b6-374">Pusty</span><span class="sxs-lookup"><span data-stu-id="a41b6-374">Empty</span></span> | <span data-ttu-id="a41b6-375">Poświadczenia do wysyłania przy każdym żądaniu HTTP.</span><span class="sxs-lookup"><span data-stu-id="a41b6-375">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="a41b6-376">5 sekund</span><span class="sxs-lookup"><span data-stu-id="a41b6-376">5 seconds</span></span> | <span data-ttu-id="a41b6-377">Tylko obiekty WebSockets.</span><span class="sxs-lookup"><span data-stu-id="a41b6-377">WebSockets only.</span></span> <span data-ttu-id="a41b6-378">Maksymalny czas oczekiwania klienta po zamknięciu serwera w celu potwierdzenia żądania zamknięcia.</span><span class="sxs-lookup"><span data-stu-id="a41b6-378">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="a41b6-379">Jeśli serwer nie potwierdzi zamknięcia w tym czasie, klient zostanie rozłączony.</span><span class="sxs-lookup"><span data-stu-id="a41b6-379">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="a41b6-380">Pusty</span><span class="sxs-lookup"><span data-stu-id="a41b6-380">Empty</span></span> | <span data-ttu-id="a41b6-381">Mapa dodatkowych nagłówków HTTP do wysłania w każdym żądaniu HTTP.</span><span class="sxs-lookup"><span data-stu-id="a41b6-381">A Map of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | `null` | <span data-ttu-id="a41b6-382">Delegat, który może służyć do konfigurowania lub zastępowania `HttpMessageHandler` używanych do wysyłania żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="a41b6-382">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="a41b6-383">Nieużywane na potrzeby połączeń protokołu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="a41b6-383">Not used for WebSocket connections.</span></span> <span data-ttu-id="a41b6-384">Ten delegat musi zwracać wartość różną od null i otrzymuje wartość domyślną jako parametr.</span><span class="sxs-lookup"><span data-stu-id="a41b6-384">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="a41b6-385">Zmodyfikuj ustawienia tej wartości domyślnej i zwróć ją lub Zwróć nowe `HttpMessageHandler` wystąpienie.</span><span class="sxs-lookup"><span data-stu-id="a41b6-385">Either modify settings on that default value and return it, or return a new `HttpMessageHandler` instance.</span></span> <span data-ttu-id="a41b6-386">**Podczas zastępowania programu obsługi należy skopiować ustawienia, które mają być zachowane z podanej procedury obsługi. w przeciwnym razie skonfigurowane opcje (takie jak pliki cookie i nagłówki) nie będą miały zastosowania do nowego programu obsługi.**</span><span class="sxs-lookup"><span data-stu-id="a41b6-386">**When replacing the handler make sure to copy the settings you want to keep from the provided handler, otherwise, the configured options (such as Cookies and Headers) won't apply to the new handler.**</span></span> |
| `Proxy` | `null` | <span data-ttu-id="a41b6-387">Serwer proxy HTTP, który ma być używany podczas wysyłania żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="a41b6-387">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | `false` | <span data-ttu-id="a41b6-388">Ustaw tę wartość logiczną, aby wysyłać domyślne poświadczenia dla żądań HTTP i WebSockets.</span><span class="sxs-lookup"><span data-stu-id="a41b6-388">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="a41b6-389">Umożliwia to korzystanie z uwierzytelniania systemu Windows.</span><span class="sxs-lookup"><span data-stu-id="a41b6-389">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | `null` | <span data-ttu-id="a41b6-390">Delegat, który może służyć do konfigurowania dodatkowych opcji protokołu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="a41b6-390">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="a41b6-391">Odbiera wystąpienie elementu [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) , którego można użyć do skonfigurowania opcji.</span><span class="sxs-lookup"><span data-stu-id="a41b6-391">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="a41b6-392">JavaScript</span><span class="sxs-lookup"><span data-stu-id="a41b6-392">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="a41b6-393">Opcja języka JavaScript</span><span class="sxs-lookup"><span data-stu-id="a41b6-393">JavaScript Option</span></span> | <span data-ttu-id="a41b6-394">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="a41b6-394">Default Value</span></span> | <span data-ttu-id="a41b6-395">Opis</span><span class="sxs-lookup"><span data-stu-id="a41b6-395">Description</span></span> |
| ----------------- | ------------- | ----------- |
| `accessTokenFactory` | `null` | <span data-ttu-id="a41b6-396">Funkcja zwracająca ciąg, który jest dostarczany jako token uwierzytelniania okaziciela w żądaniach HTTP.</span><span class="sxs-lookup"><span data-stu-id="a41b6-396">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `skipNegotiation` | `false` | <span data-ttu-id="a41b6-397">Ustaw tę `true` wartość na, aby pominąć krok negocjowania.</span><span class="sxs-lookup"><span data-stu-id="a41b6-397">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="a41b6-398">**Obsługiwane tylko wtedy, gdy transport Sockets jest jedynym włączonym**transportem.</span><span class="sxs-lookup"><span data-stu-id="a41b6-398">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="a41b6-399">Nie można włączyć tego ustawienia w przypadku korzystania z usługi Azure Signal Service.</span><span class="sxs-lookup"><span data-stu-id="a41b6-399">This setting can't be enabled when using the Azure SignalR Service.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="a41b6-400">Java</span><span class="sxs-lookup"><span data-stu-id="a41b6-400">Java</span></span>](#tab/java)

| <span data-ttu-id="a41b6-401">Opcja Java</span><span class="sxs-lookup"><span data-stu-id="a41b6-401">Java Option</span></span> | <span data-ttu-id="a41b6-402">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="a41b6-402">Default Value</span></span> | <span data-ttu-id="a41b6-403">Opis</span><span class="sxs-lookup"><span data-stu-id="a41b6-403">Description</span></span> |
| ----------- | ------------- | ----------- |
| `withAccessTokenProvider` | `null` | <span data-ttu-id="a41b6-404">Funkcja zwracająca ciąg, który jest dostarczany jako token uwierzytelniania okaziciela w żądaniach HTTP.</span><span class="sxs-lookup"><span data-stu-id="a41b6-404">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `shouldSkipNegotiate` | `false` | <span data-ttu-id="a41b6-405">Ustaw tę `true` wartość na, aby pominąć krok negocjowania.</span><span class="sxs-lookup"><span data-stu-id="a41b6-405">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="a41b6-406">**Obsługiwane tylko wtedy, gdy transport Sockets jest jedynym włączonym**transportem.</span><span class="sxs-lookup"><span data-stu-id="a41b6-406">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="a41b6-407">Nie można włączyć tego ustawienia w przypadku korzystania z usługi Azure Signal Service.</span><span class="sxs-lookup"><span data-stu-id="a41b6-407">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| <span data-ttu-id="a41b6-408">`withHeader``withHeaders`</span><span class="sxs-lookup"><span data-stu-id="a41b6-408">`withHeader` `withHeaders`</span></span> | <span data-ttu-id="a41b6-409">Pusty</span><span class="sxs-lookup"><span data-stu-id="a41b6-409">Empty</span></span> | <span data-ttu-id="a41b6-410">Mapa dodatkowych nagłówków HTTP do wysłania w każdym żądaniu HTTP.</span><span class="sxs-lookup"><span data-stu-id="a41b6-410">A Map of additional HTTP headers to send with every HTTP request.</span></span> |

---

<span data-ttu-id="a41b6-411">W kliencie platformy .NET te opcje można modyfikować za pomocą delegata opcji udostępnianego dla `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="a41b6-411">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="a41b6-412">W kliencie JavaScript te opcje można podać w obiekcie JavaScript udostępnionym `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="a41b6-412">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

<span data-ttu-id="a41b6-413">W kliencie Java Opcje te można skonfigurować przy użyciu metod `HttpHubConnectionBuilder` zwróconych z`HubConnectionBuilder.create("HUB URL")`</span><span class="sxs-lookup"><span data-stu-id="a41b6-413">In the Java client, these options can be configured with the methods on the `HttpHubConnectionBuilder` returned from the `HubConnectionBuilder.create("HUB URL")`</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
        .withHeader("Foo", "Bar")
        .shouldSkipNegotiate(true)
        .withHandshakeResponseTimeout(30*1000)
        .build();
```

## <a name="additional-resources"></a><span data-ttu-id="a41b6-414">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="a41b6-414">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>
