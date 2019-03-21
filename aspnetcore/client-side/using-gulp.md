---
title: Korzystanie z Gulp w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak korzystanie z Gulp w programie ASP.NET Core.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 10/04/2018
uid: client-side/using-gulp
ms.openlocfilehash: 9f6d03a1e8a81bceca15cb1e1aa664c22c31e1d3
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58209875"
---
# <a name="use-gulp-in-aspnet-core"></a><span data-ttu-id="afa17-103">Korzystanie z Gulp w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="afa17-103">Use Gulp in ASP.NET Core</span></span>

<span data-ttu-id="afa17-104">Przez [Scott Addie](https://scottaddie.com), [Shayne Boyer](https://twitter.com/spboyer), i [sosny David](https://twitter.com/davidpine7)</span><span class="sxs-lookup"><span data-stu-id="afa17-104">By [Scott Addie](https://scottaddie.com), [Shayne Boyer](https://twitter.com/spboyer), and [David Pine](https://twitter.com/davidpine7)</span></span>

<span data-ttu-id="afa17-105">W aplikacji Typowa Nowoczesna sieć web może być procesu kompilacji:</span><span class="sxs-lookup"><span data-stu-id="afa17-105">In a typical modern web app, the build process might:</span></span>

* <span data-ttu-id="afa17-106">Tworzenie pakietów i minimalizowanie plików JavaScript i CSS.</span><span class="sxs-lookup"><span data-stu-id="afa17-106">Bundle and minify JavaScript and CSS files.</span></span>
* <span data-ttu-id="afa17-107">Uruchom narzędzia, aby wywołać tworzenie pakietów i minimalizowanie zadań przed każdą kompilacją.</span><span class="sxs-lookup"><span data-stu-id="afa17-107">Run tools to call the bundling and minification tasks before each build.</span></span>
* <span data-ttu-id="afa17-108">Skompilować mniej lub SASS pliki CSS.</span><span class="sxs-lookup"><span data-stu-id="afa17-108">Compile LESS or SASS files to CSS.</span></span>
* <span data-ttu-id="afa17-109">Skompiluj pliki CoffeeScript lub TypeScript w kodzie JavaScript.</span><span class="sxs-lookup"><span data-stu-id="afa17-109">Compile CoffeeScript or TypeScript files to JavaScript.</span></span>

<span data-ttu-id="afa17-110">A *modułu uruchamiającego zadania* to narzędzie, które automatyzuje te zadania tworzenia procedur i nie tylko.</span><span class="sxs-lookup"><span data-stu-id="afa17-110">A *task runner* is a tool which automates these routine development tasks and more.</span></span> <span data-ttu-id="afa17-111">Program Visual Studio udostępnia wbudowaną obsługę dla dwóch uczestników popularne zadania oparte na języku JavaScript: [Program gulp](https://gulpjs.com/) i [Grunt](using-grunt.md).</span><span class="sxs-lookup"><span data-stu-id="afa17-111">Visual Studio provides built-in support for two popular JavaScript-based task runners: [Gulp](https://gulpjs.com/) and [Grunt](using-grunt.md).</span></span>

## <a name="gulp"></a><span data-ttu-id="afa17-112">Gulp</span><span class="sxs-lookup"><span data-stu-id="afa17-112">Gulp</span></span>

<span data-ttu-id="afa17-113">Gulp to oparte na języku JavaScript przesyłania strumieniowego kompilacji narzędzi kod po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="afa17-113">Gulp is a JavaScript-based streaming build toolkit for client-side code.</span></span> <span data-ttu-id="afa17-114">Często służy do przesyłania plików po stronie klienta za pomocą szeregu procesy po wyzwoleniu określonego zdarzenia w środowisku kompilacji.</span><span class="sxs-lookup"><span data-stu-id="afa17-114">It's commonly used to stream client-side files through a series of processes when a specific event is triggered in a build environment.</span></span> <span data-ttu-id="afa17-115">Na przykład Gulp może służyć do automatyzowania [tworzenie pakietów i minimalizowanie](bundling-and-minification.md) lub czyszczenie środowisko programistyczne przed nową kompilację.</span><span class="sxs-lookup"><span data-stu-id="afa17-115">For instance, Gulp can be used to automate [bundling and minification](bundling-and-minification.md) or the cleaning of a development environment before a new build.</span></span>

<span data-ttu-id="afa17-116">Zestaw zadań Gulp jest zdefiniowany w *gulpfile.js*.</span><span class="sxs-lookup"><span data-stu-id="afa17-116">A set of Gulp tasks is defined in *gulpfile.js*.</span></span> <span data-ttu-id="afa17-117">Następujący kod JavaScript obejmują moduły Gulp i określa ścieżki plików, aby odwoływać się w ciągu przyszłych zadań:</span><span class="sxs-lookup"><span data-stu-id="afa17-117">The following JavaScript includes Gulp modules and specifies file paths to be referenced within the forthcoming tasks:</span></span>

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

<span data-ttu-id="afa17-118">Powyższy kod określa, które moduły węzła są wymagane.</span><span class="sxs-lookup"><span data-stu-id="afa17-118">The above code specifies which Node modules are required.</span></span> <span data-ttu-id="afa17-119">`require` Funkcja importuje każdego modułu, aby zadania zależne, mogą korzystać z ich funkcje.</span><span class="sxs-lookup"><span data-stu-id="afa17-119">The `require` function imports each module so that the dependent tasks can utilize their features.</span></span> <span data-ttu-id="afa17-120">Każdego z zaimportowanych modułów jest przypisany do zmiennej.</span><span class="sxs-lookup"><span data-stu-id="afa17-120">Each of the imported modules is assigned to a variable.</span></span> <span data-ttu-id="afa17-121">Moduły można znaleźć, albo według nazwy lub ścieżki.</span><span class="sxs-lookup"><span data-stu-id="afa17-121">The modules can be located either by name or path.</span></span> <span data-ttu-id="afa17-122">W tym przykładzie modułów o nazwie `gulp`, `rimraf`, `gulp-concat`, `gulp-cssmin`, i `gulp-uglify` są pobierane przez nazwę.</span><span class="sxs-lookup"><span data-stu-id="afa17-122">In this example, the modules named `gulp`, `rimraf`, `gulp-concat`, `gulp-cssmin`, and `gulp-uglify` are retrieved by name.</span></span> <span data-ttu-id="afa17-123">Ponadto seria ścieżek są tworzone tak, aby lokalizacje plików CSS i JavaScript, które mogą być ponownie użyte i przywoływany w ramach zadania.</span><span class="sxs-lookup"><span data-stu-id="afa17-123">Additionally, a series of paths are created so that the locations of CSS and JavaScript files can be reused and referenced within the tasks.</span></span> <span data-ttu-id="afa17-124">Poniższa tabela zawiera opis modułów objęte *gulpfile.js*.</span><span class="sxs-lookup"><span data-stu-id="afa17-124">The following table provides descriptions of the modules included in *gulpfile.js*.</span></span>

| <span data-ttu-id="afa17-125">Nazwa modułu</span><span class="sxs-lookup"><span data-stu-id="afa17-125">Module Name</span></span> | <span data-ttu-id="afa17-126">Opis</span><span class="sxs-lookup"><span data-stu-id="afa17-126">Description</span></span> |
| ----------- | ----------- |
| <span data-ttu-id="afa17-127">Program gulp</span><span class="sxs-lookup"><span data-stu-id="afa17-127">gulp</span></span>        | <span data-ttu-id="afa17-128">System kompilacji Gulp, przesyłania strumieniowego.</span><span class="sxs-lookup"><span data-stu-id="afa17-128">The Gulp streaming build system.</span></span> <span data-ttu-id="afa17-129">Aby uzyskać więcej informacji, zobacz [gulp](https://www.npmjs.com/package/gulp).</span><span class="sxs-lookup"><span data-stu-id="afa17-129">For more information, see [gulp](https://www.npmjs.com/package/gulp).</span></span> |
| <span data-ttu-id="afa17-130">rimraf</span><span class="sxs-lookup"><span data-stu-id="afa17-130">rimraf</span></span>      | <span data-ttu-id="afa17-131">Moduł usuwania węzła.</span><span class="sxs-lookup"><span data-stu-id="afa17-131">A Node deletion module.</span></span> <span data-ttu-id="afa17-132">Aby uzyskać więcej informacji, zobacz [rimraf](https://www.npmjs.com/package/rimraf).</span><span class="sxs-lookup"><span data-stu-id="afa17-132">For more information, see [rimraf](https://www.npmjs.com/package/rimraf).</span></span> |
| <span data-ttu-id="afa17-133">gulp concat</span><span class="sxs-lookup"><span data-stu-id="afa17-133">gulp-concat</span></span> | <span data-ttu-id="afa17-134">Moduł, który łączy pliki, w oparciu o system operacyjny znak nowego wiersza.</span><span class="sxs-lookup"><span data-stu-id="afa17-134">A module that concatenates files based on the operating system's newline character.</span></span> <span data-ttu-id="afa17-135">Aby uzyskać więcej informacji, zobacz [gulp concat](https://www.npmjs.com/package/gulp-concat).</span><span class="sxs-lookup"><span data-stu-id="afa17-135">For more information, see [gulp-concat](https://www.npmjs.com/package/gulp-concat).</span></span> |
| <span data-ttu-id="afa17-136">gulp-cssmin</span><span class="sxs-lookup"><span data-stu-id="afa17-136">gulp-cssmin</span></span> | <span data-ttu-id="afa17-137">Moduł, który minimalizuje pliki CSS.</span><span class="sxs-lookup"><span data-stu-id="afa17-137">A module that minifies CSS files.</span></span> <span data-ttu-id="afa17-138">Aby uzyskać więcej informacji, zobacz [gulp cssmin](https://www.npmjs.com/package/gulp-cssmin).</span><span class="sxs-lookup"><span data-stu-id="afa17-138">For more information, see [gulp-cssmin](https://www.npmjs.com/package/gulp-cssmin).</span></span> |
| <span data-ttu-id="afa17-139">gulp uglify</span><span class="sxs-lookup"><span data-stu-id="afa17-139">gulp-uglify</span></span> | <span data-ttu-id="afa17-140">Moduł, który minimalizuje *js* plików.</span><span class="sxs-lookup"><span data-stu-id="afa17-140">A module that minifies *.js* files.</span></span> <span data-ttu-id="afa17-141">Aby uzyskać więcej informacji, zobacz [gulp uglify](https://www.npmjs.com/package/gulp-uglify).</span><span class="sxs-lookup"><span data-stu-id="afa17-141">For more information, see [gulp-uglify](https://www.npmjs.com/package/gulp-uglify).</span></span> |

<span data-ttu-id="afa17-142">Gdy wymagane moduły są importowane, można określić zadania.</span><span class="sxs-lookup"><span data-stu-id="afa17-142">Once the requisite modules are imported, the tasks can be specified.</span></span> <span data-ttu-id="afa17-143">Oto sześciu zadań podrzędnych zarejestrowany, reprezentowane przez następujący kod:</span><span class="sxs-lookup"><span data-stu-id="afa17-143">Here there are six tasks registered, represented by the following code:</span></span>

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

<span data-ttu-id="afa17-144">Poniższa tabela zawiera wyjaśnienie zadania określone w powyższym kodzie:</span><span class="sxs-lookup"><span data-stu-id="afa17-144">The following table provides an explanation of the tasks specified in the code above:</span></span>

|<span data-ttu-id="afa17-145">Nazwa zadania</span><span class="sxs-lookup"><span data-stu-id="afa17-145">Task Name</span></span>|<span data-ttu-id="afa17-146">Opis</span><span class="sxs-lookup"><span data-stu-id="afa17-146">Description</span></span>|
|--- |--- |
|<span data-ttu-id="afa17-147">Wyczyść: js</span><span class="sxs-lookup"><span data-stu-id="afa17-147">clean:js</span></span>|<span data-ttu-id="afa17-148">Zadanie, które używa modułu usuwania węzła rimraf do usunięcia zminimalizowany wersję pliku site.js.</span><span class="sxs-lookup"><span data-stu-id="afa17-148">A task that uses the rimraf Node deletion module to remove the minified version of the site.js file.</span></span>|
|<span data-ttu-id="afa17-149">Wyczyść: css</span><span class="sxs-lookup"><span data-stu-id="afa17-149">clean:css</span></span>|<span data-ttu-id="afa17-150">Zadanie, które używa modułu usuwania węzła rimraf do usunięcia zminimalizowany wersję pliku site.css.</span><span class="sxs-lookup"><span data-stu-id="afa17-150">A task that uses the rimraf Node deletion module to remove the minified version of the site.css file.</span></span>|
|<span data-ttu-id="afa17-151">czyszczenie</span><span class="sxs-lookup"><span data-stu-id="afa17-151">clean</span></span>|<span data-ttu-id="afa17-152">Zadanie, które wywołuje `clean:js` zadania, a następnie `clean:css` zadania.</span><span class="sxs-lookup"><span data-stu-id="afa17-152">A task that calls the `clean:js` task, followed by the `clean:css` task.</span></span>|
|<span data-ttu-id="afa17-153">min:js</span><span class="sxs-lookup"><span data-stu-id="afa17-153">min:js</span></span>|<span data-ttu-id="afa17-154">Zadanie, które minimalizuje i łączy wszystkie pliki .js znajdujące się w folderze js.</span><span class="sxs-lookup"><span data-stu-id="afa17-154">A task that minifies and concatenates all .js files within the js folder.</span></span> <span data-ttu-id="afa17-155">. Pliki min.js są wyłączone.</span><span class="sxs-lookup"><span data-stu-id="afa17-155">The .min.js files are excluded.</span></span>|
|<span data-ttu-id="afa17-156">min:css</span><span class="sxs-lookup"><span data-stu-id="afa17-156">min:css</span></span>|<span data-ttu-id="afa17-157">Zadanie, które minimalizuje i łączy wszystkie pliki CSS w folderze css.</span><span class="sxs-lookup"><span data-stu-id="afa17-157">A task that minifies and concatenates all .css files within the css folder.</span></span> <span data-ttu-id="afa17-158">. Pliki min.css są wyłączone.</span><span class="sxs-lookup"><span data-stu-id="afa17-158">The .min.css files are excluded.</span></span>|
|<span data-ttu-id="afa17-159">min</span><span class="sxs-lookup"><span data-stu-id="afa17-159">min</span></span>|<span data-ttu-id="afa17-160">Zadanie, które wywołuje `min:js` zadania, a następnie `min:css` zadania.</span><span class="sxs-lookup"><span data-stu-id="afa17-160">A task that calls the `min:js` task, followed by the `min:css` task.</span></span>|

## <a name="running-default-tasks"></a><span data-ttu-id="afa17-161">Uruchamianie zadań domyślne</span><span class="sxs-lookup"><span data-stu-id="afa17-161">Running default tasks</span></span>

<span data-ttu-id="afa17-162">Jeśli nie zostało jeszcze utworzone Nowa aplikacja sieci Web, należy utworzyć nowy projekt aplikacji sieci Web ASP.NET w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="afa17-162">If you haven't already created a new Web app, create a new ASP.NET Web Application project in Visual Studio.</span></span>

1. <span data-ttu-id="afa17-163">Otwórz *package.json* pliku (Dodaj Jeśli nie istnieje) i Dodaj następujący kod.</span><span class="sxs-lookup"><span data-stu-id="afa17-163">Open the *package.json* file (add if not there) and add the following.</span></span>

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

2. <span data-ttu-id="afa17-164">Dodaj nowy plik JavaScript do projektu i nadaj mu nazwę *gulpfile.js*, następnie skopiuj następujący kod.</span><span class="sxs-lookup"><span data-stu-id="afa17-164">Add a new JavaScript file to your project and name it *gulpfile.js*, then copy the following code.</span></span>

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

3. <span data-ttu-id="afa17-165">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *gulpfile.js*i wybierz **Eksplorator modułu uruchamiającego zadania**.</span><span class="sxs-lookup"><span data-stu-id="afa17-165">In **Solution Explorer**, right-click *gulpfile.js*, and select **Task Runner Explorer**.</span></span>

    ![Otwórz Eksplorator modułu uruchamiającego zadania za pomocą Eksploratora rozwiązań](using-gulp/_static/02-SolutionExplorer-TaskRunnerExplorer.png)

    <span data-ttu-id="afa17-167">**Task Runner Explorer** pokazuje listę zadań Gulp.</span><span class="sxs-lookup"><span data-stu-id="afa17-167">**Task Runner Explorer** shows the list of Gulp tasks.</span></span> <span data-ttu-id="afa17-168">(Być może trzeba kliknąć **Odśwież** przycisk, który pojawia się po lewej stronie nazwy projektu.)</span><span class="sxs-lookup"><span data-stu-id="afa17-168">(You might have to click the **Refresh** button that appears to the left of the project name.)</span></span>

    ![Eksplorator modułu uruchamiającego zadania](using-gulp/_static/03-TaskRunnerExplorer.png)

    > [!IMPORTANT]
    > <span data-ttu-id="afa17-170">**Eksplorator modułu uruchamiającego zadania** element menu kontekstowego pojawia się tylko wtedy, gdy *gulpfile.js* znajduje się w katalogu głównym projektu.</span><span class="sxs-lookup"><span data-stu-id="afa17-170">The **Task Runner Explorer** context menu item appears only if *gulpfile.js* is in the root project directory.</span></span>

4. <span data-ttu-id="afa17-171">Poniżej **zadania** w **Eksplorator modułu uruchamiającego zadania**, kliknij prawym przyciskiem myszy **czyste**i wybierz **Uruchom** z menu podręcznego.</span><span class="sxs-lookup"><span data-stu-id="afa17-171">Underneath **Tasks** in **Task Runner Explorer**, right-click **clean**, and select **Run** from the pop-up menu.</span></span>

    ![Zadanie czysty Eksplorator modułu uruchamiającego zadania](using-gulp/_static/04-TaskRunner-clean.png)

    <span data-ttu-id="afa17-173">**Task Runner Explorer** zostanie utworzona nowa karta o nazwie **czyste** i wykonać zadanie czysty, ponieważ jest on zdefiniowany w *gulpfile.js*.</span><span class="sxs-lookup"><span data-stu-id="afa17-173">**Task Runner Explorer** will create a new tab named **clean** and execute the clean task as it's defined in *gulpfile.js*.</span></span>

5. <span data-ttu-id="afa17-174">Kliknij prawym przyciskiem myszy **czyste** zadań, a następnie wybierz **powiązania** > **przed kompilacji**.</span><span class="sxs-lookup"><span data-stu-id="afa17-174">Right-click the **clean** task, then select **Bindings** > **Before Build**.</span></span>

    ![Powiązanie BeforeBuild Eksplorator modułu uruchamiającego zadania](using-gulp/_static/05-TaskRunner-BeforeBuild.png)

    <span data-ttu-id="afa17-176">**Przed kompilacji** powiązania konfiguruje zadanie czysty, aby automatycznie uruchomić przed każdą kompilacją projektu.</span><span class="sxs-lookup"><span data-stu-id="afa17-176">The **Before Build** binding configures the clean task to run automatically before each build of the project.</span></span>

<span data-ttu-id="afa17-177">Powiązania określone przy użyciu **Eksplorator modułu uruchamiającego zadania** są przechowywane w formie komentarza, w górnej części Twojej *gulpfile.js* i obowiązują tylko w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="afa17-177">The bindings you set up with **Task Runner Explorer** are stored in the form of a comment at the top of your *gulpfile.js* and are effective only in Visual Studio.</span></span> <span data-ttu-id="afa17-178">Alternatywa, która nie wymaga programu Visual Studio jest skonfigurowanie automatycznego wykonywania zadań gulp w swojej *.csproj* pliku.</span><span class="sxs-lookup"><span data-stu-id="afa17-178">An alternative that doesn't require Visual Studio is to configure automatic execution of gulp tasks in your *.csproj* file.</span></span> <span data-ttu-id="afa17-179">Na przykład, należy umieścić swojej *.csproj* pliku:</span><span class="sxs-lookup"><span data-stu-id="afa17-179">For example, put this in your *.csproj* file:</span></span>

```xml
<Target Name="MyPreCompileTarget" BeforeTargets="Build">
  <Exec Command="gulp clean" />
</Target>
```

<span data-ttu-id="afa17-180">Teraz zadanie czysty jest wykonywany, gdy uruchamiasz projekt w programie Visual Studio lub z wiersza polecenia przy użyciu [dotnet, uruchom](/dotnet/core/tools/dotnet-run) polecenia (uruchamianie `npm install` pierwszy).</span><span class="sxs-lookup"><span data-stu-id="afa17-180">Now the clean task is executed when you run the project in Visual Studio or from a command prompt using the [dotnet run](/dotnet/core/tools/dotnet-run) command (run `npm install` first).</span></span>

## <a name="defining-and-running-a-new-task"></a><span data-ttu-id="afa17-181">Definiowanie i uruchomione nowe zadanie</span><span class="sxs-lookup"><span data-stu-id="afa17-181">Defining and running a new task</span></span>

<span data-ttu-id="afa17-182">Aby zdefiniować nowe zadanie Gulp, zmodyfikuj *gulpfile.js*.</span><span class="sxs-lookup"><span data-stu-id="afa17-182">To define a new Gulp task, modify *gulpfile.js*.</span></span>

1. <span data-ttu-id="afa17-183">Dodaj następujący kod JavaScript do końca *gulpfile.js*:</span><span class="sxs-lookup"><span data-stu-id="afa17-183">Add the following JavaScript to the end of *gulpfile.js*:</span></span>

    ```javascript
    gulp.task('first', done => {
      console.log('first task! <-----');
      done(); // signal completion
    });
    ```

    <span data-ttu-id="afa17-184">To zadanie o nazwie `first`, i po prostu wyświetla ciąg.</span><span class="sxs-lookup"><span data-stu-id="afa17-184">This task is named `first`, and it simply displays a string.</span></span>

2. <span data-ttu-id="afa17-185">Zapisz *gulpfile.js*.</span><span class="sxs-lookup"><span data-stu-id="afa17-185">Save *gulpfile.js*.</span></span>

3. <span data-ttu-id="afa17-186">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *gulpfile.js*i wybierz *Eksplorator modułu uruchamiającego zadania*.</span><span class="sxs-lookup"><span data-stu-id="afa17-186">In **Solution Explorer**, right-click *gulpfile.js*, and select *Task Runner Explorer*.</span></span>

4. <span data-ttu-id="afa17-187">W **Eksplorator modułu uruchamiającego zadania**, kliknij prawym przyciskiem myszy **pierwszy**i wybierz **Uruchom**.</span><span class="sxs-lookup"><span data-stu-id="afa17-187">In **Task Runner Explorer**, right-click **first**, and select **Run**.</span></span>

    ![Uruchamianie pierwszego zadania Eksplorator modułu uruchamiającego zadania](using-gulp/_static/06-TaskRunner-First.png)

    <span data-ttu-id="afa17-189">Tekst wyjściowy jest wyświetlany.</span><span class="sxs-lookup"><span data-stu-id="afa17-189">The output text is displayed.</span></span> <span data-ttu-id="afa17-190">Aby wyświetlić przykłady oparte na typowych scenariuszy, zobacz [Gulp przepisy](#gulp-recipes).</span><span class="sxs-lookup"><span data-stu-id="afa17-190">To see examples based on common scenarios, see [Gulp Recipes](#gulp-recipes).</span></span>

## <a name="defining-and-running-tasks-in-a-series"></a><span data-ttu-id="afa17-191">Definiowanie i uruchamianie zadań w serii</span><span class="sxs-lookup"><span data-stu-id="afa17-191">Defining and running tasks in a series</span></span>

<span data-ttu-id="afa17-192">Po uruchomieniu wielu zadań, zadań jednocześnie domyślnie uruchamiane.</span><span class="sxs-lookup"><span data-stu-id="afa17-192">When you run multiple tasks, the tasks run concurrently by default.</span></span> <span data-ttu-id="afa17-193">Jednak jeśli zachodzi potrzeba uruchamiania zadań w określonej kolejności, należy określić kiedy każde zadanie jest zakończone, jak również jako zadania, które zależą od zakończenia inne zadanie.</span><span class="sxs-lookup"><span data-stu-id="afa17-193">However, if you need to run tasks in a specific order, you must specify when each task is complete, as well as which tasks depend on the completion of another task.</span></span>

1. <span data-ttu-id="afa17-194">Aby zdefiniować szereg zadań do wykonania w kolejności, Zastąp `first` zadanie, które zostały dodane powyżej w *gulpfile.js* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="afa17-194">To define a series of tasks to run in order, replace the `first` task that you added above in *gulpfile.js* with the following:</span></span>

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

    <span data-ttu-id="afa17-195">Masz teraz trzy zadania: `series:first`, `series:second`, i `series`.</span><span class="sxs-lookup"><span data-stu-id="afa17-195">You now have three tasks: `series:first`, `series:second`, and `series`.</span></span> <span data-ttu-id="afa17-196">`series:second` Zadanie zawiera drugi parametr, który określa tablicę zadań do uruchomienia i ukończone przed `series:second` zadanie zostanie uruchomione.</span><span class="sxs-lookup"><span data-stu-id="afa17-196">The `series:second` task includes a second parameter which specifies an array of tasks to be run and completed before the `series:second` task will run.</span></span> <span data-ttu-id="afa17-197">Jak określono w kodzie powyżej tylko `series:first` zadań muszą zostać wykonane przed `series:second` zadanie zostanie uruchomione.</span><span class="sxs-lookup"><span data-stu-id="afa17-197">As specified in the code above, only the `series:first` task must be completed before the `series:second` task will run.</span></span>

2. <span data-ttu-id="afa17-198">Zapisz *gulpfile.js*.</span><span class="sxs-lookup"><span data-stu-id="afa17-198">Save *gulpfile.js*.</span></span>

3. <span data-ttu-id="afa17-199">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *gulpfile.js* i wybierz **Eksplorator modułu uruchamiającego zadania** Jeśli nie jest już otwarty.</span><span class="sxs-lookup"><span data-stu-id="afa17-199">In **Solution Explorer**, right-click *gulpfile.js* and select **Task Runner Explorer** if it isn't already open.</span></span>

4. <span data-ttu-id="afa17-200">W **Eksplorator modułu uruchamiającego zadania**, kliknij prawym przyciskiem myszy **serii** i wybierz **Uruchom**.</span><span class="sxs-lookup"><span data-stu-id="afa17-200">In **Task Runner Explorer**, right-click **series** and select **Run**.</span></span>

    ![Uruchom zadanie serii Eksplorator modułu uruchamiającego zadania](using-gulp/_static/07-TaskRunner-Series.png)

## <a name="intellisense"></a><span data-ttu-id="afa17-202">IntelliSense</span><span class="sxs-lookup"><span data-stu-id="afa17-202">IntelliSense</span></span>

<span data-ttu-id="afa17-203">Technologia IntelliSense zawiera uzupełniania kodu, opisy parametrów i inne funkcje, aby zwiększyć wydajność i zmniejszyć błędy.</span><span class="sxs-lookup"><span data-stu-id="afa17-203">IntelliSense provides code completion, parameter descriptions, and other features to boost productivity and to decrease errors.</span></span> <span data-ttu-id="afa17-204">Zadań gulp są napisane w języku JavaScript. w związku z tym funkcja IntelliSense może zapewnić pomoc podczas tworzenia.</span><span class="sxs-lookup"><span data-stu-id="afa17-204">Gulp tasks are written in JavaScript; therefore, IntelliSense can provide assistance while developing.</span></span> <span data-ttu-id="afa17-205">Podczas pracy z użyciem języka JavaScript, IntelliSense wyświetla listę obiektów, funkcji, właściwości i parametrów, które są dostępne na podstawie Twojego bieżącego kontekstu.</span><span class="sxs-lookup"><span data-stu-id="afa17-205">As you work with JavaScript, IntelliSense lists the objects, functions, properties, and parameters that are available based on your current context.</span></span> <span data-ttu-id="afa17-206">Wybierz opcję kodowania z wyskakującej listy dostarczonej przez technologię IntelliSense, aby uzupełnić kod.</span><span class="sxs-lookup"><span data-stu-id="afa17-206">Select a coding option from the pop-up list provided by IntelliSense to complete the code.</span></span>

![gulp IntelliSense](using-gulp/_static/08-IntelliSense.png)

<span data-ttu-id="afa17-208">Aby uzyskać więcej informacji na temat technologii IntelliSense, zobacz [JavaScript IntelliSense](/visualstudio/ide/javascript-intellisense).</span><span class="sxs-lookup"><span data-stu-id="afa17-208">For more information about IntelliSense, see [JavaScript IntelliSense](/visualstudio/ide/javascript-intellisense).</span></span>

## <a name="development-staging-and-production-environments"></a><span data-ttu-id="afa17-209">Środowiska deweloperskie, przejściowe i produkcji</span><span class="sxs-lookup"><span data-stu-id="afa17-209">Development, staging, and production environments</span></span>

<span data-ttu-id="afa17-210">Po Gulp jest używana do optymalizacji pliki po stronie klienta dla środowisk przejściowych i produkcyjnych, przetworzone pliki są zapisywane w lokalizacji lokalnej środowisk przejściowych i produkcyjnych.</span><span class="sxs-lookup"><span data-stu-id="afa17-210">When Gulp is used to optimize client-side files for staging and production, the processed files are saved to a local staging and production location.</span></span> <span data-ttu-id="afa17-211">*_Layout.cshtml* plików używa **środowiska** tag pomocnika, aby zapewnić dwie różne wersje plików CSS.</span><span class="sxs-lookup"><span data-stu-id="afa17-211">The *_Layout.cshtml* file uses the **environment** tag helper to provide two different versions of CSS files.</span></span> <span data-ttu-id="afa17-212">Jest jedna wersja plików CSS do tworzenia aplikacji, a druga wersja jest zoptymalizowany do przemieszczania i produkcji.</span><span class="sxs-lookup"><span data-stu-id="afa17-212">One version of CSS files is for development and the other version is optimized for both staging and production.</span></span> <span data-ttu-id="afa17-213">W programie Visual Studio 2017 po zmianie **ASPNETCORE_ENVIRONMENT** zmiennej środowiskowej, aby `Production`, Visual Studio utworzy aplikację sieci Web i łącza do plików CSS w trybie zminimalizowanym.</span><span class="sxs-lookup"><span data-stu-id="afa17-213">In Visual Studio 2017, when you change the **ASPNETCORE_ENVIRONMENT** environment variable to `Production`, Visual Studio will build the Web app and link to the minimized CSS files.</span></span> <span data-ttu-id="afa17-214">Ilustruje poniższy kod znaczników **środowiska** zawierający tagi łącza do pomocników tagów `Development` CSS plików i zminimalizowany `Staging, Production` pliki CSS.</span><span class="sxs-lookup"><span data-stu-id="afa17-214">The following markup shows the **environment** tag helpers containing link tags to the `Development` CSS files and the minified `Staging, Production` CSS files.</span></span>

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

## <a name="switching-between-environments"></a><span data-ttu-id="afa17-215">Przełączanie między środowiskami</span><span class="sxs-lookup"><span data-stu-id="afa17-215">Switching between environments</span></span>

<span data-ttu-id="afa17-216">Aby przełączać się między kompilowanie dla różnych środowisk, należy zmodyfikować **ASPNETCORE_ENVIRONMENT** wartość zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="afa17-216">To switch between compiling for different environments, modify the **ASPNETCORE_ENVIRONMENT** environment variable's value.</span></span>

1. <span data-ttu-id="afa17-217">W **Eksplorator modułu uruchamiającego zadania**, upewnij się, że **min** zadania został ustawiony na uruchomienie **przed kompilacji**.</span><span class="sxs-lookup"><span data-stu-id="afa17-217">In **Task Runner Explorer**, verify that the **min** task has been set to run **Before Build**.</span></span>

2. <span data-ttu-id="afa17-218">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy nazwę projektu i wybierz **właściwości**.</span><span class="sxs-lookup"><span data-stu-id="afa17-218">In **Solution Explorer**, right-click the project name and select **Properties**.</span></span>

    <span data-ttu-id="afa17-219">Arkusz właściwości dla aplikacji sieci Web jest wyświetlany.</span><span class="sxs-lookup"><span data-stu-id="afa17-219">The property sheet for the Web app is displayed.</span></span>

3. <span data-ttu-id="afa17-220">Kliknij przycisk **debugowania** kartę.</span><span class="sxs-lookup"><span data-stu-id="afa17-220">Click the **Debug** tab.</span></span>

4. <span data-ttu-id="afa17-221">Ustaw wartość **hostingu: środowisko** zmiennej środowiskowej, aby `Production`.</span><span class="sxs-lookup"><span data-stu-id="afa17-221">Set the value of the **Hosting:Environment** environment variable to `Production`.</span></span>

5. <span data-ttu-id="afa17-222">Naciśnij klawisz **F5** do uruchamiania aplikacji w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="afa17-222">Press **F5** to run the application in a browser.</span></span>

6. <span data-ttu-id="afa17-223">W oknie przeglądarki, kliknij prawym przyciskiem myszy strony, a następnie wybierz **Wyświetl źródło** Aby wyświetlić kod HTML dla strony.</span><span class="sxs-lookup"><span data-stu-id="afa17-223">In the browser window, right-click the page and select **View Source** to view the HTML for the page.</span></span>

    <span data-ttu-id="afa17-224">Należy zauważyć, że łączy arkusza stylów wskazują zminimalizowany pliki CSS.</span><span class="sxs-lookup"><span data-stu-id="afa17-224">Notice that the stylesheet links point to the minified CSS files.</span></span>

7. <span data-ttu-id="afa17-225">Zamknij przeglądarkę, aby zatrzymać aplikacji sieci Web.</span><span class="sxs-lookup"><span data-stu-id="afa17-225">Close the browser to stop the Web app.</span></span>

8. <span data-ttu-id="afa17-226">W programie Visual Studio, wróć do arkusza właściwości dla aplikacji sieci Web i zmień **hostingu: środowisko** z powrotem do zmiennej środowiskowej `Development`.</span><span class="sxs-lookup"><span data-stu-id="afa17-226">In Visual Studio, return to the property sheet for the Web app and change the **Hosting:Environment** environment variable back to `Development`.</span></span>

9. <span data-ttu-id="afa17-227">Naciśnij klawisz **F5** ponownie uruchomić aplikację w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="afa17-227">Press **F5** to run the application in a browser again.</span></span>

10. <span data-ttu-id="afa17-228">W oknie przeglądarki, kliknij prawym przyciskiem myszy strony, a następnie wybierz **Wyświetl źródło** Aby wyświetlić kod HTML dla strony.</span><span class="sxs-lookup"><span data-stu-id="afa17-228">In the browser window, right-click the page and select **View Source** to see the HTML for the page.</span></span>

    <span data-ttu-id="afa17-229">Należy zauważyć, że łączy arkusza stylów wskazują unminified wersje plików CSS.</span><span class="sxs-lookup"><span data-stu-id="afa17-229">Notice that the stylesheet links point to the unminified versions of the CSS files.</span></span>

<span data-ttu-id="afa17-230">Aby uzyskać więcej informacji związanych z środowiska w programie ASP.NET Core, zobacz [używanie wielu środowisk](../fundamentals/environments.md).</span><span class="sxs-lookup"><span data-stu-id="afa17-230">For more information related to environments in ASP.NET Core, see [Use multiple environments](../fundamentals/environments.md).</span></span>

## <a name="task-and-module-details"></a><span data-ttu-id="afa17-231">Szczegóły zadania i modułu</span><span class="sxs-lookup"><span data-stu-id="afa17-231">Task and module details</span></span>

<span data-ttu-id="afa17-232">Zadań Gulp jest zarejestrowana z nazwą funkcji.</span><span class="sxs-lookup"><span data-stu-id="afa17-232">A Gulp task is registered with a function name.</span></span> <span data-ttu-id="afa17-233">Można określić zależności, jeśli inne zadania, musi zostać uruchomiony przed bieżącym zadaniu.</span><span class="sxs-lookup"><span data-stu-id="afa17-233">You can specify dependencies if other tasks must run before the current task.</span></span> <span data-ttu-id="afa17-234">Dodatkowe funkcje umożliwiają uruchamianie i obejrzyj zadań Gulp, a także ustawić źródło (*src*) i docelowy (*dest*) plików jest modyfikowany.</span><span class="sxs-lookup"><span data-stu-id="afa17-234">Additional functions allow you to run and watch the Gulp tasks, as well as set the source (*src*) and destination (*dest*) of the files being modified.</span></span> <span data-ttu-id="afa17-235">Poniżej przedstawiono podstawowe funkcje Gulp interfejsu API:</span><span class="sxs-lookup"><span data-stu-id="afa17-235">The following are the primary Gulp API functions:</span></span>

|<span data-ttu-id="afa17-236">Program gulp — funkcja</span><span class="sxs-lookup"><span data-stu-id="afa17-236">Gulp Function</span></span>|<span data-ttu-id="afa17-237">Składnia</span><span class="sxs-lookup"><span data-stu-id="afa17-237">Syntax</span></span>|<span data-ttu-id="afa17-238">Opis</span><span class="sxs-lookup"><span data-stu-id="afa17-238">Description</span></span>|
|---   |--- |--- |
|<span data-ttu-id="afa17-239"> — zadanie</span><span class="sxs-lookup"><span data-stu-id="afa17-239">task</span></span>  |`gulp.task(name[, deps], fn) { }`|<span data-ttu-id="afa17-240">`task` Funkcja tworzy zadanie.</span><span class="sxs-lookup"><span data-stu-id="afa17-240">The `task` function creates a task.</span></span> <span data-ttu-id="afa17-241">`name` Parametr określa nazwę zadania.</span><span class="sxs-lookup"><span data-stu-id="afa17-241">The `name` parameter defines the name of the task.</span></span> <span data-ttu-id="afa17-242">`deps` Parametr zawiera tablicę zadań do wykonania przed uruchomieniem tego zadania.</span><span class="sxs-lookup"><span data-stu-id="afa17-242">The `deps` parameter contains an array of tasks to be completed before this task runs.</span></span> <span data-ttu-id="afa17-243">`fn` Parametr reprezentuje funkcję wywołania zwrotnego, która wykonuje operacje zadania.</span><span class="sxs-lookup"><span data-stu-id="afa17-243">The `fn` parameter represents a callback function which performs the operations of the task.</span></span>|
|<span data-ttu-id="afa17-244">Wyrażenie kontrolne</span><span class="sxs-lookup"><span data-stu-id="afa17-244">watch</span></span> |`gulp.watch(glob [, opts], tasks) { }`|<span data-ttu-id="afa17-245">`watch` Funkcja monitoruje zadania plików i przebiegi po zmianie pliku.</span><span class="sxs-lookup"><span data-stu-id="afa17-245">The `watch` function monitors files and runs tasks when a file change occurs.</span></span> <span data-ttu-id="afa17-246">`glob` Parametr jest `string` lub `array` określający, plików, których obejrzeć.</span><span class="sxs-lookup"><span data-stu-id="afa17-246">The `glob` parameter is a `string` or `array` that determines which files to watch.</span></span> <span data-ttu-id="afa17-247">`opts` Parametr zawiera dodatkowy plik oglądania opcje.</span><span class="sxs-lookup"><span data-stu-id="afa17-247">The `opts` parameter provides additional file watching options.</span></span>|
|<span data-ttu-id="afa17-248">src</span><span class="sxs-lookup"><span data-stu-id="afa17-248">src</span></span>   |`gulp.src(globs[, options]) { }`|<span data-ttu-id="afa17-249">`src` Funkcja zawiera pliki, które odpowiadają wartości glob.</span><span class="sxs-lookup"><span data-stu-id="afa17-249">The `src` function provides files that match the glob value(s).</span></span> <span data-ttu-id="afa17-250">`glob` Parametr jest `string` lub `array` określający plików do odczytu.</span><span class="sxs-lookup"><span data-stu-id="afa17-250">The `glob` parameter is a `string` or `array` that determines which files to read.</span></span> <span data-ttu-id="afa17-251">`options` Parametr zapewnia dodatkowe opcje pliku.</span><span class="sxs-lookup"><span data-stu-id="afa17-251">The `options` parameter provides additional file options.</span></span>|
|<span data-ttu-id="afa17-252">dest</span><span class="sxs-lookup"><span data-stu-id="afa17-252">dest</span></span>  |`gulp.dest(path[, options]) { }`|<span data-ttu-id="afa17-253">`dest` Funkcja określa lokalizację, do którego można zapisywać pliki.</span><span class="sxs-lookup"><span data-stu-id="afa17-253">The `dest` function defines a location to which files can be written.</span></span> <span data-ttu-id="afa17-254">`path` Parametru jest ciąg lub funkcja, która określa folder docelowy.</span><span class="sxs-lookup"><span data-stu-id="afa17-254">The `path` parameter is a string or function that determines the destination folder.</span></span> <span data-ttu-id="afa17-255">`options` Parametru jest obiektem, który określa opcje folderów danych wyjściowych.</span><span class="sxs-lookup"><span data-stu-id="afa17-255">The `options` parameter is an object that specifies output folder options.</span></span>|

<span data-ttu-id="afa17-256">Aby uzyskać dodatkowe informacje Gulp interfejsu API zobacz [Gulp Docs API](https://gulpjs.org/API.html).</span><span class="sxs-lookup"><span data-stu-id="afa17-256">For additional Gulp API reference information, see [Gulp Docs API](https://gulpjs.org/API.html).</span></span>

## <a name="gulp-recipes"></a><span data-ttu-id="afa17-257">Przepisy gulp</span><span class="sxs-lookup"><span data-stu-id="afa17-257">Gulp recipes</span></span>

<span data-ttu-id="afa17-258">Społeczność Gulp oferuje Gulp [przepisy](https://github.com/gulpjs/gulp/blob/master/docs/recipes/README.md).</span><span class="sxs-lookup"><span data-stu-id="afa17-258">The Gulp community provides Gulp [Recipes](https://github.com/gulpjs/gulp/blob/master/docs/recipes/README.md).</span></span> <span data-ttu-id="afa17-259">Te przepisami składają się z zadań Gulp obsługę typowych scenariuszy.</span><span class="sxs-lookup"><span data-stu-id="afa17-259">These recipes consist of Gulp tasks to address common scenarios.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="afa17-260">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="afa17-260">Additional resources</span></span>

* [<span data-ttu-id="afa17-261">Dokumentacja gulp</span><span class="sxs-lookup"><span data-stu-id="afa17-261">Gulp documentation</span></span>](https://github.com/gulpjs/gulp/blob/master/docs/README.md)
* [<span data-ttu-id="afa17-262">Tworzenie pakietów i minimalizowanie w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="afa17-262">Bundling and minification in ASP.NET Core</span></span>](bundling-and-minification.md)
* [<span data-ttu-id="afa17-263">Korzystanie z Grunt w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="afa17-263">Use Grunt in ASP.NET Core</span></span>](using-grunt.md)
