---
title: Hostowanie i wdrażanie ASP.NET Core Blazor Server
author: guardrex
description: Dowiedz się, jak hostować i wdrażać aplikację Blazor Server przy użyciu ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/05/2019
uid: host-and-deploy/blazor/server
ms.openlocfilehash: 693d7ff67bad3a0c5bd050b795833763056ed511
ms.sourcegitcommit: dd026eceee79e943bd6b4a37b144803b50617583
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/15/2019
ms.locfileid: "72378817"
---
# <a name="host-and-deploy-blazor-server"></a><span data-ttu-id="c3642-103">Hostowanie i wdrażanie serwera Blazor</span><span class="sxs-lookup"><span data-stu-id="c3642-103">Host and deploy Blazor Server</span></span>

<span data-ttu-id="c3642-104">[Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com)i [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="c3642-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="c3642-105">Wartości konfiguracji hosta</span><span class="sxs-lookup"><span data-stu-id="c3642-105">Host configuration values</span></span>

<span data-ttu-id="c3642-106">[Aplikacje serwera Blazor](xref:blazor/hosting-models#blazor-server) mogą akceptować [ogólne wartości konfiguracji hosta](xref:fundamentals/host/generic-host#host-configuration).</span><span class="sxs-lookup"><span data-stu-id="c3642-106">[Blazor Server apps](xref:blazor/hosting-models#blazor-server) can accept [Generic Host configuration values](xref:fundamentals/host/generic-host#host-configuration).</span></span>

## <a name="deployment"></a><span data-ttu-id="c3642-107">wdrażania</span><span class="sxs-lookup"><span data-stu-id="c3642-107">Deployment</span></span>

<span data-ttu-id="c3642-108">Korzystając z [modelu hostingu serwera Blazor](xref:blazor/hosting-models#blazor-server), Blazor jest wykonywany na serwerze z poziomu aplikacji ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c3642-108">Using the [Blazor Server hosting model](xref:blazor/hosting-models#blazor-server), Blazor is executed on the server from within an ASP.NET Core app.</span></span> <span data-ttu-id="c3642-109">Aktualizacje interfejsu użytkownika, obsługa zdarzeń i wywołania języka JavaScript są obsługiwane przez połączenie [sygnalizujące](xref:signalr/introduction) .</span><span class="sxs-lookup"><span data-stu-id="c3642-109">UI updates, event handling, and JavaScript calls are handled over a [SignalR](xref:signalr/introduction) connection.</span></span>

<span data-ttu-id="c3642-110">Wymagany jest serwer sieci Web obsługujący aplikację ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c3642-110">A web server capable of hosting an ASP.NET Core app is required.</span></span> <span data-ttu-id="c3642-111">Program Visual Studio zawiera szablon projektu **aplikacji serwera Blazor** (`blazorserverside`, gdy jest używany polecenie [dotnet New](/dotnet/core/tools/dotnet-new) ).</span><span class="sxs-lookup"><span data-stu-id="c3642-111">Visual Studio includes the **Blazor Server App** project template (`blazorserverside` template when using the [dotnet new](/dotnet/core/tools/dotnet-new) command).</span></span>

## <a name="scalability"></a><span data-ttu-id="c3642-112">Względem</span><span class="sxs-lookup"><span data-stu-id="c3642-112">Scalability</span></span>

<span data-ttu-id="c3642-113">Zaplanuj wdrożenie, aby najlepiej wykorzystać dostępną infrastrukturę dla aplikacji serwera Blazor.</span><span class="sxs-lookup"><span data-stu-id="c3642-113">Plan a deployment to make the best use of the available infrastructure for a Blazor Server app.</span></span> <span data-ttu-id="c3642-114">Zapoznaj się z poniższymi zasobami, aby rozwiązać o skalowalność aplikacji serwera Blazor:</span><span class="sxs-lookup"><span data-stu-id="c3642-114">See the following resources to address Blazor Server app scalability:</span></span>

* [<span data-ttu-id="c3642-115">Podstawy aplikacji serwera Blazor</span><span class="sxs-lookup"><span data-stu-id="c3642-115">Fundamentals of Blazor Server apps</span></span>](xref:blazor/hosting-models#blazor-server)
* <xref:security/blazor/server>

### <a name="deployment-server"></a><span data-ttu-id="c3642-116">Serwer wdrażania</span><span class="sxs-lookup"><span data-stu-id="c3642-116">Deployment server</span></span>

<span data-ttu-id="c3642-117">Gdy rozważasz skalowalność pojedynczego serwera (skalowanie w górę), pamięć dostępna dla aplikacji jest prawdopodobnie pierwszym zasobem, który zostanie wyczerpany przez aplikację w miarę wzrostu wymagań użytkownika.</span><span class="sxs-lookup"><span data-stu-id="c3642-117">When considering the scalability of a single server (scale up), the memory available to an app is likely the first resource that the app will exhaust as user demands increase.</span></span> <span data-ttu-id="c3642-118">Dostępna pamięć na serwerze ma wpływ na:</span><span class="sxs-lookup"><span data-stu-id="c3642-118">The available memory on the server affects the:</span></span>

* <span data-ttu-id="c3642-119">Liczba aktywnych obwodów obsługiwanych przez serwer.</span><span class="sxs-lookup"><span data-stu-id="c3642-119">Number of active circuits that a server can support.</span></span>
* <span data-ttu-id="c3642-120">Opóźnienie interfejsu użytkownika na kliencie.</span><span class="sxs-lookup"><span data-stu-id="c3642-120">UI latency on the client.</span></span>

<span data-ttu-id="c3642-121">Aby uzyskać wskazówki dotyczące tworzenia bezpiecznych i skalowalnych aplikacji serwera Blazor, zobacz <xref:security/blazor/server>.</span><span class="sxs-lookup"><span data-stu-id="c3642-121">For guidance on building secure and scalable Blazor server apps, see <xref:security/blazor/server>.</span></span>

<span data-ttu-id="c3642-122">Każdy obwód wykorzystuje około 250 KB pamięci w przypadku aplikacji o minimalnej *Hello World*.</span><span class="sxs-lookup"><span data-stu-id="c3642-122">Each circuit uses approximately 250 KB of memory for a minimal *Hello World*-style app.</span></span> <span data-ttu-id="c3642-123">Rozmiar obwodu zależy od kodu aplikacji i wymagań dotyczących konserwacji stanu związanych z poszczególnymi składnikami.</span><span class="sxs-lookup"><span data-stu-id="c3642-123">The size of a circuit depends on the app's code and the state maintenance requirements associated with each component.</span></span> <span data-ttu-id="c3642-124">Zalecamy mierzenie wymagań dotyczących zasobów podczas opracowywania aplikacji i infrastruktury, ale następujący punkt odniesienia może być punktem początkowym w planowaniu celu wdrożenia: jeśli oczekujesz, że aplikacja będzie obsługiwać 5 000 współbieżnych użytkowników, rozważ budżetowanie o najmniej 1,3 GB pamięci serwera do aplikacji (lub ~ 273 KB na użytkownika).</span><span class="sxs-lookup"><span data-stu-id="c3642-124">We recommend that you measure resource demands during development for your app and infrastructure, but the following baseline can be a starting point in planning your deployment target: If you expect your app to support 5,000 concurrent users, consider budgeting at least 1.3 GB of server memory to the app (or ~273 KB per user).</span></span>

### <a name="signalr-configuration"></a><span data-ttu-id="c3642-125">Konfiguracja sygnalizującego</span><span class="sxs-lookup"><span data-stu-id="c3642-125">SignalR configuration</span></span>

<span data-ttu-id="c3642-126">Aplikacje serwera Blazor używają sygnalizacji ASP.NET Core do komunikowania się z przeglądarką.</span><span class="sxs-lookup"><span data-stu-id="c3642-126">Blazor Server apps use ASP.NET Core SignalR to communicate with the browser.</span></span> <span data-ttu-id="c3642-127">[Warunki hostingu i skalowania sygnalizującego](xref:signalr/publish-to-azure-web-app) dotyczą aplikacji serwera Blazor.</span><span class="sxs-lookup"><span data-stu-id="c3642-127">[SignalR's hosting and scaling conditions](xref:signalr/publish-to-azure-web-app) apply to Blazor Server apps.</span></span>

<span data-ttu-id="c3642-128">Blazor najlepiej sprawdza się w przypadku korzystania z usługi WebSockets jako transportu sygnalizującego ze względu na mniejsze opóźnienia, niezawodność i [bezpieczeństwo](xref:signalr/security).</span><span class="sxs-lookup"><span data-stu-id="c3642-128">Blazor works best when using WebSockets as the SignalR transport due to lower latency, reliability, and [security](xref:signalr/security).</span></span> <span data-ttu-id="c3642-129">Długi sondowanie jest używane przez program sygnalizujący, gdy obiekty WebSockets nie są dostępne lub gdy aplikacja jest jawnie skonfigurowana do korzystania z długiego sondowania.</span><span class="sxs-lookup"><span data-stu-id="c3642-129">Long Polling is used by SignalR when WebSockets isn't available or when the app is explicitly configured to use Long Polling.</span></span> <span data-ttu-id="c3642-130">Podczas wdrażania programu w celu Azure App Service Skonfiguruj aplikację do używania obiektów WebSockets w ustawieniach Azure Portal dla usługi.</span><span class="sxs-lookup"><span data-stu-id="c3642-130">When deploying to Azure App Service, configure the app to use WebSockets in the Azure portal settings for the service.</span></span> <span data-ttu-id="c3642-131">Aby uzyskać szczegółowe informacje dotyczące konfigurowania aplikacji na potrzeby Azure App Service, zobacz [wskazówki dotyczące publikowania sygnałów](xref:signalr/publish-to-azure-web-app).</span><span class="sxs-lookup"><span data-stu-id="c3642-131">For details on configuring the app for Azure App Service, see the [SignalR publishing guidelines](xref:signalr/publish-to-azure-web-app).</span></span>

<span data-ttu-id="c3642-132">Zalecamy korzystanie z [usługi Azure Signal Service](/azure/azure-signalr) dla aplikacji serwera Blazor.</span><span class="sxs-lookup"><span data-stu-id="c3642-132">We recommend using the [Azure SignalR Service](/azure/azure-signalr) for Blazor Server apps.</span></span> <span data-ttu-id="c3642-133">Usługa umożliwia skalowanie aplikacji serwera Blazor na dużą liczbę współbieżnych połączeń sygnałów.</span><span class="sxs-lookup"><span data-stu-id="c3642-133">The service allows for scaling up a Blazor Server app to a large number of concurrent SignalR connections.</span></span> <span data-ttu-id="c3642-134">Ponadto globalne zasięgi i wysokiej wydajności centrów danych usługi sygnalizujących znacznie ułatwiają zredukowanie opóźnień ze względu na lokalizację geograficzną.</span><span class="sxs-lookup"><span data-stu-id="c3642-134">In addition, the SignalR service's global reach and high-performance data centers significantly aid in reducing latency due to geography.</span></span> <span data-ttu-id="c3642-135">Aby skonfigurować aplikację (i opcjonalnie zainicjować obsługę administracyjną) usługi Azure Signal:</span><span class="sxs-lookup"><span data-stu-id="c3642-135">To configure an app (and optionally provision) the Azure SignalR Service:</span></span>

* <span data-ttu-id="c3642-136">Utwórz profil publikowania aplikacji platformy Azure w programie Visual Studio dla aplikacji serwera Blazor.</span><span class="sxs-lookup"><span data-stu-id="c3642-136">Create an Azure Apps publish profile in Visual Studio for the Blazor Server app.</span></span>
* <span data-ttu-id="c3642-137">Dodaj zależność **usługi sygnałów platformy Azure** do profilu.</span><span class="sxs-lookup"><span data-stu-id="c3642-137">Add the **Azure SignalR Service** dependency to the profile.</span></span> <span data-ttu-id="c3642-138">Jeśli subskrypcja platformy Azure nie ma istniejącego wystąpienia usługi Azure Signal Service do przypisania do aplikacji, wybierz opcję **Utwórz nowe wystąpienie usługi Azure Signal Service** , aby zainicjować obsługę nowego wystąpienia usługi.</span><span class="sxs-lookup"><span data-stu-id="c3642-138">If the Azure subscription doesn't have a pre-existing Azure SignalR Service instance to assign to the app, select **Create a new Azure SignalR Service instance** to provision a new service instance.</span></span>
* <span data-ttu-id="c3642-139">Opublikuj aplikację na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="c3642-139">Publish the app to Azure.</span></span>

### <a name="measure-network-latency"></a><span data-ttu-id="c3642-140">Mierzenie opóźnienia sieci</span><span class="sxs-lookup"><span data-stu-id="c3642-140">Measure network latency</span></span>

<span data-ttu-id="c3642-141">Przy użyciu kodu [js Interop](xref:blazor/javascript-interop) można mierzyć opóźnienia sieci, jak pokazano w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="c3642-141">[JS interop](xref:blazor/javascript-interop) can be used to measure network latency, as the following example demonstrates:</span></span>

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

<span data-ttu-id="c3642-142">W celu uzyskania odpowiedniego środowiska interfejsu użytkownika zalecamy opóźnienia interfejsu użytkownika 250ms lub mniej.</span><span class="sxs-lookup"><span data-stu-id="c3642-142">For a reasonable UI experience, we recommend a sustained UI latency of 250ms or less.</span></span>
