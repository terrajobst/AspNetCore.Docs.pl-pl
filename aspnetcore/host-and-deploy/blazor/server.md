---
title: Hostowanie i wdrażanie ASP.NET Core Blazor Server
author: guardrex
description: Dowiedz się, jak hostować i wdrażać aplikację Blazor Server przy użyciu ASP.NET Core.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/18/2019
no-loc:
- Blazor
- SignalR
uid: host-and-deploy/blazor/server
ms.openlocfilehash: c07cd05dd8e1c4384c6f8f019173b9b7a9a06fd0
ms.sourcegitcommit: 9ee99300a48c810ca6fd4f7700cd95c3ccb85972
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/17/2020
ms.locfileid: "76160226"
---
# <a name="host-and-deploy-opno-locblazor-server"></a>Hostowanie i wdrażanie serwera Blazor

[Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com)i [Daniel Roth](https://github.com/danroth27)

## <a name="host-configuration-values"></a>Wartości konfiguracji hosta

[aplikacje serweraBlazor](xref:blazor/hosting-models#blazor-server) mogą akceptować [ogólne wartości konfiguracji hosta](xref:fundamentals/host/generic-host#host-configuration).

## <a name="deployment"></a>Wdrażanie

Korzystając z [modelu hostinguBlazor Server](xref:blazor/hosting-models#blazor-server), Blazor jest wykonywane na serwerze z poziomu aplikacji ASP.NET Core. Aktualizacje interfejsu użytkownika, obsługa zdarzeń i wywołania języka JavaScript są obsługiwane przez połączenie [SignalR](xref:signalr/introduction) .

Wymagany jest serwer sieci Web obsługujący aplikację ASP.NET Core. Program Visual Studio zawiera szablon projektu **aplikacjiBlazor Server** (szablon`blazorserverside` przy użyciu polecenia [dotnet New](/dotnet/core/tools/dotnet-new) ).

## <a name="scalability"></a>Skalowalność

Zaplanuj wdrożenie, aby najlepiej wykorzystać dostępną infrastrukturę dla aplikacji serwera Blazor. Zapoznaj się z poniższymi zasobami, aby rozwiązać Blazor skalowalności aplikacji serwerowej:

* [Podstawowe informacje o aplikacjach serwerowych Blazor](xref:blazor/hosting-models#blazor-server)
* <xref:security/blazor/server>

### <a name="deployment-server"></a>Serwer wdrażania

Gdy rozważasz skalowalność pojedynczego serwera (skalowanie w górę), pamięć dostępna dla aplikacji jest prawdopodobnie pierwszym zasobem, który zostanie wyczerpany przez aplikację w miarę wzrostu wymagań użytkownika. Dostępna pamięć na serwerze ma wpływ na:

* Liczba aktywnych obwodów obsługiwanych przez serwer.
* Opóźnienie interfejsu użytkownika na kliencie.

Aby uzyskać wskazówki dotyczące tworzenia bezpiecznych i skalowalnych aplikacji serwera Blazor, zobacz <xref:security/blazor/server>.

Każdy obwód wykorzystuje około 250 KB pamięci w przypadku aplikacji o minimalnej *Hello World*. Rozmiar obwodu zależy od kodu aplikacji i wymagań dotyczących konserwacji stanu związanych z poszczególnymi składnikami. Zalecamy mierzenie wymagań dotyczących zasobów podczas opracowywania aplikacji i infrastruktury, ale następujący punkt odniesienia może być punktem początkowym w planowaniu celu wdrożenia: jeśli oczekujesz, że aplikacja będzie obsługiwać 5 000 współbieżnych użytkowników, rozważ budżetowanie o najmniej 1,3 GB pamięci serwera do aplikacji (lub ~ 273 KB na użytkownika).

### <a name="opno-locsignalr-configuration"></a>Konfiguracja SignalR

aplikacje serwera Blazor używają ASP.NET Core SignalR do komunikowania się z przeglądarką. [warunki hostingu i skalowaniaSignalR](xref:signalr/publish-to-azure-web-app) mają zastosowanie do aplikacji Blazor Server.

Blazor najlepiej sprawdza się w przypadku korzystania z usługi WebSockets jako transportu SignalR ze względu na mniejsze opóźnienia, niezawodność i [bezpieczeństwo](xref:signalr/security). Długie sondowanie jest używane przez SignalR, gdy obiekty WebSockets nie są dostępne lub gdy aplikacja jest jawnie skonfigurowana do korzystania z długotrwałego sondowania. Podczas wdrażania programu w celu Azure App Service Skonfiguruj aplikację do używania obiektów WebSockets w ustawieniach Azure Portal dla usługi. Aby uzyskać szczegółowe informacje dotyczące konfigurowania aplikacji na potrzeby Azure App Service, zapoznaj się z tematem [SignalR wskazówki dotyczące publikowania](xref:signalr/publish-to-azure-web-app).

#### <a name="azure-opno-locsignalr-service"></a>Usługa SignalR platformy Azure

Zalecamy korzystanie z [usługi Azure SignalR](/azure/azure-signalr) dla aplikacji Blazor Server. Usługa umożliwia skalowanie aplikacji serwera Blazor do dużej liczby jednoczesnych połączeń SignalR. Ponadto globalne zasięgi i wysokiej wydajności centrów danych usługi SignalR znacznie ułatwiają zredukowanie opóźnień ze względu na lokalizację geograficzną. Aby skonfigurować aplikację (i opcjonalnie zainicjować obsługę administracyjną) usługi Azure SignalR:

1. Włącz obsługę *sesji usługi Sticky Notes*, w przypadku których klienci są [przekierowywani z powrotem do tego samego serwera podczas renderowania](xref:blazor/hosting-models#reconnection-to-the-same-server). Ustaw opcję `ServerStickyMode` lub wartość konfiguracji na `Required`. Zazwyczaj aplikacja tworzy konfigurację przy użyciu **jednej** z następujących metod:

   * `Startup.ConfigureServices`:
  
     ```csharp
     services.AddSignalR().AddAzureSignalR(options =>
     {
         options.ServerStickyMode = 
             Microsoft.Azure.SignalR.ServerStickyMode.Required;
     });
     ```

   * Konfiguracja (Użyj **jednej** z następujących metod):
  
     * *appsettings.json*:

       ```json
       "Azure:SignalR:ServerStickyMode": "Required"
       ```

     * **Konfiguracja** usługi app Service > **ustawienia aplikacji** w Azure Portal (**Nazwa**: `Azure:SignalR:ServerStickyMode`, **wartość**: `Required`).

1. Utwórz profil publikowania aplikacji platformy Azure w programie Visual Studio dla aplikacji serwera Blazor.
1. Dodaj zależność **usługi SignalR platformy Azure** do profilu. Jeśli subskrypcja platformy Azure nie ma istniejącego wystąpienia usługi SignalR platformy Azure do przypisania do aplikacji, wybierz pozycję **Utwórz nowe wystąpienie usługi azure SignalR** , aby udostępnić nowe wystąpienie usługi.
1. Publikowanie aplikacji na platformie Azure

#### <a name="iis"></a>IIS

W przypadku korzystania z usług IIS sesje programu Sticky są włączane przy użyciu routingu żądań aplikacji. Aby uzyskać więcej informacji, zobacz [równoważenie obciążenia HTTP przy użyciu routingu żądań aplikacji](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing).

#### <a name="kubernetes"></a>Kubernetes

Utwórz definicję transferu danych przychodzących z następującymi [adnotacjami Kubernetes dla sesji programu Sticky Notes](https://kubernetes.github.io/ingress-nginx/examples/affinity/cookie/):

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

### <a name="measure-network-latency"></a>Mierzenie opóźnienia sieci

Przy użyciu kodu [js Interop](xref:blazor/javascript-interop) można mierzyć opóźnienia sieci, jak pokazano w poniższym przykładzie:

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

W celu uzyskania odpowiedniego środowiska interfejsu użytkownika zalecamy opóźnienia interfejsu użytkownika 250ms lub mniej.
