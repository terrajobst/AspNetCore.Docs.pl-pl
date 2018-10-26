---
title: Różnice między SignalR i SignalR platformy ASP.NET Core
author: tdykstra
description: Różnice między SignalR i SignalR platformy ASP.NET Core
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.date: 09/10/2018
uid: signalr/version-differences
ms.openlocfilehash: 4ac7952f26500285fc1c8f9453feb3ea8b33851a
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/25/2018
ms.locfileid: "50089833"
---
# <a name="differences-between-aspnet-signalr-and-aspnet-core-signalr"></a><span data-ttu-id="a8fcf-103">Różnice między biblioteki SignalR platformy ASP.NET i SignalR platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a8fcf-103">Differences between ASP.NET SignalR and ASP.NET Core SignalR</span></span>

<span data-ttu-id="a8fcf-104">SignalR platformy ASP.NET Core nie jest zgodny z klientami lub serwerami na potrzeby biblioteki SignalR platformy ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="a8fcf-104">ASP.NET Core SignalR isn't compatible with clients or servers for ASP.NET SignalR.</span></span> <span data-ttu-id="a8fcf-105">Ten artykuł szczegółowo opisuje funkcje, które zostały usunięte lub zmienione w biblioteki SignalR platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a8fcf-105">This article details features which have been removed or changed in ASP.NET Core SignalR.</span></span>

## <a name="how-to-identify-the-signalr-version"></a><span data-ttu-id="a8fcf-106">Jak zidentyfikować wersji biblioteki SignalR</span><span class="sxs-lookup"><span data-stu-id="a8fcf-106">How to identify the SignalR version</span></span>

|                      | <span data-ttu-id="a8fcf-107">ASP.NET SignalR</span><span class="sxs-lookup"><span data-stu-id="a8fcf-107">ASP.NET SignalR</span></span> | <span data-ttu-id="a8fcf-108">SignalR platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a8fcf-108">ASP.NET Core SignalR</span></span> |
| -------------------- | --------------- | -------------------- |
| <span data-ttu-id="a8fcf-109">Pakiet NuGet</span><span class="sxs-lookup"><span data-stu-id="a8fcf-109">Server NuGet Package</span></span> | [<span data-ttu-id="a8fcf-110">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="a8fcf-110">Microsoft.AspNet.SignalR</span></span>](https://www.nuget.org/packages/Microsoft.AspNet.SignalR/) | <span data-ttu-id="a8fcf-111">[Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) (.NET Core)</span><span class="sxs-lookup"><span data-stu-id="a8fcf-111">[Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) (.NET Core)</span></span><br><span data-ttu-id="a8fcf-112">[Microsoft.AspNetCore.SignalR](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) (.NET Framework)</span><span class="sxs-lookup"><span data-stu-id="a8fcf-112">[Microsoft.AspNetCore.SignalR](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) (.NET Framework)</span></span> |
| <span data-ttu-id="a8fcf-113">Pakiety NuGet klienta</span><span class="sxs-lookup"><span data-stu-id="a8fcf-113">Client NuGet Packages</span></span> | [<span data-ttu-id="a8fcf-114">Microsoft.AspNet.SignalR.Client</span><span class="sxs-lookup"><span data-stu-id="a8fcf-114">Microsoft.AspNet.SignalR.Client</span></span>](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.Client/)<br>[<span data-ttu-id="a8fcf-115">Microsoft.AspNet.SignalR.JS</span><span class="sxs-lookup"><span data-stu-id="a8fcf-115">Microsoft.AspNet.SignalR.JS</span></span>](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.JS/) | [<span data-ttu-id="a8fcf-116">Microsoft.AspNetCore.SignalR.Client</span><span class="sxs-lookup"><span data-stu-id="a8fcf-116">Microsoft.AspNetCore.SignalR.Client</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client/) |
| <span data-ttu-id="a8fcf-117">Klient npm pakietu</span><span class="sxs-lookup"><span data-stu-id="a8fcf-117">Client npm Package</span></span> | [<span data-ttu-id="a8fcf-118">signalr</span><span class="sxs-lookup"><span data-stu-id="a8fcf-118">signalr</span></span>](https://www.npmjs.com/package/signalr) | [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) |
| <span data-ttu-id="a8fcf-119">Typ aplikacji serwera</span><span class="sxs-lookup"><span data-stu-id="a8fcf-119">Server App Type</span></span> | <span data-ttu-id="a8fcf-120">ASP.NET (System.Web) lub samodzielnego hostowania OWIN</span><span class="sxs-lookup"><span data-stu-id="a8fcf-120">ASP.NET (System.Web) or OWIN Self-Host</span></span> | <span data-ttu-id="a8fcf-121">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a8fcf-121">ASP.NET Core</span></span> |
| <span data-ttu-id="a8fcf-122">Platformy obsługiwane serwera</span><span class="sxs-lookup"><span data-stu-id="a8fcf-122">Supported Server Platforms</span></span> | <span data-ttu-id="a8fcf-123">.NET framework 4.5 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="a8fcf-123">.NET Framework 4.5 or later</span></span> | <span data-ttu-id="a8fcf-124">Program .NET framework 4.6.1 lub nowszej</span><span class="sxs-lookup"><span data-stu-id="a8fcf-124">.NET Framework 4.6.1 or later</span></span><br><span data-ttu-id="a8fcf-125">.NET core 2.1 lub nowszej</span><span class="sxs-lookup"><span data-stu-id="a8fcf-125">.NET Core 2.1 or later</span></span> |

## <a name="feature-differences"></a><span data-ttu-id="a8fcf-126">Różnice w funkcjach</span><span class="sxs-lookup"><span data-stu-id="a8fcf-126">Feature differences</span></span>

### <a name="automatic-reconnects"></a><span data-ttu-id="a8fcf-127">Automatyczne ponowne podłączenia</span><span class="sxs-lookup"><span data-stu-id="a8fcf-127">Automatic reconnects</span></span>

<span data-ttu-id="a8fcf-128">Automatyczne ponowne podłączenia nie są obsługiwane w biblioteki SignalR platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a8fcf-128">Automatic reconnects aren't supported in ASP.NET Core SignalR.</span></span> <span data-ttu-id="a8fcf-129">Jeśli klient został odłączony, użytkownik jawnie należy uruchomić nowe połączenie, jeśli chcesz ponownie połączyć.</span><span class="sxs-lookup"><span data-stu-id="a8fcf-129">If the client is disconnected, the user must explicitly start a new connection if they want to reconnect.</span></span> <span data-ttu-id="a8fcf-130">W programie ASP.NET SignalR SignalR podejmie próbę połączenia z serwerem, jeśli połączenie zostało przerwane.</span><span class="sxs-lookup"><span data-stu-id="a8fcf-130">In ASP.NET SignalR, SignalR attempts to reconnect to the server if the connection is dropped.</span></span> 

### <a name="protocol-support"></a><span data-ttu-id="a8fcf-131">Obsługa protokołów</span><span class="sxs-lookup"><span data-stu-id="a8fcf-131">Protocol support</span></span>

<span data-ttu-id="a8fcf-132">Biblioteki SignalR platformy ASP.NET Core obsługuje JSON, a także nowy protokół binarny na podstawie [MessagePack](xref:signalr/messagepackhubprotocol).</span><span class="sxs-lookup"><span data-stu-id="a8fcf-132">ASP.NET Core SignalR supports JSON, as well as a new binary protocol based on [MessagePack](xref:signalr/messagepackhubprotocol).</span></span> <span data-ttu-id="a8fcf-133">Ponadto protokoły niestandardowe, mogą być tworzone.</span><span class="sxs-lookup"><span data-stu-id="a8fcf-133">Additionally, custom protocols can be created.</span></span>

## <a name="differences-on-the-server"></a><span data-ttu-id="a8fcf-134">Różnice na serwerze</span><span class="sxs-lookup"><span data-stu-id="a8fcf-134">Differences on the server</span></span>

<span data-ttu-id="a8fcf-135">Biblioteki po stronie serwera biblioteki SignalR platformy ASP.NET Core są objęte [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) pakiet, który jest częścią **aplikacji sieci Web programu ASP.NET Core** szablon Razor i MVC projekty.</span><span class="sxs-lookup"><span data-stu-id="a8fcf-135">The ASP.NET Core SignalR server-side libraries are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) package that's part of the **ASP.NET Core Web Application** template for both Razor and MVC projects.</span></span>

<span data-ttu-id="a8fcf-136">Biblioteki SignalR platformy ASP.NET Core jest pośredniczące platformy ASP.NET Core, więc musi być skonfigurowany, wywołując [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="a8fcf-136">ASP.NET Core SignalR is an ASP.NET Core middleware, so it must be configured by calling [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in `Startup.ConfigureServices`.</span></span>

```csharp
services.AddSignalR()
```

<span data-ttu-id="a8fcf-137">Aby skonfigurować routing, mapy trasy do koncentratorów wewnątrz [UseSignalR](/dotnet/api/microsoft.aspnetcore.builder.signalrappbuilderextensions.usesignalr) wywołania metody, w `Startup.Configure` metody.</span><span class="sxs-lookup"><span data-stu-id="a8fcf-137">To configure routing, map routes to hubs inside the [UseSignalR](/dotnet/api/microsoft.aspnetcore.builder.signalrappbuilderextensions.usesignalr) method call in the `Startup.Configure` method.</span></span>

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("/hub");
});
```

### <a name="sticky-sessions-now-required"></a><span data-ttu-id="a8fcf-138">Trwałych sesji teraz wymagany</span><span class="sxs-lookup"><span data-stu-id="a8fcf-138">Sticky sessions now required</span></span>

<span data-ttu-id="a8fcf-139">Ze względu na sposób skalowania w poziomie pracy w ASP.NET SignalR klientów może ponownie połączyć, a następnie wysyłać komunikaty do dowolnego serwera w farmie.</span><span class="sxs-lookup"><span data-stu-id="a8fcf-139">Because of how scale-out worked in ASP.NET SignalR, clients could reconnect and send messages to any server in the farm.</span></span> <span data-ttu-id="a8fcf-140">Ze względu na zmiany modelu skalowalnego w poziomie, jak również nie obsługuje ponowne podłączenia to nie jest już obsługiwana.</span><span class="sxs-lookup"><span data-stu-id="a8fcf-140">Due to changes to the scale-out model, as well as not supporting reconnects, this is no longer supported.</span></span> <span data-ttu-id="a8fcf-141">Gdy klient nawiąże połączenie z serwerem, na czas trwania połączenia musi współdziałać z tym samym serwerze.</span><span class="sxs-lookup"><span data-stu-id="a8fcf-141">Once the client connects to the server, it must interact with the same server for the duration of the connection.</span></span>

### <a name="single-hub-per-connection"></a><span data-ttu-id="a8fcf-142">Jedno centrum dla połączenia</span><span class="sxs-lookup"><span data-stu-id="a8fcf-142">Single hub per connection</span></span>

<span data-ttu-id="a8fcf-143">W bibliotece SignalR platformy ASP.NET Core został uproszczony model połączenia.</span><span class="sxs-lookup"><span data-stu-id="a8fcf-143">In ASP.NET Core SignalR, the connection model has been simplified.</span></span> <span data-ttu-id="a8fcf-144">Połączenia są nawiązywane bezpośrednio z jednym Centrum, zamiast jednego połączenia używany do udostępnienia w wielu centrach.</span><span class="sxs-lookup"><span data-stu-id="a8fcf-144">Connections are made directly to a single hub, rather than a single connection being used to share access to multiple hubs.</span></span>

### <a name="streaming"></a><span data-ttu-id="a8fcf-145">Przesyłanie strumieniowe</span><span class="sxs-lookup"><span data-stu-id="a8fcf-145">Streaming</span></span>

<span data-ttu-id="a8fcf-146">Obsługuje teraz Core SignalR platformy ASP.NET [danych przesyłanych strumieniowo](xref:signalr/streaming) z koncentratora do klienta.</span><span class="sxs-lookup"><span data-stu-id="a8fcf-146">ASP.NET Core SignalR now supports [streaming data](xref:signalr/streaming) from the hub to the client.</span></span>

### <a name="state"></a><span data-ttu-id="a8fcf-147">Stan</span><span class="sxs-lookup"><span data-stu-id="a8fcf-147">State</span></span>

<span data-ttu-id="a8fcf-148">Możliwość przekazywania dowolny stan między klientami a Centrum (często nazywanej HubState) zostało usunięte, a także obsługę wiadomości dotyczące postępu.</span><span class="sxs-lookup"><span data-stu-id="a8fcf-148">The ability to pass arbitrary state between clients and the hub (often called HubState) has been removed, as well as support for progress messages.</span></span> <span data-ttu-id="a8fcf-149">Nie ma odpowiednika serwerów proxy koncentratora, w tym momencie.</span><span class="sxs-lookup"><span data-stu-id="a8fcf-149">There is no counterpart of hub proxies at the moment.</span></span>

## <a name="differences-on-the-client"></a><span data-ttu-id="a8fcf-150">Różnice na komputerze klienckim</span><span class="sxs-lookup"><span data-stu-id="a8fcf-150">Differences on the client</span></span>

### <a name="typescript"></a><span data-ttu-id="a8fcf-151">TypeScript</span><span class="sxs-lookup"><span data-stu-id="a8fcf-151">TypeScript</span></span>

<span data-ttu-id="a8fcf-152">Klient biblioteki SignalR platformy ASP.NET Core jest zapisywany [TypeScript](https://www.typescriptlang.org/).</span><span class="sxs-lookup"><span data-stu-id="a8fcf-152">The ASP.NET Core SignalR client is written in [TypeScript](https://www.typescriptlang.org/).</span></span> <span data-ttu-id="a8fcf-153">Można napisać w JavaScript lub TypeScript, korzystając z [klienta JavaScript](xref:signalr/javascript-client).</span><span class="sxs-lookup"><span data-stu-id="a8fcf-153">You can write in JavaScript or TypeScript when using the [JavaScript client](xref:signalr/javascript-client).</span></span>

### <a name="the-javascript-client-is-hosted-at-npmhttpswwwnpmjscom"></a><span data-ttu-id="a8fcf-154">Klient JavaScript znajduje się w [npm](https://www.npmjs.com/)</span><span class="sxs-lookup"><span data-stu-id="a8fcf-154">The JavaScript client is hosted at [npm](https://www.npmjs.com/)</span></span>

<span data-ttu-id="a8fcf-155">W poprzednich wersjach klienta JavaScript uzyskano za pośrednictwem pakietu NuGet w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a8fcf-155">In previous versions, the JavaScript client was obtained through a NuGet package in Visual Studio.</span></span> <span data-ttu-id="a8fcf-156">Dla wersji podstawowej [ @aspnet/signalr ](https://www.npmjs.com/package/@aspnet/signalr) pakietu npm zawiera biblioteki języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="a8fcf-156">For the Core versions, the [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) npm package contains the JavaScript libraries.</span></span> <span data-ttu-id="a8fcf-157">Ten pakiet nie jest zawarty w **aplikacji sieci Web programu ASP.NET Core** szablonu.</span><span class="sxs-lookup"><span data-stu-id="a8fcf-157">This package isn't included in the **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="a8fcf-158">Aby uzyskać i zainstalować za pomocą usługi npm `@aspnet/signalr` pakietów Menedżera npm.</span><span class="sxs-lookup"><span data-stu-id="a8fcf-158">Use npm to obtain and install the `@aspnet/signalr` npm package.</span></span>

```console
npm init -y
npm install @aspnet/signalr
```

### <a name="jquery"></a><span data-ttu-id="a8fcf-159">jQuery</span><span class="sxs-lookup"><span data-stu-id="a8fcf-159">jQuery</span></span>

<span data-ttu-id="a8fcf-160">Zależność od jQuery zostało usunięte, jednak projekty można nadal używać jQuery.</span><span class="sxs-lookup"><span data-stu-id="a8fcf-160">The dependency on jQuery has been removed, however projects can still use jQuery.</span></span>

### <a name="javascript-client-method-syntax"></a><span data-ttu-id="a8fcf-161">Składnia metody klient JavaScript</span><span class="sxs-lookup"><span data-stu-id="a8fcf-161">JavaScript client method syntax</span></span>

<span data-ttu-id="a8fcf-162">Składnia języka JavaScript został zmieniony względem poprzedniej wersji biblioteki SignalR.</span><span class="sxs-lookup"><span data-stu-id="a8fcf-162">The JavaScript syntax has changed from the previous version of SignalR.</span></span> <span data-ttu-id="a8fcf-163">Zamiast używania `$connection` obiektów, tworzenia połączenia za pomocą [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="a8fcf-163">Rather than using the `$connection` object, create a connection using the [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) API.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/hub")
    .build();
```

<span data-ttu-id="a8fcf-164">Użyj [na](/javascript/api/@aspnet/signalr/HubConnection#on) metodę, aby określić metody klienta, które można wywołać Centrum.</span><span class="sxs-lookup"><span data-stu-id="a8fcf-164">Use the [on](/javascript/api/@aspnet/signalr/HubConnection#on) method to specify client methods that the hub can call.</span></span>

```javascript
connection.on("ReceiveMessage", (user, message) => {
    const msg = message.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;");
    const encodedMsg = user + " says " + msg;
    log(encodedMsg);
});
```

<span data-ttu-id="a8fcf-165">Po utworzeniu metody klienta, uruchom połączenie koncentratora.</span><span class="sxs-lookup"><span data-stu-id="a8fcf-165">After creating the client method, start the hub connection.</span></span> <span data-ttu-id="a8fcf-166">Łańcuch [catch](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch) metodę, aby zalogować się lub obsługiwać błędy.</span><span class="sxs-lookup"><span data-stu-id="a8fcf-166">Chain a [catch](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch) method to log or handle errors.</span></span>

```javascript
connection.start().catch(err => console.error(err.toString()));
```

### <a name="hub-proxies"></a><span data-ttu-id="a8fcf-167">Serwery proxy Centrum</span><span class="sxs-lookup"><span data-stu-id="a8fcf-167">Hub proxies</span></span>

<span data-ttu-id="a8fcf-168">Nie będzie automatycznie są generowane, serwery proxy koncentratora.</span><span class="sxs-lookup"><span data-stu-id="a8fcf-168">Hub proxies are no longer automatically generated.</span></span> <span data-ttu-id="a8fcf-169">Zamiast tego Nazwa metody jest przekazywana do [wywołania](/javascript/api/%40aspnet/signalr/hubconnection#invoke) API jako ciąg.</span><span class="sxs-lookup"><span data-stu-id="a8fcf-169">Instead, the method name is passed into the [invoke](/javascript/api/%40aspnet/signalr/hubconnection#invoke) API as a string.</span></span>

### <a name="net-and-other-clients"></a><span data-ttu-id="a8fcf-170">.NET i innych klientów</span><span class="sxs-lookup"><span data-stu-id="a8fcf-170">.NET and other clients</span></span>

<span data-ttu-id="a8fcf-171">`Microsoft.AspNetCore.SignalR.Client` Pakietu NuGet zawiera biblioteki klienckie .NET dla biblioteki SignalR platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a8fcf-171">The `Microsoft.AspNetCore.SignalR.Client` NuGet package contains the .NET client libraries for ASP.NET Core SignalR.</span></span>

<span data-ttu-id="a8fcf-172">Użyj [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder) utworzyć i skompilować wystąpienie połączenia z koncentratorem.</span><span class="sxs-lookup"><span data-stu-id="a8fcf-172">Use the [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder) to create and build an instance of a connection to a hub.</span></span>

```csharp
connection = new HubConnectionBuilder()
    .WithUrl("url")
    .Build();
```

## <a name="scaleout-differences"></a><span data-ttu-id="a8fcf-173">Różnice skalowania w poziomie</span><span class="sxs-lookup"><span data-stu-id="a8fcf-173">Scaleout differences</span></span>

<span data-ttu-id="a8fcf-174">Biblioteki SignalR platformy ASP.NET obsługuje program SQL Server i Redis.</span><span class="sxs-lookup"><span data-stu-id="a8fcf-174">ASP.NET SignalR supports SQL Server and Redis.</span></span> <span data-ttu-id="a8fcf-175">Biblioteki SignalR platformy ASP.NET Core obsługuje usługi Azure SignalR Service i Redis.</span><span class="sxs-lookup"><span data-stu-id="a8fcf-175">ASP.NET Core SignalR supports Azure SignalR Service and Redis.</span></span>

### <a name="aspnet"></a><span data-ttu-id="a8fcf-176">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="a8fcf-176">ASP.NET</span></span>

* [<span data-ttu-id="a8fcf-177">SignalR — skalowanie w poziomie za pomocą usługi Azure Service Bus</span><span class="sxs-lookup"><span data-stu-id="a8fcf-177">SignalR scaleout with Azure Service Bus</span></span>](/aspnet/signalr/overview/performance/scaleout-with-windows-azure-service-bus)
* [<span data-ttu-id="a8fcf-178">SignalR — skalowanie w poziomie przy użyciu usługi Redis</span><span class="sxs-lookup"><span data-stu-id="a8fcf-178">SignalR scaleout with Redis</span></span>](/aspnet/signalr/overview/performance/scaleout-with-redis)
* [<span data-ttu-id="a8fcf-179">SignalR — skalowanie w poziomie z programem SQL Server</span><span class="sxs-lookup"><span data-stu-id="a8fcf-179">SignalR scaleout with SQL Server</span></span>](/aspnet/signalr/overview/performance/scaleout-with-sql-server)

### <a name="aspnet-core"></a><span data-ttu-id="a8fcf-180">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a8fcf-180">ASP.NET Core</span></span>

* [<span data-ttu-id="a8fcf-181">Usługi Azure SignalR Service</span><span class="sxs-lookup"><span data-stu-id="a8fcf-181">Azure SignalR Service</span></span>](/azure/azure-signalr/)

## <a name="additional-resources"></a><span data-ttu-id="a8fcf-182">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="a8fcf-182">Additional resources</span></span>

* [<span data-ttu-id="a8fcf-183">Centra</span><span class="sxs-lookup"><span data-stu-id="a8fcf-183">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="a8fcf-184">Klient JavaScript</span><span class="sxs-lookup"><span data-stu-id="a8fcf-184">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="a8fcf-185">Klient .NET</span><span class="sxs-lookup"><span data-stu-id="a8fcf-185">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="a8fcf-186">Obsługiwane platformy</span><span class="sxs-lookup"><span data-stu-id="a8fcf-186">Supported platforms</span></span>](xref:signalr/supported-platforms)
