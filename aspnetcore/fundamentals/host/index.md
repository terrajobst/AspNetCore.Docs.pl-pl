---
title: Host platformy ASP.NET Core
author: guardrex
description: Informacje o hosta sieci Web platformy ASP.NET Core i .NET rodzajowego hosta, które są odpowiedzialni za zarządzanie uruchamiania i okresem istnienia aplikacji.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/16/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/host/index
ms.openlocfilehash: 7ad059e39866f59040c12b7ac15e9fa3405a9aad
ms.sourcegitcommit: a66f38071e13685bbe59d48d22aa141ac702b432
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/17/2018
---
# <a name="host-in-aspnet-core"></a><span data-ttu-id="c9579-103">Host platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c9579-103">Host in ASP.NET Core</span></span>

<span data-ttu-id="c9579-104">Aplikacje .NET skonfigurować i uruchomić *hosta*.</span><span class="sxs-lookup"><span data-stu-id="c9579-104">.NET apps configure and launch a *host*.</span></span> <span data-ttu-id="c9579-105">Host jest odpowiedzialny za zarządzanie uruchamiania i okresem istnienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c9579-105">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="c9579-106">Host dwa interfejsy API są dostępne do użycia:</span><span class="sxs-lookup"><span data-stu-id="c9579-106">Two host APIs are available for use:</span></span>

* <span data-ttu-id="c9579-107">[Sieci Web hosta](xref:fundamentals/host/web-host) &ndash; odpowiednia na potrzeby hostowania aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="c9579-107">[Web Host](xref:fundamentals/host/web-host) &ndash; Suitable for hosting web apps.</span></span>
* <span data-ttu-id="c9579-108">[Ogólny hosta](xref:fundamentals/host/generic-host) (platformy ASP.NET Core 2.1 lub nowszej) &ndash; odpowiednia na potrzeby hostowania aplikacji sieci web (na przykład aplikacji uruchamianych zadania w tle).</span><span class="sxs-lookup"><span data-stu-id="c9579-108">[Generic Host](xref:fundamentals/host/generic-host) (ASP.NET Core 2.1 or later) &ndash; Suitable for hosting non-web apps (for example, apps that run background tasks).</span></span> <span data-ttu-id="c9579-109">W przyszłym wydaniu rodzajowego hosta będzie odpowiednie do obsługi dowolnego rodzaju aplikacji, w tym aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="c9579-109">In a future release, the Generic Host will be suitable for hosting any kind of app, including web apps.</span></span> <span data-ttu-id="c9579-110">Ogólny hosta ostatecznie spowoduje zastąpienie hosta sieci Web.</span><span class="sxs-lookup"><span data-stu-id="c9579-110">The Generic Host will eventually replace the Web Host.</span></span>

<span data-ttu-id="c9579-111">W tej chwili Programiści powinni używać [hosta sieci Web](xref:fundamentals/host/web-host) na podstawie [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) do hostowania aplikacji platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c9579-111">At this time, developers should use the [Web Host](xref:fundamentals/host/web-host) based on [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) for hosting ASP.NET Core apps.</span></span>
