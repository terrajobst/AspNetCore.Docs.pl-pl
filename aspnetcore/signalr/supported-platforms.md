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
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/27/2019
ms.locfileid: "64898624"
---
# <a name="aspnet-core-signalr-supported-platforms"></a><span data-ttu-id="ec1ea-103">Platformy obsługiwane przez SignalR platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ec1ea-103">ASP.NET Core SignalR supported platforms</span></span>

## <a name="server-system-requirements"></a><span data-ttu-id="ec1ea-104">Wymagania systemowe serwera</span><span class="sxs-lookup"><span data-stu-id="ec1ea-104">Server system requirements</span></span>

<span data-ttu-id="ec1ea-105">Biblioteka SignalR dla programu ASP.NET Core obsługuje związanej z platformą server, który obsługuje platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ec1ea-105">SignalR for ASP.NET Core supports any server platform that ASP.NET Core supports.</span></span>

## <a name="javascript-client"></a><span data-ttu-id="ec1ea-106">Klient JavaScript</span><span class="sxs-lookup"><span data-stu-id="ec1ea-106">JavaScript client</span></span>

<span data-ttu-id="ec1ea-107">[Klienta JavaScript](https://www.npmjs.com/package/@aspnet/signalr) działa w środowisku NodeJS 8 i nowszymi wersjami i następujących przeglądarek:</span><span class="sxs-lookup"><span data-stu-id="ec1ea-107">The [JavaScript client](https://www.npmjs.com/package/@aspnet/signalr) runs on NodeJS 8 and later versions and the following browsers:</span></span>

| <span data-ttu-id="ec1ea-108">Przeglądarka</span><span class="sxs-lookup"><span data-stu-id="ec1ea-108">Browser</span></span>                         | <span data-ttu-id="ec1ea-109">Wersja</span><span class="sxs-lookup"><span data-stu-id="ec1ea-109">Version</span></span> |
| ------------------------------- | ------- |
| <span data-ttu-id="ec1ea-110">Microsoft Edge</span><span class="sxs-lookup"><span data-stu-id="ec1ea-110">Microsoft Edge</span></span>                  | <span data-ttu-id="ec1ea-111">bieżący</span><span class="sxs-lookup"><span data-stu-id="ec1ea-111">current</span></span> |
| <span data-ttu-id="ec1ea-112">Mozilla Firefox</span><span class="sxs-lookup"><span data-stu-id="ec1ea-112">Mozilla Firefox</span></span>                 | <span data-ttu-id="ec1ea-113">bieżący</span><span class="sxs-lookup"><span data-stu-id="ec1ea-113">current</span></span> |
| <span data-ttu-id="ec1ea-114">Google Chrome; obejmuje systemu Android</span><span class="sxs-lookup"><span data-stu-id="ec1ea-114">Google Chrome; includes Android</span></span> | <span data-ttu-id="ec1ea-115">bieżący</span><span class="sxs-lookup"><span data-stu-id="ec1ea-115">current</span></span> |
| <span data-ttu-id="ec1ea-116">Safari; includes iOS</span><span class="sxs-lookup"><span data-stu-id="ec1ea-116">Safari; includes iOS</span></span>            | <span data-ttu-id="ec1ea-117">bieżący</span><span class="sxs-lookup"><span data-stu-id="ec1ea-117">current</span></span> |
| <span data-ttu-id="ec1ea-118">Microsoft Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="ec1ea-118">Microsoft Internet Explorer</span></span>     | <span data-ttu-id="ec1ea-119">11</span><span class="sxs-lookup"><span data-stu-id="ec1ea-119">11</span></span>      |
 
## <a name="net-client"></a><span data-ttu-id="ec1ea-120">Klient .NET</span><span class="sxs-lookup"><span data-stu-id="ec1ea-120">.NET client</span></span>

<span data-ttu-id="ec1ea-121">[Klient modelu .NET](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) działa na dowolnej platformie, obsługiwana przez platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ec1ea-121">The [.NET client](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) runs on any platform supported by ASP.NET Core.</span></span> <span data-ttu-id="ec1ea-122">Na przykład [Xamarin deweloperzy mogą używać SignalR](https://github.com/aspnet/Announcements/issues/305) do tworzenia aplikacji dla systemu Android, korzystając z platformy Xamarin.Android 8.4.0.1 i później oraz aplikacje dla systemu iOS przy użyciu rozszerzenia Xamarin.iOS 11.14.0.4 i nowszych.</span><span class="sxs-lookup"><span data-stu-id="ec1ea-122">For example, [Xamarin developers can use SignalR](https://github.com/aspnet/Announcements/issues/305) for building Android apps using Xamarin.Android 8.4.0.1 and later and iOS apps using Xamarin.iOS 11.14.0.4 and later.</span></span>

<span data-ttu-id="ec1ea-123">Jeśli na serwerze działa program IIS, transport gniazda Websocket wymaga usług IIS w wersji 8.0 lub nowszym w systemie Windows Server 2012 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="ec1ea-123">If the server runs IIS, the WebSockets transport requires IIS 8.0 or later on Windows Server 2012 or later.</span></span> <span data-ttu-id="ec1ea-124">Inne transportów są obsługiwane na wszystkich platformach.</span><span class="sxs-lookup"><span data-stu-id="ec1ea-124">Other transports are supported on all platforms.</span></span>

## <a name="java-client"></a><span data-ttu-id="ec1ea-125">Klient Java</span><span class="sxs-lookup"><span data-stu-id="ec1ea-125">Java client</span></span>

<span data-ttu-id="ec1ea-126">[Klienta Java](https://search.maven.org/artifact/com.microsoft.aspnet/signalr) obsługuje Java 8 i nowszych wersjach.</span><span class="sxs-lookup"><span data-stu-id="ec1ea-126">The [Java client](https://search.maven.org/artifact/com.microsoft.aspnet/signalr) supports Java 8 and later versions.</span></span>

## <a name="unsupported-clients"></a><span data-ttu-id="ec1ea-127">Nieobsługiwana klientów</span><span class="sxs-lookup"><span data-stu-id="ec1ea-127">Unsupported clients</span></span>

<span data-ttu-id="ec1ea-128">Następujący klienci są dostępne, ale są eksperymentalne lub nieoficjalny.</span><span class="sxs-lookup"><span data-stu-id="ec1ea-128">The following clients are available but are experimental or unofficial.</span></span> <span data-ttu-id="ec1ea-129">One nie są obecnie obsługiwane i nigdy nie może być.</span><span class="sxs-lookup"><span data-stu-id="ec1ea-129">They aren't currently supported and may never be.</span></span>

* [<span data-ttu-id="ec1ea-130">Klient języka C++</span><span class="sxs-lookup"><span data-stu-id="ec1ea-130">C++ client</span></span>](https://github.com/aspnet/SignalR/tree/master/clients/cpp)

* [<span data-ttu-id="ec1ea-131">Klient SWIFT</span><span class="sxs-lookup"><span data-stu-id="ec1ea-131">Swift client</span></span>](https://github.com/moozzyk/SignalR-Client-Swift)
