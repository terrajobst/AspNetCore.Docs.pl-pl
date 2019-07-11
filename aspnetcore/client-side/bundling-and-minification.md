---
title: Tworzenie pakietów i minimalizowanie statycznych zasobów w programie ASP.NET Core
author: scottaddie
description: Dowiedz się, jak zoptymalizować zasoby statyczne w aplikacji sieci web platformy ASP.NET Core za pomocą metod minifikacji i tworzenia pakietów.
ms.author: scaddie
ms.custom: mvc
ms.date: 06/17/2019
uid: client-side/bundling-and-minification
ms.openlocfilehash: 6254a74fd0a11669706a2a89b156a3223e300d1c
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67813496"
---
# <a name="bundle-and-minify-static-assets-in-aspnet-core"></a><span data-ttu-id="60c9c-103">Tworzenie pakietów i minimalizowanie statycznych zasobów w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="60c9c-103">Bundle and minify static assets in ASP.NET Core</span></span>

<span data-ttu-id="60c9c-104">Przez [Scott Addie](https://twitter.com/Scott_Addie) i [sosny David](https://twitter.com/davidpine7)</span><span class="sxs-lookup"><span data-stu-id="60c9c-104">By [Scott Addie](https://twitter.com/Scott_Addie) and [David Pine](https://twitter.com/davidpine7)</span></span>

<span data-ttu-id="60c9c-105">W tym artykule wyjaśniono zalety stosowania tworzenie pakietów i minimalizowanie, w tym, jak można użyć tych funkcji w aplikacji sieci web platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="60c9c-105">This article explains the benefits of applying bundling and minification, including how these features can be used with ASP.NET Core web apps.</span></span>

## <a name="what-is-bundling-and-minification"></a><span data-ttu-id="60c9c-106">Co to jest tworzenie pakietów i minimalizowanie</span><span class="sxs-lookup"><span data-stu-id="60c9c-106">What is bundling and minification</span></span>

<span data-ttu-id="60c9c-107">Tworzenie pakietów i minimalizowanie są dwa optymalizacji wydajności distinct, które można zastosować w aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="60c9c-107">Bundling and minification are two distinct performance optimizations you can apply in a web app.</span></span> <span data-ttu-id="60c9c-108">Przy stosowaniu, tworzenie pakietów i minimalizowanie zwiększyć wydajność przez zmniejszenie liczby żądań serwera oraz redukcję rozmiaru żądanych zasobów statycznych.</span><span class="sxs-lookup"><span data-stu-id="60c9c-108">Used together, bundling and minification improve performance by reducing the number of server requests and reducing the size of the requested static assets.</span></span>

<span data-ttu-id="60c9c-109">Tworzenie pakietów i minimalizowanie przede wszystkim poprawić strony żądań obciążenia po raz pierwszy.</span><span class="sxs-lookup"><span data-stu-id="60c9c-109">Bundling and minification primarily improve the first page request load time.</span></span> <span data-ttu-id="60c9c-110">Gdy wysłano żądanie strony sieci web, przeglądarka buforuje statycznych zasobów (JavaScript, CSS i obrazów).</span><span class="sxs-lookup"><span data-stu-id="60c9c-110">Once a web page has been requested, the browser caches the static assets (JavaScript, CSS, and images).</span></span> <span data-ttu-id="60c9c-111">W związku z tym tworzenie pakietów i minimalizowanie nie poprawić wydajność podczas żądania tej samej stronie lub stron, w tej samej lokacji żądania tych samych zasobów.</span><span class="sxs-lookup"><span data-stu-id="60c9c-111">Consequently, bundling and minification don't improve performance when requesting the same page, or pages, on the same site requesting the same assets.</span></span> <span data-ttu-id="60c9c-112">Jeśli wygasa nagłówka nie jest prawidłowo ustawione na zasoby i tworzenie pakietów i minimalizowanie nie jest używany algorytm heurystyczny świeżości przeglądarki oznaczyć zasoby starych po upływie kilku dni.</span><span class="sxs-lookup"><span data-stu-id="60c9c-112">If the expires header isn't set correctly on the assets and if bundling and minification isn't used, the browser's freshness heuristics mark the assets stale after a few days.</span></span> <span data-ttu-id="60c9c-113">Ponadto przeglądarka wymaga żądanie weryfikacji dla każdego zasobu.</span><span class="sxs-lookup"><span data-stu-id="60c9c-113">Additionally, the browser requires a validation request for each asset.</span></span> <span data-ttu-id="60c9c-114">W tym przypadku tworzenie pakietów i minimalizowanie zapewniają lepszą wydajność, nawet po pierwszym żądaniu strony.</span><span class="sxs-lookup"><span data-stu-id="60c9c-114">In this case, bundling and minification provide a performance improvement even after the first page request.</span></span>

### <a name="bundling"></a><span data-ttu-id="60c9c-115">Tworzenie pakietów</span><span class="sxs-lookup"><span data-stu-id="60c9c-115">Bundling</span></span>

<span data-ttu-id="60c9c-116">Tworzenie pakietów pozwala łączyć wiele plików w jednym pliku.</span><span class="sxs-lookup"><span data-stu-id="60c9c-116">Bundling combines multiple files into a single file.</span></span> <span data-ttu-id="60c9c-117">Tworzenie pakietów pozwala zmniejszyć liczbę żądań serwera, które są niezbędne do renderowania elementu zawartości sieci web, takich jak strony sieci web.</span><span class="sxs-lookup"><span data-stu-id="60c9c-117">Bundling reduces the number of server requests that are necessary to render a web asset, such as a web page.</span></span> <span data-ttu-id="60c9c-118">Można utworzyć dowolną liczbę poszczególnych pakietów specjalnie dla CSS, JavaScript, itp. Mniej plików oznacza mniejszą liczbę żądań HTTP z przeglądarki do serwera lub usługę, zapewniając aplikacji.</span><span class="sxs-lookup"><span data-stu-id="60c9c-118">You can create any number of individual bundles specifically for CSS, JavaScript, etc. Fewer files means fewer HTTP requests from the browser to the server or from the service providing your application.</span></span> <span data-ttu-id="60c9c-119">Powoduje to zwiększona wydajność pod obciążeniem pierwszej strony.</span><span class="sxs-lookup"><span data-stu-id="60c9c-119">This results in improved first page load performance.</span></span>

### <a name="minification"></a><span data-ttu-id="60c9c-120">Minimalizowanie</span><span class="sxs-lookup"><span data-stu-id="60c9c-120">Minification</span></span>

<span data-ttu-id="60c9c-121">Minifikacja umożliwia usunięcie niepotrzebnych znaków z kodu, bez zmiany funkcjonalności.</span><span class="sxs-lookup"><span data-stu-id="60c9c-121">Minification removes unnecessary characters from code without altering functionality.</span></span> <span data-ttu-id="60c9c-122">Wynik jest zmniejszenie o znacznym rozmiarze żądanych zasobów (takich jak CSS, obrazów i plików JavaScript).</span><span class="sxs-lookup"><span data-stu-id="60c9c-122">The result is a significant size reduction in requested assets (such as CSS, images, and JavaScript files).</span></span> <span data-ttu-id="60c9c-123">Typowe efekty uboczne minimalizację obejmują zmniejszenie nazwy zmiennych, jeden znak i usuwanie komentarzy i niepotrzebnych odstępów.</span><span class="sxs-lookup"><span data-stu-id="60c9c-123">Common side effects of minification include shortening variable names to one character and removing comments and unnecessary whitespace.</span></span>

<span data-ttu-id="60c9c-124">Należy wziąć pod uwagę następujące funkcja języka JavaScript:</span><span class="sxs-lookup"><span data-stu-id="60c9c-124">Consider the following JavaScript function:</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.js)]

<span data-ttu-id="60c9c-125">Minimalizowanie zmniejsza funkcji do następujących:</span><span class="sxs-lookup"><span data-stu-id="60c9c-125">Minification reduces the function to the following:</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.min.js)]

<span data-ttu-id="60c9c-126">Oprócz usuwanie komentarzy i niepotrzebnych odstępów, następujące nazwy parametrów i zmiennych zostały zmienione w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="60c9c-126">In addition to removing the comments and unnecessary whitespace, the following parameter and variable names were renamed as follows:</span></span>

<span data-ttu-id="60c9c-127">Oryginał</span><span class="sxs-lookup"><span data-stu-id="60c9c-127">Original</span></span> | <span data-ttu-id="60c9c-128">Zmieniono nazwę</span><span class="sxs-lookup"><span data-stu-id="60c9c-128">Renamed</span></span>
--- | :---:
`imageTagAndImageID` | `t`
`imageContext` | `a`
`imageElement` | `r`

## <a name="impact-of-bundling-and-minification"></a><span data-ttu-id="60c9c-129">Wpływ tworzenie pakietów i minimalizowanie</span><span class="sxs-lookup"><span data-stu-id="60c9c-129">Impact of bundling and minification</span></span>

<span data-ttu-id="60c9c-130">W poniższej tabeli przedstawiono różnice między indywidualnie zasoby oraz za pomocą tworzenie pakietów i minimalizowanie:</span><span class="sxs-lookup"><span data-stu-id="60c9c-130">The following table outlines differences between individually loading assets and using bundling and minification:</span></span>

<span data-ttu-id="60c9c-131">Akcja</span><span class="sxs-lookup"><span data-stu-id="60c9c-131">Action</span></span> | <span data-ttu-id="60c9c-132">Za pomocą B/M</span><span class="sxs-lookup"><span data-stu-id="60c9c-132">With B/M</span></span> | <span data-ttu-id="60c9c-133">Bez B/M</span><span class="sxs-lookup"><span data-stu-id="60c9c-133">Without B/M</span></span> | <span data-ttu-id="60c9c-134">Zmiana</span><span class="sxs-lookup"><span data-stu-id="60c9c-134">Change</span></span>
--- | :---: | :---: | :---:
<span data-ttu-id="60c9c-135">Żądań plików</span><span class="sxs-lookup"><span data-stu-id="60c9c-135">File Requests</span></span>  | <span data-ttu-id="60c9c-136">7</span><span class="sxs-lookup"><span data-stu-id="60c9c-136">7</span></span>   | <span data-ttu-id="60c9c-137">18</span><span class="sxs-lookup"><span data-stu-id="60c9c-137">18</span></span>     | <span data-ttu-id="60c9c-138">157%</span><span class="sxs-lookup"><span data-stu-id="60c9c-138">157%</span></span>
<span data-ttu-id="60c9c-139">KB przesłane</span><span class="sxs-lookup"><span data-stu-id="60c9c-139">KB Transferred</span></span> | <span data-ttu-id="60c9c-140">156</span><span class="sxs-lookup"><span data-stu-id="60c9c-140">156</span></span> | <span data-ttu-id="60c9c-141">264.68</span><span class="sxs-lookup"><span data-stu-id="60c9c-141">264.68</span></span> | <span data-ttu-id="60c9c-142">70%</span><span class="sxs-lookup"><span data-stu-id="60c9c-142">70%</span></span>
<span data-ttu-id="60c9c-143">Czas ładowania (ms)</span><span class="sxs-lookup"><span data-stu-id="60c9c-143">Load Time (ms)</span></span> | <span data-ttu-id="60c9c-144">885</span><span class="sxs-lookup"><span data-stu-id="60c9c-144">885</span></span> | <span data-ttu-id="60c9c-145">2360</span><span class="sxs-lookup"><span data-stu-id="60c9c-145">2360</span></span>   | <span data-ttu-id="60c9c-146">167%</span><span class="sxs-lookup"><span data-stu-id="60c9c-146">167%</span></span>

<span data-ttu-id="60c9c-147">Przeglądarki są dość pełne w odniesieniu do nagłówków żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="60c9c-147">Browsers are fairly verbose with regard to HTTP request headers.</span></span> <span data-ttu-id="60c9c-148">Całkowita liczba bajtów wysłano metryki dostrzegła znaczące zmniejszenie podczas tworzenia pakietów.</span><span class="sxs-lookup"><span data-stu-id="60c9c-148">The total bytes sent metric saw a significant reduction when bundling.</span></span> <span data-ttu-id="60c9c-149">Czas ładowania pokazuje wprowadzono znaczące ulepszenia, jednak w tym przykładzie został uruchomiony lokalnie.</span><span class="sxs-lookup"><span data-stu-id="60c9c-149">The load time shows a significant improvement, however this example ran locally.</span></span> <span data-ttu-id="60c9c-150">Większa wzrost wydajności są realizowane przy użyciu tworzenie pakietów i minimalizowanie z zasobami przesyłana przez sieć.</span><span class="sxs-lookup"><span data-stu-id="60c9c-150">Greater performance gains are realized when using bundling and minification with assets transferred over a network.</span></span>

## <a name="choose-a-bundling-and-minification-strategy"></a><span data-ttu-id="60c9c-151">Wybierz strategię tworzenie pakietów i minimalizowanie</span><span class="sxs-lookup"><span data-stu-id="60c9c-151">Choose a bundling and minification strategy</span></span>

<span data-ttu-id="60c9c-152">Szablony projektów MVC i stron Razor zapewniają rozwiązanie poza pole do tworzenie pakietów i minimalizowanie składający się z pliku konfiguracji JSON.</span><span class="sxs-lookup"><span data-stu-id="60c9c-152">The MVC and Razor Pages project templates provide an out-of-the-box solution for bundling and minification consisting of a JSON configuration file.</span></span> <span data-ttu-id="60c9c-153">Narzędzia innych firm, takich jak [Grunt](xref:client-side/using-grunt) task runner, wykonać te same zadania przy użyciu bardziej złożoności.</span><span class="sxs-lookup"><span data-stu-id="60c9c-153">Third-party tools, such as the [Grunt](xref:client-side/using-grunt) task runner, accomplish the same tasks with a bit more complexity.</span></span> <span data-ttu-id="60c9c-154">Narzędzia innych firm jest doskonałym rozwiązaniem, gdy przepływie pracy wymaga przetwarzania poza tworzenie pakietów i minimalizowanie&mdash;takich jak Optymalizacja Zaznaczanie błędów i obrazów.</span><span class="sxs-lookup"><span data-stu-id="60c9c-154">A third-party tool is a great fit when your development workflow requires processing beyond bundling and minification&mdash;such as linting and image optimization.</span></span> <span data-ttu-id="60c9c-155">Za pomocą tworzenie pakietów i minimalizowanie czasu projektowania, zminimalizowany pliki zostaną utworzone przed wdrożeniem aplikacji.</span><span class="sxs-lookup"><span data-stu-id="60c9c-155">By using design-time bundling and minification, the minified files are created prior to the app's deployment.</span></span> <span data-ttu-id="60c9c-156">Tworzenie pakietów i minifikacja przed przystąpieniem do wdrożenia zawiera dzięki zmniejszeniu obciążenia.</span><span class="sxs-lookup"><span data-stu-id="60c9c-156">Bundling and minifying before deployment provides the advantage of reduced server load.</span></span> <span data-ttu-id="60c9c-157">Jednak ważne jest, aby rozpoznać tego czasu projektowania tworzenie pakietów i minimalizowanie zwiększa złożonością kompilacji i działa tylko z plikami statycznymi.</span><span class="sxs-lookup"><span data-stu-id="60c9c-157">However, it's important to recognize that design-time bundling and minification increases build complexity and only works with static files.</span></span>

## <a name="configure-bundling-and-minification"></a><span data-ttu-id="60c9c-158">Skonfiguruj tworzenie pakietów i minimalizowanie</span><span class="sxs-lookup"><span data-stu-id="60c9c-158">Configure bundling and minification</span></span>

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="60c9c-159">W programie ASP.NET Core 2.0 lub wcześniej, szablony projektu MVC i stron Razor zapewniają *bundleconfig.json* pliku konfiguracji, który definiuje opcje dla każdego pakietu:</span><span class="sxs-lookup"><span data-stu-id="60c9c-159">In ASP.NET Core 2.0 or earlier, the MVC and Razor Pages project templates provide a *bundleconfig.json* configuration file that defines the options for each bundle:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="60c9c-160">W programie ASP.NET Core 2.1 lub nowszej, Dodaj nowy plik JSON o nazwie *bundleconfig.json*, w katalogu głównym projektu MVC ani stron Razor.</span><span class="sxs-lookup"><span data-stu-id="60c9c-160">In ASP.NET Core 2.1 or later, add a new JSON file, named *bundleconfig.json*, to the MVC or Razor Pages project root.</span></span> <span data-ttu-id="60c9c-161">Dołącz następujące dane JSON do tego pliku jako punkt początkowy:</span><span class="sxs-lookup"><span data-stu-id="60c9c-161">Include the following JSON in that file as a starting point:</span></span>

::: moniker-end

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig.json)]

<span data-ttu-id="60c9c-162">*Bundleconfig.json* plik definiuje opcje dla każdego pakietu.</span><span class="sxs-lookup"><span data-stu-id="60c9c-162">The *bundleconfig.json* file defines the options for each bundle.</span></span> <span data-ttu-id="60c9c-163">W powyższym przykładzie konfiguracja pojedynczy pakiet jest zdefiniowana dla niestandardowych skryptów JavaScript (*wwwroot/js/site.js*) i arkusza stylów (*wwwroot/css/site.css*) plików.</span><span class="sxs-lookup"><span data-stu-id="60c9c-163">In the preceding example, a single bundle configuration is defined for the custom JavaScript (*wwwroot/js/site.js*) and stylesheet (*wwwroot/css/site.css*) files.</span></span>

<span data-ttu-id="60c9c-164">Opcje konfiguracji obejmują:</span><span class="sxs-lookup"><span data-stu-id="60c9c-164">Configuration options include:</span></span>

* <span data-ttu-id="60c9c-165">`outputFileName`: Nazwa pliku pakietu w danych wyjściowych.</span><span class="sxs-lookup"><span data-stu-id="60c9c-165">`outputFileName`: The name of the bundle file to output.</span></span> <span data-ttu-id="60c9c-166">Może zawierać ścieżki względnej z *bundleconfig.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="60c9c-166">Can contain a relative path from the *bundleconfig.json* file.</span></span> <span data-ttu-id="60c9c-167">**Wymagane**</span><span class="sxs-lookup"><span data-stu-id="60c9c-167">**required**</span></span>
* <span data-ttu-id="60c9c-168">`inputFiles`: Tablica plików, aby powiązać razem.</span><span class="sxs-lookup"><span data-stu-id="60c9c-168">`inputFiles`: An array of files to bundle together.</span></span> <span data-ttu-id="60c9c-169">To są ścieżki względne do pliku konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="60c9c-169">These are relative paths to the configuration file.</span></span> <span data-ttu-id="60c9c-170">**Opcjonalnie**, \* wartość pustą wyniki w pliku wyjściowym puste.</span><span class="sxs-lookup"><span data-stu-id="60c9c-170">**optional**, \*an empty value results in an empty output file.</span></span> <span data-ttu-id="60c9c-171">[symboli wieloznacznych](https://www.tldp.org/LDP/abs/html/globbingref.html) wzorce są obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="60c9c-171">[globbing](https://www.tldp.org/LDP/abs/html/globbingref.html) patterns are supported.</span></span>
* <span data-ttu-id="60c9c-172">`minify`: Minimalizowanie opcje typu danych wyjściowych.</span><span class="sxs-lookup"><span data-stu-id="60c9c-172">`minify`: The minification options for the output type.</span></span> <span data-ttu-id="60c9c-173">**Opcjonalnie**, *domyślnie — `minify: { enabled: true }`*</span><span class="sxs-lookup"><span data-stu-id="60c9c-173">**optional**, *default - `minify: { enabled: true }`*</span></span>
  * <span data-ttu-id="60c9c-174">Opcje konfiguracji są dostępne na typ pliku wyjściowego.</span><span class="sxs-lookup"><span data-stu-id="60c9c-174">Configuration options are available per output file type.</span></span>
    * [<span data-ttu-id="60c9c-175">Element minimalizujący CSS</span><span class="sxs-lookup"><span data-stu-id="60c9c-175">CSS Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki/cssminifier)
    * [<span data-ttu-id="60c9c-176">JavaScript Minifier</span><span class="sxs-lookup"><span data-stu-id="60c9c-176">JavaScript Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki/JavaScript-Minifier-settings)
    * [<span data-ttu-id="60c9c-177">Element minimalizujący HTML</span><span class="sxs-lookup"><span data-stu-id="60c9c-177">HTML Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki)
* <span data-ttu-id="60c9c-178">`includeInProject`: Flaga oznaczająca, czy należy dodać wygenerowanych plików do pliku projektu.</span><span class="sxs-lookup"><span data-stu-id="60c9c-178">`includeInProject`: Flag indicating whether to add generated files to project file.</span></span> <span data-ttu-id="60c9c-179">**Opcjonalnie**, *ustawienie domyślne — FAŁSZ.*</span><span class="sxs-lookup"><span data-stu-id="60c9c-179">**optional**, *default - false*</span></span>
* <span data-ttu-id="60c9c-180">`sourceMap`: Flaga wskazująca, czy mają zostać wygenerowane mapę źródłową, powiązane pliku.</span><span class="sxs-lookup"><span data-stu-id="60c9c-180">`sourceMap`: Flag indicating whether to generate a source map for the bundled file.</span></span> <span data-ttu-id="60c9c-181">**Opcjonalnie**, *ustawienie domyślne — FAŁSZ.*</span><span class="sxs-lookup"><span data-stu-id="60c9c-181">**optional**, *default - false*</span></span>
* <span data-ttu-id="60c9c-182">`sourceMapRootPath`: Ścieżka katalogu głównego do przechowywania pliku mapy wygenerowanego źródła.</span><span class="sxs-lookup"><span data-stu-id="60c9c-182">`sourceMapRootPath`: The root path for storing the generated source map file.</span></span>

## <a name="build-time-execution-of-bundling-and-minification"></a><span data-ttu-id="60c9c-183">Czas kompilacji wykonywanie tworzenie pakietów i minimalizowanie</span><span class="sxs-lookup"><span data-stu-id="60c9c-183">Build-time execution of bundling and minification</span></span>

<span data-ttu-id="60c9c-184">[BuildBundlerMinifier](https://www.nuget.org/packages/BuildBundlerMinifier/) pakietu NuGet umożliwia wykonywanie tworzenie pakietów i minimalizowanie w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="60c9c-184">The [BuildBundlerMinifier](https://www.nuget.org/packages/BuildBundlerMinifier/) NuGet package enables the execution of bundling and minification at build time.</span></span> <span data-ttu-id="60c9c-185">Wprowadza pakietu [elementów docelowych MSBuild](/visualstudio/msbuild/msbuild-targets) uruchamiania kompilacji i wyczyść godzinie.</span><span class="sxs-lookup"><span data-stu-id="60c9c-185">The package injects [MSBuild Targets](/visualstudio/msbuild/msbuild-targets) which run at build and clean time.</span></span> <span data-ttu-id="60c9c-186">*Bundleconfig.json* analizy pliku przez proces kompilacji, aby utworzyć pliki wyjściowe na podstawie zdefiniowanej konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="60c9c-186">The *bundleconfig.json* file is analyzed by the build process to produce the output files based on the defined configuration.</span></span>

> [!NOTE]
> <span data-ttu-id="60c9c-187">BuildBundlerMinifier należy do tworzonych metodą społecznościową projekt w witrynie GitHub, dla którego firmy Microsoft nie zapewnia obsługi.</span><span class="sxs-lookup"><span data-stu-id="60c9c-187">BuildBundlerMinifier belongs to a community-driven project on GitHub for which Microsoft provides no support.</span></span> <span data-ttu-id="60c9c-188">Powinny być zgłaszane problemy [tutaj](https://github.com/madskristensen/BundlerMinifier/issues).</span><span class="sxs-lookup"><span data-stu-id="60c9c-188">Issues should be filed [here](https://github.com/madskristensen/BundlerMinifier/issues).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="60c9c-189">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="60c9c-189">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="60c9c-190">Dodaj *BuildBundlerMinifier* pakietu do projektu.</span><span class="sxs-lookup"><span data-stu-id="60c9c-190">Add the *BuildBundlerMinifier* package to your project.</span></span>

<span data-ttu-id="60c9c-191">Skompiluj projekt.</span><span class="sxs-lookup"><span data-stu-id="60c9c-191">Build the project.</span></span> <span data-ttu-id="60c9c-192">Poniżej jest wyświetlany w oknie danych wyjściowych:</span><span class="sxs-lookup"><span data-stu-id="60c9c-192">The following appears in the Output window:</span></span>

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

<span data-ttu-id="60c9c-193">Czyszczenie projektu.</span><span class="sxs-lookup"><span data-stu-id="60c9c-193">Clean the project.</span></span> <span data-ttu-id="60c9c-194">Poniżej jest wyświetlany w oknie danych wyjściowych:</span><span class="sxs-lookup"><span data-stu-id="60c9c-194">The following appears in the Output window:</span></span>

```console
1>------ Clean started: Project: BuildBundlerMinifierApp, Configuration: Debug Any CPU ------
1>
1>Bundler: Cleaning output from bundleconfig.json
1>Bundler: Done cleaning output file from bundleconfig.json
========== Clean: 1 succeeded, 0 failed, 0 skipped ==========
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="60c9c-195">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="60c9c-195">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="60c9c-196">Dodaj *BuildBundlerMinifier* do projektu pakiet:</span><span class="sxs-lookup"><span data-stu-id="60c9c-196">Add the *BuildBundlerMinifier* package to your project:</span></span>

```console
dotnet add package BuildBundlerMinifier
```

<span data-ttu-id="60c9c-197">Jeśli za pomocą programu ASP.NET Core 1.x, Przywracanie pakietu nowo dodane:</span><span class="sxs-lookup"><span data-stu-id="60c9c-197">If using ASP.NET Core 1.x, restore the newly added package:</span></span>

```console
dotnet restore
```

<span data-ttu-id="60c9c-198">Skompilować projekt:</span><span class="sxs-lookup"><span data-stu-id="60c9c-198">Build the project:</span></span>

```console
dotnet build
```

<span data-ttu-id="60c9c-199">Zostanie wyświetlone następujące okno:</span><span class="sxs-lookup"><span data-stu-id="60c9c-199">The following appears:</span></span>

```console
Microsoft (R) Build Engine version 15.4.8.50001 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.


    Bundler: Begin processing bundleconfig.json
    Bundler: Done processing bundleconfig.json
    BuildBundlerMinifierApp -> C:\BuildBundlerMinifierApp\bin\Debug\netcoreapp2.0\BuildBundlerMinifierApp.dll
```

<span data-ttu-id="60c9c-200">Czyszczenie projektu:</span><span class="sxs-lookup"><span data-stu-id="60c9c-200">Clean the project:</span></span>

```console
dotnet clean
```

<span data-ttu-id="60c9c-201">Zostanie wyświetlone następujące dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="60c9c-201">The following output appears:</span></span>

```console
Microsoft (R) Build Engine version 15.4.8.50001 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.


  Bundler: Cleaning output from bundleconfig.json
  Bundler: Done cleaning output file from bundleconfig.json
```

---

## <a name="ad-hoc-execution-of-bundling-and-minification"></a><span data-ttu-id="60c9c-202">Ad hoc wykonywanie tworzenie pakietów i minimalizowanie</span><span class="sxs-lookup"><span data-stu-id="60c9c-202">Ad hoc execution of bundling and minification</span></span>

<span data-ttu-id="60c9c-203">Istnieje możliwość uruchamiania zadań tworzenie pakietów i minimalizowanie na zasadzie ad hoc, bez konieczności tworzenia projektu.</span><span class="sxs-lookup"><span data-stu-id="60c9c-203">It's possible to run the bundling and minification tasks on an ad hoc basis, without building the project.</span></span> <span data-ttu-id="60c9c-204">Dodaj [BundlerMinifier.Core](https://www.nuget.org/packages/BundlerMinifier.Core/) pakiet NuGet do projektu:</span><span class="sxs-lookup"><span data-stu-id="60c9c-204">Add the [BundlerMinifier.Core](https://www.nuget.org/packages/BundlerMinifier.Core/) NuGet package to your project:</span></span>

[!code-xml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/BuildBundlerMinifierApp.csproj?range=10)]

> [!NOTE]
> <span data-ttu-id="60c9c-205">BundlerMinifier.Core należy do tworzonych metodą społecznościową projekt w witrynie GitHub, dla którego firmy Microsoft nie zapewnia obsługi.</span><span class="sxs-lookup"><span data-stu-id="60c9c-205">BundlerMinifier.Core belongs to a community-driven project on GitHub for which Microsoft provides no support.</span></span> <span data-ttu-id="60c9c-206">Powinny być zgłaszane problemy [tutaj](https://github.com/madskristensen/BundlerMinifier/issues).</span><span class="sxs-lookup"><span data-stu-id="60c9c-206">Issues should be filed [here](https://github.com/madskristensen/BundlerMinifier/issues).</span></span>

<span data-ttu-id="60c9c-207">Ten pakiet rozszerza platformy .NET Core interfejsu wiersza polecenia obejmujący *pakietu dotnet* narzędzia.</span><span class="sxs-lookup"><span data-stu-id="60c9c-207">This package extends the .NET Core CLI to include the *dotnet-bundle* tool.</span></span> <span data-ttu-id="60c9c-208">Mogą być wykonywane poniższe polecenie, w oknie Konsola Menedżera pakietów (PMC) lub w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="60c9c-208">The following command can be executed in the Package Manager Console (PMC) window or in a command shell:</span></span>

```console
dotnet bundle
```

> [!IMPORTANT]
> <span data-ttu-id="60c9c-209">Menedżer pakietów NuGet dodaje współzależności plikowi \*.csproj jako `<PackageReference />` węzłów.</span><span class="sxs-lookup"><span data-stu-id="60c9c-209">NuGet Package Manager adds dependencies to the \*.csproj file as `<PackageReference />` nodes.</span></span> <span data-ttu-id="60c9c-210">`dotnet bundle` Polecenia jest zarejestrowana przy użyciu interfejsu wiersza polecenia platformy .NET Core tylko wtedy, gdy `<DotNetCliToolReference />` węzła jest używany.</span><span class="sxs-lookup"><span data-stu-id="60c9c-210">The `dotnet bundle` command is registered with the .NET Core CLI only when a `<DotNetCliToolReference />` node is used.</span></span> <span data-ttu-id="60c9c-211">Odpowiednio zmodyfikować plik \*.csproj.</span><span class="sxs-lookup"><span data-stu-id="60c9c-211">Modify the \*.csproj file accordingly.</span></span>

## <a name="add-files-to-workflow"></a><span data-ttu-id="60c9c-212">Dodaj pliki do przepływu pracy</span><span class="sxs-lookup"><span data-stu-id="60c9c-212">Add files to workflow</span></span>

<span data-ttu-id="60c9c-213">Rozważmy przykład, w którym dodatkowe *custome.CSS* plik zostanie dodany, podobne do następujących:</span><span class="sxs-lookup"><span data-stu-id="60c9c-213">Consider an example in which an additional *custom.css* file is added resembling the following:</span></span>

[!code-css[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/css/custom.css)]

<span data-ttu-id="60c9c-214">Do zminimalizowania *custome.CSS* i połączyć ją z *site.css* do *site.min.css* Dodaj ścieżkę względną do *bundleconfig.json*:</span><span class="sxs-lookup"><span data-stu-id="60c9c-214">To minify *custom.css* and bundle it with *site.css* into a *site.min.css* file, add the relative path to *bundleconfig.json*:</span></span>

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig2.json?highlight=6)]

> [!NOTE]
> <span data-ttu-id="60c9c-215">Alternatywnie można użyć następującego wzorca obsługi symboli wieloznacznych:</span><span class="sxs-lookup"><span data-stu-id="60c9c-215">Alternatively, the following globbing pattern could be used:</span></span>
>
> ```json
> "inputFiles": ["wwwroot/**/*(*.css|!(*.min.css))"]
> ```
>
> <span data-ttu-id="60c9c-216">Ten wzorzec symboli wieloznacznych uwzględnia wszystkie pliki CSS i nie obejmuje wzorzec zminimalizowanego pliku.</span><span class="sxs-lookup"><span data-stu-id="60c9c-216">This globbing pattern matches all CSS files and excludes the minified file pattern.</span></span>

<span data-ttu-id="60c9c-217">Skompiluj aplikację.</span><span class="sxs-lookup"><span data-stu-id="60c9c-217">Build the application.</span></span> <span data-ttu-id="60c9c-218">Otwórz *site.min.css* i zwróć uwagę, zawartość *custome.CSS* jest dołączany na końcu pliku.</span><span class="sxs-lookup"><span data-stu-id="60c9c-218">Open *site.min.css* and notice the content of *custom.css* is appended to the end of the file.</span></span>

## <a name="environment-based-bundling-and-minification"></a><span data-ttu-id="60c9c-219">Oparte na środowisku tworzenie pakietów i minimalizowanie</span><span class="sxs-lookup"><span data-stu-id="60c9c-219">Environment-based bundling and minification</span></span>

<span data-ttu-id="60c9c-220">Najlepszym rozwiązaniem jest pliki powiązane i zminimalizowany aplikacji stosuje się w środowisku produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="60c9c-220">As a best practice, the bundled and minified files of your app should be used in a production environment.</span></span> <span data-ttu-id="60c9c-221">Podczas tworzenia aplikacji oryginalne pliki należy łatwiejsze debugowanie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="60c9c-221">During development, the original files make for easier debugging of the app.</span></span>

<span data-ttu-id="60c9c-222">Określić, które pliki do uwzględnienia na stronach sieci przy użyciu [Pomocnik tagu środowiska](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) w widoków.</span><span class="sxs-lookup"><span data-stu-id="60c9c-222">Specify which files to include in your pages by using the [Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) in your views.</span></span> <span data-ttu-id="60c9c-223">Pomocnik tagu środowiska tylko powoduje wyświetlenie jej zawartości podczas pracy w określonych [środowisk](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="60c9c-223">The Environment Tag Helper only renders its contents when running in specific [environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="60c9c-224">Następujące `environment` tag renderuje nieprzetworzone pliki CSS, podczas uruchamiania w `Development` środowiska:</span><span class="sxs-lookup"><span data-stu-id="60c9c-224">The following `environment` tag renders the unprocessed CSS files when running in the `Development` environment:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=21-24)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=9-12)]

::: moniker-end

<span data-ttu-id="60c9c-225">Następujące `environment` tag renderuje pliki CSS powiązane i zminimalizowany podczas uruchamiania w środowiskach innych niż `Development`.</span><span class="sxs-lookup"><span data-stu-id="60c9c-225">The following `environment` tag renders the bundled and minified CSS files when running in an environment other than `Development`.</span></span> <span data-ttu-id="60c9c-226">Na przykład, działające w `Production` lub `Staging` wyzwala renderowania te arkusze stylów:</span><span class="sxs-lookup"><span data-stu-id="60c9c-226">For example, running in `Production` or `Staging` triggers the rendering of these stylesheets:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=5&range=25-30)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=13-18)]

::: moniker-end

## <a name="consume-bundleconfigjson-from-gulp"></a><span data-ttu-id="60c9c-227">Consume bundleconfig.json from Gulp</span><span class="sxs-lookup"><span data-stu-id="60c9c-227">Consume bundleconfig.json from Gulp</span></span>

<span data-ttu-id="60c9c-228">Istnieją przypadki, w których aplikacja tworzenie pakietów i minimalizowanie przepływ pracy wymaga dodatkowego przetwarzania.</span><span class="sxs-lookup"><span data-stu-id="60c9c-228">There are cases in which an app's bundling and minification workflow requires additional processing.</span></span> <span data-ttu-id="60c9c-229">Przykłady obejmują optymalizacji obrazu, rozrywające pamięci podręcznej i przetwarzanie zasobów w sieci CDN.</span><span class="sxs-lookup"><span data-stu-id="60c9c-229">Examples include image optimization, cache busting, and CDN asset processing.</span></span> <span data-ttu-id="60c9c-230">Aby spełnić te wymagania, możesz przekonwertować tworzenie pakietów i minimalizowanie przepływu pracy korzystanie z Gulp.</span><span class="sxs-lookup"><span data-stu-id="60c9c-230">To satisfy these requirements, you can convert the bundling and minification workflow to use Gulp.</span></span>

### <a name="use-the-bundler--minifier-extension"></a><span data-ttu-id="60c9c-231">Za pomocą narzędzia Bundler & Minifier rozszerzenia</span><span class="sxs-lookup"><span data-stu-id="60c9c-231">Use the Bundler & Minifier extension</span></span>

<span data-ttu-id="60c9c-232">Visual Studio [narzędzia Bundler & Minifier](https://marketplace.visualstudio.com/items?itemName=MadsKristensen.BundlerMinifier) rozszerzenia obsługi konwersji Gulp.</span><span class="sxs-lookup"><span data-stu-id="60c9c-232">The Visual Studio [Bundler & Minifier](https://marketplace.visualstudio.com/items?itemName=MadsKristensen.BundlerMinifier) extension handles the conversion to Gulp.</span></span>

> [!NOTE]
> <span data-ttu-id="60c9c-233">Rozszerzenie narzędzia Bundler & Minifier należy do tworzonych metodą społecznościową projekt w witrynie GitHub, dla którego firmy Microsoft nie zapewnia obsługi.</span><span class="sxs-lookup"><span data-stu-id="60c9c-233">The Bundler & Minifier extension belongs to a community-driven project on GitHub for which Microsoft provides no support.</span></span> <span data-ttu-id="60c9c-234">Powinny być zgłaszane problemy [tutaj](https://github.com/madskristensen/BundlerMinifier/issues).</span><span class="sxs-lookup"><span data-stu-id="60c9c-234">Issues should be filed [here](https://github.com/madskristensen/BundlerMinifier/issues).</span></span>

<span data-ttu-id="60c9c-235">Kliknij prawym przyciskiem myszy *bundleconfig.json* plik w Eksploratorze rozwiązań i wybierz **narzędzia Bundler & Minifier** > **przekonwertować Gulp...** :</span><span class="sxs-lookup"><span data-stu-id="60c9c-235">Right-click the *bundleconfig.json* file in Solution Explorer and select **Bundler & Minifier** > **Convert To Gulp...**:</span></span>

![Konwertuj Gulp w menu kontekstowym](../client-side/bundling-and-minification/_static/convert-to-gulp.png)

<span data-ttu-id="60c9c-237">*Gulpfile.js* i *package.json* pliki zostaną dodane do projektu.</span><span class="sxs-lookup"><span data-stu-id="60c9c-237">The *gulpfile.js* and *package.json* files are added to the project.</span></span> <span data-ttu-id="60c9c-238">Obsługa [npm](https://www.npmjs.com/) pakiety wymienione w *package.json* pliku `devDependencies` sekcji są zainstalowane.</span><span class="sxs-lookup"><span data-stu-id="60c9c-238">The supporting [npm](https://www.npmjs.com/) packages listed in the *package.json* file's `devDependencies` section are installed.</span></span>

<span data-ttu-id="60c9c-239">Uruchom następujące polecenie w oknie PMC, aby zainstalować interfejs wiersza polecenia Gulp jako globalne zależności:</span><span class="sxs-lookup"><span data-stu-id="60c9c-239">Run the following command in the PMC window to install the Gulp CLI as a global dependency:</span></span>

```console
npm i -g gulp-cli
```

<span data-ttu-id="60c9c-240">*Gulpfile.js* plików odczytów *bundleconfig.json* wejść, wyjść i ustawienia w pliku.</span><span class="sxs-lookup"><span data-stu-id="60c9c-240">The *gulpfile.js* file reads the *bundleconfig.json* file for the inputs, outputs, and settings.</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-12&highlight=10)]

### <a name="convert-manually"></a><span data-ttu-id="60c9c-241">Konwertuj ręcznie</span><span class="sxs-lookup"><span data-stu-id="60c9c-241">Convert manually</span></span>

<span data-ttu-id="60c9c-242">Jeśli program Visual Studio i/lub rozszerzenie narzędzia Bundler & Minifier nie są dostępne, przekonwertuj ręcznie.</span><span class="sxs-lookup"><span data-stu-id="60c9c-242">If Visual Studio and/or the Bundler & Minifier extension aren't available, convert manually.</span></span>

<span data-ttu-id="60c9c-243">Dodaj *package.json* pliku następującym kodem `devDependencies`, w katalogu głównym projektu:</span><span class="sxs-lookup"><span data-stu-id="60c9c-243">Add a *package.json* file, with the following `devDependencies`, to the project root:</span></span>

> [!WARNING]
> <span data-ttu-id="60c9c-244">`gulp-uglify` Modułu nie obsługuje ECMAScript (ES) 2015 / ES6 i nowszych.</span><span class="sxs-lookup"><span data-stu-id="60c9c-244">The `gulp-uglify` module doesn't support ECMAScript (ES) 2015 / ES6 and later.</span></span> <span data-ttu-id="60c9c-245">Zainstaluj [gulp terser](https://www.npmjs.com/package/gulp-terser) zamiast `gulp-uglify` używać ES2015 / ES6 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="60c9c-245">Install [gulp-terser](https://www.npmjs.com/package/gulp-terser) instead of `gulp-uglify` to use ES2015 / ES6 or later.</span></span>

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/package.json?range=5-13)]

<span data-ttu-id="60c9c-246">Instalowanie zależności, uruchamiając następujące polecenie w tym samym poziomie co *package.json*:</span><span class="sxs-lookup"><span data-stu-id="60c9c-246">Install the dependencies by running the following command at the same level as *package.json*:</span></span>

```console
npm i
```

<span data-ttu-id="60c9c-247">Zainstaluj interfejs wiersza polecenia Gulp jako globalne zależności:</span><span class="sxs-lookup"><span data-stu-id="60c9c-247">Install the Gulp CLI as a global dependency:</span></span>

```console
npm i -g gulp-cli
```

<span data-ttu-id="60c9c-248">Kopiuj *gulpfile.js* pliku poniżej do katalogu głównego projektu:</span><span class="sxs-lookup"><span data-stu-id="60c9c-248">Copy the *gulpfile.js* file below to the project root:</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-11,14-)]

### <a name="run-gulp-tasks"></a><span data-ttu-id="60c9c-249">Uruchamianie zadań Gulp</span><span class="sxs-lookup"><span data-stu-id="60c9c-249">Run Gulp tasks</span></span>

<span data-ttu-id="60c9c-250">Aby wyzwolić zadanie minimalizowanie Gulp, zanim projekt zostanie skompilowany w programie Visual Studio, Dodaj następujący element [programu MSBuild](/visualstudio/msbuild/msbuild-targets) pliku \*.csproj:</span><span class="sxs-lookup"><span data-stu-id="60c9c-250">To trigger the Gulp minification task before the project builds in Visual Studio, add the following [MSBuild Target](/visualstudio/msbuild/msbuild-targets) to the \*.csproj file:</span></span>

[!code-xml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/BuildBundlerMinifierApp.csproj?range=14-16)]

<span data-ttu-id="60c9c-251">W tym przykładzie wszystkie zadania zdefiniowane w ramach `MyPreCompileTarget` docelowy uruchomienia przed wstępnie zdefiniowanego `Build` docelowej.</span><span class="sxs-lookup"><span data-stu-id="60c9c-251">In this example, any tasks defined within the `MyPreCompileTarget` target run before the predefined `Build` target.</span></span> <span data-ttu-id="60c9c-252">W oknie danych wyjściowych programu Visual Studio pojawia się dane wyjściowe podobne do następujących:</span><span class="sxs-lookup"><span data-stu-id="60c9c-252">Output similar to the following appears in Visual Studio's Output window:</span></span>

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


## <a name="additional-resources"></a><span data-ttu-id="60c9c-253">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="60c9c-253">Additional resources</span></span>

* [<span data-ttu-id="60c9c-254">Korzystanie z Grunt</span><span class="sxs-lookup"><span data-stu-id="60c9c-254">Use Grunt</span></span>](xref:client-side/using-grunt)
* [<span data-ttu-id="60c9c-255">Używanie wielu środowisk</span><span class="sxs-lookup"><span data-stu-id="60c9c-255">Use multiple environments</span></span>](xref:fundamentals/environments)
* [<span data-ttu-id="60c9c-256">Pomocnicy tagów</span><span class="sxs-lookup"><span data-stu-id="60c9c-256">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
