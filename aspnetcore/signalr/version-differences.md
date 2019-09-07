---
title: Różnice między sygnalizującym i ASP.NET Core sygnalizującym
author: bradygaster
description: Różnice między sygnalizującym i ASP.NET Core sygnalizującym
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.date: 11/14/2018
uid: signalr/version-differences
ms.openlocfilehash: 70b09493d9b4c96c897465d60e53e93a793c42f9
ms.sourcegitcommit: 387cf29f5d5addef2cbc70670a11d612806b36b2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/06/2019
ms.locfileid: "70746537"
---
# <a name="differences-between-aspnet-signalr-and-aspnet-core-signalr"></a><span data-ttu-id="68813-103">Różnice między sygnalizującym ASP.NET a ASP.NET Core sygnalizującym</span><span class="sxs-lookup"><span data-stu-id="68813-103">Differences between ASP.NET SignalR and ASP.NET Core SignalR</span></span>

<span data-ttu-id="68813-104">ASP.NET Core sygnalizujący nie jest zgodny z klientami lub serwerami dla sygnalizującego ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="68813-104">ASP.NET Core SignalR isn't compatible with clients or servers for ASP.NET SignalR.</span></span> <span data-ttu-id="68813-105">Ten artykuł zawiera szczegółowe informacje o funkcjach, które zostały usunięte lub zmienione w programie ASP.NET Core Signaler.</span><span class="sxs-lookup"><span data-stu-id="68813-105">This article details features which have been removed or changed in ASP.NET Core SignalR.</span></span>

## <a name="how-to-identify-the-signalr-version"></a><span data-ttu-id="68813-106">Jak zidentyfikować wersję sygnalizującego</span><span class="sxs-lookup"><span data-stu-id="68813-106">How to identify the SignalR version</span></span>

|                      | <span data-ttu-id="68813-107">ASP.NET SignalR</span><span class="sxs-lookup"><span data-stu-id="68813-107">ASP.NET SignalR</span></span> | <span data-ttu-id="68813-108">ASP.NET Core sygnalizujący</span><span class="sxs-lookup"><span data-stu-id="68813-108">ASP.NET Core SignalR</span></span> |
| -------------------- | --------------- | -------------------- |
| <span data-ttu-id="68813-109">Pakiet NuGet serwera</span><span class="sxs-lookup"><span data-stu-id="68813-109">Server NuGet Package</span></span> | [<span data-ttu-id="68813-110">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="68813-110">Microsoft.AspNet.SignalR</span></span>](https://www.nuget.org/packages/Microsoft.AspNet.SignalR/) | <span data-ttu-id="68813-111">[Microsoft. AspNetCore. app](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) (.NET Core)</span><span class="sxs-lookup"><span data-stu-id="68813-111">[Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) (.NET Core)</span></span><br><span data-ttu-id="68813-112">[Microsoft.AspNetCore.SignalR](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) (.NET Framework)</span><span class="sxs-lookup"><span data-stu-id="68813-112">[Microsoft.AspNetCore.SignalR](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) (.NET Framework)</span></span> |
| <span data-ttu-id="68813-113">Pakiety NuGet klienta</span><span class="sxs-lookup"><span data-stu-id="68813-113">Client NuGet Packages</span></span> | [<span data-ttu-id="68813-114">Microsoft.AspNet.SignalR.Client</span><span class="sxs-lookup"><span data-stu-id="68813-114">Microsoft.AspNet.SignalR.Client</span></span>](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.Client/)<br>[<span data-ttu-id="68813-115">Microsoft.AspNet.SignalR.JS</span><span class="sxs-lookup"><span data-stu-id="68813-115">Microsoft.AspNet.SignalR.JS</span></span>](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.JS/) | [<span data-ttu-id="68813-116">Microsoft.AspNetCore.SignalR.Client</span><span class="sxs-lookup"><span data-stu-id="68813-116">Microsoft.AspNetCore.SignalR.Client</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client/) |
| <span data-ttu-id="68813-117">Pakiet npm klienta</span><span class="sxs-lookup"><span data-stu-id="68813-117">Client npm Package</span></span> | [<span data-ttu-id="68813-118">SignalR</span><span class="sxs-lookup"><span data-stu-id="68813-118">signalr</span></span>](https://www.npmjs.com/package/signalr) | [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) |
| <span data-ttu-id="68813-119">Klient Java</span><span class="sxs-lookup"><span data-stu-id="68813-119">Java Client</span></span> | <span data-ttu-id="68813-120">[Repozytorium GitHub](https://github.com/SignalR/java-client) przestarzałe</span><span class="sxs-lookup"><span data-stu-id="68813-120">[GitHub Repository](https://github.com/SignalR/java-client) (deprecated)</span></span>  | <span data-ttu-id="68813-121">Maven pakietu [com. Microsoft. Signal](https://search.maven.org/artifact/com.microsoft.signalr/signalr)</span><span class="sxs-lookup"><span data-stu-id="68813-121">Maven package [com.microsoft.signalr](https://search.maven.org/artifact/com.microsoft.signalr/signalr)</span></span> |
| <span data-ttu-id="68813-122">Typ aplikacji serwera</span><span class="sxs-lookup"><span data-stu-id="68813-122">Server App Type</span></span> | <span data-ttu-id="68813-123">ASP.NET (System. Web) lub samoobsługowy OWIN</span><span class="sxs-lookup"><span data-stu-id="68813-123">ASP.NET (System.Web) or OWIN Self-Host</span></span> | <span data-ttu-id="68813-124">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="68813-124">ASP.NET Core</span></span> |
| <span data-ttu-id="68813-125">Obsługiwane platformy serwera</span><span class="sxs-lookup"><span data-stu-id="68813-125">Supported Server Platforms</span></span> | <span data-ttu-id="68813-126">.NET Framework 4,5 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="68813-126">.NET Framework 4.5 or later</span></span> | <span data-ttu-id="68813-127">.NET Framework 4.6.1 lub nowsze</span><span class="sxs-lookup"><span data-stu-id="68813-127">.NET Framework 4.6.1 or later</span></span><br><span data-ttu-id="68813-128">.NET Core 2,1 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="68813-128">.NET Core 2.1 or later</span></span> |

## <a name="feature-differences"></a><span data-ttu-id="68813-129">Różnice w funkcji</span><span class="sxs-lookup"><span data-stu-id="68813-129">Feature differences</span></span>

### <a name="automatic-reconnects"></a><span data-ttu-id="68813-130">Automatyczne ponowne łączenie</span><span class="sxs-lookup"><span data-stu-id="68813-130">Automatic reconnects</span></span>

<span data-ttu-id="68813-131">Automatyczne ponowne łączenie nie są obsługiwane w usłudze ASP.NET Core Signaler.</span><span class="sxs-lookup"><span data-stu-id="68813-131">Automatic reconnects aren't supported in ASP.NET Core SignalR.</span></span> <span data-ttu-id="68813-132">Jeśli klient zostanie odłączony, użytkownik musi jawnie rozpocząć nowe połączenie, jeśli chce ponownie nawiązać połączenie.</span><span class="sxs-lookup"><span data-stu-id="68813-132">If the client is disconnected, the user must explicitly start a new connection if they want to reconnect.</span></span> <span data-ttu-id="68813-133">W ASP.NET sygnalizujący, sygnalizujący próbuje ponownie nawiązać połączenie z serwerem, jeśli połączenie zostanie zerwane.</span><span class="sxs-lookup"><span data-stu-id="68813-133">In ASP.NET SignalR, SignalR attempts to reconnect to the server if the connection is dropped.</span></span>

### <a name="protocol-support"></a><span data-ttu-id="68813-134">Obsługa protokołu</span><span class="sxs-lookup"><span data-stu-id="68813-134">Protocol support</span></span>

<span data-ttu-id="68813-135">ASP.NET Core sygnalizujący obsługuje kod JSON, a także nowy protokół binarny na podstawie [MessagePack](xref:signalr/messagepackhubprotocol).</span><span class="sxs-lookup"><span data-stu-id="68813-135">ASP.NET Core SignalR supports JSON, as well as a new binary protocol based on [MessagePack](xref:signalr/messagepackhubprotocol).</span></span> <span data-ttu-id="68813-136">Dodatkowo można utworzyć niestandardowe protokoły.</span><span class="sxs-lookup"><span data-stu-id="68813-136">Additionally, custom protocols can be created.</span></span>

### <a name="transports"></a><span data-ttu-id="68813-137">Transporty</span><span class="sxs-lookup"><span data-stu-id="68813-137">Transports</span></span>

<span data-ttu-id="68813-138">Transport ramki bez ograniczeń nie jest obsługiwany w przypadku ASP.NET Core sygnalizującego.</span><span class="sxs-lookup"><span data-stu-id="68813-138">The Forever Frame transport is not supported in ASP.NET Core SignalR.</span></span>

## <a name="differences-on-the-server"></a><span data-ttu-id="68813-139">Różnice na serwerze</span><span class="sxs-lookup"><span data-stu-id="68813-139">Differences on the server</span></span>

<span data-ttu-id="68813-140">Biblioteki po stronie serwera dla sygnałów ASP.NET Core są zawarte w pakiecie [Microsoft. AspNetCore. app pakietu aplikacji](xref:fundamentals/metapackage-app) , który jest częścią szablonu **ASP.NET Core aplikacji sieci Web** dla projektów Razor i MVC.</span><span class="sxs-lookup"><span data-stu-id="68813-140">The ASP.NET Core SignalR server-side libraries are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) package that's part of the **ASP.NET Core Web Application** template for both Razor and MVC projects.</span></span>

<span data-ttu-id="68813-141">ASP.NET Core sygnalizujący to ASP.NET Core oprogramowanie pośredniczące, dlatego należy je skonfigurować przez wywołanie funkcji [Addsignaler](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="68813-141">ASP.NET Core SignalR is an ASP.NET Core middleware, so it must be configured by calling [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in `Startup.ConfigureServices`.</span></span>

```csharp
services.AddSignalR()
```

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="68813-142">Aby skonfigurować Routing, Mapuj trasy do centrów wewnątrz wywołania metody [UseEndpoints](/dotnet/api/microsoft.aspnetcore.builder.endpointroutingapplicationbuilderextensions.useendpoints) w `Startup.Configure` metodzie.</span><span class="sxs-lookup"><span data-stu-id="68813-142">To configure routing, map routes to hubs inside the [UseEndpoints](/dotnet/api/microsoft.aspnetcore.builder.endpointroutingapplicationbuilderextensions.useendpoints) method call in the `Startup.Configure` method.</span></span>


```csharp
app.UseRouting();

app.UseEndpoints(endpoints =>
{
    endpoints.MapHub<ChatHub>("/hub");
});
```

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="68813-143">Aby skonfigurować Routing, Mapuj trasy do centrów wewnątrz wywołania metody [UseSignalR](/dotnet/api/microsoft.aspnetcore.builder.signalrappbuilderextensions.usesignalr) w `Startup.Configure` metodzie.</span><span class="sxs-lookup"><span data-stu-id="68813-143">To configure routing, map routes to hubs inside the [UseSignalR](/dotnet/api/microsoft.aspnetcore.builder.signalrappbuilderextensions.usesignalr) method call in the `Startup.Configure` method.</span></span>

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("/hub");
});
```

::: moniker-end

### <a name="sticky-sessions"></a><span data-ttu-id="68813-144">Sesje programu Sticky</span><span class="sxs-lookup"><span data-stu-id="68813-144">Sticky sessions</span></span>

<span data-ttu-id="68813-145">Model skalowania dla sygnalizującego ASP.NET umożliwia klientom Ponowne nawiązywanie połączenia i wysyłanie komunikatów do dowolnego serwera w farmie.</span><span class="sxs-lookup"><span data-stu-id="68813-145">The scaleout model for ASP.NET SignalR allows clients to reconnect and send messages to any server in the farm.</span></span> <span data-ttu-id="68813-146">W ASP.NET Core sygnalizujący klient musi korzystać z tego samego serwera na czas trwania połączenia.</span><span class="sxs-lookup"><span data-stu-id="68813-146">In ASP.NET Core SignalR, the client must interact with the same server for the duration of the connection.</span></span> <span data-ttu-id="68813-147">W przypadku skalowania przy użyciu Redis, oznacza to, że są wymagane sesje programu Sticky.</span><span class="sxs-lookup"><span data-stu-id="68813-147">For scaleout using Redis, that means sticky sessions are required.</span></span> <span data-ttu-id="68813-148">W przypadku skalowania za pomocą [usługi Azure Signal](/azure/azure-signalr/)Session sesje nie są wymagane, ponieważ usługa obsługuje połączenia z klientami.</span><span class="sxs-lookup"><span data-stu-id="68813-148">For scaleout using [Azure SignalR Service](/azure/azure-signalr/), sticky sessions are not required because the service handles connections to clients.</span></span>

### <a name="single-hub-per-connection"></a><span data-ttu-id="68813-149">Jedno centrum na połączenie</span><span class="sxs-lookup"><span data-stu-id="68813-149">Single hub per connection</span></span>

<span data-ttu-id="68813-150">W ASP.NET Core sygnalizujący model połączenia został uproszczony.</span><span class="sxs-lookup"><span data-stu-id="68813-150">In ASP.NET Core SignalR, the connection model has been simplified.</span></span> <span data-ttu-id="68813-151">Połączenia są nawiązywane bezpośrednio w jednym centrum, a nie za pomocą jednego połączenia, które jest używane do udostępniania dostępu do wielu centrów.</span><span class="sxs-lookup"><span data-stu-id="68813-151">Connections are made directly to a single hub, rather than a single connection being used to share access to multiple hubs.</span></span>

### <a name="streaming"></a><span data-ttu-id="68813-152">Przesyłanie strumieniowe</span><span class="sxs-lookup"><span data-stu-id="68813-152">Streaming</span></span>

<span data-ttu-id="68813-153">ASP.NET Core sygnalizujący teraz obsługuje [przesyłanie strumieniowe danych](xref:signalr/streaming) z centrum do klienta programu.</span><span class="sxs-lookup"><span data-stu-id="68813-153">ASP.NET Core SignalR now supports [streaming data](xref:signalr/streaming) from the hub to the client.</span></span>

### <a name="state"></a><span data-ttu-id="68813-154">Stan</span><span class="sxs-lookup"><span data-stu-id="68813-154">State</span></span>

<span data-ttu-id="68813-155">Możliwość przekazania dowolnego stanu między klientami a centrum (często nazywane HubState) została usunięta, a także do obsługi komunikatów o postępie.</span><span class="sxs-lookup"><span data-stu-id="68813-155">The ability to pass arbitrary state between clients and the hub (often called HubState) has been removed, as well as support for progress messages.</span></span> <span data-ttu-id="68813-156">W tej chwili nie ma żadnego odpowiednika serwerów proxy centrum.</span><span class="sxs-lookup"><span data-stu-id="68813-156">There is no counterpart of hub proxies at the moment.</span></span>

### <a name="persistentconnection-removal"></a><span data-ttu-id="68813-157">Usuwanie PersistentConnection</span><span class="sxs-lookup"><span data-stu-id="68813-157">PersistentConnection removal</span></span>

<span data-ttu-id="68813-158">W ASP.NET Core sygnalizujący Klasa [PersistentConnection](https://docs.microsoft.com/previous-versions/aspnet/jj919047(v%3dvs.118)) została usunięta.</span><span class="sxs-lookup"><span data-stu-id="68813-158">In ASP.NET Core SignalR, the [PersistentConnection](https://docs.microsoft.com/previous-versions/aspnet/jj919047(v%3dvs.118)) class has been removed.</span></span>

### <a name="globalhost"></a><span data-ttu-id="68813-159">GlobalHost</span><span class="sxs-lookup"><span data-stu-id="68813-159">GlobalHost</span></span>

<span data-ttu-id="68813-160">ASP.NET Core ma iniekcję zależności (DI) wbudowaną w strukturę.</span><span class="sxs-lookup"><span data-stu-id="68813-160">ASP.NET Core has dependency injection (DI) built into the framework.</span></span> <span data-ttu-id="68813-161">Usługi mogą używać DI do uzyskiwania dostępu do [HubContext](xref:signalr/hubcontext).</span><span class="sxs-lookup"><span data-stu-id="68813-161">Services can use DI to access the [HubContext](xref:signalr/hubcontext).</span></span> <span data-ttu-id="68813-162">Obiekt, który jest używany w ASP.NET sygnalizujący, aby uzyskać `HubContext` , nie istnieje w sygnalizacji ASP.NET Core. `GlobalHost`</span><span class="sxs-lookup"><span data-stu-id="68813-162">The `GlobalHost` object that is used in ASP.NET SignalR to get a `HubContext` doesn't exist in ASP.NET Core SignalR.</span></span>

### <a name="hubpipeline"></a><span data-ttu-id="68813-163">HubPipeline</span><span class="sxs-lookup"><span data-stu-id="68813-163">HubPipeline</span></span>

<span data-ttu-id="68813-164">ASP.NET Core sygnalizujący nie obsługuje `HubPipeline` modułów.</span><span class="sxs-lookup"><span data-stu-id="68813-164">ASP.NET Core SignalR doesn't have support for `HubPipeline` modules.</span></span>

## <a name="differences-on-the-client"></a><span data-ttu-id="68813-165">Różnice dotyczące klienta</span><span class="sxs-lookup"><span data-stu-id="68813-165">Differences on the client</span></span>

### <a name="typescript"></a><span data-ttu-id="68813-166">TypeScript</span><span class="sxs-lookup"><span data-stu-id="68813-166">TypeScript</span></span>

<span data-ttu-id="68813-167">Klient ASP.NET Core sygnalizujący jest zapisywana w języku [TypeScript](https://www.typescriptlang.org/).</span><span class="sxs-lookup"><span data-stu-id="68813-167">The ASP.NET Core SignalR client is written in [TypeScript](https://www.typescriptlang.org/).</span></span> <span data-ttu-id="68813-168">Podczas korzystania z [klienta JavaScript](xref:signalr/javascript-client)można pisać w języku JavaScript lub TypeScript.</span><span class="sxs-lookup"><span data-stu-id="68813-168">You can write in JavaScript or TypeScript when using the [JavaScript client](xref:signalr/javascript-client).</span></span>

### <a name="the-javascript-client-is-hosted-at-npmhttpswwwnpmjscom"></a><span data-ttu-id="68813-169">Klient JavaScript jest hostowany w [npm](https://www.npmjs.com/)</span><span class="sxs-lookup"><span data-stu-id="68813-169">The JavaScript client is hosted at [npm](https://www.npmjs.com/)</span></span>

<span data-ttu-id="68813-170">W poprzednich wersjach klient JavaScript został uzyskany za pomocą pakietu NuGet w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="68813-170">In previous versions, the JavaScript client was obtained through a NuGet package in Visual Studio.</span></span> <span data-ttu-id="68813-171">W przypadku wersji [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) podstawowych pakiet npm zawiera biblioteki JavaScript.</span><span class="sxs-lookup"><span data-stu-id="68813-171">For the Core versions, the [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) npm package contains the JavaScript libraries.</span></span> <span data-ttu-id="68813-172">Ten pakiet nie jest uwzględniony w szablonie **ASP.NET Core aplikacji sieci Web** .</span><span class="sxs-lookup"><span data-stu-id="68813-172">This package isn't included in the **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="68813-173">Użyj npm, aby uzyskać i zainstalować `@aspnet/signalr` pakiet npm.</span><span class="sxs-lookup"><span data-stu-id="68813-173">Use npm to obtain and install the `@aspnet/signalr` npm package.</span></span>

```console
npm init -y
npm install @aspnet/signalr
```

### <a name="jquery"></a><span data-ttu-id="68813-174">jQuery</span><span class="sxs-lookup"><span data-stu-id="68813-174">jQuery</span></span>

<span data-ttu-id="68813-175">Zależność od jQuery została usunięta, jednak projekty nadal mogą korzystać z jQuery.</span><span class="sxs-lookup"><span data-stu-id="68813-175">The dependency on jQuery has been removed, however projects can still use jQuery.</span></span>

### <a name="internet-explorer-support"></a><span data-ttu-id="68813-176">Obsługa programu Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="68813-176">Internet Explorer support</span></span>

<span data-ttu-id="68813-177">ASP.NET Core sygnalizujący wymaga programu Microsoft Internet Explorer 11 lub nowszego (ASP.NET sygnalizujący obsługiwaną przez program Microsoft Internet Explorer 8 i nowsze).</span><span class="sxs-lookup"><span data-stu-id="68813-177">ASP.NET Core SignalR requires Microsoft Internet Explorer 11 or later (ASP.NET SignalR supported Microsoft Internet Explorer 8 and later).</span></span>

### <a name="javascript-client-method-syntax"></a><span data-ttu-id="68813-178">Składnia metody klienta JavaScript</span><span class="sxs-lookup"><span data-stu-id="68813-178">JavaScript client method syntax</span></span>

<span data-ttu-id="68813-179">Składnia języka JavaScript została zmieniona z poprzedniej wersji usługi sygnalizującej.</span><span class="sxs-lookup"><span data-stu-id="68813-179">The JavaScript syntax has changed from the previous version of SignalR.</span></span> <span data-ttu-id="68813-180">Zamiast używać `$connection` obiektu, Utwórz połączenie przy użyciu interfejsu API [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) .</span><span class="sxs-lookup"><span data-stu-id="68813-180">Rather than using the `$connection` object, create a connection using the [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) API.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/hub")
    .build();
```

<span data-ttu-id="68813-181">Użyj metody [on](/javascript/api/@aspnet/signalr/HubConnection#on) , aby określić metody klienta, które mogą być wywoływane przez centrum.</span><span class="sxs-lookup"><span data-stu-id="68813-181">Use the [on](/javascript/api/@aspnet/signalr/HubConnection#on) method to specify client methods that the hub can call.</span></span>

```javascript
connection.on("ReceiveMessage", (user, message) => {
    const msg = message.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;");
    const encodedMsg = user + " says " + msg;
    log(encodedMsg);
});
```

<span data-ttu-id="68813-182">Po utworzeniu metody klienta należy uruchomić połączenie z centrum.</span><span class="sxs-lookup"><span data-stu-id="68813-182">After creating the client method, start the hub connection.</span></span> <span data-ttu-id="68813-183">Tworzenie łańcucha metod [przechwytywania](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch) w celu rejestrowania lub obsługi błędów.</span><span class="sxs-lookup"><span data-stu-id="68813-183">Chain a [catch](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch) method to log or handle errors.</span></span>

```javascript
connection.start().catch(err => console.error(err.toString()));
```

### <a name="hub-proxies"></a><span data-ttu-id="68813-184">Serwery proxy centrum</span><span class="sxs-lookup"><span data-stu-id="68813-184">Hub proxies</span></span>

<span data-ttu-id="68813-185">Serwery proxy centrum nie są już generowane automatycznie.</span><span class="sxs-lookup"><span data-stu-id="68813-185">Hub proxies are no longer automatically generated.</span></span> <span data-ttu-id="68813-186">Zamiast tego nazwa metody jest przenoszona do [wywoływania](/javascript/api/%40aspnet/signalr/hubconnection#invoke) API jako ciąg.</span><span class="sxs-lookup"><span data-stu-id="68813-186">Instead, the method name is passed into the [invoke](/javascript/api/%40aspnet/signalr/hubconnection#invoke) API as a string.</span></span>

### <a name="net-and-other-clients"></a><span data-ttu-id="68813-187">.NET i inni klienci</span><span class="sxs-lookup"><span data-stu-id="68813-187">.NET and other clients</span></span>

<span data-ttu-id="68813-188">Pakiet `Microsoft.AspNetCore.SignalR.Client` NuGet zawiera biblioteki klienckie platformy .NET dla sygnalizującego ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="68813-188">The `Microsoft.AspNetCore.SignalR.Client` NuGet package contains the .NET client libraries for ASP.NET Core SignalR.</span></span>

<span data-ttu-id="68813-189">Użyj [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder) , aby utworzyć i skompilować wystąpienie połączenia z koncentratorem.</span><span class="sxs-lookup"><span data-stu-id="68813-189">Use the [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder) to create and build an instance of a connection to a hub.</span></span>

```csharp
connection = new HubConnectionBuilder()
    .WithUrl("url")
    .Build();
```

## <a name="scaleout-differences"></a><span data-ttu-id="68813-190">Różnice skalowania</span><span class="sxs-lookup"><span data-stu-id="68813-190">Scaleout differences</span></span>

<span data-ttu-id="68813-191">ASP.NET sygnalizujący obsługuje SQL Server i Redis.</span><span class="sxs-lookup"><span data-stu-id="68813-191">ASP.NET SignalR supports SQL Server and Redis.</span></span> <span data-ttu-id="68813-192">ASP.NET Core sygnalizujący obsługuje usługę Azure Signal Service i Redis.</span><span class="sxs-lookup"><span data-stu-id="68813-192">ASP.NET Core SignalR supports Azure SignalR Service and Redis.</span></span>

### <a name="aspnet"></a><span data-ttu-id="68813-193">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="68813-193">ASP.NET</span></span>

* [<span data-ttu-id="68813-194">Skalowania sygnalizujący z Azure Service Bus</span><span class="sxs-lookup"><span data-stu-id="68813-194">SignalR scaleout with Azure Service Bus</span></span>](/aspnet/signalr/overview/performance/scaleout-with-windows-azure-service-bus)
* [<span data-ttu-id="68813-195">Skalowania sygnalizujący z Redis</span><span class="sxs-lookup"><span data-stu-id="68813-195">SignalR scaleout with Redis</span></span>](/aspnet/signalr/overview/performance/scaleout-with-redis)
* [<span data-ttu-id="68813-196">Skalowania sygnalizujący z SQL Server</span><span class="sxs-lookup"><span data-stu-id="68813-196">SignalR scaleout with SQL Server</span></span>](/aspnet/signalr/overview/performance/scaleout-with-sql-server)

### <a name="aspnet-core"></a><span data-ttu-id="68813-197">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="68813-197">ASP.NET Core</span></span>

* [<span data-ttu-id="68813-198">Usługa Azure SignalR Service</span><span class="sxs-lookup"><span data-stu-id="68813-198">Azure SignalR Service</span></span>](/azure/azure-signalr/)
* [<span data-ttu-id="68813-199">Redis</span><span class="sxs-lookup"><span data-stu-id="68813-199">Redis Backplane</span></span>](xref:signalr/redis-backplane)

## <a name="additional-resources"></a><span data-ttu-id="68813-200">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="68813-200">Additional resources</span></span>

* [<span data-ttu-id="68813-201">Centra</span><span class="sxs-lookup"><span data-stu-id="68813-201">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="68813-202">Klient JavaScript</span><span class="sxs-lookup"><span data-stu-id="68813-202">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="68813-203">Klient .NET</span><span class="sxs-lookup"><span data-stu-id="68813-203">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="68813-204">Obsługiwane platformy</span><span class="sxs-lookup"><span data-stu-id="68813-204">Supported platforms</span></span>](xref:signalr/supported-platforms)
