---
title: Obsługiwane platformy ASP.NET Core SignalR
author: bradygaster
description: Poznaj obsługiwane platformy dla ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- SignalR
uid: signalr/supported-platforms
ms.openlocfilehash: 86ba5b1aec230d78c1a0e1709187e129df6cb4cc
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/12/2019
ms.locfileid: "73963730"
---
# <a name="aspnet-core-opno-locsignalr-supported-platforms"></a><span data-ttu-id="9dcf2-103">Obsługiwane platformy ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="9dcf2-103">ASP.NET Core SignalR supported platforms</span></span>

## <a name="server-system-requirements"></a><span data-ttu-id="9dcf2-104">Wymagania systemowe serwera</span><span class="sxs-lookup"><span data-stu-id="9dcf2-104">Server system requirements</span></span>

SignalR<span data-ttu-id="9dcf2-105"> dla ASP.NET Core obsługuje dowolną platformę serwera, która ASP.NET Core obsługiwana przez program.</span><span class="sxs-lookup"><span data-stu-id="9dcf2-105"> for ASP.NET Core supports any server platform that ASP.NET Core supports.</span></span>

## <a name="javascript-client"></a><span data-ttu-id="9dcf2-106">Klient JavaScript</span><span class="sxs-lookup"><span data-stu-id="9dcf2-106">JavaScript client</span></span>

<span data-ttu-id="9dcf2-107">[Klient JavaScript](https://www.npmjs.com/package/@aspnet/signalr) działa w NodeJS 8 i nowszych wersjach oraz w następujących przeglądarkach:</span><span class="sxs-lookup"><span data-stu-id="9dcf2-107">The [JavaScript client](https://www.npmjs.com/package/@aspnet/signalr) runs on NodeJS 8 and later versions and the following browsers:</span></span>

| <span data-ttu-id="9dcf2-108">Przeglądarka</span><span class="sxs-lookup"><span data-stu-id="9dcf2-108">Browser</span></span>                         | <span data-ttu-id="9dcf2-109">Wersja</span><span class="sxs-lookup"><span data-stu-id="9dcf2-109">Version</span></span>         |
| ------------------------------- | --------------- |
| <span data-ttu-id="9dcf2-110">Microsoft Edge</span><span class="sxs-lookup"><span data-stu-id="9dcf2-110">Microsoft Edge</span></span>                  | <span data-ttu-id="9dcf2-111">Bieżąca&dagger;</span><span class="sxs-lookup"><span data-stu-id="9dcf2-111">Current&dagger;</span></span> |
| <span data-ttu-id="9dcf2-112">Mozilla Firefox</span><span class="sxs-lookup"><span data-stu-id="9dcf2-112">Mozilla Firefox</span></span>                 | <span data-ttu-id="9dcf2-113">Bieżąca&dagger;</span><span class="sxs-lookup"><span data-stu-id="9dcf2-113">Current&dagger;</span></span> |
| <span data-ttu-id="9dcf2-114">Google Chrome; obejmuje system Android</span><span class="sxs-lookup"><span data-stu-id="9dcf2-114">Google Chrome; includes Android</span></span> | <span data-ttu-id="9dcf2-115">Bieżąca&dagger;</span><span class="sxs-lookup"><span data-stu-id="9dcf2-115">Current&dagger;</span></span> |
| <span data-ttu-id="9dcf2-116">Safari obejmuje system iOS</span><span class="sxs-lookup"><span data-stu-id="9dcf2-116">Safari; includes iOS</span></span>            | <span data-ttu-id="9dcf2-117">Bieżąca&dagger;</span><span class="sxs-lookup"><span data-stu-id="9dcf2-117">Current&dagger;</span></span> |
| <span data-ttu-id="9dcf2-118">Microsoft Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="9dcf2-118">Microsoft Internet Explorer</span></span>     | <span data-ttu-id="9dcf2-119">11</span><span class="sxs-lookup"><span data-stu-id="9dcf2-119">11</span></span>              |

<span data-ttu-id="9dcf2-120">&dagger;*Current* odwołuje się do najnowszej wersji przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="9dcf2-120">&dagger;*Current* refers to the latest version of the browser.</span></span>

## <a name="net-client"></a><span data-ttu-id="9dcf2-121">Klient .NET</span><span class="sxs-lookup"><span data-stu-id="9dcf2-121">.NET client</span></span>

<span data-ttu-id="9dcf2-122">[Klient platformy .NET](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) działa na dowolnej platformie obsługiwanej przez ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="9dcf2-122">The [.NET client](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) runs on any platform supported by ASP.NET Core.</span></span> <span data-ttu-id="9dcf2-123">Na przykład [Deweloperzy platformy Xamarin mogą używać SignalR](https://github.com/aspnet/Announcements/issues/305) do tworzenia aplikacji dla systemu Android za pomocą platformy Xamarin. Android 8.4.0.1 i nowszych oraz aplikacji systemu iOS przy użyciu platformy Xamarin. iOS 11.14.0.4 i nowszych.</span><span class="sxs-lookup"><span data-stu-id="9dcf2-123">For example, [Xamarin developers can use SignalR](https://github.com/aspnet/Announcements/issues/305) for building Android apps using Xamarin.Android 8.4.0.1 and later and iOS apps using Xamarin.iOS 11.14.0.4 and later.</span></span>

<span data-ttu-id="9dcf2-124">Jeśli na serwerze są uruchomione usługi IIS, transport gniazd internetowych wymaga usług IIS 8,0 lub nowszych w systemie Windows Server 2012 lub nowszym.</span><span class="sxs-lookup"><span data-stu-id="9dcf2-124">If the server runs IIS, the WebSockets transport requires IIS 8.0 or later on Windows Server 2012 or later.</span></span> <span data-ttu-id="9dcf2-125">Inne transporty są obsługiwane na wszystkich platformach.</span><span class="sxs-lookup"><span data-stu-id="9dcf2-125">Other transports are supported on all platforms.</span></span>

## <a name="java-client"></a><span data-ttu-id="9dcf2-126">Klient Java</span><span class="sxs-lookup"><span data-stu-id="9dcf2-126">Java client</span></span>

<span data-ttu-id="9dcf2-127">[Klient Java](https://search.maven.org/artifact/com.microsoft.aspnet/signalr) obsługuje język Java 8 i nowsze wersje.</span><span class="sxs-lookup"><span data-stu-id="9dcf2-127">The [Java client](https://search.maven.org/artifact/com.microsoft.aspnet/signalr) supports Java 8 and later versions.</span></span>

## <a name="unsupported-clients"></a><span data-ttu-id="9dcf2-128">Nieobsługiwane klienci</span><span class="sxs-lookup"><span data-stu-id="9dcf2-128">Unsupported clients</span></span>

<span data-ttu-id="9dcf2-129">Następujący klienci są dostępni, ale są eksperymentalni lub nieoficjalni.</span><span class="sxs-lookup"><span data-stu-id="9dcf2-129">The following clients are available but are experimental or unofficial.</span></span> <span data-ttu-id="9dcf2-130">Nie są one obecnie obsługiwane i mogą nie być dostępne.</span><span class="sxs-lookup"><span data-stu-id="9dcf2-130">They aren't currently supported and may never be.</span></span>

* <span data-ttu-id="9dcf2-131">[C++Klient](https://github.com/aspnet/SignalR/tree/master/clients/cpp)</span><span class="sxs-lookup"><span data-stu-id="9dcf2-131">[C++ client](https://github.com/aspnet/SignalR/tree/master/clients/cpp)</span></span>

* <span data-ttu-id="9dcf2-132">[Klient SWIFT](https://github.com/moozzyk/SignalR-Client-Swift)</span><span class="sxs-lookup"><span data-stu-id="9dcf2-132">[Swift client](https://github.com/moozzyk/SignalR-Client-Swift)</span></span>
