---
title: ASP.NET Core Blazor konfiguracji modelu hostingu
author: guardrex
description: Dowiedz się więcej o Blazor konfiguracji modelu hostingu, w tym o sposobie integrowania składników Razor z aplikacjami Razor Pages i MVC.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/04/2020
no-loc:
- Blazor
- SignalR
uid: blazor/hosting-model-configuration
ms.openlocfilehash: 91dd1aa32bb5165845eb4794377a50ccbd01adc3
ms.sourcegitcommit: d2ba66023884f0dca115ff010bd98d5ed6459283
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/14/2020
ms.locfileid: "77215061"
---
# <a name="aspnet-core-blazor-hosting-model-configuration"></a><span data-ttu-id="1c9ca-103">Konfiguracja modelu hostingu ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="1c9ca-103">ASP.NET Core Blazor hosting model configuration</span></span>

<span data-ttu-id="1c9ca-104">Autor [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="1c9ca-104">By [Daniel Roth](https://github.com/danroth27)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="1c9ca-105">W tym artykule opisano hostowanie konfiguracji modelu.</span><span class="sxs-lookup"><span data-stu-id="1c9ca-105">This article covers hosting model configuration.</span></span>

<!-- For future use:

## Blazor WebAssembly

-->

## <a name="blazor-server"></a><span data-ttu-id="1c9ca-106">Serwer Blazor</span><span class="sxs-lookup"><span data-stu-id="1c9ca-106">Blazor Server</span></span>

### <a name="integrate-razor-components-into-razor-pages-and-mvc-apps"></a><span data-ttu-id="1c9ca-107">Integrowanie składników Razor z aplikacjami Razor Pages i MVC</span><span class="sxs-lookup"><span data-stu-id="1c9ca-107">Integrate Razor components into Razor Pages and MVC apps</span></span>

#### <a name="use-components-in-pages-and-views"></a><span data-ttu-id="1c9ca-108">Używanie składników na stronach i widokach</span><span class="sxs-lookup"><span data-stu-id="1c9ca-108">Use components in pages and views</span></span>

<span data-ttu-id="1c9ca-109">Istniejąca aplikacja Razor Pages lub MVC może zintegrować składniki Razor ze stronami i widokami:</span><span class="sxs-lookup"><span data-stu-id="1c9ca-109">An existing Razor Pages or MVC app can integrate Razor components into pages and views:</span></span>

1. <span data-ttu-id="1c9ca-110">W pliku układu aplikacji ( *_Layout. cshtml*):</span><span class="sxs-lookup"><span data-stu-id="1c9ca-110">In the app's layout file (*_Layout.cshtml*):</span></span>

   * <span data-ttu-id="1c9ca-111">Dodaj następujący tag `<base>` do elementu `<head>`:</span><span class="sxs-lookup"><span data-stu-id="1c9ca-111">Add the following `<base>` tag to the `<head>` element:</span></span>

     ```html
     <base href="~/" />
     ```

     <span data-ttu-id="1c9ca-112">Wartość `href` ( *Ścieżka podstawowa aplikacji*) w poprzednim przykładzie założono, że aplikacja znajduje się w ścieżce adresu URL katalogu głównego (`/`).</span><span class="sxs-lookup"><span data-stu-id="1c9ca-112">The `href` value (the *app base path*) in the preceding example assumes that the app resides at the root URL path (`/`).</span></span> <span data-ttu-id="1c9ca-113">Jeśli aplikacja jest aplikacją podrzędną, postępuj zgodnie ze wskazówkami w sekcji *Ścieżka podstawowa aplikacji* w artykule <xref:host-and-deploy/blazor/index#app-base-path>.</span><span class="sxs-lookup"><span data-stu-id="1c9ca-113">If the app is a sub-application, follow the guidance in the *App base path* section of the <xref:host-and-deploy/blazor/index#app-base-path> article.</span></span>

     <span data-ttu-id="1c9ca-114">Plik *_Layout. cshtml* znajduje się w folderze *Pages/shared* w aplikacji Razor Pages lub *widokach/folderze udostępnionym* w aplikacji MVC.</span><span class="sxs-lookup"><span data-stu-id="1c9ca-114">The *_Layout.cshtml* file is located in the *Pages/Shared* folder in a Razor Pages app or *Views/Shared* folder in an MVC app.</span></span>

   * <span data-ttu-id="1c9ca-115">Dodaj tag `<script>` dla skryptu *blazor. Server. js* bezpośrednio przed tagiem zamykającym `</body>`:</span><span class="sxs-lookup"><span data-stu-id="1c9ca-115">Add a `<script>` tag for the *blazor.server.js* script right before of the closing `</body>` tag:</span></span>

     ```html
     <script src="_framework/blazor.server.js"></script>
     ```

     <span data-ttu-id="1c9ca-116">Struktura dodaje skrypt *blazor. Server. js* do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1c9ca-116">The framework adds the *blazor.server.js* script to the app.</span></span> <span data-ttu-id="1c9ca-117">Nie trzeba ręcznie dodawać skryptu do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1c9ca-117">There's no need to manually add the script to the app.</span></span>

1. <span data-ttu-id="1c9ca-118">Dodaj plik *_Imports. Razor* do folderu głównego projektu o następującej zawartości (Zmień ostatnią przestrzeń nazw, `MyAppNamespace`, na przestrzeń nazw aplikacji):</span><span class="sxs-lookup"><span data-stu-id="1c9ca-118">Add an *_Imports.razor* file to the root folder of the project with the following content (change the last namespace, `MyAppNamespace`, to the namespace of the app):</span></span>

   ```csharp
   @using System.Net.Http
   @using Microsoft.AspNetCore.Authorization
   @using Microsoft.AspNetCore.Components.Authorization
   @using Microsoft.AspNetCore.Components.Forms
   @using Microsoft.AspNetCore.Components.Routing
   @using Microsoft.AspNetCore.Components.Web
   @using Microsoft.JSInterop
   @using MyAppNamespace
   ```

1. <span data-ttu-id="1c9ca-119">W `Startup.ConfigureServices`Zarejestruj usługę serwera Blazor:</span><span class="sxs-lookup"><span data-stu-id="1c9ca-119">In `Startup.ConfigureServices`, register the Blazor Server service:</span></span>

   ```csharp
   services.AddServerSideBlazor();
   ```

1. <span data-ttu-id="1c9ca-120">W `Startup.Configure`Dodaj punkt końcowy centrum Blazor do `app.UseEndpoints`:</span><span class="sxs-lookup"><span data-stu-id="1c9ca-120">In `Startup.Configure`, add the Blazor Hub endpoint to `app.UseEndpoints`:</span></span>

   ```csharp
   endpoints.MapBlazorHub();
   ```

1. <span data-ttu-id="1c9ca-121">Integruj składniki na dowolną stronę lub widok.</span><span class="sxs-lookup"><span data-stu-id="1c9ca-121">Integrate components into any page or view.</span></span> <span data-ttu-id="1c9ca-122">Aby uzyskać więcej informacji, zobacz sekcję *integrowanie składników w Razor Pages i aplikacje MVC* w artykule <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps>.</span><span class="sxs-lookup"><span data-stu-id="1c9ca-122">For more information, see the *Integrate components into Razor Pages and MVC apps* section of the <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps> article.</span></span>

#### <a name="use-routable-components-in-a-razor-pages-app"></a><span data-ttu-id="1c9ca-123">Używanie składników rutowanych w aplikacji Razor Pages</span><span class="sxs-lookup"><span data-stu-id="1c9ca-123">Use routable components in a Razor Pages app</span></span>

<span data-ttu-id="1c9ca-124">Aby obsługiwać Routing składników Razor w aplikacjach Razor Pages:</span><span class="sxs-lookup"><span data-stu-id="1c9ca-124">To support routable Razor components in Razor Pages apps:</span></span>

1. <span data-ttu-id="1c9ca-125">Postępuj zgodnie ze wskazówkami zawartymi w sekcji [Używanie składników w stronach i widokach](#use-components-in-pages-and-views) .</span><span class="sxs-lookup"><span data-stu-id="1c9ca-125">Follow the guidance in the [Use components in pages and views](#use-components-in-pages-and-views) section.</span></span>

1. <span data-ttu-id="1c9ca-126">Dodaj plik *App. Razor* do katalogu głównego projektu z następującą zawartością:</span><span class="sxs-lookup"><span data-stu-id="1c9ca-126">Add an *App.razor* file to the root of the project with the following content:</span></span>

   ```razor
   @using Microsoft.AspNetCore.Components.Routing

   <Router AppAssembly="typeof(Program).Assembly">
       <Found Context="routeData">
           <RouteView RouteData="routeData" />
       </Found>
       <NotFound>
           <h1>Page not found</h1>
           <p>Sorry, but there's nothing here!</p>
       </NotFound>
   </Router>
   ```

1. <span data-ttu-id="1c9ca-127">Dodaj plik *_Host. cshtml* do folderu *stron* o następującej zawartości:</span><span class="sxs-lookup"><span data-stu-id="1c9ca-127">Add a *_Host.cshtml* file to the *Pages* folder with the following content:</span></span>

   ```cshtml
   @page "/blazor"
   @{
       Layout = "_Layout";
   }

   <app>
       <component type="typeof(App)" render-mode="ServerPrerendered" />
   </app>
   ```

   <span data-ttu-id="1c9ca-128">Składniki używają udostępnionego pliku *_Layout. cshtml* dla ich układu.</span><span class="sxs-lookup"><span data-stu-id="1c9ca-128">Components use the shared *_Layout.cshtml* file for their layout.</span></span>

1. <span data-ttu-id="1c9ca-129">Dodaj trasę o niskim priorytecie dla strony *_Host. cshtml* do konfiguracji punktu końcowego w `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="1c9ca-129">Add a low-priority route for the *_Host.cshtml* page to endpoint configuration in `Startup.Configure`:</span></span>

   ```csharp
   app.UseEndpoints(endpoints =>
   {
       ...

       endpoints.MapFallbackToPage("/_Host");
   });
   ```

1. <span data-ttu-id="1c9ca-130">Dodaj składniki routingu do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1c9ca-130">Add routable components to the app.</span></span> <span data-ttu-id="1c9ca-131">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="1c9ca-131">For example:</span></span>

   ```razor
   @page "/counter"

   <h1>Counter</h1>

   ...
   ```

   <span data-ttu-id="1c9ca-132">W przypadku używania folderu niestandardowego do przechowywania składników aplikacji należy dodać przestrzeń nazw reprezentującą folder do pliku *Pages/_ViewImports. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="1c9ca-132">When using a custom folder to hold the app's components, add the namespace representing the folder to the *Pages/_ViewImports.cshtml* file.</span></span> <span data-ttu-id="1c9ca-133">Aby uzyskać więcej informacji, zobacz temat <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps>.</span><span class="sxs-lookup"><span data-stu-id="1c9ca-133">For more information, see <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps>.</span></span>

#### <a name="use-routable-components-in-an-mvc-app"></a><span data-ttu-id="1c9ca-134">Używanie składników rutowanych w aplikacji MVC</span><span class="sxs-lookup"><span data-stu-id="1c9ca-134">Use routable components in an MVC app</span></span>

<span data-ttu-id="1c9ca-135">Aby zapewnić obsługę routingu składników Razor w aplikacjach MVC:</span><span class="sxs-lookup"><span data-stu-id="1c9ca-135">To support routable Razor components in MVC apps:</span></span>

1. <span data-ttu-id="1c9ca-136">Postępuj zgodnie ze wskazówkami zawartymi w sekcji [Używanie składników w stronach i widokach](#use-components-in-pages-and-views) .</span><span class="sxs-lookup"><span data-stu-id="1c9ca-136">Follow the guidance in the [Use components in pages and views](#use-components-in-pages-and-views) section.</span></span>

1. <span data-ttu-id="1c9ca-137">Dodaj plik *App. Razor* do katalogu głównego projektu z następującą zawartością:</span><span class="sxs-lookup"><span data-stu-id="1c9ca-137">Add an *App.razor* file to the root of the project with the following content:</span></span>

   ```razor
   @using Microsoft.AspNetCore.Components.Routing

   <Router AppAssembly="typeof(Program).Assembly">
       <Found Context="routeData">
           <RouteView RouteData="routeData" />
       </Found>
       <NotFound>
           <h1>Page not found</h1>
           <p>Sorry, but there's nothing here!</p>
       </NotFound>
   </Router>
   ```

1. <span data-ttu-id="1c9ca-138">Dodaj plik *_Host. cshtml* do folderu *widoki/główne* z następującą zawartością:</span><span class="sxs-lookup"><span data-stu-id="1c9ca-138">Add a *_Host.cshtml* file to the *Views/Home* folder with the following content:</span></span>

   ```cshtml
   @{
       Layout = "_Layout";
   }

   <app>
       <component type="typeof(App)" render-mode="ServerPrerendered" />
   </app>
   ```

   <span data-ttu-id="1c9ca-139">Składniki używają udostępnionego pliku *_Layout. cshtml* dla ich układu.</span><span class="sxs-lookup"><span data-stu-id="1c9ca-139">Components use the shared *_Layout.cshtml* file for their layout.</span></span>

1. <span data-ttu-id="1c9ca-140">Dodaj akcję do kontrolera macierzystego:</span><span class="sxs-lookup"><span data-stu-id="1c9ca-140">Add an action to the Home controller:</span></span>

   ```csharp
   public IActionResult Blazor()
   {
      return View("_Host");
   }
   ```

1. <span data-ttu-id="1c9ca-141">Dodaj trasę o niskim priorytecie dla akcji kontrolera, która zwraca widok *_Host. cshtml* do konfiguracji punktu końcowego w `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="1c9ca-141">Add a low-priority route for the controller action that returns the *_Host.cshtml* view to the endpoint configuration in `Startup.Configure`:</span></span>

   ```csharp
   app.UseEndpoints(endpoints =>
   {
       ...

       endpoints.MapFallbackToController("Blazor", "Home");
   });
   ```

1. <span data-ttu-id="1c9ca-142">Utwórz folder *strony* i Dodaj składniki do obsługi routingu do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1c9ca-142">Create a *Pages* folder and add routable components to the app.</span></span> <span data-ttu-id="1c9ca-143">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="1c9ca-143">For example:</span></span>

   ```razor
   @page "/counter"

   <h1>Counter</h1>

   ...
   ```

   <span data-ttu-id="1c9ca-144">W przypadku używania folderu niestandardowego do przechowywania składników aplikacji należy dodać przestrzeń nazw reprezentującą folder do pliku *views/_ViewImports. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="1c9ca-144">When using a custom folder to hold the app's components, add the namespace representing the folder to the *Views/_ViewImports.cshtml* file.</span></span> <span data-ttu-id="1c9ca-145">Aby uzyskać więcej informacji, zobacz temat <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps>.</span><span class="sxs-lookup"><span data-stu-id="1c9ca-145">For more information, see <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps>.</span></span>

### <a name="reflect-the-connection-state-in-the-ui"></a><span data-ttu-id="1c9ca-146">Odzwierciedlanie stanu połączenia w interfejsie użytkownika</span><span class="sxs-lookup"><span data-stu-id="1c9ca-146">Reflect the connection state in the UI</span></span>

<span data-ttu-id="1c9ca-147">Gdy klient wykryje, że połączenie zostało utracone, do użytkownika jest wyświetlany domyślny interfejs użytkownika, podczas gdy klient próbuje ponownie nawiązać połączenie.</span><span class="sxs-lookup"><span data-stu-id="1c9ca-147">When the client detects that the connection has been lost, a default UI is displayed to the user while the client attempts to reconnect.</span></span> <span data-ttu-id="1c9ca-148">Jeśli ponowne połączenie nie powiedzie się, użytkownik otrzymuje opcję ponowienia próby.</span><span class="sxs-lookup"><span data-stu-id="1c9ca-148">If reconnection fails, the user is provided the option to retry.</span></span>

<span data-ttu-id="1c9ca-149">Aby dostosować interfejs użytkownika, zdefiniuj element z `id` `components-reconnect-modal` w `<body>` na stronie *_Host. cshtml* Razor:</span><span class="sxs-lookup"><span data-stu-id="1c9ca-149">To customize the UI, define an element with an `id` of `components-reconnect-modal` in the `<body>` of the *_Host.cshtml* Razor page:</span></span>

```cshtml
<div id="components-reconnect-modal">
    ...
</div>
```

<span data-ttu-id="1c9ca-150">W poniższej tabeli opisano klasy CSS stosowane do elementu `components-reconnect-modal`.</span><span class="sxs-lookup"><span data-stu-id="1c9ca-150">The following table describes the CSS classes applied to the `components-reconnect-modal` element.</span></span>

| <span data-ttu-id="1c9ca-151">Klasa CSS</span><span class="sxs-lookup"><span data-stu-id="1c9ca-151">CSS class</span></span>                       | <span data-ttu-id="1c9ca-152">Wskazuje&hellip;</span><span class="sxs-lookup"><span data-stu-id="1c9ca-152">Indicates&hellip;</span></span> |
| ------------------------------- | ----------------- |
| `components-reconnect-show`     | <span data-ttu-id="1c9ca-153">Utracono połączenie.</span><span class="sxs-lookup"><span data-stu-id="1c9ca-153">A lost connection.</span></span> <span data-ttu-id="1c9ca-154">Klient próbuje ponownie nawiązać połączenie.</span><span class="sxs-lookup"><span data-stu-id="1c9ca-154">The client is attempting to reconnect.</span></span> <span data-ttu-id="1c9ca-155">Pokaż modalne.</span><span class="sxs-lookup"><span data-stu-id="1c9ca-155">Show the modal.</span></span> |
| `components-reconnect-hide`     | <span data-ttu-id="1c9ca-156">Aktywne połączenie zostanie ponownie nawiązane z serwerem.</span><span class="sxs-lookup"><span data-stu-id="1c9ca-156">An active connection is re-established to the server.</span></span> <span data-ttu-id="1c9ca-157">Ukryj modalne.</span><span class="sxs-lookup"><span data-stu-id="1c9ca-157">Hide the modal.</span></span> |
| `components-reconnect-failed`   | <span data-ttu-id="1c9ca-158">Ponowne połączenie nie powiodło się, prawdopodobnie z powodu błędu sieci.</span><span class="sxs-lookup"><span data-stu-id="1c9ca-158">Reconnection failed, probably due to a network failure.</span></span> <span data-ttu-id="1c9ca-159">Aby spróbować ponownie nawiązać połączenie, wywołaj `window.Blazor.reconnect()`.</span><span class="sxs-lookup"><span data-stu-id="1c9ca-159">To attempt reconnection, call `window.Blazor.reconnect()`.</span></span> |
| `components-reconnect-rejected` | <span data-ttu-id="1c9ca-160">Odrzucono ponowne połączenie.</span><span class="sxs-lookup"><span data-stu-id="1c9ca-160">Reconnection rejected.</span></span> <span data-ttu-id="1c9ca-161">Serwer został osiągnięty, ale odmówił połączenia, a stan użytkownika na serwerze został utracony.</span><span class="sxs-lookup"><span data-stu-id="1c9ca-161">The server was reached but refused the connection, and the user's state on the server is lost.</span></span> <span data-ttu-id="1c9ca-162">Aby ponownie załadować aplikację, wywołaj `location.reload()`.</span><span class="sxs-lookup"><span data-stu-id="1c9ca-162">To reload the app, call `location.reload()`.</span></span> <span data-ttu-id="1c9ca-163">Ten stan połączenia może skutkować tym, że:</span><span class="sxs-lookup"><span data-stu-id="1c9ca-163">This connection state may result when:</span></span><ul><li><span data-ttu-id="1c9ca-164">Wystąpił awaria w obwodzie po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="1c9ca-164">A crash in the server-side circuit occurs.</span></span></li><li><span data-ttu-id="1c9ca-165">Klient jest odłączony wystarczająco długo, aby serwer mógł porzucić stan użytkownika.</span><span class="sxs-lookup"><span data-stu-id="1c9ca-165">The client is disconnected long enough for the server to drop the user's state.</span></span> <span data-ttu-id="1c9ca-166">Wystąpienia składników, z którymi łączy się użytkownik, są usuwane.</span><span class="sxs-lookup"><span data-stu-id="1c9ca-166">Instances of the components that the user is interacting with are disposed.</span></span></li><li><span data-ttu-id="1c9ca-167">Serwer zostanie uruchomiony ponownie lub proces roboczy aplikacji zostanie odtworzony.</span><span class="sxs-lookup"><span data-stu-id="1c9ca-167">The server is restarted, or the app's worker process is recycled.</span></span></li></ul> |

### <a name="render-mode"></a><span data-ttu-id="1c9ca-168">Tryb renderowania</span><span class="sxs-lookup"><span data-stu-id="1c9ca-168">Render mode</span></span>

<span data-ttu-id="1c9ca-169">Aplikacje serwera Blazor są domyślnie skonfigurowane, aby skonfigurować interfejs użytkownika na serwerze przed nawiązaniem połączenia z serwerem.</span><span class="sxs-lookup"><span data-stu-id="1c9ca-169">Blazor Server apps are set up by default to prerender the UI on the server before the client connection to the server is established.</span></span> <span data-ttu-id="1c9ca-170">Ta konfiguracja jest ustawiana na stronie *_Host. cshtml* Razor:</span><span class="sxs-lookup"><span data-stu-id="1c9ca-170">This is set up in the *_Host.cshtml* Razor page:</span></span>

```cshtml
<body>
    <app>
      <component type="typeof(App)" render-mode="ServerPrerendered" />
    </app>

    <script src="_framework/blazor.server.js"></script>
</body>
```

<span data-ttu-id="1c9ca-171">`RenderMode` określa, czy składnik:</span><span class="sxs-lookup"><span data-stu-id="1c9ca-171">`RenderMode` configures whether the component:</span></span>

* <span data-ttu-id="1c9ca-172">Jest wstępnie renderowany na stronie.</span><span class="sxs-lookup"><span data-stu-id="1c9ca-172">Is prerendered into the page.</span></span>
* <span data-ttu-id="1c9ca-173">Jest renderowany jako statyczny kod HTML na stronie lub zawiera informacje niezbędne do uruchomienia aplikacji Blazor z poziomu agenta użytkownika.</span><span class="sxs-lookup"><span data-stu-id="1c9ca-173">Is rendered as static HTML on the page or if it includes the necessary information to bootstrap a Blazor app from the user agent.</span></span>

| `RenderMode`        | <span data-ttu-id="1c9ca-174">Opis</span><span class="sxs-lookup"><span data-stu-id="1c9ca-174">Description</span></span> |
| ------------------- | ----------- |
| `ServerPrerendered` | <span data-ttu-id="1c9ca-175">Renderuje składnik do statycznego kodu HTML i zawiera znacznik dla aplikacji serwera Blazor.</span><span class="sxs-lookup"><span data-stu-id="1c9ca-175">Renders the component into static HTML and includes a marker for a Blazor Server app.</span></span> <span data-ttu-id="1c9ca-176">Po uruchomieniu agenta użytkownika ten znacznik jest używany do uruchamiania aplikacji Blazor.</span><span class="sxs-lookup"><span data-stu-id="1c9ca-176">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Server`            | <span data-ttu-id="1c9ca-177">Renderuje znacznik dla aplikacji serwera Blazor.</span><span class="sxs-lookup"><span data-stu-id="1c9ca-177">Renders a marker for a Blazor Server app.</span></span> <span data-ttu-id="1c9ca-178">Dane wyjściowe ze składnika nie są uwzględniane.</span><span class="sxs-lookup"><span data-stu-id="1c9ca-178">Output from the component isn't included.</span></span> <span data-ttu-id="1c9ca-179">Po uruchomieniu agenta użytkownika ten znacznik jest używany do uruchamiania aplikacji Blazor.</span><span class="sxs-lookup"><span data-stu-id="1c9ca-179">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Static`            | <span data-ttu-id="1c9ca-180">Renderuje składnik do statycznego kodu HTML.</span><span class="sxs-lookup"><span data-stu-id="1c9ca-180">Renders the component into static HTML.</span></span> |

<span data-ttu-id="1c9ca-181">Renderowanie składników serwera ze statyczną stroną HTML nie jest obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="1c9ca-181">Rendering server components from a static HTML page isn't supported.</span></span>

### <a name="render-stateful-interactive-components-from-razor-pages-and-views"></a><span data-ttu-id="1c9ca-182">Renderuj stanowe składniki interaktywne ze stron Razor i widoków</span><span class="sxs-lookup"><span data-stu-id="1c9ca-182">Render stateful interactive components from Razor pages and views</span></span>

<span data-ttu-id="1c9ca-183">Można dodać składniki interaktywne ze stanem do strony lub widoku Razor.</span><span class="sxs-lookup"><span data-stu-id="1c9ca-183">Stateful interactive components can be added to a Razor page or view.</span></span>

<span data-ttu-id="1c9ca-184">Gdy renderuje stronę lub widok:</span><span class="sxs-lookup"><span data-stu-id="1c9ca-184">When the page or view renders:</span></span>

* <span data-ttu-id="1c9ca-185">Składnik jest wstępnie renderowany przy użyciu strony lub widoku.</span><span class="sxs-lookup"><span data-stu-id="1c9ca-185">The component is prerendered with the page or view.</span></span>
* <span data-ttu-id="1c9ca-186">Początkowy stan składnika używany na potrzeby renderowania wstępnego został utracony.</span><span class="sxs-lookup"><span data-stu-id="1c9ca-186">The initial component state used for prerendering is lost.</span></span>
* <span data-ttu-id="1c9ca-187">Nowy stan składnika jest tworzony po nawiązaniu połączenia SignalR.</span><span class="sxs-lookup"><span data-stu-id="1c9ca-187">New component state is created when the SignalR connection is established.</span></span>

<span data-ttu-id="1c9ca-188">Następująca strona Razor renderuje składnik `Counter`:</span><span class="sxs-lookup"><span data-stu-id="1c9ca-188">The following Razor page renders a `Counter` component:</span></span>

```cshtml
<h1>My Razor Page</h1>

<component type="typeof(Counter)" render-mode="ServerPrerendered" 
    param-InitialValue="InitialValue" />

@code {
    [BindProperty(SupportsGet=true)]
    public int InitialValue { get; set; }
}
```

### <a name="render-noninteractive-components-from-razor-pages-and-views"></a><span data-ttu-id="1c9ca-189">Renderuj nieinteraktywne składniki ze stron Razor i widoków</span><span class="sxs-lookup"><span data-stu-id="1c9ca-189">Render noninteractive components from Razor pages and views</span></span>

<span data-ttu-id="1c9ca-190">Na poniższej stronie Razor składnik `Counter` jest renderowany statycznie z wartością początkową określoną przy użyciu formularza:</span><span class="sxs-lookup"><span data-stu-id="1c9ca-190">In the following Razor page, the `Counter` component is statically rendered with an initial value that's specified using a form:</span></span>

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

<span data-ttu-id="1c9ca-191">Ponieważ `MyComponent` jest renderowany statycznie, składnik nie może być interaktywny.</span><span class="sxs-lookup"><span data-stu-id="1c9ca-191">Since `MyComponent` is statically rendered, the component can't be interactive.</span></span>

### <a name="configure-the-opno-locsignalr-client-for-opno-locblazor-server-apps"></a><span data-ttu-id="1c9ca-192">Konfigurowanie klienta SignalR dla aplikacji Blazor Server</span><span class="sxs-lookup"><span data-stu-id="1c9ca-192">Configure the SignalR client for Blazor Server apps</span></span>

<span data-ttu-id="1c9ca-193">Czasami trzeba skonfigurować klienta SignalR używanego przez aplikacje Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="1c9ca-193">Sometimes, you need to configure the SignalR client used by Blazor Server apps.</span></span> <span data-ttu-id="1c9ca-194">Na przykład możesz chcieć skonfigurować rejestrowanie na kliencie SignalR, aby zdiagnozować problem z połączeniem.</span><span class="sxs-lookup"><span data-stu-id="1c9ca-194">For example, you might want to configure logging on the SignalR client to diagnose a connection issue.</span></span>

<span data-ttu-id="1c9ca-195">Aby skonfigurować klienta SignalR w pliku *Pages/_Host. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="1c9ca-195">To configure the SignalR client in the *Pages/_Host.cshtml* file:</span></span>

* <span data-ttu-id="1c9ca-196">Dodaj atrybut `autostart="false"` do tagu `<script>` dla skryptu `blazor.server.js`.</span><span class="sxs-lookup"><span data-stu-id="1c9ca-196">Add an `autostart="false"` attribute to the `<script>` tag for the `blazor.server.js` script.</span></span>
* <span data-ttu-id="1c9ca-197">Wywołaj `Blazor.start` i przekaż obiekt konfiguracji, który określa SignalR konstruktora.</span><span class="sxs-lookup"><span data-stu-id="1c9ca-197">Call `Blazor.start` and pass in a configuration object that specifies the SignalR builder.</span></span>

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
