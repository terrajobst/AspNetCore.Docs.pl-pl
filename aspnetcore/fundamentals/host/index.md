---
title: Host sieci Web i ogólny hosta w programie ASP.NET Core
author: guardrex
description: Informacje o hosta sieci Web programu ASP.NET Core i .NET ogólnego hosta, które są odpowiedzialni za zarządzanie uruchamiania i czasu życia aplikacji.
ms.author: riande
ms.custom: mvc,seodec18
ms.date: 08/28/2018
uid: fundamentals/host/index
ms.openlocfilehash: 3e67d8338aa7ac1b1530d0498ee0126d36a8d72b
ms.sourcegitcommit: 49faca2644590fc081d86db46ea5e29edfc28b7b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/09/2018
ms.locfileid: "53121521"
---
# <a name="web-host-and-generic-host-in-aspnet-core"></a><span data-ttu-id="660bd-103">Host sieci Web i ogólny hosta w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="660bd-103">Web Host and Generic Host in ASP.NET Core</span></span>

<span data-ttu-id="660bd-104">Konfigurowanie aplikacji platformy .NET i uruchamiania *hosta*.</span><span class="sxs-lookup"><span data-stu-id="660bd-104">.NET apps configure and launch a *host*.</span></span> <span data-ttu-id="660bd-105">Host jest odpowiedzialny za zarządzanie uruchamiania i czasu życia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="660bd-105">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="660bd-106">Host dwa interfejsy API są dostępne do użycia:</span><span class="sxs-lookup"><span data-stu-id="660bd-106">Two host APIs are available for use:</span></span>

* <span data-ttu-id="660bd-107">[Sieci Web hosta](xref:fundamentals/host/web-host) &ndash; odpowiednia na potrzeby hostowania aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="660bd-107">[Web Host](xref:fundamentals/host/web-host) &ndash; Suitable for hosting web apps.</span></span>
* <span data-ttu-id="660bd-108">[Ogólny hosta](xref:fundamentals/host/generic-host) (platformy ASP.NET Core 2.1 lub nowszej) &ndash; odpowiednia na potrzeby hostowania aplikacji sieci web (na przykład aplikacje, które uruchamianie zadań w tle).</span><span class="sxs-lookup"><span data-stu-id="660bd-108">[Generic Host](xref:fundamentals/host/generic-host) (ASP.NET Core 2.1 or later) &ndash; Suitable for hosting non-web apps (for example, apps that run background tasks).</span></span> <span data-ttu-id="660bd-109">W przyszłej wersji ogólnych hosta będzie odpowiedni do hostowania dowolnych aplikacji, w tym aplikacje sieci web.</span><span class="sxs-lookup"><span data-stu-id="660bd-109">In a future release, the Generic Host will be suitable for hosting any kind of app, including web apps.</span></span> <span data-ttu-id="660bd-110">Ogólny hosta ostatecznie spowoduje zastąpienie hosta sieci Web.</span><span class="sxs-lookup"><span data-stu-id="660bd-110">The Generic Host will eventually replace the Web Host.</span></span>

<span data-ttu-id="660bd-111">Dla hostingu platformy ASP.NET Core *aplikacje sieci web*, programiści powinni używać hosta sieci Web, na podstawie <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>.</span><span class="sxs-lookup"><span data-stu-id="660bd-111">For hosting ASP.NET Core *web apps*, developers should use the Web Host based on <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>.</span></span> <span data-ttu-id="660bd-112">Do hostowania *-web apps*, deweloperzy należy używać ogólnych hosta, na podstawie <xref:Microsoft.Extensions.Hosting.HostBuilder>.</span><span class="sxs-lookup"><span data-stu-id="660bd-112">For hosting *non-web apps*, developers should use the Generic Host based on <xref:Microsoft.Extensions.Hosting.HostBuilder>.</span></span>

<xref:fundamentals/host/hosted-services>  
<span data-ttu-id="660bd-113">Dowiedz się, jak wdrożyć zadania w tle z usługami hostowanymi na platformie ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="660bd-113">Learn how to implement background tasks with hosted services in ASP.NET Core.</span></span>

<xref:fundamentals/configuration/platform-specific-configuration>  
<span data-ttu-id="660bd-114">Odkryj, jak i zwiększanie możliwości aplikacji platformy ASP.NET Core z odwołania lub nieużywany zestawu przy użyciu <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementacji.</span><span class="sxs-lookup"><span data-stu-id="660bd-114">Discover how to enhance an ASP.NET Core app from a referenced or unreferenced assembly using an <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation.</span></span>
