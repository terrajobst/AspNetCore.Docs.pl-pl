---
title: "Łącze przeglądarki w platformy ASP.NET Core"
author: ncarandini
description: "Wyjaśnia, jak łącze przeglądarki funkcja Visual Studio, która łączy środowiska programistycznego z co najmniej jeden przeglądarki sieci web."
keywords: "Platformy ASP.NET Core łącze przeglądarki, synchronizacja CSS"
ms.author: riande
manager: wpickett
ms.date: 09/22/2017
ms.topic: article
ms.assetid: 11813d4c-3f8a-445a-b23b-e4a57d001abc
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/using-browserlink
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b69d085e8bee4cdac2dff08b46a95a8869e263b7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
# <a name="browser-link-in-aspnet-core"></a>Łącze przeglądarki w platformy ASP.NET Core 

Przez [Nicolò Carandini](https://github.com/ncarandini), [Wasson Jan](https://github.com/MikeWasson), i [Dykstra niestandardowy](https://github.com/tdykstra)

Łącze przeglądarki jest funkcją w programie Visual Studio, tworzącym kanał komunikacji między Środowisko deweloperskie i co najmniej jeden przeglądarki sieci web. Można użyć łącze przeglądarki, aby odświeżyć aplikacji sieci web w wielu przeglądarkach jednocześnie, która jest przydatna przy testowaniu różnych przeglądarkach.

## <a name="browser-link-setup"></a>Ustawienia Linku przeglądarki

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[Program ASP.NET Core 2.x](#tab/aspnetcore2x)

Platformy ASP.NET Core 2.x **aplikacji sieci Web**, **pusty**, i **interfejsu API sieci Web** szablonów projektów użyj [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) meta pakiet, który zawiera odwołania do pakietu dla [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/). W związku z tym przy użyciu `Microsoft.AspNetCore.All` meta pakiet wymaga żadnych dodatkowych czynności, aby udostępnić łącze przeglądarki do użycia.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[Program ASP.NET Core 1.x](#tab/aspnetcore1x)

Platformy ASP.NET Core 1.x **aplikacji sieci Web** szablon projektu zawiera odwołania do pakietu dla [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) pakietu. **Pusty** lub **interfejsu API sieci Web** szablonów projektów trzeba dodać odwołanie do pakietu `Microsoft.VisualStudio.Web.BrowserLink`.

Ponieważ jest najprostszym sposobem funkcja programu Visual Studio, aby dodać pakiet do **pusty** lub **interfejsu API sieci Web** szablonu projektu, należy otworzyć **Konsola Menedżera pakietów** (**Widoku** > **inne okna** > **Konsola Menedżera pakietów**) i uruchom następujące polecenie:

```console
install-package Microsoft.VisualStudio.Web.BrowserLink
```

Alternatywnie można użyć **Menedżera pakietów NuGet**. Kliknij prawym przyciskiem myszy nazwę projektu w **Eksploratora rozwiązań** i wybierz polecenie **Zarządzaj pakietami NuGet**:

![Menedżer pakietów NuGet Otwórz](using-browserlink/_static/open-nuget-package-manager.png)

Znajdź i zainstaluj pakiet:

![Dodaj pakiet Menedżera pakietów NuGet](using-browserlink/_static/add-package-with-nuget-package-manager.png)

---

### <a name="configuration"></a>Konfiguracja

W `Configure` metody *Startup.cs* pliku:

```csharp
app.UseBrowserLink();
```

Zazwyczaj kod znajduje się wewnątrz `if` bloku umożliwiającą tylko łącze przeglądarki w środowisku programistycznym, jak pokazano poniżej:

```csharp
if (env.IsDevelopment())
{
    app.UseDeveloperExceptionPage();
    app.UseBrowserLink();
}
```

Aby uzyskać więcej informacji, zobacz [Praca w środowiskach wielu](xref:fundamentals/environments).

## <a name="how-to-use-browser-link"></a>Jak używać łączy przeglądarki

Jeśli projekt platformy ASP.NET Core, Otwórz, Visual Studio zawiera formantu toolbar Browser Link obok pozycji **docelowego debugowania** formantu paska narzędzi:

![Menu rozwijane łącza przeglądarki](using-browserlink/_static/browserLink-dropdown-menu.png)

Z formantem paska narzędzi łącze przeglądarki możesz:

* Odświeżanie aplikacji sieci web w wielu przeglądarkach jednocześnie.
* Otwórz **nawigacyjnym łącza przeglądarki**.
* Włącz lub wyłącz **łącze przeglądarki**. Uwaga: Łącze przeglądarki jest domyślnie wyłączona w programie Visual Studio 2017 (15 ustęp 3).
* Włącz lub wyłącz [automatyczna synchronizacja CSS](#enable-or-disable-css-auto-sync).

> [!NOTE]
> Niektóre Visual Studio dodatków plug-in, głównie *2015 pakiet rozszerzenia sieci Web* i *2017 pakiet rozszerzenia sieci Web*, oferują rozszerzoną funkcjonalność łącza przeglądarki, ale niektóre dodatkowe funkcje nie działają w aplikacji ASP. Projekty sieci podstawowej.

## <a name="refresh-the-web-application-in-several-browsers-at-once"></a>Odświeżanie aplikacji sieci web w wielu przeglądarkach jednocześnie

Aby wybrać jeden przeglądarki do uruchomienia przy uruchamianiu projektu, użyj menu rozwijanego w **docelowego debugowania** formantu paska narzędzi:

![Menu rozwijane F5](using-browserlink/_static/debug-target-dropdown-menu.png)

Aby otworzyć jednocześnie wiele przeglądarek, wybierz **przeglądania przy użyciu...**  z tej samej listy rozwijanej. Naciśnij i przytrzymaj klawisz CTRL, aby wybrać przeglądarki ma, a następnie kliknij przycisk **Przeglądaj**:

![Otwórz jednocześnie wiele przeglądarek](using-browserlink/_static/open-many-browsers-at-once.png)

Poniżej przedstawiono zrzut ekranu przedstawiający open Visual Studio z widokiem indeksu i otwórz przeglądarek:

![Synchronizacja przykład przeglądarek](using-browserlink/_static/sync-with-two-browsers-example.png)

Umieść kursor nad formantem paska narzędzi łącze przeglądarki do Zobacz przeglądarki, które są podłączone do projektu:

![Umieść kursor Porada](using-browserlink/_static/hoover-tip.png)

Zmień widok indeksu i wszystkich podłączonych przeglądarek są aktualizowane po kliknięciu przycisku Odśwież łącze przeglądarki:

![przeglądarki synchronizacji do zmiany](using-browserlink/_static/browsers-sync-to-changes.png)

Łącze przeglądarki działa także w przypadku przeglądarek, które uruchamianie z zewnętrznego programu Visual Studio i przejdź do adresu URL aplikacji.

### <a name="the-browser-link-dashboard"></a>Na pulpicie nawigacyjnym łącza przeglądarki

Otwórz pulpicie nawigacyjnym łącza przeglądarki z menu Zarządzanie połączenia z przeglądarkami Otwórz rozwijanego łącze przeglądarki:

![Open — browserslink-pulpit nawigacyjny](using-browserlink/_static/open-browserlink-dashboard.png)

Jeśli przeglądarka nie jest połączona, można uruchomić sesji z systemem innym niż debugowanie, wybierając *Wyświetl w przeglądarce* łącza:

![browserlink-pulpit nawigacyjny — nie połączenia](using-browserlink/_static/browserlink-dashboard-no-connections.png)

W przeciwnym razie podłączonych przeglądarek są wyświetlane ze ścieżką do strony, która pokazuje każdą przeglądarkę:

![browserlink-pulpit nawigacyjny — dwa połączenia](using-browserlink/_static/browserlink-dashboard-two-connections.png)

Jeśli chcesz możesz kliknąć nazwę wymienionych przeglądarki, aby odświeżyć jednego przeglądarka.

### <a name="enable-or-disable-browser-link"></a>Włącz lub Wyłącz łącza przeglądarki

Po ponownym włączeniu łącze przeglądarki po jej wyłączeniu, należy odświeżyć przeglądarki dla podłącz je ponownie.

### <a name="enable-or-disable-css-auto-sync"></a>Włączanie lub wyłączanie automatycznej synchronizacji CSS

Po włączeniu automatycznej synchronizacji CSS podłączonych przeglądarek są automatycznie odświeżane przy wprowadzaniu zmian do plików CSS.

## <a name="how-does-it-work"></a>Jak to działa?

Łącze przeglądarki używa SignalR do utworzenia kanału komunikacyjnego między Visual Studio i przeglądarki. Po włączeniu łącze przeglądarki, programu Visual Studio działa jako wielu klientów (przeglądarki) mogą łączyć się z serwerem SignalR. Łącze przeglądarki rejestruje również składnik oprogramowania pośredniczącego w potoku żądania ASP.NET. Ten składnik injects specjalne `<script>` odwołań w każdym żądaniu strony z serwera. Widać odwołań do skryptów, wybierając **Wyświetl źródło** w przeglądarce i przewijania na końcu `<body>` tagu zawartości:

```javascript
    <!-- Visual Studio Browser Link -->
    <script type="application/json" id="__browserLink_initializationData">
        {"requestId":"a717d5a07c1741949a7cefd6fa2bad08","requestMappingFromServer":false}
    </script>
    <script type="text/javascript" src="http://localhost:54139/b6e36e429d034f578ebccd6a79bf19bf/browserLink" async="async"></script>
    <!-- End Browser Link -->
</body>
```

Plików źródłowych nie jest modyfikowany. Składnik oprogramowania pośredniczącego dynamicznie injects odwołań do skryptów. 

Ponieważ kod po stronie przeglądarki jest kod JavaScript, działa na wszystkich przeglądarek obsługiwanych przez SignalR bez konieczności dodatek plug-in dla przeglądarki.
