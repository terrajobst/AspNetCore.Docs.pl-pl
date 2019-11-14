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
# <a name="differences-between-aspnet-opno-locsignalr-and-aspnet-core-opno-locsignalr"></a>Różnice między ASP.NET SignalR i ASP.NET Core SignalR

SignalR ASP.NET Core nie są zgodne z klientami lub serwerami ASP.NET SignalR. W tym artykule opisano funkcje, które zostały usunięte lub zmienione w SignalRASP.NET Core.

## <a name="how-to-identify-the-opno-locsignalr-version"></a>Jak zidentyfikować wersję SignalR

|                      | ASP.NET SignalR | ASP.NET Core SignalR |
| -------------------- | --------------- | -------------------- |
| Pakiet NuGet serwera | [Microsoft. AspNet.SignalR](https://www.nuget.org/packages/Microsoft.AspNet.SignalR/) | [Microsoft. AspNetCore. app](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) (.NET Core)<br>[Microsoft. AspNetCore.SignalR](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) (.NET Framework) |
| Pakiety NuGet klienta | [Microsoft. AspNet.SignalR. Klient](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.Client/)<br>[Microsoft. AspNet.SignalR. JS](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.JS/) | [Microsoft. AspNetCore.SignalR. Klient](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client/) |
| Pakiet npm klienta | [SignalR](https://www.npmjs.com/package/signalr) | [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) |
| Klient Java | [Repozytorium GitHub](https://github.com/SignalR/java-client) (przestarzałe)  | Maven pakietu [com. Microsoft. Signal](https://search.maven.org/artifact/com.microsoft.signalr/signalr) |
| Typ aplikacji serwera | ASP.NET (System. Web) lub samoobsługowy OWIN | ASP.NET Core |
| Obsługiwane platformy serwera | .NET Framework 4,5 lub nowszy | .NET Framework 4.6.1 lub nowsze<br>.NET Core 2,1 lub nowszy |

## <a name="feature-differences"></a>Różnice w funkcji

### <a name="automatic-reconnects"></a>Automatyczne ponowne łączenie

Automatyczne ponowne łączenie nie są obsługiwane w SignalRASP.NET Core. Jeśli klient zostanie odłączony, użytkownik musi jawnie rozpocząć nowe połączenie, jeśli chce ponownie nawiązać połączenie. W programie ASP.NET SignalRSignalR próbuje ponownie nawiązać połączenie z serwerem, jeśli połączenie zostanie zerwane.

### <a name="protocol-support"></a>Obsługa protokołu

ASP.NET Core SignalR obsługuje kod JSON, a także nowy protokół binarny na podstawie [MessagePack](xref:signalr/messagepackhubprotocol). Dodatkowo można utworzyć niestandardowe protokoły.

### <a name="transports"></a>Transporty

Transport ramki bez ograniczeń nie jest obsługiwany w SignalRASP.NET Core.

## <a name="differences-on-the-server"></a>Różnice na serwerze

ASP.NET Core SignalR biblioteki po stronie serwera są zawarte w pakiecie [Microsoft. AspNetCore. app pakietu](xref:fundamentals/metapackage-app) , który jest częścią szablonu **aplikacji sieci Web ASP.NET Core** dla obu projektów Razor i MVC.

ASP.NET Core SignalR to ASP.NET Core oprogramowanie pośredniczące, dlatego należy je skonfigurować przez wywołanie metody [Addsignaler](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) w `Startup.ConfigureServices`.

```csharp
services.AddSignalR()
```

::: moniker range=">= aspnetcore-3.0"

Aby skonfigurować Routing, Mapuj trasy do centrów wewnątrz wywołania metody [UseEndpoints](/dotnet/api/microsoft.aspnetcore.builder.endpointroutingapplicationbuilderextensions.useendpoints) w metodzie `Startup.Configure`.


```csharp
app.UseRouting();

app.UseEndpoints(endpoints =>
{
    endpoints.MapHub<ChatHub>("/hub");
});
```

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

Aby skonfigurować Routing, Mapuj trasy do centrów wewnątrz wywołania metody [UseSignalR](/dotnet/api/microsoft.aspnetcore.builder.signalrappbuilderextensions.usesignalr) w metodzie `Startup.Configure`.

```csharp
app.UseSignalR(routes =>
{
    routes.MapHub<ChatHub>("/hub");
});
```

::: moniker-end

### <a name="sticky-sessions"></a>Sesje programu Sticky

Model skalowania dla ASP.NET SignalR umożliwia klientom Ponowne nawiązywanie połączenia i wysyłanie komunikatów do dowolnego serwera w farmie. W ASP.NET Core SignalRklient musi korzystać z tego samego serwera na czas trwania połączenia. W przypadku skalowania przy użyciu Redis, oznacza to, że są wymagane sesje programu Sticky. W przypadku skalowania przy użyciu [usługi Azure SignalR](/azure/azure-signalr/)sesje nie są wymagane, ponieważ usługa obsługuje połączenia z klientami.

### <a name="single-hub-per-connection"></a>Jedno centrum na połączenie

W ASP.NET Core SignalRmodel połączenia został uproszczony. Połączenia są nawiązywane bezpośrednio w jednym centrum, a nie za pomocą jednego połączenia, które jest używane do udostępniania dostępu do wielu centrów.

### <a name="streaming"></a>Przesyłanie strumieniowe

ASP.NET Core SignalR obsługuje teraz [przesyłanie strumieniowe danych](xref:signalr/streaming) z centrum do klienta programu.

### <a name="state"></a>Stan

Możliwość przekazania dowolnego stanu między klientami a centrum (często nazywane HubState) została usunięta, a także do obsługi komunikatów o postępie. W tej chwili nie ma żadnego odpowiednika serwerów proxy centrum.

### <a name="persistentconnection-removal"></a>Usuwanie PersistentConnection

W ASP.NET Core SignalRKlasa [PersistentConnection](https://docs.microsoft.com/previous-versions/aspnet/jj919047(v%3dvs.118)) została usunięta.

### <a name="globalhost"></a>GlobalHost

ASP.NET Core ma iniekcję zależności (DI) wbudowaną w strukturę. Usługi mogą używać DI do uzyskiwania dostępu do [HubContext](xref:signalr/hubcontext). Obiekt `GlobalHost`, który jest używany w ASP.NET SignalR do pobrania `HubContext` nie istnieje w ASP.NET Core SignalR.

### <a name="hubpipeline"></a>HubPipeline

SignalR ASP.NET Core nie obsługuje modułów `HubPipeline`.

## <a name="differences-on-the-client"></a>Różnice dotyczące klienta

### <a name="typescript"></a>TypeScript

Klient SignalR ASP.NET Core jest zapisywana w języku [TypeScript](https://www.typescriptlang.org/). Podczas korzystania z [klienta JavaScript](xref:signalr/javascript-client)można pisać w języku JavaScript lub TypeScript.

### <a name="the-javascript-client-is-hosted-at-npmhttpswwwnpmjscom"></a>Klient JavaScript jest hostowany w [npm](https://www.npmjs.com/)

W poprzednich wersjach klient JavaScript został uzyskany za pomocą pakietu NuGet w programie Visual Studio. W przypadku wersji podstawowych pakiet [@aspnet/signalr](https://www.npmjs.com/package/@aspnet/signalr) npm zawiera biblioteki JavaScript. Ten pakiet nie jest uwzględniony w szablonie **ASP.NET Core aplikacji sieci Web** . Użyj npm, aby uzyskać i zainstalować pakiet `@aspnet/signalr` npm.

```console
npm init -y
npm install @aspnet/signalr
```

### <a name="jquery"></a>jQuery

Zależność od jQuery została usunięta, jednak projekty nadal mogą korzystać z jQuery.

### <a name="internet-explorer-support"></a>Obsługa programu Internet Explorer

ASP.NET Core SignalR wymaga programu Microsoft Internet Explorer 11 lub nowszego (ASP.NET SignalR obsługiwany program Microsoft Internet Explorer 8 lub nowszy).

### <a name="javascript-client-method-syntax"></a>Składnia metody klienta JavaScript

Składnia języka JavaScript została zmieniona z poprzedniej wersji SignalR. Zamiast używać obiektu `$connection`, Utwórz połączenie przy użyciu interfejsu API [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) .

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/hub")
    .build();
```

Użyj metody [on](/javascript/api/@aspnet/signalr/HubConnection#on) , aby określić metody klienta, które mogą być wywoływane przez centrum.

```javascript
connection.on("ReceiveMessage", (user, message) => {
    const msg = message.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;");
    const encodedMsg = user + " says " + msg;
    log(encodedMsg);
});
```

Po utworzeniu metody klienta należy uruchomić połączenie z centrum. Tworzenie łańcucha metod [przechwytywania](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch) w celu rejestrowania lub obsługi błędów.

```javascript
connection.start().catch(err => console.error(err.toString()));
```

### <a name="hub-proxies"></a>Serwery proxy centrum

Serwery proxy centrum nie są już generowane automatycznie. Zamiast tego nazwa metody jest przenoszona do [wywoływania](/javascript/api/%40aspnet/signalr/hubconnection#invoke) API jako ciąg.

### <a name="net-and-other-clients"></a>.NET i inni klienci

Pakiet NuGet `Microsoft.AspNetCore.SignalR.Client` zawiera biblioteki klienckie platformy .NET dla ASP.NET Core SignalR.

Użyj [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder) , aby utworzyć i skompilować wystąpienie połączenia z koncentratorem.

```csharp
connection = new HubConnectionBuilder()
    .WithUrl("url")
    .Build();
```

## <a name="scaleout-differences"></a>Różnice skalowania

ASP.NET SignalR obsługuje SQL Server i Redis. ASP.NET Core SignalR obsługuje usługi Azure SignalR Service i Redis.

### <a name="aspnet"></a>Platforma ASP.NET

* [SignalR skalowania z Azure Service Bus](/aspnet/signalr/overview/performance/scaleout-with-windows-azure-service-bus)
* [SignalR skalowania z Redis](/aspnet/signalr/overview/performance/scaleout-with-redis)
* [SignalR skalowania z SQL Server](/aspnet/signalr/overview/performance/scaleout-with-sql-server)

### <a name="aspnet-core"></a>ASP.NET Core

* [Usługa SignalR platformy Azure](/azure/azure-signalr/)
* [Redis](xref:signalr/redis-backplane)

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Centra](xref:signalr/hubs)
* [Klient JavaScript](xref:signalr/javascript-client)
* [Klient .NET](xref:signalr/dotnet-client)
* [Obsługiwane platformy](xref:signalr/supported-platforms)
