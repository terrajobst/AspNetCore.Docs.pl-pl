---
title: Korzystanie z Grunt w programie ASP.NET Core
author: rick-anderson
description: ''
ms.author: riande
ms.date: 10/14/2016
uid: client-side/using-grunt
ms.openlocfilehash: fc912974fb6ed3c65bb46a7d616d9e531587d946
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58208882"
---
# <a name="use-grunt-in-aspnet-core"></a>Korzystanie z Grunt w programie ASP.NET Core

Przez [ryżu Noel](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/)

Grunt jest modułu uruchamiającego zadania JavaScript, który automatyzuje minimalizację skryptu, kompilacji TypeScript, narzędzia "lint" jakość kodu, procesory wstępne CSS i niemal dowolnym powtarzających się kwestii, wymagającym, wykonując obsługi opracowywania aplikacji klienta. Grunt jest w pełni obsługiwana w programie Visual Studio, choć szablony projektów programu ASP.NET za pomocą Gulp domyślne (zobacz [Użyj Gulp](using-gulp.md)).

W tym przykładzie używa pusty projekt platformy ASP.NET Core jako punktu wyjścia laboratorium, aby pokazać, jak zautomatyzować proces kompilacji klienta od podstaw.

Zakończono przykład oczyszcza katalog docelowy wdrażania, łączy pliki JavaScript, sprawdza, czy jakość kodu, zmniejsza objętość zawartość pliku JavaScript i wdraża w katalogu głównym aplikacji sieci web. Firma Microsoft użyje następujących pakietów:

* **grunt**: Pakiet modułu uruchamiającego zadania Grunt.

* **grunt-contrib-clean**: Dodatek, który usuwa pliki lub katalogi.

* **grunt-contrib-jshint**: Dodatek, który monitoruje jakość kodu JavaScript.

* **concat — contrib-grunt**: Dodatek, który łączy pliki w jednym pliku.

* **grunt contrib uglify**: Dodatek, który minimalizuje język JavaScript, aby zmniejszyć rozmiar.

* **grunt-contrib-watch**: Dodatek, który obserwuje działań dotyczących plików.

## <a name="preparing-the-application"></a>Przygotowywanie aplikacji

Aby rozpocząć, skonfiguruj nową pustą aplikację i dodać pliki przykładowe TypeScript. Pliki TypeScript automatycznie są kompilowane do kodu JavaScript przy użyciu domyślnych ustawień programu Visual Studio i będzie naszym surowce do przetwarzania, korzystanie z Grunt.

1. W programie Visual Studio Utwórz nowy `ASP.NET Web Application`.

2. W **nowy projekt ASP.NET** okno dialogowe, wybierz pozycję ASP.NET Core **pusty** szablon i kliknij przycisk OK.

3. Sprawdź struktury projektu w Eksploratorze rozwiązań. `\src` Folder zawiera pusty `wwwroot` i `Dependencies` węzłów.

    ![rozwiązanie pusty sieci web](using-grunt/_static/grunt-solution-explorer.png)

4. Dodaj nowy folder o nazwie `TypeScript` do katalogu projektu.

5. Przed dodaniem wszystkie pliki, upewnij się, że program Visual Studio oferuje opcję "Kompiluj przy zapisywaniu" dla plików TypeScript zaznaczone. Przejdź do **narzędzia** > **opcje** > **edytora tekstów** > **Typescript**  >  **Projektu**:

    ![Opcje ustawienia automatycznej kompilacji plików TypeScript](using-grunt/_static/typescript-options.png)

6. Kliknij prawym przyciskiem myszy `TypeScript` katalogu i zaznacz **Dodaj > Nowy element** z menu kontekstowego. Wybierz **plik JavaScript** elementu i nazwij plik *Tastes.ts* (Uwaga \*rozszerzenia TS). Skopiuj wiersz kodu TypeScript poniżej do pliku (podczas zapisywania, nowy *Tastes.js* plik pojawi się ze źródłem JavaScript).

    ```typescript
    enum Tastes { Sweet, Sour, Salty, Bitter }
    ```

7. Dodaj drugi plik do **TypeScript** katalogu i nadaj mu nazwę `Food.ts`. Skopiuj poniższy kod do pliku.

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

## <a name="configuring-npm"></a>Konfigurowanie Menedżera NPM

Skonfiguruj, aby pobrać grunt i grunt zadania programu NPM.

1. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj > Nowy element** z menu kontekstowego. Wybierz **pliku konfiguracyjnego NPM** , należy pozostawić nazwę domyślną *package.json*i kliknij przycisk **Dodaj** przycisku.

2. W *package.json* pliku wewnątrz `devDependencies` obiektu nawiasów klamrowych, wprowadź "grunt". Wybierz `grunt` z funkcji Intellisense listy, a następnie naciśnij klawisz Enter. Visual Studio zacytować nazwę pakietu grunt i Dodaj dwukropka. Po prawej stronie dwukropek, wybierz najnowszą stabilną wersję pakietu z góry na liście funkcji Intellisense (naciśnij klawisz `Ctrl-Space` Intellisense, nie są wyświetlane).

    ![grunt Intellisense](using-grunt/_static/devdependencies-grunt.png)

    > [!NOTE]
    > Używa NPM [wersji semantycznej](http://semver.org/) do organizowania zależności. Semantyczne przechowywania wersji, znany także jako SemVer identyfikuje pakiety ze schematu numerowania \<główna >.\< pomocnicza >. \<poprawki >. IntelliSense ułatwia semantycznego versioning przedstawiający kilka typowe opcje. Pierwszy element na liście funkcji Intellisense (0.4.5 w powyższym przykładzie), jest uznawana za stabilną najnowszą wersję pakietu. Symbolu daszka (^) odpowiada najbardziej aktualną wersję główną i tyldy (~) dopasowuje najbardziej aktualną wersję pomocniczą. Zobacz [odwołanie analizatora wersji semver NPM](https://www.npmjs.com/package/semver) rolę przewodnika po pełnej expressivity, która zapewnia SemVer.

3. Dodaj więcej zależności, aby załadować grunt-contrib -\* pakietów dla *czyste*, *jshint*, *concat*, *uglify*i *Obejrzyj* jak pokazano w poniższym przykładzie. Wersje muszą być zgodne w przykładzie.

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

Pakiety dla każdego elementu devDependencies pobierze oraz wszystkie pliki, które wymaga każdego pakietu. Można znaleźć plików pakietu w `node_modules` katalogu, włączając **Pokaż wszystkie pliki** przycisku w Eksploratorze rozwiązań.

![grunt node_modules](using-grunt/_static/node-modules.png)

> [!NOTE]
> Jeśli zachodzi potrzeba, można ręcznie przywrócić zależności w Eksploratorze rozwiązań kliknij prawym przyciskiem myszy `Dependencies\NPM` i wybierając polecenie **przywracania pakietów** opcji menu.

![Przywracanie pakietów](using-grunt/_static/restore-packages.png)

## <a name="configuring-grunt"></a>Konfigurowanie Grunt

Grunt jest konfigurowana przy użyciu manifestu o nazwie *plik Gruntfile.js* który definiuje, ładuje i rejestruje zadania, które mogą być uruchamiane ręcznie lub skonfigurowany do uruchamiania automatycznie na podstawie zdarzeń w programie Visual Studio.

1. Kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj > Nowy element**. Wybierz **plik konfiguracyjny Grunt** opcji, pozostaw nazwę domyślną *plik Gruntfile.js*i kliknij przycisk **Dodaj** przycisku.

   Kod początkowy obejmuje definicji modułu i `grunt.initConfig()` metody. `initConfig()` Służy do ustawiania opcji dla każdego pakietu, a pozostała część modułu zostaną załadowane i zarejestrować zadań.

   ```javascript
   module.exports = function (grunt) {
     grunt.initConfig({
     });
   };
   ```

2. Wewnątrz `initConfig()` metody, dodać opcje dla `clean` zadań, jak pokazano w przykładzie *plik Gruntfile.js* poniżej. Zadanie czysty akceptuje tablicę ciągów katalogów. To zadanie usuwa pliki z wwwroot/lib i usuwa całą/tymczasowego katalogu.

    ```javascript
    module.exports = function (grunt) {
      grunt.initConfig({
        clean: ["wwwroot/lib/*", "temp/"],
      });
    };
    ```

3. Poniżej metody initConfig(), dodaj wywołanie `grunt.loadNpmTasks()`. Dzięki temu zadania możliwe do uruchomienia w programie Visual Studio.

    ```javascript
    grunt.loadNpmTasks("grunt-contrib-clean");
    ```

4. Zapisz *plik Gruntfile.js*. Plik powinien wyglądać podobnie jak na poniższym zrzucie ekranu.

    ![początkowa gruntfile](using-grunt/_static/gruntfile-js-initial.png)

5. Kliknij prawym przyciskiem myszy *plik Gruntfile.js* i wybierz **Eksplorator modułu uruchamiającego zadania** z menu kontekstowego. Zostanie otwarte okno Eksplorator modułu uruchamiającego zadania.

    ![menu Eksploratora modułu uruchamiającego zadania](using-grunt/_static/task-runner-explorer-menu.png)

6. Upewnij się, że `clean` pokazuje, w obszarze **zadania** w Eksplorator modułu uruchamiającego zadania.

    ![Lista zadań Eksploratora modułu uruchamiającego zadania](using-grunt/_static/task-runner-explorer-tasks.png)

7. Kliknij prawym przyciskiem myszy zadanie czysty, a następnie wybierz pozycję **Uruchom** z menu kontekstowego. Okno polecenia wyświetla postęp zadania.

    ![zadanie czysty Uruchom Eksploratora modułu uruchamiającego zadania](using-grunt/_static/task-runner-explorer-run-clean.png)

    > [!NOTE]
    > Brak plików i katalogów, aby wyczyścić jeszcze. Jeśli chcesz możesz ręcznie utworzyć je w Eksploratorze rozwiązań i następnie uruchom zadanie czysty jako test.

8. W metodzie initConfig() Dodaj wpis dla `concat` przy użyciu kodu poniżej.

    `src` Tablicy właściwości zawiera listę plików, połączyć w kolejności, powinny być połączone. `dest` Właściwość przypisuje ścieżce do połączonego pliku, który jest generowany.

    ```javascript
    concat: {
      all: {
        src: ['TypeScript/Tastes.js', 'TypeScript/Food.js'],
        dest: 'temp/combined.js'
      }
    },
    ```

    > [!NOTE]
    > `all` Właściwość w powyższym kodzie jest nazwą obiektu docelowego. Obiekty docelowe są używane w niektórych zadań Grunt do wielu środowisk kompilacji. Możesz wyświetlić wbudowane obiekty docelowe za pomocą technologii Intellisense lub przypisać własne.

9. Dodaj `jshint` zadań przy użyciu kodu poniżej.

    Narzędzie jakości kodu jshint jest wykonywany dla każdego pliku JavaScript, znajduje się w katalogu temp.

    ```javascript
    jshint: {
      files: ['temp/*.js'],
      options: {
        '-W069': false,
      }
    },
    ```

    > [!NOTE]
    > Opcja "-W069" jest błąd generowany przez jshint podczas używa języka JavaScript dopasowywanie składni, aby przypisać właściwość zamiast notacji z kropką, czyli `Tastes["Sweet"]` zamiast `Tastes.Sweet`. Opcja wyłącza ostrzeżenie, aby umożliwić resztą procesu, aby kontynuować.

10. Dodaj `uglify` zadań przy użyciu kodu poniżej.

    Zadanie minimalizuje *combined.js* plików znajduje się w katalogu temp i tworzy plik wyników w wwwroot/lib zgodnie ze standardową konwencją nazewnictwa  *\<nazwy pliku\>. min.js*.

    ```javascript
    uglify: {
     all: {
       src: ['temp/combined.js'],
       dest: 'wwwroot/lib/combined.min.js'
     }
    },
    ```

11. W obszarze grunt.loadNpmTasks() wywołań, który ładuje, wyczyść contrib grunt należy uwzględnić to samo wywołanie dla jshint, concat i uglify przy użyciu poniższego kodu.

    ```javascript
    grunt.loadNpmTasks('grunt-contrib-jshint');
    grunt.loadNpmTasks('grunt-contrib-concat');
    grunt.loadNpmTasks('grunt-contrib-uglify');
    ```

12. Zapisz *plik Gruntfile.js*. Plik powinien wyglądać podobnie do poniższego przykładu.

    ![przykład pliku pełną grunt](using-grunt/_static/gruntfile-js-complete.png)

13. Należy zauważyć, że lista zadań Eksploratora modułu uruchamiającego zadania zawiera `clean`, `concat`, `jshint` i `uglify` zadania. Każde zadanie podrzędne są uruchamiane w kolejności i obserwują rezultaty w Eksploratorze rozwiązań. Każde zadanie powinno działać bez błędów.

    ![Eksplorator modułu uruchamiającego zadania, każde zadanie podrzędne uruchamiania](using-grunt/_static/task-runner-explorer-run-each-task.png)

    Zadanie concat tworzy nową *combined.js* pliku i umieszcza go w katalogu tymczasowego. Zadanie jshint po prostu działa i nie generuje danych wyjściowych. Zadanie uglify tworzy nową *combined.min.js* pliku i umieszcza go w wwwroot/lib. Po zakończeniu rozwiązania powinien wyglądać podobnie jak na poniższym zrzucie ekranu:

    ![Eksplorator rozwiązań po wszystkich zadań.](using-grunt/_static/solution-explorer-after-all-tasks.png)

    > [!NOTE]
    > Aby uzyskać więcej informacji na temat opcji dla każdego pakietu, odwiedź stronę [ https://www.npmjs.com/ ](https://www.npmjs.com/) i wyszukiwania nazwy pakietu, w polu wyszukiwania na stronie głównej. Na przykład możesz wyszukać wyczyścić contrib grunt pakiet do pobrania link do dokumentacji, który objaśnia, wszystkie jego parametry.

### <a name="all-together-now"></a>Wszystko w jednym miejscu

Korzystanie z Grunt `registerTask()` metodę, aby uruchomić szereg zadań w określonej kolejności. Na przykład, aby uruchomić przykład powyższe kroki w kolejności clean -> concat -> jshint -> uglify, Dodaj poniższy kod do modułu. Kod należy dodać do tego samego poziomu wywołania metody loadNpmTasks() poza initConfig.

```javascript
grunt.registerTask("all", ['clean', 'concat', 'jshint', 'uglify']);
```

Nowe zadanie pojawia się w Eksplorator modułu uruchamiającego zadania w obszarze zadania aliasów. Można prawym przyciskiem myszy, a następnie uruchom go, tak samo jak inne zadania. `all` Zadanie zostanie uruchomione `clean`, `concat`, `jshint` i `uglify`, w kolejności.

![alias grunt zadania](using-grunt/_static/alias-tasks.png)

## <a name="watching-for-changes"></a>Obserwowanie zmian

A `watch` zadań przechowuje list plików i katalogów. Obejrzyj automatycznie wyzwala zadania, jeśli wykryje zmiany. Dodaj poniższy kod do initConfig, aby obejrzeć zmiany \*pliki .js w katalogu TypeScript. Po zmianie pliku JavaScript `watch` uruchomi `all` zadania.

```javascript
watch: {
  files: ["TypeScript/*.js"],
  tasks: ["all"]
}
```

Dodaj wywołanie do `loadNpmTasks()` pokazanie `watch` zadania w Eksplorator modułu uruchamiającego zadania.

```javascript
grunt.loadNpmTasks('grunt-contrib-watch');
```

Kliknij prawym przyciskiem myszy zadanie Obejrzyj Eksplorator modułu uruchamiającego zadania, a następnie wybierz polecenie Uruchom z menu kontekstowego. W oknie polecenia, pokazujący zadania Obejrzyj działającego będą wyświetlane "Oczekiwanie..." Komunikat. Otwórz jeden z plików TypeScript, Dodaj spację, a następnie zapisz plik. Spowoduje to wyzwalanie zadań wyrażenie kontrolne i wyzwalać inne zadania są uruchamiane w kolejności. Poniższy zrzut ekranu przedstawia Uruchom przykład.

![Uruchamianie zadań w danych wyjściowych](using-grunt/_static/watch-running.png)

## <a name="binding-to-visual-studio-events"></a>Powiązanie z zdarzenia programu Visual Studio

Jeśli nie chcesz ręcznie uruchomić zadania za każdym razem, gdy pracujesz w programie Visual Studio, można powiązać zadań **przed kompilacji**, **po kompilacji**, **czysty**, i  **Projekt Open** zdarzenia.

Powiążmy `watch` tak, aby była uruchamiana za każdym razem, gdy zostanie otwarty program Visual Studio. W Eksplorator modułu uruchamiającego zadania, kliknij prawym przyciskiem myszy zadanie wyrażenie kontrolne, a następnie wybierz **powiązania > Otwórz projekt** z menu kontekstowego.

![Powiąż zadania do otwarcia projektu](using-grunt/_static/bindings-project-open.png)

Zwolnij i ponownie Załaduj projekt. Gdy projekt ładuje się ponownie, zadanie Obejrzyj zostanie uruchomione automatycznie.

## <a name="summary"></a>Podsumowanie

Grunt jest modułu uruchamiającego zadania zaawansowane, który może służyć do zautomatyzowania większości zadań kompilacji klienta. Grunt korzysta z programu NPM dostarczanie pakietów i funkcje narzędzi integracji z programem Visual Studio. Eksplorator modułu uruchamiającego zadania programu Visual Studio wykrywa zmiany w plikach konfiguracji i oferuje wygodny interfejs do uruchamiania zadań, Wyświetl uruchomione zadania podrzędne i powiąż zadania ze zdarzenia programu Visual Studio.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Korzystanie z Gulp](using-gulp.md)
