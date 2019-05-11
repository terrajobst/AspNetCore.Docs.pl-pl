---
title: Korzystanie z Grunt w programie ASP.NET Core
author: rick-anderson
description: ''
ms.author: riande
ms.date: 05/10/2019
uid: client-side/using-grunt
ms.openlocfilehash: 73b7704726db9d93588dddd3f3b05a23fb3425b3
ms.sourcegitcommit: ffe3ed7921ec6c7c70abaac1d10703ec9a43374c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/11/2019
ms.locfileid: "65535938"
---
# <a name="use-grunt-in-aspnet-core"></a><span data-ttu-id="a98f1-102">Korzystanie z Grunt w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a98f1-102">Use Grunt in ASP.NET Core</span></span>

<span data-ttu-id="a98f1-103">Przez [ryżu Noel](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/)</span><span class="sxs-lookup"><span data-stu-id="a98f1-103">By [Noel Rice](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/)</span></span>

<span data-ttu-id="a98f1-104">Grunt jest modułu uruchamiającego zadania JavaScript, który automatyzuje minimalizację skryptu, kompilacji TypeScript, narzędzia "lint" jakość kodu, procesory wstępne CSS i niemal dowolnym powtarzających się kwestii, wymagającym, wykonując obsługi opracowywania aplikacji klienta.</span><span class="sxs-lookup"><span data-stu-id="a98f1-104">Grunt is a JavaScript task runner that automates script minification, TypeScript compilation, code quality "lint" tools, CSS pre-processors, and just about any repetitive chore that needs doing to support client development.</span></span> <span data-ttu-id="a98f1-105">Grunt jest w pełni obsługiwany w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a98f1-105">Grunt is fully supported in Visual Studio.</span></span>

<span data-ttu-id="a98f1-106">W tym przykładzie używa pusty projekt platformy ASP.NET Core jako punktu wyjścia laboratorium, aby pokazać, jak zautomatyzować proces kompilacji klienta od podstaw.</span><span class="sxs-lookup"><span data-stu-id="a98f1-106">This example uses an empty ASP.NET Core project as its starting point, to show how to automate the client build process from scratch.</span></span>

<span data-ttu-id="a98f1-107">Zakończono przykład oczyszcza katalog docelowy wdrażania, łączy pliki JavaScript, sprawdza, czy jakość kodu, zmniejsza objętość zawartość pliku JavaScript i wdraża w katalogu głównym aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="a98f1-107">The finished example cleans the target deployment directory, combines JavaScript files, checks code quality, condenses JavaScript file content and deploys to the root of your web application.</span></span> <span data-ttu-id="a98f1-108">Firma Microsoft użyje następujących pakietów:</span><span class="sxs-lookup"><span data-stu-id="a98f1-108">We will use the following packages:</span></span>

* <span data-ttu-id="a98f1-109">**grunt**: Pakiet modułu uruchamiającego zadania Grunt.</span><span class="sxs-lookup"><span data-stu-id="a98f1-109">**grunt**: The Grunt task runner package.</span></span>

* <span data-ttu-id="a98f1-110">**grunt-contrib-clean**: Dodatek, który usuwa pliki lub katalogi.</span><span class="sxs-lookup"><span data-stu-id="a98f1-110">**grunt-contrib-clean**: A plugin that removes files or directories.</span></span>

* <span data-ttu-id="a98f1-111">**grunt-contrib-jshint**: Dodatek, który monitoruje jakość kodu JavaScript.</span><span class="sxs-lookup"><span data-stu-id="a98f1-111">**grunt-contrib-jshint**: A plugin that reviews JavaScript code quality.</span></span>

* <span data-ttu-id="a98f1-112">**concat — contrib-grunt**: Dodatek, który łączy pliki w jednym pliku.</span><span class="sxs-lookup"><span data-stu-id="a98f1-112">**grunt-contrib-concat**: A plugin that joins files into a single file.</span></span>

* <span data-ttu-id="a98f1-113">**grunt contrib uglify**: Dodatek, który minimalizuje język JavaScript, aby zmniejszyć rozmiar.</span><span class="sxs-lookup"><span data-stu-id="a98f1-113">**grunt-contrib-uglify**: A plugin that minifies JavaScript to reduce size.</span></span>

* <span data-ttu-id="a98f1-114">**grunt-contrib-watch**: Dodatek, który obserwuje działań dotyczących plików.</span><span class="sxs-lookup"><span data-stu-id="a98f1-114">**grunt-contrib-watch**: A plugin that watches file activity.</span></span>

## <a name="preparing-the-application"></a><span data-ttu-id="a98f1-115">Przygotowywanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="a98f1-115">Preparing the application</span></span>

<span data-ttu-id="a98f1-116">Aby rozpocząć, skonfiguruj nową pustą aplikację i dodać pliki przykładowe TypeScript.</span><span class="sxs-lookup"><span data-stu-id="a98f1-116">To begin, set up a new empty web application and add TypeScript example files.</span></span> <span data-ttu-id="a98f1-117">Pliki TypeScript automatycznie są kompilowane do kodu JavaScript przy użyciu domyślnych ustawień programu Visual Studio i będzie naszym surowce do przetwarzania, korzystanie z Grunt.</span><span class="sxs-lookup"><span data-stu-id="a98f1-117">TypeScript files are automatically compiled into JavaScript using default Visual Studio settings and will be our raw material to process using Grunt.</span></span>

1. <span data-ttu-id="a98f1-118">W programie Visual Studio Utwórz nowy `ASP.NET Web Application`.</span><span class="sxs-lookup"><span data-stu-id="a98f1-118">In Visual Studio, create a new `ASP.NET Web Application`.</span></span>

2. <span data-ttu-id="a98f1-119">W **nowy projekt ASP.NET** okno dialogowe, wybierz pozycję ASP.NET Core **pusty** szablon i kliknij przycisk OK.</span><span class="sxs-lookup"><span data-stu-id="a98f1-119">In the **New ASP.NET Project** dialog, select the ASP.NET Core **Empty** template and click the OK button.</span></span>

3. <span data-ttu-id="a98f1-120">Sprawdź struktury projektu w Eksploratorze rozwiązań.</span><span class="sxs-lookup"><span data-stu-id="a98f1-120">In the Solution Explorer, review the project structure.</span></span> <span data-ttu-id="a98f1-121">`\src` Folder zawiera pusty `wwwroot` i `Dependencies` węzłów.</span><span class="sxs-lookup"><span data-stu-id="a98f1-121">The `\src` folder includes empty `wwwroot` and `Dependencies` nodes.</span></span>

    ![rozwiązanie pusty sieci web](using-grunt/_static/grunt-solution-explorer.png)

4. <span data-ttu-id="a98f1-123">Dodaj nowy folder o nazwie `TypeScript` do katalogu projektu.</span><span class="sxs-lookup"><span data-stu-id="a98f1-123">Add a new folder named `TypeScript` to your project directory.</span></span>

5. <span data-ttu-id="a98f1-124">Przed dodaniem wszystkie pliki, upewnij się, że program Visual Studio oferuje opcję "Kompiluj przy zapisywaniu" dla plików TypeScript zaznaczone.</span><span class="sxs-lookup"><span data-stu-id="a98f1-124">Before adding any files, make sure that Visual Studio has the option 'compile on save' for TypeScript files checked.</span></span> <span data-ttu-id="a98f1-125">Przejdź do **narzędzia** > **opcje** > **edytora tekstów** > **Typescript**  >  **Projektu**:</span><span class="sxs-lookup"><span data-stu-id="a98f1-125">Navigate to **Tools** > **Options** > **Text Editor** > **Typescript** > **Project**:</span></span>

    ![Opcje ustawienia automatycznej kompilacji plików TypeScript](using-grunt/_static/typescript-options.png)

6. <span data-ttu-id="a98f1-127">Kliknij prawym przyciskiem myszy `TypeScript` katalogu i zaznacz **Dodaj > Nowy element** z menu kontekstowego.</span><span class="sxs-lookup"><span data-stu-id="a98f1-127">Right-click the `TypeScript` directory and select **Add > New Item** from the context menu.</span></span> <span data-ttu-id="a98f1-128">Wybierz **plik JavaScript** elementu i nazwij plik *Tastes.ts* (Uwaga \*rozszerzenia TS).</span><span class="sxs-lookup"><span data-stu-id="a98f1-128">Select the **JavaScript file** item and name the file *Tastes.ts* (note the \*.ts extension).</span></span> <span data-ttu-id="a98f1-129">Skopiuj wiersz kodu TypeScript poniżej do pliku (podczas zapisywania, nowy *Tastes.js* plik pojawi się ze źródłem JavaScript).</span><span class="sxs-lookup"><span data-stu-id="a98f1-129">Copy the line of TypeScript code below into the file (when you save, a new *Tastes.js* file will appear with the JavaScript source).</span></span>

    ```typescript
    enum Tastes { Sweet, Sour, Salty, Bitter }
    ```

7. <span data-ttu-id="a98f1-130">Dodaj drugi plik do **TypeScript** katalogu i nadaj mu nazwę `Food.ts`.</span><span class="sxs-lookup"><span data-stu-id="a98f1-130">Add a second file to the **TypeScript** directory and name it `Food.ts`.</span></span> <span data-ttu-id="a98f1-131">Skopiuj poniższy kod do pliku.</span><span class="sxs-lookup"><span data-stu-id="a98f1-131">Copy the code below into the file.</span></span>

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

## <a name="configuring-npm"></a><span data-ttu-id="a98f1-132">Konfigurowanie Menedżera NPM</span><span class="sxs-lookup"><span data-stu-id="a98f1-132">Configuring NPM</span></span>

<span data-ttu-id="a98f1-133">Skonfiguruj, aby pobrać grunt i grunt zadania programu NPM.</span><span class="sxs-lookup"><span data-stu-id="a98f1-133">Next, configure NPM to download grunt and grunt-tasks.</span></span>

1. <span data-ttu-id="a98f1-134">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj > Nowy element** z menu kontekstowego.</span><span class="sxs-lookup"><span data-stu-id="a98f1-134">In the Solution Explorer, right-click the project and select **Add > New Item** from the context menu.</span></span> <span data-ttu-id="a98f1-135">Wybierz **pliku konfiguracyjnego NPM** , należy pozostawić nazwę domyślną *package.json*i kliknij przycisk **Dodaj** przycisku.</span><span class="sxs-lookup"><span data-stu-id="a98f1-135">Select the **NPM configuration file** item, leave the default name, *package.json*, and click the **Add** button.</span></span>

2. <span data-ttu-id="a98f1-136">W *package.json* pliku wewnątrz `devDependencies` obiektu nawiasów klamrowych, wprowadź "grunt".</span><span class="sxs-lookup"><span data-stu-id="a98f1-136">In the *package.json* file, inside the `devDependencies` object braces, enter "grunt".</span></span> <span data-ttu-id="a98f1-137">Wybierz `grunt` z funkcji Intellisense listy, a następnie naciśnij klawisz Enter.</span><span class="sxs-lookup"><span data-stu-id="a98f1-137">Select `grunt` from the Intellisense list and press the Enter key.</span></span> <span data-ttu-id="a98f1-138">Visual Studio zacytować nazwę pakietu grunt i Dodaj dwukropka.</span><span class="sxs-lookup"><span data-stu-id="a98f1-138">Visual Studio will quote the grunt package name, and add a colon.</span></span> <span data-ttu-id="a98f1-139">Po prawej stronie dwukropek, wybierz najnowszą stabilną wersję pakietu z góry na liście funkcji Intellisense (naciśnij klawisz `Ctrl-Space` Intellisense, nie są wyświetlane).</span><span class="sxs-lookup"><span data-stu-id="a98f1-139">To the right of the colon, select the latest stable version of the package from the top of the Intellisense list (press `Ctrl-Space` if Intellisense doesn't appear).</span></span>

    ![grunt Intellisense](using-grunt/_static/devdependencies-grunt.png)

    > [!NOTE]
    > <span data-ttu-id="a98f1-141">Używa NPM [wersji semantycznej](http://semver.org/) do organizowania zależności.</span><span class="sxs-lookup"><span data-stu-id="a98f1-141">NPM uses [semantic versioning](http://semver.org/) to organize dependencies.</span></span> <span data-ttu-id="a98f1-142">Semantyczne przechowywania wersji, znany także jako SemVer identyfikuje pakiety ze schematu numerowania \<główna >.\< pomocnicza >. \<poprawki >.</span><span class="sxs-lookup"><span data-stu-id="a98f1-142">Semantic versioning, also known as SemVer, identifies packages with the numbering scheme \<major>.\<minor>.\<patch>.</span></span> <span data-ttu-id="a98f1-143">IntelliSense ułatwia semantycznego versioning przedstawiający kilka typowe opcje.</span><span class="sxs-lookup"><span data-stu-id="a98f1-143">Intellisense simplifies semantic versioning by showing only a few common choices.</span></span> <span data-ttu-id="a98f1-144">Pierwszy element na liście funkcji Intellisense (0.4.5 w powyższym przykładzie), jest uznawana za stabilną najnowszą wersję pakietu.</span><span class="sxs-lookup"><span data-stu-id="a98f1-144">The top item in the Intellisense list (0.4.5 in the example above) is considered the latest stable version of the package.</span></span> <span data-ttu-id="a98f1-145">Symbolu daszka (^) odpowiada najbardziej aktualną wersję główną i tyldy (~) dopasowuje najbardziej aktualną wersję pomocniczą.</span><span class="sxs-lookup"><span data-stu-id="a98f1-145">The caret (^) symbol matches the most recent major version and the tilde (~) matches the most recent minor version.</span></span> <span data-ttu-id="a98f1-146">Zobacz [odwołanie analizatora wersji semver NPM](https://www.npmjs.com/package/semver) rolę przewodnika po pełnej expressivity, która zapewnia SemVer.</span><span class="sxs-lookup"><span data-stu-id="a98f1-146">See the [NPM semver version parser reference](https://www.npmjs.com/package/semver) as a guide to the full expressivity that SemVer provides.</span></span>

3. <span data-ttu-id="a98f1-147">Dodaj więcej zależności, aby załadować grunt-contrib -\* pakietów dla *czyste*, *jshint*, *concat*, *uglify*i *Obejrzyj* jak pokazano w poniższym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="a98f1-147">Add more dependencies to load grunt-contrib-\* packages for *clean*, *jshint*, *concat*, *uglify*, and *watch* as shown in the example below.</span></span> <span data-ttu-id="a98f1-148">Wersje muszą być zgodne w przykładzie.</span><span class="sxs-lookup"><span data-stu-id="a98f1-148">The versions don't need to match the example.</span></span>

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

4. <span data-ttu-id="a98f1-149">Zapisz *package.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="a98f1-149">Save the *package.json* file.</span></span>

<span data-ttu-id="a98f1-150">Pakiety dla każdego elementu devDependencies pobierze oraz wszystkie pliki, które wymaga każdego pakietu.</span><span class="sxs-lookup"><span data-stu-id="a98f1-150">The packages for each devDependencies item will download, along with any files that each package requires.</span></span> <span data-ttu-id="a98f1-151">Można znaleźć plików pakietu w `node_modules` katalogu, włączając **Pokaż wszystkie pliki** przycisku w Eksploratorze rozwiązań.</span><span class="sxs-lookup"><span data-stu-id="a98f1-151">You can find the package files in the `node_modules` directory by enabling the **Show All Files** button in the Solution Explorer.</span></span>

![grunt node_modules](using-grunt/_static/node-modules.png)

> [!NOTE]
> <span data-ttu-id="a98f1-153">Jeśli zachodzi potrzeba, można ręcznie przywrócić zależności w Eksploratorze rozwiązań kliknij prawym przyciskiem myszy `Dependencies\NPM` i wybierając polecenie **przywracania pakietów** opcji menu.</span><span class="sxs-lookup"><span data-stu-id="a98f1-153">If you need to, you can manually restore dependencies in Solution Explorer by right-clicking on `Dependencies\NPM` and selecting the **Restore Packages** menu option.</span></span>

![Przywracanie pakietów](using-grunt/_static/restore-packages.png)

## <a name="configuring-grunt"></a><span data-ttu-id="a98f1-155">Konfigurowanie Grunt</span><span class="sxs-lookup"><span data-stu-id="a98f1-155">Configuring Grunt</span></span>

<span data-ttu-id="a98f1-156">Grunt jest konfigurowana przy użyciu manifestu o nazwie *plik Gruntfile.js* który definiuje, ładuje i rejestruje zadania, które mogą być uruchamiane ręcznie lub skonfigurowany do uruchamiania automatycznie na podstawie zdarzeń w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a98f1-156">Grunt is configured using a manifest named *Gruntfile.js* that defines, loads and registers tasks that can be run manually or configured to run automatically based on events in Visual Studio.</span></span>

1. <span data-ttu-id="a98f1-157">Kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj > Nowy element**.</span><span class="sxs-lookup"><span data-stu-id="a98f1-157">Right-click the project and select **Add > New Item**.</span></span> <span data-ttu-id="a98f1-158">Wybierz **plik konfiguracyjny Grunt** opcji, pozostaw nazwę domyślną *plik Gruntfile.js*i kliknij przycisk **Dodaj** przycisku.</span><span class="sxs-lookup"><span data-stu-id="a98f1-158">Select the **Grunt Configuration file** option, leave the default name, *Gruntfile.js*, and click the **Add** button.</span></span>

   <span data-ttu-id="a98f1-159">Kod początkowy obejmuje definicji modułu i `grunt.initConfig()` metody.</span><span class="sxs-lookup"><span data-stu-id="a98f1-159">The initial code includes a module definition and the `grunt.initConfig()` method.</span></span> <span data-ttu-id="a98f1-160">`initConfig()` Służy do ustawiania opcji dla każdego pakietu, a pozostała część modułu zostaną załadowane i zarejestrować zadań.</span><span class="sxs-lookup"><span data-stu-id="a98f1-160">The `initConfig()` is used to set options for each package, and the remainder of the module will load and register tasks.</span></span>

   ```javascript
   module.exports = function (grunt) {
     grunt.initConfig({
     });
   };
   ```

2. <span data-ttu-id="a98f1-161">Wewnątrz `initConfig()` metody, dodać opcje dla `clean` zadań, jak pokazano w przykładzie *plik Gruntfile.js* poniżej.</span><span class="sxs-lookup"><span data-stu-id="a98f1-161">Inside the `initConfig()` method, add options for the `clean` task as shown in the example *Gruntfile.js* below.</span></span> <span data-ttu-id="a98f1-162">Zadanie czysty akceptuje tablicę ciągów katalogów.</span><span class="sxs-lookup"><span data-stu-id="a98f1-162">The clean task accepts an array of directory strings.</span></span> <span data-ttu-id="a98f1-163">To zadanie usuwa pliki z wwwroot/lib i usuwa całą/tymczasowego katalogu.</span><span class="sxs-lookup"><span data-stu-id="a98f1-163">This task removes files from wwwroot/lib and removes the entire /temp directory.</span></span>

    ```javascript
    module.exports = function (grunt) {
      grunt.initConfig({
        clean: ["wwwroot/lib/*", "temp/"],
      });
    };
    ```

3. <span data-ttu-id="a98f1-164">Poniżej metody initConfig(), dodaj wywołanie `grunt.loadNpmTasks()`.</span><span class="sxs-lookup"><span data-stu-id="a98f1-164">Below the initConfig() method, add a call to `grunt.loadNpmTasks()`.</span></span> <span data-ttu-id="a98f1-165">Dzięki temu zadania możliwe do uruchomienia w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a98f1-165">This will make the task runnable from Visual Studio.</span></span>

    ```javascript
    grunt.loadNpmTasks("grunt-contrib-clean");
    ```

4. <span data-ttu-id="a98f1-166">Zapisz *plik Gruntfile.js*.</span><span class="sxs-lookup"><span data-stu-id="a98f1-166">Save *Gruntfile.js*.</span></span> <span data-ttu-id="a98f1-167">Plik powinien wyglądać podobnie jak na poniższym zrzucie ekranu.</span><span class="sxs-lookup"><span data-stu-id="a98f1-167">The file should look something like the screenshot below.</span></span>

    ![początkowa gruntfile](using-grunt/_static/gruntfile-js-initial.png)

5. <span data-ttu-id="a98f1-169">Kliknij prawym przyciskiem myszy *plik Gruntfile.js* i wybierz **Eksplorator modułu uruchamiającego zadania** z menu kontekstowego.</span><span class="sxs-lookup"><span data-stu-id="a98f1-169">Right-click *Gruntfile.js* and select **Task Runner Explorer** from the context menu.</span></span> <span data-ttu-id="a98f1-170">Zostanie otwarte okno Eksplorator modułu uruchamiającego zadania.</span><span class="sxs-lookup"><span data-stu-id="a98f1-170">The Task Runner Explorer window will open.</span></span>

    ![menu Eksploratora modułu uruchamiającego zadania](using-grunt/_static/task-runner-explorer-menu.png)

6. <span data-ttu-id="a98f1-172">Upewnij się, że `clean` pokazuje, w obszarze **zadania** w Eksplorator modułu uruchamiającego zadania.</span><span class="sxs-lookup"><span data-stu-id="a98f1-172">Verify that `clean` shows under **Tasks** in the Task Runner Explorer.</span></span>

    ![Lista zadań Eksploratora modułu uruchamiającego zadania](using-grunt/_static/task-runner-explorer-tasks.png)

7. <span data-ttu-id="a98f1-174">Kliknij prawym przyciskiem myszy zadanie czysty, a następnie wybierz pozycję **Uruchom** z menu kontekstowego.</span><span class="sxs-lookup"><span data-stu-id="a98f1-174">Right-click the clean task and select **Run** from the context menu.</span></span> <span data-ttu-id="a98f1-175">Okno polecenia wyświetla postęp zadania.</span><span class="sxs-lookup"><span data-stu-id="a98f1-175">A command window displays progress of the task.</span></span>

    ![zadanie czysty Uruchom Eksploratora modułu uruchamiającego zadania](using-grunt/_static/task-runner-explorer-run-clean.png)

    > [!NOTE]
    > <span data-ttu-id="a98f1-177">Brak plików i katalogów, aby wyczyścić jeszcze.</span><span class="sxs-lookup"><span data-stu-id="a98f1-177">There are no files or directories to clean yet.</span></span> <span data-ttu-id="a98f1-178">Jeśli chcesz możesz ręcznie utworzyć je w Eksploratorze rozwiązań i następnie uruchom zadanie czysty jako test.</span><span class="sxs-lookup"><span data-stu-id="a98f1-178">If you like, you can manually create them in the Solution Explorer and then run the clean task as a test.</span></span>

8. <span data-ttu-id="a98f1-179">W metodzie initConfig() Dodaj wpis dla `concat` przy użyciu kodu poniżej.</span><span class="sxs-lookup"><span data-stu-id="a98f1-179">In the initConfig() method, add an entry for `concat` using the code below.</span></span>

    <span data-ttu-id="a98f1-180">`src` Tablicy właściwości zawiera listę plików, połączyć w kolejności, powinny być połączone.</span><span class="sxs-lookup"><span data-stu-id="a98f1-180">The `src` property array lists files to combine, in the order that they should be combined.</span></span> <span data-ttu-id="a98f1-181">`dest` Właściwość przypisuje ścieżce do połączonego pliku, który jest generowany.</span><span class="sxs-lookup"><span data-stu-id="a98f1-181">The `dest` property assigns the path to the combined file that's produced.</span></span>

    ```javascript
    concat: {
      all: {
        src: ['TypeScript/Tastes.js', 'TypeScript/Food.js'],
        dest: 'temp/combined.js'
      }
    },
    ```

    > [!NOTE]
    > <span data-ttu-id="a98f1-182">`all` Właściwość w powyższym kodzie jest nazwą obiektu docelowego.</span><span class="sxs-lookup"><span data-stu-id="a98f1-182">The `all` property in the code above is the name of a target.</span></span> <span data-ttu-id="a98f1-183">Obiekty docelowe są używane w niektórych zadań Grunt do wielu środowisk kompilacji.</span><span class="sxs-lookup"><span data-stu-id="a98f1-183">Targets are used in some Grunt tasks to allow multiple build environments.</span></span> <span data-ttu-id="a98f1-184">Możesz wyświetlić wbudowane obiekty docelowe za pomocą technologii Intellisense lub przypisać własne.</span><span class="sxs-lookup"><span data-stu-id="a98f1-184">You can view the built-in targets using Intellisense or assign your own.</span></span>

9. <span data-ttu-id="a98f1-185">Dodaj `jshint` zadań przy użyciu kodu poniżej.</span><span class="sxs-lookup"><span data-stu-id="a98f1-185">Add the `jshint` task using the code below.</span></span>

    <span data-ttu-id="a98f1-186">Narzędzie jakości kodu jshint jest wykonywany dla każdego pliku JavaScript, znajduje się w katalogu temp.</span><span class="sxs-lookup"><span data-stu-id="a98f1-186">The jshint code-quality utility is run against every JavaScript file found in the temp directory.</span></span>

    ```javascript
    jshint: {
      files: ['temp/*.js'],
      options: {
        '-W069': false,
      }
    },
    ```

    > [!NOTE]
    > <span data-ttu-id="a98f1-187">Opcja "-W069" jest błąd generowany przez jshint podczas używa języka JavaScript dopasowywanie składni, aby przypisać właściwość zamiast notacji z kropką, czyli `Tastes["Sweet"]` zamiast `Tastes.Sweet`.</span><span class="sxs-lookup"><span data-stu-id="a98f1-187">The option "-W069" is an error produced by jshint when JavaScript uses bracket syntax to assign a property instead of dot notation, i.e. `Tastes["Sweet"]` instead of `Tastes.Sweet`.</span></span> <span data-ttu-id="a98f1-188">Opcja wyłącza ostrzeżenie, aby umożliwić resztą procesu, aby kontynuować.</span><span class="sxs-lookup"><span data-stu-id="a98f1-188">The option turns off the warning to allow the rest of the process to continue.</span></span>

10. <span data-ttu-id="a98f1-189">Dodaj `uglify` zadań przy użyciu kodu poniżej.</span><span class="sxs-lookup"><span data-stu-id="a98f1-189">Add the `uglify` task using the code below.</span></span>

    <span data-ttu-id="a98f1-190">Zadanie minimalizuje *combined.js* plików znajduje się w katalogu temp i tworzy plik wyników w wwwroot/lib zgodnie ze standardową konwencją nazewnictwa  *\<nazwy pliku\>. min.js*.</span><span class="sxs-lookup"><span data-stu-id="a98f1-190">The task minifies the *combined.js* file found in the temp directory and creates the result file in wwwroot/lib following the standard naming convention *\<file name\>.min.js*.</span></span>

    ```javascript
    uglify: {
     all: {
       src: ['temp/combined.js'],
       dest: 'wwwroot/lib/combined.min.js'
     }
    },
    ```

11. <span data-ttu-id="a98f1-191">W obszarze grunt.loadNpmTasks() wywołań, który ładuje, wyczyść contrib grunt należy uwzględnić to samo wywołanie dla jshint, concat i uglify przy użyciu poniższego kodu.</span><span class="sxs-lookup"><span data-stu-id="a98f1-191">Under the call grunt.loadNpmTasks() that loads grunt-contrib-clean, include the same call for jshint, concat and uglify using the code below.</span></span>

    ```javascript
    grunt.loadNpmTasks('grunt-contrib-jshint');
    grunt.loadNpmTasks('grunt-contrib-concat');
    grunt.loadNpmTasks('grunt-contrib-uglify');
    ```

12. <span data-ttu-id="a98f1-192">Zapisz *plik Gruntfile.js*.</span><span class="sxs-lookup"><span data-stu-id="a98f1-192">Save *Gruntfile.js*.</span></span> <span data-ttu-id="a98f1-193">Plik powinien wyglądać podobnie do poniższego przykładu.</span><span class="sxs-lookup"><span data-stu-id="a98f1-193">The file should look something like the example below.</span></span>

    ![przykład pliku pełną grunt](using-grunt/_static/gruntfile-js-complete.png)

13. <span data-ttu-id="a98f1-195">Należy zauważyć, że lista zadań Eksploratora modułu uruchamiającego zadania zawiera `clean`, `concat`, `jshint` i `uglify` zadania.</span><span class="sxs-lookup"><span data-stu-id="a98f1-195">Notice that the Task Runner Explorer Tasks list includes `clean`, `concat`, `jshint` and `uglify` tasks.</span></span> <span data-ttu-id="a98f1-196">Każde zadanie podrzędne są uruchamiane w kolejności i obserwują rezultaty w Eksploratorze rozwiązań.</span><span class="sxs-lookup"><span data-stu-id="a98f1-196">Run each task in order and observe the results in Solution Explorer.</span></span> <span data-ttu-id="a98f1-197">Każde zadanie powinno działać bez błędów.</span><span class="sxs-lookup"><span data-stu-id="a98f1-197">Each task should run without errors.</span></span>

    ![Eksplorator modułu uruchamiającego zadania, każde zadanie podrzędne uruchamiania](using-grunt/_static/task-runner-explorer-run-each-task.png)

    <span data-ttu-id="a98f1-199">Zadanie concat tworzy nową *combined.js* pliku i umieszcza go w katalogu tymczasowego.</span><span class="sxs-lookup"><span data-stu-id="a98f1-199">The concat task creates a new *combined.js* file and places it into the temp directory.</span></span> <span data-ttu-id="a98f1-200">Zadanie jshint po prostu działa i nie generuje danych wyjściowych.</span><span class="sxs-lookup"><span data-stu-id="a98f1-200">The jshint task simply runs and doesn't produce output.</span></span> <span data-ttu-id="a98f1-201">Zadanie uglify tworzy nową *combined.min.js* pliku i umieszcza go w wwwroot/lib.</span><span class="sxs-lookup"><span data-stu-id="a98f1-201">The uglify task creates a new *combined.min.js* file and places it into wwwroot/lib.</span></span> <span data-ttu-id="a98f1-202">Po zakończeniu rozwiązania powinien wyglądać podobnie jak na poniższym zrzucie ekranu:</span><span class="sxs-lookup"><span data-stu-id="a98f1-202">On completion, the solution should look something like the screenshot below:</span></span>

    ![Eksplorator rozwiązań po wszystkich zadań.](using-grunt/_static/solution-explorer-after-all-tasks.png)

    > [!NOTE]
    > <span data-ttu-id="a98f1-204">Aby uzyskać więcej informacji na temat opcji dla każdego pakietu, odwiedź stronę [ https://www.npmjs.com/ ](https://www.npmjs.com/) i wyszukiwania nazwy pakietu, w polu wyszukiwania na stronie głównej.</span><span class="sxs-lookup"><span data-stu-id="a98f1-204">For more information on the options for each package, visit [https://www.npmjs.com/](https://www.npmjs.com/) and lookup the package name in the search box on the main page.</span></span> <span data-ttu-id="a98f1-205">Na przykład możesz wyszukać wyczyścić contrib grunt pakiet do pobrania link do dokumentacji, który objaśnia, wszystkie jego parametry.</span><span class="sxs-lookup"><span data-stu-id="a98f1-205">For example, you can look up the grunt-contrib-clean package to get a documentation link that explains all of its parameters.</span></span>

### <a name="all-together-now"></a><span data-ttu-id="a98f1-206">Wszystko w jednym miejscu</span><span class="sxs-lookup"><span data-stu-id="a98f1-206">All together now</span></span>

<span data-ttu-id="a98f1-207">Korzystanie z Grunt `registerTask()` metodę, aby uruchomić szereg zadań w określonej kolejności.</span><span class="sxs-lookup"><span data-stu-id="a98f1-207">Use the Grunt `registerTask()` method to run a series of tasks in a particular sequence.</span></span> <span data-ttu-id="a98f1-208">Na przykład, aby uruchomić przykład powyższe kroki w kolejności clean -> concat -> jshint -> uglify, Dodaj poniższy kod do modułu.</span><span class="sxs-lookup"><span data-stu-id="a98f1-208">For example, to run the example steps above in the order clean -> concat -> jshint -> uglify, add the code below to the module.</span></span> <span data-ttu-id="a98f1-209">Kod należy dodać do tego samego poziomu wywołania metody loadNpmTasks() poza initConfig.</span><span class="sxs-lookup"><span data-stu-id="a98f1-209">The code should be added to the same level as the loadNpmTasks() calls, outside initConfig.</span></span>

```javascript
grunt.registerTask("all", ['clean', 'concat', 'jshint', 'uglify']);
```

<span data-ttu-id="a98f1-210">Nowe zadanie pojawia się w Eksplorator modułu uruchamiającego zadania w obszarze zadania aliasów.</span><span class="sxs-lookup"><span data-stu-id="a98f1-210">The new task shows up in Task Runner Explorer under Alias Tasks.</span></span> <span data-ttu-id="a98f1-211">Można prawym przyciskiem myszy, a następnie uruchom go, tak samo jak inne zadania.</span><span class="sxs-lookup"><span data-stu-id="a98f1-211">You can right-click and run it just as you would other tasks.</span></span> <span data-ttu-id="a98f1-212">`all` Zadanie zostanie uruchomione `clean`, `concat`, `jshint` i `uglify`, w kolejności.</span><span class="sxs-lookup"><span data-stu-id="a98f1-212">The `all` task will run `clean`, `concat`, `jshint` and `uglify`, in order.</span></span>

![alias grunt zadania](using-grunt/_static/alias-tasks.png)

## <a name="watching-for-changes"></a><span data-ttu-id="a98f1-214">Obserwowanie zmian</span><span class="sxs-lookup"><span data-stu-id="a98f1-214">Watching for changes</span></span>

<span data-ttu-id="a98f1-215">A `watch` zadań przechowuje list plików i katalogów.</span><span class="sxs-lookup"><span data-stu-id="a98f1-215">A `watch` task keeps an eye on files and directories.</span></span> <span data-ttu-id="a98f1-216">Obejrzyj automatycznie wyzwala zadania, jeśli wykryje zmiany.</span><span class="sxs-lookup"><span data-stu-id="a98f1-216">The watch triggers tasks automatically if it detects changes.</span></span> <span data-ttu-id="a98f1-217">Dodaj poniższy kod do initConfig, aby obejrzeć zmiany \*pliki .js w katalogu TypeScript.</span><span class="sxs-lookup"><span data-stu-id="a98f1-217">Add the code below to initConfig to watch for changes to \*.js files in the TypeScript directory.</span></span> <span data-ttu-id="a98f1-218">Po zmianie pliku JavaScript `watch` uruchomi `all` zadania.</span><span class="sxs-lookup"><span data-stu-id="a98f1-218">If a JavaScript file is changed, `watch` will run the `all` task.</span></span>

```javascript
watch: {
  files: ["TypeScript/*.js"],
  tasks: ["all"]
}
```

<span data-ttu-id="a98f1-219">Dodaj wywołanie do `loadNpmTasks()` pokazanie `watch` zadania w Eksplorator modułu uruchamiającego zadania.</span><span class="sxs-lookup"><span data-stu-id="a98f1-219">Add a call to `loadNpmTasks()` to show the `watch` task in Task Runner Explorer.</span></span>

```javascript
grunt.loadNpmTasks('grunt-contrib-watch');
```

<span data-ttu-id="a98f1-220">Kliknij prawym przyciskiem myszy zadanie Obejrzyj Eksplorator modułu uruchamiającego zadania, a następnie wybierz polecenie Uruchom z menu kontekstowego.</span><span class="sxs-lookup"><span data-stu-id="a98f1-220">Right-click the watch task in Task Runner Explorer and select Run from the context menu.</span></span> <span data-ttu-id="a98f1-221">W oknie polecenia, pokazujący zadania Obejrzyj działającego będą wyświetlane "Oczekiwanie..." Komunikat.</span><span class="sxs-lookup"><span data-stu-id="a98f1-221">The command window that shows the watch task running will display a "Waiting…" message.</span></span> <span data-ttu-id="a98f1-222">Otwórz jeden z plików TypeScript, Dodaj spację, a następnie zapisz plik.</span><span class="sxs-lookup"><span data-stu-id="a98f1-222">Open one of the TypeScript files, add a space, and then save the file.</span></span> <span data-ttu-id="a98f1-223">Spowoduje to wyzwalanie zadań wyrażenie kontrolne i wyzwalać inne zadania są uruchamiane w kolejności.</span><span class="sxs-lookup"><span data-stu-id="a98f1-223">This will trigger the watch task and trigger the other tasks to run in order.</span></span> <span data-ttu-id="a98f1-224">Poniższy zrzut ekranu przedstawia Uruchom przykład.</span><span class="sxs-lookup"><span data-stu-id="a98f1-224">The screenshot below shows a sample run.</span></span>

![Uruchamianie zadań w danych wyjściowych](using-grunt/_static/watch-running.png)

## <a name="binding-to-visual-studio-events"></a><span data-ttu-id="a98f1-226">Powiązanie z zdarzenia programu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a98f1-226">Binding to Visual Studio events</span></span>

<span data-ttu-id="a98f1-227">Jeśli nie chcesz ręcznie uruchomić zadania za każdym razem, gdy pracujesz w programie Visual Studio, można powiązać zadań **przed kompilacji**, **po kompilacji**, **czysty**, i  **Projekt Open** zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="a98f1-227">Unless you want to manually start your tasks every time you work in Visual Studio, you can bind tasks to **Before Build**, **After Build**, **Clean**, and **Project Open** events.</span></span>

<span data-ttu-id="a98f1-228">Powiążmy `watch` tak, aby była uruchamiana za każdym razem, gdy zostanie otwarty program Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a98f1-228">Let’s bind `watch` so that it runs every time Visual Studio opens.</span></span> <span data-ttu-id="a98f1-229">W Eksplorator modułu uruchamiającego zadania, kliknij prawym przyciskiem myszy zadanie wyrażenie kontrolne, a następnie wybierz **powiązania > Otwórz projekt** z menu kontekstowego.</span><span class="sxs-lookup"><span data-stu-id="a98f1-229">In Task Runner Explorer, right-click the watch task and select **Bindings > Project Open** from the context menu.</span></span>

![Powiąż zadania do otwarcia projektu](using-grunt/_static/bindings-project-open.png)

<span data-ttu-id="a98f1-231">Zwolnij i ponownie Załaduj projekt.</span><span class="sxs-lookup"><span data-stu-id="a98f1-231">Unload and reload the project.</span></span> <span data-ttu-id="a98f1-232">Gdy projekt ładuje się ponownie, zadanie Obejrzyj zostanie uruchomione automatycznie.</span><span class="sxs-lookup"><span data-stu-id="a98f1-232">When the project loads again, the watch task will start running automatically.</span></span>

## <a name="summary"></a><span data-ttu-id="a98f1-233">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="a98f1-233">Summary</span></span>

<span data-ttu-id="a98f1-234">Grunt jest modułu uruchamiającego zadania zaawansowane, który może służyć do zautomatyzowania większości zadań kompilacji klienta.</span><span class="sxs-lookup"><span data-stu-id="a98f1-234">Grunt is a powerful task runner that can be used to automate most client-build tasks.</span></span> <span data-ttu-id="a98f1-235">Grunt korzysta z programu NPM dostarczanie pakietów i funkcje narzędzi integracji z programem Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a98f1-235">Grunt leverages NPM to deliver its packages, and features tooling integration with Visual Studio.</span></span> <span data-ttu-id="a98f1-236">Eksplorator modułu uruchamiającego zadania programu Visual Studio wykrywa zmiany w plikach konfiguracji i oferuje wygodny interfejs do uruchamiania zadań, Wyświetl uruchomione zadania podrzędne i powiąż zadania ze zdarzenia programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a98f1-236">Visual Studio's Task Runner Explorer detects changes to configuration files and provides a convenient interface to run tasks, view running tasks, and bind tasks to Visual Studio events.</span></span>
