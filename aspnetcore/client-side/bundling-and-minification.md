---
title: Łączenie i zminifikować zasobów statycznych w ASP.NET Core
author: scottaddie
description: Dowiedz się, jak zoptymalizować zasoby statyczne w ASP.NET Core aplikacji sieci Web przez zastosowanie technik tworzenia i minifikacja.
ms.author: scaddie
ms.custom: mvc
ms.date: 06/17/2019
uid: client-side/bundling-and-minification
ms.openlocfilehash: a7a5c40d6c31c4416212c02c1b491dd794f2a1d3
ms.sourcegitcommit: b3e1e31e5d8bdd94096cf27444594d4a7b065525
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/04/2019
ms.locfileid: "74803282"
---
# <a name="bundle-and-minify-static-assets-in-aspnet-core"></a><span data-ttu-id="6b0c8-103">Łączenie i zminifikować zasobów statycznych w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6b0c8-103">Bundle and minify static assets in ASP.NET Core</span></span>

<span data-ttu-id="6b0c8-104">Przez [Scott Addie](https://twitter.com/Scott_Addie) i [David sosny](https://twitter.com/davidpine7)</span><span class="sxs-lookup"><span data-stu-id="6b0c8-104">By [Scott Addie](https://twitter.com/Scott_Addie) and [David Pine](https://twitter.com/davidpine7)</span></span>

<span data-ttu-id="6b0c8-105">W tym artykule wyjaśniono zalety stosowania funkcji tworzenia i minifikacja, w tym ich używania z aplikacjami sieci Web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="6b0c8-105">This article explains the benefits of applying bundling and minification, including how these features can be used with ASP.NET Core web apps.</span></span>

## <a name="what-is-bundling-and-minification"></a><span data-ttu-id="6b0c8-106">Co to jest rozdziały i minifikacja</span><span class="sxs-lookup"><span data-stu-id="6b0c8-106">What is bundling and minification</span></span>

<span data-ttu-id="6b0c8-107">Tworzenie i minifikacja to dwie różne optymalizacje wydajności, które można zastosować w aplikacji sieci Web.</span><span class="sxs-lookup"><span data-stu-id="6b0c8-107">Bundling and minification are two distinct performance optimizations you can apply in a web app.</span></span> <span data-ttu-id="6b0c8-108">Używane razem, grupując i minifikacja poprawić wydajność poprzez zmniejszenie liczby żądań serwera i zmniejszenie rozmiaru żądanych zasobów statycznych.</span><span class="sxs-lookup"><span data-stu-id="6b0c8-108">Used together, bundling and minification improve performance by reducing the number of server requests and reducing the size of the requested static assets.</span></span>

<span data-ttu-id="6b0c8-109">Zgrupowanie i minifikacja przede wszystkim zwiększy czas ładowania żądania pierwszej strony.</span><span class="sxs-lookup"><span data-stu-id="6b0c8-109">Bundling and minification primarily improve the first page request load time.</span></span> <span data-ttu-id="6b0c8-110">Po zażądaniu strony sieci Web przeglądarka buforuje statyczne zasoby (JavaScript, CSS i obrazy).</span><span class="sxs-lookup"><span data-stu-id="6b0c8-110">Once a web page has been requested, the browser caches the static assets (JavaScript, CSS, and images).</span></span> <span data-ttu-id="6b0c8-111">W związku z tym, zgrupowanie i minifikacja nie poprawia wydajności podczas żądania tej samej strony lub stron w tej samej lokacji, w której zażądają tych samych zasobów.</span><span class="sxs-lookup"><span data-stu-id="6b0c8-111">Consequently, bundling and minification don't improve performance when requesting the same page, or pages, on the same site requesting the same assets.</span></span> <span data-ttu-id="6b0c8-112">Jeśli Nagłówek Expires nie jest poprawnie ustawiony na elementach zawartości i jeśli nie jest używane minifikacja i nie jest używany, heurystyka Aktualności przeglądarki Oznacz zasoby jako przestarzałe po kilku dniach.</span><span class="sxs-lookup"><span data-stu-id="6b0c8-112">If the expires header isn't set correctly on the assets and if bundling and minification isn't used, the browser's freshness heuristics mark the assets stale after a few days.</span></span> <span data-ttu-id="6b0c8-113">Ponadto przeglądarka wymaga żądania weryfikacji dla każdego elementu zawartości.</span><span class="sxs-lookup"><span data-stu-id="6b0c8-113">Additionally, the browser requires a validation request for each asset.</span></span> <span data-ttu-id="6b0c8-114">W takim przypadku zgrupowanie i minifikacja zapewnia poprawę wydajności nawet po pierwszym żądaniu strony.</span><span class="sxs-lookup"><span data-stu-id="6b0c8-114">In this case, bundling and minification provide a performance improvement even after the first page request.</span></span>

### <a name="bundling"></a><span data-ttu-id="6b0c8-115">Tworzenia pakietów</span><span class="sxs-lookup"><span data-stu-id="6b0c8-115">Bundling</span></span>

<span data-ttu-id="6b0c8-116">Tworzenie pakietów pozwala łączyć wiele plików w jeden plik.</span><span class="sxs-lookup"><span data-stu-id="6b0c8-116">Bundling combines multiple files into a single file.</span></span> <span data-ttu-id="6b0c8-117">Zgrupowanie zmniejsza liczbę żądań serwera, które są niezbędne do renderowania zasobów sieci Web, takich jak strona sieci Web.</span><span class="sxs-lookup"><span data-stu-id="6b0c8-117">Bundling reduces the number of server requests that are necessary to render a web asset, such as a web page.</span></span> <span data-ttu-id="6b0c8-118">Można utworzyć dowolną liczbę pojedynczych pakietów przeznaczonych dla CSS, JavaScript itd. Mniejsza liczba plików oznacza mniejszą liczbę żądań HTTP z przeglądarki do serwera lub z usługi dostarczającej aplikację.</span><span class="sxs-lookup"><span data-stu-id="6b0c8-118">You can create any number of individual bundles specifically for CSS, JavaScript, etc. Fewer files means fewer HTTP requests from the browser to the server or from the service providing your application.</span></span> <span data-ttu-id="6b0c8-119">Powoduje to zwiększenie wydajności pierwszej strony.</span><span class="sxs-lookup"><span data-stu-id="6b0c8-119">This results in improved first page load performance.</span></span>

### <a name="minification"></a><span data-ttu-id="6b0c8-120">Minifikacja</span><span class="sxs-lookup"><span data-stu-id="6b0c8-120">Minification</span></span>

<span data-ttu-id="6b0c8-121">Minifikacja usuwa zbędne znaki z kodu bez zmiany funkcjonalności.</span><span class="sxs-lookup"><span data-stu-id="6b0c8-121">Minification removes unnecessary characters from code without altering functionality.</span></span> <span data-ttu-id="6b0c8-122">Wynikiem jest znaczny spadek rozmiaru żądanych zasobów (takich jak CSS, obrazy i pliki JavaScript).</span><span class="sxs-lookup"><span data-stu-id="6b0c8-122">The result is a significant size reduction in requested assets (such as CSS, images, and JavaScript files).</span></span> <span data-ttu-id="6b0c8-123">Typowe efekty uboczne minifikacja obejmują skracanie nazw zmiennych do jednego znaku oraz usuwanie komentarzy i niepotrzebnych białych znaków.</span><span class="sxs-lookup"><span data-stu-id="6b0c8-123">Common side effects of minification include shortening variable names to one character and removing comments and unnecessary whitespace.</span></span>

<span data-ttu-id="6b0c8-124">Weź pod uwagę następującą funkcję języka JavaScript:</span><span class="sxs-lookup"><span data-stu-id="6b0c8-124">Consider the following JavaScript function:</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.js)]

<span data-ttu-id="6b0c8-125">Minifikacja zmniejsza funkcję do następujących:</span><span class="sxs-lookup"><span data-stu-id="6b0c8-125">Minification reduces the function to the following:</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.min.js)]

<span data-ttu-id="6b0c8-126">Oprócz usuwania komentarzy i niepotrzebnych białych znaków, nazwy następujących parametrów i zmiennych zostały zmienione w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="6b0c8-126">In addition to removing the comments and unnecessary whitespace, the following parameter and variable names were renamed as follows:</span></span>

<span data-ttu-id="6b0c8-127">Oryginał</span><span class="sxs-lookup"><span data-stu-id="6b0c8-127">Original</span></span> | <span data-ttu-id="6b0c8-128">Zmiany</span><span class="sxs-lookup"><span data-stu-id="6b0c8-128">Renamed</span></span>
--- | :---:
`imageTagAndImageID` | `t`
`imageContext` | `a`
`imageElement` | `r`

## <a name="impact-of-bundling-and-minification"></a><span data-ttu-id="6b0c8-129">Wpływ tworzenia i minifikacja</span><span class="sxs-lookup"><span data-stu-id="6b0c8-129">Impact of bundling and minification</span></span>

<span data-ttu-id="6b0c8-130">W poniższej tabeli przedstawiono różnice między pojedynczym ładowaniem zasobów i użyciem grupowania i minifikacja:</span><span class="sxs-lookup"><span data-stu-id="6b0c8-130">The following table outlines differences between individually loading assets and using bundling and minification:</span></span>

<span data-ttu-id="6b0c8-131">Akcja</span><span class="sxs-lookup"><span data-stu-id="6b0c8-131">Action</span></span> | <span data-ttu-id="6b0c8-132">Z B/M</span><span class="sxs-lookup"><span data-stu-id="6b0c8-132">With B/M</span></span> | <span data-ttu-id="6b0c8-133">Bez B/M</span><span class="sxs-lookup"><span data-stu-id="6b0c8-133">Without B/M</span></span> | <span data-ttu-id="6b0c8-134">Zmiana</span><span class="sxs-lookup"><span data-stu-id="6b0c8-134">Change</span></span>
--- | :---: | :---: | :---:
<span data-ttu-id="6b0c8-135">Żądania plików</span><span class="sxs-lookup"><span data-stu-id="6b0c8-135">File Requests</span></span>  | <span data-ttu-id="6b0c8-136">7</span><span class="sxs-lookup"><span data-stu-id="6b0c8-136">7</span></span>   | <span data-ttu-id="6b0c8-137">18</span><span class="sxs-lookup"><span data-stu-id="6b0c8-137">18</span></span>     | <span data-ttu-id="6b0c8-138">157%</span><span class="sxs-lookup"><span data-stu-id="6b0c8-138">157%</span></span>
<span data-ttu-id="6b0c8-139">Przeniesiono KB</span><span class="sxs-lookup"><span data-stu-id="6b0c8-139">KB Transferred</span></span> | <span data-ttu-id="6b0c8-140">156</span><span class="sxs-lookup"><span data-stu-id="6b0c8-140">156</span></span> | <span data-ttu-id="6b0c8-141">264.68</span><span class="sxs-lookup"><span data-stu-id="6b0c8-141">264.68</span></span> | <span data-ttu-id="6b0c8-142">70%</span><span class="sxs-lookup"><span data-stu-id="6b0c8-142">70%</span></span>
<span data-ttu-id="6b0c8-143">Czas ładowania (MS)</span><span class="sxs-lookup"><span data-stu-id="6b0c8-143">Load Time (ms)</span></span> | <span data-ttu-id="6b0c8-144">885</span><span class="sxs-lookup"><span data-stu-id="6b0c8-144">885</span></span> | <span data-ttu-id="6b0c8-145">2360</span><span class="sxs-lookup"><span data-stu-id="6b0c8-145">2360</span></span>   | <span data-ttu-id="6b0c8-146">167%</span><span class="sxs-lookup"><span data-stu-id="6b0c8-146">167%</span></span>

<span data-ttu-id="6b0c8-147">Przeglądarki są dość szczegółowe w odniesieniu do nagłówków żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="6b0c8-147">Browsers are fairly verbose with regard to HTTP request headers.</span></span> <span data-ttu-id="6b0c8-148">Metryka całkowita liczba wysłanych bajtów osiągnęła znaczącą redukcję podczas grupowania.</span><span class="sxs-lookup"><span data-stu-id="6b0c8-148">The total bytes sent metric saw a significant reduction when bundling.</span></span> <span data-ttu-id="6b0c8-149">Czas ładowania przedstawia znaczącą poprawę, jednak ten przykład jest uruchamiany lokalnie.</span><span class="sxs-lookup"><span data-stu-id="6b0c8-149">The load time shows a significant improvement, however this example ran locally.</span></span> <span data-ttu-id="6b0c8-150">W przypadku korzystania z funkcji grupowania i minifikacja z zasobami transferowanymi za pośrednictwem sieci są osiągane większe zyski wydajności.</span><span class="sxs-lookup"><span data-stu-id="6b0c8-150">Greater performance gains are realized when using bundling and minification with assets transferred over a network.</span></span>

## <a name="choose-a-bundling-and-minification-strategy"></a><span data-ttu-id="6b0c8-151">Wybierz strategię tworzenia i minifikacja</span><span class="sxs-lookup"><span data-stu-id="6b0c8-151">Choose a bundling and minification strategy</span></span>

<span data-ttu-id="6b0c8-152">Szablony projektów MVC i Razor Pages stanowią wbudowane rozwiązanie do tworzenia i minifikacja składające się z pliku konfiguracji JSON.</span><span class="sxs-lookup"><span data-stu-id="6b0c8-152">The MVC and Razor Pages project templates provide an out-of-the-box solution for bundling and minification consisting of a JSON configuration file.</span></span> <span data-ttu-id="6b0c8-153">Narzędzia innych firm, takie jak [grunt](xref:client-side/using-grunt) Task Runner, spełniają te same zadania o nieco większej złożoności.</span><span class="sxs-lookup"><span data-stu-id="6b0c8-153">Third-party tools, such as the [Grunt](xref:client-side/using-grunt) task runner, accomplish the same tasks with a bit more complexity.</span></span> <span data-ttu-id="6b0c8-154">Narzędzie innej firmy jest doskonałym rozwiązaniem, gdy przepływ pracy deweloperskiej wymaga przetwarzania poza tworzeniem i minifikacja&mdash;takich jak zaznaczanie błędów i Optymalizacja obrazu.</span><span class="sxs-lookup"><span data-stu-id="6b0c8-154">A third-party tool is a great fit when your development workflow requires processing beyond bundling and minification&mdash;such as linting and image optimization.</span></span> <span data-ttu-id="6b0c8-155">Korzystając z konstrukcji i minifikacja w czasie projektowania, pliki zminimalizowanego są tworzone przed wdrożeniem aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6b0c8-155">By using design-time bundling and minification, the minified files are created prior to the app's deployment.</span></span> <span data-ttu-id="6b0c8-156">Przydzielenie i minifikacja przed wdrożeniem zapewnia zalety mniejszego obciążenia serwera.</span><span class="sxs-lookup"><span data-stu-id="6b0c8-156">Bundling and minifying before deployment provides the advantage of reduced server load.</span></span> <span data-ttu-id="6b0c8-157">Należy jednak pamiętać, że konstrukcja czasu projektowania i minifikacja zwiększa złożoność kompilacji i działa tylko z plikami statycznymi.</span><span class="sxs-lookup"><span data-stu-id="6b0c8-157">However, it's important to recognize that design-time bundling and minification increases build complexity and only works with static files.</span></span>

## <a name="configure-bundling-and-minification"></a><span data-ttu-id="6b0c8-158">Konfigurowanie grupowania i minifikacja</span><span class="sxs-lookup"><span data-stu-id="6b0c8-158">Configure bundling and minification</span></span>

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="6b0c8-159">W ASP.NET Core 2,0 lub starszych, szablony projektów MVC i Razor Pages udostępniają plik konfiguracji *bundleconfig. JSON* , który definiuje opcje dla każdego pakietu:</span><span class="sxs-lookup"><span data-stu-id="6b0c8-159">In ASP.NET Core 2.0 or earlier, the MVC and Razor Pages project templates provide a *bundleconfig.json* configuration file that defines the options for each bundle:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="6b0c8-160">W ASP.NET Core 2,1 lub nowszej Dodaj nowy plik JSON o nazwie *bundleconfig. JSON*, do elementu głównego MVC lub Razor Pages projektu.</span><span class="sxs-lookup"><span data-stu-id="6b0c8-160">In ASP.NET Core 2.1 or later, add a new JSON file, named *bundleconfig.json*, to the MVC or Razor Pages project root.</span></span> <span data-ttu-id="6b0c8-161">Dołącz następujący kod JSON do tego pliku jako punkt początkowy:</span><span class="sxs-lookup"><span data-stu-id="6b0c8-161">Include the following JSON in that file as a starting point:</span></span>

::: moniker-end

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig.json)]

<span data-ttu-id="6b0c8-162">Plik *bundleconfig. JSON* definiuje opcje dla każdego pakietu.</span><span class="sxs-lookup"><span data-stu-id="6b0c8-162">The *bundleconfig.json* file defines the options for each bundle.</span></span> <span data-ttu-id="6b0c8-163">W poprzednim przykładzie została zdefiniowana jedna konfiguracja pakietu dla plików niestandardowych JavaScript (*wwwroot/js/site. js*) i stylesheet (*wwwroot/CSS/site. css*).</span><span class="sxs-lookup"><span data-stu-id="6b0c8-163">In the preceding example, a single bundle configuration is defined for the custom JavaScript (*wwwroot/js/site.js*) and stylesheet (*wwwroot/css/site.css*) files.</span></span>

<span data-ttu-id="6b0c8-164">Opcje konfiguracji obejmują:</span><span class="sxs-lookup"><span data-stu-id="6b0c8-164">Configuration options include:</span></span>

* <span data-ttu-id="6b0c8-165">`outputFileName`: nazwa pliku pakietu do wyprowadzenia.</span><span class="sxs-lookup"><span data-stu-id="6b0c8-165">`outputFileName`: The name of the bundle file to output.</span></span> <span data-ttu-id="6b0c8-166">Może zawierać ścieżkę względną z pliku *bundleconfig. JSON* .</span><span class="sxs-lookup"><span data-stu-id="6b0c8-166">Can contain a relative path from the *bundleconfig.json* file.</span></span> <span data-ttu-id="6b0c8-167">**Wymagane**</span><span class="sxs-lookup"><span data-stu-id="6b0c8-167">**required**</span></span>
* <span data-ttu-id="6b0c8-168">`inputFiles`: tablica plików do powiązania ze sobą.</span><span class="sxs-lookup"><span data-stu-id="6b0c8-168">`inputFiles`: An array of files to bundle together.</span></span> <span data-ttu-id="6b0c8-169">Są to względne ścieżki do pliku konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="6b0c8-169">These are relative paths to the configuration file.</span></span> <span data-ttu-id="6b0c8-170">**opcjonalne**, \* pusta wartość powoduje pusty plik wyjściowy.</span><span class="sxs-lookup"><span data-stu-id="6b0c8-170">**optional**, \*an empty value results in an empty output file.</span></span> <span data-ttu-id="6b0c8-171">Obsługiwane są wzorce [obsługi symboli wieloznacznych](https://www.tldp.org/LDP/abs/html/globbingref.html) .</span><span class="sxs-lookup"><span data-stu-id="6b0c8-171">[globbing](https://www.tldp.org/LDP/abs/html/globbingref.html) patterns are supported.</span></span>
* <span data-ttu-id="6b0c8-172">`minify`: opcje minifikacja dla typu danych wyjściowych.</span><span class="sxs-lookup"><span data-stu-id="6b0c8-172">`minify`: The minification options for the output type.</span></span> <span data-ttu-id="6b0c8-173">**opcjonalne**, *domyślne-`minify: { enabled: true }`*</span><span class="sxs-lookup"><span data-stu-id="6b0c8-173">**optional**, *default - `minify: { enabled: true }`*</span></span>
  * <span data-ttu-id="6b0c8-174">Opcje konfiguracji są dostępne dla każdego typu pliku wyjściowego.</span><span class="sxs-lookup"><span data-stu-id="6b0c8-174">Configuration options are available per output file type.</span></span>
    * [<span data-ttu-id="6b0c8-175">Minifier CSS</span><span class="sxs-lookup"><span data-stu-id="6b0c8-175">CSS Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki/cssminifier)
    * [<span data-ttu-id="6b0c8-176">JavaScript Minifier</span><span class="sxs-lookup"><span data-stu-id="6b0c8-176">JavaScript Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki/JavaScript-Minifier-settings)
    * [<span data-ttu-id="6b0c8-177">Minifier HTML</span><span class="sxs-lookup"><span data-stu-id="6b0c8-177">HTML Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki)
* <span data-ttu-id="6b0c8-178">`includeInProject`: Flaga oznaczająca, czy dodać wygenerowane pliki do pliku projektu.</span><span class="sxs-lookup"><span data-stu-id="6b0c8-178">`includeInProject`: Flag indicating whether to add generated files to project file.</span></span> <span data-ttu-id="6b0c8-179">**opcjonalne**, *Domyślnie-false*</span><span class="sxs-lookup"><span data-stu-id="6b0c8-179">**optional**, *default - false*</span></span>
* <span data-ttu-id="6b0c8-180">`sourceMap`: Flaga oznaczająca, czy generować mapę źródłową dla powiązanego pliku.</span><span class="sxs-lookup"><span data-stu-id="6b0c8-180">`sourceMap`: Flag indicating whether to generate a source map for the bundled file.</span></span> <span data-ttu-id="6b0c8-181">**opcjonalne**, *Domyślnie-false*</span><span class="sxs-lookup"><span data-stu-id="6b0c8-181">**optional**, *default - false*</span></span>
* <span data-ttu-id="6b0c8-182">`sourceMapRootPath`: ścieżka katalogu głównego do przechowywania wygenerowanego pliku mapy źródłowej.</span><span class="sxs-lookup"><span data-stu-id="6b0c8-182">`sourceMapRootPath`: The root path for storing the generated source map file.</span></span>

## <a name="build-time-execution-of-bundling-and-minification"></a><span data-ttu-id="6b0c8-183">Wykonywanie operacji grupowania i minifikacja w czasie kompilacji</span><span class="sxs-lookup"><span data-stu-id="6b0c8-183">Build-time execution of bundling and minification</span></span>

<span data-ttu-id="6b0c8-184">Pakiet NuGet [BuildBundlerMinifier](https://www.nuget.org/packages/BuildBundlerMinifier/) umożliwia wykonywanie operacji grupowania i minifikacja w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="6b0c8-184">The [BuildBundlerMinifier](https://www.nuget.org/packages/BuildBundlerMinifier/) NuGet package enables the execution of bundling and minification at build time.</span></span> <span data-ttu-id="6b0c8-185">Pakiet wprowadza [elementy docelowe programu MSBuild](/visualstudio/msbuild/msbuild-targets) , które są uruchamiane w czasie kompilacji i czyszczenia.</span><span class="sxs-lookup"><span data-stu-id="6b0c8-185">The package injects [MSBuild Targets](/visualstudio/msbuild/msbuild-targets) which run at build and clean time.</span></span> <span data-ttu-id="6b0c8-186">Plik *bundleconfig. JSON* jest analizowany przez proces kompilacji w celu utworzenia plików wyjściowych na podstawie zdefiniowanej konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="6b0c8-186">The *bundleconfig.json* file is analyzed by the build process to produce the output files based on the defined configuration.</span></span>

> [!NOTE]
> <span data-ttu-id="6b0c8-187">BuildBundlerMinifier należy do projektu opartego na społeczności w usłudze GitHub, dla którego firma Microsoft nie zapewnia pomocy technicznej.</span><span class="sxs-lookup"><span data-stu-id="6b0c8-187">BuildBundlerMinifier belongs to a community-driven project on GitHub for which Microsoft provides no support.</span></span> <span data-ttu-id="6b0c8-188">[Tutaj](https://github.com/madskristensen/BundlerMinifier/issues)należy zgłosić problemy.</span><span class="sxs-lookup"><span data-stu-id="6b0c8-188">Issues should be filed [here](https://github.com/madskristensen/BundlerMinifier/issues).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6b0c8-189">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6b0c8-189">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="6b0c8-190">Dodaj pakiet *BuildBundlerMinifier* do projektu.</span><span class="sxs-lookup"><span data-stu-id="6b0c8-190">Add the *BuildBundlerMinifier* package to your project.</span></span>

<span data-ttu-id="6b0c8-191">Skompiluj projekt.</span><span class="sxs-lookup"><span data-stu-id="6b0c8-191">Build the project.</span></span> <span data-ttu-id="6b0c8-192">W oknie dane wyjściowe pojawia się następujący komunikat:</span><span class="sxs-lookup"><span data-stu-id="6b0c8-192">The following appears in the Output window:</span></span>

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

<span data-ttu-id="6b0c8-193">Wyczyść projekt.</span><span class="sxs-lookup"><span data-stu-id="6b0c8-193">Clean the project.</span></span> <span data-ttu-id="6b0c8-194">W oknie dane wyjściowe pojawia się następujący komunikat:</span><span class="sxs-lookup"><span data-stu-id="6b0c8-194">The following appears in the Output window:</span></span>

```console
1>------ Clean started: Project: BuildBundlerMinifierApp, Configuration: Debug Any CPU ------
1>
1>Bundler: Cleaning output from bundleconfig.json
1>Bundler: Done cleaning output file from bundleconfig.json
========== Clean: 1 succeeded, 0 failed, 0 skipped ==========
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="6b0c8-195">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="6b0c8-195">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="6b0c8-196">Dodaj pakiet *BuildBundlerMinifier* do projektu:</span><span class="sxs-lookup"><span data-stu-id="6b0c8-196">Add the *BuildBundlerMinifier* package to your project:</span></span>

```dotnetcli
dotnet add package BuildBundlerMinifier
```

<span data-ttu-id="6b0c8-197">W przypadku używania ASP.NET Core 1. x Przywróć nowo dodany pakiet:</span><span class="sxs-lookup"><span data-stu-id="6b0c8-197">If using ASP.NET Core 1.x, restore the newly added package:</span></span>

```dotnetcli
dotnet restore
```

<span data-ttu-id="6b0c8-198">Kompiluj projekt:</span><span class="sxs-lookup"><span data-stu-id="6b0c8-198">Build the project:</span></span>

```dotnetcli
dotnet build
```

<span data-ttu-id="6b0c8-199">Zostanie wyświetlony następujący komunikat:</span><span class="sxs-lookup"><span data-stu-id="6b0c8-199">The following appears:</span></span>

```console
Microsoft (R) Build Engine version 15.4.8.50001 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.


    Bundler: Begin processing bundleconfig.json
    Bundler: Done processing bundleconfig.json
    BuildBundlerMinifierApp -> C:\BuildBundlerMinifierApp\bin\Debug\netcoreapp2.0\BuildBundlerMinifierApp.dll
```

<span data-ttu-id="6b0c8-200">Wyczyść projekt:</span><span class="sxs-lookup"><span data-stu-id="6b0c8-200">Clean the project:</span></span>

```dotnetcli
dotnet clean
```

<span data-ttu-id="6b0c8-201">Zostaną wyświetlone następujące dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="6b0c8-201">The following output appears:</span></span>

```console
Microsoft (R) Build Engine version 15.4.8.50001 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.


  Bundler: Cleaning output from bundleconfig.json
  Bundler: Done cleaning output file from bundleconfig.json
```

---

## <a name="ad-hoc-execution-of-bundling-and-minification"></a><span data-ttu-id="6b0c8-202">Wykonywanie operacji tworzenia i minifikacja w trybie ad hoc</span><span class="sxs-lookup"><span data-stu-id="6b0c8-202">Ad hoc execution of bundling and minification</span></span>

<span data-ttu-id="6b0c8-203">Możliwe jest uruchamianie zadań tworzenia i minifikacja na podstawie ad hoc bez kompilowania projektu.</span><span class="sxs-lookup"><span data-stu-id="6b0c8-203">It's possible to run the bundling and minification tasks on an ad hoc basis, without building the project.</span></span> <span data-ttu-id="6b0c8-204">Dodaj pakiet NuGet [BundlerMinifier. Core](https://www.nuget.org/packages/BundlerMinifier.Core/) do projektu:</span><span class="sxs-lookup"><span data-stu-id="6b0c8-204">Add the [BundlerMinifier.Core](https://www.nuget.org/packages/BundlerMinifier.Core/) NuGet package to your project:</span></span>

[!code-xml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/BuildBundlerMinifierApp.csproj?range=10)]

> [!NOTE]
> <span data-ttu-id="6b0c8-205">BundlerMinifier. Core należy do projektu opartego na społeczności w witrynie GitHub, dla którego firma Microsoft nie zapewnia pomocy technicznej.</span><span class="sxs-lookup"><span data-stu-id="6b0c8-205">BundlerMinifier.Core belongs to a community-driven project on GitHub for which Microsoft provides no support.</span></span> <span data-ttu-id="6b0c8-206">[Tutaj](https://github.com/madskristensen/BundlerMinifier/issues)należy zgłosić problemy.</span><span class="sxs-lookup"><span data-stu-id="6b0c8-206">Issues should be filed [here](https://github.com/madskristensen/BundlerMinifier/issues).</span></span>

<span data-ttu-id="6b0c8-207">Ten pakiet rozszerza interfejs wiersza polecenia platformy .NET Core, aby dołączyć narzędzie *dotnet-pakiet* .</span><span class="sxs-lookup"><span data-stu-id="6b0c8-207">This package extends the .NET Core CLI to include the *dotnet-bundle* tool.</span></span> <span data-ttu-id="6b0c8-208">Następujące polecenie można wykonać w oknie Konsola Menedżera pakietów (PMC) lub w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="6b0c8-208">The following command can be executed in the Package Manager Console (PMC) window or in a command shell:</span></span>

```dotnetcli
dotnet bundle
```

> [!IMPORTANT]
> <span data-ttu-id="6b0c8-209">Menedżer pakietów NuGet dodaje zależności do pliku \*. csproj jako węzły `<PackageReference />`.</span><span class="sxs-lookup"><span data-stu-id="6b0c8-209">NuGet Package Manager adds dependencies to the \*.csproj file as `<PackageReference />` nodes.</span></span> <span data-ttu-id="6b0c8-210">Polecenie `dotnet bundle` jest rejestrowane interfejs wiersza polecenia platformy .NET Core tylko wtedy, gdy jest używany węzeł `<DotNetCliToolReference />`.</span><span class="sxs-lookup"><span data-stu-id="6b0c8-210">The `dotnet bundle` command is registered with the .NET Core CLI only when a `<DotNetCliToolReference />` node is used.</span></span> <span data-ttu-id="6b0c8-211">Zmodyfikuj odpowiednio plik \*. csproj.</span><span class="sxs-lookup"><span data-stu-id="6b0c8-211">Modify the \*.csproj file accordingly.</span></span>

## <a name="add-files-to-workflow"></a><span data-ttu-id="6b0c8-212">Dodaj pliki do przepływu pracy</span><span class="sxs-lookup"><span data-stu-id="6b0c8-212">Add files to workflow</span></span>

<span data-ttu-id="6b0c8-213">Rozważmy przykład, w którym dodatkowy *niestandardowy plik CSS* został dodany podobny do poniższego:</span><span class="sxs-lookup"><span data-stu-id="6b0c8-213">Consider an example in which an additional *custom.css* file is added resembling the following:</span></span>

[!code-css[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/css/custom.css)]

<span data-ttu-id="6b0c8-214">Aby zminifikować *niestandardowy. css* i powiązać go z plikiem *site. css* w pliku *site. min. css* , Dodaj ścieżkę względną do *bundleconfig. JSON*:</span><span class="sxs-lookup"><span data-stu-id="6b0c8-214">To minify *custom.css* and bundle it with *site.css* into a *site.min.css* file, add the relative path to *bundleconfig.json*:</span></span>

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig2.json?highlight=6)]

> [!NOTE]
> <span data-ttu-id="6b0c8-215">Alternatywnie można użyć następującego wzorca obsługi symboli wieloznacznych:</span><span class="sxs-lookup"><span data-stu-id="6b0c8-215">Alternatively, the following globbing pattern could be used:</span></span>
>
> ```json
> "inputFiles": ["wwwroot/**/!(*.min).css" ]
> ```
>
> <span data-ttu-id="6b0c8-216">Ten wzorzec obsługi symboli wieloznacznych dopasowuje wszystkie pliki CSS i wyklucza wzorzec pliku zminimalizowanego.</span><span class="sxs-lookup"><span data-stu-id="6b0c8-216">This globbing pattern matches all CSS files and excludes the minified file pattern.</span></span>

<span data-ttu-id="6b0c8-217">Kompiluj aplikację.</span><span class="sxs-lookup"><span data-stu-id="6b0c8-217">Build the application.</span></span> <span data-ttu-id="6b0c8-218">Otwórz *witrynę site. min. css* i zwróć uwagę na zawartość *Custom. css* , która jest dołączana na końcu pliku.</span><span class="sxs-lookup"><span data-stu-id="6b0c8-218">Open *site.min.css* and notice the content of *custom.css* is appended to the end of the file.</span></span>

## <a name="environment-based-bundling-and-minification"></a><span data-ttu-id="6b0c8-219">Tworzenie i minifikacja oparte na środowisku</span><span class="sxs-lookup"><span data-stu-id="6b0c8-219">Environment-based bundling and minification</span></span>

<span data-ttu-id="6b0c8-220">Najlepszym rozwiązaniem jest użycie w środowisku produkcyjnym plików z pakietem i zminimalizowanego aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6b0c8-220">As a best practice, the bundled and minified files of your app should be used in a production environment.</span></span> <span data-ttu-id="6b0c8-221">Podczas opracowywania oryginalne pliki ułatwiają debugowanie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6b0c8-221">During development, the original files make for easier debugging of the app.</span></span>

<span data-ttu-id="6b0c8-222">Określ pliki do uwzględnienia na stronach przy użyciu [pomocnika tagów środowiska](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) w widokach.</span><span class="sxs-lookup"><span data-stu-id="6b0c8-222">Specify which files to include in your pages by using the [Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) in your views.</span></span> <span data-ttu-id="6b0c8-223">Pomocnik tagów środowiska renderuje jego zawartość tylko w przypadku uruchamiania w określonych [środowiskach](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="6b0c8-223">The Environment Tag Helper only renders its contents when running in specific [environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="6b0c8-224">Poniższy tag `environment` renderuje nieprzetworzone pliki CSS podczas działania w środowisku `Development`:</span><span class="sxs-lookup"><span data-stu-id="6b0c8-224">The following `environment` tag renders the unprocessed CSS files when running in the `Development` environment:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=21-24)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=9-12)]

::: moniker-end

<span data-ttu-id="6b0c8-225">Poniższy tag `environment` renderuje powiązane i zminimalizowanego pliki CSS, gdy działa w środowisku innym niż `Development`.</span><span class="sxs-lookup"><span data-stu-id="6b0c8-225">The following `environment` tag renders the bundled and minified CSS files when running in an environment other than `Development`.</span></span> <span data-ttu-id="6b0c8-226">Na przykład uruchomienie w `Production` lub `Staging` wyzwala renderowanie tych arkuszy stylów:</span><span class="sxs-lookup"><span data-stu-id="6b0c8-226">For example, running in `Production` or `Staging` triggers the rendering of these stylesheets:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=5&range=25-30)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=13-18)]

::: moniker-end

## <a name="consume-bundleconfigjson-from-gulp"></a><span data-ttu-id="6b0c8-227">Korzystanie z bundleconfig. JSON z Gulp</span><span class="sxs-lookup"><span data-stu-id="6b0c8-227">Consume bundleconfig.json from Gulp</span></span>

<span data-ttu-id="6b0c8-228">Istnieją przypadki, w których aplikacja i przepływy pracy minifikacja aplikacji wymagają dodatkowego przetwarzania.</span><span class="sxs-lookup"><span data-stu-id="6b0c8-228">There are cases in which an app's bundling and minification workflow requires additional processing.</span></span> <span data-ttu-id="6b0c8-229">Przykładami są Optymalizacja obrazu, Busting pamięci podręcznej i przetwarzanie zasobów sieci CDN.</span><span class="sxs-lookup"><span data-stu-id="6b0c8-229">Examples include image optimization, cache busting, and CDN asset processing.</span></span> <span data-ttu-id="6b0c8-230">Aby spełnić te wymagania, można skonwertować przepływ pracy tworzenia i minifikacja w celu użycia Gulp.</span><span class="sxs-lookup"><span data-stu-id="6b0c8-230">To satisfy these requirements, you can convert the bundling and minification workflow to use Gulp.</span></span>

### <a name="use-the-bundler--minifier-extension"></a><span data-ttu-id="6b0c8-231">Użyj pakietu & rozszerzenia Minifier</span><span class="sxs-lookup"><span data-stu-id="6b0c8-231">Use the Bundler & Minifier extension</span></span>

<span data-ttu-id="6b0c8-232">Pakiet Visual Studio [pakietu & rozszerzenia Minifier](https://marketplace.visualstudio.com/items?itemName=MadsKristensen.BundlerMinifier) obsługuje konwersję do Gulp.</span><span class="sxs-lookup"><span data-stu-id="6b0c8-232">The Visual Studio [Bundler & Minifier](https://marketplace.visualstudio.com/items?itemName=MadsKristensen.BundlerMinifier) extension handles the conversion to Gulp.</span></span>

> [!NOTE]
> <span data-ttu-id="6b0c8-233">Pakiet & rozszerzenie Minifier należy do projektu opartego na społeczności w witrynie GitHub, dla którego firma Microsoft nie zapewnia pomocy technicznej.</span><span class="sxs-lookup"><span data-stu-id="6b0c8-233">The Bundler & Minifier extension belongs to a community-driven project on GitHub for which Microsoft provides no support.</span></span> <span data-ttu-id="6b0c8-234">[Tutaj](https://github.com/madskristensen/BundlerMinifier/issues)należy zgłosić problemy.</span><span class="sxs-lookup"><span data-stu-id="6b0c8-234">Issues should be filed [here](https://github.com/madskristensen/BundlerMinifier/issues).</span></span>

<span data-ttu-id="6b0c8-235">Kliknij prawym przyciskiem myszy plik *bundleconfig. JSON* w Eksplorator rozwiązań i wybierz pozycję **pakiet & Minifier** > **Konwertuj na Gulp...** :</span><span class="sxs-lookup"><span data-stu-id="6b0c8-235">Right-click the *bundleconfig.json* file in Solution Explorer and select **Bundler & Minifier** > **Convert To Gulp...**:</span></span>

![Konwertuj na element menu kontekstowego Gulp](../client-side/bundling-and-minification/_static/convert-to-gulp.png)

<span data-ttu-id="6b0c8-237">Pliki *Gulpfile. js* i *Package. JSON* są dodawane do projektu.</span><span class="sxs-lookup"><span data-stu-id="6b0c8-237">The *gulpfile.js* and *package.json* files are added to the project.</span></span> <span data-ttu-id="6b0c8-238">Zainstalowano pomocnicze pakiety [npm](https://www.npmjs.com/) wymienione w sekcji `devDependencies` pliku *Package. JSON* .</span><span class="sxs-lookup"><span data-stu-id="6b0c8-238">The supporting [npm](https://www.npmjs.com/) packages listed in the *package.json* file's `devDependencies` section are installed.</span></span>

<span data-ttu-id="6b0c8-239">Uruchom następujące polecenie w oknie PMC, aby zainstalować interfejs wiersza polecenia Gulp jako zależność globalną:</span><span class="sxs-lookup"><span data-stu-id="6b0c8-239">Run the following command in the PMC window to install the Gulp CLI as a global dependency:</span></span>

```console
npm i -g gulp-cli
```

<span data-ttu-id="6b0c8-240">Plik *Gulpfile. js* odczytuje plik *bundleconfig. JSON* dla danych wejściowych, wyjściowych i ustawień.</span><span class="sxs-lookup"><span data-stu-id="6b0c8-240">The *gulpfile.js* file reads the *bundleconfig.json* file for the inputs, outputs, and settings.</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-12&highlight=10)]

### <a name="convert-manually"></a><span data-ttu-id="6b0c8-241">Konwertuj ręcznie</span><span class="sxs-lookup"><span data-stu-id="6b0c8-241">Convert manually</span></span>

<span data-ttu-id="6b0c8-242">Jeśli program Visual Studio i/lub pakiet & rozszerzenia Minifier nie są dostępne, przekonwertuj go ręcznie.</span><span class="sxs-lookup"><span data-stu-id="6b0c8-242">If Visual Studio and/or the Bundler & Minifier extension aren't available, convert manually.</span></span>

<span data-ttu-id="6b0c8-243">Dodaj plik *Package. JSON* z następującymi `devDependencies`do katalogu głównego projektu:</span><span class="sxs-lookup"><span data-stu-id="6b0c8-243">Add a *package.json* file, with the following `devDependencies`, to the project root:</span></span>

> [!WARNING]
> <span data-ttu-id="6b0c8-244">Moduł `gulp-uglify` nie obsługuje ECMAScript (ES) 2015/ES6 i nowszych.</span><span class="sxs-lookup"><span data-stu-id="6b0c8-244">The `gulp-uglify` module doesn't support ECMAScript (ES) 2015 / ES6 and later.</span></span> <span data-ttu-id="6b0c8-245">Zainstaluj [Gulp-Terser](https://www.npmjs.com/package/gulp-terser) zamiast `gulp-uglify`, aby użyć ES2015/ES6 lub nowszego.</span><span class="sxs-lookup"><span data-stu-id="6b0c8-245">Install [gulp-terser](https://www.npmjs.com/package/gulp-terser) instead of `gulp-uglify` to use ES2015 / ES6 or later.</span></span>

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/package.json?range=5-13)]

<span data-ttu-id="6b0c8-246">Zainstaluj zależności, uruchamiając następujące polecenie na tym samym poziomie, co plik *Package. JSON*:</span><span class="sxs-lookup"><span data-stu-id="6b0c8-246">Install the dependencies by running the following command at the same level as *package.json*:</span></span>

```console
npm i
```

<span data-ttu-id="6b0c8-247">Zainstaluj interfejs wiersza polecenia Gulp jako zależność globalną:</span><span class="sxs-lookup"><span data-stu-id="6b0c8-247">Install the Gulp CLI as a global dependency:</span></span>

```console
npm i -g gulp-cli
```

<span data-ttu-id="6b0c8-248">Skopiuj plik *Gulpfile. js* poniżej do katalogu głównego projektu:</span><span class="sxs-lookup"><span data-stu-id="6b0c8-248">Copy the *gulpfile.js* file below to the project root:</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-11,14-)]

### <a name="run-gulp-tasks"></a><span data-ttu-id="6b0c8-249">Uruchamianie zadań Gulp</span><span class="sxs-lookup"><span data-stu-id="6b0c8-249">Run Gulp tasks</span></span>

<span data-ttu-id="6b0c8-250">Aby wyzwolić zadanie Gulp minifikacja przed kompilacją projektu w programie Visual Studio, Dodaj następujący [obiekt docelowy programu MSBuild](/visualstudio/msbuild/msbuild-targets) do pliku \*. csproj:</span><span class="sxs-lookup"><span data-stu-id="6b0c8-250">To trigger the Gulp minification task before the project builds in Visual Studio, add the following [MSBuild Target](/visualstudio/msbuild/msbuild-targets) to the \*.csproj file:</span></span>

[!code-xml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/BuildBundlerMinifierApp.csproj?range=14-16)]

<span data-ttu-id="6b0c8-251">W tym przykładzie wszystkie zadania zdefiniowane w `MyPreCompileTarget` celu są uruchamiane przed wstępnie zdefiniowanym elementem docelowym `Build`.</span><span class="sxs-lookup"><span data-stu-id="6b0c8-251">In this example, any tasks defined within the `MyPreCompileTarget` target run before the predefined `Build` target.</span></span> <span data-ttu-id="6b0c8-252">Dane wyjściowe podobne do następujących pojawiają się w oknie danych wyjściowych programu Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="6b0c8-252">Output similar to the following appears in Visual Studio's Output window:</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="6b0c8-253">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="6b0c8-253">Additional resources</span></span>

* [<span data-ttu-id="6b0c8-254">Korzystanie z Grunt</span><span class="sxs-lookup"><span data-stu-id="6b0c8-254">Use Grunt</span></span>](xref:client-side/using-grunt)
* [<span data-ttu-id="6b0c8-255">Używanie wielu środowisk</span><span class="sxs-lookup"><span data-stu-id="6b0c8-255">Use multiple environments</span></span>](xref:fundamentals/environments)
* [<span data-ttu-id="6b0c8-256">Pomocnicy tagów</span><span class="sxs-lookup"><span data-stu-id="6b0c8-256">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
