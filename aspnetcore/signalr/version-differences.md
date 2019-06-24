---
title: Różnice między SignalR i SignalR platformy ASP.NET Core
author: bradygaster
description: Różnice między SignalR i SignalR platformy ASP.NET Core
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.date: 11/14/2018
uid: signalr/version-differences
ms.openlocfilehash: d140becaed5b4fc765b7a72571c7f5dce7f4e3d4
ms.sourcegitcommit: 28646e8ca62fb094db1557b5c0c02d5b45531824
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/23/2019
ms.locfileid: "67333446"
---
# <a name="differences-between-aspnet-signalr-and-aspnet-core-signalr"></a><span data-ttu-id="49746-103">Różnice między biblioteki SignalR platformy ASP.NET i SignalR platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="49746-103">Differences between ASP.NET SignalR and ASP.NET Core SignalR</span></span>

<span data-ttu-id="49746-104">SignalR platformy ASP.NET Core nie jest zgodny z klientami lub serwerami na potrzeby biblioteki SignalR platformy ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="49746-104">ASP.NET Core SignalR isn't compatible with clients or servers for ASP.NET SignalR.</span></span> <span data-ttu-id="49746-105">Ten artykuł szczegółowo opisuje funkcje, które zostały usunięte lub zmienione w biblioteki SignalR platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="49746-105">This article details features which have been removed or changed in ASP.NET Core SignalR.</span></span>

## <a name="how-to-identify-the-signalr-version"></a><span data-ttu-id="49746-106">Jak zidentyfikować wersji biblioteki SignalR</span><span class="sxs-lookup"><span data-stu-id="49746-106">How to identify the SignalR version</span></span>

|                      | <span data-ttu-id="49746-107">ASP.NET SignalR</span><span class="sxs-lookup"><span data-stu-id="49746-107">ASP.NET SignalR</span></span> | <span data-ttu-id="49746-108">SignalR platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="49746-108">ASP.NET Core SignalR</span></span> |
| -------------------- | --------------- | -------------------- |
| <span data-ttu-id="49746-109">Pakiet NuGet</span><span class="sxs-lookup"><span data-stu-id="49746-109">Server NuGet Package</span></span> | [<span data-ttu-id="49746-110">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="49746-110">Microsoft.AspNet.SignalR</span></span>](https://www.nuget.org/packages/Microsoft.AspNet.SignalR/) | <span data-ttu-id="49746-111">[Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) (.NET Core)</span><span class="sxs-lookup"><span data-stu-id="49746-111">[Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) (.NET Core)</span></span><br><span data-ttu-id="49746-112">[Microsoft.AspNetCore.SignalR](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) (.NET Framework)</span><span class="sxs-lookup"><span data-stu-id="49746-112">[Microsoft.AspNetCore.SignalR](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) (.NET Framework)</span></span> |
| <span data-ttu-id="49746-113">Pakiety NuGet klienta</span><span class="sxs-lookup"><span data-stu-id="49746-113">Client NuGet Packages</span></span> | [<span data-ttu-id="49746-114">Microsoft.AspNet.SignalR.Client</span><span class="sxs-lookup"><span data-stu-id="49746-114">Microsoft.AspNet.SignalR.Client</span></span>](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.Client/)<br>[<span data-ttu-id="49746-115">Microsoft.AspNet.SignalR.JS</span><span class="sxs-lookup"><span data-stu-id="49746-115">Microsoft.AspNet.SignalR.JS</span></span>](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.JS/) | [<span data-ttu-id="49746-116">Microsoft.AspNetCore.SignalR.Client</span><span class="sxs-lookup"><span data-stu-id="49746-116">Microsoft.AspNetCore.SignalR.Client</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client/) |
| <span data-ttu-id="49746-117">Klient npm pakietu</span><span class="sxs-lookup"><span data-stu-id="49746-117">Client npm Package</span></span> | [<span data-ttu-id="49746-118">signalr</span><span class="sxs-lookup"><span data-stu-id="49746-118">signalr</span></span>](https://www.npmjs.com/package/signalr) | [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) |
| <span data-ttu-id="49746-119">Klienta Java</span><span class="sxs-lookup"><span data-stu-id="49746-119">Java Client</span></span> | <span data-ttu-id="49746-120">[Repozytorium GitHub](https://github.com/SignalR/java-client) (przestarzałe)</span><span class="sxs-lookup"><span data-stu-id="49746-120">[GitHub Repository](https://github.com/SignalR/java-client) (deprecated)</span></span>  | <span data-ttu-id="49746-121">Pakiet maven [com.microsoft.signalr](https://search.maven.org/artifact/com.microsoft.signalr/signalr)</span><span class="sxs-lookup"><span data-stu-id="49746-121">Maven package [com.microsoft.signalr](https://search.maven.org/artifact/com.microsoft.signalr/signalr)</span></span> |
| <span data-ttu-id="49746-122">Typ aplikacji serwera</span><span class="sxs-lookup"><span data-stu-id="49746-122">Server App Type</span></span> | <span data-ttu-id="49746-123">ASP.NET (System.Web) lub samodzielnego hostowania OWIN</span><span class="sxs-lookup"><span data-stu-id="49746-123">ASP.NET (System.Web) or OWIN Self-Host</span></span> | <span data-ttu-id="49746-124">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="49746-124">ASP.NET Core</span></span> |
| <span data-ttu-id="49746-125">Platformy obsługiwane serwera</span><span class="sxs-lookup"><span data-stu-id="49746-125">Supported Server Platforms</span></span> | <span data-ttu-id="49746-126">.NET framework 4.5 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="49746-126">.NET Framework 4.5 or later</span></span> | <span data-ttu-id="49746-127">Program .NET framework 4.6.1 lub nowszej</span><span class="sxs-lookup"><span data-stu-id="49746-127">.NET Framework 4.6.1 or later</span></span><br><span data-ttu-id="49746-128">.NET core 2.1 lub nowszej</span><span class="sxs-lookup"><span data-stu-id="49746-128">.NET Core 2.1 or later</span></span> |

## <a name="feature-differences"></a><span data-ttu-id="49746-129">Różnice w funkcjach</span><span class="sxs-lookup"><span data-stu-id="49746-129">Feature differences</span></span>

### <a name="automatic-reconnects"></a><span data-ttu-id="49746-130">Automatyczne ponowne podłączenia</span><span class="sxs-lookup"><span data-stu-id="49746-130">Automatic reconnects</span></span>

<span data-ttu-id="49746-131">Automatyczne ponowne podłączenia nie są obsługiwane w biblioteki SignalR platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="49746-131">Automatic reconnects aren't supported in ASP.NET Core SignalR.</span></span> <span data-ttu-id="49746-132">Jeśli klient został odłączony, użytkownik jawnie należy uruchomić nowe połączenie, jeśli chcesz ponownie połączyć.</span><span class="sxs-lookup"><span data-stu-id="49746-132">If the client is disconnected, the user must explicitly start a new connection if they want to reconnect.</span></span> <span data-ttu-id="49746-133">W programie ASP.NET SignalR SignalR podejmie próbę połączenia z serwerem, jeśli połączenie zostało przerwane.</span><span class="sxs-lookup"><span data-stu-id="49746-133">In ASP.NET SignalR, SignalR attempts to reconnect to the server if the connection is dropped.</span></span> 

### <a name="protocol-support"></a><span data-ttu-id="49746-134">Obsługa protokołów</span><span class="sxs-lookup"><span data-stu-id="49746-134">Protocol support</span></span>

<span data-ttu-id="49746-135">Biblioteki SignalR platformy ASP.NET Core obsługuje JSON, a także nowy protokół binarny na podstawie [MessagePack](xref:signalr/messagepackhubprotocol).</span><span class="sxs-lookup"><span data-stu-id="49746-135">ASP.NET Core SignalR supports JSON, as well as a new binary protocol based on [MessagePack](xref:signalr/messagepackhubprotocol).</span></span> <span data-ttu-id="49746-136">Ponadto protokoły niestandardowe, mogą być tworzone.</span><span class="sxs-lookup"><span data-stu-id="49746-136">Additionally, custom protocols can be created.</span></span>

### <a name="transports"></a><span data-ttu-id="49746-137">Transporty</span><span class="sxs-lookup"><span data-stu-id="49746-137">Transports</span></span>

<span data-ttu-id="49746-138">Transport nieskończona ramki nie jest obsługiwany w biblioteki SignalR platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="49746-138">The Forever Frame transport is not supported in ASP.NET Core SignalR.</span></span>

## <a name="differences-on-the-server"></a><span data-ttu-id="49746-139">Różnice na serwerze</span><span class="sxs-lookup"><span data-stu-id="49746-139">Differences on the server</span></span>

<span data-ttu-id="49746-140">Biblioteki po stronie serwera biblioteki SignalR platformy ASP.NET Core są objęte [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) pakiet, który jest częścią **aplikacji sieci Web programu ASP.NET Core** szablon Razor i MVC projekty.</span><span class="sxs-lookup"><span data-stu-id="49746-140">The ASP.NET Core SignalR server-side libraries are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) package that's part of the **ASP.NET Core Web Application** template for both Razor and MVC projects.</span></span>

<span data-ttu-id="49746-141">Biblioteki SignalR platformy ASP.NET Core jest pośredniczące platformy ASP.NET Core, więc musi być skonfigurowany, wywołując [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="49746-141">ASP.NET Core SignalR is an ASP.NET Core middleware, so it must be configured by calling [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in `Startup.ConfigureServices`.</span></span>

```csharp
services.AddSignalR()
```

<span data-ttu-id="49746-142">Aby skonfigurować routing, mapy trasy do koncentratorów wewnątrz [UseSignalR](/dotnet/api/microsoft.aspnetcore.builder.signalrappbuilderextensions.usesignalr) wywołania metody, w `Startup.Configure` metody.</span><span class="sxs-lookup"><span data-stu-id="49746-142">To configure routing, map routes to hubs inside the [UseSignalR](/dotnet/api/microsoft.aspnetcore.builder.signalrappbuilderextensions.usesignalr) method call in the `Startup.Configure` method.</span></span>

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("/hub");
});
```

### <a name="sticky-sessions"></a><span data-ttu-id="49746-143">Trwałych sesji</span><span class="sxs-lookup"><span data-stu-id="49746-143">Sticky sessions</span></span>

<span data-ttu-id="49746-144">Model skalowania w poziomie SignalR platformy ASP.NET umożliwia klientom ponownego połączenia i wysyłanie komunikatów do dowolnego serwera w farmie.</span><span class="sxs-lookup"><span data-stu-id="49746-144">The scaleout model for ASP.NET SignalR allows clients to reconnect and send messages to any server in the farm.</span></span> <span data-ttu-id="49746-145">W biblioteki SignalR platformy ASP.NET Core klient musi korzystać z tego samego serwera na czas trwania połączenia.</span><span class="sxs-lookup"><span data-stu-id="49746-145">In ASP.NET Core SignalR, the client must interact with the same server for the duration of the connection.</span></span> <span data-ttu-id="49746-146">Do skalowania w poziomie przy użyciu pamięci podręcznej Redis oznacza to, że wymagane są trwałych sesji.</span><span class="sxs-lookup"><span data-stu-id="49746-146">For scaleout using Redis, that means sticky sessions are required.</span></span> <span data-ttu-id="49746-147">Do skalowania w poziomie przy użyciu [usługi Azure SignalR Service](/azure/azure-signalr/), trwałych sesji nie są wymagane, ponieważ usługa obsługuje połączenia z klientami.</span><span class="sxs-lookup"><span data-stu-id="49746-147">For scaleout using [Azure SignalR Service](/azure/azure-signalr/), sticky sessions are not required because the service handles connections to clients.</span></span> 

### <a name="single-hub-per-connection"></a><span data-ttu-id="49746-148">Jedno centrum dla połączenia</span><span class="sxs-lookup"><span data-stu-id="49746-148">Single hub per connection</span></span>

<span data-ttu-id="49746-149">W bibliotece SignalR platformy ASP.NET Core został uproszczony model połączenia.</span><span class="sxs-lookup"><span data-stu-id="49746-149">In ASP.NET Core SignalR, the connection model has been simplified.</span></span> <span data-ttu-id="49746-150">Połączenia są nawiązywane bezpośrednio z jednym Centrum, zamiast jednego połączenia używany do udostępnienia w wielu centrach.</span><span class="sxs-lookup"><span data-stu-id="49746-150">Connections are made directly to a single hub, rather than a single connection being used to share access to multiple hubs.</span></span>

### <a name="streaming"></a><span data-ttu-id="49746-151">Przesyłanie strumieniowe</span><span class="sxs-lookup"><span data-stu-id="49746-151">Streaming</span></span>

<span data-ttu-id="49746-152">Obsługuje teraz Core SignalR platformy ASP.NET [danych przesyłanych strumieniowo](xref:signalr/streaming) z koncentratora do klienta.</span><span class="sxs-lookup"><span data-stu-id="49746-152">ASP.NET Core SignalR now supports [streaming data](xref:signalr/streaming) from the hub to the client.</span></span>

### <a name="state"></a><span data-ttu-id="49746-153">Stan</span><span class="sxs-lookup"><span data-stu-id="49746-153">State</span></span>

<span data-ttu-id="49746-154">Możliwość przekazywania dowolny stan między klientami a Centrum (często nazywanej HubState) zostało usunięte, a także obsługę wiadomości dotyczące postępu.</span><span class="sxs-lookup"><span data-stu-id="49746-154">The ability to pass arbitrary state between clients and the hub (often called HubState) has been removed, as well as support for progress messages.</span></span> <span data-ttu-id="49746-155">Nie ma odpowiednika serwerów proxy koncentratora, w tym momencie.</span><span class="sxs-lookup"><span data-stu-id="49746-155">There is no counterpart of hub proxies at the moment.</span></span>

### <a name="persistentconnection-removal"></a><span data-ttu-id="49746-156">Usuwanie PersistentConnection</span><span class="sxs-lookup"><span data-stu-id="49746-156">PersistentConnection removal</span></span>

<span data-ttu-id="49746-157">W bibliotece SignalR platformy ASP.NET Core [PersistentConnection](https://docs.microsoft.com/previous-versions/aspnet/jj919047(v%3dvs.118)) klasa została usunięta.</span><span class="sxs-lookup"><span data-stu-id="49746-157">In ASP.NET Core SignalR, the [PersistentConnection](https://docs.microsoft.com/previous-versions/aspnet/jj919047(v%3dvs.118)) class has been removed.</span></span> 

### <a name="globalhost"></a><span data-ttu-id="49746-158">GlobalHost</span><span class="sxs-lookup"><span data-stu-id="49746-158">GlobalHost</span></span>

<span data-ttu-id="49746-159">Platforma ASP.NET Core ma wstrzykiwanie zależności (DI) wbudowane w platformę.</span><span class="sxs-lookup"><span data-stu-id="49746-159">ASP.NET Core has dependency injection (DI) built into the framework.</span></span> <span data-ttu-id="49746-160">Usługi umożliwiają dostęp DI [HubContext](xref:signalr/hubcontext).</span><span class="sxs-lookup"><span data-stu-id="49746-160">Services can use DI to access the [HubContext](xref:signalr/hubcontext).</span></span> <span data-ttu-id="49746-161">`GlobalHost` Obiekt, który jest używany w SignalR platformy ASP.NET można pobrać `HubContext` nie istnieje w biblioteki SignalR platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="49746-161">The `GlobalHost` object that is used in ASP.NET SignalR to get a `HubContext` doesn't exist in ASP.NET Core SignalR.</span></span>

### <a name="hubpipeline"></a><span data-ttu-id="49746-162">HubPipeline</span><span class="sxs-lookup"><span data-stu-id="49746-162">HubPipeline</span></span>

<span data-ttu-id="49746-163">Biblioteki SignalR platformy ASP.NET Core nie ma obsługi `HubPipeline` modułów.</span><span class="sxs-lookup"><span data-stu-id="49746-163">ASP.NET Core SignalR doesn't have support for `HubPipeline` modules.</span></span>

## <a name="differences-on-the-client"></a><span data-ttu-id="49746-164">Różnice na komputerze klienckim</span><span class="sxs-lookup"><span data-stu-id="49746-164">Differences on the client</span></span>

### <a name="typescript"></a><span data-ttu-id="49746-165">TypeScript</span><span class="sxs-lookup"><span data-stu-id="49746-165">TypeScript</span></span>

<span data-ttu-id="49746-166">Klient biblioteki SignalR platformy ASP.NET Core jest zapisywany [TypeScript](https://www.typescriptlang.org/).</span><span class="sxs-lookup"><span data-stu-id="49746-166">The ASP.NET Core SignalR client is written in [TypeScript](https://www.typescriptlang.org/).</span></span> <span data-ttu-id="49746-167">Można napisać w JavaScript lub TypeScript, korzystając z [klienta JavaScript](xref:signalr/javascript-client).</span><span class="sxs-lookup"><span data-stu-id="49746-167">You can write in JavaScript or TypeScript when using the [JavaScript client](xref:signalr/javascript-client).</span></span>

### <a name="the-javascript-client-is-hosted-at-npmhttpswwwnpmjscom"></a><span data-ttu-id="49746-168">Klient JavaScript znajduje się w [npm](https://www.npmjs.com/)</span><span class="sxs-lookup"><span data-stu-id="49746-168">The JavaScript client is hosted at [npm](https://www.npmjs.com/)</span></span>

<span data-ttu-id="49746-169">W poprzednich wersjach klienta JavaScript uzyskano za pośrednictwem pakietu NuGet w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="49746-169">In previous versions, the JavaScript client was obtained through a NuGet package in Visual Studio.</span></span> <span data-ttu-id="49746-170">Dla wersji podstawowej [ @aspnet/signalr ](https://www.npmjs.com/package/@aspnet/signalr) pakietu npm zawiera biblioteki języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="49746-170">For the Core versions, the [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) npm package contains the JavaScript libraries.</span></span> <span data-ttu-id="49746-171">Ten pakiet nie jest zawarty w **aplikacji sieci Web programu ASP.NET Core** szablonu.</span><span class="sxs-lookup"><span data-stu-id="49746-171">This package isn't included in the **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="49746-172">Aby uzyskać i zainstalować za pomocą usługi npm `@aspnet/signalr` pakietów Menedżera npm.</span><span class="sxs-lookup"><span data-stu-id="49746-172">Use npm to obtain and install the `@aspnet/signalr` npm package.</span></span>

```console
npm init -y
npm install @aspnet/signalr
```

### <a name="jquery"></a><span data-ttu-id="49746-173">jQuery</span><span class="sxs-lookup"><span data-stu-id="49746-173">jQuery</span></span>

<span data-ttu-id="49746-174">Zależność od jQuery zostało usunięte, jednak projekty można nadal używać jQuery.</span><span class="sxs-lookup"><span data-stu-id="49746-174">The dependency on jQuery has been removed, however projects can still use jQuery.</span></span>

### <a name="internet-explorer-support"></a><span data-ttu-id="49746-175">Obsługa programu Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="49746-175">Internet Explorer support</span></span>

<span data-ttu-id="49746-176">Biblioteki SignalR platformy ASP.NET Core wymaga programu Microsoft Internet Explorer 11 lub nowszy (ASP.NET SignalR obsługiwany program Microsoft Internet Explorer 8 lub nowszy).</span><span class="sxs-lookup"><span data-stu-id="49746-176">ASP.NET Core SignalR requires Microsoft Internet Explorer 11 or later (ASP.NET SignalR supported Microsoft Internet Explorer 8 and later).</span></span>

### <a name="javascript-client-method-syntax"></a><span data-ttu-id="49746-177">Składnia metody klient JavaScript</span><span class="sxs-lookup"><span data-stu-id="49746-177">JavaScript client method syntax</span></span>

<span data-ttu-id="49746-178">Składnia języka JavaScript został zmieniony względem poprzedniej wersji biblioteki SignalR.</span><span class="sxs-lookup"><span data-stu-id="49746-178">The JavaScript syntax has changed from the previous version of SignalR.</span></span> <span data-ttu-id="49746-179">Zamiast używania `$connection` obiektów, tworzenia połączenia za pomocą [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="49746-179">Rather than using the `$connection` object, create a connection using the [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) API.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/hub")
    .build();
```

<span data-ttu-id="49746-180">Użyj [na](/javascript/api/@aspnet/signalr/HubConnection#on) metodę, aby określić metody klienta, które można wywołać Centrum.</span><span class="sxs-lookup"><span data-stu-id="49746-180">Use the [on](/javascript/api/@aspnet/signalr/HubConnection#on) method to specify client methods that the hub can call.</span></span>

```javascript
connection.on("ReceiveMessage", (user, message) => {
    const msg = message.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;");
    const encodedMsg = user + " says " + msg;
    log(encodedMsg);
});
```

<span data-ttu-id="49746-181">Po utworzeniu metody klienta, uruchom połączenie koncentratora.</span><span class="sxs-lookup"><span data-stu-id="49746-181">After creating the client method, start the hub connection.</span></span> <span data-ttu-id="49746-182">Łańcuch [catch](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch) metodę, aby zalogować się lub obsługiwać błędy.</span><span class="sxs-lookup"><span data-stu-id="49746-182">Chain a [catch](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch) method to log or handle errors.</span></span>

```javascript
connection.start().catch(err => console.error(err.toString()));
```

### <a name="hub-proxies"></a><span data-ttu-id="49746-183">Serwery proxy Centrum</span><span class="sxs-lookup"><span data-stu-id="49746-183">Hub proxies</span></span>

<span data-ttu-id="49746-184">Nie będzie automatycznie są generowane, serwery proxy koncentratora.</span><span class="sxs-lookup"><span data-stu-id="49746-184">Hub proxies are no longer automatically generated.</span></span> <span data-ttu-id="49746-185">Zamiast tego Nazwa metody jest przekazywana do [wywołania](/javascript/api/%40aspnet/signalr/hubconnection#invoke) API jako ciąg.</span><span class="sxs-lookup"><span data-stu-id="49746-185">Instead, the method name is passed into the [invoke](/javascript/api/%40aspnet/signalr/hubconnection#invoke) API as a string.</span></span>

### <a name="net-and-other-clients"></a><span data-ttu-id="49746-186">.NET i innych klientów</span><span class="sxs-lookup"><span data-stu-id="49746-186">.NET and other clients</span></span>

<span data-ttu-id="49746-187">`Microsoft.AspNetCore.SignalR.Client` Pakietu NuGet zawiera biblioteki klienckie .NET dla biblioteki SignalR platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="49746-187">The `Microsoft.AspNetCore.SignalR.Client` NuGet package contains the .NET client libraries for ASP.NET Core SignalR.</span></span>

<span data-ttu-id="49746-188">Użyj [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder) utworzyć i skompilować wystąpienie połączenia z koncentratorem.</span><span class="sxs-lookup"><span data-stu-id="49746-188">Use the [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder) to create and build an instance of a connection to a hub.</span></span>

```csharp
connection = new HubConnectionBuilder()
    .WithUrl("url")
    .Build();
```

## <a name="scaleout-differences"></a><span data-ttu-id="49746-189">Różnice skalowania w poziomie</span><span class="sxs-lookup"><span data-stu-id="49746-189">Scaleout differences</span></span>

<span data-ttu-id="49746-190">Biblioteki SignalR platformy ASP.NET obsługuje program SQL Server i Redis.</span><span class="sxs-lookup"><span data-stu-id="49746-190">ASP.NET SignalR supports SQL Server and Redis.</span></span> <span data-ttu-id="49746-191">Biblioteki SignalR platformy ASP.NET Core obsługuje usługi Azure SignalR Service i Redis.</span><span class="sxs-lookup"><span data-stu-id="49746-191">ASP.NET Core SignalR supports Azure SignalR Service and Redis.</span></span>

### <a name="aspnet"></a><span data-ttu-id="49746-192">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="49746-192">ASP.NET</span></span>

* [<span data-ttu-id="49746-193">SignalR — skalowanie w poziomie za pomocą usługi Azure Service Bus</span><span class="sxs-lookup"><span data-stu-id="49746-193">SignalR scaleout with Azure Service Bus</span></span>](/aspnet/signalr/overview/performance/scaleout-with-windows-azure-service-bus)
* [<span data-ttu-id="49746-194">SignalR — skalowanie w poziomie przy użyciu usługi Redis</span><span class="sxs-lookup"><span data-stu-id="49746-194">SignalR scaleout with Redis</span></span>](/aspnet/signalr/overview/performance/scaleout-with-redis)
* [<span data-ttu-id="49746-195">SignalR — skalowanie w poziomie z programem SQL Server</span><span class="sxs-lookup"><span data-stu-id="49746-195">SignalR scaleout with SQL Server</span></span>](/aspnet/signalr/overview/performance/scaleout-with-sql-server)

### <a name="aspnet-core"></a><span data-ttu-id="49746-196">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="49746-196">ASP.NET Core</span></span>

* [<span data-ttu-id="49746-197">Usługa Azure SignalR Service</span><span class="sxs-lookup"><span data-stu-id="49746-197">Azure SignalR Service</span></span>](/azure/azure-signalr/)
* [<span data-ttu-id="49746-198">Redis Backplane</span><span class="sxs-lookup"><span data-stu-id="49746-198">Redis Backplane</span></span>](xref:signalr/redis-backplane)

## <a name="additional-resources"></a><span data-ttu-id="49746-199">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="49746-199">Additional resources</span></span>

* [<span data-ttu-id="49746-200">Centra</span><span class="sxs-lookup"><span data-stu-id="49746-200">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="49746-201">Klient JavaScript</span><span class="sxs-lookup"><span data-stu-id="49746-201">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="49746-202">Klient .NET</span><span class="sxs-lookup"><span data-stu-id="49746-202">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="49746-203">Obsługiwane platformy</span><span class="sxs-lookup"><span data-stu-id="49746-203">Supported platforms</span></span>](xref:signalr/supported-platforms)
