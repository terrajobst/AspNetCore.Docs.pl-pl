---
title: Link przeglądarki w ASP.NET Core
author: ncarandini
description: Wyjaśnia, w jaki sposób łącze przeglądarki jest funkcją programu Visual Studio, która łączy środowisko programistyczne z co najmniej jedną przeglądarką sieci Web.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 11/12/2019
no-loc:
- SignalR
uid: client-side/using-browserlink
ms.openlocfilehash: b21b698d49e72b559cd9cd3753c48a38c99db24d
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/12/2019
ms.locfileid: "73962785"
---
# <a name="browser-link-in-aspnet-core"></a>Link przeglądarki w ASP.NET Core

Autorzy [Nicolò Carandini](https://github.com/ncarandini), [Jan Wasson](https://github.com/MikeWasson)i [Tomasz Dykstra](https://github.com/tdykstra)

Link do przeglądarki to funkcja w programie Visual Studio, która tworzy kanał komunikacyjny między środowiskiem deweloperskim i jedną lub wieloma przeglądarkami sieci Web. Możesz użyć linku przeglądarki, aby odświeżyć aplikację sieci Web w kilku przeglądarkach jednocześnie, co jest przydatne w przypadku testowania między przeglądarkami.

## <a name="browser-link-setup"></a>Konfiguracja linku przeglądarki

::: moniker range=">= aspnetcore-2.1"

Podczas konwertowania projektu ASP.NET Core 2,0 na ASP.NET Core 2,1 i przejścia do [pakietu Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app), zainstaluj pakiet [Microsoft. VisualStudio. Web. BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) dla funkcji BrowserLink. Szablony projektów ASP.NET Core 2,1 używają domyślnie pakietu `Microsoft.AspNetCore.App`.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Szablony projektu **aplikacji sieci Web**, **pustej**i **internetowego interfejsu API** ASP.NET Core 2,0 używają [pakietu Microsoft. AspNetCore. All](xref:fundamentals/metapackage), który zawiera odwołanie do pakietu dla [Microsoft. VisualStudio. Web. BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/). W związku z tym używanie pakietu `Microsoft.AspNetCore.All` nie wymaga żadnych dalszych działań w celu udostępnienia linku przeglądarki do użycia.

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

Szablon projektu **aplikacji sieci Web** ASP.NET Core 1. x zawiera odwołanie do pakietu dla pakietu [Microsoft. VisualStudio. Web. BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) . Aby dodać odwołanie do `Microsoft.VisualStudio.Web.BrowserLink`, w projektach szablonu lub **interfejsie API sieci Web** **jest wymagane** dodanie odwołania do pakietu.

Ponieważ jest to funkcja programu Visual Studio, najprostszym sposobem dodania pakietu do **pustego** lub projektu szablonu **interfejsu API sieci Web** jest otwarcie **konsoli Menedżera pakietów** (**Wyświetl** > innej **konsoli Menedżera pakietów**> **Windows** ) i uruchom następujące polecenie:

```console
install-package Microsoft.VisualStudio.Web.BrowserLink
```

Alternatywnie można użyć **Menedżera pakietów NuGet**. Kliknij prawym przyciskiem myszy nazwę projektu w **Eksplorator rozwiązań** i wybierz polecenie **Zarządzaj pakietami NuGet**:

![Otwórz Menedżera pakietów NuGet](using-browserlink/_static/open-nuget-package-manager.png)

Znajdź i zainstaluj pakiet:

![Dodaj pakiet za pomocą Menedżera pakietów NuGet](using-browserlink/_static/add-package-with-nuget-package-manager.png)

::: moniker-end

### <a name="configuration"></a>Konfiguracja

W `Startup.Configure` metodzie:

```csharp
app.UseBrowserLink();
```

Zazwyczaj kod znajduje się w bloku `if`, który umożliwia tylko łącze przeglądarki w środowisku programistycznym, jak pokazano poniżej:

```csharp
if (env.IsDevelopment())
{
    app.UseDeveloperExceptionPage();
    app.UseBrowserLink();
}
```

Aby uzyskać więcej informacji, zobacz [Korzystanie z wielu środowisk](xref:fundamentals/environments).

## <a name="how-to-use-browser-link"></a>Jak używać linku przeglądarki

Gdy masz otwarty projekt ASP.NET Core, program Visual Studio wyświetli kontrolkę pasek narzędzi linku przeglądarki obok kontrolki paska narzędzi **elementu docelowego debugowania** :

![Menu rozwijane linku przeglądarki](using-browserlink/_static/browserLink-dropdown-menu.png)

Za pomocą kontrolki paska narzędzi łącza przeglądarki można:

* Odśwież aplikację sieci Web w kilku przeglądarkach jednocześnie.
* Otwórz **pulpit nawigacyjny linku do przeglądarki**.
* Włącza lub wyłącza **link przeglądarki**. Uwaga: łącze przeglądarki jest domyślnie wyłączone w programie Visual Studio 2017 (15,3).
* Włączać lub wyłączać funkcję [autosynchronizacji CSS](#enable-or-disable-css-auto-sync).

> [!NOTE]
> Niektóre wtyczki programu Visual Studio, w tym *pakiet rozszerzeń sieci web 2015* i *pakiet rozszerzeń sieci Web 2017*, oferują rozszerzoną funkcję dla łącza przeglądarki, ale niektóre dodatkowe funkcje nie współpracują z projektami ASP.NET Core.

## <a name="refresh-the-web-app-in-several-browsers-at-once"></a>Odśwież aplikację sieci Web w kilku przeglądarkach jednocześnie

Aby wybrać jedną przeglądarkę sieci Web, która ma być uruchamiana podczas uruchamiania projektu, użyj menu rozwijanego w formancie paska narzędzi **elementu docelowego debugowania** :

![Menu rozwijane F5](using-browserlink/_static/debug-target-dropdown-menu.png)

Aby otworzyć wiele przeglądarek jednocześnie, wybierz pozycję **Przeglądaj za pomocą...** z tego samego listy rozwijanej. Naciśnij i przytrzymaj klawisz CTRL, aby wybrać przeglądarki, a następnie kliknij przycisk **Przeglądaj**:

![Otwórz wiele przeglądarek jednocześnie](using-browserlink/_static/open-many-browsers-at-once.png)

Oto zrzut ekranu przedstawiający program Visual Studio z otwartym widokiem indeksu i dwiema otwartymi przeglądarkami:

![Przykład synchronizacji z dwoma przeglądarkami](using-browserlink/_static/sync-with-two-browsers-example.png)

Umieść kursor nad kontrolką paska narzędzi łącza przeglądarki, aby wyświetlić przeglądarki, które są połączone z projektem:

![Wskazówki dotyczące aktywowania](using-browserlink/_static/hoover-tip.png)

Zmień widok indeksu, a wszystkie połączone przeglądarki są aktualizowane po kliknięciu przycisku Odśwież linku do przeglądarki:

![przeglądarki — synchronizacja ze zmianami](using-browserlink/_static/browsers-sync-to-changes.png)

Łącze przeglądarki współpracuje również z przeglądarkami uruchamianymi spoza programu Visual Studio i przejdź do adresu URL aplikacji.

### <a name="the-browser-link-dashboard"></a>Pulpit nawigacyjny linków przeglądarki

Otwórz pulpit nawigacyjny link przeglądarki z menu rozwijanego linku do przeglądarki, aby zarządzać połączeniem przy użyciu otwartych przeglądarek:

![Open-browserslink — pulpit nawigacyjny](using-browserlink/_static/open-browserlink-dashboard.png)

Jeśli żadna przeglądarka nie jest połączona, możesz uruchomić sesję niedebugowania, wybierając *Widok w przeglądarce* link:

![browserlink — pulpit nawigacyjny — brak połączeń](using-browserlink/_static/browserlink-dashboard-no-connections.png)

W przeciwnym razie połączone przeglądarki są wyświetlane ze ścieżką do strony, którą pokazuje Każda przeglądarka:

![browserlink — pulpit nawigacyjny — dwa połączenia](using-browserlink/_static/browserlink-dashboard-two-connections.png)

Jeśli chcesz, możesz kliknąć nazwę przeglądarki wyświetlonej w celu odświeżenia pojedynczej przeglądarki.

### <a name="enable-or-disable-browser-link"></a>Włącz lub Wyłącz link przeglądarki

Po ponownym włączeniu linku przeglądarki po jego wyłączeniu należy odświeżyć przeglądarki w celu ponownego nawiązania połączenia.

### <a name="enable-or-disable-css-auto-sync"></a>Włączanie lub wyłączanie autosynchronizacji CSS

Gdy automatyczna synchronizacja CSS jest włączona, podłączane przeglądarki są automatycznie odświeżane po wprowadzeniu jakichkolwiek zmian w plikach CSS.

## <a name="how-it-works"></a>Jak to działa

Link przeglądarki używa SignalR do tworzenia kanału komunikacyjnego między programem Visual Studio i przeglądarką. Gdy łącze przeglądarki jest włączone, program Visual Studio działa jako serwer SignalR, z którym mogą się łączyć wielu klientów (przeglądarki). Link przeglądarki rejestruje również składnik pośredniczący w potoku żądania ASP.NET Core. Ten składnik wprowadza specjalne odwołania `<script>` do każdego żądania strony z serwera. Odwołania do skryptów można zobaczyć, wybierając pozycję **Wyświetl źródło** w przeglądarce i przewinięcie do końca `<body>` zawartości tagu:

```html
    <!-- Visual Studio Browser Link -->
    <script type="application/json" id="__browserLink_initializationData">
        {"requestId":"a717d5a07c1741949a7cefd6fa2bad08","requestMappingFromServer":false}
    </script>
    <script type="text/javascript" src="http://localhost:54139/b6e36e429d034f578ebccd6a79bf19bf/browserLink" async="async"></script>
    <!-- End Browser Link -->
</body>
```

Pliki źródłowe nie są modyfikowane. Składnik pośredniczący wprowadza odwołania do skryptów dynamicznie.

Ponieważ kod po stronie przeglądarki to wszystkie skrypty JavaScript, działa on we wszystkich przeglądarkach, które SignalR obsługuje bez wtyczki przeglądarki.
