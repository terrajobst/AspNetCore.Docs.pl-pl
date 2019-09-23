---
title: Funkcje klienta sygnalizującego
author: bradygaster
description: Dowiedz się, które funkcje są obsługiwane przez różnych klientów ASP.NET Core sygnalizujących.
monikerRange: '>= aspnetcore-3.0'
ms.author: bradyg
ms.custom: mvc
ms.date: 09/18/2019
uid: signalr/client-features
ms.openlocfilehash: 55086673e0c9f9b73f07730ea25c3fa322f7fd98
ms.sourcegitcommit: d34b2627a69bc8940b76a949de830335db9701d3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/23/2019
ms.locfileid: "71187468"
---
# <a name="aspnet-core-signalr-client-features"></a><span data-ttu-id="abfde-103">Funkcje klienta sygnalizującego ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="abfde-103">ASP.NET Core SignalR client features</span></span>

## <a name="feature-distribution"></a><span data-ttu-id="abfde-104">Dystrybucja funkcji</span><span class="sxs-lookup"><span data-stu-id="abfde-104">Feature distribution</span></span>

<span data-ttu-id="abfde-105">W poniższej tabeli przedstawiono funkcje i obsługę klientów, którzy oferują pomoc techniczną w czasie rzeczywistym.</span><span class="sxs-lookup"><span data-stu-id="abfde-105">The table below shows the features and support for the clients that offer real-time support.</span></span>

| <span data-ttu-id="abfde-106">Funkcja</span><span class="sxs-lookup"><span data-stu-id="abfde-106">Feature</span></span> | <span data-ttu-id="abfde-107">.NET Core</span><span class="sxs-lookup"><span data-stu-id="abfde-107">.NET Core</span></span> | <span data-ttu-id="abfde-108">JavaScript</span><span class="sxs-lookup"><span data-stu-id="abfde-108">JavaScript</span></span> | <span data-ttu-id="abfde-109">Java</span><span class="sxs-lookup"><span data-stu-id="abfde-109">Java</span></span> |
| ---- | :-: | :-: | :-: |
| [<span data-ttu-id="abfde-110">Przesyłanie strumieniowe między serwerami i klientami</span><span class="sxs-lookup"><span data-stu-id="abfde-110">Server-to-client Streaming</span></span>](xref:signalr/streaming)          |<span data-ttu-id="abfde-111">✔</span><span class="sxs-lookup"><span data-stu-id="abfde-111">✔</span></span>|<span data-ttu-id="abfde-112">✔</span><span class="sxs-lookup"><span data-stu-id="abfde-112">✔</span></span>|<span data-ttu-id="abfde-113">✔</span><span class="sxs-lookup"><span data-stu-id="abfde-113">✔</span></span>|
| [<span data-ttu-id="abfde-114">Przesyłanie strumieniowe klient-serwer</span><span class="sxs-lookup"><span data-stu-id="abfde-114">Client-to-server Streaming</span></span>](xref:signalr/streaming)          |<span data-ttu-id="abfde-115">✔</span><span class="sxs-lookup"><span data-stu-id="abfde-115">✔</span></span>|<span data-ttu-id="abfde-116">✔</span><span class="sxs-lookup"><span data-stu-id="abfde-116">✔</span></span>|<span data-ttu-id="abfde-117">✔</span><span class="sxs-lookup"><span data-stu-id="abfde-117">✔</span></span>|
| <span data-ttu-id="abfde-118">Automatyczne ponowne łączenie ([.NET](/aspnet/core/signalr/dotnet-client?view=aspnetcore-3.0&tabs=visual-studio#handle-lost-connection), [JavaScript](/aspnet/core/signalr/javascript-client?view=aspnetcore-3.0#reconnect-clients))</span><span class="sxs-lookup"><span data-stu-id="abfde-118">Automatic Reconnection ([.NET](/aspnet/core/signalr/dotnet-client?view=aspnetcore-3.0&tabs=visual-studio#handle-lost-connection), [JavaScript](/aspnet/core/signalr/javascript-client?view=aspnetcore-3.0#reconnect-clients))</span></span>          |<span data-ttu-id="abfde-119">✔</span><span class="sxs-lookup"><span data-stu-id="abfde-119">✔</span></span>|<span data-ttu-id="abfde-120">✔</span><span class="sxs-lookup"><span data-stu-id="abfde-120">✔</span></span>| |

## <a name="additional-resources"></a><span data-ttu-id="abfde-121">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="abfde-121">Additional resources</span></span>

* [<span data-ttu-id="abfde-122">Wprowadzenie do programu sygnalizującego dla ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="abfde-122">Get started with SignalR for ASP.NET Core</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="abfde-123">Obsługiwane platformy</span><span class="sxs-lookup"><span data-stu-id="abfde-123">Supported platforms</span></span>](xref:signalr/supported-platforms)
* [<span data-ttu-id="abfde-124">Centra</span><span class="sxs-lookup"><span data-stu-id="abfde-124">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="abfde-125">Klient JavaScript</span><span class="sxs-lookup"><span data-stu-id="abfde-125">JavaScript client</span></span>](xref:signalr/javascript-client)
