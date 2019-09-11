---
title: Hostowanie i wdrażanie ASP.NET Core Blazor po stronie serwera
author: guardrex
description: Dowiedz się, jak hostować i wdrażać aplikację Blazor po stronie serwera przy użyciu ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/07/2019
uid: host-and-deploy/blazor/server-side
ms.openlocfilehash: fc47dfa1344b74ec7110211e3698217e246ab86d
ms.sourcegitcommit: e7c56e8da5419bbc20b437c2dd531dedf9b0dc6b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/10/2019
ms.locfileid: "70878488"
---
# <a name="host-and-deploy-blazor-server-side"></a><span data-ttu-id="66e1a-103">Hostowanie i wdrażanie Blazor po stronie serwera</span><span class="sxs-lookup"><span data-stu-id="66e1a-103">Host and deploy Blazor server-side</span></span>

<span data-ttu-id="66e1a-104">[Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com)i [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="66e1a-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="66e1a-105">Wartości konfiguracji hosta</span><span class="sxs-lookup"><span data-stu-id="66e1a-105">Host configuration values</span></span>

<span data-ttu-id="66e1a-106">Aplikacje po stronie serwera, które używają [modelu hostingu po stronie serwera](xref:blazor/hosting-models#server-side) , mogą akceptować [ogólne wartości konfiguracji hosta](xref:fundamentals/host/generic-host#host-configuration).</span><span class="sxs-lookup"><span data-stu-id="66e1a-106">Server-side apps that use the [server-side hosting model](xref:blazor/hosting-models#server-side) can accept [Generic Host configuration values](xref:fundamentals/host/generic-host#host-configuration).</span></span>

## <a name="deployment"></a><span data-ttu-id="66e1a-107">wdrażania</span><span class="sxs-lookup"><span data-stu-id="66e1a-107">Deployment</span></span>

<span data-ttu-id="66e1a-108">[Model hostingu po stronie serwera](xref:blazor/hosting-models#server-side)Blazor jest wykonywany na serwerze z poziomu aplikacji ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="66e1a-108">With the [server-side hosting model](xref:blazor/hosting-models#server-side), Blazor is executed on the server from within an ASP.NET Core app.</span></span> <span data-ttu-id="66e1a-109">Aktualizacje interfejsu użytkownika, obsługa zdarzeń i wywołania języka JavaScript są obsługiwane przez [](xref:signalr/introduction) połączenie sygnalizujące.</span><span class="sxs-lookup"><span data-stu-id="66e1a-109">UI updates, event handling, and JavaScript calls are handled over a [SignalR](xref:signalr/introduction) connection.</span></span>

<span data-ttu-id="66e1a-110">Wymagany jest serwer sieci Web obsługujący aplikację ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="66e1a-110">A web server capable of hosting an ASP.NET Core app is required.</span></span> <span data-ttu-id="66e1a-111">Program Visual Studio zawiera szablon projektu **aplikacji Blazor Server** (`blazorserverside` szablon w przypadku używania polecenia [dotnet New](/dotnet/core/tools/dotnet-new) ).</span><span class="sxs-lookup"><span data-stu-id="66e1a-111">Visual Studio includes the **Blazor Server App** project template (`blazorserverside` template when using the [dotnet new](/dotnet/core/tools/dotnet-new) command).</span></span>

## <a name="scalability"></a><span data-ttu-id="66e1a-112">Skalowalność</span><span class="sxs-lookup"><span data-stu-id="66e1a-112">Scalability</span></span>

<span data-ttu-id="66e1a-113">Zaplanuj wdrożenie, aby najlepiej wykorzystać dostępną infrastrukturę dla aplikacji serwera Blazor.</span><span class="sxs-lookup"><span data-stu-id="66e1a-113">Plan a deployment to make the best use of the available infrastructure for a Blazor Server app.</span></span> <span data-ttu-id="66e1a-114">Zapoznaj się z poniższymi zasobami, aby rozwiązać o skalowalność aplikacji serwera Blazor:</span><span class="sxs-lookup"><span data-stu-id="66e1a-114">See the following resources to address Blazor Server app scalability:</span></span>

* [<span data-ttu-id="66e1a-115">Podstawy aplikacji serwera Blazor</span><span class="sxs-lookup"><span data-stu-id="66e1a-115">Fundamentals of Blazor Server apps</span></span>](xref:blazor/hosting-models#server-side)
* <xref:security/blazor/server-side>

### <a name="deployment-server"></a><span data-ttu-id="66e1a-116">Serwer wdrażania</span><span class="sxs-lookup"><span data-stu-id="66e1a-116">Deployment server</span></span>

<span data-ttu-id="66e1a-117">Gdy rozważasz skalowalność pojedynczego serwera (skalowanie w górę), pamięć dostępna dla aplikacji jest prawdopodobnie pierwszym zasobem, który zostanie wyczerpany przez aplikację w miarę wzrostu wymagań użytkownika.</span><span class="sxs-lookup"><span data-stu-id="66e1a-117">When considering the scalability of a single server (scale up), the memory available to an app is likely the first resource that the app will exhaust as user demands increase.</span></span> <span data-ttu-id="66e1a-118">Dostępna pamięć na serwerze ma wpływ na:</span><span class="sxs-lookup"><span data-stu-id="66e1a-118">The available memory on the server affects the:</span></span>

* <span data-ttu-id="66e1a-119">Liczba aktywnych obwodów obsługiwanych przez serwer.</span><span class="sxs-lookup"><span data-stu-id="66e1a-119">Number of active circuits that a server can support.</span></span>
* <span data-ttu-id="66e1a-120">Opóźnienie interfejsu użytkownika na kliencie.</span><span class="sxs-lookup"><span data-stu-id="66e1a-120">UI latency on the client.</span></span>

<span data-ttu-id="66e1a-121">Aby uzyskać wskazówki dotyczące tworzenia bezpiecznych i skalowalnych aplikacji serwera Blazor <xref:security/blazor/server-side>, zobacz.</span><span class="sxs-lookup"><span data-stu-id="66e1a-121">For guidance on building secure and scalable Blazor server apps, see <xref:security/blazor/server-side>.</span></span>

<span data-ttu-id="66e1a-122">Każdy obwód wykorzystuje około 250 KB pamięci w przypadku aplikacji o minimalnej *Hello World*.</span><span class="sxs-lookup"><span data-stu-id="66e1a-122">Each circuit uses approximately 250 KB of memory for a minimal *Hello World*-style app.</span></span> <span data-ttu-id="66e1a-123">Rozmiar obwodu zależy od kodu aplikacji i wymagań dotyczących konserwacji stanu związanych z poszczególnymi składnikami.</span><span class="sxs-lookup"><span data-stu-id="66e1a-123">The size of a circuit depends on the app's code and the state maintenance requirements associated with each component.</span></span> <span data-ttu-id="66e1a-124">Zalecamy mierzenie wymagań dotyczących zasobów podczas opracowywania aplikacji i infrastruktury, ale następujący punkt odniesienia może być punktem początkowym w planowaniu celu wdrożenia: Jeśli oczekujesz, że aplikacja będzie obsługiwać 5 000 użytkowników współbieżnych, rozważ budżetowanie co najmniej 1,3 GB pamięci serwera do aplikacji (lub ~ 273 KB na użytkownika).</span><span class="sxs-lookup"><span data-stu-id="66e1a-124">We recommend that you measure resource demands during development for your app and infrastructure, but the following baseline can be a starting point in planning your deployment target: If you expect your app to support 5,000 concurrent users, consider budgeting at least 1.3 GB of server memory to the app (or ~273 KB per user).</span></span>

### <a name="signalr-configuration"></a><span data-ttu-id="66e1a-125">Konfiguracja sygnalizującego</span><span class="sxs-lookup"><span data-stu-id="66e1a-125">SignalR configuration</span></span>

<span data-ttu-id="66e1a-126">Aplikacje serwera Blazor używają sygnalizacji ASP.NET Core do komunikowania się z przeglądarką.</span><span class="sxs-lookup"><span data-stu-id="66e1a-126">Blazor Server apps use ASP.NET Core SignalR to communicate with the browser.</span></span> <span data-ttu-id="66e1a-127">[Warunki hostingu i skalowania sygnalizującego](xref:signalr/publish-to-azure-web-app) dotyczą aplikacji serwera Blazor.</span><span class="sxs-lookup"><span data-stu-id="66e1a-127">[SignalR's hosting and scaling conditions](xref:signalr/publish-to-azure-web-app) apply to Blazor Server apps.</span></span>

<span data-ttu-id="66e1a-128">Blazor najlepiej sprawdza się w przypadku korzystania z usługi WebSockets jako transportu sygnalizującego ze względu na mniejsze opóźnienia, niezawodność i [bezpieczeństwo](xref:signalr/security).</span><span class="sxs-lookup"><span data-stu-id="66e1a-128">Blazor works best when using WebSockets as the SignalR transport due to lower latency, reliability, and [security](xref:signalr/security).</span></span> <span data-ttu-id="66e1a-129">Długi sondowanie jest używane przez program sygnalizujący, gdy obiekty WebSockets nie są dostępne lub gdy aplikacja jest jawnie skonfigurowana do korzystania z długiego sondowania.</span><span class="sxs-lookup"><span data-stu-id="66e1a-129">Long Polling is used by SignalR when WebSockets isn't available or when the app is explicitly configured to use Long Polling.</span></span> <span data-ttu-id="66e1a-130">Podczas wdrażania programu w celu Azure App Service Skonfiguruj aplikację do używania obiektów WebSockets w ustawieniach Azure Portal dla usługi.</span><span class="sxs-lookup"><span data-stu-id="66e1a-130">When deploying to Azure App Service, configure the app to use WebSockets in the Azure portal settings for the service.</span></span> <span data-ttu-id="66e1a-131">Aby uzyskać szczegółowe informacje dotyczące konfigurowania aplikacji na potrzeby Azure App Service, zobacz [wskazówki dotyczące publikowania sygnałów](xref:signalr/publish-to-azure-web-app).</span><span class="sxs-lookup"><span data-stu-id="66e1a-131">For details on configuring the app for Azure App Service, see the [SignalR publishing guidelines](xref:signalr/publish-to-azure-web-app).</span></span>

<span data-ttu-id="66e1a-132">Zalecamy korzystanie z [usługi Azure Signal Service](/azure/azure-signalr) dla aplikacji serwera Blazor.</span><span class="sxs-lookup"><span data-stu-id="66e1a-132">We recommend using the [Azure SignalR Service](/azure/azure-signalr) for Blazor Server apps.</span></span> <span data-ttu-id="66e1a-133">Usługa umożliwia skalowanie aplikacji serwera Blazor na dużą liczbę współbieżnych połączeń sygnałów.</span><span class="sxs-lookup"><span data-stu-id="66e1a-133">The service allows for scaling up a Blazor Server app to a large number of concurrent SignalR connections.</span></span> <span data-ttu-id="66e1a-134">Ponadto globalne zasięgi i wysokiej wydajności centrów danych usługi sygnalizujących znacznie ułatwiają zredukowanie opóźnień ze względu na lokalizację geograficzną.</span><span class="sxs-lookup"><span data-stu-id="66e1a-134">In addition, the SignalR service's global reach and high-performance data centers significantly aid in reducing latency due to geography.</span></span>

### <a name="measure-network-latency"></a><span data-ttu-id="66e1a-135">Mierzenie opóźnienia sieci</span><span class="sxs-lookup"><span data-stu-id="66e1a-135">Measure network latency</span></span>

<span data-ttu-id="66e1a-136">Przy użyciu kodu [js Interop](xref:blazor/javascript-interop) można mierzyć opóźnienia sieci, jak pokazano w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="66e1a-136">[JS interop](xref:blazor/javascript-interop) can be used to measure network latency, as the following example demonstrates:</span></span>

```cshtml
@inject IJSRuntime JS

@if (latency is null)
{
    <span>Calculating...</span>
}
else
{
    <span>@(latency.Value.TotalMilliseconds)ms</span>
}

@code
{
    private DateTime startTime;
    private TimeSpan? latency;

    protected override async Task OnInitializedAsync()
    {
        startTime = DateTime.UtcNow;
        var _ = await JS.InvokeAsync<string>("toString");
        latency = DateTime.UtcNow - startTime;
    }
}
```

<span data-ttu-id="66e1a-137">W celu uzyskania odpowiedniego środowiska interfejsu użytkownika zalecamy opóźnienia interfejsu użytkownika 250ms lub mniej.</span><span class="sxs-lookup"><span data-stu-id="66e1a-137">For a reasonable UI experience, we recommend a sustained UI latency of 250ms or less.</span></span>
