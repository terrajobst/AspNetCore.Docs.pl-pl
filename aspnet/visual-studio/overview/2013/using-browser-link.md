---
uid: visual-studio/overview/2013/using-browser-link
title: W programie Visual Studio 2013 za pomocą łączność z przeglądarkami | Dokumentacja firmy Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 10/04/2013
ms.assetid: 46cbfe20-b4dc-449b-9016-80657dd44fbe
msc.legacyurl: /visual-studio/overview/2013/using-browser-link
msc.type: authoredcontent
ms.openlocfilehash: 9da93c279bfa2af614733e3234ba62429abf688a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37822198"
---
<a name="using-browser-link-in-visual-studio-2013"></a><span data-ttu-id="c36e4-102">Za pomocą łącza przeglądarki w programie Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="c36e4-102">Using Browser Link in Visual Studio 2013</span></span>
====================
<span data-ttu-id="c36e4-103">przez [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="c36e4-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="c36e4-104">Łącze przeglądarki jest nową funkcją w programie Visual Studio 2013, która tworzy kanał komunikacyjny między środowiska programistycznego i jeden lub więcej przeglądarek sieci web.</span><span class="sxs-lookup"><span data-stu-id="c36e4-104">Browser Link is a new feature in Visual Studio 2013 that creates a communication channel between the development environment and one or more web browsers.</span></span> <span data-ttu-id="c36e4-105">Można użyć łącze przeglądarki, aby odświeżyć aplikację sieci web w kilku przeglądarkach jednocześnie, która jest przydatna przy testowaniu obsługiwania wielu przeglądarek.</span><span class="sxs-lookup"><span data-stu-id="c36e4-105">You can use Browser Link to refresh your web application in several browsers at once, which is useful for cross-browser testing.</span></span>

- [<span data-ttu-id="c36e4-106">Odśwież w przeglądarce</span><span class="sxs-lookup"><span data-stu-id="c36e4-106">Browser Refresh</span></span>](#browser-refresh)
- [<span data-ttu-id="c36e4-107">Wyświetlanie na pulpicie nawigacyjnym łącza przeglądarki</span><span class="sxs-lookup"><span data-stu-id="c36e4-107">Viewing the Browser Link Dashboard</span></span>](#dashboard)
- [<span data-ttu-id="c36e4-108">Włączanie łączność z przeglądarkami plików Statycznych</span><span class="sxs-lookup"><span data-stu-id="c36e4-108">Enabling Browser Link for Static HTML Files</span></span>](#static-html)
- [<span data-ttu-id="c36e4-109">Wyłączanie łączność z przeglądarkami</span><span class="sxs-lookup"><span data-stu-id="c36e4-109">Disabling Browser Link</span></span>](#disabling)
- [<span data-ttu-id="c36e4-110">Jak to działa?</span><span class="sxs-lookup"><span data-stu-id="c36e4-110">How Does It Work?</span></span>](#how-it-works)

<a id="browser-refresh"></a>
## <a name="browser-refresh"></a><span data-ttu-id="c36e4-111">Odśwież w przeglądarce</span><span class="sxs-lookup"><span data-stu-id="c36e4-111">Browser Refresh</span></span>

<span data-ttu-id="c36e4-112">Za pomocą odświeżyć przeglądarkę można odświeżyć wielu przeglądarek, które są podłączone do programu Visual Studio za pośrednictwem łącza przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="c36e4-112">With Browser Refresh, you can refresh multiple browsers that are connected to Visual Studio through Browser Link.</span></span>

<span data-ttu-id="c36e4-113">Aby użyć, Odśwież przeglądarkę, należy najpierw utworzyć aplikację ASP.NET przy użyciu jednego z szablonów projektu.</span><span class="sxs-lookup"><span data-stu-id="c36e4-113">To use Browser Refresh, first create an ASP.NET application, using any of the project templates.</span></span> <span data-ttu-id="c36e4-114">Debugowanie aplikacji, naciskając klawisz F5 lub klikając ikonę strzałki na pasku narzędzi:</span><span class="sxs-lookup"><span data-stu-id="c36e4-114">Debug the application by pressing F5 or clicking the arrow icon in the toolbar:</span></span>

![](using-browser-link/_static/image1.png)

<span data-ttu-id="c36e4-115">Umożliwia także listy rozwijanej do wybierz określonych przeglądarki dla debugowania.</span><span class="sxs-lookup"><span data-stu-id="c36e4-115">You can also use the dropdown to select a specific browser for debugging.</span></span>

![](using-browser-link/_static/image2.png)

<span data-ttu-id="c36e4-116">Aby debugować za pomocą różnych przeglądarkach, wybierz **przeglądanie za pomocą**.</span><span class="sxs-lookup"><span data-stu-id="c36e4-116">To debug with multiple browsers, select **Browse With**.</span></span> <span data-ttu-id="c36e4-117">W **przeglądanie za pomocą** okno dialogowe, przytrzymaj wciśnięty klawisz CTRL, aby wybrać więcej niż jednej przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="c36e4-117">In the **Browse With** dialog, hold down the CTRL key to select more than one browser.</span></span> <span data-ttu-id="c36e4-118">Kliknij przycisk **Przeglądaj** debugowania przy użyciu wybranej przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="c36e4-118">Click **Browse** to debug with the selected browsers.</span></span> <span data-ttu-id="c36e4-119">Łączność z przeglądarkami współpracuje również jeśli przeglądarkę z poza programem Visual Studio i przejdź do adresu URL aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c36e4-119">Browser Link also works if you launch a browser from outside Visual Studio and navigate to the application URL.</span></span>

![](using-browser-link/_static/image3.png)

<span data-ttu-id="c36e4-120">Formanty łączność z przeglądarkami znajdują się na liście rozwijanej ikoną strzałkę kolistą.</span><span class="sxs-lookup"><span data-stu-id="c36e4-120">The Browser Link controls are located in the dropdown with the circular arrow icon.</span></span> <span data-ttu-id="c36e4-121">Ikona strzałki jest **Odśwież** przycisku.</span><span class="sxs-lookup"><span data-stu-id="c36e4-121">The arrow icon is the **Refresh** button.</span></span>

![](using-browser-link/_static/image4.png)

<span data-ttu-id="c36e4-122">Aby zobaczyć, jakie przeglądarki są połączone, umieść kursor myszy nad **Odśwież** przycisk podczas debugowania.</span><span class="sxs-lookup"><span data-stu-id="c36e4-122">To see which browsers are connected, hover the mouse over the **Refresh** button while debugging.</span></span> <span data-ttu-id="c36e4-123">Połączonych przeglądarek są wyświetlane w oknie etykietki narzędzia.</span><span class="sxs-lookup"><span data-stu-id="c36e4-123">The connected browsers are shown in a ToolTip window.</span></span>

![](using-browser-link/_static/image5.png)

<span data-ttu-id="c36e4-124">Aby odświeżyć połączonych przeglądarek, kliknij **Odśwież** przycisk lub naciśnij klawisze CTRL + ALT + ENTER.</span><span class="sxs-lookup"><span data-stu-id="c36e4-124">To refresh the connected browsers, click the **Refresh** button or press CTRL+ALT+ENTER.</span></span> <span data-ttu-id="c36e4-125">Na przykład poniższy zrzut ekranu przedstawia projektu ASP.NET, który został utworzony przy użyciu szablonu projektu MVC 5.</span><span class="sxs-lookup"><span data-stu-id="c36e4-125">For example, the following screenshot shows an ASP.NET project, which I created using the MVC 5 project template.</span></span> <span data-ttu-id="c36e4-126">Możesz zobaczyć aplikację uruchomioną w dwóch przeglądarek u góry.</span><span class="sxs-lookup"><span data-stu-id="c36e4-126">You can see the application running in two browsers at the top.</span></span> <span data-ttu-id="c36e4-127">Na dole projekt jest otwarty w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c36e4-127">At the bottom, the project is open in Visual Studio.</span></span>

![](using-browser-link/_static/image6.png)

<span data-ttu-id="c36e4-128">W programie Visual Studio po zmianie &lt;h1&gt; nagłówek strony głównej:</span><span class="sxs-lookup"><span data-stu-id="c36e4-128">In Visual Studio, I changed the &lt;h1&gt; heading for the home page:</span></span>

![](using-browser-link/_static/image7.png)

<span data-ttu-id="c36e4-129">Po kliknięciu **Odśwież** przycisk, zmiana pojawiła się oba okna przeglądarki:</span><span class="sxs-lookup"><span data-stu-id="c36e4-129">When I clicked the **Refresh** button, the change appeared in both browser windows:</span></span>

![](using-browser-link/_static/image8.png)

<span data-ttu-id="c36e4-130">**Uwagi**</span><span class="sxs-lookup"><span data-stu-id="c36e4-130">**Notes**</span></span>

- <span data-ttu-id="c36e4-131">Aby włączyć łączność z przeglądarkami, ustaw `debug=true` w [ &lt;kompilacji&gt; ](https://msdn.microsoft.com/library/s10awwz0(v=vs.85).aspx) elementu w pliku Web.config projektu.</span><span class="sxs-lookup"><span data-stu-id="c36e4-131">To enable Browser Link, set `debug=true` in the [&lt;compilation&gt;](https://msdn.microsoft.com/library/s10awwz0(v=vs.85).aspx) element in the project's Web.config file.</span></span>
- <span data-ttu-id="c36e4-132">Aplikacja musi działać na hoście lokalnym.</span><span class="sxs-lookup"><span data-stu-id="c36e4-132">The application must be running on localhost.</span></span>
- <span data-ttu-id="c36e4-133">Aplikacja musi być przeznaczony dla platformy .NET 4.0 lub nowszy.</span><span class="sxs-lookup"><span data-stu-id="c36e4-133">The application must target .NET 4.0 or later.</span></span>

<a id="dashboard"></a>
## <a name="viewing-the-browser-link-dashboard"></a><span data-ttu-id="c36e4-134">Wyświetlanie na pulpicie nawigacyjnym łącza przeglądarki</span><span class="sxs-lookup"><span data-stu-id="c36e4-134">Viewing the Browser Link Dashboard</span></span>

<span data-ttu-id="c36e4-135">Pulpit nawigacyjny Browser Link zawiera informacje o połączeniach łączność z przeglądarkami.</span><span class="sxs-lookup"><span data-stu-id="c36e4-135">The Browser Link dashboard shows information about the Browser Link connections.</span></span> <span data-ttu-id="c36e4-136">Aby wyświetlić pulpit nawigacyjny, wybierz menu rozwijane łączność z przeglądarkami (małą strzałkę obok pozycji **Odśwież** przycisk).</span><span class="sxs-lookup"><span data-stu-id="c36e4-136">To view the dashboard, select the Browser Link dropdown menu (the small arrow next to the **Refresh** button).</span></span> <span data-ttu-id="c36e4-137">Następnie kliknij przycisk **nawigacyjnym łącza przeglądarki**.</span><span class="sxs-lookup"><span data-stu-id="c36e4-137">Then click **Browser Link Dashboard**.</span></span>

![](using-browser-link/_static/image9.png)

<span data-ttu-id="c36e4-138">Pulpit nawigacyjny zawiera listę połączonych przeglądarek i adres URL, do którego nawigację każdą przeglądarkę.</span><span class="sxs-lookup"><span data-stu-id="c36e4-138">The dashboard lists the connected Browsers and the URL to which each browser has navigated.</span></span>

![](using-browser-link/_static/image10.png)

<span data-ttu-id="c36e4-139">**Wymagania wstępne** sekcja pokazuje wszystkie kroki wymagane do włączenia łączność z przeglądarkami dla tego projektu.</span><span class="sxs-lookup"><span data-stu-id="c36e4-139">The **Prerequisites** section shows any steps needed to enable Browser Link for that project.</span></span> <span data-ttu-id="c36e4-140">Na przykład poniższy zrzut ekranu przedstawia projekt gdzie "debug" jest ustawiona na wartość false w pliku Web.config.</span><span class="sxs-lookup"><span data-stu-id="c36e4-140">For example, the following screenshot shows a project where "debug" is set to false in the Web.config file.</span></span>

![](using-browser-link/_static/image11.png)

<a id="static-html"></a>
## <a name="enabling-browser-link-for-static-html-files"></a><span data-ttu-id="c36e4-141">Włączanie łączność z przeglądarkami plików Statycznych</span><span class="sxs-lookup"><span data-stu-id="c36e4-141">Enabling Browser Link for Static HTML Files</span></span>

<span data-ttu-id="c36e4-142">Aby włączyć łączność z przeglądarkami na statyczne pliki HTML, należy dodać następujące do pliku Web.config.</span><span class="sxs-lookup"><span data-stu-id="c36e4-142">To enable Browser Link for static HTML files, add the following to your Web.config file.</span></span>

[!code-xml[Main](using-browser-link/samples/sample1.xml)]

<span data-ttu-id="c36e4-143">Ze względu na wydajność należy usunąć ustawienie podczas publikowania projektu.</span><span class="sxs-lookup"><span data-stu-id="c36e4-143">For performance reasons, remove this setting when you publish your project.</span></span>

<a id="disabling"></a>
## <a name="disabling-browser-link"></a><span data-ttu-id="c36e4-144">Wyłączanie łączność z przeglądarkami</span><span class="sxs-lookup"><span data-stu-id="c36e4-144">Disabling Browser Link</span></span>

<span data-ttu-id="c36e4-145">Łącze przeglądarki jest domyślnie włączona.</span><span class="sxs-lookup"><span data-stu-id="c36e4-145">Browser Link is enabled by default.</span></span> <span data-ttu-id="c36e4-146">Istnieje kilka sposobów, aby ją wyłączyć:</span><span class="sxs-lookup"><span data-stu-id="c36e4-146">There are several ways to disable it:</span></span>

- <span data-ttu-id="c36e4-147">W menu rozwijanym łączność z przeglądarkami, usuń zaznaczenie pola wyboru **Włącz łącze przeglądarki**.</span><span class="sxs-lookup"><span data-stu-id="c36e4-147">In the Browser Link dropdown menu, uncheck **Enable Browser Link**.</span></span> 

    ![](using-browser-link/_static/image12.png)
- <span data-ttu-id="c36e4-148">W pliku Web.config Dodaj klucz o nazwie "vs: EnableBrowserLink" na wartość "false" w sekcji appSettings.</span><span class="sxs-lookup"><span data-stu-id="c36e4-148">In the Web.config file, add a key named "vs:EnableBrowserLink" with the value "false" in the appSettings section.</span></span> 

    [!code-xml[Main](using-browser-link/samples/sample2.xml)]
- <span data-ttu-id="c36e4-149">W pliku Web.config należy ustawić debugowania na wartość false.</span><span class="sxs-lookup"><span data-stu-id="c36e4-149">In the Web.config file, set debug to false.</span></span> 

    [!code-xml[Main](using-browser-link/samples/sample3.xml)]

<a id="how-it-works"></a>
## <a name="how-does-it-work"></a><span data-ttu-id="c36e4-150">Jak to działa?</span><span class="sxs-lookup"><span data-stu-id="c36e4-150">How Does It Work?</span></span>

<span data-ttu-id="c36e4-151">Łączność z przeglądarkami używa [SignalR](../../../signalr/index.md) próba utworzenia kanału komunikacji między Visual Studio i przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="c36e4-151">Browser Link uses [SignalR](../../../signalr/index.md) to create a communication channel between Visual Studio and the browser.</span></span> <span data-ttu-id="c36e4-152">Po włączeniu łączność z przeglądarkami programu Visual Studio działa jako serwer biblioteki SignalR, wielu klientów (przeglądarki) można łączyć się.</span><span class="sxs-lookup"><span data-stu-id="c36e4-152">When Browser Link is enabled, Visual Studio acts as a SignalR server that multiple clients (browsers) can connect to.</span></span> <span data-ttu-id="c36e4-153">Łączność z przeglądarkami rejestruje także moduł HTTP za pomocą platformy ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="c36e4-153">Browser Link also registers an HTTP module with ASP.NET.</span></span> <span data-ttu-id="c36e4-154">Ten moduł wprowadza specjalne &lt;skryptu&gt; odwołań w każdym żądaniu strony z serwera.</span><span class="sxs-lookup"><span data-stu-id="c36e4-154">This module injects special &lt;script&gt; references into every page request from the server.</span></span> <span data-ttu-id="c36e4-155">Odwołania do skryptu można wyświetlić, wybierając pozycję "Wyświetl źródło" w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="c36e4-155">You can see the script references by selecting "View source" in the browser.</span></span>

![](using-browser-link/_static/image13.png)

<span data-ttu-id="c36e4-156">Plików źródłowych nie są modyfikowane.</span><span class="sxs-lookup"><span data-stu-id="c36e4-156">Your source files are not modified.</span></span> <span data-ttu-id="c36e4-157">Moduł HTTP wprowadza odwołania do skryptu dynamicznie.</span><span class="sxs-lookup"><span data-stu-id="c36e4-157">The HTTP module injects the script references dynamically.</span></span>

<span data-ttu-id="c36e4-158">Ponieważ kod po stronie przeglądarki jest kod JavaScript, działa we wszystkich przeglądarkach, [obsługuje SignalR](../../../signalr/overview/getting-started/supported-platforms.md), bez konieczności wtyczkę przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="c36e4-158">Because the browser-side code is all JavaScript, it works on all browsers that [SignalR supports](../../../signalr/overview/getting-started/supported-platforms.md), without requiring any browser plug-in.</span></span>
