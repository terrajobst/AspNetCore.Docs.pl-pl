---
title: ASP.NET Core Blazor konfiguracji modelu hostingu
author: guardrex
description: Dowiedz się więcej o Blazor konfiguracji modelu hostingu, w tym o sposobie integrowania składników Razor z aplikacjami Razor Pages i MVC.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/12/2020
no-loc:
- Blazor
- SignalR
uid: blazor/hosting-model-configuration
ms.openlocfilehash: bd44643877e45c5b48b0972bcc2f637fbc5d98f2
ms.sourcegitcommit: 6645435fc8f5092fc7e923742e85592b56e37ada
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/19/2020
ms.locfileid: "77447038"
---
# <a name="aspnet-core-blazor-hosting-model-configuration"></a><span data-ttu-id="a267a-103">Konfiguracja modelu hostingu ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="a267a-103">ASP.NET Core Blazor hosting model configuration</span></span>

<span data-ttu-id="a267a-104">Autor [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="a267a-104">By [Daniel Roth](https://github.com/danroth27)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="a267a-105">W tym artykule opisano hostowanie konfiguracji modelu.</span><span class="sxs-lookup"><span data-stu-id="a267a-105">This article covers hosting model configuration.</span></span>

<!-- For future use:

## Blazor WebAssembly

-->

## <a name="blazor-server"></a><span data-ttu-id="a267a-106">Serwer Blazor</span><span class="sxs-lookup"><span data-stu-id="a267a-106">Blazor Server</span></span>

### <a name="reflect-the-connection-state-in-the-ui"></a><span data-ttu-id="a267a-107">Odzwierciedlanie stanu połączenia w interfejsie użytkownika</span><span class="sxs-lookup"><span data-stu-id="a267a-107">Reflect the connection state in the UI</span></span>

<span data-ttu-id="a267a-108">Gdy klient wykryje, że połączenie zostało utracone, do użytkownika jest wyświetlany domyślny interfejs użytkownika, podczas gdy klient próbuje ponownie nawiązać połączenie.</span><span class="sxs-lookup"><span data-stu-id="a267a-108">When the client detects that the connection has been lost, a default UI is displayed to the user while the client attempts to reconnect.</span></span> <span data-ttu-id="a267a-109">Jeśli ponowne połączenie nie powiedzie się, użytkownik otrzymuje opcję ponowienia próby.</span><span class="sxs-lookup"><span data-stu-id="a267a-109">If reconnection fails, the user is provided the option to retry.</span></span>

<span data-ttu-id="a267a-110">Aby dostosować interfejs użytkownika, zdefiniuj element z `id` `components-reconnect-modal` w `<body>` na stronie *_Host. cshtml* Razor:</span><span class="sxs-lookup"><span data-stu-id="a267a-110">To customize the UI, define an element with an `id` of `components-reconnect-modal` in the `<body>` of the *_Host.cshtml* Razor page:</span></span>

```cshtml
<div id="components-reconnect-modal">
    ...
</div>
```

<span data-ttu-id="a267a-111">W poniższej tabeli opisano klasy CSS stosowane do elementu `components-reconnect-modal`.</span><span class="sxs-lookup"><span data-stu-id="a267a-111">The following table describes the CSS classes applied to the `components-reconnect-modal` element.</span></span>

| <span data-ttu-id="a267a-112">Klasa CSS</span><span class="sxs-lookup"><span data-stu-id="a267a-112">CSS class</span></span>                       | <span data-ttu-id="a267a-113">Wskazuje&hellip;</span><span class="sxs-lookup"><span data-stu-id="a267a-113">Indicates&hellip;</span></span> |
| ------------------------------- | ----------------- |
| `components-reconnect-show`     | <span data-ttu-id="a267a-114">Utracono połączenie.</span><span class="sxs-lookup"><span data-stu-id="a267a-114">A lost connection.</span></span> <span data-ttu-id="a267a-115">Klient próbuje ponownie nawiązać połączenie.</span><span class="sxs-lookup"><span data-stu-id="a267a-115">The client is attempting to reconnect.</span></span> <span data-ttu-id="a267a-116">Pokaż modalne.</span><span class="sxs-lookup"><span data-stu-id="a267a-116">Show the modal.</span></span> |
| `components-reconnect-hide`     | <span data-ttu-id="a267a-117">Aktywne połączenie zostanie ponownie nawiązane z serwerem.</span><span class="sxs-lookup"><span data-stu-id="a267a-117">An active connection is re-established to the server.</span></span> <span data-ttu-id="a267a-118">Ukryj modalne.</span><span class="sxs-lookup"><span data-stu-id="a267a-118">Hide the modal.</span></span> |
| `components-reconnect-failed`   | <span data-ttu-id="a267a-119">Ponowne połączenie nie powiodło się, prawdopodobnie z powodu błędu sieci.</span><span class="sxs-lookup"><span data-stu-id="a267a-119">Reconnection failed, probably due to a network failure.</span></span> <span data-ttu-id="a267a-120">Aby spróbować ponownie nawiązać połączenie, wywołaj `window.Blazor.reconnect()`.</span><span class="sxs-lookup"><span data-stu-id="a267a-120">To attempt reconnection, call `window.Blazor.reconnect()`.</span></span> |
| `components-reconnect-rejected` | <span data-ttu-id="a267a-121">Odrzucono ponowne połączenie.</span><span class="sxs-lookup"><span data-stu-id="a267a-121">Reconnection rejected.</span></span> <span data-ttu-id="a267a-122">Serwer został osiągnięty, ale odmówił połączenia, a stan użytkownika na serwerze został utracony.</span><span class="sxs-lookup"><span data-stu-id="a267a-122">The server was reached but refused the connection, and the user's state on the server is lost.</span></span> <span data-ttu-id="a267a-123">Aby ponownie załadować aplikację, wywołaj `location.reload()`.</span><span class="sxs-lookup"><span data-stu-id="a267a-123">To reload the app, call `location.reload()`.</span></span> <span data-ttu-id="a267a-124">Ten stan połączenia może skutkować tym, że:</span><span class="sxs-lookup"><span data-stu-id="a267a-124">This connection state may result when:</span></span><ul><li><span data-ttu-id="a267a-125">Wystąpił awaria w obwodzie po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="a267a-125">A crash in the server-side circuit occurs.</span></span></li><li><span data-ttu-id="a267a-126">Klient jest odłączony wystarczająco długo, aby serwer mógł porzucić stan użytkownika.</span><span class="sxs-lookup"><span data-stu-id="a267a-126">The client is disconnected long enough for the server to drop the user's state.</span></span> <span data-ttu-id="a267a-127">Wystąpienia składników, z którymi łączy się użytkownik, są usuwane.</span><span class="sxs-lookup"><span data-stu-id="a267a-127">Instances of the components that the user is interacting with are disposed.</span></span></li><li><span data-ttu-id="a267a-128">Serwer zostanie uruchomiony ponownie lub proces roboczy aplikacji zostanie odtworzony.</span><span class="sxs-lookup"><span data-stu-id="a267a-128">The server is restarted, or the app's worker process is recycled.</span></span></li></ul> |

### <a name="render-mode"></a><span data-ttu-id="a267a-129">Tryb renderowania</span><span class="sxs-lookup"><span data-stu-id="a267a-129">Render mode</span></span>

<span data-ttu-id="a267a-130">Aplikacje serwera Blazor są domyślnie skonfigurowane, aby skonfigurować interfejs użytkownika na serwerze przed nawiązaniem połączenia z serwerem.</span><span class="sxs-lookup"><span data-stu-id="a267a-130">Blazor Server apps are set up by default to prerender the UI on the server before the client connection to the server is established.</span></span> <span data-ttu-id="a267a-131">Ta konfiguracja jest ustawiana na stronie *_Host. cshtml* Razor:</span><span class="sxs-lookup"><span data-stu-id="a267a-131">This is set up in the *_Host.cshtml* Razor page:</span></span>

```cshtml
<body>
    <app>
      <component type="typeof(App)" render-mode="ServerPrerendered" />
    </app>

    <script src="_framework/blazor.server.js"></script>
</body>
```

<span data-ttu-id="a267a-132">`RenderMode` określa, czy składnik:</span><span class="sxs-lookup"><span data-stu-id="a267a-132">`RenderMode` configures whether the component:</span></span>

* <span data-ttu-id="a267a-133">Jest wstępnie renderowany na stronie.</span><span class="sxs-lookup"><span data-stu-id="a267a-133">Is prerendered into the page.</span></span>
* <span data-ttu-id="a267a-134">Jest renderowany jako statyczny kod HTML na stronie lub zawiera informacje niezbędne do uruchomienia aplikacji Blazor z poziomu agenta użytkownika.</span><span class="sxs-lookup"><span data-stu-id="a267a-134">Is rendered as static HTML on the page or if it includes the necessary information to bootstrap a Blazor app from the user agent.</span></span>

| `RenderMode`        | <span data-ttu-id="a267a-135">Opis</span><span class="sxs-lookup"><span data-stu-id="a267a-135">Description</span></span> |
| ------------------- | ----------- |
| `ServerPrerendered` | <span data-ttu-id="a267a-136">Renderuje składnik do statycznego kodu HTML i zawiera znacznik dla aplikacji serwera Blazor.</span><span class="sxs-lookup"><span data-stu-id="a267a-136">Renders the component into static HTML and includes a marker for a Blazor Server app.</span></span> <span data-ttu-id="a267a-137">Po uruchomieniu agenta użytkownika ten znacznik jest używany do uruchamiania aplikacji Blazor.</span><span class="sxs-lookup"><span data-stu-id="a267a-137">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Server`            | <span data-ttu-id="a267a-138">Renderuje znacznik dla aplikacji serwera Blazor.</span><span class="sxs-lookup"><span data-stu-id="a267a-138">Renders a marker for a Blazor Server app.</span></span> <span data-ttu-id="a267a-139">Dane wyjściowe ze składnika nie są uwzględniane.</span><span class="sxs-lookup"><span data-stu-id="a267a-139">Output from the component isn't included.</span></span> <span data-ttu-id="a267a-140">Po uruchomieniu agenta użytkownika ten znacznik jest używany do uruchamiania aplikacji Blazor.</span><span class="sxs-lookup"><span data-stu-id="a267a-140">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Static`            | <span data-ttu-id="a267a-141">Renderuje składnik do statycznego kodu HTML.</span><span class="sxs-lookup"><span data-stu-id="a267a-141">Renders the component into static HTML.</span></span> |

<span data-ttu-id="a267a-142">Renderowanie składników serwera ze statyczną stroną HTML nie jest obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="a267a-142">Rendering server components from a static HTML page isn't supported.</span></span>

### <a name="render-stateful-interactive-components-from-razor-pages-and-views"></a><span data-ttu-id="a267a-143">Renderuj stanowe składniki interaktywne ze stron Razor i widoków</span><span class="sxs-lookup"><span data-stu-id="a267a-143">Render stateful interactive components from Razor pages and views</span></span>

<span data-ttu-id="a267a-144">Można dodać składniki interaktywne ze stanem do strony lub widoku Razor.</span><span class="sxs-lookup"><span data-stu-id="a267a-144">Stateful interactive components can be added to a Razor page or view.</span></span>

<span data-ttu-id="a267a-145">Gdy renderuje stronę lub widok:</span><span class="sxs-lookup"><span data-stu-id="a267a-145">When the page or view renders:</span></span>

* <span data-ttu-id="a267a-146">Składnik jest wstępnie renderowany przy użyciu strony lub widoku.</span><span class="sxs-lookup"><span data-stu-id="a267a-146">The component is prerendered with the page or view.</span></span>
* <span data-ttu-id="a267a-147">Początkowy stan składnika używany na potrzeby renderowania wstępnego został utracony.</span><span class="sxs-lookup"><span data-stu-id="a267a-147">The initial component state used for prerendering is lost.</span></span>
* <span data-ttu-id="a267a-148">Nowy stan składnika jest tworzony po nawiązaniu połączenia SignalR.</span><span class="sxs-lookup"><span data-stu-id="a267a-148">New component state is created when the SignalR connection is established.</span></span>

<span data-ttu-id="a267a-149">Następująca strona Razor renderuje składnik `Counter`:</span><span class="sxs-lookup"><span data-stu-id="a267a-149">The following Razor page renders a `Counter` component:</span></span>

```cshtml
<h1>My Razor Page</h1>

<component type="typeof(Counter)" render-mode="ServerPrerendered" 
    param-InitialValue="InitialValue" />

@code {
    [BindProperty(SupportsGet=true)]
    public int InitialValue { get; set; }
}
```

### <a name="render-noninteractive-components-from-razor-pages-and-views"></a><span data-ttu-id="a267a-150">Renderuj nieinteraktywne składniki ze stron Razor i widoków</span><span class="sxs-lookup"><span data-stu-id="a267a-150">Render noninteractive components from Razor pages and views</span></span>

<span data-ttu-id="a267a-151">Na poniższej stronie Razor składnik `Counter` jest renderowany statycznie z wartością początkową określoną przy użyciu formularza:</span><span class="sxs-lookup"><span data-stu-id="a267a-151">In the following Razor page, the `Counter` component is statically rendered with an initial value that's specified using a form:</span></span>

```cshtml
<h1>My Razor Page</h1>

<form>
    <input type="number" asp-for="InitialValue" />
    <button type="submit">Set initial value</button>
</form>

<component type="typeof(Counter)" render-mode="Static" 
    param-InitialValue="InitialValue" />

@code {
    [BindProperty(SupportsGet=true)]
    public int InitialValue { get; set; }
}
```

<span data-ttu-id="a267a-152">Ponieważ `MyComponent` jest renderowany statycznie, składnik nie może być interaktywny.</span><span class="sxs-lookup"><span data-stu-id="a267a-152">Since `MyComponent` is statically rendered, the component can't be interactive.</span></span>

### <a name="configure-the-opno-locsignalr-client-for-opno-locblazor-server-apps"></a><span data-ttu-id="a267a-153">Konfigurowanie klienta SignalR dla aplikacji Blazor Server</span><span class="sxs-lookup"><span data-stu-id="a267a-153">Configure the SignalR client for Blazor Server apps</span></span>

<span data-ttu-id="a267a-154">Czasami trzeba skonfigurować klienta SignalR używanego przez aplikacje Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="a267a-154">Sometimes, you need to configure the SignalR client used by Blazor Server apps.</span></span> <span data-ttu-id="a267a-155">Na przykład możesz chcieć skonfigurować rejestrowanie na kliencie SignalR, aby zdiagnozować problem z połączeniem.</span><span class="sxs-lookup"><span data-stu-id="a267a-155">For example, you might want to configure logging on the SignalR client to diagnose a connection issue.</span></span>

<span data-ttu-id="a267a-156">Aby skonfigurować klienta SignalR w pliku *Pages/_Host. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="a267a-156">To configure the SignalR client in the *Pages/_Host.cshtml* file:</span></span>

* <span data-ttu-id="a267a-157">Dodaj atrybut `autostart="false"` do tagu `<script>` dla skryptu `blazor.server.js`.</span><span class="sxs-lookup"><span data-stu-id="a267a-157">Add an `autostart="false"` attribute to the `<script>` tag for the `blazor.server.js` script.</span></span>
* <span data-ttu-id="a267a-158">Wywołaj `Blazor.start` i przekaż obiekt konfiguracji, który określa SignalR konstruktora.</span><span class="sxs-lookup"><span data-stu-id="a267a-158">Call `Blazor.start` and pass in a configuration object that specifies the SignalR builder.</span></span>

```html
<script src="_framework/blazor.server.js" autostart="false"></script>
<script>
  Blazor.start({
    configureSignalR: function (builder) {
      builder.configureLogging("information"); // LogLevel.Information
    }
  });
</script>
```
