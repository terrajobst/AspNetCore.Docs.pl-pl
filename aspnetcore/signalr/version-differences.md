---
title: Różnice między SignalR i ASP.NET Core SignalR
author: bradygaster
description: Różnice między SignalR i ASP.NET Core SignalR
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.date: 11/12/2019
no-loc:
- SignalR
uid: signalr/version-differences
ms.openlocfilehash: 0f644c132b0fcf9a0ecf0ab181791a6477c97f76
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/12/2019
ms.locfileid: "73963720"
---
# <a name="differences-between-aspnet-opno-locsignalr-and-aspnet-core-opno-locsignalr"></a><span data-ttu-id="79615-103">Różnice między ASP.NET SignalR i ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="79615-103">Differences between ASP.NET SignalR and ASP.NET Core SignalR</span></span>

<span data-ttu-id="79615-104">SignalR ASP.NET Core nie są zgodne z klientami lub serwerami ASP.NET SignalR.</span><span class="sxs-lookup"><span data-stu-id="79615-104">ASP.NET Core SignalR isn't compatible with clients or servers for ASP.NET SignalR.</span></span> <span data-ttu-id="79615-105">W tym artykule opisano funkcje, które zostały usunięte lub zmienione w SignalRASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="79615-105">This article details features which have been removed or changed in ASP.NET Core SignalR.</span></span>

## <a name="how-to-identify-the-opno-locsignalr-version"></a><span data-ttu-id="79615-106">Jak zidentyfikować wersję SignalR</span><span class="sxs-lookup"><span data-stu-id="79615-106">How to identify the SignalR version</span></span>

|                      | <span data-ttu-id="79615-107">ASP.NET SignalR</span><span class="sxs-lookup"><span data-stu-id="79615-107">ASP.NET SignalR</span></span> | <span data-ttu-id="79615-108">ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="79615-108">ASP.NET Core SignalR</span></span> |
| -------------------- | --------------- | -------------------- |
| <span data-ttu-id="79615-109">Pakiet NuGet serwera</span><span class="sxs-lookup"><span data-stu-id="79615-109">Server NuGet Package</span></span> | <span data-ttu-id="79615-110">[Microsoft. AspNet.SignalR](https://www.nuget.org/packages/Microsoft.AspNet.SignalR/)</span><span class="sxs-lookup"><span data-stu-id="79615-110">[Microsoft.AspNet.SignalR](https://www.nuget.org/packages/Microsoft.AspNet.SignalR/)</span></span> | <span data-ttu-id="79615-111">[Microsoft. AspNetCore. app](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) (.NET Core)</span><span class="sxs-lookup"><span data-stu-id="79615-111">[Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) (.NET Core)</span></span><br><span data-ttu-id="79615-112">[Microsoft. AspNetCore.SignalR](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/)</span><span class="sxs-lookup"><span data-stu-id="79615-112">[Microsoft.AspNetCore.SignalR](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/)</span></span> <span data-ttu-id="79615-113">(.NET Framework)</span><span class="sxs-lookup"><span data-stu-id="79615-113">(.NET Framework)</span></span> |
| <span data-ttu-id="79615-114">Pakiety NuGet klienta</span><span class="sxs-lookup"><span data-stu-id="79615-114">Client NuGet Packages</span></span> | <span data-ttu-id="79615-115">[Microsoft. AspNet.SignalR. Klient](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.Client/)</span><span class="sxs-lookup"><span data-stu-id="79615-115">[Microsoft.AspNet.SignalR.Client](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.Client/)</span></span><br><span data-ttu-id="79615-116">[Microsoft. AspNet.SignalR. JS](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.JS/)</span><span class="sxs-lookup"><span data-stu-id="79615-116">[Microsoft.AspNet.SignalR.JS](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.JS/)</span></span> | <span data-ttu-id="79615-117">[Microsoft. AspNetCore.SignalR. Klient](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client/)</span><span class="sxs-lookup"><span data-stu-id="79615-117">[Microsoft.AspNetCore.SignalR.Client](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client/)</span></span> |
| <span data-ttu-id="79615-118">Pakiet npm klienta</span><span class="sxs-lookup"><span data-stu-id="79615-118">Client npm Package</span></span> | [<span data-ttu-id="79615-119">SignalR</span><span class="sxs-lookup"><span data-stu-id="79615-119">signalr</span></span>](https://www.npmjs.com/package/signalr) | [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) |
| <span data-ttu-id="79615-120">Klient Java</span><span class="sxs-lookup"><span data-stu-id="79615-120">Java Client</span></span> | <span data-ttu-id="79615-121">[Repozytorium GitHub](https://github.com/SignalR/java-client) (przestarzałe)</span><span class="sxs-lookup"><span data-stu-id="79615-121">[GitHub Repository](https://github.com/SignalR/java-client) (deprecated)</span></span>  | <span data-ttu-id="79615-122">Maven pakietu [com. Microsoft. Signal](https://search.maven.org/artifact/com.microsoft.signalr/signalr)</span><span class="sxs-lookup"><span data-stu-id="79615-122">Maven package [com.microsoft.signalr](https://search.maven.org/artifact/com.microsoft.signalr/signalr)</span></span> |
| <span data-ttu-id="79615-123">Typ aplikacji serwera</span><span class="sxs-lookup"><span data-stu-id="79615-123">Server App Type</span></span> | <span data-ttu-id="79615-124">ASP.NET (System. Web) lub samoobsługowy OWIN</span><span class="sxs-lookup"><span data-stu-id="79615-124">ASP.NET (System.Web) or OWIN Self-Host</span></span> | <span data-ttu-id="79615-125">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="79615-125">ASP.NET Core</span></span> |
| <span data-ttu-id="79615-126">Obsługiwane platformy serwera</span><span class="sxs-lookup"><span data-stu-id="79615-126">Supported Server Platforms</span></span> | <span data-ttu-id="79615-127">.NET Framework 4,5 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="79615-127">.NET Framework 4.5 or later</span></span> | <span data-ttu-id="79615-128">.NET Framework 4.6.1 lub nowsze</span><span class="sxs-lookup"><span data-stu-id="79615-128">.NET Framework 4.6.1 or later</span></span><br><span data-ttu-id="79615-129">.NET Core 2,1 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="79615-129">.NET Core 2.1 or later</span></span> |

## <a name="feature-differences"></a><span data-ttu-id="79615-130">Różnice w funkcji</span><span class="sxs-lookup"><span data-stu-id="79615-130">Feature differences</span></span>

### <a name="automatic-reconnects"></a><span data-ttu-id="79615-131">Automatyczne ponowne łączenie</span><span class="sxs-lookup"><span data-stu-id="79615-131">Automatic reconnects</span></span>

<span data-ttu-id="79615-132">Automatyczne ponowne łączenie nie są obsługiwane w SignalRASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="79615-132">Automatic reconnects aren't supported in ASP.NET Core SignalR.</span></span> <span data-ttu-id="79615-133">Jeśli klient zostanie odłączony, użytkownik musi jawnie rozpocząć nowe połączenie, jeśli chce ponownie nawiązać połączenie.</span><span class="sxs-lookup"><span data-stu-id="79615-133">If the client is disconnected, the user must explicitly start a new connection if they want to reconnect.</span></span> <span data-ttu-id="79615-134">W programie ASP.NET SignalRSignalR próbuje ponownie nawiązać połączenie z serwerem, jeśli połączenie zostanie zerwane.</span><span class="sxs-lookup"><span data-stu-id="79615-134">In ASP.NET SignalR, SignalR attempts to reconnect to the server if the connection is dropped.</span></span>

### <a name="protocol-support"></a><span data-ttu-id="79615-135">Obsługa protokołu</span><span class="sxs-lookup"><span data-stu-id="79615-135">Protocol support</span></span>

<span data-ttu-id="79615-136">ASP.NET Core SignalR obsługuje kod JSON, a także nowy protokół binarny na podstawie [MessagePack](xref:signalr/messagepackhubprotocol).</span><span class="sxs-lookup"><span data-stu-id="79615-136">ASP.NET Core SignalR supports JSON, as well as a new binary protocol based on [MessagePack](xref:signalr/messagepackhubprotocol).</span></span> <span data-ttu-id="79615-137">Dodatkowo można utworzyć niestandardowe protokoły.</span><span class="sxs-lookup"><span data-stu-id="79615-137">Additionally, custom protocols can be created.</span></span>

### <a name="transports"></a><span data-ttu-id="79615-138">Transporty</span><span class="sxs-lookup"><span data-stu-id="79615-138">Transports</span></span>

<span data-ttu-id="79615-139">Transport ramki bez ograniczeń nie jest obsługiwany w SignalRASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="79615-139">The Forever Frame transport is not supported in ASP.NET Core SignalR.</span></span>

## <a name="differences-on-the-server"></a><span data-ttu-id="79615-140">Różnice na serwerze</span><span class="sxs-lookup"><span data-stu-id="79615-140">Differences on the server</span></span>

<span data-ttu-id="79615-141">ASP.NET Core SignalR biblioteki po stronie serwera są zawarte w pakiecie [Microsoft. AspNetCore. app pakietu](xref:fundamentals/metapackage-app) , który jest częścią szablonu **aplikacji sieci Web ASP.NET Core** dla obu projektów Razor i MVC.</span><span class="sxs-lookup"><span data-stu-id="79615-141">The ASP.NET Core SignalR server-side libraries are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) package that's part of the **ASP.NET Core Web Application** template for both Razor and MVC projects.</span></span>

<span data-ttu-id="79615-142">ASP.NET Core SignalR to ASP.NET Core oprogramowanie pośredniczące, dlatego należy je skonfigurować przez wywołanie metody [Addsignaler](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="79615-142">ASP.NET Core SignalR is an ASP.NET Core middleware, so it must be configured by calling [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in `Startup.ConfigureServices`.</span></span>

```csharp
services.AddSignalR()
```

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="79615-143">Aby skonfigurować Routing, Mapuj trasy do centrów wewnątrz wywołania metody [UseEndpoints](/dotnet/api/microsoft.aspnetcore.builder.endpointroutingapplicationbuilderextensions.useendpoints) w metodzie `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="79615-143">To configure routing, map routes to hubs inside the [UseEndpoints](/dotnet/api/microsoft.aspnetcore.builder.endpointroutingapplicationbuilderextensions.useendpoints) method call in the `Startup.Configure` method.</span></span>


```csharp
app.UseRouting();

app.UseEndpoints(endpoints =>
{
    endpoints.MapHub<ChatHub>("/hub");
});
```

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="79615-144">Aby skonfigurować Routing, Mapuj trasy do centrów wewnątrz wywołania metody [UseSignalR](/dotnet/api/microsoft.aspnetcore.builder.signalrappbuilderextensions.usesignalr) w metodzie `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="79615-144">To configure routing, map routes to hubs inside the [UseSignalR](/dotnet/api/microsoft.aspnetcore.builder.signalrappbuilderextensions.usesignalr) method call in the `Startup.Configure` method.</span></span>

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("/hub");
});
```

::: moniker-end

### <a name="sticky-sessions"></a><span data-ttu-id="79615-145">Sesje programu Sticky</span><span class="sxs-lookup"><span data-stu-id="79615-145">Sticky sessions</span></span>

<span data-ttu-id="79615-146">Model skalowania dla ASP.NET SignalR umożliwia klientom Ponowne nawiązywanie połączenia i wysyłanie komunikatów do dowolnego serwera w farmie.</span><span class="sxs-lookup"><span data-stu-id="79615-146">The scaleout model for ASP.NET SignalR allows clients to reconnect and send messages to any server in the farm.</span></span> <span data-ttu-id="79615-147">W ASP.NET Core SignalRklient musi korzystać z tego samego serwera na czas trwania połączenia.</span><span class="sxs-lookup"><span data-stu-id="79615-147">In ASP.NET Core SignalR, the client must interact with the same server for the duration of the connection.</span></span> <span data-ttu-id="79615-148">W przypadku skalowania przy użyciu Redis, oznacza to, że są wymagane sesje programu Sticky.</span><span class="sxs-lookup"><span data-stu-id="79615-148">For scaleout using Redis, that means sticky sessions are required.</span></span> <span data-ttu-id="79615-149">W przypadku skalowania przy użyciu [usługi Azure SignalR](/azure/azure-signalr/)sesje nie są wymagane, ponieważ usługa obsługuje połączenia z klientami.</span><span class="sxs-lookup"><span data-stu-id="79615-149">For scaleout using [Azure SignalR Service](/azure/azure-signalr/), sticky sessions are not required because the service handles connections to clients.</span></span>

### <a name="single-hub-per-connection"></a><span data-ttu-id="79615-150">Jedno centrum na połączenie</span><span class="sxs-lookup"><span data-stu-id="79615-150">Single hub per connection</span></span>

<span data-ttu-id="79615-151">W ASP.NET Core SignalRmodel połączenia został uproszczony.</span><span class="sxs-lookup"><span data-stu-id="79615-151">In ASP.NET Core SignalR, the connection model has been simplified.</span></span> <span data-ttu-id="79615-152">Połączenia są nawiązywane bezpośrednio w jednym centrum, a nie za pomocą jednego połączenia, które jest używane do udostępniania dostępu do wielu centrów.</span><span class="sxs-lookup"><span data-stu-id="79615-152">Connections are made directly to a single hub, rather than a single connection being used to share access to multiple hubs.</span></span>

### <a name="streaming"></a><span data-ttu-id="79615-153">Przesyłanie strumieniowe</span><span class="sxs-lookup"><span data-stu-id="79615-153">Streaming</span></span>

<span data-ttu-id="79615-154">ASP.NET Core SignalR obsługuje teraz [przesyłanie strumieniowe danych](xref:signalr/streaming) z centrum do klienta programu.</span><span class="sxs-lookup"><span data-stu-id="79615-154">ASP.NET Core SignalR now supports [streaming data](xref:signalr/streaming) from the hub to the client.</span></span>

### <a name="state"></a><span data-ttu-id="79615-155">Stan</span><span class="sxs-lookup"><span data-stu-id="79615-155">State</span></span>

<span data-ttu-id="79615-156">Możliwość przekazania dowolnego stanu między klientami a centrum (często nazywane HubState) została usunięta, a także do obsługi komunikatów o postępie.</span><span class="sxs-lookup"><span data-stu-id="79615-156">The ability to pass arbitrary state between clients and the hub (often called HubState) has been removed, as well as support for progress messages.</span></span> <span data-ttu-id="79615-157">W tej chwili nie ma żadnego odpowiednika serwerów proxy centrum.</span><span class="sxs-lookup"><span data-stu-id="79615-157">There is no counterpart of hub proxies at the moment.</span></span>

### <a name="persistentconnection-removal"></a><span data-ttu-id="79615-158">Usuwanie PersistentConnection</span><span class="sxs-lookup"><span data-stu-id="79615-158">PersistentConnection removal</span></span>

<span data-ttu-id="79615-159">W ASP.NET Core SignalRKlasa [PersistentConnection](https://docs.microsoft.com/previous-versions/aspnet/jj919047(v%3dvs.118)) została usunięta.</span><span class="sxs-lookup"><span data-stu-id="79615-159">In ASP.NET Core SignalR, the [PersistentConnection](https://docs.microsoft.com/previous-versions/aspnet/jj919047(v%3dvs.118)) class has been removed.</span></span>

### <a name="globalhost"></a><span data-ttu-id="79615-160">GlobalHost</span><span class="sxs-lookup"><span data-stu-id="79615-160">GlobalHost</span></span>

<span data-ttu-id="79615-161">ASP.NET Core ma iniekcję zależności (DI) wbudowaną w strukturę.</span><span class="sxs-lookup"><span data-stu-id="79615-161">ASP.NET Core has dependency injection (DI) built into the framework.</span></span> <span data-ttu-id="79615-162">Usługi mogą używać DI do uzyskiwania dostępu do [HubContext](xref:signalr/hubcontext).</span><span class="sxs-lookup"><span data-stu-id="79615-162">Services can use DI to access the [HubContext](xref:signalr/hubcontext).</span></span> <span data-ttu-id="79615-163">Obiekt `GlobalHost`, który jest używany w ASP.NET SignalR do pobrania `HubContext` nie istnieje w ASP.NET Core SignalR.</span><span class="sxs-lookup"><span data-stu-id="79615-163">The `GlobalHost` object that is used in ASP.NET SignalR to get a `HubContext` doesn't exist in ASP.NET Core SignalR.</span></span>

### <a name="hubpipeline"></a><span data-ttu-id="79615-164">HubPipeline</span><span class="sxs-lookup"><span data-stu-id="79615-164">HubPipeline</span></span>

<span data-ttu-id="79615-165">SignalR ASP.NET Core nie obsługuje modułów `HubPipeline`.</span><span class="sxs-lookup"><span data-stu-id="79615-165">ASP.NET Core SignalR doesn't have support for `HubPipeline` modules.</span></span>

## <a name="differences-on-the-client"></a><span data-ttu-id="79615-166">Różnice dotyczące klienta</span><span class="sxs-lookup"><span data-stu-id="79615-166">Differences on the client</span></span>

### <a name="typescript"></a><span data-ttu-id="79615-167">TypeScript</span><span class="sxs-lookup"><span data-stu-id="79615-167">TypeScript</span></span>

<span data-ttu-id="79615-168">Klient SignalR ASP.NET Core jest zapisywana w języku [TypeScript](https://www.typescriptlang.org/).</span><span class="sxs-lookup"><span data-stu-id="79615-168">The ASP.NET Core SignalR client is written in [TypeScript](https://www.typescriptlang.org/).</span></span> <span data-ttu-id="79615-169">Podczas korzystania z [klienta JavaScript](xref:signalr/javascript-client)można pisać w języku JavaScript lub TypeScript.</span><span class="sxs-lookup"><span data-stu-id="79615-169">You can write in JavaScript or TypeScript when using the [JavaScript client](xref:signalr/javascript-client).</span></span>

### <a name="the-javascript-client-is-hosted-at-npmhttpswwwnpmjscom"></a><span data-ttu-id="79615-170">Klient JavaScript jest hostowany w [npm](https://www.npmjs.com/)</span><span class="sxs-lookup"><span data-stu-id="79615-170">The JavaScript client is hosted at [npm](https://www.npmjs.com/)</span></span>

<span data-ttu-id="79615-171">W poprzednich wersjach klient JavaScript został uzyskany za pomocą pakietu NuGet w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="79615-171">In previous versions, the JavaScript client was obtained through a NuGet package in Visual Studio.</span></span> <span data-ttu-id="79615-172">W przypadku wersji podstawowych pakiet [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) npm zawiera biblioteki JavaScript.</span><span class="sxs-lookup"><span data-stu-id="79615-172">For the Core versions, the [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) npm package contains the JavaScript libraries.</span></span> <span data-ttu-id="79615-173">Ten pakiet nie jest uwzględniony w szablonie **ASP.NET Core aplikacji sieci Web** .</span><span class="sxs-lookup"><span data-stu-id="79615-173">This package isn't included in the **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="79615-174">Użyj npm, aby uzyskać i zainstalować pakiet `@aspnet/signalr` npm.</span><span class="sxs-lookup"><span data-stu-id="79615-174">Use npm to obtain and install the `@aspnet/signalr` npm package.</span></span>

```console
npm init -y
npm install @aspnet/signalr
```

### <a name="jquery"></a><span data-ttu-id="79615-175">jQuery</span><span class="sxs-lookup"><span data-stu-id="79615-175">jQuery</span></span>

<span data-ttu-id="79615-176">Zależność od jQuery została usunięta, jednak projekty nadal mogą korzystać z jQuery.</span><span class="sxs-lookup"><span data-stu-id="79615-176">The dependency on jQuery has been removed, however projects can still use jQuery.</span></span>

### <a name="internet-explorer-support"></a><span data-ttu-id="79615-177">Obsługa programu Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="79615-177">Internet Explorer support</span></span>

<span data-ttu-id="79615-178">ASP.NET Core SignalR wymaga programu Microsoft Internet Explorer 11 lub nowszego (ASP.NET SignalR obsługiwany program Microsoft Internet Explorer 8 lub nowszy).</span><span class="sxs-lookup"><span data-stu-id="79615-178">ASP.NET Core SignalR requires Microsoft Internet Explorer 11 or later (ASP.NET SignalR supported Microsoft Internet Explorer 8 and later).</span></span>

### <a name="javascript-client-method-syntax"></a><span data-ttu-id="79615-179">Składnia metody klienta JavaScript</span><span class="sxs-lookup"><span data-stu-id="79615-179">JavaScript client method syntax</span></span>

<span data-ttu-id="79615-180">Składnia języka JavaScript została zmieniona z poprzedniej wersji SignalR.</span><span class="sxs-lookup"><span data-stu-id="79615-180">The JavaScript syntax has changed from the previous version of SignalR.</span></span> <span data-ttu-id="79615-181">Zamiast używać obiektu `$connection`, Utwórz połączenie przy użyciu interfejsu API [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) .</span><span class="sxs-lookup"><span data-stu-id="79615-181">Rather than using the `$connection` object, create a connection using the [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) API.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/hub")
    .build();
```

<span data-ttu-id="79615-182">Użyj metody [on](/javascript/api/@aspnet/signalr/HubConnection#on) , aby określić metody klienta, które mogą być wywoływane przez centrum.</span><span class="sxs-lookup"><span data-stu-id="79615-182">Use the [on](/javascript/api/@aspnet/signalr/HubConnection#on) method to specify client methods that the hub can call.</span></span>

```javascript
connection.on("ReceiveMessage", (user, message) => {
    const msg = message.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;");
    const encodedMsg = user + " says " + msg;
    log(encodedMsg);
});
```

<span data-ttu-id="79615-183">Po utworzeniu metody klienta należy uruchomić połączenie z centrum.</span><span class="sxs-lookup"><span data-stu-id="79615-183">After creating the client method, start the hub connection.</span></span> <span data-ttu-id="79615-184">Tworzenie łańcucha metod [przechwytywania](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch) w celu rejestrowania lub obsługi błędów.</span><span class="sxs-lookup"><span data-stu-id="79615-184">Chain a [catch](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch) method to log or handle errors.</span></span>

```javascript
connection.start().catch(err => console.error(err.toString()));
```

### <a name="hub-proxies"></a><span data-ttu-id="79615-185">Serwery proxy centrum</span><span class="sxs-lookup"><span data-stu-id="79615-185">Hub proxies</span></span>

<span data-ttu-id="79615-186">Serwery proxy centrum nie są już generowane automatycznie.</span><span class="sxs-lookup"><span data-stu-id="79615-186">Hub proxies are no longer automatically generated.</span></span> <span data-ttu-id="79615-187">Zamiast tego nazwa metody jest przenoszona do [wywoływania](/javascript/api/%40aspnet/signalr/hubconnection#invoke) API jako ciąg.</span><span class="sxs-lookup"><span data-stu-id="79615-187">Instead, the method name is passed into the [invoke](/javascript/api/%40aspnet/signalr/hubconnection#invoke) API as a string.</span></span>

### <a name="net-and-other-clients"></a><span data-ttu-id="79615-188">.NET i inni klienci</span><span class="sxs-lookup"><span data-stu-id="79615-188">.NET and other clients</span></span>

<span data-ttu-id="79615-189">Pakiet NuGet `Microsoft.AspNetCore.SignalR.Client` zawiera biblioteki klienckie platformy .NET dla ASP.NET Core SignalR.</span><span class="sxs-lookup"><span data-stu-id="79615-189">The `Microsoft.AspNetCore.SignalR.Client` NuGet package contains the .NET client libraries for ASP.NET Core SignalR.</span></span>

<span data-ttu-id="79615-190">Użyj [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder) , aby utworzyć i skompilować wystąpienie połączenia z koncentratorem.</span><span class="sxs-lookup"><span data-stu-id="79615-190">Use the [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder) to create and build an instance of a connection to a hub.</span></span>

```csharp
connection = new HubConnectionBuilder()
    .WithUrl("url")
    .Build();
```

## <a name="scaleout-differences"></a><span data-ttu-id="79615-191">Różnice skalowania</span><span class="sxs-lookup"><span data-stu-id="79615-191">Scaleout differences</span></span>

<span data-ttu-id="79615-192">ASP.NET SignalR obsługuje SQL Server i Redis.</span><span class="sxs-lookup"><span data-stu-id="79615-192">ASP.NET SignalR supports SQL Server and Redis.</span></span> <span data-ttu-id="79615-193">ASP.NET Core SignalR obsługuje usługi Azure SignalR Service i Redis.</span><span class="sxs-lookup"><span data-stu-id="79615-193">ASP.NET Core SignalR supports Azure SignalR Service and Redis.</span></span>

### <a name="aspnet"></a><span data-ttu-id="79615-194">Platforma ASP.NET</span><span class="sxs-lookup"><span data-stu-id="79615-194">ASP.NET</span></span>

* <span data-ttu-id="79615-195">[SignalR skalowania z Azure Service Bus](/aspnet/signalr/overview/performance/scaleout-with-windows-azure-service-bus)</span><span class="sxs-lookup"><span data-stu-id="79615-195">[SignalR scaleout with Azure Service Bus](/aspnet/signalr/overview/performance/scaleout-with-windows-azure-service-bus)</span></span>
* <span data-ttu-id="79615-196">[SignalR skalowania z Redis](/aspnet/signalr/overview/performance/scaleout-with-redis)</span><span class="sxs-lookup"><span data-stu-id="79615-196">[SignalR scaleout with Redis](/aspnet/signalr/overview/performance/scaleout-with-redis)</span></span>
* <span data-ttu-id="79615-197">[SignalR skalowania z SQL Server](/aspnet/signalr/overview/performance/scaleout-with-sql-server)</span><span class="sxs-lookup"><span data-stu-id="79615-197">[SignalR scaleout with SQL Server](/aspnet/signalr/overview/performance/scaleout-with-sql-server)</span></span>

### <a name="aspnet-core"></a><span data-ttu-id="79615-198">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="79615-198">ASP.NET Core</span></span>

* <span data-ttu-id="79615-199">[Usługa SignalR platformy Azure](/azure/azure-signalr/)</span><span class="sxs-lookup"><span data-stu-id="79615-199">[Azure SignalR Service](/azure/azure-signalr/)</span></span>
* [<span data-ttu-id="79615-200">Redis</span><span class="sxs-lookup"><span data-stu-id="79615-200">Redis Backplane</span></span>](xref:signalr/redis-backplane)

## <a name="additional-resources"></a><span data-ttu-id="79615-201">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="79615-201">Additional resources</span></span>

* [<span data-ttu-id="79615-202">Centra</span><span class="sxs-lookup"><span data-stu-id="79615-202">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="79615-203">Klient JavaScript</span><span class="sxs-lookup"><span data-stu-id="79615-203">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="79615-204">Klient .NET</span><span class="sxs-lookup"><span data-stu-id="79615-204">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="79615-205">Obsługiwane platformy</span><span class="sxs-lookup"><span data-stu-id="79615-205">Supported platforms</span></span>](xref:signalr/supported-platforms)
