---
title: Co nowego w ASP.NET Core 3,1
author: rick-anderson
description: Dowiedz się więcej o nowych funkcjach w ASP.NET Core 3,1.
ms.author: riande
ms.custom: mvc
ms.date: 12/05/2019
no-loc:
- Blazor
- SignalR
uid: aspnetcore-3.1
ms.openlocfilehash: 5eaf14f3b9c5a5b2b83e469c4dc8119b5fc341c1
ms.sourcegitcommit: c0b72b344dadea835b0e7943c52463f13ab98dd1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/06/2019
ms.locfileid: "74880817"
---
# <a name="whats-new-in-aspnet-core-31"></a><span data-ttu-id="143f0-103">Co nowego w ASP.NET Core 3,1</span><span class="sxs-lookup"><span data-stu-id="143f0-103">What's new in ASP.NET Core 3.1</span></span>

<span data-ttu-id="143f0-104">W tym artykule przedstawiono najbardziej znaczące zmiany w ASP.NET Core 3,1 z linkami do odpowiedniej dokumentacji.</span><span class="sxs-lookup"><span data-stu-id="143f0-104">This article highlights the most significant changes in ASP.NET Core 3.1 with links to relevant documentation.</span></span>

## <a name="partial-class-support-for-razor-components"></a><span data-ttu-id="143f0-105">Obsługa częściowej klasy dla składników Razor</span><span class="sxs-lookup"><span data-stu-id="143f0-105">Partial class support for Razor components</span></span>

<span data-ttu-id="143f0-106">Składniki Razor są teraz generowane jako klasy częściowe.</span><span class="sxs-lookup"><span data-stu-id="143f0-106">Razor components are now generated as partial classes.</span></span> <span data-ttu-id="143f0-107">Kod dla składnika Razor można napisać przy użyciu pliku związanego z kodem zdefiniowanego jako Klasa częściowa zamiast definiować cały kod dla składnika w pojedynczym pliku.</span><span class="sxs-lookup"><span data-stu-id="143f0-107">Code for a Razor component can be written using a code-behind file defined as a partial class rather than defining all the code for the component in a single file.</span></span> <span data-ttu-id="143f0-108">Aby uzyskać więcej informacji, zobacz temat [Obsługa klasy częściowej](xref:blazor/components#partial-class-support).</span><span class="sxs-lookup"><span data-stu-id="143f0-108">For more information, see [Partial class support](xref:blazor/components#partial-class-support).</span></span>

## <a name="opno-locblazor-component-tag-helper-and-pass-parameters-to-top-level-components"></a>Blazor<span data-ttu-id="143f0-109"> pomocnika tagów składników i przekazywania parametrów do składników najwyższego poziomu</span><span class="sxs-lookup"><span data-stu-id="143f0-109"> Component Tag Helper and pass parameters to top-level components</span></span>

<span data-ttu-id="143f0-110">W Blazor z ASP.NET Core 3,0 składniki były renderowane na stronach i w widokach za pomocą pomocnika HTML (`Html.RenderComponentAsync`).</span><span class="sxs-lookup"><span data-stu-id="143f0-110">In Blazor with ASP.NET Core 3.0, components were rendered into pages and views using an HTML Helper (`Html.RenderComponentAsync`).</span></span> <span data-ttu-id="143f0-111">W ASP.NET Core 3,1 Renderuj składnik ze strony lub widoku przy użyciu nowego pomocnika tagów składnika:</span><span class="sxs-lookup"><span data-stu-id="143f0-111">In ASP.NET Core 3.1, render a component from a page or view with the new Component Tag Helper:</span></span>

```razor
<component type="typeof(Counter)" render-mode="ServerPrerendered" />
```

<span data-ttu-id="143f0-112">Pomocnik HTML pozostaje obsługiwany w ASP.NET Core 3,1, ale zaleca się pomocnika tagów składnika.</span><span class="sxs-lookup"><span data-stu-id="143f0-112">The HTML Helper remains supported in ASP.NET Core 3.1, but the Component Tag Helper is recommended.</span></span>

<span data-ttu-id="143f0-113">aplikacje serwera Blazor umożliwiają teraz przekazywanie parametrów do składników najwyższego poziomu podczas początkowego renderowania.</span><span class="sxs-lookup"><span data-stu-id="143f0-113">Blazor Server apps can now pass parameters to top-level components during the initial render.</span></span> <span data-ttu-id="143f0-114">Wcześniej można było przekazać parametry do składnika najwyższego poziomu za pomocą elementu [RenderMode. static](xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Static).</span><span class="sxs-lookup"><span data-stu-id="143f0-114">Previously you could only pass parameters to a top-level component with [RenderMode.Static](xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Static).</span></span> <span data-ttu-id="143f0-115">W tej wersji obsługiwane są zarówno metody [RenderMode. Server](xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Server) , jak i [RenderModel. ServerPrerendered](xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.ServerPrerendered) .</span><span class="sxs-lookup"><span data-stu-id="143f0-115">With this release, both [RenderMode.Server](xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Server) and [RenderModel.ServerPrerendered](xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.ServerPrerendered) are supported.</span></span> <span data-ttu-id="143f0-116">Wszystkie określone wartości parametrów są serializowane jako kod JSON i zawarte w początkowej odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="143f0-116">Any specified parameter values are serialized as JSON and included in the initial response.</span></span>

<span data-ttu-id="143f0-117">Na przykład wyprerender składnik `Counter` z ilością przyrostu (`IncrementAmount`):</span><span class="sxs-lookup"><span data-stu-id="143f0-117">For example, prerender a `Counter` component with an increment amount (`IncrementAmount`):</span></span>

```razor
<component type="typeof(Counter)" render-mode="ServerPrerendered" 
    param-IncrementAmount="10" />
```

<span data-ttu-id="143f0-118">Aby uzyskać więcej informacji, zobacz [integrowanie składników w aplikacjach Razor Pages i MVC](xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps).</span><span class="sxs-lookup"><span data-stu-id="143f0-118">For more information, see [Integrate components into Razor Pages and MVC apps](xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps).</span></span>

## <a name="support-for-shared-queues-in-httpsys"></a><span data-ttu-id="143f0-119">Obsługa kolejek udostępnionych w pliku HTTP. sys</span><span class="sxs-lookup"><span data-stu-id="143f0-119">Support for shared queues in HTTP.sys</span></span>

<span data-ttu-id="143f0-120">[Http. sys](xref:fundamentals/servers/httpsys) obsługuje tworzenie anonimowych kolejek żądań.</span><span class="sxs-lookup"><span data-stu-id="143f0-120">[HTTP.sys](xref:fundamentals/servers/httpsys) supports creating anonymous request queues.</span></span> <span data-ttu-id="143f0-121">W ASP.NET Core 3,1 dodaliśmy do możliwości tworzenia lub dołączania istniejącej nazwanej kolejki żądań HTTP. sys.</span><span class="sxs-lookup"><span data-stu-id="143f0-121">In ASP.NET Core 3.1, we've added to ability to create or attach to an existing named HTTP.sys request queue.</span></span> <span data-ttu-id="143f0-122">Utworzenie lub dołączenie istniejącej kolejki żądań HTTP. sys umożliwia scenariusze, w których proces kontrolera HTTP. sys, który jest właścicielem kolejki, jest niezależny od procesu odbiornika.</span><span class="sxs-lookup"><span data-stu-id="143f0-122">Creating or attaching to an existing named HTTP.sys request queue enables scenarios where the HTTP.sys controller process that owns the queue is independent of the listener process.</span></span> <span data-ttu-id="143f0-123">Ta niezależność umożliwia zachowywanie istniejących połączeń i zakolejce żądań między ponownymi uruchomieniami procesu odbiornika:</span><span class="sxs-lookup"><span data-stu-id="143f0-123">This independence makes it possible to preserve existing connections and enqueued requests between listener process restarts:</span></span>

[!code-csharp[](sample/Program.cs?name=snippet)]

## <a name="breaking-changes-for-samesite-cookies"></a><span data-ttu-id="143f0-124">Istotne zmiany plików cookie SameSite</span><span class="sxs-lookup"><span data-stu-id="143f0-124">Breaking changes for SameSite cookies</span></span>

<span data-ttu-id="143f0-125">Zachowanie plików cookie SameSite zostało zmienione w celu odzwierciedlenia przyszłych zmian przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="143f0-125">The behavior of SameSite cookies has changed to reflect upcoming browser changes.</span></span> <span data-ttu-id="143f0-126">Może to mieć wpływ na scenariusze uwierzytelniania, takie jak AzureAd, OpenIdConnect lub WsFederation.</span><span class="sxs-lookup"><span data-stu-id="143f0-126">This may affect authentication scenarios like AzureAd, OpenIdConnect, or WsFederation.</span></span> <span data-ttu-id="143f0-127">Aby uzyskać więcej informacji, zobacz temat <xref:security/samesite>.</span><span class="sxs-lookup"><span data-stu-id="143f0-127">For more information, see <xref:security/samesite>.</span></span>

## <a name="prevent-default-actions-for-events-in-opno-locblazor-apps"></a><span data-ttu-id="143f0-128">Zapobiegaj domyślnym akcjom dla zdarzeń w aplikacjach Blazor</span><span class="sxs-lookup"><span data-stu-id="143f0-128">Prevent default actions for events in Blazor apps</span></span>

<span data-ttu-id="143f0-129">Użyj atrybutu dyrektywy `@on{EVENT}:preventDefault`, aby zapobiec domyślnej akcji dla zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="143f0-129">Use the `@on{EVENT}:preventDefault` directive attribute to prevent the default action for an event.</span></span> <span data-ttu-id="143f0-130">W poniższym przykładzie jest blokowane domyślne działanie wyświetlania znaku klucza w polu tekstowym:</span><span class="sxs-lookup"><span data-stu-id="143f0-130">In the following example, the default action of displaying the key's character in the text box is prevented:</span></span>

```razor
<input value="@_count" @onkeypress="KeyHandler" @onkeypress:preventDefault />
```

<span data-ttu-id="143f0-131">Aby uzyskać więcej informacji, zobacz [zapobieganie domyślnym akcjom](xref:blazor/components#prevent-default-actions).</span><span class="sxs-lookup"><span data-stu-id="143f0-131">For more information, see [Prevent default actions](xref:blazor/components#prevent-default-actions).</span></span>

## <a name="stop-event-propagation-in-opno-locblazor-apps"></a><span data-ttu-id="143f0-132">Zatrzymaj propagację zdarzeń w aplikacjach Blazor</span><span class="sxs-lookup"><span data-stu-id="143f0-132">Stop event propagation in Blazor apps</span></span>

<span data-ttu-id="143f0-133">Użyj atrybutu dyrektywy `@on{EVENT}:stopPropagation`, aby zatrzymać propagację zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="143f0-133">Use the `@on{EVENT}:stopPropagation` directive attribute to stop event propagation.</span></span> <span data-ttu-id="143f0-134">W poniższym przykładzie, zaznaczając pole wyboru, Zapobiegaj kliknięciu zdarzeń z elementu podrzędnego `<div>` od propagowania do `<div>`nadrzędnego:</span><span class="sxs-lookup"><span data-stu-id="143f0-134">In the following example, selecting the check box prevents click events from the child `<div>` from propagating to the parent `<div>`:</span></span>

```razor
<input @bind="_stopPropagation" type="checkbox" />

<div @onclick="OnSelectParentDiv">
    <div @onclick="OnSelectChildDiv" @onclick:stopPropagation="_stopPropagation">
        ...
    </div>
</div>

@code {
    private bool _stopPropagation = false;
}
```

<span data-ttu-id="143f0-135">Aby uzyskać więcej informacji, zobacz sekcję [Zatrzymaj propagację zdarzeń](xref:blazor/components#stop-event-propagation).</span><span class="sxs-lookup"><span data-stu-id="143f0-135">For more information, see [Stop event propagation](xref:blazor/components#stop-event-propagation).</span></span>

## <a name="detailed-errors-during-opno-locblazor-app-development"></a><span data-ttu-id="143f0-136">Szczegóły błędów podczas tworzenia aplikacji Blazor</span><span class="sxs-lookup"><span data-stu-id="143f0-136">Detailed errors during Blazor app development</span></span>

<span data-ttu-id="143f0-137">Gdy aplikacja Blazor nie działa prawidłowo podczas opracowywania, otrzymywanie szczegółowych informacji o błędach z aplikacji pomaga w rozwiązywaniu problemów i rozwiązaniu problemu.</span><span class="sxs-lookup"><span data-stu-id="143f0-137">When a Blazor app isn't functioning properly during development, receiving detailed error information from the app assists in troubleshooting and fixing the issue.</span></span> <span data-ttu-id="143f0-138">Gdy wystąpi błąd, Blazor aplikacje wyświetlają złoty pasek u dołu ekranu:</span><span class="sxs-lookup"><span data-stu-id="143f0-138">When an error occurs, Blazor apps display a gold bar at the bottom of the screen:</span></span>

* <span data-ttu-id="143f0-139">W trakcie programowania złoty pasek kieruje użytkownika do konsoli przeglądarki, gdzie można zobaczyć wyjątek.</span><span class="sxs-lookup"><span data-stu-id="143f0-139">During development, the gold bar directs you to the browser console, where you can see the exception.</span></span>
* <span data-ttu-id="143f0-140">W środowisku produkcyjnym złoty pasek powiadamia użytkownika o wystąpieniu błędu i zaleca odświeżenie przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="143f0-140">In production, the gold bar notifies the user that an error has occurred and recommends refreshing the browser.</span></span>

<span data-ttu-id="143f0-141">Aby uzyskać więcej informacji, zobacz [szczegóły błędów podczas opracowywania](xref:blazor/handle-errors#detailed-errors-during-development).</span><span class="sxs-lookup"><span data-stu-id="143f0-141">For more information, see [Detailed errors during development](xref:blazor/handle-errors#detailed-errors-during-development).</span></span>
