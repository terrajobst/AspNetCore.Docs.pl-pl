---
uid: visual-studio/overview/2013/using-browser-link
title: "Przy użyciu Browser Link w programie Visual Studio 2013 | Dokumentacja firmy Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/04/2013
ms.topic: article
ms.assetid: 46cbfe20-b4dc-449b-9016-80657dd44fbe
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2013/using-browser-link
msc.type: authoredcontent
ms.openlocfilehash: e5a13405a303580ec8c1d4cdacafc26c6f8ff34a
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="using-browser-link-in-visual-studio-2013"></a><span data-ttu-id="864d6-102">Przy użyciu Browser Link w programie Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="864d6-102">Using Browser Link in Visual Studio 2013</span></span>
====================
<span data-ttu-id="864d6-103">przez [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="864d6-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="864d6-104">Łącze przeglądarki jest nową funkcją w programie Visual Studio 2013, tworzącym kanał komunikacji między Środowisko deweloperskie i co najmniej jeden przeglądarki sieci web.</span><span class="sxs-lookup"><span data-stu-id="864d6-104">Browser Link is a new feature in Visual Studio 2013 that creates a communication channel between the development environment and one or more web browsers.</span></span> <span data-ttu-id="864d6-105">Można użyć łącze przeglądarki, aby odświeżyć aplikacji sieci web w wielu przeglądarkach jednocześnie, która jest przydatna przy testowaniu różnych przeglądarkach.</span><span class="sxs-lookup"><span data-stu-id="864d6-105">You can use Browser Link to refresh your web application in several browsers at once, which is useful for cross-browser testing.</span></span>

- [<span data-ttu-id="864d6-106">Odśwież przeglądarkę</span><span class="sxs-lookup"><span data-stu-id="864d6-106">Browser Refresh</span></span>](#browser-refresh)
- [<span data-ttu-id="864d6-107">Wyświetlanie pulpicie nawigacyjnym łącza przeglądarki</span><span class="sxs-lookup"><span data-stu-id="864d6-107">Viewing the Browser Link Dashboard</span></span>](#dashboard)
- [<span data-ttu-id="864d6-108">Włączanie łącze przeglądarki plików Statycznych</span><span class="sxs-lookup"><span data-stu-id="864d6-108">Enabling Browser Link for Static HTML Files</span></span>](#static-html)
- [<span data-ttu-id="864d6-109">Wyłączanie łącza przeglądarki</span><span class="sxs-lookup"><span data-stu-id="864d6-109">Disabling Browser Link</span></span>](#disabling)
- [<span data-ttu-id="864d6-110">Jak to działa?</span><span class="sxs-lookup"><span data-stu-id="864d6-110">How Does It Work?</span></span>](#how-it-works)

<a id="browser-refresh"></a>
## <a name="browser-refresh"></a><span data-ttu-id="864d6-111">Odśwież przeglądarkę</span><span class="sxs-lookup"><span data-stu-id="864d6-111">Browser Refresh</span></span>

<span data-ttu-id="864d6-112">Z Odśwież przeglądarkę można odświeżyć wiele przeglądarek, które są połączone z programu Visual Studio za pośrednictwem łącza przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="864d6-112">With Browser Refresh, you can refresh multiple browsers that are connected to Visual Studio through Browser Link.</span></span>

<span data-ttu-id="864d6-113">Aby użyć Odśwież przeglądarkę, należy najpierw utworzyć aplikacji ASP.NET przy użyciu tych szablonów projektu.</span><span class="sxs-lookup"><span data-stu-id="864d6-113">To use Browser Refresh, first create an ASP.NET application, using any of the project templates.</span></span> <span data-ttu-id="864d6-114">Debugowanie aplikacji, naciskając klawisz F5 lub klikając ikonę strzałki na pasku narzędzi:</span><span class="sxs-lookup"><span data-stu-id="864d6-114">Debug the application by pressing F5 or clicking the arrow icon in the toolbar:</span></span>

![](using-browser-link/_static/image1.png)

<span data-ttu-id="864d6-115">Można też użyć listy rozwijanej do wybrania określonej przeglądarki do debugowania.</span><span class="sxs-lookup"><span data-stu-id="864d6-115">You can also use the dropdown to select a specific browser for debugging.</span></span>

![](using-browser-link/_static/image2.png)

<span data-ttu-id="864d6-116">Aby debugować z różnych przeglądarkach, wybierz **przeglądanie za pomocą**.</span><span class="sxs-lookup"><span data-stu-id="864d6-116">To debug with multiple browsers, select **Browse With**.</span></span> <span data-ttu-id="864d6-117">W **przeglądanie za pomocą** okna dialogowego, przytrzymaj klawisz CTRL, aby wybrać więcej niż jednej przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="864d6-117">In the **Browse With** dialog, hold down the CTRL key to select more than one browser.</span></span> <span data-ttu-id="864d6-118">Kliknij przycisk **Przeglądaj** do debugowania z wybranej przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="864d6-118">Click **Browse** to debug with the selected browsers.</span></span> <span data-ttu-id="864d6-119">Łącze przeglądarki również działa, jeśli przeglądarki z zewnętrznego programu Visual Studio i przejdź do adresu URL aplikacji.</span><span class="sxs-lookup"><span data-stu-id="864d6-119">Browser Link also works if you launch a browser from outside Visual Studio and navigate to the application URL.</span></span>

![](using-browser-link/_static/image3.png)

<span data-ttu-id="864d6-120">Formanty łączy przeglądarki znajdują się w ikoną cykliczne strzałkę listy rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="864d6-120">The Browser Link controls are located in the dropdown with the circular arrow icon.</span></span> <span data-ttu-id="864d6-121">Ikona strzałki jest **Odśwież** przycisku.</span><span class="sxs-lookup"><span data-stu-id="864d6-121">The arrow icon is the **Refresh** button.</span></span>

![](using-browser-link/_static/image4.png)

<span data-ttu-id="864d6-122">Aby zobaczyć, które przeglądarki są połączone, umieść kursor myszy nad **Odśwież** przycisk podczas debugowania.</span><span class="sxs-lookup"><span data-stu-id="864d6-122">To see which browsers are connected, hover the mouse over the **Refresh** button while debugging.</span></span> <span data-ttu-id="864d6-123">Podłączonych przeglądarek są wyświetlane w oknie etykietka narzędzia.</span><span class="sxs-lookup"><span data-stu-id="864d6-123">The connected browsers are shown in a ToolTip window.</span></span>

![](using-browser-link/_static/image5.png)

<span data-ttu-id="864d6-124">Aby odświeżyć podłączonych przeglądarek, kliknij przycisk **Odśwież** przycisk lub naciśnij klawisze CTRL + ALT + ENTER.</span><span class="sxs-lookup"><span data-stu-id="864d6-124">To refresh the connected browsers, click the **Refresh** button or press CTRL+ALT+ENTER.</span></span> <span data-ttu-id="864d6-125">Na przykład poniższy zrzut ekranu przedstawia projektu ASP.NET, który został utworzony za pomocą szablonu projektu MVC 5.</span><span class="sxs-lookup"><span data-stu-id="864d6-125">For example, the following screenshot shows an ASP.NET project, which I created using the MVC 5 project template.</span></span> <span data-ttu-id="864d6-126">Widać aplikacji działający w przeglądarkach dwóch u góry.</span><span class="sxs-lookup"><span data-stu-id="864d6-126">You can see the application running in two browsers at the top.</span></span> <span data-ttu-id="864d6-127">Na dole projekt jest otwarty w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="864d6-127">At the bottom, the project is open in Visual Studio.</span></span>

![](using-browser-link/_static/image6.png)

<span data-ttu-id="864d6-128">W programie Visual Studio po zmianie &lt;h1&gt; nagłówek strony głównej:</span><span class="sxs-lookup"><span data-stu-id="864d6-128">In Visual Studio, I changed the &lt;h1&gt; heading for the home page:</span></span>

![](using-browser-link/_static/image7.png)

<span data-ttu-id="864d6-129">Po kliknięciu **Odśwież** przycisk zmiany znajdowały się w oba okna przeglądarki:</span><span class="sxs-lookup"><span data-stu-id="864d6-129">When I clicked the **Refresh** button, the change appeared in both browser windows:</span></span>

![](using-browser-link/_static/image8.png)

<span data-ttu-id="864d6-130">**Uwagi**</span><span class="sxs-lookup"><span data-stu-id="864d6-130">**Notes**</span></span>

- <span data-ttu-id="864d6-131">Aby włączyć łącze przeglądarki, ustaw `debug=true` w [ &lt;kompilacji&gt; ](https://msdn.microsoft.com/library/s10awwz0(v=vs.85).aspx) elementu w pliku Web.config dla projektu.</span><span class="sxs-lookup"><span data-stu-id="864d6-131">To enable Browser Link, set `debug=true` in the [&lt;compilation&gt;](https://msdn.microsoft.com/library/s10awwz0(v=vs.85).aspx) element in the project's Web.config file.</span></span>
- <span data-ttu-id="864d6-132">Aplikacja musi być uruchomiona na hoście lokalnym.</span><span class="sxs-lookup"><span data-stu-id="864d6-132">The application must be running on localhost.</span></span>
- <span data-ttu-id="864d6-133">Aplikacja musi wskazywać .NET 4.0 lub nowszy.</span><span class="sxs-lookup"><span data-stu-id="864d6-133">The application must target .NET 4.0 or later.</span></span>

<a id="dashboard"></a>
## <a name="viewing-the-browser-link-dashboard"></a><span data-ttu-id="864d6-134">Wyświetlanie pulpicie nawigacyjnym łącza przeglądarki</span><span class="sxs-lookup"><span data-stu-id="864d6-134">Viewing the Browser Link Dashboard</span></span>

<span data-ttu-id="864d6-135">Na pulpicie nawigacyjnym łącza przeglądarki zawiera informacje dotyczące połączeń łącze przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="864d6-135">The Browser Link dashboard shows information about the Browser Link connections.</span></span> <span data-ttu-id="864d6-136">Aby wyświetlić pulpit nawigacyjny, wybierz menu rozwijane łącze przeglądarki (małą strzałkę obok **Odśwież** przycisku).</span><span class="sxs-lookup"><span data-stu-id="864d6-136">To view the dashboard, select the Browser Link dropdown menu (the small arrow next to the **Refresh** button).</span></span> <span data-ttu-id="864d6-137">Następnie kliknij przycisk **nawigacyjnym łącza przeglądarki**.</span><span class="sxs-lookup"><span data-stu-id="864d6-137">Then click **Browser Link Dashboard**.</span></span>

![](using-browser-link/_static/image9.png)

<span data-ttu-id="864d6-138">Pulpit nawigacyjny zawiera listę podłączonych przeglądarek i adres URL, do którego nawigację każdą przeglądarkę.</span><span class="sxs-lookup"><span data-stu-id="864d6-138">The dashboard lists the connected Browsers and the URL to which each browser has navigated.</span></span>

![](using-browser-link/_static/image10.png)

<span data-ttu-id="864d6-139">**Wymagania wstępne** sekcji przedstawiono wszystkie czynności, aby włączyć łącze przeglądarki dla tego projektu.</span><span class="sxs-lookup"><span data-stu-id="864d6-139">The **Prerequisites** section shows any steps needed to enable Browser Link for that project.</span></span> <span data-ttu-id="864d6-140">Na przykład poniższy zrzut ekranu przedstawia projekt gdzie "debug" ma wartość false w pliku Web.config.</span><span class="sxs-lookup"><span data-stu-id="864d6-140">For example, the following screenshot shows a project where "debug" is set to false in the Web.config file.</span></span>

![](using-browser-link/_static/image11.png)

<a id="static-html"></a>
## <a name="enabling-browser-link-for-static-html-files"></a><span data-ttu-id="864d6-141">Włączanie łącze przeglądarki plików Statycznych</span><span class="sxs-lookup"><span data-stu-id="864d6-141">Enabling Browser Link for Static HTML Files</span></span>

<span data-ttu-id="864d6-142">Aby włączyć łącze przeglądarki dla statyczne pliki HTML, należy dodać następujące do pliku Web.config.</span><span class="sxs-lookup"><span data-stu-id="864d6-142">To enable Browser Link for static HTML files, add the following to your Web.config file.</span></span>

[!code-xml[Main](using-browser-link/samples/sample1.xml)]

<span data-ttu-id="864d6-143">Ze względu na wydajność Usuń to ustawienie podczas publikowania projektu.</span><span class="sxs-lookup"><span data-stu-id="864d6-143">For performance reasons, remove this setting when you publish your project.</span></span>

<a id="disabling"></a>
## <a name="disabling-browser-link"></a><span data-ttu-id="864d6-144">Wyłączanie łącza przeglądarki</span><span class="sxs-lookup"><span data-stu-id="864d6-144">Disabling Browser Link</span></span>

<span data-ttu-id="864d6-145">Łącze przeglądarki jest domyślnie włączona.</span><span class="sxs-lookup"><span data-stu-id="864d6-145">Browser Link is enabled by default.</span></span> <span data-ttu-id="864d6-146">Istnieje kilka sposobów, aby ją wyłączyć:</span><span class="sxs-lookup"><span data-stu-id="864d6-146">There are several ways to disable it:</span></span>

- <span data-ttu-id="864d6-147">W menu rozwijanym łącze przeglądarki, usuń zaznaczenie pola wyboru **Włącz łącze przeglądarki**.</span><span class="sxs-lookup"><span data-stu-id="864d6-147">In the Browser Link dropdown menu, uncheck **Enable Browser Link**.</span></span> 

    ![](using-browser-link/_static/image12.png)
- <span data-ttu-id="864d6-148">W pliku Web.config Dodaj klucz o nazwie "vs: EnableBrowserLink" o wartości "false" w sekcji appSettings.</span><span class="sxs-lookup"><span data-stu-id="864d6-148">In the Web.config file, add a key named "vs:EnableBrowserLink" with the value "false" in the appSettings section.</span></span> 

    [!code-xml[Main](using-browser-link/samples/sample2.xml)]
- <span data-ttu-id="864d6-149">W pliku Web.config należy ustawić debugowania na wartość false.</span><span class="sxs-lookup"><span data-stu-id="864d6-149">In the Web.config file, set debug to false.</span></span> 

    [!code-xml[Main](using-browser-link/samples/sample3.xml)]

<a id="how-it-works"></a>
## <a name="how-does-it-work"></a><span data-ttu-id="864d6-150">Jak to działa?</span><span class="sxs-lookup"><span data-stu-id="864d6-150">How Does It Work?</span></span>

<span data-ttu-id="864d6-151">Łącze przeglądarki używa [SignalR](../../../signalr/index.md) próba utworzenia kanału komunikacji między Visual Studio i przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="864d6-151">Browser Link uses [SignalR](../../../signalr/index.md) to create a communication channel between Visual Studio and the browser.</span></span> <span data-ttu-id="864d6-152">Po włączeniu łącze przeglądarki, programu Visual Studio działa jako wielu klientów (przeglądarki) mogą łączyć się z serwerem SignalR.</span><span class="sxs-lookup"><span data-stu-id="864d6-152">When Browser Link is enabled, Visual Studio acts as a SignalR server that multiple clients (browsers) can connect to.</span></span> <span data-ttu-id="864d6-153">Łącze przeglądarki rejestruje również moduł protokołu HTTP z programem ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="864d6-153">Browser Link also registers an HTTP module with ASP.NET.</span></span> <span data-ttu-id="864d6-154">Ten moduł injects specjalne &lt;skryptu&gt; odwołań w każdym żądaniu strony z serwera.</span><span class="sxs-lookup"><span data-stu-id="864d6-154">This module injects special &lt;script&gt; references into every page request from the server.</span></span> <span data-ttu-id="864d6-155">Zobacz z odwołań do skryptów, wybierając polecenie "Wyświetl źródło" w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="864d6-155">You can see the script references by selecting "View source" in the browser.</span></span>

![](using-browser-link/_static/image13.png)

<span data-ttu-id="864d6-156">Plików źródłowych nie są modyfikowane.</span><span class="sxs-lookup"><span data-stu-id="864d6-156">Your source files are not modified.</span></span> <span data-ttu-id="864d6-157">Moduł HTTP dynamicznie injects odwołań do skryptów.</span><span class="sxs-lookup"><span data-stu-id="864d6-157">The HTTP module injects the script references dynamically.</span></span>

<span data-ttu-id="864d6-158">Ponieważ kod po stronie przeglądarki jest kod JavaScript, działa na wszystkie przeglądarki który [SignalR obsługuje](../../../signalr/overview/getting-started/supported-platforms.md), bez konieczności wtyczki przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="864d6-158">Because the browser-side code is all JavaScript, it works on all browsers that [SignalR supports](../../../signalr/overview/getting-started/supported-platforms.md), without requiring any browser plug-in.</span></span>
