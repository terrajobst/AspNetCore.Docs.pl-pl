---
title: Pakiet i minifiy zasoby statyczne platformy ASP.NET Core
author: scottaddie
description: Dowiedz się, jak zoptymalizować zasoby statyczne w aplikacji sieci web platformy ASP.NET Core za pomocą techniki tworzenie pakietów i minimalizowanie.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 01/10/2018
ms.devlang: csharp
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: client-side/bundling-and-minification
ms.openlocfilehash: 381025d1c4a9951349e242482ca9b780c461e588
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/07/2018
---
# <a name="bundle-and-minifiy-static-assets-in-aspnet-core"></a><span data-ttu-id="344cd-103">Pakiet i minifiy zasoby statyczne platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="344cd-103">Bundle and minifiy static assets in ASP.NET Core</span></span>

<span data-ttu-id="344cd-104">Przez [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="344cd-104">By [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="344cd-105">W tym artykule opisano zalety stosowania tworzenie pakietów i minimalizowanie, łącznie z tych funkcji możliwości korzystania z aplikacji sieci web platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="344cd-105">This article explains the benefits of applying bundling and minification, including how these features can be used with ASP.NET Core web apps.</span></span>

## <a name="what-is-bundling-and-minification"></a><span data-ttu-id="344cd-106">Co to jest tworzenie pakietów i minimalizowanie?</span><span class="sxs-lookup"><span data-stu-id="344cd-106">What is bundling and minification?</span></span>

<span data-ttu-id="344cd-107">Tworzenie pakietów i minimalizowanie są dwa optymalizacji wydajności distinct, które można zastosować w aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="344cd-107">Bundling and minification are two distinct performance optimizations you can apply in a web app.</span></span> <span data-ttu-id="344cd-108">Używane razem, tworzenie pakietów i minimalizowanie zwiększyć wydajność przez zmniejszenie liczby żądań serwera oraz redukcję rozmiaru żądanych zasobów statycznych.</span><span class="sxs-lookup"><span data-stu-id="344cd-108">Used together, bundling and minification improve performance by reducing the number of server requests and reducing the size of the requested static assets.</span></span>

<span data-ttu-id="344cd-109">Tworzenie pakietów i minimalizowanie głównie zwiększyć czas ładowania pierwsze żądanie strony.</span><span class="sxs-lookup"><span data-stu-id="344cd-109">Bundling and minification primarily improve the first page request load time.</span></span> <span data-ttu-id="344cd-110">Gdy zażądano strony sieci web, przeglądarka buforuje zasoby statyczne (JavaScript, CSS i obrazy).</span><span class="sxs-lookup"><span data-stu-id="344cd-110">Once a web page has been requested, the browser caches the static assets (JavaScript, CSS, and images).</span></span> <span data-ttu-id="344cd-111">W związku z tym tworzenie pakietów i minimalizowanie nie zwiększyć wydajność podczas żądania tej samej stronie lub strony w tej samej witrynie żąda tego samego zasoby.</span><span class="sxs-lookup"><span data-stu-id="344cd-111">Consequently, bundling and minification don't improve performance when requesting the same page, or pages, on the same site requesting the same assets.</span></span> <span data-ttu-id="344cd-112">Jeśli wygasa nagłówka nie jest poprawnie ustawiona na zasoby i tworzenie pakietów i minimalizowanie nie jest używany algorytm heurystyczny świeżości przeglądarki oznaczyć zasoby starych po upływie kilku dni.</span><span class="sxs-lookup"><span data-stu-id="344cd-112">If the expires header isn't set correctly on the assets and if bundling and minification isn't used, the browser's freshness heuristics mark the assets stale after a few days.</span></span> <span data-ttu-id="344cd-113">Ponadto przeglądarka wymaga żądanie sprawdzania poprawności dla każdego zasobu.</span><span class="sxs-lookup"><span data-stu-id="344cd-113">Additionally, the browser requires a validation request for each asset.</span></span> <span data-ttu-id="344cd-114">W takim przypadku tworzenie pakietów i minimalizowanie zapewnić lepszą wydajność, nawet po pierwszym żądaniu strony.</span><span class="sxs-lookup"><span data-stu-id="344cd-114">In this case, bundling and minification provide a performance improvement even after the first page request.</span></span>

### <a name="bundling"></a><span data-ttu-id="344cd-115">Tworzenie pakietów</span><span class="sxs-lookup"><span data-stu-id="344cd-115">Bundling</span></span>

<span data-ttu-id="344cd-116">Tworzenie pakietów łączy wiele plików do pojedynczego pliku.</span><span class="sxs-lookup"><span data-stu-id="344cd-116">Bundling combines multiple files into a single file.</span></span> <span data-ttu-id="344cd-117">Tworzenie pakietów zmniejsza liczbę żądań serwera, które są niezbędne do renderowania zawartości sieci web, takich jak strony sieci web.</span><span class="sxs-lookup"><span data-stu-id="344cd-117">Bundling reduces the number of server requests which are necessary to render a web asset, such as a web page.</span></span> <span data-ttu-id="344cd-118">Można utworzyć dowolną liczbę poszczególnych pakietów specjalnie z myślą o CSS, JavaScript,... itd. Mniej plików oznacza mniejszą liczbę żądań HTTP z przeglądarki do serwera lub usługi, zapewniając aplikacji.</span><span class="sxs-lookup"><span data-stu-id="344cd-118">You can create any number of individual bundles specifically for CSS, JavaScript, etc. Fewer files means fewer HTTP requests from the browser to the server or from the service providing your application.</span></span> <span data-ttu-id="344cd-119">Powoduje to wyższą wydajność obciążenia w usłudze pierwszej strony.</span><span class="sxs-lookup"><span data-stu-id="344cd-119">This results in improved first page load performance.</span></span>

### <a name="minification"></a><span data-ttu-id="344cd-120">Minimalizowanie</span><span class="sxs-lookup"><span data-stu-id="344cd-120">Minification</span></span>

<span data-ttu-id="344cd-121">Minimalizowanie usuwa niepotrzebne znaki z kodu bez zmiany funkcji.</span><span class="sxs-lookup"><span data-stu-id="344cd-121">Minification removes unnecessary characters from code without altering functionality.</span></span> <span data-ttu-id="344cd-122">Wynik jest zmniejszenie rozmiaru znaczących żądanych zasobów (takich jak CSS, obrazy i pliki JavaScript).</span><span class="sxs-lookup"><span data-stu-id="344cd-122">The result is a significant size reduction in requested assets (such as CSS, images, and JavaScript files).</span></span> <span data-ttu-id="344cd-123">Typowe efektami ubocznymi minimalizację obejmują skrócić nazwy zmiennych o jeden znak i usuwanie komentarzy i niepotrzebne odstępu.</span><span class="sxs-lookup"><span data-stu-id="344cd-123">Common side effects of minification include shortening variable names to one character and removing comments and unnecessary whitespace.</span></span>

<span data-ttu-id="344cd-124">Należy wziąć pod uwagę następujące funkcji JavaScript:</span><span class="sxs-lookup"><span data-stu-id="344cd-124">Consider the following JavaScript function:</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.js)]

<span data-ttu-id="344cd-125">Minimalizowanie zmniejsza funkcji do następującego:</span><span class="sxs-lookup"><span data-stu-id="344cd-125">Minification reduces the function to the following:</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.min.js)]

<span data-ttu-id="344cd-126">Oprócz usuwanie komentarzy i niepotrzebne odstępu, następujące nazwy parametrów i zmiennych nazwy zostały zmienione w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="344cd-126">In addition to removing the comments and unnecessary whitespace, the following parameter and variable names were renamed as follows:</span></span>

<span data-ttu-id="344cd-127">Oryginał</span><span class="sxs-lookup"><span data-stu-id="344cd-127">Original</span></span> | <span data-ttu-id="344cd-128">Zmieniono jego nazwę</span><span class="sxs-lookup"><span data-stu-id="344cd-128">Renamed</span></span>
--- | :---:
`imageTagAndImageID` | `t`
`imageContext` | `a`
`imageElement` | `r`

## <a name="impact-of-bundling-and-minification"></a><span data-ttu-id="344cd-129">Wpływ na tworzenie pakietów i minimalizowanie</span><span class="sxs-lookup"><span data-stu-id="344cd-129">Impact of bundling and minification</span></span>

<span data-ttu-id="344cd-130">W poniższej tabeli przedstawiono różnice między indywidualnie zasoby oraz za pomocą tworzenie pakietów i minimalizowanie:</span><span class="sxs-lookup"><span data-stu-id="344cd-130">The following table outlines differences between individually loading assets and using bundling and minification:</span></span>

<span data-ttu-id="344cd-131">Akcja</span><span class="sxs-lookup"><span data-stu-id="344cd-131">Action</span></span> | <span data-ttu-id="344cd-132">Z B/M</span><span class="sxs-lookup"><span data-stu-id="344cd-132">With B/M</span></span> | <span data-ttu-id="344cd-133">Bez B/M</span><span class="sxs-lookup"><span data-stu-id="344cd-133">Without B/M</span></span> | <span data-ttu-id="344cd-134">Zmiana</span><span class="sxs-lookup"><span data-stu-id="344cd-134">Change</span></span>
--- | :---: | :---: | :---:
<span data-ttu-id="344cd-135">Żądań plików</span><span class="sxs-lookup"><span data-stu-id="344cd-135">File Requests</span></span>  | <span data-ttu-id="344cd-136">7</span><span class="sxs-lookup"><span data-stu-id="344cd-136">7</span></span>   | <span data-ttu-id="344cd-137">18</span><span class="sxs-lookup"><span data-stu-id="344cd-137">18</span></span>     | <span data-ttu-id="344cd-138">157%</span><span class="sxs-lookup"><span data-stu-id="344cd-138">157%</span></span>
<span data-ttu-id="344cd-139">KB transferu</span><span class="sxs-lookup"><span data-stu-id="344cd-139">KB Transferred</span></span> | <span data-ttu-id="344cd-140">156</span><span class="sxs-lookup"><span data-stu-id="344cd-140">156</span></span> | <span data-ttu-id="344cd-141">264.68</span><span class="sxs-lookup"><span data-stu-id="344cd-141">264.68</span></span> | <span data-ttu-id="344cd-142">70%</span><span class="sxs-lookup"><span data-stu-id="344cd-142">70%</span></span>
<span data-ttu-id="344cd-143">Czas ładowania (ms)</span><span class="sxs-lookup"><span data-stu-id="344cd-143">Load Time (ms)</span></span> | <span data-ttu-id="344cd-144">885</span><span class="sxs-lookup"><span data-stu-id="344cd-144">885</span></span> | <span data-ttu-id="344cd-145">2360</span><span class="sxs-lookup"><span data-stu-id="344cd-145">2360</span></span>   | <span data-ttu-id="344cd-146">167%</span><span class="sxs-lookup"><span data-stu-id="344cd-146">167%</span></span>

<span data-ttu-id="344cd-147">Przeglądarek jest dość pełne względem nagłówków żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="344cd-147">Browsers are fairly verbose with regard to HTTP request headers.</span></span> <span data-ttu-id="344cd-148">Całkowita liczba bajtów wysłane Metryka wyświetlony znaczne obniżenie zużycia przy tworzeniu pakietów.</span><span class="sxs-lookup"><span data-stu-id="344cd-148">The total bytes sent metric saw a significant reduction when bundling.</span></span> <span data-ttu-id="344cd-149">Czas ładowania pokazuje znaczne ulepszenia, jednak w tym przykładzie został uruchomiony lokalnie.</span><span class="sxs-lookup"><span data-stu-id="344cd-149">The load time shows a significant improvement, however this example ran locally.</span></span> <span data-ttu-id="344cd-150">Większa wzrost wydajności są realizowane przy użyciu zasobów tworzenie pakietów i minimalizowanie przesyłanych przez sieć.</span><span class="sxs-lookup"><span data-stu-id="344cd-150">Greater performance gains are realized when using bundling and minification with assets transferred over a network.</span></span>

## <a name="choose-a-bundling-and-minification-strategy"></a><span data-ttu-id="344cd-151">Wybierz strategię tworzenie pakietów i minimalizowanie</span><span class="sxs-lookup"><span data-stu-id="344cd-151">Choose a bundling and minification strategy</span></span>

<span data-ttu-id="344cd-152">Szablony projektów MVC i stron Razor stanowią rozwiązanie poza pole dla tworzenie pakietów i minimalizowanie składające się z plikiem konfiguracji JSON.</span><span class="sxs-lookup"><span data-stu-id="344cd-152">The MVC and Razor Pages project templates provide an out-of-the-box solution for bundling and minification consisting of a JSON configuration file.</span></span> <span data-ttu-id="344cd-153">Narzędzia innych firm, takich jak [system Gulp](xref:client-side/using-gulp) i [Grunt](xref:client-side/using-grunt) zadanie uczestników, wykonać te same zadania z nieco bardziej złożona.</span><span class="sxs-lookup"><span data-stu-id="344cd-153">Third-party tools, such as the [Gulp](xref:client-side/using-gulp) and [Grunt](xref:client-side/using-grunt) task runners, accomplish the same tasks with a bit more complexity.</span></span> <span data-ttu-id="344cd-154">Narzędzia innej firmy jest doskonałym dopasowania, gdy przepływ pracy tworzenia wymaga przetwarzania poza tworzenie pakietów i minimalizowanie&mdash;takie jak Optymalizacja linting i obrazów.</span><span class="sxs-lookup"><span data-stu-id="344cd-154">A third-party tool is a great fit when your development workflow requires processing beyond bundling and minification&mdash;such as linting and image optimization.</span></span> <span data-ttu-id="344cd-155">Za pomocą tworzenie pakietów i minimalizowanie czasu projektowania, zminimalizowany pliki zostaną utworzone przed wdrożeniem aplikacji.</span><span class="sxs-lookup"><span data-stu-id="344cd-155">By using design-time bundling and minification, the minified files are created prior to the app's deployment.</span></span> <span data-ttu-id="344cd-156">Tworzenie pakietów i zminimalizowania liczby przed przystąpieniem do wdrożenia zawiera zaletą serwera zmniejszenie obciążenia.</span><span class="sxs-lookup"><span data-stu-id="344cd-156">Bundling and minifying before deployment provides the advantage of reduced server load.</span></span> <span data-ttu-id="344cd-157">Jednak ważne jest, aby rozpoznać tego czasu projektowania tworzenie pakietów i minimalizowanie zwiększa złożoności kompilacji i działa tylko w przypadku plików statycznych.</span><span class="sxs-lookup"><span data-stu-id="344cd-157">However, it's important to recognize that design-time bundling and minification increases build complexity and only works with static files.</span></span>

## <a name="configure-bundling-and-minification"></a><span data-ttu-id="344cd-158">Konfigurowanie, tworzenie pakietów i minimalizowanie</span><span class="sxs-lookup"><span data-stu-id="344cd-158">Configure bundling and minification</span></span>

<span data-ttu-id="344cd-159">Szablony projektów MVC i stron Razor zapewniają *bundleconfig.json* pliku konfiguracji, który definiuje opcje dla każdego pakietu.</span><span class="sxs-lookup"><span data-stu-id="344cd-159">The MVC and Razor Pages project templates provide a *bundleconfig.json* configuration file which defines the options for each bundle.</span></span> <span data-ttu-id="344cd-160">Domyślnie konfiguracja pojedynczy pakiet jest zdefiniowany dla niestandardowych skryptów JavaScript (*wwwroot/js/site.js*) i arkusza stylów (*wwwroot/css/site.css*) plików:</span><span class="sxs-lookup"><span data-stu-id="344cd-160">By default, a single bundle configuration is defined for the custom JavaScript (*wwwroot/js/site.js*) and stylesheet (*wwwroot/css/site.css*) files:</span></span>

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig.json)]

<span data-ttu-id="344cd-161">Opcje konfiguracji obejmują:</span><span class="sxs-lookup"><span data-stu-id="344cd-161">Configuration options include:</span></span>

* <span data-ttu-id="344cd-162">`outputFileName`: Nazwa pliku pakietu do danych wyjściowych.</span><span class="sxs-lookup"><span data-stu-id="344cd-162">`outputFileName`: The name of the bundle file to output.</span></span> <span data-ttu-id="344cd-163">Może zawierać ścieżki względnej z *bundleconfig.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="344cd-163">Can contain a relative path from the *bundleconfig.json* file.</span></span> <span data-ttu-id="344cd-164">**Wymagane**</span><span class="sxs-lookup"><span data-stu-id="344cd-164">**required**</span></span>
* <span data-ttu-id="344cd-165">`inputFiles`: Tablica plików do łączenia się ze sobą.</span><span class="sxs-lookup"><span data-stu-id="344cd-165">`inputFiles`: An array of files to bundle together.</span></span> <span data-ttu-id="344cd-166">Są to względne ścieżki do pliku konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="344cd-166">These are relative paths to the configuration file.</span></span> <span data-ttu-id="344cd-167">**opcjonalne**, \* pustą wartość wyniki w pliku wyjściowym puste.</span><span class="sxs-lookup"><span data-stu-id="344cd-167">**optional**, \*an empty value results in an empty output file.</span></span> <span data-ttu-id="344cd-168">[Globbing](http://www.tldp.org/LDP/abs/html/globbingref.html) wzorce są obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="344cd-168">[globbing](http://www.tldp.org/LDP/abs/html/globbingref.html) patterns are supported.</span></span>
* <span data-ttu-id="344cd-169">`minify`: Minimalizację opcje dla typu danych wyjściowych.</span><span class="sxs-lookup"><span data-stu-id="344cd-169">`minify`: The minification options for the output type.</span></span> <span data-ttu-id="344cd-170">**opcjonalne**, *domyślnie — `minify: { enabled: true }`*</span><span class="sxs-lookup"><span data-stu-id="344cd-170">**optional**, *default - `minify: { enabled: true }`*</span></span>
  * <span data-ttu-id="344cd-171">Opcje konfiguracji są dostępne na typ pliku wyjściowego.</span><span class="sxs-lookup"><span data-stu-id="344cd-171">Configuration options are available per output file type.</span></span>
    * [<span data-ttu-id="344cd-172">Element minimalizujący CSS</span><span class="sxs-lookup"><span data-stu-id="344cd-172">CSS Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki/cssminifier)
    * [<span data-ttu-id="344cd-173">JavaScript Minifier</span><span class="sxs-lookup"><span data-stu-id="344cd-173">JavaScript Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki/JavaScript-Minifier-settings)
    * [<span data-ttu-id="344cd-174">Element minimalizujący HTML</span><span class="sxs-lookup"><span data-stu-id="344cd-174">HTML Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki)
* <span data-ttu-id="344cd-175">`includeInProject`: Flaga oznaczająca, czy dodać pliki wygenerowane do pliku projektu.</span><span class="sxs-lookup"><span data-stu-id="344cd-175">`includeInProject`: Flag indicating whether to add generated files to project file.</span></span> <span data-ttu-id="344cd-176">**opcjonalne**, *domyślne — FAŁSZ.*</span><span class="sxs-lookup"><span data-stu-id="344cd-176">**optional**, *default - false*</span></span>
* <span data-ttu-id="344cd-177">`sourceMap`: Flaga oznaczająca, czy można wygenerować mapy źródła dla pliku powiązane.</span><span class="sxs-lookup"><span data-stu-id="344cd-177">`sourceMap`: Flag indicating whether to generate a source map for the bundled file.</span></span> <span data-ttu-id="344cd-178">**opcjonalne**, *domyślne — FAŁSZ.*</span><span class="sxs-lookup"><span data-stu-id="344cd-178">**optional**, *default - false*</span></span>
* <span data-ttu-id="344cd-179">`sourceMapRootPath`Ścieżka katalogu głównego do przechowywania pliku mapy wygenerowanego źródła.</span><span class="sxs-lookup"><span data-stu-id="344cd-179">`sourceMapRootPath`: The root path for storing the generated source map file.</span></span>

## <a name="build-time-execution-of-bundling-and-minification"></a><span data-ttu-id="344cd-180">Tworzenie pakietów i minimalizowanie wykonywanie czas kompilacji</span><span class="sxs-lookup"><span data-stu-id="344cd-180">Build-time execution of bundling and minification</span></span>

<span data-ttu-id="344cd-181">[BuildBundlerMinifier](https://www.nuget.org/packages/BuildBundlerMinifier/) pakietu NuGet umożliwia wykonywanie tworzenie pakietów i minimalizowanie podczas kompilacji.</span><span class="sxs-lookup"><span data-stu-id="344cd-181">The [BuildBundlerMinifier](https://www.nuget.org/packages/BuildBundlerMinifier/) NuGet package enables the execution of bundling and minification at build time.</span></span> <span data-ttu-id="344cd-182">Pakiet injects [docelowych elementów MSBuild](/visualstudio/msbuild/msbuild-targets) uruchamiania kompilacji i wyczyść godzinie.</span><span class="sxs-lookup"><span data-stu-id="344cd-182">The package injects [MSBuild Targets](/visualstudio/msbuild/msbuild-targets) which run at build and clean time.</span></span> <span data-ttu-id="344cd-183">*Bundleconfig.json* plików są analizowane przez proces kompilacji, aby utworzyć pliki wyjściowe na podstawie zdefiniowanych konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="344cd-183">The *bundleconfig.json* file is analyzed by the build process to produce the output files based on the defined configuration.</span></span>

> [!NOTE]
> <span data-ttu-id="344cd-184">BuildBundlerMinifier należy do projektu społeczność w witrynie GitHub, dla którego Microsoft nie zapewnia obsługi.</span><span class="sxs-lookup"><span data-stu-id="344cd-184">BuildBundlerMinifier belongs to a community-driven project on GitHub for which Microsoft provides no support.</span></span> <span data-ttu-id="344cd-185">Powinny być zgłaszane problemy [tutaj](https://github.com/madskristensen/BundlerMinifier/issues).</span><span class="sxs-lookup"><span data-stu-id="344cd-185">Issues should be filed [here](https://github.com/madskristensen/BundlerMinifier/issues).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="344cd-186">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="344cd-186">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="344cd-187">Dodaj *BuildBundlerMinifier* pakietu do projektu.</span><span class="sxs-lookup"><span data-stu-id="344cd-187">Add the *BuildBundlerMinifier* package to your project.</span></span>

<span data-ttu-id="344cd-188">Skompiluj projekt.</span><span class="sxs-lookup"><span data-stu-id="344cd-188">Build the project.</span></span> <span data-ttu-id="344cd-189">Poniżej jest wyświetlany w oknie danych wyjściowych:</span><span class="sxs-lookup"><span data-stu-id="344cd-189">The following appears in the Output window:</span></span>

```console
1>------ Build started: Project: BuildBundlerMinifierApp, Configuration: Debug Any CPU ------
1>
1>Bundler: Begin processing bundleconfig.json
1>  Minified wwwroot/css/site.min.css
1>  Minified wwwroot/js/site.min.js
1>Bundler: Done processing bundleconfig.json
1>BuildBundlerMinifierApp -> C:\BuildBundlerMinifierApp\bin\Debug\netcoreapp2.0\BuildBundlerMinifierApp.dll
========== Build: 1 succeeded, 0 failed, 0 up-to-date, 0 skipped ==========
```

<span data-ttu-id="344cd-190">Czyszczenie projektu.</span><span class="sxs-lookup"><span data-stu-id="344cd-190">Clean the project.</span></span> <span data-ttu-id="344cd-191">Poniżej jest wyświetlany w oknie danych wyjściowych:</span><span class="sxs-lookup"><span data-stu-id="344cd-191">The following appears in the Output window:</span></span>

```console
1>------ Clean started: Project: BuildBundlerMinifierApp, Configuration: Debug Any CPU ------
1>
1>Bundler: Cleaning output from bundleconfig.json
1>Bundler: Done cleaning output file from bundleconfig.json
========== Clean: 1 succeeded, 0 failed, 0 skipped ==========
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="344cd-192">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="344cd-192">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="344cd-193">Dodaj *BuildBundlerMinifier* pakietu do projektu:</span><span class="sxs-lookup"><span data-stu-id="344cd-193">Add the *BuildBundlerMinifier* package to your project:</span></span>

```console
dotnet add package BuildBundlerMinifier
```

<span data-ttu-id="344cd-194">Jeśli przy użyciu platformy ASP.NET Core 1.x, Przywróć nowo dodany pakiet:</span><span class="sxs-lookup"><span data-stu-id="344cd-194">If using ASP.NET Core 1.x, restore the newly added package:</span></span>

```console
dotnet restore
```

<span data-ttu-id="344cd-195">Skompiluj projekt:</span><span class="sxs-lookup"><span data-stu-id="344cd-195">Build the project:</span></span>

```console
dotnet build
```

<span data-ttu-id="344cd-196">Pojawia się poniżej:</span><span class="sxs-lookup"><span data-stu-id="344cd-196">The following appears:</span></span>

```console
Microsoft (R) Build Engine version 15.4.8.50001 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.


    Bundler: Begin processing bundleconfig.json
    Bundler: Done processing bundleconfig.json
    BuildBundlerMinifierApp -> C:\BuildBundlerMinifierApp\bin\Debug\netcoreapp2.0\BuildBundlerMinifierApp.dll
```

<span data-ttu-id="344cd-197">Czyszczenie projektu:</span><span class="sxs-lookup"><span data-stu-id="344cd-197">Clean the project:</span></span>

```console
dotnet clean
```

<span data-ttu-id="344cd-198">Zostaną wyświetlone następujące dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="344cd-198">The following output appears:</span></span>

```console
Microsoft (R) Build Engine version 15.4.8.50001 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.


  Bundler: Cleaning output from bundleconfig.json
  Bundler: Done cleaning output file from bundleconfig.json
```

---

## <a name="ad-hoc-execution-of-bundling-and-minification"></a><span data-ttu-id="344cd-199">Ad hoc wykonywanie tworzenie pakietów i minimalizowanie</span><span class="sxs-lookup"><span data-stu-id="344cd-199">Ad hoc execution of bundling and minification</span></span>

<span data-ttu-id="344cd-200">Istnieje możliwość uruchomienia zadania Tworzenie pakietów i minimalizowanie na zasadzie ad hoc bez skompilowanie projektu.</span><span class="sxs-lookup"><span data-stu-id="344cd-200">It's possible to run the bundling and minification tasks on an ad hoc basis, without building the project.</span></span> <span data-ttu-id="344cd-201">Dodaj [BundlerMinifier.Core](https://www.nuget.org/packages/BundlerMinifier.Core/) pakiet NuGet do projektu:</span><span class="sxs-lookup"><span data-stu-id="344cd-201">Add the [BundlerMinifier.Core](https://www.nuget.org/packages/BundlerMinifier.Core/) NuGet package to your project:</span></span>

[!code-xml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/BuildBundlerMinifierApp.csproj?range=10)]

> [!NOTE]
> <span data-ttu-id="344cd-202">BundlerMinifier.Core należy do projektu społeczność w witrynie GitHub, dla którego Microsoft nie zapewnia obsługi.</span><span class="sxs-lookup"><span data-stu-id="344cd-202">BundlerMinifier.Core belongs to a community-driven project on GitHub for which Microsoft provides no support.</span></span> <span data-ttu-id="344cd-203">Powinny być zgłaszane problemy [tutaj](https://github.com/madskristensen/BundlerMinifier/issues).</span><span class="sxs-lookup"><span data-stu-id="344cd-203">Issues should be filed [here](https://github.com/madskristensen/BundlerMinifier/issues).</span></span>

<span data-ttu-id="344cd-204">Ten pakiet rozszerza .NET Core interfejsu wiersza polecenia do uwzględnienia *pakietu dotnet* narzędzia.</span><span class="sxs-lookup"><span data-stu-id="344cd-204">This package extends the .NET Core CLI to include the *dotnet-bundle* tool.</span></span> <span data-ttu-id="344cd-205">Polecenie można wykonać w oknie konsoli Menedżera pakietów (PMC) lub w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="344cd-205">The following command can be executed in the Package Manager Console (PMC) window or in a command shell:</span></span>

```console
dotnet bundle
```

> [!IMPORTANT]
> <span data-ttu-id="344cd-206">Menedżer pakietów NuGet dodaje zależności do pliku \*.csproj jako `<PackageReference />` węzłów.</span><span class="sxs-lookup"><span data-stu-id="344cd-206">NuGet Package Manager adds dependencies to the \*.csproj file as `<PackageReference />` nodes.</span></span> <span data-ttu-id="344cd-207">`dotnet bundle` Polecenia został zarejestrowany za pomocą interfejsu wiersza polecenia platformy .NET Core tylko wtedy, gdy `<DotNetCliToolReference />` węzła jest używany.</span><span class="sxs-lookup"><span data-stu-id="344cd-207">The `dotnet bundle` command is registered with the .NET Core CLI only when a `<DotNetCliToolReference />` node is used.</span></span> <span data-ttu-id="344cd-208">Zmodyfikuj odpowiednio \*.csproj pliku.</span><span class="sxs-lookup"><span data-stu-id="344cd-208">Modify the \*.csproj file accordingly.</span></span>

## <a name="add-files-to-workflow"></a><span data-ttu-id="344cd-209">Dodaj pliki do przepływu pracy</span><span class="sxs-lookup"><span data-stu-id="344cd-209">Add files to workflow</span></span>

<span data-ttu-id="344cd-210">Rozważ przykład, w którym dodatkowe *custome.CSS* zostanie dodany plik podobne do następujących:</span><span class="sxs-lookup"><span data-stu-id="344cd-210">Consider an example in which an additional *custom.css* file is added resembling the following:</span></span>

[!code-css[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/css/custom.css)]

<span data-ttu-id="344cd-211">Do zminimalizowania *custome.CSS* i połączyć ją z *site.css* do *site.min.css* plików, Dodaj ścieżkę względną do *bundleconfig.json*:</span><span class="sxs-lookup"><span data-stu-id="344cd-211">To minify *custom.css* and bundle it with *site.css* into a *site.min.css* file, add the relative path to *bundleconfig.json*:</span></span>

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig2.json?highlight=6)]

> [!NOTE]
> <span data-ttu-id="344cd-212">Alternatywnie można użyć następującego wzorca globbing:</span><span class="sxs-lookup"><span data-stu-id="344cd-212">Alternatively, the following globbing pattern could be used:</span></span>
>
> ```json
> "inputFiles": ["wwwroot/**/*(*.css|!(*.min.css)"]
> ```
>
> <span data-ttu-id="344cd-213">Ten wzorzec globbing dopasowanie wszystkich plików CSS i wyklucza wzorzec pliku zminimalizowany.</span><span class="sxs-lookup"><span data-stu-id="344cd-213">This globbing pattern matches all CSS files and excludes the minified file pattern.</span></span>

<span data-ttu-id="344cd-214">Tworzenie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="344cd-214">Build the application.</span></span> <span data-ttu-id="344cd-215">Otwórz *site.min.css* i zwróć uwagę, zawartość *custome.CSS* jest dołączany na końcu pliku.</span><span class="sxs-lookup"><span data-stu-id="344cd-215">Open *site.min.css* and notice the content of *custom.css* is appended to the end of the file.</span></span>

## <a name="environment-based-bundling-and-minification"></a><span data-ttu-id="344cd-216">Oparte na środowisku tworzenie pakietów i minimalizowanie</span><span class="sxs-lookup"><span data-stu-id="344cd-216">Environment-based bundling and minification</span></span>

<span data-ttu-id="344cd-217">Najlepszym rozwiązaniem pliki powiązane i zminimalizowany aplikacji używanego w środowisku produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="344cd-217">As a best practice, the bundled and minified files of your app should be used in a production environment.</span></span> <span data-ttu-id="344cd-218">Podczas tworzenia oryginalnych plików upewnij się łatwiejsze debugowania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="344cd-218">During development, the original files make for easier debugging of the app.</span></span>

<span data-ttu-id="344cd-219">Określ pliki, których ma obejmować na swoich stronach za pomocą [pomocnika Tag środowiska](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) w widoków.</span><span class="sxs-lookup"><span data-stu-id="344cd-219">Specify which files to include in your pages by using the [Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) in your views.</span></span> <span data-ttu-id="344cd-220">Pomocnik Tag środowiska renderuje tylko jego zawartość podczas uruchamiania w określonej [środowisk](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="344cd-220">The Environment Tag Helper only renders its contents when running in specific [environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="344cd-221">Następujące `environment` tag renderuje nieprzetworzone pliki CSS w `Development` środowiska:</span><span class="sxs-lookup"><span data-stu-id="344cd-221">The following `environment` tag renders the unprocessed CSS files when running in the `Development` environment:</span></span>

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="344cd-222">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="344cd-222">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)
[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=21-24)]

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="344cd-223">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="344cd-223">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)
[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=9-12)]

* * *
<span data-ttu-id="344cd-224">Następujące `environment` tag renderuje pliki CSS powiązane i zminimalizowany podczas uruchamiania w środowisku innym niż `Development`.</span><span class="sxs-lookup"><span data-stu-id="344cd-224">The following `environment` tag renders the bundled and minified CSS files when running in an environment other than `Development`.</span></span> <span data-ttu-id="344cd-225">Na przykład uruchomiona `Production` lub `Staging` wyzwala renderowania tych arkuszy stylów:</span><span class="sxs-lookup"><span data-stu-id="344cd-225">For example, running in `Production` or `Staging` triggers the rendering of these stylesheets:</span></span>

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="344cd-226">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="344cd-226">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)
[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=5&range=25-30)]

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="344cd-227">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="344cd-227">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)
[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=13-18)]

* * *
## <a name="consume-bundleconfigjson-from-gulp"></a><span data-ttu-id="344cd-228">Korzystanie z bundleconfig.json z Gulp</span><span class="sxs-lookup"><span data-stu-id="344cd-228">Consume bundleconfig.json from Gulp</span></span>

<span data-ttu-id="344cd-229">Istnieją przypadki, w których aplikacja tworzenie pakietów i minimalizowanie przepływ pracy wymaga dodatkowego przetwarzania.</span><span class="sxs-lookup"><span data-stu-id="344cd-229">There are cases in which an app's bundling and minification workflow requires additional processing.</span></span> <span data-ttu-id="344cd-230">Przykładami Optymalizacja obrazu, rozrywające pamięci podręcznej i przetwarzania zasobów w sieci CDN.</span><span class="sxs-lookup"><span data-stu-id="344cd-230">Examples include image optimization, cache busting, and CDN asset processing.</span></span> <span data-ttu-id="344cd-231">Aby spełnić te wymagania, można przekonwertować tworzenie pakietów i minimalizowanie przepływu pracy Użyj Gulp.</span><span class="sxs-lookup"><span data-stu-id="344cd-231">To satisfy these requirements, you can convert the bundling and minification workflow to use Gulp.</span></span>

### <a name="use-the-bundler--minifier-extension"></a><span data-ttu-id="344cd-232">Użyj rozszerzenia program instalujący niezamówione pakiety & element minimalizujący</span><span class="sxs-lookup"><span data-stu-id="344cd-232">Use the Bundler & Minifier extension</span></span>

<span data-ttu-id="344cd-233">Visual Studio [program instalujący niezamówione pakiety & element minimalizujący](https://marketplace.visualstudio.com/items?itemName=MadsKristensen.BundlerMinifier) rozszerzenie obsługuje konwersję Gulp.</span><span class="sxs-lookup"><span data-stu-id="344cd-233">The Visual Studio [Bundler & Minifier](https://marketplace.visualstudio.com/items?itemName=MadsKristensen.BundlerMinifier) extension handles the conversion to Gulp.</span></span>

> [!NOTE]
> <span data-ttu-id="344cd-234">Program instalujący niezamówione pakiety & element minimalizujący rozszerzenia należy do projektu społeczność w witrynie GitHub, dla którego Microsoft nie zapewnia obsługi.</span><span class="sxs-lookup"><span data-stu-id="344cd-234">The Bundler & Minifier extension belongs to a community-driven project on GitHub for which Microsoft provides no support.</span></span> <span data-ttu-id="344cd-235">Powinny być zgłaszane problemy [tutaj](https://github.com/madskristensen/BundlerMinifier/issues).</span><span class="sxs-lookup"><span data-stu-id="344cd-235">Issues should be filed [here](https://github.com/madskristensen/BundlerMinifier/issues).</span></span>

<span data-ttu-id="344cd-236">Kliknij prawym przyciskiem myszy *bundleconfig.json* plików w Eksploratorze rozwiązań i wybierz **program instalujący niezamówione pakiety & element minimalizujący** > **Konwertuj do system Gulp...** :</span><span class="sxs-lookup"><span data-stu-id="344cd-236">Right-click the *bundleconfig.json* file in Solution Explorer and select **Bundler & Minifier** > **Convert To Gulp...**:</span></span>

![Konwertuj na system Gulp w menu kontekstowym](../client-side/bundling-and-minification/_static/convert-to-gulp.png)

<span data-ttu-id="344cd-238">*Gulpfile.js* i *package.json* pliki zostaną dodane do projektu.</span><span class="sxs-lookup"><span data-stu-id="344cd-238">The *gulpfile.js* and *package.json* files are added to the project.</span></span> <span data-ttu-id="344cd-239">Obsługa [npm](https://www.npmjs.com/) pakiety wymienione w *package.json* pliku `devDependencies` sekcji są zainstalowane.</span><span class="sxs-lookup"><span data-stu-id="344cd-239">The supporting [npm](https://www.npmjs.com/) packages listed in the *package.json* file's `devDependencies` section are installed.</span></span>

<span data-ttu-id="344cd-240">Uruchom następujące polecenie w oknie PMC, aby zainstalować system Gulp CLI jako zależność globalne:</span><span class="sxs-lookup"><span data-stu-id="344cd-240">Run the following command in the PMC window to install the Gulp CLI as a global dependency:</span></span>

```console
npm i -g gulp-cli
```

<span data-ttu-id="344cd-241">*Gulpfile.js* pliku odczyty *bundleconfig.json* wejść, wyjść i ustawienia w pliku.</span><span class="sxs-lookup"><span data-stu-id="344cd-241">The *gulpfile.js* file reads the *bundleconfig.json* file for the inputs, outputs, and settings.</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-12&highlight=10)]

### <a name="convert-manually"></a><span data-ttu-id="344cd-242">Ręcznie konwertowanie</span><span class="sxs-lookup"><span data-stu-id="344cd-242">Convert manually</span></span>

<span data-ttu-id="344cd-243">Jeśli program Visual Studio i/lub program instalujący niezamówione pakiety & element minimalizujący rozszerzenia nie są dostępne, przekonwertować ręcznie.</span><span class="sxs-lookup"><span data-stu-id="344cd-243">If Visual Studio and/or the Bundler & Minifier extension aren't available, convert manually.</span></span>

<span data-ttu-id="344cd-244">Dodaj *package.json* pliku następującym kodem `devDependencies`, katalog główny projektu:</span><span class="sxs-lookup"><span data-stu-id="344cd-244">Add a *package.json* file, with the following `devDependencies`, to the project root:</span></span>

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/package.json?range=5-13)]

<span data-ttu-id="344cd-245">Zainstaluj zależności, uruchamiając następujące polecenie na tym samym poziomie jako *package.json*:</span><span class="sxs-lookup"><span data-stu-id="344cd-245">Install the dependencies by running the following command at the same level as *package.json*:</span></span>

```console
npm i
```

<span data-ttu-id="344cd-246">Zainstaluj interfejs wiersza polecenia system Gulp jako zależność globalne:</span><span class="sxs-lookup"><span data-stu-id="344cd-246">Install the Gulp CLI as a global dependency:</span></span>

```console
npm i -g gulp-cli
```

<span data-ttu-id="344cd-247">Kopiuj *gulpfile.js* pliku poniżej w katalogu głównym projektu:</span><span class="sxs-lookup"><span data-stu-id="344cd-247">Copy the *gulpfile.js* file below to the project root:</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-11,14-)]

### <a name="run-gulp-tasks"></a><span data-ttu-id="344cd-248">Uruchom zadania Gulp</span><span class="sxs-lookup"><span data-stu-id="344cd-248">Run Gulp tasks</span></span>

<span data-ttu-id="344cd-249">Aby wyzwolić zadanie minimalizowanie Gulp przed kompilacji projektu programu Visual Studio, Dodaj następujący [docelowy programu MSBuild](/visualstudio/msbuild/msbuild-targets) do pliku \*.csproj:</span><span class="sxs-lookup"><span data-stu-id="344cd-249">To trigger the Gulp minification task before the project builds in Visual Studio, add the following [MSBuild Target](/visualstudio/msbuild/msbuild-targets) to the \*.csproj file:</span></span>

[!code-xml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/BuildBundlerMinifierApp.csproj?range=14-16)]

<span data-ttu-id="344cd-250">W tym przykładzie wszystkie zadania zdefiniowany w ramach `MyPreCompileTarget` docelowych uruchomienia przed wstępnie zdefiniowane `Build` docelowej.</span><span class="sxs-lookup"><span data-stu-id="344cd-250">In this example, any tasks defined within the `MyPreCompileTarget` target run before the predefined `Build` target.</span></span> <span data-ttu-id="344cd-251">W oknie danych wyjściowych programu Visual Studio zostaną wyświetlone dane wyjściowe podobne do następujących:</span><span class="sxs-lookup"><span data-stu-id="344cd-251">Output similar to the following appears in Visual Studio's Output window:</span></span>

```console
1>------ Build started: Project: BuildBundlerMinifierApp, Configuration: Debug Any CPU ------
1>BuildBundlerMinifierApp -> C:\BuildBundlerMinifierApp\bin\Debug\netcoreapp2.0\BuildBundlerMinifierApp.dll
1>[14:17:49] Using gulpfile C:\BuildBundlerMinifierApp\gulpfile.js
1>[14:17:49] Starting 'min:js'...
1>[14:17:49] Starting 'min:css'...
1>[14:17:49] Starting 'min:html'...
1>[14:17:49] Finished 'min:js' after 83 ms
1>[14:17:49] Finished 'min:css' after 88 ms
========== Build: 1 succeeded, 0 failed, 0 up-to-date, 0 skipped ==========
```

<span data-ttu-id="344cd-252">Alternatywnie Eksploratora modułu uruchamiającego zadania programu Visual Studio mogą służyć do powiązania zadania Gulp określonych zdarzeń programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="344cd-252">Alternatively, Visual Studio's Task Runner Explorer may be used to bind Gulp tasks to specific Visual Studio events.</span></span> <span data-ttu-id="344cd-253">Zobacz [uruchomionych zadań domyślne](xref:client-side/using-gulp#running-default-tasks) instrukcje dotyczące operacją.</span><span class="sxs-lookup"><span data-stu-id="344cd-253">See [Running default tasks](xref:client-side/using-gulp#running-default-tasks) for instructions on doing that.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="344cd-254">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="344cd-254">Additional resources</span></span>

* [<span data-ttu-id="344cd-255">Korzystanie z Gulp</span><span class="sxs-lookup"><span data-stu-id="344cd-255">Use Gulp</span></span>](xref:client-side/using-gulp)
* [<span data-ttu-id="344cd-256">Korzystanie z Grunt</span><span class="sxs-lookup"><span data-stu-id="344cd-256">Use Grunt</span></span>](xref:client-side/using-grunt)
* [<span data-ttu-id="344cd-257">Użyj wiele środowisk</span><span class="sxs-lookup"><span data-stu-id="344cd-257">Use multiple environments</span></span>](xref:fundamentals/environments)
* [<span data-ttu-id="344cd-258">Pomocnicy tagów</span><span class="sxs-lookup"><span data-stu-id="344cd-258">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
