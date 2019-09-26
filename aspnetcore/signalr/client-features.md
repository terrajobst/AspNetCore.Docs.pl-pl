---
title: Funkcje klienta SignalR
author: bradygaster
description: Dowiedz się, które funkcje są obsługiwane przez różnych klientów ASP.NET Core sygnalizujących.
monikerRange: '>= aspnetcore-3.0'
ms.author: bradyg
ms.custom: mvc
ms.date: 09/18/2019
uid: signalr/client-features
ms.openlocfilehash: 2d6759a5484c37aee6db3d22b3127414231605ae
ms.sourcegitcommit: 14b25156e34c82ed0495b4aff5776ac5b1950b5e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/26/2019
ms.locfileid: "71301190"
---
# <a name="aspnet-core-signalr-client-features"></a><span data-ttu-id="4b04b-103">Funkcje klienta sygnalizującego ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4b04b-103">ASP.NET Core SignalR client features</span></span>

## <a name="feature-distribution"></a><span data-ttu-id="4b04b-104">Dystrybucja funkcji</span><span class="sxs-lookup"><span data-stu-id="4b04b-104">Feature distribution</span></span>

<span data-ttu-id="4b04b-105">W poniższej tabeli przedstawiono funkcje i obsługę klientów, którzy oferują pomoc techniczną w czasie rzeczywistym.</span><span class="sxs-lookup"><span data-stu-id="4b04b-105">The table below shows the features and support for the clients that offer real-time support.</span></span>

| <span data-ttu-id="4b04b-106">Funkcja</span><span class="sxs-lookup"><span data-stu-id="4b04b-106">Feature</span></span> | <span data-ttu-id="4b04b-107">.NET</span><span class="sxs-lookup"><span data-stu-id="4b04b-107">.NET</span></span> | <span data-ttu-id="4b04b-108">JavaScript</span><span class="sxs-lookup"><span data-stu-id="4b04b-108">JavaScript</span></span> | <span data-ttu-id="4b04b-109">Java</span><span class="sxs-lookup"><span data-stu-id="4b04b-109">Java</span></span> |
| ---- | :-: | :-: | :-: |
| <span data-ttu-id="4b04b-110">Obsługa usługi sygnałów platformy Azure</span><span class="sxs-lookup"><span data-stu-id="4b04b-110">Azure SignalR Service Support</span></span> |<span data-ttu-id="4b04b-111">✔</span><span class="sxs-lookup"><span data-stu-id="4b04b-111">✔</span></span>|<span data-ttu-id="4b04b-112">✔</span><span class="sxs-lookup"><span data-stu-id="4b04b-112">✔</span></span>|<span data-ttu-id="4b04b-113">✔</span><span class="sxs-lookup"><span data-stu-id="4b04b-113">✔</span></span>|
| [<span data-ttu-id="4b04b-114">Przesyłanie strumieniowe między serwerami i klientami</span><span class="sxs-lookup"><span data-stu-id="4b04b-114">Server-to-client Streaming</span></span>](xref:signalr/streaming)          |<span data-ttu-id="4b04b-115">✔</span><span class="sxs-lookup"><span data-stu-id="4b04b-115">✔</span></span>|<span data-ttu-id="4b04b-116">✔</span><span class="sxs-lookup"><span data-stu-id="4b04b-116">✔</span></span>|<span data-ttu-id="4b04b-117">✔</span><span class="sxs-lookup"><span data-stu-id="4b04b-117">✔</span></span>|
| [<span data-ttu-id="4b04b-118">Przesyłanie strumieniowe klient-serwer</span><span class="sxs-lookup"><span data-stu-id="4b04b-118">Client-to-server Streaming</span></span>](xref:signalr/streaming)          |<span data-ttu-id="4b04b-119">✔</span><span class="sxs-lookup"><span data-stu-id="4b04b-119">✔</span></span>|<span data-ttu-id="4b04b-120">✔</span><span class="sxs-lookup"><span data-stu-id="4b04b-120">✔</span></span>|<span data-ttu-id="4b04b-121">✔</span><span class="sxs-lookup"><span data-stu-id="4b04b-121">✔</span></span>|
| <span data-ttu-id="4b04b-122">Automatyczne ponowne łączenie ([.NET](/aspnet/core/signalr/dotnet-client?view=aspnetcore-3.0&tabs=visual-studio#handle-lost-connection), [JavaScript](/aspnet/core/signalr/javascript-client?view=aspnetcore-3.0#reconnect-clients))</span><span class="sxs-lookup"><span data-stu-id="4b04b-122">Automatic Reconnection ([.NET](/aspnet/core/signalr/dotnet-client?view=aspnetcore-3.0&tabs=visual-studio#handle-lost-connection), [JavaScript](/aspnet/core/signalr/javascript-client?view=aspnetcore-3.0#reconnect-clients))</span></span>          |<span data-ttu-id="4b04b-123">✔</span><span class="sxs-lookup"><span data-stu-id="4b04b-123">✔</span></span>|<span data-ttu-id="4b04b-124">✔</span><span class="sxs-lookup"><span data-stu-id="4b04b-124">✔</span></span>| |
| <span data-ttu-id="4b04b-125">Transport gniazd WebSockets</span><span class="sxs-lookup"><span data-stu-id="4b04b-125">WebSockets Transport</span></span> |<span data-ttu-id="4b04b-126">✔</span><span class="sxs-lookup"><span data-stu-id="4b04b-126">✔</span></span>|<span data-ttu-id="4b04b-127">✔</span><span class="sxs-lookup"><span data-stu-id="4b04b-127">✔</span></span>|<span data-ttu-id="4b04b-128">✔</span><span class="sxs-lookup"><span data-stu-id="4b04b-128">✔</span></span>|
| <span data-ttu-id="4b04b-129">Transport zdarzeń wysłanych przez serwer</span><span class="sxs-lookup"><span data-stu-id="4b04b-129">Server-Sent Events Transport</span></span> |<span data-ttu-id="4b04b-130">✔</span><span class="sxs-lookup"><span data-stu-id="4b04b-130">✔</span></span>|<span data-ttu-id="4b04b-131">✔</span><span class="sxs-lookup"><span data-stu-id="4b04b-131">✔</span></span>| |
| <span data-ttu-id="4b04b-132">Długotrwały transport sondowania</span><span class="sxs-lookup"><span data-stu-id="4b04b-132">Long Polling Transport</span></span> |<span data-ttu-id="4b04b-133">✔</span><span class="sxs-lookup"><span data-stu-id="4b04b-133">✔</span></span>|<span data-ttu-id="4b04b-134">✔</span><span class="sxs-lookup"><span data-stu-id="4b04b-134">✔</span></span>|<span data-ttu-id="4b04b-135">✔</span><span class="sxs-lookup"><span data-stu-id="4b04b-135">✔</span></span>|
| <span data-ttu-id="4b04b-136">Protokół centrum JSON</span><span class="sxs-lookup"><span data-stu-id="4b04b-136">JSON Hub Protocol</span></span> |<span data-ttu-id="4b04b-137">✔</span><span class="sxs-lookup"><span data-stu-id="4b04b-137">✔</span></span>|<span data-ttu-id="4b04b-138">✔</span><span class="sxs-lookup"><span data-stu-id="4b04b-138">✔</span></span>|<span data-ttu-id="4b04b-139">✔</span><span class="sxs-lookup"><span data-stu-id="4b04b-139">✔</span></span>|
| <span data-ttu-id="4b04b-140">Protokół centrum MessagePack</span><span class="sxs-lookup"><span data-stu-id="4b04b-140">MessagePack Hub Protocol</span></span> |<span data-ttu-id="4b04b-141">✔</span><span class="sxs-lookup"><span data-stu-id="4b04b-141">✔</span></span>|<span data-ttu-id="4b04b-142">✔</span><span class="sxs-lookup"><span data-stu-id="4b04b-142">✔</span></span>| |

<span data-ttu-id="4b04b-143">Obsługa automatycznego ponownego łączenia w kliencie Java jest śledzona w [naszym monitorze problemów](https://github.com/aspnet/AspNetCore/issues/8711).</span><span class="sxs-lookup"><span data-stu-id="4b04b-143">Support for automatic reconnect in the Java client is tracked in [our issue tracker](https://github.com/aspnet/AspNetCore/issues/8711).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4b04b-144">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="4b04b-144">Additional resources</span></span>

* [<span data-ttu-id="4b04b-145">Wprowadzenie do programu sygnalizującego dla ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4b04b-145">Get started with SignalR for ASP.NET Core</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="4b04b-146">Obsługiwane platformy</span><span class="sxs-lookup"><span data-stu-id="4b04b-146">Supported platforms</span></span>](xref:signalr/supported-platforms)
* [<span data-ttu-id="4b04b-147">Centra</span><span class="sxs-lookup"><span data-stu-id="4b04b-147">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="4b04b-148">Klient JavaScript</span><span class="sxs-lookup"><span data-stu-id="4b04b-148">JavaScript client</span></span>](xref:signalr/javascript-client)
