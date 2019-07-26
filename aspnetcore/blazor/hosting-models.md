---
title: ASP.NET Core modele hostingowe Blazor
author: guardrex
description: Poznaj modele hostingu po stronie klienta i serwera Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 07/01/2019
uid: blazor/hosting-models
ms.openlocfilehash: 9dd96ff6e3539bf1c3e932b33776b16d0fbb2d34
ms.sourcegitcommit: 849af69ee3c94cdb9fd8fa1f1bb8f5a5dda7b9eb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/22/2019
ms.locfileid: "68371793"
---
# <a name="aspnet-core-blazor-hosting-models"></a>ASP.NET Core modele hostingowe Blazor

Autor [Daniel Roth](https://github.com/danroth27)

Blazor to platforma internetowa przeznaczona do uruchamiania po stronie klienta w przeglądarce w środowisku uruchomieniowym .NET runtime (*Blazor Client*) lub po stronie serwera w ASP.NET Core (*Blazor po stronie serwera*). [](https://webassembly.org/) Niezależnie od modelu hostingu modele aplikacji i składników *są takie same*.

Aby utworzyć projekt dla modeli hostingu opisanych w tym artykule, zobacz <xref:blazor/get-started>.

## <a name="client-side"></a>Po stronie klienta

Główny model hostingu dla Blazor jest uruchomiony po stronie klienta w przeglądarce w programie webassembly. Aplikacja Blazor, jej zależności i środowisko uruchomieniowe platformy .NET są pobierane do przeglądarki. Aplikacja jest wykonywana bezpośrednio w wątku interfejsu użytkownika przeglądarki. Aktualizacje interfejsu użytkownika i obsługa zdarzeń są wykonywane w ramach tego samego procesu. Zasoby aplikacji są wdrażane jako pliki statyczne na serwerze sieci Web lub usłudze obsługującej zawartość statyczną dla klientów.

![Blazor po stronie klienta: Aplikacja Blazor jest uruchamiana w wątku interfejsu użytkownika w przeglądarce.](hosting-models/_static/client-side.png)

Aby utworzyć aplikację Blazor przy użyciu modelu hostingu po stronie klienta, użyj jednego z następujących szablonów:

* **Blazor (po stronie klienta)** ([dotnet New blazor](/dotnet/core/tools/dotnet-new)) &ndash; Wdrożony jako zestaw plików statycznych.
* **Blazor (hostowana ASP.NET Core)** ([dotnet New blazorhosted](/dotnet/core/tools/dotnet-new)) &ndash; Obsługiwane przez serwer ASP.NET Core. Aplikacja ASP.NET Core udostępnia klientom aplikację Blazor. Aplikacja po stronie klienta Blazor może współdziałać z serwerem za pośrednictwem sieci przy użyciu wywołań interfejsu [](xref:signalr/introduction)API sieci Web lub sygnalizującego.

Szablony obejmują skrypt *blazor. webassembly. js* , który obsługuje:

* Pobieranie środowiska uruchomieniowego platformy .NET, aplikacji i zależności aplikacji.
* Inicjowanie środowiska uruchomieniowego w celu uruchomienia aplikacji.

Model hostingu po stronie klienta oferuje kilka korzyści:

* Nie istnieje zależność po stronie serwera .NET. Aplikacja jest w pełni funkcjonalna po pobraniu do klienta programu.
* Zasoby i możliwości klienta są w pełni wykorzystywane.
* Zadania są Odciążone z serwera do klienta programu.
* Serwer sieci Web ASP.NET Core nie jest wymagany do hostowania aplikacji. Możliwe są scenariusze wdrażania bezserwerowego (na przykład obsługujące aplikację z sieci CDN).

Downsides do hostingu po stronie klienta:

* Aplikacja jest ograniczona do możliwości przeglądarki.
* Wymagany jest sprzęt i oprogramowanie klienta (na przykład obsługa zestawu webassembly).
* Rozmiar pobieranych plików jest większy i ładowanie aplikacji trwa dłużej.
* Środowisko uruchomieniowe platformy .NET i obsługa narzędzi są mniej dojrzałe. Na przykład istnieją ograniczenia dotyczące obsługi [.NET Standard](/dotnet/standard/net-standard) i debugowania.

## <a name="server-side"></a>Po stronie serwera

W modelu hostingu po stronie serwera aplikacja jest wykonywana na serwerze z poziomu aplikacji ASP.NET Core. Aktualizacje interfejsu użytkownika, obsługa zdarzeń i wywołania języka JavaScript są obsługiwane przez [](xref:signalr/introduction) połączenie sygnalizujące.

![Przeglądarka współdziała z aplikacją (hostowaną wewnątrz aplikacji ASP.NET Core) na serwerze za pośrednictwem połączenia sygnalizującego.](hosting-models/_static/server-side.png)

Aby utworzyć aplikację Blazor przy użyciu modelu hostingu po stronie serwera, użyj szablonu aplikacji ASP.NET Core **Blazor Server** ([dotnet New blazorserverside](/dotnet/core/tools/dotnet-new)). Aplikacja ASP.NET Core obsługuje aplikację po stronie serwera i tworzy punkt końcowy sygnalizujący, do którego klienci nawiązują połączenie.

Aplikacja ASP.NET Core odwołuje się do `Startup` klasy aplikacji do dodania:

* Usługi po stronie serwera.
* Aplikacja do potoku obsługi żądania.

Skrypt&dagger; *blazor. Server. js* nawiązuje połączenie z klientem. Jest on odpowiedzialny za utrzymanie i przywrócenie stanu aplikacji zgodnie z wymaganiami (na przykład w przypadku utraconego połączenia sieciowego).

Model hostingu po stronie serwera oferuje kilka korzyści:

* Rozmiar pobieranych plików jest znacznie mniejszy niż aplikacja po stronie klienta, a aplikacja jest znacznie szybsza.
* Aplikacja w pełni wykorzystuje możliwości serwera, w tym używanie dowolnego zgodnego z platformą .NET Core interfejsów API.
* Program .NET Core na serwerze jest używany do uruchamiania aplikacji, więc istniejące narzędzia platformy .NET, takie jak debugowanie, działają zgodnie z oczekiwaniami.
* Klienci zubożoni są obsługiwani. Na przykład aplikacje po stronie serwera współpracują z przeglądarkami, które nie obsługują zestawu webassembly i na urządzeniach z ograniczeniami zasobów.
* Baza danych platformy .NET/C# kodu aplikacji, w tym kod składnika aplikacji, nie jest obsługiwana dla klientów.

Downsides do hostingu po stronie serwera:

* Wyższe opóźnienia zwykle istnieją. Każda interakcja użytkownika obejmuje przeskok sieci.
* Brak obsługi offline. Jeśli połączenie z klientem zakończy się niepowodzeniem, aplikacja przestanie działać.
* Skalowalność jest wyzwaniem dla aplikacji z wieloma użytkownikami. Serwer musi zarządzać wieloma połączeniami klientów i obsługiwać stan klienta.
* Do obsłużynia aplikacji wymagany jest serwer ASP.NET Core. Scenariusze wdrażania bez użycia serwera nie są możliwe (na przykład w celu obsługi aplikacji z sieci CDN).

&dagger;Skrypt *blazor. Server. js* jest obsługiwany z zasobów osadzonych w ASP.NET Core udostępnionej platformie.

### <a name="reconnection-to-the-same-server"></a>Ponowne nawiązywanie połączenia z tym samym serwerem

Blazor aplikacje po stronie serwera wymagają aktywnego połączenia z serwerem. Jeśli połączenie zostanie utracone, aplikacja spróbuje ponownie nawiązać połączenie z serwerem. O ile stan klienta nadal znajduje się w pamięci, sesja klienta zostaje wznowiona bez utraty stanu.
 
Gdy klient wykryje, że połączenie zostało utracone, do użytkownika jest wyświetlany domyślny interfejs użytkownika, podczas gdy klient próbuje ponownie nawiązać połączenie. Jeśli ponowne połączenie nie powiedzie się, użytkownik otrzymuje opcję ponowienia próby. Aby dostosować interfejs użytkownika, zdefiniuj element `components-reconnect-modal` jako jego `id` na stronie Razor *_Host. cshtml* . Klient aktualizuje ten element za pomocą jednej z następujących klas CSS w oparciu o stan połączenia:
 
* `components-reconnect-show`&ndash; Pokaż interfejs użytkownika wskazujący, że połączenie zostało utracone, a klient próbuje ponownie nawiązać połączenie.
* `components-reconnect-hide`&ndash; Klient ma aktywne połączenie, Ukryj interfejs użytkownika.
* `components-reconnect-failed`&ndash; Ponowne nawiązywanie połączenia nie powiodło się. Aby spróbować ponownie nawiązać połączenie, `window.Blazor.reconnect()`Wywołaj polecenie.

### <a name="stateful-reconnection-after-prerendering"></a>Stanowe Ponowne nawiązywanie połączenia po przeprowadzeniu prerenderowania
 
Aplikacje po stronie serwera Blazor są domyślnie skonfigurowane w taki sposób, aby były oczyszczane przez interfejs użytkownika na serwerze przed nawiązaniem połączenia z serwerem. Ta konfiguracja jest ustawiana na stronie *_Host. cshtml* Razor:
 
```cshtml
<body>
    <app>@(await Html.RenderComponentAsync<App>())</app>
 
    <script src="_framework/blazor.server.js"></script>
</body>
```
 
Klient ponownie nawiązuje połączenie z serwerem z tym samym stanem, który został użyty do wygenerowania aplikacji. Jeśli stan aplikacji nadal znajduje się w pamięci, stan składnika nie jest ponownie renderowany po nawiązaniu połączenia z sygnałem.

### <a name="render-stateful-interactive-components-from-razor-pages-and-views"></a>Renderuj stanowe składniki interaktywne ze stron Razor i widoków
 
Można dodać składniki interaktywne ze stanem do strony lub widoku Razor. Gdy renderuje stronę lub widok, składnik jest wstępnie renderowany. Następnie aplikacja ponownie nawiązuje połączenie ze stanem składnika po ustanowieniu połączenia klienta, o ile stan nadal znajduje się w pamięci.
 
Na przykład następująca strona Razor renderuje `Counter` składnik z początkową liczbą określoną za pomocą formularza:
 
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

### <a name="detect-when-the-app-is-prerendering"></a>Wykryj, kiedy aplikacja jest przedrenderowana
 
[!INCLUDE[](~/includes/blazor-prerendering.md)]

### <a name="configure-the-signalr-client-for-blazor-server-side-apps"></a>Konfigurowanie klienta sygnalizującego dla aplikacji po stronie serwera Blazor
 
Czasami trzeba skonfigurować klienta sygnalizującego używany przez aplikacje po stronie serwera Blazor. Na przykład może być konieczne skonfigurowanie rejestrowania na kliencie sygnalizującego, aby zdiagnozować problem z połączeniem.
 
Aby skonfigurować klienta sygnalizującego w pliku *Pages/_Host. cshtml* :

* Dodaj atrybut do znacznika dla skryptu *blazor. Server. js.* `<script>` `autostart="false"`
* Wywoływanie `Blazor.start` i przekazywanie obiektu konfiguracji, który określa konstruktora sygnalizującego.
 
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
