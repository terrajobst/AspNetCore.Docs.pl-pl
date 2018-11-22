---
title: Tworzenie pakietów i minimalizowanie statycznych zasobów w programie ASP.NET Core
author: scottaddie
description: Dowiedz się, jak zoptymalizować zasoby statyczne w aplikacji sieci web platformy ASP.NET Core za pomocą metod minifikacji i tworzenia pakietów.
ms.author: scaddie
ms.custom: mvc
ms.date: 11/20/2018
uid: client-side/bundling-and-minification
ms.openlocfilehash: 5d5f0aadb7740c9b2b959d12a585cd8c91758ce8
ms.sourcegitcommit: 4225e2c49a0081e6ac15acff673587201f54b4aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/21/2018
ms.locfileid: "52282147"
---
# <a name="bundle-and-minify-static-assets-in-aspnet-core"></a>Tworzenie pakietów i minimalizowanie statycznych zasobów w programie ASP.NET Core

Przez [Scott Addie](https://twitter.com/Scott_Addie) i [sosny David](https://twitter.com/davidpine7)

W tym artykule wyjaśniono zalety stosowania tworzenie pakietów i minimalizowanie, w tym, jak można użyć tych funkcji w aplikacji sieci web platformy ASP.NET Core.

## <a name="what-is-bundling-and-minification"></a>Co to jest tworzenie pakietów i minimalizowanie

Tworzenie pakietów i minimalizowanie są dwa optymalizacji wydajności distinct, które można zastosować w aplikacji sieci web. Przy stosowaniu, tworzenie pakietów i minimalizowanie zwiększyć wydajność przez zmniejszenie liczby żądań serwera oraz redukcję rozmiaru żądanych zasobów statycznych.

Tworzenie pakietów i minimalizowanie przede wszystkim poprawić strony żądań obciążenia po raz pierwszy. Gdy wysłano żądanie strony sieci web, przeglądarka buforuje statycznych zasobów (JavaScript, CSS i obrazów). W związku z tym tworzenie pakietów i minimalizowanie nie poprawić wydajność podczas żądania tej samej stronie lub stron, w tej samej lokacji żądania tych samych zasobów. Jeśli wygasa nagłówka nie jest prawidłowo ustawione na zasoby i tworzenie pakietów i minimalizowanie nie jest używany algorytm heurystyczny świeżości przeglądarki oznaczyć zasoby starych po upływie kilku dni. Ponadto przeglądarka wymaga żądanie weryfikacji dla każdego zasobu. W tym przypadku tworzenie pakietów i minimalizowanie zapewniają lepszą wydajność, nawet po pierwszym żądaniu strony.

### <a name="bundling"></a>Tworzenie pakietów

Tworzenie pakietów pozwala łączyć wiele plików w jednym pliku. Tworzenie pakietów pozwala zmniejszyć liczbę żądań serwera, które są niezbędne do renderowania elementu zawartości sieci web, takich jak strony sieci web. Można utworzyć dowolną liczbę poszczególnych pakietów specjalnie dla CSS, JavaScript, itp. Mniej plików oznacza mniejszą liczbę żądań HTTP z przeglądarki do serwera lub usługę, zapewniając aplikacji. Powoduje to zwiększona wydajność pod obciążeniem pierwszej strony.

### <a name="minification"></a>Minimalizowanie

Minifikacja umożliwia usunięcie niepotrzebnych znaków z kodu, bez zmiany funkcjonalności. Wynik jest zmniejszenie o znacznym rozmiarze żądanych zasobów (takich jak CSS, obrazów i plików JavaScript). Typowe efekty uboczne minimalizację obejmują zmniejszenie nazwy zmiennych, jeden znak i usuwanie komentarzy i niepotrzebnych odstępów.

Należy wziąć pod uwagę następujące funkcja języka JavaScript:

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.js)]

Minimalizowanie zmniejsza funkcji do następujących:

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.min.js)]

Oprócz usuwanie komentarzy i niepotrzebnych odstępów, następujące nazwy parametrów i zmiennych zostały zmienione w następujący sposób:

Oryginał | Zmieniono nazwę
--- | :---:
`imageTagAndImageID` | `t`
`imageContext` | `a`
`imageElement` | `r`

## <a name="impact-of-bundling-and-minification"></a>Wpływ tworzenie pakietów i minimalizowanie

W poniższej tabeli przedstawiono różnice między indywidualnie zasoby oraz za pomocą tworzenie pakietów i minimalizowanie:

Akcja | Za pomocą B/M | Bez B/M | Zmiana
--- | :---: | :---: | :---:
Żądań plików  | 7   | 18     | 157%
KB przesłane | 156 | 264.68 | 70%
Czas ładowania (ms) | 885 | 2360   | 167%

Przeglądarki są dość pełne w odniesieniu do nagłówków żądań HTTP. Całkowita liczba bajtów wysłano metryki dostrzegła znaczące zmniejszenie podczas tworzenia pakietów. Czas ładowania pokazuje wprowadzono znaczące ulepszenia, jednak w tym przykładzie został uruchomiony lokalnie. Większa wzrost wydajności są realizowane przy użyciu tworzenie pakietów i minimalizowanie z zasobami przesyłana przez sieć.

## <a name="choose-a-bundling-and-minification-strategy"></a>Wybierz strategię tworzenie pakietów i minimalizowanie

Szablony projektów MVC i stron Razor zapewniają rozwiązanie poza pole do tworzenie pakietów i minimalizowanie składający się z pliku konfiguracji JSON. Narzędzia innych firm, takich jak [Gulp](xref:client-side/using-gulp) i [Grunt](xref:client-side/using-grunt) task Runner, należy wykonać te same zadania przy użyciu bardziej złożoności. Narzędzia innych firm jest doskonałym rozwiązaniem, gdy przepływie pracy wymaga przetwarzania poza tworzenie pakietów i minimalizowanie&mdash;takich jak Optymalizacja Zaznaczanie błędów i obrazów. Za pomocą tworzenie pakietów i minimalizowanie czasu projektowania, zminimalizowany pliki zostaną utworzone przed wdrożeniem aplikacji. Tworzenie pakietów i minifikacja przed przystąpieniem do wdrożenia zawiera dzięki zmniejszeniu obciążenia. Jednak ważne jest, aby rozpoznać tego czasu projektowania tworzenie pakietów i minimalizowanie zwiększa złożonością kompilacji i działa tylko z plikami statycznymi.

## <a name="configure-bundling-and-minification"></a>Skonfiguruj tworzenie pakietów i minimalizowanie

::: moniker range="<= aspnetcore-2.0"

W programie ASP.NET Core 2.0 lub wcześniej, szablony projektu MVC i stron Razor zapewniają *bundleconfig.json* pliku konfiguracji, który definiuje opcje dla każdego pakietu:

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

W programie ASP.NET Core 2.1 lub nowszej, Dodaj nowy plik JSON o nazwie *bundleconfig.json*, w katalogu głównym projektu MVC ani stron Razor. Dołącz następujące dane JSON do tego pliku jako punkt początkowy:

::: moniker-end

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig.json)]

*Bundleconfig.json* plik definiuje opcje dla każdego pakietu. W powyższym przykładzie konfiguracja pojedynczy pakiet jest zdefiniowana dla niestandardowych skryptów JavaScript (*wwwroot/js/site.js*) i arkusza stylów (*wwwroot/css/site.css*) plików.

Opcje konfiguracji obejmują:

* `outputFileName`: Nazwa pliku pakietu w danych wyjściowych. Może zawierać ścieżki względnej z *bundleconfig.json* pliku. **Wymagane**
* `inputFiles`: Tablica plików, aby powiązać razem. To są ścieżki względne do pliku konfiguracji. **Opcjonalnie**, * wartość pustą wyniki w pliku wyjściowym puste. [symboli wieloznacznych](http://www.tldp.org/LDP/abs/html/globbingref.html) wzorce są obsługiwane.
* `minify`: Minimalizację opcje dla typu danych wyjściowych. **Opcjonalnie**, *domyślnie — `minify: { enabled: true }`*
  * Opcje konfiguracji są dostępne na typ pliku wyjściowego.
    * [Element minimalizujący CSS](https://github.com/madskristensen/BundlerMinifier/wiki/cssminifier)
    * [JavaScript Minifier](https://github.com/madskristensen/BundlerMinifier/wiki/JavaScript-Minifier-settings)
    * [Element minimalizujący HTML](https://github.com/madskristensen/BundlerMinifier/wiki)
* `includeInProject`: Flaga oznaczająca, czy dodać wygenerowanych plików do pliku projektu. **Opcjonalnie**, *ustawienie domyślne — FAŁSZ.*
* `sourceMap`: Flaga oznaczająca, czy można wygenerować mapy źródła dla plików powiązane. **Opcjonalnie**, *ustawienie domyślne — FAŁSZ.*
* `sourceMapRootPath`: Ścieżka katalogu głównego do przechowywania pliku mapy wygenerowanego źródła.

## <a name="build-time-execution-of-bundling-and-minification"></a>Czas kompilacji wykonywanie tworzenie pakietów i minimalizowanie

[BuildBundlerMinifier](https://www.nuget.org/packages/BuildBundlerMinifier/) pakietu NuGet umożliwia wykonywanie tworzenie pakietów i minimalizowanie w czasie kompilacji. Wprowadza pakietu [elementów docelowych MSBuild](/visualstudio/msbuild/msbuild-targets) uruchamiania kompilacji i wyczyść godzinie. *Bundleconfig.json* analizy pliku przez proces kompilacji, aby utworzyć pliki wyjściowe na podstawie zdefiniowanej konfiguracji.

> [!NOTE]
> BuildBundlerMinifier należy do tworzonych metodą społecznościową projekt w witrynie GitHub, dla którego firmy Microsoft nie zapewnia obsługi. Powinny być zgłaszane problemy [tutaj](https://github.com/madskristensen/BundlerMinifier/issues).

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

Dodaj *BuildBundlerMinifier* do projektu pakiet:

```console
dotnet add package BuildBundlerMinifier
```

Jeśli za pomocą programu ASP.NET Core 1.x, Przywracanie pakietu nowo dodane:

```console
dotnet restore
```

Skompilować projekt:

```console
dotnet build
```

Zostanie wyświetlone następujące okno:

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

Zostanie wyświetlone następujące dane wyjściowe:

```console
Microsoft (R) Build Engine version 15.4.8.50001 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.


  Bundler: Cleaning output from bundleconfig.json
  Bundler: Done cleaning output file from bundleconfig.json
```

---

## <a name="ad-hoc-execution-of-bundling-and-minification"></a>Ad hoc wykonywanie tworzenie pakietów i minimalizowanie

Istnieje możliwość uruchamiania zadań tworzenie pakietów i minimalizowanie na zasadzie ad hoc, bez konieczności tworzenia projektu. Dodaj [BundlerMinifier.Core](https://www.nuget.org/packages/BundlerMinifier.Core/) pakiet NuGet do projektu:

[!code-xml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/BuildBundlerMinifierApp.csproj?range=10)]

> [!NOTE]
> BundlerMinifier.Core należy do tworzonych metodą społecznościową projekt w witrynie GitHub, dla którego firmy Microsoft nie zapewnia obsługi. Powinny być zgłaszane problemy [tutaj](https://github.com/madskristensen/BundlerMinifier/issues).

Ten pakiet rozszerza platformy .NET Core interfejsu wiersza polecenia obejmujący *pakietu dotnet* narzędzia. Mogą być wykonywane poniższe polecenie, w oknie Konsola Menedżera pakietów (PMC) lub w powłoce poleceń:

```console
dotnet bundle
```

> [!IMPORTANT]
> Menedżer pakietów NuGet dodaje współzależności plikowi *.csproj jako `<PackageReference />` węzłów. `dotnet bundle` Polecenia jest zarejestrowana przy użyciu interfejsu wiersza polecenia platformy .NET Core tylko wtedy, gdy `<DotNetCliToolReference />` węzła jest używany. Odpowiednio zmodyfikować plik *.csproj.

## <a name="add-files-to-workflow"></a>Dodaj pliki do przepływu pracy

Rozważmy przykład, w którym dodatkowe *custome.CSS* plik zostanie dodany, podobne do następujących:

[!code-css[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/css/custom.css)]

Do zminimalizowania *custome.CSS* i połączyć ją z *site.css* do *site.min.css* Dodaj ścieżkę względną do *bundleconfig.json*:

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig2.json?highlight=6)]

> [!NOTE]
> Alternatywnie można użyć następującego wzorca obsługi symboli wieloznacznych:
>
> ```json
> "inputFiles": ["wwwroot/**/*(*.css|!(*.min.css))"]
> ```
>
> Ten wzorzec symboli wieloznacznych uwzględnia wszystkie pliki CSS i nie obejmuje wzorzec zminimalizowanego pliku.

Skompiluj aplikację. Otwórz *site.min.css* i zwróć uwagę, zawartość *custome.CSS* jest dołączany na końcu pliku.

## <a name="environment-based-bundling-and-minification"></a>Oparte na środowisku tworzenie pakietów i minimalizowanie

Najlepszym rozwiązaniem jest pliki powiązane i zminimalizowany aplikacji stosuje się w środowisku produkcyjnym. Podczas tworzenia aplikacji oryginalne pliki należy łatwiejsze debugowanie aplikacji.

Określić, które pliki do uwzględnienia na stronach sieci przy użyciu [Pomocnik tagu środowiska](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) w widoków. Pomocnik tagu środowiska tylko powoduje wyświetlenie jej zawartości podczas pracy w określonych [środowisk](xref:fundamentals/environments).

Następujące `environment` tag renderuje nieprzetworzone pliki CSS, podczas uruchamiania w `Development` środowiska:

::: moniker range=">= aspnetcore-2.0"

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=21-24)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=9-12)]

::: moniker-end

Następujące `environment` tag renderuje pliki CSS powiązane i zminimalizowany podczas uruchamiania w środowiskach innych niż `Development`. Na przykład, działające w `Production` lub `Staging` wyzwala renderowania te arkusze stylów:

::: moniker range=">= aspnetcore-2.0"

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=5&range=25-30)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=13-18)]

::: moniker-end

## <a name="consume-bundleconfigjson-from-gulp"></a>Używanie bundleconfig.json z Gulp

Istnieją przypadki, w których aplikacja tworzenie pakietów i minimalizowanie przepływ pracy wymaga dodatkowego przetwarzania. Przykłady obejmują optymalizacji obrazu, rozrywające pamięci podręcznej i przetwarzanie zasobów w sieci CDN. Aby spełnić te wymagania, możesz przekonwertować tworzenie pakietów i minimalizowanie przepływu pracy korzystanie z Gulp.

### <a name="use-the-bundler--minifier-extension"></a>Za pomocą narzędzia Bundler & Minifier rozszerzenia

Visual Studio [narzędzia Bundler & Minifier](https://marketplace.visualstudio.com/items?itemName=MadsKristensen.BundlerMinifier) rozszerzenia obsługi konwersji Gulp.

> [!NOTE]
> Rozszerzenie narzędzia Bundler & Minifier należy do tworzonych metodą społecznościową projekt w witrynie GitHub, dla którego firmy Microsoft nie zapewnia obsługi. Powinny być zgłaszane problemy [tutaj](https://github.com/madskristensen/BundlerMinifier/issues).

Kliknij prawym przyciskiem myszy *bundleconfig.json* plik w Eksploratorze rozwiązań i wybierz **narzędzia Bundler & Minifier** > **przekonwertować Gulp...** :

![Konwertuj Gulp w menu kontekstowym](../client-side/bundling-and-minification/_static/convert-to-gulp.png)

*Gulpfile.js* i *package.json* pliki zostaną dodane do projektu. Obsługa [npm](https://www.npmjs.com/) pakiety wymienione w *package.json* pliku `devDependencies` sekcji są zainstalowane.

Uruchom następujące polecenie w oknie PMC, aby zainstalować interfejs wiersza polecenia Gulp jako globalne zależności:

```console
npm i -g gulp-cli
```

*Gulpfile.js* plików odczytów *bundleconfig.json* wejść, wyjść i ustawienia w pliku.

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-12&highlight=10)]

### <a name="convert-manually"></a>Konwertuj ręcznie

Jeśli program Visual Studio i/lub rozszerzenie narzędzia Bundler & Minifier nie są dostępne, przekonwertuj ręcznie.

Dodaj *package.json* pliku następującym kodem `devDependencies`, w katalogu głównym projektu:

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/package.json?range=5-13)]

Instalowanie zależności, uruchamiając następujące polecenie w tym samym poziomie co *package.json*:

```console
npm i
```

Zainstaluj interfejs wiersza polecenia Gulp jako globalne zależności:

```console
npm i -g gulp-cli
```

Kopiuj *gulpfile.js* pliku poniżej do katalogu głównego projektu:

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-11,14-)]

### <a name="run-gulp-tasks"></a>Uruchamianie zadań Gulp

Aby wyzwolić zadanie minimalizowanie Gulp, zanim projekt zostanie skompilowany w programie Visual Studio, Dodaj następujący element [programu MSBuild](/visualstudio/msbuild/msbuild-targets) pliku *.csproj:

[!code-xml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/BuildBundlerMinifierApp.csproj?range=14-16)]

W tym przykładzie wszystkie zadania zdefiniowane w ramach `MyPreCompileTarget` docelowy uruchomienia przed wstępnie zdefiniowanego `Build` docelowej. W oknie danych wyjściowych programu Visual Studio pojawia się dane wyjściowe podobne do następujących:

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

Alternatywnie Eksplorator modułu uruchamiającego zadania programu Visual Studio pozwala powiązać zadań Gulp określonych zdarzeń programu Visual Studio. Zobacz [uruchomionych zadań domyślne](xref:client-side/using-gulp#running-default-tasks) instrukcje dotyczące tych czynności.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Korzystanie z Gulp](xref:client-side/using-gulp)
* [Korzystanie z Grunt](xref:client-side/using-grunt)
* [Używanie wielu środowisk](xref:fundamentals/environments)
* [Pomocnicy tagów](xref:mvc/views/tag-helpers/intro)
