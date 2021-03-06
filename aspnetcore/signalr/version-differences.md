---
title: Różnice między SignalR i ASP.NET Core SignalR
author: bradygaster
description: Różnice między SignalR i ASP.NET Core SignalR
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.date: 11/21/2019
no-loc:
- SignalR
uid: signalr/version-differences
ms.openlocfilehash: cca9a0cb0c46fc25eb5d1f7127d31fd3ab92f0b4
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78663549"
---
# <a name="differences-between-aspnet-signalr-and-aspnet-core-signalr"></a>Różnice między sygnalizującym ASP.NET a ASP.NET Core sygnalizującym

ASP.NET Core sygnalizujący nie jest zgodny z klientami lub serwerami dla sygnalizującego ASP.NET. Ten artykuł zawiera szczegółowe informacje o funkcjach, które zostały usunięte lub zmienione w programie ASP.NET Core Signaler.

## <a name="how-to-identify-the-signalr-version"></a>Jak zidentyfikować wersję sygnalizującego

::: moniker range=">= aspnetcore-3.0"

|                      | ASP.NET SignalR | ASP.NET Core SignalR |
| -------------------- | --------------- | -------------------- |
| Pakiet NuGet serwera | [Microsoft. AspNet. Signal](https://www.nuget.org/packages/Microsoft.AspNet.SignalR/) | Brak. Uwzględnione w udostępnionej platformie [Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app) . |
| Pakiety NuGet klienta | [Microsoft. AspNet. Signal. Client](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.Client/)<br>[Microsoft. AspNet. Signaler. JS](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.JS/) | [Microsoft. AspNetCore. Signaler. Client](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client/) |
| Pakiet npm klienta języka JavaScript | [signalr](https://www.npmjs.com/package/signalr) | [`@microsoft/signalr`](https://www.npmjs.com/package/@microsoft/signalr) |
| Klient Java | [Repozytorium GitHub](https://github.com/SignalR/java-client) (przestarzałe)  | Maven pakietu [com. Microsoft. Signal](https://search.maven.org/artifact/com.microsoft.signalr/signalr) |
| Typ aplikacji serwera | ASP.NET (System. Web) lub samoobsługowy OWIN | ASP.NET Core |
| Obsługiwane platformy serwera | .NET Framework 4,5 lub nowszy | .NET Core 3,0 lub nowszy |

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

|                      | ASP.NET SignalR | ASP.NET Core SignalR |
| -------------------- | --------------- | -------------------- |
| Pakiet NuGet serwera | [Microsoft. AspNet.SignalR](https://www.nuget.org/packages/Microsoft.AspNet.SignalR/) | [Microsoft. AspNetCore. app](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) (.NET Core)<br>[Microsoft. AspNetCore.SignalR](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) (.NET Framework) |
| Pakiety NuGet klienta | [Microsoft. AspNet.SignalR. Klient](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.Client/)<br>[Microsoft. AspNet.SignalR. JS](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.JS/) | [Microsoft. AspNetCore.SignalR. Klient](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client/) |
| Pakiet npm klienta języka JavaScript | [signalr](https://www.npmjs.com/package/signalr) | [`@aspnet/signalr`](https://www.npmjs.com/package/@aspnet/signalr) |
| Klient Java | [Repozytorium GitHub](https://github.com/SignalR/java-client) (przestarzałe)  | Maven pakietu [com. Microsoft. Signal](https://search.maven.org/artifact/com.microsoft.signalr/signalr) |
| Typ aplikacji serwera | ASP.NET (System. Web) lub samoobsługowy OWIN | ASP.NET Core |
| Obsługiwane platformy serwera | .NET Framework 4,5 lub nowszy | .NET Framework 4.6.1 lub nowsze<br>.NET Core 2,1 lub nowszy |

::: moniker-end

## <a name="feature-differences"></a>Różnice w funkcji

### <a name="automatic-reconnects"></a>Automatyczne ponowne łączenie

::: moniker range=">= aspnetcore-3.0"

W ASP.NET SignalR:

* Domyślnie program SignalR próbuje ponownie nawiązać połączenie z serwerem, jeśli połączenie zostało porzucone. 

W ASP.NET Core SignalR:

* Automatyczne ponowne łączenie jest zgodą na [klienta .NET](xref:signalr/dotnet-client#automatically-reconnect) i [klienta JavaScript](xref:signalr/javascript-client#automatically-reconnect):

```csharp
HubConnection connection = new HubConnectionBuilder()
    .WithUrl(new Uri("http://127.0.0.1:5000/chatHub"))
    .WithAutomaticReconnect()
    .Build();
```

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withAutomaticReconnect()
    .build();
```

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Przed ASP.NET Core 3,0 SignalR nie obsługuje automatycznego ponownego nawiązywania połączeń. Jeśli klient zostanie odłączony, użytkownik musi jawnie rozpocząć nowe połączenie, aby ponownie nawiązać połączenie. W programie ASP.NET SignalRSignalR próbuje ponownie nawiązać połączenie z serwerem, jeśli połączenie zostanie zerwane.

::: moniker-end

### <a name="protocol-support"></a>Obsługa protokołu

ASP.NET Core SignalR obsługuje kod JSON, a także nowy protokół binarny na podstawie [MessagePack](xref:signalr/messagepackhubprotocol). Dodatkowo można utworzyć niestandardowe protokoły.

### <a name="transports"></a>Transporty

Transport ramki bez ograniczeń nie jest obsługiwany w SignalRASP.NET Core.

## <a name="differences-on-the-server"></a>Różnice na serwerze

ASP.NET Core SignalR biblioteki po stronie serwera są zawarte w [pakiecie Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app), który jest używany w szablonie **aplikacji sieci Web ASP.NET Core** dla obu projektów Razor i MVC.

ASP.NET Core SignalR to ASP.NET Core oprogramowanie pośredniczące. Należy ją skonfigurować, wywołując <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddSignalR%2A> w `Startup.ConfigureServices`.

```csharp
services.AddSignalR()
```

::: moniker range=">= aspnetcore-3.0"

Aby skonfigurować Routing, Mapuj trasy do centrów wewnątrz wywołania metody <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints%2A> w metodzie `Startup.Configure`.

```csharp
app.UseRouting();

app.UseEndpoints(endpoints =>
{
    endpoints.MapHub<ChatHub>("/hub");
});
```

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

Aby skonfigurować Routing, Mapuj trasy do centrów wewnątrz wywołania metody <xref:Microsoft.AspNetCore.Builder.SignalRAppBuilderExtensions.UseSignalR%2A> w metodzie `Startup.Configure`.

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

Możliwość przekazania dowolnego stanu między klientami a centrum (często nazywane `HubState`) została usunięta, a także do obsługi komunikatów o postępie. W tej chwili nie ma żadnego odpowiednika serwerów proxy centrum.

### <a name="persistentconnection-removal"></a>Usuwanie PersistentConnection

W ASP.NET Core SignalRKlasa [PersistentConnection](https://docs.microsoft.com/previous-versions/aspnet/jj919047(v%3dvs.118)) została usunięta.

### <a name="globalhost"></a>GlobalHost

ASP.NET Core ma iniekcję zależności (DI) wbudowaną w strukturę. Usługi mogą używać DI do uzyskiwania dostępu do [HubContext](xref:signalr/hubcontext). Obiekt `GlobalHost`, który jest używany w ASP.NET SignalR do pobrania `HubContext` nie istnieje w ASP.NET Core SignalR.

### <a name="hubpipeline"></a>HubPipeline

SignalR ASP.NET Core nie obsługuje modułów `HubPipeline`.

## <a name="differences-on-the-client"></a>Różnice dotyczące klienta

### <a name="typescript"></a>TypeScript

Klient SignalR ASP.NET Core jest zapisywana w języku [TypeScript](https://www.typescriptlang.org/). Podczas korzystania z [klienta JavaScript](xref:signalr/javascript-client)można pisać w języku JavaScript lub TypeScript.

### <a name="the-javascript-client-is-hosted-at-npm"></a>Klient JavaScript jest hostowany w npm

::: moniker range=">= aspnetcore-3.0"

W wersjach ASP.NET klient JavaScript został uzyskany za pomocą pakietu NuGet w programie Visual Studio. W wersjach ASP.NET Core pakiet [`@microsoft/signalr`](https://www.npmjs.com/package/@microsoft/signalr) npm zawiera biblioteki JavaScript. Ten pakiet nie jest uwzględniony w szablonie **ASP.NET Core aplikacji sieci Web** . Użyj npm, aby uzyskać i zainstalować pakiet `@microsoft/signalr` npm.

```console
npm init -y
npm install @microsoft/signalr
```

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

W wersjach ASP.NET klient JavaScript został uzyskany za pomocą pakietu NuGet w programie Visual Studio. W wersjach ASP.NET Core pakiet [`@aspnet/signalr`](https://www.npmjs.com/package/@aspnet/signalr) npm zawiera biblioteki JavaScript. Ten pakiet nie jest uwzględniony w szablonie **ASP.NET Core aplikacji sieci Web** . Użyj npm, aby uzyskać i zainstalować pakiet `@aspnet/signalr` npm.

```console
npm init -y
npm install @aspnet/signalr
```

::: moniker-end

### <a name="jquery"></a>jQuery

Zależność od jQuery została usunięta, jednak projekty nadal mogą korzystać z jQuery.

### <a name="internet-explorer-support"></a>Obsługa programu Internet Explorer

ASP.NET Core SignalR wymaga programu Microsoft Internet Explorer 11 lub nowszego (ASP.NET SignalR obsługiwany program Microsoft Internet Explorer 8 lub nowszy).

### <a name="javascript-client-method-syntax"></a>Składnia metody klienta JavaScript

::: moniker range=">= aspnetcore-3.0"

Składnia języka JavaScript została zmieniona z wersji ASP.NET SignalR. Zamiast używać obiektu `$connection`, Utwórz połączenie przy użyciu interfejsu API [HubConnectionBuilder](/javascript/api/@aspnet/signalr/hubconnectionbuilder) .

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/hub")
    .build();
```

Użyj metody [on](/javascript/api/@microsoft/signalr/HubConnection#on) , aby określić metody klienta, które mogą być wywoływane przez centrum.

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

Składnia języka JavaScript została zmieniona z wersji ASP.NET SignalR. Zamiast używać obiektu `$connection`, Utwórz połączenie przy użyciu interfejsu API [HubConnectionBuilder](/javascript/api/@microsoft/signalr/hubconnectionbuilder) .

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/hub")
    .build();
```

Użyj metody [on](/javascript/api/@aspnet/signalr/HubConnection#on) , aby określić metody klienta, które mogą być wywoływane przez centrum.

::: moniker-end

```javascript
connection.on("ReceiveMessage", (user, message) => {
    const msg = message.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;");
    const encodedMsg = `${user} says ${msg}`;
    console.log(encodedMsg);
});
```

Po utworzeniu metody klienta należy uruchomić połączenie z centrum. Tworzenie łańcucha metod [przechwytywania](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch) w celu rejestrowania lub obsługi błędów.

```javascript
connection.start().catch(err => console.error(err));
```

### <a name="hub-proxies"></a>Serwery proxy centrum

::: moniker range=">= aspnetcore-3.0"

Serwery proxy centrum nie są już generowane automatycznie. Zamiast tego nazwa metody jest przenoszona do [wywoływania](/javascript/api/@microsoft/signalr/hubconnection#invoke) API jako ciąg.

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

Serwery proxy centrum nie są już generowane automatycznie. Zamiast tego nazwa metody jest przenoszona do [wywoływania](/javascript/api/@aspnet/signalr/hubconnection#invoke) API jako ciąg.

::: moniker-end

### <a name="net-and-other-clients"></a>.NET i inni klienci

[Microsoft. AspNetCore.SignalR. ](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client)Pakiet NuGet klienta zawiera biblioteki klienckie platformy .NET dla ASP.NET Core SignalR.

Użyj <xref:Microsoft.AspNetCore.SignalR.Client.HubConnectionBuilder>, aby utworzyć i skompilować wystąpienie połączenia z centrum.

```csharp
connection = new HubConnectionBuilder()
    .WithUrl("url")
    .Build();
```

## <a name="scaleout-differences"></a>Różnice skalowania

ASP.NET SignalR obsługuje SQL Server i Redis. ASP.NET Core SignalR obsługuje usługi Azure SignalR Service i Redis.

### <a name="aspnet"></a>ASP.NET

* [SignalR skalowania z Azure Service Bus](/aspnet/signalr/overview/performance/scaleout-with-windows-azure-service-bus)
* [SignalR skalowania z Redis](/aspnet/signalr/overview/performance/scaleout-with-redis)
* [SignalR skalowania z SQL Server](/aspnet/signalr/overview/performance/scaleout-with-sql-server)

### <a name="aspnet-core"></a>ASP.NET Core

* [Usługa SignalR platformy Azure](/azure/azure-signalr/)
* [Redis](xref:signalr/redis-backplane)

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Centra](xref:signalr/hubs)
* [Klient środowiska JavaScript](xref:signalr/javascript-client)
* [Klient .NET](xref:signalr/dotnet-client)
* [Obsługiwane platformy](xref:signalr/supported-platforms)
