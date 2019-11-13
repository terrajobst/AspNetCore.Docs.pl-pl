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
# <a name="browser-link-in-aspnet-core"></a><span data-ttu-id="98713-103">Link przeglądarki w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="98713-103">Browser Link in ASP.NET Core</span></span>

<span data-ttu-id="98713-104">Autorzy [Nicolò Carandini](https://github.com/ncarandini), [Jan Wasson](https://github.com/MikeWasson)i [Tomasz Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="98713-104">By [Nicolò Carandini](https://github.com/ncarandini), [Mike Wasson](https://github.com/MikeWasson), and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="98713-105">Link do przeglądarki to funkcja w programie Visual Studio, która tworzy kanał komunikacyjny między środowiskiem deweloperskim i jedną lub wieloma przeglądarkami sieci Web.</span><span class="sxs-lookup"><span data-stu-id="98713-105">Browser Link is a feature in Visual Studio that creates a communication channel between the development environment and one or more web browsers.</span></span> <span data-ttu-id="98713-106">Możesz użyć linku przeglądarki, aby odświeżyć aplikację sieci Web w kilku przeglądarkach jednocześnie, co jest przydatne w przypadku testowania między przeglądarkami.</span><span class="sxs-lookup"><span data-stu-id="98713-106">You can use Browser Link to refresh your web application in several browsers at once, which is useful for cross-browser testing.</span></span>

## <a name="browser-link-setup"></a><span data-ttu-id="98713-107">Konfiguracja linku przeglądarki</span><span class="sxs-lookup"><span data-stu-id="98713-107">Browser Link setup</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="98713-108">Podczas konwertowania projektu ASP.NET Core 2,0 na ASP.NET Core 2,1 i przejścia do [pakietu Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app), zainstaluj pakiet [Microsoft. VisualStudio. Web. BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) dla funkcji BrowserLink.</span><span class="sxs-lookup"><span data-stu-id="98713-108">When converting an ASP.NET Core 2.0 project to ASP.NET Core 2.1 and transitioning to the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), install the [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) package for BrowserLink functionality.</span></span> <span data-ttu-id="98713-109">Szablony projektów ASP.NET Core 2,1 używają domyślnie pakietu `Microsoft.AspNetCore.App`.</span><span class="sxs-lookup"><span data-stu-id="98713-109">The ASP.NET Core 2.1 project templates use the `Microsoft.AspNetCore.App` metapackage by default.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="98713-110">Szablony projektu **aplikacji sieci Web**, **pustej**i **internetowego interfejsu API** ASP.NET Core 2,0 używają [pakietu Microsoft. AspNetCore. All](xref:fundamentals/metapackage), który zawiera odwołanie do pakietu dla [Microsoft. VisualStudio. Web. BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/).</span><span class="sxs-lookup"><span data-stu-id="98713-110">The ASP.NET Core 2.0 **Web Application**, **Empty**, and **Web API** project templates use the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage), which contains a package reference for [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/).</span></span> <span data-ttu-id="98713-111">W związku z tym używanie pakietu `Microsoft.AspNetCore.All` nie wymaga żadnych dalszych działań w celu udostępnienia linku przeglądarki do użycia.</span><span class="sxs-lookup"><span data-stu-id="98713-111">Therefore, using the `Microsoft.AspNetCore.All` metapackage requires no further action to make Browser Link available for use.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="98713-112">Szablon projektu **aplikacji sieci Web** ASP.NET Core 1. x zawiera odwołanie do pakietu dla pakietu [Microsoft. VisualStudio. Web. BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) .</span><span class="sxs-lookup"><span data-stu-id="98713-112">The ASP.NET Core 1.x **Web Application** project template has a package reference for the [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) package.</span></span> <span data-ttu-id="98713-113">Aby dodać odwołanie do `Microsoft.VisualStudio.Web.BrowserLink`, w projektach szablonu lub **interfejsie API sieci Web** **jest wymagane** dodanie odwołania do pakietu.</span><span class="sxs-lookup"><span data-stu-id="98713-113">The **Empty** or **Web API** template projects require you to add a package reference to `Microsoft.VisualStudio.Web.BrowserLink`.</span></span>

<span data-ttu-id="98713-114">Ponieważ jest to funkcja programu Visual Studio, najprostszym sposobem dodania pakietu do **pustego** lub projektu szablonu **interfejsu API sieci Web** jest otwarcie **konsoli Menedżera pakietów** (**Wyświetl** > innej **konsoli Menedżera pakietów**> **Windows** ) i uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="98713-114">Since this is a Visual Studio feature, the easiest way to add the package to an **Empty** or **Web API** template project is to open the **Package Manager Console** (**View** > **Other Windows** > **Package Manager Console**) and run the following command:</span></span>

```console
install-package Microsoft.VisualStudio.Web.BrowserLink
```

<span data-ttu-id="98713-115">Alternatywnie można użyć **Menedżera pakietów NuGet**.</span><span class="sxs-lookup"><span data-stu-id="98713-115">Alternatively, you can use **NuGet Package Manager**.</span></span> <span data-ttu-id="98713-116">Kliknij prawym przyciskiem myszy nazwę projektu w **Eksplorator rozwiązań** i wybierz polecenie **Zarządzaj pakietami NuGet**:</span><span class="sxs-lookup"><span data-stu-id="98713-116">Right-click the project name in **Solution Explorer** and choose **Manage NuGet Packages**:</span></span>

![Otwórz Menedżera pakietów NuGet](using-browserlink/_static/open-nuget-package-manager.png)

<span data-ttu-id="98713-118">Znajdź i zainstaluj pakiet:</span><span class="sxs-lookup"><span data-stu-id="98713-118">Find and install the package:</span></span>

![Dodaj pakiet za pomocą Menedżera pakietów NuGet](using-browserlink/_static/add-package-with-nuget-package-manager.png)

::: moniker-end

### <a name="configuration"></a><span data-ttu-id="98713-120">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="98713-120">Configuration</span></span>

<span data-ttu-id="98713-121">W `Startup.Configure` metodzie:</span><span class="sxs-lookup"><span data-stu-id="98713-121">In the `Startup.Configure` method:</span></span>

```csharp
app.UseBrowserLink();
```

<span data-ttu-id="98713-122">Zazwyczaj kod znajduje się w bloku `if`, który umożliwia tylko łącze przeglądarki w środowisku programistycznym, jak pokazano poniżej:</span><span class="sxs-lookup"><span data-stu-id="98713-122">Usually the code is inside an `if` block that only enables Browser Link in the Development environment, as shown here:</span></span>

```csharp
if (env.IsDevelopment())
{
    app.UseDeveloperExceptionPage();
    app.UseBrowserLink();
}
```

<span data-ttu-id="98713-123">Aby uzyskać więcej informacji, zobacz [Korzystanie z wielu środowisk](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="98713-123">For more information, see [Use multiple environments](xref:fundamentals/environments).</span></span>

## <a name="how-to-use-browser-link"></a><span data-ttu-id="98713-124">Jak używać linku przeglądarki</span><span class="sxs-lookup"><span data-stu-id="98713-124">How to use Browser Link</span></span>

<span data-ttu-id="98713-125">Gdy masz otwarty projekt ASP.NET Core, program Visual Studio wyświetli kontrolkę pasek narzędzi linku przeglądarki obok kontrolki paska narzędzi **elementu docelowego debugowania** :</span><span class="sxs-lookup"><span data-stu-id="98713-125">When you have an ASP.NET Core project open, Visual Studio shows the Browser Link toolbar control next to the **Debug Target** toolbar control:</span></span>

![Menu rozwijane linku przeglądarki](using-browserlink/_static/browserLink-dropdown-menu.png)

<span data-ttu-id="98713-127">Za pomocą kontrolki paska narzędzi łącza przeglądarki można:</span><span class="sxs-lookup"><span data-stu-id="98713-127">From the Browser Link toolbar control, you can:</span></span>

* <span data-ttu-id="98713-128">Odśwież aplikację sieci Web w kilku przeglądarkach jednocześnie.</span><span class="sxs-lookup"><span data-stu-id="98713-128">Refresh the web application in several browsers at once.</span></span>
* <span data-ttu-id="98713-129">Otwórz **pulpit nawigacyjny linku do przeglądarki**.</span><span class="sxs-lookup"><span data-stu-id="98713-129">Open the **Browser Link Dashboard**.</span></span>
* <span data-ttu-id="98713-130">Włącza lub wyłącza **link przeglądarki**.</span><span class="sxs-lookup"><span data-stu-id="98713-130">Enable or disable **Browser Link**.</span></span> <span data-ttu-id="98713-131">Uwaga: łącze przeglądarki jest domyślnie wyłączone w programie Visual Studio 2017 (15,3).</span><span class="sxs-lookup"><span data-stu-id="98713-131">Note: Browser Link is disabled by default in Visual Studio 2017 (15.3).</span></span>
* <span data-ttu-id="98713-132">Włączać lub wyłączać funkcję [autosynchronizacji CSS](#enable-or-disable-css-auto-sync).</span><span class="sxs-lookup"><span data-stu-id="98713-132">Enable or disable [CSS Auto-Sync](#enable-or-disable-css-auto-sync).</span></span>

> [!NOTE]
> <span data-ttu-id="98713-133">Niektóre wtyczki programu Visual Studio, w tym *pakiet rozszerzeń sieci web 2015* i *pakiet rozszerzeń sieci Web 2017*, oferują rozszerzoną funkcję dla łącza przeglądarki, ale niektóre dodatkowe funkcje nie współpracują z projektami ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="98713-133">Some Visual Studio plug-ins, most notably *Web Extension Pack 2015* and *Web Extension Pack 2017*, offer extended functionality for Browser Link, but some of the additional features don't work with ASP.NET Core projects.</span></span>

## <a name="refresh-the-web-app-in-several-browsers-at-once"></a><span data-ttu-id="98713-134">Odśwież aplikację sieci Web w kilku przeglądarkach jednocześnie</span><span class="sxs-lookup"><span data-stu-id="98713-134">Refresh the web app in several browsers at once</span></span>

<span data-ttu-id="98713-135">Aby wybrać jedną przeglądarkę sieci Web, która ma być uruchamiana podczas uruchamiania projektu, użyj menu rozwijanego w formancie paska narzędzi **elementu docelowego debugowania** :</span><span class="sxs-lookup"><span data-stu-id="98713-135">To choose a single web browser to launch when starting the project, use the drop-down menu in the **Debug Target** toolbar control:</span></span>

![Menu rozwijane F5](using-browserlink/_static/debug-target-dropdown-menu.png)

<span data-ttu-id="98713-137">Aby otworzyć wiele przeglądarek jednocześnie, wybierz pozycję **Przeglądaj za pomocą...** z tego samego listy rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="98713-137">To open multiple browsers at once, choose **Browse with...** from the same drop-down.</span></span> <span data-ttu-id="98713-138">Naciśnij i przytrzymaj klawisz CTRL, aby wybrać przeglądarki, a następnie kliknij przycisk **Przeglądaj**:</span><span class="sxs-lookup"><span data-stu-id="98713-138">Hold down the CTRL key to select the browsers you want, and then click **Browse**:</span></span>

![Otwórz wiele przeglądarek jednocześnie](using-browserlink/_static/open-many-browsers-at-once.png)

<span data-ttu-id="98713-140">Oto zrzut ekranu przedstawiający program Visual Studio z otwartym widokiem indeksu i dwiema otwartymi przeglądarkami:</span><span class="sxs-lookup"><span data-stu-id="98713-140">Here's a screenshot showing Visual Studio with the Index view open and two open browsers:</span></span>

![Przykład synchronizacji z dwoma przeglądarkami](using-browserlink/_static/sync-with-two-browsers-example.png)

<span data-ttu-id="98713-142">Umieść kursor nad kontrolką paska narzędzi łącza przeglądarki, aby wyświetlić przeglądarki, które są połączone z projektem:</span><span class="sxs-lookup"><span data-stu-id="98713-142">Hover over the Browser Link toolbar control to see the browsers that are connected to the project:</span></span>

![Wskazówki dotyczące aktywowania](using-browserlink/_static/hoover-tip.png)

<span data-ttu-id="98713-144">Zmień widok indeksu, a wszystkie połączone przeglądarki są aktualizowane po kliknięciu przycisku Odśwież linku do przeglądarki:</span><span class="sxs-lookup"><span data-stu-id="98713-144">Change the Index view, and all connected browsers are updated when you click the Browser Link refresh button:</span></span>

![przeglądarki — synchronizacja ze zmianami](using-browserlink/_static/browsers-sync-to-changes.png)

<span data-ttu-id="98713-146">Łącze przeglądarki współpracuje również z przeglądarkami uruchamianymi spoza programu Visual Studio i przejdź do adresu URL aplikacji.</span><span class="sxs-lookup"><span data-stu-id="98713-146">Browser Link also works with browsers that you launch from outside Visual Studio and navigate to the application URL.</span></span>

### <a name="the-browser-link-dashboard"></a><span data-ttu-id="98713-147">Pulpit nawigacyjny linków przeglądarki</span><span class="sxs-lookup"><span data-stu-id="98713-147">The Browser Link Dashboard</span></span>

<span data-ttu-id="98713-148">Otwórz pulpit nawigacyjny link przeglądarki z menu rozwijanego linku do przeglądarki, aby zarządzać połączeniem przy użyciu otwartych przeglądarek:</span><span class="sxs-lookup"><span data-stu-id="98713-148">Open the Browser Link Dashboard from the Browser Link drop down menu to manage the connection with open browsers:</span></span>

![Open-browserslink — pulpit nawigacyjny](using-browserlink/_static/open-browserlink-dashboard.png)

<span data-ttu-id="98713-150">Jeśli żadna przeglądarka nie jest połączona, możesz uruchomić sesję niedebugowania, wybierając *Widok w przeglądarce* link:</span><span class="sxs-lookup"><span data-stu-id="98713-150">If no browser is connected, you can start a non-debugging session by selecting the *View in Browser* link:</span></span>

![browserlink — pulpit nawigacyjny — brak połączeń](using-browserlink/_static/browserlink-dashboard-no-connections.png)

<span data-ttu-id="98713-152">W przeciwnym razie połączone przeglądarki są wyświetlane ze ścieżką do strony, którą pokazuje Każda przeglądarka:</span><span class="sxs-lookup"><span data-stu-id="98713-152">Otherwise, the connected browsers are shown with the path to the page that each browser is showing:</span></span>

![browserlink — pulpit nawigacyjny — dwa połączenia](using-browserlink/_static/browserlink-dashboard-two-connections.png)

<span data-ttu-id="98713-154">Jeśli chcesz, możesz kliknąć nazwę przeglądarki wyświetlonej w celu odświeżenia pojedynczej przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="98713-154">If you like, you can click on a listed browser name to refresh that single browser.</span></span>

### <a name="enable-or-disable-browser-link"></a><span data-ttu-id="98713-155">Włącz lub Wyłącz link przeglądarki</span><span class="sxs-lookup"><span data-stu-id="98713-155">Enable or disable Browser Link</span></span>

<span data-ttu-id="98713-156">Po ponownym włączeniu linku przeglądarki po jego wyłączeniu należy odświeżyć przeglądarki w celu ponownego nawiązania połączenia.</span><span class="sxs-lookup"><span data-stu-id="98713-156">When you re-enable Browser Link after disabling it, you must refresh the browsers to reconnect them.</span></span>

### <a name="enable-or-disable-css-auto-sync"></a><span data-ttu-id="98713-157">Włączanie lub wyłączanie autosynchronizacji CSS</span><span class="sxs-lookup"><span data-stu-id="98713-157">Enable or disable CSS Auto-Sync</span></span>

<span data-ttu-id="98713-158">Gdy automatyczna synchronizacja CSS jest włączona, podłączane przeglądarki są automatycznie odświeżane po wprowadzeniu jakichkolwiek zmian w plikach CSS.</span><span class="sxs-lookup"><span data-stu-id="98713-158">When CSS Auto-Sync is enabled, connected browsers are automatically refreshed when you make any change to CSS files.</span></span>

## <a name="how-it-works"></a><span data-ttu-id="98713-159">Jak to działa</span><span class="sxs-lookup"><span data-stu-id="98713-159">How it works</span></span>

<span data-ttu-id="98713-160">Link przeglądarki używa SignalR do tworzenia kanału komunikacyjnego między programem Visual Studio i przeglądarką.</span><span class="sxs-lookup"><span data-stu-id="98713-160">Browser Link uses SignalR to create a communication channel between Visual Studio and the browser.</span></span> <span data-ttu-id="98713-161">Gdy łącze przeglądarki jest włączone, program Visual Studio działa jako serwer SignalR, z którym mogą się łączyć wielu klientów (przeglądarki).</span><span class="sxs-lookup"><span data-stu-id="98713-161">When Browser Link is enabled, Visual Studio acts as a SignalR server that multiple clients (browsers) can connect to.</span></span> <span data-ttu-id="98713-162">Link przeglądarki rejestruje również składnik pośredniczący w potoku żądania ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="98713-162">Browser Link also registers a middleware component in the ASP.NET Core request pipeline.</span></span> <span data-ttu-id="98713-163">Ten składnik wprowadza specjalne odwołania `<script>` do każdego żądania strony z serwera.</span><span class="sxs-lookup"><span data-stu-id="98713-163">This component injects special `<script>` references into every page request from the server.</span></span> <span data-ttu-id="98713-164">Odwołania do skryptów można zobaczyć, wybierając pozycję **Wyświetl źródło** w przeglądarce i przewinięcie do końca `<body>` zawartości tagu:</span><span class="sxs-lookup"><span data-stu-id="98713-164">You can see the script references by selecting **View source** in the browser and scrolling to the end of the `<body>` tag content:</span></span>

```html
    <!-- Visual Studio Browser Link -->
    <script type="application/json" id="__browserLink_initializationData">
        {"requestId":"a717d5a07c1741949a7cefd6fa2bad08","requestMappingFromServer":false}
    </script>
    <script type="text/javascript" src="http://localhost:54139/b6e36e429d034f578ebccd6a79bf19bf/browserLink" async="async"></script>
    <!-- End Browser Link -->
</body>
```

<span data-ttu-id="98713-165">Pliki źródłowe nie są modyfikowane.</span><span class="sxs-lookup"><span data-stu-id="98713-165">Your source files aren't modified.</span></span> <span data-ttu-id="98713-166">Składnik pośredniczący wprowadza odwołania do skryptów dynamicznie.</span><span class="sxs-lookup"><span data-stu-id="98713-166">The middleware component injects the script references dynamically.</span></span>

<span data-ttu-id="98713-167">Ponieważ kod po stronie przeglądarki to wszystkie skrypty JavaScript, działa on we wszystkich przeglądarkach, które SignalR obsługuje bez wtyczki przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="98713-167">Because the browser-side code is all JavaScript, it works on all browsers that SignalR supports without requiring a browser plug-in.</span></span>
