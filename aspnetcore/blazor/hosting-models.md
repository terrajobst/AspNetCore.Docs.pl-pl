---
title: ASP.NET Core modele hostingowe Blazor
author: guardrex
description: Poznaj modele hostingu Blazor webassembly i Blazor Server.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 11/03/2019
uid: blazor/hosting-models
ms.openlocfilehash: d1b9e6ab7ba93c00a569be309f2334df9e3f4140
ms.sourcegitcommit: e5d4768aaf85703effb4557a520d681af8284e26
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/05/2019
ms.locfileid: "73616589"
---
# <a name="aspnet-core-blazor-hosting-models"></a>ASP.NET Core modele hostingowe Blazor

Autor [Daniel Roth](https://github.com/danroth27)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Blazor to platforma internetowa, która umożliwia uruchamianie po stronie klienta w przeglądarce w środowisku [](https://webassembly.org/)uruchomieniowym .NET runtime (*Blazor webassembly*) lub po stronie serwera w ASP.NET Core (*Blazor Server*). Niezależnie od modelu hostingu modele aplikacji i składników *są takie same*.

Aby utworzyć projekt dla modeli hostingu opisanych w tym artykule, zobacz <xref:blazor/get-started>.

## <a name="blazor-webassembly"></a>Zestaw WebAssembly Blazor

Główny model hostingu dla Blazor jest uruchomiony po stronie klienta w przeglądarce w programie webassembly. Aplikacja Blazor, jej zależności i środowisko uruchomieniowe platformy .NET są pobierane do przeglądarki. Aplikacja jest wykonywana bezpośrednio w wątku interfejsu użytkownika przeglądarki. Aktualizacje interfejsu użytkownika i obsługa zdarzeń są wykonywane w ramach tego samego procesu. Zasoby aplikacji są wdrażane jako pliki statyczne na serwerze sieci Web lub usłudze obsługującej zawartość statyczną dla klientów.

![Blazor webassembly: aplikacja Blazor jest uruchamiana w wątku interfejsu użytkownika w przeglądarce.](hosting-models/_static/blazor-webassembly.png)

Aby utworzyć aplikację Blazor przy użyciu modelu hostingu po stronie klienta, użyj szablonu **aplikacji Blazor webassembly** ([dotnet New blazorwasm](/dotnet/core/tools/dotnet-new)).

Po wybraniu szablonu **aplikacji Blazor webassembly** można skonfigurować aplikację do korzystania z zaplecza ASP.NET Core, zaznaczając pole wyboru **hostowane ASP.NET Core** (polecenie[dotnet New blazorwasm--Hosted](/dotnet/core/tools/dotnet-new)). Aplikacja ASP.NET Core udostępnia klientom aplikację Blazor. Aplikacja webassembly Blazor może współdziałać z serwerem za pośrednictwem sieci przy użyciu wywołań interfejsu API sieci Web lub [sygnalizującego](xref:signalr/introduction).

Szablony obejmują skrypt *blazor. webassembly. js* , który obsługuje:

* Pobieranie środowiska uruchomieniowego platformy .NET, aplikacji i zależności aplikacji.
* Inicjowanie środowiska uruchomieniowego w celu uruchomienia aplikacji.

Model hostingu zestawu webBlazor oferuje kilka korzyści:

* Nie istnieje zależność po stronie serwera .NET. Aplikacja jest w pełni funkcjonalna po pobraniu do klienta programu.
* Zasoby i możliwości klienta są w pełni wykorzystywane.
* Zadania są Odciążone z serwera do klienta programu.
* Serwer sieci Web ASP.NET Core nie jest wymagany do hostowania aplikacji. Możliwe są scenariusze wdrażania bezserwerowego (na przykład obsługujące aplikację z sieci CDN).

Istnieją Downsides do hostingu Blazor webassembly:

* Aplikacja jest ograniczona do możliwości przeglądarki.
* Wymagany jest sprzęt i oprogramowanie klienta (na przykład obsługa zestawu webassembly).
* Rozmiar pobieranych plików jest większy i ładowanie aplikacji trwa dłużej.
* Środowisko uruchomieniowe platformy .NET i obsługa narzędzi są mniej dojrzałe. Na przykład istnieją ograniczenia dotyczące obsługi [.NET Standard](/dotnet/standard/net-standard) i debugowania.

## <a name="blazor-server"></a>Serwer Blazor

W modelu hostingu serwera Blazor aplikacja jest wykonywana na serwerze z poziomu aplikacji ASP.NET Core. Aktualizacje interfejsu użytkownika, obsługa zdarzeń i wywołania języka JavaScript są obsługiwane przez połączenie [sygnalizujące](xref:signalr/introduction) .

![Przeglądarka współdziała z aplikacją (hostowaną wewnątrz aplikacji ASP.NET Core) na serwerze za pośrednictwem połączenia sygnalizującego.](hosting-models/_static/blazor-server.png)

Aby utworzyć aplikację Blazor przy użyciu modelu hostingu serwera Blazor, użyj szablonu aplikacji ASP.NET Core **Blazor Server** ([dotnet New blazorserver](/dotnet/core/tools/dotnet-new)). Aplikacja ASP.NET Core hostuje aplikację serwera Blazor i tworzy punkt końcowy sygnalizujący, do którego klienci nawiązują połączenie.

Aplikacja ASP.NET Core odwołuje się do klasy `Startup` aplikacji do dodania:

* Usługi po stronie serwera.
* Aplikacja do potoku obsługi żądania.

Skrypt *blazor. Server. js*&dagger; nawiązuje połączenie z klientem. Jest on odpowiedzialny za utrzymanie i przywrócenie stanu aplikacji zgodnie z wymaganiami (na przykład w przypadku utraconego połączenia sieciowego).

Model hostingu serwera Blazor oferuje kilka korzyści:

* Rozmiar pobieranych plików jest znacznie mniejszy niż aplikacja Blazor webassembly, a aplikacja jest znacznie szybsza.
* Aplikacja w pełni wykorzystuje możliwości serwera, w tym używanie dowolnego zgodnego z platformą .NET Core interfejsów API.
* Program .NET Core na serwerze jest używany do uruchamiania aplikacji, więc istniejące narzędzia platformy .NET, takie jak debugowanie, działają zgodnie z oczekiwaniami.
* Klienci zubożoni są obsługiwani. Na przykład aplikacje serwera Blazor działają z przeglądarkami, które nie obsługują zestawu webassembly ani urządzeń z ograniczeniami zasobów.
* Baza danych platformy .NET/C# kodu aplikacji, w tym kod składnika aplikacji, nie jest obsługiwana dla klientów.

Istnieją Downsides do hostingu serwera Blazor:

* Wyższe opóźnienia zwykle istnieją. Każda interakcja użytkownika obejmuje przeskok sieci.
* Brak obsługi offline. Jeśli połączenie z klientem zakończy się niepowodzeniem, aplikacja przestanie działać.
* Skalowalność jest wyzwaniem dla aplikacji z wieloma użytkownikami. Serwer musi zarządzać wieloma połączeniami klientów i obsługiwać stan klienta.
* Do obsłużynia aplikacji wymagany jest serwer ASP.NET Core. Scenariusze wdrażania bez użycia serwera nie są możliwe (na przykład w celu obsługi aplikacji z sieci CDN).

&dagger;skrypt *blazor. Server. js* jest obsługiwany z zasobów osadzonych w ASP.NET Core współdzielonej platformie.

### <a name="comparison-to-server-rendered-ui"></a>Porównanie z renderowanym przez serwer interfejsem użytkownika

Jednym ze sposobów zrozumienia aplikacji Blazor Server jest zrozumienie, jak różni się od tradycyjnych modeli na potrzeby renderowania interfejsu użytkownika w aplikacjach ASP.NET Core przy użyciu widoków Razor lub Razor Pages. Oba modele używają języka Razor do opisywania zawartości HTML, ale znacząco różnią się sposobem renderowania znaczników.

Gdy strona Razor lub widok jest renderowany, każdy wiersz kodu Razor emituje kod HTML w postaci tekstowej. Po wyrenderowaniu serwer usuwa wystąpienie strony lub widoku, w tym dowolny utworzony stan. Gdy występuje inne żądanie dotyczące strony, na przykład w przypadku niepowodzenia walidacji serwera i wyświetlenia podsumowania walidacji:

* Cała strona zostanie ponownie przerenderowana na tekst HTML.
* Strona jest wysyłana do klienta.

Aplikacja Blazor składa się z elementów wielokrotnego użytku interfejsu użytkownika o nazwie *Components*. Składnik zawiera C# kod, znacznik i inne składniki. Gdy składnik jest renderowany, Blazor tworzy wykres dołączonych składników podobny do Document Object Model HTML lub XML (DOM). Ten wykres zawiera stan składnika przechowywany w właściwościach i polach. Blazor oblicza wykres składnika, aby utworzyć binarną reprezentację znaczników. Format binarny może:

* Włączono tekst HTML (podczas renderowania prerenderingu).
* Służy do wydajnej aktualizacji znaczników podczas normalnego renderowania.

Aktualizacja interfejsu użytkownika w Blazor jest wyzwalana przez:

* Interakcja z użytkownikiem, na przykład wybranie przycisku.
* Wyzwalacze aplikacji, takie jak czasomierz.

Wykres jest ponownie renderowany i obliczana *jest różnica między interfejsami* użytkownika. Różnica ta jest najmniejszym zestawem zmian modelu DOM wymaganym do zaktualizowania interfejsu użytkownika na kliencie. Różnica jest wysyłana do klienta w formacie binarnym i stosowana przez przeglądarkę.

Składnik jest usuwany po przejściu przez użytkownika na klienta. Gdy użytkownik korzysta ze składnika, stan składnika (usługi, zasoby) musi być przechowywany w pamięci serwera. Ponieważ stan wielu składników może być obsługiwany przez serwer współbieżnie, wyczerpanie pamięci jest problemem, który należy rozwiązać. Aby uzyskać wskazówki dotyczące sposobu tworzenia aplikacji serwera Blazor w celu zapewnienia najlepszego wykorzystania pamięci serwera, zobacz <xref:security/blazor/server>.

### <a name="circuits"></a>Elektrycznych

Aplikacja serwera Blazor jest tworzona na podstawie [sygnału ASP.NET Core](xref:signalr/introduction). Każdy klient komunikuje się z serwerem za pośrednictwem co najmniej jednego połączenia sygnalizującego zwanego *obwodem*. Obwód jest abstrakcją Blazor za pośrednictwem połączeń sygnalizujących, które mogą tolerować tymczasowe przerwy w działaniu sieci. Gdy klient Blazor widzi, że połączenie sygnalizujące jest odłączone, próbuje ponownie nawiązać połączenie z serwerem przy użyciu nowego połączenia sygnalizującego.

Każdy ekran przeglądarki (karta przeglądarki lub iframe) połączony z aplikacją serwera Blazor korzysta z połączenia sygnalizującego. Jest to jeszcze inna ważna różnica w porównaniu z typowymi aplikacjami renderowanymi przez serwer. W aplikacji renderowanej na serwerze otwieranie tej samej aplikacji na wielu ekranach przeglądarki zazwyczaj nie jest przeważnie uwzględniane w dodatkowych wymaganiach dotyczących zasobów na serwerze. W aplikacji serwera Blazor każdy ekran przeglądarki wymaga oddzielnego obwodu i oddzielnych wystąpień stanu składnika, które mają być zarządzane przez serwer programu.

Blazor uważa *, że zamyka* kartę przeglądarki lub przechodzenie do zewnętrznego adresu URL. W przypadku bezpiecznego zakończenia obwód i skojarzone zasoby są natychmiast uwalniane. Klient może również odłączyć się niebezpiecznie, na przykład z powodu przerwy w działaniu sieci. Serwer Blazor przechowuje rozłączone obwody przez konfigurowalny interwał, aby umożliwić klientowi Ponowne nawiązywanie połączenia. Aby uzyskać więcej informacji, zobacz sekcję [ponowne łączenie z tym samym serwerem](#reconnection-to-the-same-server) .

### <a name="ui-latency"></a>Opóźnienie interfejsu użytkownika

Opóźnienie interfejsu użytkownika to czas od zainicjowanej akcji do momentu zaktualizowania interfejsu użytkownika. Mniejsze wartości opóźnienia interfejsu użytkownika są bezwzględnie konieczne, aby aplikacja mogła reagować na użytkownika. W aplikacji serwera Blazor każda akcja jest wysyłana do serwera, przetwarzane i różnic interfejsu użytkownika jest wysyłana z powrotem. W związku z tym opóźnienia interfejsu użytkownika to suma opóźnień sieci i opóźnienia serwera w przetwarzaniu akcji.

W przypadku aplikacji biznesowych, która jest ograniczona do prywatnej sieci firmowej, wpływ na postrzeganie opóźnień przez użytkownika z powodu opóźnienia sieci jest zwykle niezauważalny. W przypadku aplikacji wdrożonej za pośrednictwem Internetu opóźnienie może być zauważalne dla użytkowników, szczególnie w przypadku, gdy użytkownicy są szeroko rozproszona geograficznie.

Użycie pamięci może również przyczynić się do opóźnienia aplikacji. Zwiększone użycie pamięci powoduje częste zbieranie elementów bezużytecznych lub stronicowanie pamięci na dysku, co zmniejsza wydajność aplikacji i w związku z tym zwiększa opóźnienia interfejsu użytkownika. Aby uzyskać więcej informacji, zobacz <xref:security/blazor/server>.

Aplikacje serwera Blazor powinny być zoptymalizowane w celu zminimalizowania opóźnień interfejsu użytkownika przez zmniejszenie opóźnienia sieci i użycie pamięci. Aby uzyskać podejście do mierzenia opóźnień sieci, zobacz <xref:host-and-deploy/blazor/server#measure-network-latency>. Aby uzyskać więcej informacji na temat sygnałów i Blazor, zobacz:

* <xref:host-and-deploy/blazor/server>
* <xref:security/blazor/server>

### <a name="reconnection-to-the-same-server"></a>Ponowne nawiązywanie połączenia z tym samym serwerem

Aplikacje serwera Blazor wymagają aktywnego połączenia z serwerem. Jeśli połączenie zostanie utracone, aplikacja spróbuje ponownie nawiązać połączenie z serwerem. O ile stan klienta nadal znajduje się w pamięci, sesja klienta zostaje wznowiona bez utraty stanu.

Gdy klient wykryje, że połączenie zostało utracone, do użytkownika jest wyświetlany domyślny interfejs użytkownika, podczas gdy klient próbuje ponownie nawiązać połączenie. Jeśli ponowne połączenie nie powiedzie się, użytkownik otrzymuje opcję ponowienia próby. Aby dostosować interfejs użytkownika, zdefiniuj element z `components-reconnect-modal` jako `id` na stronie Razor *_Host. cshtml* . Klient aktualizuje ten element za pomocą jednej z następujących klas CSS w oparciu o stan połączenia:

* `components-reconnect-show` &ndash; Pokaż interfejs użytkownika w celu wskazania utraconych połączeń, a klient próbuje ponownie nawiązać połączenie.
* `components-reconnect-hide` &ndash; klient ma aktywne połączenie, Ukryj interfejs użytkownika.
* `components-reconnect-failed` &ndash; ponowne połączenie nie powiodło się, prawdopodobnie z powodu błędu sieci. Aby spróbować ponownie nawiązać połączenie, wywołaj `window.Blazor.reconnect()`.
* `components-reconnect-rejected` &ndash; ponowne połączenie zostało odrzucone. Serwer został osiągnięty, ale odmówił połączenia, a stan użytkownika na serwerze został usunięty. Aby ponownie załadować aplikację, wywołaj `location.reload()`. Ten stan połączenia może skutkować tym, że:
  * Wystąpił awaria w obwodzie (kod po stronie serwera).
  * Klient jest odłączony wystarczająco długo, aby serwer mógł porzucić stan użytkownika. Wystąpienia składników, z którymi użytkownik pracowali, zostały usunięte.

### <a name="stateful-reconnection-after-prerendering"></a>Stanowe Ponowne nawiązywanie połączenia po przeprowadzeniu prerenderowania

Aplikacje serwera Blazor są domyślnie skonfigurowane, aby skonfigurować interfejs użytkownika na serwerze przed nawiązaniem połączenia z serwerem. Ta konfiguracja jest ustawiana na stronie *_Host. cshtml* Razor:

::: moniker range=">= aspnetcore-3.1"

```cshtml
<body>
    <app>
      <component type="typeof(App)" render-mode="ServerPrerendered" />
    </app>

    <script src="_framework/blazor.server.js"></script>
</body>
```

::: moniker-end

::: moniker range="< aspnetcore-3.1"

```cshtml
<body>
    <app>@(await Html.RenderComponentAsync<App>(RenderMode.ServerPrerendered))</app>

    <script src="_framework/blazor.server.js"></script>
</body>
```

::: moniker-end

`RenderMode` Określa, czy składnik:

* Jest wstępnie renderowany na stronie.
* Jest renderowany jako statyczny kod HTML na stronie lub zawiera informacje niezbędne do uruchomienia aplikacji Blazor z poziomu agenta użytkownika.

::: moniker range=">= aspnetcore-3.1"

| `RenderMode`        | Opis |
| ------------------- | ----------- |
| `ServerPrerendered` | Renderuje składnik do statycznego kodu HTML i zawiera znacznik dla aplikacji serwera Blazor. Po uruchomieniu agenta użytkownika ten znacznik jest używany do uruchamiania aplikacji Blazor. |
| `Server`            | Renderuje znacznik dla aplikacji serwera Blazor. Dane wyjściowe ze składnika nie są uwzględniane. Po uruchomieniu agenta użytkownika ten znacznik jest używany do uruchamiania aplikacji Blazor. |
| `Static`            | Renderuje składnik do statycznego kodu HTML. |

::: moniker-end

::: moniker range="< aspnetcore-3.1"

| `RenderMode`        | Opis |
| ------------------- | ----------- |
| `ServerPrerendered` | Renderuje składnik do statycznego kodu HTML i zawiera znacznik dla aplikacji serwera Blazor. Po uruchomieniu agenta użytkownika ten znacznik jest używany do uruchamiania aplikacji Blazor. Parametry nie są obsługiwane. |
| `Server`            | Renderuje znacznik dla aplikacji serwera Blazor. Dane wyjściowe ze składnika nie są uwzględniane. Po uruchomieniu agenta użytkownika ten znacznik jest używany do uruchamiania aplikacji Blazor. Parametry nie są obsługiwane. |
| `Static`            | Renderuje składnik do statycznego kodu HTML. Parametry są obsługiwane. |

::: moniker-end

Renderowanie składników serwera ze statyczną stroną HTML nie jest obsługiwane.

Gdy `RenderMode` jest `ServerPrerendered`, składnik jest początkowo renderowany statycznie jako część strony. Gdy przeglądarka nawiąże połączenie z serwerem, składnik jest renderowany *ponownie*, a składnik jest teraz interaktywny. Jeśli istnieje [Metoda cyklu życia](xref:blazor/components#lifecycle-methods) służąca do inicjowania składnika (`OnInitialized{Async}`), metoda jest wykonywana *dwukrotnie*:

* Gdy składnik jest wstępnie renderowany statycznie.
* Po nawiązaniu połączenia z serwerem.

Może to spowodować zauważalną zmianę danych wyświetlanych w interfejsie użytkownika, gdy składnik jest renderowany.

Aby uniknąć podwójnego renderowania w aplikacji serwera Blazor:

* Przekaż identyfikator, który może służyć do buforowania stanu podczas wykonywania prerenderowania i pobierania stanu po ponownym uruchomieniu aplikacji.
* Użyj identyfikatora podczas renderowania, aby zapisać stan składnika.
* Użyj identyfikatora po włączeniu, aby pobrać buforowany stan.

Poniższy kod ilustruje zaktualizowaną `WeatherForecastService` w aplikacji serwera Blazor opartej na szablonie, która pozwala uniknąć podwójnego renderowania:

```csharp
public class WeatherForecastService
{
    private static readonly string[] Summaries = new[]
    {
        "Freezing", "Bracing", "Chilly", "Cool", "Mild",
        "Warm", "Balmy", "Hot", "Sweltering", "Scorching"
    };
    
    public WeatherForecastService(IMemoryCache memoryCache)
    {
        MemoryCache = memoryCache;
    }
    
    public IMemoryCache MemoryCache { get; }

    public Task<WeatherForecast[]> GetForecastAsync(DateTime startDate)
    {
        return MemoryCache.GetOrCreateAsync(startDate, async e =>
        {
            e.SetOptions(new MemoryCacheEntryOptions
            {
                AbsoluteExpirationRelativeToNow = 
                    TimeSpan.FromSeconds(30)
            });

            var rng = new Random();

            await Task.Delay(TimeSpan.FromSeconds(10));

            return Enumerable.Range(1, 5).Select(index => new WeatherForecast
            {
                Date = startDate.AddDays(index),
                TemperatureC = rng.Next(-20, 55),
                Summary = Summaries[rng.Next(Summaries.Length)]
            }).ToArray();
        });
    }
}
```

### <a name="render-stateful-interactive-components-from-razor-pages-and-views"></a>Renderuj stanowe składniki interaktywne ze stron Razor i widoków

Można dodać składniki interaktywne ze stanem do strony lub widoku Razor.

Gdy renderuje stronę lub widok:

* Składnik jest wstępnie renderowany przy użyciu strony lub widoku.
* Początkowy stan składnika używany na potrzeby renderowania wstępnego został utracony.
* Nowy stan składnika jest tworzony po nawiązaniu połączenia z sygnałem.

Następująca strona Razor renderuje składnik `Counter`:

::: moniker range=">= aspnetcore-3.1"

```cshtml
<h1>My Razor Page</h1>

<component type="typeof(Counter)" render-mode="ServerPrerendered" 
    param-InitialValue="InitialValue" />

@code {
    [BindProperty(SupportsGet=true)]
    public int InitialValue { get; set; }
}
```

::: moniker-end

::: moniker range="< aspnetcore-3.1"

```cshtml
<h1>My Razor Page</h1>

@(await Html.RenderComponentAsync<Counter>(RenderMode.ServerPrerendered))

@code {
    [BindProperty(SupportsGet=true)]
    public int InitialValue { get; set; }
}
```

::: moniker-end

### <a name="render-noninteractive-components-from-razor-pages-and-views"></a>Renderuj nieinteraktywne składniki ze stron Razor i widoków

Na poniższej stronie Razor składnik `MyComponent` jest renderowany statycznie z wartością początkową określoną przy użyciu formularza:

::: moniker range=">= aspnetcore-3.1"

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

::: moniker-end

::: moniker range="< aspnetcore-3.1"

```cshtml
<h1>My Razor Page</h1>

<form>
    <input type="number" asp-for="InitialValue" />
    <button type="submit">Set initial value</button>
</form>

@(await Html.RenderComponentAsync<MyComponent>(RenderMode.Static, 
    new { InitialValue = InitialValue }))

@code {
    [BindProperty(SupportsGet=true)]
    public int InitialValue { get; set; }
}
```

::: moniker-end

Ponieważ `MyComponent` jest renderowany statycznie, składnik nie może być interaktywny.

### <a name="detect-when-the-app-is-prerendering"></a>Wykryj, kiedy aplikacja jest przedrenderowana

[!INCLUDE[](~/includes/blazor-prerendering.md)]

### <a name="configure-the-signalr-client-for-blazor-server-apps"></a>Konfigurowanie klienta sygnalizującego dla aplikacji serwera Blazor

Czasami trzeba skonfigurować klienta sygnalizującego używany przez aplikacje serwera Blazor. Na przykład może być konieczne skonfigurowanie rejestrowania na kliencie sygnalizującego, aby zdiagnozować problem z połączeniem.

Aby skonfigurować klienta sygnalizującego w pliku *Pages/_Host. cshtml* :

* Dodaj atrybut `autostart="false"` do tagu `<script>` dla skryptu *blazor. Server. js* .
* Wywołaj `Blazor.start` i przekaż obiekt konfiguracji, który określa konstruktora sygnalizującego.

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

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:blazor/get-started>
* <xref:signalr/introduction>
