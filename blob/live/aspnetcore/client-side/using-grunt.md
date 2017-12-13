---
title: "Przy użyciu Grunt w platformy ASP.NET Core"
author: rick-anderson
description: 
keywords: Platformy ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 471112e9-2c33-454b-96fc-32916102ce73
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/using-grunt
ms.openlocfilehash: 8ae50514ce24c7f9e3bb1e347d5d860e1de43c5f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
# <a name="using-grunt-in-aspnet-core"></a><span data-ttu-id="04162-103">Przy użyciu Grunt w platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="04162-103">Using Grunt in ASP.NET Core</span></span> 

<span data-ttu-id="04162-104">Przez [ryżu Noel](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/)</span><span class="sxs-lookup"><span data-stu-id="04162-104">By [Noel Rice](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/)</span></span>

<span data-ttu-id="04162-105">Grunt jest modułu uruchamiającego zadania JavaScript, który zautomatyzuje minimalizację skryptu, kompilowanie kodu TypeScript narzędzia "w wierszu" jakości kodu, procesory wstępne CSS i niemal wszystkie powtarzających się kwestii, wymagającym czynności umożliwiających rozwój klienta.</span><span class="sxs-lookup"><span data-stu-id="04162-105">Grunt is a JavaScript task runner that automates script minification, TypeScript compilation, code quality "lint" tools, CSS pre-processors, and just about any repetitive chore that needs doing to support client development.</span></span> <span data-ttu-id="04162-106">Grunt jest w pełni obsługiwana w programie Visual Studio, że szablony projektów programu ASP.NET Gulp domyślnie (zobacz [przy użyciu system Gulp](using-gulp.md)).</span><span class="sxs-lookup"><span data-stu-id="04162-106">Grunt is fully supported in Visual Studio, though the ASP.NET project templates use Gulp by default (see [Using Gulp](using-gulp.md)).</span></span>

<span data-ttu-id="04162-107">W tym przykładzie używane pusty projekt platformy ASP.NET Core jako punktu początkowego pokazanie sposobu automatyzacji procesu kompilacji klienta od początku.</span><span class="sxs-lookup"><span data-stu-id="04162-107">This example uses an empty ASP.NET Core project as its starting point, to show how to automate the client build process from scratch.</span></span>

<span data-ttu-id="04162-108">Zakończono przykład oczyszcza katalog docelowy wdrożenia, łączy pliki JavaScript, sprawdza jakości kodu, pozwala zapisać zawartości pliku JavaScript i wdraża do katalogu głównego aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="04162-108">The finished example cleans the target deployment directory, combines JavaScript files, checks code quality, condenses JavaScript file content and deploys to the root of your web application.</span></span> <span data-ttu-id="04162-109">Firma Microsoft będzie używać następujących pakietów:</span><span class="sxs-lookup"><span data-stu-id="04162-109">We will use the following packages:</span></span>

* <span data-ttu-id="04162-110">**grunt**: pakiet modułu uruchamiającego zadania Grunt.</span><span class="sxs-lookup"><span data-stu-id="04162-110">**grunt**: The Grunt task runner package.</span></span>

* <span data-ttu-id="04162-111">**Wyczyść contrib grunt**: wtyczkę, która usuwa pliki lub katalogi.</span><span class="sxs-lookup"><span data-stu-id="04162-111">**grunt-contrib-clean**: A plugin that removes files or directories.</span></span>

* <span data-ttu-id="04162-112">**grunt-contrib-jshint**: dodatek, który przegląda jakości kodu JavaScript.</span><span class="sxs-lookup"><span data-stu-id="04162-112">**grunt-contrib-jshint**: A plugin that reviews JavaScript code quality.</span></span>

* <span data-ttu-id="04162-113">**concat — contrib-grunt**: wtyczkę, która łączy pliki w jednym pliku.</span><span class="sxs-lookup"><span data-stu-id="04162-113">**grunt-contrib-concat**: A plugin that joins files into a single file.</span></span>

* <span data-ttu-id="04162-114">**grunt contrib uglify**: wtyczkę, która minimalizuje JavaScript, aby zmniejszyć rozmiar.</span><span class="sxs-lookup"><span data-stu-id="04162-114">**grunt-contrib-uglify**: A plugin that minifies JavaScript to reduce size.</span></span>

* <span data-ttu-id="04162-115">**grunt-contrib czujki**: wtyczkę, która prowadzi obserwację operacje na plikach.</span><span class="sxs-lookup"><span data-stu-id="04162-115">**grunt-contrib-watch**: A plugin that watches file activity.</span></span>

## <a name="preparing-the-application"></a><span data-ttu-id="04162-116">Przygotowywanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="04162-116">Preparing the application</span></span>

<span data-ttu-id="04162-117">Aby rozpocząć, skonfiguruj nową aplikację sieci web pusty, a następnie dodaj pliki TypeScript przykład.</span><span class="sxs-lookup"><span data-stu-id="04162-117">To begin, set up a new empty web application and add TypeScript example files.</span></span> <span data-ttu-id="04162-118">Pliki typeScript automatycznie są kompilowane do języka JavaScript przy użyciu domyślnych ustawień programu Visual Studio i będą naszych materiałów raw do przetworzenia za pomocą Grunt.</span><span class="sxs-lookup"><span data-stu-id="04162-118">TypeScript files are automatically compiled into JavaScript using default Visual Studio settings and will be our raw material to process using Grunt.</span></span>

1.  <span data-ttu-id="04162-119">W programie Visual Studio Utwórz nową `ASP.NET Web Application`.</span><span class="sxs-lookup"><span data-stu-id="04162-119">In Visual Studio, create a new `ASP.NET Web Application`.</span></span>

2.  <span data-ttu-id="04162-120">W **nowy projekt ASP.NET** okno dialogowe, wybierz platformy ASP.NET Core **pusty** szablonu i kliknij przycisk OK.</span><span class="sxs-lookup"><span data-stu-id="04162-120">In the **New ASP.NET Project** dialog, select the ASP.NET Core **Empty** template and click the OK button.</span></span>

3.  <span data-ttu-id="04162-121">Sprawdź struktury projektu w Eksploratorze rozwiązań.</span><span class="sxs-lookup"><span data-stu-id="04162-121">In the Solution Explorer, review the project structure.</span></span> <span data-ttu-id="04162-122">`\src` Folder zawiera pusty `wwwroot` i `Dependencies` węzłów.</span><span class="sxs-lookup"><span data-stu-id="04162-122">The `\src` folder includes empty `wwwroot` and `Dependencies` nodes.</span></span>

    ![rozwiązanie pusty sieci web](using-grunt/_static/grunt-solution-explorer.png)

4.  <span data-ttu-id="04162-124">Dodaj nowy folder o nazwie `TypeScript` do katalogu projektu.</span><span class="sxs-lookup"><span data-stu-id="04162-124">Add a new folder named `TypeScript` to your project directory.</span></span>

5.  <span data-ttu-id="04162-125">Przed dodaniem wszystkie pliki, upewnijmy się, że program Visual Studio jest opcja "skompilować przy zapisie" dla plików TypeScript zaznaczone.</span><span class="sxs-lookup"><span data-stu-id="04162-125">Before adding any files, let’s make sure that Visual Studio has the option 'compile on save' for TypeScript files checked.</span></span> <span data-ttu-id="04162-126">*Narzędzia > Opcje > Edytor tekstu > Typescript > projektu*</span><span class="sxs-lookup"><span data-stu-id="04162-126">*Tools > Options > Text Editor > Typescript > Project*</span></span>

    ![Opcje ustawienia automatycznego compliation plików TypeScript](using-grunt/_static/typescript-options.png)

6.  <span data-ttu-id="04162-128">Kliknij prawym przyciskiem myszy `TypeScript` katalogu i zaznacz **Dodaj > Nowy element** z menu kontekstowego.</span><span class="sxs-lookup"><span data-stu-id="04162-128">Right-click the `TypeScript` directory and select **Add > New Item** from the context menu.</span></span> <span data-ttu-id="04162-129">Wybierz **plik JavaScript** elementu i nazwij plik *Tastes.ts* (Uwaga \*rozszerzenia TS).</span><span class="sxs-lookup"><span data-stu-id="04162-129">Select the **JavaScript file** item and name the file *Tastes.ts* (note the \*.ts extension).</span></span> <span data-ttu-id="04162-130">Skopiuj wiersz kodu TypeScript poniżej do pliku (po zapisaniu, nowy *Tastes.js* plik pojawi się ze źródłem JavaScript).</span><span class="sxs-lookup"><span data-stu-id="04162-130">Copy the line of TypeScript code below into the file (when you save, a new *Tastes.js* file will appear with the JavaScript source).</span></span>
    
    ```typescript
    enum Tastes { Sweet, Sour, Salty, Bitter }
    ```

7.  <span data-ttu-id="04162-131">Dodaj drugi plik do **TypeScript** katalogu i nadaj mu nazwę `Food.ts`.</span><span class="sxs-lookup"><span data-stu-id="04162-131">Add a second file to the **TypeScript** directory and name it `Food.ts`.</span></span> <span data-ttu-id="04162-132">Skopiuj poniższy kod do pliku.</span><span class="sxs-lookup"><span data-stu-id="04162-132">Copy the code below into the file.</span></span>

    ```typescript
    class Food {
      constructor(name: string, calories: number) {
        this._name = name;
        this._calories = calories;
      }
    
      private _name: string;
      get Name() {
        return this._name;
      }
    
      private _calories: number;
      get Calories() {
        return this._calories;
      }
    
      private _taste: Tastes;
      get Taste(): Tastes { return this._taste }
      set Taste(value: Tastes) {
        this._taste = value;
      }
    }
    ```

## <a name="configuring-npm"></a><span data-ttu-id="04162-133">Konfigurowanie programu NPM</span><span class="sxs-lookup"><span data-stu-id="04162-133">Configuring NPM</span></span>

<span data-ttu-id="04162-134">Skonfiguruj NPM, aby pobrać grunt i grunt zadania.</span><span class="sxs-lookup"><span data-stu-id="04162-134">Next, configure NPM to download grunt and grunt-tasks.</span></span>

1. <span data-ttu-id="04162-135">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj > Nowy element** z menu kontekstowego.</span><span class="sxs-lookup"><span data-stu-id="04162-135">In the Solution Explorer, right-click the project and select **Add > New Item** from the context menu.</span></span> <span data-ttu-id="04162-136">Wybierz **plik konfiguracji NPM** element, pozostaw nazwę domyślną *package.json*i kliknij przycisk **Dodaj** przycisku.</span><span class="sxs-lookup"><span data-stu-id="04162-136">Select the **NPM configuration file** item, leave the default name, *package.json*, and click the **Add** button.</span></span>

2. <span data-ttu-id="04162-137">W *package.json* pliku wewnątrz `devDependencies` obiektu nawiasów klamrowych, wprowadź "grunt".</span><span class="sxs-lookup"><span data-stu-id="04162-137">In the *package.json* file, inside the `devDependencies` object braces, enter "grunt".</span></span> <span data-ttu-id="04162-138">Wybierz `grunt` z funkcji Intellisense listy, a następnie naciśnij klawisz Enter.</span><span class="sxs-lookup"><span data-stu-id="04162-138">Select `grunt` from the Intellisense list and press the Enter key.</span></span> <span data-ttu-id="04162-139">Visual Studio oferta grunt nazwę pakietu i dodać dwukropkiem.</span><span class="sxs-lookup"><span data-stu-id="04162-139">Visual Studio will quote the grunt package name, and add a colon.</span></span> <span data-ttu-id="04162-140">Po prawej stronie dwukropek, wybierz najnowsza stabilna wersja pakietu z początku listy Intellisense (naciśnij klawisz `Ctrl-Space` Jeżeli nie ma Intellisense).</span><span class="sxs-lookup"><span data-stu-id="04162-140">To the right of the colon, select the latest stable version of the package from the top of the Intellisense list (press `Ctrl-Space` if Intellisense does not appear).</span></span>

    ![grun Intellisense](using-grunt/_static/devdependencies-grunt.png)
    
    > [!NOTE]
    > <span data-ttu-id="04162-142">Używa NPM [wersjonowania semantycznego](http://semver.org/) do organizowania zależności.</span><span class="sxs-lookup"><span data-stu-id="04162-142">NPM uses [semantic versioning](http://semver.org/) to organize dependencies.</span></span> <span data-ttu-id="04162-143">Wersjonowania semantycznego, znanej także jako programu SemVer identyfikuje pakiety ze schematu numerowania <major>.<minor>. <patch>. Przedstawiający kilka typowe opcje IntelliSense upraszcza wersjonowania semantycznego.</span><span class="sxs-lookup"><span data-stu-id="04162-143">Semantic versioning, also known as SemVer, identifies packages with the numbering scheme <major>.<minor>.<patch>. Intellisense simplifies semantic versioning by showing only a few common choices.</span></span> <span data-ttu-id="04162-144">Pierwszy element na liście Intellisense (0.4.5 w powyższym przykładzie) jest uznawany za najnowsza stabilna wersja pakietu.</span><span class="sxs-lookup"><span data-stu-id="04162-144">The top item in the Intellisense list (0.4.5 in the example above) is considered the latest stable version of the package.</span></span> <span data-ttu-id="04162-145">Symbol daszek (^) najnowszą wersją główną i tyldy (~) najnowszą wersją pomocniczą.</span><span class="sxs-lookup"><span data-stu-id="04162-145">The caret (^) symbol matches the most recent major version and the tilde (~) matches the most recent minor version.</span></span> <span data-ttu-id="04162-146">Zobacz [odwołanie analizatora wersji programu semver NPM](https://www.npmjs.com/package/semver) jako przewodnik dotyczący expressivity pełnego, który zapewnia programu SemVer.</span><span class="sxs-lookup"><span data-stu-id="04162-146">See the [NPM semver version parser reference](https://www.npmjs.com/package/semver) as a guide to the full expressivity that SemVer provides.</span></span>

3. <span data-ttu-id="04162-147">Dodaj więcej zależności, aby załadować grunt-contrib -\* pakietami *czystą*, *jshint*, *concat*, *uglify*i *czujki* jak pokazano w poniższym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="04162-147">Add more dependencies to load grunt-contrib-\* packages for *clean*, *jshint*, *concat*, *uglify*, and *watch* as shown in the example below.</span></span> <span data-ttu-id="04162-148">Wersje musi być zgodny z przykładu.</span><span class="sxs-lookup"><span data-stu-id="04162-148">The versions do not need to match the example.</span></span>

    ```json
    "devDependencies": {
      "grunt": "0.4.5",
      "grunt-contrib-clean": "0.6.0",
      "grunt-contrib-jshint": "0.11.0",
      "grunt-contrib-concat": "0.5.1",
      "grunt-contrib-uglify": "0.8.0",
      "grunt-contrib-watch": "0.6.1"
    }
    ```

4. <span data-ttu-id="04162-149">Zapisz *package.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="04162-149">Save the *package.json* file.</span></span>

<span data-ttu-id="04162-150">Pakiety dla każdego elementu devDependencies pobierze oraz wszystkie pliki, które wymaga każdego pakietu.</span><span class="sxs-lookup"><span data-stu-id="04162-150">The packages for each devDependencies item will download, along with any files that each package requires.</span></span> <span data-ttu-id="04162-151">Można znaleźć plików pakietu w `node_modules` katalogu przez włączenie **Pokaż wszystkie pliki** przycisk w Eksploratorze rozwiązań.</span><span class="sxs-lookup"><span data-stu-id="04162-151">You can find the package files in the `node_modules` directory by enabling the **Show All Files** button in the Solution Explorer.</span></span>

![grunt node_modules](using-grunt/_static/node-modules.png)

> [!NOTE]
> <span data-ttu-id="04162-153">Jeśli zachodzi potrzeba, można ręcznie przywrócić zależności w Eksploratorze rozwiązań, klikając prawym przyciskiem myszy `Dependencies\NPM` i wybierając **Przywracanie pakietów** opcji menu.</span><span class="sxs-lookup"><span data-stu-id="04162-153">If you need to, you can manually restore dependencies in Solution Explorer by right-clicking on `Dependencies\NPM` and selecting the **Restore Packages** menu option.</span></span>

![Przywracanie pakietów](using-grunt/_static/restore-packages.png)

## <a name="configuring-grunt"></a><span data-ttu-id="04162-155">Konfigurowanie Grunt</span><span class="sxs-lookup"><span data-stu-id="04162-155">Configuring Grunt</span></span>

<span data-ttu-id="04162-156">Grunt jest konfigurowana przy użyciu manifestu o nazwie *Gruntfile.js* który definiuje, ładuje i rejestruje zadania, które można uruchomić ręcznie lub skonfigurowany do uruchamiania automatycznie na podstawie zdarzeń w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="04162-156">Grunt is configured using a manifest named *Gruntfile.js* that defines, loads and registers tasks that can be run manually or configured to run automatically based on events in Visual Studio.</span></span>

1.  <span data-ttu-id="04162-157">Kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj > Nowy element**.</span><span class="sxs-lookup"><span data-stu-id="04162-157">Right-click the project and select **Add > New Item**.</span></span> <span data-ttu-id="04162-158">Wybierz **pliku konfiguracji Grunt** opcji, pozostaw nazwę domyślną *Gruntfile.js*i kliknij przycisk **Dodaj** przycisku.</span><span class="sxs-lookup"><span data-stu-id="04162-158">Select the **Grunt Configuration file** option, leave the default name, *Gruntfile.js*, and click the **Add** button.</span></span>

    <span data-ttu-id="04162-159">Początkowa kod zawiera definicję modułu i `grunt.initConfig()` metody.</span><span class="sxs-lookup"><span data-stu-id="04162-159">The initial code includes a module definition and the `grunt.initConfig()` method.</span></span> <span data-ttu-id="04162-160">`initConfig()` Służy do ustawiania opcji dla każdego pakietu, a w pozostałej części modułu zostanie obciążenia i zarejestrować zadań.</span><span class="sxs-lookup"><span data-stu-id="04162-160">The `initConfig()` is used to set options for each package, and the remainder of the module will load and register tasks.</span></span>
    
    ```javascript
    module.exports = function (grunt) {
      grunt.initConfig({
      });
    };
    ```

2. <span data-ttu-id="04162-161">Wewnątrz `initConfig()` metody, Dodaj opcje `clean` zadań, jak pokazano w przykładzie *Gruntfile.js* poniżej.</span><span class="sxs-lookup"><span data-stu-id="04162-161">Inside the `initConfig()` method, add options for the `clean` task as shown in the example *Gruntfile.js* below.</span></span> <span data-ttu-id="04162-162">Wyczyść zadań akceptuje tablica ciągów katalogu.</span><span class="sxs-lookup"><span data-stu-id="04162-162">The clean task accepts an array of directory strings.</span></span> <span data-ttu-id="04162-163">To zadanie usuwa z wwwroot/lib pliki i usuwa cały/tymczasowego katalogu.</span><span class="sxs-lookup"><span data-stu-id="04162-163">This task removes files from wwwroot/lib and removes the entire /temp directory.</span></span>

    ```javascript
    module.exports = function (grunt) {
      grunt.initConfig({
        clean: ["wwwroot/lib/*", "temp/"],
      });
    };
    ```

3. <span data-ttu-id="04162-164">Poniżej metody initConfig(), dodaj wywołanie do `grunt.loadNpmTasks()`.</span><span class="sxs-lookup"><span data-stu-id="04162-164">Below the initConfig() method, add a call to `grunt.loadNpmTasks()`.</span></span> <span data-ttu-id="04162-165">Spowoduje to zadanie do uruchomienia z programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="04162-165">This will make the task runnable from Visual Studio.</span></span>

    ```javascript
    grunt.loadNpmTasks("grunt-contrib-clean");
    ```

4. <span data-ttu-id="04162-166">Zapisz *Gruntfile.js*.</span><span class="sxs-lookup"><span data-stu-id="04162-166">Save *Gruntfile.js*.</span></span> <span data-ttu-id="04162-167">Plik powinien wyglądać jak na poniższym zrzucie ekranu.</span><span class="sxs-lookup"><span data-stu-id="04162-167">The file should look something like the screenshot below.</span></span>

    ![gruntfile początkowej](using-grunt/_static/gruntfile-js-initial.png)

5. <span data-ttu-id="04162-169">Kliknij prawym przyciskiem myszy *Gruntfile.js* i wybierz **Eksploratora modułu uruchamiającego zadania** z menu kontekstowego.</span><span class="sxs-lookup"><span data-stu-id="04162-169">Right-click *Gruntfile.js* and select **Task Runner Explorer** from the context menu.</span></span> <span data-ttu-id="04162-170">Zostanie otwarte okno Eksploratora modułu uruchamiającego zadania.</span><span class="sxs-lookup"><span data-stu-id="04162-170">The Task Runner Explorer window will open.</span></span>

    ![menu Eksploratora modułu uruchamiającego zadania](using-grunt/_static/task-runner-explorer-menu.png)

6. <span data-ttu-id="04162-172">Sprawdź, czy `clean` przedstawiono w obszarze **zadania** w Eksploratora modułu uruchamiającego zadania.</span><span class="sxs-lookup"><span data-stu-id="04162-172">Verify that `clean` shows under **Tasks** in the Task Runner Explorer.</span></span>

    ![Lista zadań Eksploratora modułu uruchamiającego zadania](using-grunt/_static/task-runner-explorer-tasks.png)

7. <span data-ttu-id="04162-174">Kliknij prawym przyciskiem myszy czystą zadań i wybierz **Uruchom** z menu kontekstowego.</span><span class="sxs-lookup"><span data-stu-id="04162-174">Right-click the clean task and select **Run** from the context menu.</span></span> <span data-ttu-id="04162-175">Okno polecenia wyświetla postęp zadania.</span><span class="sxs-lookup"><span data-stu-id="04162-175">A command window displays progress of the task.</span></span>

    ![Uruchom Eksploratora modułu uruchamiającego zadania, czystej zadania](using-grunt/_static/task-runner-explorer-run-clean.png)
    
    > [!NOTE]
    > <span data-ttu-id="04162-177">Nie ma żadnych plików lub katalogów, aby wyczyścić jeszcze.</span><span class="sxs-lookup"><span data-stu-id="04162-177">There are no files or directories to clean yet.</span></span> <span data-ttu-id="04162-178">Jeśli chcesz można ręcznie utworzyć je w Eksploratorze rozwiązań, a następnie uruchom zadanie czystą jako test.</span><span class="sxs-lookup"><span data-stu-id="04162-178">If you like, you can manually create them in the Solution Explorer and then run the clean task as a test.</span></span>
    
8. <span data-ttu-id="04162-179">W metodzie initConfig(), Dodaj wpis dla `concat` przy użyciu kodu poniżej.</span><span class="sxs-lookup"><span data-stu-id="04162-179">In the initConfig() method, add an entry for `concat` using the code below.</span></span>

    <span data-ttu-id="04162-180">`src` Tablicy właściwości listy plików do łączenia, w kolejności, że powinny być połączone.</span><span class="sxs-lookup"><span data-stu-id="04162-180">The `src` property array lists files to combine, in the order that they should be combined.</span></span> <span data-ttu-id="04162-181">`dest` Właściwość przypisuje ścieżka do połączonego pliku, który jest generowany.</span><span class="sxs-lookup"><span data-stu-id="04162-181">The `dest` property assigns the path to the combined file that is produced.</span></span>

    ```javascript
    concat: {
      all: {
        src: ['TypeScript/Tastes.js', 'TypeScript/Food.js'],
        dest: 'temp/combined.js'
      }
    },
    ```
    
    > [!NOTE]
    > <span data-ttu-id="04162-182">`all` Właściwości w powyższym kodzie to nazwa obiektu docelowego.</span><span class="sxs-lookup"><span data-stu-id="04162-182">The `all` property in the code above is the name of a target.</span></span> <span data-ttu-id="04162-183">Obiekty docelowe są używane w niektórych zadań Grunt umożliwiają wielu środowiska kompilacji.</span><span class="sxs-lookup"><span data-stu-id="04162-183">Targets are used in some Grunt tasks to allow multiple build environments.</span></span> <span data-ttu-id="04162-184">Można wyświetlać wbudowanych elementów docelowych przy użyciu funkcji Intellisense lub przypisać własne.</span><span class="sxs-lookup"><span data-stu-id="04162-184">You can view the built-in targets using Intellisense or assign your own.</span></span>
    
9. <span data-ttu-id="04162-185">Dodaj `jshint` zadań przy użyciu kodu poniżej.</span><span class="sxs-lookup"><span data-stu-id="04162-185">Add the `jshint` task using the code below.</span></span>

    <span data-ttu-id="04162-186">Narzędzie jakości kodu jshint jest wykonywane na każdy plik JavaScript znalezione w katalogu tymczasowym.</span><span class="sxs-lookup"><span data-stu-id="04162-186">The jshint code-quality utility is run against every JavaScript file found in the temp directory.</span></span>
    
    ```javascript
    jshint: {
      files: ['temp/*.js'],
      options: {
        '-W069': false,
      }
    },
    ```

    > [!NOTE]
    > <span data-ttu-id="04162-187">Opcja "-W069" jest błąd generowany przez jshint podczas JavaScript używa nawiasów składni, aby przypisać właściwości zamiast kropkowego, tj. `Tastes["Sweet"]` zamiast `Tastes.Sweet`.</span><span class="sxs-lookup"><span data-stu-id="04162-187">The option "-W069" is an error produced by jshint when JavaScript uses bracket syntax to assign a property instead of dot notation, i.e. `Tastes["Sweet"]` instead of `Tastes.Sweet`.</span></span> <span data-ttu-id="04162-188">Opcja powoduje wyłączenie ostrzeżenia, aby umożliwić resztę procesu, aby kontynuować.</span><span class="sxs-lookup"><span data-stu-id="04162-188">The option turns off the warning to allow the rest of the process to continue.</span></span>

10.  <span data-ttu-id="04162-189">Dodaj `uglify` zadań przy użyciu kodu poniżej.</span><span class="sxs-lookup"><span data-stu-id="04162-189">Add the `uglify` task using the code below.</span></span>

    <span data-ttu-id="04162-190">Zadanie minimalizuje *combined.js* plik znaleziono w katalogu tymczasowym i tworzy plik wyników w wwwroot/lib zgodnie ze standardową konwencją nazewnictwa  *\<nazwę pliku\>. min.js*.</span><span class="sxs-lookup"><span data-stu-id="04162-190">The task minifies the *combined.js* file found in the temp directory and creates the result file in wwwroot/lib following the standard naming convention *\<file name\>.min.js*.</span></span>
    
    ```javascript
    uglify: {
      all: {
        src: ['temp/combined.js'],
        dest: 'wwwroot/lib/combined.min.js'
      }
    },
    ```

11. <span data-ttu-id="04162-191">W obszarze grunt.loadNpmTasks() wywołania, który ładuje czyszczenia contrib grunt obejmują to samo wywołanie dla jshint, concat i uglify przy użyciu kodu poniżej.</span><span class="sxs-lookup"><span data-stu-id="04162-191">Under the call grunt.loadNpmTasks() that loads grunt-contrib-clean, include the same call for jshint, concat and uglify using the code below.</span></span>
    
    ```javascript
    grunt.loadNpmTasks('grunt-contrib-jshint');
    grunt.loadNpmTasks('grunt-contrib-concat');
    grunt.loadNpmTasks('grunt-contrib-uglify');
    ```

12. <span data-ttu-id="04162-192">Zapisz *Gruntfile.js*.</span><span class="sxs-lookup"><span data-stu-id="04162-192">Save *Gruntfile.js*.</span></span> <span data-ttu-id="04162-193">Plik powinien wyglądać podobnie jak w poniższym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="04162-193">The file should look something like the example below.</span></span>

    ![przykład pliku grunt ukończone](using-grunt/_static/gruntfile-js-complete.png)

13. <span data-ttu-id="04162-195">Należy zauważyć, że lista zadań Eksploratora modułu uruchamiającego zadania zawiera `clean`, `concat`, `jshint` i `uglify` zadania.</span><span class="sxs-lookup"><span data-stu-id="04162-195">Notice that the Task Runner Explorer Tasks list includes `clean`, `concat`, `jshint` and `uglify` tasks.</span></span> <span data-ttu-id="04162-196">Wszystkie zadania są uruchamiane w kolejności i obserwować wyniki w Eksploratorze rozwiązań.</span><span class="sxs-lookup"><span data-stu-id="04162-196">Run each task in order and observe the results in Solution Explorer.</span></span> <span data-ttu-id="04162-197">Wszystkie zadania są uruchamiane bez błędów.</span><span class="sxs-lookup"><span data-stu-id="04162-197">Each task should run without errors.</span></span>
    
    ![Uruchom każde zadanie Eksploratora modułu uruchamiającego zadania](using-grunt/_static/task-runner-explorer-run-each-task.png)
    
    <span data-ttu-id="04162-199">Tworzy nowe zadanie concat *combined.js* pliku i umieszcza je w katalogu tymczasowego.</span><span class="sxs-lookup"><span data-stu-id="04162-199">The concat task creates a new *combined.js* file and places it into the temp directory.</span></span> <span data-ttu-id="04162-200">Zadanie jshint po prostu działa i nie tworzy dane wyjściowe.</span><span class="sxs-lookup"><span data-stu-id="04162-200">The jshint task simply runs and doesn’t produce output.</span></span> <span data-ttu-id="04162-201">Tworzy nowe zadanie uglify *combined.min.js* pliku i umieszcza je w wwwroot/lib.</span><span class="sxs-lookup"><span data-stu-id="04162-201">The uglify task creates a new *combined.min.js* file and places it into wwwroot/lib.</span></span> <span data-ttu-id="04162-202">Po zakończeniu rozwiązanie powinien wyglądać jak na poniższym zrzucie ekranu:</span><span class="sxs-lookup"><span data-stu-id="04162-202">On completion, the solution should look something like the screenshot below:</span></span>
    
    ![Eksplorator rozwiązań po wszystkich zadań.](using-grunt/_static/solution-explorer-after-all-tasks.png)
    
    > [!NOTE]
    > <span data-ttu-id="04162-204">Aby uzyskać więcej informacji na temat opcji dla każdego pakietu, odwiedź stronę [https://www.npmjs.com/](https://www.npmjs.com/) i wyszukiwania nazwę pakietu w polu wyszukiwania na stronie głównej.</span><span class="sxs-lookup"><span data-stu-id="04162-204">For more information on the options for each package, visit [https://www.npmjs.com/](https://www.npmjs.com/) and lookup the package name in the search box on the main page.</span></span> <span data-ttu-id="04162-205">Na przykład można wyszukiwać czyszczenia contrib grunt pakiet do pobrania łącze dokumentacji, który objaśnia, wszystkie jego parametrów.</span><span class="sxs-lookup"><span data-stu-id="04162-205">For example, you can look up the grunt-contrib-clean package to get a documentation link that explains all of its parameters.</span></span>

### <a name="all-together-now"></a><span data-ttu-id="04162-206">Wszystko w jednym miejscu</span><span class="sxs-lookup"><span data-stu-id="04162-206">All together now</span></span>

<span data-ttu-id="04162-207">Użyj Grunt `registerTask()` metodę, aby uruchomić sekwencję zadań w określonej kolejności.</span><span class="sxs-lookup"><span data-stu-id="04162-207">Use the Grunt `registerTask()` method to run a series of tasks in a particular sequence.</span></span> <span data-ttu-id="04162-208">Na przykład, aby uruchomić przykład, wykonując powyższe kroki w kolejności czystą -> concat -> jshint -> uglify, Dodaj poniższy kod do modułu.</span><span class="sxs-lookup"><span data-stu-id="04162-208">For example, to run the example steps above in the order clean -> concat -> jshint -> uglify, add the code below to the module.</span></span> <span data-ttu-id="04162-209">Kod należy dodać do tego samego poziomu wywołania metody loadNpmTasks() poza initConfig.</span><span class="sxs-lookup"><span data-stu-id="04162-209">The code should be added to the same level as the loadNpmTasks() calls, outside initConfig.</span></span>

```javascript
grunt.registerTask("all", ['clean', 'concat', 'jshint', 'uglify']);
```

<span data-ttu-id="04162-210">Nowe zadania zostaną wyświetlone w Eksploratora modułu uruchamiającego zadania w obszarze zadania Alias.</span><span class="sxs-lookup"><span data-stu-id="04162-210">The new task shows up in Task Runner Explorer under Alias Tasks.</span></span> <span data-ttu-id="04162-211">Można kliknąć prawym przyciskiem myszy i uruchomić tak samo jak inne zadania.</span><span class="sxs-lookup"><span data-stu-id="04162-211">You can right-click and run it just as you would other tasks.</span></span> <span data-ttu-id="04162-212">`all` Zadanie zostanie uruchomione `clean`, `concat`, `jshint` i `uglify`, w kolejności.</span><span class="sxs-lookup"><span data-stu-id="04162-212">The `all` task will run `clean`, `concat`, `jshint` and `uglify`, in order.</span></span>

![alias grunt zadania](using-grunt/_static/alias-tasks.png)

## <a name="watching-for-changes"></a><span data-ttu-id="04162-214">Monitorowanie zmian</span><span class="sxs-lookup"><span data-stu-id="04162-214">Watching for changes</span></span>

<span data-ttu-id="04162-215">A `watch` zadań przechowuje list plików i katalogów.</span><span class="sxs-lookup"><span data-stu-id="04162-215">A `watch` task keeps an eye on files and directories.</span></span> <span data-ttu-id="04162-216">Obejrzyj automatycznie wyzwala zadania, jeśli wykryje zmiany.</span><span class="sxs-lookup"><span data-stu-id="04162-216">The watch triggers tasks automatically if it detects changes.</span></span> <span data-ttu-id="04162-217">Dodaj poniższy kod do initConfig Aby obejrzeć wprowadzenie zmian w \*js plików w katalogu TypeScript.</span><span class="sxs-lookup"><span data-stu-id="04162-217">Add the code below to initConfig to watch for changes to \*.js files in the TypeScript directory.</span></span> <span data-ttu-id="04162-218">Jeśli plik JavaScript została zmieniona, `watch` uruchomi `all` zadań.</span><span class="sxs-lookup"><span data-stu-id="04162-218">If a JavaScript file is changed, `watch` will run the `all` task.</span></span>

```javascript
watch: {
  files: ["TypeScript/*.js"],
  tasks: ["all"]
}
```

<span data-ttu-id="04162-219">Dodaj wywołanie do `loadNpmTasks()` pokazanie `watch` zadanie w Eksploratora modułu uruchamiającego zadania.</span><span class="sxs-lookup"><span data-stu-id="04162-219">Add a call to `loadNpmTasks()` to show the `watch` task in Task Runner Explorer.</span></span>

```javascript
grunt.loadNpmTasks('grunt-contrib-watch');
```

<span data-ttu-id="04162-220">Kliknij prawym przyciskiem myszy zadanie czujki w Eksploratora modułu uruchamiającego zadania i wybierz polecenie Uruchom z menu kontekstowego.</span><span class="sxs-lookup"><span data-stu-id="04162-220">Right-click the watch task in Task Runner Explorer and select Run from the context menu.</span></span> <span data-ttu-id="04162-221">W oknie polecenia, pokazujący zadanie czujki uruchomione będą wyświetlane "Oczekiwanie..."</span><span class="sxs-lookup"><span data-stu-id="04162-221">The command window that shows the watch task running will display a "Waiting…"</span></span> <span data-ttu-id="04162-222">Komunikat.</span><span class="sxs-lookup"><span data-stu-id="04162-222">message.</span></span> <span data-ttu-id="04162-223">Otwórz jeden z plików TypeScript, Dodaj miejsce, a następnie zapisz plik.</span><span class="sxs-lookup"><span data-stu-id="04162-223">Open one of the TypeScript files, add a space, and then save the file.</span></span> <span data-ttu-id="04162-224">To zadanie czujki wyzwolą i wyzwalanie innych zadań są uruchamiane w kolejności.</span><span class="sxs-lookup"><span data-stu-id="04162-224">This will trigger the watch task and trigger the other tasks to run in order.</span></span> <span data-ttu-id="04162-225">Poniższy zrzut ekranu przedstawia uruchamianie przykładowych.</span><span class="sxs-lookup"><span data-stu-id="04162-225">The screenshot below shows a sample run.</span></span>

![dane wyjściowe zadania uruchamiania](using-grunt/_static/watch-running.png)

## <a name="binding-to-visual-studio-events"></a><span data-ttu-id="04162-227">Powiązanie z zdarzeń programu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="04162-227">Binding to Visual Studio events</span></span>

<span data-ttu-id="04162-228">Chyba że chcesz ręcznie uruchomić zadania za każdym razem, gdy działa w programie Visual Studio, możesz powiązać zadań **przed kompilacji**, **po kompilacji**, **wyczyść**, i  **Otwórz projekt** zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="04162-228">Unless you want to manually start your tasks every time you work in Visual Studio, you can bind tasks to **Before Build**, **After Build**, **Clean**, and **Project Open** events.</span></span>

<span data-ttu-id="04162-229">Teraz powiązać `watch` , aby został uruchomiony za każdym razem, gdy zostanie otwarty program Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="04162-229">Let’s bind `watch` so that it runs every time Visual Studio opens.</span></span> <span data-ttu-id="04162-230">W Eksploratora modułu uruchamiającego zadania, kliknij prawym przyciskiem myszy zadanie czujki i wybierz **powiązania > Otwórz projekt** z menu kontekstowego.</span><span class="sxs-lookup"><span data-stu-id="04162-230">In Task Runner Explorer, right-click the watch task and select **Bindings > Project Open** from the context menu.</span></span>

![Powiąż zadania do otwarcia projektu](using-grunt/_static/bindings-project-open.png)

<span data-ttu-id="04162-232">Zwolnienie i ponowne załadowanie projektu.</span><span class="sxs-lookup"><span data-stu-id="04162-232">Unload and reload the project.</span></span> <span data-ttu-id="04162-233">Podczas ponownie ładowania projektu czujki zadanie zostanie uruchomione automatycznie.</span><span class="sxs-lookup"><span data-stu-id="04162-233">When the project loads again, the watch task will start running automatically.</span></span>

## <a name="summary"></a><span data-ttu-id="04162-234">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="04162-234">Summary</span></span>

<span data-ttu-id="04162-235">Grunt jest modułu uruchamiającego zadania zaawansowanego, który może służyć do automatyzowania większość zadań kompilacji klienta.</span><span class="sxs-lookup"><span data-stu-id="04162-235">Grunt is a powerful task runner that can be used to automate most client-build tasks.</span></span> <span data-ttu-id="04162-236">Grunt wykorzystuje NPM w celu dostarczania pakietów i funkcje narzędzi integracji z programem Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="04162-236">Grunt leverages NPM to deliver its packages, and features tooling integration with Visual Studio.</span></span> <span data-ttu-id="04162-237">Eksploratora modułu uruchamiającego zadania programu Visual Studio wykrywa zmiany w plikach konfiguracji i oferuje wygodny interfejs do uruchamiania zadań, Wyświetl uruchomione zadania i powiąż zadania ze zdarzeniami w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="04162-237">Visual Studio's Task Runner Explorer detects changes to configuration files and provides a convenient interface to run tasks, view running tasks, and bind tasks to Visual Studio events.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="04162-238">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="04162-238">Additional resources</span></span>

   * [<span data-ttu-id="04162-239">Przy użyciu Gulp</span><span class="sxs-lookup"><span data-stu-id="04162-239">Using Gulp</span></span>](using-gulp.md)
