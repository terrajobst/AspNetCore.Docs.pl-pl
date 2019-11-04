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
# <a name="aspnet-core-signalr-supported-platforms"></a><span data-ttu-id="bc972-103">Obsługiwane platformy ASP.NET Core sygnalizujące</span><span class="sxs-lookup"><span data-stu-id="bc972-103">ASP.NET Core SignalR supported platforms</span></span>

## <a name="server-system-requirements"></a><span data-ttu-id="bc972-104">Wymagania systemowe serwera</span><span class="sxs-lookup"><span data-stu-id="bc972-104">Server system requirements</span></span>

<span data-ttu-id="bc972-105">Program sygnalizujący dla ASP.NET Core obsługuje dowolną platformę serwera, którą ASP.NET Core obsługuje.</span><span class="sxs-lookup"><span data-stu-id="bc972-105">SignalR for ASP.NET Core supports any server platform that ASP.NET Core supports.</span></span>

## <a name="javascript-client"></a><span data-ttu-id="bc972-106">Klient JavaScript</span><span class="sxs-lookup"><span data-stu-id="bc972-106">JavaScript client</span></span>

<span data-ttu-id="bc972-107">[Klient JavaScript](https://www.npmjs.com/package/@aspnet/signalr) działa w NodeJS 8 i nowszych wersjach oraz w następujących przeglądarkach:</span><span class="sxs-lookup"><span data-stu-id="bc972-107">The [JavaScript client](https://www.npmjs.com/package/@aspnet/signalr) runs on NodeJS 8 and later versions and the following browsers:</span></span>

| <span data-ttu-id="bc972-108">Przeglądarka</span><span class="sxs-lookup"><span data-stu-id="bc972-108">Browser</span></span>                         | <span data-ttu-id="bc972-109">Wersja</span><span class="sxs-lookup"><span data-stu-id="bc972-109">Version</span></span>         |
| ------------------------------- | --------------- |
| <span data-ttu-id="bc972-110">Microsoft Edge</span><span class="sxs-lookup"><span data-stu-id="bc972-110">Microsoft Edge</span></span>                  | <span data-ttu-id="bc972-111">Bieżąca&dagger;</span><span class="sxs-lookup"><span data-stu-id="bc972-111">Current&dagger;</span></span> |
| <span data-ttu-id="bc972-112">Mozilla Firefox</span><span class="sxs-lookup"><span data-stu-id="bc972-112">Mozilla Firefox</span></span>                 | <span data-ttu-id="bc972-113">Bieżąca&dagger;</span><span class="sxs-lookup"><span data-stu-id="bc972-113">Current&dagger;</span></span> |
| <span data-ttu-id="bc972-114">Google Chrome; obejmuje system Android</span><span class="sxs-lookup"><span data-stu-id="bc972-114">Google Chrome; includes Android</span></span> | <span data-ttu-id="bc972-115">Bieżąca&dagger;</span><span class="sxs-lookup"><span data-stu-id="bc972-115">Current&dagger;</span></span> |
| <span data-ttu-id="bc972-116">Safari obejmuje system iOS</span><span class="sxs-lookup"><span data-stu-id="bc972-116">Safari; includes iOS</span></span>            | <span data-ttu-id="bc972-117">Bieżąca&dagger;</span><span class="sxs-lookup"><span data-stu-id="bc972-117">Current&dagger;</span></span> |
| <span data-ttu-id="bc972-118">Microsoft Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="bc972-118">Microsoft Internet Explorer</span></span>     | <span data-ttu-id="bc972-119">11</span><span class="sxs-lookup"><span data-stu-id="bc972-119">11</span></span>              |

<span data-ttu-id="bc972-120">&dagger;*Current* odwołuje się do najnowszej wersji przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="bc972-120">&dagger;*Current* refers to the latest version of the browser.</span></span>

## <a name="net-client"></a><span data-ttu-id="bc972-121">Klient .NET</span><span class="sxs-lookup"><span data-stu-id="bc972-121">.NET client</span></span>

<span data-ttu-id="bc972-122">[Klient platformy .NET](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) działa na dowolnej platformie obsługiwanej przez ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="bc972-122">The [.NET client](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) runs on any platform supported by ASP.NET Core.</span></span> <span data-ttu-id="bc972-123">Na przykład [Deweloperzy platformy Xamarin mogą używać sygnałów](https://github.com/aspnet/Announcements/issues/305) do kompilowania aplikacji dla systemu Android za pomocą platformy Xamarin. Android 8.4.0.1 i nowszych oraz aplikacji systemu iOS przy użyciu platformy Xamarin. iOS 11.14.0.4 i nowszych.</span><span class="sxs-lookup"><span data-stu-id="bc972-123">For example, [Xamarin developers can use SignalR](https://github.com/aspnet/Announcements/issues/305) for building Android apps using Xamarin.Android 8.4.0.1 and later and iOS apps using Xamarin.iOS 11.14.0.4 and later.</span></span>

<span data-ttu-id="bc972-124">Jeśli na serwerze są uruchomione usługi IIS, transport gniazd internetowych wymaga usług IIS 8,0 lub nowszych w systemie Windows Server 2012 lub nowszym.</span><span class="sxs-lookup"><span data-stu-id="bc972-124">If the server runs IIS, the WebSockets transport requires IIS 8.0 or later on Windows Server 2012 or later.</span></span> <span data-ttu-id="bc972-125">Inne transporty są obsługiwane na wszystkich platformach.</span><span class="sxs-lookup"><span data-stu-id="bc972-125">Other transports are supported on all platforms.</span></span>

## <a name="java-client"></a><span data-ttu-id="bc972-126">Klient Java</span><span class="sxs-lookup"><span data-stu-id="bc972-126">Java client</span></span>

<span data-ttu-id="bc972-127">[Klient Java](https://search.maven.org/artifact/com.microsoft.aspnet/signalr) obsługuje język Java 8 i nowsze wersje.</span><span class="sxs-lookup"><span data-stu-id="bc972-127">The [Java client](https://search.maven.org/artifact/com.microsoft.aspnet/signalr) supports Java 8 and later versions.</span></span>

## <a name="unsupported-clients"></a><span data-ttu-id="bc972-128">Nieobsługiwane klienci</span><span class="sxs-lookup"><span data-stu-id="bc972-128">Unsupported clients</span></span>

<span data-ttu-id="bc972-129">Następujący klienci są dostępni, ale są eksperymentalni lub nieoficjalni.</span><span class="sxs-lookup"><span data-stu-id="bc972-129">The following clients are available but are experimental or unofficial.</span></span> <span data-ttu-id="bc972-130">Nie są one obecnie obsługiwane i mogą nie być dostępne.</span><span class="sxs-lookup"><span data-stu-id="bc972-130">They aren't currently supported and may never be.</span></span>

* [<span data-ttu-id="bc972-131">C++Klient</span><span class="sxs-lookup"><span data-stu-id="bc972-131">C++ client</span></span>](https://github.com/aspnet/SignalR/tree/master/clients/cpp)

* [<span data-ttu-id="bc972-132">Klient SWIFT</span><span class="sxs-lookup"><span data-stu-id="bc972-132">Swift client</span></span>](https://github.com/moozzyk/SignalR-Client-Swift)
