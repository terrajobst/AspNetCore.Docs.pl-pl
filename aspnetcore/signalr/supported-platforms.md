---
title: Obsługiwane platformy ASP.NET Core sygnalizujące
author: bradygaster
description: Dowiedz się więcej na temat obsługiwanych platform dla ASP.NET Core sygnalizującego.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/01/2019
uid: signalr/supported-platforms
ms.openlocfilehash: 1be7a307710e6e522c0088fd1ca01da11a13eda1
ms.sourcegitcommit: 77c8be22d5e88dd710f42c739748869f198865dd
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/01/2019
ms.locfileid: "73426972"
---
# <a name="aspnet-core-signalr-supported-platforms"></a>Obsługiwane platformy ASP.NET Core sygnalizujące

## <a name="server-system-requirements"></a>Wymagania systemowe serwera

Program sygnalizujący dla ASP.NET Core obsługuje dowolną platformę serwera, którą ASP.NET Core obsługuje.

## <a name="javascript-client"></a>Klient JavaScript

[Klient JavaScript](https://www.npmjs.com/package/@aspnet/signalr) działa w NodeJS 8 i nowszych wersjach oraz w następujących przeglądarkach:

| Przeglądarka                         | Wersja         |
| ------------------------------- | --------------- |
| Microsoft Edge                  | Bieżąca&dagger; |
| Mozilla Firefox                 | Bieżąca&dagger; |
| Google Chrome; obejmuje system Android | Bieżąca&dagger; |
| Safari obejmuje system iOS            | Bieżąca&dagger; |
| Microsoft Internet Explorer     | 11              |

&dagger;*Current* odwołuje się do najnowszej wersji przeglądarki.

## <a name="net-client"></a>Klient .NET

[Klient platformy .NET](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) działa na dowolnej platformie obsługiwanej przez ASP.NET Core. Na przykład [Deweloperzy platformy Xamarin mogą używać sygnałów](https://github.com/aspnet/Announcements/issues/305) do kompilowania aplikacji dla systemu Android za pomocą platformy Xamarin. Android 8.4.0.1 i nowszych oraz aplikacji systemu iOS przy użyciu platformy Xamarin. iOS 11.14.0.4 i nowszych.

Jeśli na serwerze są uruchomione usługi IIS, transport gniazd internetowych wymaga usług IIS 8,0 lub nowszych w systemie Windows Server 2012 lub nowszym. Inne transporty są obsługiwane na wszystkich platformach.

## <a name="java-client"></a>Klient Java

[Klient Java](https://search.maven.org/artifact/com.microsoft.aspnet/signalr) obsługuje język Java 8 i nowsze wersje.

## <a name="unsupported-clients"></a>Nieobsługiwane klienci

Następujący klienci są dostępni, ale są eksperymentalni lub nieoficjalni. Nie są one obecnie obsługiwane i mogą nie być dostępne.

* [C++Klient](https://github.com/aspnet/SignalR/tree/master/clients/cpp)

* [Klient SWIFT](https://github.com/moozzyk/SignalR-Client-Swift)
