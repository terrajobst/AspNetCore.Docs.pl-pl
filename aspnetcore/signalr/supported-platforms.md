---
title: Obsługiwane platformy ASP.NET Core SignalR
author: bradygaster
description: Poznaj obsługiwane platformy dla ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 01/16/2020
no-loc:
- SignalR
uid: signalr/supported-platforms
ms.openlocfilehash: 054965921c87c1a9be27e5ddaa8a87b0fa1f4113
ms.sourcegitcommit: cbd30479f42cbb3385000ef834d9c7d021fd218d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/16/2020
ms.locfileid: "76146501"
---
# <a name="aspnet-core-opno-locsignalr-supported-platforms"></a>Obsługiwane platformy ASP.NET Core SignalR

## <a name="server-system-requirements"></a>Wymagania systemowe serwera

SignalR dla ASP.NET Core obsługuje dowolną platformę serwera, która ASP.NET Core obsługiwana przez program.

## <a name="javascript-client"></a>Klient JavaScript

[Klient JavaScript](xref:signalr/javascript-client) działa w NodeJS 8 i nowszych wersjach oraz w następujących przeglądarkach:

| Przeglądarka                         | Wersja         |
| ------------------------------- | --------------- |
| Microsoft Edge                  | Bieżąca&dagger; |
| Mozilla Firefox                 | Bieżąca&dagger; |
| Google Chrome; obejmuje system Android | Bieżąca&dagger; |
| Safari obejmuje system iOS            | Bieżąca&dagger; |
| Microsoft Internet Explorer     | 11              |

&dagger;*Current* odwołuje się do najnowszej wersji przeglądarki.

## <a name="net-client"></a>Klient .NET

[Klient platformy .NET](xref:signalr/dotnet-client) działa na dowolnej platformie obsługiwanej przez ASP.NET Core. Na przykład [Deweloperzy platformy Xamarin mogą używać SignalR](https://github.com/aspnet/Announcements/issues/305) do tworzenia aplikacji dla systemu Android za pomocą platformy Xamarin. Android 8.4.0.1 i nowszych oraz aplikacji systemu iOS przy użyciu platformy Xamarin. iOS 11.14.0.4 i nowszych.

Jeśli na serwerze są uruchomione usługi IIS, transport gniazd internetowych wymaga usług IIS 8,0 lub nowszych w systemie Windows Server 2012 lub nowszym. Inne transporty są obsługiwane na wszystkich platformach.

## <a name="java-client"></a>Klient Java

[Klient Java](xref:signalr/java-client) obsługuje język Java 8 i nowsze wersje.

## <a name="unsupported-clients"></a>Nieobsługiwane klienci

Następujący klienci są dostępni, ale są eksperymentalni lub nieoficjalni. Nie są one obecnie obsługiwane i mogą nie być dostępne.

* [C++Klient](https://github.com/aspnet/SignalR-Client-Cpp)

* [Klient SWIFT](https://github.com/moozzyk/SignalR-Client-Swift)
