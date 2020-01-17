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
# <a name="aspnet-core-opno-locsignalr-supported-platforms"></a><span data-ttu-id="a6fa3-103">Obsługiwane platformy ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="a6fa3-103">ASP.NET Core SignalR supported platforms</span></span>

## <a name="server-system-requirements"></a><span data-ttu-id="a6fa3-104">Wymagania systemowe serwera</span><span class="sxs-lookup"><span data-stu-id="a6fa3-104">Server system requirements</span></span>

SignalR<span data-ttu-id="a6fa3-105"> dla ASP.NET Core obsługuje dowolną platformę serwera, która ASP.NET Core obsługiwana przez program.</span><span class="sxs-lookup"><span data-stu-id="a6fa3-105"> for ASP.NET Core supports any server platform that ASP.NET Core supports.</span></span>

## <a name="javascript-client"></a><span data-ttu-id="a6fa3-106">Klient JavaScript</span><span class="sxs-lookup"><span data-stu-id="a6fa3-106">JavaScript client</span></span>

<span data-ttu-id="a6fa3-107">[Klient JavaScript](xref:signalr/javascript-client) działa w NodeJS 8 i nowszych wersjach oraz w następujących przeglądarkach:</span><span class="sxs-lookup"><span data-stu-id="a6fa3-107">The [JavaScript client](xref:signalr/javascript-client) runs on NodeJS 8 and later versions and the following browsers:</span></span>

| <span data-ttu-id="a6fa3-108">Przeglądarka</span><span class="sxs-lookup"><span data-stu-id="a6fa3-108">Browser</span></span>                         | <span data-ttu-id="a6fa3-109">Wersja</span><span class="sxs-lookup"><span data-stu-id="a6fa3-109">Version</span></span>         |
| ------------------------------- | --------------- |
| <span data-ttu-id="a6fa3-110">Microsoft Edge</span><span class="sxs-lookup"><span data-stu-id="a6fa3-110">Microsoft Edge</span></span>                  | <span data-ttu-id="a6fa3-111">Bieżąca&dagger;</span><span class="sxs-lookup"><span data-stu-id="a6fa3-111">Current&dagger;</span></span> |
| <span data-ttu-id="a6fa3-112">Mozilla Firefox</span><span class="sxs-lookup"><span data-stu-id="a6fa3-112">Mozilla Firefox</span></span>                 | <span data-ttu-id="a6fa3-113">Bieżąca&dagger;</span><span class="sxs-lookup"><span data-stu-id="a6fa3-113">Current&dagger;</span></span> |
| <span data-ttu-id="a6fa3-114">Google Chrome; obejmuje system Android</span><span class="sxs-lookup"><span data-stu-id="a6fa3-114">Google Chrome; includes Android</span></span> | <span data-ttu-id="a6fa3-115">Bieżąca&dagger;</span><span class="sxs-lookup"><span data-stu-id="a6fa3-115">Current&dagger;</span></span> |
| <span data-ttu-id="a6fa3-116">Safari obejmuje system iOS</span><span class="sxs-lookup"><span data-stu-id="a6fa3-116">Safari; includes iOS</span></span>            | <span data-ttu-id="a6fa3-117">Bieżąca&dagger;</span><span class="sxs-lookup"><span data-stu-id="a6fa3-117">Current&dagger;</span></span> |
| <span data-ttu-id="a6fa3-118">Microsoft Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="a6fa3-118">Microsoft Internet Explorer</span></span>     | <span data-ttu-id="a6fa3-119">11</span><span class="sxs-lookup"><span data-stu-id="a6fa3-119">11</span></span>              |

<span data-ttu-id="a6fa3-120">&dagger;*Current* odwołuje się do najnowszej wersji przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="a6fa3-120">&dagger;*Current* refers to the latest version of the browser.</span></span>

## <a name="net-client"></a><span data-ttu-id="a6fa3-121">Klient .NET</span><span class="sxs-lookup"><span data-stu-id="a6fa3-121">.NET client</span></span>

<span data-ttu-id="a6fa3-122">[Klient platformy .NET](xref:signalr/dotnet-client) działa na dowolnej platformie obsługiwanej przez ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a6fa3-122">The [.NET client](xref:signalr/dotnet-client) runs on any platform supported by ASP.NET Core.</span></span> <span data-ttu-id="a6fa3-123">Na przykład [Deweloperzy platformy Xamarin mogą używać SignalR](https://github.com/aspnet/Announcements/issues/305) do tworzenia aplikacji dla systemu Android za pomocą platformy Xamarin. Android 8.4.0.1 i nowszych oraz aplikacji systemu iOS przy użyciu platformy Xamarin. iOS 11.14.0.4 i nowszych.</span><span class="sxs-lookup"><span data-stu-id="a6fa3-123">For example, [Xamarin developers can use SignalR](https://github.com/aspnet/Announcements/issues/305) for building Android apps using Xamarin.Android 8.4.0.1 and later and iOS apps using Xamarin.iOS 11.14.0.4 and later.</span></span>

<span data-ttu-id="a6fa3-124">Jeśli na serwerze są uruchomione usługi IIS, transport gniazd internetowych wymaga usług IIS 8,0 lub nowszych w systemie Windows Server 2012 lub nowszym.</span><span class="sxs-lookup"><span data-stu-id="a6fa3-124">If the server runs IIS, the WebSockets transport requires IIS 8.0 or later on Windows Server 2012 or later.</span></span> <span data-ttu-id="a6fa3-125">Inne transporty są obsługiwane na wszystkich platformach.</span><span class="sxs-lookup"><span data-stu-id="a6fa3-125">Other transports are supported on all platforms.</span></span>

## <a name="java-client"></a><span data-ttu-id="a6fa3-126">Klient Java</span><span class="sxs-lookup"><span data-stu-id="a6fa3-126">Java client</span></span>

<span data-ttu-id="a6fa3-127">[Klient Java](xref:signalr/java-client) obsługuje język Java 8 i nowsze wersje.</span><span class="sxs-lookup"><span data-stu-id="a6fa3-127">The [Java client](xref:signalr/java-client) supports Java 8 and later versions.</span></span>

## <a name="unsupported-clients"></a><span data-ttu-id="a6fa3-128">Nieobsługiwane klienci</span><span class="sxs-lookup"><span data-stu-id="a6fa3-128">Unsupported clients</span></span>

<span data-ttu-id="a6fa3-129">Następujący klienci są dostępni, ale są eksperymentalni lub nieoficjalni.</span><span class="sxs-lookup"><span data-stu-id="a6fa3-129">The following clients are available but are experimental or unofficial.</span></span> <span data-ttu-id="a6fa3-130">Nie są one obecnie obsługiwane i mogą nie być dostępne.</span><span class="sxs-lookup"><span data-stu-id="a6fa3-130">They aren't currently supported and may never be.</span></span>

* <span data-ttu-id="a6fa3-131">[C++Klient](https://github.com/aspnet/SignalR-Client-Cpp)</span><span class="sxs-lookup"><span data-stu-id="a6fa3-131">[C++ client](https://github.com/aspnet/SignalR-Client-Cpp)</span></span>

* <span data-ttu-id="a6fa3-132">[Klient SWIFT](https://github.com/moozzyk/SignalR-Client-Swift)</span><span class="sxs-lookup"><span data-stu-id="a6fa3-132">[Swift client](https://github.com/moozzyk/SignalR-Client-Swift)</span></span>
