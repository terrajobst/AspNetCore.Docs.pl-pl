---
title: Platformy obsługiwane przez SignalR platformy ASP.NET Core
author: bradygaster
description: Dowiedz się więcej o obsługiwane platformy dla biblioteki SignalR platformy ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 04/06/2019
uid: signalr/supported-platforms
ms.openlocfilehash: fefaaf97de3f1fabf8f3154bf5b4ccb37195ccff
ms.sourcegitcommit: 78339e9891c8676db01a6e81e9cb0cdaa280162f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/17/2019
ms.locfileid: "59068225"
---
# <a name="aspnet-core-signalr-supported-platforms"></a>Platformy obsługiwane przez SignalR platformy ASP.NET Core

## <a name="server-system-requirements"></a>Wymagania systemowe serwera

Biblioteka SignalR dla programu ASP.NET Core obsługuje związanej z platformą server, który obsługuje platformy ASP.NET Core.

## <a name="javascript-client"></a>Klient JavaScript

[Klienta JavaScript](https://www.npmjs.com/package/@aspnet/signalr) działa w środowisku NodeJS 8 i nowszymi wersjami i następujących przeglądarek:

| Przeglądarka                         | Wersja |
| ------------------------------- | ------- |
| Microsoft Edge                  | bieżący |
| Mozilla Firefox                 | bieżący |
| Google Chrome; obejmuje systemu Android | bieżący |
| Safari; includes iOS            | bieżący |
| Microsoft Internet Explorer     | 11      |
 
## <a name="net-client"></a>Klient .NET

[Klient modelu .NET](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) działa na dowolnej platformie, obsługiwana przez platformy ASP.NET Core. Na przykład [Xamarin deweloperzy mogą używać SignalR](https://github.com/aspnet/Announcements/issues/305) do tworzenia aplikacji dla systemu Android, korzystając z platformy Xamarin.Android 8.4.0.1 i później oraz aplikacje dla systemu iOS przy użyciu rozszerzenia Xamarin.iOS 11.14.0.4 i nowszych.

Jeśli na serwerze działa program IIS, transport gniazda Websocket wymaga usług IIS w wersji 8.0 lub nowszym w systemie Windows Server 2012 lub nowszej. Inne transportów są obsługiwane na wszystkich platformach.

## <a name="java-client"></a>Klient Java

[Klienta Java](https://search.maven.org/artifact/com.microsoft.aspnet/signalr) obsługuje Java 8 i nowszych wersjach.

## <a name="unsupported-clients"></a>Nieobsługiwana klientów

Następujący klienci są dostępne, ale są eksperymentalne lub nieoficjalny. One nie są obecnie obsługiwane i nigdy nie może być.

* [Klient języka C++](https://github.com/aspnet/SignalR/tree/master/clients/cpp)

* [Klient SWIFT](https://github.com/moozzyk/SignalR-Client-Swift)
