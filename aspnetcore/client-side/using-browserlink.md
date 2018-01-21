---
title: "Łącze przeglądarki w platformy ASP.NET Core"
author: ncarandini
description: "Wyjaśnia, jak łącze przeglądarki funkcja Visual Studio, która łączy środowiska programistycznego z co najmniej jeden przeglądarki sieci web."
ms.author: riande
manager: wpickett
ms.date: 09/22/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/using-browserlink
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d5db65c268923e96c45b034639437fc3496ccac1
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/19/2018
---
# <a name="browser-link-in-aspnet-core"></a><span data-ttu-id="c9eec-103">Łącze przeglądarki w platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c9eec-103">Browser Link in ASP.NET Core</span></span> 

<span data-ttu-id="c9eec-104">Przez [Nicolò Carandini](https://github.com/ncarandini), [Wasson Jan](https://github.com/MikeWasson), i [Dykstra niestandardowy](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="c9eec-104">By [Nicolò Carandini](https://github.com/ncarandini), [Mike Wasson](https://github.com/MikeWasson), and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="c9eec-105">Łącze przeglądarki jest funkcją w programie Visual Studio, tworzącym kanał komunikacji między Środowisko deweloperskie i co najmniej jeden przeglądarki sieci web.</span><span class="sxs-lookup"><span data-stu-id="c9eec-105">Browser Link is a feature in Visual Studio that creates a communication channel between the development environment and one or more web browsers.</span></span> <span data-ttu-id="c9eec-106">Można użyć łącze przeglądarki, aby odświeżyć aplikacji sieci web w wielu przeglądarkach jednocześnie, która jest przydatna przy testowaniu różnych przeglądarkach.</span><span class="sxs-lookup"><span data-stu-id="c9eec-106">You can use Browser Link to refresh your web application in several browsers at once, which is useful for cross-browser testing.</span></span>

## <a name="browser-link-setup"></a><span data-ttu-id="c9eec-107">Ustawienia Linku przeglądarki</span><span class="sxs-lookup"><span data-stu-id="c9eec-107">Browser Link setup</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c9eec-108">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c9eec-108">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="c9eec-109">Platformy ASP.NET Core 2.x **aplikacji sieci Web**, **pusty**, i **interfejsu API sieci Web** szablonów projektów użyj [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) meta pakiet, który zawiera odwołania do pakietu dla [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/).</span><span class="sxs-lookup"><span data-stu-id="c9eec-109">The ASP.NET Core 2.x **Web Application**, **Empty**, and **Web API** template projects use the [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) meta-package, which contains a package reference for [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/).</span></span> <span data-ttu-id="c9eec-110">W związku z tym przy użyciu `Microsoft.AspNetCore.All` meta pakiet wymaga żadnych dodatkowych czynności, aby udostępnić łącze przeglądarki do użycia.</span><span class="sxs-lookup"><span data-stu-id="c9eec-110">Therefore, using the `Microsoft.AspNetCore.All` meta-package requires no further action to make Browser Link available for use.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c9eec-111">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c9eec-111">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="c9eec-112">Platformy ASP.NET Core 1.x **aplikacji sieci Web** szablon projektu zawiera odwołania do pakietu dla [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) pakietu.</span><span class="sxs-lookup"><span data-stu-id="c9eec-112">The ASP.NET Core 1.x **Web Application** project template has a package reference for the [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) package.</span></span> <span data-ttu-id="c9eec-113">**Pusty** lub **interfejsu API sieci Web** szablonów projektów trzeba dodać odwołanie do pakietu `Microsoft.VisualStudio.Web.BrowserLink`.</span><span class="sxs-lookup"><span data-stu-id="c9eec-113">The **Empty** or **Web API** template projects require you to add a package reference to `Microsoft.VisualStudio.Web.BrowserLink`.</span></span>

<span data-ttu-id="c9eec-114">Ponieważ jest najprostszym sposobem funkcja programu Visual Studio, aby dodać pakiet do **pusty** lub **interfejsu API sieci Web** szablonu projektu, należy otworzyć **Konsola Menedżera pakietów** (**Widoku** > **inne okna** > **Konsola Menedżera pakietów**) i uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="c9eec-114">Since this is a Visual Studio feature, the easiest way to add the package to an **Empty** or **Web API** template project is to open the **Package Manager Console** (**View** > **Other Windows** > **Package Manager Console**) and run the following command:</span></span>

```console
install-package Microsoft.VisualStudio.Web.BrowserLink
```

<span data-ttu-id="c9eec-115">Alternatywnie można użyć **Menedżera pakietów NuGet**.</span><span class="sxs-lookup"><span data-stu-id="c9eec-115">Alternatively, you can use **NuGet Package Manager**.</span></span> <span data-ttu-id="c9eec-116">Kliknij prawym przyciskiem myszy nazwę projektu w **Eksploratora rozwiązań** i wybierz polecenie **Zarządzaj pakietami NuGet**:</span><span class="sxs-lookup"><span data-stu-id="c9eec-116">Right-click the project name in **Solution Explorer** and choose **Manage NuGet Packages**:</span></span>

![Menedżer pakietów NuGet Otwórz](using-browserlink/_static/open-nuget-package-manager.png)

<span data-ttu-id="c9eec-118">Znajdź i zainstaluj pakiet:</span><span class="sxs-lookup"><span data-stu-id="c9eec-118">Find and install the package:</span></span>

![Dodaj pakiet Menedżera pakietów NuGet](using-browserlink/_static/add-package-with-nuget-package-manager.png)

---

### <a name="configuration"></a><span data-ttu-id="c9eec-120">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="c9eec-120">Configuration</span></span>

<span data-ttu-id="c9eec-121">W `Configure` metody *Startup.cs* pliku:</span><span class="sxs-lookup"><span data-stu-id="c9eec-121">In the `Configure` method of the *Startup.cs* file:</span></span>

```csharp
app.UseBrowserLink();
```

<span data-ttu-id="c9eec-122">Zazwyczaj kod znajduje się wewnątrz `if` bloku umożliwiającą tylko łącze przeglądarki w środowisku programistycznym, jak pokazano poniżej:</span><span class="sxs-lookup"><span data-stu-id="c9eec-122">Usually the code is inside an `if` block that only enables Browser Link in the Development environment, as shown here:</span></span>

```csharp
if (env.IsDevelopment())
{
    app.UseDeveloperExceptionPage();
    app.UseBrowserLink();
}
```

<span data-ttu-id="c9eec-123">Aby uzyskać więcej informacji, zobacz [Praca w środowiskach wielu](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="c9eec-123">For more information, see [Working with Multiple Environments](xref:fundamentals/environments).</span></span>

## <a name="how-to-use-browser-link"></a><span data-ttu-id="c9eec-124">Jak używać łączy przeglądarki</span><span class="sxs-lookup"><span data-stu-id="c9eec-124">How to use Browser Link</span></span>

<span data-ttu-id="c9eec-125">Jeśli projekt platformy ASP.NET Core, Otwórz, Visual Studio zawiera formantu toolbar Browser Link obok pozycji **docelowego debugowania** formantu paska narzędzi:</span><span class="sxs-lookup"><span data-stu-id="c9eec-125">When you have an ASP.NET Core project open, Visual Studio shows the Browser Link toolbar control next to the **Debug Target** toolbar control:</span></span>

![Menu rozwijane łącza przeglądarki](using-browserlink/_static/browserLink-dropdown-menu.png)

<span data-ttu-id="c9eec-127">Z formantem paska narzędzi łącze przeglądarki możesz:</span><span class="sxs-lookup"><span data-stu-id="c9eec-127">From the Browser Link toolbar control, you can:</span></span>

* <span data-ttu-id="c9eec-128">Odświeżanie aplikacji sieci web w wielu przeglądarkach jednocześnie.</span><span class="sxs-lookup"><span data-stu-id="c9eec-128">Refresh the web application in several browsers at once.</span></span>
* <span data-ttu-id="c9eec-129">Otwórz **nawigacyjnym łącza przeglądarki**.</span><span class="sxs-lookup"><span data-stu-id="c9eec-129">Open the **Browser Link Dashboard**.</span></span>
* <span data-ttu-id="c9eec-130">Włącz lub wyłącz **łącze przeglądarki**.</span><span class="sxs-lookup"><span data-stu-id="c9eec-130">Enable or disable **Browser Link**.</span></span> <span data-ttu-id="c9eec-131">Uwaga: Łącze przeglądarki jest domyślnie wyłączona w programie Visual Studio 2017 (15 ustęp 3).</span><span class="sxs-lookup"><span data-stu-id="c9eec-131">Note: Browser Link is disabled by default in Visual Studio 2017 (15.3).</span></span>
* <span data-ttu-id="c9eec-132">Włącz lub wyłącz [automatyczna synchronizacja CSS](#enable-or-disable-css-auto-sync).</span><span class="sxs-lookup"><span data-stu-id="c9eec-132">Enable or disable [CSS Auto-Sync](#enable-or-disable-css-auto-sync).</span></span>

> [!NOTE]
> <span data-ttu-id="c9eec-133">Niektóre Visual Studio dodatków plug-in, głównie *2015 pakiet rozszerzenia sieci Web* i *2017 pakiet rozszerzenia sieci Web*, oferują rozszerzoną funkcjonalność łącza przeglądarki, ale niektóre dodatkowe funkcje nie działają w aplikacji ASP. Projekty sieci podstawowej.</span><span class="sxs-lookup"><span data-stu-id="c9eec-133">Some Visual Studio plug-ins, most notably *Web Extension Pack 2015* and *Web Extension Pack 2017*, offer extended functionality for Browser Link, but some of the additional features don't work with ASP.NET Core projects.</span></span>

## <a name="refresh-the-web-application-in-several-browsers-at-once"></a><span data-ttu-id="c9eec-134">Odświeżanie aplikacji sieci web w wielu przeglądarkach jednocześnie</span><span class="sxs-lookup"><span data-stu-id="c9eec-134">Refresh the web application in several browsers at once</span></span>

<span data-ttu-id="c9eec-135">Aby wybrać jeden przeglądarki do uruchomienia przy uruchamianiu projektu, użyj menu rozwijanego w **docelowego debugowania** formantu paska narzędzi:</span><span class="sxs-lookup"><span data-stu-id="c9eec-135">To choose a single web browser to launch when starting the project, use the drop-down menu in the **Debug Target** toolbar control:</span></span>

![Menu rozwijane F5](using-browserlink/_static/debug-target-dropdown-menu.png)

<span data-ttu-id="c9eec-137">Aby otworzyć jednocześnie wiele przeglądarek, wybierz **przeglądania przy użyciu...**  z tej samej listy rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="c9eec-137">To open multiple browsers at once, choose **Browse with...** from the same drop-down.</span></span> <span data-ttu-id="c9eec-138">Naciśnij i przytrzymaj klawisz CTRL, aby wybrać przeglądarki ma, a następnie kliknij przycisk **Przeglądaj**:</span><span class="sxs-lookup"><span data-stu-id="c9eec-138">Hold down the CTRL key to select the browsers you want, and then click **Browse**:</span></span>

![Otwórz jednocześnie wiele przeglądarek](using-browserlink/_static/open-many-browsers-at-once.png)

<span data-ttu-id="c9eec-140">Poniżej przedstawiono zrzut ekranu przedstawiający open Visual Studio z widokiem indeksu i otwórz przeglądarek:</span><span class="sxs-lookup"><span data-stu-id="c9eec-140">Here's a screenshot showing Visual Studio with the Index view open and two open browsers:</span></span>

![Synchronizacja przykład przeglądarek](using-browserlink/_static/sync-with-two-browsers-example.png)

<span data-ttu-id="c9eec-142">Umieść kursor nad formantem paska narzędzi łącze przeglądarki do Zobacz przeglądarki, które są podłączone do projektu:</span><span class="sxs-lookup"><span data-stu-id="c9eec-142">Hover over the Browser Link toolbar control to see the browsers that are connected to the project:</span></span>

![Umieść kursor Porada](using-browserlink/_static/hoover-tip.png)

<span data-ttu-id="c9eec-144">Zmień widok indeksu i wszystkich podłączonych przeglądarek są aktualizowane po kliknięciu przycisku Odśwież łącze przeglądarki:</span><span class="sxs-lookup"><span data-stu-id="c9eec-144">Change the Index view, and all connected browsers are updated when you click the Browser Link refresh button:</span></span>

![przeglądarki synchronizacji do zmiany](using-browserlink/_static/browsers-sync-to-changes.png)

<span data-ttu-id="c9eec-146">Łącze przeglądarki działa także w przypadku przeglądarek, które uruchamianie z zewnętrznego programu Visual Studio i przejdź do adresu URL aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c9eec-146">Browser Link also works with browsers that you launch from outside Visual Studio and navigate to the application URL.</span></span>

### <a name="the-browser-link-dashboard"></a><span data-ttu-id="c9eec-147">Na pulpicie nawigacyjnym łącza przeglądarki</span><span class="sxs-lookup"><span data-stu-id="c9eec-147">The Browser Link Dashboard</span></span>

<span data-ttu-id="c9eec-148">Otwórz pulpicie nawigacyjnym łącza przeglądarki z menu Zarządzanie połączenia z przeglądarkami Otwórz rozwijanego łącze przeglądarki:</span><span class="sxs-lookup"><span data-stu-id="c9eec-148">Open the Browser Link Dashboard from the Browser Link drop down menu to manage the connection with open browsers:</span></span>

![Open — browserslink-pulpit nawigacyjny](using-browserlink/_static/open-browserlink-dashboard.png)

<span data-ttu-id="c9eec-150">Jeśli przeglądarka nie jest połączona, można uruchomić sesji z systemem innym niż debugowanie, wybierając *Wyświetl w przeglądarce* łącza:</span><span class="sxs-lookup"><span data-stu-id="c9eec-150">If no browser is connected, you can start a non-debugging session by selecting the *View in Browser* link:</span></span>

![browserlink-dashboard-no-connections](using-browserlink/_static/browserlink-dashboard-no-connections.png)

<span data-ttu-id="c9eec-152">W przeciwnym razie podłączonych przeglądarek są wyświetlane ze ścieżką do strony, która pokazuje każdą przeglądarkę:</span><span class="sxs-lookup"><span data-stu-id="c9eec-152">Otherwise, the connected browsers are shown with the path to the page that each browser is showing:</span></span>

![browserlink-dashboard-two-connections](using-browserlink/_static/browserlink-dashboard-two-connections.png)

<span data-ttu-id="c9eec-154">Jeśli chcesz możesz kliknąć nazwę wymienionych przeglądarki, aby odświeżyć jednego przeglądarka.</span><span class="sxs-lookup"><span data-stu-id="c9eec-154">If you like, you can click on a listed browser name to refresh that single browser.</span></span>

### <a name="enable-or-disable-browser-link"></a><span data-ttu-id="c9eec-155">Włącz lub Wyłącz łącza przeglądarki</span><span class="sxs-lookup"><span data-stu-id="c9eec-155">Enable or disable Browser Link</span></span>

<span data-ttu-id="c9eec-156">Po ponownym włączeniu łącze przeglądarki po jej wyłączeniu, należy odświeżyć przeglądarki dla podłącz je ponownie.</span><span class="sxs-lookup"><span data-stu-id="c9eec-156">When you re-enable Browser Link after disabling it, you must refresh the browsers to reconnect them.</span></span>

### <a name="enable-or-disable-css-auto-sync"></a><span data-ttu-id="c9eec-157">Włączanie lub wyłączanie automatycznej synchronizacji CSS</span><span class="sxs-lookup"><span data-stu-id="c9eec-157">Enable or disable CSS Auto-Sync</span></span>

<span data-ttu-id="c9eec-158">Po włączeniu automatycznej synchronizacji CSS podłączonych przeglądarek są automatycznie odświeżane przy wprowadzaniu zmian do plików CSS.</span><span class="sxs-lookup"><span data-stu-id="c9eec-158">When CSS Auto-Sync is enabled, connected browsers are automatically refreshed when you make any change to CSS files.</span></span>

## <a name="how-does-it-work"></a><span data-ttu-id="c9eec-159">Jak to działa?</span><span class="sxs-lookup"><span data-stu-id="c9eec-159">How does it work?</span></span>

<span data-ttu-id="c9eec-160">Łącze przeglądarki używa SignalR do utworzenia kanału komunikacyjnego między Visual Studio i przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="c9eec-160">Browser Link uses SignalR to create a communication channel between Visual Studio and the browser.</span></span> <span data-ttu-id="c9eec-161">Po włączeniu łącze przeglądarki, programu Visual Studio działa jako wielu klientów (przeglądarki) mogą łączyć się z serwerem SignalR.</span><span class="sxs-lookup"><span data-stu-id="c9eec-161">When Browser Link is enabled, Visual Studio acts as a SignalR server that multiple clients (browsers) can connect to.</span></span> <span data-ttu-id="c9eec-162">Łącze przeglądarki rejestruje również składnik oprogramowania pośredniczącego w potoku żądania ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="c9eec-162">Browser Link also registers a middleware component in the ASP.NET request pipeline.</span></span> <span data-ttu-id="c9eec-163">Ten składnik injects specjalne `<script>` odwołań w każdym żądaniu strony z serwera.</span><span class="sxs-lookup"><span data-stu-id="c9eec-163">This component injects special `<script>` references into every page request from the server.</span></span> <span data-ttu-id="c9eec-164">Widać odwołań do skryptów, wybierając **Wyświetl źródło** w przeglądarce i przewijania na końcu `<body>` tagu zawartości:</span><span class="sxs-lookup"><span data-stu-id="c9eec-164">You can see the script references by selecting **View source** in the browser and scrolling to the end of the `<body>` tag content:</span></span>

```javascript
    <!-- Visual Studio Browser Link -->
    <script type="application/json" id="__browserLink_initializationData">
        {"requestId":"a717d5a07c1741949a7cefd6fa2bad08","requestMappingFromServer":false}
    </script>
    <script type="text/javascript" src="http://localhost:54139/b6e36e429d034f578ebccd6a79bf19bf/browserLink" async="async"></script>
    <!-- End Browser Link -->
</body>
```

<span data-ttu-id="c9eec-165">Plików źródłowych nie jest modyfikowany.</span><span class="sxs-lookup"><span data-stu-id="c9eec-165">Your source files aren't modified.</span></span> <span data-ttu-id="c9eec-166">Składnik oprogramowania pośredniczącego dynamicznie injects odwołań do skryptów.</span><span class="sxs-lookup"><span data-stu-id="c9eec-166">The middleware component injects the script references dynamically.</span></span> 

<span data-ttu-id="c9eec-167">Ponieważ kod po stronie przeglądarki jest kod JavaScript, działa na wszystkich przeglądarek obsługiwanych przez SignalR bez konieczności dodatek plug-in dla przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="c9eec-167">Because the browser-side code is all JavaScript, it works on all browsers that SignalR supports without requiring a browser plug-in.</span></span>
