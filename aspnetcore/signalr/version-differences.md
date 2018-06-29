---
title: Różnice między SignalR i SignalR platformy ASP.NET Core
author: rachelappel
description: Różnice między SignalR i SignalR platformy ASP.NET Core
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.date: 06/30/2018
uid: signalr/version-differences
ms.openlocfilehash: fd314d93333bd159aef4bb4863be50c646809cf0
ms.sourcegitcommit: 1faf2525902236428dae6a59e375519bafd5d6d7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/28/2018
ms.locfileid: "37090178"
---
# <a name="differences-between-signalr-and-aspnet-core-signalr"></a>Różnice między SignalR i SignalR platformy ASP.NET Core

SignalR platformy ASP.NET Core nie jest zgodny z klientów lub serwerów dla produktu ASP.NET SignalR. Ten artykuł zawiera szczegóły dotyczące funkcji, które zostały usunięte lub zmienione w ASP.NET Core SignalR.

## <a name="feature-differences"></a>Różnice w funkcjach

### <a name="automatic-reconnects"></a>Automatyczne ponowne podłączenia

Automatyczne ponowne podłączenia nie są już obsługiwane. Wcześniej SignalR próby połączenia z serwerem, jeśli połączenie zostało przerwane. Teraz, jeśli klient został odłączony, użytkownik musi jawnie uruchamiane jest nowe połączenie, aby połączyć się ponownie.

### <a name="protocol-support"></a>Obsługa protokołu

Biblioteka SignalR platformy ASP.NET Core obsługuje JSON, a także nowy protokół binarny na podstawie [MessagePack](xref:signalr/messagepackhubprotocol). Ponadto można tworzyć niestandardowe protokołów.

## <a name="differences-on-the-server"></a>Różnice na serwerze

Biblioteki SignalR po stronie serwera znajdują się w `Microsoft.AspNetCore.App` pakietu, który jest częścią **aplikacji sieci Web platformy ASP.NET Core** szablonów dla projektów MVC jak Razor.

SignalR to pośredniczące platformy ASP.NET Core, musi być skonfigurowany przez wywołanie metody `AddSignalR` w `Startup.ConfigureServices`.

```csharp
services.AddSignalR();
```

Aby skonfigurować routing, mapować trasy koncentratory wewnątrz `UseSignalR` wywołanie metody `Startup.Configure` metody.

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("/hub");
});
```

### <a name="sticky-sessions-now-required"></a>Trwałe sesje teraz wymagane

Ze względu na sposób skalowania w poziomie działał w poprzednich wersjach programu SignalR klientów można ponownie połączyć i wysyłanie komunikatów do dowolnego serwera w farmie. Z powodu zmiany modelu skalowalnego w poziomie, jak również nie obsługuje ponowne podłączenia to nie jest już obsługiwana. Teraz gdy klient łączy się z serwerem musi interakcję z tym samym serwerze, na czas trwania połączenia.

### <a name="single-hub-per-connection"></a>Jednego koncentratora połączenia dla

W programie ASP.NET SignalR Core został uproszczony model połączenia. Połączenia są nawiązywane bezpośrednio do jednego koncentratora, a nie jedno połączenie używane do udostępniania dostęp do wielu centrów.

### <a name="streaming"></a>Przesyłanie strumieniowe

Obsługuje teraz SignalR [przesyłanie strumieniowe danych](xref:signalr/streaming) z koncentratora do klienta.

### <a name="state"></a>Stan

Możliwość przekazywania stan dowolnego między klientami a koncentratora (często nazywane HubState) została usunięta, a także obsługę wiadomości dotyczące postępu. Nie ma odpowiednika serwerów proxy Centrum w tej chwili.

## <a name="differences-on-the-client"></a>Różnice na kliencie

### <a name="typescript"></a>TypeScript

Wersja platformy ASP.NET Core SignalR są zapisywane [TypeScript](https://www.typescriptlang.org/). Można pisać w JavaScript i TypeScript, korzystając z [JavaScript klienta](xref:signalr/javascript-client).

### <a name="the-javascript-client-is-hosted-at-npmhttpswwwnpmjscom"></a>Klient JavaScript jest obsługiwany w [npm](https://www.npmjs.com/)

W poprzednich wersjach klienta JavaScript uzyskano za pośrednictwem pakietu NuGet w programie Visual Studio. W przypadku wersji Core [ @aspnet/signalr pakietu npm](https://www.npmjs.com/package/@aspnet/signalr) zawiera biblioteki JavaScript. Ten pakiet nie jest zawarty w **aplikacji sieci Web platformy ASP.NET Core** szablonu. Użyj programu npm, aby uzyskać i zainstalować `@aspnet/signalr` pakietu npm.

```console
npm init -y
npm install @aspnet/signalr
```

### <a name="jquery"></a>JQuery

Zależność od jQuery została usunięta, jednak projekty można nadal używać jQuery.

### <a name="javascript-client-method-syntax"></a>Składnia metody JavaScript klienta

Składnia języka JavaScript został zmieniony z poprzedniej wersji programu SignalR. Zamiast przy użyciu `$connection` obiektów, tworzenia połączenia za pomocą `HubConnectionBuilder` interfejsu API.

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/hub")
    .build();
```

Użyj `connection.on` do określenia metod klienta, które można wywołać koncentratora.

```javascript
connection.on("ReceiveMessage", (user, message) => {
    const msg = message.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;");
    const encodedMsg = user + " says " + msg;
    log(encodedMsg);
});
```

Po utworzeniu metody klienta, uruchom połączenie koncentratora. Łańcuch `catch` metoda logowania lub obsługi błędów.

```javascript
connection.start().catch(err => console.error(err.toString()));
```

### <a name="hub-proxies"></a>Serwery proxy Centrum

Nie będzie automatycznie generowanych proxy koncentratora. Zamiast tego w nazwie metody jest przekazywany do `invoke` interfejsu API w postaci ciągu.

### <a name="net-and-other-clients"></a>.NET i innych klientów

`Microsoft.AspNetCore.SignalR.Client` Pakietu NuGet zawiera biblioteki klienta .NET dla produktu ASP.NET SignalR Core.

Użyj `HubConnectionBuilder` do tworzenia i tworzenie wystąpienia połączenia z koncentratorem.

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
