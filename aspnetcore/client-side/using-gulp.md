---
title: Użyj Gulp w platformy ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak używać Gulp w ASP.NET Core.
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 02/28/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: client-side/using-gulp
ms.openlocfilehash: f776b2025b6ebfeff28d3903aaeac4d7d89665b3
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
# <a name="use-gulp-in-aspnet-core"></a><span data-ttu-id="fcdea-103">Użyj Gulp w platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fcdea-103">Use Gulp in ASP.NET Core</span></span>

<span data-ttu-id="fcdea-104">Przez [Erik Reitan](https://github.com/Erikre), [Scott Addie](https://scottaddie.com), [Roth Danielowi](https://github.com/danroth27), i [Shayne Boyer](https://twitter.com/spboyer)</span><span class="sxs-lookup"><span data-stu-id="fcdea-104">By [Erik Reitan](https://github.com/Erikre), [Scott Addie](https://scottaddie.com), [Daniel Roth](https://github.com/danroth27), and [Shayne Boyer](https://twitter.com/spboyer)</span></span>

<span data-ttu-id="fcdea-105">W typowej nowoczesnych witryn sieci web aplikacji może być procesu kompilacji:</span><span class="sxs-lookup"><span data-stu-id="fcdea-105">In a typical modern web app, the build process might:</span></span>

* <span data-ttu-id="fcdea-106">Paczkę i zminimalizowania pliki JavaScript i CSS.</span><span class="sxs-lookup"><span data-stu-id="fcdea-106">Bundle and minify JavaScript and CSS files.</span></span>
* <span data-ttu-id="fcdea-107">Uruchamianie narzędzi do wywołania tworzenie pakietów i minimalizowanie zadań przed każdej kompilacji.</span><span class="sxs-lookup"><span data-stu-id="fcdea-107">Run tools to call the bundling and minification tasks before each build.</span></span>
* <span data-ttu-id="fcdea-108">Kompiluj mniej lub SASS pliki CSS.</span><span class="sxs-lookup"><span data-stu-id="fcdea-108">Compile LESS or SASS files to CSS.</span></span>
* <span data-ttu-id="fcdea-109">Kompiluj CoffeeScript lub maszynie pliki JavaScript.</span><span class="sxs-lookup"><span data-stu-id="fcdea-109">Compile CoffeeScript or TypeScript files to JavaScript.</span></span>

<span data-ttu-id="fcdea-110">A *modułu uruchamiającego zadania* to narzędzie, które automatyzuje tych zadań związanych z projektowaniem rutynowych i inne.</span><span class="sxs-lookup"><span data-stu-id="fcdea-110">A *task runner* is a tool which automates these routine development tasks and more.</span></span> <span data-ttu-id="fcdea-111">Program Visual Studio udostępnia wbudowaną obsługę dla dwóch uczestników popularnych zadań oparte na języku JavaScript: [system Gulp](https://gulpjs.com/) i [Grunt](using-grunt.md).</span><span class="sxs-lookup"><span data-stu-id="fcdea-111">Visual Studio provides built-in support for two popular JavaScript-based task runners: [Gulp](https://gulpjs.com/) and [Grunt](using-grunt.md).</span></span>

## <a name="gulp"></a><span data-ttu-id="fcdea-112">gulp</span><span class="sxs-lookup"><span data-stu-id="fcdea-112">Gulp</span></span>

<span data-ttu-id="fcdea-113">Gulp jest oparte na języku JavaScript przesyłania strumieniowego kompilacji pakiet narzędzi dla kodu po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="fcdea-113">Gulp is a JavaScript-based streaming build toolkit for client-side code.</span></span> <span data-ttu-id="fcdea-114">Często służy do przesyłania strumieniowego plików po stronie klienta za pośrednictwem serii procesów, po wyzwoleniu określonego zdarzenia w środowisku kompilacji.</span><span class="sxs-lookup"><span data-stu-id="fcdea-114">It's commonly used to stream client-side files through a series of processes when a specific event is triggered in a build environment.</span></span> <span data-ttu-id="fcdea-115">Na przykład Gulp może służyć do automatyzowania [tworzenie pakietów i minimalizowanie](bundling-and-minification.md) lub czyszczenia Środowisko deweloperskie przed nowej kompilacji.</span><span class="sxs-lookup"><span data-stu-id="fcdea-115">For instance, Gulp can be used to automate [bundling and minification](bundling-and-minification.md) or the cleansing of a development environment before a new build.</span></span>

<span data-ttu-id="fcdea-116">Zestaw zadań Gulp jest zdefiniowany w *gulpfile.js*.</span><span class="sxs-lookup"><span data-stu-id="fcdea-116">A set of Gulp tasks is defined in *gulpfile.js*.</span></span> <span data-ttu-id="fcdea-117">Następujący kod JavaScript zawiera modułów Gulp i określa ścieżki plików można odwoływać się w ciągu przyszłych zadań:</span><span class="sxs-lookup"><span data-stu-id="fcdea-117">The following JavaScript includes Gulp modules and specifies file paths to be referenced within the forthcoming tasks:</span></span>

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

<span data-ttu-id="fcdea-118">Powyższy kod określa, które moduły węzła są wymagane.</span><span class="sxs-lookup"><span data-stu-id="fcdea-118">The above code specifies which Node modules are required.</span></span> <span data-ttu-id="fcdea-119">`require` Funkcja importuje poszczególnych modułów, aby zadania zależne mogą wykorzystywać swoje funkcje.</span><span class="sxs-lookup"><span data-stu-id="fcdea-119">The `require` function imports each module so that the dependent tasks can utilize their features.</span></span> <span data-ttu-id="fcdea-120">Każdego z zaimportowanych modułów jest przypisany do zmiennej.</span><span class="sxs-lookup"><span data-stu-id="fcdea-120">Each of the imported modules is assigned to a variable.</span></span> <span data-ttu-id="fcdea-121">Moduły można odnaleźć, albo według nazwy lub ścieżki.</span><span class="sxs-lookup"><span data-stu-id="fcdea-121">The modules can be located either by name or path.</span></span> <span data-ttu-id="fcdea-122">W tym przykładzie modułów o nazwie `gulp`, `rimraf`, `gulp-concat`, `gulp-cssmin`, i `gulp-uglify` są pobierane przez nazwę.</span><span class="sxs-lookup"><span data-stu-id="fcdea-122">In this example, the modules named `gulp`, `rimraf`, `gulp-concat`, `gulp-cssmin`, and `gulp-uglify` are retrieved by name.</span></span> <span data-ttu-id="fcdea-123">Ponadto szereg ścieżki są tworzone, dzięki czemu lokalizacje plików CSS i JavaScript można użyć ponownie i odwołuje zadań.</span><span class="sxs-lookup"><span data-stu-id="fcdea-123">Additionally, a series of paths are created so that the locations of CSS and JavaScript files can be reused and referenced within the tasks.</span></span> <span data-ttu-id="fcdea-124">Poniższa tabela zawiera opisy modułów objęte *gulpfile.js*.</span><span class="sxs-lookup"><span data-stu-id="fcdea-124">The following table provides descriptions of the modules included in *gulpfile.js*.</span></span>

| <span data-ttu-id="fcdea-125">Nazwa modułu</span><span class="sxs-lookup"><span data-stu-id="fcdea-125">Module Name</span></span> | <span data-ttu-id="fcdea-126">Opis</span><span class="sxs-lookup"><span data-stu-id="fcdea-126">Description</span></span> |
| ----------- | ----------- |
| <span data-ttu-id="fcdea-127">gulp</span><span class="sxs-lookup"><span data-stu-id="fcdea-127">gulp</span></span>        | <span data-ttu-id="fcdea-128">Gulp przesyłania strumieniowego system kompilacji.</span><span class="sxs-lookup"><span data-stu-id="fcdea-128">The Gulp streaming build system.</span></span> <span data-ttu-id="fcdea-129">Aby uzyskać więcej informacji, zobacz [system gulp](https://www.npmjs.com/package/gulp).</span><span class="sxs-lookup"><span data-stu-id="fcdea-129">For more information, see [gulp](https://www.npmjs.com/package/gulp).</span></span> |
| <span data-ttu-id="fcdea-130">rimraf</span><span class="sxs-lookup"><span data-stu-id="fcdea-130">rimraf</span></span>      | <span data-ttu-id="fcdea-131">Moduł usuwania węzła.</span><span class="sxs-lookup"><span data-stu-id="fcdea-131">A Node deletion module.</span></span> <span data-ttu-id="fcdea-132">Aby uzyskać więcej informacji, zobacz [rimraf](https://www.npmjs.com/package/rimraf).</span><span class="sxs-lookup"><span data-stu-id="fcdea-132">For more information, see [rimraf](https://www.npmjs.com/package/rimraf).</span></span> |
| <span data-ttu-id="fcdea-133">gulp concat</span><span class="sxs-lookup"><span data-stu-id="fcdea-133">gulp-concat</span></span> | <span data-ttu-id="fcdea-134">Moduł, który łączy pliki oparte na znak nowego wiersza systemu operacyjnego.</span><span class="sxs-lookup"><span data-stu-id="fcdea-134">A module that concatenates files based on the operating system's newline character.</span></span> <span data-ttu-id="fcdea-135">Aby uzyskać więcej informacji, zobacz [gulp concat](https://www.npmjs.com/package/gulp-concat).</span><span class="sxs-lookup"><span data-stu-id="fcdea-135">For more information, see [gulp-concat](https://www.npmjs.com/package/gulp-concat).</span></span> |
| <span data-ttu-id="fcdea-136">gulp-cssmin</span><span class="sxs-lookup"><span data-stu-id="fcdea-136">gulp-cssmin</span></span> | <span data-ttu-id="fcdea-137">Moduł, który minimalizuje pliki CSS.</span><span class="sxs-lookup"><span data-stu-id="fcdea-137">A module that minifies CSS files.</span></span> <span data-ttu-id="fcdea-138">Aby uzyskać więcej informacji, zobacz [gulp cssmin](https://www.npmjs.com/package/gulp-cssmin).</span><span class="sxs-lookup"><span data-stu-id="fcdea-138">For more information, see [gulp-cssmin](https://www.npmjs.com/package/gulp-cssmin).</span></span> |
| <span data-ttu-id="fcdea-139">gulp uglify</span><span class="sxs-lookup"><span data-stu-id="fcdea-139">gulp-uglify</span></span> | <span data-ttu-id="fcdea-140">Moduł, który minimalizuje *js* plików.</span><span class="sxs-lookup"><span data-stu-id="fcdea-140">A module that minifies *.js* files.</span></span> <span data-ttu-id="fcdea-141">Aby uzyskać więcej informacji, zobacz [gulp uglify](https://www.npmjs.com/package/gulp-uglify).</span><span class="sxs-lookup"><span data-stu-id="fcdea-141">For more information, see [gulp-uglify](https://www.npmjs.com/package/gulp-uglify).</span></span> |

<span data-ttu-id="fcdea-142">Gdy wymagane moduły są importowane, można określić zadania.</span><span class="sxs-lookup"><span data-stu-id="fcdea-142">Once the requisite modules are imported, the tasks can be specified.</span></span> <span data-ttu-id="fcdea-143">W tym miejscu jest sześć zadań zarejestrowany, reprezentowany przez następujący kod:</span><span class="sxs-lookup"><span data-stu-id="fcdea-143">Here there are six tasks registered, represented by the following code:</span></span>

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

<span data-ttu-id="fcdea-144">Poniższa tabela zawiera objaśnienie zadania określone w powyższym kodzie:</span><span class="sxs-lookup"><span data-stu-id="fcdea-144">The following table provides an explanation of the tasks specified in the code above:</span></span>

|<span data-ttu-id="fcdea-145">Nazwa zadania</span><span class="sxs-lookup"><span data-stu-id="fcdea-145">Task Name</span></span>|<span data-ttu-id="fcdea-146">Opis</span><span class="sxs-lookup"><span data-stu-id="fcdea-146">Description</span></span>|
|--- |--- |
|<span data-ttu-id="fcdea-147">clean:js</span><span class="sxs-lookup"><span data-stu-id="fcdea-147">clean:js</span></span>|<span data-ttu-id="fcdea-148">Zadanie, które używa modułu usunięcie węzła rimraf usunąć zminimalizowany wersję pliku site.js.</span><span class="sxs-lookup"><span data-stu-id="fcdea-148">A task that uses the rimraf Node deletion module to remove the minified version of the site.js file.</span></span>|
|<span data-ttu-id="fcdea-149">Wyczyść: css</span><span class="sxs-lookup"><span data-stu-id="fcdea-149">clean:css</span></span>|<span data-ttu-id="fcdea-150">Zadanie, które używa modułu usunięcie węzła rimraf usunąć zminimalizowany wersję pliku site.css.</span><span class="sxs-lookup"><span data-stu-id="fcdea-150">A task that uses the rimraf Node deletion module to remove the minified version of the site.css file.</span></span>|
|<span data-ttu-id="fcdea-151">Czyszczenie</span><span class="sxs-lookup"><span data-stu-id="fcdea-151">clean</span></span>|<span data-ttu-id="fcdea-152">Zadanie, które wywołuje `clean:js` zadań, a następnie `clean:css` zadań.</span><span class="sxs-lookup"><span data-stu-id="fcdea-152">A task that calls the `clean:js` task, followed by the `clean:css` task.</span></span>|
|<span data-ttu-id="fcdea-153">min:js</span><span class="sxs-lookup"><span data-stu-id="fcdea-153">min:js</span></span>|<span data-ttu-id="fcdea-154">Zadanie, które minimalizuje i łączy wszystkie pliki js znajdujących się w folderze js.</span><span class="sxs-lookup"><span data-stu-id="fcdea-154">A task that minifies and concatenates all .js files within the js folder.</span></span> <span data-ttu-id="fcdea-155">. Pliki min.js są wyłączone.</span><span class="sxs-lookup"><span data-stu-id="fcdea-155">The .min.js files are excluded.</span></span>|
|<span data-ttu-id="fcdea-156">min:css</span><span class="sxs-lookup"><span data-stu-id="fcdea-156">min:css</span></span>|<span data-ttu-id="fcdea-157">Zadanie, które minimalizuje i łączy wszystkie pliki CSS w folderze css.</span><span class="sxs-lookup"><span data-stu-id="fcdea-157">A task that minifies and concatenates all .css files within the css folder.</span></span> <span data-ttu-id="fcdea-158">. Pliki min.css są wyłączone.</span><span class="sxs-lookup"><span data-stu-id="fcdea-158">The .min.css files are excluded.</span></span>|
|<span data-ttu-id="fcdea-159">min</span><span class="sxs-lookup"><span data-stu-id="fcdea-159">min</span></span>|<span data-ttu-id="fcdea-160">Zadanie, które wywołuje `min:js` zadań, a następnie `min:css` zadań.</span><span class="sxs-lookup"><span data-stu-id="fcdea-160">A task that calls the `min:js` task, followed by the `min:css` task.</span></span>|

## <a name="running-default-tasks"></a><span data-ttu-id="fcdea-161">Uruchomione zadania domyślne</span><span class="sxs-lookup"><span data-stu-id="fcdea-161">Running default tasks</span></span>

<span data-ttu-id="fcdea-162">Jeśli jeszcze nie utworzono nową aplikację sieci Web, Utwórz nowy projekt aplikacji sieci Web ASP.NET w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="fcdea-162">If you haven't already created a new Web app, create a new ASP.NET Web Application project in Visual Studio.</span></span>

1.  <span data-ttu-id="fcdea-163">Dodaj nowy plik JavaScript do projektu i nadaj mu nazwę *gulpfile.js*, następnie skopiuj poniższy kod.</span><span class="sxs-lookup"><span data-stu-id="fcdea-163">Add a new JavaScript file to your project and name it *gulpfile.js*, then copy the following code.</span></span>

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

2.  <span data-ttu-id="fcdea-164">Otwórz *package.json* pliku (Dodawanie, jeśli nie istnieje) i Dodaj następujący kod.</span><span class="sxs-lookup"><span data-stu-id="fcdea-164">Open the *package.json* file (add if not there) and add the following.</span></span>

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

3.  <span data-ttu-id="fcdea-165">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *gulpfile.js*i wybierz **Eksploratora modułu uruchamiającego zadania**.</span><span class="sxs-lookup"><span data-stu-id="fcdea-165">In **Solution Explorer**, right-click *gulpfile.js*, and select **Task Runner Explorer**.</span></span>
    
    ![Otwórz Eksploratora modułu uruchamiającego zadania w Eksploratorze rozwiązań](using-gulp/_static/02-SolutionExplorer-TaskRunnerExplorer.png)
    
    <span data-ttu-id="fcdea-167">**Zadanie Eksploratora modułu uruchamiającego** z listą zadań Gulp.</span><span class="sxs-lookup"><span data-stu-id="fcdea-167">**Task Runner Explorer** shows the list of Gulp tasks.</span></span> <span data-ttu-id="fcdea-168">(Może być konieczne kliknięcie pozycji **Odśwież** przycisku, który wydaje się po lewej stronie nazwy projektu.)</span><span class="sxs-lookup"><span data-stu-id="fcdea-168">(You might have to click the **Refresh** button that appears to the left of the project name.)</span></span>
    
    ![Eksploratora modułu uruchamiającego zadania](using-gulp/_static/03-TaskRunnerExplorer.png)
    
    > [!IMPORTANT]
    > <span data-ttu-id="fcdea-170">**Eksploratora modułu uruchamiającego zadania** elementu menu kontekstowego jest wyświetlana tylko wtedy, gdy *gulpfile.js* znajduje się w katalogu głównym projektu.</span><span class="sxs-lookup"><span data-stu-id="fcdea-170">The **Task Runner Explorer** context menu item appears only if *gulpfile.js* is in the root project directory.</span></span>

4.  <span data-ttu-id="fcdea-171">Underneath **zadania** w **Eksploratora modułu uruchamiającego zadania**, kliknij prawym przyciskiem myszy **czystą**i wybierz **Uruchom** z menu podręcznego.</span><span class="sxs-lookup"><span data-stu-id="fcdea-171">Underneath **Tasks** in **Task Runner Explorer**, right-click **clean**, and select **Run** from the pop-up menu.</span></span>

    ![Zadanie czystą Eksploratora modułu uruchamiającego zadania](using-gulp/_static/04-TaskRunner-clean.png)

    <span data-ttu-id="fcdea-173">**Zadanie Eksploratora modułu uruchamiającego** utworzy nową kartę o nazwie **czystą** i wykonać czystą zadania zdefiniowanej w *gulpfile.js*.</span><span class="sxs-lookup"><span data-stu-id="fcdea-173">**Task Runner Explorer** will create a new tab named **clean** and execute the clean task as it's defined in *gulpfile.js*.</span></span>

5.  <span data-ttu-id="fcdea-174">Kliknij prawym przyciskiem myszy **czystą** zadań, a następnie wybierz **powiązania** > **przed kompilacji**.</span><span class="sxs-lookup"><span data-stu-id="fcdea-174">Right-click the **clean** task, then select **Bindings** > **Before Build**.</span></span>

    ![Powiązanie BeforeBuild Eksploratora modułu uruchamiającego zadania](using-gulp/_static/05-TaskRunner-BeforeBuild.png)

    <span data-ttu-id="fcdea-176">**Przed kompilacji** powiązania konfiguruje czystą zadanie do automatycznego uruchamiania przed każdym kompilacji projektu.</span><span class="sxs-lookup"><span data-stu-id="fcdea-176">The **Before Build** binding configures the clean task to run automatically before each build of the project.</span></span>

<span data-ttu-id="fcdea-177">Powiązania można skonfigurować przy użyciu **Eksploratora modułu uruchamiającego zadania** są przechowywane w formie komentarzy w górnej części programu *gulpfile.js* i obowiązują tylko w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="fcdea-177">The bindings you set up with **Task Runner Explorer** are stored in the form of a comment at the top of your *gulpfile.js* and are effective only in Visual Studio.</span></span> <span data-ttu-id="fcdea-178">Alternatywa, która nie wymaga programu Visual Studio jest skonfigurowanie automatycznego wykonywania zadań gulp w Twojej *.csproj* pliku.</span><span class="sxs-lookup"><span data-stu-id="fcdea-178">An alternative that doesn't require Visual Studio is to configure automatic execution of gulp tasks in your *.csproj* file.</span></span> <span data-ttu-id="fcdea-179">Na przykład umieścić Twojej *.csproj* pliku:</span><span class="sxs-lookup"><span data-stu-id="fcdea-179">For example, put this in your *.csproj* file:</span></span>

```xml
<Target Name="MyPreCompileTarget" BeforeTargets="Build">
  <Exec Command="gulp clean" />
</Target>
```

<span data-ttu-id="fcdea-180">Teraz czystą zadanie jest wykonywane po uruchomieniu projektu programu Visual Studio lub z wiersza polecenia przy użyciu [dotnet Uruchom](/dotnet/core/tools/dotnet-run) polecenia (Uruchom `npm install` pierwszy).</span><span class="sxs-lookup"><span data-stu-id="fcdea-180">Now the clean task is executed when you run the project in Visual Studio or from a command prompt using the [dotnet run](/dotnet/core/tools/dotnet-run) command (run `npm install` first).</span></span>

## <a name="defining-and-running-a-new-task"></a><span data-ttu-id="fcdea-181">Definiowanie i uruchamiania nowego zadania</span><span class="sxs-lookup"><span data-stu-id="fcdea-181">Defining and running a new task</span></span>

<span data-ttu-id="fcdea-182">Aby zdefiniować nowe zadanie Gulp, zmodyfikuj *gulpfile.js*.</span><span class="sxs-lookup"><span data-stu-id="fcdea-182">To define a new Gulp task, modify *gulpfile.js*.</span></span>

1.  <span data-ttu-id="fcdea-183">Dodaj następujący kod JavaScript do końca *gulpfile.js*:</span><span class="sxs-lookup"><span data-stu-id="fcdea-183">Add the following JavaScript to the end of *gulpfile.js*:</span></span>

    ```javascript
    gulp.task("first", function () {
      console.log('first task! <-----');
    });
    ```

    <span data-ttu-id="fcdea-184">To zadanie jest o nazwie `first`, i po prostu wyświetla ciąg.</span><span class="sxs-lookup"><span data-stu-id="fcdea-184">This task is named `first`, and it simply displays a string.</span></span>

2.  <span data-ttu-id="fcdea-185">Zapisz *gulpfile.js*.</span><span class="sxs-lookup"><span data-stu-id="fcdea-185">Save *gulpfile.js*.</span></span>

3.  <span data-ttu-id="fcdea-186">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *gulpfile.js*i wybierz *Eksploratora modułu uruchamiającego zadania*.</span><span class="sxs-lookup"><span data-stu-id="fcdea-186">In **Solution Explorer**, right-click *gulpfile.js*, and select *Task Runner Explorer*.</span></span>

4.  <span data-ttu-id="fcdea-187">W **Eksploratora modułu uruchamiającego zadania**, kliknij prawym przyciskiem myszy **pierwszy**i wybierz **Uruchom**.</span><span class="sxs-lookup"><span data-stu-id="fcdea-187">In **Task Runner Explorer**, right-click **first**, and select **Run**.</span></span>

    ![Uruchom zadanie pierwszy Eksploratora modułu uruchamiającego zadania](using-gulp/_static/06-TaskRunner-First.png)

    <span data-ttu-id="fcdea-189">Tekst wyjściowy jest wyświetlany.</span><span class="sxs-lookup"><span data-stu-id="fcdea-189">The output text is displayed.</span></span> <span data-ttu-id="fcdea-190">Aby wyświetlić przykłady oparte na typowych scenariuszy, zobacz [system Gulp przepisami](#gulp-recipes).</span><span class="sxs-lookup"><span data-stu-id="fcdea-190">To see examples based on common scenarios, see [Gulp Recipes](#gulp-recipes).</span></span>

## <a name="defining-and-running-tasks-in-a-series"></a><span data-ttu-id="fcdea-191">Definiowanie i uruchamianie zadań w serii</span><span class="sxs-lookup"><span data-stu-id="fcdea-191">Defining and running tasks in a series</span></span>

<span data-ttu-id="fcdea-192">Po uruchomieniu zadania wielu zadań jednocześnie domyślnie uruchamiane.</span><span class="sxs-lookup"><span data-stu-id="fcdea-192">When you run multiple tasks, the tasks run concurrently by default.</span></span> <span data-ttu-id="fcdea-193">Jednak jeśli chcesz uruchamiać zadania w określonej kolejności, należy określić podczas każdego zadania zostanie zakończone, a także jako zadania, które są zależne od zakończenia inne zadanie.</span><span class="sxs-lookup"><span data-stu-id="fcdea-193">However, if you need to run tasks in a specific order, you must specify when each task is complete, as well as which tasks depend on the completion of another task.</span></span>

1.  <span data-ttu-id="fcdea-194">Aby zdefiniować serię zadań do wykonania w kolejności, Zastąp `first` zadań dodanych powyżej w *gulpfile.js* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="fcdea-194">To define a series of tasks to run in order, replace the `first` task that you added above in *gulpfile.js* with the following:</span></span>

    ```javascript
    gulp.task("series:first", function () {
      console.log('first task! <-----');
    });
 
    gulp.task("series:second", ["series:first"], function () {
      console.log('second task! <-----');
    });
 
    gulp.task("series", ["series:first", "series:second"], function () {});
    ```
 
    <span data-ttu-id="fcdea-195">Masz teraz trzy zadania: `series:first`, `series:second`, i `series`.</span><span class="sxs-lookup"><span data-stu-id="fcdea-195">You now have three tasks: `series:first`, `series:second`, and `series`.</span></span> <span data-ttu-id="fcdea-196">`series:second` Zadanie zawiera drugi parametr, który określa tablicę zadania do uruchomienia, a ukończone przed `series:second` zadanie zostanie uruchomione.</span><span class="sxs-lookup"><span data-stu-id="fcdea-196">The `series:second` task includes a second parameter which specifies an array of tasks to be run and completed before the `series:second` task will run.</span></span> <span data-ttu-id="fcdea-197">Jak określono w kodzie powyżej, tylko `series:first` zadań muszą zostać wykonane przed `series:second` zadanie zostanie uruchomione.</span><span class="sxs-lookup"><span data-stu-id="fcdea-197">As specified in the code above, only the `series:first` task must be completed before the `series:second` task will run.</span></span>

2.  <span data-ttu-id="fcdea-198">Zapisz *gulpfile.js*.</span><span class="sxs-lookup"><span data-stu-id="fcdea-198">Save *gulpfile.js*.</span></span>

3.  <span data-ttu-id="fcdea-199">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *gulpfile.js* i wybierz **Eksploratora modułu uruchamiającego zadania** Jeśli nie jest już otwarty.</span><span class="sxs-lookup"><span data-stu-id="fcdea-199">In **Solution Explorer**, right-click *gulpfile.js* and select **Task Runner Explorer** if it isn't already open.</span></span>

4.  <span data-ttu-id="fcdea-200">W **Eksploratora modułu uruchamiającego zadania**, kliknij prawym przyciskiem myszy **serii** i wybierz **Uruchom**.</span><span class="sxs-lookup"><span data-stu-id="fcdea-200">In **Task Runner Explorer**, right-click **series** and select **Run**.</span></span>

    ![Uruchom zadanie serii Eksploratora modułu uruchamiającego zadania](using-gulp/_static/07-TaskRunner-Series.png)

## <a name="intellisense"></a><span data-ttu-id="fcdea-202">IntelliSense</span><span class="sxs-lookup"><span data-stu-id="fcdea-202">IntelliSense</span></span>

<span data-ttu-id="fcdea-203">IntelliSense zawiera kod zakończenia, opisy parametrów i innych funkcji, aby zwiększyć wydajność i zmniejszyć błędy.</span><span class="sxs-lookup"><span data-stu-id="fcdea-203">IntelliSense provides code completion, parameter descriptions, and other features to boost productivity and to decrease errors.</span></span> <span data-ttu-id="fcdea-204">Zadania gulp są napisane w języku JavaScript. w związku z tym IntelliSense zapewniają pomoc podczas tworzenia.</span><span class="sxs-lookup"><span data-stu-id="fcdea-204">Gulp tasks are written in JavaScript; therefore, IntelliSense can provide assistance while developing.</span></span> <span data-ttu-id="fcdea-205">Podczas pracy z użyciem języka JavaScript, IntelliSense wyświetla obiekty, funkcji, właściwości i parametrów, które są dostępne na podstawie użytkownika bieżącego kontekstu.</span><span class="sxs-lookup"><span data-stu-id="fcdea-205">As you work with JavaScript, IntelliSense lists the objects, functions, properties, and parameters that are available based on your current context.</span></span> <span data-ttu-id="fcdea-206">Wybierz opcję kodowania z podanej listy wyskakujących przez funkcję IntelliSense kodu.</span><span class="sxs-lookup"><span data-stu-id="fcdea-206">Select a coding option from the pop-up list provided by IntelliSense to complete the code.</span></span>

![System gulp IntelliSense](using-gulp/_static/08-IntelliSense.png)

<span data-ttu-id="fcdea-208">Aby uzyskać więcej informacji o funkcji IntelliSense, zobacz [IntelliSense dla JavaScript](https://docs.microsoft.com/visualstudio/ide/javascript-intellisense).</span><span class="sxs-lookup"><span data-stu-id="fcdea-208">For more information about IntelliSense, see [JavaScript IntelliSense](https://docs.microsoft.com/visualstudio/ide/javascript-intellisense).</span></span>

## <a name="development-staging-and-production-environments"></a><span data-ttu-id="fcdea-209">Środowiska deweloperskie, przemieszczania i produkcji</span><span class="sxs-lookup"><span data-stu-id="fcdea-209">Development, staging, and production environments</span></span>

<span data-ttu-id="fcdea-210">W przypadku Gulp w celu zoptymalizowania plików po stronie klienta dla tymczasowych i produkcyjnych przetworzonych plików są zapisywane w lokalizacji lokalnej tymczasową i produkcyjną.</span><span class="sxs-lookup"><span data-stu-id="fcdea-210">When Gulp is used to optimize client-side files for staging and production, the processed files are saved to a local staging and production location.</span></span> <span data-ttu-id="fcdea-211">*_Layout.cshtml* plików używa **środowiska** tagu pomocnika zapewnienie dwie różne wersje plików CSS.</span><span class="sxs-lookup"><span data-stu-id="fcdea-211">The *_Layout.cshtml* file uses the **environment** tag helper to provide two different versions of CSS files.</span></span> <span data-ttu-id="fcdea-212">Jednej wersji plików CSS do rozwoju i innych wersji zoptymalizowano pod kątem zarówno tymczasową i produkcyjną.</span><span class="sxs-lookup"><span data-stu-id="fcdea-212">One version of CSS files is for development and the other version is optimized for both staging and production.</span></span> <span data-ttu-id="fcdea-213">W Visual Studio 2017 r, jeśli zmienisz **ASPNETCORE_ENVIRONMENT** zmienną środowiskową `Production`, Visual Studio utworzy aplikację sieci Web i łącza do zminimalizowane pliki CSS.</span><span class="sxs-lookup"><span data-stu-id="fcdea-213">In Visual Studio 2017, when you change the **ASPNETCORE_ENVIRONMENT** environment variable to `Production`, Visual Studio will build the Web app and link to the minimized CSS files.</span></span> <span data-ttu-id="fcdea-214">Poniżej przedstawiono znaczników **środowiska** zawierający tagów łącza do pomocników tagów `Development` CSS plików i zminimalizowany `Staging, Production` pliki CSS.</span><span class="sxs-lookup"><span data-stu-id="fcdea-214">The following markup shows the **environment** tag helpers containing link tags to the `Development` CSS files and the minified `Staging, Production` CSS files.</span></span>

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

## <a name="switching-between-environments"></a><span data-ttu-id="fcdea-215">Przełączanie między środowiskami</span><span class="sxs-lookup"><span data-stu-id="fcdea-215">Switching between environments</span></span>

<span data-ttu-id="fcdea-216">Aby przełączyć między kompilowania kodu dla różnych środowisk, należy zmodyfikować **ASPNETCORE_ENVIRONMENT** wartość zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="fcdea-216">To switch between compiling for different environments, modify the **ASPNETCORE_ENVIRONMENT** environment variable's value.</span></span>

1.  <span data-ttu-id="fcdea-217">W **Eksploratora modułu uruchamiającego zadania**, upewnij się, że **min** zadań została ustawiona do uruchamiania **przed kompilacji**.</span><span class="sxs-lookup"><span data-stu-id="fcdea-217">In **Task Runner Explorer**, verify that the **min** task has been set to run **Before Build**.</span></span>

2.  <span data-ttu-id="fcdea-218">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy nazwę projektu i wybierz **właściwości**.</span><span class="sxs-lookup"><span data-stu-id="fcdea-218">In **Solution Explorer**, right-click the project name and select **Properties**.</span></span>

    <span data-ttu-id="fcdea-219">Arkusz właściwości dla aplikacji sieci Web jest wyświetlany.</span><span class="sxs-lookup"><span data-stu-id="fcdea-219">The property sheet for the Web app is displayed.</span></span>

3.  <span data-ttu-id="fcdea-220">Kliknij przycisk **debugowania** kartę.</span><span class="sxs-lookup"><span data-stu-id="fcdea-220">Click the **Debug** tab.</span></span>

4.  <span data-ttu-id="fcdea-221">Ustaw wartość **hostingu: środowisko** zmienną środowiskową `Production`.</span><span class="sxs-lookup"><span data-stu-id="fcdea-221">Set the value of the **Hosting:Environment** environment variable to `Production`.</span></span>

5.  <span data-ttu-id="fcdea-222">Naciśnij klawisz **F5** Aby uruchomić aplikację w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="fcdea-222">Press **F5** to run the application in a browser.</span></span>

6.  <span data-ttu-id="fcdea-223">W oknie przeglądarki, kliknij prawym przyciskiem myszy strony, a następnie wybierz **Wyświetl źródło** Aby wyświetlić kod HTML dla strony.</span><span class="sxs-lookup"><span data-stu-id="fcdea-223">In the browser window, right-click the page and select **View Source** to view the HTML for the page.</span></span>

    <span data-ttu-id="fcdea-224">Należy zauważyć, że łączy arkusza stylów wskaż zminimalizowany pliki CSS.</span><span class="sxs-lookup"><span data-stu-id="fcdea-224">Notice that the stylesheet links point to the minified CSS files.</span></span>

7.  <span data-ttu-id="fcdea-225">Zamknij przeglądarkę, aby zatrzymać aplikacji sieci Web.</span><span class="sxs-lookup"><span data-stu-id="fcdea-225">Close the browser to stop the Web app.</span></span>

8.  <span data-ttu-id="fcdea-226">W programie Visual Studio, wróć do arkusza właściwości dla aplikacji sieci Web i zmienić **hostingu: środowisko** z powrotem do zmiennej środowiskowej `Development`.</span><span class="sxs-lookup"><span data-stu-id="fcdea-226">In Visual Studio, return to the property sheet for the Web app and change the **Hosting:Environment** environment variable back to `Development`.</span></span>

9.  <span data-ttu-id="fcdea-227">Naciśnij klawisz **F5** Aby ponownie uruchomić aplikację w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="fcdea-227">Press **F5** to run the application in a browser again.</span></span>

10. <span data-ttu-id="fcdea-228">W oknie przeglądarki, kliknij prawym przyciskiem myszy strony, a następnie wybierz **Wyświetl źródło** Aby wyświetlić kod HTML dla strony.</span><span class="sxs-lookup"><span data-stu-id="fcdea-228">In the browser window, right-click the page and select **View Source** to see the HTML for the page.</span></span>

    <span data-ttu-id="fcdea-229">Należy zauważyć, że łączy arkusza stylów wskaż unminified wersje plików CSS.</span><span class="sxs-lookup"><span data-stu-id="fcdea-229">Notice that the stylesheet links point to the unminified versions of the CSS files.</span></span>

<span data-ttu-id="fcdea-230">Aby uzyskać więcej informacji związanych z środowiska w programie ASP.NET Core, zobacz [pracy w środowiskach wielu](../fundamentals/environments.md).</span><span class="sxs-lookup"><span data-stu-id="fcdea-230">For more information related to environments in ASP.NET Core, see [Work with multiple environments](../fundamentals/environments.md).</span></span>

## <a name="task-and-module-details"></a><span data-ttu-id="fcdea-231">Szczegóły zadania i modułu</span><span class="sxs-lookup"><span data-stu-id="fcdea-231">Task and module details</span></span>

<span data-ttu-id="fcdea-232">Zadanie Gulp jest zarejestrowana nazwa funkcji.</span><span class="sxs-lookup"><span data-stu-id="fcdea-232">A Gulp task is registered with a function name.</span></span> <span data-ttu-id="fcdea-233">Jeśli inne zadania musi zostać uruchomiony przed bieżącym zadań można określić zależności.</span><span class="sxs-lookup"><span data-stu-id="fcdea-233">You can specify dependencies if other tasks must run before the current task.</span></span> <span data-ttu-id="fcdea-234">Dodatkowe funkcje umożliwiają uruchamianie i obejrzyj Gulp zadania, a także Ustaw źródło (*src*) i docelowego (*dest*) plików jest modyfikowany.</span><span class="sxs-lookup"><span data-stu-id="fcdea-234">Additional functions allow you to run and watch the Gulp tasks, as well as set the source (*src*) and destination (*dest*) of the files being modified.</span></span> <span data-ttu-id="fcdea-235">Poniżej przedstawiono podstawowe funkcje system Gulp interfejsu API:</span><span class="sxs-lookup"><span data-stu-id="fcdea-235">The following are the primary Gulp API functions:</span></span>

|<span data-ttu-id="fcdea-236">Funkcja gulp</span><span class="sxs-lookup"><span data-stu-id="fcdea-236">Gulp Function</span></span>|<span data-ttu-id="fcdea-237">Składnia</span><span class="sxs-lookup"><span data-stu-id="fcdea-237">Syntax</span></span>|<span data-ttu-id="fcdea-238">Opis</span><span class="sxs-lookup"><span data-stu-id="fcdea-238">Description</span></span>|
|---   |--- |--- |
|<span data-ttu-id="fcdea-239">— zadanie</span><span class="sxs-lookup"><span data-stu-id="fcdea-239">task</span></span>  |`gulp.task(name[, deps], fn) { }`|<span data-ttu-id="fcdea-240">`task` Funkcja tworzy zadanie.</span><span class="sxs-lookup"><span data-stu-id="fcdea-240">The `task` function creates a task.</span></span> <span data-ttu-id="fcdea-241">`name` Parametr określa nazwę zadania.</span><span class="sxs-lookup"><span data-stu-id="fcdea-241">The `name` parameter defines the name of the task.</span></span> <span data-ttu-id="fcdea-242">`deps` Parametr zawiera tablicę zadań do wykonania przed uruchomieniem tego zadania.</span><span class="sxs-lookup"><span data-stu-id="fcdea-242">The `deps` parameter contains an array of tasks to be completed before this task runs.</span></span> <span data-ttu-id="fcdea-243">`fn` Parametr reprezentuje funkcję wywołania zwrotnego, która wykonuje operacje zadania.</span><span class="sxs-lookup"><span data-stu-id="fcdea-243">The `fn` parameter represents a callback function which performs the operations of the task.</span></span>|
|<span data-ttu-id="fcdea-244">Czujki</span><span class="sxs-lookup"><span data-stu-id="fcdea-244">watch</span></span> |`gulp.watch(glob [, opts], tasks) { }`|<span data-ttu-id="fcdea-245">`watch` Funkcja monitoruje pliki i uruchamia zadania po zmianie pliku.</span><span class="sxs-lookup"><span data-stu-id="fcdea-245">The `watch` function monitors files and runs tasks when a file change occurs.</span></span> <span data-ttu-id="fcdea-246">`glob` Parametr jest `string` lub `array` Określa, które pliki, aby obejrzeć.</span><span class="sxs-lookup"><span data-stu-id="fcdea-246">The `glob` parameter is a `string` or `array` that determines which files to watch.</span></span> <span data-ttu-id="fcdea-247">`opts` Parametru zapewnia dodatkowy plik obserwowanie opcje.</span><span class="sxs-lookup"><span data-stu-id="fcdea-247">The `opts` parameter provides additional file watching options.</span></span>|
|<span data-ttu-id="fcdea-248">src</span><span class="sxs-lookup"><span data-stu-id="fcdea-248">src</span></span>   |`gulp.src(globs[, options]) { }`|<span data-ttu-id="fcdea-249">`src` Funkcja zawiera pliki, które są zgodne z wartościami glob.</span><span class="sxs-lookup"><span data-stu-id="fcdea-249">The `src` function provides files that match the glob value(s).</span></span> <span data-ttu-id="fcdea-250">`glob` Parametr jest `string` lub `array` Określa, które pliki do odczytu.</span><span class="sxs-lookup"><span data-stu-id="fcdea-250">The `glob` parameter is a `string` or `array` that determines which files to read.</span></span> <span data-ttu-id="fcdea-251">`options` Parametru zapewnia dodatkowe opcje pliku.</span><span class="sxs-lookup"><span data-stu-id="fcdea-251">The `options` parameter provides additional file options.</span></span>|
|<span data-ttu-id="fcdea-252">Docelowy</span><span class="sxs-lookup"><span data-stu-id="fcdea-252">dest</span></span>  |`gulp.dest(path[, options]) { }`|<span data-ttu-id="fcdea-253">`dest` Funkcja określa lokalizację, do której można zapisywać pliki.</span><span class="sxs-lookup"><span data-stu-id="fcdea-253">The `dest` function defines a location to which files can be written.</span></span> <span data-ttu-id="fcdea-254">`path` Parametr jest ciągiem lub funkcja, która określa folder docelowy.</span><span class="sxs-lookup"><span data-stu-id="fcdea-254">The `path` parameter is a string or function that determines the destination folder.</span></span> <span data-ttu-id="fcdea-255">`options` Parametr jest obiekt, który określa opcje folderów danych wyjściowych.</span><span class="sxs-lookup"><span data-stu-id="fcdea-255">The `options` parameter is an object that specifies output folder options.</span></span>|

<span data-ttu-id="fcdea-256">Aby uzyskać dodatkowe informacje system Gulp interfejsu API zobacz [system Gulp API Docs](https://github.com/gulpjs/gulp/blob/master/docs/API.md).</span><span class="sxs-lookup"><span data-stu-id="fcdea-256">For additional Gulp API reference information, see [Gulp Docs API](https://github.com/gulpjs/gulp/blob/master/docs/API.md).</span></span>

## <a name="gulp-recipes"></a><span data-ttu-id="fcdea-257">System gulp przepisami</span><span class="sxs-lookup"><span data-stu-id="fcdea-257">Gulp recipes</span></span>

<span data-ttu-id="fcdea-258">Społeczność Gulp oferuje Gulp [przepisami](https://github.com/gulpjs/gulp/blob/master/docs/recipes/README.md).</span><span class="sxs-lookup"><span data-stu-id="fcdea-258">The Gulp community provides Gulp [Recipes](https://github.com/gulpjs/gulp/blob/master/docs/recipes/README.md).</span></span> <span data-ttu-id="fcdea-259">Te przepisami składają się z Gulp zadań w celu rozwiązania typowych scenariuszy.</span><span class="sxs-lookup"><span data-stu-id="fcdea-259">These recipes consist of Gulp tasks to address common scenarios.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="fcdea-260">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="fcdea-260">Additional resources</span></span>

* [<span data-ttu-id="fcdea-261">Dokumentacja gulp</span><span class="sxs-lookup"><span data-stu-id="fcdea-261">Gulp documentation</span></span>](https://github.com/gulpjs/gulp/blob/master/docs/README.md)
* [<span data-ttu-id="fcdea-262">Tworzenie pakietów i minimalizowanie w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fcdea-262">Bundling and minification in ASP.NET Core</span></span>](bundling-and-minification.md)
* [<span data-ttu-id="fcdea-263">Użyj Grunt w platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fcdea-263">Use Grunt in ASP.NET Core</span></span>](using-grunt.md)
