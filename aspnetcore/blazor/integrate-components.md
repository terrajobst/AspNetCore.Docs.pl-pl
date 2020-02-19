---
title: Integrowanie składników ASP.NET Core Razor z aplikacjami Razor Pages i MVC
author: guardrex
description: Dowiedz się więcej na temat scenariuszy powiązań danych dla składników i elementów DOM w aplikacjach Blazor.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/12/2020
no-loc:
- Blazor
- SignalR
uid: blazor/integrate-components
ms.openlocfilehash: de1a37ffd9456c956e3d84fcc69431ecb794513c
ms.sourcegitcommit: 6645435fc8f5092fc7e923742e85592b56e37ada
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/19/2020
ms.locfileid: "77453297"
---
# <a name="integrate-aspnet-core-razor-components-into-razor-pages-and-mvc-apps"></a><span data-ttu-id="0ed4f-103">Integrowanie składników ASP.NET Core Razor z aplikacjami Razor Pages i MVC</span><span class="sxs-lookup"><span data-stu-id="0ed4f-103">Integrate ASP.NET Core Razor components into Razor Pages and MVC apps</span></span>

<span data-ttu-id="0ed4f-104">Autorzy [Luke Latham](https://github.com/guardrex) i [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="0ed4f-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="0ed4f-105">Składniki Razor można zintegrować z aplikacjami Razor Pages i MVC.</span><span class="sxs-lookup"><span data-stu-id="0ed4f-105">Razor components can be integrated into Razor Pages and MVC apps.</span></span> <span data-ttu-id="0ed4f-106">Gdy strona lub widok jest renderowany, składniki mogą być wstępnie renderowane w tym samym czasie.</span><span class="sxs-lookup"><span data-stu-id="0ed4f-106">When the page or view is rendered, components can be prerendered at the same time.</span></span>

## <a name="prepare-the-app-to-use-components-in-pages-and-views"></a><span data-ttu-id="0ed4f-107">Przygotowywanie aplikacji do używania składników na stronach i widokach</span><span class="sxs-lookup"><span data-stu-id="0ed4f-107">Prepare the app to use components in pages and views</span></span>

<span data-ttu-id="0ed4f-108">Istniejąca aplikacja Razor Pages lub MVC może zintegrować składniki Razor ze stronami i widokami:</span><span class="sxs-lookup"><span data-stu-id="0ed4f-108">An existing Razor Pages or MVC app can integrate Razor components into pages and views:</span></span>

1. <span data-ttu-id="0ed4f-109">W pliku układu aplikacji ( *_Layout. cshtml*):</span><span class="sxs-lookup"><span data-stu-id="0ed4f-109">In the app's layout file (*_Layout.cshtml*):</span></span>

   * <span data-ttu-id="0ed4f-110">Dodaj następujący tag `<base>` do elementu `<head>`:</span><span class="sxs-lookup"><span data-stu-id="0ed4f-110">Add the following `<base>` tag to the `<head>` element:</span></span>

     ```html
     <base href="~/" />
     ```

     <span data-ttu-id="0ed4f-111">Wartość `href` ( *Ścieżka podstawowa aplikacji*) w poprzednim przykładzie założono, że aplikacja znajduje się w ścieżce adresu URL katalogu głównego (`/`).</span><span class="sxs-lookup"><span data-stu-id="0ed4f-111">The `href` value (the *app base path*) in the preceding example assumes that the app resides at the root URL path (`/`).</span></span> <span data-ttu-id="0ed4f-112">Jeśli aplikacja jest aplikacją podrzędną, postępuj zgodnie ze wskazówkami w sekcji *Ścieżka podstawowa aplikacji* w artykule <xref:host-and-deploy/blazor/index#app-base-path>.</span><span class="sxs-lookup"><span data-stu-id="0ed4f-112">If the app is a sub-application, follow the guidance in the *App base path* section of the <xref:host-and-deploy/blazor/index#app-base-path> article.</span></span>

     <span data-ttu-id="0ed4f-113">Plik *_Layout. cshtml* znajduje się w folderze *Pages/shared* w aplikacji Razor Pages lub *widokach/folderze udostępnionym* w aplikacji MVC.</span><span class="sxs-lookup"><span data-stu-id="0ed4f-113">The *_Layout.cshtml* file is located in the *Pages/Shared* folder in a Razor Pages app or *Views/Shared* folder in an MVC app.</span></span>

   * <span data-ttu-id="0ed4f-114">Dodaj tag `<script>` dla skryptu *blazor. Server. js* bezpośrednio przed tagiem zamykającym `</body>`:</span><span class="sxs-lookup"><span data-stu-id="0ed4f-114">Add a `<script>` tag for the *blazor.server.js* script immediately before of the closing `</body>` tag:</span></span>

     ```html
     <script src="_framework/blazor.server.js"></script>
     ```

     <span data-ttu-id="0ed4f-115">Struktura dodaje skrypt *blazor. Server. js* do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0ed4f-115">The framework adds the *blazor.server.js* script to the app.</span></span> <span data-ttu-id="0ed4f-116">Nie trzeba ręcznie dodawać skryptu do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0ed4f-116">There's no need to manually add the script to the app.</span></span>

1. <span data-ttu-id="0ed4f-117">Dodaj plik *_Imports. Razor* do folderu głównego projektu o następującej zawartości (Zmień ostatnią przestrzeń nazw, `MyAppNamespace`, na przestrzeń nazw aplikacji):</span><span class="sxs-lookup"><span data-stu-id="0ed4f-117">Add an *_Imports.razor* file to the root folder of the project with the following content (change the last namespace, `MyAppNamespace`, to the namespace of the app):</span></span>

   ```razor
   @using System.Net.Http
   @using Microsoft.AspNetCore.Authorization
   @using Microsoft.AspNetCore.Components.Authorization
   @using Microsoft.AspNetCore.Components.Forms
   @using Microsoft.AspNetCore.Components.Routing
   @using Microsoft.AspNetCore.Components.Web
   @using Microsoft.JSInterop
   @using MyAppNamespace
   ```

1. <span data-ttu-id="0ed4f-118">W `Startup.ConfigureServices`Zarejestruj usługę serwera Blazor:</span><span class="sxs-lookup"><span data-stu-id="0ed4f-118">In `Startup.ConfigureServices`, register the Blazor Server service:</span></span>

   ```csharp
   services.AddServerSideBlazor();
   ```

1. <span data-ttu-id="0ed4f-119">W `Startup.Configure`Dodaj punkt końcowy centrum Blazor do `app.UseEndpoints`:</span><span class="sxs-lookup"><span data-stu-id="0ed4f-119">In `Startup.Configure`, add the Blazor Hub endpoint to `app.UseEndpoints`:</span></span>

   ```csharp
   endpoints.MapBlazorHub();
   ```

1. <span data-ttu-id="0ed4f-120">Integruj składniki na dowolną stronę lub widok.</span><span class="sxs-lookup"><span data-stu-id="0ed4f-120">Integrate components into any page or view.</span></span> <span data-ttu-id="0ed4f-121">Aby uzyskać więcej informacji, zobacz [składniki renderowania ze strony lub widoku](#render-components-from-a-page-or-view) .</span><span class="sxs-lookup"><span data-stu-id="0ed4f-121">For more information, see the [Render components from a page or view](#render-components-from-a-page-or-view) section.</span></span>

## <a name="use-routable-components-in-a-razor-pages-app"></a><span data-ttu-id="0ed4f-122">Używanie składników rutowanych w aplikacji Razor Pages</span><span class="sxs-lookup"><span data-stu-id="0ed4f-122">Use routable components in a Razor Pages app</span></span>

<span data-ttu-id="0ed4f-123">*Ta sekcja dotyczy dodawania składników, które są bezpośrednio trasowane z żądań użytkowników.*</span><span class="sxs-lookup"><span data-stu-id="0ed4f-123">*This section pertains to adding components that are directly routable from user requests.*</span></span>

<span data-ttu-id="0ed4f-124">Aby obsługiwać Routing składników Razor w aplikacjach Razor Pages:</span><span class="sxs-lookup"><span data-stu-id="0ed4f-124">To support routable Razor components in Razor Pages apps:</span></span>

1. <span data-ttu-id="0ed4f-125">Postępuj zgodnie ze wskazówkami zawartymi w sekcji [przygotowanie aplikacji do używania składników w stronach i widokach](#prepare-the-app-to-use-components-in-pages-and-views) .</span><span class="sxs-lookup"><span data-stu-id="0ed4f-125">Follow the guidance in the [Prepare the app to use components in pages and views](#prepare-the-app-to-use-components-in-pages-and-views) section.</span></span>

1. <span data-ttu-id="0ed4f-126">Dodaj plik *App. Razor* do katalogu głównego projektu z następującą zawartością:</span><span class="sxs-lookup"><span data-stu-id="0ed4f-126">Add an *App.razor* file to the project root with the following content:</span></span>

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

1. <span data-ttu-id="0ed4f-127">Dodaj plik *_Host. cshtml* do folderu *stron* o następującej zawartości:</span><span class="sxs-lookup"><span data-stu-id="0ed4f-127">Add a *_Host.cshtml* file to the *Pages* folder with the following content:</span></span>

   ```cshtml
   @page "/blazor"
   @{
       Layout = "_Layout";
   }

   <app>
       <component type="typeof(App)" render-mode="ServerPrerendered" />
   </app>
   ```

   <span data-ttu-id="0ed4f-128">Składniki używają udostępnionego pliku *_Layout. cshtml* dla ich układu.</span><span class="sxs-lookup"><span data-stu-id="0ed4f-128">Components use the shared *_Layout.cshtml* file for their layout.</span></span>

1. <span data-ttu-id="0ed4f-129">Dodaj trasę o niskim priorytecie dla strony *_Host. cshtml* do konfiguracji punktu końcowego w `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="0ed4f-129">Add a low-priority route for the *_Host.cshtml* page to endpoint configuration in `Startup.Configure`:</span></span>

   ```csharp
   app.UseEndpoints(endpoints =>
   {
       ...

       endpoints.MapFallbackToPage("/_Host");
   });
   ```

1. <span data-ttu-id="0ed4f-130">Dodaj składniki routingu do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0ed4f-130">Add routable components to the app.</span></span> <span data-ttu-id="0ed4f-131">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="0ed4f-131">For example:</span></span>

   ```razor
   @page "/counter"

   <h1>Counter</h1>

   ...
   ```

   <span data-ttu-id="0ed4f-132">Aby uzyskać więcej informacji na temat przestrzeni nazw, zobacz sekcję [przestrzenie nazw składników](#component-namespaces) .</span><span class="sxs-lookup"><span data-stu-id="0ed4f-132">For more information on namespaces, see the [Component namespaces](#component-namespaces) section.</span></span>

## <a name="use-routable-components-in-an-mvc-app"></a><span data-ttu-id="0ed4f-133">Używanie składników rutowanych w aplikacji MVC</span><span class="sxs-lookup"><span data-stu-id="0ed4f-133">Use routable components in an MVC app</span></span>

<span data-ttu-id="0ed4f-134">*Ta sekcja dotyczy dodawania składników, które są bezpośrednio trasowane z żądań użytkowników.*</span><span class="sxs-lookup"><span data-stu-id="0ed4f-134">*This section pertains to adding components that are directly routable from user requests.*</span></span>

<span data-ttu-id="0ed4f-135">Aby zapewnić obsługę routingu składników Razor w aplikacjach MVC:</span><span class="sxs-lookup"><span data-stu-id="0ed4f-135">To support routable Razor components in MVC apps:</span></span>

1. <span data-ttu-id="0ed4f-136">Postępuj zgodnie ze wskazówkami zawartymi w sekcji [przygotowanie aplikacji do używania składników w stronach i widokach](#prepare-the-app-to-use-components-in-pages-and-views) .</span><span class="sxs-lookup"><span data-stu-id="0ed4f-136">Follow the guidance in the [Prepare the app to use components in pages and views](#prepare-the-app-to-use-components-in-pages-and-views) section.</span></span>

1. <span data-ttu-id="0ed4f-137">Dodaj plik *App. Razor* do katalogu głównego projektu z następującą zawartością:</span><span class="sxs-lookup"><span data-stu-id="0ed4f-137">Add an *App.razor* file to the root of the project with the following content:</span></span>

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

1. <span data-ttu-id="0ed4f-138">Dodaj plik *_Host. cshtml* do folderu *widoki/główne* z następującą zawartością:</span><span class="sxs-lookup"><span data-stu-id="0ed4f-138">Add a *_Host.cshtml* file to the *Views/Home* folder with the following content:</span></span>

   ```cshtml
   @{
       Layout = "_Layout";
   }

   <app>
       <component type="typeof(App)" render-mode="ServerPrerendered" />
   </app>
   ```

   <span data-ttu-id="0ed4f-139">Składniki używają udostępnionego pliku *_Layout. cshtml* dla ich układu.</span><span class="sxs-lookup"><span data-stu-id="0ed4f-139">Components use the shared *_Layout.cshtml* file for their layout.</span></span>

1. <span data-ttu-id="0ed4f-140">Dodaj akcję do kontrolera macierzystego:</span><span class="sxs-lookup"><span data-stu-id="0ed4f-140">Add an action to the Home controller:</span></span>

   ```csharp
   public IActionResult Blazor()
   {
      return View("_Host");
   }
   ```

1. <span data-ttu-id="0ed4f-141">Dodaj trasę o niskim priorytecie dla akcji kontrolera, która zwraca widok *_Host. cshtml* do konfiguracji punktu końcowego w `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="0ed4f-141">Add a low-priority route for the controller action that returns the *_Host.cshtml* view to the endpoint configuration in `Startup.Configure`:</span></span>

   ```csharp
   app.UseEndpoints(endpoints =>
   {
       ...

       endpoints.MapFallbackToController("Blazor", "Home");
   });
   ```

1. <span data-ttu-id="0ed4f-142">Utwórz folder *strony* i Dodaj składniki do obsługi routingu do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0ed4f-142">Create a *Pages* folder and add routable components to the app.</span></span> <span data-ttu-id="0ed4f-143">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="0ed4f-143">For example:</span></span>

   ```razor
   @page "/counter"

   <h1>Counter</h1>

   ...
   ```

   <span data-ttu-id="0ed4f-144">Aby uzyskać więcej informacji na temat przestrzeni nazw, zobacz sekcję [przestrzenie nazw składników](#component-namespaces) .</span><span class="sxs-lookup"><span data-stu-id="0ed4f-144">For more information on namespaces, see the [Component namespaces](#component-namespaces) section.</span></span>

## <a name="component-namespaces"></a><span data-ttu-id="0ed4f-145">Przestrzenie nazw składników</span><span class="sxs-lookup"><span data-stu-id="0ed4f-145">Component namespaces</span></span>

<span data-ttu-id="0ed4f-146">W przypadku używania folderu niestandardowego do przechowywania składników aplikacji należy dodać przestrzeń nazw reprezentującą folder do strony/widoku lub pliku *_ViewImports. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="0ed4f-146">When using a custom folder to hold the app's components, add the namespace representing the folder to either the page/view or to the *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="0ed4f-147">W poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="0ed4f-147">In the following example:</span></span>

* <span data-ttu-id="0ed4f-148">Zmień `MyAppNamespace` na przestrzeń nazw aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0ed4f-148">Change `MyAppNamespace` to the app's namespace.</span></span>
* <span data-ttu-id="0ed4f-149">Jeśli folder o nazwie *Components* nie jest używany do przechowywania składników, należy zmienić `Components` do folderu, w którym znajdują się składniki.</span><span class="sxs-lookup"><span data-stu-id="0ed4f-149">If a folder named *Components* isn't used to hold the components, change `Components` to the folder where the components reside.</span></span>

```cshtml
@using MyAppNamespace.Components
```

<span data-ttu-id="0ed4f-150">Plik *_ViewImports. cshtml* znajduje się w folderze *strony* aplikacji Razor Pages lub folderu *widoki* aplikacji MVC.</span><span class="sxs-lookup"><span data-stu-id="0ed4f-150">The *_ViewImports.cshtml* file is located in the *Pages* folder of a Razor Pages app or the *Views* folder of an MVC app.</span></span>

<span data-ttu-id="0ed4f-151">Aby uzyskać więcej informacji, zobacz <xref:blazor/components#import-components>.</span><span class="sxs-lookup"><span data-stu-id="0ed4f-151">For more information, see <xref:blazor/components#import-components>.</span></span>

## <a name="render-components-from-a-page-or-view"></a><span data-ttu-id="0ed4f-152">Renderuj składniki ze strony lub widoku</span><span class="sxs-lookup"><span data-stu-id="0ed4f-152">Render components from a page or view</span></span>

<span data-ttu-id="0ed4f-153">*Ta sekcja dotyczy dodawania składników do stron lub widoków, w których składniki nie są bezpośrednio trasowane z żądań użytkownika.*</span><span class="sxs-lookup"><span data-stu-id="0ed4f-153">*This section pertains to adding components to pages or views, where the components aren't directly routable from user requests.*</span></span>

<span data-ttu-id="0ed4f-154">Aby renderować składnik ze strony lub widoku, użyj pomocnika tagów `Component`:</span><span class="sxs-lookup"><span data-stu-id="0ed4f-154">To render a component from a page or view, use the `Component` Tag Helper:</span></span>

```cshtml
<component type="typeof(Counter)" render-mode="ServerPrerendered" 
    param-IncrementAmount="10" />
```

<span data-ttu-id="0ed4f-155">Typ parametru musi być możliwy do serializacji JSON, co oznacza, że typ musi mieć domyślny Konstruktor i właściwości settable.</span><span class="sxs-lookup"><span data-stu-id="0ed4f-155">The parameter type must be JSON serializable, which typically means that the type must have a default constructor and settable properties.</span></span> <span data-ttu-id="0ed4f-156">Na przykład można określić wartość `IncrementAmount`, ponieważ typ `IncrementAmount` jest `int`, który jest typem pierwotnym obsługiwanym przez serializator JSON.</span><span class="sxs-lookup"><span data-stu-id="0ed4f-156">For example, you can specify a value for `IncrementAmount` because the type of `IncrementAmount` is an `int`, which is a primitive type supported by the JSON serializer.</span></span>

<span data-ttu-id="0ed4f-157">`RenderMode` określa, czy składnik:</span><span class="sxs-lookup"><span data-stu-id="0ed4f-157">`RenderMode` configures whether the component:</span></span>

* <span data-ttu-id="0ed4f-158">Jest wstępnie renderowany na stronie.</span><span class="sxs-lookup"><span data-stu-id="0ed4f-158">Is prerendered into the page.</span></span>
* <span data-ttu-id="0ed4f-159">Jest renderowany jako statyczny kod HTML na stronie lub zawiera informacje niezbędne do uruchomienia aplikacji Blazor z poziomu agenta użytkownika.</span><span class="sxs-lookup"><span data-stu-id="0ed4f-159">Is rendered as static HTML on the page or if it includes the necessary information to bootstrap a Blazor app from the user agent.</span></span>

| `RenderMode`        | <span data-ttu-id="0ed4f-160">Opis</span><span class="sxs-lookup"><span data-stu-id="0ed4f-160">Description</span></span> |
| ------------------- | ----------- |
| `ServerPrerendered` | <span data-ttu-id="0ed4f-161">Renderuje składnik do statycznego kodu HTML i zawiera znacznik dla aplikacji serwera Blazor.</span><span class="sxs-lookup"><span data-stu-id="0ed4f-161">Renders the component into static HTML and includes a marker for a Blazor Server app.</span></span> <span data-ttu-id="0ed4f-162">Po uruchomieniu agenta użytkownika ten znacznik jest używany do uruchamiania aplikacji Blazor.</span><span class="sxs-lookup"><span data-stu-id="0ed4f-162">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Server`            | <span data-ttu-id="0ed4f-163">Renderuje znacznik dla aplikacji serwera Blazor.</span><span class="sxs-lookup"><span data-stu-id="0ed4f-163">Renders a marker for a Blazor Server app.</span></span> <span data-ttu-id="0ed4f-164">Dane wyjściowe ze składnika nie są uwzględniane.</span><span class="sxs-lookup"><span data-stu-id="0ed4f-164">Output from the component isn't included.</span></span> <span data-ttu-id="0ed4f-165">Po uruchomieniu agenta użytkownika ten znacznik jest używany do uruchamiania aplikacji Blazor.</span><span class="sxs-lookup"><span data-stu-id="0ed4f-165">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Static`            | <span data-ttu-id="0ed4f-166">Renderuje składnik do statycznego kodu HTML.</span><span class="sxs-lookup"><span data-stu-id="0ed4f-166">Renders the component into static HTML.</span></span> |

<span data-ttu-id="0ed4f-167">Podczas gdy strony i widoki mogą korzystać ze składników, wartość nie jest równa "true".</span><span class="sxs-lookup"><span data-stu-id="0ed4f-167">While pages and views can use components, the converse isn't true.</span></span> <span data-ttu-id="0ed4f-168">Składniki nie mogą używać scenariuszy dotyczących widoków i stron, takich jak częściowe widoki i sekcje.</span><span class="sxs-lookup"><span data-stu-id="0ed4f-168">Components can't use view- and page-specific scenarios, such as partial views and sections.</span></span> <span data-ttu-id="0ed4f-169">Aby użyć logiki z widoku częściowego w składniku, należy rozłożyć logikę widoku częściowego na składnik.</span><span class="sxs-lookup"><span data-stu-id="0ed4f-169">To use logic from partial view in a component, factor out the partial view logic into a component.</span></span>

<span data-ttu-id="0ed4f-170">Renderowanie składników serwera ze statyczną stroną HTML nie jest obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="0ed4f-170">Rendering server components from a static HTML page isn't supported.</span></span>

<span data-ttu-id="0ed4f-171">Aby uzyskać więcej informacji na temat sposobu renderowania składników, stanu składnika i pomocnika tagów `Component`, zobacz następujące artykuły:</span><span class="sxs-lookup"><span data-stu-id="0ed4f-171">For more information on how components are rendered, component state, and the `Component` Tag Helper, see the following articles:</span></span>

* <xref:blazor/hosting-models>
* <xref:blazor/hosting-model-configuration>
