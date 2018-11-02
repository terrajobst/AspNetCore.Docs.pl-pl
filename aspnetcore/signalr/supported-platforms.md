---
title: Platformy obsługiwane przez SignalR platformy ASP.NET Core
author: tdykstra
description: Dowiedz się więcej o obsługiwane platformy dla biblioteki SignalR platformy ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 10/31/2018
uid: signalr/supported-platforms
ms.openlocfilehash: 773f6c020dbb2982911e177b55855473c750d52a
ms.sourcegitcommit: fc2486ddbeb15ab4969168d99b3fe0fbe91e8661
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/01/2018
ms.locfileid: "50758183"
---
# <a name="aspnet-core-signalr-supported-platforms"></a><span data-ttu-id="1e240-103">Platformy obsługiwane przez SignalR platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1e240-103">ASP.NET Core SignalR supported platforms</span></span>

## <a name="server-system-requirements"></a><span data-ttu-id="1e240-104">Wymagania systemowe serwera</span><span class="sxs-lookup"><span data-stu-id="1e240-104">Server system requirements</span></span>

<span data-ttu-id="1e240-105">Biblioteka SignalR dla programu ASP.NET Core obsługuje związanej z platformą server, który obsługuje platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1e240-105">SignalR for ASP.NET Core supports any server platform that ASP.NET Core supports.</span></span>

## <a name="javascript-client"></a><span data-ttu-id="1e240-106">Klient JavaScript</span><span class="sxs-lookup"><span data-stu-id="1e240-106">JavaScript client</span></span>

<span data-ttu-id="1e240-107">[Klienta JavaScript](https://www.npmjs.com/package/@aspnet/signalr) działa w środowisku NodeJS 8 i nowszymi wersjami i następujących przeglądarek:</span><span class="sxs-lookup"><span data-stu-id="1e240-107">The [JavaScript client](https://www.npmjs.com/package/@aspnet/signalr) runs on NodeJS 8 and later versions and the following browsers:</span></span>

| <span data-ttu-id="1e240-108">Przeglądarka</span><span class="sxs-lookup"><span data-stu-id="1e240-108">Browser</span></span>                         | <span data-ttu-id="1e240-109">Wersja</span><span class="sxs-lookup"><span data-stu-id="1e240-109">Version</span></span> |
| ------------------------------- | ------- |
| <span data-ttu-id="1e240-110">Microsoft Edge</span><span class="sxs-lookup"><span data-stu-id="1e240-110">Microsoft Edge</span></span>                  | <span data-ttu-id="1e240-111">bieżący</span><span class="sxs-lookup"><span data-stu-id="1e240-111">current</span></span> |
| <span data-ttu-id="1e240-112">Mozilla Firefox</span><span class="sxs-lookup"><span data-stu-id="1e240-112">Mozilla Firefox</span></span>                 | <span data-ttu-id="1e240-113">bieżący</span><span class="sxs-lookup"><span data-stu-id="1e240-113">current</span></span> |
| <span data-ttu-id="1e240-114">Google Chrome; obejmuje systemu Android</span><span class="sxs-lookup"><span data-stu-id="1e240-114">Google Chrome; includes Android</span></span> | <span data-ttu-id="1e240-115">bieżący</span><span class="sxs-lookup"><span data-stu-id="1e240-115">current</span></span> |
| <span data-ttu-id="1e240-116">Safari; obejmuje dla systemu iOS</span><span class="sxs-lookup"><span data-stu-id="1e240-116">Safari; includes iOS</span></span>            | <span data-ttu-id="1e240-117">bieżący</span><span class="sxs-lookup"><span data-stu-id="1e240-117">current</span></span> |
| <span data-ttu-id="1e240-118">Microsoft Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="1e240-118">Microsoft Internet Explorer</span></span>     | <span data-ttu-id="1e240-119">11</span><span class="sxs-lookup"><span data-stu-id="1e240-119">11</span></span>      |
 
## <a name="net-client"></a><span data-ttu-id="1e240-120">Klient modelu .NET</span><span class="sxs-lookup"><span data-stu-id="1e240-120">.NET client</span></span>

<span data-ttu-id="1e240-121">[Klient modelu .NET](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) działa na dowolnej platformie server obsługiwana przez platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1e240-121">The [.NET client](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) runs on any server platform supported by ASP.NET Core.</span></span>

<span data-ttu-id="1e240-122">Jeśli na serwerze działa program IIS, transport gniazda Websocket wymaga usług IIS 8.0 lub nowszego w systemie Windows Server 2012 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="1e240-122">If the server runs IIS, the WebSockets transport requires IIS 8.0 or higher on Windows Server 2012 or higher.</span></span> <span data-ttu-id="1e240-123">Inne transportów są obsługiwane na wszystkich platformach.</span><span class="sxs-lookup"><span data-stu-id="1e240-123">Other transports are supported on all platforms.</span></span>

## <a name="java-client"></a><span data-ttu-id="1e240-124">Klienta Java</span><span class="sxs-lookup"><span data-stu-id="1e240-124">Java client</span></span>

<span data-ttu-id="1e240-125">[Klienta Java](https://search.maven.org/artifact/com.microsoft.aspnet/signalr) obsługuje Java 8 i nowszych wersjach.</span><span class="sxs-lookup"><span data-stu-id="1e240-125">The [Java client](https://search.maven.org/artifact/com.microsoft.aspnet/signalr) supports Java 8 and later versions.</span></span>

## <a name="unsupported-clients"></a><span data-ttu-id="1e240-126">Nieobsługiwana klientów</span><span class="sxs-lookup"><span data-stu-id="1e240-126">Unsupported clients</span></span>

<span data-ttu-id="1e240-127">Następujący klienci są dostępne, ale są eksperymentalne lub nieoficjalny.</span><span class="sxs-lookup"><span data-stu-id="1e240-127">The following clients are available but are experimental or unofficial.</span></span> <span data-ttu-id="1e240-128">One nie są obecnie obsługiwane i nigdy nie może być.</span><span class="sxs-lookup"><span data-stu-id="1e240-128">They aren't currently supported and may never be.</span></span>

* [<span data-ttu-id="1e240-129">Klient języka C++</span><span class="sxs-lookup"><span data-stu-id="1e240-129">C++ client</span></span>](https://github.com/aspnet/SignalR/tree/master/clients/cpp)

* [<span data-ttu-id="1e240-130">Klient SWIFT</span><span class="sxs-lookup"><span data-stu-id="1e240-130">Swift client</span></span>](https://github.com/moozzyk/SignalR-Client-Swift)
