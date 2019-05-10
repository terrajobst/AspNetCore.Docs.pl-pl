---
title: Łączność z przeglądarkami w programie ASP.NET Core
author: ncarandini
description: Wyjaśnia, jak Browser Link jest funkcja programu Visual Studio, która łączy środowisko projektowe z jednej lub kilku przeglądarkach sieci web.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 09/22/2017
uid: client-side/using-browserlink
ms.openlocfilehash: 452ba5149563c186750466f471c7b950f0017614
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/27/2019
ms.locfileid: "64900850"
---
# <a name="browser-link-in-aspnet-core"></a><span data-ttu-id="0f117-103">Łączność z przeglądarkami w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0f117-103">Browser Link in ASP.NET Core</span></span>

<span data-ttu-id="0f117-104">Przez [Nicolò Carandini](https://github.com/ncarandini), [Mike Wasson](https://github.com/MikeWasson), i [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="0f117-104">By [Nicolò Carandini](https://github.com/ncarandini), [Mike Wasson](https://github.com/MikeWasson), and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="0f117-105">Łącze przeglądarki jest funkcją w programie Visual Studio tworzy kanał komunikacyjny między środowiska programistycznego i jeden lub więcej przeglądarek sieci web.</span><span class="sxs-lookup"><span data-stu-id="0f117-105">Browser Link is a feature in Visual Studio that creates a communication channel between the development environment and one or more web browsers.</span></span> <span data-ttu-id="0f117-106">Można użyć łącze przeglądarki, aby odświeżyć aplikację sieci web w kilku przeglądarkach jednocześnie, która jest przydatna przy testowaniu obsługiwania wielu przeglądarek.</span><span class="sxs-lookup"><span data-stu-id="0f117-106">You can use Browser Link to refresh your web application in several browsers at once, which is useful for cross-browser testing.</span></span>

## <a name="browser-link-setup"></a><span data-ttu-id="0f117-107">Konfigurowanie Linku przeglądarki</span><span class="sxs-lookup"><span data-stu-id="0f117-107">Browser Link setup</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="0f117-108">Podczas konwersji projektu programu ASP.NET Core 2.0 platformy ASP.NET Core 2.1 i przechodzenie do [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app), zainstaluj [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) pakietu dla Funkcje BrowserLink.</span><span class="sxs-lookup"><span data-stu-id="0f117-108">When converting an ASP.NET Core 2.0 project to ASP.NET Core 2.1 and transitioning to the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), install the [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) package for BrowserLink functionality.</span></span> <span data-ttu-id="0f117-109">Użyj szablonów projektów platformy ASP.NET Core 2.1 `Microsoft.AspNetCore.App` meta Microsoft.aspnetcore.all domyślnie.</span><span class="sxs-lookup"><span data-stu-id="0f117-109">The ASP.NET Core 2.1 project templates use the `Microsoft.AspNetCore.App` metapackage by default.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="0f117-110">ASP.NET Core 2.0 **aplikacji sieci Web**, **pusty**, i **interfejsu API sieci Web** Użyj szablonów projektu [pakiet meta Microsoft.aspnetcore.all](xref:fundamentals/metapackage) , który zawiera odwołania do pakietu dla [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/).</span><span class="sxs-lookup"><span data-stu-id="0f117-110">The ASP.NET Core 2.0 **Web Application**, **Empty**, and **Web API** project templates use the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage), which contains a package reference for [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/).</span></span> <span data-ttu-id="0f117-111">W związku z tym, za pomocą `Microsoft.AspNetCore.All` meta Microsoft.aspnetcore.all nie wymaga żadnych dodatkowych czynności, aby udostępnić łącze przeglądarki do użycia.</span><span class="sxs-lookup"><span data-stu-id="0f117-111">Therefore, using the `Microsoft.AspNetCore.All` metapackage requires no further action to make Browser Link available for use.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="0f117-112">ASP.NET Core 1.x **aplikacji sieci Web** szablon projektu zawiera odwołania do pakietu dla [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) pakietu.</span><span class="sxs-lookup"><span data-stu-id="0f117-112">The ASP.NET Core 1.x **Web Application** project template has a package reference for the [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) package.</span></span> <span data-ttu-id="0f117-113">**Pusty** lub **interfejsu API sieci Web** wymagają szablonów projektów, można dodać odwołania do pakietu do `Microsoft.VisualStudio.Web.BrowserLink`.</span><span class="sxs-lookup"><span data-stu-id="0f117-113">The **Empty** or **Web API** template projects require you to add a package reference to `Microsoft.VisualStudio.Web.BrowserLink`.</span></span>

<span data-ttu-id="0f117-114">Ponieważ jest najprostszym sposobem to funkcja programu Visual Studio, aby dodać pakiet do **pusty** lub **interfejsu API sieci Web** szablon projektu, należy otworzyć **Konsola Menedżera pakietów** (**Widoku** > **Windows inne** > **Konsola Menedżera pakietów**) i uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="0f117-114">Since this is a Visual Studio feature, the easiest way to add the package to an **Empty** or **Web API** template project is to open the **Package Manager Console** (**View** > **Other Windows** > **Package Manager Console**) and run the following command:</span></span>

```console
install-package Microsoft.VisualStudio.Web.BrowserLink
```

<span data-ttu-id="0f117-115">Alternatywnie, można użyć **Menedżera pakietów NuGet**.</span><span class="sxs-lookup"><span data-stu-id="0f117-115">Alternatively, you can use **NuGet Package Manager**.</span></span> <span data-ttu-id="0f117-116">Kliknij prawym przyciskiem myszy nazwę projektu w **Eksploratora rozwiązań** i wybierz polecenie **Zarządzaj pakietami NuGet**:</span><span class="sxs-lookup"><span data-stu-id="0f117-116">Right-click the project name in **Solution Explorer** and choose **Manage NuGet Packages**:</span></span>

![Menedżer pakietów NuGet Otwórz](using-browserlink/_static/open-nuget-package-manager.png)

<span data-ttu-id="0f117-118">Znajdź i zainstaluj pakiet:</span><span class="sxs-lookup"><span data-stu-id="0f117-118">Find and install the package:</span></span>

![Dodaj pakiet przy użyciu Menedżera pakietów NuGet](using-browserlink/_static/add-package-with-nuget-package-manager.png)

::: moniker-end

### <a name="configuration"></a><span data-ttu-id="0f117-120">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="0f117-120">Configuration</span></span>

<span data-ttu-id="0f117-121">W `Startup.Configure` metody:</span><span class="sxs-lookup"><span data-stu-id="0f117-121">In the `Startup.Configure` method:</span></span>

```csharp
app.UseBrowserLink();
```

<span data-ttu-id="0f117-122">Zazwyczaj kod znajduje się wewnątrz `if` blok, który umożliwia łączność z przeglądarkami jedynie w środowisku deweloperskim, jak pokazano poniżej:</span><span class="sxs-lookup"><span data-stu-id="0f117-122">Usually the code is inside an `if` block that only enables Browser Link in the Development environment, as shown here:</span></span>

```csharp
if (env.IsDevelopment())
{
    app.UseDeveloperExceptionPage();
    app.UseBrowserLink();
}
```

<span data-ttu-id="0f117-123">Aby uzyskać więcej informacji, zobacz [używanie wielu środowisk](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="0f117-123">For more information, see [Use multiple environments](xref:fundamentals/environments).</span></span>

## <a name="how-to-use-browser-link"></a><span data-ttu-id="0f117-124">Jak używać łączy przeglądarki</span><span class="sxs-lookup"><span data-stu-id="0f117-124">How to use Browser Link</span></span>

<span data-ttu-id="0f117-125">Mając otwarty projekt platformy ASP.NET Core, Visual Studio wyświetla obok pozycji formantu paska narzędzi łącza przeglądarki **Debuguj element docelowy** formantu paska narzędzi:</span><span class="sxs-lookup"><span data-stu-id="0f117-125">When you have an ASP.NET Core project open, Visual Studio shows the Browser Link toolbar control next to the **Debug Target** toolbar control:</span></span>

![Menu rozwijane łącza przeglądarki](using-browserlink/_static/browserLink-dropdown-menu.png)

<span data-ttu-id="0f117-127">Z formantem paska narzędzi łącza przeglądarki możesz wykonywać następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="0f117-127">From the Browser Link toolbar control, you can:</span></span>

* <span data-ttu-id="0f117-128">Odświeżanie aplikacji sieci web w kilku przeglądarkach naraz.</span><span class="sxs-lookup"><span data-stu-id="0f117-128">Refresh the web application in several browsers at once.</span></span>
* <span data-ttu-id="0f117-129">Otwórz **nawigacyjnym łącza przeglądarki**.</span><span class="sxs-lookup"><span data-stu-id="0f117-129">Open the **Browser Link Dashboard**.</span></span>
* <span data-ttu-id="0f117-130">Włączanie lub wyłączanie **łączność z przeglądarkami**.</span><span class="sxs-lookup"><span data-stu-id="0f117-130">Enable or disable **Browser Link**.</span></span> <span data-ttu-id="0f117-131">Uwaga: Łącze przeglądarki jest domyślnie wyłączona, w programie Visual Studio 2017 (15.3).</span><span class="sxs-lookup"><span data-stu-id="0f117-131">Note: Browser Link is disabled by default in Visual Studio 2017 (15.3).</span></span>
* <span data-ttu-id="0f117-132">Włączanie lub wyłączanie [automatyczna synchronizacja CSS](#enable-or-disable-css-auto-sync).</span><span class="sxs-lookup"><span data-stu-id="0f117-132">Enable or disable [CSS Auto-Sync](#enable-or-disable-css-auto-sync).</span></span>

> [!NOTE]
> <span data-ttu-id="0f117-133">Niektóre dodatki plug-in programu Visual Studio, głównie *2015 pakietu rozszerzenia sieci Web* i *2017 pakietu rozszerzenia sieci Web*, oferują rozszerzoną funkcjonalność na łączność z przeglądarkami, ale niektóre dodatkowe funkcje nie działają z ASP. Projekty .NET Core.</span><span class="sxs-lookup"><span data-stu-id="0f117-133">Some Visual Studio plug-ins, most notably *Web Extension Pack 2015* and *Web Extension Pack 2017*, offer extended functionality for Browser Link, but some of the additional features don't work with ASP.NET Core projects.</span></span>

## <a name="refresh-the-web-app-in-several-browsers-at-once"></a><span data-ttu-id="0f117-134">Odśwież aplikację sieci web w kilku przeglądarkach jednocześnie</span><span class="sxs-lookup"><span data-stu-id="0f117-134">Refresh the web app in several browsers at once</span></span>

<span data-ttu-id="0f117-135">Aby wybrać przeglądarkę jednej sieci web do uruchamiania podczas uruchamiania projektu, użyj menu rozwijane w **Debuguj element docelowy** formantu paska narzędzi:</span><span class="sxs-lookup"><span data-stu-id="0f117-135">To choose a single web browser to launch when starting the project, use the drop-down menu in the **Debug Target** toolbar control:</span></span>

![Menu rozwijane F5](using-browserlink/_static/debug-target-dropdown-menu.png)

<span data-ttu-id="0f117-137">Aby otworzyć jednocześnie wiele przeglądarek, wybierz **przeglądania przy użyciu...**  z tej samej listy rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="0f117-137">To open multiple browsers at once, choose **Browse with...** from the same drop-down.</span></span> <span data-ttu-id="0f117-138">Naciśnij i przytrzymaj klawisz CTRL, aby wybrać przeglądarek ma, a następnie kliknij przycisk **Przeglądaj**:</span><span class="sxs-lookup"><span data-stu-id="0f117-138">Hold down the CTRL key to select the browsers you want, and then click **Browse**:</span></span>

![Otwórz jednocześnie wiele przeglądarek](using-browserlink/_static/open-many-browsers-at-once.png)

<span data-ttu-id="0f117-140">Poniżej przedstawiono zrzut ekranu przedstawiający otwarte programu Visual Studio przy użyciu widoku indeksu i otwórz przeglądarek:</span><span class="sxs-lookup"><span data-stu-id="0f117-140">Here's a screenshot showing Visual Studio with the Index view open and two open browsers:</span></span>

![Synchronizuj z przykład przeglądarek](using-browserlink/_static/sync-with-two-browsers-example.png)

<span data-ttu-id="0f117-142">Zatrzymaj wskaźnik myszy nad formant paska narzędzi łącza przeglądarki, aby przeglądarki, które są podłączone do projektu:</span><span class="sxs-lookup"><span data-stu-id="0f117-142">Hover over the Browser Link toolbar control to see the browsers that are connected to the project:</span></span>

![Porada po wskazaniu wskaźnikiem](using-browserlink/_static/hoover-tip.png)

<span data-ttu-id="0f117-144">Zmień widok indeksu i wszystkich połączonych przeglądarek są aktualizowane, po kliknięciu przycisku odświeżania łączność z przeglądarkami:</span><span class="sxs-lookup"><span data-stu-id="0f117-144">Change the Index view, and all connected browsers are updated when you click the Browser Link refresh button:</span></span>

![przeglądarki synchronizacji do zmiany](using-browserlink/_static/browsers-sync-to-changes.png)

<span data-ttu-id="0f117-146">Łączność z przeglądarkami współpracuje również z przeglądarki, które Uruchom z poza programem Visual Studio i przejdź do adresu URL aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0f117-146">Browser Link also works with browsers that you launch from outside Visual Studio and navigate to the application URL.</span></span>

### <a name="the-browser-link-dashboard"></a><span data-ttu-id="0f117-147">Pulpicie nawigacyjnym łącza przeglądarki</span><span class="sxs-lookup"><span data-stu-id="0f117-147">The Browser Link Dashboard</span></span>

<span data-ttu-id="0f117-148">Otwórz pulpicie nawigacyjnym łącza przeglądarki z przeglądarkami rozwijanego menu, aby zarządzać połączeniem z przeglądarkami Otwórz:</span><span class="sxs-lookup"><span data-stu-id="0f117-148">Open the Browser Link Dashboard from the Browser Link drop down menu to manage the connection with open browsers:</span></span>

![open-browserslink-dashboard](using-browserlink/_static/open-browserlink-dashboard.png)

<span data-ttu-id="0f117-150">Jeśli przeglądarka nie jest połączony, można uruchomić sesji bez debugowania, wybierając *Pokaż w przeglądarce* łącza:</span><span class="sxs-lookup"><span data-stu-id="0f117-150">If no browser is connected, you can start a non-debugging session by selecting the *View in Browser* link:</span></span>

![browserlink-dashboard-no-connections](using-browserlink/_static/browserlink-dashboard-no-connections.png)

<span data-ttu-id="0f117-152">W przeciwnym razie połączonych przeglądarek są wyświetlane ze ścieżką do strony, która jest wyświetlane w każdej przeglądarce:</span><span class="sxs-lookup"><span data-stu-id="0f117-152">Otherwise, the connected browsers are shown with the path to the page that each browser is showing:</span></span>

![browserlink-dashboard-two-connections](using-browserlink/_static/browserlink-dashboard-two-connections.png)

<span data-ttu-id="0f117-154">Jeśli chcesz możesz kliknąć nazwę listy przeglądarki, aby odświeżyć tego jednej przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="0f117-154">If you like, you can click on a listed browser name to refresh that single browser.</span></span>

### <a name="enable-or-disable-browser-link"></a><span data-ttu-id="0f117-155">Włączanie lub wyłączanie łączność z przeglądarkami</span><span class="sxs-lookup"><span data-stu-id="0f117-155">Enable or disable Browser Link</span></span>

<span data-ttu-id="0f117-156">Po ponownym włączeniu łączność z przeglądarkami po wyłączeniu go, należy odświeżyć przeglądarek, podłącz je ponownie.</span><span class="sxs-lookup"><span data-stu-id="0f117-156">When you re-enable Browser Link after disabling it, you must refresh the browsers to reconnect them.</span></span>

### <a name="enable-or-disable-css-auto-sync"></a><span data-ttu-id="0f117-157">Włączanie lub wyłączanie automatycznej synchronizacji CSS</span><span class="sxs-lookup"><span data-stu-id="0f117-157">Enable or disable CSS Auto-Sync</span></span>

<span data-ttu-id="0f117-158">Po włączeniu automatycznej synchronizacji CSS połączonych przeglądarek są automatycznie odświeżane, gdy wprowadzać zmian w plikach CSS.</span><span class="sxs-lookup"><span data-stu-id="0f117-158">When CSS Auto-Sync is enabled, connected browsers are automatically refreshed when you make any change to CSS files.</span></span>

## <a name="how-it-works"></a><span data-ttu-id="0f117-159">Jak to działa</span><span class="sxs-lookup"><span data-stu-id="0f117-159">How it works</span></span>

<span data-ttu-id="0f117-160">Łączność z przeglądarkami używa SignalR w celu utworzenia kanał komunikacyjny między Visual Studio i przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="0f117-160">Browser Link uses SignalR to create a communication channel between Visual Studio and the browser.</span></span> <span data-ttu-id="0f117-161">Po włączeniu łączność z przeglądarkami programu Visual Studio działa jako serwer biblioteki SignalR, wielu klientów (przeglądarki) można łączyć się.</span><span class="sxs-lookup"><span data-stu-id="0f117-161">When Browser Link is enabled, Visual Studio acts as a SignalR server that multiple clients (browsers) can connect to.</span></span> <span data-ttu-id="0f117-162">Łączność z przeglądarkami rejestruje również składnik oprogramowania pośredniczącego w potoku żądania programu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0f117-162">Browser Link also registers a middleware component in the ASP.NET Core request pipeline.</span></span> <span data-ttu-id="0f117-163">Ten składnik wprowadza specjalne `<script>` odwołań w każdym żądaniu strony z serwera.</span><span class="sxs-lookup"><span data-stu-id="0f117-163">This component injects special `<script>` references into every page request from the server.</span></span> <span data-ttu-id="0f117-164">Odwołania do skryptu można wyświetlić, wybierając **Wyświetl źródło** w przeglądarce i przewijając do końca `<body>` tagować zawartość:</span><span class="sxs-lookup"><span data-stu-id="0f117-164">You can see the script references by selecting **View source** in the browser and scrolling to the end of the `<body>` tag content:</span></span>

```html
    <!-- Visual Studio Browser Link -->
    <script type="application/json" id="__browserLink_initializationData">
        {"requestId":"a717d5a07c1741949a7cefd6fa2bad08","requestMappingFromServer":false}
    </script>
    <script type="text/javascript" src="http://localhost:54139/b6e36e429d034f578ebccd6a79bf19bf/browserLink" async="async"></script>
    <!-- End Browser Link -->
</body>
```

<span data-ttu-id="0f117-165">Nie modyfikować plików źródłowych.</span><span class="sxs-lookup"><span data-stu-id="0f117-165">Your source files aren't modified.</span></span> <span data-ttu-id="0f117-166">Składnik oprogramowania pośredniczącego, które dynamicznie wprowadza odwołania do skryptu.</span><span class="sxs-lookup"><span data-stu-id="0f117-166">The middleware component injects the script references dynamically.</span></span>

<span data-ttu-id="0f117-167">Ponieważ kod po stronie przeglądarki jest kod JavaScript, działa we wszystkich przeglądarkach obsługiwanych przez SignalR bez konieczności wtyczkę przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="0f117-167">Because the browser-side code is all JavaScript, it works on all browsers that SignalR supports without requiring a browser plug-in.</span></span>
