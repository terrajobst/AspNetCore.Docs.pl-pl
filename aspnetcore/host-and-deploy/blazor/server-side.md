---
title: Hostowanie i wdrażanie Blazor po stronie serwera
author: guardrex
description: Dowiedz się, jak hostowanie i wdrażanie aplikacji po stronie serwera Blazor, przy użyciu platformy ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/26/2019
uid: host-and-deploy/blazor/server-side
ms.openlocfilehash: 8e44be09a4cceba2509f3e86abf3ce5fd2d077bd
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/27/2019
ms.locfileid: "64901048"
---
# <a name="host-and-deploy-blazor-server-side"></a><span data-ttu-id="1d5e5-103">Hostowanie i wdrażanie Blazor po stronie serwera</span><span class="sxs-lookup"><span data-stu-id="1d5e5-103">Host and deploy Blazor server-side</span></span>

<span data-ttu-id="1d5e5-104">Przez [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), i [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="1d5e5-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="1d5e5-105">Wartości konfiguracji hosta</span><span class="sxs-lookup"><span data-stu-id="1d5e5-105">Host configuration values</span></span>

<span data-ttu-id="1d5e5-106">Aplikacje serwerowe, które używają [po stronie serwera modelu hostingu](xref:blazor/hosting-models#server-side) może akceptować [wartości konfiguracji hosta ogólny](xref:fundamentals/host/generic-host#host-configuration).</span><span class="sxs-lookup"><span data-stu-id="1d5e5-106">Server-side apps that use the [server-side hosting model](xref:blazor/hosting-models#server-side) can accept [Generic Host configuration values](xref:fundamentals/host/generic-host#host-configuration).</span></span>

## <a name="deployment"></a><span data-ttu-id="1d5e5-107">wdrażania</span><span class="sxs-lookup"><span data-stu-id="1d5e5-107">Deployment</span></span>

<span data-ttu-id="1d5e5-108">Za pomocą [po stronie serwera modelu hostingu](xref:blazor/hosting-models#server-side), Blazor jest wykonywana na serwerze z poziomu aplikacji ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1d5e5-108">With the [server-side hosting model](xref:blazor/hosting-models#server-side), Blazor is executed on the server from within an ASP.NET Core app.</span></span> <span data-ttu-id="1d5e5-109">Aktualizacje interfejsu użytkownika, obsługa zdarzeń i wywołania języka JavaScript są obsługiwane za pośrednictwem [SignalR](xref:signalr/introduction) połączenia.</span><span class="sxs-lookup"><span data-stu-id="1d5e5-109">UI updates, event handling, and JavaScript calls are handled over a [SignalR](xref:signalr/introduction) connection.</span></span>

<span data-ttu-id="1d5e5-110">Wymagany jest serwer sieci web, zdolne do obsługi aplikacji ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1d5e5-110">A web server capable of hosting an ASP.NET Core app is required.</span></span> <span data-ttu-id="1d5e5-111">Program Visual Studio obejmuje **Blazor (po stronie serwera)** szablonu projektu (`blazorserverside` szablon, korzystając z [dotnet nowe](/dotnet/core/tools/dotnet-new) polecenie).</span><span class="sxs-lookup"><span data-stu-id="1d5e5-111">Visual Studio includes the **Blazor (server-side)** project template (`blazorserverside` template when using the [dotnet new](/dotnet/core/tools/dotnet-new) command).</span></span>

<!--

**INSERT: Concerns are the same as publishing an ASP.NET Core SignalR app**

**INSERT: Content on the Azure SignalR Service**

**INSERT: Manually turn on WebSockets support**

-->

## <a name="additional-resources"></a><span data-ttu-id="1d5e5-112">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="1d5e5-112">Additional resources</span></span>

* <xref:signalr/introduction>
* <xref:host-and-deploy/index>
* <xref:tutorials/publish-to-azure-webapp-using-vs>
* [<span data-ttu-id="1d5e5-113">Wdrażanie platformy ASP.NET Core w wersji zapoznawczej w usłudze Azure App Service</span><span class="sxs-lookup"><span data-stu-id="1d5e5-113">Deploy ASP.NET Core preview release to Azure App Service</span></span>](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service)
