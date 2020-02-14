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
# <a name="aspnet-core-blazor-hosting-model-configuration"></a>Konfiguracja modelu hostingu ASP.NET Core Blazor

Autor [Daniel Roth](https://github.com/danroth27)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

W tym artykule opisano hostowanie konfiguracji modelu.

<!-- For future use:

## Blazor WebAssembly

-->

## <a name="blazor-server"></a>Serwer Blazor

### <a name="integrate-razor-components-into-razor-pages-and-mvc-apps"></a>Integrowanie składników Razor z aplikacjami Razor Pages i MVC

#### <a name="use-components-in-pages-and-views"></a>Używanie składników na stronach i widokach

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

1. W `Startup.ConfigureServices`Zarejestruj usługę serwera Blazor:

   ```csharp
   services.AddServerSideBlazor();
   ```

1. W `Startup.Configure`Dodaj punkt końcowy centrum Blazor do `app.UseEndpoints`:

   ```csharp
   endpoints.MapBlazorHub();
   ```

1. Integruj składniki na dowolną stronę lub widok. Aby uzyskać więcej informacji, zobacz sekcję *integrowanie składników w Razor Pages i aplikacje MVC* w artykule <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps>.

#### <a name="use-routable-components-in-a-razor-pages-app"></a>Używanie składników rutowanych w aplikacji Razor Pages

Aby obsługiwać Routing składników Razor w aplikacjach Razor Pages:

1. Postępuj zgodnie ze wskazówkami zawartymi w sekcji [Używanie składników w stronach i widokach](#use-components-in-pages-and-views) .

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

   W przypadku używania folderu niestandardowego do przechowywania składników aplikacji należy dodać przestrzeń nazw reprezentującą folder do pliku *Pages/_ViewImports. cshtml* . Aby uzyskać więcej informacji, zobacz temat <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps>.

#### <a name="use-routable-components-in-an-mvc-app"></a>Używanie składników rutowanych w aplikacji MVC

Aby zapewnić obsługę routingu składników Razor w aplikacjach MVC:

1. Postępuj zgodnie ze wskazówkami zawartymi w sekcji [Używanie składników w stronach i widokach](#use-components-in-pages-and-views) .

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

   W przypadku używania folderu niestandardowego do przechowywania składników aplikacji należy dodać przestrzeń nazw reprezentującą folder do pliku *views/_ViewImports. cshtml* . Aby uzyskać więcej informacji, zobacz temat <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps>.

### <a name="reflect-the-connection-state-in-the-ui"></a>Odzwierciedlanie stanu połączenia w interfejsie użytkownika

Gdy klient wykryje, że połączenie zostało utracone, do użytkownika jest wyświetlany domyślny interfejs użytkownika, podczas gdy klient próbuje ponownie nawiązać połączenie. Jeśli ponowne połączenie nie powiedzie się, użytkownik otrzymuje opcję ponowienia próby.

Aby dostosować interfejs użytkownika, zdefiniuj element z `id` `components-reconnect-modal` w `<body>` na stronie *_Host. cshtml* Razor:

```cshtml
<div id="components-reconnect-modal">
    ...
</div>
```

W poniższej tabeli opisano klasy CSS stosowane do elementu `components-reconnect-modal`.

| Klasa CSS                       | Wskazuje&hellip; |
| ------------------------------- | ----------------- |
| `components-reconnect-show`     | Utracono połączenie. Klient próbuje ponownie nawiązać połączenie. Pokaż modalne. |
| `components-reconnect-hide`     | Aktywne połączenie zostanie ponownie nawiązane z serwerem. Ukryj modalne. |
| `components-reconnect-failed`   | Ponowne połączenie nie powiodło się, prawdopodobnie z powodu błędu sieci. Aby spróbować ponownie nawiązać połączenie, wywołaj `window.Blazor.reconnect()`. |
| `components-reconnect-rejected` | Odrzucono ponowne połączenie. Serwer został osiągnięty, ale odmówił połączenia, a stan użytkownika na serwerze został utracony. Aby ponownie załadować aplikację, wywołaj `location.reload()`. Ten stan połączenia może skutkować tym, że:<ul><li>Wystąpił awaria w obwodzie po stronie serwera.</li><li>Klient jest odłączony wystarczająco długo, aby serwer mógł porzucić stan użytkownika. Wystąpienia składników, z którymi łączy się użytkownik, są usuwane.</li><li>Serwer zostanie uruchomiony ponownie lub proces roboczy aplikacji zostanie odtworzony.</li></ul> |

### <a name="render-mode"></a>Tryb renderowania

Aplikacje serwera Blazor są domyślnie skonfigurowane, aby skonfigurować interfejs użytkownika na serwerze przed nawiązaniem połączenia z serwerem. Ta konfiguracja jest ustawiana na stronie *_Host. cshtml* Razor:

```cshtml
<body>
    <app>
      <component type="typeof(App)" render-mode="ServerPrerendered" />
    </app>

    <script src="_framework/blazor.server.js"></script>
</body>
```

`RenderMode` określa, czy składnik:

* Jest wstępnie renderowany na stronie.
* Jest renderowany jako statyczny kod HTML na stronie lub zawiera informacje niezbędne do uruchomienia aplikacji Blazor z poziomu agenta użytkownika.

| `RenderMode`        | Opis |
| ------------------- | ----------- |
| `ServerPrerendered` | Renderuje składnik do statycznego kodu HTML i zawiera znacznik dla aplikacji serwera Blazor. Po uruchomieniu agenta użytkownika ten znacznik jest używany do uruchamiania aplikacji Blazor. |
| `Server`            | Renderuje znacznik dla aplikacji serwera Blazor. Dane wyjściowe ze składnika nie są uwzględniane. Po uruchomieniu agenta użytkownika ten znacznik jest używany do uruchamiania aplikacji Blazor. |
| `Static`            | Renderuje składnik do statycznego kodu HTML. |

Renderowanie składników serwera ze statyczną stroną HTML nie jest obsługiwane.

### <a name="render-stateful-interactive-components-from-razor-pages-and-views"></a>Renderuj stanowe składniki interaktywne ze stron Razor i widoków

Można dodać składniki interaktywne ze stanem do strony lub widoku Razor.

Gdy renderuje stronę lub widok:

* Składnik jest wstępnie renderowany przy użyciu strony lub widoku.
* Początkowy stan składnika używany na potrzeby renderowania wstępnego został utracony.
* Nowy stan składnika jest tworzony po nawiązaniu połączenia SignalR.

Następująca strona Razor renderuje składnik `Counter`:

```cshtml
<h1>My Razor Page</h1>

<component type="typeof(Counter)" render-mode="ServerPrerendered" 
    param-InitialValue="InitialValue" />

@code {
    [BindProperty(SupportsGet=true)]
    public int InitialValue { get; set; }
}
```

### <a name="render-noninteractive-components-from-razor-pages-and-views"></a>Renderuj nieinteraktywne składniki ze stron Razor i widoków

Na poniższej stronie Razor składnik `Counter` jest renderowany statycznie z wartością początkową określoną przy użyciu formularza:

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

Ponieważ `MyComponent` jest renderowany statycznie, składnik nie może być interaktywny.

### <a name="configure-the-opno-locsignalr-client-for-opno-locblazor-server-apps"></a>Konfigurowanie klienta SignalR dla aplikacji Blazor Server

Czasami trzeba skonfigurować klienta SignalR używanego przez aplikacje Blazor Server. Na przykład możesz chcieć skonfigurować rejestrowanie na kliencie SignalR, aby zdiagnozować problem z połączeniem.

Aby skonfigurować klienta SignalR w pliku *Pages/_Host. cshtml* :

* Dodaj atrybut `autostart="false"` do tagu `<script>` dla skryptu `blazor.server.js`.
* Wywołaj `Blazor.start` i przekaż obiekt konfiguracji, który określa SignalR konstruktora.

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
