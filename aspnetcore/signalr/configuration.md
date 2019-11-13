---
title: Konfiguracja SignalR ASP.NET Core
author: bradygaster
description: Dowiedz się, jak skonfigurować aplikacje SignalR ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- SignalR
uid: signalr/configuration
ms.openlocfilehash: 682cc36216a4dc9b38c87b4f67100ab565a70e5c
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/12/2019
ms.locfileid: "73963983"
---
# <a name="aspnet-core-opno-locsignalr-configuration"></a><span data-ttu-id="5141a-103">Konfiguracja SignalR ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5141a-103">ASP.NET Core SignalR configuration</span></span>

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="5141a-104">Opcje serializacji JSON/MessagePack</span><span class="sxs-lookup"><span data-stu-id="5141a-104">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="5141a-105">ASP.NET Core SignalR obsługuje dwa protokoły dla komunikatów kodowania: [JSON](https://www.json.org/) i [MessagePack](https://msgpack.org/index.html).</span><span class="sxs-lookup"><span data-stu-id="5141a-105">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="5141a-106">Każdy protokół ma opcje konfiguracji serializacji.</span><span class="sxs-lookup"><span data-stu-id="5141a-106">Each protocol has serialization configuration options.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="5141a-107">Serializacji JSON można skonfigurować na serwerze przy użyciu metody rozszerzenia [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) .</span><span class="sxs-lookup"><span data-stu-id="5141a-107">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method.</span></span> <span data-ttu-id="5141a-108">`AddJsonProtocol` można dodać po elemencie [Addsignaler](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="5141a-108">`AddJsonProtocol` can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="5141a-109">Metoda `AddJsonProtocol` przyjmuje delegata, który odbiera obiekt `options`.</span><span class="sxs-lookup"><span data-stu-id="5141a-109">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="5141a-110">Właściwość [PayloadSerializerOptions](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializeroptions) tego obiektu jest obiektem <xref:System.Text.Json.JsonSerializerOptions> `System.Text.Json`, którego można użyć do skonfigurowania serializacji argumentów i zwracanych wartości.</span><span class="sxs-lookup"><span data-stu-id="5141a-110">The [PayloadSerializerOptions](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializeroptions) property on that object is a `System.Text.Json` <xref:System.Text.Json.JsonSerializerOptions> object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="5141a-111">Aby uzyskać więcej informacji, zobacz [dokumentację system. Text. JSON](/dotnet/api/system.text.json).</span><span class="sxs-lookup"><span data-stu-id="5141a-111">For more information, see the [System.Text.Json documentation](/dotnet/api/system.text.json).</span></span>

<span data-ttu-id="5141a-112">Przykładowo, aby skonfigurować Serializator, aby nie zmieniać wielkości liter nazw właściwości zamiast domyślnych nazw "camelCase", należy użyć następującego kodu w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="5141a-112">As an example, to configure the serializer to not change the casing of property names, instead of the default "camelCase" names, use the following code in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerOptions.PropertyNamingPolicy = null
    });
```

<span data-ttu-id="5141a-113">W kliencie .NET ta sama Metoda rozszerzenia `AddJsonProtocol` istnieje w [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="5141a-113">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="5141a-114">Aby rozwiązać metodę rozszerzenia, należy zaimportować przestrzeń nazw `Microsoft.Extensions.DependencyInjection`:</span><span class="sxs-lookup"><span data-stu-id="5141a-114">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

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
> <span data-ttu-id="5141a-115">W tej chwili nie można skonfigurować serializacji JSON w kliencie JavaScript.</span><span class="sxs-lookup"><span data-stu-id="5141a-115">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="switch-to-newtonsoftjson"></a><span data-ttu-id="5141a-116">Przejdź do pliku Newtonsoft. JSON</span><span class="sxs-lookup"><span data-stu-id="5141a-116">Switch to Newtonsoft.Json</span></span>

<span data-ttu-id="5141a-117">Jeśli potrzebujesz funkcji `Newtonsoft.Json`, które nie są obsługiwane w `System.Text.Json`, zobacz [Switch to Newtonsoft. JSON](xref:migration/22-to-30#switch-to-newtonsoftjson).</span><span class="sxs-lookup"><span data-stu-id="5141a-117">If you need features of `Newtonsoft.Json` that aren't supported in `System.Text.Json`, See [Switch to Newtonsoft.Json](xref:migration/22-to-30#switch-to-newtonsoftjson).</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="5141a-118">Serializacji JSON można skonfigurować na serwerze przy użyciu metody rozszerzenia [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) , która może zostać dodana po metodzie [addsygnalizująca](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="5141a-118">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method, which can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in your `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="5141a-119">Metoda `AddJsonProtocol` przyjmuje delegata, który odbiera obiekt `options`.</span><span class="sxs-lookup"><span data-stu-id="5141a-119">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="5141a-120">Właściwość [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) tego obiektu jest obiektem JSON.NET `JsonSerializerSettings`, którego można użyć do skonfigurowania serializacji argumentów i zwracanych wartości.</span><span class="sxs-lookup"><span data-stu-id="5141a-120">The [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) property on that object is a JSON.NET `JsonSerializerSettings` object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="5141a-121">Aby uzyskać więcej informacji, zapoznaj się z [dokumentacją JSON.NET](https://www.newtonsoft.com/json/help/html/Introduction.htm).</span><span class="sxs-lookup"><span data-stu-id="5141a-121">For more information, see the [JSON.NET documentation](https://www.newtonsoft.com/json/help/html/Introduction.htm).</span></span>
 
<span data-ttu-id="5141a-122">Przykładowo, aby skonfigurować serializator do używania nazw właściwości "PascalCase" zamiast domyślnych nazw "camelCase", należy użyć następującego kodu w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="5141a-122">As an example, to configure the serializer to use "PascalCase" property names, instead of the default "camelCase" names, use the following code in `Startup.ConfigureServices`:</span></span>
 
```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver =
            new DefaultContractResolver();
    });
```

<span data-ttu-id="5141a-123">W kliencie .NET ta sama Metoda rozszerzenia `AddJsonProtocol` istnieje w [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="5141a-123">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="5141a-124">Aby rozwiązać metodę rozszerzenia, należy zaimportować przestrzeń nazw `Microsoft.Extensions.DependencyInjection`:</span><span class="sxs-lookup"><span data-stu-id="5141a-124">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

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
> <span data-ttu-id="5141a-125">W tej chwili nie można skonfigurować serializacji JSON w kliencie JavaScript.</span><span class="sxs-lookup"><span data-stu-id="5141a-125">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

::: moniker-end

### <a name="messagepack-serialization-options"></a><span data-ttu-id="5141a-126">Opcje serializacji MessagePack</span><span class="sxs-lookup"><span data-stu-id="5141a-126">MessagePack serialization options</span></span>

<span data-ttu-id="5141a-127">Serializacji MessagePack można skonfigurować, dostarczając delegata do wywołania [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) .</span><span class="sxs-lookup"><span data-stu-id="5141a-127">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="5141a-128">Aby uzyskać więcej informacji, zobacz [MessagePack w SignalR](xref:signalr/messagepackhubprotocol) .</span><span class="sxs-lookup"><span data-stu-id="5141a-128">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="5141a-129">W tej chwili nie można skonfigurować serializacji MessagePack w kliencie JavaScript.</span><span class="sxs-lookup"><span data-stu-id="5141a-129">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="5141a-130">Konfigurowanie opcji serwera</span><span class="sxs-lookup"><span data-stu-id="5141a-130">Configure server options</span></span>

<span data-ttu-id="5141a-131">W poniższej tabeli opisano opcje konfigurowania centrów SignalR:</span><span class="sxs-lookup"><span data-stu-id="5141a-131">The following table describes options for configuring SignalR hubs:</span></span>

::: moniker range=">= aspnetcore-3.0"

| <span data-ttu-id="5141a-132">Opcja</span><span class="sxs-lookup"><span data-stu-id="5141a-132">Option</span></span> | <span data-ttu-id="5141a-133">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="5141a-133">Default Value</span></span> | <span data-ttu-id="5141a-134">Opis</span><span class="sxs-lookup"><span data-stu-id="5141a-134">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="5141a-135">30 sekund</span><span class="sxs-lookup"><span data-stu-id="5141a-135">30 seconds</span></span> | <span data-ttu-id="5141a-136">Serwer Rozważ odłączenie klienta, jeśli nie odebrano komunikatu (w tym w tym interwale).</span><span class="sxs-lookup"><span data-stu-id="5141a-136">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="5141a-137">Może upłynąć dłużej niż ten interwał limitu czasu, aby klient mógł zostać oznaczony jako odłączony, ze względu na to, jak jest to zaimplementowane.</span><span class="sxs-lookup"><span data-stu-id="5141a-137">It could take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="5141a-138">Zalecaną wartością jest podwójna wartość `KeepAliveInterval`.</span><span class="sxs-lookup"><span data-stu-id="5141a-138">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="5141a-139">15 sekund</span><span class="sxs-lookup"><span data-stu-id="5141a-139">15 seconds</span></span> | <span data-ttu-id="5141a-140">Jeśli klient nie wyśle początkowego komunikatu uzgadniania w tym przedziale czasu, połączenie zostanie zamknięte.</span><span class="sxs-lookup"><span data-stu-id="5141a-140">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="5141a-141">Jest to ustawienie zaawansowane, które należy zmodyfikować tylko w przypadku wystąpienia błędów limitu czasu uzgadniania z powodu poważnego opóźnienia sieci.</span><span class="sxs-lookup"><span data-stu-id="5141a-141">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="5141a-142">Aby uzyskać więcej szczegółów na temat procesu uzgadniania, zobacz [Specyfikacja protokołuSignalR Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="5141a-142">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="5141a-143">15 sekund</span><span class="sxs-lookup"><span data-stu-id="5141a-143">15 seconds</span></span> | <span data-ttu-id="5141a-144">Jeśli serwer nie wysłał komunikatu w tym interwale, zostanie automatycznie wysłana wiadomość ping w celu pozostawienia otwartego połączenia.</span><span class="sxs-lookup"><span data-stu-id="5141a-144">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="5141a-145">Podczas zmiany `KeepAliveInterval`Zmień ustawienie `ServerTimeout`/`serverTimeoutInMilliseconds` na kliencie.</span><span class="sxs-lookup"><span data-stu-id="5141a-145">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="5141a-146">Zalecana wartość `ServerTimeout`/`serverTimeoutInMilliseconds` jest podwójna `KeepAliveInterval` wartość.</span><span class="sxs-lookup"><span data-stu-id="5141a-146">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="5141a-147">Wszystkie zainstalowane protokoły</span><span class="sxs-lookup"><span data-stu-id="5141a-147">All installed protocols</span></span> | <span data-ttu-id="5141a-148">Protokoły obsługiwane przez to centrum.</span><span class="sxs-lookup"><span data-stu-id="5141a-148">Protocols supported by this hub.</span></span> <span data-ttu-id="5141a-149">Domyślnie wszystkie protokoły zarejestrowane na serwerze są dozwolone, ale protokoły można usunąć z tej listy, aby wyłączyć określone protokoły dla poszczególnych centrów.</span><span class="sxs-lookup"><span data-stu-id="5141a-149">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="5141a-150">Jeśli `true`, szczegółowe komunikaty o wyjątkach są zwracane do klientów, gdy wyjątek jest zgłaszany w metodzie centrum.</span><span class="sxs-lookup"><span data-stu-id="5141a-150">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="5141a-151">Wartość domyślna to `false`, ponieważ te komunikaty o wyjątkach mogą zawierać informacje poufne.</span><span class="sxs-lookup"><span data-stu-id="5141a-151">The default is `false`, as these exception messages can contain sensitive information.</span></span> |
| `StreamBufferCapacity` | `10` | <span data-ttu-id="5141a-152">Maksymalna liczba elementów, które mogą być buforowane dla strumieni przekazywania klientów.</span><span class="sxs-lookup"><span data-stu-id="5141a-152">The maximum number of items that can be buffered for client upload streams.</span></span> <span data-ttu-id="5141a-153">Po osiągnięciu tego limitu przetwarzanie wywołań jest blokowane, dopóki serwer nie przetwarza elementów strumienia.</span><span class="sxs-lookup"><span data-stu-id="5141a-153">If this limit is reached, the processing of invocations is blocked until the server processes stream items.</span></span>|
| `MaximumReceiveMessageSize` | <span data-ttu-id="5141a-154">32 KB</span><span class="sxs-lookup"><span data-stu-id="5141a-154">32 KB</span></span> | <span data-ttu-id="5141a-155">Maksymalny rozmiar pojedynczego przychodzącego komunikatu centrum.</span><span class="sxs-lookup"><span data-stu-id="5141a-155">Maximum size of a single incoming hub message.</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-2.2"

| <span data-ttu-id="5141a-156">Opcja</span><span class="sxs-lookup"><span data-stu-id="5141a-156">Option</span></span> | <span data-ttu-id="5141a-157">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="5141a-157">Default Value</span></span> | <span data-ttu-id="5141a-158">Opis</span><span class="sxs-lookup"><span data-stu-id="5141a-158">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="5141a-159">30 sekund</span><span class="sxs-lookup"><span data-stu-id="5141a-159">30 seconds</span></span> | <span data-ttu-id="5141a-160">Serwer Rozważ odłączenie klienta, jeśli nie odebrano komunikatu (w tym w tym interwale).</span><span class="sxs-lookup"><span data-stu-id="5141a-160">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="5141a-161">Może upłynąć dłużej niż ten interwał limitu czasu, aby klient mógł zostać oznaczony jako odłączony, ze względu na to, jak jest to zaimplementowane.</span><span class="sxs-lookup"><span data-stu-id="5141a-161">It could take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="5141a-162">Zalecaną wartością jest podwójna wartość `KeepAliveInterval`.</span><span class="sxs-lookup"><span data-stu-id="5141a-162">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="5141a-163">15 sekund</span><span class="sxs-lookup"><span data-stu-id="5141a-163">15 seconds</span></span> | <span data-ttu-id="5141a-164">Jeśli klient nie wyśle początkowego komunikatu uzgadniania w tym przedziale czasu, połączenie zostanie zamknięte.</span><span class="sxs-lookup"><span data-stu-id="5141a-164">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="5141a-165">Jest to ustawienie zaawansowane, które należy zmodyfikować tylko w przypadku wystąpienia błędów limitu czasu uzgadniania z powodu poważnego opóźnienia sieci.</span><span class="sxs-lookup"><span data-stu-id="5141a-165">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="5141a-166">Aby uzyskać więcej szczegółów na temat procesu uzgadniania, zobacz [Specyfikacja protokołuSignalR Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="5141a-166">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="5141a-167">15 sekund</span><span class="sxs-lookup"><span data-stu-id="5141a-167">15 seconds</span></span> | <span data-ttu-id="5141a-168">Jeśli serwer nie wysłał komunikatu w tym interwale, zostanie automatycznie wysłana wiadomość ping w celu pozostawienia otwartego połączenia.</span><span class="sxs-lookup"><span data-stu-id="5141a-168">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="5141a-169">Podczas zmiany `KeepAliveInterval`Zmień ustawienie `ServerTimeout`/`serverTimeoutInMilliseconds` na kliencie.</span><span class="sxs-lookup"><span data-stu-id="5141a-169">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="5141a-170">Zalecana wartość `ServerTimeout`/`serverTimeoutInMilliseconds` jest podwójna `KeepAliveInterval` wartość.</span><span class="sxs-lookup"><span data-stu-id="5141a-170">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="5141a-171">Wszystkie zainstalowane protokoły</span><span class="sxs-lookup"><span data-stu-id="5141a-171">All installed protocols</span></span> | <span data-ttu-id="5141a-172">Protokoły obsługiwane przez to centrum.</span><span class="sxs-lookup"><span data-stu-id="5141a-172">Protocols supported by this hub.</span></span> <span data-ttu-id="5141a-173">Domyślnie wszystkie protokoły zarejestrowane na serwerze są dozwolone, ale protokoły można usunąć z tej listy, aby wyłączyć określone protokoły dla poszczególnych centrów.</span><span class="sxs-lookup"><span data-stu-id="5141a-173">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="5141a-174">Jeśli `true`, szczegółowe komunikaty o wyjątkach są zwracane do klientów, gdy wyjątek jest zgłaszany w metodzie centrum.</span><span class="sxs-lookup"><span data-stu-id="5141a-174">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="5141a-175">Wartość domyślna to `false`, ponieważ te komunikaty o wyjątkach mogą zawierać informacje poufne.</span><span class="sxs-lookup"><span data-stu-id="5141a-175">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-2.1"

| <span data-ttu-id="5141a-176">Opcja</span><span class="sxs-lookup"><span data-stu-id="5141a-176">Option</span></span> | <span data-ttu-id="5141a-177">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="5141a-177">Default Value</span></span> | <span data-ttu-id="5141a-178">Opis</span><span class="sxs-lookup"><span data-stu-id="5141a-178">Description</span></span> |
| ------ | ------------- | ----------- |
| `HandshakeTimeout` | <span data-ttu-id="5141a-179">15 sekund</span><span class="sxs-lookup"><span data-stu-id="5141a-179">15 seconds</span></span> | <span data-ttu-id="5141a-180">Jeśli klient nie wyśle początkowego komunikatu uzgadniania w tym przedziale czasu, połączenie zostanie zamknięte.</span><span class="sxs-lookup"><span data-stu-id="5141a-180">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="5141a-181">Jest to ustawienie zaawansowane, które należy zmodyfikować tylko w przypadku wystąpienia błędów limitu czasu uzgadniania z powodu poważnego opóźnienia sieci.</span><span class="sxs-lookup"><span data-stu-id="5141a-181">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="5141a-182">Aby uzyskać więcej szczegółów na temat procesu uzgadniania, zobacz [Specyfikacja protokołuSignalR Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="5141a-182">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="5141a-183">15 sekund</span><span class="sxs-lookup"><span data-stu-id="5141a-183">15 seconds</span></span> | <span data-ttu-id="5141a-184">Jeśli serwer nie wysłał komunikatu w tym interwale, zostanie automatycznie wysłana wiadomość ping w celu pozostawienia otwartego połączenia.</span><span class="sxs-lookup"><span data-stu-id="5141a-184">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="5141a-185">Podczas zmiany `KeepAliveInterval`Zmień ustawienie `ServerTimeout`/`serverTimeoutInMilliseconds` na kliencie.</span><span class="sxs-lookup"><span data-stu-id="5141a-185">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="5141a-186">Zalecana wartość `ServerTimeout`/`serverTimeoutInMilliseconds` jest podwójna `KeepAliveInterval` wartość.</span><span class="sxs-lookup"><span data-stu-id="5141a-186">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="5141a-187">Wszystkie zainstalowane protokoły</span><span class="sxs-lookup"><span data-stu-id="5141a-187">All installed protocols</span></span> | <span data-ttu-id="5141a-188">Protokoły obsługiwane przez to centrum.</span><span class="sxs-lookup"><span data-stu-id="5141a-188">Protocols supported by this hub.</span></span> <span data-ttu-id="5141a-189">Domyślnie wszystkie protokoły zarejestrowane na serwerze są dozwolone, ale protokoły można usunąć z tej listy, aby wyłączyć określone protokoły dla poszczególnych centrów.</span><span class="sxs-lookup"><span data-stu-id="5141a-189">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="5141a-190">Jeśli `true`, szczegółowe komunikaty o wyjątkach są zwracane do klientów, gdy wyjątek jest zgłaszany w metodzie centrum.</span><span class="sxs-lookup"><span data-stu-id="5141a-190">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="5141a-191">Wartość domyślna to `false`, ponieważ te komunikaty o wyjątkach mogą zawierać informacje poufne.</span><span class="sxs-lookup"><span data-stu-id="5141a-191">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

::: moniker-end

<span data-ttu-id="5141a-192">Opcje można skonfigurować dla wszystkich centrów, dostarczając opcje delegata do wywołania `AddSignalR` w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="5141a-192">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

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

<span data-ttu-id="5141a-193">Opcje jednego centrum przesłaniają opcje globalne dostępne w `AddSignalR` i można je skonfigurować za pomocą <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span><span class="sxs-lookup"><span data-stu-id="5141a-193">Options for a single hub override the global options provided in `AddSignalR` and can be configured using <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

### <a name="advanced-http-configuration-options"></a><span data-ttu-id="5141a-194">Zaawansowane opcje konfiguracji HTTP</span><span class="sxs-lookup"><span data-stu-id="5141a-194">Advanced HTTP configuration options</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="5141a-195">Użyj `HttpConnectionDispatcherOptions`, aby skonfigurować zaawansowane ustawienia związane z transportami i zarządzaniem buforem pamięci.</span><span class="sxs-lookup"><span data-stu-id="5141a-195">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="5141a-196">Te opcje są konfigurowane przez przekazanie delegata do [MapHub\<t >](/dotnet/api/microsoft.aspnetcore.builder.hubendpointroutebuilderextensions.maphub) w `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="5141a-196">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.builder.hubendpointroutebuilderextensions.maphub) in `Startup.Configure`.</span></span>

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

<span data-ttu-id="5141a-197">Użyj `HttpConnectionDispatcherOptions`, aby skonfigurować zaawansowane ustawienia związane z transportami i zarządzaniem buforem pamięci.</span><span class="sxs-lookup"><span data-stu-id="5141a-197">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="5141a-198">Te opcje są konfigurowane przez przekazanie delegata do [MapHub\<t >](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) w `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="5141a-198">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) in `Startup.Configure`.</span></span>

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

<span data-ttu-id="5141a-199">W poniższej tabeli opisano opcje konfigurowania zaawansowanych opcji protokołu HTTP w programie ASP.NET Core SignalR:</span><span class="sxs-lookup"><span data-stu-id="5141a-199">The following table describes options for configuring ASP.NET Core SignalR's advanced HTTP options:</span></span>

::: moniker range=">= aspnetcore-3.0"

| <span data-ttu-id="5141a-200">Opcja</span><span class="sxs-lookup"><span data-stu-id="5141a-200">Option</span></span> | <span data-ttu-id="5141a-201">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="5141a-201">Default Value</span></span> | <span data-ttu-id="5141a-202">Opis</span><span class="sxs-lookup"><span data-stu-id="5141a-202">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="5141a-203">32 KB</span><span class="sxs-lookup"><span data-stu-id="5141a-203">32 KB</span></span> | <span data-ttu-id="5141a-204">Maksymalna liczba bajtów odebranych od klienta, które buforują serwer przed zastosowaniem nadużycia.</span><span class="sxs-lookup"><span data-stu-id="5141a-204">The maximum number of bytes received from the client that the server buffers before applying backpressure.</span></span> <span data-ttu-id="5141a-205">Zwiększenie tej wartości umożliwia serwerowi otrzymywanie większych komunikatów szybciej bez naciskania, ale może zwiększyć zużycie pamięci.</span><span class="sxs-lookup"><span data-stu-id="5141a-205">Increasing this value allows the server to receive larger messages more quickly without applying backpressure, but can increase memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="5141a-206">Dane zbierane automatycznie z `Authorize` atrybuty zastosowane do klasy centrum.</span><span class="sxs-lookup"><span data-stu-id="5141a-206">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="5141a-207">Lista obiektów [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) służąca do określenia, czy klient jest autoryzowany do łączenia się z centrum.</span><span class="sxs-lookup"><span data-stu-id="5141a-207">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="5141a-208">32 KB</span><span class="sxs-lookup"><span data-stu-id="5141a-208">32 KB</span></span> | <span data-ttu-id="5141a-209">Maksymalna liczba bajtów wysłanych przez aplikację, które buforuje serwer przed zaobserwowaniem presji.</span><span class="sxs-lookup"><span data-stu-id="5141a-209">The maximum number of bytes sent by the app that the server buffers before observing backpressure.</span></span> <span data-ttu-id="5141a-210">Zwiększenie tej wartości umożliwia serwerowi przebuforowanie większych komunikatów szybciej bez czekania na obciążenie, ale może zwiększyć zużycie pamięci.</span><span class="sxs-lookup"><span data-stu-id="5141a-210">Increasing this value allows the server to buffer larger messages more quickly without awaiting backpressure, but can increase memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="5141a-211">Wszystkie transporty są włączone.</span><span class="sxs-lookup"><span data-stu-id="5141a-211">All Transports are enabled.</span></span> | <span data-ttu-id="5141a-212">Wyliczenie flag bitowych wartości `HttpTransportType`, które mogą ograniczyć transporty, z których może korzystać klient do nawiązywania połączeń.</span><span class="sxs-lookup"><span data-stu-id="5141a-212">A bit flags enum of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="5141a-213">Zobacz poniżej.</span><span class="sxs-lookup"><span data-stu-id="5141a-213">See below.</span></span> | <span data-ttu-id="5141a-214">Dodatkowe opcje specyficzne dla długiego transportu sondowania.</span><span class="sxs-lookup"><span data-stu-id="5141a-214">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="5141a-215">Zobacz poniżej.</span><span class="sxs-lookup"><span data-stu-id="5141a-215">See below.</span></span> | <span data-ttu-id="5141a-216">Dodatkowe opcje specyficzne dla transportu obiektów WebSockets.</span><span class="sxs-lookup"><span data-stu-id="5141a-216">Additional options specific to the WebSockets transport.</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-3.0"

| <span data-ttu-id="5141a-217">Opcja</span><span class="sxs-lookup"><span data-stu-id="5141a-217">Option</span></span> | <span data-ttu-id="5141a-218">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="5141a-218">Default Value</span></span> | <span data-ttu-id="5141a-219">Opis</span><span class="sxs-lookup"><span data-stu-id="5141a-219">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="5141a-220">32 KB</span><span class="sxs-lookup"><span data-stu-id="5141a-220">32 KB</span></span> | <span data-ttu-id="5141a-221">Maksymalna liczba bajtów odebranych od klienta, które buforuje serwer.</span><span class="sxs-lookup"><span data-stu-id="5141a-221">The maximum number of bytes received from the client that the server buffers.</span></span> <span data-ttu-id="5141a-222">Zwiększenie tej wartości umożliwia serwerowi otrzymywanie większych komunikatów, ale może mieć negatywny wpływ na użycie pamięci.</span><span class="sxs-lookup"><span data-stu-id="5141a-222">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="5141a-223">Dane zbierane automatycznie z `Authorize` atrybuty zastosowane do klasy centrum.</span><span class="sxs-lookup"><span data-stu-id="5141a-223">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="5141a-224">Lista obiektów [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) służąca do określenia, czy klient jest autoryzowany do łączenia się z centrum.</span><span class="sxs-lookup"><span data-stu-id="5141a-224">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="5141a-225">32 KB</span><span class="sxs-lookup"><span data-stu-id="5141a-225">32 KB</span></span> | <span data-ttu-id="5141a-226">Maksymalna liczba bajtów wysłanych przez aplikację, które buforują serwer.</span><span class="sxs-lookup"><span data-stu-id="5141a-226">The maximum number of bytes sent by the app that the server buffers.</span></span> <span data-ttu-id="5141a-227">Zwiększenie tej wartości umożliwia serwerowi wysyłanie większych komunikatów, ale może mieć negatywny wpływ na użycie pamięci.</span><span class="sxs-lookup"><span data-stu-id="5141a-227">Increasing this value allows the server to send larger messages, but can negatively impact memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="5141a-228">Wszystkie transporty są włączone.</span><span class="sxs-lookup"><span data-stu-id="5141a-228">All Transports are enabled.</span></span> | <span data-ttu-id="5141a-229">Wyliczenie flag bitowych wartości `HttpTransportType`, które mogą ograniczyć transporty, z których może korzystać klient do nawiązywania połączeń.</span><span class="sxs-lookup"><span data-stu-id="5141a-229">A bit flags enum of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="5141a-230">Zobacz poniżej.</span><span class="sxs-lookup"><span data-stu-id="5141a-230">See below.</span></span> | <span data-ttu-id="5141a-231">Dodatkowe opcje specyficzne dla długiego transportu sondowania.</span><span class="sxs-lookup"><span data-stu-id="5141a-231">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="5141a-232">Zobacz poniżej.</span><span class="sxs-lookup"><span data-stu-id="5141a-232">See below.</span></span> | <span data-ttu-id="5141a-233">Dodatkowe opcje specyficzne dla transportu obiektów WebSockets.</span><span class="sxs-lookup"><span data-stu-id="5141a-233">Additional options specific to the WebSockets transport.</span></span> |

::: moniker-end

<span data-ttu-id="5141a-234">W przypadku długiego transportu sondowania dostępne są dodatkowe opcje, które można skonfigurować przy użyciu właściwości `LongPolling`:</span><span class="sxs-lookup"><span data-stu-id="5141a-234">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="5141a-235">Opcja</span><span class="sxs-lookup"><span data-stu-id="5141a-235">Option</span></span> | <span data-ttu-id="5141a-236">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="5141a-236">Default Value</span></span> | <span data-ttu-id="5141a-237">Opis</span><span class="sxs-lookup"><span data-stu-id="5141a-237">Description</span></span> |
| ------ | ------------- | ----------- |
| `PollTimeout` | <span data-ttu-id="5141a-238">90 sekund</span><span class="sxs-lookup"><span data-stu-id="5141a-238">90 seconds</span></span> | <span data-ttu-id="5141a-239">Maksymalny czas, przez jaki serwer czeka na wysłanie wiadomości do klienta przed zakończeniem jednego żądania sondowania.</span><span class="sxs-lookup"><span data-stu-id="5141a-239">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="5141a-240">Zmniejszenie tej wartości powoduje, że klient wystawia nowe żądania sondy częściej.</span><span class="sxs-lookup"><span data-stu-id="5141a-240">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> |

<span data-ttu-id="5141a-241">Transport WebSocket ma dodatkowe opcje, które można skonfigurować za pomocą właściwości `WebSockets`:</span><span class="sxs-lookup"><span data-stu-id="5141a-241">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="5141a-242">Opcja</span><span class="sxs-lookup"><span data-stu-id="5141a-242">Option</span></span> | <span data-ttu-id="5141a-243">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="5141a-243">Default Value</span></span> | <span data-ttu-id="5141a-244">Opis</span><span class="sxs-lookup"><span data-stu-id="5141a-244">Description</span></span> |
| ------ | ------------- | ----------- |
| `CloseTimeout` | <span data-ttu-id="5141a-245">5 sekund</span><span class="sxs-lookup"><span data-stu-id="5141a-245">5 seconds</span></span> | <span data-ttu-id="5141a-246">Gdy serwer zostanie zamknięty, jeśli klient nie zostanie zamknięty w tym okresie, połączenie zostanie przerwane.</span><span class="sxs-lookup"><span data-stu-id="5141a-246">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | `null` | <span data-ttu-id="5141a-247">Delegat, którego można użyć do ustawienia nagłówka `Sec-WebSocket-Protocol` na wartość niestandardową.</span><span class="sxs-lookup"><span data-stu-id="5141a-247">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="5141a-248">Delegat otrzymuje wartości żądane przez klienta jako dane wejściowe i oczekuje na zwrócenie żądanej wartości.</span><span class="sxs-lookup"><span data-stu-id="5141a-248">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="5141a-249">Konfigurowanie opcji klienta</span><span class="sxs-lookup"><span data-stu-id="5141a-249">Configure client options</span></span>

<span data-ttu-id="5141a-250">Opcje klienta można skonfigurować dla typu `HubConnectionBuilder` (dostępnego w klientach .NET i JavaScript).</span><span class="sxs-lookup"><span data-stu-id="5141a-250">Client options can be configured on the `HubConnectionBuilder` type (available in the .NET and JavaScript clients).</span></span> <span data-ttu-id="5141a-251">Jest ona również dostępna w kliencie Java, ale podklasa `HttpHubConnectionBuilder` zawiera opcje konfiguracji konstruktora, a także `HubConnection` samego siebie.</span><span class="sxs-lookup"><span data-stu-id="5141a-251">It's also available in the Java client, but the `HttpHubConnectionBuilder` subclass is what contains the builder configuration options, as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="5141a-252">Konfigurowanie rejestrowania</span><span class="sxs-lookup"><span data-stu-id="5141a-252">Configure logging</span></span>

<span data-ttu-id="5141a-253">Rejestrowanie jest konfigurowane w kliencie .NET przy użyciu metody `ConfigureLogging`.</span><span class="sxs-lookup"><span data-stu-id="5141a-253">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="5141a-254">Dostawcy i filtry rejestrowania mogą być rejestrowane w taki sam sposób, w jaki znajdują się na serwerze.</span><span class="sxs-lookup"><span data-stu-id="5141a-254">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="5141a-255">Aby uzyskać więcej informacji, zobacz dokumentację dotyczącą [rejestrowania w ASP.NET Core](xref:fundamentals/logging/index) .</span><span class="sxs-lookup"><span data-stu-id="5141a-255">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="5141a-256">Aby zarejestrować dostawców rejestrowania, należy zainstalować wymagane pakiety.</span><span class="sxs-lookup"><span data-stu-id="5141a-256">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="5141a-257">Aby zapoznać się z pełną listą, zobacz sekcję [wbudowane dostawcy rejestrowania](xref:fundamentals/logging/index#built-in-logging-providers) w witrynie docs.</span><span class="sxs-lookup"><span data-stu-id="5141a-257">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="5141a-258">Aby na przykład włączyć rejestrowanie konsoli, należy zainstalować pakiet NuGet `Microsoft.Extensions.Logging.Console`.</span><span class="sxs-lookup"><span data-stu-id="5141a-258">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="5141a-259">Wywołaj metodę rozszerzenia `AddConsole`:</span><span class="sxs-lookup"><span data-stu-id="5141a-259">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="5141a-260">W kliencie JavaScript istnieje podobna Metoda `configureLogging`.</span><span class="sxs-lookup"><span data-stu-id="5141a-260">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="5141a-261">Podaj wartość `LogLevel` wskazującą minimalny poziom komunikatów dziennika do wygenerowania.</span><span class="sxs-lookup"><span data-stu-id="5141a-261">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="5141a-262">Dzienniki są zapisywane w oknie konsoli przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="5141a-262">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="5141a-263">Zamiast wartości `LogLevel` można również podać wartość `string` reprezentującą nazwę poziomu dziennika.</span><span class="sxs-lookup"><span data-stu-id="5141a-263">Instead of a `LogLevel` value, you can also provide a `string` value representing a log level name.</span></span> <span data-ttu-id="5141a-264">Jest to przydatne podczas konfigurowania rejestrowania SignalR w środowiskach, w których nie masz dostępu do stałych `LogLevel`.</span><span class="sxs-lookup"><span data-stu-id="5141a-264">This is useful when configuring SignalR logging in environments where you don't have access to the `LogLevel` constants.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging("warn")
    .build();
```

<span data-ttu-id="5141a-265">Poniższa tabela zawiera listę dostępnych poziomów dzienników.</span><span class="sxs-lookup"><span data-stu-id="5141a-265">The following table lists the available log levels.</span></span> <span data-ttu-id="5141a-266">Wybrana wartość `configureLogging` ustawia **minimalny** poziom dziennika, który zostanie zarejestrowany.</span><span class="sxs-lookup"><span data-stu-id="5141a-266">The value you provide to `configureLogging` sets the **minimum** log level that will be logged.</span></span> <span data-ttu-id="5141a-267">Komunikaty zarejestrowane na tym poziomie **lub poziomy wymienione po nim w tabeli**zostaną zarejestrowane.</span><span class="sxs-lookup"><span data-stu-id="5141a-267">Messages logged at this level, **or the levels listed after it in the table**, will be logged.</span></span>

| <span data-ttu-id="5141a-268">String</span><span class="sxs-lookup"><span data-stu-id="5141a-268">String</span></span>                      | <span data-ttu-id="5141a-269">LogLevel</span><span class="sxs-lookup"><span data-stu-id="5141a-269">LogLevel</span></span>               |
| --------------------------- | ---------------------- |
| `trace`                     | `LogLevel.Trace`       |
| `debug`                     | `LogLevel.Debug`       |
| <span data-ttu-id="5141a-270">`info` **lub** `information`</span><span class="sxs-lookup"><span data-stu-id="5141a-270">`info` **or** `information`</span></span> | `LogLevel.Information` |
| <span data-ttu-id="5141a-271">`warn` **lub** `warning`</span><span class="sxs-lookup"><span data-stu-id="5141a-271">`warn` **or** `warning`</span></span>     | `LogLevel.Warning`     |
| `error`                     | `LogLevel.Error`       |
| `critical`                  | `LogLevel.Critical`    |
| `none`                      | `LogLevel.None`        |

::: moniker-end

> [!NOTE]
> <span data-ttu-id="5141a-272">Aby całkowicie wyłączyć rejestrowanie, określ `signalR.LogLevel.None` w `configureLogging` metodzie.</span><span class="sxs-lookup"><span data-stu-id="5141a-272">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="5141a-273">Aby uzyskać więcej informacji na temat rejestrowania, zobacz [dokumentację diagnostykiSignalR](xref:signalr/diagnostics).</span><span class="sxs-lookup"><span data-stu-id="5141a-273">For more information on logging, see the [SignalR Diagnostics documentation](xref:signalr/diagnostics).</span></span>

<span data-ttu-id="5141a-274">Klient języka Java SignalR używa biblioteki [SLF4J](https://www.slf4j.org/) do rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="5141a-274">The SignalR Java client uses the [SLF4J](https://www.slf4j.org/) library for logging.</span></span> <span data-ttu-id="5141a-275">Jest to interfejs API rejestrowania wysokiego poziomu, który umożliwia użytkownikom biblioteki wybranie własnych implementacji rejestrowania przez nałożenie określonej zależności rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="5141a-275">It's a high-level logging API that allows users of the library to chose their own specific logging implementation by bringing in a specific logging dependency.</span></span> <span data-ttu-id="5141a-276">Poniższy fragment kodu przedstawia sposób korzystania z `java.util.logging` z klientem SignalR Java.</span><span class="sxs-lookup"><span data-stu-id="5141a-276">The following code snippet shows how to use `java.util.logging` with the SignalR Java client.</span></span>

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

<span data-ttu-id="5141a-277">Jeśli nie skonfigurujesz rejestrowania w zależnościach, SLF4J ładuje domyślny Rejestrator braku operacji z następującym komunikatem ostrzegawczym:</span><span class="sxs-lookup"><span data-stu-id="5141a-277">If you don't configure logging in your dependencies, SLF4J loads a default no-operation logger with the following warning message:</span></span>

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

<span data-ttu-id="5141a-278">Można je bezpiecznie zignorować.</span><span class="sxs-lookup"><span data-stu-id="5141a-278">This can safely be ignored.</span></span>

### <a name="configure-allowed-transports"></a><span data-ttu-id="5141a-279">Skonfiguruj dozwolone transporty</span><span class="sxs-lookup"><span data-stu-id="5141a-279">Configure allowed transports</span></span>

<span data-ttu-id="5141a-280">Transporty używane przez SignalR można skonfigurować w wywołaniu `WithUrl` (`withUrl` w języku JavaScript).</span><span class="sxs-lookup"><span data-stu-id="5141a-280">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="5141a-281">Wartości `HttpTransportType` mogą być używane w celu ograniczenia klienta do używania tylko określonych transportów.</span><span class="sxs-lookup"><span data-stu-id="5141a-281">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="5141a-282">Wszystkie transporty są domyślnie włączone.</span><span class="sxs-lookup"><span data-stu-id="5141a-282">All transports are enabled by default.</span></span>

<span data-ttu-id="5141a-283">Na przykład, aby wyłączyć transport zdarzeń wysłanych przez serwer, ale zezwolić na połączenia z usługą WebSockets i długie sondowanie:</span><span class="sxs-lookup"><span data-stu-id="5141a-283">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="5141a-284">W kliencie JavaScript transporty są konfigurowane przez ustawienie pola `transport` w obiekcie Options udostępnianym `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="5141a-284">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="5141a-285">W tej wersji elementu WebSockets klienta Java jest jedynym dostępnym transportem.</span><span class="sxs-lookup"><span data-stu-id="5141a-285">In this version of the Java client websockets is the only available transport.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-3.0"

<span data-ttu-id="5141a-286">W kliencie Java, transport jest wybierany z metodą `withTransport`ową `HttpHubConnectionBuilder`.</span><span class="sxs-lookup"><span data-stu-id="5141a-286">In the Java client, the transport is selected with the `withTransport` method on the `HttpHubConnectionBuilder`.</span></span> <span data-ttu-id="5141a-287">Klient Java domyślnie używa transportu usługi WebSockets.</span><span class="sxs-lookup"><span data-stu-id="5141a-287">The Java client defaults to using the WebSockets transport.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withTransport(TransportEnum.WEBSOCKETS)
    .build();
```

> [!NOTE]
> <span data-ttu-id="5141a-288">Klient języka Java SignalR nie obsługuje jeszcze rezerwy transportowej.</span><span class="sxs-lookup"><span data-stu-id="5141a-288">The SignalR Java client doesn't support transport fallback yet.</span></span>

::: moniker-end

### <a name="configure-bearer-authentication"></a><span data-ttu-id="5141a-289">Konfigurowanie uwierzytelniania okaziciela</span><span class="sxs-lookup"><span data-stu-id="5141a-289">Configure bearer authentication</span></span>

<span data-ttu-id="5141a-290">Aby zapewnić dane uwierzytelniania wraz z SignalR żądaniami, użyj opcji `AccessTokenProvider` (`accessTokenFactory` w języku JavaScript), aby określić funkcję, która zwraca żądany token dostępu.</span><span class="sxs-lookup"><span data-stu-id="5141a-290">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="5141a-291">W kliencie .NET token dostępu jest przenoszona jako token "uwierzytelnianie okaziciela" protokołu HTTP (przy użyciu nagłówka `Authorization` z typem `Bearer`).</span><span class="sxs-lookup"><span data-stu-id="5141a-291">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="5141a-292">W kliencie JavaScript token dostępu jest używany jako token okaziciela, **z wyjątkiem** kilku przypadków, w których interfejsy API przeglądarki ograniczają możliwość zastosowania nagłówków (w konkretnym przypadku zdarzeń wysyłanych przez serwer i żądań WebSockets).</span><span class="sxs-lookup"><span data-stu-id="5141a-292">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="5141a-293">W takich przypadkach token dostępu jest dostarczany jako wartość ciągu zapytania `access_token`.</span><span class="sxs-lookup"><span data-stu-id="5141a-293">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="5141a-294">W kliencie .NET opcja `AccessTokenProvider` można określić za pomocą delegata opcji w `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="5141a-294">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

<span data-ttu-id="5141a-295">W kliencie JavaScript token dostępu jest konfigurowany przez ustawienie pola `accessTokenFactory` w obiekcie Options w `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="5141a-295">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

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

<span data-ttu-id="5141a-296">W kliencie SignalR Java można skonfigurować token okaziciela do użycia na potrzeby uwierzytelniania, dostarczając fabrykę tokenów dostępu do [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span><span class="sxs-lookup"><span data-stu-id="5141a-296">In the SignalR Java client, you can configure a bearer token to use for authentication by providing an access token factory to the [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span></span> <span data-ttu-id="5141a-297">Użyj [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) , aby podać [](https://github.com/ReactiveX/RxJava) [\<pojedynczego ciągu](https://reactivex.io/documentation/single.html).</span><span class="sxs-lookup"><span data-stu-id="5141a-297">Use [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) to provide an [RxJava](https://github.com/ReactiveX/RxJava) [Single\<String>](https://reactivex.io/documentation/single.html).</span></span> <span data-ttu-id="5141a-298">Wywołanie metody [Single. Ustąp](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-)umożliwia zapisanie logiki w celu utworzenia tokenów dostępu dla klienta.</span><span class="sxs-lookup"><span data-stu-id="5141a-298">With a call to [Single.defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), you can write logic to produce access tokens for your client.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="5141a-299">Konfigurowanie opcji limitu czasu i utrzymywania aktywności</span><span class="sxs-lookup"><span data-stu-id="5141a-299">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="5141a-300">W celu skonfigurowania zachowania przekroczenia limitu czasu i utrzymywania aktywności są dostępne dodatkowe opcje dla samego obiektu `HubConnection`:</span><span class="sxs-lookup"><span data-stu-id="5141a-300">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>


::: moniker range=">= aspnetcore-2.2"

# <a name="nettabdotnet"></a>[<span data-ttu-id="5141a-301">.NET</span><span class="sxs-lookup"><span data-stu-id="5141a-301">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="5141a-302">Opcja</span><span class="sxs-lookup"><span data-stu-id="5141a-302">Option</span></span> | <span data-ttu-id="5141a-303">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="5141a-303">Default value</span></span> | <span data-ttu-id="5141a-304">Opis</span><span class="sxs-lookup"><span data-stu-id="5141a-304">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="5141a-305">30 sekund (30 000 MS)</span><span class="sxs-lookup"><span data-stu-id="5141a-305">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="5141a-306">Limit czasu dla działania serwera.</span><span class="sxs-lookup"><span data-stu-id="5141a-306">Timeout for server activity.</span></span> <span data-ttu-id="5141a-307">Jeśli serwer nie wysłał komunikatu w tym interwale, Klient uważa, że serwer odłączył się i wyzwala zdarzenie `Closed` (`onclose` w języku JavaScript).</span><span class="sxs-lookup"><span data-stu-id="5141a-307">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="5141a-308">Ta wartość musi być wystarczająca do wysłania wiadomości ping z serwera **i** odebrana przez klienta w ramach przedziału czasu.</span><span class="sxs-lookup"><span data-stu-id="5141a-308">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="5141a-309">Zalecana wartość to liczba z co najmniej podwójną wartością `KeepAliveInterval` serwera, aby umożliwić czas na nadejście poleceń ping.</span><span class="sxs-lookup"><span data-stu-id="5141a-309">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="5141a-310">15 sekund</span><span class="sxs-lookup"><span data-stu-id="5141a-310">15 seconds</span></span> | <span data-ttu-id="5141a-311">Limit czasu początkowej uzgadniania z serwerem.</span><span class="sxs-lookup"><span data-stu-id="5141a-311">Timeout for initial server handshake.</span></span> <span data-ttu-id="5141a-312">Jeśli serwer nie wyśle odpowiedzi uzgadniania w tym interwale, klient anuluje uzgadnianie i wyzwala zdarzenie `Closed` (`onclose` w języku JavaScript).</span><span class="sxs-lookup"><span data-stu-id="5141a-312">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="5141a-313">Jest to ustawienie zaawansowane, które należy zmodyfikować tylko w przypadku wystąpienia błędów limitu czasu uzgadniania z powodu poważnego opóźnienia sieci.</span><span class="sxs-lookup"><span data-stu-id="5141a-313">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="5141a-314">Aby uzyskać więcej szczegółów na temat procesu uzgadniania, zobacz [Specyfikacja protokołuSignalR Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="5141a-314">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="5141a-315">15 sekund</span><span class="sxs-lookup"><span data-stu-id="5141a-315">15 seconds</span></span> | <span data-ttu-id="5141a-316">Określa interwał wysyłania komunikatów ping przez klienta.</span><span class="sxs-lookup"><span data-stu-id="5141a-316">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="5141a-317">Wysłanie wszelkich komunikatów z klienta powoduje zresetowanie czasomierza do początku interwału.</span><span class="sxs-lookup"><span data-stu-id="5141a-317">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="5141a-318">Jeśli klient nie wyśle komunikatu w `ClientTimeoutInterval` zestawie na serwerze, serwer traktuje klienta jako odłączony.</span><span class="sxs-lookup"><span data-stu-id="5141a-318">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

<span data-ttu-id="5141a-319">W kliencie .NET wartości limitu czasu są określane jako wartości `TimeSpan`.</span><span class="sxs-lookup"><span data-stu-id="5141a-319">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="5141a-320">JavaScript</span><span class="sxs-lookup"><span data-stu-id="5141a-320">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="5141a-321">Opcja</span><span class="sxs-lookup"><span data-stu-id="5141a-321">Option</span></span> | <span data-ttu-id="5141a-322">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="5141a-322">Default value</span></span> | <span data-ttu-id="5141a-323">Opis</span><span class="sxs-lookup"><span data-stu-id="5141a-323">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="5141a-324">30 sekund (30 000 MS)</span><span class="sxs-lookup"><span data-stu-id="5141a-324">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="5141a-325">Limit czasu dla działania serwera.</span><span class="sxs-lookup"><span data-stu-id="5141a-325">Timeout for server activity.</span></span> <span data-ttu-id="5141a-326">Jeśli serwer nie wysłał komunikatu w tym interwale, Klient uważa, że serwer odłączył się i wyzwala zdarzenie `onclose`.</span><span class="sxs-lookup"><span data-stu-id="5141a-326">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="5141a-327">Ta wartość musi być wystarczająca do wysłania wiadomości ping z serwera **i** odebrana przez klienta w ramach przedziału czasu.</span><span class="sxs-lookup"><span data-stu-id="5141a-327">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="5141a-328">Zalecana wartość to liczba z co najmniej podwójną wartością `KeepAliveInterval` serwera, aby umożliwić czas na nadejście poleceń ping.</span><span class="sxs-lookup"><span data-stu-id="5141a-328">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `keepAliveIntervalInMilliseconds` | <span data-ttu-id="5141a-329">15 sekund (15 000 milisekund)</span><span class="sxs-lookup"><span data-stu-id="5141a-329">15 seconds (15,000 miliseconds)</span></span> | <span data-ttu-id="5141a-330">Określa interwał wysyłania komunikatów ping przez klienta.</span><span class="sxs-lookup"><span data-stu-id="5141a-330">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="5141a-331">Wysłanie wszelkich komunikatów z klienta powoduje zresetowanie czasomierza do początku interwału.</span><span class="sxs-lookup"><span data-stu-id="5141a-331">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="5141a-332">Jeśli klient nie wyśle komunikatu w `ClientTimeoutInterval` zestawie na serwerze, serwer traktuje klienta jako odłączony.</span><span class="sxs-lookup"><span data-stu-id="5141a-332">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="5141a-333">Java</span><span class="sxs-lookup"><span data-stu-id="5141a-333">Java</span></span>](#tab/java)

| <span data-ttu-id="5141a-334">Opcja</span><span class="sxs-lookup"><span data-stu-id="5141a-334">Option</span></span> | <span data-ttu-id="5141a-335">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="5141a-335">Default value</span></span> | <span data-ttu-id="5141a-336">Opis</span><span class="sxs-lookup"><span data-stu-id="5141a-336">Description</span></span> |
| ------ | ------------- | ----------- |
| <span data-ttu-id="5141a-337">`getServerTimeout` / `setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="5141a-337">`getServerTimeout` / `setServerTimeout`</span></span> | <span data-ttu-id="5141a-338">30 sekund (30 000 MS)</span><span class="sxs-lookup"><span data-stu-id="5141a-338">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="5141a-339">Limit czasu dla działania serwera.</span><span class="sxs-lookup"><span data-stu-id="5141a-339">Timeout for server activity.</span></span> <span data-ttu-id="5141a-340">Jeśli serwer nie wysłał komunikatu w tym interwale, Klient uważa, że serwer odłączył się i wyzwala zdarzenie `onClose`.</span><span class="sxs-lookup"><span data-stu-id="5141a-340">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="5141a-341">Ta wartość musi być wystarczająca do wysłania wiadomości ping z serwera **i** odebrana przez klienta w ramach przedziału czasu.</span><span class="sxs-lookup"><span data-stu-id="5141a-341">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="5141a-342">Zalecana wartość to liczba z co najmniej podwójną wartością `KeepAliveInterval` serwera, aby umożliwić czas na nadejście poleceń ping.</span><span class="sxs-lookup"><span data-stu-id="5141a-342">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="5141a-343">15 sekund</span><span class="sxs-lookup"><span data-stu-id="5141a-343">15 seconds</span></span> | <span data-ttu-id="5141a-344">Limit czasu początkowej uzgadniania z serwerem.</span><span class="sxs-lookup"><span data-stu-id="5141a-344">Timeout for initial server handshake.</span></span> <span data-ttu-id="5141a-345">Jeśli serwer nie wyśle odpowiedzi uzgadniania w tym interwale, klient anuluje uzgadnianie i wyzwala zdarzenie `onClose`.</span><span class="sxs-lookup"><span data-stu-id="5141a-345">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="5141a-346">Jest to ustawienie zaawansowane, które należy zmodyfikować tylko w przypadku wystąpienia błędów limitu czasu uzgadniania z powodu poważnego opóźnienia sieci.</span><span class="sxs-lookup"><span data-stu-id="5141a-346">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="5141a-347">Aby uzyskać więcej szczegółów na temat procesu uzgadniania, zobacz [Specyfikacja protokołuSignalR Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="5141a-347">For more detail on the Handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| <span data-ttu-id="5141a-348">`getKeepAliveInterval` / `setKeepAliveInterval`</span><span class="sxs-lookup"><span data-stu-id="5141a-348">`getKeepAliveInterval` / `setKeepAliveInterval`</span></span> | <span data-ttu-id="5141a-349">15 sekund (15 000 MS)</span><span class="sxs-lookup"><span data-stu-id="5141a-349">15 seconds (15,000 milliseconds)</span></span> | <span data-ttu-id="5141a-350">Określa interwał wysyłania komunikatów ping przez klienta.</span><span class="sxs-lookup"><span data-stu-id="5141a-350">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="5141a-351">Wysłanie wszelkich komunikatów z klienta powoduje zresetowanie czasomierza do początku interwału.</span><span class="sxs-lookup"><span data-stu-id="5141a-351">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="5141a-352">Jeśli klient nie wyśle komunikatu w `ClientTimeoutInterval` zestawie na serwerze, serwer traktuje klienta jako odłączony.</span><span class="sxs-lookup"><span data-stu-id="5141a-352">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="nettabdotnet"></a>[<span data-ttu-id="5141a-353">.NET</span><span class="sxs-lookup"><span data-stu-id="5141a-353">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="5141a-354">Opcja</span><span class="sxs-lookup"><span data-stu-id="5141a-354">Option</span></span> | <span data-ttu-id="5141a-355">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="5141a-355">Default value</span></span> | <span data-ttu-id="5141a-356">Opis</span><span class="sxs-lookup"><span data-stu-id="5141a-356">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="5141a-357">30 sekund (30 000 MS)</span><span class="sxs-lookup"><span data-stu-id="5141a-357">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="5141a-358">Limit czasu dla działania serwera.</span><span class="sxs-lookup"><span data-stu-id="5141a-358">Timeout for server activity.</span></span> <span data-ttu-id="5141a-359">Jeśli serwer nie wysłał komunikatu w tym interwale, Klient uważa, że serwer odłączył się i wyzwala zdarzenie `Closed` (`onclose` w języku JavaScript).</span><span class="sxs-lookup"><span data-stu-id="5141a-359">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="5141a-360">Ta wartość musi być wystarczająca do wysłania wiadomości ping z serwera **i** odebrana przez klienta w ramach przedziału czasu.</span><span class="sxs-lookup"><span data-stu-id="5141a-360">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="5141a-361">Zalecana wartość to liczba z co najmniej podwójną wartością `KeepAliveInterval` serwera, aby umożliwić czas na nadejście poleceń ping.</span><span class="sxs-lookup"><span data-stu-id="5141a-361">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="5141a-362">15 sekund</span><span class="sxs-lookup"><span data-stu-id="5141a-362">15 seconds</span></span> | <span data-ttu-id="5141a-363">Limit czasu początkowej uzgadniania z serwerem.</span><span class="sxs-lookup"><span data-stu-id="5141a-363">Timeout for initial server handshake.</span></span> <span data-ttu-id="5141a-364">Jeśli serwer nie wyśle odpowiedzi uzgadniania w tym interwale, klient anuluje uzgadnianie i wyzwala zdarzenie `Closed` (`onclose` w języku JavaScript).</span><span class="sxs-lookup"><span data-stu-id="5141a-364">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="5141a-365">Jest to ustawienie zaawansowane, które należy zmodyfikować tylko w przypadku wystąpienia błędów limitu czasu uzgadniania z powodu poważnego opóźnienia sieci.</span><span class="sxs-lookup"><span data-stu-id="5141a-365">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="5141a-366">Aby uzyskać więcej szczegółów na temat procesu uzgadniania, zobacz [Specyfikacja protokołuSignalR Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="5141a-366">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

<span data-ttu-id="5141a-367">W kliencie .NET wartości limitu czasu są określane jako wartości `TimeSpan`.</span><span class="sxs-lookup"><span data-stu-id="5141a-367">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="5141a-368">JavaScript</span><span class="sxs-lookup"><span data-stu-id="5141a-368">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="5141a-369">Opcja</span><span class="sxs-lookup"><span data-stu-id="5141a-369">Option</span></span> | <span data-ttu-id="5141a-370">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="5141a-370">Default value</span></span> | <span data-ttu-id="5141a-371">Opis</span><span class="sxs-lookup"><span data-stu-id="5141a-371">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="5141a-372">30 sekund (30 000 MS)</span><span class="sxs-lookup"><span data-stu-id="5141a-372">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="5141a-373">Limit czasu dla działania serwera.</span><span class="sxs-lookup"><span data-stu-id="5141a-373">Timeout for server activity.</span></span> <span data-ttu-id="5141a-374">Jeśli serwer nie wysłał komunikatu w tym interwale, Klient uważa, że serwer odłączył się i wyzwala zdarzenie `onclose`.</span><span class="sxs-lookup"><span data-stu-id="5141a-374">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="5141a-375">Ta wartość musi być wystarczająca do wysłania wiadomości ping z serwera **i** odebrana przez klienta w ramach przedziału czasu.</span><span class="sxs-lookup"><span data-stu-id="5141a-375">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="5141a-376">Zalecana wartość to liczba z co najmniej podwójną wartością `KeepAliveInterval` serwera, aby umożliwić czas na nadejście poleceń ping.</span><span class="sxs-lookup"><span data-stu-id="5141a-376">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="5141a-377">Java</span><span class="sxs-lookup"><span data-stu-id="5141a-377">Java</span></span>](#tab/java)

| <span data-ttu-id="5141a-378">Opcja</span><span class="sxs-lookup"><span data-stu-id="5141a-378">Option</span></span> | <span data-ttu-id="5141a-379">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="5141a-379">Default value</span></span> | <span data-ttu-id="5141a-380">Opis</span><span class="sxs-lookup"><span data-stu-id="5141a-380">Description</span></span> |
| ------ | ------------- | ----------- |
| <span data-ttu-id="5141a-381">`getServerTimeout` / `setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="5141a-381">`getServerTimeout` / `setServerTimeout`</span></span> | <span data-ttu-id="5141a-382">30 sekund (30 000 MS)</span><span class="sxs-lookup"><span data-stu-id="5141a-382">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="5141a-383">Limit czasu dla działania serwera.</span><span class="sxs-lookup"><span data-stu-id="5141a-383">Timeout for server activity.</span></span> <span data-ttu-id="5141a-384">Jeśli serwer nie wysłał komunikatu w tym interwale, Klient uważa, że serwer odłączył się i wyzwala zdarzenie `onClose`.</span><span class="sxs-lookup"><span data-stu-id="5141a-384">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="5141a-385">Ta wartość musi być wystarczająca do wysłania wiadomości ping z serwera **i** odebrana przez klienta w ramach przedziału czasu.</span><span class="sxs-lookup"><span data-stu-id="5141a-385">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="5141a-386">Zalecana wartość to liczba z co najmniej podwójną wartością `KeepAliveInterval` serwera, aby umożliwić czas na nadejście poleceń ping.</span><span class="sxs-lookup"><span data-stu-id="5141a-386">The recommended value is a number at least double the server's `KeepAliveInterval` value, to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="5141a-387">15 sekund</span><span class="sxs-lookup"><span data-stu-id="5141a-387">15 seconds</span></span> | <span data-ttu-id="5141a-388">Limit czasu początkowej uzgadniania z serwerem.</span><span class="sxs-lookup"><span data-stu-id="5141a-388">Timeout for initial server handshake.</span></span> <span data-ttu-id="5141a-389">Jeśli serwer nie wyśle odpowiedzi uzgadniania w tym interwale, klient anuluje uzgadnianie i wyzwala zdarzenie `onClose`.</span><span class="sxs-lookup"><span data-stu-id="5141a-389">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="5141a-390">Jest to ustawienie zaawansowane, które należy zmodyfikować tylko w przypadku wystąpienia błędów limitu czasu uzgadniania z powodu poważnego opóźnienia sieci.</span><span class="sxs-lookup"><span data-stu-id="5141a-390">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="5141a-391">Aby uzyskać więcej szczegółów na temat procesu uzgadniania, zobacz [Specyfikacja protokołuSignalR Hub](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="5141a-391">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

::: moniker-end

---

### <a name="configure-additional-options"></a><span data-ttu-id="5141a-392">Skonfiguruj dodatkowe opcje</span><span class="sxs-lookup"><span data-stu-id="5141a-392">Configure additional options</span></span>

<span data-ttu-id="5141a-393">Dodatkowe opcje można skonfigurować w metodzie `WithUrl` (`withUrl` w języku JavaScript) na `HubConnectionBuilder` lub w różnych interfejsach API konfiguracji na `HttpHubConnectionBuilder` klienta Java:</span><span class="sxs-lookup"><span data-stu-id="5141a-393">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder` or on the various configuration APIs on the `HttpHubConnectionBuilder` in the Java client:</span></span>

# <a name="nettabdotnet"></a>[<span data-ttu-id="5141a-394">.NET</span><span class="sxs-lookup"><span data-stu-id="5141a-394">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="5141a-395">Opcja .NET</span><span class="sxs-lookup"><span data-stu-id="5141a-395">.NET Option</span></span> |  <span data-ttu-id="5141a-396">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="5141a-396">Default value</span></span> | <span data-ttu-id="5141a-397">Opis</span><span class="sxs-lookup"><span data-stu-id="5141a-397">Description</span></span> |
| ----------- | -------------- | ----------- |
| `AccessTokenProvider` | `null` | <span data-ttu-id="5141a-398">Funkcja zwracająca ciąg, który jest dostarczany jako token uwierzytelniania okaziciela w żądaniach HTTP.</span><span class="sxs-lookup"><span data-stu-id="5141a-398">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `false` | <span data-ttu-id="5141a-399">Ustaw tę wartość na `true`, aby pominąć etap negocjacji.</span><span class="sxs-lookup"><span data-stu-id="5141a-399">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="5141a-400">**Obsługiwane tylko wtedy, gdy transport Sockets jest jedynym włączonym transportem**.</span><span class="sxs-lookup"><span data-stu-id="5141a-400">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="5141a-401">Nie można włączyć tego ustawienia w przypadku korzystania z usługi Azure SignalR.</span><span class="sxs-lookup"><span data-stu-id="5141a-401">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="5141a-402">Pusty</span><span class="sxs-lookup"><span data-stu-id="5141a-402">Empty</span></span> | <span data-ttu-id="5141a-403">Kolekcja certyfikatów TLS do wysłania w celu uwierzytelniania żądań.</span><span class="sxs-lookup"><span data-stu-id="5141a-403">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="5141a-404">Pusty</span><span class="sxs-lookup"><span data-stu-id="5141a-404">Empty</span></span> | <span data-ttu-id="5141a-405">Kolekcja plików cookie protokołu HTTP do wysłania w każdym żądaniu HTTP.</span><span class="sxs-lookup"><span data-stu-id="5141a-405">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="5141a-406">Pusty</span><span class="sxs-lookup"><span data-stu-id="5141a-406">Empty</span></span> | <span data-ttu-id="5141a-407">Poświadczenia do wysyłania przy każdym żądaniu HTTP.</span><span class="sxs-lookup"><span data-stu-id="5141a-407">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="5141a-408">5 sekund</span><span class="sxs-lookup"><span data-stu-id="5141a-408">5 seconds</span></span> | <span data-ttu-id="5141a-409">Tylko obiekty WebSockets.</span><span class="sxs-lookup"><span data-stu-id="5141a-409">WebSockets only.</span></span> <span data-ttu-id="5141a-410">Maksymalny czas oczekiwania klienta po zamknięciu serwera w celu potwierdzenia żądania zamknięcia.</span><span class="sxs-lookup"><span data-stu-id="5141a-410">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="5141a-411">Jeśli serwer nie potwierdzi zamknięcia w tym czasie, klient zostanie rozłączony.</span><span class="sxs-lookup"><span data-stu-id="5141a-411">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="5141a-412">Pusty</span><span class="sxs-lookup"><span data-stu-id="5141a-412">Empty</span></span> | <span data-ttu-id="5141a-413">Mapa dodatkowych nagłówków HTTP do wysłania w każdym żądaniu HTTP.</span><span class="sxs-lookup"><span data-stu-id="5141a-413">A Map of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | `null` | <span data-ttu-id="5141a-414">Delegat, który może służyć do konfigurowania lub zastępowania `HttpMessageHandler` używany do wysyłania żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="5141a-414">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="5141a-415">Nieużywane na potrzeby połączeń protokołu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="5141a-415">Not used for WebSocket connections.</span></span> <span data-ttu-id="5141a-416">Ten delegat musi zwracać wartość różną od null i otrzymuje wartość domyślną jako parametr.</span><span class="sxs-lookup"><span data-stu-id="5141a-416">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="5141a-417">Zmodyfikuj ustawienia tej wartości domyślnej i zwróć ją lub Zwróć nowe wystąpienie `HttpMessageHandler`.</span><span class="sxs-lookup"><span data-stu-id="5141a-417">Either modify settings on that default value and return it, or return a new `HttpMessageHandler` instance.</span></span> <span data-ttu-id="5141a-418">**Podczas zastępowania programu obsługi należy skopiować ustawienia, które mają być zachowane z podanej procedury obsługi. w przeciwnym razie skonfigurowane opcje (takie jak pliki cookie i nagłówki) nie będą miały zastosowania do nowego programu obsługi.**</span><span class="sxs-lookup"><span data-stu-id="5141a-418">**When replacing the handler make sure to copy the settings you want to keep from the provided handler, otherwise, the configured options (such as Cookies and Headers) won't apply to the new handler.**</span></span> |
| `Proxy` | `null` | <span data-ttu-id="5141a-419">Serwer proxy HTTP, który ma być używany podczas wysyłania żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="5141a-419">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | `false` | <span data-ttu-id="5141a-420">Ustaw tę wartość logiczną, aby wysyłać domyślne poświadczenia dla żądań HTTP i WebSockets.</span><span class="sxs-lookup"><span data-stu-id="5141a-420">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="5141a-421">Umożliwia to korzystanie z uwierzytelniania systemu Windows.</span><span class="sxs-lookup"><span data-stu-id="5141a-421">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | `null` | <span data-ttu-id="5141a-422">Delegat, który może służyć do konfigurowania dodatkowych opcji protokołu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="5141a-422">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="5141a-423">Odbiera wystąpienie elementu [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) , którego można użyć do skonfigurowania opcji.</span><span class="sxs-lookup"><span data-stu-id="5141a-423">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="5141a-424">JavaScript</span><span class="sxs-lookup"><span data-stu-id="5141a-424">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="5141a-425">Opcja języka JavaScript</span><span class="sxs-lookup"><span data-stu-id="5141a-425">JavaScript Option</span></span> | <span data-ttu-id="5141a-426">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="5141a-426">Default Value</span></span> | <span data-ttu-id="5141a-427">Opis</span><span class="sxs-lookup"><span data-stu-id="5141a-427">Description</span></span> |
| ----------------- | ------------- | ----------- |
| `accessTokenFactory` | `null` | <span data-ttu-id="5141a-428">Funkcja zwracająca ciąg, który jest dostarczany jako token uwierzytelniania okaziciela w żądaniach HTTP.</span><span class="sxs-lookup"><span data-stu-id="5141a-428">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `skipNegotiation` | `false` | <span data-ttu-id="5141a-429">Ustaw tę wartość na `true`, aby pominąć etap negocjacji.</span><span class="sxs-lookup"><span data-stu-id="5141a-429">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="5141a-430">**Obsługiwane tylko wtedy, gdy transport Sockets jest jedynym włączonym transportem**.</span><span class="sxs-lookup"><span data-stu-id="5141a-430">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="5141a-431">Nie można włączyć tego ustawienia w przypadku korzystania z usługi Azure SignalR.</span><span class="sxs-lookup"><span data-stu-id="5141a-431">This setting can't be enabled when using the Azure SignalR Service.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="5141a-432">Java</span><span class="sxs-lookup"><span data-stu-id="5141a-432">Java</span></span>](#tab/java)

| <span data-ttu-id="5141a-433">Opcja Java</span><span class="sxs-lookup"><span data-stu-id="5141a-433">Java Option</span></span> | <span data-ttu-id="5141a-434">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="5141a-434">Default Value</span></span> | <span data-ttu-id="5141a-435">Opis</span><span class="sxs-lookup"><span data-stu-id="5141a-435">Description</span></span> |
| ----------- | ------------- | ----------- |
| `withAccessTokenProvider` | `null` | <span data-ttu-id="5141a-436">Funkcja zwracająca ciąg, który jest dostarczany jako token uwierzytelniania okaziciela w żądaniach HTTP.</span><span class="sxs-lookup"><span data-stu-id="5141a-436">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `shouldSkipNegotiate` | `false` | <span data-ttu-id="5141a-437">Ustaw tę wartość na `true`, aby pominąć etap negocjacji.</span><span class="sxs-lookup"><span data-stu-id="5141a-437">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="5141a-438">**Obsługiwane tylko wtedy, gdy transport Sockets jest jedynym włączonym transportem**.</span><span class="sxs-lookup"><span data-stu-id="5141a-438">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="5141a-439">Nie można włączyć tego ustawienia w przypadku korzystania z usługi Azure SignalR.</span><span class="sxs-lookup"><span data-stu-id="5141a-439">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| <span data-ttu-id="5141a-440">`withHeader``withHeaders`</span><span class="sxs-lookup"><span data-stu-id="5141a-440">`withHeader` `withHeaders`</span></span> | <span data-ttu-id="5141a-441">Pusty</span><span class="sxs-lookup"><span data-stu-id="5141a-441">Empty</span></span> | <span data-ttu-id="5141a-442">Mapa dodatkowych nagłówków HTTP do wysłania w każdym żądaniu HTTP.</span><span class="sxs-lookup"><span data-stu-id="5141a-442">A Map of additional HTTP headers to send with every HTTP request.</span></span> |

---

<span data-ttu-id="5141a-443">W kliencie .NET opcje te można modyfikować za pomocą delegata opcji udostępnianego do `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="5141a-443">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="5141a-444">W kliencie JavaScript te opcje można podać w obiekcie JavaScript udostępnionym do `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="5141a-444">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

<span data-ttu-id="5141a-445">W kliencie Java Opcje te można skonfigurować przy użyciu metod na `HttpHubConnectionBuilder` zwracanych z `HubConnectionBuilder.create("HUB URL")`</span><span class="sxs-lookup"><span data-stu-id="5141a-445">In the Java client, these options can be configured with the methods on the `HttpHubConnectionBuilder` returned from the `HubConnectionBuilder.create("HUB URL")`</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
        .withHeader("Foo", "Bar")
        .shouldSkipNegotiate(true)
        .withHandshakeResponseTimeout(30*1000)
        .build();
```

## <a name="additional-resources"></a><span data-ttu-id="5141a-446">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="5141a-446">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>
