---
title: Host sieci Web i ogólny hosta w programie ASP.NET Core
author: guardrex
description: Informacje o hosta sieci Web programu ASP.NET Core i .NET ogólnego hosta, które są odpowiedzialni za zarządzanie uruchamiania i czasu życia aplikacji.
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 08/28/2018
uid: fundamentals/host/index
ms.openlocfilehash: 43a68f67368ce37a1fb4032579c6c4e4c05d2719
ms.sourcegitcommit: b34b25da2ab68e6495b2460ff570468f16a9bf0d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53284581"
---
# <a name="web-host-and-generic-host-in-aspnet-core"></a><span data-ttu-id="db7f2-103">Host sieci Web i ogólny hosta w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="db7f2-103">Web Host and Generic Host in ASP.NET Core</span></span>

<span data-ttu-id="db7f2-104">Konfigurowanie aplikacji platformy .NET i uruchamiania *hosta*.</span><span class="sxs-lookup"><span data-stu-id="db7f2-104">.NET apps configure and launch a *host*.</span></span> <span data-ttu-id="db7f2-105">Host jest odpowiedzialny za zarządzanie uruchamiania i czasu życia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="db7f2-105">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="db7f2-106">Host dwa interfejsy API są dostępne do użycia:</span><span class="sxs-lookup"><span data-stu-id="db7f2-106">Two host APIs are available for use:</span></span>

* <span data-ttu-id="db7f2-107">[Sieci Web hosta](xref:fundamentals/host/web-host) &ndash; odpowiednia na potrzeby hostowania aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="db7f2-107">[Web Host](xref:fundamentals/host/web-host) &ndash; Suitable for hosting web apps.</span></span>
* <span data-ttu-id="db7f2-108">[Ogólny hosta](xref:fundamentals/host/generic-host) (platformy ASP.NET Core 2.1 lub nowszej) &ndash; odpowiednia na potrzeby hostowania aplikacji sieci web (na przykład aplikacje, które uruchamianie zadań w tle).</span><span class="sxs-lookup"><span data-stu-id="db7f2-108">[Generic Host](xref:fundamentals/host/generic-host) (ASP.NET Core 2.1 or later) &ndash; Suitable for hosting non-web apps (for example, apps that run background tasks).</span></span> <span data-ttu-id="db7f2-109">W przyszłej wersji ogólnych hosta będzie odpowiedni do hostowania dowolnych aplikacji, w tym aplikacje sieci web.</span><span class="sxs-lookup"><span data-stu-id="db7f2-109">In a future release, the Generic Host will be suitable for hosting any kind of app, including web apps.</span></span> <span data-ttu-id="db7f2-110">Ogólny hosta ostatecznie spowoduje zastąpienie hosta sieci Web.</span><span class="sxs-lookup"><span data-stu-id="db7f2-110">The Generic Host will eventually replace the Web Host.</span></span>

<span data-ttu-id="db7f2-111">Dla hostingu platformy ASP.NET Core *aplikacje sieci web*, programiści powinni używać hosta sieci Web, na podstawie <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>.</span><span class="sxs-lookup"><span data-stu-id="db7f2-111">For hosting ASP.NET Core *web apps*, developers should use the Web Host based on <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>.</span></span> <span data-ttu-id="db7f2-112">Do hostowania *-web apps*, deweloperzy należy używać ogólnych hosta, na podstawie <xref:Microsoft.Extensions.Hosting.HostBuilder>.</span><span class="sxs-lookup"><span data-stu-id="db7f2-112">For hosting *non-web apps*, developers should use the Generic Host based on <xref:Microsoft.Extensions.Hosting.HostBuilder>.</span></span>

<xref:fundamentals/host/hosted-services>  
<span data-ttu-id="db7f2-113">Dowiedz się, jak wdrożyć zadania w tle z usługami hostowanymi na platformie ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="db7f2-113">Learn how to implement background tasks with hosted services in ASP.NET Core.</span></span>

<xref:fundamentals/configuration/platform-specific-configuration>  
<span data-ttu-id="db7f2-114">Odkryj, jak i zwiększanie możliwości aplikacji platformy ASP.NET Core z odwołania lub nieużywany zestawu przy użyciu <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementacji.</span><span class="sxs-lookup"><span data-stu-id="db7f2-114">Discover how to enhance an ASP.NET Core app from a referenced or unreferenced assembly using an <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation.</span></span>
