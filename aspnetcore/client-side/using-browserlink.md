---
title: Łączność z przeglądarkami w programie ASP.NET Core
author: ncarandini
description: Wyjaśnia, jak Browser Link jest funkcja programu Visual Studio, która łączy środowisko projektowe z jednej lub kilku przeglądarkach sieci web.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 09/22/2017
uid: client-side/using-browserlink
ms.openlocfilehash: 452ba5149563c186750466f471c7b950f0017614
ms.sourcegitcommit: d53e0cc71542b92de867bcce51575b054886f529
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754246"
---
# <a name="browser-link-in-aspnet-core"></a>Łączność z przeglądarkami w programie ASP.NET Core

Przez [Nicolò Carandini](https://github.com/ncarandini), [Mike Wasson](https://github.com/MikeWasson), i [Tom Dykstra](https://github.com/tdykstra)

Łącze przeglądarki jest funkcją w programie Visual Studio tworzy kanał komunikacyjny między środowiska programistycznego i jeden lub więcej przeglądarek sieci web. Można użyć łącze przeglądarki, aby odświeżyć aplikację sieci web w kilku przeglądarkach jednocześnie, która jest przydatna przy testowaniu obsługiwania wielu przeglądarek.

## <a name="browser-link-setup"></a>Konfigurowanie Linku przeglądarki

::: moniker range=">= aspnetcore-2.1"

Podczas konwersji projektu programu ASP.NET Core 2.0 platformy ASP.NET Core 2.1 i przechodzenie do [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app), zainstaluj [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) pakietu dla Funkcje BrowserLink. Użyj szablonów projektów platformy ASP.NET Core 2.1 `Microsoft.AspNetCore.App` meta Microsoft.aspnetcore.all domyślnie.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

ASP.NET Core 2.0 **aplikacji sieci Web**, **pusty**, i **interfejsu API sieci Web** Użyj szablonów projektu [pakiet meta Microsoft.aspnetcore.all](xref:fundamentals/metapackage) , który zawiera odwołania do pakietu dla [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/). W związku z tym, za pomocą `Microsoft.AspNetCore.All` meta Microsoft.aspnetcore.all nie wymaga żadnych dodatkowych czynności, aby udostępnić łącze przeglądarki do użycia.

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

ASP.NET Core 1.x **aplikacji sieci Web** szablon projektu zawiera odwołania do pakietu dla [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) pakietu. **Pusty** lub **interfejsu API sieci Web** wymagają szablonów projektów, można dodać odwołania do pakietu do `Microsoft.VisualStudio.Web.BrowserLink`.

Ponieważ jest najprostszym sposobem to funkcja programu Visual Studio, aby dodać pakiet do **pusty** lub **interfejsu API sieci Web** szablon projektu, należy otworzyć **Konsola Menedżera pakietów** (**Widoku** > **Windows inne** > **Konsola Menedżera pakietów**) i uruchom następujące polecenie:

```console
install-package Microsoft.VisualStudio.Web.BrowserLink
```

Alternatywnie, można użyć **Menedżera pakietów NuGet**. Kliknij prawym przyciskiem myszy nazwę projektu w **Eksploratora rozwiązań** i wybierz polecenie **Zarządzaj pakietami NuGet**:

![Menedżer pakietów NuGet Otwórz](using-browserlink/_static/open-nuget-package-manager.png)

Znajdź i zainstaluj pakiet:

![Dodaj pakiet przy użyciu Menedżera pakietów NuGet](using-browserlink/_static/add-package-with-nuget-package-manager.png)

::: moniker-end

### <a name="configuration"></a>Konfiguracja

W `Startup.Configure` metody:

```csharp
app.UseBrowserLink();
```

Zazwyczaj kod znajduje się wewnątrz `if` blok, który umożliwia łączność z przeglądarkami jedynie w środowisku deweloperskim, jak pokazano poniżej:

```csharp
if (env.IsDevelopment())
{
    app.UseDeveloperExceptionPage();
    app.UseBrowserLink();
}
```

Aby uzyskać więcej informacji, zobacz [używanie wielu środowisk](xref:fundamentals/environments).

## <a name="how-to-use-browser-link"></a>Jak używać łączy przeglądarki

Mając otwarty projekt platformy ASP.NET Core, Visual Studio wyświetla obok pozycji formantu paska narzędzi łącza przeglądarki **Debuguj element docelowy** formantu paska narzędzi:

![Menu rozwijane łącza przeglądarki](using-browserlink/_static/browserLink-dropdown-menu.png)

Z formantem paska narzędzi łącza przeglądarki możesz wykonywać następujące czynności:

* Odświeżanie aplikacji sieci web w kilku przeglądarkach naraz.
* Otwórz **nawigacyjnym łącza przeglądarki**.
* Włączanie lub wyłączanie **łączność z przeglądarkami**. Uwaga: Łącze przeglądarki jest domyślnie wyłączona w programie Visual Studio 2017 (15.3).
* Włączanie lub wyłączanie [automatyczna synchronizacja CSS](#enable-or-disable-css-auto-sync).

> [!NOTE]
> Niektóre dodatki plug-in programu Visual Studio, głównie *2015 pakietu rozszerzenia sieci Web* i *2017 pakietu rozszerzenia sieci Web*, oferują rozszerzoną funkcjonalność na łączność z przeglądarkami, ale niektóre dodatkowe funkcje nie działają z ASP. Projekty .NET Core.

## <a name="refresh-the-web-app-in-several-browsers-at-once"></a>Odśwież aplikację sieci web w kilku przeglądarkach jednocześnie

Aby wybrać przeglądarkę jednej sieci web do uruchamiania podczas uruchamiania projektu, użyj menu rozwijane w **Debuguj element docelowy** formantu paska narzędzi:

![Menu rozwijane F5](using-browserlink/_static/debug-target-dropdown-menu.png)

Aby otworzyć jednocześnie wiele przeglądarek, wybierz **przeglądania przy użyciu...**  z tej samej listy rozwijanej. Naciśnij i przytrzymaj klawisz CTRL, aby wybrać przeglądarek ma, a następnie kliknij przycisk **Przeglądaj**:

![Otwórz jednocześnie wiele przeglądarek](using-browserlink/_static/open-many-browsers-at-once.png)

Poniżej przedstawiono zrzut ekranu przedstawiający otwarte programu Visual Studio przy użyciu widoku indeksu i otwórz przeglądarek:

![Synchronizuj z przykład przeglądarek](using-browserlink/_static/sync-with-two-browsers-example.png)

Zatrzymaj wskaźnik myszy nad formant paska narzędzi łącza przeglądarki, aby przeglądarki, które są podłączone do projektu:

![Porada po wskazaniu wskaźnikiem](using-browserlink/_static/hoover-tip.png)

Zmień widok indeksu i wszystkich połączonych przeglądarek są aktualizowane, po kliknięciu przycisku odświeżania łączność z przeglądarkami:

![przeglądarki synchronizacji do zmiany](using-browserlink/_static/browsers-sync-to-changes.png)

Łączność z przeglądarkami współpracuje również z przeglądarki, które Uruchom z poza programem Visual Studio i przejdź do adresu URL aplikacji.

### <a name="the-browser-link-dashboard"></a>Pulpicie nawigacyjnym łącza przeglądarki

Otwórz pulpicie nawigacyjnym łącza przeglądarki z przeglądarkami rozwijanego menu, aby zarządzać połączeniem z przeglądarkami Otwórz:

![Open — browserslink — pulpit nawigacyjny](using-browserlink/_static/open-browserlink-dashboard.png)

Jeśli przeglądarka nie jest połączony, można uruchomić sesji bez debugowania, wybierając *Pokaż w przeglądarce* łącza:

![nie połączenia, browserlink pulpitu nawigacyjnego w-](using-browserlink/_static/browserlink-dashboard-no-connections.png)

W przeciwnym razie połączonych przeglądarek są wyświetlane ze ścieżką do strony, która jest wyświetlane w każdej przeglądarce:

![dwa połączenia, browserlink pulpitu nawigacyjnego w-](using-browserlink/_static/browserlink-dashboard-two-connections.png)

Jeśli chcesz możesz kliknąć nazwę listy przeglądarki, aby odświeżyć tego jednej przeglądarki.

### <a name="enable-or-disable-browser-link"></a>Włączanie lub wyłączanie łączność z przeglądarkami

Po ponownym włączeniu łączność z przeglądarkami po wyłączeniu go, należy odświeżyć przeglądarek, podłącz je ponownie.

### <a name="enable-or-disable-css-auto-sync"></a>Włączanie lub wyłączanie automatycznej synchronizacji CSS

Po włączeniu automatycznej synchronizacji CSS połączonych przeglądarek są automatycznie odświeżane, gdy wprowadzać zmian w plikach CSS.

## <a name="how-it-works"></a>Jak to działa

Łączność z przeglądarkami używa SignalR w celu utworzenia kanał komunikacyjny między Visual Studio i przeglądarki. Po włączeniu łączność z przeglądarkami programu Visual Studio działa jako serwer biblioteki SignalR, wielu klientów (przeglądarki) można łączyć się. Łączność z przeglądarkami rejestruje również składnik oprogramowania pośredniczącego w potoku żądania programu ASP.NET Core. Ten składnik wprowadza specjalne `<script>` odwołań w każdym żądaniu strony z serwera. Odwołania do skryptu można wyświetlić, wybierając **Wyświetl źródło** w przeglądarce i przewijając do końca `<body>` tagować zawartość:

```html
    <!-- Visual Studio Browser Link -->
    <script type="application/json" id="__browserLink_initializationData">
        {"requestId":"a717d5a07c1741949a7cefd6fa2bad08","requestMappingFromServer":false}
    </script>
    <script type="text/javascript" src="http://localhost:54139/b6e36e429d034f578ebccd6a79bf19bf/browserLink" async="async"></script>
    <!-- End Browser Link -->
</body>
```

Nie modyfikować plików źródłowych. Składnik oprogramowania pośredniczącego, które dynamicznie wprowadza odwołania do skryptu.

Ponieważ kod po stronie przeglądarki jest kod JavaScript, działa we wszystkich przeglądarkach obsługiwanych przez SignalR bez konieczności wtyczkę przeglądarki.
