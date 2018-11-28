---
title: Korzystanie z Gulp w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak korzystanie z Gulp w programie ASP.NET Core.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 10/04/2018
uid: client-side/using-gulp
ms.openlocfilehash: e280eabecbd427f3e1418b3d7a60e0ea3df46a5a
ms.sourcegitcommit: e9b99854b0a8021dafabee0db5e1338067f250a9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2018
ms.locfileid: "52450609"
---
# <a name="use-gulp-in-aspnet-core"></a>Korzystanie z Gulp w programie ASP.NET Core

Przez [Scott Addie](https://scottaddie.com), [Shayne Boyer](https://twitter.com/spboyer), i [sosny David](https://twitter.com/davidpine7)

W aplikacji Typowa Nowoczesna sieć web może być procesu kompilacji:

* Tworzenie pakietów i minimalizowanie plików JavaScript i CSS.
* Uruchom narzędzia, aby wywołać tworzenie pakietów i minimalizowanie zadań przed każdą kompilacją.
* Skompilować mniej lub SASS pliki CSS.
* Skompiluj pliki CoffeeScript lub TypeScript w kodzie JavaScript.

A *modułu uruchamiającego zadania* to narzędzie, które automatyzuje te zadania tworzenia procedur i nie tylko. Program Visual Studio udostępnia wbudowaną obsługę dla dwóch uczestników popularne zadania oparte na języku JavaScript: [Gulp](https://gulpjs.com/) i [Grunt](using-grunt.md).

## <a name="gulp"></a>Program gulp

Gulp to oparte na języku JavaScript przesyłania strumieniowego kompilacji narzędzi kod po stronie klienta. Często służy do przesyłania plików po stronie klienta za pomocą szeregu procesy po wyzwoleniu określonego zdarzenia w środowisku kompilacji. Na przykład Gulp może służyć do automatyzowania [tworzenie pakietów i minimalizowanie](bundling-and-minification.md) lub czyszczenie środowisko programistyczne przed nową kompilację.

Zestaw zadań Gulp jest zdefiniowany w *gulpfile.js*. Następujący kod JavaScript obejmują moduły Gulp i określa ścieżki plików, aby odwoływać się w ciągu przyszłych zadań:

```javascript
/// <binding Clean='clean' />
"use strict";

var gulp = require("gulp"),
    rimraf = require("rimraf"),
    concat = require("gulp-concat"),
    cssmin = require("gulp-cssmin"),
    uglify = require("gulp-uglify");

var paths = {
  webroot: "./wwwroot/"
};

paths.js = paths.webroot + "js/**/*.js";
paths.minJs = paths.webroot + "js/**/*.min.js";
paths.css = paths.webroot + "css/**/*.css";
paths.minCss = paths.webroot + "css/**/*.min.css";
paths.concatJsDest = paths.webroot + "js/site.min.js";
paths.concatCssDest = paths.webroot + "css/site.min.css";
```

Powyższy kod określa, które moduły węzła są wymagane. `require` Funkcja importuje każdego modułu, aby zadania zależne, mogą korzystać z ich funkcje. Każdego z zaimportowanych modułów jest przypisany do zmiennej. Moduły można znaleźć, albo według nazwy lub ścieżki. W tym przykładzie modułów o nazwie `gulp`, `rimraf`, `gulp-concat`, `gulp-cssmin`, i `gulp-uglify` są pobierane przez nazwę. Ponadto seria ścieżek są tworzone tak, aby lokalizacje plików CSS i JavaScript, które mogą być ponownie użyte i przywoływany w ramach zadania. Poniższa tabela zawiera opis modułów objęte *gulpfile.js*.

| Nazwa modułu | Opis |
| ----------- | ----------- |
| Program gulp        | System kompilacji Gulp, przesyłania strumieniowego. Aby uzyskać więcej informacji, zobacz [gulp](https://www.npmjs.com/package/gulp). |
| rimraf      | Moduł usuwania węzła. Aby uzyskać więcej informacji, zobacz [rimraf](https://www.npmjs.com/package/rimraf). |
| gulp concat | Moduł, który łączy pliki, w oparciu o system operacyjny znak nowego wiersza. Aby uzyskać więcej informacji, zobacz [gulp concat](https://www.npmjs.com/package/gulp-concat). |
| gulp-cssmin | Moduł, który minimalizuje pliki CSS. Aby uzyskać więcej informacji, zobacz [gulp cssmin](https://www.npmjs.com/package/gulp-cssmin). |
| gulp uglify | Moduł, który minimalizuje *js* plików. Aby uzyskać więcej informacji, zobacz [gulp uglify](https://www.npmjs.com/package/gulp-uglify). |

Gdy wymagane moduły są importowane, można określić zadania. Oto sześciu zadań podrzędnych zarejestrowany, reprezentowane przez następujący kod:

```javascript
gulp.task("clean:js", done => rimraf(paths.concatJsDest, done));
gulp.task("clean:css", done => rimraf(paths.concatCssDest, done));
gulp.task("clean", gulp.series(["clean:js", "clean:css"]));

gulp.task("min:js", () => {
  return gulp.src([paths.js, "!" + paths.minJs], { base: "." })
    .pipe(concat(paths.concatJsDest))
    .pipe(uglify())
    .pipe(gulp.dest("."));
});

gulp.task("min:css", () => {
  return gulp.src([paths.css, "!" + paths.minCss])
    .pipe(concat(paths.concatCssDest))
    .pipe(cssmin())
    .pipe(gulp.dest("."));
});

gulp.task("min", gulp.series(["min:js", "min:css"]));
    
// A 'default' task is required by Gulp v4
gulp.task("default", gulp.series(["min"]));
```

---

Poniższa tabela zawiera wyjaśnienie zadania określone w powyższym kodzie:

|Nazwa zadania|Opis|
|--- |--- |
|Wyczyść: js|Zadanie, które używa modułu usuwania węzła rimraf do usunięcia zminimalizowany wersję pliku site.js.|
|Wyczyść: css|Zadanie, które używa modułu usuwania węzła rimraf do usunięcia zminimalizowany wersję pliku site.css.|
|czyszczenie|Zadanie, które wywołuje `clean:js` zadania, a następnie `clean:css` zadania.|
|min:js|Zadanie, które minimalizuje i łączy wszystkie pliki .js znajdujące się w folderze js. . Pliki min.js są wyłączone.|
|min:css|Zadanie, które minimalizuje i łączy wszystkie pliki CSS w folderze css. . Pliki min.css są wyłączone.|
|min|Zadanie, które wywołuje `min:js` zadania, a następnie `min:css` zadania.|

## <a name="running-default-tasks"></a>Uruchamianie zadań domyślne

Jeśli nie zostało jeszcze utworzone Nowa aplikacja sieci Web, należy utworzyć nowy projekt aplikacji sieci Web ASP.NET w programie Visual Studio.

1.  Otwórz *package.json* pliku (Dodaj Jeśli nie istnieje) i Dodaj następujący kod.

    ```json
    {
      "devDependencies": {
        "gulp": "^4.0.0",
        "gulp-concat": "2.6.1",
        "gulp-cssmin": "0.2.0",
        "gulp-uglify": "3.0.0",
        "rimraf": "2.6.1"
      }
    }
    ```

2.  Dodaj nowy plik JavaScript do projektu i nadaj mu nazwę *gulpfile.js*, następnie skopiuj następujący kod.

    ```javascript
    /// <binding Clean='clean' />
    "use strict";
    
    const gulp = require("gulp"),
          rimraf = require("rimraf"),
          concat = require("gulp-concat"),
          cssmin = require("gulp-cssmin"),
          uglify = require("gulp-uglify");
    
    const paths = {
      webroot: "./wwwroot/"
    };
    
    paths.js = paths.webroot + "js/**/*.js";
    paths.minJs = paths.webroot + "js/**/*.min.js";
    paths.css = paths.webroot + "css/**/*.css";
    paths.minCss = paths.webroot + "css/**/*.min.css";
    paths.concatJsDest = paths.webroot + "js/site.min.js";
    paths.concatCssDest = paths.webroot + "css/site.min.css";
    
    gulp.task("clean:js", done => rimraf(paths.concatJsDest, done));
    gulp.task("clean:css", done => rimraf(paths.concatCssDest, done));
    gulp.task("clean", gulp.series(["clean:js", "clean:css"]));

    gulp.task("min:js", () => {
      return gulp.src([paths.js, "!" + paths.minJs], { base: "." })
        .pipe(concat(paths.concatJsDest))
        .pipe(uglify())
        .pipe(gulp.dest("."));
    });

    gulp.task("min:css", () => {
      return gulp.src([paths.css, "!" + paths.minCss])
        .pipe(concat(paths.concatCssDest))
        .pipe(cssmin())
        .pipe(gulp.dest("."));
    });

    gulp.task("min", gulp.series(["min:js", "min:css"]));
    
    // A 'default' task is required by Gulp v4
    gulp.task("default", gulp.series(["min"]));
    ```

3.  W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *gulpfile.js*i wybierz **Eksplorator modułu uruchamiającego zadania**.
    
    ![Otwórz Eksplorator modułu uruchamiającego zadania za pomocą Eksploratora rozwiązań](using-gulp/_static/02-SolutionExplorer-TaskRunnerExplorer.png)
    
    **Task Runner Explorer** pokazuje listę zadań Gulp. (Być może trzeba kliknąć **Odśwież** przycisk, który pojawia się po lewej stronie nazwy projektu.)
    
    ![Eksplorator modułu uruchamiającego zadania](using-gulp/_static/03-TaskRunnerExplorer.png)
    
    > [!IMPORTANT]
    > **Eksplorator modułu uruchamiającego zadania** element menu kontekstowego pojawia się tylko wtedy, gdy *gulpfile.js* znajduje się w katalogu głównym projektu.

4.  Poniżej **zadania** w **Eksplorator modułu uruchamiającego zadania**, kliknij prawym przyciskiem myszy **czyste**i wybierz **Uruchom** z menu podręcznego.

    ![Zadanie czysty Eksplorator modułu uruchamiającego zadania](using-gulp/_static/04-TaskRunner-clean.png)

    **Task Runner Explorer** zostanie utworzona nowa karta o nazwie **czyste** i wykonać zadanie czysty, ponieważ jest on zdefiniowany w *gulpfile.js*.

5.  Kliknij prawym przyciskiem myszy **czyste** zadań, a następnie wybierz **powiązania** > **przed kompilacji**.

    ![Powiązanie BeforeBuild Eksplorator modułu uruchamiającego zadania](using-gulp/_static/05-TaskRunner-BeforeBuild.png)

    **Przed kompilacji** powiązania konfiguruje zadanie czysty, aby automatycznie uruchomić przed każdą kompilacją projektu.

Powiązania określone przy użyciu **Eksplorator modułu uruchamiającego zadania** są przechowywane w formie komentarza, w górnej części Twojej *gulpfile.js* i obowiązują tylko w programie Visual Studio. Alternatywa, która nie wymaga programu Visual Studio jest skonfigurowanie automatycznego wykonywania zadań gulp w swojej *.csproj* pliku. Na przykład, należy umieścić swojej *.csproj* pliku:

```xml
<Target Name="MyPreCompileTarget" BeforeTargets="Build">
  <Exec Command="gulp clean" />
</Target>
```

Teraz zadanie czysty jest wykonywany, gdy uruchamiasz projekt w programie Visual Studio lub z wiersza polecenia przy użyciu [dotnet, uruchom](/dotnet/core/tools/dotnet-run) polecenia (uruchamianie `npm install` pierwszy).

## <a name="defining-and-running-a-new-task"></a>Definiowanie i uruchomione nowe zadanie

Aby zdefiniować nowe zadanie Gulp, zmodyfikuj *gulpfile.js*.

1.  Dodaj następujący kod JavaScript do końca *gulpfile.js*:

    ```javascript
    gulp.task('first', done => {
      console.log('first task! <-----');
      done(); // signal completion
    });
    ```

    To zadanie o nazwie `first`, i po prostu wyświetla ciąg.

2.  Zapisz *gulpfile.js*.

3.  W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *gulpfile.js*i wybierz *Eksplorator modułu uruchamiającego zadania*.

4.  W **Eksplorator modułu uruchamiającego zadania**, kliknij prawym przyciskiem myszy **pierwszy**i wybierz **Uruchom**.

    ![Uruchamianie pierwszego zadania Eksplorator modułu uruchamiającego zadania](using-gulp/_static/06-TaskRunner-First.png)

    Tekst wyjściowy jest wyświetlany. Aby wyświetlić przykłady oparte na typowych scenariuszy, zobacz [Gulp przepisy](#gulp-recipes).

## <a name="defining-and-running-tasks-in-a-series"></a>Definiowanie i uruchamianie zadań w serii

Po uruchomieniu wielu zadań, zadań jednocześnie domyślnie uruchamiane. Jednak jeśli zachodzi potrzeba uruchamiania zadań w określonej kolejności, należy określić kiedy każde zadanie jest zakończone, jak również jako zadania, które zależą od zakończenia inne zadanie.

1.  Aby zdefiniować szereg zadań do wykonania w kolejności, Zastąp `first` zadanie, które zostały dodane powyżej w *gulpfile.js* następującym kodem:

    ```javascript
    gulp.task('series:first', done => {
      console.log('first task! <-----');
      done(); // signal completion
    });
    gulp.task('series:second', done => {
      console.log('second task! <-----');
      done(); // signal completion
    });

    gulp.task('series', gulp.series(['series:first', 'series:second']), () => { });

    // A 'default' task is required by Gulp v4
    gulp.task('default', gulp.series('series'));
    ```
 
    Masz teraz trzy zadania: `series:first`, `series:second`, i `series`. `series:second` Zadanie zawiera drugi parametr, który określa tablicę zadań do uruchomienia i ukończone przed `series:second` zadanie zostanie uruchomione. Jak określono w kodzie powyżej tylko `series:first` zadań muszą zostać wykonane przed `series:second` zadanie zostanie uruchomione.

2.  Zapisz *gulpfile.js*.

3.  W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *gulpfile.js* i wybierz **Eksplorator modułu uruchamiającego zadania** Jeśli nie jest już otwarty.

4.  W **Eksplorator modułu uruchamiającego zadania**, kliknij prawym przyciskiem myszy **serii** i wybierz **Uruchom**.

    ![Uruchom zadanie serii Eksplorator modułu uruchamiającego zadania](using-gulp/_static/07-TaskRunner-Series.png)

## <a name="intellisense"></a>IntelliSense

Technologia IntelliSense zawiera uzupełniania kodu, opisy parametrów i inne funkcje, aby zwiększyć wydajność i zmniejszyć błędy. Zadań gulp są napisane w języku JavaScript. w związku z tym funkcja IntelliSense może zapewnić pomoc podczas tworzenia. Podczas pracy z użyciem języka JavaScript, IntelliSense wyświetla listę obiektów, funkcji, właściwości i parametrów, które są dostępne na podstawie Twojego bieżącego kontekstu. Wybierz opcję kodowania z wyskakującej listy dostarczonej przez technologię IntelliSense, aby uzupełnić kod.

![Program gulp IntelliSense](using-gulp/_static/08-IntelliSense.png)

Aby uzyskać więcej informacji na temat technologii IntelliSense, zobacz [JavaScript IntelliSense](/visualstudio/ide/javascript-intellisense).

## <a name="development-staging-and-production-environments"></a>Środowiska deweloperskie, przejściowe i produkcji

Po Gulp jest używana do optymalizacji pliki po stronie klienta dla środowisk przejściowych i produkcyjnych, przetworzone pliki są zapisywane w lokalizacji lokalnej środowisk przejściowych i produkcyjnych. *_Layout.cshtml* plików używa **środowiska** tag pomocnika, aby zapewnić dwie różne wersje plików CSS. Jest jedna wersja plików CSS do tworzenia aplikacji, a druga wersja jest zoptymalizowany do przemieszczania i produkcji. W programie Visual Studio 2017 po zmianie **ASPNETCORE_ENVIRONMENT** zmiennej środowiskowej, aby `Production`, Visual Studio utworzy aplikację sieci Web i łącza do plików CSS w trybie zminimalizowanym. Ilustruje poniższy kod znaczników **środowiska** zawierający tagi łącza do pomocników tagów `Development` CSS plików i zminimalizowany `Staging, Production` pliki CSS.

```html
<environment names="Development">
    <script src="~/lib/jquery/dist/jquery.js"></script>
    <script src="~/lib/bootstrap/dist/js/bootstrap.js"></script>
    <script src="~/js/site.js" asp-append-version="true"></script>
</environment>
<environment names="Staging,Production">
    <script src="https://ajax.aspnetcdn.com/ajax/jquery/jquery-2.2.0.min.js"
            asp-fallback-src="~/lib/jquery/dist/jquery.min.js"
            asp-fallback-test="window.jQuery"
            crossorigin="anonymous"
            integrity="sha384-K+ctZQ+LL8q6tP7I94W+qzQsfRV2a+AfHIi9k8z8l9ggpc8X+Ytst4yBo/hH+8Fk">
    </script>
    <script src="https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/bootstrap.min.js"
            asp-fallback-src="~/lib/bootstrap/dist/js/bootstrap.min.js"
            asp-fallback-test="window.jQuery && window.jQuery.fn && window.jQuery.fn.modal"
            crossorigin="anonymous"
            integrity="sha384-Tc5IQib027qvyjSMfHjOMaLkfuWVxZxUPnCJA7l2mCWNIpG9mGCD8wGNIcPD7Txa">
    </script>
    <script src="~/js/site.min.js" asp-append-version="true"></script>
</environment>
```

## <a name="switching-between-environments"></a>Przełączanie między środowiskami

Aby przełączać się między kompilowanie dla różnych środowisk, należy zmodyfikować **ASPNETCORE_ENVIRONMENT** wartość zmiennej środowiskowej.

1.  W **Eksplorator modułu uruchamiającego zadania**, upewnij się, że **min** zadania został ustawiony na uruchomienie **przed kompilacji**.

2.  W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy nazwę projektu i wybierz **właściwości**.

    Arkusz właściwości dla aplikacji sieci Web jest wyświetlany.

3.  Kliknij przycisk **debugowania** kartę.

4.  Ustaw wartość **hostingu: środowisko** zmiennej środowiskowej, aby `Production`.

5.  Naciśnij klawisz **F5** do uruchamiania aplikacji w przeglądarce.

6.  W oknie przeglądarki, kliknij prawym przyciskiem myszy strony, a następnie wybierz **Wyświetl źródło** Aby wyświetlić kod HTML dla strony.

    Należy zauważyć, że łączy arkusza stylów wskazują zminimalizowany pliki CSS.

7.  Zamknij przeglądarkę, aby zatrzymać aplikacji sieci Web.

8.  W programie Visual Studio, wróć do arkusza właściwości dla aplikacji sieci Web i zmień **hostingu: środowisko** z powrotem do zmiennej środowiskowej `Development`.

9.  Naciśnij klawisz **F5** ponownie uruchomić aplikację w przeglądarce.

10. W oknie przeglądarki, kliknij prawym przyciskiem myszy strony, a następnie wybierz **Wyświetl źródło** Aby wyświetlić kod HTML dla strony.

    Należy zauważyć, że łączy arkusza stylów wskazują unminified wersje plików CSS.

Aby uzyskać więcej informacji związanych z środowiska w programie ASP.NET Core, zobacz [używanie wielu środowisk](../fundamentals/environments.md).

## <a name="task-and-module-details"></a>Szczegóły zadania i modułu

Zadań Gulp jest zarejestrowana z nazwą funkcji. Można określić zależności, jeśli inne zadania, musi zostać uruchomiony przed bieżącym zadaniu. Dodatkowe funkcje umożliwiają uruchamianie i obejrzyj zadań Gulp, a także ustawić źródło (*src*) i docelowy (*dest*) plików jest modyfikowany. Poniżej przedstawiono podstawowe funkcje Gulp interfejsu API:

|Program gulp — funkcja|Składnia|Opis|
|---   |--- |--- |
|— zadanie  |`gulp.task(name[, deps], fn) { }`|`task` Funkcja tworzy zadanie. `name` Parametr określa nazwę zadania. `deps` Parametr zawiera tablicę zadań do wykonania przed uruchomieniem tego zadania. `fn` Parametr reprezentuje funkcję wywołania zwrotnego, która wykonuje operacje zadania.|
|Wyrażenie kontrolne |`gulp.watch(glob [, opts], tasks) { }`|`watch` Funkcja monitoruje zadania plików i przebiegi po zmianie pliku. `glob` Parametr jest `string` lub `array` określający, plików, których obejrzeć. `opts` Parametr zawiera dodatkowy plik oglądania opcje.|
|src   |`gulp.src(globs[, options]) { }`|`src` Funkcja zawiera pliki, które odpowiadają wartości glob. `glob` Parametr jest `string` lub `array` określający plików do odczytu. `options` Parametr zapewnia dodatkowe opcje pliku.|
|docelowy  |`gulp.dest(path[, options]) { }`|`dest` Funkcja określa lokalizację, do którego można zapisywać pliki. `path` Parametru jest ciąg lub funkcja, która określa folder docelowy. `options` Parametru jest obiektem, który określa opcje folderów danych wyjściowych.|

Aby uzyskać dodatkowe informacje Gulp interfejsu API zobacz [Gulp Docs API](https://github.com/gulpjs/gulp/blob/master/docs/API.md).

## <a name="gulp-recipes"></a>Przepisy gulp

Społeczność Gulp oferuje Gulp [przepisy](https://github.com/gulpjs/gulp/blob/master/docs/recipes/README.md). Te przepisami składają się z zadań Gulp obsługę typowych scenariuszy.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Dokumentacja gulp](https://github.com/gulpjs/gulp/blob/master/docs/README.md)
* [Tworzenie pakietów i minimalizowanie w programie ASP.NET Core](bundling-and-minification.md)
* [Korzystanie z Grunt w programie ASP.NET Core](using-grunt.md)
