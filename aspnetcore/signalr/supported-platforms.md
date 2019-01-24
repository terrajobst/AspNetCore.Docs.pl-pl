---
title: Platformy obsługiwane przez SignalR platformy ASP.NET Core
author: bradygaster
description: Dowiedz się więcej o obsługiwane platformy dla biblioteki SignalR platformy ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/12/2018
uid: signalr/supported-platforms
ms.openlocfilehash: e4e84baf0120036b473eac256107b46a4accfe37
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2019
ms.locfileid: "54837718"
---
# <a name="aspnet-core-signalr-supported-platforms"></a><span data-ttu-id="cd586-103">Platformy obsługiwane przez SignalR platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="cd586-103">ASP.NET Core SignalR supported platforms</span></span>

## <a name="server-system-requirements"></a><span data-ttu-id="cd586-104">Wymagania systemowe serwera</span><span class="sxs-lookup"><span data-stu-id="cd586-104">Server system requirements</span></span>

<span data-ttu-id="cd586-105">Biblioteka SignalR dla programu ASP.NET Core obsługuje związanej z platformą server, który obsługuje platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="cd586-105">SignalR for ASP.NET Core supports any server platform that ASP.NET Core supports.</span></span>

## <a name="javascript-client"></a><span data-ttu-id="cd586-106">Klient JavaScript</span><span class="sxs-lookup"><span data-stu-id="cd586-106">JavaScript client</span></span>

<span data-ttu-id="cd586-107">[Klienta JavaScript](https://www.npmjs.com/package/@aspnet/signalr) działa w środowisku NodeJS 8 i nowszymi wersjami i następujących przeglądarek:</span><span class="sxs-lookup"><span data-stu-id="cd586-107">The [JavaScript client](https://www.npmjs.com/package/@aspnet/signalr) runs on NodeJS 8 and later versions and the following browsers:</span></span>

| <span data-ttu-id="cd586-108">Przeglądarka</span><span class="sxs-lookup"><span data-stu-id="cd586-108">Browser</span></span>                         | <span data-ttu-id="cd586-109">Wersja</span><span class="sxs-lookup"><span data-stu-id="cd586-109">Version</span></span> |
| ------------------------------- | ------- |
| <span data-ttu-id="cd586-110">Microsoft Edge</span><span class="sxs-lookup"><span data-stu-id="cd586-110">Microsoft Edge</span></span>                  | <span data-ttu-id="cd586-111">bieżący</span><span class="sxs-lookup"><span data-stu-id="cd586-111">current</span></span> |
| <span data-ttu-id="cd586-112">Mozilla Firefox</span><span class="sxs-lookup"><span data-stu-id="cd586-112">Mozilla Firefox</span></span>                 | <span data-ttu-id="cd586-113">bieżący</span><span class="sxs-lookup"><span data-stu-id="cd586-113">current</span></span> |
| <span data-ttu-id="cd586-114">Google Chrome; obejmuje systemu Android</span><span class="sxs-lookup"><span data-stu-id="cd586-114">Google Chrome; includes Android</span></span> | <span data-ttu-id="cd586-115">bieżący</span><span class="sxs-lookup"><span data-stu-id="cd586-115">current</span></span> |
| <span data-ttu-id="cd586-116">Safari; includes iOS</span><span class="sxs-lookup"><span data-stu-id="cd586-116">Safari; includes iOS</span></span>            | <span data-ttu-id="cd586-117">bieżący</span><span class="sxs-lookup"><span data-stu-id="cd586-117">current</span></span> |
| <span data-ttu-id="cd586-118">Microsoft Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="cd586-118">Microsoft Internet Explorer</span></span>     | <span data-ttu-id="cd586-119">11</span><span class="sxs-lookup"><span data-stu-id="cd586-119">11</span></span>      |
 
## <a name="net-client"></a><span data-ttu-id="cd586-120">Klient modelu .NET</span><span class="sxs-lookup"><span data-stu-id="cd586-120">.NET client</span></span>

<span data-ttu-id="cd586-121">[Klient modelu .NET](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) działa na dowolnej platformie, obsługiwana przez platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="cd586-121">The [.NET client](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) runs on any platform supported by ASP.NET Core.</span></span> <span data-ttu-id="cd586-122">Na przykład [Xamarin deweloperzy mogą używać SignalR](https://github.com/aspnet/Announcements/issues/305) do tworzenia aplikacji dla systemu Android, korzystając z platformy Xamarin.Android 8.4.0.1 i później oraz aplikacje dla systemu iOS przy użyciu rozszerzenia Xamarin.iOS 11.14.0.4 i nowszych.</span><span class="sxs-lookup"><span data-stu-id="cd586-122">For example, [Xamarin developers can use SignalR](https://github.com/aspnet/Announcements/issues/305) for building Android apps using Xamarin.Android 8.4.0.1 and later and iOS apps using Xamarin.iOS 11.14.0.4 and later.</span></span>

<span data-ttu-id="cd586-123">Jeśli na serwerze działa program IIS, transport gniazda Websocket wymaga usług IIS 8.0 lub nowszego w systemie Windows Server 2012 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="cd586-123">If the server runs IIS, the WebSockets transport requires IIS 8.0 or higher on Windows Server 2012 or higher.</span></span> <span data-ttu-id="cd586-124">Inne transportów są obsługiwane na wszystkich platformach.</span><span class="sxs-lookup"><span data-stu-id="cd586-124">Other transports are supported on all platforms.</span></span>

## <a name="java-client"></a><span data-ttu-id="cd586-125">Klienta Java</span><span class="sxs-lookup"><span data-stu-id="cd586-125">Java client</span></span>

<span data-ttu-id="cd586-126">[Klienta Java](https://search.maven.org/artifact/com.microsoft.aspnet/signalr) obsługuje Java 8 i nowszych wersjach.</span><span class="sxs-lookup"><span data-stu-id="cd586-126">The [Java client](https://search.maven.org/artifact/com.microsoft.aspnet/signalr) supports Java 8 and later versions.</span></span>

## <a name="unsupported-clients"></a><span data-ttu-id="cd586-127">Nieobsługiwana klientów</span><span class="sxs-lookup"><span data-stu-id="cd586-127">Unsupported clients</span></span>

<span data-ttu-id="cd586-128">Następujący klienci są dostępne, ale są eksperymentalne lub nieoficjalny.</span><span class="sxs-lookup"><span data-stu-id="cd586-128">The following clients are available but are experimental or unofficial.</span></span> <span data-ttu-id="cd586-129">One nie są obecnie obsługiwane i nigdy nie może być.</span><span class="sxs-lookup"><span data-stu-id="cd586-129">They aren't currently supported and may never be.</span></span>

* [<span data-ttu-id="cd586-130">Klient języka C++</span><span class="sxs-lookup"><span data-stu-id="cd586-130">C++ client</span></span>](https://github.com/aspnet/SignalR/tree/master/clients/cpp)

* [<span data-ttu-id="cd586-131">Klient SWIFT</span><span class="sxs-lookup"><span data-stu-id="cd586-131">Swift client</span></span>](https://github.com/moozzyk/SignalR-Client-Swift)
