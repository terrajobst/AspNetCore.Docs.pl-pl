---
title: Hosta w programie ASP.NET Core
author: guardrex
description: Informacje o hosta sieci Web programu ASP.NET Core i .NET ogólnego hosta, które są odpowiedzialni za zarządzanie uruchamiania i czasu życia aplikacji.
ms.author: riande
ms.custom: mvc
ms.date: 08/28/2018
uid: fundamentals/host/index
ms.openlocfilehash: 9927722b5080beb94e5628d9e7b54e6d50a5bff8
ms.sourcegitcommit: a669c4e3f42e387e214a354ac4143555602e6f66
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/31/2018
ms.locfileid: "43336053"
---
# <a name="host-in-aspnet-core"></a><span data-ttu-id="1c96b-103">Hosta w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1c96b-103">Host in ASP.NET Core</span></span>

<span data-ttu-id="1c96b-104">Konfigurowanie aplikacji platformy .NET i uruchamiania *hosta*.</span><span class="sxs-lookup"><span data-stu-id="1c96b-104">.NET apps configure and launch a *host*.</span></span> <span data-ttu-id="1c96b-105">Host jest odpowiedzialny za zarządzanie uruchamiania i czasu życia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1c96b-105">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="1c96b-106">Host dwa interfejsy API są dostępne do użycia:</span><span class="sxs-lookup"><span data-stu-id="1c96b-106">Two host APIs are available for use:</span></span>

* <span data-ttu-id="1c96b-107">[Sieci Web hosta](xref:fundamentals/host/web-host) &ndash; odpowiednia na potrzeby hostowania aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="1c96b-107">[Web Host](xref:fundamentals/host/web-host) &ndash; Suitable for hosting web apps.</span></span>
* <span data-ttu-id="1c96b-108">[Ogólny hosta](xref:fundamentals/host/generic-host) (platformy ASP.NET Core 2.1 lub nowszej) &ndash; odpowiednia na potrzeby hostowania aplikacji sieci web (na przykład aplikacje, które uruchamianie zadań w tle).</span><span class="sxs-lookup"><span data-stu-id="1c96b-108">[Generic Host](xref:fundamentals/host/generic-host) (ASP.NET Core 2.1 or later) &ndash; Suitable for hosting non-web apps (for example, apps that run background tasks).</span></span> <span data-ttu-id="1c96b-109">W przyszłej wersji ogólnych hosta będzie odpowiedni do hostowania dowolnych aplikacji, w tym aplikacje sieci web.</span><span class="sxs-lookup"><span data-stu-id="1c96b-109">In a future release, the Generic Host will be suitable for hosting any kind of app, including web apps.</span></span> <span data-ttu-id="1c96b-110">Ogólny hosta ostatecznie spowoduje zastąpienie hosta sieci Web.</span><span class="sxs-lookup"><span data-stu-id="1c96b-110">The Generic Host will eventually replace the Web Host.</span></span>

<span data-ttu-id="1c96b-111">Dla hostingu platformy ASP.NET Core *aplikacje sieci web*, programiści powinni używać hosta sieci Web, na podstawie <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>.</span><span class="sxs-lookup"><span data-stu-id="1c96b-111">For hosting ASP.NET Core *web apps*, developers should use the Web Host based on <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>.</span></span> <span data-ttu-id="1c96b-112">Do hostowania *-web apps*, deweloperzy należy używać ogólnych hosta, na podstawie <xref:Microsoft.Extensions.Hosting.HostBuilder>.</span><span class="sxs-lookup"><span data-stu-id="1c96b-112">For hosting *non-web apps*, developers should use the Generic Host based on <xref:Microsoft.Extensions.Hosting.HostBuilder>.</span></span>

<xref:fundamentals/host/hosted-services>  
<span data-ttu-id="1c96b-113">Dowiedz się, jak wdrożyć zadania w tle z usługami hostowanymi na platformie ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1c96b-113">Learn how to implement background tasks with hosted services in ASP.NET Core.</span></span>

<xref:fundamentals/configuration/platform-specific-configuration>  
<span data-ttu-id="1c96b-114">Odkryj, jak i zwiększanie możliwości aplikacji platformy ASP.NET Core z odwołania lub nieużywany zestawu przy użyciu <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementacji.</span><span class="sxs-lookup"><span data-stu-id="1c96b-114">Discover how to enhance an ASP.NET Core app from a referenced or unreferenced assembly using an <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation.</span></span>
