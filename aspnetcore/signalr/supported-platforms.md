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
# <a name="aspnet-core-signalr-supported-platforms"></a><span data-ttu-id="ea702-103">Platformy obsługiwane przez SignalR platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ea702-103">ASP.NET Core SignalR supported platforms</span></span>

## <a name="server-system-requirements"></a><span data-ttu-id="ea702-104">Wymagania systemowe serwera</span><span class="sxs-lookup"><span data-stu-id="ea702-104">Server system requirements</span></span>

<span data-ttu-id="ea702-105">Biblioteka SignalR dla programu ASP.NET Core obsługuje związanej z platformą server, które obsługuje platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ea702-105">SignalR for ASP.NET Core supports any server platform ASP.NET Core supports.</span></span>

## <a name="javascript-client"></a><span data-ttu-id="ea702-106">Klient JavaScript</span><span class="sxs-lookup"><span data-stu-id="ea702-106">JavaScript client</span></span>

<span data-ttu-id="ea702-107">[Klienta JavaScript](https://www.npmjs.com/package/@aspnet/signalr) działa w środowisku NodeJS 8 i nowszymi wersjami i następujących przeglądarek:</span><span class="sxs-lookup"><span data-stu-id="ea702-107">The [JavaScript client](https://www.npmjs.com/package/@aspnet/signalr) runs on NodeJS 8 and later versions and the following browsers:</span></span>

| <span data-ttu-id="ea702-108">Przeglądarka</span><span class="sxs-lookup"><span data-stu-id="ea702-108">Browser</span></span> | <span data-ttu-id="ea702-109">Wersja</span><span class="sxs-lookup"><span data-stu-id="ea702-109">Version</span></span> |
| ------- | ------- |
| <span data-ttu-id="ea702-110">Microsoft Edge</span><span class="sxs-lookup"><span data-stu-id="ea702-110">Microsoft Edge</span></span> | <span data-ttu-id="ea702-111">bieżący</span><span class="sxs-lookup"><span data-stu-id="ea702-111">current</span></span> |
| <span data-ttu-id="ea702-112">Mozilla Firefox</span><span class="sxs-lookup"><span data-stu-id="ea702-112">Mozilla Firefox</span></span> | <span data-ttu-id="ea702-113">bieżący</span><span class="sxs-lookup"><span data-stu-id="ea702-113">current</span></span> |
| <span data-ttu-id="ea702-114">Google Chrome; obejmuje systemu Android</span><span class="sxs-lookup"><span data-stu-id="ea702-114">Google Chrome; includes Android</span></span> | <span data-ttu-id="ea702-115">bieżący</span><span class="sxs-lookup"><span data-stu-id="ea702-115">current</span></span> |
| <span data-ttu-id="ea702-116">Safari; obejmuje dla systemu iOS</span><span class="sxs-lookup"><span data-stu-id="ea702-116">Safari; includes iOS</span></span> | <span data-ttu-id="ea702-117">bieżący</span><span class="sxs-lookup"><span data-stu-id="ea702-117">current</span></span> |
| <span data-ttu-id="ea702-118">Microsoft Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="ea702-118">Microsoft Internet Explorer</span></span> | <span data-ttu-id="ea702-119">11</span><span class="sxs-lookup"><span data-stu-id="ea702-119">11</span></span> |
 
## <a name="net-client"></a><span data-ttu-id="ea702-120">Klient modelu .NET</span><span class="sxs-lookup"><span data-stu-id="ea702-120">.NET client</span></span>

<span data-ttu-id="ea702-121">[Klient modelu .NET](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) działa na dowolnej platformie server obsługiwana przez platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ea702-121">The [.NET client](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) runs on any server platform supported by ASP.NET Core.</span></span>

<span data-ttu-id="ea702-122">Gdy serwer działa program IIS, transport gniazda Websocket wymaga usług IIS w wersji 8.0 lub nowszym, w systemie Windows Server 2012 lub nowszym.</span><span class="sxs-lookup"><span data-stu-id="ea702-122">When the server runs IIS, the WebSockets transport requires IIS 8.0 or higher, on Windows Server 2012 or higher.</span></span> <span data-ttu-id="ea702-123">Inne transportów są obsługiwane na wszystkich platformach.</span><span class="sxs-lookup"><span data-stu-id="ea702-123">Other transports are supported on all platforms.</span></span>

## <a name="java-client"></a><span data-ttu-id="ea702-124">Klienta Java</span><span class="sxs-lookup"><span data-stu-id="ea702-124">Java client</span></span>

<span data-ttu-id="ea702-125">[Klienta Java](https://search.maven.org/artifact/com.microsoft.aspnet/signalr) obsługuje Java 8 i nowszych wersjach.</span><span class="sxs-lookup"><span data-stu-id="ea702-125">The [Java client](https://search.maven.org/artifact/com.microsoft.aspnet/signalr) supports Java 8 and later versions.</span></span>

## <a name="unsupported-clients"></a><span data-ttu-id="ea702-126">Nieobsługiwana klientów</span><span class="sxs-lookup"><span data-stu-id="ea702-126">Unsupported clients</span></span>

<span data-ttu-id="ea702-127">Następujący klienci są dostępne, ale są eksperymentalne lub nieoficjalny.</span><span class="sxs-lookup"><span data-stu-id="ea702-127">The following clients are available but are experimental or unofficial.</span></span> <span data-ttu-id="ea702-128">One nie są obecnie obsługiwane i może nigdy nie jest nieobsługiwany.</span><span class="sxs-lookup"><span data-stu-id="ea702-128">They are not supported now and may not ever be supported.</span></span>

* [<span data-ttu-id="ea702-129">Klient języka C++</span><span class="sxs-lookup"><span data-stu-id="ea702-129">C++ client</span></span>](https://github.com/aspnet/SignalR/tree/master/clients/cpp)

* [<span data-ttu-id="ea702-130">Klient SWIFT</span><span class="sxs-lookup"><span data-stu-id="ea702-130">Swift client</span></span>](https://github.com/moozzyk/SignalR-Client-Swift)
