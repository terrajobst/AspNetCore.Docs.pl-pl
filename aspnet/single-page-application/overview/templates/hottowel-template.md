---
uid: single-page-application/overview/templates/hottowel-template
title: Hot Towel szablonu | Dokumentacja firmy Microsoft
author: madskristensen
description: Szablon HotTowel
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/09/2013
ms.topic: article
ms.assetid: 75af2e17-6ed3-4d24-8ea1-bc340027c318
ms.technology: ''
msc.legacyurl: /single-page-application/overview/templates/hottowel-template
msc.type: authoredcontent
ms.openlocfilehash: de81f12f57d7f2fb7c6478bfa1f3a278ae905a39
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37388552"
---
<a name="hot-towel-template"></a><span data-ttu-id="e4758-103">Szablon hot Towel</span><span class="sxs-lookup"><span data-stu-id="e4758-103">Hot Towel template</span></span>
====================
<span data-ttu-id="e4758-104">przez [: Mads Kristensen](https://github.com/madskristensen)</span><span class="sxs-lookup"><span data-stu-id="e4758-104">by [Mads Kristensen](https://github.com/madskristensen)</span></span>

> <span data-ttu-id="e4758-105">Szablon MVC Hot Towel została zapisana przy John Papa</span><span class="sxs-lookup"><span data-stu-id="e4758-105">The Hot Towel MVC Template is written by John Papa</span></span>
> 
> <span data-ttu-id="e4758-106">Wybierz wersję, która do pobrania:</span><span class="sxs-lookup"><span data-stu-id="e4758-106">Choose which version to download:</span></span>
> 
> [<span data-ttu-id="e4758-107">Szablon MVC hot Towel dla programu Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="e4758-107">Hot Towel MVC Template for Visual Studio 2012</span></span>](https://visualstudiogallery.msdn.microsoft.com/1f68fbe8-b4e9-4968-9fd3-ddc7cbc52dca)
> 
> [<span data-ttu-id="e4758-108">Szablon MVC hot Towel dla programu Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="e4758-108">Hot Towel MVC Template for Visual Studio 2013</span></span>](https://visualstudiogallery.msdn.microsoft.com/1eb8780d-d522-4dcf-bf56-56f0eab305c2)
> 
> 
> <span data-ttu-id="e4758-109">Hot Towel: Ponieważ nie chcesz przejść do SPA bez ani jednego!</span><span class="sxs-lookup"><span data-stu-id="e4758-109">Hot Towel: Because you don't want to go to the SPA without one!</span></span>


<span data-ttu-id="e4758-110">Chcesz tworzyć SPA, ale nie można zdecydować, gdzie zacząć?</span><span class="sxs-lookup"><span data-stu-id="e4758-110">Want to build a SPA but can't decide where to start?</span></span> <span data-ttu-id="e4758-111">Użyj Hot Towel i w ciągu kilku sekund będziesz mieć SPA i wszystkie narzędzia potrzebne do tworzenia na nim!</span><span class="sxs-lookup"><span data-stu-id="e4758-111">Use Hot Towel and in seconds you'll have a SPA and all the tools you need to build on it!</span></span>

<span data-ttu-id="e4758-112">Hot Towel tworzy doskonałe punkt początkowy do tworzenia pojedynczej strony aplikacji (SPA) za pomocą platformy ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="e4758-112">Hot Towel creates a great starting point for building a Single Page Application (SPA) with ASP.NET.</span></span> <span data-ttu-id="e4758-113">Gotowych użytkownik udostępnia modularna struktura do kodu, nawigacji w widoku, powiązanie danych, zarządzania zaawansowanych danych i style proste, ale elegancki.</span><span class="sxs-lookup"><span data-stu-id="e4758-113">Out of the box you it provides a modular structure for your code, view navigation, data binding, rich data management and simple but elegant styling.</span></span> <span data-ttu-id="e4758-114">Hot Towel zawiera wszystko, czego potrzebujesz do tworzenia SPA, dzięki czemu możesz skupić się na aplikacji, a nie podstawami.</span><span class="sxs-lookup"><span data-stu-id="e4758-114">Hot Towel provides everything you need to build a SPA, so you can focus on your app, not the plumbing.</span></span>

> <span data-ttu-id="e4758-115">Dowiedz się więcej o tworzeniu SPA z [filmów wideo i samouczki John Papa, a także kursów Pluralsight](http://johnpapa.net/spa?vsix).</span><span class="sxs-lookup"><span data-stu-id="e4758-115">Learn more about building a SPA from [John Papa's videos, tutorials and Pluralsight courses](http://johnpapa.net/spa?vsix).</span></span>


## <a name="application-structure"></a><span data-ttu-id="e4758-116">Struktury aplikacji</span><span class="sxs-lookup"><span data-stu-id="e4758-116">Application Structure</span></span>

<span data-ttu-id="e4758-117">Hot Towel SPA zawiera folder aplikacji, który zawiera pliki JavaScript i HTML, które definiują aplikację.</span><span class="sxs-lookup"><span data-stu-id="e4758-117">Hot Towel SPA provides an App folder which contains the JavaScript and HTML files that define your application.</span></span>

<span data-ttu-id="e4758-118">Znajdujące się w folderze aplikacji:</span><span class="sxs-lookup"><span data-stu-id="e4758-118">Inside the App folder:</span></span>

- <span data-ttu-id="e4758-119">Durandal</span><span class="sxs-lookup"><span data-stu-id="e4758-119">durandal</span></span>
- <span data-ttu-id="e4758-120">usługi</span><span class="sxs-lookup"><span data-stu-id="e4758-120">services</span></span>
- <span data-ttu-id="e4758-121">modele widoków</span><span class="sxs-lookup"><span data-stu-id="e4758-121">viewmodels</span></span>
- <span data-ttu-id="e4758-122">widoki</span><span class="sxs-lookup"><span data-stu-id="e4758-122">views</span></span>

<span data-ttu-id="e4758-123">Folder aplikacji zawiera zbiór modułów.</span><span class="sxs-lookup"><span data-stu-id="e4758-123">The App folder contains a collection of modules.</span></span> <span data-ttu-id="e4758-124">Te moduły zapewniają funkcjonalność hermetyzacji i zadeklarować zależności w innych modułach.</span><span class="sxs-lookup"><span data-stu-id="e4758-124">These modules encapsulate functionality and declare dependencies on other modules.</span></span> <span data-ttu-id="e4758-125">Folder widoków zawiera kod HTML dla aplikacji i modele widoków folder zawiera logikę prezentacji widoki (wspólny wzorzec MVVM).</span><span class="sxs-lookup"><span data-stu-id="e4758-125">The views folder contains the HTML for your application and the viewmodels folder contains the presentation logic for the views (a common MVVM pattern).</span></span> <span data-ttu-id="e4758-126">Folder usługi jest idealny dla obudowie wspólne usługi, których aplikacja może być konieczne takie jak pobieranie danych protokołu HTTP lub interakcji z magazynu lokalnego.</span><span class="sxs-lookup"><span data-stu-id="e4758-126">The services folder is ideal for housing any common services that your application may need such as HTTP data retrieval or local storage interaction.</span></span> <span data-ttu-id="e4758-127">Bardzo często modele wielu widoków do ponownego użycia kodu z modułów usługi.</span><span class="sxs-lookup"><span data-stu-id="e4758-127">It is common for multiple viewmodels to re-use code from the service modules.</span></span>

## <a name="aspnet-mvc-server-side-application-structure"></a><span data-ttu-id="e4758-128">Struktury aplikacji po stronie serwera platformy ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="e4758-128">ASP.NET MVC Server Side Application Structure</span></span>

<span data-ttu-id="e4758-129">Hot Towel opiera się na znanych i wydajnych struktury ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="e4758-129">Hot Towel builds on the familiar and powerful ASP.NET MVC structure.</span></span>

- <span data-ttu-id="e4758-130">Aplikacja\_Start</span><span class="sxs-lookup"><span data-stu-id="e4758-130">App\_Start</span></span>
- <span data-ttu-id="e4758-131">Zawartość</span><span class="sxs-lookup"><span data-stu-id="e4758-131">Content</span></span>
- <span data-ttu-id="e4758-132">Kontrolery</span><span class="sxs-lookup"><span data-stu-id="e4758-132">Controllers</span></span>
- <span data-ttu-id="e4758-133">Modele</span><span class="sxs-lookup"><span data-stu-id="e4758-133">Models</span></span>
- <span data-ttu-id="e4758-134">Skrypty</span><span class="sxs-lookup"><span data-stu-id="e4758-134">Scripts</span></span>
- <span data-ttu-id="e4758-135">Widoki</span><span class="sxs-lookup"><span data-stu-id="e4758-135">Views</span></span>

## <a name="featured-libraries"></a><span data-ttu-id="e4758-136">Polecane bibliotek</span><span class="sxs-lookup"><span data-stu-id="e4758-136">Featured Libraries</span></span>

- <span data-ttu-id="e4758-137">ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="e4758-137">ASP.NET MVC</span></span>
- <span data-ttu-id="e4758-138">ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="e4758-138">ASP.NET Web API</span></span>
- <span data-ttu-id="e4758-139">Optymalizacja sieci Web ASP.NET - tworzenie pakietów i minimalizowanie</span><span class="sxs-lookup"><span data-stu-id="e4758-139">ASP.NET Web Optimization - bundling and minification</span></span>
- <span data-ttu-id="e4758-140">[BREEZE.js](http://Breezejs.com) — zarządzanie danymi zaawansowane</span><span class="sxs-lookup"><span data-stu-id="e4758-140">[Breeze.js](http://Breezejs.com) - rich data management</span></span>
- <span data-ttu-id="e4758-141">[Durandal.js](http://Durandaljs.com) -nawigacji i widoku kompozycji</span><span class="sxs-lookup"><span data-stu-id="e4758-141">[Durandal.js](http://Durandaljs.com) - navigation and View composition</span></span>
- <span data-ttu-id="e4758-142">[Struktura Knockout.js](http://Knockoutjs.com) -powiązania danych</span><span class="sxs-lookup"><span data-stu-id="e4758-142">[Knockout.js](http://Knockoutjs.com) - data bindings</span></span>
- <span data-ttu-id="e4758-143">[Require.js](http://requirejs.org) -Modułowość AMD i optymalizacja</span><span class="sxs-lookup"><span data-stu-id="e4758-143">[Require.js](http://requirejs.org) - Modularity with AMD and optimization</span></span>
- <span data-ttu-id="e4758-144">[Toastr.js](http://jpapa.me/c7toastr) -komunikatów podręcznych</span><span class="sxs-lookup"><span data-stu-id="e4758-144">[Toastr.js](http://jpapa.me/c7toastr) - pop-up messages</span></span>
- <span data-ttu-id="e4758-145">[Twitter Bootstrap](http://twitter.github.com/bootstrap/) — niezawodne CSS style</span><span class="sxs-lookup"><span data-stu-id="e4758-145">[Twitter Bootstrap](http://twitter.github.com/bootstrap/) - robust CSS styling</span></span>

## <a name="installing-via-the-visual-studio-2012-hot-towel-spa-template"></a><span data-ttu-id="e4758-146">Instalowanie za pośrednictwem SPA szablon Hot Towel programu Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="e4758-146">Installing via the Visual Studio 2012 Hot Towel SPA Template</span></span>

<span data-ttu-id="e4758-147">Hot Towel można zainstalować jako szablon programu Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="e4758-147">Hot Towel can be installed as a Visual Studio 2012 template.</span></span> <span data-ttu-id="e4758-148">Po prostu kliknij `File`  |  `New Project` i wybierz polecenie `ASP.NET MVC 4 Web Application`.</span><span class="sxs-lookup"><span data-stu-id="e4758-148">Just click `File` | `New Project` and choose `ASP.NET MVC 4 Web Application`.</span></span> <span data-ttu-id="e4758-149">Następnie wybierz pozycję "Hot Towel aplikacji jednostronicowej" szablon i uruchom!</span><span class="sxs-lookup"><span data-stu-id="e4758-149">Then select the 'Hot Towel Single Page Application" template and run!</span></span>

## <a name="installing-via-the-nuget-package"></a><span data-ttu-id="e4758-150">Instalowanie za pośrednictwem pakietu NuGet</span><span class="sxs-lookup"><span data-stu-id="e4758-150">Installing via the NuGet Package</span></span>

<span data-ttu-id="e4758-151">Hot Towel jest również pakiet NuGet, który rozszerza istniejący pusty projekt platformy ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="e4758-151">Hot Towel is also a NuGet package that augments an existing empty ASP.NET MVC project.</span></span> <span data-ttu-id="e4758-152">Po prostu zainstaluj za pomocą narzędzia Nuget, a następnie uruchom!</span><span class="sxs-lookup"><span data-stu-id="e4758-152">Just install using Nuget and then run!</span></span>

[!code-powershell[Main](hottowel-template/samples/sample1.ps1)]

## <a name="how-do-i-build-on-hot-towel"></a><span data-ttu-id="e4758-153">Jak tworzyć na Hot Towel</span><span class="sxs-lookup"><span data-stu-id="e4758-153">How Do I Build On Hot Towel?</span></span>

<span data-ttu-id="e4758-154">Po prostu Rozpocznij, dodając kod!</span><span class="sxs-lookup"><span data-stu-id="e4758-154">Simply start adding code!</span></span>

1. <span data-ttu-id="e4758-155">Dodaj własny kod po stronie serwera, najlepiej Entity Framework i WebAPI (które tak naprawdę przykuć uwagę przy użyciu Breeze.js)</span><span class="sxs-lookup"><span data-stu-id="e4758-155">Add your own server-side code, preferably Entity Framework and WebAPI (which really shine with Breeze.js)</span></span>
2. <span data-ttu-id="e4758-156">Dodaj widoki `App/views` folderu</span><span class="sxs-lookup"><span data-stu-id="e4758-156">Add views to the `App/views` folder</span></span>
3. <span data-ttu-id="e4758-157">Modele widoków, aby dodać `App/viewmodels` folderu</span><span class="sxs-lookup"><span data-stu-id="e4758-157">Add viewmodels to the `App/viewmodels` folder</span></span>
4. <span data-ttu-id="e4758-158">Dodawanie powiązania danych HTML i Knockout do nowych widoków</span><span class="sxs-lookup"><span data-stu-id="e4758-158">Add HTML and Knockout data bindings to your new views</span></span>
5. <span data-ttu-id="e4758-159">Aktualizowanie tras nawigacji w `shell.js`</span><span class="sxs-lookup"><span data-stu-id="e4758-159">Update the navigation routes in `shell.js`</span></span>

## <a name="walkthrough-of-the-htmljavascript"></a><span data-ttu-id="e4758-160">Przewodnik po języku HTML/JavaScript</span><span class="sxs-lookup"><span data-stu-id="e4758-160">Walkthrough of the HTML/JavaScript</span></span>

### <a name="viewshottowelindexcshtml"></a><span data-ttu-id="e4758-161">Views/HotTowel/index.cshtml</span><span class="sxs-lookup"><span data-stu-id="e4758-161">Views/HotTowel/index.cshtml</span></span>

<span data-ttu-id="e4758-162">index.cshtml jest początkowy trasy i widok dla aplikacji MVC.</span><span class="sxs-lookup"><span data-stu-id="e4758-162">index.cshtml is the starting route and view for the MVC application.</span></span> <span data-ttu-id="e4758-163">Zawiera on wszystkich standardowych tagów meta, łączy css i JavaScript odwołań, czego można oczekiwać.</span><span class="sxs-lookup"><span data-stu-id="e4758-163">It contains all the standard meta tags, css links, and JavaScript references you would expect.</span></span> <span data-ttu-id="e4758-164">Treść zawiera pojedynczy `<div>` czyli, gdzie całej zawartości (widoki) zostaną umieszczone w przypadku żądania.</span><span class="sxs-lookup"><span data-stu-id="e4758-164">The body contains a single `<div>` which is where all of the content (your views) will be placed when they are requested.</span></span> <span data-ttu-id="e4758-165">`@Scripts.Render` Używa Require.js do uruchomienia punktu wejścia dla kodu aplikacji, która jest zawarta w `main.js` pliku.</span><span class="sxs-lookup"><span data-stu-id="e4758-165">The `@Scripts.Render` uses Require.js to run the entrance point for the application's code, which is contained in the `main.js` file.</span></span> <span data-ttu-id="e4758-166">Ekran powitalny jest udostępniane na pokazują, jak utworzyć ekran powitalny, podczas ładowania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e4758-166">A splash screen is provided to demonstrate how to create a splash screen while your app loads.</span></span>

[!code-cshtml[Main](hottowel-template/samples/sample2.cshtml)]

### <a name="appmainjs"></a><span data-ttu-id="e4758-167">App/main.js</span><span class="sxs-lookup"><span data-stu-id="e4758-167">App/main.js</span></span>

<span data-ttu-id="e4758-168">`main.js` Plik zawiera kod, który będzie uruchamiany natychmiast po załadowaniu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e4758-168">The `main.js` file contains the code that will run as soon as your app is loaded.</span></span> <span data-ttu-id="e4758-169">Jest to, które chcesz zdefiniować trasy nawigacji, Ustaw użytkownika uruchamiania widoków i wykonać wszelkie ustawienia/uruchamianie takich jak zalewanie danych aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e4758-169">This is where you want to define your navigation routes, set your start up views, and perform any setup/bootstrapping such as priming your application's data.</span></span>

<span data-ttu-id="e4758-170">`main.js` Plik definiuje kilka modułów durandal firmy, aby ułatwić rozpoczęcie aplikacji start.</span><span class="sxs-lookup"><span data-stu-id="e4758-170">The `main.js` file defines several of durandal's modules to help the application kick start.</span></span> <span data-ttu-id="e4758-171">Zdefiniuj instrukcję pomaga rozpoznać zależności modułów, aby były dostępne dla tej funkcji.</span><span class="sxs-lookup"><span data-stu-id="e4758-171">The define statement helps resolve the modules dependencies so they are available for the function.</span></span> <span data-ttu-id="e4758-172">Najpierw komunikaty debugowania są włączone, który wysyłać wiadomości o jakie zdarzenia, że aplikacja działa w oknie konsoli.</span><span class="sxs-lookup"><span data-stu-id="e4758-172">First the debugging messages are enabled, which send messages about what events the application is performing to the console window.</span></span> <span data-ttu-id="e4758-173">Kod app.start informuje durandal framework, aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="e4758-173">The app.start code tells durandal framework to start the application.</span></span> <span data-ttu-id="e4758-174">Konwencje są ustawione tak, aby durandal zna wszystkie widoki i modele widoków są zawarte w tych samych folderach nazwanych, odpowiednio.</span><span class="sxs-lookup"><span data-stu-id="e4758-174">The conventions are set so that durandal knows all views and viewmodels are contained in the same named folders, respectively.</span></span> <span data-ttu-id="e4758-175">Na koniec `app.setRoot` uruchamiane obciążenia `shell` przy użyciu wstępnie zdefiniowanej `entrance` animacji.</span><span class="sxs-lookup"><span data-stu-id="e4758-175">Finally, the `app.setRoot` kicks loads the `shell` using a predefined `entrance` animation.</span></span>

[!code-javascript[Main](hottowel-template/samples/sample3.js)]

## <a name="views"></a><span data-ttu-id="e4758-176">Widoki</span><span class="sxs-lookup"><span data-stu-id="e4758-176">Views</span></span>

<span data-ttu-id="e4758-177">Widoki znajdują się w `App/views` folderu.</span><span class="sxs-lookup"><span data-stu-id="e4758-177">Views are found in the `App/views` folder.</span></span>

### <a name="shellhtml"></a><span data-ttu-id="e4758-178">shell.html</span><span class="sxs-lookup"><span data-stu-id="e4758-178">shell.html</span></span>

<span data-ttu-id="e4758-179">`shell.html` Zawiera układem głównym na kodzie HTML.</span><span class="sxs-lookup"><span data-stu-id="e4758-179">The `shell.html` contains the master layout for your HTML.</span></span> <span data-ttu-id="e4758-180">Wszystkie inne widoków będzie składać się gdzieś po stronie usługi `shell` widoku.</span><span class="sxs-lookup"><span data-stu-id="e4758-180">All of your other views will be composed somewhere in side of your `shell` view.</span></span> <span data-ttu-id="e4758-181">Udostępnia hot Towel `shell` z trzech takich regionów: nagłówek, obszarem zawartości i stopkę.</span><span class="sxs-lookup"><span data-stu-id="e4758-181">Hot Towel provides a `shell` with three such regions: a header, a content area, and a footer.</span></span> <span data-ttu-id="e4758-182">Każda z tych regionów jest załadowana zawartość formularza inne widoki, gdy wymagane.</span><span class="sxs-lookup"><span data-stu-id="e4758-182">Each of these regions is loaded with contents form other views when requested.</span></span>

<span data-ttu-id="e4758-183">`compose` Powiązania w nagłówku i stopce występują ustalone w Hot Towel, aby wskazywał `nav` i `footer` widoków, odpowiednio.</span><span class="sxs-lookup"><span data-stu-id="e4758-183">The `compose` bindings for the header and footer are hard coded in Hot Towel to point to the `nav` and `footer` views, respectively.</span></span> <span data-ttu-id="e4758-184">Powiązanie compose sekcji `#content` jest powiązany z `router` aktywny element modułu.</span><span class="sxs-lookup"><span data-stu-id="e4758-184">The compose binding for the section `#content` is bound to the `router` module's active item.</span></span> <span data-ttu-id="e4758-185">Innymi słowy po kliknięciu łącza nawigacji jest odpowiedni widok jest ładowany w tym obszarze.</span><span class="sxs-lookup"><span data-stu-id="e4758-185">In other words, when you click a navigation link it's corresponding view is loaded in this area.</span></span>

[!code-html[Main](hottowel-template/samples/sample4.html)]

### <a name="navhtml"></a><span data-ttu-id="e4758-186">nav.html</span><span class="sxs-lookup"><span data-stu-id="e4758-186">nav.html</span></span>

<span data-ttu-id="e4758-187">`nav.html` Zawiera łącza nawigacji dla SPA.</span><span class="sxs-lookup"><span data-stu-id="e4758-187">The `nav.html` contains the navigation links for the SPA.</span></span> <span data-ttu-id="e4758-188">Jest to, gdzie struktury menu mogą być umieszczane, na przykład.</span><span class="sxs-lookup"><span data-stu-id="e4758-188">This is where the menu structure can be placed, for example.</span></span> <span data-ttu-id="e4758-189">Często jest to dane powiązane (przy użyciu Knockout) do `router` modułu, aby wyświetlić nawigacji zdefiniowane w `shell.js`.</span><span class="sxs-lookup"><span data-stu-id="e4758-189">Often this is data bound (using Knockout) to the `router` module to display the navigation you defined in the `shell.js`.</span></span> <span data-ttu-id="e4758-190">Knockout wyszukuje wiązania danych atrybutów, a także tych, które mają powiązania `shell` viewmodel w celu wyświetlania tras nawigacji i wyświetlania elementu progressbar (przy użyciu usługi Twitter Bootstrap) Jeśli `router` modułu jest w trakcie przechodzenia z jednego widoku do innego (patrz `router.isNavigating`).</span><span class="sxs-lookup"><span data-stu-id="e4758-190">Knockout looks for the data-bind attributes and binds those to the `shell` viewmodel to display the navigation routes and to show a progressbar (using Twitter Bootstrap) if the `router` module is in the middle of navigating from one view to another (see `router.isNavigating`).</span></span>

[!code-html[Main](hottowel-template/samples/sample5.html)]

### <a name="homehtml-and-detailshtml"></a><span data-ttu-id="e4758-191">Home.HTML i details.html</span><span class="sxs-lookup"><span data-stu-id="e4758-191">home.html and details.html</span></span>

<span data-ttu-id="e4758-192">Widoki te zawierają HTML do widoków niestandardowych.</span><span class="sxs-lookup"><span data-stu-id="e4758-192">These views contain HTML for custom views.</span></span> <span data-ttu-id="e4758-193">Gdy `home` łącze w `nav` kliknięciu menu widoku `home` widoku zostaną umieszczone w obszarze zawartości `shell` widoku.</span><span class="sxs-lookup"><span data-stu-id="e4758-193">When the `home` link in the `nav` view's menu is clicked, the `home` view will be placed in the content area of the `shell` view.</span></span> <span data-ttu-id="e4758-194">Widoki te mogą rozszerzone lub zastąpić widoki niestandardowe.</span><span class="sxs-lookup"><span data-stu-id="e4758-194">These views can be augmented or replaced with your own custom views.</span></span>

### <a name="footerhtml"></a><span data-ttu-id="e4758-195">footer.html</span><span class="sxs-lookup"><span data-stu-id="e4758-195">footer.html</span></span>

<span data-ttu-id="e4758-196">`footer.html` Zawiera kod HTML, który pojawia się w stopce w dolnej części `shell` widoku.</span><span class="sxs-lookup"><span data-stu-id="e4758-196">The `footer.html` contains HTML that appears in the footer, at the bottom of the `shell` view.</span></span>

## <a name="viewmodels"></a><span data-ttu-id="e4758-197">Modele widoków</span><span class="sxs-lookup"><span data-stu-id="e4758-197">ViewModels</span></span>

<span data-ttu-id="e4758-198">Modele widoków znajdują się w `App/viewmodels` folderu.</span><span class="sxs-lookup"><span data-stu-id="e4758-198">ViewModels are found in the `App/viewmodels` folder.</span></span>

### <a name="shelljs"></a><span data-ttu-id="e4758-199">shell.js</span><span class="sxs-lookup"><span data-stu-id="e4758-199">shell.js</span></span>

<span data-ttu-id="e4758-200">`shell` Viewmodel zawiera właściwości i funkcje, które są powiązane z `shell` widoku.</span><span class="sxs-lookup"><span data-stu-id="e4758-200">The `shell` viewmodel contains properties and functions that are bound to the `shell` view.</span></span> <span data-ttu-id="e4758-201">Często jest to, gdzie znajdują się menu nawigacji powiązania (zobacz `router.mapNav` logiki).</span><span class="sxs-lookup"><span data-stu-id="e4758-201">Often this is where the menu navigation bindings are found (see the `router.mapNav` logic).</span></span>

[!code-javascript[Main](hottowel-template/samples/sample6.js)]

### <a name="homejs-and-detailsjs"></a><span data-ttu-id="e4758-202">Home.js i details.js</span><span class="sxs-lookup"><span data-stu-id="e4758-202">home.js and details.js</span></span>

<span data-ttu-id="e4758-203">Te modele widoków zawierają właściwości i funkcje, które są powiązane z `home` widoku.</span><span class="sxs-lookup"><span data-stu-id="e4758-203">These viewmodels contain the properties and functions that are bound to the `home` view.</span></span> <span data-ttu-id="e4758-204">on również zawiera logikę prezentacji dla widoku i jest pośredniczącego między danymi a widoku.</span><span class="sxs-lookup"><span data-stu-id="e4758-204">it also contains the presentation logic for the view, and is the glue between the data and the view.</span></span>

[!code-javascript[Main](hottowel-template/samples/sample7.js)]

## <a name="services"></a><span data-ttu-id="e4758-205">Usługi</span><span class="sxs-lookup"><span data-stu-id="e4758-205">Services</span></span>

<span data-ttu-id="e4758-206">Usługi znajdują się w folderze aplikacji/usługi.</span><span class="sxs-lookup"><span data-stu-id="e4758-206">Services are found in the App/services folder.</span></span> <span data-ttu-id="e4758-207">Najlepiej przyszłych usług takich jak moduł usługi danych, który jest odpowiedzialny za uzyskanie i publikowanie danych zdalnych, można umieścić.</span><span class="sxs-lookup"><span data-stu-id="e4758-207">Ideally your future services such as a dataservice module, that is responsible for getting and posting remote data, could be placed.</span></span>

### <a name="loggerjs"></a><span data-ttu-id="e4758-208">logger.js</span><span class="sxs-lookup"><span data-stu-id="e4758-208">logger.js</span></span>

<span data-ttu-id="e4758-209">Udostępnia hot Towel `logger` modułu w folderze usługi.</span><span class="sxs-lookup"><span data-stu-id="e4758-209">Hot Towel provides a `logger` module in the services folder.</span></span> <span data-ttu-id="e4758-210">`logger` Modułu jest idealny dla rejestrowanie komunikatów do konsoli i użytkownika w menu podręcznym wyskakujące powiadomienia.</span><span class="sxs-lookup"><span data-stu-id="e4758-210">The `logger` module is ideal for logging messages to the console and to the user in pop up toasts.</span></span>
