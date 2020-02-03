---
title: Hostowanie i wdrażanie ASP.NET Core Blazor Server
author: guardrex
description: Dowiedz się, jak hostować i wdrażać aplikację Blazor Server przy użyciu ASP.NET Core.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/17/2020
no-loc:
- Blazor
- SignalR
uid: host-and-deploy/blazor/server
ms.openlocfilehash: e8b3a7faaf1dc88059a79abbc7e74657ebb2068c
ms.sourcegitcommit: eca76bd065eb94386165a0269f1e95092f23fa58
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2020
ms.locfileid: "76726731"
---
# <a name="host-and-deploy-opno-locblazor-server"></a><span data-ttu-id="d93f3-103">Hostowanie i wdrażanie serwera Blazor</span><span class="sxs-lookup"><span data-stu-id="d93f3-103">Host and deploy Blazor Server</span></span>

<span data-ttu-id="d93f3-104">[Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com)i [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="d93f3-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="d93f3-105">Wartości konfiguracji hosta</span><span class="sxs-lookup"><span data-stu-id="d93f3-105">Host configuration values</span></span>

<span data-ttu-id="d93f3-106">[aplikacje serweraBlazor](xref:blazor/hosting-models#blazor-server) mogą akceptować [ogólne wartości konfiguracji hosta](xref:fundamentals/host/generic-host#host-configuration).</span><span class="sxs-lookup"><span data-stu-id="d93f3-106">[Blazor Server apps](xref:blazor/hosting-models#blazor-server) can accept [Generic Host configuration values](xref:fundamentals/host/generic-host#host-configuration).</span></span>

## <a name="deployment"></a><span data-ttu-id="d93f3-107">wdrażania</span><span class="sxs-lookup"><span data-stu-id="d93f3-107">Deployment</span></span>

<span data-ttu-id="d93f3-108">Korzystając z [modelu hostinguBlazor Server](xref:blazor/hosting-models#blazor-server), Blazor jest wykonywane na serwerze z poziomu aplikacji ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d93f3-108">Using the [Blazor Server hosting model](xref:blazor/hosting-models#blazor-server), Blazor is executed on the server from within an ASP.NET Core app.</span></span> <span data-ttu-id="d93f3-109">Aktualizacje interfejsu użytkownika, obsługa zdarzeń i wywołania języka JavaScript są obsługiwane przez połączenie [SignalR](xref:signalr/introduction) .</span><span class="sxs-lookup"><span data-stu-id="d93f3-109">UI updates, event handling, and JavaScript calls are handled over a [SignalR](xref:signalr/introduction) connection.</span></span>

<span data-ttu-id="d93f3-110">Wymagany jest serwer sieci Web obsługujący aplikację ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d93f3-110">A web server capable of hosting an ASP.NET Core app is required.</span></span> <span data-ttu-id="d93f3-111">Program Visual Studio zawiera szablon projektu **aplikacjiBlazor Server** (szablon`blazorserverside` przy użyciu polecenia [dotnet New](/dotnet/core/tools/dotnet-new) ).</span><span class="sxs-lookup"><span data-stu-id="d93f3-111">Visual Studio includes the **Blazor Server App** project template (`blazorserverside` template when using the [dotnet new](/dotnet/core/tools/dotnet-new) command).</span></span>

## <a name="scalability"></a><span data-ttu-id="d93f3-112">Skalowalność</span><span class="sxs-lookup"><span data-stu-id="d93f3-112">Scalability</span></span>

<span data-ttu-id="d93f3-113">Zaplanuj wdrożenie, aby najlepiej wykorzystać dostępną infrastrukturę dla aplikacji serwera Blazor.</span><span class="sxs-lookup"><span data-stu-id="d93f3-113">Plan a deployment to make the best use of the available infrastructure for a Blazor Server app.</span></span> <span data-ttu-id="d93f3-114">Zapoznaj się z poniższymi zasobami, aby rozwiązać Blazor skalowalności aplikacji serwerowej:</span><span class="sxs-lookup"><span data-stu-id="d93f3-114">See the following resources to address Blazor Server app scalability:</span></span>

* <span data-ttu-id="d93f3-115">[Podstawowe informacje o aplikacjach serwerowych Blazor](xref:blazor/hosting-models#blazor-server)</span><span class="sxs-lookup"><span data-stu-id="d93f3-115">[Fundamentals of Blazor Server apps](xref:blazor/hosting-models#blazor-server)</span></span>
* <xref:security/blazor/server>

### <a name="deployment-server"></a><span data-ttu-id="d93f3-116">Serwer wdrażania</span><span class="sxs-lookup"><span data-stu-id="d93f3-116">Deployment server</span></span>

<span data-ttu-id="d93f3-117">Gdy rozważasz skalowalność pojedynczego serwera (skalowanie w górę), pamięć dostępna dla aplikacji jest prawdopodobnie pierwszym zasobem, który zostanie wyczerpany przez aplikację w miarę wzrostu wymagań użytkownika.</span><span class="sxs-lookup"><span data-stu-id="d93f3-117">When considering the scalability of a single server (scale up), the memory available to an app is likely the first resource that the app will exhaust as user demands increase.</span></span> <span data-ttu-id="d93f3-118">Dostępna pamięć na serwerze ma wpływ na:</span><span class="sxs-lookup"><span data-stu-id="d93f3-118">The available memory on the server affects the:</span></span>

* <span data-ttu-id="d93f3-119">Liczba aktywnych obwodów obsługiwanych przez serwer.</span><span class="sxs-lookup"><span data-stu-id="d93f3-119">Number of active circuits that a server can support.</span></span>
* <span data-ttu-id="d93f3-120">Opóźnienie interfejsu użytkownika na kliencie.</span><span class="sxs-lookup"><span data-stu-id="d93f3-120">UI latency on the client.</span></span>

<span data-ttu-id="d93f3-121">Aby uzyskać wskazówki dotyczące tworzenia bezpiecznych i skalowalnych aplikacji serwera Blazor, zobacz <xref:security/blazor/server>.</span><span class="sxs-lookup"><span data-stu-id="d93f3-121">For guidance on building secure and scalable Blazor server apps, see <xref:security/blazor/server>.</span></span>

<span data-ttu-id="d93f3-122">Każdy obwód wykorzystuje około 250 KB pamięci w przypadku aplikacji o minimalnej *Hello World*.</span><span class="sxs-lookup"><span data-stu-id="d93f3-122">Each circuit uses approximately 250 KB of memory for a minimal *Hello World*-style app.</span></span> <span data-ttu-id="d93f3-123">Rozmiar obwodu zależy od kodu aplikacji i wymagań dotyczących konserwacji stanu związanych z poszczególnymi składnikami.</span><span class="sxs-lookup"><span data-stu-id="d93f3-123">The size of a circuit depends on the app's code and the state maintenance requirements associated with each component.</span></span> <span data-ttu-id="d93f3-124">Zalecamy mierzenie wymagań dotyczących zasobów podczas opracowywania aplikacji i infrastruktury, ale następujący punkt odniesienia może być punktem początkowym w planowaniu celu wdrożenia: jeśli oczekujesz, że aplikacja będzie obsługiwać 5 000 współbieżnych użytkowników, rozważ budżetowanie o najmniej 1,3 GB pamięci serwera do aplikacji (lub ~ 273 KB na użytkownika).</span><span class="sxs-lookup"><span data-stu-id="d93f3-124">We recommend that you measure resource demands during development for your app and infrastructure, but the following baseline can be a starting point in planning your deployment target: If you expect your app to support 5,000 concurrent users, consider budgeting at least 1.3 GB of server memory to the app (or ~273 KB per user).</span></span>

### <a name="opno-locsignalr-configuration"></a><span data-ttu-id="d93f3-125">Konfiguracja SignalR</span><span class="sxs-lookup"><span data-stu-id="d93f3-125">SignalR configuration</span></span>

<span data-ttu-id="d93f3-126">aplikacje serwera Blazor używają ASP.NET Core SignalR do komunikowania się z przeglądarką.</span><span class="sxs-lookup"><span data-stu-id="d93f3-126">Blazor Server apps use ASP.NET Core SignalR to communicate with the browser.</span></span> <span data-ttu-id="d93f3-127">[warunki hostingu i skalowaniaSignalR](xref:signalr/publish-to-azure-web-app) mają zastosowanie do aplikacji Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="d93f3-127">[SignalR's hosting and scaling conditions](xref:signalr/publish-to-azure-web-app) apply to Blazor Server apps.</span></span>

Blazor<span data-ttu-id="d93f3-128"> najlepiej sprawdza się w przypadku korzystania z usługi WebSockets jako transportu SignalR ze względu na mniejsze opóźnienia, niezawodność i [bezpieczeństwo](xref:signalr/security).</span><span class="sxs-lookup"><span data-stu-id="d93f3-128"> works best when using WebSockets as the SignalR transport due to lower latency, reliability, and [security](xref:signalr/security).</span></span> <span data-ttu-id="d93f3-129">Długie sondowanie jest używane przez SignalR, gdy obiekty WebSockets nie są dostępne lub gdy aplikacja jest jawnie skonfigurowana do korzystania z długotrwałego sondowania.</span><span class="sxs-lookup"><span data-stu-id="d93f3-129">Long Polling is used by SignalR when WebSockets isn't available or when the app is explicitly configured to use Long Polling.</span></span> <span data-ttu-id="d93f3-130">Podczas wdrażania programu w celu Azure App Service Skonfiguruj aplikację do używania obiektów WebSockets w ustawieniach Azure Portal dla usługi.</span><span class="sxs-lookup"><span data-stu-id="d93f3-130">When deploying to Azure App Service, configure the app to use WebSockets in the Azure portal settings for the service.</span></span> <span data-ttu-id="d93f3-131">Aby uzyskać szczegółowe informacje dotyczące konfigurowania aplikacji na potrzeby Azure App Service, zapoznaj się z tematem [SignalR wskazówki dotyczące publikowania](xref:signalr/publish-to-azure-web-app).</span><span class="sxs-lookup"><span data-stu-id="d93f3-131">For details on configuring the app for Azure App Service, see the [SignalR publishing guidelines](xref:signalr/publish-to-azure-web-app).</span></span>

#### <a name="azure-opno-locsignalr-service"></a><span data-ttu-id="d93f3-132">Usługa SignalR platformy Azure</span><span class="sxs-lookup"><span data-stu-id="d93f3-132">Azure SignalR Service</span></span>

<span data-ttu-id="d93f3-133">Zalecamy korzystanie z [usługi Azure SignalR](/azure/azure-signalr) dla aplikacji Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="d93f3-133">We recommend using the [Azure SignalR Service](/azure/azure-signalr) for Blazor Server apps.</span></span> <span data-ttu-id="d93f3-134">Usługa umożliwia skalowanie aplikacji serwera Blazor do dużej liczby jednoczesnych połączeń SignalR.</span><span class="sxs-lookup"><span data-stu-id="d93f3-134">The service allows for scaling up a Blazor Server app to a large number of concurrent SignalR connections.</span></span> <span data-ttu-id="d93f3-135">Ponadto globalne zasięgi i wysokiej wydajności centrów danych usługi SignalR znacznie ułatwiają zredukowanie opóźnień ze względu na lokalizację geograficzną.</span><span class="sxs-lookup"><span data-stu-id="d93f3-135">In addition, the SignalR service's global reach and high-performance data centers significantly aid in reducing latency due to geography.</span></span> <span data-ttu-id="d93f3-136">Aby skonfigurować aplikację (i opcjonalnie zainicjować obsługę administracyjną) usługi Azure SignalR:</span><span class="sxs-lookup"><span data-stu-id="d93f3-136">To configure an app (and optionally provision) the Azure SignalR Service:</span></span>

1. <span data-ttu-id="d93f3-137">Włącz obsługę *sesji usługi Sticky Notes*, w przypadku których klienci są [przekierowywani z powrotem do tego samego serwera podczas renderowania](xref:blazor/hosting-models#reconnection-to-the-same-server).</span><span class="sxs-lookup"><span data-stu-id="d93f3-137">Enable the service to support *sticky sessions*, where clients are [redirected back to the same server when prerendering](xref:blazor/hosting-models#reconnection-to-the-same-server).</span></span> <span data-ttu-id="d93f3-138">Ustaw opcję `ServerStickyMode` lub wartość konfiguracji na `Required`.</span><span class="sxs-lookup"><span data-stu-id="d93f3-138">Set the `ServerStickyMode` option or configuration value to `Required`.</span></span> <span data-ttu-id="d93f3-139">Zazwyczaj aplikacja tworzy konfigurację przy użyciu **jednej** z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="d93f3-139">Typically, an app creates the configuration using **one** of the following approaches:</span></span>

   * <span data-ttu-id="d93f3-140">`Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="d93f3-140">`Startup.ConfigureServices`:</span></span>
  
     ```csharp
     services.AddSignalR().AddAzureSignalR(options =>
     {
         options.ServerStickyMode = 
             Microsoft.Azure.SignalR.ServerStickyMode.Required;
     });
     ```

   * <span data-ttu-id="d93f3-141">Konfiguracja (Użyj **jednej** z następujących metod):</span><span class="sxs-lookup"><span data-stu-id="d93f3-141">Configuration (use **one** of the following approaches):</span></span>
  
     * <span data-ttu-id="d93f3-142">*appSettings. JSON*:</span><span class="sxs-lookup"><span data-stu-id="d93f3-142">*appsettings.json*:</span></span>

       ```json
       "Azure:SignalR:ServerStickyMode": "Required"
       ```

     * <span data-ttu-id="d93f3-143">**Konfiguracja** usługi app Service > **ustawienia aplikacji** w Azure Portal (**Nazwa**: `Azure:SignalR:ServerStickyMode`, **wartość**: `Required`).</span><span class="sxs-lookup"><span data-stu-id="d93f3-143">The app service's **Configuration** > **Application settings** in the Azure portal (**Name**: `Azure:SignalR:ServerStickyMode`, **Value**: `Required`).</span></span>

1. <span data-ttu-id="d93f3-144">Utwórz profil publikowania aplikacji platformy Azure w programie Visual Studio dla aplikacji serwera Blazor.</span><span class="sxs-lookup"><span data-stu-id="d93f3-144">Create an Azure Apps publish profile in Visual Studio for the Blazor Server app.</span></span>
1. <span data-ttu-id="d93f3-145">Dodaj zależność **usługi SignalR platformy Azure** do profilu.</span><span class="sxs-lookup"><span data-stu-id="d93f3-145">Add the **Azure SignalR Service** dependency to the profile.</span></span> <span data-ttu-id="d93f3-146">Jeśli subskrypcja platformy Azure nie ma istniejącego wystąpienia usługi SignalR platformy Azure do przypisania do aplikacji, wybierz pozycję **Utwórz nowe wystąpienie usługi azure SignalR** , aby udostępnić nowe wystąpienie usługi.</span><span class="sxs-lookup"><span data-stu-id="d93f3-146">If the Azure subscription doesn't have a pre-existing Azure SignalR Service instance to assign to the app, select **Create a new Azure SignalR Service instance** to provision a new service instance.</span></span>
1. <span data-ttu-id="d93f3-147">Publikowanie aplikacji na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="d93f3-147">Publish the app to Azure.</span></span>

#### <a name="iis"></a><span data-ttu-id="d93f3-148">IIS</span><span class="sxs-lookup"><span data-stu-id="d93f3-148">IIS</span></span>

<span data-ttu-id="d93f3-149">W przypadku korzystania z usług IIS sesje programu Sticky są włączane przy użyciu routingu żądań aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d93f3-149">When using IIS, sticky sessions are enabled with Application Request Routing.</span></span> <span data-ttu-id="d93f3-150">Aby uzyskać więcej informacji, zobacz [równoważenie obciążenia HTTP przy użyciu routingu żądań aplikacji](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing).</span><span class="sxs-lookup"><span data-stu-id="d93f3-150">For more information, see [HTTP Load Balancing using Application Request Routing](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing).</span></span>

#### <a name="kubernetes"></a><span data-ttu-id="d93f3-151">Kubernetes</span><span class="sxs-lookup"><span data-stu-id="d93f3-151">Kubernetes</span></span>

<span data-ttu-id="d93f3-152">Utwórz definicję transferu danych przychodzących z następującymi [adnotacjami Kubernetes dla sesji programu Sticky Notes](https://kubernetes.github.io/ingress-nginx/examples/affinity/cookie/):</span><span class="sxs-lookup"><span data-stu-id="d93f3-152">Create an ingress definition with the following [Kubernetes annotations for sticky sessions](https://kubernetes.github.io/ingress-nginx/examples/affinity/cookie/):</span></span>

```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: <ingress-name>
  annotations:
    nginx.ingress.kubernetes.io/affinity: "cookie"
    nginx.ingress.kubernetes.io/session-cookie-name: "affinity"
    nginx.ingress.kubernetes.io/session-cookie-expires: "14400"
    nginx.ingress.kubernetes.io/session-cookie-max-age: "14400"
```

#### <a name="linux-with-nginx"></a><span data-ttu-id="d93f3-153">System Linux z serwerem Nginx</span><span class="sxs-lookup"><span data-stu-id="d93f3-153">Linux with Nginx</span></span>

<span data-ttu-id="d93f3-154">Aby SignalR obiekty WebSockets działały prawidłowo, należy ustawić `Upgrade` i `Connection` serwera proxy w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="d93f3-154">For SignalR WebSockets to function properly, set the proxy's `Upgrade` and `Connection` headers to the following:</span></span>

```
proxy_set_header Upgrade $http_upgrade;
proxy_set_header Connection $connection_upgrade;
```

<span data-ttu-id="d93f3-155">Aby uzyskać więcej informacji, zobacz [Nginx jako proxy protokołu WebSocket](https://www.nginx.com/blog/websocket-nginx/).</span><span class="sxs-lookup"><span data-stu-id="d93f3-155">For more information, see [NGINX as a WebSocket Proxy](https://www.nginx.com/blog/websocket-nginx/).</span></span>

### <a name="measure-network-latency"></a><span data-ttu-id="d93f3-156">Mierzenie opóźnienia sieci</span><span class="sxs-lookup"><span data-stu-id="d93f3-156">Measure network latency</span></span>

<span data-ttu-id="d93f3-157">Przy użyciu kodu [js Interop](xref:blazor/javascript-interop) można mierzyć opóźnienia sieci, jak pokazano w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="d93f3-157">[JS interop](xref:blazor/javascript-interop) can be used to measure network latency, as the following example demonstrates:</span></span>

```razor
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

<span data-ttu-id="d93f3-158">W celu uzyskania odpowiedniego środowiska interfejsu użytkownika zalecamy opóźnienia interfejsu użytkownika 250ms lub mniej.</span><span class="sxs-lookup"><span data-stu-id="d93f3-158">For a reasonable UI experience, we recommend a sustained UI latency of 250ms or less.</span></span>
