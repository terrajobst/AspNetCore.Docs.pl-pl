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
ms.openlocfilehash: a3d49315fbb62eb1a42eb1b30885dc19a81c0a91
ms.sourcegitcommit: 9bc34b8269d2a150b844c3b8646dcb30278a95ea
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/12/2018
---
# <a name="bundle-and-minifiy-static-assets-in-aspnet-core"></a>Pakiet i minifiy zasoby statyczne platformy ASP.NET Core

Przez [Scott Addie](https://twitter.com/Scott_Addie)

W tym artykule opisano zalety stosowania tworzenie pakietów i minimalizowanie, łącznie z tych funkcji możliwości korzystania z aplikacji sieci web platformy ASP.NET Core.

## <a name="what-is-bundling-and-minification"></a>Co to jest tworzenie pakietów i minimalizowanie?

Tworzenie pakietów i minimalizowanie są dwa optymalizacji wydajności distinct, które można zastosować w aplikacji sieci web. Używane razem, tworzenie pakietów i minimalizowanie zwiększyć wydajność przez zmniejszenie liczby żądań serwera oraz redukcję rozmiaru żądanych zasobów statycznych.

Tworzenie pakietów i minimalizowanie głównie zwiększyć czas ładowania pierwsze żądanie strony. Gdy zażądano strony sieci web, przeglądarka buforuje zasoby statyczne (JavaScript, CSS i obrazy). W związku z tym tworzenie pakietów i minimalizowanie nie zwiększyć wydajność podczas żądania tej samej stronie lub strony w tej samej witrynie żąda tego samego zasoby. Jeśli wygasa nagłówka nie jest poprawnie ustawiona na zasoby i tworzenie pakietów i minimalizowanie nie jest używany algorytm heurystyczny świeżości przeglądarki oznaczyć zasoby starych po upływie kilku dni. Ponadto przeglądarka wymaga żądanie sprawdzania poprawności dla każdego zasobu. W takim przypadku tworzenie pakietów i minimalizowanie zapewnić lepszą wydajność, nawet po pierwszym żądaniu strony.

### <a name="bundling"></a>Tworzenie pakietów

Tworzenie pakietów łączy wiele plików do pojedynczego pliku. Tworzenie pakietów zmniejsza liczbę żądań serwera, które są niezbędne do renderowania zawartości sieci web, takich jak strony sieci web. Można utworzyć dowolną liczbę poszczególnych pakietów specjalnie z myślą o CSS, JavaScript,... itd. Mniej plików oznacza mniejszą liczbę żądań HTTP z przeglądarki do serwera lub usługi, zapewniając aplikacji. Powoduje to wyższą wydajność obciążenia w usłudze pierwszej strony.

### <a name="minification"></a>Minimalizowanie

Minimalizowanie usuwa niepotrzebne znaki z kodu bez zmiany funkcji. Wynik jest zmniejszenie rozmiaru znaczących żądanych zasobów (takich jak CSS, obrazy i pliki JavaScript). Typowe efektami ubocznymi minimalizację obejmują skrócić nazwy zmiennych o jeden znak i usuwanie komentarzy i niepotrzebne odstępu.

Należy wziąć pod uwagę następujące funkcji JavaScript:

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.js)]

Minimalizowanie zmniejsza funkcji do następującego:

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.min.js)]

Oprócz usuwanie komentarzy i niepotrzebne odstępu, następujące nazwy parametrów i zmiennych nazwy zostały zmienione w następujący sposób:

Oryginał | Zmieniono jego nazwę
--- | :---:
`imageTagAndImageID` | `t`
`imageContext` | `a`
`imageElement` | `r`

## <a name="impact-of-bundling-and-minification"></a>Wpływ na tworzenie pakietów i minimalizowanie

W poniższej tabeli przedstawiono różnice między indywidualnie zasoby oraz za pomocą tworzenie pakietów i minimalizowanie:

Akcja | Z B/M | Bez B/M | Zmiana
--- | :---: | :---: | :---:
Żądań plików  | 7   | 18     | 157%
KB transferu | 156 | 264.68 | 70%
Czas ładowania (ms) | 885 | 2360   | 167%

Przeglądarek jest dość pełne względem nagłówków żądań HTTP. Całkowita liczba bajtów wysłane Metryka wyświetlony znaczne obniżenie zużycia przy tworzeniu pakietów. Czas ładowania pokazuje znaczne ulepszenia, jednak w tym przykładzie został uruchomiony lokalnie. Większa wzrost wydajności są realizowane przy użyciu zasobów tworzenie pakietów i minimalizowanie przesyłanych przez sieć.

## <a name="choose-a-bundling-and-minification-strategy"></a>Wybierz strategię tworzenie pakietów i minimalizowanie

Szablony projektów MVC i stron Razor stanowią rozwiązanie poza pole dla tworzenie pakietów i minimalizowanie składające się z plikiem konfiguracji JSON. Narzędzia innych firm, takich jak [system Gulp](xref:client-side/using-gulp) i [Grunt](xref:client-side/using-grunt) zadanie uczestników, wykonać te same zadania z nieco bardziej złożona. Narzędzia innej firmy jest doskonałym dopasowania, gdy przepływ pracy tworzenia wymaga przetwarzania poza tworzenie pakietów i minimalizowanie&mdash;takie jak Optymalizacja linting i obrazów. Za pomocą tworzenie pakietów i minimalizowanie czasu projektowania, zminimalizowany pliki zostaną utworzone przed wdrożeniem aplikacji. Tworzenie pakietów i zminimalizowania liczby przed przystąpieniem do wdrożenia zawiera zaletą serwera zmniejszenie obciążenia. Jednak ważne jest, aby rozpoznać tego czasu projektowania tworzenie pakietów i minimalizowanie zwiększa złożoności kompilacji i działa tylko w przypadku plików statycznych.

## <a name="configure-bundling-and-minification"></a>Konfigurowanie, tworzenie pakietów i minimalizowanie

Szablony projektów MVC i stron Razor zapewniają *bundleconfig.json* pliku konfiguracji, który definiuje opcje dla każdego pakietu. Domyślnie konfiguracja pojedynczy pakiet jest zdefiniowany dla niestandardowych skryptów JavaScript (*wwwroot/js/site.js*) i arkusza stylów (*wwwroot/css/site.css*) plików:

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig.json)]

Opcje konfiguracji obejmują:

* `outputFileName`: Nazwa pliku pakietu do danych wyjściowych. Może zawierać ścieżki względnej z *bundleconfig.json* pliku. **Wymagane**
* `inputFiles`: Tablica plików do łączenia się ze sobą. Są to względne ścieżki do pliku konfiguracji. **opcjonalne**, * pustą wartość wyniki w pliku wyjściowym puste. [Globbing](http://www.tldp.org/LDP/abs/html/globbingref.html) wzorce są obsługiwane.
* `minify`: Minimalizację opcje dla typu danych wyjściowych. **opcjonalne**, *domyślnie — `minify: { enabled: true }`*
  * Opcje konfiguracji są dostępne na typ pliku wyjściowego.
    * [Element minimalizujący CSS](https://github.com/madskristensen/BundlerMinifier/wiki/cssminifier)
    * [JavaScript Minifier](https://github.com/madskristensen/BundlerMinifier/wiki/JavaScript-Minifier-settings)
    * [Element minimalizujący HTML](https://github.com/madskristensen/BundlerMinifier/wiki)
* `includeInProject`: Flaga oznaczająca, czy dodać pliki wygenerowane do pliku projektu. **opcjonalne**, *domyślne — FAŁSZ.*
* `sourceMap`: Flaga oznaczająca, czy można wygenerować mapy źródła dla pliku powiązane. **opcjonalne**, *domyślne — FAŁSZ.*
* `sourceMapRootPath`Ścieżka katalogu głównego do przechowywania pliku mapy wygenerowanego źródła.

## <a name="build-time-execution-of-bundling-and-minification"></a>Tworzenie pakietów i minimalizowanie wykonywanie czas kompilacji

[BuildBundlerMinifier](https://www.nuget.org/packages/BuildBundlerMinifier/) pakietu NuGet umożliwia wykonywanie tworzenie pakietów i minimalizowanie podczas kompilacji. Pakiet injects [docelowych elementów MSBuild](/visualstudio/msbuild/msbuild-targets) uruchamiania kompilacji i wyczyść godzinie. *Bundleconfig.json* plików są analizowane przez proces kompilacji, aby utworzyć pliki wyjściowe na podstawie zdefiniowanych konfiguracji.

> [!NOTE]
> BuildBundlerMinifier należy do projektu społeczność w witrynie GitHub, dla którego Microsoft nie zapewnia obsługi. Powinny być zgłaszane problemy [tutaj](https://github.com/madskristensen/BundlerMinifier/issues).

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio) 

Dodaj *BuildBundlerMinifier* pakietu do projektu.

Skompiluj projekt. Poniżej jest wyświetlany w oknie danych wyjściowych:

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

Czyszczenie projektu. Poniżej jest wyświetlany w oknie danych wyjściowych:

```console
1>------ Clean started: Project: BuildBundlerMinifierApp, Configuration: Debug Any CPU ------
1>
1>Bundler: Cleaning output from bundleconfig.json
1>Bundler: Done cleaning output file from bundleconfig.json
========== Clean: 1 succeeded, 0 failed, 0 skipped ==========
```

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli) 

Dodaj *BuildBundlerMinifier* pakietu do projektu:

```console
dotnet add package BuildBundlerMinifier
```

Jeśli przy użyciu platformy ASP.NET Core 1.x, Przywróć nowo dodany pakiet:

```console
dotnet restore
```

Skompiluj projekt:

```console
dotnet build
```

Pojawia się poniżej:

```console
Microsoft (R) Build Engine version 15.4.8.50001 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.


    Bundler: Begin processing bundleconfig.json
    Bundler: Done processing bundleconfig.json
    BuildBundlerMinifierApp -> C:\BuildBundlerMinifierApp\bin\Debug\netcoreapp2.0\BuildBundlerMinifierApp.dll
```

Czyszczenie projektu:

```console
dotnet clean
```

Zostaną wyświetlone następujące dane wyjściowe:

```console
Microsoft (R) Build Engine version 15.4.8.50001 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.


  Bundler: Cleaning output from bundleconfig.json
  Bundler: Done cleaning output file from bundleconfig.json
```

---

## <a name="ad-hoc-execution-of-bundling-and-minification"></a>Ad hoc wykonywanie tworzenie pakietów i minimalizowanie

Istnieje możliwość uruchomienia zadania Tworzenie pakietów i minimalizowanie na zasadzie ad hoc bez skompilowanie projektu. Dodaj [BundlerMinifier.Core](https://www.nuget.org/packages/BundlerMinifier.Core/) pakiet NuGet do projektu:

[!code-xml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/BuildBundlerMinifierApp.csproj?range=10)]

> [!NOTE]
> BundlerMinifier.Core należy do projektu społeczność w witrynie GitHub, dla którego Microsoft nie zapewnia obsługi. Powinny być zgłaszane problemy [tutaj](https://github.com/madskristensen/BundlerMinifier/issues).

Ten pakiet rozszerza .NET Core interfejsu wiersza polecenia do uwzględnienia *pakietu dotnet* narzędzia. Polecenie można wykonać w oknie konsoli Menedżera pakietów (PMC) lub w powłoce poleceń:

```console
dotnet bundle
```

> [!IMPORTANT]
> Menedżer pakietów NuGet dodaje zależności do pliku *.csproj jako `<PackageReference />` węzłów. `dotnet bundle` Polecenia został zarejestrowany za pomocą interfejsu wiersza polecenia platformy .NET Core tylko wtedy, gdy `<DotNetCliToolReference />` węzła jest używany. Zmodyfikuj odpowiednio *.csproj pliku.

## <a name="add-files-to-workflow"></a>Dodaj pliki do przepływu pracy

Rozważ przykład, w którym dodatkowe *custome.CSS* zostanie dodany plik podobne do następujących:

[!code-css[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/css/custom.css)]

Do zminimalizowania *custome.CSS* i połączyć ją z *site.css* do *site.min.css* plików, Dodaj ścieżkę względną do *bundleconfig.json*:

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig2.json?highlight=6)]

> [!NOTE]
> Alternatywnie można użyć następującego wzorca globbing:
>
> ```json
> "inputFiles": ["wwwroot/**/*(*.css|!(*.min.css)"]
> ```
>
> Ten wzorzec globbing dopasowanie wszystkich plików CSS i wyklucza wzorzec pliku zminimalizowany.

Tworzenie aplikacji. Otwórz *site.min.css* i zwróć uwagę, zawartość *custome.CSS* jest dołączany na końcu pliku.

## <a name="environment-based-bundling-and-minification"></a>Oparte na środowisku tworzenie pakietów i minimalizowanie

Najlepszym rozwiązaniem pliki powiązane i zminimalizowany aplikacji używanego w środowisku produkcyjnym. Podczas tworzenia oryginalnych plików upewnij się łatwiejsze debugowania aplikacji.

Określ pliki, których ma obejmować na swoich stronach za pomocą [pomocnika Tag środowiska](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) w widoków. Pomocnik Tag środowiska renderuje tylko jego zawartość podczas uruchamiania w określonej [środowisk](xref:fundamentals/environments).

Następujące `environment` tag renderuje nieprzetworzone pliki CSS w `Development` środowiska:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=21-24)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=9-12)]

---

Następujące `environment` tag renderuje pliki CSS powiązane i zminimalizowany podczas uruchamiania w środowisku innym niż `Development`. Na przykład uruchomiona `Production` lub `Staging` wyzwala renderowania tych arkuszy stylów:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=5&range=25-30)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=13-18)]

---

## <a name="consume-bundleconfigjson-from-gulp"></a>Korzystanie z bundleconfig.json z Gulp

Istnieją przypadki, w których aplikacja tworzenie pakietów i minimalizowanie przepływ pracy wymaga dodatkowego przetwarzania. Przykładami Optymalizacja obrazu, rozrywające pamięci podręcznej i przetwarzania zasobów w sieci CDN. Aby spełnić te wymagania, można przekonwertować tworzenie pakietów i minimalizowanie przepływu pracy Użyj Gulp.

### <a name="use-the-bundler--minifier-extension"></a>Użyj rozszerzenia program instalujący niezamówione pakiety & element minimalizujący

Visual Studio [program instalujący niezamówione pakiety & element minimalizujący](https://marketplace.visualstudio.com/items?itemName=MadsKristensen.BundlerMinifier) rozszerzenie obsługuje konwersję Gulp.

> [!NOTE]
> Program instalujący niezamówione pakiety & element minimalizujący rozszerzenia należy do projektu społeczność w witrynie GitHub, dla którego Microsoft nie zapewnia obsługi. Powinny być zgłaszane problemy [tutaj](https://github.com/madskristensen/BundlerMinifier/issues).

Kliknij prawym przyciskiem myszy *bundleconfig.json* plików w Eksploratorze rozwiązań i wybierz **program instalujący niezamówione pakiety & element minimalizujący** > **Konwertuj do system Gulp...** :

![Konwertuj na system Gulp w menu kontekstowym](../client-side/bundling-and-minification/_static/convert-to-gulp.png)

*Gulpfile.js* i *package.json* pliki zostaną dodane do projektu. Obsługa [npm](https://www.npmjs.com/) pakiety wymienione w *package.json* pliku `devDependencies` sekcji są zainstalowane.

Uruchom następujące polecenie w oknie PMC, aby zainstalować system Gulp CLI jako zależność globalne:

```console
npm i -g gulp-cli
```

*Gulpfile.js* pliku odczyty *bundleconfig.json* wejść, wyjść i ustawienia w pliku.

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-12&highlight=10)]

### <a name="convert-manually"></a>Ręcznie konwertowanie

Jeśli program Visual Studio i/lub program instalujący niezamówione pakiety & element minimalizujący rozszerzenia nie są dostępne, przekonwertować ręcznie.

Dodaj *package.json* pliku następującym kodem `devDependencies`, katalog główny projektu:

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/package.json?range=5-13)]

Zainstaluj zależności, uruchamiając następujące polecenie na tym samym poziomie jako *package.json*:

```console
npm i
```

Zainstaluj interfejs wiersza polecenia system Gulp jako zależność globalne:

```console
npm i -g gulp-cli
```

Kopiuj *gulpfile.js* pliku poniżej w katalogu głównym projektu:

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-11,14-)]

### <a name="run-gulp-tasks"></a>Uruchom zadania Gulp

Aby wyzwolić zadanie minimalizowanie Gulp przed kompilacji projektu programu Visual Studio, Dodaj następujący [docelowy programu MSBuild](/visualstudio/msbuild/msbuild-targets) do pliku *.csproj:

[!code-xml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/BuildBundlerMinifierApp.csproj?range=14-16)]

W tym przykładzie wszystkie zadania zdefiniowany w ramach `MyPreCompileTarget` docelowych uruchomienia przed wstępnie zdefiniowane `Build` docelowej. W oknie danych wyjściowych programu Visual Studio zostaną wyświetlone dane wyjściowe podobne do następujących:

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

Alternatywnie Eksploratora modułu uruchamiającego zadania programu Visual Studio mogą służyć do powiązania zadania Gulp określonych zdarzeń programu Visual Studio. Zobacz [uruchomionych zadań domyślne](xref:client-side/using-gulp#running-default-tasks) instrukcje dotyczące operacją.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Korzystanie z Gulp](xref:client-side/using-gulp)
* [Korzystanie z Grunt](xref:client-side/using-grunt)
* [Używanie wielu środowisk](xref:fundamentals/environments)
* [Pomocnicy tagów](xref:mvc/views/tag-helpers/intro)
