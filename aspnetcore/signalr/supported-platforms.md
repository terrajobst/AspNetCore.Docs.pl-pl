---
title: Platformy obsługiwane przez SignalR platformy ASP.NET Core
author: tdykstra
description: Obsługiwane platformy dla biblioteki SignalR platformy ASP.NET Core
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/26/2018
uid: signalr/supported-platforms
ms.openlocfilehash: d6d74a55d35ddb34a6f66a171bfe3f343dd61b63
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577629"
---
# <a name="aspnet-core-signalr-supported-platforms"></a>Platformy obsługiwane przez SignalR platformy ASP.NET Core

## <a name="server-system-requirements"></a>Wymagania systemowe serwera

Biblioteka SignalR dla programu ASP.NET Core obsługuje związanej z platformą server, które obsługuje platformy ASP.NET Core.

## <a name="javascript-client"></a>Klient JavaScript

[Klienta JavaScript](https://www.npmjs.com/package/@aspnet/signalr) działa w środowisku NodeJS 8 i nowszymi wersjami i następujących przeglądarek:

| Przeglądarka | Wersja |
| ------- | ------- |
| Microsoft Edge | bieżący |
| Mozilla Firefox | bieżący |
| Google Chrome; obejmuje systemu Android | bieżący |
| Safari; obejmuje dla systemu iOS | bieżący |
| Microsoft Internet Explorer | 11 |
 
## <a name="net-client"></a>Klient modelu .NET

[Klient modelu .NET](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) działa na dowolnej platformie server obsługiwana przez platformy ASP.NET Core.

Gdy serwer działa program IIS, transport gniazda Websocket wymaga usług IIS w wersji 8.0 lub nowszym, w systemie Windows Server 2012 lub nowszym. Inne transportów są obsługiwane na wszystkich platformach.

## <a name="java-client"></a>Klienta Java

[Klienta Java](https://search.maven.org/artifact/com.microsoft.aspnet/signalr) obsługuje Java 8 i nowszych wersjach.

## <a name="unsupported-clients"></a>Nieobsługiwana klientów

Następujący klienci są dostępne, ale są eksperymentalne lub nieoficjalny. One nie są obecnie obsługiwane i może nigdy nie jest nieobsługiwany.

* [Klient języka C++](https://github.com/aspnet/SignalR/tree/master/clients/cpp)

* [Klient SWIFT](https://github.com/moozzyk/SignalR-Client-Swift)
