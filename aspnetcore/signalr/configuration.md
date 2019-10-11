---
title: Konfiguracja sygnałów ASP.NET Core
author: bradygaster
description: Dowiedz się, jak skonfigurować aplikacje sygnalizujące ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 08/05/2019
uid: signalr/configuration
ms.openlocfilehash: 66f274fcda27392091de6b4be8c7221bc87b7585
ms.sourcegitcommit: c452e6af92e130413106c4863193f377cde4cd9c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/10/2019
ms.locfileid: "72246492"
---
# <a name="aspnet-core-signalr-configuration"></a><span data-ttu-id="ce7e6-103">Konfiguracja sygnałów ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ce7e6-103">ASP.NET Core SignalR configuration</span></span>

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="ce7e6-104">Opcje serializacji JSON/MessagePack</span><span class="sxs-lookup"><span data-stu-id="ce7e6-104">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="ce7e6-105">ASP.NET Core sygnalizujący obsługuje dwa protokoły dla komunikatów kodowania: [JSON](https://www.json.org/) i [MessagePack](https://msgpack.org/index.html).</span><span class="sxs-lookup"><span data-stu-id="ce7e6-105">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="ce7e6-106">Każdy protokół ma opcje konfiguracji serializacji.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-106">Each protocol has serialization configuration options.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="ce7e6-107">Serializacji JSON można skonfigurować na serwerze przy użyciu metody rozszerzenia [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) .</span><span class="sxs-lookup"><span data-stu-id="ce7e6-107">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method.</span></span> <span data-ttu-id="ce7e6-108">`AddJsonProtocol` można dodać po [Addsignaler](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-108">`AddJsonProtocol` can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="ce7e6-109">Metoda `AddJsonProtocol` przyjmuje delegata, który odbiera obiekt `options`.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-109">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="ce7e6-110">Właściwość [PayloadSerializerOptions](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializeroptions) dla tego obiektu jest obiektem `System.Text.Json` <xref:System.Text.Json.JsonSerializerOptions>, którego można użyć do skonfigurowania serializacji argumentów i zwracanych wartości.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-110">The [PayloadSerializerOptions](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializeroptions) property on that object is a `System.Text.Json` <xref:System.Text.Json.JsonSerializerOptions> object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="ce7e6-111">Aby uzyskać więcej informacji, zobacz [dokumentację system. Text. JSON](/dotnet/api/system.text.json).</span><span class="sxs-lookup"><span data-stu-id="ce7e6-111">For more information, see the [System.Text.Json documentation](/dotnet/api/system.text.json).</span></span>

<span data-ttu-id="ce7e6-112">Przykładowo, aby skonfigurować Serializator, aby nie zmieniać wielkości liter nazw właściwości zamiast domyślnych nazw "camelCase", należy użyć następującego kodu w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="ce7e6-112">As an example, to configure the serializer to not change the casing of property names, instead of the default "camelCase" names, use the following code in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerOptions.PropertyNamingPolicy = null
    });
```

<span data-ttu-id="ce7e6-113">W kliencie .NET ta sama Metoda rozszerzenia `AddJsonProtocol` istnieje w [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="ce7e6-113">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="ce7e6-114">Aby można było rozpoznać metodę rozszerzenia, należy zaimportować przestrzeń nazw `Microsoft.Extensions.DependencyInjection`:</span><span class="sxs-lookup"><span data-stu-id="ce7e6-114">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

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

### <a name="switch-to-newtonsoftjson"></a><span data-ttu-id="ce7e6-115">Przejdź do pliku Newtonsoft. JSON</span><span class="sxs-lookup"><span data-stu-id="ce7e6-115">Switch to Newtonsoft.Json</span></span>

<span data-ttu-id="ce7e6-116">Jeśli potrzebujesz funkcji `Newtonsoft.Json`, które nie są obsługiwane w `System.Text.Json`, zobacz [Switch to Newtonsoft. JSON](xref:migration/22-to-30#switch-to-newtonsoftjson).</span><span class="sxs-lookup"><span data-stu-id="ce7e6-116">If you need features of `Newtonsoft.Json` that aren't supported in `System.Text.Json`, See [Switch to Newtonsoft.Json](xref:migration/22-to-30#switch-to-newtonsoftjson).</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="ce7e6-117">Serializacji JSON można skonfigurować na serwerze przy użyciu metody rozszerzenia [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) , która może zostać dodana po [addsignaler](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) w metodzie `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-117">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method, which can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in your `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="ce7e6-118">Metoda `AddJsonProtocol` przyjmuje delegata, który odbiera obiekt `options`.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-118">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="ce7e6-119">Właściwość [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) tego obiektu jest obiektem JSON.NET `JsonSerializerSettings`, którego można użyć do skonfigurowania serializacji argumentów i zwracanych wartości.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-119">The [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) property on that object is a JSON.NET `JsonSerializerSettings` object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="ce7e6-120">Aby uzyskać więcej informacji, zapoznaj się z [dokumentacją JSON.NET](https://www.newtonsoft.com/json/help/html/Introduction.htm).</span><span class="sxs-lookup"><span data-stu-id="ce7e6-120">For more information, see the [JSON.NET documentation](https://www.newtonsoft.com/json/help/html/Introduction.htm).</span></span>
 
<span data-ttu-id="ce7e6-121">Przykładowo, aby skonfigurować serializator do używania nazw właściwości "PascalCase" zamiast domyślnych nazw "camelCase", należy użyć następującego kodu w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="ce7e6-121">As an example, to configure the serializer to use "PascalCase" property names, instead of the default "camelCase" names, use the following code in `Startup.ConfigureServices`:</span></span>
 
```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver =
            new DefaultContractResolver();
    });
```

<span data-ttu-id="ce7e6-122">W kliencie .NET ta sama Metoda rozszerzenia `AddJsonProtocol` istnieje w [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="ce7e6-122">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="ce7e6-123">Aby można było rozpoznać metodę rozszerzenia, należy zaimportować przestrzeń nazw `Microsoft.Extensions.DependencyInjection`:</span><span class="sxs-lookup"><span data-stu-id="ce7e6-123">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

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

::: moniker-end

> [!NOTE]
> <span data-ttu-id="ce7e6-124">W tej chwili nie można skonfigurować serializacji JSON w kliencie JavaScript.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-124">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="ce7e6-125">Opcje serializacji MessagePack</span><span class="sxs-lookup"><span data-stu-id="ce7e6-125">MessagePack serialization options</span></span>

<span data-ttu-id="ce7e6-126">Serializacji MessagePack można skonfigurować, dostarczając delegata do wywołania [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) .</span><span class="sxs-lookup"><span data-stu-id="ce7e6-126">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="ce7e6-127">Aby uzyskać więcej informacji, zobacz [MessagePack w sygnalizacji](xref:signalr/messagepackhubprotocol) .</span><span class="sxs-lookup"><span data-stu-id="ce7e6-127">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="ce7e6-128">W tej chwili nie można skonfigurować serializacji MessagePack w kliencie JavaScript.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-128">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="ce7e6-129">Konfigurowanie opcji serwera</span><span class="sxs-lookup"><span data-stu-id="ce7e6-129">Configure server options</span></span>

<span data-ttu-id="ce7e6-130">W poniższej tabeli opisano opcje konfigurowania centrów sygnałów:</span><span class="sxs-lookup"><span data-stu-id="ce7e6-130">The following table describes options for configuring SignalR hubs:</span></span>

::: moniker range=">= aspnetcore-3.0"

| <span data-ttu-id="ce7e6-131">Opcja</span><span class="sxs-lookup"><span data-stu-id="ce7e6-131">Option</span></span> | <span data-ttu-id="ce7e6-132">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="ce7e6-132">Default Value</span></span> | <span data-ttu-id="ce7e6-133">Opis</span><span class="sxs-lookup"><span data-stu-id="ce7e6-133">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="ce7e6-134">30 sekund</span><span class="sxs-lookup"><span data-stu-id="ce7e6-134">30 seconds</span></span> | <span data-ttu-id="ce7e6-135">Serwer Rozważ odłączenie klienta, jeśli nie odebrano komunikatu (w tym w tym interwale).</span><span class="sxs-lookup"><span data-stu-id="ce7e6-135">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="ce7e6-136">Może upłynąć dłużej niż ten interwał limitu czasu, aby klient mógł zostać oznaczony jako odłączony, ze względu na to, jak jest to zaimplementowane.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-136">It could take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="ce7e6-137">Zalecaną wartością jest podwójna wartość `KeepAliveInterval`.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-137">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="ce7e6-138">15 sekund</span><span class="sxs-lookup"><span data-stu-id="ce7e6-138">15 seconds</span></span> | <span data-ttu-id="ce7e6-139">Jeśli klient nie wyśle początkowego komunikatu uzgadniania w tym przedziale czasu, połączenie zostanie zamknięte.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-139">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="ce7e6-140">Jest to ustawienie zaawansowane, które należy zmodyfikować tylko w przypadku wystąpienia błędów limitu czasu uzgadniania z powodu poważnego opóźnienia sieci.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-140">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="ce7e6-141">Aby uzyskać więcej szczegółów na temat procesu uzgadniania, zobacz [specyfikację protokołu centrum sygnału](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="ce7e6-141">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="ce7e6-142">15 sekund</span><span class="sxs-lookup"><span data-stu-id="ce7e6-142">15 seconds</span></span> | <span data-ttu-id="ce7e6-143">Jeśli serwer nie wysłał komunikatu w tym interwale, zostanie automatycznie wysłana wiadomość ping w celu pozostawienia otwartego połączenia.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-143">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="ce7e6-144">Podczas zmiany `KeepAliveInterval` Zmień ustawienie `ServerTimeout` @ no__t-2 @ no__t-3 na kliencie.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-144">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="ce7e6-145">Zalecana wartość `ServerTimeout` @ no__t-1 @ no__t-2 jest podwójnie wartością `KeepAliveInterval`.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-145">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="ce7e6-146">Wszystkie zainstalowane protokoły</span><span class="sxs-lookup"><span data-stu-id="ce7e6-146">All installed protocols</span></span> | <span data-ttu-id="ce7e6-147">Protokoły obsługiwane przez to centrum.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-147">Protocols supported by this hub.</span></span> <span data-ttu-id="ce7e6-148">Domyślnie wszystkie protokoły zarejestrowane na serwerze są dozwolone, ale protokoły można usunąć z tej listy, aby wyłączyć określone protokoły dla poszczególnych centrów.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-148">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="ce7e6-149">Jeśli `true`, szczegółowe komunikaty o wyjątkach są zwracane do klientów, gdy wyjątek jest zgłaszany w metodzie centrum.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-149">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="ce7e6-150">Wartość domyślna to `false`, ponieważ te komunikaty o wyjątkach mogą zawierać informacje poufne.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-150">The default is `false`, as these exception messages can contain sensitive information.</span></span> |
| `StreamBufferCapacity` | `10` | <span data-ttu-id="ce7e6-151">Maksymalna liczba elementów, które mogą być buforowane dla strumieni przekazywania klientów.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-151">The maximum number of items that can be buffered for client upload streams.</span></span> <span data-ttu-id="ce7e6-152">Po osiągnięciu tego limitu przetwarzanie wywołań jest blokowane, dopóki serwer nie przetwarza elementów strumienia.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-152">If this limit is reached, the processing of invocations is blocked until the server processes stream items.</span></span>|
| `MaximumReceiveMessageSize` | <span data-ttu-id="ce7e6-153">32 KB</span><span class="sxs-lookup"><span data-stu-id="ce7e6-153">32 KB</span></span> | <span data-ttu-id="ce7e6-154">Maksymalny rozmiar pojedynczego przychodzącego komunikatu centrum.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-154">Maximum size of a single incoming hub message.</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-2.2"

| <span data-ttu-id="ce7e6-155">Opcja</span><span class="sxs-lookup"><span data-stu-id="ce7e6-155">Option</span></span> | <span data-ttu-id="ce7e6-156">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="ce7e6-156">Default Value</span></span> | <span data-ttu-id="ce7e6-157">Opis</span><span class="sxs-lookup"><span data-stu-id="ce7e6-157">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="ce7e6-158">30 sekund</span><span class="sxs-lookup"><span data-stu-id="ce7e6-158">30 seconds</span></span> | <span data-ttu-id="ce7e6-159">Serwer Rozważ odłączenie klienta, jeśli nie odebrano komunikatu (w tym w tym interwale).</span><span class="sxs-lookup"><span data-stu-id="ce7e6-159">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="ce7e6-160">Może upłynąć dłużej niż ten interwał limitu czasu, aby klient mógł zostać oznaczony jako odłączony, ze względu na to, jak jest to zaimplementowane.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-160">It could take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="ce7e6-161">Zalecaną wartością jest podwójna wartość `KeepAliveInterval`.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-161">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="ce7e6-162">15 sekund</span><span class="sxs-lookup"><span data-stu-id="ce7e6-162">15 seconds</span></span> | <span data-ttu-id="ce7e6-163">Jeśli klient nie wyśle początkowego komunikatu uzgadniania w tym przedziale czasu, połączenie zostanie zamknięte.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-163">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="ce7e6-164">Jest to ustawienie zaawansowane, które należy zmodyfikować tylko w przypadku wystąpienia błędów limitu czasu uzgadniania z powodu poważnego opóźnienia sieci.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-164">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="ce7e6-165">Aby uzyskać więcej szczegółów na temat procesu uzgadniania, zobacz [specyfikację protokołu centrum sygnału](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="ce7e6-165">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="ce7e6-166">15 sekund</span><span class="sxs-lookup"><span data-stu-id="ce7e6-166">15 seconds</span></span> | <span data-ttu-id="ce7e6-167">Jeśli serwer nie wysłał komunikatu w tym interwale, zostanie automatycznie wysłana wiadomość ping w celu pozostawienia otwartego połączenia.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-167">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="ce7e6-168">Podczas zmiany `KeepAliveInterval` Zmień ustawienie `ServerTimeout` @ no__t-2 @ no__t-3 na kliencie.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-168">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="ce7e6-169">Zalecana wartość `ServerTimeout` @ no__t-1 @ no__t-2 jest podwójnie wartością `KeepAliveInterval`.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-169">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="ce7e6-170">Wszystkie zainstalowane protokoły</span><span class="sxs-lookup"><span data-stu-id="ce7e6-170">All installed protocols</span></span> | <span data-ttu-id="ce7e6-171">Protokoły obsługiwane przez to centrum.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-171">Protocols supported by this hub.</span></span> <span data-ttu-id="ce7e6-172">Domyślnie wszystkie protokoły zarejestrowane na serwerze są dozwolone, ale protokoły można usunąć z tej listy, aby wyłączyć określone protokoły dla poszczególnych centrów.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-172">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="ce7e6-173">Jeśli `true`, szczegółowe komunikaty o wyjątkach są zwracane do klientów, gdy wyjątek jest zgłaszany w metodzie centrum.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-173">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="ce7e6-174">Wartość domyślna to `false`, ponieważ te komunikaty o wyjątkach mogą zawierać informacje poufne.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-174">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-2.1"

| <span data-ttu-id="ce7e6-175">Opcja</span><span class="sxs-lookup"><span data-stu-id="ce7e6-175">Option</span></span> | <span data-ttu-id="ce7e6-176">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="ce7e6-176">Default Value</span></span> | <span data-ttu-id="ce7e6-177">Opis</span><span class="sxs-lookup"><span data-stu-id="ce7e6-177">Description</span></span> |
| ------ | ------------- | ----------- |
| `HandshakeTimeout` | <span data-ttu-id="ce7e6-178">15 sekund</span><span class="sxs-lookup"><span data-stu-id="ce7e6-178">15 seconds</span></span> | <span data-ttu-id="ce7e6-179">Jeśli klient nie wyśle początkowego komunikatu uzgadniania w tym przedziale czasu, połączenie zostanie zamknięte.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-179">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="ce7e6-180">Jest to ustawienie zaawansowane, które należy zmodyfikować tylko w przypadku wystąpienia błędów limitu czasu uzgadniania z powodu poważnego opóźnienia sieci.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-180">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="ce7e6-181">Aby uzyskać więcej szczegółów na temat procesu uzgadniania, zobacz [specyfikację protokołu centrum sygnału](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="ce7e6-181">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="ce7e6-182">15 sekund</span><span class="sxs-lookup"><span data-stu-id="ce7e6-182">15 seconds</span></span> | <span data-ttu-id="ce7e6-183">Jeśli serwer nie wysłał komunikatu w tym interwale, zostanie automatycznie wysłana wiadomość ping w celu pozostawienia otwartego połączenia.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-183">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="ce7e6-184">Podczas zmiany `KeepAliveInterval` Zmień ustawienie `ServerTimeout` @ no__t-2 @ no__t-3 na kliencie.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-184">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="ce7e6-185">Zalecana wartość `ServerTimeout` @ no__t-1 @ no__t-2 jest podwójnie wartością `KeepAliveInterval`.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-185">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="ce7e6-186">Wszystkie zainstalowane protokoły</span><span class="sxs-lookup"><span data-stu-id="ce7e6-186">All installed protocols</span></span> | <span data-ttu-id="ce7e6-187">Protokoły obsługiwane przez to centrum.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-187">Protocols supported by this hub.</span></span> <span data-ttu-id="ce7e6-188">Domyślnie wszystkie protokoły zarejestrowane na serwerze są dozwolone, ale protokoły można usunąć z tej listy, aby wyłączyć określone protokoły dla poszczególnych centrów.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-188">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="ce7e6-189">Jeśli `true`, szczegółowe komunikaty o wyjątkach są zwracane do klientów, gdy wyjątek jest zgłaszany w metodzie centrum.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-189">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="ce7e6-190">Wartość domyślna to `false`, ponieważ te komunikaty o wyjątkach mogą zawierać informacje poufne.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-190">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

::: moniker-end

<span data-ttu-id="ce7e6-191">Opcje można skonfigurować dla wszystkich centrów, dostarczając opcje delegatów wywołania `AddSignalR` w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-191">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

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

<span data-ttu-id="ce7e6-192">Opcje jednego centrum przesłaniają opcje globalne podane w `AddSignalR` i można je skonfigurować przy użyciu <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span><span class="sxs-lookup"><span data-stu-id="ce7e6-192">Options for a single hub override the global options provided in `AddSignalR` and can be configured using <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

### <a name="advanced-http-configuration-options"></a><span data-ttu-id="ce7e6-193">Zaawansowane opcje konfiguracji HTTP</span><span class="sxs-lookup"><span data-stu-id="ce7e6-193">Advanced HTTP configuration options</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="ce7e6-194">Użyj `HttpConnectionDispatcherOptions`, aby skonfigurować zaawansowane ustawienia związane z transportami i zarządzaniem buforem pamięci.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-194">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="ce7e6-195">Te opcje są konfigurowane przez przekazanie delegata do [MapHub @ no__t-1T >](/dotnet/api/microsoft.aspnetcore.builder.hubendpointroutebuilderextensions.maphub) w `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-195">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.builder.hubendpointroutebuilderextensions.maphub) in `Startup.Configure`.</span></span>

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

<span data-ttu-id="ce7e6-196">Użyj `HttpConnectionDispatcherOptions`, aby skonfigurować zaawansowane ustawienia związane z transportami i zarządzaniem buforem pamięci.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-196">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="ce7e6-197">Te opcje są konfigurowane przez przekazanie delegata do [MapHub @ no__t-1T >](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) w `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-197">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) in `Startup.Configure`.</span></span>

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

<span data-ttu-id="ce7e6-198">W poniższej tabeli opisano opcje konfigurowania zaawansowanych opcji protokołu HTTP w programie ASP.NET Core Signal:</span><span class="sxs-lookup"><span data-stu-id="ce7e6-198">The following table describes options for configuring ASP.NET Core SignalR's advanced HTTP options:</span></span>

::: moniker range=">= aspnetcore-3.0"

| <span data-ttu-id="ce7e6-199">Opcja</span><span class="sxs-lookup"><span data-stu-id="ce7e6-199">Option</span></span> | <span data-ttu-id="ce7e6-200">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="ce7e6-200">Default Value</span></span> | <span data-ttu-id="ce7e6-201">Opis</span><span class="sxs-lookup"><span data-stu-id="ce7e6-201">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="ce7e6-202">32 KB</span><span class="sxs-lookup"><span data-stu-id="ce7e6-202">32 KB</span></span> | <span data-ttu-id="ce7e6-203">Maksymalna liczba bajtów odebranych od klienta, które buforują serwer przed zastosowaniem nadużycia.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-203">The maximum number of bytes received from the client that the server buffers before applying backpressure.</span></span> <span data-ttu-id="ce7e6-204">Zwiększenie tej wartości umożliwia serwerowi otrzymywanie większych komunikatów szybciej bez naciskania, ale może zwiększyć zużycie pamięci.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-204">Increasing this value allows the server to receive larger messages more quickly without applying backpressure, but can increase memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="ce7e6-205">Dane zbierane automatycznie z atrybutów `Authorize` zastosowanych do klasy centrum.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-205">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="ce7e6-206">Lista obiektów [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) służąca do określenia, czy klient jest autoryzowany do łączenia się z centrum.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-206">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="ce7e6-207">32 KB</span><span class="sxs-lookup"><span data-stu-id="ce7e6-207">32 KB</span></span> | <span data-ttu-id="ce7e6-208">Maksymalna liczba bajtów wysłanych przez aplikację, które buforuje serwer przed zaobserwowaniem presji.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-208">The maximum number of bytes sent by the app that the server buffers before observing backpressure.</span></span> <span data-ttu-id="ce7e6-209">Zwiększenie tej wartości umożliwia serwerowi przebuforowanie większych komunikatów szybciej bez czekania na obciążenie, ale może zwiększyć zużycie pamięci.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-209">Increasing this value allows the server to buffer larger messages more quickly without awaiting backpressure, but can increase memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="ce7e6-210">Wszystkie transporty są włączone.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-210">All Transports are enabled.</span></span> | <span data-ttu-id="ce7e6-211">Wyliczenia flag bitowych wartości `HttpTransportType`, które mogą ograniczyć transporty, których klient może użyć do nawiązania połączenia.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-211">A bit flags enum of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="ce7e6-212">Zobacz poniżej.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-212">See below.</span></span> | <span data-ttu-id="ce7e6-213">Dodatkowe opcje specyficzne dla długiego transportu sondowania.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-213">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="ce7e6-214">Zobacz poniżej.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-214">See below.</span></span> | <span data-ttu-id="ce7e6-215">Dodatkowe opcje specyficzne dla transportu obiektów WebSockets.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-215">Additional options specific to the WebSockets transport.</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-3.0"

| <span data-ttu-id="ce7e6-216">Opcja</span><span class="sxs-lookup"><span data-stu-id="ce7e6-216">Option</span></span> | <span data-ttu-id="ce7e6-217">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="ce7e6-217">Default Value</span></span> | <span data-ttu-id="ce7e6-218">Opis</span><span class="sxs-lookup"><span data-stu-id="ce7e6-218">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="ce7e6-219">32 KB</span><span class="sxs-lookup"><span data-stu-id="ce7e6-219">32 KB</span></span> | <span data-ttu-id="ce7e6-220">Maksymalna liczba bajtów odebranych od klienta, które buforuje serwer.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-220">The maximum number of bytes received from the client that the server buffers.</span></span> <span data-ttu-id="ce7e6-221">Zwiększenie tej wartości umożliwia serwerowi otrzymywanie większych komunikatów, ale może mieć negatywny wpływ na użycie pamięci.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-221">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="ce7e6-222">Dane zbierane automatycznie z atrybutów `Authorize` zastosowanych do klasy centrum.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-222">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="ce7e6-223">Lista obiektów [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) służąca do określenia, czy klient jest autoryzowany do łączenia się z centrum.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-223">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="ce7e6-224">32 KB</span><span class="sxs-lookup"><span data-stu-id="ce7e6-224">32 KB</span></span> | <span data-ttu-id="ce7e6-225">Maksymalna liczba bajtów wysłanych przez aplikację, które buforują serwer.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-225">The maximum number of bytes sent by the app that the server buffers.</span></span> <span data-ttu-id="ce7e6-226">Zwiększenie tej wartości umożliwia serwerowi wysyłanie większych komunikatów, ale może mieć negatywny wpływ na użycie pamięci.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-226">Increasing this value allows the server to send larger messages, but can negatively impact memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="ce7e6-227">Wszystkie transporty są włączone.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-227">All Transports are enabled.</span></span> | <span data-ttu-id="ce7e6-228">Wyliczenia flag bitowych wartości `HttpTransportType`, które mogą ograniczyć transporty, których klient może użyć do nawiązania połączenia.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-228">A bit flags enum of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="ce7e6-229">Zobacz poniżej.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-229">See below.</span></span> | <span data-ttu-id="ce7e6-230">Dodatkowe opcje specyficzne dla długiego transportu sondowania.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-230">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="ce7e6-231">Zobacz poniżej.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-231">See below.</span></span> | <span data-ttu-id="ce7e6-232">Dodatkowe opcje specyficzne dla transportu obiektów WebSockets.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-232">Additional options specific to the WebSockets transport.</span></span> |

::: moniker-end

<span data-ttu-id="ce7e6-233">W przypadku długiego transportu sondowania dostępne są dodatkowe opcje, które można skonfigurować za pomocą właściwości `LongPolling`:</span><span class="sxs-lookup"><span data-stu-id="ce7e6-233">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="ce7e6-234">Opcja</span><span class="sxs-lookup"><span data-stu-id="ce7e6-234">Option</span></span> | <span data-ttu-id="ce7e6-235">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="ce7e6-235">Default Value</span></span> | <span data-ttu-id="ce7e6-236">Opis</span><span class="sxs-lookup"><span data-stu-id="ce7e6-236">Description</span></span> |
| ------ | ------------- | ----------- |
| `PollTimeout` | <span data-ttu-id="ce7e6-237">90 sekund</span><span class="sxs-lookup"><span data-stu-id="ce7e6-237">90 seconds</span></span> | <span data-ttu-id="ce7e6-238">Maksymalny czas, przez jaki serwer czeka na wysłanie wiadomości do klienta przed zakończeniem jednego żądania sondowania.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-238">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="ce7e6-239">Zmniejszenie tej wartości powoduje, że klient wystawia nowe żądania sondy częściej.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-239">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> |

<span data-ttu-id="ce7e6-240">Transport WebSocket ma dodatkowe opcje, które można skonfigurować za pomocą właściwości `WebSockets`:</span><span class="sxs-lookup"><span data-stu-id="ce7e6-240">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="ce7e6-241">Opcja</span><span class="sxs-lookup"><span data-stu-id="ce7e6-241">Option</span></span> | <span data-ttu-id="ce7e6-242">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="ce7e6-242">Default Value</span></span> | <span data-ttu-id="ce7e6-243">Opis</span><span class="sxs-lookup"><span data-stu-id="ce7e6-243">Description</span></span> |
| ------ | ------------- | ----------- |
| `CloseTimeout` | <span data-ttu-id="ce7e6-244">5 sekund</span><span class="sxs-lookup"><span data-stu-id="ce7e6-244">5 seconds</span></span> | <span data-ttu-id="ce7e6-245">Gdy serwer zostanie zamknięty, jeśli klient nie zostanie zamknięty w tym okresie, połączenie zostanie przerwane.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-245">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | `null` | <span data-ttu-id="ce7e6-246">Delegat, którego można użyć do ustawienia nagłówka `Sec-WebSocket-Protocol` na wartość niestandardową.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-246">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="ce7e6-247">Delegat otrzymuje wartości żądane przez klienta jako dane wejściowe i oczekuje na zwrócenie żądanej wartości.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-247">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="ce7e6-248">Konfigurowanie opcji klienta</span><span class="sxs-lookup"><span data-stu-id="ce7e6-248">Configure client options</span></span>

<span data-ttu-id="ce7e6-249">Opcje klienta można skonfigurować dla typu `HubConnectionBuilder` (dostępnego w klientach .NET i JavaScript).</span><span class="sxs-lookup"><span data-stu-id="ce7e6-249">Client options can be configured on the `HubConnectionBuilder` type (available in the .NET and JavaScript clients).</span></span> <span data-ttu-id="ce7e6-250">Jest ona również dostępna w kliencie Java, ale podklasa `HttpHubConnectionBuilder` zawiera opcje konfiguracji konstruktora, a także `HubConnection`.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-250">It's also available in the Java client, but the `HttpHubConnectionBuilder` subclass is what contains the builder configuration options, as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="ce7e6-251">Konfigurowanie rejestrowania</span><span class="sxs-lookup"><span data-stu-id="ce7e6-251">Configure logging</span></span>

<span data-ttu-id="ce7e6-252">Rejestrowanie jest konfigurowane w kliencie .NET przy użyciu metody `ConfigureLogging`.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-252">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="ce7e6-253">Dostawcy i filtry rejestrowania mogą być rejestrowane w taki sam sposób, w jaki znajdują się na serwerze.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-253">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="ce7e6-254">Aby uzyskać więcej informacji, zobacz dokumentację dotyczącą [rejestrowania w ASP.NET Core](xref:fundamentals/logging/index) .</span><span class="sxs-lookup"><span data-stu-id="ce7e6-254">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="ce7e6-255">Aby zarejestrować dostawców rejestrowania, należy zainstalować wymagane pakiety.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-255">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="ce7e6-256">Aby zapoznać się z pełną listą, zobacz sekcję [wbudowane dostawcy rejestrowania](xref:fundamentals/logging/index#built-in-logging-providers) w witrynie docs.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-256">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="ce7e6-257">Aby na przykład włączyć rejestrowanie konsoli, zainstaluj pakiet NuGet `Microsoft.Extensions.Logging.Console`.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-257">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="ce7e6-258">Wywołaj metodę rozszerzenia `AddConsole`:</span><span class="sxs-lookup"><span data-stu-id="ce7e6-258">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="ce7e6-259">W kliencie JavaScript istnieje podobna Metoda `configureLogging`.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-259">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="ce7e6-260">Podaj wartość `LogLevel` wskazującą minimalny poziom komunikatów dziennika do wygenerowania.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-260">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="ce7e6-261">Dzienniki są zapisywane w oknie konsoli przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-261">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="ce7e6-262">Zamiast wartości `LogLevel` można także podać wartość `string` reprezentującą nazwę poziomu dziennika.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-262">Instead of a `LogLevel` value, you can also provide a `string` value representing a log level name.</span></span> <span data-ttu-id="ce7e6-263">Jest to przydatne podczas konfigurowania rejestrowania sygnałów w środowiskach, w których nie masz dostępu do stałych `LogLevel`.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-263">This is useful when configuring SignalR logging in environments where you don't have access to the `LogLevel` constants.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging("warn")
    .build();
```

<span data-ttu-id="ce7e6-264">Poniższa tabela zawiera listę dostępnych poziomów dzienników.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-264">The following table lists the available log levels.</span></span> <span data-ttu-id="ce7e6-265">Wartość `configureLogging` ustawia **minimalny** poziom rejestrowania, który zostanie zarejestrowany.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-265">The value you provide to `configureLogging` sets the **minimum** log level that will be logged.</span></span> <span data-ttu-id="ce7e6-266">Komunikaty zarejestrowane na tym poziomie **lub poziomy wymienione po nim w tabeli**zostaną zarejestrowane.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-266">Messages logged at this level, **or the levels listed after it in the table**, will be logged.</span></span>

| <span data-ttu-id="ce7e6-267">Ciąg</span><span class="sxs-lookup"><span data-stu-id="ce7e6-267">String</span></span>                      | <span data-ttu-id="ce7e6-268">logLevel</span><span class="sxs-lookup"><span data-stu-id="ce7e6-268">LogLevel</span></span>               |
| --------------------------- | ---------------------- |
| `trace`                     | `LogLevel.Trace`       |
| `debug`                     | `LogLevel.Debug`       |
| <span data-ttu-id="ce7e6-269">`info` **lub** `information`</span><span class="sxs-lookup"><span data-stu-id="ce7e6-269">`info` **or** `information`</span></span> | `LogLevel.Information` |
| <span data-ttu-id="ce7e6-270">`warn` **lub** `warning`</span><span class="sxs-lookup"><span data-stu-id="ce7e6-270">`warn` **or** `warning`</span></span>     | `LogLevel.Warning`     |
| `error`                     | `LogLevel.Error`       |
| `critical`                  | `LogLevel.Critical`    |
| `none`                      | `LogLevel.None`        |

::: moniker-end

> [!NOTE]
> <span data-ttu-id="ce7e6-271">Aby całkowicie wyłączyć rejestrowanie, określ `signalR.LogLevel.None` w metodzie `configureLogging`.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-271">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="ce7e6-272">Aby uzyskać więcej informacji na temat rejestrowania, zobacz [dokumentację diagnostyki sygnału](xref:signalr/diagnostics).</span><span class="sxs-lookup"><span data-stu-id="ce7e6-272">For more information on logging, see the [SignalR Diagnostics documentation](xref:signalr/diagnostics).</span></span>

<span data-ttu-id="ce7e6-273">Klient Java sygnalizujący używa biblioteki [SLF4J](https://www.slf4j.org/) do rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-273">The SignalR Java client uses the [SLF4J](https://www.slf4j.org/) library for logging.</span></span> <span data-ttu-id="ce7e6-274">Jest to interfejs API rejestrowania wysokiego poziomu, który umożliwia użytkownikom biblioteki wybranie własnych implementacji rejestrowania przez nałożenie określonej zależności rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-274">It's a high-level logging API that allows users of the library to chose their own specific logging implementation by bringing in a specific logging dependency.</span></span> <span data-ttu-id="ce7e6-275">Poniższy fragment kodu przedstawia sposób używania `java.util.logging` z klientem Java sygnalizującego.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-275">The following code snippet shows how to use `java.util.logging` with the SignalR Java client.</span></span>

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

<span data-ttu-id="ce7e6-276">Jeśli nie skonfigurujesz rejestrowania w zależnościach, SLF4J ładuje domyślny Rejestrator braku operacji z następującym komunikatem ostrzegawczym:</span><span class="sxs-lookup"><span data-stu-id="ce7e6-276">If you don't configure logging in your dependencies, SLF4J loads a default no-operation logger with the following warning message:</span></span>

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

<span data-ttu-id="ce7e6-277">Można je bezpiecznie zignorować.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-277">This can safely be ignored.</span></span>

### <a name="configure-allowed-transports"></a><span data-ttu-id="ce7e6-278">Skonfiguruj dozwolone transporty</span><span class="sxs-lookup"><span data-stu-id="ce7e6-278">Configure allowed transports</span></span>

<span data-ttu-id="ce7e6-279">Transporty używane przez sygnalizującego można skonfigurować w wywołaniu `WithUrl` (`withUrl` w języku JavaScript).</span><span class="sxs-lookup"><span data-stu-id="ce7e6-279">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="ce7e6-280">Wartości `HttpTransportType` można używać w celu ograniczenia liczby klientów do używania tylko określonych transportów.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-280">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="ce7e6-281">Wszystkie transporty są domyślnie włączone.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-281">All transports are enabled by default.</span></span>

<span data-ttu-id="ce7e6-282">Na przykład, aby wyłączyć transport zdarzeń wysłanych przez serwer, ale zezwolić na połączenia z usługą WebSockets i długie sondowanie:</span><span class="sxs-lookup"><span data-stu-id="ce7e6-282">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="ce7e6-283">W kliencie JavaScript transporty są konfigurowane przez ustawienie pola `transport` w obiekcie Options udostępnionym do `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="ce7e6-283">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="ce7e6-284">W tej wersji elementu WebSockets klienta Java jest jedynym dostępnym transportem.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-284">In this version of the Java client websockets is the only available transport.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-3.0"

<span data-ttu-id="ce7e6-285">W kliencie Java, transport jest wybierany z metodą `withTransport` na `HttpHubConnectionBuilder`.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-285">In the Java client, the transport is selected with the `withTransport` method on the `HttpHubConnectionBuilder`.</span></span> <span data-ttu-id="ce7e6-286">Klient Java domyślnie używa transportu usługi WebSockets.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-286">The Java client defaults to using the WebSockets transport.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withTransport(TransportEnum.WEBSOCKETS)
    .build();
```

> [!NOTE]
> <span data-ttu-id="ce7e6-287">Klient Java sygnalizujący nie obsługuje jeszcze rezerwy transportowej.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-287">The SignalR Java client doesn't support transport fallback yet.</span></span>

::: moniker-end

### <a name="configure-bearer-authentication"></a><span data-ttu-id="ce7e6-288">Konfigurowanie uwierzytelniania okaziciela</span><span class="sxs-lookup"><span data-stu-id="ce7e6-288">Configure bearer authentication</span></span>

<span data-ttu-id="ce7e6-289">Aby zapewnić dane uwierzytelniania wraz z żądaniami sygnałów, użyj opcji `AccessTokenProvider` (`accessTokenFactory` w języku JavaScript), aby określić funkcję, która zwraca żądany token dostępu.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-289">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="ce7e6-290">W kliencie .NET token dostępu jest przenoszona jako token "uwierzytelnianie okaziciela" protokołu HTTP (przy użyciu nagłówka `Authorization` z typem `Bearer`).</span><span class="sxs-lookup"><span data-stu-id="ce7e6-290">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="ce7e6-291">W kliencie JavaScript token dostępu jest używany jako token okaziciela, **z wyjątkiem** kilku przypadków, w których interfejsy API przeglądarki ograniczają możliwość zastosowania nagłówków (w konkretnym przypadku zdarzeń wysyłanych przez serwer i żądań WebSockets).</span><span class="sxs-lookup"><span data-stu-id="ce7e6-291">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="ce7e6-292">W takich przypadkach token dostępu jest podawany jako wartość ciągu zapytania `access_token`.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-292">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="ce7e6-293">W kliencie .NET opcja `AccessTokenProvider` może być określona za pomocą delegata opcji w `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="ce7e6-293">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

<span data-ttu-id="ce7e6-294">W kliencie JavaScript token dostępu jest konfigurowany przez ustawienie pola `accessTokenFactory` w obiekcie Options w `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="ce7e6-294">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

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

<span data-ttu-id="ce7e6-295">W kliencie Java programu sygnalizującego można skonfigurować token okaziciela do użycia na potrzeby uwierzytelniania, dostarczając fabrykę tokenów dostępu do [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span><span class="sxs-lookup"><span data-stu-id="ce7e6-295">In the SignalR Java client, you can configure a bearer token to use for authentication by providing an access token factory to the [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span></span> <span data-ttu-id="ce7e6-296">Użyj [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) , aby dostarczyć [RxJava](https://github.com/ReactiveX/RxJava) [pojedyncze @ no__t-> 3String](https://reactivex.io/documentation/single.html).</span><span class="sxs-lookup"><span data-stu-id="ce7e6-296">Use [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) to provide an [RxJava](https://github.com/ReactiveX/RxJava) [Single\<String>](https://reactivex.io/documentation/single.html).</span></span> <span data-ttu-id="ce7e6-297">Wywołanie metody [Single. Ustąp](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-)umożliwia zapisanie logiki w celu utworzenia tokenów dostępu dla klienta.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-297">With a call to [Single.defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), you can write logic to produce access tokens for your client.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="ce7e6-298">Konfigurowanie opcji limitu czasu i utrzymywania aktywności</span><span class="sxs-lookup"><span data-stu-id="ce7e6-298">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="ce7e6-299">W samym obiekcie `HubConnection` dostępne są dodatkowe opcje konfigurowania trybu przekroczenia limitu czasu i zachowania utrzymywania aktywności:</span><span class="sxs-lookup"><span data-stu-id="ce7e6-299">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>


::: moniker range=">= aspnetcore-2.2"

# <a name="nettabdotnet"></a>[<span data-ttu-id="ce7e6-300">.NET</span><span class="sxs-lookup"><span data-stu-id="ce7e6-300">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="ce7e6-301">Opcja</span><span class="sxs-lookup"><span data-stu-id="ce7e6-301">Option</span></span> | <span data-ttu-id="ce7e6-302">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="ce7e6-302">Default value</span></span> | <span data-ttu-id="ce7e6-303">Opis</span><span class="sxs-lookup"><span data-stu-id="ce7e6-303">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="ce7e6-304">30 sekund (30 000 MS)</span><span class="sxs-lookup"><span data-stu-id="ce7e6-304">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="ce7e6-305">Limit czasu dla działania serwera.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-305">Timeout for server activity.</span></span> <span data-ttu-id="ce7e6-306">Jeśli serwer nie wysłał komunikatu w tym interwale, Klient uważa, że serwer odłączył się i wyzwala zdarzenie `Closed` (`onclose` w języku JavaScript).</span><span class="sxs-lookup"><span data-stu-id="ce7e6-306">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="ce7e6-307">Ta wartość musi być wystarczająca do wysłania wiadomości ping z serwera **i** odebrana przez klienta w ramach przedziału czasu.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-307">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="ce7e6-308">Zalecana wartość to liczba równa co najmniej podwójnej wartości @no__t na serwerze, aby umożliwić czas na nadejście poleceń ping.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-308">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="ce7e6-309">15 sekund</span><span class="sxs-lookup"><span data-stu-id="ce7e6-309">15 seconds</span></span> | <span data-ttu-id="ce7e6-310">Limit czasu początkowej uzgadniania z serwerem.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-310">Timeout for initial server handshake.</span></span> <span data-ttu-id="ce7e6-311">Jeśli serwer nie wyśle odpowiedzi uzgadniania w tym interwale, klient anuluje uzgadnianie i wyzwala zdarzenie `Closed` (`onclose` w języku JavaScript).</span><span class="sxs-lookup"><span data-stu-id="ce7e6-311">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="ce7e6-312">Jest to ustawienie zaawansowane, które należy zmodyfikować tylko w przypadku wystąpienia błędów limitu czasu uzgadniania z powodu poważnego opóźnienia sieci.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-312">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="ce7e6-313">Aby uzyskać więcej szczegółów na temat procesu uzgadniania, zobacz [specyfikację protokołu centrum sygnału](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="ce7e6-313">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="ce7e6-314">15 sekund</span><span class="sxs-lookup"><span data-stu-id="ce7e6-314">15 seconds</span></span> | <span data-ttu-id="ce7e6-315">Określa interwał wysyłania komunikatów ping przez klienta.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-315">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="ce7e6-316">Wysłanie wszelkich komunikatów z klienta powoduje zresetowanie czasomierza do początku interwału.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-316">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="ce7e6-317">Jeśli klient nie wysłał komunikatu w `ClientTimeoutInterval` ustawionym na serwerze, serwer traktuje klienta jako odłączony.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-317">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

<span data-ttu-id="ce7e6-318">W kliencie .NET wartości limitu czasu są określane jako wartości `TimeSpan`.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-318">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="ce7e6-319">JavaScript</span><span class="sxs-lookup"><span data-stu-id="ce7e6-319">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="ce7e6-320">Opcja</span><span class="sxs-lookup"><span data-stu-id="ce7e6-320">Option</span></span> | <span data-ttu-id="ce7e6-321">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="ce7e6-321">Default value</span></span> | <span data-ttu-id="ce7e6-322">Opis</span><span class="sxs-lookup"><span data-stu-id="ce7e6-322">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="ce7e6-323">30 sekund (30 000 MS)</span><span class="sxs-lookup"><span data-stu-id="ce7e6-323">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="ce7e6-324">Limit czasu dla działania serwera.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-324">Timeout for server activity.</span></span> <span data-ttu-id="ce7e6-325">Jeśli serwer nie wysłał komunikatu w tym interwale, Klient uważa, że serwer odłączył się i wyzwala zdarzenie `onclose`.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-325">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="ce7e6-326">Ta wartość musi być wystarczająca do wysłania wiadomości ping z serwera **i** odebrana przez klienta w ramach przedziału czasu.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-326">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="ce7e6-327">Zalecana wartość to liczba równa co najmniej podwójnej wartości @no__t na serwerze, aby umożliwić czas na nadejście poleceń ping.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-327">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `keepAliveIntervalInMilliseconds` | <span data-ttu-id="ce7e6-328">15 sekund (15 000 milisekund)</span><span class="sxs-lookup"><span data-stu-id="ce7e6-328">15 seconds (15,000 miliseconds)</span></span> | <span data-ttu-id="ce7e6-329">Określa interwał wysyłania komunikatów ping przez klienta.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-329">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="ce7e6-330">Wysłanie wszelkich komunikatów z klienta powoduje zresetowanie czasomierza do początku interwału.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-330">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="ce7e6-331">Jeśli klient nie wysłał komunikatu w `ClientTimeoutInterval` ustawionym na serwerze, serwer traktuje klienta jako odłączony.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-331">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="ce7e6-332">Java</span><span class="sxs-lookup"><span data-stu-id="ce7e6-332">Java</span></span>](#tab/java)

| <span data-ttu-id="ce7e6-333">Opcja</span><span class="sxs-lookup"><span data-stu-id="ce7e6-333">Option</span></span> | <span data-ttu-id="ce7e6-334">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="ce7e6-334">Default value</span></span> | <span data-ttu-id="ce7e6-335">Opis</span><span class="sxs-lookup"><span data-stu-id="ce7e6-335">Description</span></span> |
| ------ | ------------- | ----------- |
| <span data-ttu-id="ce7e6-336">`getServerTimeout` / `setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="ce7e6-336">`getServerTimeout` / `setServerTimeout`</span></span> | <span data-ttu-id="ce7e6-337">30 sekund (30 000 MS)</span><span class="sxs-lookup"><span data-stu-id="ce7e6-337">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="ce7e6-338">Limit czasu dla działania serwera.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-338">Timeout for server activity.</span></span> <span data-ttu-id="ce7e6-339">Jeśli serwer nie wysłał komunikatu w tym interwale, Klient uważa, że serwer odłączył się i wyzwala zdarzenie `onClose`.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-339">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="ce7e6-340">Ta wartość musi być wystarczająca do wysłania wiadomości ping z serwera **i** odebrana przez klienta w ramach przedziału czasu.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-340">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="ce7e6-341">Zalecana wartość to liczba równa co najmniej podwójnej wartości @no__t na serwerze, aby umożliwić czas na nadejście poleceń ping.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-341">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="ce7e6-342">15 sekund</span><span class="sxs-lookup"><span data-stu-id="ce7e6-342">15 seconds</span></span> | <span data-ttu-id="ce7e6-343">Limit czasu początkowej uzgadniania z serwerem.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-343">Timeout for initial server handshake.</span></span> <span data-ttu-id="ce7e6-344">Jeśli serwer nie wyśle odpowiedzi uzgadniania w tym interwale, klient anuluje uzgadnianie i wyzwala zdarzenie `onClose`.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-344">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="ce7e6-345">Jest to ustawienie zaawansowane, które należy zmodyfikować tylko w przypadku wystąpienia błędów limitu czasu uzgadniania z powodu poważnego opóźnienia sieci.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-345">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="ce7e6-346">Aby uzyskać więcej szczegółów na temat procesu uzgadniania, zobacz [specyfikację protokołu centrum sygnału](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="ce7e6-346">For more detail on the Handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| <span data-ttu-id="ce7e6-347">`getKeepAliveInterval` / `setKeepAliveInterval`</span><span class="sxs-lookup"><span data-stu-id="ce7e6-347">`getKeepAliveInterval` / `setKeepAliveInterval`</span></span> | <span data-ttu-id="ce7e6-348">15 sekund (15 000 MS)</span><span class="sxs-lookup"><span data-stu-id="ce7e6-348">15 seconds (15,000 milliseconds)</span></span> | <span data-ttu-id="ce7e6-349">Określa interwał wysyłania komunikatów ping przez klienta.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-349">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="ce7e6-350">Wysłanie wszelkich komunikatów z klienta powoduje zresetowanie czasomierza do początku interwału.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-350">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="ce7e6-351">Jeśli klient nie wysłał komunikatu w `ClientTimeoutInterval` ustawionym na serwerze, serwer traktuje klienta jako odłączony.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-351">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="nettabdotnet"></a>[<span data-ttu-id="ce7e6-352">.NET</span><span class="sxs-lookup"><span data-stu-id="ce7e6-352">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="ce7e6-353">Opcja</span><span class="sxs-lookup"><span data-stu-id="ce7e6-353">Option</span></span> | <span data-ttu-id="ce7e6-354">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="ce7e6-354">Default value</span></span> | <span data-ttu-id="ce7e6-355">Opis</span><span class="sxs-lookup"><span data-stu-id="ce7e6-355">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="ce7e6-356">30 sekund (30 000 MS)</span><span class="sxs-lookup"><span data-stu-id="ce7e6-356">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="ce7e6-357">Limit czasu dla działania serwera.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-357">Timeout for server activity.</span></span> <span data-ttu-id="ce7e6-358">Jeśli serwer nie wysłał komunikatu w tym interwale, Klient uważa, że serwer odłączył się i wyzwala zdarzenie `Closed` (`onclose` w języku JavaScript).</span><span class="sxs-lookup"><span data-stu-id="ce7e6-358">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="ce7e6-359">Ta wartość musi być wystarczająca do wysłania wiadomości ping z serwera **i** odebrana przez klienta w ramach przedziału czasu.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-359">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="ce7e6-360">Zalecana wartość to liczba równa co najmniej podwójnej wartości @no__t na serwerze, aby umożliwić czas na nadejście poleceń ping.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-360">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="ce7e6-361">15 sekund</span><span class="sxs-lookup"><span data-stu-id="ce7e6-361">15 seconds</span></span> | <span data-ttu-id="ce7e6-362">Limit czasu początkowej uzgadniania z serwerem.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-362">Timeout for initial server handshake.</span></span> <span data-ttu-id="ce7e6-363">Jeśli serwer nie wyśle odpowiedzi uzgadniania w tym interwale, klient anuluje uzgadnianie i wyzwala zdarzenie `Closed` (`onclose` w języku JavaScript).</span><span class="sxs-lookup"><span data-stu-id="ce7e6-363">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="ce7e6-364">Jest to ustawienie zaawansowane, które należy zmodyfikować tylko w przypadku wystąpienia błędów limitu czasu uzgadniania z powodu poważnego opóźnienia sieci.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-364">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="ce7e6-365">Aby uzyskać więcej szczegółów na temat procesu uzgadniania, zobacz [specyfikację protokołu centrum sygnału](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="ce7e6-365">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

<span data-ttu-id="ce7e6-366">W kliencie .NET wartości limitu czasu są określane jako wartości `TimeSpan`.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-366">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="ce7e6-367">JavaScript</span><span class="sxs-lookup"><span data-stu-id="ce7e6-367">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="ce7e6-368">Opcja</span><span class="sxs-lookup"><span data-stu-id="ce7e6-368">Option</span></span> | <span data-ttu-id="ce7e6-369">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="ce7e6-369">Default value</span></span> | <span data-ttu-id="ce7e6-370">Opis</span><span class="sxs-lookup"><span data-stu-id="ce7e6-370">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="ce7e6-371">30 sekund (30 000 MS)</span><span class="sxs-lookup"><span data-stu-id="ce7e6-371">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="ce7e6-372">Limit czasu dla działania serwera.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-372">Timeout for server activity.</span></span> <span data-ttu-id="ce7e6-373">Jeśli serwer nie wysłał komunikatu w tym interwale, Klient uważa, że serwer odłączył się i wyzwala zdarzenie `onclose`.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-373">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="ce7e6-374">Ta wartość musi być wystarczająca do wysłania wiadomości ping z serwera **i** odebrana przez klienta w ramach przedziału czasu.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-374">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="ce7e6-375">Zalecana wartość to liczba równa co najmniej podwójnej wartości @no__t na serwerze, aby umożliwić czas na nadejście poleceń ping.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-375">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="ce7e6-376">Java</span><span class="sxs-lookup"><span data-stu-id="ce7e6-376">Java</span></span>](#tab/java)

| <span data-ttu-id="ce7e6-377">Opcja</span><span class="sxs-lookup"><span data-stu-id="ce7e6-377">Option</span></span> | <span data-ttu-id="ce7e6-378">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="ce7e6-378">Default value</span></span> | <span data-ttu-id="ce7e6-379">Opis</span><span class="sxs-lookup"><span data-stu-id="ce7e6-379">Description</span></span> |
| ------ | ------------- | ----------- |
| <span data-ttu-id="ce7e6-380">`getServerTimeout` / `setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="ce7e6-380">`getServerTimeout` / `setServerTimeout`</span></span> | <span data-ttu-id="ce7e6-381">30 sekund (30 000 MS)</span><span class="sxs-lookup"><span data-stu-id="ce7e6-381">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="ce7e6-382">Limit czasu dla działania serwera.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-382">Timeout for server activity.</span></span> <span data-ttu-id="ce7e6-383">Jeśli serwer nie wysłał komunikatu w tym interwale, Klient uważa, że serwer odłączył się i wyzwala zdarzenie `onClose`.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-383">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="ce7e6-384">Ta wartość musi być wystarczająca do wysłania wiadomości ping z serwera **i** odebrana przez klienta w ramach przedziału czasu.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-384">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="ce7e6-385">Zalecana wartość to liczba równa co najmniej podwójnej wartości `KeepAliveInterval` serwera, aby umożliwić czas na nadejście poleceń ping.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-385">The recommended value is a number at least double the server's `KeepAliveInterval` value, to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="ce7e6-386">15 sekund</span><span class="sxs-lookup"><span data-stu-id="ce7e6-386">15 seconds</span></span> | <span data-ttu-id="ce7e6-387">Limit czasu początkowej uzgadniania z serwerem.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-387">Timeout for initial server handshake.</span></span> <span data-ttu-id="ce7e6-388">Jeśli serwer nie wyśle odpowiedzi uzgadniania w tym interwale, klient anuluje uzgadnianie i wyzwala zdarzenie `onClose`.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-388">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="ce7e6-389">Jest to ustawienie zaawansowane, które należy zmodyfikować tylko w przypadku wystąpienia błędów limitu czasu uzgadniania z powodu poważnego opóźnienia sieci.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-389">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="ce7e6-390">Aby uzyskać więcej szczegółów na temat procesu uzgadniania, zobacz [specyfikację protokołu centrum sygnału](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="ce7e6-390">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

::: moniker-end

---

### <a name="configure-additional-options"></a><span data-ttu-id="ce7e6-391">Skonfiguruj dodatkowe opcje</span><span class="sxs-lookup"><span data-stu-id="ce7e6-391">Configure additional options</span></span>

<span data-ttu-id="ce7e6-392">Dodatkowe opcje można skonfigurować w metodzie `WithUrl` (`withUrl` w języku JavaScript) na `HubConnectionBuilder` lub w różnych interfejsach API konfiguracji na `HttpHubConnectionBuilder` w kliencie Java:</span><span class="sxs-lookup"><span data-stu-id="ce7e6-392">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder` or on the various configuration APIs on the `HttpHubConnectionBuilder` in the Java client:</span></span>

# <a name="nettabdotnet"></a>[<span data-ttu-id="ce7e6-393">.NET</span><span class="sxs-lookup"><span data-stu-id="ce7e6-393">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="ce7e6-394">Opcja .NET</span><span class="sxs-lookup"><span data-stu-id="ce7e6-394">.NET Option</span></span> |  <span data-ttu-id="ce7e6-395">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="ce7e6-395">Default value</span></span> | <span data-ttu-id="ce7e6-396">Opis</span><span class="sxs-lookup"><span data-stu-id="ce7e6-396">Description</span></span> |
| ----------- | -------------- | ----------- |
| `AccessTokenProvider` | `null` | <span data-ttu-id="ce7e6-397">Funkcja zwracająca ciąg, który jest dostarczany jako token uwierzytelniania okaziciela w żądaniach HTTP.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-397">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `false` | <span data-ttu-id="ce7e6-398">Ustaw tę wartość na `true`, aby pominąć etap negocjacji.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-398">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="ce7e6-399">**Obsługiwane tylko wtedy, gdy transport Sockets jest jedynym włączonym transportem**.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-399">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="ce7e6-400">Nie można włączyć tego ustawienia w przypadku korzystania z usługi Azure Signal Service.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-400">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="ce7e6-401">Pusty</span><span class="sxs-lookup"><span data-stu-id="ce7e6-401">Empty</span></span> | <span data-ttu-id="ce7e6-402">Kolekcja certyfikatów TLS do wysłania w celu uwierzytelniania żądań.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-402">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="ce7e6-403">Pusty</span><span class="sxs-lookup"><span data-stu-id="ce7e6-403">Empty</span></span> | <span data-ttu-id="ce7e6-404">Kolekcja plików cookie protokołu HTTP do wysłania w każdym żądaniu HTTP.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-404">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="ce7e6-405">Pusty</span><span class="sxs-lookup"><span data-stu-id="ce7e6-405">Empty</span></span> | <span data-ttu-id="ce7e6-406">Poświadczenia do wysyłania przy każdym żądaniu HTTP.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-406">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="ce7e6-407">5 sekund</span><span class="sxs-lookup"><span data-stu-id="ce7e6-407">5 seconds</span></span> | <span data-ttu-id="ce7e6-408">Tylko obiekty WebSockets.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-408">WebSockets only.</span></span> <span data-ttu-id="ce7e6-409">Maksymalny czas oczekiwania klienta po zamknięciu serwera w celu potwierdzenia żądania zamknięcia.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-409">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="ce7e6-410">Jeśli serwer nie potwierdzi zamknięcia w tym czasie, klient zostanie rozłączony.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-410">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="ce7e6-411">Pusty</span><span class="sxs-lookup"><span data-stu-id="ce7e6-411">Empty</span></span> | <span data-ttu-id="ce7e6-412">Mapa dodatkowych nagłówków HTTP do wysłania w każdym żądaniu HTTP.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-412">A Map of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | `null` | <span data-ttu-id="ce7e6-413">Delegat, który może służyć do konfigurowania lub zastępowania `HttpMessageHandler` używanych do wysyłania żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-413">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="ce7e6-414">Nieużywane na potrzeby połączeń protokołu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-414">Not used for WebSocket connections.</span></span> <span data-ttu-id="ce7e6-415">Ten delegat musi zwracać wartość różną od null i otrzymuje wartość domyślną jako parametr.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-415">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="ce7e6-416">Zmodyfikuj ustawienia tej wartości domyślnej i zwróć ją lub Zwróć nowe wystąpienie `HttpMessageHandler`.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-416">Either modify settings on that default value and return it, or return a new `HttpMessageHandler` instance.</span></span> <span data-ttu-id="ce7e6-417">**Podczas zastępowania programu obsługi należy skopiować ustawienia, które mają być zachowane z podanej procedury obsługi. w przeciwnym razie skonfigurowane opcje (takie jak pliki cookie i nagłówki) nie będą miały zastosowania do nowego programu obsługi.**</span><span class="sxs-lookup"><span data-stu-id="ce7e6-417">**When replacing the handler make sure to copy the settings you want to keep from the provided handler, otherwise, the configured options (such as Cookies and Headers) won't apply to the new handler.**</span></span> |
| `Proxy` | `null` | <span data-ttu-id="ce7e6-418">Serwer proxy HTTP, który ma być używany podczas wysyłania żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-418">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | `false` | <span data-ttu-id="ce7e6-419">Ustaw tę wartość logiczną, aby wysyłać domyślne poświadczenia dla żądań HTTP i WebSockets.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-419">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="ce7e6-420">Umożliwia to korzystanie z uwierzytelniania systemu Windows.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-420">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | `null` | <span data-ttu-id="ce7e6-421">Delegat, który może służyć do konfigurowania dodatkowych opcji protokołu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-421">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="ce7e6-422">Odbiera wystąpienie elementu [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) , którego można użyć do skonfigurowania opcji.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-422">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="ce7e6-423">JavaScript</span><span class="sxs-lookup"><span data-stu-id="ce7e6-423">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="ce7e6-424">Opcja języka JavaScript</span><span class="sxs-lookup"><span data-stu-id="ce7e6-424">JavaScript Option</span></span> | <span data-ttu-id="ce7e6-425">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="ce7e6-425">Default Value</span></span> | <span data-ttu-id="ce7e6-426">Opis</span><span class="sxs-lookup"><span data-stu-id="ce7e6-426">Description</span></span> |
| ----------------- | ------------- | ----------- |
| `accessTokenFactory` | `null` | <span data-ttu-id="ce7e6-427">Funkcja zwracająca ciąg, który jest dostarczany jako token uwierzytelniania okaziciela w żądaniach HTTP.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-427">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `skipNegotiation` | `false` | <span data-ttu-id="ce7e6-428">Ustaw tę wartość na `true`, aby pominąć etap negocjacji.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-428">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="ce7e6-429">**Obsługiwane tylko wtedy, gdy transport Sockets jest jedynym włączonym transportem**.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-429">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="ce7e6-430">Nie można włączyć tego ustawienia w przypadku korzystania z usługi Azure Signal Service.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-430">This setting can't be enabled when using the Azure SignalR Service.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="ce7e6-431">Java</span><span class="sxs-lookup"><span data-stu-id="ce7e6-431">Java</span></span>](#tab/java)

| <span data-ttu-id="ce7e6-432">Opcja Java</span><span class="sxs-lookup"><span data-stu-id="ce7e6-432">Java Option</span></span> | <span data-ttu-id="ce7e6-433">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="ce7e6-433">Default Value</span></span> | <span data-ttu-id="ce7e6-434">Opis</span><span class="sxs-lookup"><span data-stu-id="ce7e6-434">Description</span></span> |
| ----------- | ------------- | ----------- |
| `withAccessTokenProvider` | `null` | <span data-ttu-id="ce7e6-435">Funkcja zwracająca ciąg, który jest dostarczany jako token uwierzytelniania okaziciela w żądaniach HTTP.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-435">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `shouldSkipNegotiate` | `false` | <span data-ttu-id="ce7e6-436">Ustaw tę wartość na `true`, aby pominąć etap negocjacji.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-436">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="ce7e6-437">**Obsługiwane tylko wtedy, gdy transport Sockets jest jedynym włączonym transportem**.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-437">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="ce7e6-438">Nie można włączyć tego ustawienia w przypadku korzystania z usługi Azure Signal Service.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-438">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| <span data-ttu-id="ce7e6-439">`withHeader` `withHeaders`</span><span class="sxs-lookup"><span data-stu-id="ce7e6-439">`withHeader` `withHeaders`</span></span> | <span data-ttu-id="ce7e6-440">Pusty</span><span class="sxs-lookup"><span data-stu-id="ce7e6-440">Empty</span></span> | <span data-ttu-id="ce7e6-441">Mapa dodatkowych nagłówków HTTP do wysłania w każdym żądaniu HTTP.</span><span class="sxs-lookup"><span data-stu-id="ce7e6-441">A Map of additional HTTP headers to send with every HTTP request.</span></span> |

---

<span data-ttu-id="ce7e6-442">W kliencie .NET opcje te mogą być modyfikowane przez delegata opcji udostępnionych dla `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="ce7e6-442">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="ce7e6-443">W kliencie JavaScript te opcje można podać w obiekcie JavaScript dostarczonym do `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="ce7e6-443">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

<span data-ttu-id="ce7e6-444">W kliencie Java Opcje te można skonfigurować przy użyciu metod w `HttpHubConnectionBuilder` zwracanych z `HubConnectionBuilder.create("HUB URL")`</span><span class="sxs-lookup"><span data-stu-id="ce7e6-444">In the Java client, these options can be configured with the methods on the `HttpHubConnectionBuilder` returned from the `HubConnectionBuilder.create("HUB URL")`</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
        .withHeader("Foo", "Bar")
        .shouldSkipNegotiate(true)
        .withHandshakeResponseTimeout(30*1000)
        .build();
```

## <a name="additional-resources"></a><span data-ttu-id="ce7e6-445">Zasoby dodatkowe</span><span class="sxs-lookup"><span data-stu-id="ce7e6-445">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>
