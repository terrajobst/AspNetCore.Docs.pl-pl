---
title: Hostowanie i wdrażanie platformy ASP.NET Core Blazor po stronie serwera
author: guardrex
description: Dowiedz się, jak hostowanie i wdrażanie aplikacji po stronie serwera Blazor, przy użyciu platformy ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 06/11/2019
uid: host-and-deploy/blazor/server-side
ms.openlocfilehash: 56a03ff583bf85497e2b3bacc70123845a046e3d
ms.sourcegitcommit: 040aedca220ed24ee1726e6886daf6906f95a028
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/15/2019
ms.locfileid: "67892695"
---
# <a name="host-and-deploy-blazor-server-side"></a><span data-ttu-id="ae5e0-103">Hostowanie i wdrażanie Blazor po stronie serwera</span><span class="sxs-lookup"><span data-stu-id="ae5e0-103">Host and deploy Blazor server-side</span></span>

<span data-ttu-id="ae5e0-104">Przez [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), i [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="ae5e0-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="ae5e0-105">Wartości konfiguracji hosta</span><span class="sxs-lookup"><span data-stu-id="ae5e0-105">Host configuration values</span></span>

<span data-ttu-id="ae5e0-106">Aplikacje serwerowe, które używają [po stronie serwera modelu hostingu](xref:blazor/hosting-models#server-side) może akceptować [wartości konfiguracji hosta ogólny](xref:fundamentals/host/generic-host#host-configuration).</span><span class="sxs-lookup"><span data-stu-id="ae5e0-106">Server-side apps that use the [server-side hosting model](xref:blazor/hosting-models#server-side) can accept [Generic Host configuration values](xref:fundamentals/host/generic-host#host-configuration).</span></span>

## <a name="deployment"></a><span data-ttu-id="ae5e0-107">wdrażania</span><span class="sxs-lookup"><span data-stu-id="ae5e0-107">Deployment</span></span>

<span data-ttu-id="ae5e0-108">Za pomocą [po stronie serwera modelu hostingu](xref:blazor/hosting-models#server-side), Blazor jest wykonywana na serwerze z poziomu aplikacji ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ae5e0-108">With the [server-side hosting model](xref:blazor/hosting-models#server-side), Blazor is executed on the server from within an ASP.NET Core app.</span></span> <span data-ttu-id="ae5e0-109">Aktualizacje interfejsu użytkownika, obsługa zdarzeń i wywołania języka JavaScript są obsługiwane za pośrednictwem [SignalR](xref:signalr/introduction) połączenia.</span><span class="sxs-lookup"><span data-stu-id="ae5e0-109">UI updates, event handling, and JavaScript calls are handled over a [SignalR](xref:signalr/introduction) connection.</span></span>

<span data-ttu-id="ae5e0-110">Wymagany jest serwer sieci web, zdolne do obsługi aplikacji ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ae5e0-110">A web server capable of hosting an ASP.NET Core app is required.</span></span> <span data-ttu-id="ae5e0-111">Program Visual Studio obejmuje **Blazor Server App** szablonu projektu (`blazorserverside` szablon, korzystając z [dotnet nowe](/dotnet/core/tools/dotnet-new) polecenie).</span><span class="sxs-lookup"><span data-stu-id="ae5e0-111">Visual Studio includes the **Blazor Server App** project template (`blazorserverside` template when using the [dotnet new](/dotnet/core/tools/dotnet-new) command).</span></span>

## <a name="connection-scale-out"></a><span data-ttu-id="ae5e0-112">Skalowanie do połączenia</span><span class="sxs-lookup"><span data-stu-id="ae5e0-112">Connection scale out</span></span>

<span data-ttu-id="ae5e0-113">Aplikacje serwerowe Blazor wymagają jednego aktywnego połączenia SignalR dla każdego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="ae5e0-113">Blazor server-side apps require one active SignalR connection for each user.</span></span> <span data-ttu-id="ae5e0-114">Produkcyjne wdrożenie po stronie serwera Blazor wymaga rozwiązanie do obsługi dowolnej liczby jednoczesnych połączeń, zgodnie z wymaganiami aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ae5e0-114">A production Blazor server-side deployment requires a solution for supporting as many concurrent connections as required by the app.</span></span> <span data-ttu-id="ae5e0-115">[Usługi Azure SignalR Service](/azure/azure-signalr/) obsługuje skalowanie połączeń i jest zalecana jako rozwiązanie skalowania dla aplikacji po stronie serwera Blazor.</span><span class="sxs-lookup"><span data-stu-id="ae5e0-115">The [Azure SignalR Service](/azure/azure-signalr/) handles the scaling of connections and is recommended as a scaling solution for Blazor server-side apps.</span></span> <span data-ttu-id="ae5e0-116">Aby uzyskać więcej informacji, zobacz <xref:signalr/publish-to-azure-web-app>.</span><span class="sxs-lookup"><span data-stu-id="ae5e0-116">For more information, see <xref:signalr/publish-to-azure-web-app>.</span></span>

## <a name="signalr-configuration"></a><span data-ttu-id="ae5e0-117">Konfiguracja SignalR</span><span class="sxs-lookup"><span data-stu-id="ae5e0-117">SignalR configuration</span></span>

<span data-ttu-id="ae5e0-118">Dla najbardziej typowych scenariuszy serwerowych Blazor skonfigurowano SignalR, ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ae5e0-118">SignalR is configured by ASP.NET Core for the most common Blazor server-side scenarios.</span></span> <span data-ttu-id="ae5e0-119">Niestandardowe i zaawansowane scenariusze, można znaleźć w artykułach SignalR w [dodatkowe zasoby](#additional-resources) sekcji.</span><span class="sxs-lookup"><span data-stu-id="ae5e0-119">For custom and advanced scenarios, consult the SignalR articles in the [Additional resources](#additional-resources) section.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ae5e0-120">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="ae5e0-120">Additional resources</span></span>

* <xref:signalr/introduction>
* [<span data-ttu-id="ae5e0-121">Dokumentacja usługi Azure SignalR Service</span><span class="sxs-lookup"><span data-stu-id="ae5e0-121">Azure SignalR Service Documentation</span></span>](/azure/azure-signalr/)
* [<span data-ttu-id="ae5e0-122">Szybki start: Tworzenie pokoju rozmów przy użyciu usługi SignalR</span><span class="sxs-lookup"><span data-stu-id="ae5e0-122">Quickstart: Create a chat room by using SignalR Service</span></span>](/azure/azure-signalr/signalr-quickstart-dotnet-core)
* <xref:host-and-deploy/index>
* <xref:tutorials/publish-to-azure-webapp-using-vs>
* [<span data-ttu-id="ae5e0-123">Wdrażanie platformy ASP.NET Core w wersji zapoznawczej w usłudze Azure App Service</span><span class="sxs-lookup"><span data-stu-id="ae5e0-123">Deploy ASP.NET Core preview release to Azure App Service</span></span>](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service)
