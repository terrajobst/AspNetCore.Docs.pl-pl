---
title: Różnice między SignalR i SignalR platformy ASP.NET Core
author: tdykstra
description: Różnice między SignalR i SignalR platformy ASP.NET Core
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.date: 06/30/2018
uid: signalr/version-differences
ms.openlocfilehash: 6ed7e2e1ecadef08d71c4d7a7c3469738d07bcda
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095011"
---
# <a name="differences-between-signalr-and-aspnet-core-signalr"></a><span data-ttu-id="f0c5e-103">Różnice między SignalR i SignalR platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f0c5e-103">Differences between SignalR and ASP.NET Core SignalR</span></span>

<span data-ttu-id="f0c5e-104">SignalR platformy ASP.NET Core nie jest zgodny z klientami lub serwerami na potrzeby biblioteki SignalR platformy ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="f0c5e-104">ASP.NET Core SignalR is not compatible with clients or servers for ASP.NET SignalR.</span></span> <span data-ttu-id="f0c5e-105">Ten artykuł szczegółowo opisuje funkcje, które zostały usunięte lub zmienione w SignalR platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f0c5e-105">This article details the features which have been removed or changed in the ASP.NET Core SignalR.</span></span>

## <a name="feature-differences"></a><span data-ttu-id="f0c5e-106">Różnice w funkcjach</span><span class="sxs-lookup"><span data-stu-id="f0c5e-106">Feature differences</span></span>

### <a name="automatic-reconnects"></a><span data-ttu-id="f0c5e-107">Automatyczne ponowne podłączenia</span><span class="sxs-lookup"><span data-stu-id="f0c5e-107">Automatic reconnects</span></span>

<span data-ttu-id="f0c5e-108">Automatyczne ponowne podłączenia nie są już obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="f0c5e-108">Automatic reconnects are no longer supported.</span></span> <span data-ttu-id="f0c5e-109">Wcześniej SignalR próbował ponownie połączyć z serwerem, jeśli połączenie zostało przerwane.</span><span class="sxs-lookup"><span data-stu-id="f0c5e-109">Previously, SignalR tried to reconnect to the server if the connection was dropped.</span></span> <span data-ttu-id="f0c5e-110">Jeśli klient został odłączony teraz użytkownik musi jawnie uruchomić nowe połączenie, jeśli chcą ponownie połączyć.</span><span class="sxs-lookup"><span data-stu-id="f0c5e-110">Now, if the client is disconnected the user must explicitly start a new connection if they want to reconnect.</span></span>

### <a name="protocol-support"></a><span data-ttu-id="f0c5e-111">Obsługa protokołów</span><span class="sxs-lookup"><span data-stu-id="f0c5e-111">Protocol support</span></span>

<span data-ttu-id="f0c5e-112">Biblioteki SignalR platformy ASP.NET Core obsługuje JSON, a także nowy protokół binarny na podstawie [MessagePack](xref:signalr/messagepackhubprotocol).</span><span class="sxs-lookup"><span data-stu-id="f0c5e-112">ASP.NET Core SignalR supports JSON, as well as a new binary protocol based on [MessagePack](xref:signalr/messagepackhubprotocol).</span></span> <span data-ttu-id="f0c5e-113">Ponadto protokoły niestandardowe, mogą być tworzone.</span><span class="sxs-lookup"><span data-stu-id="f0c5e-113">Additionally, custom protocols can be created.</span></span>

## <a name="differences-on-the-server"></a><span data-ttu-id="f0c5e-114">Różnice na serwerze</span><span class="sxs-lookup"><span data-stu-id="f0c5e-114">Differences on the server</span></span>

<span data-ttu-id="f0c5e-115">SignalR biblioteki po stronie serwera są zawarte w `Microsoft.AspNetCore.App` pakiet, który jest częścią **aplikacji sieci Web programu ASP.NET Core** szablonów dla projektów MVC i Razor.</span><span class="sxs-lookup"><span data-stu-id="f0c5e-115">The SignalR server-side libraries are included in the `Microsoft.AspNetCore.App` package that is part of the **ASP.NET Core Web Application** template for both Razor and MVC projects.</span></span>

<span data-ttu-id="f0c5e-116">SignalR to pośredniczące platformy ASP.NET Core, dlatego musi być skonfigurowany, wywołując `AddSignalR` w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="f0c5e-116">SignalR is an ASP.NET Core middleware, so it must be configured by calling `AddSignalR` in `Startup.ConfigureServices`.</span></span>

```csharp
services.AddSignalR();
```

<span data-ttu-id="f0c5e-117">Aby skonfigurować routing, mapy trasy do koncentratorów wewnątrz `UseSignalR` wywołania metody, w `Startup.Configure` metody.</span><span class="sxs-lookup"><span data-stu-id="f0c5e-117">To configure routing, map routes to hubs inside the `UseSignalR` method call in the `Startup.Configure` method.</span></span>

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("/hub");
});
```

### <a name="sticky-sessions-now-required"></a><span data-ttu-id="f0c5e-118">Trwałych sesji teraz wymagany</span><span class="sxs-lookup"><span data-stu-id="f0c5e-118">Sticky sessions now required</span></span>

<span data-ttu-id="f0c5e-119">Ze względu na sposób skalowania w poziomie pracy w poprzedniej wersji biblioteki SignalR klientów może ponownie połączyć, a następnie wysyłać komunikaty do dowolnego serwera w farmie.</span><span class="sxs-lookup"><span data-stu-id="f0c5e-119">Because of how scale-out worked in the previous versions of SignalR, clients could reconnect and send messages to any server in the farm.</span></span> <span data-ttu-id="f0c5e-120">Ze względu na zmiany modelu skalowalnego w poziomie, jak również nie obsługuje ponowne podłączenia to nie jest już obsługiwana.</span><span class="sxs-lookup"><span data-stu-id="f0c5e-120">Due to changes to the scale-out model, as well as not supporting reconnects, this is no longer supported.</span></span> <span data-ttu-id="f0c5e-121">Teraz gdy klient łączy się z serwerem musi korzystać z tego samego serwera dla czas trwania połączenia.</span><span class="sxs-lookup"><span data-stu-id="f0c5e-121">Now, once the client connects to the server it needs to interact with the same server for the duration of the connection.</span></span>

### <a name="single-hub-per-connection"></a><span data-ttu-id="f0c5e-122">Jedno centrum dla połączenia</span><span class="sxs-lookup"><span data-stu-id="f0c5e-122">Single hub per connection</span></span>

<span data-ttu-id="f0c5e-123">W bibliotece SignalR platformy ASP.NET Core został uproszczony model połączenia.</span><span class="sxs-lookup"><span data-stu-id="f0c5e-123">In ASP.NET Core SignalR, the connection model has been simplified.</span></span> <span data-ttu-id="f0c5e-124">Połączenia są nawiązywane bezpośrednio z jednym Centrum, zamiast jednego połączenia używany do udostępnienia w wielu centrach.</span><span class="sxs-lookup"><span data-stu-id="f0c5e-124">Connections are made directly to a single hub, rather than a single connection being used to share access to multiple hubs.</span></span>

### <a name="streaming"></a><span data-ttu-id="f0c5e-125">Przesyłanie strumieniowe</span><span class="sxs-lookup"><span data-stu-id="f0c5e-125">Streaming</span></span>

<span data-ttu-id="f0c5e-126">Obsługuje teraz SignalR [danych przesyłanych strumieniowo](xref:signalr/streaming) z koncentratora do klienta.</span><span class="sxs-lookup"><span data-stu-id="f0c5e-126">SignalR now supports [streaming data](xref:signalr/streaming) from the hub to the client.</span></span>

### <a name="state"></a><span data-ttu-id="f0c5e-127">Stan</span><span class="sxs-lookup"><span data-stu-id="f0c5e-127">State</span></span>

<span data-ttu-id="f0c5e-128">Możliwość przekazywania dowolny stan między klientami a Centrum (często nazywanej HubState) zostało usunięte, a także obsługę wiadomości dotyczące postępu.</span><span class="sxs-lookup"><span data-stu-id="f0c5e-128">The ability to pass arbitrary state between clients and the hub (often called HubState) has been removed, as well as support for progress messages.</span></span> <span data-ttu-id="f0c5e-129">Nie ma odpowiednika serwerów proxy koncentratora, w tym momencie.</span><span class="sxs-lookup"><span data-stu-id="f0c5e-129">There is no counterpart of hub proxies at the moment.</span></span>

## <a name="differences-on-the-client"></a><span data-ttu-id="f0c5e-130">Różnice na komputerze klienckim</span><span class="sxs-lookup"><span data-stu-id="f0c5e-130">Differences on the client</span></span>

### <a name="typescript"></a><span data-ttu-id="f0c5e-131">TypeScript</span><span class="sxs-lookup"><span data-stu-id="f0c5e-131">TypeScript</span></span>

<span data-ttu-id="f0c5e-132">Wersja SignalR platformy ASP.NET Core jest zapisywany [TypeScript](https://www.typescriptlang.org/).</span><span class="sxs-lookup"><span data-stu-id="f0c5e-132">The ASP.NET Core version of SignalR is written in [TypeScript](https://www.typescriptlang.org/).</span></span> <span data-ttu-id="f0c5e-133">Można napisać w JavaScript lub TypeScript, korzystając z [klienta JavaScript](xref:signalr/javascript-client).</span><span class="sxs-lookup"><span data-stu-id="f0c5e-133">You can write in JavaScript or TypeScript when using the [JavaScript client](xref:signalr/javascript-client).</span></span>

### <a name="the-javascript-client-is-hosted-at-npmhttpswwwnpmjscom"></a><span data-ttu-id="f0c5e-134">Klient JavaScript znajduje się w [npm](https://www.npmjs.com/)</span><span class="sxs-lookup"><span data-stu-id="f0c5e-134">The JavaScript client is hosted at [npm](https://www.npmjs.com/)</span></span>

<span data-ttu-id="f0c5e-135">W poprzednich wersjach klienta JavaScript uzyskano za pośrednictwem pakietu NuGet w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f0c5e-135">In previous versions, the JavaScript client was obtained through a NuGet package in Visual Studio.</span></span> <span data-ttu-id="f0c5e-136">Dla wersji podstawowej [ @aspnet/signalr pakietu npm](https://www.npmjs.com/package/@aspnet/signalr) zawiera biblioteki języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="f0c5e-136">For the Core versions, the [@aspnet/signalr npm package](https://www.npmjs.com/package/@aspnet/signalr) contains the JavaScript libraries.</span></span> <span data-ttu-id="f0c5e-137">Ten pakiet nie jest zawarty w **aplikacji sieci Web programu ASP.NET Core** szablonu.</span><span class="sxs-lookup"><span data-stu-id="f0c5e-137">This package isn't included in the **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="f0c5e-138">Aby uzyskać i zainstalować za pomocą usługi npm `@aspnet/signalr` pakietów Menedżera npm.</span><span class="sxs-lookup"><span data-stu-id="f0c5e-138">Use npm to obtain and install the `@aspnet/signalr` npm package.</span></span>

```console
npm init -y
npm install @aspnet/signalr
```

### <a name="jquery"></a><span data-ttu-id="f0c5e-139">jQuery</span><span class="sxs-lookup"><span data-stu-id="f0c5e-139">jQuery</span></span>

<span data-ttu-id="f0c5e-140">Zależność od jQuery zostało usunięte, jednak projekty można nadal używać jQuery.</span><span class="sxs-lookup"><span data-stu-id="f0c5e-140">The dependency on jQuery has been removed, however projects can still use jQuery.</span></span>

### <a name="javascript-client-method-syntax"></a><span data-ttu-id="f0c5e-141">Składnia metody klient JavaScript</span><span class="sxs-lookup"><span data-stu-id="f0c5e-141">JavaScript client method syntax</span></span>

<span data-ttu-id="f0c5e-142">Składnia języka JavaScript został zmieniony względem poprzedniej wersji biblioteki SignalR.</span><span class="sxs-lookup"><span data-stu-id="f0c5e-142">The JavaScript syntax has changed from the previous version of SignalR.</span></span> <span data-ttu-id="f0c5e-143">Zamiast używania `$connection` obiektów, tworzenia połączenia za pomocą `HubConnectionBuilder` interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="f0c5e-143">Rather than using the `$connection` object, create a connection using the `HubConnectionBuilder` API.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/hub")
    .build();
```

<span data-ttu-id="f0c5e-144">Użyj `connection.on` do określenia metody klienta, które można wywołać Centrum.</span><span class="sxs-lookup"><span data-stu-id="f0c5e-144">Use `connection.on` to specify client methods that the hub can call.</span></span>

```javascript
connection.on("ReceiveMessage", (user, message) => {
    const msg = message.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;");
    const encodedMsg = user + " says " + msg;
    log(encodedMsg);
});
```

<span data-ttu-id="f0c5e-145">Po utworzeniu metody klienta, uruchom połączenie koncentratora.</span><span class="sxs-lookup"><span data-stu-id="f0c5e-145">After creating the client method, start the hub connection.</span></span> <span data-ttu-id="f0c5e-146">Łańcuch `catch` metodę, aby zalogować się lub obsługiwać błędy.</span><span class="sxs-lookup"><span data-stu-id="f0c5e-146">Chain a `catch` method to log or handle errors.</span></span>

```javascript
connection.start().catch(err => console.error(err.toString()));
```

### <a name="hub-proxies"></a><span data-ttu-id="f0c5e-147">Serwery proxy Centrum</span><span class="sxs-lookup"><span data-stu-id="f0c5e-147">Hub proxies</span></span>

<span data-ttu-id="f0c5e-148">Nie będzie automatycznie są generowane, serwery proxy koncentratora.</span><span class="sxs-lookup"><span data-stu-id="f0c5e-148">Hub proxies are no longer automatically generated.</span></span> <span data-ttu-id="f0c5e-149">Zamiast tego Nazwa metody jest przekazywana do `invoke` API jako ciąg.</span><span class="sxs-lookup"><span data-stu-id="f0c5e-149">Instead, the method name is passed into the `invoke` API as a string.</span></span>

### <a name="net-and-other-clients"></a><span data-ttu-id="f0c5e-150">.NET i innych klientów</span><span class="sxs-lookup"><span data-stu-id="f0c5e-150">.NET and other clients</span></span>

<span data-ttu-id="f0c5e-151">`Microsoft.AspNetCore.SignalR.Client` Pakietu NuGet zawiera biblioteki klienckie .NET dla biblioteki SignalR platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f0c5e-151">The `Microsoft.AspNetCore.SignalR.Client` NuGet package contains the .NET client libraries for ASP.NET Core SignalR.</span></span>

<span data-ttu-id="f0c5e-152">Użyj `HubConnectionBuilder` utworzyć i skompilować wystąpienie połączenia z koncentratorem.</span><span class="sxs-lookup"><span data-stu-id="f0c5e-152">Use the `HubConnectionBuilder` to create and build an instance of a connection to a hub.</span></span>

```csharp
connection = new HubConnectionBuilder()
    .WithUrl("url")
    .Build();
```

## <a name="additional-resources"></a><span data-ttu-id="f0c5e-153">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="f0c5e-153">Additional resources</span></span>

* [<span data-ttu-id="f0c5e-154">Centra</span><span class="sxs-lookup"><span data-stu-id="f0c5e-154">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="f0c5e-155">Klient JavaScript</span><span class="sxs-lookup"><span data-stu-id="f0c5e-155">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="f0c5e-156">Klient .NET</span><span class="sxs-lookup"><span data-stu-id="f0c5e-156">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="f0c5e-157">Obsługiwane platformy</span><span class="sxs-lookup"><span data-stu-id="f0c5e-157">Supported platforms</span></span>](xref:signalr/supported-platforms)
