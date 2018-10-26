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
# <a name="differences-between-aspnet-signalr-and-aspnet-core-signalr"></a>Różnice między biblioteki SignalR platformy ASP.NET i SignalR platformy ASP.NET Core

SignalR platformy ASP.NET Core nie jest zgodny z klientami lub serwerami na potrzeby biblioteki SignalR platformy ASP.NET. Ten artykuł szczegółowo opisuje funkcje, które zostały usunięte lub zmienione w biblioteki SignalR platformy ASP.NET Core.

## <a name="how-to-identify-the-signalr-version"></a>Jak zidentyfikować wersji biblioteki SignalR

|                      | ASP.NET SignalR | SignalR platformy ASP.NET Core |
| -------------------- | --------------- | -------------------- |
| Pakiet NuGet | [Microsoft.AspNet.SignalR](https://www.nuget.org/packages/Microsoft.AspNet.SignalR/) | [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) (.NET Core)<br>[Microsoft.AspNetCore.SignalR](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) (.NET Framework) |
| Pakiety NuGet klienta | [Microsoft.AspNet.SignalR.Client](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.Client/)<br>[Microsoft.AspNet.SignalR.JS](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.JS/) | [Microsoft.AspNetCore.SignalR.Client](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client/) |
| Klient npm pakietu | [signalr](https://www.npmjs.com/package/signalr) | [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) |
| Typ aplikacji serwera | ASP.NET (System.Web) lub samodzielnego hostowania OWIN | ASP.NET Core |
| Platformy obsługiwane serwera | .NET framework 4.5 lub nowszy | Program .NET framework 4.6.1 lub nowszej<br>.NET core 2.1 lub nowszej |

## <a name="feature-differences"></a>Różnice w funkcjach

### <a name="automatic-reconnects"></a>Automatyczne ponowne podłączenia

Automatyczne ponowne podłączenia nie są obsługiwane w biblioteki SignalR platformy ASP.NET Core. Jeśli klient został odłączony, użytkownik jawnie należy uruchomić nowe połączenie, jeśli chcesz ponownie połączyć. W programie ASP.NET SignalR SignalR podejmie próbę połączenia z serwerem, jeśli połączenie zostało przerwane. 

### <a name="protocol-support"></a>Obsługa protokołów

Biblioteki SignalR platformy ASP.NET Core obsługuje JSON, a także nowy protokół binarny na podstawie [MessagePack](xref:signalr/messagepackhubprotocol). Ponadto protokoły niestandardowe, mogą być tworzone.

## <a name="differences-on-the-server"></a>Różnice na serwerze

Biblioteki po stronie serwera biblioteki SignalR platformy ASP.NET Core są objęte [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) pakiet, który jest częścią **aplikacji sieci Web programu ASP.NET Core** szablon Razor i MVC projekty.

Biblioteki SignalR platformy ASP.NET Core jest pośredniczące platformy ASP.NET Core, więc musi być skonfigurowany, wywołując [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) w `Startup.ConfigureServices`.

```csharp
services.AddSignalR()
```

Aby skonfigurować routing, mapy trasy do koncentratorów wewnątrz [UseSignalR](/dotnet/api/microsoft.aspnetcore.builder.signalrappbuilderextensions.usesignalr) wywołania metody, w `Startup.Configure` metody.

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("/hub");
});
```

### <a name="sticky-sessions-now-required"></a>Trwałych sesji teraz wymagany

Ze względu na sposób skalowania w poziomie pracy w ASP.NET SignalR klientów może ponownie połączyć, a następnie wysyłać komunikaty do dowolnego serwera w farmie. Ze względu na zmiany modelu skalowalnego w poziomie, jak również nie obsługuje ponowne podłączenia to nie jest już obsługiwana. Gdy klient nawiąże połączenie z serwerem, na czas trwania połączenia musi współdziałać z tym samym serwerze.

### <a name="single-hub-per-connection"></a>Jedno centrum dla połączenia

W bibliotece SignalR platformy ASP.NET Core został uproszczony model połączenia. Połączenia są nawiązywane bezpośrednio z jednym Centrum, zamiast jednego połączenia używany do udostępnienia w wielu centrach.

### <a name="streaming"></a>Przesyłanie strumieniowe

Obsługuje teraz Core SignalR platformy ASP.NET [danych przesyłanych strumieniowo](xref:signalr/streaming) z koncentratora do klienta.

### <a name="state"></a>Stan

Możliwość przekazywania dowolny stan między klientami a Centrum (często nazywanej HubState) zostało usunięte, a także obsługę wiadomości dotyczące postępu. Nie ma odpowiednika serwerów proxy koncentratora, w tym momencie.

## <a name="differences-on-the-client"></a>Różnice na komputerze klienckim

### <a name="typescript"></a>TypeScript

Klient biblioteki SignalR platformy ASP.NET Core jest zapisywany [TypeScript](https://www.typescriptlang.org/). Można napisać w JavaScript lub TypeScript, korzystając z [klienta JavaScript](xref:signalr/javascript-client).

### <a name="the-javascript-client-is-hosted-at-npmhttpswwwnpmjscom"></a>Klient JavaScript znajduje się w [npm](https://www.npmjs.com/)

W poprzednich wersjach klienta JavaScript uzyskano za pośrednictwem pakietu NuGet w programie Visual Studio. Dla wersji podstawowej [ @aspnet/signalr ](https://www.npmjs.com/package/@aspnet/signalr) pakietu npm zawiera biblioteki języka JavaScript. Ten pakiet nie jest zawarty w **aplikacji sieci Web programu ASP.NET Core** szablonu. Aby uzyskać i zainstalować za pomocą usługi npm `@aspnet/signalr` pakietów Menedżera npm.

```console
npm init -y
npm install @aspnet/signalr
```

### <a name="jquery"></a>jQuery

Zależność od jQuery zostało usunięte, jednak projekty można nadal używać jQuery.

### <a name="javascript-client-method-syntax"></a>Składnia metody klient JavaScript

Składnia języka JavaScript został zmieniony względem poprzedniej wersji biblioteki SignalR. Zamiast używania `$connection` obiektów, tworzenia połączenia za pomocą [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) interfejsu API.

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/hub")
    .build();
```

Użyj [na](/javascript/api/@aspnet/signalr/HubConnection#on) metodę, aby określić metody klienta, które można wywołać Centrum.

```javascript
connection.on("ReceiveMessage", (user, message) => {
    const msg = message.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;");
    const encodedMsg = user + " says " + msg;
    log(encodedMsg);
});
```

Po utworzeniu metody klienta, uruchom połączenie koncentratora. Łańcuch [catch](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch) metodę, aby zalogować się lub obsługiwać błędy.

```javascript
connection.start().catch(err => console.error(err.toString()));
```

### <a name="hub-proxies"></a>Serwery proxy Centrum

Nie będzie automatycznie są generowane, serwery proxy koncentratora. Zamiast tego Nazwa metody jest przekazywana do [wywołania](/javascript/api/%40aspnet/signalr/hubconnection#invoke) API jako ciąg.

### <a name="net-and-other-clients"></a>.NET i innych klientów

`Microsoft.AspNetCore.SignalR.Client` Pakietu NuGet zawiera biblioteki klienckie .NET dla biblioteki SignalR platformy ASP.NET Core.

Użyj [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder) utworzyć i skompilować wystąpienie połączenia z koncentratorem.

```csharp
connection = new HubConnectionBuilder()
    .WithUrl("url")
    .Build();
```

## <a name="scaleout-differences"></a>Różnice skalowania w poziomie

Biblioteki SignalR platformy ASP.NET obsługuje program SQL Server i Redis. Biblioteki SignalR platformy ASP.NET Core obsługuje usługi Azure SignalR Service i Redis.

### <a name="aspnet"></a>ASP.NET

* [SignalR — skalowanie w poziomie za pomocą usługi Azure Service Bus](/aspnet/signalr/overview/performance/scaleout-with-windows-azure-service-bus)
* [SignalR — skalowanie w poziomie przy użyciu usługi Redis](/aspnet/signalr/overview/performance/scaleout-with-redis)
* [SignalR — skalowanie w poziomie z programem SQL Server](/aspnet/signalr/overview/performance/scaleout-with-sql-server)

### <a name="aspnet-core"></a>ASP.NET Core

* [Usługi Azure SignalR Service](/azure/azure-signalr/)

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Centra](xref:signalr/hubs)
* [Klient JavaScript](xref:signalr/javascript-client)
* [Klient .NET](xref:signalr/dotnet-client)
* [Obsługiwane platformy](xref:signalr/supported-platforms)
