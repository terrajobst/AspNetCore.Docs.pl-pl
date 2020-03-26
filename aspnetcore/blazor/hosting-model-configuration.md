---
title: ASP.NET Core Blazor konfiguracji modelu hostingu
author: guardrex
description: Dowiedz się więcej o Blazor konfiguracji modelu hostingu, w tym o sposobie integrowania składników Razor z aplikacjami Razor Pages i MVC.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/24/2020
no-loc:
- Blazor
- SignalR
uid: blazor/hosting-model-configuration
ms.openlocfilehash: 1f71ac63bbe9dc9d56cfca2ded19a5b863be828f
ms.sourcegitcommit: 6ffb583991d6689326605a24565130083a28ef85
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/26/2020
ms.locfileid: "80306433"
---
# <a name="aspnet-core-blazor-hosting-model-configuration"></a><span data-ttu-id="6a19d-103">Konfiguracja modelu hostingu ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="6a19d-103">ASP.NET Core Blazor hosting model configuration</span></span>

<span data-ttu-id="6a19d-104">Autor [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="6a19d-104">By [Daniel Roth](https://github.com/danroth27)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="6a19d-105">W tym artykule opisano hostowanie konfiguracji modelu.</span><span class="sxs-lookup"><span data-stu-id="6a19d-105">This article covers hosting model configuration.</span></span>

## <a name="blazor-webassembly"></a><span data-ttu-id="6a19d-106">Zestaw WebAssembly Blazor</span><span class="sxs-lookup"><span data-stu-id="6a19d-106">Blazor WebAssembly</span></span>

<span data-ttu-id="6a19d-107">Począwszy od wersji ASP.NET Core 3,2 wersji zapoznawczej 3, Blazor webassembly obsługuje konfigurację z:</span><span class="sxs-lookup"><span data-stu-id="6a19d-107">As of the ASP.NET Core 3.2 Preview 3 release, Blazor WebAssembly supports configuration from:</span></span>

* <span data-ttu-id="6a19d-108">*wwwroot/appSettings. JSON*</span><span class="sxs-lookup"><span data-stu-id="6a19d-108">*wwwroot/appsettings.json*</span></span>
* <span data-ttu-id="6a19d-109">*wwwroot/appSettings. {ENVIRONMENT}. JSON*</span><span class="sxs-lookup"><span data-stu-id="6a19d-109">*wwwroot/appsettings.{ENVIRONMENT}.json*</span></span>

<span data-ttu-id="6a19d-110">W hostowanej aplikacji Blazor [środowisko uruchomieniowe](xref:fundamentals/environments) jest takie samo jak wartość aplikacji serwera.</span><span class="sxs-lookup"><span data-stu-id="6a19d-110">In a Blazor Hosted app, the [runtime environment](xref:fundamentals/environments) is the same as the server app's value.</span></span>

<span data-ttu-id="6a19d-111">Podczas lokalnego uruchamiania aplikacji środowisko jest domyślnie opracowywane.</span><span class="sxs-lookup"><span data-stu-id="6a19d-111">When running the app locally, the environment defaults to Development.</span></span> <span data-ttu-id="6a19d-112">Gdy aplikacja zostanie opublikowana, środowisko jest domyślne dla środowiska produkcyjnego.</span><span class="sxs-lookup"><span data-stu-id="6a19d-112">When the app is published, the environment defaults to Production.</span></span> <span data-ttu-id="6a19d-113">Aby uzyskać więcej informacji, w tym o sposobie konfigurowania środowiska, zobacz <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="6a19d-113">For more information, including how to configure the environment, see <xref:fundamentals/environments>.</span></span>

> [!WARNING]
> <span data-ttu-id="6a19d-114">Konfiguracja w aplikacji Blazor webassembly jest widoczna dla użytkowników.</span><span class="sxs-lookup"><span data-stu-id="6a19d-114">Configuration in a Blazor WebAssembly app is visible to users.</span></span> <span data-ttu-id="6a19d-115">**Nie przechowuj wpisów tajnych aplikacji ani poświadczeń w konfiguracji.**</span><span class="sxs-lookup"><span data-stu-id="6a19d-115">**Don't store app secrets or credentials in configuration.**</span></span>

<span data-ttu-id="6a19d-116">Pliki konfiguracji są buforowane do użycia w trybie offline.</span><span class="sxs-lookup"><span data-stu-id="6a19d-116">Configuration files are cached for offline use.</span></span> <span data-ttu-id="6a19d-117">Przy użyciu [progresywnych aplikacji sieci Web (PWAs)](xref:blazor/progressive-web-app)można aktualizować tylko pliki konfiguracji podczas tworzenia nowego wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="6a19d-117">With [Progressive Web Applications (PWAs)](xref:blazor/progressive-web-app), you can only update configuration files when creating a new deployment.</span></span> <span data-ttu-id="6a19d-118">Edytowanie plików konfiguracji między wdrożeniami nie ma żadnego skutku, ponieważ:</span><span class="sxs-lookup"><span data-stu-id="6a19d-118">Editing configuration files between deployments has no effect because:</span></span>

* <span data-ttu-id="6a19d-119">Użytkownicy mają buforowane wersje plików, które nadal są używane.</span><span class="sxs-lookup"><span data-stu-id="6a19d-119">Users have cached versions of the files that they continue to use.</span></span>
* <span data-ttu-id="6a19d-120">Pliki *Service-Worker. js* i *Service-Worker-Assets. js* programu PWA muszą zostać ponownie skompilowane w ramach kompilacji, która sygnalizuje aplikacji w następnym trybie online, że aplikacja została ponownie wdrożona.</span><span class="sxs-lookup"><span data-stu-id="6a19d-120">The PWA's *service-worker.js* and *service-worker-assets.js* files must be rebuilt on compilation, which signal to the app on the user's next online visit that the app has been redeployed.</span></span>

<span data-ttu-id="6a19d-121">Aby uzyskać więcej informacji o tym, jak aktualizacje w tle są obsługiwane przez PWAs, zobacz <xref:blazor/progressive-web-app#background-updates>.</span><span class="sxs-lookup"><span data-stu-id="6a19d-121">For more information on how background updates are handled by PWAs, see <xref:blazor/progressive-web-app#background-updates>.</span></span>

## <a name="blazor-server"></a><span data-ttu-id="6a19d-122">Serwer Blazor</span><span class="sxs-lookup"><span data-stu-id="6a19d-122">Blazor Server</span></span>

### <a name="reflect-the-connection-state-in-the-ui"></a><span data-ttu-id="6a19d-123">Odzwierciedlanie stanu połączenia w interfejsie użytkownika</span><span class="sxs-lookup"><span data-stu-id="6a19d-123">Reflect the connection state in the UI</span></span>

<span data-ttu-id="6a19d-124">Gdy klient wykryje, że połączenie zostało utracone, do użytkownika jest wyświetlany domyślny interfejs użytkownika, podczas gdy klient próbuje ponownie nawiązać połączenie.</span><span class="sxs-lookup"><span data-stu-id="6a19d-124">When the client detects that the connection has been lost, a default UI is displayed to the user while the client attempts to reconnect.</span></span> <span data-ttu-id="6a19d-125">Jeśli ponowne połączenie nie powiedzie się, użytkownik otrzymuje opcję ponowienia próby.</span><span class="sxs-lookup"><span data-stu-id="6a19d-125">If reconnection fails, the user is provided the option to retry.</span></span>

<span data-ttu-id="6a19d-126">Aby dostosować interfejs użytkownika, zdefiniuj element z `id` `components-reconnect-modal` w `<body>` na stronie *_Host. cshtml* Razor:</span><span class="sxs-lookup"><span data-stu-id="6a19d-126">To customize the UI, define an element with an `id` of `components-reconnect-modal` in the `<body>` of the *_Host.cshtml* Razor page:</span></span>

```cshtml
<div id="components-reconnect-modal">
    ...
</div>
```

<span data-ttu-id="6a19d-127">W poniższej tabeli opisano klasy CSS stosowane do elementu `components-reconnect-modal`.</span><span class="sxs-lookup"><span data-stu-id="6a19d-127">The following table describes the CSS classes applied to the `components-reconnect-modal` element.</span></span>

| <span data-ttu-id="6a19d-128">Klasa CSS</span><span class="sxs-lookup"><span data-stu-id="6a19d-128">CSS class</span></span>                       | <span data-ttu-id="6a19d-129">Wskazuje&hellip;</span><span class="sxs-lookup"><span data-stu-id="6a19d-129">Indicates&hellip;</span></span> |
| ------------------------------- | ----------------- |
| `components-reconnect-show`     | <span data-ttu-id="6a19d-130">Utracono połączenie.</span><span class="sxs-lookup"><span data-stu-id="6a19d-130">A lost connection.</span></span> <span data-ttu-id="6a19d-131">Klient próbuje ponownie nawiązać połączenie.</span><span class="sxs-lookup"><span data-stu-id="6a19d-131">The client is attempting to reconnect.</span></span> <span data-ttu-id="6a19d-132">Pokaż modalne.</span><span class="sxs-lookup"><span data-stu-id="6a19d-132">Show the modal.</span></span> |
| `components-reconnect-hide`     | <span data-ttu-id="6a19d-133">Aktywne połączenie zostanie ponownie nawiązane z serwerem.</span><span class="sxs-lookup"><span data-stu-id="6a19d-133">An active connection is re-established to the server.</span></span> <span data-ttu-id="6a19d-134">Ukryj modalne.</span><span class="sxs-lookup"><span data-stu-id="6a19d-134">Hide the modal.</span></span> |
| `components-reconnect-failed`   | <span data-ttu-id="6a19d-135">Ponowne połączenie nie powiodło się, prawdopodobnie z powodu błędu sieci.</span><span class="sxs-lookup"><span data-stu-id="6a19d-135">Reconnection failed, probably due to a network failure.</span></span> <span data-ttu-id="6a19d-136">Aby spróbować ponownie nawiązać połączenie, wywołaj `window.Blazor.reconnect()`.</span><span class="sxs-lookup"><span data-stu-id="6a19d-136">To attempt reconnection, call `window.Blazor.reconnect()`.</span></span> |
| `components-reconnect-rejected` | <span data-ttu-id="6a19d-137">Odrzucono ponowne połączenie.</span><span class="sxs-lookup"><span data-stu-id="6a19d-137">Reconnection rejected.</span></span> <span data-ttu-id="6a19d-138">Serwer został osiągnięty, ale odmówił połączenia, a stan użytkownika na serwerze został utracony.</span><span class="sxs-lookup"><span data-stu-id="6a19d-138">The server was reached but refused the connection, and the user's state on the server is lost.</span></span> <span data-ttu-id="6a19d-139">Aby ponownie załadować aplikację, wywołaj `location.reload()`.</span><span class="sxs-lookup"><span data-stu-id="6a19d-139">To reload the app, call `location.reload()`.</span></span> <span data-ttu-id="6a19d-140">Ten stan połączenia może skutkować tym, że:</span><span class="sxs-lookup"><span data-stu-id="6a19d-140">This connection state may result when:</span></span><ul><li><span data-ttu-id="6a19d-141">Wystąpił awaria w obwodzie po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="6a19d-141">A crash in the server-side circuit occurs.</span></span></li><li><span data-ttu-id="6a19d-142">Klient jest odłączony wystarczająco długo, aby serwer mógł porzucić stan użytkownika.</span><span class="sxs-lookup"><span data-stu-id="6a19d-142">The client is disconnected long enough for the server to drop the user's state.</span></span> <span data-ttu-id="6a19d-143">Wystąpienia składników, z którymi łączy się użytkownik, są usuwane.</span><span class="sxs-lookup"><span data-stu-id="6a19d-143">Instances of the components that the user is interacting with are disposed.</span></span></li><li><span data-ttu-id="6a19d-144">Serwer zostanie uruchomiony ponownie lub proces roboczy aplikacji zostanie odtworzony.</span><span class="sxs-lookup"><span data-stu-id="6a19d-144">The server is restarted, or the app's worker process is recycled.</span></span></li></ul> |

### <a name="render-mode"></a><span data-ttu-id="6a19d-145">Tryb renderowania</span><span class="sxs-lookup"><span data-stu-id="6a19d-145">Render mode</span></span>

<span data-ttu-id="6a19d-146">Aplikacje serwera Blazor są domyślnie skonfigurowane, aby skonfigurować interfejs użytkownika na serwerze przed nawiązaniem połączenia z serwerem.</span><span class="sxs-lookup"><span data-stu-id="6a19d-146">Blazor Server apps are set up by default to prerender the UI on the server before the client connection to the server is established.</span></span> <span data-ttu-id="6a19d-147">Ta konfiguracja jest ustawiana na stronie *_Host. cshtml* Razor:</span><span class="sxs-lookup"><span data-stu-id="6a19d-147">This is set up in the *_Host.cshtml* Razor page:</span></span>

```cshtml
<body>
    <app>
      <component type="typeof(App)" render-mode="ServerPrerendered" />
    </app>

    <script src="_framework/blazor.server.js"></script>
</body>
```

<span data-ttu-id="6a19d-148">`RenderMode` określa, czy składnik:</span><span class="sxs-lookup"><span data-stu-id="6a19d-148">`RenderMode` configures whether the component:</span></span>

* <span data-ttu-id="6a19d-149">Jest wstępnie renderowany na stronie.</span><span class="sxs-lookup"><span data-stu-id="6a19d-149">Is prerendered into the page.</span></span>
* <span data-ttu-id="6a19d-150">Jest renderowany jako statyczny kod HTML na stronie lub zawiera informacje niezbędne do uruchomienia aplikacji Blazor z poziomu agenta użytkownika.</span><span class="sxs-lookup"><span data-stu-id="6a19d-150">Is rendered as static HTML on the page or if it includes the necessary information to bootstrap a Blazor app from the user agent.</span></span>

| `RenderMode`        | <span data-ttu-id="6a19d-151">Opis</span><span class="sxs-lookup"><span data-stu-id="6a19d-151">Description</span></span> |
| ------------------- | ----------- |
| `ServerPrerendered` | <span data-ttu-id="6a19d-152">Renderuje składnik do statycznego kodu HTML i zawiera znacznik dla aplikacji serwera Blazor.</span><span class="sxs-lookup"><span data-stu-id="6a19d-152">Renders the component into static HTML and includes a marker for a Blazor Server app.</span></span> <span data-ttu-id="6a19d-153">Po uruchomieniu agenta użytkownika ten znacznik jest używany do uruchamiania aplikacji Blazor.</span><span class="sxs-lookup"><span data-stu-id="6a19d-153">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Server`            | <span data-ttu-id="6a19d-154">Renderuje znacznik dla aplikacji serwera Blazor.</span><span class="sxs-lookup"><span data-stu-id="6a19d-154">Renders a marker for a Blazor Server app.</span></span> <span data-ttu-id="6a19d-155">Dane wyjściowe ze składnika nie są uwzględniane.</span><span class="sxs-lookup"><span data-stu-id="6a19d-155">Output from the component isn't included.</span></span> <span data-ttu-id="6a19d-156">Po uruchomieniu agenta użytkownika ten znacznik jest używany do uruchamiania aplikacji Blazor.</span><span class="sxs-lookup"><span data-stu-id="6a19d-156">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Static`            | <span data-ttu-id="6a19d-157">Renderuje składnik do statycznego kodu HTML.</span><span class="sxs-lookup"><span data-stu-id="6a19d-157">Renders the component into static HTML.</span></span> |

<span data-ttu-id="6a19d-158">Renderowanie składników serwera ze statyczną stroną HTML nie jest obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="6a19d-158">Rendering server components from a static HTML page isn't supported.</span></span>

### <a name="render-stateful-interactive-components-from-razor-pages-and-views"></a><span data-ttu-id="6a19d-159">Renderuj stanowe składniki interaktywne ze stron Razor i widoków</span><span class="sxs-lookup"><span data-stu-id="6a19d-159">Render stateful interactive components from Razor pages and views</span></span>

<span data-ttu-id="6a19d-160">Można dodać składniki interaktywne ze stanem do strony lub widoku Razor.</span><span class="sxs-lookup"><span data-stu-id="6a19d-160">Stateful interactive components can be added to a Razor page or view.</span></span>

<span data-ttu-id="6a19d-161">Gdy renderuje stronę lub widok:</span><span class="sxs-lookup"><span data-stu-id="6a19d-161">When the page or view renders:</span></span>

* <span data-ttu-id="6a19d-162">Składnik jest wstępnie renderowany przy użyciu strony lub widoku.</span><span class="sxs-lookup"><span data-stu-id="6a19d-162">The component is prerendered with the page or view.</span></span>
* <span data-ttu-id="6a19d-163">Początkowy stan składnika używany na potrzeby renderowania wstępnego został utracony.</span><span class="sxs-lookup"><span data-stu-id="6a19d-163">The initial component state used for prerendering is lost.</span></span>
* <span data-ttu-id="6a19d-164">Nowy stan składnika jest tworzony po nawiązaniu połączenia SignalR.</span><span class="sxs-lookup"><span data-stu-id="6a19d-164">New component state is created when the SignalR connection is established.</span></span>

<span data-ttu-id="6a19d-165">Następująca strona Razor renderuje składnik `Counter`:</span><span class="sxs-lookup"><span data-stu-id="6a19d-165">The following Razor page renders a `Counter` component:</span></span>

```cshtml
<h1>My Razor Page</h1>

<component type="typeof(Counter)" render-mode="ServerPrerendered" 
    param-InitialValue="InitialValue" />

@code {
    [BindProperty(SupportsGet=true)]
    public int InitialValue { get; set; }
}
```

### <a name="render-noninteractive-components-from-razor-pages-and-views"></a><span data-ttu-id="6a19d-166">Renderuj nieinteraktywne składniki ze stron Razor i widoków</span><span class="sxs-lookup"><span data-stu-id="6a19d-166">Render noninteractive components from Razor pages and views</span></span>

<span data-ttu-id="6a19d-167">Na poniższej stronie Razor składnik `Counter` jest renderowany statycznie z wartością początkową określoną przy użyciu formularza:</span><span class="sxs-lookup"><span data-stu-id="6a19d-167">In the following Razor page, the `Counter` component is statically rendered with an initial value that's specified using a form:</span></span>

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

<span data-ttu-id="6a19d-168">Ponieważ `MyComponent` jest renderowany statycznie, składnik nie może być interaktywny.</span><span class="sxs-lookup"><span data-stu-id="6a19d-168">Since `MyComponent` is statically rendered, the component can't be interactive.</span></span>

### <a name="configure-the-opno-locsignalr-client-for-opno-locblazor-server-apps"></a><span data-ttu-id="6a19d-169">Konfigurowanie klienta SignalR dla aplikacji Blazor Server</span><span class="sxs-lookup"><span data-stu-id="6a19d-169">Configure the SignalR client for Blazor Server apps</span></span>

<span data-ttu-id="6a19d-170">Czasami trzeba skonfigurować klienta SignalR używanego przez aplikacje Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="6a19d-170">Sometimes, you need to configure the SignalR client used by Blazor Server apps.</span></span> <span data-ttu-id="6a19d-171">Na przykład możesz chcieć skonfigurować rejestrowanie na kliencie SignalR, aby zdiagnozować problem z połączeniem.</span><span class="sxs-lookup"><span data-stu-id="6a19d-171">For example, you might want to configure logging on the SignalR client to diagnose a connection issue.</span></span>

<span data-ttu-id="6a19d-172">Aby skonfigurować klienta SignalR w pliku *Pages/_Host. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="6a19d-172">To configure the SignalR client in the *Pages/_Host.cshtml* file:</span></span>

* <span data-ttu-id="6a19d-173">Dodaj atrybut `autostart="false"` do tagu `<script>` dla skryptu `blazor.server.js`.</span><span class="sxs-lookup"><span data-stu-id="6a19d-173">Add an `autostart="false"` attribute to the `<script>` tag for the `blazor.server.js` script.</span></span>
* <span data-ttu-id="6a19d-174">Wywołaj `Blazor.start` i przekaż obiekt konfiguracji, który określa SignalR konstruktora.</span><span class="sxs-lookup"><span data-stu-id="6a19d-174">Call `Blazor.start` and pass in a configuration object that specifies the SignalR builder.</span></span>

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
