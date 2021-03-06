---
title: Integrowanie składników ASP.NET Core Razor z aplikacjami Razor Pages i MVC
author: guardrex
description: Dowiedz się więcej na temat scenariuszy powiązań danych dla składników i elementów DOM w aplikacjach Blazor.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/17/2020
no-loc:
- Blazor
- SignalR
uid: blazor/integrate-components
ms.openlocfilehash: cf6056e0985d5433bddecac8dd183ca3f4c2af5b
ms.sourcegitcommit: 91dc1dd3d055b4c7d7298420927b3fd161067c64
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/24/2020
ms.locfileid: "80218937"
---
# <a name="integrate-aspnet-core-razor-components-into-razor-pages-and-mvc-apps"></a>Integrowanie składników ASP.NET Core Razor z aplikacjami Razor Pages i MVC

Autorzy [Luke Latham](https://github.com/guardrex) i [Daniel Roth](https://github.com/danroth27)

Składniki Razor można zintegrować z aplikacjami Razor Pages i MVC. Gdy strona lub widok jest renderowany, składniki mogą być wstępnie renderowane w tym samym czasie.

## <a name="prepare-the-app-to-use-components-in-pages-and-views"></a>Przygotowywanie aplikacji do używania składników na stronach i widokach

Istniejąca aplikacja Razor Pages lub MVC może zintegrować składniki Razor ze stronami i widokami:

1. W pliku układu aplikacji ( *_Layout. cshtml*):

   * Dodaj następujący tag `<base>` do elementu `<head>`:

     ```html
     <base href="~/" />
     ```

     Wartość `href` ( *Ścieżka podstawowa aplikacji*) w poprzednim przykładzie założono, że aplikacja znajduje się w ścieżce adresu URL katalogu głównego (`/`). Jeśli aplikacja jest aplikacją podrzędną, postępuj zgodnie ze wskazówkami w sekcji *Ścieżka podstawowa aplikacji* w artykule <xref:host-and-deploy/blazor/index#app-base-path>.

     Plik *_Layout. cshtml* znajduje się w folderze *Pages/shared* w aplikacji Razor Pages lub *widokach/folderze udostępnionym* w aplikacji MVC.

   * Dodaj tag `<script>` dla skryptu *blazor. Server. js* bezpośrednio przed tagiem zamykającym `</body>`:

     ```html
     <script src="_framework/blazor.server.js"></script>
     ```

     Struktura dodaje skrypt *blazor. Server. js* do aplikacji. Nie trzeba ręcznie dodawać skryptu do aplikacji.

1. Dodaj plik *_Imports. Razor* do folderu głównego projektu o następującej zawartości (Zmień ostatnią przestrzeń nazw, `MyAppNamespace`, na przestrzeń nazw aplikacji):

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

1. W `Startup.ConfigureServices`Zarejestruj usługę Blazor Server:

   ```csharp
   services.AddServerSideBlazor();
   ```

1. W `Startup.Configure`Dodaj punkt końcowy centrum Blazor do `app.UseEndpoints`:

   ```csharp
   endpoints.MapBlazorHub();
   ```

1. Integruj składniki na dowolną stronę lub widok. Aby uzyskać więcej informacji, zobacz [składniki renderowania ze strony lub widoku](#render-components-from-a-page-or-view) .

## <a name="use-routable-components-in-a-razor-pages-app"></a>Używanie składników rutowanych w aplikacji Razor Pages

*Ta sekcja dotyczy dodawania składników, które są bezpośrednio trasowane z żądań użytkowników.*

Aby obsługiwać Routing składników Razor w aplikacjach Razor Pages:

1. Postępuj zgodnie ze wskazówkami zawartymi w sekcji [przygotowanie aplikacji do używania składników w stronach i widokach](#prepare-the-app-to-use-components-in-pages-and-views) .

1. Dodaj plik *App. Razor* do katalogu głównego projektu z następującą zawartością:

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

1. Dodaj plik *_Host. cshtml* do folderu *stron* o następującej zawartości:

   ```cshtml
   @page "/blazor"
   @{
       Layout = "_Layout";
   }

   <app>
       <component type="typeof(App)" render-mode="ServerPrerendered" />
   </app>
   ```

   Składniki używają udostępnionego pliku *_Layout. cshtml* dla ich układu.

1. Dodaj trasę o niskim priorytecie dla strony *_Host. cshtml* do konfiguracji punktu końcowego w `Startup.Configure`:

   ```csharp
   app.UseEndpoints(endpoints =>
   {
       ...

       endpoints.MapFallbackToPage("/_Host");
   });
   ```

1. Dodaj składniki routingu do aplikacji. Na przykład:

   ```razor
   @page "/counter"

   <h1>Counter</h1>

   ...
   ```

   Aby uzyskać więcej informacji na temat przestrzeni nazw, zobacz sekcję [przestrzenie nazw składników](#component-namespaces) .

## <a name="use-routable-components-in-an-mvc-app"></a>Używanie składników rutowanych w aplikacji MVC

*Ta sekcja dotyczy dodawania składników, które są bezpośrednio trasowane z żądań użytkowników.*

Aby zapewnić obsługę routingu składników Razor w aplikacjach MVC:

1. Postępuj zgodnie ze wskazówkami zawartymi w sekcji [przygotowanie aplikacji do używania składników w stronach i widokach](#prepare-the-app-to-use-components-in-pages-and-views) .

1. Dodaj plik *App. Razor* do katalogu głównego projektu z następującą zawartością:

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

1. Dodaj plik *_Host. cshtml* do folderu *widoki/główne* z następującą zawartością:

   ```cshtml
   @{
       Layout = "_Layout";
   }

   <app>
       <component type="typeof(App)" render-mode="ServerPrerendered" />
   </app>
   ```

   Składniki używają udostępnionego pliku *_Layout. cshtml* dla ich układu.

1. Dodaj akcję do kontrolera macierzystego:

   ```csharp
   public IActionResult Blazor()
   {
      return View("_Host");
   }
   ```

1. Dodaj trasę o niskim priorytecie dla akcji kontrolera, która zwraca widok *_Host. cshtml* do konfiguracji punktu końcowego w `Startup.Configure`:

   ```csharp
   app.UseEndpoints(endpoints =>
   {
       ...

       endpoints.MapFallbackToController("Blazor", "Home");
   });
   ```

1. Utwórz folder *strony* i Dodaj składniki do obsługi routingu do aplikacji. Na przykład:

   ```razor
   @page "/counter"

   <h1>Counter</h1>

   ...
   ```

   Aby uzyskać więcej informacji na temat przestrzeni nazw, zobacz sekcję [przestrzenie nazw składników](#component-namespaces) .

## <a name="component-namespaces"></a>Przestrzenie nazw składników

W przypadku używania folderu niestandardowego do przechowywania składników aplikacji należy dodać przestrzeń nazw reprezentującą folder do strony/widoku lub pliku *_ViewImports. cshtml* . W poniższym przykładzie:

* Zmień `MyAppNamespace` na przestrzeń nazw aplikacji.
* Jeśli folder o nazwie *Components* nie jest używany do przechowywania składników, należy zmienić `Components` do folderu, w którym znajdują się składniki.

```cshtml
@using MyAppNamespace.Components
```

Plik *_ViewImports. cshtml* znajduje się w folderze *strony* aplikacji Razor Pages lub folderu *widoki* aplikacji MVC.

Aby uzyskać więcej informacji, zobacz <xref:blazor/components#import-components>.

## <a name="render-components-from-a-page-or-view"></a>Renderuj składniki ze strony lub widoku

*Ta sekcja dotyczy dodawania składników do stron lub widoków, w których składniki nie są bezpośrednio trasowane z żądań użytkownika.*

Aby renderować składnik ze strony lub widoku, użyj [pomocnika tagów składnika](xref:mvc/views/tag-helpers/builtin-th/component-tag-helper).

Aby uzyskać więcej informacji na temat sposobu renderowania składników, stanu składnika i pomocnika tagów `Component`, zobacz następujące artykuły:

* <xref:blazor/hosting-models>
* <xref:blazor/hosting-model-configuration>
* <xref:mvc/views/tag-helpers/builtin-th/component-tag-helper>
