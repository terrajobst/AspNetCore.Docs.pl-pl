---
title: Konfiguracja sygnałów ASP.NET Core
author: bradygaster
description: Dowiedz się, jak skonfigurować aplikacje sygnalizujące ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 08/05/2019
uid: signalr/configuration
ms.openlocfilehash: 475d9664c588c06bfcd816959be8a425ee01c023
ms.sourcegitcommit: 776367717e990bdd600cb3c9148ffb905d56862d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/09/2019
ms.locfileid: "68915083"
---
# <a name="aspnet-core-signalr-configuration"></a><span data-ttu-id="6c39e-103">Konfiguracja sygnałów ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6c39e-103">ASP.NET Core SignalR configuration</span></span>

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="6c39e-104">Opcje serializacji JSON/MessagePack</span><span class="sxs-lookup"><span data-stu-id="6c39e-104">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="6c39e-105">ASP.NET Core sygnalizujący obsługuje dwa protokoły dla komunikatów kodowania: [JSON](https://www.json.org/) i [MessagePack](https://msgpack.org/index.html).</span><span class="sxs-lookup"><span data-stu-id="6c39e-105">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="6c39e-106">Każdy protokół ma opcje konfiguracji serializacji.</span><span class="sxs-lookup"><span data-stu-id="6c39e-106">Each protocol has serialization configuration options.</span></span>

<span data-ttu-id="6c39e-107">Serializacji JSON można skonfigurować na serwerze przy użyciu metody rozszerzenia [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) , która może zostać dodana po `Startup.ConfigureServices` metodzie [addsygnalizująca](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) w ramach metody.</span><span class="sxs-lookup"><span data-stu-id="6c39e-107">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method, which can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in your `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="6c39e-108">Metoda przyjmuje delegata, który `options` odbiera obiekt. `AddJsonProtocol`</span><span class="sxs-lookup"><span data-stu-id="6c39e-108">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="6c39e-109">Właściwość [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) tego obiektu jest obiektem JSON.NET `JsonSerializerSettings` , którego można użyć do skonfigurowania serializacji argumentów i zwracanych wartości.</span><span class="sxs-lookup"><span data-stu-id="6c39e-109">The [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) property on that object is a JSON.NET `JsonSerializerSettings` object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="6c39e-110">Aby uzyskać więcej informacji, zobacz [dokumentację JSON.NET](https://www.newtonsoft.com/json/help/html/Introduction.htm) .</span><span class="sxs-lookup"><span data-stu-id="6c39e-110">See the [JSON.NET Documentation](https://www.newtonsoft.com/json/help/html/Introduction.htm) for more details.</span></span>

<span data-ttu-id="6c39e-111">Przykładowo, aby skonfigurować serializator do używania nazw właściwości "PascalCase" zamiast domyślnych nazw "camelCase", należy użyć następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="6c39e-111">As an example, to configure the serializer to use "PascalCase" property names, instead of the default "camelCase" names, use the following code:</span></span>

```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver =
        new DefaultContractResolver();
    });
```

<span data-ttu-id="6c39e-112">W kliencie .NET ta sama `AddJsonProtocol` Metoda rozszerzająca istnieje w [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="6c39e-112">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="6c39e-113">`Microsoft.Extensions.DependencyInjection` Przestrzeń nazw musi zostać zaimportowana, aby można było rozpoznać metodę rozszerzenia:</span><span class="sxs-lookup"><span data-stu-id="6c39e-113">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

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
> <span data-ttu-id="6c39e-114">W tej chwili nie można skonfigurować serializacji JSON w kliencie JavaScript.</span><span class="sxs-lookup"><span data-stu-id="6c39e-114">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="6c39e-115">Opcje serializacji MessagePack</span><span class="sxs-lookup"><span data-stu-id="6c39e-115">MessagePack serialization options</span></span>

<span data-ttu-id="6c39e-116">Serializacji MessagePack można skonfigurować, dostarczając delegata do wywołania [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) .</span><span class="sxs-lookup"><span data-stu-id="6c39e-116">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="6c39e-117">Aby uzyskać więcej informacji, zobacz [MessagePack w sygnalizacji](xref:signalr/messagepackhubprotocol) .</span><span class="sxs-lookup"><span data-stu-id="6c39e-117">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="6c39e-118">W tej chwili nie można skonfigurować serializacji MessagePack w kliencie JavaScript.</span><span class="sxs-lookup"><span data-stu-id="6c39e-118">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="6c39e-119">Konfigurowanie opcji serwera</span><span class="sxs-lookup"><span data-stu-id="6c39e-119">Configure server options</span></span>

<span data-ttu-id="6c39e-120">W poniższej tabeli opisano opcje konfigurowania centrów sygnałów:</span><span class="sxs-lookup"><span data-stu-id="6c39e-120">The following table describes options for configuring SignalR hubs:</span></span>

::: moniker range=">= aspnetcore-3.0"

| <span data-ttu-id="6c39e-121">Opcja</span><span class="sxs-lookup"><span data-stu-id="6c39e-121">Option</span></span> | <span data-ttu-id="6c39e-122">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="6c39e-122">Default Value</span></span> | <span data-ttu-id="6c39e-123">Opis</span><span class="sxs-lookup"><span data-stu-id="6c39e-123">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="6c39e-124">30 sekund</span><span class="sxs-lookup"><span data-stu-id="6c39e-124">30 seconds</span></span> | <span data-ttu-id="6c39e-125">Serwer Rozważ odłączenie klienta, jeśli nie odebrano komunikatu (w tym w tym interwale).</span><span class="sxs-lookup"><span data-stu-id="6c39e-125">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="6c39e-126">Może upłynąć dłużej niż ten interwał limitu czasu, aby klient mógł zostać oznaczony jako odłączony, ze względu na to, jak jest to zaimplementowane.</span><span class="sxs-lookup"><span data-stu-id="6c39e-126">It could take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="6c39e-127">Zalecaną wartością jest podwójna `KeepAliveInterval` wartość.</span><span class="sxs-lookup"><span data-stu-id="6c39e-127">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="6c39e-128">15 sekund</span><span class="sxs-lookup"><span data-stu-id="6c39e-128">15 seconds</span></span> | <span data-ttu-id="6c39e-129">Jeśli klient nie wyśle początkowego komunikatu uzgadniania w tym przedziale czasu, połączenie zostanie zamknięte.</span><span class="sxs-lookup"><span data-stu-id="6c39e-129">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="6c39e-130">Jest to ustawienie zaawansowane, które należy zmodyfikować tylko w przypadku wystąpienia błędów limitu czasu uzgadniania z powodu poważnego opóźnienia sieci.</span><span class="sxs-lookup"><span data-stu-id="6c39e-130">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="6c39e-131">Aby uzyskać więcej szczegółów na temat procesu uzgadniania, zobacz [specyfikację protokołu centrum sygnału](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="6c39e-131">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="6c39e-132">15 sekund</span><span class="sxs-lookup"><span data-stu-id="6c39e-132">15 seconds</span></span> | <span data-ttu-id="6c39e-133">Jeśli serwer nie wysłał komunikatu w tym interwale, zostanie automatycznie wysłana wiadomość ping w celu pozostawienia otwartego połączenia.</span><span class="sxs-lookup"><span data-stu-id="6c39e-133">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="6c39e-134">W przypadku `KeepAliveInterval`zmiany należy `ServerTimeout` / zmienić`serverTimeoutInMilliseconds` ustawienie na kliencie.</span><span class="sxs-lookup"><span data-stu-id="6c39e-134">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="6c39e-135">Zalecaną `ServerTimeout` / wartościąjest`KeepAliveInterval` podwójna wartość. `serverTimeoutInMilliseconds`</span><span class="sxs-lookup"><span data-stu-id="6c39e-135">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="6c39e-136">Wszystkie zainstalowane protokoły</span><span class="sxs-lookup"><span data-stu-id="6c39e-136">All installed protocols</span></span> | <span data-ttu-id="6c39e-137">Protokoły obsługiwane przez to centrum.</span><span class="sxs-lookup"><span data-stu-id="6c39e-137">Protocols supported by this hub.</span></span> <span data-ttu-id="6c39e-138">Domyślnie wszystkie protokoły zarejestrowane na serwerze są dozwolone, ale protokoły można usunąć z tej listy, aby wyłączyć określone protokoły dla poszczególnych centrów.</span><span class="sxs-lookup"><span data-stu-id="6c39e-138">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="6c39e-139">Jeśli `true`szczegółowe komunikaty o wyjątkach są zwracane do klientów, gdy wyjątek jest zgłaszany w metodzie centrum.</span><span class="sxs-lookup"><span data-stu-id="6c39e-139">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="6c39e-140">Wartość domyślna to `false`, ponieważ te komunikaty o wyjątkach mogą zawierać informacje poufne.</span><span class="sxs-lookup"><span data-stu-id="6c39e-140">The default is `false`, as these exception messages can contain sensitive information.</span></span> |
| `StreamBufferCapacity` | `10` | <span data-ttu-id="6c39e-141">Maksymalna liczba elementów, które mogą być buforowane dla strumieni przekazywania klientów.</span><span class="sxs-lookup"><span data-stu-id="6c39e-141">The maximum number of items that can be buffered for client upload streams.</span></span> <span data-ttu-id="6c39e-142">Po osiągnięciu tego limitu przetwarzanie wywołań jest blokowane, dopóki serwer nie przetwarza elementów strumienia.</span><span class="sxs-lookup"><span data-stu-id="6c39e-142">If this limit is reached, the processing of invocations is blocked until the server processes stream items.</span></span>|
| `MaximumReceiveMessageSize` | <span data-ttu-id="6c39e-143">32 KB</span><span class="sxs-lookup"><span data-stu-id="6c39e-143">32 KB</span></span> | <span data-ttu-id="6c39e-144">Maksymalny rozmiar pojedynczego przychodzącego komunikatu centrum.</span><span class="sxs-lookup"><span data-stu-id="6c39e-144">Maximum size of a single incoming hub message.</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-2.2"

| <span data-ttu-id="6c39e-145">Opcja</span><span class="sxs-lookup"><span data-stu-id="6c39e-145">Option</span></span> | <span data-ttu-id="6c39e-146">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="6c39e-146">Default Value</span></span> | <span data-ttu-id="6c39e-147">Opis</span><span class="sxs-lookup"><span data-stu-id="6c39e-147">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="6c39e-148">30 sekund</span><span class="sxs-lookup"><span data-stu-id="6c39e-148">30 seconds</span></span> | <span data-ttu-id="6c39e-149">Serwer Rozważ odłączenie klienta, jeśli nie odebrano komunikatu (w tym w tym interwale).</span><span class="sxs-lookup"><span data-stu-id="6c39e-149">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="6c39e-150">Może upłynąć dłużej niż ten interwał limitu czasu, aby klient mógł zostać oznaczony jako odłączony, ze względu na to, jak jest to zaimplementowane.</span><span class="sxs-lookup"><span data-stu-id="6c39e-150">It could take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="6c39e-151">Zalecaną wartością jest podwójna `KeepAliveInterval` wartość.</span><span class="sxs-lookup"><span data-stu-id="6c39e-151">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="6c39e-152">15 sekund</span><span class="sxs-lookup"><span data-stu-id="6c39e-152">15 seconds</span></span> | <span data-ttu-id="6c39e-153">Jeśli klient nie wyśle początkowego komunikatu uzgadniania w tym przedziale czasu, połączenie zostanie zamknięte.</span><span class="sxs-lookup"><span data-stu-id="6c39e-153">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="6c39e-154">Jest to ustawienie zaawansowane, które należy zmodyfikować tylko w przypadku wystąpienia błędów limitu czasu uzgadniania z powodu poważnego opóźnienia sieci.</span><span class="sxs-lookup"><span data-stu-id="6c39e-154">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="6c39e-155">Aby uzyskać więcej szczegółów na temat procesu uzgadniania, zobacz [specyfikację protokołu centrum sygnału](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="6c39e-155">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="6c39e-156">15 sekund</span><span class="sxs-lookup"><span data-stu-id="6c39e-156">15 seconds</span></span> | <span data-ttu-id="6c39e-157">Jeśli serwer nie wysłał komunikatu w tym interwale, zostanie automatycznie wysłana wiadomość ping w celu pozostawienia otwartego połączenia.</span><span class="sxs-lookup"><span data-stu-id="6c39e-157">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="6c39e-158">W przypadku `KeepAliveInterval`zmiany należy `ServerTimeout` / zmienić`serverTimeoutInMilliseconds` ustawienie na kliencie.</span><span class="sxs-lookup"><span data-stu-id="6c39e-158">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="6c39e-159">Zalecaną `ServerTimeout` / wartościąjest`KeepAliveInterval` podwójna wartość. `serverTimeoutInMilliseconds`</span><span class="sxs-lookup"><span data-stu-id="6c39e-159">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="6c39e-160">Wszystkie zainstalowane protokoły</span><span class="sxs-lookup"><span data-stu-id="6c39e-160">All installed protocols</span></span> | <span data-ttu-id="6c39e-161">Protokoły obsługiwane przez to centrum.</span><span class="sxs-lookup"><span data-stu-id="6c39e-161">Protocols supported by this hub.</span></span> <span data-ttu-id="6c39e-162">Domyślnie wszystkie protokoły zarejestrowane na serwerze są dozwolone, ale protokoły można usunąć z tej listy, aby wyłączyć określone protokoły dla poszczególnych centrów.</span><span class="sxs-lookup"><span data-stu-id="6c39e-162">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="6c39e-163">Jeśli `true`szczegółowe komunikaty o wyjątkach są zwracane do klientów, gdy wyjątek jest zgłaszany w metodzie centrum.</span><span class="sxs-lookup"><span data-stu-id="6c39e-163">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="6c39e-164">Wartość domyślna to `false`, ponieważ te komunikaty o wyjątkach mogą zawierać informacje poufne.</span><span class="sxs-lookup"><span data-stu-id="6c39e-164">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-2.1"

| <span data-ttu-id="6c39e-165">Opcja</span><span class="sxs-lookup"><span data-stu-id="6c39e-165">Option</span></span> | <span data-ttu-id="6c39e-166">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="6c39e-166">Default Value</span></span> | <span data-ttu-id="6c39e-167">Opis</span><span class="sxs-lookup"><span data-stu-id="6c39e-167">Description</span></span> |
| ------ | ------------- | ----------- |
| `HandshakeTimeout` | <span data-ttu-id="6c39e-168">15 sekund</span><span class="sxs-lookup"><span data-stu-id="6c39e-168">15 seconds</span></span> | <span data-ttu-id="6c39e-169">Jeśli klient nie wyśle początkowego komunikatu uzgadniania w tym przedziale czasu, połączenie zostanie zamknięte.</span><span class="sxs-lookup"><span data-stu-id="6c39e-169">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="6c39e-170">Jest to ustawienie zaawansowane, które należy zmodyfikować tylko w przypadku wystąpienia błędów limitu czasu uzgadniania z powodu poważnego opóźnienia sieci.</span><span class="sxs-lookup"><span data-stu-id="6c39e-170">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="6c39e-171">Aby uzyskać więcej szczegółów na temat procesu uzgadniania, zobacz [specyfikację protokołu centrum sygnału](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="6c39e-171">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="6c39e-172">15 sekund</span><span class="sxs-lookup"><span data-stu-id="6c39e-172">15 seconds</span></span> | <span data-ttu-id="6c39e-173">Jeśli serwer nie wysłał komunikatu w tym interwale, zostanie automatycznie wysłana wiadomość ping w celu pozostawienia otwartego połączenia.</span><span class="sxs-lookup"><span data-stu-id="6c39e-173">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="6c39e-174">W przypadku `KeepAliveInterval`zmiany należy `ServerTimeout` / zmienić`serverTimeoutInMilliseconds` ustawienie na kliencie.</span><span class="sxs-lookup"><span data-stu-id="6c39e-174">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="6c39e-175">Zalecaną `ServerTimeout` / wartościąjest`KeepAliveInterval` podwójna wartość. `serverTimeoutInMilliseconds`</span><span class="sxs-lookup"><span data-stu-id="6c39e-175">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="6c39e-176">Wszystkie zainstalowane protokoły</span><span class="sxs-lookup"><span data-stu-id="6c39e-176">All installed protocols</span></span> | <span data-ttu-id="6c39e-177">Protokoły obsługiwane przez to centrum.</span><span class="sxs-lookup"><span data-stu-id="6c39e-177">Protocols supported by this hub.</span></span> <span data-ttu-id="6c39e-178">Domyślnie wszystkie protokoły zarejestrowane na serwerze są dozwolone, ale protokoły można usunąć z tej listy, aby wyłączyć określone protokoły dla poszczególnych centrów.</span><span class="sxs-lookup"><span data-stu-id="6c39e-178">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="6c39e-179">Jeśli `true`szczegółowe komunikaty o wyjątkach są zwracane do klientów, gdy wyjątek jest zgłaszany w metodzie centrum.</span><span class="sxs-lookup"><span data-stu-id="6c39e-179">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="6c39e-180">Wartość domyślna to `false`, ponieważ te komunikaty o wyjątkach mogą zawierać informacje poufne.</span><span class="sxs-lookup"><span data-stu-id="6c39e-180">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

::: moniker-end

<span data-ttu-id="6c39e-181">Opcje można skonfigurować dla wszystkich centrów, dostarczając opcje delegata do `AddSignalR` wywołania w. `Startup.ConfigureServices`</span><span class="sxs-lookup"><span data-stu-id="6c39e-181">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

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

<span data-ttu-id="6c39e-182">Opcje jednego centrum przesłaniają opcje globalne podane w `AddSignalR` i można je skonfigurować przy użyciu: <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*></span><span class="sxs-lookup"><span data-stu-id="6c39e-182">Options for a single hub override the global options provided in `AddSignalR` and can be configured using <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

### <a name="advanced-http-configuration-options"></a><span data-ttu-id="6c39e-183">Zaawansowane opcje konfiguracji HTTP</span><span class="sxs-lookup"><span data-stu-id="6c39e-183">Advanced HTTP configuration options</span></span>

<span data-ttu-id="6c39e-184">Służy `HttpConnectionDispatcherOptions` do konfigurowania ustawień zaawansowanych związanych z transportami i zarządzaniem buforem pamięci.</span><span class="sxs-lookup"><span data-stu-id="6c39e-184">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="6c39e-185">Te opcje są konfigurowane przez przekazanie delegata [do\<MapHub T >](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) w. `Startup.Configure`</span><span class="sxs-lookup"><span data-stu-id="6c39e-185">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) in `Startup.Configure`.</span></span>

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

<span data-ttu-id="6c39e-186">W poniższej tabeli opisano opcje konfigurowania zaawansowanych opcji protokołu HTTP w programie ASP.NET Core Signal:</span><span class="sxs-lookup"><span data-stu-id="6c39e-186">The following table describes options for configuring ASP.NET Core SignalR's advanced HTTP options:</span></span>

::: moniker range=">= aspnetcore-3.0"

| <span data-ttu-id="6c39e-187">Opcja</span><span class="sxs-lookup"><span data-stu-id="6c39e-187">Option</span></span> | <span data-ttu-id="6c39e-188">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="6c39e-188">Default Value</span></span> | <span data-ttu-id="6c39e-189">Opis</span><span class="sxs-lookup"><span data-stu-id="6c39e-189">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="6c39e-190">32 KB</span><span class="sxs-lookup"><span data-stu-id="6c39e-190">32 KB</span></span> | <span data-ttu-id="6c39e-191">Maksymalna liczba bajtów odebranych od klienta, które buforują serwer przed zastosowaniem nadużycia.</span><span class="sxs-lookup"><span data-stu-id="6c39e-191">The maximum number of bytes received from the client that the server buffers before applying backpressure.</span></span> <span data-ttu-id="6c39e-192">Zwiększenie tej wartości umożliwia serwerowi otrzymywanie większych komunikatów szybciej bez naciskania, ale może zwiększyć zużycie pamięci.</span><span class="sxs-lookup"><span data-stu-id="6c39e-192">Increasing this value allows the server to receive larger messages more quickly without applying backpressure, but can increase memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="6c39e-193">Dane zbierane automatycznie z `Authorize` atrybutów zastosowanych do klasy centrum.</span><span class="sxs-lookup"><span data-stu-id="6c39e-193">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="6c39e-194">Lista obiektów [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) służąca do określenia, czy klient jest autoryzowany do łączenia się z centrum.</span><span class="sxs-lookup"><span data-stu-id="6c39e-194">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="6c39e-195">32 KB</span><span class="sxs-lookup"><span data-stu-id="6c39e-195">32 KB</span></span> | <span data-ttu-id="6c39e-196">Maksymalna liczba bajtów wysłanych przez aplikację, które buforuje serwer przed zaobserwowaniem presji.</span><span class="sxs-lookup"><span data-stu-id="6c39e-196">The maximum number of bytes sent by the app that the server buffers before observing backpressure.</span></span> <span data-ttu-id="6c39e-197">Zwiększenie tej wartości umożliwia serwerowi przebuforowanie większych komunikatów szybciej bez czekania na obciążenie, ale może zwiększyć zużycie pamięci.</span><span class="sxs-lookup"><span data-stu-id="6c39e-197">Increasing this value allows the server to buffer larger messages more quickly without awaiting backpressure, but can increase memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="6c39e-198">Wszystkie transporty są włączone.</span><span class="sxs-lookup"><span data-stu-id="6c39e-198">All Transports are enabled.</span></span> | <span data-ttu-id="6c39e-199">Wyliczenie flag bitowych `HttpTransportType` wartości, które mogą ograniczyć transporty, których klient może użyć do nawiązania połączenia.</span><span class="sxs-lookup"><span data-stu-id="6c39e-199">A bit flags enum of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="6c39e-200">Zobacz poniżej.</span><span class="sxs-lookup"><span data-stu-id="6c39e-200">See below.</span></span> | <span data-ttu-id="6c39e-201">Dodatkowe opcje specyficzne dla długiego transportu sondowania.</span><span class="sxs-lookup"><span data-stu-id="6c39e-201">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="6c39e-202">Zobacz poniżej.</span><span class="sxs-lookup"><span data-stu-id="6c39e-202">See below.</span></span> | <span data-ttu-id="6c39e-203">Dodatkowe opcje specyficzne dla transportu obiektów WebSockets.</span><span class="sxs-lookup"><span data-stu-id="6c39e-203">Additional options specific to the WebSockets transport.</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-3.0"

| <span data-ttu-id="6c39e-204">Opcja</span><span class="sxs-lookup"><span data-stu-id="6c39e-204">Option</span></span> | <span data-ttu-id="6c39e-205">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="6c39e-205">Default Value</span></span> | <span data-ttu-id="6c39e-206">Opis</span><span class="sxs-lookup"><span data-stu-id="6c39e-206">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="6c39e-207">32 KB</span><span class="sxs-lookup"><span data-stu-id="6c39e-207">32 KB</span></span> | <span data-ttu-id="6c39e-208">Maksymalna liczba bajtów odebranych od klienta, które buforuje serwer.</span><span class="sxs-lookup"><span data-stu-id="6c39e-208">The maximum number of bytes received from the client that the server buffers.</span></span> <span data-ttu-id="6c39e-209">Zwiększenie tej wartości umożliwia serwerowi otrzymywanie większych komunikatów, ale może mieć negatywny wpływ na użycie pamięci.</span><span class="sxs-lookup"><span data-stu-id="6c39e-209">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="6c39e-210">Dane zbierane automatycznie z `Authorize` atrybutów zastosowanych do klasy centrum.</span><span class="sxs-lookup"><span data-stu-id="6c39e-210">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="6c39e-211">Lista obiektów [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) służąca do określenia, czy klient jest autoryzowany do łączenia się z centrum.</span><span class="sxs-lookup"><span data-stu-id="6c39e-211">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="6c39e-212">32 KB</span><span class="sxs-lookup"><span data-stu-id="6c39e-212">32 KB</span></span> | <span data-ttu-id="6c39e-213">Maksymalna liczba bajtów wysłanych przez aplikację, które buforują serwer.</span><span class="sxs-lookup"><span data-stu-id="6c39e-213">The maximum number of bytes sent by the app that the server buffers.</span></span> <span data-ttu-id="6c39e-214">Zwiększenie tej wartości umożliwia serwerowi wysyłanie większych komunikatów, ale może mieć negatywny wpływ na użycie pamięci.</span><span class="sxs-lookup"><span data-stu-id="6c39e-214">Increasing this value allows the server to send larger messages, but can negatively impact memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="6c39e-215">Wszystkie transporty są włączone.</span><span class="sxs-lookup"><span data-stu-id="6c39e-215">All Transports are enabled.</span></span> | <span data-ttu-id="6c39e-216">Wyliczenie flag bitowych `HttpTransportType` wartości, które mogą ograniczyć transporty, których klient może użyć do nawiązania połączenia.</span><span class="sxs-lookup"><span data-stu-id="6c39e-216">A bit flags enum of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="6c39e-217">Zobacz poniżej.</span><span class="sxs-lookup"><span data-stu-id="6c39e-217">See below.</span></span> | <span data-ttu-id="6c39e-218">Dodatkowe opcje specyficzne dla długiego transportu sondowania.</span><span class="sxs-lookup"><span data-stu-id="6c39e-218">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="6c39e-219">Zobacz poniżej.</span><span class="sxs-lookup"><span data-stu-id="6c39e-219">See below.</span></span> | <span data-ttu-id="6c39e-220">Dodatkowe opcje specyficzne dla transportu obiektów WebSockets.</span><span class="sxs-lookup"><span data-stu-id="6c39e-220">Additional options specific to the WebSockets transport.</span></span> |

::: moniker-end

<span data-ttu-id="6c39e-221">W przypadku długiego transportu sondowania dostępne są dodatkowe opcje, które można `LongPolling` skonfigurować przy użyciu właściwości:</span><span class="sxs-lookup"><span data-stu-id="6c39e-221">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="6c39e-222">Opcja</span><span class="sxs-lookup"><span data-stu-id="6c39e-222">Option</span></span> | <span data-ttu-id="6c39e-223">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="6c39e-223">Default Value</span></span> | <span data-ttu-id="6c39e-224">Opis</span><span class="sxs-lookup"><span data-stu-id="6c39e-224">Description</span></span> |
| ------ | ------------- | ----------- |
| `PollTimeout` | <span data-ttu-id="6c39e-225">90 sekund</span><span class="sxs-lookup"><span data-stu-id="6c39e-225">90 seconds</span></span> | <span data-ttu-id="6c39e-226">Maksymalny czas, przez jaki serwer czeka na wysłanie wiadomości do klienta przed zakończeniem jednego żądania sondowania.</span><span class="sxs-lookup"><span data-stu-id="6c39e-226">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="6c39e-227">Zmniejszenie tej wartości powoduje, że klient wystawia nowe żądania sondy częściej.</span><span class="sxs-lookup"><span data-stu-id="6c39e-227">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> |

<span data-ttu-id="6c39e-228">Transport WebSocket ma dodatkowe opcje, które można skonfigurować przy użyciu `WebSockets` właściwości:</span><span class="sxs-lookup"><span data-stu-id="6c39e-228">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="6c39e-229">Opcja</span><span class="sxs-lookup"><span data-stu-id="6c39e-229">Option</span></span> | <span data-ttu-id="6c39e-230">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="6c39e-230">Default Value</span></span> | <span data-ttu-id="6c39e-231">Opis</span><span class="sxs-lookup"><span data-stu-id="6c39e-231">Description</span></span> |
| ------ | ------------- | ----------- |
| `CloseTimeout` | <span data-ttu-id="6c39e-232">5 sekund</span><span class="sxs-lookup"><span data-stu-id="6c39e-232">5 seconds</span></span> | <span data-ttu-id="6c39e-233">Gdy serwer zostanie zamknięty, jeśli klient nie zostanie zamknięty w tym okresie, połączenie zostanie przerwane.</span><span class="sxs-lookup"><span data-stu-id="6c39e-233">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | `null` | <span data-ttu-id="6c39e-234">Delegat, którego można użyć do ustawienia `Sec-WebSocket-Protocol` nagłówka na wartość niestandardową.</span><span class="sxs-lookup"><span data-stu-id="6c39e-234">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="6c39e-235">Delegat otrzymuje wartości żądane przez klienta jako dane wejściowe i oczekuje na zwrócenie żądanej wartości.</span><span class="sxs-lookup"><span data-stu-id="6c39e-235">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="6c39e-236">Konfigurowanie opcji klienta</span><span class="sxs-lookup"><span data-stu-id="6c39e-236">Configure client options</span></span>

<span data-ttu-id="6c39e-237">Opcje klienta można skonfigurować dla `HubConnectionBuilder` typu (dostępnego w klientach .NET i JavaScript).</span><span class="sxs-lookup"><span data-stu-id="6c39e-237">Client options can be configured on the `HubConnectionBuilder` type (available in the .NET and JavaScript clients).</span></span> <span data-ttu-id="6c39e-238">Jest ona również dostępna w kliencie Java, ale `HttpHubConnectionBuilder` podklasa zawiera opcje konfiguracji konstruktora, a także `HubConnection` samego siebie.</span><span class="sxs-lookup"><span data-stu-id="6c39e-238">It's also available in the Java client, but the `HttpHubConnectionBuilder` subclass is what contains the builder configuration options, as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="6c39e-239">Konfigurowanie rejestrowania</span><span class="sxs-lookup"><span data-stu-id="6c39e-239">Configure logging</span></span>

<span data-ttu-id="6c39e-240">Rejestrowanie jest konfigurowane w kliencie .NET przy użyciu `ConfigureLogging` metody.</span><span class="sxs-lookup"><span data-stu-id="6c39e-240">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="6c39e-241">Dostawcy i filtry rejestrowania mogą być rejestrowane w taki sam sposób, w jaki znajdują się na serwerze.</span><span class="sxs-lookup"><span data-stu-id="6c39e-241">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="6c39e-242">Aby uzyskać więcej informacji, zobacz dokumentację dotyczącą [rejestrowania w ASP.NET Core](xref:fundamentals/logging/index) .</span><span class="sxs-lookup"><span data-stu-id="6c39e-242">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="6c39e-243">Aby zarejestrować dostawców rejestrowania, należy zainstalować wymagane pakiety.</span><span class="sxs-lookup"><span data-stu-id="6c39e-243">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="6c39e-244">Aby zapoznać się z pełną listą, zobacz sekcję [wbudowane dostawcy rejestrowania](xref:fundamentals/logging/index#built-in-logging-providers) w witrynie docs.</span><span class="sxs-lookup"><span data-stu-id="6c39e-244">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="6c39e-245">Na przykład aby włączyć rejestrowanie konsoli, zainstaluj `Microsoft.Extensions.Logging.Console` pakiet NuGet.</span><span class="sxs-lookup"><span data-stu-id="6c39e-245">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="6c39e-246">Wywołaj `AddConsole` metodę rozszerzenia:</span><span class="sxs-lookup"><span data-stu-id="6c39e-246">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="6c39e-247">W kliencie JavaScript istnieje podobna `configureLogging` Metoda.</span><span class="sxs-lookup"><span data-stu-id="6c39e-247">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="6c39e-248">`LogLevel` Podaj wartość wskazującą minimalny poziom komunikatów dziennika do wygenerowania.</span><span class="sxs-lookup"><span data-stu-id="6c39e-248">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="6c39e-249">Dzienniki są zapisywane w oknie konsoli przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="6c39e-249">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="6c39e-250">Zamiast wartości można także podać wartość reprezentującą nazwę poziomu dziennika. `string` `LogLevel`</span><span class="sxs-lookup"><span data-stu-id="6c39e-250">Instead of a `LogLevel` value, you can also provide a `string` value representing a log level name.</span></span> <span data-ttu-id="6c39e-251">Jest to przydatne podczas konfigurowania rejestrowania sygnałów w środowiskach, w których nie masz dostępu do `LogLevel` stałych.</span><span class="sxs-lookup"><span data-stu-id="6c39e-251">This is useful when configuring SignalR logging in environments where you don't have access to the `LogLevel` constants.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging("warn")
    .build();
```

<span data-ttu-id="6c39e-252">Poniższa tabela zawiera listę dostępnych poziomów dzienników.</span><span class="sxs-lookup"><span data-stu-id="6c39e-252">The following table lists the available log levels.</span></span> <span data-ttu-id="6c39e-253">Wartość określana przez użytkownika `configureLogging` w celu ustawienia minimalnego poziomu dziennika, który zostanie zarejestrowany.</span><span class="sxs-lookup"><span data-stu-id="6c39e-253">The value you provide to `configureLogging` sets the **minimum** log level that will be logged.</span></span> <span data-ttu-id="6c39e-254">Komunikaty zarejestrowane na tym poziomie **lub poziomy wymienione po nim w tabeli**zostaną zarejestrowane.</span><span class="sxs-lookup"><span data-stu-id="6c39e-254">Messages logged at this level, **or the levels listed after it in the table**, will be logged.</span></span>

| <span data-ttu-id="6c39e-255">String</span><span class="sxs-lookup"><span data-stu-id="6c39e-255">String</span></span>                      | <span data-ttu-id="6c39e-256">LogLevel</span><span class="sxs-lookup"><span data-stu-id="6c39e-256">LogLevel</span></span>               |
| --------------------------- | ---------------------- |
| `trace`                     | `LogLevel.Trace`       |
| `debug`                     | `LogLevel.Debug`       |
| <span data-ttu-id="6c39e-257">`info`**lub**`information`</span><span class="sxs-lookup"><span data-stu-id="6c39e-257">`info` **or** `information`</span></span> | `LogLevel.Information` |
| <span data-ttu-id="6c39e-258">`warn`**lub**`warning`</span><span class="sxs-lookup"><span data-stu-id="6c39e-258">`warn` **or** `warning`</span></span>     | `LogLevel.Warning`     |
| `error`                     | `LogLevel.Error`       |
| `critical`                  | `LogLevel.Critical`    |
| `none`                      | `LogLevel.None`        |

::: moniker-end

> [!NOTE]
> <span data-ttu-id="6c39e-259">Aby całkowicie wyłączyć rejestrowanie, określ `signalR.LogLevel.None` `configureLogging` w metodzie.</span><span class="sxs-lookup"><span data-stu-id="6c39e-259">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="6c39e-260">Aby uzyskać więcej informacji na temat rejestrowania, zobacz [dokumentację diagnostyki sygnału](xref:signalr/diagnostics).</span><span class="sxs-lookup"><span data-stu-id="6c39e-260">For more information on logging, see the [SignalR Diagnostics documentation](xref:signalr/diagnostics).</span></span>

<span data-ttu-id="6c39e-261">Klient Java sygnalizujący używa biblioteki [SLF4J](https://www.slf4j.org/) do rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="6c39e-261">The SignalR Java client uses the [SLF4J](https://www.slf4j.org/) library for logging.</span></span> <span data-ttu-id="6c39e-262">Jest to interfejs API rejestrowania wysokiego poziomu, który umożliwia użytkownikom biblioteki wybranie własnych implementacji rejestrowania przez nałożenie określonej zależności rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="6c39e-262">It's a high-level logging API that allows users of the library to chose their own specific logging implementation by bringing in a specific logging dependency.</span></span> <span data-ttu-id="6c39e-263">Poniższy fragment kodu przedstawia sposób użycia `java.util.logging` z klientem programu sygnalizującego środowisko Java.</span><span class="sxs-lookup"><span data-stu-id="6c39e-263">The following code snippet shows how to use `java.util.logging` with the SignalR Java client.</span></span>

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

<span data-ttu-id="6c39e-264">Jeśli nie skonfigurujesz rejestrowania w zależnościach, SLF4J ładuje domyślny Rejestrator braku operacji z następującym komunikatem ostrzegawczym:</span><span class="sxs-lookup"><span data-stu-id="6c39e-264">If you don't configure logging in your dependencies, SLF4J loads a default no-operation logger with the following warning message:</span></span>

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

<span data-ttu-id="6c39e-265">Można je bezpiecznie zignorować.</span><span class="sxs-lookup"><span data-stu-id="6c39e-265">This can safely be ignored.</span></span>

### <a name="configure-allowed-transports"></a><span data-ttu-id="6c39e-266">Skonfiguruj dozwolone transporty</span><span class="sxs-lookup"><span data-stu-id="6c39e-266">Configure allowed transports</span></span>

<span data-ttu-id="6c39e-267">Transporty używane przez sygnalizującego można skonfigurować w `WithUrl` wywołaniu (`withUrl` w języku JavaScript).</span><span class="sxs-lookup"><span data-stu-id="6c39e-267">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="6c39e-268">Wartości bitowe i `HttpTransportType` można użyć, aby ograniczyć klient do używania tylko określonych transportów.</span><span class="sxs-lookup"><span data-stu-id="6c39e-268">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="6c39e-269">Wszystkie transporty są domyślnie włączone.</span><span class="sxs-lookup"><span data-stu-id="6c39e-269">All transports are enabled by default.</span></span>

<span data-ttu-id="6c39e-270">Na przykład, aby wyłączyć transport zdarzeń wysłanych przez serwer, ale zezwolić na połączenia z usługą WebSockets i długie sondowanie:</span><span class="sxs-lookup"><span data-stu-id="6c39e-270">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="6c39e-271">W kliencie JavaScript transporty są konfigurowane przez ustawienie `transport` pola w obiekcie Options dostarczonego do: `withUrl`</span><span class="sxs-lookup"><span data-stu-id="6c39e-271">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="6c39e-272">W tej wersji elementu WebSockets klienta Java jest jedynym dostępnym transportem.</span><span class="sxs-lookup"><span data-stu-id="6c39e-272">In this version of the Java client websockets is the only available transport.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-3.0"

<span data-ttu-id="6c39e-273">W kliencie Java, transport jest wybierany przy użyciu `withTransport` metody `HttpHubConnectionBuilder`w.</span><span class="sxs-lookup"><span data-stu-id="6c39e-273">In the Java client, the transport is selected with the `withTransport` method on the `HttpHubConnectionBuilder`.</span></span> <span data-ttu-id="6c39e-274">Klient Java domyślnie używa transportu usługi WebSockets.</span><span class="sxs-lookup"><span data-stu-id="6c39e-274">The Java client defaults to using the WebSockets transport.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withTransport(TransportEnum.WEBSOCKETS)
    .build();
```

> [!NOTE]
> <span data-ttu-id="6c39e-275">Klient Java sygnalizujący nie obsługuje jeszcze rezerwy transportowej.</span><span class="sxs-lookup"><span data-stu-id="6c39e-275">The SignalR Java client doesn't support transport fallback yet.</span></span>

::: moniker-end

### <a name="configure-bearer-authentication"></a><span data-ttu-id="6c39e-276">Konfigurowanie uwierzytelniania okaziciela</span><span class="sxs-lookup"><span data-stu-id="6c39e-276">Configure bearer authentication</span></span>

<span data-ttu-id="6c39e-277">Aby zapewnić dane uwierzytelniania wraz z żądaniami sygnałów, użyj `AccessTokenProvider` opcji (`accessTokenFactory` w języku JavaScript), aby określić funkcję, która zwraca żądany token dostępu.</span><span class="sxs-lookup"><span data-stu-id="6c39e-277">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="6c39e-278">W kliencie .NET token dostępu jest przenoszona jako token "uwierzytelnianie okaziciela" protokołu HTTP (przy użyciu `Authorization` nagłówka z `Bearer`typem).</span><span class="sxs-lookup"><span data-stu-id="6c39e-278">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="6c39e-279">W kliencie JavaScript token dostępu jest używany jako token okaziciela, **z wyjątkiem** kilku przypadków, w których interfejsy API przeglądarki ograniczają możliwość zastosowania nagłówków (w konkretnym przypadku zdarzeń wysyłanych przez serwer i żądań WebSockets).</span><span class="sxs-lookup"><span data-stu-id="6c39e-279">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="6c39e-280">W takich przypadkach token dostępu jest podawany jako wartość `access_token`ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="6c39e-280">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="6c39e-281">W kliencie `AccessTokenProvider` .NET opcji można określić za pomocą delegata opcji w `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="6c39e-281">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

<span data-ttu-id="6c39e-282">W kliencie JavaScript token dostępu jest konfigurowany przez ustawienie `accessTokenFactory` pola w obiekcie Options w: `withUrl`</span><span class="sxs-lookup"><span data-stu-id="6c39e-282">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

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

<span data-ttu-id="6c39e-283">W kliencie Java programu sygnalizującego można skonfigurować token okaziciela do użycia na potrzeby uwierzytelniania, dostarczając fabrykę tokenów dostępu do [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span><span class="sxs-lookup"><span data-stu-id="6c39e-283">In the SignalR Java client, you can configure a bearer token to use for authentication by providing an access token factory to the [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span></span> <span data-ttu-id="6c39e-284">Użyj [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) , aby dostarczyć [RxJava](https://github.com/ReactiveX/RxJava) [pojedynczej\<>](https://reactivex.io/documentation/single.html).</span><span class="sxs-lookup"><span data-stu-id="6c39e-284">Use [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) to provide an [RxJava](https://github.com/ReactiveX/RxJava) [Single\<String>](https://reactivex.io/documentation/single.html).</span></span> <span data-ttu-id="6c39e-285">Wywołanie metody [Single. Ustąp](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-)umożliwia zapisanie logiki w celu utworzenia tokenów dostępu dla klienta.</span><span class="sxs-lookup"><span data-stu-id="6c39e-285">With a call to [Single.defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), you can write logic to produce access tokens for your client.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="6c39e-286">Konfigurowanie opcji limitu czasu i utrzymywania aktywności</span><span class="sxs-lookup"><span data-stu-id="6c39e-286">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="6c39e-287">W `HubConnection` samym obiekcie są dostępne dodatkowe opcje konfigurowania trybu przekroczenia limitu czasu i zachowania utrzymywania aktywności:</span><span class="sxs-lookup"><span data-stu-id="6c39e-287">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>


::: moniker range=">= aspnetcore-2.2"

# <a name="nettabdotnet"></a>[<span data-ttu-id="6c39e-288">.NET</span><span class="sxs-lookup"><span data-stu-id="6c39e-288">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="6c39e-289">Opcja</span><span class="sxs-lookup"><span data-stu-id="6c39e-289">Option</span></span> | <span data-ttu-id="6c39e-290">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="6c39e-290">Default value</span></span> | <span data-ttu-id="6c39e-291">Opis</span><span class="sxs-lookup"><span data-stu-id="6c39e-291">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="6c39e-292">30 sekund (30 000 MS)</span><span class="sxs-lookup"><span data-stu-id="6c39e-292">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="6c39e-293">Limit czasu dla działania serwera.</span><span class="sxs-lookup"><span data-stu-id="6c39e-293">Timeout for server activity.</span></span> <span data-ttu-id="6c39e-294">Jeśli serwer nie wysłał komunikatu w tym interwale, Klient uważa, że serwer odłączył się i wyzwala `Closed` zdarzenie (`onclose` w języku JavaScript).</span><span class="sxs-lookup"><span data-stu-id="6c39e-294">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="6c39e-295">Ta wartość musi być wystarczająca do wysłania wiadomości ping z serwera **i** odebrana przez klienta w ramach przedziału czasu.</span><span class="sxs-lookup"><span data-stu-id="6c39e-295">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="6c39e-296">Zalecana wartość to liczba z co najmniej podwójną `KeepAliveInterval` wartością serwera, aby umożliwić czas na nadejście poleceń ping.</span><span class="sxs-lookup"><span data-stu-id="6c39e-296">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="6c39e-297">15 sekund</span><span class="sxs-lookup"><span data-stu-id="6c39e-297">15 seconds</span></span> | <span data-ttu-id="6c39e-298">Limit czasu początkowej uzgadniania z serwerem.</span><span class="sxs-lookup"><span data-stu-id="6c39e-298">Timeout for initial server handshake.</span></span> <span data-ttu-id="6c39e-299">Jeśli serwer nie wyśle odpowiedzi uzgadniania w tym interwale, klient anuluje uzgadnianie i wyzwala `Closed` zdarzenie (`onclose` w języku JavaScript).</span><span class="sxs-lookup"><span data-stu-id="6c39e-299">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="6c39e-300">Jest to ustawienie zaawansowane, które należy zmodyfikować tylko w przypadku wystąpienia błędów limitu czasu uzgadniania z powodu poważnego opóźnienia sieci.</span><span class="sxs-lookup"><span data-stu-id="6c39e-300">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="6c39e-301">Aby uzyskać więcej szczegółów na temat procesu uzgadniania, zobacz [specyfikację protokołu centrum sygnału](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="6c39e-301">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="6c39e-302">15 sekund</span><span class="sxs-lookup"><span data-stu-id="6c39e-302">15 seconds</span></span> | <span data-ttu-id="6c39e-303">Określa interwał wysyłania komunikatów ping przez klienta.</span><span class="sxs-lookup"><span data-stu-id="6c39e-303">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="6c39e-304">Wysłanie wszelkich komunikatów z klienta powoduje zresetowanie czasomierza do początku interwału.</span><span class="sxs-lookup"><span data-stu-id="6c39e-304">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="6c39e-305">Jeśli klient nie wysłał komunikatu w `ClientTimeoutInterval` zestawie na serwerze, serwer traktuje klienta jako odłączony.</span><span class="sxs-lookup"><span data-stu-id="6c39e-305">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

<span data-ttu-id="6c39e-306">W kliencie .NET wartości limitu czasu są określone jako `TimeSpan` wartości.</span><span class="sxs-lookup"><span data-stu-id="6c39e-306">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="6c39e-307">JavaScript</span><span class="sxs-lookup"><span data-stu-id="6c39e-307">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="6c39e-308">Opcja</span><span class="sxs-lookup"><span data-stu-id="6c39e-308">Option</span></span> | <span data-ttu-id="6c39e-309">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="6c39e-309">Default value</span></span> | <span data-ttu-id="6c39e-310">Opis</span><span class="sxs-lookup"><span data-stu-id="6c39e-310">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="6c39e-311">30 sekund (30 000 MS)</span><span class="sxs-lookup"><span data-stu-id="6c39e-311">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="6c39e-312">Limit czasu dla działania serwera.</span><span class="sxs-lookup"><span data-stu-id="6c39e-312">Timeout for server activity.</span></span> <span data-ttu-id="6c39e-313">Jeśli serwer nie wysłał komunikatu w tym interwale, Klient uważa, że serwer odłączył się i wyzwala `onclose` zdarzenie.</span><span class="sxs-lookup"><span data-stu-id="6c39e-313">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="6c39e-314">Ta wartość musi być wystarczająca do wysłania wiadomości ping z serwera **i** odebrana przez klienta w ramach przedziału czasu.</span><span class="sxs-lookup"><span data-stu-id="6c39e-314">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="6c39e-315">Zalecana wartość to liczba z co najmniej podwójną `KeepAliveInterval` wartością serwera, aby umożliwić czas na nadejście poleceń ping.</span><span class="sxs-lookup"><span data-stu-id="6c39e-315">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `keepAliveIntervalInMilliseconds` | <span data-ttu-id="6c39e-316">15 sekund (15 000 milisekund)</span><span class="sxs-lookup"><span data-stu-id="6c39e-316">15 seconds (15,000 miliseconds)</span></span> | <span data-ttu-id="6c39e-317">Określa interwał wysyłania komunikatów ping przez klienta.</span><span class="sxs-lookup"><span data-stu-id="6c39e-317">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="6c39e-318">Wysłanie wszelkich komunikatów z klienta powoduje zresetowanie czasomierza do początku interwału.</span><span class="sxs-lookup"><span data-stu-id="6c39e-318">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="6c39e-319">Jeśli klient nie wysłał komunikatu w `ClientTimeoutInterval` zestawie na serwerze, serwer traktuje klienta jako odłączony.</span><span class="sxs-lookup"><span data-stu-id="6c39e-319">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="6c39e-320">Java</span><span class="sxs-lookup"><span data-stu-id="6c39e-320">Java</span></span>](#tab/java)

| <span data-ttu-id="6c39e-321">Opcja</span><span class="sxs-lookup"><span data-stu-id="6c39e-321">Option</span></span> | <span data-ttu-id="6c39e-322">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="6c39e-322">Default value</span></span> | <span data-ttu-id="6c39e-323">Opis</span><span class="sxs-lookup"><span data-stu-id="6c39e-323">Description</span></span> |
| ------ | ------------- | ----------- |
| <span data-ttu-id="6c39e-324">`getServerTimeout` / `setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="6c39e-324">`getServerTimeout` / `setServerTimeout`</span></span> | <span data-ttu-id="6c39e-325">30 sekund (30 000 MS)</span><span class="sxs-lookup"><span data-stu-id="6c39e-325">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="6c39e-326">Limit czasu dla działania serwera.</span><span class="sxs-lookup"><span data-stu-id="6c39e-326">Timeout for server activity.</span></span> <span data-ttu-id="6c39e-327">Jeśli serwer nie wysłał komunikatu w tym interwale, Klient uważa, że serwer odłączył się i wyzwala `onClose` zdarzenie.</span><span class="sxs-lookup"><span data-stu-id="6c39e-327">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="6c39e-328">Ta wartość musi być wystarczająca do wysłania wiadomości ping z serwera **i** odebrana przez klienta w ramach przedziału czasu.</span><span class="sxs-lookup"><span data-stu-id="6c39e-328">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="6c39e-329">Zalecana wartość to liczba z co najmniej podwójną `KeepAliveInterval` wartością serwera, aby umożliwić czas na nadejście poleceń ping.</span><span class="sxs-lookup"><span data-stu-id="6c39e-329">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="6c39e-330">15 sekund</span><span class="sxs-lookup"><span data-stu-id="6c39e-330">15 seconds</span></span> | <span data-ttu-id="6c39e-331">Limit czasu początkowej uzgadniania z serwerem.</span><span class="sxs-lookup"><span data-stu-id="6c39e-331">Timeout for initial server handshake.</span></span> <span data-ttu-id="6c39e-332">Jeśli serwer nie wyśle odpowiedzi uzgadniania w tym interwale, klient anuluje uzgadnianie i wyzwala `onClose` zdarzenie.</span><span class="sxs-lookup"><span data-stu-id="6c39e-332">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="6c39e-333">Jest to ustawienie zaawansowane, które należy zmodyfikować tylko w przypadku wystąpienia błędów limitu czasu uzgadniania z powodu poważnego opóźnienia sieci.</span><span class="sxs-lookup"><span data-stu-id="6c39e-333">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="6c39e-334">Aby uzyskać więcej szczegółów na temat procesu uzgadniania, zobacz [specyfikację protokołu centrum sygnału](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="6c39e-334">For more detail on the Handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| <span data-ttu-id="6c39e-335">`getKeepAliveInterval` / `setKeepAliveInterval`</span><span class="sxs-lookup"><span data-stu-id="6c39e-335">`getKeepAliveInterval` / `setKeepAliveInterval`</span></span> | <span data-ttu-id="6c39e-336">15 sekund (15 000 MS)</span><span class="sxs-lookup"><span data-stu-id="6c39e-336">15 seconds (15,000 milliseconds)</span></span> | <span data-ttu-id="6c39e-337">Określa interwał wysyłania komunikatów ping przez klienta.</span><span class="sxs-lookup"><span data-stu-id="6c39e-337">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="6c39e-338">Wysłanie wszelkich komunikatów z klienta powoduje zresetowanie czasomierza do początku interwału.</span><span class="sxs-lookup"><span data-stu-id="6c39e-338">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="6c39e-339">Jeśli klient nie wysłał komunikatu w `ClientTimeoutInterval` zestawie na serwerze, serwer traktuje klienta jako odłączony.</span><span class="sxs-lookup"><span data-stu-id="6c39e-339">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="nettabdotnet"></a>[<span data-ttu-id="6c39e-340">.NET</span><span class="sxs-lookup"><span data-stu-id="6c39e-340">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="6c39e-341">Opcja</span><span class="sxs-lookup"><span data-stu-id="6c39e-341">Option</span></span> | <span data-ttu-id="6c39e-342">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="6c39e-342">Default value</span></span> | <span data-ttu-id="6c39e-343">Opis</span><span class="sxs-lookup"><span data-stu-id="6c39e-343">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="6c39e-344">30 sekund (30 000 MS)</span><span class="sxs-lookup"><span data-stu-id="6c39e-344">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="6c39e-345">Limit czasu dla działania serwera.</span><span class="sxs-lookup"><span data-stu-id="6c39e-345">Timeout for server activity.</span></span> <span data-ttu-id="6c39e-346">Jeśli serwer nie wysłał komunikatu w tym interwale, Klient uważa, że serwer odłączył się i wyzwala `Closed` zdarzenie (`onclose` w języku JavaScript).</span><span class="sxs-lookup"><span data-stu-id="6c39e-346">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="6c39e-347">Ta wartość musi być wystarczająca do wysłania wiadomości ping z serwera **i** odebrana przez klienta w ramach przedziału czasu.</span><span class="sxs-lookup"><span data-stu-id="6c39e-347">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="6c39e-348">Zalecana wartość to liczba z co najmniej podwójną `KeepAliveInterval` wartością serwera, aby umożliwić czas na nadejście poleceń ping.</span><span class="sxs-lookup"><span data-stu-id="6c39e-348">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="6c39e-349">15 sekund</span><span class="sxs-lookup"><span data-stu-id="6c39e-349">15 seconds</span></span> | <span data-ttu-id="6c39e-350">Limit czasu początkowej uzgadniania z serwerem.</span><span class="sxs-lookup"><span data-stu-id="6c39e-350">Timeout for initial server handshake.</span></span> <span data-ttu-id="6c39e-351">Jeśli serwer nie wyśle odpowiedzi uzgadniania w tym interwale, klient anuluje uzgadnianie i wyzwala `Closed` zdarzenie (`onclose` w języku JavaScript).</span><span class="sxs-lookup"><span data-stu-id="6c39e-351">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="6c39e-352">Jest to ustawienie zaawansowane, które należy zmodyfikować tylko w przypadku wystąpienia błędów limitu czasu uzgadniania z powodu poważnego opóźnienia sieci.</span><span class="sxs-lookup"><span data-stu-id="6c39e-352">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="6c39e-353">Aby uzyskać więcej szczegółów na temat procesu uzgadniania, zobacz [specyfikację protokołu centrum sygnału](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="6c39e-353">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

<span data-ttu-id="6c39e-354">W kliencie .NET wartości limitu czasu są określone jako `TimeSpan` wartości.</span><span class="sxs-lookup"><span data-stu-id="6c39e-354">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="6c39e-355">JavaScript</span><span class="sxs-lookup"><span data-stu-id="6c39e-355">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="6c39e-356">Opcja</span><span class="sxs-lookup"><span data-stu-id="6c39e-356">Option</span></span> | <span data-ttu-id="6c39e-357">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="6c39e-357">Default value</span></span> | <span data-ttu-id="6c39e-358">Opis</span><span class="sxs-lookup"><span data-stu-id="6c39e-358">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="6c39e-359">30 sekund (30 000 MS)</span><span class="sxs-lookup"><span data-stu-id="6c39e-359">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="6c39e-360">Limit czasu dla działania serwera.</span><span class="sxs-lookup"><span data-stu-id="6c39e-360">Timeout for server activity.</span></span> <span data-ttu-id="6c39e-361">Jeśli serwer nie wysłał komunikatu w tym interwale, Klient uważa, że serwer odłączył się i wyzwala `onclose` zdarzenie.</span><span class="sxs-lookup"><span data-stu-id="6c39e-361">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="6c39e-362">Ta wartość musi być wystarczająca do wysłania wiadomości ping z serwera **i** odebrana przez klienta w ramach przedziału czasu.</span><span class="sxs-lookup"><span data-stu-id="6c39e-362">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="6c39e-363">Zalecana wartość to liczba z co najmniej podwójną `KeepAliveInterval` wartością serwera, aby umożliwić czas na nadejście poleceń ping.</span><span class="sxs-lookup"><span data-stu-id="6c39e-363">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="6c39e-364">Java</span><span class="sxs-lookup"><span data-stu-id="6c39e-364">Java</span></span>](#tab/java)

| <span data-ttu-id="6c39e-365">Opcja</span><span class="sxs-lookup"><span data-stu-id="6c39e-365">Option</span></span> | <span data-ttu-id="6c39e-366">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="6c39e-366">Default value</span></span> | <span data-ttu-id="6c39e-367">Opis</span><span class="sxs-lookup"><span data-stu-id="6c39e-367">Description</span></span> |
| ------ | ------------- | ----------- |
| <span data-ttu-id="6c39e-368">`getServerTimeout` / `setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="6c39e-368">`getServerTimeout` / `setServerTimeout`</span></span> | <span data-ttu-id="6c39e-369">30 sekund (30 000 MS)</span><span class="sxs-lookup"><span data-stu-id="6c39e-369">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="6c39e-370">Limit czasu dla działania serwera.</span><span class="sxs-lookup"><span data-stu-id="6c39e-370">Timeout for server activity.</span></span> <span data-ttu-id="6c39e-371">Jeśli serwer nie wysłał komunikatu w tym interwale, Klient uważa, że serwer odłączył się i wyzwala `onClose` zdarzenie.</span><span class="sxs-lookup"><span data-stu-id="6c39e-371">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="6c39e-372">Ta wartość musi być wystarczająca do wysłania wiadomości ping z serwera **i** odebrana przez klienta w ramach przedziału czasu.</span><span class="sxs-lookup"><span data-stu-id="6c39e-372">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="6c39e-373">Zalecana wartość to liczba z co najmniej podwójną `KeepAliveInterval` wartością serwera, aby umożliwić czas na nadejście poleceń ping.</span><span class="sxs-lookup"><span data-stu-id="6c39e-373">The recommended value is a number at least double the server's `KeepAliveInterval` value, to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="6c39e-374">15 sekund</span><span class="sxs-lookup"><span data-stu-id="6c39e-374">15 seconds</span></span> | <span data-ttu-id="6c39e-375">Limit czasu początkowej uzgadniania z serwerem.</span><span class="sxs-lookup"><span data-stu-id="6c39e-375">Timeout for initial server handshake.</span></span> <span data-ttu-id="6c39e-376">Jeśli serwer nie wyśle odpowiedzi uzgadniania w tym interwale, klient anuluje uzgadnianie i wyzwala `onClose` zdarzenie.</span><span class="sxs-lookup"><span data-stu-id="6c39e-376">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="6c39e-377">Jest to ustawienie zaawansowane, które należy zmodyfikować tylko w przypadku wystąpienia błędów limitu czasu uzgadniania z powodu poważnego opóźnienia sieci.</span><span class="sxs-lookup"><span data-stu-id="6c39e-377">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="6c39e-378">Aby uzyskać więcej szczegółów na temat procesu uzgadniania, zobacz [specyfikację protokołu centrum sygnału](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="6c39e-378">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

::: moniker-end

---

### <a name="configure-additional-options"></a><span data-ttu-id="6c39e-379">Skonfiguruj dodatkowe opcje</span><span class="sxs-lookup"><span data-stu-id="6c39e-379">Configure additional options</span></span>

<span data-ttu-id="6c39e-380">Dodatkowe opcje `WithUrl` można skonfigurować w metodzie (`withUrl` w języku JavaScript) dla `HubConnectionBuilder` `HttpHubConnectionBuilder` lub w różnych interfejsach API konfiguracji w kliencie Java:</span><span class="sxs-lookup"><span data-stu-id="6c39e-380">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder` or on the various configuration APIs on the `HttpHubConnectionBuilder` in the Java client:</span></span>

# <a name="nettabdotnet"></a>[<span data-ttu-id="6c39e-381">.NET</span><span class="sxs-lookup"><span data-stu-id="6c39e-381">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="6c39e-382">Opcja .NET</span><span class="sxs-lookup"><span data-stu-id="6c39e-382">.NET Option</span></span> |  <span data-ttu-id="6c39e-383">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="6c39e-383">Default value</span></span> | <span data-ttu-id="6c39e-384">Opis</span><span class="sxs-lookup"><span data-stu-id="6c39e-384">Description</span></span> |
| ----------- | -------------- | ----------- |
| `AccessTokenProvider` | `null` | <span data-ttu-id="6c39e-385">Funkcja zwracająca ciąg, który jest dostarczany jako token uwierzytelniania okaziciela w żądaniach HTTP.</span><span class="sxs-lookup"><span data-stu-id="6c39e-385">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `false` | <span data-ttu-id="6c39e-386">Ustaw tę `true` wartość na, aby pominąć krok negocjowania.</span><span class="sxs-lookup"><span data-stu-id="6c39e-386">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="6c39e-387">**Obsługiwane tylko wtedy, gdy transport Sockets jest jedynym włączonym**transportem.</span><span class="sxs-lookup"><span data-stu-id="6c39e-387">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="6c39e-388">Nie można włączyć tego ustawienia w przypadku korzystania z usługi Azure Signal Service.</span><span class="sxs-lookup"><span data-stu-id="6c39e-388">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="6c39e-389">Pusty</span><span class="sxs-lookup"><span data-stu-id="6c39e-389">Empty</span></span> | <span data-ttu-id="6c39e-390">Kolekcja certyfikatów TLS do wysłania w celu uwierzytelniania żądań.</span><span class="sxs-lookup"><span data-stu-id="6c39e-390">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="6c39e-391">Pusty</span><span class="sxs-lookup"><span data-stu-id="6c39e-391">Empty</span></span> | <span data-ttu-id="6c39e-392">Kolekcja plików cookie protokołu HTTP do wysłania w każdym żądaniu HTTP.</span><span class="sxs-lookup"><span data-stu-id="6c39e-392">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="6c39e-393">Pusty</span><span class="sxs-lookup"><span data-stu-id="6c39e-393">Empty</span></span> | <span data-ttu-id="6c39e-394">Poświadczenia do wysyłania przy każdym żądaniu HTTP.</span><span class="sxs-lookup"><span data-stu-id="6c39e-394">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="6c39e-395">5 sekund</span><span class="sxs-lookup"><span data-stu-id="6c39e-395">5 seconds</span></span> | <span data-ttu-id="6c39e-396">Tylko obiekty WebSockets.</span><span class="sxs-lookup"><span data-stu-id="6c39e-396">WebSockets only.</span></span> <span data-ttu-id="6c39e-397">Maksymalny czas oczekiwania klienta po zamknięciu serwera w celu potwierdzenia żądania zamknięcia.</span><span class="sxs-lookup"><span data-stu-id="6c39e-397">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="6c39e-398">Jeśli serwer nie potwierdzi zamknięcia w tym czasie, klient zostanie rozłączony.</span><span class="sxs-lookup"><span data-stu-id="6c39e-398">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="6c39e-399">Pusty</span><span class="sxs-lookup"><span data-stu-id="6c39e-399">Empty</span></span> | <span data-ttu-id="6c39e-400">Mapa dodatkowych nagłówków HTTP do wysłania w każdym żądaniu HTTP.</span><span class="sxs-lookup"><span data-stu-id="6c39e-400">A Map of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | `null` | <span data-ttu-id="6c39e-401">Delegat, który może służyć do konfigurowania lub zastępowania `HttpMessageHandler` używanych do wysyłania żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="6c39e-401">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="6c39e-402">Nieużywane na potrzeby połączeń protokołu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="6c39e-402">Not used for WebSocket connections.</span></span> <span data-ttu-id="6c39e-403">Ten delegat musi zwracać wartość różną od null i otrzymuje wartość domyślną jako parametr.</span><span class="sxs-lookup"><span data-stu-id="6c39e-403">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="6c39e-404">Zmodyfikuj ustawienia tej wartości domyślnej i zwróć ją lub Zwróć nowe `HttpMessageHandler` wystąpienie.</span><span class="sxs-lookup"><span data-stu-id="6c39e-404">Either modify settings on that default value and return it, or return a new `HttpMessageHandler` instance.</span></span> <span data-ttu-id="6c39e-405">**Podczas zastępowania programu obsługi należy skopiować ustawienia, które mają być zachowane z podanej procedury obsługi. w przeciwnym razie skonfigurowane opcje (takie jak pliki cookie i nagłówki) nie będą miały zastosowania do nowego programu obsługi.**</span><span class="sxs-lookup"><span data-stu-id="6c39e-405">**When replacing the handler make sure to copy the settings you want to keep from the provided handler, otherwise, the configured options (such as Cookies and Headers) won't apply to the new handler.**</span></span> |
| `Proxy` | `null` | <span data-ttu-id="6c39e-406">Serwer proxy HTTP, który ma być używany podczas wysyłania żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="6c39e-406">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | `false` | <span data-ttu-id="6c39e-407">Ustaw tę wartość logiczną, aby wysyłać domyślne poświadczenia dla żądań HTTP i WebSockets.</span><span class="sxs-lookup"><span data-stu-id="6c39e-407">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="6c39e-408">Umożliwia to korzystanie z uwierzytelniania systemu Windows.</span><span class="sxs-lookup"><span data-stu-id="6c39e-408">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | `null` | <span data-ttu-id="6c39e-409">Delegat, który może służyć do konfigurowania dodatkowych opcji protokołu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="6c39e-409">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="6c39e-410">Odbiera wystąpienie elementu [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) , którego można użyć do skonfigurowania opcji.</span><span class="sxs-lookup"><span data-stu-id="6c39e-410">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="6c39e-411">JavaScript</span><span class="sxs-lookup"><span data-stu-id="6c39e-411">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="6c39e-412">Opcja języka JavaScript</span><span class="sxs-lookup"><span data-stu-id="6c39e-412">JavaScript Option</span></span> | <span data-ttu-id="6c39e-413">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="6c39e-413">Default Value</span></span> | <span data-ttu-id="6c39e-414">Opis</span><span class="sxs-lookup"><span data-stu-id="6c39e-414">Description</span></span> |
| ----------------- | ------------- | ----------- |
| `accessTokenFactory` | `null` | <span data-ttu-id="6c39e-415">Funkcja zwracająca ciąg, który jest dostarczany jako token uwierzytelniania okaziciela w żądaniach HTTP.</span><span class="sxs-lookup"><span data-stu-id="6c39e-415">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `skipNegotiation` | `false` | <span data-ttu-id="6c39e-416">Ustaw tę `true` wartość na, aby pominąć krok negocjowania.</span><span class="sxs-lookup"><span data-stu-id="6c39e-416">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="6c39e-417">**Obsługiwane tylko wtedy, gdy transport Sockets jest jedynym włączonym**transportem.</span><span class="sxs-lookup"><span data-stu-id="6c39e-417">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="6c39e-418">Nie można włączyć tego ustawienia w przypadku korzystania z usługi Azure Signal Service.</span><span class="sxs-lookup"><span data-stu-id="6c39e-418">This setting can't be enabled when using the Azure SignalR Service.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="6c39e-419">Java</span><span class="sxs-lookup"><span data-stu-id="6c39e-419">Java</span></span>](#tab/java)

| <span data-ttu-id="6c39e-420">Opcja Java</span><span class="sxs-lookup"><span data-stu-id="6c39e-420">Java Option</span></span> | <span data-ttu-id="6c39e-421">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="6c39e-421">Default Value</span></span> | <span data-ttu-id="6c39e-422">Opis</span><span class="sxs-lookup"><span data-stu-id="6c39e-422">Description</span></span> |
| ----------- | ------------- | ----------- |
| `withAccessTokenProvider` | `null` | <span data-ttu-id="6c39e-423">Funkcja zwracająca ciąg, który jest dostarczany jako token uwierzytelniania okaziciela w żądaniach HTTP.</span><span class="sxs-lookup"><span data-stu-id="6c39e-423">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `shouldSkipNegotiate` | `false` | <span data-ttu-id="6c39e-424">Ustaw tę `true` wartość na, aby pominąć krok negocjowania.</span><span class="sxs-lookup"><span data-stu-id="6c39e-424">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="6c39e-425">**Obsługiwane tylko wtedy, gdy transport Sockets jest jedynym włączonym**transportem.</span><span class="sxs-lookup"><span data-stu-id="6c39e-425">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="6c39e-426">Nie można włączyć tego ustawienia w przypadku korzystania z usługi Azure Signal Service.</span><span class="sxs-lookup"><span data-stu-id="6c39e-426">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| <span data-ttu-id="6c39e-427">`withHeader``withHeaders`</span><span class="sxs-lookup"><span data-stu-id="6c39e-427">`withHeader` `withHeaders`</span></span> | <span data-ttu-id="6c39e-428">Pusty</span><span class="sxs-lookup"><span data-stu-id="6c39e-428">Empty</span></span> | <span data-ttu-id="6c39e-429">Mapa dodatkowych nagłówków HTTP do wysłania w każdym żądaniu HTTP.</span><span class="sxs-lookup"><span data-stu-id="6c39e-429">A Map of additional HTTP headers to send with every HTTP request.</span></span> |

---

<span data-ttu-id="6c39e-430">W kliencie platformy .NET te opcje można modyfikować za pomocą delegata opcji udostępnianego dla `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="6c39e-430">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="6c39e-431">W kliencie JavaScript te opcje można podać w obiekcie JavaScript udostępnionym `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="6c39e-431">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

<span data-ttu-id="6c39e-432">W kliencie Java Opcje te można skonfigurować przy użyciu metod `HttpHubConnectionBuilder` zwróconych z`HubConnectionBuilder.create("HUB URL")`</span><span class="sxs-lookup"><span data-stu-id="6c39e-432">In the Java client, these options can be configured with the methods on the `HttpHubConnectionBuilder` returned from the `HubConnectionBuilder.create("HUB URL")`</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
        .withHeader("Foo", "Bar")
        .shouldSkipNegotiate(true)
        .withHandshakeResponseTimeout(30*1000)
        .build();
```

## <a name="additional-resources"></a><span data-ttu-id="6c39e-433">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="6c39e-433">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>
