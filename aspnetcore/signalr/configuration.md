---
title: Konfiguracja Core SignalR platformy ASP.NET
author: bradygaster
description: Informacje o sposobie konfigurowania aplikacji ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 06/03/2019
uid: signalr/configuration
ms.openlocfilehash: 8c9fcaecb04555718f5da6a42a8e56c258e795af
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67813453"
---
# <a name="aspnet-core-signalr-configuration"></a><span data-ttu-id="74dee-103">Konfiguracja Core SignalR platformy ASP.NET</span><span class="sxs-lookup"><span data-stu-id="74dee-103">ASP.NET Core SignalR configuration</span></span>

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="74dee-104">Opcje serializacji JSON/MessagePack</span><span class="sxs-lookup"><span data-stu-id="74dee-104">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="74dee-105">SignalR platformy ASP.NET Core obsługuje dwa protokoły, kodowania wiadomości: [JSON](https://www.json.org/) i [MessagePack](https://msgpack.org/index.html).</span><span class="sxs-lookup"><span data-stu-id="74dee-105">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="74dee-106">Dla każdego protokołu zawiera opcje konfiguracji serializacji.</span><span class="sxs-lookup"><span data-stu-id="74dee-106">Each protocol has serialization configuration options.</span></span>

<span data-ttu-id="74dee-107">Serializacja kodu JSON można skonfigurować na serwerze za pomocą [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) metodę rozszerzenia, które mogą być dodawane po [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) w swojej `Startup.ConfigureServices` metody.</span><span class="sxs-lookup"><span data-stu-id="74dee-107">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method, which can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in your `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="74dee-108">`AddJsonProtocol` Metoda przyjmuje obiekt delegowany, który odbiera `options` obiektu.</span><span class="sxs-lookup"><span data-stu-id="74dee-108">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="74dee-109">[PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) właściwość obiektu, na którym jest na składnik JSON.NET `JsonSerializerSettings` obiektu, który może służyć do konfigurowania serializacji argumenty i zwracać wartości.</span><span class="sxs-lookup"><span data-stu-id="74dee-109">The [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) property on that object is a JSON.NET `JsonSerializerSettings` object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="74dee-110">Zobacz [dokumentacji na składnik JSON.NET](https://www.newtonsoft.com/json/help/html/Introduction.htm) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="74dee-110">See the [JSON.NET Documentation](https://www.newtonsoft.com/json/help/html/Introduction.htm) for more details.</span></span>

<span data-ttu-id="74dee-111">Na przykład aby skonfigurować serializator do użycia nazwy właściwości "PascalCase", zamiast domyślnej nazwy "camelCase", użyj poniższego kodu:</span><span class="sxs-lookup"><span data-stu-id="74dee-111">As an example, to configure the serializer to use "PascalCase" property names, instead of the default "camelCase" names, use the following code:</span></span>

```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver =
        new DefaultContractResolver();
    });
```

<span data-ttu-id="74dee-112">W kliencie programu .NET, takie same `AddJsonProtocol` — metoda rozszerzenia istnieje na [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="74dee-112">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="74dee-113">`Microsoft.Extensions.DependencyInjection` Przestrzeni nazw, należy zaimportować resolve — metoda rozszerzenia:</span><span class="sxs-lookup"><span data-stu-id="74dee-113">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

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
> <span data-ttu-id="74dee-114">Nie jest możliwe do skonfigurowania serializacji JSON w kliencie JavaScript w tej chwili.</span><span class="sxs-lookup"><span data-stu-id="74dee-114">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="74dee-115">Opcje serializacji MessagePack</span><span class="sxs-lookup"><span data-stu-id="74dee-115">MessagePack serialization options</span></span>

<span data-ttu-id="74dee-116">Serializacja MessagePack można skonfigurować poprzez dostarczenie delegata do [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) wywołania.</span><span class="sxs-lookup"><span data-stu-id="74dee-116">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="74dee-117">Zobacz [MessagePack w SignalR](xref:signalr/messagepackhubprotocol) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="74dee-117">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="74dee-118">Nie jest możliwe do skonfigurowania MessagePack serializacji w kliencie JavaScript w tej chwili.</span><span class="sxs-lookup"><span data-stu-id="74dee-118">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="74dee-119">Konfigurowanie opcji serwera</span><span class="sxs-lookup"><span data-stu-id="74dee-119">Configure server options</span></span>

<span data-ttu-id="74dee-120">W poniższej tabeli opisano opcje dotyczące konfigurowania centrów SignalR:</span><span class="sxs-lookup"><span data-stu-id="74dee-120">The following table describes options for configuring SignalR hubs:</span></span>

::: moniker range=">= aspnetcore-3.0"

| <span data-ttu-id="74dee-121">Opcja</span><span class="sxs-lookup"><span data-stu-id="74dee-121">Option</span></span> | <span data-ttu-id="74dee-122">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="74dee-122">Default Value</span></span> | <span data-ttu-id="74dee-123">Opis</span><span class="sxs-lookup"><span data-stu-id="74dee-123">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="74dee-124">30 sekund</span><span class="sxs-lookup"><span data-stu-id="74dee-124">30 seconds</span></span> | <span data-ttu-id="74dee-125">Serwer będzie należy wziąć pod uwagę klient odłączony, jeśli go nie odebrał komunikat (w tym keep-alive) w tym zakresie.</span><span class="sxs-lookup"><span data-stu-id="74dee-125">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="74dee-126">Może potrwać dłużej niż ten interwał limitu czasu dla klienta faktycznie był oznaczony jako rozłączona, ze względu na to implementacji.</span><span class="sxs-lookup"><span data-stu-id="74dee-126">It could take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="74dee-127">Zalecana wartość to double `KeepAliveInterval` wartość.</span><span class="sxs-lookup"><span data-stu-id="74dee-127">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="74dee-128">15 sekund</span><span class="sxs-lookup"><span data-stu-id="74dee-128">15 seconds</span></span> | <span data-ttu-id="74dee-129">Jeśli klient nie wysyła komunikat uzgadniania połączenia początkowego, w tym przedziale czasu, połączenie jest zamknięte.</span><span class="sxs-lookup"><span data-stu-id="74dee-129">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="74dee-130">To ustawienie Zaawansowane, które powinny być modyfikowane tylko, jeśli występują błędy przekroczenia limitu czasu uzgadnianie ze względu na opóźnienie sieci poważne.</span><span class="sxs-lookup"><span data-stu-id="74dee-130">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="74dee-131">Aby uzyskać więcej szczegółów na temat procesu uzgadniania, zobacz [specyfikacji protokołu Centrum SignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="74dee-131">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="74dee-132">15 sekund</span><span class="sxs-lookup"><span data-stu-id="74dee-132">15 seconds</span></span> | <span data-ttu-id="74dee-133">Jeśli serwer nie wysłał wiadomości, w tym przedziale czasu, wiadomość ping są wysyłane automatycznie do utrzymanie otwartego połączenia.</span><span class="sxs-lookup"><span data-stu-id="74dee-133">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="74dee-134">Po zmianie `KeepAliveInterval`, zmień `ServerTimeout` / `serverTimeoutInMilliseconds` ustawienie na komputerze klienckim.</span><span class="sxs-lookup"><span data-stu-id="74dee-134">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="74dee-135">Zalecanym `ServerTimeout` / `serverTimeoutInMilliseconds` wartość double `KeepAliveInterval` wartość.</span><span class="sxs-lookup"><span data-stu-id="74dee-135">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="74dee-136">Wszystkie zainstalowane protokołów</span><span class="sxs-lookup"><span data-stu-id="74dee-136">All installed protocols</span></span> | <span data-ttu-id="74dee-137">Protokoły obsługiwane przez tego koncentratora.</span><span class="sxs-lookup"><span data-stu-id="74dee-137">Protocols supported by this hub.</span></span> <span data-ttu-id="74dee-138">Domyślnie są dozwolone wszystkie protokoły zarejestrowany na serwerze, ale protokoły mogą być usunięte z tej listy, aby wyłączyć określone protokoły dla poszczególnych centrów.</span><span class="sxs-lookup"><span data-stu-id="74dee-138">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="74dee-139">Jeśli `true`, szczegółowe komunikaty o wyjątkach są zwracane do klientów, gdy wyjątek jest zgłaszany w przypadku metody koncentratora.</span><span class="sxs-lookup"><span data-stu-id="74dee-139">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="74dee-140">Wartość domyślna to `false`, jak te komunikaty o wyjątkach mogą zawierać poufne informacje.</span><span class="sxs-lookup"><span data-stu-id="74dee-140">The default is `false`, as these exception messages can contain sensitive information.</span></span> |
| `StreamBufferCapacity` | `10` | <span data-ttu-id="74dee-141">Maksymalna liczba elementów, które mogą być buforowane dla klienta przekazać strumieni.</span><span class="sxs-lookup"><span data-stu-id="74dee-141">The maximum number of items that can be buffered for client upload streams.</span></span> <span data-ttu-id="74dee-142">Po osiągnięciu tego limitu przetwarzania wywołań jest zablokowany, dopóki serwer przetwarza elementy strumienia.</span><span class="sxs-lookup"><span data-stu-id="74dee-142">If this limit is reached, the processing of invocations is blocked until the server processes stream items.</span></span>|

::: moniker-end

::: moniker range="= aspnetcore-2.2"

| <span data-ttu-id="74dee-143">Opcja</span><span class="sxs-lookup"><span data-stu-id="74dee-143">Option</span></span> | <span data-ttu-id="74dee-144">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="74dee-144">Default Value</span></span> | <span data-ttu-id="74dee-145">Opis</span><span class="sxs-lookup"><span data-stu-id="74dee-145">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="74dee-146">30 sekund</span><span class="sxs-lookup"><span data-stu-id="74dee-146">30 seconds</span></span> | <span data-ttu-id="74dee-147">Serwer będzie należy wziąć pod uwagę klient odłączony, jeśli go nie odebrał komunikat (w tym keep-alive) w tym zakresie.</span><span class="sxs-lookup"><span data-stu-id="74dee-147">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="74dee-148">Może potrwać dłużej niż ten interwał limitu czasu dla klienta faktycznie był oznaczony jako rozłączona, ze względu na to implementacji.</span><span class="sxs-lookup"><span data-stu-id="74dee-148">It could take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="74dee-149">Zalecana wartość to double `KeepAliveInterval` wartość.</span><span class="sxs-lookup"><span data-stu-id="74dee-149">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="74dee-150">15 sekund</span><span class="sxs-lookup"><span data-stu-id="74dee-150">15 seconds</span></span> | <span data-ttu-id="74dee-151">Jeśli klient nie wysyła komunikat uzgadniania połączenia początkowego, w tym przedziale czasu, połączenie jest zamknięte.</span><span class="sxs-lookup"><span data-stu-id="74dee-151">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="74dee-152">To ustawienie Zaawansowane, które powinny być modyfikowane tylko, jeśli występują błędy przekroczenia limitu czasu uzgadnianie ze względu na opóźnienie sieci poważne.</span><span class="sxs-lookup"><span data-stu-id="74dee-152">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="74dee-153">Aby uzyskać więcej szczegółów na temat procesu uzgadniania, zobacz [specyfikacji protokołu Centrum SignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="74dee-153">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="74dee-154">15 sekund</span><span class="sxs-lookup"><span data-stu-id="74dee-154">15 seconds</span></span> | <span data-ttu-id="74dee-155">Jeśli serwer nie wysłał wiadomości, w tym przedziale czasu, wiadomość ping są wysyłane automatycznie do utrzymanie otwartego połączenia.</span><span class="sxs-lookup"><span data-stu-id="74dee-155">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="74dee-156">Po zmianie `KeepAliveInterval`, zmień `ServerTimeout` / `serverTimeoutInMilliseconds` ustawienie na komputerze klienckim.</span><span class="sxs-lookup"><span data-stu-id="74dee-156">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="74dee-157">Zalecanym `ServerTimeout` / `serverTimeoutInMilliseconds` wartość double `KeepAliveInterval` wartość.</span><span class="sxs-lookup"><span data-stu-id="74dee-157">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="74dee-158">Wszystkie zainstalowane protokołów</span><span class="sxs-lookup"><span data-stu-id="74dee-158">All installed protocols</span></span> | <span data-ttu-id="74dee-159">Protokoły obsługiwane przez tego koncentratora.</span><span class="sxs-lookup"><span data-stu-id="74dee-159">Protocols supported by this hub.</span></span> <span data-ttu-id="74dee-160">Domyślnie są dozwolone wszystkie protokoły zarejestrowany na serwerze, ale protokoły mogą być usunięte z tej listy, aby wyłączyć określone protokoły dla poszczególnych centrów.</span><span class="sxs-lookup"><span data-stu-id="74dee-160">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="74dee-161">Jeśli `true`, szczegółowe komunikaty o wyjątkach są zwracane do klientów, gdy wyjątek jest zgłaszany w przypadku metody koncentratora.</span><span class="sxs-lookup"><span data-stu-id="74dee-161">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="74dee-162">Wartość domyślna to `false`, jak te komunikaty o wyjątkach mogą zawierać poufne informacje.</span><span class="sxs-lookup"><span data-stu-id="74dee-162">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-2.1"

| <span data-ttu-id="74dee-163">Opcja</span><span class="sxs-lookup"><span data-stu-id="74dee-163">Option</span></span> | <span data-ttu-id="74dee-164">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="74dee-164">Default Value</span></span> | <span data-ttu-id="74dee-165">Opis</span><span class="sxs-lookup"><span data-stu-id="74dee-165">Description</span></span> |
| ------ | ------------- | ----------- |
| `HandshakeTimeout` | <span data-ttu-id="74dee-166">15 sekund</span><span class="sxs-lookup"><span data-stu-id="74dee-166">15 seconds</span></span> | <span data-ttu-id="74dee-167">Jeśli klient nie wysyła komunikat uzgadniania połączenia początkowego, w tym przedziale czasu, połączenie jest zamknięte.</span><span class="sxs-lookup"><span data-stu-id="74dee-167">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="74dee-168">To ustawienie Zaawansowane, które powinny być modyfikowane tylko, jeśli występują błędy przekroczenia limitu czasu uzgadnianie ze względu na opóźnienie sieci poważne.</span><span class="sxs-lookup"><span data-stu-id="74dee-168">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="74dee-169">Aby uzyskać więcej szczegółów na temat procesu uzgadniania, zobacz [specyfikacji protokołu Centrum SignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="74dee-169">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="74dee-170">15 sekund</span><span class="sxs-lookup"><span data-stu-id="74dee-170">15 seconds</span></span> | <span data-ttu-id="74dee-171">Jeśli serwer nie wysłał wiadomości, w tym przedziale czasu, wiadomość ping są wysyłane automatycznie do utrzymanie otwartego połączenia.</span><span class="sxs-lookup"><span data-stu-id="74dee-171">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="74dee-172">Po zmianie `KeepAliveInterval`, zmień `ServerTimeout` / `serverTimeoutInMilliseconds` ustawienie na komputerze klienckim.</span><span class="sxs-lookup"><span data-stu-id="74dee-172">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="74dee-173">Zalecanym `ServerTimeout` / `serverTimeoutInMilliseconds` wartość double `KeepAliveInterval` wartość.</span><span class="sxs-lookup"><span data-stu-id="74dee-173">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="74dee-174">Wszystkie zainstalowane protokołów</span><span class="sxs-lookup"><span data-stu-id="74dee-174">All installed protocols</span></span> | <span data-ttu-id="74dee-175">Protokoły obsługiwane przez tego koncentratora.</span><span class="sxs-lookup"><span data-stu-id="74dee-175">Protocols supported by this hub.</span></span> <span data-ttu-id="74dee-176">Domyślnie są dozwolone wszystkie protokoły zarejestrowany na serwerze, ale protokoły mogą być usunięte z tej listy, aby wyłączyć określone protokoły dla poszczególnych centrów.</span><span class="sxs-lookup"><span data-stu-id="74dee-176">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="74dee-177">Jeśli `true`, szczegółowe komunikaty o wyjątkach są zwracane do klientów, gdy wyjątek jest zgłaszany w przypadku metody koncentratora.</span><span class="sxs-lookup"><span data-stu-id="74dee-177">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="74dee-178">Wartość domyślna to `false`, jak te komunikaty o wyjątkach mogą zawierać poufne informacje.</span><span class="sxs-lookup"><span data-stu-id="74dee-178">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

::: moniker-end

<span data-ttu-id="74dee-179">Można skonfigurować opcje do wszystkich centrów, zapewniając delegat opcje do `AddSignalR` wywołania `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="74dee-179">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

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

<span data-ttu-id="74dee-180">Opcje dla jednego centrum zastępują opcje globalne w `AddSignalR` i może być konfigurowana przy użyciu <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span><span class="sxs-lookup"><span data-stu-id="74dee-180">Options for a single hub override the global options provided in `AddSignalR` and can be configured using <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

### <a name="advanced-http-configuration-options"></a><span data-ttu-id="74dee-181">Zaawansowane opcje konfiguracji HTTP</span><span class="sxs-lookup"><span data-stu-id="74dee-181">Advanced HTTP configuration options</span></span>

<span data-ttu-id="74dee-182">Użyj `HttpConnectionDispatcherOptions` skonfigurować zaawansowane ustawienia powiązane z transportami i Zarządzanie buforem pamięci.</span><span class="sxs-lookup"><span data-stu-id="74dee-182">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="74dee-183">Te opcje są konfigurowane przez przekazanie delegat [MapHub\<T >](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) w `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="74dee-183">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) in `Startup.Configure`.</span></span>

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

<span data-ttu-id="74dee-184">W poniższej tabeli opisano opcje dotyczące konfigurowania zaawansowanych opcji HTTP SignalR platformy ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="74dee-184">The following table describes options for configuring ASP.NET Core SignalR's advanced HTTP options:</span></span>

| <span data-ttu-id="74dee-185">Opcja</span><span class="sxs-lookup"><span data-stu-id="74dee-185">Option</span></span> | <span data-ttu-id="74dee-186">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="74dee-186">Default Value</span></span> | <span data-ttu-id="74dee-187">Opis</span><span class="sxs-lookup"><span data-stu-id="74dee-187">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="74dee-188">32 KB</span><span class="sxs-lookup"><span data-stu-id="74dee-188">32 KB</span></span> | <span data-ttu-id="74dee-189">Maksymalna liczba bajtów odebranych od klienta, bufory serwera.</span><span class="sxs-lookup"><span data-stu-id="74dee-189">The maximum number of bytes received from the client that the server buffers.</span></span> <span data-ttu-id="74dee-190">Zwiększenie tej wartości umożliwia serwer do odbierania komunikatów większy, ale może mieć negatywny wpływ na zużycie pamięci.</span><span class="sxs-lookup"><span data-stu-id="74dee-190">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="74dee-191">Dane są automatycznie zbierane z `Authorize` atrybuty stosowane do klasy koncentratora.</span><span class="sxs-lookup"><span data-stu-id="74dee-191">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="74dee-192">Lista [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) obiekty używane do określenia, czy klient jest autoryzowany do podłączać do koncentratora.</span><span class="sxs-lookup"><span data-stu-id="74dee-192">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="74dee-193">32 KB</span><span class="sxs-lookup"><span data-stu-id="74dee-193">32 KB</span></span> | <span data-ttu-id="74dee-194">Maksymalna liczba bajtów wysłanych przez aplikację, bufory serwera.</span><span class="sxs-lookup"><span data-stu-id="74dee-194">The maximum number of bytes sent by the app that the server buffers.</span></span> <span data-ttu-id="74dee-195">Zwiększenie tej wartości umożliwia serwerowi na wysyłanie wiadomości większych, ale może mieć negatywny wpływ na zużycie pamięci.</span><span class="sxs-lookup"><span data-stu-id="74dee-195">Increasing this value allows the server to send larger messages, but can negatively impact memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="74dee-196">Wszystkich transportów są włączone.</span><span class="sxs-lookup"><span data-stu-id="74dee-196">All Transports are enabled.</span></span> | <span data-ttu-id="74dee-197">Maska bitów `HttpTransportType` wartości, które mogą ograniczyć transportów klienta można się połączyć.</span><span class="sxs-lookup"><span data-stu-id="74dee-197">A bitmask of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="74dee-198">Zobacz poniżej.</span><span class="sxs-lookup"><span data-stu-id="74dee-198">See below.</span></span> | <span data-ttu-id="74dee-199">Dodatkowe opcje specyficzne dla transportu sondowania długie.</span><span class="sxs-lookup"><span data-stu-id="74dee-199">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="74dee-200">Zobacz poniżej.</span><span class="sxs-lookup"><span data-stu-id="74dee-200">See below.</span></span> | <span data-ttu-id="74dee-201">Dodatkowe opcje specyficzne dla transportu funkcji WebSockets.</span><span class="sxs-lookup"><span data-stu-id="74dee-201">Additional options specific to the WebSockets transport.</span></span> |

<span data-ttu-id="74dee-202">Transport sondowania długie ma dodatkowe opcje, które można skonfigurować przy użyciu `LongPolling` właściwości:</span><span class="sxs-lookup"><span data-stu-id="74dee-202">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="74dee-203">Opcja</span><span class="sxs-lookup"><span data-stu-id="74dee-203">Option</span></span> | <span data-ttu-id="74dee-204">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="74dee-204">Default Value</span></span> | <span data-ttu-id="74dee-205">Opis</span><span class="sxs-lookup"><span data-stu-id="74dee-205">Description</span></span> |
| ------ | ------------- | ----------- |
| `PollTimeout` | <span data-ttu-id="74dee-206">90 sekund</span><span class="sxs-lookup"><span data-stu-id="74dee-206">90 seconds</span></span> | <span data-ttu-id="74dee-207">Maksymalna ilość czasu serwer oczekuje na komunikat do wysłania do klienta przed zakończeniem żądania sondowania pojedynczego.</span><span class="sxs-lookup"><span data-stu-id="74dee-207">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="74dee-208">Zmniejszenie tej wartości powoduje, że klient wystawiania nowego żądania sondowania częściej.</span><span class="sxs-lookup"><span data-stu-id="74dee-208">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> |

<span data-ttu-id="74dee-209">WebSocket transport ma dodatkowe opcje, które można skonfigurować przy użyciu `WebSockets` właściwości:</span><span class="sxs-lookup"><span data-stu-id="74dee-209">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="74dee-210">Opcja</span><span class="sxs-lookup"><span data-stu-id="74dee-210">Option</span></span> | <span data-ttu-id="74dee-211">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="74dee-211">Default Value</span></span> | <span data-ttu-id="74dee-212">Opis</span><span class="sxs-lookup"><span data-stu-id="74dee-212">Description</span></span> |
| ------ | ------------- | ----------- |
| `CloseTimeout` | <span data-ttu-id="74dee-213">5 sekund</span><span class="sxs-lookup"><span data-stu-id="74dee-213">5 seconds</span></span> | <span data-ttu-id="74dee-214">Po zamknięciu serwera, jeśli klient nie może zamknąć w tym przedziale czasu, połączenie zostanie przerwane.</span><span class="sxs-lookup"><span data-stu-id="74dee-214">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | `null` | <span data-ttu-id="74dee-215">Delegat, który może służyć do ustawiania `Sec-WebSocket-Protocol` nagłówka niestandardowej wartości.</span><span class="sxs-lookup"><span data-stu-id="74dee-215">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="74dee-216">Delegat otrzymuje wartości żądanego przez klienta jako dane wejściowe i powinien zwrócić na żądaną wartość.</span><span class="sxs-lookup"><span data-stu-id="74dee-216">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="74dee-217">Skonfiguruj opcje klienta</span><span class="sxs-lookup"><span data-stu-id="74dee-217">Configure client options</span></span>

<span data-ttu-id="74dee-218">Opcje klienta można skonfigurować na `HubConnectionBuilder` typu (dostępne w klientach .NET i języka JavaScript).</span><span class="sxs-lookup"><span data-stu-id="74dee-218">Client options can be configured on the `HubConnectionBuilder` type (available in the .NET and JavaScript clients).</span></span> <span data-ttu-id="74dee-219">Jest również dostępna w klienta Java, ale `HttpHubConnectionBuilder` podklasy to, co zawiera konstruktora opcje konfiguracji, a także jak na `HubConnection` sam.</span><span class="sxs-lookup"><span data-stu-id="74dee-219">It's also available in the Java client, but the `HttpHubConnectionBuilder` subclass is what contains the builder configuration options, as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="74dee-220">Konfigurowanie rejestrowania</span><span class="sxs-lookup"><span data-stu-id="74dee-220">Configure logging</span></span>

<span data-ttu-id="74dee-221">Rejestrowania skonfigurowano w kliencie programu .NET przy użyciu `ConfigureLogging` metody.</span><span class="sxs-lookup"><span data-stu-id="74dee-221">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="74dee-222">Rejestrowanie dostawcy i filtrów można zarejestrować w taki sam sposób jak znajdują się na serwerze.</span><span class="sxs-lookup"><span data-stu-id="74dee-222">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="74dee-223">Zobacz [rejestrowania w programie ASP.NET Core](xref:fundamentals/logging/index) dokumentacji, aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="74dee-223">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="74dee-224">Aby można było zarejestrować dostawców logowania, należy zainstalować wymagane pakiety.</span><span class="sxs-lookup"><span data-stu-id="74dee-224">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="74dee-225">Zobacz [wbudowane funkcje rejestrowania dostawców](xref:fundamentals/logging/index#built-in-logging-providers) sekcji dokumentacji, aby uzyskać pełną listę.</span><span class="sxs-lookup"><span data-stu-id="74dee-225">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="74dee-226">Na przykład, aby włączyć rejestrowanie konsoli, należy zainstalować `Microsoft.Extensions.Logging.Console` pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="74dee-226">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="74dee-227">Wywołaj `AddConsole` — metoda rozszerzenia:</span><span class="sxs-lookup"><span data-stu-id="74dee-227">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="74dee-228">W kliencie JavaScript podobny `configureLogging` istnieje metoda.</span><span class="sxs-lookup"><span data-stu-id="74dee-228">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="74dee-229">Podaj `LogLevel` wartość wskazującą, minimalny poziom komunikaty dziennika do produkcji.</span><span class="sxs-lookup"><span data-stu-id="74dee-229">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="74dee-230">Dzienniki są zapisywane do okna konsoli przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="74dee-230">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="74dee-231">Zamiast `LogLevel` wartości, można też podać `string` reprezentującą nazwę poziomu dziennika.</span><span class="sxs-lookup"><span data-stu-id="74dee-231">Instead of a `LogLevel` value, you can also provide a `string` value representing a log level name.</span></span> <span data-ttu-id="74dee-232">Jest to przydatne podczas konfigurowania SignalR rejestrowania w środowiskach, w których nie mają dostępu do `LogLevel` stałe.</span><span class="sxs-lookup"><span data-stu-id="74dee-232">This is useful when configuring SignalR logging in environments where you don't have access to the `LogLevel` constants.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging("warn")
    .build();
```

<span data-ttu-id="74dee-233">W poniższej tabeli wymieniono poziomy dziennika dostępne.</span><span class="sxs-lookup"><span data-stu-id="74dee-233">The following table lists the available log levels.</span></span> <span data-ttu-id="74dee-234">Wartość do `configureLogging` ustawia **minimalne** dziennika poziomu, które będą rejestrowane.</span><span class="sxs-lookup"><span data-stu-id="74dee-234">The value you provide to `configureLogging` sets the **minimum** log level that will be logged.</span></span> <span data-ttu-id="74dee-235">Komunikaty zarejestrowane na tym poziomie **lub poziomy wymienione po nim w tabeli**, zostaną zarejestrowane.</span><span class="sxs-lookup"><span data-stu-id="74dee-235">Messages logged at this level, **or the levels listed after it in the table**, will be logged.</span></span>

| <span data-ttu-id="74dee-236">string</span><span class="sxs-lookup"><span data-stu-id="74dee-236">string</span></span> | <span data-ttu-id="74dee-237">LogLevel</span><span class="sxs-lookup"><span data-stu-id="74dee-237">LogLevel</span></span> |
| - | - |
| `"trace"` | `LogLevel.Trace` |
| `"debug"` | `LogLevel.Debug` |
| <span data-ttu-id="74dee-238">`"info"` **lub** `"information"`</span><span class="sxs-lookup"><span data-stu-id="74dee-238">`"info"` **or** `"information"`</span></span> | `LogLevel.Information` |
| <span data-ttu-id="74dee-239">`"warn"` **lub** `"warning"`</span><span class="sxs-lookup"><span data-stu-id="74dee-239">`"warn"` **or** `"warning"`</span></span> | `LogLevel.Warning` |
| `"error"` | `LogLevel.Error` |
| `"critical"` | `LogLevel.Critical` |
| `"none"` | `LogLevel.None` |

::: moniker-end

> [!NOTE]
> <span data-ttu-id="74dee-240">Aby wyłączyć rejestrowanie w całości, należy określić `signalR.LogLevel.None` w `configureLogging` metody.</span><span class="sxs-lookup"><span data-stu-id="74dee-240">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="74dee-241">Aby uzyskać więcej informacji na temat rejestrowania, zobacz [dokumentacja funkcji diagnostyki SignalR](xref:signalr/diagnostics).</span><span class="sxs-lookup"><span data-stu-id="74dee-241">For more information on logging, see the [SignalR Diagnostics documentation](xref:signalr/diagnostics).</span></span>

<span data-ttu-id="74dee-242">Klient SignalR Java używa [SLF4J](https://www.slf4j.org/) biblioteki do rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="74dee-242">The SignalR Java client uses the [SLF4J](https://www.slf4j.org/) library for logging.</span></span> <span data-ttu-id="74dee-243">To API wysokiego poziomu rejestrowania, który umożliwia użytkownikom Biblioteka wybrana zapewniali własną implementację określonych rejestrowania, przenosząc powstanie zależności określonych rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="74dee-243">It's a high-level logging API that allows users of the library to chose their own specific logging implementation by bringing in a specific logging dependency.</span></span> <span data-ttu-id="74dee-244">Poniższy fragment kodu przedstawia sposób użycia `java.util.logging` za pomocą klienta SignalR Java.</span><span class="sxs-lookup"><span data-stu-id="74dee-244">The following code snippet shows how to use `java.util.logging` with the SignalR Java client.</span></span>

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

<span data-ttu-id="74dee-245">Jeśli nie skonfigurowano rejestrowanie, w zależności, SLF4J ładuje domyślny Rejestrator nie operacji następujący komunikat ostrzegawczy:</span><span class="sxs-lookup"><span data-stu-id="74dee-245">If you don't configure logging in your dependencies, SLF4J loads a default no-operation logger with the following warning message:</span></span>

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

<span data-ttu-id="74dee-246">To można bezpiecznie zignorować.</span><span class="sxs-lookup"><span data-stu-id="74dee-246">This can safely be ignored.</span></span>

### <a name="configure-allowed-transports"></a><span data-ttu-id="74dee-247">Konfigurowanie transportów dozwolone</span><span class="sxs-lookup"><span data-stu-id="74dee-247">Configure allowed transports</span></span>

<span data-ttu-id="74dee-248">Transporty używane przez element SignalR można skonfigurować w `WithUrl` wywołania (`withUrl` w języku JavaScript).</span><span class="sxs-lookup"><span data-stu-id="74dee-248">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="74dee-249">Bitowe OR dla wartości z `HttpTransportType` może służyć do ograniczania klienta do użycia tylko określonych transportów.</span><span class="sxs-lookup"><span data-stu-id="74dee-249">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="74dee-250">Wszystkich transportów są domyślnie włączone.</span><span class="sxs-lookup"><span data-stu-id="74dee-250">All transports are enabled by default.</span></span>

<span data-ttu-id="74dee-251">Na przykład aby wyłączyć transportu Server-Sent zdarzenia, ale zezwalaj na WebSockets długie sondowania połączenia i:</span><span class="sxs-lookup"><span data-stu-id="74dee-251">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="74dee-252">W kliencie JavaScript transportów są skonfigurowane, ustawiając `transport` pola w obiekcie opcje udostępniane `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="74dee-252">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="74dee-253">W tej wersji programu Java websockets klienta jest dostępne tylko transportu.</span><span class="sxs-lookup"><span data-stu-id="74dee-253">In this version of the Java client websockets is the only available transport.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-3.0"

<span data-ttu-id="74dee-254">W kliencie Java transportu jest wybrane i ma ustawioną `withTransport` metody `HttpHubConnectionBuilder`.</span><span class="sxs-lookup"><span data-stu-id="74dee-254">In the Java client, the transport is selected with the `withTransport` method on the `HttpHubConnectionBuilder`.</span></span> <span data-ttu-id="74dee-255">Ustawienia domyślne klienta Java za pomocą transportu WebSockets.</span><span class="sxs-lookup"><span data-stu-id="74dee-255">The Java client defaults to using the WebSockets transport.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withTransport(TransportEnum.WEBSOCKETS)
    .build();
```

> [!NOTE]
> <span data-ttu-id="74dee-256">Klienta SignalR Java nie obsługuje jeszcze transportu rezerwowego.</span><span class="sxs-lookup"><span data-stu-id="74dee-256">The SignalR Java client doesn't support transport fallback yet.</span></span>

::: moniker-end

### <a name="configure-bearer-authentication"></a><span data-ttu-id="74dee-257">Konfigurowanie uwierzytelniania elementu nośnego</span><span class="sxs-lookup"><span data-stu-id="74dee-257">Configure bearer authentication</span></span>

<span data-ttu-id="74dee-258">Aby przekazać dane uwierzytelniania wraz z SignalR żądania, użyj `AccessTokenProvider` opcji (`accessTokenFactory` w języku JavaScript), aby określić funkcję, która zwraca token żądanego dostępu.</span><span class="sxs-lookup"><span data-stu-id="74dee-258">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="74dee-259">W kliencie programu .NET ten token dostępu jest przekazywany jako "Uwierzytelniania elementu nośnego" HTTP tokenu (używanie `Authorization` nagłówka o typie `Bearer`).</span><span class="sxs-lookup"><span data-stu-id="74dee-259">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="74dee-260">W kliencie JavaScript token dostępu jest używany jako token elementu nośnego **z wyjątkiem** w niektórych przypadkach, gdy przeglądarka interfejsów API ograniczyć możliwość stosowania nagłówki (w szczególności w żądaniach Server-Sent zdarzeń i technologia WebSockets).</span><span class="sxs-lookup"><span data-stu-id="74dee-260">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="74dee-261">W takich przypadkach token dostępu jest oferowana jako wartość ciągu zapytania `access_token`.</span><span class="sxs-lookup"><span data-stu-id="74dee-261">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="74dee-262">W kliencie programu .NET `AccessTokenProvider` opcja może być określona za pomocą opcji delegata `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="74dee-262">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

<span data-ttu-id="74dee-263">W kliencie JavaScript token dostępu jest skonfigurowana, ustawiając `accessTokenFactory` pola w obiekcie opcje w `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="74dee-263">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

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

<span data-ttu-id="74dee-264">W kliencie SignalR Java, można skonfigurować token elementu nośnego do użycia na potrzeby uwierzytelniania, zapewniając fabryki tokenów dostępu w celu [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span><span class="sxs-lookup"><span data-stu-id="74dee-264">In the SignalR Java client, you can configure a bearer token to use for authentication by providing an access token factory to the [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span></span> <span data-ttu-id="74dee-265">Użyj [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) zapewnienie [RxJava](https://github.com/ReactiveX/RxJava) [pojedynczego\<ciągu >](https://reactivex.io/documentation/single.html).</span><span class="sxs-lookup"><span data-stu-id="74dee-265">Use [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) to provide an [RxJava](https://github.com/ReactiveX/RxJava) [Single\<String>](https://reactivex.io/documentation/single.html).</span></span> <span data-ttu-id="74dee-266">W wyniku wywołania [Single.defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), można napisać logikę do tworzenia tokenów dostępu klienta.</span><span class="sxs-lookup"><span data-stu-id="74dee-266">With a call to [Single.defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), you can write logic to produce access tokens for your client.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="74dee-267">Konfigurowanie limitu czasu i opcje keep-alive</span><span class="sxs-lookup"><span data-stu-id="74dee-267">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="74dee-268">Dodatkowe opcje dotyczące konfigurowania limitu czasu i zachowanie keep-alive są dostępne na `HubConnection` samego obiektu:</span><span class="sxs-lookup"><span data-stu-id="74dee-268">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>

# <a name="nettabdotnet"></a>[<span data-ttu-id="74dee-269">.NET</span><span class="sxs-lookup"><span data-stu-id="74dee-269">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="74dee-270">Opcja</span><span class="sxs-lookup"><span data-stu-id="74dee-270">Option</span></span> | <span data-ttu-id="74dee-271">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="74dee-271">Default value</span></span> | <span data-ttu-id="74dee-272">Opis</span><span class="sxs-lookup"><span data-stu-id="74dee-272">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="74dee-273">30 sekund (ponad 30 000 MS)</span><span class="sxs-lookup"><span data-stu-id="74dee-273">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="74dee-274">Limit czasu aktywności serwera.</span><span class="sxs-lookup"><span data-stu-id="74dee-274">Timeout for server activity.</span></span> <span data-ttu-id="74dee-275">Jeśli serwer nie wysłał wiadomości, w tym interwał, klient traktuje serwera odłączona i wyzwalacze `Closed` zdarzeń (`onclose` w języku JavaScript).</span><span class="sxs-lookup"><span data-stu-id="74dee-275">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="74dee-276">Ta wartość musi być wystarczająco duży dla komunikat ping do wysłania z serwera **i** odebranych przez klienta w ciągu interwału limitu czasu.</span><span class="sxs-lookup"><span data-stu-id="74dee-276">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="74dee-277">Zalecana wartość to co najmniej dwukrotnie serwera numeru `KeepAliveInterval` wartość, aby dać czas na polecenia ping do odbierania.</span><span class="sxs-lookup"><span data-stu-id="74dee-277">The recommended value is a number at least double the server's `KeepAliveInterval` value, to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="74dee-278">15 sekund</span><span class="sxs-lookup"><span data-stu-id="74dee-278">15 seconds</span></span> | <span data-ttu-id="74dee-279">Limit czasu dla serwera początkowego uzgadniania.</span><span class="sxs-lookup"><span data-stu-id="74dee-279">Timeout for initial server handshake.</span></span> <span data-ttu-id="74dee-280">Jeśli serwer nie wysyłać odpowiedzi uzgadniania, w tym interwał, klient anuluje uzgadnianiu i wyzwalacze `Closed` zdarzeń (`onclose` w języku JavaScript).</span><span class="sxs-lookup"><span data-stu-id="74dee-280">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="74dee-281">To ustawienie Zaawansowane, które powinny być modyfikowane tylko, jeśli występują błędy przekroczenia limitu czasu uzgadnianie ze względu na opóźnienie sieci poważne.</span><span class="sxs-lookup"><span data-stu-id="74dee-281">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="74dee-282">Aby uzyskać więcej szczegółów na temat procesu uzgadniania, zobacz [specyfikacji protokołu Centrum SignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="74dee-282">For more detail on the Handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

<span data-ttu-id="74dee-283">W kliencie programu .NET wartości limitu czasu są określane jako `TimeSpan` wartości.</span><span class="sxs-lookup"><span data-stu-id="74dee-283">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="74dee-284">JavaScript</span><span class="sxs-lookup"><span data-stu-id="74dee-284">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="74dee-285">Opcja</span><span class="sxs-lookup"><span data-stu-id="74dee-285">Option</span></span> | <span data-ttu-id="74dee-286">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="74dee-286">Default value</span></span> | <span data-ttu-id="74dee-287">Opis</span><span class="sxs-lookup"><span data-stu-id="74dee-287">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="74dee-288">30 sekund (ponad 30 000 MS)</span><span class="sxs-lookup"><span data-stu-id="74dee-288">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="74dee-289">Limit czasu aktywności serwera.</span><span class="sxs-lookup"><span data-stu-id="74dee-289">Timeout for server activity.</span></span> <span data-ttu-id="74dee-290">Jeśli serwer nie wysłał wiadomości, w tym interwał, klient traktuje serwera odłączona i wyzwalacze `onclose` zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="74dee-290">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="74dee-291">Ta wartość musi być wystarczająco duży dla komunikat ping do wysłania z serwera **i** odebranych przez klienta w ciągu interwału limitu czasu.</span><span class="sxs-lookup"><span data-stu-id="74dee-291">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="74dee-292">Zalecana wartość to co najmniej dwukrotnie serwera numeru `KeepAliveInterval` wartość, aby dać czas na polecenia ping do odbierania.</span><span class="sxs-lookup"><span data-stu-id="74dee-292">The recommended value is a number at least double the server's `KeepAliveInterval` value, to allow time for pings to arrive.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="74dee-293">Java</span><span class="sxs-lookup"><span data-stu-id="74dee-293">Java</span></span>](#tab/java)

| <span data-ttu-id="74dee-294">Opcja</span><span class="sxs-lookup"><span data-stu-id="74dee-294">Option</span></span> | <span data-ttu-id="74dee-295">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="74dee-295">Default value</span></span> | <span data-ttu-id="74dee-296">Opis</span><span class="sxs-lookup"><span data-stu-id="74dee-296">Description</span></span> |
| ----------- | ------------- | ----------- |
|<span data-ttu-id="74dee-297">`getServerTimeout``setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="74dee-297">`getServerTimeout` `setServerTimeout`</span></span> | <span data-ttu-id="74dee-298">30 sekund (ponad 30 000 MS)</span><span class="sxs-lookup"><span data-stu-id="74dee-298">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="74dee-299">Limit czasu aktywności serwera.</span><span class="sxs-lookup"><span data-stu-id="74dee-299">Timeout for server activity.</span></span> <span data-ttu-id="74dee-300">Jeśli serwer nie wysłał wiadomości, w tym interwał, klient traktuje serwera odłączona i wyzwalacze `onClose` zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="74dee-300">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="74dee-301">Ta wartość musi być wystarczająco duży dla komunikat ping do wysłania z serwera **i** odebranych przez klienta w ciągu interwału limitu czasu.</span><span class="sxs-lookup"><span data-stu-id="74dee-301">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="74dee-302">Zalecana wartość to co najmniej dwukrotnie serwera numeru `KeepAliveInterval` wartość, aby dać czas na polecenia ping do odbierania.</span><span class="sxs-lookup"><span data-stu-id="74dee-302">The recommended value is a number at least double the server's `KeepAliveInterval` value, to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="74dee-303">15 sekund</span><span class="sxs-lookup"><span data-stu-id="74dee-303">15 seconds</span></span> | <span data-ttu-id="74dee-304">Limit czasu dla serwera początkowego uzgadniania.</span><span class="sxs-lookup"><span data-stu-id="74dee-304">Timeout for initial server handshake.</span></span> <span data-ttu-id="74dee-305">Jeśli serwer nie wysyłać odpowiedzi uzgadniania, w tym interwał, klient anuluje uzgadnianiu i wyzwalacze `onClose` zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="74dee-305">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="74dee-306">To ustawienie Zaawansowane, które powinny być modyfikowane tylko, jeśli występują błędy przekroczenia limitu czasu uzgadnianie ze względu na opóźnienie sieci poważne.</span><span class="sxs-lookup"><span data-stu-id="74dee-306">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="74dee-307">Aby uzyskać więcej szczegółów na temat procesu uzgadniania, zobacz [specyfikacji protokołu Centrum SignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="74dee-307">For more detail on the Handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

---

### <a name="configure-additional-options"></a><span data-ttu-id="74dee-308">Konfigurowanie opcji dodatkowych</span><span class="sxs-lookup"><span data-stu-id="74dee-308">Configure additional options</span></span>

<span data-ttu-id="74dee-309">Dodatkowe opcje można skonfigurować w `WithUrl` (`withUrl` w języku JavaScript) metody `HubConnectionBuilder` lub w konfiguracji różnych interfejsów API na `HttpHubConnectionBuilder` klienta Java:</span><span class="sxs-lookup"><span data-stu-id="74dee-309">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder` or on the various configuration APIs on the `HttpHubConnectionBuilder` in the Java client:</span></span>

# <a name="nettabdotnet"></a>[<span data-ttu-id="74dee-310">.NET</span><span class="sxs-lookup"><span data-stu-id="74dee-310">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="74dee-311">.NET — opcja</span><span class="sxs-lookup"><span data-stu-id="74dee-311">.NET Option</span></span> |  <span data-ttu-id="74dee-312">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="74dee-312">Default value</span></span> | <span data-ttu-id="74dee-313">Opis</span><span class="sxs-lookup"><span data-stu-id="74dee-313">Description</span></span> |
| ----------- | -------------- | ----------- |
| `AccessTokenProvider` | `null` | <span data-ttu-id="74dee-314">Funkcja zwraca ciąg, który jest dostarczana jako token uwierzytelniania elementu nośnego w żądaniach HTTP.</span><span class="sxs-lookup"><span data-stu-id="74dee-314">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `false` | <span data-ttu-id="74dee-315">Ustaw tę opcję na `true` Aby pominąć krok negocjacji.</span><span class="sxs-lookup"><span data-stu-id="74dee-315">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="74dee-316">**Obsługiwane tylko w przypadku transportu WebSockets jest tylko transportu włączone**.</span><span class="sxs-lookup"><span data-stu-id="74dee-316">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="74dee-317">Nie można włączyć to ustawienie, korzystając z usługi Azure SignalR Service.</span><span class="sxs-lookup"><span data-stu-id="74dee-317">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="74dee-318">Pusty</span><span class="sxs-lookup"><span data-stu-id="74dee-318">Empty</span></span> | <span data-ttu-id="74dee-319">Kolekcja certyfikaty protokołu TLS do wysłania do uwierzytelniania żądań.</span><span class="sxs-lookup"><span data-stu-id="74dee-319">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="74dee-320">Pusty</span><span class="sxs-lookup"><span data-stu-id="74dee-320">Empty</span></span> | <span data-ttu-id="74dee-321">Kolekcja plików cookie protokołu HTTP ma wysłać z każdym żądaniem HTTP.</span><span class="sxs-lookup"><span data-stu-id="74dee-321">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="74dee-322">Pusty</span><span class="sxs-lookup"><span data-stu-id="74dee-322">Empty</span></span> | <span data-ttu-id="74dee-323">Poświadczenia, aby wysłać z każdym żądaniem HTTP.</span><span class="sxs-lookup"><span data-stu-id="74dee-323">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="74dee-324">5 sekund</span><span class="sxs-lookup"><span data-stu-id="74dee-324">5 seconds</span></span> | <span data-ttu-id="74dee-325">Tylko WebSockets.</span><span class="sxs-lookup"><span data-stu-id="74dee-325">WebSockets only.</span></span> <span data-ttu-id="74dee-326">Maksymalna ilość czasu klient odczekuje po zamknięciu dla serwera potwierdzić żądanie zamknięcia.</span><span class="sxs-lookup"><span data-stu-id="74dee-326">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="74dee-327">Jeśli serwer nie potwierdzenia zamknięcia w tej chwili, klient odłączy się.</span><span class="sxs-lookup"><span data-stu-id="74dee-327">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="74dee-328">Pusty</span><span class="sxs-lookup"><span data-stu-id="74dee-328">Empty</span></span> | <span data-ttu-id="74dee-329">Mapa dodatkowych nagłówków HTTP ma wysłać z każdym żądaniem HTTP.</span><span class="sxs-lookup"><span data-stu-id="74dee-329">A Map of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | `null` | <span data-ttu-id="74dee-330">Delegat, który może służyć do konfigurowania lub Zastąp `HttpMessageHandler` używane do wysyłania żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="74dee-330">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="74dee-331">Nie są używane dla połączeń protokołu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="74dee-331">Not used for WebSocket connections.</span></span> <span data-ttu-id="74dee-332">Ten delegat musi zwracać wartość inną niż null i otrzymuje wartość domyślna, jako parametr.</span><span class="sxs-lookup"><span data-stu-id="74dee-332">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="74dee-333">Albo zmodyfikować ustawienia na tej wartości domyślne i przywrócić go albo zwraca nową `HttpMessageHandler` wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="74dee-333">Either modify settings on that default value and return it, or return a new `HttpMessageHandler` instance.</span></span> <span data-ttu-id="74dee-334">**Podczas zastępowania programu obsługi upewnij się, że Skopiuj ustawienia które mają być przechowywane z podanego programu obsługi, w przeciwnym razie skonfigurowanych opcji (takich jak pliki cookie i nagłówki) nie zostaną zastosowane do nowego programu obsługi.**</span><span class="sxs-lookup"><span data-stu-id="74dee-334">**When replacing the handler make sure to copy the settings you want to keep from the provided handler, otherwise, the configured options (such as Cookies and Headers) won't apply to the new handler.**</span></span> |
| `Proxy` | `null` | <span data-ttu-id="74dee-335">Serwer proxy HTTP do użycia podczas wysyłania żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="74dee-335">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | `false` | <span data-ttu-id="74dee-336">Ustaw ten atrybut typu wartość logiczna do wysyłania poświadczeń domyślnych dla żądań HTTP i Websocket.</span><span class="sxs-lookup"><span data-stu-id="74dee-336">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="74dee-337">Umożliwia to korzystanie z uwierzytelniania Windows.</span><span class="sxs-lookup"><span data-stu-id="74dee-337">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | `null` | <span data-ttu-id="74dee-338">Delegat, który można skonfigurować dodatkowe opcje protokołu WebSocket.</span><span class="sxs-lookup"><span data-stu-id="74dee-338">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="74dee-339">Otrzymuje wystąpienie elementu [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) można skonfigurować opcje.</span><span class="sxs-lookup"><span data-stu-id="74dee-339">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="74dee-340">JavaScript</span><span class="sxs-lookup"><span data-stu-id="74dee-340">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="74dee-341">JavaScript Option</span><span class="sxs-lookup"><span data-stu-id="74dee-341">JavaScript Option</span></span> | <span data-ttu-id="74dee-342">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="74dee-342">Default Value</span></span> | <span data-ttu-id="74dee-343">Opis</span><span class="sxs-lookup"><span data-stu-id="74dee-343">Description</span></span> |
| ----------------- | ------------- | ----------- |
| `accessTokenFactory` | `null` | <span data-ttu-id="74dee-344">Funkcja zwraca ciąg, który jest dostarczana jako token uwierzytelniania elementu nośnego w żądaniach HTTP.</span><span class="sxs-lookup"><span data-stu-id="74dee-344">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `skipNegotiation` | `false` | <span data-ttu-id="74dee-345">Ustaw tę opcję na `true` Aby pominąć krok negocjacji.</span><span class="sxs-lookup"><span data-stu-id="74dee-345">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="74dee-346">**Obsługiwane tylko w przypadku transportu WebSockets jest tylko transportu włączone**.</span><span class="sxs-lookup"><span data-stu-id="74dee-346">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="74dee-347">Nie można włączyć to ustawienie, korzystając z usługi Azure SignalR Service.</span><span class="sxs-lookup"><span data-stu-id="74dee-347">This setting can't be enabled when using the Azure SignalR Service.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="74dee-348">Java</span><span class="sxs-lookup"><span data-stu-id="74dee-348">Java</span></span>](#tab/java)

| <span data-ttu-id="74dee-349">Opcja języka Java</span><span class="sxs-lookup"><span data-stu-id="74dee-349">Java Option</span></span> | <span data-ttu-id="74dee-350">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="74dee-350">Default Value</span></span> | <span data-ttu-id="74dee-351">Opis</span><span class="sxs-lookup"><span data-stu-id="74dee-351">Description</span></span> |
| ----------- | ------------- | ----------- |
| `withAccessTokenProvider` | `null` | <span data-ttu-id="74dee-352">Funkcja zwraca ciąg, który jest dostarczana jako token uwierzytelniania elementu nośnego w żądaniach HTTP.</span><span class="sxs-lookup"><span data-stu-id="74dee-352">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `shouldSkipNegotiate` | `false` | <span data-ttu-id="74dee-353">Ustaw tę opcję na `true` Aby pominąć krok negocjacji.</span><span class="sxs-lookup"><span data-stu-id="74dee-353">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="74dee-354">**Obsługiwane tylko w przypadku transportu WebSockets jest tylko transportu włączone**.</span><span class="sxs-lookup"><span data-stu-id="74dee-354">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="74dee-355">Nie można włączyć to ustawienie, korzystając z usługi Azure SignalR Service.</span><span class="sxs-lookup"><span data-stu-id="74dee-355">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| <span data-ttu-id="74dee-356">`withHeader``withHeaders`</span><span class="sxs-lookup"><span data-stu-id="74dee-356">`withHeader` `withHeaders`</span></span> | <span data-ttu-id="74dee-357">Pusty</span><span class="sxs-lookup"><span data-stu-id="74dee-357">Empty</span></span> | <span data-ttu-id="74dee-358">Mapa dodatkowych nagłówków HTTP ma wysłać z każdym żądaniem HTTP.</span><span class="sxs-lookup"><span data-stu-id="74dee-358">A Map of additional HTTP headers to send with every HTTP request.</span></span> |

---

<span data-ttu-id="74dee-359">W kliencie programu .NET, te opcje mogą być modyfikowane przez delegata opcje udostępnionego `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="74dee-359">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="74dee-360">W kliencie JavaScript, te opcje można podać w obiekcie JavaScript udostępniane `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="74dee-360">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

<span data-ttu-id="74dee-361">W kliencie Java, te opcje można skonfigurować za pomocą metod na `HttpHubConnectionBuilder` zwrócone w wyniku `HubConnectionBuilder.create("HUB URL")`</span><span class="sxs-lookup"><span data-stu-id="74dee-361">In the Java client, these options can be configured with the methods on the `HttpHubConnectionBuilder` returned from the `HubConnectionBuilder.create("HUB URL")`</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
        .withHeader("Foo", "Bar")
        .shouldSkipNegotiate(true)
        .withHandshakeResponseTimeout(30*1000)
        .build();
```

## <a name="additional-resources"></a><span data-ttu-id="74dee-362">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="74dee-362">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>
