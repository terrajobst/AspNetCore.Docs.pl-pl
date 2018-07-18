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
# <a name="differences-between-signalr-and-aspnet-core-signalr"></a>Różnice między SignalR i SignalR platformy ASP.NET Core

SignalR platformy ASP.NET Core nie jest zgodny z klientami lub serwerami na potrzeby biblioteki SignalR platformy ASP.NET. Ten artykuł szczegółowo opisuje funkcje, które zostały usunięte lub zmienione w SignalR platformy ASP.NET Core.

## <a name="feature-differences"></a>Różnice w funkcjach

### <a name="automatic-reconnects"></a>Automatyczne ponowne podłączenia

Automatyczne ponowne podłączenia nie są już obsługiwane. Wcześniej SignalR próbował ponownie połączyć z serwerem, jeśli połączenie zostało przerwane. Jeśli klient został odłączony teraz użytkownik musi jawnie uruchomić nowe połączenie, jeśli chcą ponownie połączyć.

### <a name="protocol-support"></a>Obsługa protokołów

Biblioteki SignalR platformy ASP.NET Core obsługuje JSON, a także nowy protokół binarny na podstawie [MessagePack](xref:signalr/messagepackhubprotocol). Ponadto protokoły niestandardowe, mogą być tworzone.

## <a name="differences-on-the-server"></a>Różnice na serwerze

SignalR biblioteki po stronie serwera są zawarte w `Microsoft.AspNetCore.App` pakiet, który jest częścią **aplikacji sieci Web programu ASP.NET Core** szablonów dla projektów MVC i Razor.

SignalR to pośredniczące platformy ASP.NET Core, dlatego musi być skonfigurowany, wywołując `AddSignalR` w `Startup.ConfigureServices`.

```csharp
services.AddSignalR();
```

Aby skonfigurować routing, mapy trasy do koncentratorów wewnątrz `UseSignalR` wywołania metody, w `Startup.Configure` metody.

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("/hub");
});
```

### <a name="sticky-sessions-now-required"></a>Trwałych sesji teraz wymagany

Ze względu na sposób skalowania w poziomie pracy w poprzedniej wersji biblioteki SignalR klientów może ponownie połączyć, a następnie wysyłać komunikaty do dowolnego serwera w farmie. Ze względu na zmiany modelu skalowalnego w poziomie, jak również nie obsługuje ponowne podłączenia to nie jest już obsługiwana. Teraz gdy klient łączy się z serwerem musi korzystać z tego samego serwera dla czas trwania połączenia.

### <a name="single-hub-per-connection"></a>Jedno centrum dla połączenia

W bibliotece SignalR platformy ASP.NET Core został uproszczony model połączenia. Połączenia są nawiązywane bezpośrednio z jednym Centrum, zamiast jednego połączenia używany do udostępnienia w wielu centrach.

### <a name="streaming"></a>Przesyłanie strumieniowe

Obsługuje teraz SignalR [danych przesyłanych strumieniowo](xref:signalr/streaming) z koncentratora do klienta.

### <a name="state"></a>Stan

Możliwość przekazywania dowolny stan między klientami a Centrum (często nazywanej HubState) zostało usunięte, a także obsługę wiadomości dotyczące postępu. Nie ma odpowiednika serwerów proxy koncentratora, w tym momencie.

## <a name="differences-on-the-client"></a>Różnice na komputerze klienckim

### <a name="typescript"></a>TypeScript

Wersja SignalR platformy ASP.NET Core jest zapisywany [TypeScript](https://www.typescriptlang.org/). Można napisać w JavaScript lub TypeScript, korzystając z [klienta JavaScript](xref:signalr/javascript-client).

### <a name="the-javascript-client-is-hosted-at-npmhttpswwwnpmjscom"></a>Klient JavaScript znajduje się w [npm](https://www.npmjs.com/)

W poprzednich wersjach klienta JavaScript uzyskano za pośrednictwem pakietu NuGet w programie Visual Studio. Dla wersji podstawowej [ @aspnet/signalr pakietu npm](https://www.npmjs.com/package/@aspnet/signalr) zawiera biblioteki języka JavaScript. Ten pakiet nie jest zawarty w **aplikacji sieci Web programu ASP.NET Core** szablonu. Aby uzyskać i zainstalować za pomocą usługi npm `@aspnet/signalr` pakietów Menedżera npm.

```console
npm init -y
npm install @aspnet/signalr
```

### <a name="jquery"></a>jQuery

Zależność od jQuery zostało usunięte, jednak projekty można nadal używać jQuery.

### <a name="javascript-client-method-syntax"></a>Składnia metody klient JavaScript

Składnia języka JavaScript został zmieniony względem poprzedniej wersji biblioteki SignalR. Zamiast używania `$connection` obiektów, tworzenia połączenia za pomocą `HubConnectionBuilder` interfejsu API.

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/hub")
    .build();
```

Użyj `connection.on` do określenia metody klienta, które można wywołać Centrum.

```javascript
connection.on("ReceiveMessage", (user, message) => {
    const msg = message.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;");
    const encodedMsg = user + " says " + msg;
    log(encodedMsg);
});
```

Po utworzeniu metody klienta, uruchom połączenie koncentratora. Łańcuch `catch` metodę, aby zalogować się lub obsługiwać błędy.

```javascript
connection.start().catch(err => console.error(err.toString()));
```

### <a name="hub-proxies"></a>Serwery proxy Centrum

Nie będzie automatycznie są generowane, serwery proxy koncentratora. Zamiast tego Nazwa metody jest przekazywana do `invoke` API jako ciąg.

### <a name="net-and-other-clients"></a>.NET i innych klientów

`Microsoft.AspNetCore.SignalR.Client` Pakietu NuGet zawiera biblioteki klienckie .NET dla biblioteki SignalR platformy ASP.NET Core.

Użyj `HubConnectionBuilder` utworzyć i skompilować wystąpienie połączenia z koncentratorem.

```csharp
connection = new HubConnectionBuilder()
    .WithUrl("url")
    .Build();
```

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Centra](xref:signalr/hubs)
* [Klient JavaScript](xref:signalr/javascript-client)
* [Klient .NET](xref:signalr/dotnet-client)
* [Obsługiwane platformy](xref:signalr/supported-platforms)
