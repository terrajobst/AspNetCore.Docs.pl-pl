---
title: Różnice między SignalR i SignalR platformy ASP.NET Core
author: tdykstra
description: Różnice między SignalR i SignalR platformy ASP.NET Core
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.date: 11/14/2018
uid: signalr/version-differences
ms.openlocfilehash: c9302f1c9e7cd4e62eaeaef871feb54ef26aa3ca
ms.sourcegitcommit: 09bcda59a58019fdf47b2db5259fe87acf19dd38
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/15/2018
ms.locfileid: "51708416"
---
# <a name="differences-between-aspnet-signalr-and-aspnet-core-signalr"></a><span data-ttu-id="db972-103">Różnice między biblioteki SignalR platformy ASP.NET i SignalR platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="db972-103">Differences between ASP.NET SignalR and ASP.NET Core SignalR</span></span>

<span data-ttu-id="db972-104">SignalR platformy ASP.NET Core nie jest zgodny z klientami lub serwerami na potrzeby biblioteki SignalR platformy ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="db972-104">ASP.NET Core SignalR isn't compatible with clients or servers for ASP.NET SignalR.</span></span> <span data-ttu-id="db972-105">Ten artykuł szczegółowo opisuje funkcje, które zostały usunięte lub zmienione w biblioteki SignalR platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="db972-105">This article details features which have been removed or changed in ASP.NET Core SignalR.</span></span>

## <a name="how-to-identify-the-signalr-version"></a><span data-ttu-id="db972-106">Jak zidentyfikować wersji biblioteki SignalR</span><span class="sxs-lookup"><span data-stu-id="db972-106">How to identify the SignalR version</span></span>

|                      | <span data-ttu-id="db972-107">ASP.NET SignalR</span><span class="sxs-lookup"><span data-stu-id="db972-107">ASP.NET SignalR</span></span> | <span data-ttu-id="db972-108">SignalR platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="db972-108">ASP.NET Core SignalR</span></span> |
| -------------------- | --------------- | -------------------- |
| <span data-ttu-id="db972-109">Pakiet NuGet</span><span class="sxs-lookup"><span data-stu-id="db972-109">Server NuGet Package</span></span> | [<span data-ttu-id="db972-110">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="db972-110">Microsoft.AspNet.SignalR</span></span>](https://www.nuget.org/packages/Microsoft.AspNet.SignalR/) | <span data-ttu-id="db972-111">[Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) (.NET Core)</span><span class="sxs-lookup"><span data-stu-id="db972-111">[Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) (.NET Core)</span></span><br><span data-ttu-id="db972-112">[Microsoft.AspNetCore.SignalR](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) (.NET Framework)</span><span class="sxs-lookup"><span data-stu-id="db972-112">[Microsoft.AspNetCore.SignalR](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) (.NET Framework)</span></span> |
| <span data-ttu-id="db972-113">Pakiety NuGet klienta</span><span class="sxs-lookup"><span data-stu-id="db972-113">Client NuGet Packages</span></span> | [<span data-ttu-id="db972-114">Microsoft.AspNet.SignalR.Client</span><span class="sxs-lookup"><span data-stu-id="db972-114">Microsoft.AspNet.SignalR.Client</span></span>](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.Client/)<br>[<span data-ttu-id="db972-115">Microsoft.AspNet.SignalR.JS</span><span class="sxs-lookup"><span data-stu-id="db972-115">Microsoft.AspNet.SignalR.JS</span></span>](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.JS/) | [<span data-ttu-id="db972-116">Microsoft.AspNetCore.SignalR.Client</span><span class="sxs-lookup"><span data-stu-id="db972-116">Microsoft.AspNetCore.SignalR.Client</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client/) |
| <span data-ttu-id="db972-117">Klient npm pakietu</span><span class="sxs-lookup"><span data-stu-id="db972-117">Client npm Package</span></span> | [<span data-ttu-id="db972-118">signalr</span><span class="sxs-lookup"><span data-stu-id="db972-118">signalr</span></span>](https://www.npmjs.com/package/signalr) | [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) |
| <span data-ttu-id="db972-119">Typ aplikacji serwera</span><span class="sxs-lookup"><span data-stu-id="db972-119">Server App Type</span></span> | <span data-ttu-id="db972-120">ASP.NET (System.Web) lub samodzielnego hostowania OWIN</span><span class="sxs-lookup"><span data-stu-id="db972-120">ASP.NET (System.Web) or OWIN Self-Host</span></span> | <span data-ttu-id="db972-121">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="db972-121">ASP.NET Core</span></span> |
| <span data-ttu-id="db972-122">Platformy obsługiwane serwera</span><span class="sxs-lookup"><span data-stu-id="db972-122">Supported Server Platforms</span></span> | <span data-ttu-id="db972-123">.NET framework 4.5 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="db972-123">.NET Framework 4.5 or later</span></span> | <span data-ttu-id="db972-124">Program .NET framework 4.6.1 lub nowszej</span><span class="sxs-lookup"><span data-stu-id="db972-124">.NET Framework 4.6.1 or later</span></span><br><span data-ttu-id="db972-125">.NET core 2.1 lub nowszej</span><span class="sxs-lookup"><span data-stu-id="db972-125">.NET Core 2.1 or later</span></span> |

## <a name="feature-differences"></a><span data-ttu-id="db972-126">Różnice w funkcjach</span><span class="sxs-lookup"><span data-stu-id="db972-126">Feature differences</span></span>

### <a name="automatic-reconnects"></a><span data-ttu-id="db972-127">Automatyczne ponowne podłączenia</span><span class="sxs-lookup"><span data-stu-id="db972-127">Automatic reconnects</span></span>

<span data-ttu-id="db972-128">Automatyczne ponowne podłączenia nie są obsługiwane w biblioteki SignalR platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="db972-128">Automatic reconnects aren't supported in ASP.NET Core SignalR.</span></span> <span data-ttu-id="db972-129">Jeśli klient został odłączony, użytkownik jawnie należy uruchomić nowe połączenie, jeśli chcesz ponownie połączyć.</span><span class="sxs-lookup"><span data-stu-id="db972-129">If the client is disconnected, the user must explicitly start a new connection if they want to reconnect.</span></span> <span data-ttu-id="db972-130">W programie ASP.NET SignalR SignalR podejmie próbę połączenia z serwerem, jeśli połączenie zostało przerwane.</span><span class="sxs-lookup"><span data-stu-id="db972-130">In ASP.NET SignalR, SignalR attempts to reconnect to the server if the connection is dropped.</span></span> 

### <a name="protocol-support"></a><span data-ttu-id="db972-131">Obsługa protokołów</span><span class="sxs-lookup"><span data-stu-id="db972-131">Protocol support</span></span>

<span data-ttu-id="db972-132">Biblioteki SignalR platformy ASP.NET Core obsługuje JSON, a także nowy protokół binarny na podstawie [MessagePack](xref:signalr/messagepackhubprotocol).</span><span class="sxs-lookup"><span data-stu-id="db972-132">ASP.NET Core SignalR supports JSON, as well as a new binary protocol based on [MessagePack](xref:signalr/messagepackhubprotocol).</span></span> <span data-ttu-id="db972-133">Ponadto protokoły niestandardowe, mogą być tworzone.</span><span class="sxs-lookup"><span data-stu-id="db972-133">Additionally, custom protocols can be created.</span></span>

### <a name="transports"></a><span data-ttu-id="db972-134">Transporty</span><span class="sxs-lookup"><span data-stu-id="db972-134">Transports</span></span>

<span data-ttu-id="db972-135">Transport nieskończona ramki nie jest obsługiwany w biblioteki SignalR platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="db972-135">The Forever Frame transport is not supported in ASP.NET Core SignalR.</span></span>

## <a name="differences-on-the-server"></a><span data-ttu-id="db972-136">Różnice na serwerze</span><span class="sxs-lookup"><span data-stu-id="db972-136">Differences on the server</span></span>

<span data-ttu-id="db972-137">Biblioteki po stronie serwera biblioteki SignalR platformy ASP.NET Core są objęte [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) pakiet, który jest częścią **aplikacji sieci Web programu ASP.NET Core** szablon Razor i MVC projekty.</span><span class="sxs-lookup"><span data-stu-id="db972-137">The ASP.NET Core SignalR server-side libraries are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) package that's part of the **ASP.NET Core Web Application** template for both Razor and MVC projects.</span></span>

<span data-ttu-id="db972-138">Biblioteki SignalR platformy ASP.NET Core jest pośredniczące platformy ASP.NET Core, więc musi być skonfigurowany, wywołując [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="db972-138">ASP.NET Core SignalR is an ASP.NET Core middleware, so it must be configured by calling [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in `Startup.ConfigureServices`.</span></span>

```csharp
services.AddSignalR()
```

<span data-ttu-id="db972-139">Aby skonfigurować routing, mapy trasy do koncentratorów wewnątrz [UseSignalR](/dotnet/api/microsoft.aspnetcore.builder.signalrappbuilderextensions.usesignalr) wywołania metody, w `Startup.Configure` metody.</span><span class="sxs-lookup"><span data-stu-id="db972-139">To configure routing, map routes to hubs inside the [UseSignalR](/dotnet/api/microsoft.aspnetcore.builder.signalrappbuilderextensions.usesignalr) method call in the `Startup.Configure` method.</span></span>

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("/hub");
});
```

### <a name="sticky-sessions"></a><span data-ttu-id="db972-140">Trwałych sesji</span><span class="sxs-lookup"><span data-stu-id="db972-140">Sticky sessions</span></span>

<span data-ttu-id="db972-141">Model skalowania w poziomie SignalR platformy ASP.NET umożliwia klientom ponownego połączenia i wysyłanie komunikatów do dowolnego serwera w farmie.</span><span class="sxs-lookup"><span data-stu-id="db972-141">The scaleout model for ASP.NET SignalR allows clients to reconnect and send messages to any server in the farm.</span></span> <span data-ttu-id="db972-142">W biblioteki SignalR platformy ASP.NET Core klient musi korzystać z tego samego serwera na czas trwania połączenia.</span><span class="sxs-lookup"><span data-stu-id="db972-142">In ASP.NET Core SignalR, the client must interact with the same server for the duration of the connection.</span></span> <span data-ttu-id="db972-143">Do skalowania w poziomie przy użyciu pamięci podręcznej Redis oznacza to, że wymagane są trwałych sesji.</span><span class="sxs-lookup"><span data-stu-id="db972-143">For scaleout using Redis, that means sticky sessions are required.</span></span> <span data-ttu-id="db972-144">Do skalowania w poziomie przy użyciu [usługi Azure SignalR Service](/azure/azure-signalr/), trwałych sesji nie są wymagane, ponieważ usługa obsługuje połączenia z klientami.</span><span class="sxs-lookup"><span data-stu-id="db972-144">For scaleout using [Azure SignalR Service](/azure/azure-signalr/), sticky sessions are not required because the service handles connections to clients.</span></span> 

### <a name="single-hub-per-connection"></a><span data-ttu-id="db972-145">Jedno centrum dla połączenia</span><span class="sxs-lookup"><span data-stu-id="db972-145">Single hub per connection</span></span>

<span data-ttu-id="db972-146">W bibliotece SignalR platformy ASP.NET Core został uproszczony model połączenia.</span><span class="sxs-lookup"><span data-stu-id="db972-146">In ASP.NET Core SignalR, the connection model has been simplified.</span></span> <span data-ttu-id="db972-147">Połączenia są nawiązywane bezpośrednio z jednym Centrum, zamiast jednego połączenia używany do udostępnienia w wielu centrach.</span><span class="sxs-lookup"><span data-stu-id="db972-147">Connections are made directly to a single hub, rather than a single connection being used to share access to multiple hubs.</span></span>

### <a name="streaming"></a><span data-ttu-id="db972-148">Przesyłanie strumieniowe</span><span class="sxs-lookup"><span data-stu-id="db972-148">Streaming</span></span>

<span data-ttu-id="db972-149">Obsługuje teraz Core SignalR platformy ASP.NET [danych przesyłanych strumieniowo](xref:signalr/streaming) z koncentratora do klienta.</span><span class="sxs-lookup"><span data-stu-id="db972-149">ASP.NET Core SignalR now supports [streaming data](xref:signalr/streaming) from the hub to the client.</span></span>

### <a name="state"></a><span data-ttu-id="db972-150">Stan</span><span class="sxs-lookup"><span data-stu-id="db972-150">State</span></span>

<span data-ttu-id="db972-151">Możliwość przekazywania dowolny stan między klientami a Centrum (często nazywanej HubState) zostało usunięte, a także obsługę wiadomości dotyczące postępu.</span><span class="sxs-lookup"><span data-stu-id="db972-151">The ability to pass arbitrary state between clients and the hub (often called HubState) has been removed, as well as support for progress messages.</span></span> <span data-ttu-id="db972-152">Nie ma odpowiednika serwerów proxy koncentratora, w tym momencie.</span><span class="sxs-lookup"><span data-stu-id="db972-152">There is no counterpart of hub proxies at the moment.</span></span>

### <a name="persistentconnection-removal"></a><span data-ttu-id="db972-153">Usuwanie PersistentConnection</span><span class="sxs-lookup"><span data-stu-id="db972-153">PersistentConnection removal</span></span>

<span data-ttu-id="db972-154">W bibliotece SignalR platformy ASP.NET Core [PersistentConnection](https://docs.microsoft.com/previous-versions/aspnet/jj919047(v%3dvs.118)) klasa została usunięta.</span><span class="sxs-lookup"><span data-stu-id="db972-154">In ASP.NET Core SignalR, the [PersistentConnection](https://docs.microsoft.com/previous-versions/aspnet/jj919047(v%3dvs.118)) class has been removed.</span></span> 

### <a name="globalhost"></a><span data-ttu-id="db972-155">GlobalHost</span><span class="sxs-lookup"><span data-stu-id="db972-155">GlobalHost</span></span>

<span data-ttu-id="db972-156">Platforma ASP.NET Core ma wstrzykiwanie zależności (DI) wbudowane w platformę.</span><span class="sxs-lookup"><span data-stu-id="db972-156">ASP.NET Core has dependency injection (DI) built into the framework.</span></span> <span data-ttu-id="db972-157">Usługi umożliwiają dostęp DI [HubContext](xref:signalr/hubcontext).</span><span class="sxs-lookup"><span data-stu-id="db972-157">Services can use DI to access the [HubContext](xref:signalr/hubcontext).</span></span> <span data-ttu-id="db972-158">`GlobalHost` Obiekt, który jest używany w SignalR platformy ASP.NET można pobrać `HubContext` nie istnieje w biblioteki SignalR platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="db972-158">The `GlobalHost` object that is used in ASP.NET SignalR to get a `HubContext` doesn't exist in ASP.NET Core SignalR.</span></span>

### <a name="hubpipeline"></a><span data-ttu-id="db972-159">HubPipeline</span><span class="sxs-lookup"><span data-stu-id="db972-159">HubPipeline</span></span>

<span data-ttu-id="db972-160">Biblioteki SignalR platformy ASP.NET Core nie ma obsługi `HubPipeline` modułów.</span><span class="sxs-lookup"><span data-stu-id="db972-160">ASP.NET Core SignalR doesn't have support for `HubPipeline` modules.</span></span>

## <a name="differences-on-the-client"></a><span data-ttu-id="db972-161">Różnice na komputerze klienckim</span><span class="sxs-lookup"><span data-stu-id="db972-161">Differences on the client</span></span>

### <a name="typescript"></a><span data-ttu-id="db972-162">TypeScript</span><span class="sxs-lookup"><span data-stu-id="db972-162">TypeScript</span></span>

<span data-ttu-id="db972-163">Klient biblioteki SignalR platformy ASP.NET Core jest zapisywany [TypeScript](https://www.typescriptlang.org/).</span><span class="sxs-lookup"><span data-stu-id="db972-163">The ASP.NET Core SignalR client is written in [TypeScript](https://www.typescriptlang.org/).</span></span> <span data-ttu-id="db972-164">Można napisać w JavaScript lub TypeScript, korzystając z [klienta JavaScript](xref:signalr/javascript-client).</span><span class="sxs-lookup"><span data-stu-id="db972-164">You can write in JavaScript or TypeScript when using the [JavaScript client](xref:signalr/javascript-client).</span></span>

### <a name="the-javascript-client-is-hosted-at-npmhttpswwwnpmjscom"></a><span data-ttu-id="db972-165">Klient JavaScript znajduje się w [npm](https://www.npmjs.com/)</span><span class="sxs-lookup"><span data-stu-id="db972-165">The JavaScript client is hosted at [npm](https://www.npmjs.com/)</span></span>

<span data-ttu-id="db972-166">W poprzednich wersjach klienta JavaScript uzyskano za pośrednictwem pakietu NuGet w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="db972-166">In previous versions, the JavaScript client was obtained through a NuGet package in Visual Studio.</span></span> <span data-ttu-id="db972-167">Dla wersji podstawowej [ @aspnet/signalr ](https://www.npmjs.com/package/@aspnet/signalr) pakietu npm zawiera biblioteki języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="db972-167">For the Core versions, the [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) npm package contains the JavaScript libraries.</span></span> <span data-ttu-id="db972-168">Ten pakiet nie jest zawarty w **aplikacji sieci Web programu ASP.NET Core** szablonu.</span><span class="sxs-lookup"><span data-stu-id="db972-168">This package isn't included in the **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="db972-169">Aby uzyskać i zainstalować za pomocą usługi npm `@aspnet/signalr` pakietów Menedżera npm.</span><span class="sxs-lookup"><span data-stu-id="db972-169">Use npm to obtain and install the `@aspnet/signalr` npm package.</span></span>

```console
npm init -y
npm install @aspnet/signalr
```

### <a name="jquery"></a><span data-ttu-id="db972-170">jQuery</span><span class="sxs-lookup"><span data-stu-id="db972-170">jQuery</span></span>

<span data-ttu-id="db972-171">Zależność od jQuery zostało usunięte, jednak projekty można nadal używać jQuery.</span><span class="sxs-lookup"><span data-stu-id="db972-171">The dependency on jQuery has been removed, however projects can still use jQuery.</span></span>

### <a name="internet-explorer-support"></a><span data-ttu-id="db972-172">Obsługa programu Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="db972-172">Internet Explorer support</span></span>

<span data-ttu-id="db972-173">Biblioteki SignalR platformy ASP.NET Core wymaga programu Microsoft Internet Explorer 11 lub nowszy (ASP.NET SignalR obsługiwany program Microsoft Internet Explorer 8 lub nowszy).</span><span class="sxs-lookup"><span data-stu-id="db972-173">ASP.NET Core SignalR requires Microsoft Internet Explorer 11 or later (ASP.NET SignalR supported Microsoft Internet Explorer 8 and later).</span></span>

### <a name="javascript-client-method-syntax"></a><span data-ttu-id="db972-174">Składnia metody klient JavaScript</span><span class="sxs-lookup"><span data-stu-id="db972-174">JavaScript client method syntax</span></span>

<span data-ttu-id="db972-175">Składnia języka JavaScript został zmieniony względem poprzedniej wersji biblioteki SignalR.</span><span class="sxs-lookup"><span data-stu-id="db972-175">The JavaScript syntax has changed from the previous version of SignalR.</span></span> <span data-ttu-id="db972-176">Zamiast używania `$connection` obiektów, tworzenia połączenia za pomocą [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="db972-176">Rather than using the `$connection` object, create a connection using the [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) API.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/hub")
    .build();
```

<span data-ttu-id="db972-177">Użyj [na](/javascript/api/@aspnet/signalr/HubConnection#on) metodę, aby określić metody klienta, które można wywołać Centrum.</span><span class="sxs-lookup"><span data-stu-id="db972-177">Use the [on](/javascript/api/@aspnet/signalr/HubConnection#on) method to specify client methods that the hub can call.</span></span>

```javascript
connection.on("ReceiveMessage", (user, message) => {
    const msg = message.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;");
    const encodedMsg = user + " says " + msg;
    log(encodedMsg);
});
```

<span data-ttu-id="db972-178">Po utworzeniu metody klienta, uruchom połączenie koncentratora.</span><span class="sxs-lookup"><span data-stu-id="db972-178">After creating the client method, start the hub connection.</span></span> <span data-ttu-id="db972-179">Łańcuch [catch](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch) metodę, aby zalogować się lub obsługiwać błędy.</span><span class="sxs-lookup"><span data-stu-id="db972-179">Chain a [catch](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch) method to log or handle errors.</span></span>

```javascript
connection.start().catch(err => console.error(err.toString()));
```

### <a name="hub-proxies"></a><span data-ttu-id="db972-180">Serwery proxy Centrum</span><span class="sxs-lookup"><span data-stu-id="db972-180">Hub proxies</span></span>

<span data-ttu-id="db972-181">Nie będzie automatycznie są generowane, serwery proxy koncentratora.</span><span class="sxs-lookup"><span data-stu-id="db972-181">Hub proxies are no longer automatically generated.</span></span> <span data-ttu-id="db972-182">Zamiast tego Nazwa metody jest przekazywana do [wywołania](/javascript/api/%40aspnet/signalr/hubconnection#invoke) API jako ciąg.</span><span class="sxs-lookup"><span data-stu-id="db972-182">Instead, the method name is passed into the [invoke](/javascript/api/%40aspnet/signalr/hubconnection#invoke) API as a string.</span></span>

### <a name="net-and-other-clients"></a><span data-ttu-id="db972-183">.NET i innych klientów</span><span class="sxs-lookup"><span data-stu-id="db972-183">.NET and other clients</span></span>

<span data-ttu-id="db972-184">`Microsoft.AspNetCore.SignalR.Client` Pakietu NuGet zawiera biblioteki klienckie .NET dla biblioteki SignalR platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="db972-184">The `Microsoft.AspNetCore.SignalR.Client` NuGet package contains the .NET client libraries for ASP.NET Core SignalR.</span></span>

<span data-ttu-id="db972-185">Użyj [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder) utworzyć i skompilować wystąpienie połączenia z koncentratorem.</span><span class="sxs-lookup"><span data-stu-id="db972-185">Use the [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder) to create and build an instance of a connection to a hub.</span></span>

```csharp
connection = new HubConnectionBuilder()
    .WithUrl("url")
    .Build();
```

## <a name="scaleout-differences"></a><span data-ttu-id="db972-186">Różnice skalowania w poziomie</span><span class="sxs-lookup"><span data-stu-id="db972-186">Scaleout differences</span></span>

<span data-ttu-id="db972-187">Biblioteki SignalR platformy ASP.NET obsługuje program SQL Server i Redis.</span><span class="sxs-lookup"><span data-stu-id="db972-187">ASP.NET SignalR supports SQL Server and Redis.</span></span> <span data-ttu-id="db972-188">Biblioteki SignalR platformy ASP.NET Core obsługuje usługi Azure SignalR Service i Redis.</span><span class="sxs-lookup"><span data-stu-id="db972-188">ASP.NET Core SignalR supports Azure SignalR Service and Redis.</span></span>

### <a name="aspnet"></a><span data-ttu-id="db972-189">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="db972-189">ASP.NET</span></span>

* [<span data-ttu-id="db972-190">SignalR — skalowanie w poziomie za pomocą usługi Azure Service Bus</span><span class="sxs-lookup"><span data-stu-id="db972-190">SignalR scaleout with Azure Service Bus</span></span>](/aspnet/signalr/overview/performance/scaleout-with-windows-azure-service-bus)
* [<span data-ttu-id="db972-191">SignalR — skalowanie w poziomie przy użyciu usługi Redis</span><span class="sxs-lookup"><span data-stu-id="db972-191">SignalR scaleout with Redis</span></span>](/aspnet/signalr/overview/performance/scaleout-with-redis)
* [<span data-ttu-id="db972-192">SignalR — skalowanie w poziomie z programem SQL Server</span><span class="sxs-lookup"><span data-stu-id="db972-192">SignalR scaleout with SQL Server</span></span>](/aspnet/signalr/overview/performance/scaleout-with-sql-server)

### <a name="aspnet-core"></a><span data-ttu-id="db972-193">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="db972-193">ASP.NET Core</span></span>

* [<span data-ttu-id="db972-194">Usługi Azure SignalR Service</span><span class="sxs-lookup"><span data-stu-id="db972-194">Azure SignalR Service</span></span>](/azure/azure-signalr/)

## <a name="additional-resources"></a><span data-ttu-id="db972-195">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="db972-195">Additional resources</span></span>

* [<span data-ttu-id="db972-196">Centra</span><span class="sxs-lookup"><span data-stu-id="db972-196">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="db972-197">Klient JavaScript</span><span class="sxs-lookup"><span data-stu-id="db972-197">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="db972-198">Klient .NET</span><span class="sxs-lookup"><span data-stu-id="db972-198">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="db972-199">Obsługiwane platformy</span><span class="sxs-lookup"><span data-stu-id="db972-199">Supported platforms</span></span>](xref:signalr/supported-platforms)
