---
title: "Przy użyciu Gulp w platformy ASP.NET Core"
author: rick-anderson
description: "Dowiedz się, jak używać Gulp w ASP.NET Core."
keywords: Platformy ASP.NET Core Gulp
ms.author: riande
manager: wpickett
ms.date: 02/28/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/using-gulp
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 68f6838889cfb830f2c5a1976b3140ae5d94ac25
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
# <a name="introduction-to-using-gulp-in-aspnet-core"></a>Wprowadzenie do korzystania z Gulp w ASP.NET Core 

Przez [Erik Reitan](https://github.com/Erikre), [Scott Addie](https://scottaddie.com), [Roth Danielowi](https://github.com/danroth27), i [Shayne Boyer](https://twitter.com/spboyer)

W typowej nowoczesnych witryn sieci web aplikacji może być procesu kompilacji:

* Paczkę i zminimalizowania pliki JavaScript i CSS.
* Uruchamianie narzędzi do wywołania tworzenie pakietów i minimalizowanie zadań przed każdej kompilacji.
* Kompiluj mniej lub SASS pliki CSS.
* Kompiluj CoffeeScript lub maszynie pliki JavaScript.

A *modułu uruchamiającego zadania* to narzędzie, które automatyzuje tych zadań związanych z projektowaniem rutynowych i inne. Program Visual Studio udostępnia wbudowaną obsługę dla dwóch uczestników popularnych zadań oparte na języku JavaScript: [system Gulp](https://gulpjs.com/) i [Grunt](using-grunt.md).

## <a name="gulp"></a>Gulp

Gulp jest oparte na języku JavaScript przesyłania strumieniowego kompilacji pakiet narzędzi dla kodu po stronie klienta. Często służy do przesyłania strumieniowego plików po stronie klienta za pośrednictwem serii procesów, po wyzwoleniu określonego zdarzenia w środowisku kompilacji. Na przykład Gulp może służyć do automatyzowania [tworzenie pakietów i minimalizowanie](bundling-and-minification.md) lub czyszczenia Środowisko deweloperskie przed nowej kompilacji.

Zestaw zadań Gulp jest zdefiniowany w *gulpfile.js*. Następujący kod JavaScript zawiera modułów Gulp i określa ścieżki plików można odwoływać się w ciągu przyszłych zadań:

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

Powyższy kod określa, które moduły węzła są wymagane. `require` Funkcja importuje poszczególnych modułów, aby zadania zależne mogą wykorzystywać swoje funkcje. Każdego z zaimportowanych modułów jest przypisany do zmiennej. Moduły można odnaleźć, albo według nazwy lub ścieżki. W tym przykładzie modułów o nazwie `gulp`, `rimraf`, `gulp-concat`, `gulp-cssmin`, i `gulp-uglify` są pobierane przez nazwę. Ponadto szereg ścieżki są tworzone, dzięki czemu lokalizacje plików CSS i JavaScript można użyć ponownie i odwołuje zadań. Poniższa tabela zawiera opisy modułów objęte *gulpfile.js*.

|Nazwa modułu|Opis|
|---|---|
|gulp|Gulp przesyłania strumieniowego system kompilacji. Aby uzyskać więcej informacji, zobacz [system gulp](https://www.npmjs.com/package/gulp).|
|rimraf|Moduł usuwania węzła. Aby uzyskać więcej informacji, zobacz [rimraf](https://www.npmjs.com/package/rimraf).|
|gulp concat|Moduł, który łączy pliki oparte na znak nowego wiersza systemu operacyjnego. Aby uzyskać więcej informacji, zobacz [gulp concat](https://www.npmjs.com/package/gulp-concat).|
|gulp cssmin|Moduł, który minimalizuje pliki CSS. Aby uzyskać więcej informacji, zobacz [gulp cssmin](https://www.npmjs.com/package/gulp-cssmin).|
|gulp uglify|Moduł, który minimalizuje *js* plików. Aby uzyskać więcej informacji, zobacz [gulp uglify](https://www.npmjs.com/package/gulp-uglify).|

Gdy wymagane moduły są importowane, można określić zadania. W tym miejscu jest sześć zadań zarejestrowany, reprezentowany przez następujący kod:

```javascript
gulp.task("clean:js", function (cb) {
  rimraf(paths.concatJsDest, cb);
});

gulp.task("clean:css", function (cb) {
  rimraf(paths.concatCssDest, cb);
});

gulp.task("clean", ["clean:js", "clean:css"]);

gulp.task("min:js", function () {
  return gulp.src([paths.js, "!" + paths.minJs], { base: "." })
    .pipe(concat(paths.concatJsDest))
    .pipe(uglify())
    .pipe(gulp.dest("."));
});

gulp.task("min:css", function () {
  return gulp.src([paths.css, "!" + paths.minCss])
    .pipe(concat(paths.concatCssDest))
    .pipe(cssmin())
    .pipe(gulp.dest("."));
});

gulp.task("min", ["min:js", "min:css"]);
```

Poniższa tabela zawiera objaśnienie zadania określone w powyższym kodzie:

|Nazwa zadania|Opis|
|--- |--- |
|Wyczyść: js|Zadanie, które używa modułu usunięcie węzła rimraf usunąć zminimalizowany wersję pliku site.js.|
|Wyczyść: css|Zadanie, które używa modułu usunięcie węzła rimraf usunąć zminimalizowany wersję pliku site.css.|
|Czyszczenie|Zadanie, które wywołuje `clean:js` zadań, a następnie `clean:css` zadań.|
|min:js|Zadanie, które minimalizuje i łączy wszystkie pliki js znajdujących się w folderze js. . Pliki min.js są wyłączone.|
|min:CSS|Zadanie, które minimalizuje i łączy wszystkie pliki CSS w folderze css. . Pliki min.css są wyłączone.|
|min|Zadanie, które wywołuje `min:js` zadań, a następnie `min:css` zadań.|

## <a name="running-default-tasks"></a>Uruchomione zadania domyślne

Jeśli jeszcze nie utworzono nową aplikację sieci Web, Utwórz nowy projekt aplikacji sieci Web ASP.NET w programie Visual Studio.

1.  Dodaj nowy plik JavaScript do projektu i nadaj mu nazwę *gulpfile.js*, następnie skopiuj poniższy kod.

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
    
    gulp.task("clean:js", function (cb) {
      rimraf(paths.concatJsDest, cb);
    });
    
    gulp.task("clean:css", function (cb) {
      rimraf(paths.concatCssDest, cb);
    });
    
    gulp.task("clean", ["clean:js", "clean:css"]);
    
    gulp.task("min:js", function () {
      return gulp.src([paths.js, "!" + paths.minJs], { base: "." })
        .pipe(concat(paths.concatJsDest))
        .pipe(uglify())
        .pipe(gulp.dest("."));
    });
    
    gulp.task("min:css", function () {
      return gulp.src([paths.css, "!" + paths.minCss])
        .pipe(concat(paths.concatCssDest))
        .pipe(cssmin())
        .pipe(gulp.dest("."));
    });
    
    gulp.task("min", ["min:js", "min:css"]);
    ```

2.  Otwórz *package.json* pliku (Dodawanie, jeśli nie istnieje) i Dodaj następujący kod.

    ```json
    {
      "devDependencies": {
        "gulp": "3.9.1",
        "gulp-concat": "2.6.1",
        "gulp-cssmin": "0.1.7",
        "gulp-uglify": "2.0.1",
        "rimraf": "2.6.1"
      }
    }
    ```

3.  W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *gulpfile.js*i wybierz **Eksploratora modułu uruchamiającego zadania**.
    
    ![Otwórz Eksploratora modułu uruchamiającego zadania w Eksploratorze rozwiązań](using-gulp/_static/02-SolutionExplorer-TaskRunnerExplorer.png)
    
    **Zadanie Eksploratora modułu uruchamiającego** z listą zadań Gulp. (Może być konieczne kliknięcie pozycji **Odśwież** przycisku, który wydaje się po lewej stronie nazwy projektu.)
    
    ![Eksploratora modułu uruchamiającego zadania](using-gulp/_static/03-TaskRunnerExplorer.png)

4.  Underneath **zadania** w **Eksploratora modułu uruchamiającego zadania**, kliknij prawym przyciskiem myszy **czystą**i wybierz **Uruchom** z menu podręcznego.

    ![Zadanie czystą Eksploratora modułu uruchamiającego zadania](using-gulp/_static/04-TaskRunner-clean.png)

    **Zadanie Eksploratora modułu uruchamiającego** utworzy nową kartę o nazwie **czystą** i wykonać czystą zadania zdefiniowanej w *gulpfile.js*.

5.  Kliknij prawym przyciskiem myszy **czystą** zadań, a następnie wybierz **powiązania** > **przed kompilacji**.

    ![Powiązanie BeforeBuild Eksploratora modułu uruchamiającego zadania](using-gulp/_static/05-TaskRunner-BeforeBuild.png)

    **Przed kompilacji** powiązania konfiguruje czystą zadanie do automatycznego uruchamiania przed każdym kompilacji projektu.

Powiązania można skonfigurować przy użyciu **Eksploratora modułu uruchamiającego zadania** są przechowywane w formie komentarzy w górnej części programu *gulpfile.js* i obowiązują tylko w programie Visual Studio. Alternatywa, która nie wymaga programu Visual Studio jest skonfigurowanie automatycznego wykonywania zadań gulp w Twojej *.csproj* pliku. Na przykład umieścić Twojej *.csproj* pliku:

```xml
<Target Name="MyPreCompileTarget" BeforeTargets="Build">
  <Exec Command="gulp clean" />
</Target>
```

Teraz czystą zadanie jest wykonywane po uruchomieniu projektu programu Visual Studio lub z wiersza polecenia przy użyciu `dotnet run` polecenia (Uruchom `npm install` pierwszy).

## <a name="defining-and-running-a-new-task"></a>Definiowanie i uruchamiania nowego zadania

Aby zdefiniować nowe zadanie Gulp, zmodyfikuj *gulpfile.js*.

1.  Dodaj następujący kod JavaScript do końca *gulpfile.js*:

    ```javascript
    gulp.task("first", function () {
      console.log('first task! <-----');
    });
    ```

    To zadanie jest o nazwie `first`, i po prostu wyświetla ciąg.

2.  Zapisz *gulpfile.js*.

3.  W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *gulpfile.js*i wybierz *Eksploratora modułu uruchamiającego zadania*.

4.  W **Eksploratora modułu uruchamiającego zadania**, kliknij prawym przyciskiem myszy **pierwszy**i wybierz **Uruchom**.

    ![Uruchom zadanie pierwszy Eksploratora modułu uruchamiającego zadania](using-gulp/_static/06-TaskRunner-First.png)

    Zobaczysz, że jest wyświetlany tekst wyjściowy. Jeśli interesuje Cię w przykładach oparte na typowy scenariusz, zobacz system Gulp przepisami.

## <a name="defining-and-running-tasks-in-a-series"></a>Definiowanie i uruchamianie zadań w serii

Po uruchomieniu zadania wielu zadań jednocześnie domyślnie uruchamiane. Jednak jeśli chcesz uruchamiać zadania w określonej kolejności, należy określić podczas każdego zadania zostanie zakończone, a także jako zadania, które są zależne od zakończenia inne zadanie.

1.  Aby zdefiniować serię zadań do wykonania w kolejności, Zastąp `first` zadań dodanych powyżej w *gulpfile.js* następującym kodem:

    ```javascript
    gulp.task("series:first", function () {
      console.log('first task! <-----');
    });
 
    gulp.task("series:second", ["series:first"], function () {
      console.log('second task! <-----');
    });
 
    gulp.task("series", ["series:first", "series:second"], function () {});
    ```
 
    Masz teraz trzy zadania: `series:first`, `series:second`, i `series`. `series:second` Zadanie zawiera drugi parametr, który określa tablicę zadania do uruchomienia, a ukończone przed `series:second` zadanie zostanie uruchomione.  Jak określono w kodzie powyżej, tylko `series:first` zadań muszą zostać wykonane przed `series:second` zadanie zostanie uruchomione.

2.  Zapisz *gulpfile.js*.

3.  W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *gulpfile.js* i wybierz **Eksploratora modułu uruchamiającego zadania** Jeśli nie jest już otwarty.

4.  W **Eksploratora modułu uruchamiającego zadania**, kliknij prawym przyciskiem myszy **serii** i wybierz **Uruchom**.

    ![Uruchom zadanie serii Eksploratora modułu uruchamiającego zadania](using-gulp/_static/07-TaskRunner-Series.png)

## <a name="intellisense"></a>IntelliSense

IntelliSense zawiera kod zakończenia, opisy parametrów i innych funkcji, aby zwiększyć wydajność i zmniejszyć błędy. Zadania gulp są napisane w języku JavaScript. w związku z tym IntelliSense zapewniają pomoc podczas tworzenia. Podczas pracy z użyciem języka JavaScript, IntelliSense wyświetla obiekty, funkcji, właściwości i parametrów, które są dostępne na podstawie użytkownika bieżącego kontekstu. Wybierz opcję kodowania z podanej listy wyskakujących przez funkcję IntelliSense kodu.

![System gulp IntelliSense](using-gulp/_static/08-IntelliSense.png)

Aby uzyskać więcej informacji o funkcji IntelliSense, zobacz [IntelliSense dla JavaScript](https://docs.microsoft.com/visualstudio/ide/javascript-intellisense).

## <a name="development-staging-and-production-environments"></a>Środowiska deweloperskie, przemieszczania i produkcji

W przypadku Gulp w celu zoptymalizowania plików po stronie klienta dla tymczasowych i produkcyjnych przetworzonych plików są zapisywane w lokalizacji lokalnej tymczasową i produkcyjną. *_Layout.cshtml* plików używa **środowiska** tagu pomocnika zapewnienie dwie różne wersje plików CSS. Jednej wersji plików CSS do rozwoju i innych wersji zoptymalizowano pod kątem zarówno tymczasową i produkcyjną. W Visual Studio 2017 r, jeśli zmienisz **ASPNETCORE_ENVIRONMENT** zmienną środowiskową `Production`, Visual Studio utworzy aplikację sieci Web i łącza do zminimalizowane pliki CSS. Poniżej przedstawiono znaczników **środowiska** zawierający tagów łącza do pomocników tagów `Development` CSS plików i zminimalizowany `Staging, Production` pliki CSS.

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

Aby przełączyć między kompilowania kodu dla różnych środowisk, należy zmodyfikować **ASPNETCORE_ENVIRONMENT** wartość zmiennej środowiskowej.

1.  W **Eksploratora modułu uruchamiającego zadania**, upewnij się, że **min** zadań została ustawiona do uruchamiania **przed kompilacji**.

2.  W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy nazwę projektu i wybierz **właściwości**.

    Arkusz właściwości dla aplikacji sieci Web jest wyświetlany.

3.  Kliknij przycisk **debugowania** kartę.

4.  Ustaw wartość **hostingu: środowisko** zmienną środowiskową `Production`.

5.  Naciśnij klawisz **F5** Aby uruchomić aplikację w przeglądarce.

6.  W oknie przeglądarki, kliknij prawym przyciskiem myszy strony, a następnie wybierz **Wyświetl źródło** Aby wyświetlić kod HTML dla strony.

    Należy zauważyć, że łączy arkusza stylów wskaż zminimalizowany pliki CSS.

7.  Zamknij przeglądarkę, aby zatrzymać aplikacji sieci Web.

8.  W programie Visual Studio, wróć do arkusza właściwości dla aplikacji sieci Web i zmienić **hostingu: środowisko** z powrotem do zmiennej środowiskowej `Development`.

9.  Naciśnij klawisz **F5** Aby ponownie uruchomić aplikację w przeglądarce.

10. W oknie przeglądarki, kliknij prawym przyciskiem myszy strony, a następnie wybierz **Wyświetl źródło** Aby wyświetlić kod HTML dla strony.

    Należy zauważyć, że łączy arkusza stylów wskaż unminified wersje plików CSS.

Aby uzyskać więcej informacji związanych z środowiska w programie ASP.NET Core, zobacz [Praca w środowiskach wielu](../fundamentals/environments.md).

## <a name="task-and-module-details"></a>Szczegóły zadania i modułu

Zadanie Gulp jest zarejestrowana nazwa funkcji.  Jeśli inne zadania musi zostać uruchomiony przed bieżącym zadań można określić zależności. Dodatkowe funkcje umożliwiają uruchamianie i obejrzyj Gulp zadania, a także Ustaw źródło (*src*) i docelowego (*dest*) plików jest modyfikowany. Poniżej przedstawiono podstawowe funkcje system Gulp interfejsu API:

|Funkcja gulp|Składnia|Opis|
|---   |--- |--- |
|— zadanie  |`gulp.task(name[, deps], fn) { }`|`task` Funkcja tworzy zadanie. `name` Parametr określa nazwę zadania. `deps` Parametr zawiera tablicę zadań do wykonania przed uruchomieniem tego zadania. `fn` Parametr reprezentuje funkcję wywołania zwrotnego, która wykonuje operacje zadania.|
|Czujki |`gulp.watch(glob [, opts], tasks) { }`|`watch` Funkcja monitoruje pliki i uruchamia zadania po zmianie pliku. `glob` Parametr jest `string` lub `array` Określa, które pliki, aby obejrzeć. `opts` Parametru zapewnia dodatkowy plik obserwowanie opcje.|
|src   |`gulp.src(globs[, options]) { }`|`src` Funkcja zawiera pliki, które są zgodne z wartościami glob. `glob` Parametr jest `string` lub `array` Określa, które pliki do odczytu. `options` Parametru zapewnia dodatkowe opcje pliku.|
|Docelowy  |`gulp.dest(path[, options]) { }`|`dest` Funkcja określa lokalizację, do której można zapisywać pliki. `path` Parametr jest ciągiem lub funkcja, która określa folder docelowy. `options` Parametr jest obiekt, który określa opcje folderów danych wyjściowych.|

Aby uzyskać dodatkowe informacje system Gulp interfejsu API zobacz [system Gulp API Docs](https://github.com/gulpjs/gulp/blob/master/docs/API.md).

## <a name="gulp-recipes"></a>System gulp przepisami

Społeczność Gulp oferuje Gulp [przepisami](https://github.com/gulpjs/gulp/blob/master/docs/recipes/README.md). Te przepisami składają się z Gulp zadań w celu rozwiązania typowych scenariuszy.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Dokumentacja gulp](https://github.com/gulpjs/gulp/blob/master/docs/README.md)
* [Tworzenie pakietów i minimalizowanie w ASP.NET Core](bundling-and-minification.md)
* [Przy użyciu Grunt w platformy ASP.NET Core](using-grunt.md)
