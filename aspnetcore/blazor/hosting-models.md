---
title: ASP.NET Core Blazor hostingu modeli
author: guardrex
description: Dowiedz się, Blazor po stronie klienta i po stronie serwera, hostowania modeli.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 06/14/2019
uid: blazor/hosting-models
ms.openlocfilehash: c794daf6f33138c57500a04116a3d172f0201bd5
ms.sourcegitcommit: 4ef0362ef8b6e5426fc5af18f22734158fe587e1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/17/2019
ms.locfileid: "67152797"
---
# <a name="aspnet-core-blazor-hosting-models"></a>ASP.NET Core Blazor hostingu modeli

Przez [Daniel Roth](https://github.com/danroth27)

Blazor to struktura sieci web przeznaczony do działania po stronie klienta w przeglądarce na [format WebAssembly](http://webassembly.org/)— na podstawie środowiska uruchomieniowego .NET (*Blazor po stronie klienta*) lub po stronie serwera w programie ASP.NET Core (*Blazor po stronie serwera* ). Niezależnie od tego modelu, aplikacji i składników modelach hostingu *są takie same*.

Aby utworzyć projekt modelach hostingu opisane w tym artykule, zobacz <xref:blazor/get-started>.

## <a name="client-side"></a>Po stronie klienta

Jednostki modelu hostowania Blazor jest uruchomiona po stronie klienta w przeglądarce na format WebAssembly. Aplikacja Blazor, jego zależności i środowisko uruchomieniowe platformy .NET są pobierane do przeglądarki. Aplikacja jest wykonywane bezpośrednio w przeglądarce wątku interfejsu użytkownika. Aktualizacje interfejsu użytkownika i obsługa zdarzeń odbywa się w obrębie tego samego procesu. Zasoby aplikacji są wdrażane jako pliki statyczne z serwera sieci web lub usługą umożliwia obsługę zawartości statycznej dla klientów.

![Blazor po stronie klienta: Aplikacja Blazor jest uruchamiana w wątku interfejsu użytkownika w przeglądarce.](hosting-models/_static/client-side.png)

Aby utworzyć aplikację Blazor przy użyciu modelu hostingu w sieci po stronie klienta, użyj jednej z następujących szablonów:

* **Blazor (po stronie klienta)** ([blazor nowe dotnet](/dotnet/core/tools/dotnet-new)) &ndash; wdrożony jako zbiór plików statycznych.
* **Blazor (ASP.NET Core hostowane)** ([blazorhosted nowe dotnet](/dotnet/core/tools/dotnet-new)) &ndash; obsługiwanej przez serwer programu ASP.NET Core. Aplikacja platformy ASP.NET Core udostępnia aplikacji Blazor klientom. Aplikacja Blazor po stronie klienta mogą wchodzić w interakcje z serwerem za pośrednictwem sieci przy użyciu wywołań interfejsu API sieci web lub [SignalR](xref:signalr/introduction).

Szablony zawierają *blazor.webassembly.js* skrypt, który obsługuje:

* Pobieranie, środowisko uruchomieniowe platformy .NET, aplikacji i zależności aplikacji.
* Inicjalizacja środowiska uruchomieniowego, aby uruchomić aplikację.

Model hostingu w sieci po stronie klienta oferuje wiele korzyści. Blazor po stronie klienta:

* Ma nie zależności po stronie serwera .NET.
* W pełni wykorzystuje zasoby klienta i możliwości.
* Odciążanie pracować z serwera do klienta.
* Obsługuje scenariusze w trybie offline.

Brak wad hostingu po stronie klienta. Blazor po stronie klienta:

* Ogranicza ją do możliwości przeglądarki.
* Wymaga klienta obsługującego sprzętu i oprogramowania (na przykład w przypadku, format WebAssembly Obsługa).
* Ma większy rozmiar pobierania i aplikacji dłuższy czas ładowania.
* Ma mniejszą dla dorosłych, środowisko uruchomieniowe platformy .NET oraz narzędzia do obsługi (na przykład ograniczenia w [.NET Standard](/dotnet/standard/net-standard) pomocy technicznej i debugowania).

## <a name="server-side"></a>Po stronie serwera

Za pomocą modelu hostingu po stronie serwera aplikacji jest wykonywana na serwerze z poziomu aplikacji ASP.NET Core. Aktualizacje interfejsu użytkownika, obsługa zdarzeń i wywołania języka JavaScript są obsługiwane za pośrednictwem [SignalR](xref:signalr/introduction) połączenia.

![Przeglądarka wchodzi w interakcję z aplikacją (obsługiwana wewnątrz aplikacji ASP.NET Core) na serwerze za pośrednictwem połączenia SignalR.](hosting-models/_static/server-side.png)

Aby utworzyć aplikację Blazor przy użyciu modelu hostingu w sieci po stronie serwera, należy użyć platformy ASP.NET Core **Blazor (po stronie serwera)** szablonu ([blazorserverside nowe dotnet](/dotnet/core/tools/dotnet-new)). Aplikacja platformy ASP.NET Core obsługuje aplikacji po stronie serwera i ustawia punkt końcowy SignalR, gdy klienci łączą.

Aplikacja platformy ASP.NET Core odwołuje się do aplikacji `Startup` klasy do dodania:

* Usługi po stronie serwera.
* Aplikacja do żądania obsługi potoku.

*Blazor.server.js* skryptu&dagger; nawiązuje połączenie z klientem. To aplikacja odpowiada za utrwalanie i przywracanie stanu aplikacji, zgodnie z potrzebami (na przykład w przypadku połączenia sieciowego utracone).

Model hostingu w sieci po stronie serwera oferuje wiele korzyści:

* Rozmiar aplikacji znacznie mniejszy niż aplikacji po stronie klienta i ładuje znacznie szybciej.
* Pełne wykorzystanie możliwości serwera, takie jak przy użyciu zgodnych interfejsów API dowolnej platformy .NET Core.
* Działa na platformie .NET Core w serwer, więc .NET istniejących narzędzi, takich jak debugowanie, działa zgodnie z oczekiwaniami.
* Działa z elastycznej klientów. Na przykład współpracuje z przeglądarek, które nie obsługują format WebAssembly i zasobów ograniczone urządzeń.
* .NET /C# bazy kodu, w tym kodu składnika aplikacji, nie jest obsługiwane dla klientów.

Istnieją wad po stronie serwera hostingu:

* Czas oczekiwania: Każdy interakcja użytkownika obejmuje przeskok sieci.
* Brak obsługi w trybie offline: Jeśli połączenie klienta zakończy się niepowodzeniem, aplikacja przestaje działać.
* Ograniczoną skalowalność: Serwer musi zarządzać wieloma połączeń klientów i obsługi stanu klienta.
* Serwer programu ASP.NET Core jest wymagana do obsługi aplikacji. Wdrożenie bez serwera (na przykład z sieci CDN) nie jest możliwe.

&dagger;*Blazor.server.js* skrypt jest obsługiwany z zasobu osadzonego w udostępnionej platformy ASP.NET Core.

### <a name="reconnection-to-the-same-server"></a>Ponowne nawiązanie połączenia z tym samym serwerem

Aplikacje serwerowe Blazor wymagają aktywnego połączenia SignalR do serwera. Jeśli połączenie zostanie przerwane, aplikacja próbuje ponownie połączyć się z serwerem. Tak długo, jak stan klienta jest nadal w pamięci, bez utraty stanu wznawia działanie sesji klienta.
 
Klient wykryje, że połączenie zostało utracone, domyślny interfejs użytkownika jest wyświetlany użytkownikowi, gdy klient próbuje ponownie połączyć się. W przypadku niepowodzenia ponowne nawiązanie połączenia użytkownika podano opcję, aby spróbować ponownie. Aby dostosować interfejsu użytkownika, należy zdefiniować element z `components-reconnect-modal` jako jego `id`. Klient aktualizuje tego elementu z jedną z następujących klas CSS na podstawie stanu połączenia:
 
* `components-reconnect-show` &ndash; Umożliwia wyświetlenie interfejsu użytkownika, aby wskazać, połączenie zostało utracone, a klient próbuje połączyć.
* `components-reconnect-hide` &ndash; Klient ma aktywne połączenie, Ukryj interfejsu użytkownika.
* `components-reconnect-failed` &ndash; Ponowne nawiązanie połączenia nie powiodło się. Próba ponownego połączenia ponownie, należy wywołać `window.Blazor.reconnect()`.

### <a name="stateful-reconnection-after-prerendering"></a>Stanowe ponownego łączenia po prerendering
 
Blazor po stronie serwera aplikacji są konfigurowane domyślnie prerender interfejsu użytkownika na serwerze, zanim zostanie nawiązane połączenie klienta z serwerem. Tę opcję można zdefiniować *_Host.cshtml* strona Razor:
 
```cshtml
<body>
    <app>@(await Html.RenderComponentAsync<App>())</app>
 
    <script src="_framework/blazor.server.js"></script>
</body>
```
 
Klient ponownie nawiąże połączenie z serwerem za pomocą takiego samego stanu, który został użyty do prerender aplikacji. Jeśli stan aplikacji jest nadal w pamięci, stan składnika nie jest rerendered po nawiązaniu połączenia SignalR.

### <a name="render-stateful-interactive-components-from-razor-pages-and-views"></a>Renderowanie stanowych interaktywnych składników z widoków i stron Razor
 
Stanowe interaktywnych składników można dodać do strony Razor lub widoku. Gdy powoduje wyświetlenie strony lub widoku składnika jest prerendered z nim. Aplikacja następnie ponownie nawiązuje połączenie stan składnika po ustanowieniu połączenia klienta, tak długo, jak stan jest nadal w pamięci.
 
Na przykład następująca strona Razor renderuje składnikiem licznika z liczbą początkowej, który jest określony za pomocą formularza:
 
```cshtml
<h1>My Razor Page</h1>

<form>
    <input type="number" asp-for="InitialCount" />
    <button type="submit">Set initial count</button>
</form>
 
@(await Html.RenderComponentAsync<Counter>(new { InitialCount = InitialCount }))
 
@code {
    [BindProperty(SupportsGet=true)]
    public int InitialCount { get; set; }
}
```

### <a name="detect-when-the-app-is-prerendering"></a>Wykryj, kiedy aplikacja jest prerendering
 
[!INCLUDE[](~/includes/blazor-prerendering.md)]

### <a name="configure-the-signalr-client-for-blazor-server-side-apps"></a>Konfigurowanie klienta SignalR Blazor po stronie serwera aplikacji
 
Czasami trzeba skonfigurować używane przez aplikacje serwerowe Blazor klienta SignalR. Na przykład można skonfigurować klienta SignalR, aby zdiagnozować problem z połączeniem.
 
Aby skonfigurować klienta SignalR w *stron /\_Host.cshtml* pliku:

* Dodaj `autostart="false"` atrybutu `<script>` tagu dla *blazor.server.js* skryptu.
* Wywołaj `Blazor.start` i przekaż obiekt konfiguracji, który określa konstruktora SignalR.
 
```html
<script src="_framework/blazor.server.js" autostart="false"></script>
<script>
  Blazor.start({
    configureSignalR: function (builder) {
      builder.configureLogging(2); // LogLevel.Information
    }
  });
</script>
```

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:blazor/get-started>
* <xref:signalr/introduction>
