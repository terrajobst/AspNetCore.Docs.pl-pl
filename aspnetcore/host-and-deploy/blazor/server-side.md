---
title: Hostowanie i wdrażanie ASP.NET Core Blazor po stronie serwera
author: guardrex
description: Dowiedz się, jak hostować i wdrażać aplikację Blazor po stronie serwera przy użyciu ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/05/2019
uid: host-and-deploy/blazor/server-side
ms.openlocfilehash: 8da71faf6abc5929d6cd43d42fd896e378d99ef6
ms.sourcegitcommit: f65d8765e4b7c894481db9b37aa6969abc625a48
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/06/2019
ms.locfileid: "70773570"
---
# <a name="host-and-deploy-blazor-server-side"></a><span data-ttu-id="56b96-103">Hostowanie i wdrażanie Blazor po stronie serwera</span><span class="sxs-lookup"><span data-stu-id="56b96-103">Host and deploy Blazor server-side</span></span>

<span data-ttu-id="56b96-104">[Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com)i [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="56b96-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="56b96-105">Wartości konfiguracji hosta</span><span class="sxs-lookup"><span data-stu-id="56b96-105">Host configuration values</span></span>

<span data-ttu-id="56b96-106">Aplikacje po stronie serwera, które używają [modelu hostingu po stronie serwera](xref:blazor/hosting-models#server-side) , mogą akceptować [ogólne wartości konfiguracji hosta](xref:fundamentals/host/generic-host#host-configuration).</span><span class="sxs-lookup"><span data-stu-id="56b96-106">Server-side apps that use the [server-side hosting model](xref:blazor/hosting-models#server-side) can accept [Generic Host configuration values](xref:fundamentals/host/generic-host#host-configuration).</span></span>

## <a name="deployment"></a><span data-ttu-id="56b96-107">wdrażania</span><span class="sxs-lookup"><span data-stu-id="56b96-107">Deployment</span></span>

<span data-ttu-id="56b96-108">[Model hostingu po stronie serwera](xref:blazor/hosting-models#server-side)Blazor jest wykonywany na serwerze z poziomu aplikacji ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="56b96-108">With the [server-side hosting model](xref:blazor/hosting-models#server-side), Blazor is executed on the server from within an ASP.NET Core app.</span></span> <span data-ttu-id="56b96-109">Aktualizacje interfejsu użytkownika, obsługa zdarzeń i wywołania języka JavaScript są obsługiwane przez połączenie [sygnalizujące](xref:signalr/introduction) .</span><span class="sxs-lookup"><span data-stu-id="56b96-109">UI updates, event handling, and JavaScript calls are handled over a [SignalR](xref:signalr/introduction) connection.</span></span>

<span data-ttu-id="56b96-110">Wymagany jest serwer sieci Web obsługujący aplikację ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="56b96-110">A web server capable of hosting an ASP.NET Core app is required.</span></span> <span data-ttu-id="56b96-111">Program Visual Studio zawiera szablon projektu **aplikacji Blazor Server** (`blazorserverside` szablon w przypadku używania polecenia [dotnet New](/dotnet/core/tools/dotnet-new) ).</span><span class="sxs-lookup"><span data-stu-id="56b96-111">Visual Studio includes the **Blazor Server App** project template (`blazorserverside` template when using the [dotnet new](/dotnet/core/tools/dotnet-new) command).</span></span>

## <a name="connection-scale-out"></a><span data-ttu-id="56b96-112">Skalowanie w poziomie połączenia</span><span class="sxs-lookup"><span data-stu-id="56b96-112">Connection scale out</span></span>

<span data-ttu-id="56b96-113">Blazor aplikacje po stronie serwera wymagają jednego aktywnego połączenia sygnalizującego dla każdego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="56b96-113">Blazor server-side apps require one active SignalR connection for each user.</span></span> <span data-ttu-id="56b96-114">Wdrożenie po stronie serwera produkcyjnego Blazor wymaga rozwiązania do obsługi tylu połączeń współbieżnych wymaganych przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="56b96-114">A production Blazor server-side deployment requires a solution for supporting as many concurrent connections as required by the app.</span></span> <span data-ttu-id="56b96-115">[Usługa Azure Signal](/azure/azure-signalr/) obsługuje skalowanie połączeń i jest zalecana jako rozwiązanie do skalowania dla aplikacji po stronie serwera Blazor.</span><span class="sxs-lookup"><span data-stu-id="56b96-115">The [Azure SignalR Service](/azure/azure-signalr/) handles the scaling of connections and is recommended as a scaling solution for Blazor server-side apps.</span></span> <span data-ttu-id="56b96-116">Aby uzyskać więcej informacji, zobacz <xref:signalr/publish-to-azure-web-app>.</span><span class="sxs-lookup"><span data-stu-id="56b96-116">For more information, see <xref:signalr/publish-to-azure-web-app>.</span></span>

## <a name="signalr-configuration"></a><span data-ttu-id="56b96-117">Konfiguracja sygnalizującego</span><span class="sxs-lookup"><span data-stu-id="56b96-117">SignalR configuration</span></span>

<span data-ttu-id="56b96-118">Program sygnalizujący jest konfigurowany przez ASP.NET Core dla najpopularniejszych scenariuszy po stronie serwera Blazor.</span><span class="sxs-lookup"><span data-stu-id="56b96-118">SignalR is configured by ASP.NET Core for the most common Blazor server-side scenarios.</span></span> <span data-ttu-id="56b96-119">W przypadku scenariuszy niestandardowych i zaawansowanych zapoznaj się z artykułami dotyczącymi sygnałów w sekcji [dodatkowe zasoby](#additional-resources) .</span><span class="sxs-lookup"><span data-stu-id="56b96-119">For custom and advanced scenarios, consult the SignalR articles in the [Additional resources](#additional-resources) section.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="56b96-120">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="56b96-120">Additional resources</span></span>

* <xref:signalr/introduction>
* [<span data-ttu-id="56b96-121">Dokumentacja usługi Azure sygnalizującego</span><span class="sxs-lookup"><span data-stu-id="56b96-121">Azure SignalR Service Documentation</span></span>](/azure/azure-signalr/)
* [<span data-ttu-id="56b96-122">Szybki start: Tworzenie pokoju rozmów przy użyciu usługi sygnalizującej</span><span class="sxs-lookup"><span data-stu-id="56b96-122">Quickstart: Create a chat room by using SignalR Service</span></span>](/azure/azure-signalr/signalr-quickstart-dotnet-core)
* <xref:host-and-deploy/index>
* <xref:tutorials/publish-to-azure-webapp-using-vs>
* [<span data-ttu-id="56b96-123">Wdróż ASP.NET Core wersji zapoznawczej do Azure App Service</span><span class="sxs-lookup"><span data-stu-id="56b96-123">Deploy ASP.NET Core preview release to Azure App Service</span></span>](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service)
