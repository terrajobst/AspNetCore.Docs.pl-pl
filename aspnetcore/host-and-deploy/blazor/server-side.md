---
title: Hostowanie i wdrażanie Blazor po stronie serwera
author: guardrex
description: Dowiedz się, jak hostowanie i wdrażanie aplikacji po stronie serwera Blazor, przy użyciu platformy ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/15/2019
uid: host-and-deploy/blazor/server-side
ms.openlocfilehash: 940020ee44d72d50395aad64bc924413c1bbecfb
ms.sourcegitcommit: 017b673b3c700d2976b77201d0ac30172e2abc87
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/16/2019
ms.locfileid: "59614879"
---
# <a name="host-and-deploy-blazor-server-side"></a><span data-ttu-id="82ece-103">Hostowanie i wdrażanie Blazor po stronie serwera</span><span class="sxs-lookup"><span data-stu-id="82ece-103">Host and deploy Blazor server-side</span></span>

<span data-ttu-id="82ece-104">Przez [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), i [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="82ece-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="82ece-105">Wartości konfiguracji hosta</span><span class="sxs-lookup"><span data-stu-id="82ece-105">Host configuration values</span></span>

<span data-ttu-id="82ece-106">Aplikacje serwerowe, które używają [po stronie serwera modelu hostingu](xref:blazor/hosting-models#server-side-hosting-model) może akceptować [wartości konfiguracji hosta ogólny](xref:fundamentals/host/generic-host#host-configuration).</span><span class="sxs-lookup"><span data-stu-id="82ece-106">Server-side apps that use the [server-side hosting model](xref:blazor/hosting-models#server-side-hosting-model) can accept [Generic Host configuration values](xref:fundamentals/host/generic-host#host-configuration).</span></span>

## <a name="deployment"></a><span data-ttu-id="82ece-107">wdrażania</span><span class="sxs-lookup"><span data-stu-id="82ece-107">Deployment</span></span>

<span data-ttu-id="82ece-108">Za pomocą [po stronie serwera modelu hostingu](xref:blazor/hosting-models#server-side-hosting-model), Blazor jest wykonywana na serwerze z poziomu aplikacji ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="82ece-108">With the [server-side hosting model](xref:blazor/hosting-models#server-side-hosting-model), Blazor is executed on the server from within an ASP.NET Core app.</span></span> <span data-ttu-id="82ece-109">Aktualizacje interfejsu użytkownika, obsługa zdarzeń i wywołania języka JavaScript są obsługiwane za pośrednictwem [SignalR](xref:signalr/introduction) połączenia.</span><span class="sxs-lookup"><span data-stu-id="82ece-109">UI updates, event handling, and JavaScript calls are handled over a [SignalR](xref:signalr/introduction) connection.</span></span>

<span data-ttu-id="82ece-110">Aplikacja jest dołączony do aplikacji ASP.NET Core w opublikowanych danych wyjściowych, a dwie aplikacje wdrażane razem.</span><span class="sxs-lookup"><span data-stu-id="82ece-110">The app is included with the ASP.NET Core app in the published output, and the two apps are deployed together.</span></span> <span data-ttu-id="82ece-111">Wymagany jest serwer sieci web, który jest zdolny do obsługi aplikacji ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="82ece-111">A web server that's capable of hosting an ASP.NET Core app is required.</span></span> <span data-ttu-id="82ece-112">Wdrożenia po stronie serwera, program Visual Studio obejmuje **składniki Razor** szablonu projektu (`razorcomponents` szablon, korzystając z [dotnet nowe](/dotnet/core/tools/dotnet-new) polecenia).</span><span class="sxs-lookup"><span data-stu-id="82ece-112">For a server-side deployment, Visual Studio includes the **Razor Components** project template (`razorcomponents` template when using the [dotnet new](/dotnet/core/tools/dotnet-new) command).</span></span>

<!--

**INSERT: Concerns are the same as publishing an ASP.NET Core SignalR app**

**INSERT: Content on the Azure SignalR Service**

**INSERT: Manually turn on WebSockets support**

-->

<span data-ttu-id="82ece-113">Aby uzyskać więcej informacji na temat hosting aplikacji platformy ASP.NET Core i wdrażanie, zobacz <xref:host-and-deploy/index>.</span><span class="sxs-lookup"><span data-stu-id="82ece-113">For more information on ASP.NET Core app hosting and deployment, see <xref:host-and-deploy/index>.</span></span>

<span data-ttu-id="82ece-114">Aby uzyskać informacje na temat wdrażania w usłudze Azure App Service, zobacz <xref:tutorials/publish-to-azure-webapp-using-vs>.</span><span class="sxs-lookup"><span data-stu-id="82ece-114">For information on deploying to Azure App Service, see <xref:tutorials/publish-to-azure-webapp-using-vs>.</span></span>
