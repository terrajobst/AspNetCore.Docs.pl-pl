---
title: Hostowanie i wdrażanie składników Razor
author: guardrex
description: Dowiedz się, jak hostowanie i wdrażanie aplikacji składniki Razor przy użyciu platformy ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 03/28/2019
uid: host-and-deploy/razor-components-blazor/razor-components
ms.openlocfilehash: 18b09dfc80e2f517c7750ce0fe1510641624aea7
ms.sourcegitcommit: 6bde1fdf686326c080a7518a6725e56e56d8886e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/08/2019
ms.locfileid: "59069775"
---
# <a name="host-and-deploy-razor-components"></a><span data-ttu-id="62a69-103">Hostowanie i wdrażanie składników Razor</span><span class="sxs-lookup"><span data-stu-id="62a69-103">Host and deploy Razor Components</span></span>

<span data-ttu-id="62a69-104">Przez [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), i [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="62a69-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="62a69-105">Wartości konfiguracji hosta</span><span class="sxs-lookup"><span data-stu-id="62a69-105">Host configuration values</span></span>

<span data-ttu-id="62a69-106">Razor składniki aplikacje, które używają [po stronie serwera modelu hostingu](xref:razor-components/hosting-models#server-side-hosting-model) może akceptować [wartości konfiguracji hosta ogólny](xref:fundamentals/host/generic-host#host-configuration).</span><span class="sxs-lookup"><span data-stu-id="62a69-106">Razor Components apps that use the [server-side hosting model](xref:razor-components/hosting-models#server-side-hosting-model) can accept [Generic Host configuration values](xref:fundamentals/host/generic-host#host-configuration).</span></span>

## <a name="deployment"></a><span data-ttu-id="62a69-107">wdrażania</span><span class="sxs-lookup"><span data-stu-id="62a69-107">Deployment</span></span>

<span data-ttu-id="62a69-108">Za pomocą [po stronie serwera modelu hostingu](xref:razor-components/hosting-models#server-side-hosting-model), składniki Razor jest wykonywane na serwerze z poziomu aplikacji ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="62a69-108">With the [server-side hosting model](xref:razor-components/hosting-models#server-side-hosting-model), Razor Components is executed on the server from within an ASP.NET Core app.</span></span> <span data-ttu-id="62a69-109">Aktualizacje interfejsu użytkownika, obsługa zdarzeń i wywołania języka JavaScript są obsługiwane za pośrednictwem [SignalR](xref:signalr/introduction) połączenia.</span><span class="sxs-lookup"><span data-stu-id="62a69-109">UI updates, event handling, and JavaScript calls are handled over a [SignalR](xref:signalr/introduction) connection.</span></span>

<span data-ttu-id="62a69-110">Aplikacja jest dołączony do aplikacji ASP.NET Core w opublikowanych danych wyjściowych, a dwie aplikacje wdrażane razem.</span><span class="sxs-lookup"><span data-stu-id="62a69-110">The app is included with the ASP.NET Core app in the published output, and the two apps are deployed together.</span></span> <span data-ttu-id="62a69-111">Wymagany jest serwer sieci web, który jest zdolny do obsługi aplikacji ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="62a69-111">A web server that's capable of hosting an ASP.NET Core app is required.</span></span> <span data-ttu-id="62a69-112">Wdrożenia po stronie serwera, program Visual Studio obejmuje **składniki Razor** szablonu projektu (`razorcomponents` szablon, korzystając z [dotnet nowe](/dotnet/core/tools/dotnet-new) polecenia).</span><span class="sxs-lookup"><span data-stu-id="62a69-112">For a server-side deployment, Visual Studio includes the **Razor Components** project template (`razorcomponents` template when using the [dotnet new](/dotnet/core/tools/dotnet-new) command).</span></span>

<!--

**INSERT: Concerns are the same as publishing an ASP.NET Core SignalR app**

**INSERT: Content on the Azure SignalR Service**

**INSERT: Manually turn on WebSockets support**

-->

<span data-ttu-id="62a69-113">Aby uzyskać więcej informacji na temat hosting aplikacji platformy ASP.NET Core i wdrażanie, zobacz <xref:host-and-deploy/index>.</span><span class="sxs-lookup"><span data-stu-id="62a69-113">For more information on ASP.NET Core app hosting and deployment, see <xref:host-and-deploy/index>.</span></span>

<span data-ttu-id="62a69-114">Aby uzyskać informacje na temat wdrażania w usłudze Azure App Service, zobacz <xref:tutorials/publish-to-azure-webapp-using-vs>.</span><span class="sxs-lookup"><span data-stu-id="62a69-114">For information on deploying to Azure App Service, see <xref:tutorials/publish-to-azure-webapp-using-vs>.</span></span>
