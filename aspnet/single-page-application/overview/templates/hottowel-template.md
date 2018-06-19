---
uid: single-page-application/overview/templates/hottowel-template
title: Szablon ręczników gorących | Dokumentacja firmy Microsoft
author: madskristensen
description: Szablon HotTowel
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/09/2013
ms.topic: article
ms.assetid: 75af2e17-6ed3-4d24-8ea1-bc340027c318
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /single-page-application/overview/templates/hottowel-template
msc.type: authoredcontent
ms.openlocfilehash: dbd037c2469d326a3d3248ca07492ed9eb93e225
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30875022"
---
<a name="hot-towel-template"></a><span data-ttu-id="e4587-103">Hot ręczników szablonu</span><span class="sxs-lookup"><span data-stu-id="e4587-103">Hot Towel template</span></span>
====================
<span data-ttu-id="e4587-104">przez [Mads Kristensen](https://github.com/madskristensen)</span><span class="sxs-lookup"><span data-stu-id="e4587-104">by [Mads Kristensen](https://github.com/madskristensen)</span></span>

> <span data-ttu-id="e4587-105">Gorących szablonu MVC ręczników została napisana przez Papa Jan</span><span class="sxs-lookup"><span data-stu-id="e4587-105">The Hot Towel MVC Template is written by John Papa</span></span>
> 
> <span data-ttu-id="e4587-106">Należy wybrać wersję, która do pobrania:</span><span class="sxs-lookup"><span data-stu-id="e4587-106">Choose which version to download:</span></span>
> 
> [<span data-ttu-id="e4587-107">Szablon MVC ręczników dostępu dla programu Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="e4587-107">Hot Towel MVC Template for Visual Studio 2012</span></span>](https://visualstudiogallery.msdn.microsoft.com/1f68fbe8-b4e9-4968-9fd3-ddc7cbc52dca)
> 
> [<span data-ttu-id="e4587-108">Szablon MVC gorących ręczników dla programu Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="e4587-108">Hot Towel MVC Template for Visual Studio 2013</span></span>](https://visualstudiogallery.msdn.microsoft.com/1eb8780d-d522-4dcf-bf56-56f0eab305c2)
> 
> 
> <span data-ttu-id="e4587-109">Gorących ręczników: Ponieważ nie chcesz przejść do SPA bez!</span><span class="sxs-lookup"><span data-stu-id="e4587-109">Hot Towel: Because you don't want to go to the SPA without one!</span></span>


<span data-ttu-id="e4587-110">Chcesz tworzyć SPA, ale nie można zdecydować, jak zacząć?</span><span class="sxs-lookup"><span data-stu-id="e4587-110">Want to build a SPA but can't decide where to start?</span></span> <span data-ttu-id="e4587-111">Użyj gorących ręczników i w sekundach będziesz mieć SPA i wszystkie narzędzia potrzebne do utworzenia na nim!</span><span class="sxs-lookup"><span data-stu-id="e4587-111">Use Hot Towel and in seconds you'll have a SPA and all the tools you need to build on it!</span></span>

<span data-ttu-id="e4587-112">Gorących ręczników tworzy doskonały punkt początkowy do tworzenia jednej strony aplikacji JEDNOSTRONICOWEJ ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="e4587-112">Hot Towel creates a great starting point for building a Single Page Application (SPA) with ASP.NET.</span></span> <span data-ttu-id="e4587-113">Poza pole użytkownik udostępnia modularna struktura do kodu, nawigacji widoku, powiązania danych, zarządzanie danymi sformatowanego i style proste, ale elegancki.</span><span class="sxs-lookup"><span data-stu-id="e4587-113">Out of the box you it provides a modular structure for your code, view navigation, data binding, rich data management and simple but elegant styling.</span></span> <span data-ttu-id="e4587-114">Gorących ręczników zawiera wszystko, co jest potrzebne do tworzenia SPA, dzięki czemu można skoncentrować się na aplikacji, nie żmudne procesy.</span><span class="sxs-lookup"><span data-stu-id="e4587-114">Hot Towel provides everything you need to build a SPA, so you can focus on your app, not the plumbing.</span></span>

> <span data-ttu-id="e4587-115">Dowiedz się więcej o tworzeniu SPA z [Papa Jan wideo, samouczki i szkolenia Pluralsight](http://johnpapa.net/spa?vsix).</span><span class="sxs-lookup"><span data-stu-id="e4587-115">Learn more about building a SPA from [John Papa's videos, tutorials and Pluralsight courses](http://johnpapa.net/spa?vsix).</span></span>


## <a name="application-structure"></a><span data-ttu-id="e4587-116">Struktura aplikacji</span><span class="sxs-lookup"><span data-stu-id="e4587-116">Application Structure</span></span>

<span data-ttu-id="e4587-117">Gorących SPA ręczników zawiera folder aplikacji zawierającej pliki JavaScript i HTML, które definiują aplikację.</span><span class="sxs-lookup"><span data-stu-id="e4587-117">Hot Towel SPA provides an App folder which contains the JavaScript and HTML files that define your application.</span></span>

<span data-ttu-id="e4587-118">W folderze aplikacji:</span><span class="sxs-lookup"><span data-stu-id="e4587-118">Inside the App folder:</span></span>

- <span data-ttu-id="e4587-119">Durandal</span><span class="sxs-lookup"><span data-stu-id="e4587-119">durandal</span></span>
- <span data-ttu-id="e4587-120">usługi</span><span class="sxs-lookup"><span data-stu-id="e4587-120">services</span></span>
- <span data-ttu-id="e4587-121">viewmodels</span><span class="sxs-lookup"><span data-stu-id="e4587-121">viewmodels</span></span>
- <span data-ttu-id="e4587-122">widoki</span><span class="sxs-lookup"><span data-stu-id="e4587-122">views</span></span>

<span data-ttu-id="e4587-123">Folder aplikacji zawiera zbiór modułów.</span><span class="sxs-lookup"><span data-stu-id="e4587-123">The App folder contains a collection of modules.</span></span> <span data-ttu-id="e4587-124">Te moduły Hermetyzowanie funkcjonalność i zadeklarować zależności w innych modułach.</span><span class="sxs-lookup"><span data-stu-id="e4587-124">These modules encapsulate functionality and declare dependencies on other modules.</span></span> <span data-ttu-id="e4587-125">Folder widoków zawiera kod HTML dla aplikacji i viewmodels folder zawiera logikę prezentacji widoków (wspólnego wzorca MVVM).</span><span class="sxs-lookup"><span data-stu-id="e4587-125">The views folder contains the HTML for your application and the viewmodels folder contains the presentation logic for the views (a common MVVM pattern).</span></span> <span data-ttu-id="e4587-126">Folder usługi jest idealne rozwiązanie w przypadku których wspólne usługi, które mogą wymagać aplikacji, takie jak pobieranie danych protokołu HTTP lub interakcji lokalnej pamięci masowej.</span><span class="sxs-lookup"><span data-stu-id="e4587-126">The services folder is ideal for housing any common services that your application may need such as HTTP data retrieval or local storage interaction.</span></span> <span data-ttu-id="e4587-127">Jest typowe dla wielu viewmodels do ponownego użycia kodu z modułów usługi.</span><span class="sxs-lookup"><span data-stu-id="e4587-127">It is common for multiple viewmodels to re-use code from the service modules.</span></span>

## <a name="aspnet-mvc-server-side-application-structure"></a><span data-ttu-id="e4587-128">Struktura aplikacji po stronie serwera programu ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="e4587-128">ASP.NET MVC Server Side Application Structure</span></span>

<span data-ttu-id="e4587-129">Gorących ręczników opiera się na znanych i wydajnych struktury ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="e4587-129">Hot Towel builds on the familiar and powerful ASP.NET MVC structure.</span></span>

- <span data-ttu-id="e4587-130">Aplikacja\_Start</span><span class="sxs-lookup"><span data-stu-id="e4587-130">App\_Start</span></span>
- <span data-ttu-id="e4587-131">Zawartość</span><span class="sxs-lookup"><span data-stu-id="e4587-131">Content</span></span>
- <span data-ttu-id="e4587-132">Kontrolery</span><span class="sxs-lookup"><span data-stu-id="e4587-132">Controllers</span></span>
- <span data-ttu-id="e4587-133">Modele</span><span class="sxs-lookup"><span data-stu-id="e4587-133">Models</span></span>
- <span data-ttu-id="e4587-134">Skrypty</span><span class="sxs-lookup"><span data-stu-id="e4587-134">Scripts</span></span>
- <span data-ttu-id="e4587-135">Widoki</span><span class="sxs-lookup"><span data-stu-id="e4587-135">Views</span></span>

## <a name="featured-libraries"></a><span data-ttu-id="e4587-136">Promowanie bibliotek</span><span class="sxs-lookup"><span data-stu-id="e4587-136">Featured Libraries</span></span>

- <span data-ttu-id="e4587-137">ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="e4587-137">ASP.NET MVC</span></span>
- <span data-ttu-id="e4587-138">ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="e4587-138">ASP.NET Web API</span></span>
- <span data-ttu-id="e4587-139">Optymalizacja sieci Web platformy ASP.NET — tworzenie pakietów i minimalizowanie</span><span class="sxs-lookup"><span data-stu-id="e4587-139">ASP.NET Web Optimization - bundling and minification</span></span>
- <span data-ttu-id="e4587-140">[BREEZE.js](http://Breezejs.com) -zaawansowanych danych zarządzania</span><span class="sxs-lookup"><span data-stu-id="e4587-140">[Breeze.js](http://Breezejs.com) - rich data management</span></span>
- <span data-ttu-id="e4587-141">[Durandal.js](http://Durandaljs.com) -nawigacji i kompozycji widoku</span><span class="sxs-lookup"><span data-stu-id="e4587-141">[Durandal.js](http://Durandaljs.com) - navigation and View composition</span></span>
- <span data-ttu-id="e4587-142">[Knockout.js](http://Knockoutjs.com) -powiązania danych</span><span class="sxs-lookup"><span data-stu-id="e4587-142">[Knockout.js](http://Knockoutjs.com) - data bindings</span></span>
- <span data-ttu-id="e4587-143">[Require.js](http://requirejs.org) -Modułowość AMD i optymalizacji</span><span class="sxs-lookup"><span data-stu-id="e4587-143">[Require.js](http://requirejs.org) - Modularity with AMD and optimization</span></span>
- <span data-ttu-id="e4587-144">[Toastr.js](http://jpapa.me/c7toastr) -komunikatów podręcznych</span><span class="sxs-lookup"><span data-stu-id="e4587-144">[Toastr.js](http://jpapa.me/c7toastr) - pop-up messages</span></span>
- <span data-ttu-id="e4587-145">[W usłudze Twitter Bootstrap](http://twitter.github.com/bootstrap/) — niezawodny CSS Ustawianie stylów</span><span class="sxs-lookup"><span data-stu-id="e4587-145">[Twitter Bootstrap](http://twitter.github.com/bootstrap/) - robust CSS styling</span></span>

## <a name="installing-via-the-visual-studio-2012-hot-towel-spa-template"></a><span data-ttu-id="e4587-146">Instalowanie przy użyciu szablonu SPA gorących ręczników programu Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="e4587-146">Installing via the Visual Studio 2012 Hot Towel SPA Template</span></span>

<span data-ttu-id="e4587-147">Ręczników dynamicznej można zainstalować jako szablon programu Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="e4587-147">Hot Towel can be installed as a Visual Studio 2012 template.</span></span> <span data-ttu-id="e4587-148">Po prostu kliknij `File`  |  `New Project` i wybierz polecenie `ASP.NET MVC 4 Web Application`.</span><span class="sxs-lookup"><span data-stu-id="e4587-148">Just click `File` | `New Project` and choose `ASP.NET MVC 4 Web Application`.</span></span> <span data-ttu-id="e4587-149">Następnie wybierz pozycję "gorących ręczników jednej strony aplikacji" szablonu i uruchom!</span><span class="sxs-lookup"><span data-stu-id="e4587-149">Then select the 'Hot Towel Single Page Application" template and run!</span></span>

## <a name="installing-via-the-nuget-package"></a><span data-ttu-id="e4587-150">Instalowanie za pośrednictwem pakietu NuGet</span><span class="sxs-lookup"><span data-stu-id="e4587-150">Installing via the NuGet Package</span></span>

<span data-ttu-id="e4587-151">Gorących ręczników jest również pakietu NuGet, który wspomaga istniejącego pustego projektu platformy ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="e4587-151">Hot Towel is also a NuGet package that augments an existing empty ASP.NET MVC project.</span></span> <span data-ttu-id="e4587-152">Po prostu zainstaluj przy użyciu narzędzia Nuget, a następnie uruchom!</span><span class="sxs-lookup"><span data-stu-id="e4587-152">Just install using Nuget and then run!</span></span>

[!code-powershell[Main](hottowel-template/samples/sample1.ps1)]

## <a name="how-do-i-build-on-hot-towel"></a><span data-ttu-id="e4587-153">Jak zbudować na gorąco ręczników?</span><span class="sxs-lookup"><span data-stu-id="e4587-153">How Do I Build On Hot Towel?</span></span>

<span data-ttu-id="e4587-154">Wystarczy uruchomić Dodawanie kodu!</span><span class="sxs-lookup"><span data-stu-id="e4587-154">Simply start adding code!</span></span>

1. <span data-ttu-id="e4587-155">Dodaj własny kod po stronie serwera, najlepiej Entity Framework i WebAPI (które są naprawdę widoczne z Breeze.js)</span><span class="sxs-lookup"><span data-stu-id="e4587-155">Add your own server-side code, preferably Entity Framework and WebAPI (which really shine with Breeze.js)</span></span>
2. <span data-ttu-id="e4587-156">Dodawanie widoków `App/views` folderu</span><span class="sxs-lookup"><span data-stu-id="e4587-156">Add views to the `App/views` folder</span></span>
3. <span data-ttu-id="e4587-157">Dodaj viewmodels do `App/viewmodels` folderu</span><span class="sxs-lookup"><span data-stu-id="e4587-157">Add viewmodels to the `App/viewmodels` folder</span></span>
4. <span data-ttu-id="e4587-158">Dodawanie nowych widoków HTML i odcinania powiązania danych</span><span class="sxs-lookup"><span data-stu-id="e4587-158">Add HTML and Knockout data bindings to your new views</span></span>
5. <span data-ttu-id="e4587-159">Aktualizowanie tras nawigacji w `shell.js`</span><span class="sxs-lookup"><span data-stu-id="e4587-159">Update the navigation routes in `shell.js`</span></span>

## <a name="walkthrough-of-the-htmljavascript"></a><span data-ttu-id="e4587-160">Wskazówki HTML/JavaScript</span><span class="sxs-lookup"><span data-stu-id="e4587-160">Walkthrough of the HTML/JavaScript</span></span>

### <a name="viewshottowelindexcshtml"></a><span data-ttu-id="e4587-161">Views/HotTowel/index.cshtml</span><span class="sxs-lookup"><span data-stu-id="e4587-161">Views/HotTowel/index.cshtml</span></span>

<span data-ttu-id="e4587-162">index.cshtml jest początkowy trasy i widoku dla aplikacji MVC.</span><span class="sxs-lookup"><span data-stu-id="e4587-162">index.cshtml is the starting route and view for the MVC application.</span></span> <span data-ttu-id="e4587-163">Zawiera wszystkie standardowe tagi meta, łącza css i JavaScript odwołania, których można oczekiwać.</span><span class="sxs-lookup"><span data-stu-id="e4587-163">It contains all the standard meta tags, css links, and JavaScript references you would expect.</span></span> <span data-ttu-id="e4587-164">Treść zawiera jeden `<div>` czyli gdzie całej zawartości (widoki) zostaną umieszczone w przypadku żądania.</span><span class="sxs-lookup"><span data-stu-id="e4587-164">The body contains a single `<div>` which is where all of the content (your views) will be placed when they are requested.</span></span> <span data-ttu-id="e4587-165">`@Scripts.Render` Używa Require.js do uruchomienia punktu wejścia dla kodu aplikacji, która jest zawarta w `main.js` pliku.</span><span class="sxs-lookup"><span data-stu-id="e4587-165">The `@Scripts.Render` uses Require.js to run the entrance point for the application's code, which is contained in the `main.js` file.</span></span> <span data-ttu-id="e4587-166">Ekran powitalny zapewnia pokazują, jak utworzyć ekranu powitalnego aplikacji ładowane.</span><span class="sxs-lookup"><span data-stu-id="e4587-166">A splash screen is provided to demonstrate how to create a splash screen while your app loads.</span></span>

[!code-cshtml[Main](hottowel-template/samples/sample2.cshtml)]

### <a name="appmainjs"></a><span data-ttu-id="e4587-167">App/main.js</span><span class="sxs-lookup"><span data-stu-id="e4587-167">App/main.js</span></span>

<span data-ttu-id="e4587-168">`main.js` Plik zawiera kod, który będzie uruchamiany natychmiast po załadowaniu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e4587-168">The `main.js` file contains the code that will run as soon as your app is loaded.</span></span> <span data-ttu-id="e4587-169">Jest to, które chcesz zdefiniować trasy nawigacji, ustaw uruchamiania się widoki i wykonać wszelkie ustawienia/uruchamianie takich jak priming danych aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e4587-169">This is where you want to define your navigation routes, set your start up views, and perform any setup/bootstrapping such as priming your application's data.</span></span>

<span data-ttu-id="e4587-170">`main.js` Plik definiuje kilka modułów w durandal ułatwiające rozpoczęcie aplikacji start.</span><span class="sxs-lookup"><span data-stu-id="e4587-170">The `main.js` file defines several of durandal's modules to help the application kick start.</span></span> <span data-ttu-id="e4587-171">Instrukcja Definiuj pomaga rozpoznać zależności modułów, aby były dostępne dla tej funkcji.</span><span class="sxs-lookup"><span data-stu-id="e4587-171">The define statement helps resolve the modules dependencies so they are available for the function.</span></span> <span data-ttu-id="e4587-172">Najpierw komunikaty debugowania są włączone, który wysyła komunikaty o zdarzeniach, jakie się, że aplikacja działa w oknie konsoli.</span><span class="sxs-lookup"><span data-stu-id="e4587-172">First the debugging messages are enabled, which send messages about what events the application is performing to the console window.</span></span> <span data-ttu-id="e4587-173">Kod app.start informuje durandal framework do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e4587-173">The app.start code tells durandal framework to start the application.</span></span> <span data-ttu-id="e4587-174">Konwencje są ustawiane tak, aby durandal zna wszystkie widoki i viewmodels są zawarte w tych samych folderach nazwanego, odpowiednio.</span><span class="sxs-lookup"><span data-stu-id="e4587-174">The conventions are set so that durandal knows all views and viewmodels are contained in the same named folders, respectively.</span></span> <span data-ttu-id="e4587-175">Na koniec `app.setRoot` uruchamiane obciążenia `shell` przy użyciu wstępnie zdefiniowanej `entrance` animacji.</span><span class="sxs-lookup"><span data-stu-id="e4587-175">Finally, the `app.setRoot` kicks loads the `shell` using a predefined `entrance` animation.</span></span>

[!code-javascript[Main](hottowel-template/samples/sample3.js)]

## <a name="views"></a><span data-ttu-id="e4587-176">Widoki</span><span class="sxs-lookup"><span data-stu-id="e4587-176">Views</span></span>

<span data-ttu-id="e4587-177">Widoki znajdują się w `App/views` folderu.</span><span class="sxs-lookup"><span data-stu-id="e4587-177">Views are found in the `App/views` folder.</span></span>

### <a name="shellhtml"></a><span data-ttu-id="e4587-178">shell.html</span><span class="sxs-lookup"><span data-stu-id="e4587-178">shell.html</span></span>

<span data-ttu-id="e4587-179">`shell.html` Zawiera głównego układu dla kodzie HTML.</span><span class="sxs-lookup"><span data-stu-id="e4587-179">The `shell.html` contains the master layout for your HTML.</span></span> <span data-ttu-id="e4587-180">Wszystkie inne widoków będzie składać się gdzieś po stronie użytkownika `shell` widoku.</span><span class="sxs-lookup"><span data-stu-id="e4587-180">All of your other views will be composed somewhere in side of your `shell` view.</span></span> <span data-ttu-id="e4587-181">Udostępnia ręczników gorących `shell` z trzech takich regionów: nagłówek, obszar zawartości i stopki.</span><span class="sxs-lookup"><span data-stu-id="e4587-181">Hot Towel provides a `shell` with three such regions: a header, a content area, and a footer.</span></span> <span data-ttu-id="e4587-182">Każdy z tych obszarów z załadowanymi zawartość formularza innych widoków na żądanie.</span><span class="sxs-lookup"><span data-stu-id="e4587-182">Each of these regions is loaded with contents form other views when requested.</span></span>

<span data-ttu-id="e4587-183">`compose` Powiązania nagłówku i stopce występują ustalone w dynamicznej ręczników wskaż `nav` i `footer` widoków odpowiednio.</span><span class="sxs-lookup"><span data-stu-id="e4587-183">The `compose` bindings for the header and footer are hard coded in Hot Towel to point to the `nav` and `footer` views, respectively.</span></span> <span data-ttu-id="e4587-184">Powiązanie compose dla sekcji `#content` jest powiązany z `router` aktywny element modułu.</span><span class="sxs-lookup"><span data-stu-id="e4587-184">The compose binding for the section `#content` is bound to the `router` module's active item.</span></span> <span data-ttu-id="e4587-185">Innymi słowy po kliknięciu łącza nawigacji jest odpowiedni widok został załadowany w tym obszarze.</span><span class="sxs-lookup"><span data-stu-id="e4587-185">In other words, when you click a navigation link it's corresponding view is loaded in this area.</span></span>

[!code-html[Main](hottowel-template/samples/sample4.html)]

### <a name="navhtml"></a><span data-ttu-id="e4587-186">nav.html</span><span class="sxs-lookup"><span data-stu-id="e4587-186">nav.html</span></span>

<span data-ttu-id="e4587-187">`nav.html` Zawiera łącza nawigacji, aby aplikacja JEDNOSTRONICOWA.</span><span class="sxs-lookup"><span data-stu-id="e4587-187">The `nav.html` contains the navigation links for the SPA.</span></span> <span data-ttu-id="e4587-188">Jest to struktura menu rozmieszczenia, np.</span><span class="sxs-lookup"><span data-stu-id="e4587-188">This is where the menu structure can be placed, for example.</span></span> <span data-ttu-id="e4587-189">Często jest to danymi powiązanymi (przy użyciu odcinania) do `router` modułu, aby wyświetlić nawigacji zdefiniowane w `shell.js`.</span><span class="sxs-lookup"><span data-stu-id="e4587-189">Often this is data bound (using Knockout) to the `router` module to display the navigation you defined in the `shell.js`.</span></span> <span data-ttu-id="e4587-190">Odcinania szuka wiązania danych atrybutów i wiąże tych `shell` viewmodel w celu wyświetlenia trasy nawigacji i wyświetlania elementu progressbar (przy użyciu Twitter Bootstrap) Jeśli `router` modułu jest w trakcie przechodzenia z jednego widoku do innego (patrz `router.isNavigating`).</span><span class="sxs-lookup"><span data-stu-id="e4587-190">Knockout looks for the data-bind attributes and binds those to the `shell` viewmodel to display the navigation routes and to show a progressbar (using Twitter Bootstrap) if the `router` module is in the middle of navigating from one view to another (see `router.isNavigating`).</span></span>

[!code-html[Main](hottowel-template/samples/sample5.html)]

### <a name="homehtml-and-detailshtml"></a><span data-ttu-id="e4587-191">Home.HTML i details.html</span><span class="sxs-lookup"><span data-stu-id="e4587-191">home.html and details.html</span></span>

<span data-ttu-id="e4587-192">Widoki te zawierają HTML, niestandardowe widoki.</span><span class="sxs-lookup"><span data-stu-id="e4587-192">These views contain HTML for custom views.</span></span> <span data-ttu-id="e4587-193">Gdy `home` łącze w `nav` kliknięciu menu widoku `home` widoku zostaną umieszczone w obszarze zawartości `shell` widoku.</span><span class="sxs-lookup"><span data-stu-id="e4587-193">When the `home` link in the `nav` view's menu is clicked, the `home` view will be placed in the content area of the `shell` view.</span></span> <span data-ttu-id="e4587-194">Widoki te można rozszerzony lub zastąpione własnych widoków niestandardowych.</span><span class="sxs-lookup"><span data-stu-id="e4587-194">These views can be augmented or replaced with your own custom views.</span></span>

### <a name="footerhtml"></a><span data-ttu-id="e4587-195">footer.html</span><span class="sxs-lookup"><span data-stu-id="e4587-195">footer.html</span></span>

<span data-ttu-id="e4587-196">`footer.html` Zawiera kod HTML, który jest wyświetlany w stopce strony, u dołu `shell` widoku.</span><span class="sxs-lookup"><span data-stu-id="e4587-196">The `footer.html` contains HTML that appears in the footer, at the bottom of the `shell` view.</span></span>

## <a name="viewmodels"></a><span data-ttu-id="e4587-197">ViewModels</span><span class="sxs-lookup"><span data-stu-id="e4587-197">ViewModels</span></span>

<span data-ttu-id="e4587-198">Znaleziono ViewModels `App/viewmodels` folderu.</span><span class="sxs-lookup"><span data-stu-id="e4587-198">ViewModels are found in the `App/viewmodels` folder.</span></span>

### <a name="shelljs"></a><span data-ttu-id="e4587-199">shell.js</span><span class="sxs-lookup"><span data-stu-id="e4587-199">shell.js</span></span>

<span data-ttu-id="e4587-200">`shell` Viewmodel zawiera właściwości i funkcje, które są powiązane z `shell` widoku.</span><span class="sxs-lookup"><span data-stu-id="e4587-200">The `shell` viewmodel contains properties and functions that are bound to the `shell` view.</span></span> <span data-ttu-id="e4587-201">Często jest to, gdzie znajdują się powiązania z menu nawigacji (zobacz `router.mapNav` logiki).</span><span class="sxs-lookup"><span data-stu-id="e4587-201">Often this is where the menu navigation bindings are found (see the `router.mapNav` logic).</span></span>

[!code-javascript[Main](hottowel-template/samples/sample6.js)]

### <a name="homejs-and-detailsjs"></a><span data-ttu-id="e4587-202">Home.js i details.js</span><span class="sxs-lookup"><span data-stu-id="e4587-202">home.js and details.js</span></span>

<span data-ttu-id="e4587-203">Te viewmodels zawierają właściwości i funkcje, które są powiązane z `home` widoku.</span><span class="sxs-lookup"><span data-stu-id="e4587-203">These viewmodels contain the properties and functions that are bound to the `home` view.</span></span> <span data-ttu-id="e4587-204">on również zawiera logikę prezentacji dla widoku i jest sklejki między danymi i widoku.</span><span class="sxs-lookup"><span data-stu-id="e4587-204">it also contains the presentation logic for the view, and is the glue between the data and the view.</span></span>

[!code-javascript[Main](hottowel-template/samples/sample7.js)]

## <a name="services"></a><span data-ttu-id="e4587-205">Usługi</span><span class="sxs-lookup"><span data-stu-id="e4587-205">Services</span></span>

<span data-ttu-id="e4587-206">Usługi znajdują się w folderze aplikacji i usług.</span><span class="sxs-lookup"><span data-stu-id="e4587-206">Services are found in the App/services folder.</span></span> <span data-ttu-id="e4587-207">W idealnym przypadku przyszłych usług takich jak moduł dataservice, która jest odpowiedzialna za uzyskanie i przesyłanie danych zdalnych, może zostać umieszczona.</span><span class="sxs-lookup"><span data-stu-id="e4587-207">Ideally your future services such as a dataservice module, that is responsible for getting and posting remote data, could be placed.</span></span>

### <a name="loggerjs"></a><span data-ttu-id="e4587-208">logger.js</span><span class="sxs-lookup"><span data-stu-id="e4587-208">logger.js</span></span>

<span data-ttu-id="e4587-209">Udostępnia ręczników gorących `logger` modułu w folderze usługi.</span><span class="sxs-lookup"><span data-stu-id="e4587-209">Hot Towel provides a `logger` module in the services folder.</span></span> <span data-ttu-id="e4587-210">`logger` Modułu jest idealny dla rejestrowanie komunikatów do konsoli i użytkownikowi w wyświetlonym wyskakujące powiadomienia.</span><span class="sxs-lookup"><span data-stu-id="e4587-210">The `logger` module is ideal for logging messages to the console and to the user in pop up toasts.</span></span>
