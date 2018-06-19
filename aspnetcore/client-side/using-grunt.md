---
title: Użyj Grunt w platformy ASP.NET Core
author: rick-anderson
description: ''
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: client-side/using-grunt
ms.openlocfilehash: 169552e9b5dd811884ce1c65952677ba83626b58
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30897184"
---
# <a name="use-grunt-in-aspnet-core"></a>Użyj Grunt w platformy ASP.NET Core

Przez [ryżu Noel](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/)

Grunt jest modułu uruchamiającego zadania JavaScript, który zautomatyzuje minimalizację skryptu, kompilowanie kodu TypeScript narzędzia "w wierszu" jakości kodu, procesory wstępne CSS i niemal wszystkie powtarzających się kwestii, wymagającym czynności umożliwiających rozwój klienta. Grunt jest w pełni obsługiwana w programie Visual Studio, że szablony projektów programu ASP.NET Gulp domyślnie (zobacz [Użyj system Gulp](using-gulp.md)).

W tym przykładzie używane pusty projekt platformy ASP.NET Core jako punktu początkowego pokazanie sposobu automatyzacji procesu kompilacji klienta od początku.

Zakończono przykład oczyszcza katalog docelowy wdrożenia, łączy pliki JavaScript, sprawdza jakości kodu, pozwala zapisać zawartości pliku JavaScript i wdraża do katalogu głównego aplikacji sieci web. Firma Microsoft będzie używać następujących pakietów:

* **grunt**: pakiet modułu uruchamiającego zadania Grunt.

* **Wyczyść contrib grunt**: wtyczkę, która usuwa pliki lub katalogi.

* **grunt-contrib-jshint**: dodatek, który przegląda jakości kodu JavaScript.

* **concat — contrib-grunt**: wtyczkę, która łączy pliki w jednym pliku.

* **grunt contrib uglify**: wtyczkę, która minimalizuje JavaScript, aby zmniejszyć rozmiar.

* **grunt-contrib czujki**: wtyczkę, która prowadzi obserwację operacje na plikach.

## <a name="preparing-the-application"></a>Przygotowywanie aplikacji

Aby rozpocząć, skonfiguruj nową aplikację sieci web pusty, a następnie dodaj pliki TypeScript przykład. Pliki typeScript automatycznie są kompilowane do języka JavaScript przy użyciu domyślnych ustawień programu Visual Studio i będą naszych materiałów raw do przetworzenia za pomocą Grunt.

1.  W programie Visual Studio Utwórz nową `ASP.NET Web Application`.

2.  W **nowy projekt ASP.NET** okno dialogowe, wybierz platformy ASP.NET Core **pusty** szablonu i kliknij przycisk OK.

3.  Sprawdź struktury projektu w Eksploratorze rozwiązań. `\src` Folder zawiera pusty `wwwroot` i `Dependencies` węzłów.

    ![rozwiązanie pusty sieci web](using-grunt/_static/grunt-solution-explorer.png)

4.  Dodaj nowy folder o nazwie `TypeScript` do katalogu projektu.

5.  Przed dodaniem wszystkie pliki, upewnij się, że program Visual Studio jest opcja "skompilować przy zapisie" dla plików TypeScript zaznaczone. Przejdź do **narzędzia** > **opcje** > **Edytor tekstu** > **Typescript**  >  **Projektu**:

    ![Opcje ustawienia automatycznego compliation plików TypeScript](using-grunt/_static/typescript-options.png)

6.  Kliknij prawym przyciskiem myszy `TypeScript` katalogu i zaznacz **Dodaj > Nowy element** z menu kontekstowego. Wybierz **plik JavaScript** elementu i nazwij plik *Tastes.ts* (Uwaga \*rozszerzenia TS). Skopiuj wiersz kodu TypeScript poniżej do pliku (po zapisaniu, nowy *Tastes.js* plik pojawi się ze źródłem JavaScript).
    
    ```typescript
    enum Tastes { Sweet, Sour, Salty, Bitter }
    ```

7.  Dodaj drugi plik do **TypeScript** katalogu i nadaj mu nazwę `Food.ts`. Skopiuj poniższy kod do pliku.

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

## <a name="configuring-npm"></a>Konfigurowanie programu NPM

Skonfiguruj NPM, aby pobrać grunt i grunt zadania.

1. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj > Nowy element** z menu kontekstowego. Wybierz **plik konfiguracji NPM** element, pozostaw nazwę domyślną *package.json*i kliknij przycisk **Dodaj** przycisku.

2. W *package.json* pliku wewnątrz `devDependencies` obiektu nawiasów klamrowych, wprowadź "grunt". Wybierz `grunt` z funkcji Intellisense listy, a następnie naciśnij klawisz Enter. Visual Studio oferta grunt nazwę pakietu i dodać dwukropkiem. Po prawej stronie dwukropek, wybierz najnowsza stabilna wersja pakietu z początku listy Intellisense (naciśnij klawisz `Ctrl-Space` Intellisense, nie pojawia się).

    ![grun Intellisense](using-grunt/_static/devdependencies-grunt.png)
    
    > [!NOTE]
    > Używa NPM [wersjonowania semantycznego](http://semver.org/) do organizowania zależności. Wersjonowania semantycznego, znanej także jako programu SemVer identyfikuje pakiety ze schematu numerowania <major>.<minor>. <patch>. Przedstawiający kilka typowe opcje IntelliSense upraszcza wersjonowania semantycznego. Pierwszy element na liście Intellisense (0.4.5 w powyższym przykładzie) jest uznawany za najnowsza stabilna wersja pakietu. Symbol daszek (^) najnowszą wersją główną i tyldy (~) najnowszą wersją pomocniczą. Zobacz [odwołanie analizatora wersji programu semver NPM](https://www.npmjs.com/package/semver) jako przewodnik dotyczący expressivity pełnego, który zapewnia programu SemVer.

3. Dodaj więcej zależności, aby załadować grunt-contrib -\* pakietami *czystą*, *jshint*, *concat*, *uglify*i *czujki* jak pokazano w poniższym przykładzie. Wersje nie muszą być identyczne w przykładzie.

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

4. Zapisz *package.json* pliku.

Pakiety dla każdego elementu devDependencies pobierze oraz wszystkie pliki, które wymaga każdego pakietu. Można znaleźć plików pakietu w `node_modules` katalogu przez włączenie **Pokaż wszystkie pliki** przycisk w Eksploratorze rozwiązań.

![grunt node_modules](using-grunt/_static/node-modules.png)

> [!NOTE]
> Jeśli zachodzi potrzeba, można ręcznie przywrócić zależności w Eksploratorze rozwiązań, klikając prawym przyciskiem myszy `Dependencies\NPM` i wybierając **Przywracanie pakietów** opcji menu.

![Przywracanie pakietów](using-grunt/_static/restore-packages.png)

## <a name="configuring-grunt"></a>Konfigurowanie Grunt

Grunt jest konfigurowana przy użyciu manifestu o nazwie *Gruntfile.js* który definiuje, ładuje i rejestruje zadania, które można uruchomić ręcznie lub skonfigurowany do uruchamiania automatycznie na podstawie zdarzeń w programie Visual Studio.

1. Kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj > Nowy element**. Wybierz **pliku konfiguracji Grunt** opcji, pozostaw nazwę domyślną *Gruntfile.js*i kliknij przycisk **Dodaj** przycisku.

   Początkowa kod zawiera definicję modułu i `grunt.initConfig()` metody. `initConfig()` Służy do ustawiania opcji dla każdego pakietu, a w pozostałej części modułu zostanie obciążenia i zarejestrować zadań.
    
   ```javascript
   module.exports = function (grunt) {
     grunt.initConfig({
     });
   };
   ```

2. Wewnątrz `initConfig()` metody, Dodaj opcje `clean` zadań, jak pokazano w przykładzie *Gruntfile.js* poniżej. Wyczyść zadań akceptuje tablica ciągów katalogu. To zadanie usuwa z wwwroot/lib pliki i usuwa cały/tymczasowego katalogu.

    ```javascript
    module.exports = function (grunt) {
      grunt.initConfig({
        clean: ["wwwroot/lib/*", "temp/"],
      });
    };
    ```

3. Poniżej metody initConfig(), dodaj wywołanie do `grunt.loadNpmTasks()`. Spowoduje to zadanie do uruchomienia z programu Visual Studio.

    ```javascript
    grunt.loadNpmTasks("grunt-contrib-clean");
    ```

4. Zapisz *Gruntfile.js*. Plik powinien wyglądać jak na poniższym zrzucie ekranu.

    ![gruntfile początkowej](using-grunt/_static/gruntfile-js-initial.png)

5. Kliknij prawym przyciskiem myszy *Gruntfile.js* i wybierz **Eksploratora modułu uruchamiającego zadania** z menu kontekstowego. Zostanie otwarte okno Eksploratora modułu uruchamiającego zadania.

    ![menu Eksploratora modułu uruchamiającego zadania](using-grunt/_static/task-runner-explorer-menu.png)

6. Sprawdź, czy `clean` przedstawiono w obszarze **zadania** w Eksploratora modułu uruchamiającego zadania.

    ![Lista zadań Eksploratora modułu uruchamiającego zadania](using-grunt/_static/task-runner-explorer-tasks.png)

7. Kliknij prawym przyciskiem myszy czystą zadań i wybierz **Uruchom** z menu kontekstowego. Okno polecenia wyświetla postęp zadania.

    ![Uruchom Eksploratora modułu uruchamiającego zadania, czystej zadania](using-grunt/_static/task-runner-explorer-run-clean.png)
    
    > [!NOTE]
    > Nie ma żadnych plików lub katalogów, aby wyczyścić jeszcze. Jeśli chcesz można ręcznie utworzyć je w Eksploratorze rozwiązań, a następnie uruchom zadanie czystą jako test.
    
8. W metodzie initConfig(), Dodaj wpis dla `concat` przy użyciu kodu poniżej.

    `src` Tablicy właściwości listy plików do łączenia, w kolejności, że powinny być połączone. `dest` Właściwość przypisuje ścieżka do połączonego pliku, który jest generowany.

    ```javascript
    concat: {
      all: {
        src: ['TypeScript/Tastes.js', 'TypeScript/Food.js'],
        dest: 'temp/combined.js'
      }
    },
    ```
    
    > [!NOTE]
    > `all` Właściwości w powyższym kodzie to nazwa obiektu docelowego. Obiekty docelowe są używane w niektórych zadań Grunt umożliwiają wielu środowiska kompilacji. Można wyświetlać wbudowanych elementów docelowych przy użyciu funkcji Intellisense lub przypisać własne.
    
9. Dodaj `jshint` zadań przy użyciu kodu poniżej.

    Narzędzie jakości kodu jshint jest wykonywane na każdy plik JavaScript znalezione w katalogu tymczasowym.
    
    ```javascript
    jshint: {
      files: ['temp/*.js'],
      options: {
        '-W069': false,
      }
    },
    ```

    > [!NOTE]
    > Opcja "-W069" jest błąd generowany przez jshint podczas JavaScript używa nawiasów składni, aby przypisać właściwości zamiast kropkowego, tj. `Tastes["Sweet"]` zamiast `Tastes.Sweet`. Opcja powoduje wyłączenie ostrzeżenia, aby umożliwić resztę procesu, aby kontynuować.

10. Dodaj `uglify` zadań przy użyciu kodu poniżej.

    Zadanie minimalizuje *combined.js* plik znaleziono w katalogu tymczasowym i tworzy plik wyników w wwwroot/lib zgodnie ze standardową konwencją nazewnictwa  *\<nazwę pliku\>. min.js*.
    
    ```javascript
    uglify: {
     all: {
       src: ['temp/combined.js'],
       dest: 'wwwroot/lib/combined.min.js'
     }
    },
    ```

11. W obszarze grunt.loadNpmTasks() wywołania, który ładuje czyszczenia contrib grunt obejmują to samo wywołanie dla jshint, concat i uglify przy użyciu kodu poniżej.
    
    ```javascript
    grunt.loadNpmTasks('grunt-contrib-jshint');
    grunt.loadNpmTasks('grunt-contrib-concat');
    grunt.loadNpmTasks('grunt-contrib-uglify');
    ```

12. Zapisz *Gruntfile.js*. Plik powinien wyglądać podobnie jak w poniższym przykładzie.

    ![przykład pliku grunt ukończone](using-grunt/_static/gruntfile-js-complete.png)

13. Należy zauważyć, że lista zadań Eksploratora modułu uruchamiającego zadania zawiera `clean`, `concat`, `jshint` i `uglify` zadania. Wszystkie zadania są uruchamiane w kolejności i obserwować wyniki w Eksploratorze rozwiązań. Wszystkie zadania są uruchamiane bez błędów.
    
    ![Uruchom każde zadanie Eksploratora modułu uruchamiającego zadania](using-grunt/_static/task-runner-explorer-run-each-task.png)
    
    Tworzy nowe zadanie concat *combined.js* pliku i umieszcza je w katalogu tymczasowego. Zadanie jshint po prostu działa i nie tworzy dane wyjściowe. Tworzy nowe zadanie uglify *combined.min.js* pliku i umieszcza je w wwwroot/lib. Po zakończeniu rozwiązanie powinien wyglądać jak na poniższym zrzucie ekranu:
    
    ![Eksplorator rozwiązań po wszystkich zadań.](using-grunt/_static/solution-explorer-after-all-tasks.png)
    
    > [!NOTE]
    > Aby uzyskać więcej informacji na temat opcji dla każdego pakietu, odwiedź stronę [ https://www.npmjs.com/ ](https://www.npmjs.com/) i wyszukiwania nazwę pakietu w polu wyszukiwania na stronie głównej. Na przykład można wyszukiwać czyszczenia contrib grunt pakiet do pobrania łącze dokumentacji, który objaśnia, wszystkie jego parametrów.

### <a name="all-together-now"></a>Wszystko w jednym miejscu

Użyj Grunt `registerTask()` metodę, aby uruchomić sekwencję zadań w określonej kolejności. Na przykład, aby uruchomić przykład, wykonując powyższe kroki w kolejności czystą -> concat -> jshint -> uglify, Dodaj poniższy kod do modułu. Kod należy dodać do tego samego poziomu wywołania metody loadNpmTasks() poza initConfig.

```javascript
grunt.registerTask("all", ['clean', 'concat', 'jshint', 'uglify']);
```

Nowe zadania zostaną wyświetlone w Eksploratora modułu uruchamiającego zadania w obszarze zadania Alias. Można kliknąć prawym przyciskiem myszy i uruchomić tak samo jak inne zadania. `all` Zadanie zostanie uruchomione `clean`, `concat`, `jshint` i `uglify`, w kolejności.

![alias grunt zadania](using-grunt/_static/alias-tasks.png)

## <a name="watching-for-changes"></a>Monitorowanie zmian

A `watch` zadań przechowuje list plików i katalogów. Obejrzyj automatycznie wyzwala zadania, jeśli wykryje zmiany. Dodaj poniższy kod do initConfig Aby obejrzeć wprowadzenie zmian w \*js plików w katalogu TypeScript. Jeśli plik JavaScript została zmieniona, `watch` uruchomi `all` zadań.

```javascript
watch: {
  files: ["TypeScript/*.js"],
  tasks: ["all"]
}
```

Dodaj wywołanie do `loadNpmTasks()` pokazanie `watch` zadanie w Eksploratora modułu uruchamiającego zadania.

```javascript
grunt.loadNpmTasks('grunt-contrib-watch');
```

Kliknij prawym przyciskiem myszy zadanie czujki w Eksploratora modułu uruchamiającego zadania i wybierz polecenie Uruchom z menu kontekstowego. W oknie polecenia, pokazujący zadanie czujki uruchomione będą wyświetlane "Oczekiwanie..." Komunikat. Otwórz jeden z plików TypeScript, Dodaj miejsce, a następnie zapisz plik. To zadanie czujki wyzwolą i wyzwalanie innych zadań są uruchamiane w kolejności. Poniższy zrzut ekranu przedstawia uruchamianie przykładowych.

![dane wyjściowe zadania uruchamiania](using-grunt/_static/watch-running.png)

## <a name="binding-to-visual-studio-events"></a>Powiązanie z zdarzeń programu Visual Studio

Chyba że chcesz ręcznie uruchomić zadania za każdym razem, gdy działa w programie Visual Studio, możesz powiązać zadań **przed kompilacji**, **po kompilacji**, **wyczyść**, i  **Otwórz projekt** zdarzenia.

Teraz powiązać `watch` , aby został uruchomiony za każdym razem, gdy zostanie otwarty program Visual Studio. W Eksploratora modułu uruchamiającego zadania, kliknij prawym przyciskiem myszy zadanie czujki i wybierz **powiązania > Otwórz projekt** z menu kontekstowego.

![Powiąż zadania do otwarcia projektu](using-grunt/_static/bindings-project-open.png)

Zwolnienie i ponowne załadowanie projektu. Podczas ponownie ładowania projektu czujki zadanie zostanie uruchomione automatycznie.

## <a name="summary"></a>Podsumowanie

Grunt jest modułu uruchamiającego zadania zaawansowanego, który może służyć do automatyzowania większość zadań kompilacji klienta. Grunt wykorzystuje NPM w celu dostarczania pakietów i funkcje narzędzi integracji z programem Visual Studio. Eksploratora modułu uruchamiającego zadania programu Visual Studio wykrywa zmiany w plikach konfiguracji i oferuje wygodny interfejs do uruchamiania zadań, Wyświetl uruchomione zadania i powiąż zadania ze zdarzeniami w programie Visual Studio.

## <a name="additional-resources"></a>Dodatkowe zasoby

   * [Korzystanie z Gulp](using-gulp.md)
