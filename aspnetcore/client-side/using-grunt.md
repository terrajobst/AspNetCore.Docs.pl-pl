---
title: Używanie grunt w ASP.NET Core
author: rick-anderson
description: Używanie grunt w ASP.NET Core
ms.author: riande
ms.date: 12/05/2019
uid: client-side/using-grunt
ms.openlocfilehash: e516b85da7e94d0c93be642086fede0a11fea3c2
ms.sourcegitcommit: c0b72b344dadea835b0e7943c52463f13ab98dd1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/06/2019
ms.locfileid: "74879791"
---
# <a name="use-grunt-in-aspnet-core"></a>Używanie grunt w ASP.NET Core

Grunt to moduł uruchamiający zadania języka JavaScript, który automatyzuje skrypty minifikacja, kompilacja języka TypeScript, jakość kodu "lint", a także wszystkie powtarzalne zadania, które wymagają obsługi rozwoju klienta. Grunt jest w pełni obsługiwany w programie Visual Studio.

Ten przykład używa pustego projektu ASP.NET Core jako punktu początkowego, aby pokazać, jak zautomatyzować proces kompilacji klienta od podstaw.

Zakończono przykład czyści docelowy katalog wdrożenia, łączy pliki JavaScript, sprawdza jakość kodu, skrapla zawartość pliku JavaScript i wdraża je w katalogu głównym aplikacji sieci Web. Będziemy używać następujących pakietów:

* **grunt**: pakiet Runner zadania grunt.

* **grunt-contrib-Clean**: wtyczka, która usuwa pliki lub katalogi.

* **grunt-contrib-jshint**: wtyczka, która przegląda jakość kodu JavaScript.

* **grunt-contrib-concat**: wtyczka, która przyłącza pliki do pojedynczego pliku.

* **grunt-contrib-uglify**: wtyczka, która minifies JavaScript, aby zmniejszyć rozmiar.

* **grunt-contrib-Watch**: wtyczka, która obserwuje aktywność pliku.

## <a name="preparing-the-application"></a>Przygotowywanie aplikacji

Aby rozpocząć, skonfiguruj nową pustą aplikację sieci Web i Dodaj przykładowe pliki języka TypeScript. Pliki TypeScript są automatycznie kompilowane do języka JavaScript przy użyciu domyślnych ustawień programu Visual Studio i będą nasze surowy materiał do przetwarzania za pomocą grunt.

1. W programie Visual Studio Utwórz nowy `ASP.NET Web Application`.

2. W oknie dialogowym **Nowy projekt ASP.NET** wybierz ASP.NET Core **pusty** szablon i kliknij przycisk OK.

3. W Eksplorator rozwiązań Przejrzyj strukturę projektu. Folder `\src` zawiera puste węzły `wwwroot` i `Dependencies`.

    ![puste rozwiązanie sieci Web](using-grunt/_static/grunt-solution-explorer.png)

4. Dodaj nowy folder o nazwie `TypeScript` do katalogu projektu.

5. Przed dodaniem jakichkolwiek plików upewnij się, że program Visual Studio ma zaznaczoną opcję "Kompiluj przy zapisywaniu" dla plików TypeScript. Przejdź do **opcji** **narzędzia** > opcje > **edytor tekstów** > języku **TypeScript** > **projekt**:

    ![Opcje ustawiania opcji autokompilowania plików TypeScript](using-grunt/_static/typescript-options.png)

6. Kliknij prawym przyciskiem myszy katalog `TypeScript` i wybierz polecenie **dodaj > nowy element** z menu kontekstowego. Wybierz element **pliku JavaScript** i Nazwij plik *smaku. TS* (zanotuj \*rozszerzenie TS). Skopiuj wiersz kodu języka TypeScript poniżej do pliku (podczas zapisywania zostanie wyświetlony nowy plik *smak. js* ze źródłem JavaScript).

    ```typescript
    enum Tastes { Sweet, Sour, Salty, Bitter }
    ```

7. Dodaj drugi plik do katalogu **TypeScript** i nadaj mu nazwę `Food.ts`. Skopiuj poniższy kod do pliku.

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

## <a name="configuring-npm"></a>Konfigurowanie NPM

Następnie skonfiguruj NPM do pobierania grunt i grunt-Tasks.

1. W Eksplorator rozwiązań kliknij prawym przyciskiem myszy projekt i wybierz polecenie **dodaj > nowy element** z menu kontekstowego. Wybierz element **pliku konfiguracji npm** , pozostaw nazwę domyślną, *Package. JSON*, a następnie kliknij przycisk **Dodaj** .

2. W pliku *Package. JSON* w nawiasach klamrowych obiektu `devDependencies` wprowadź wartość "Grunt". Wybierz pozycję `grunt` z listy IntelliSense i naciśnij klawisz ENTER. Program Visual Studio zwróci nazwę pakietu grunt i doda dwukropek. Z prawej strony dwukropek wybierz najnowszą stabilną wersję pakietu z góry listy IntelliSense (naciśnij `Ctrl-Space`, jeśli IntelliSense nie pojawia się).

    ![grunt IntelliSense](using-grunt/_static/devdependencies-grunt.png)

    > [!NOTE]
    > NPM używa [semantycznej wersji](https://semver.org/) do organizowania zależności. Wersja semantyczna, znana również jako SemVer, identyfikuje pakiety z schematem numeracji \<głównym >.\<> pomocnicze.\<> poprawki. Technologia IntelliSense upraszcza obsługę wersji semantycznych, pokazując tylko kilka typowych opcji. Górny element na liście IntelliSense (0.4.5 w powyższym przykładzie) jest traktowany jako Najnowsza stabilna wersja pakietu. Symbol karetki (^) jest zgodny z najnowszą wersją główną, a Tylda (~) jest zgodna z najnowszą wersją pomocniczą. Zapoznaj się z informacjami o [analizatorze analizatora wersji npm semver](https://www.npmjs.com/package/semver) jako wskazówką dla expressivity semver.

3. Dodaj więcej zależności, aby załadować pakiety grunt-contrib-\* dla elementów *Clean*, *jshint*, *concat*, *uglify*i *Watch* , jak pokazano w poniższym przykładzie. Wersje nie muszą być zgodne z przykładem.

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

4. Zapisz plik *Package. JSON* .

Pakiety dla każdego elementu `devDependencies` będą pobierane wraz z wszelkimi plikami wymaganymi przez każdy pakiet. Pliki pakietów można znaleźć w katalogu *node_modules* , włączając przycisk **Pokaż wszystkie pliki** w **Eksplorator rozwiązań**.

![grunt node_modules](using-grunt/_static/node-modules.png)

> [!NOTE]
> Jeśli zachodzi taka potrzeba, można ręcznie przywrócić zależności w **Eksplorator rozwiązań** przez kliknięcie prawym przyciskiem myszy `Dependencies\NPM` i wybranie opcji menu **Przywróć pakiety** .

![Przywróć pakiety](using-grunt/_static/restore-packages.png)

## <a name="configuring-grunt"></a>Konfigurowanie grunt

Grunt jest skonfigurowany przy użyciu manifestu o nazwie *Gruntfile. js* , który definiuje, ładuje i rejestruje zadania, które można uruchomić ręcznie lub skonfigurować do automatycznego uruchamiania na podstawie zdarzeń w programie Visual Studio.

1. Kliknij prawym przyciskiem myszy projekt i wybierz polecenie **dodaj** > **nowy element**. Wybierz szablon element **pliku JavaScript** , Zmień nazwę na *Gruntfile. js*, a następnie kliknij przycisk **Dodaj** .

1. Dodaj następujący kod do *Gruntfile. js*. Funkcja `initConfig` ustawia opcje dla każdego pakietu, a pozostała część modułu ładuje i rejestruje zadania.

   ```javascript
   module.exports = function (grunt) {
     grunt.initConfig({
     });
   };
   ```

1. W funkcji `initConfig` Dodaj opcje dla zadania `clean`, jak pokazano w przykładzie *Gruntfile. js* poniżej. Zadanie `clean` akceptuje tablicę ciągów katalogów. To zadanie usuwa pliki z katalogu *wwwroot/lib* i usuwa cały katalog */temp* .

    ```javascript
    module.exports = function (grunt) {
      grunt.initConfig({
        clean: ["wwwroot/lib/*", "temp/"],
      });
    };
    ```

1. Poniżej funkcji `initConfig` Dodaj wywołanie do `grunt.loadNpmTasks`. Spowoduje to możliwy do uruchomienia zadania z programu Visual Studio.

    ```javascript
    grunt.loadNpmTasks("grunt-contrib-clean");
    ```

1. Zapisz *Gruntfile. js*. Plik powinien wyglądać podobnie do poniższego zrzutu ekranu.

    ![początkowy gruntfile](using-grunt/_static/gruntfile-js-initial.png)

1. Kliknij prawym przyciskiem myszy pozycję *Gruntfile. js* i wybierz pozycję **Eksplorator modułu uruchamiającego zadania** z menu kontekstowego. Zostanie otwarte okno **Eksplorator Runner zadań** .

    ![menu Eksploratora modułu uruchamiającego zadania](using-grunt/_static/task-runner-explorer-menu.png)

1. Sprawdź, czy `clean` są wyświetlane w obszarze **zadania** w **Eksploratorze modułu uruchamiającego zadania**.

    ![Lista zadań Eksploratora modułu uruchamiającego zadania](using-grunt/_static/task-runner-explorer-tasks.png)

1. Kliknij prawym przyciskiem myszy zadanie czyste i wybierz polecenie **Uruchom** z menu kontekstowego. W oknie polecenia zostanie wyświetlony postęp zadania.

    ![Eksplorator modułu uruchamiającego zadania — uruchamianie czystego zadania](using-grunt/_static/task-runner-explorer-run-clean.png)

    > [!NOTE]
    > Nie ma jeszcze plików lub katalogów do czyszczenia. Jeśli chcesz, możesz je ręcznie utworzyć w Eksplorator rozwiązań a następnie uruchomić czyste zadanie jako test.

1. W funkcji `initConfig` Dodaj wpis dla `concat` przy użyciu poniższego kodu.

    Tablica właściwości `src` zawiera listę plików do połączenia w kolejności, w jakiej powinny być połączone. Właściwość `dest` przypisuje ścieżkę do połączonego pliku, który został utworzony.

    ```javascript
    concat: {
      all: {
        src: ['TypeScript/Tastes.js', 'TypeScript/Food.js'],
        dest: 'temp/combined.js'
      }
    },
    ```

    > [!NOTE]
    > Właściwość `all` w powyższym kodzie jest nazwą obiektu docelowego. Elementy docelowe są używane w niektórych zadaniach grunt, aby umożliwić wiele środowisk kompilacji. Możesz wyświetlić wbudowane elementy docelowe przy użyciu funkcji IntelliSense lub przypisać własne.

1. Dodaj `jshint` zadanie przy użyciu poniższego kodu.

    Narzędzie jshint `code-quality` jest uruchamiane dla każdego pliku JavaScript znajdującego się w katalogu *temp* .

    ```javascript
    jshint: {
      files: ['temp/*.js'],
      options: {
        '-W069': false,
      }
    },
    ```

    > [!NOTE]
    > Opcja "-W069" jest błędem wytwarzanym przez jshint, gdy język JavaScript używa składni nawiasu do przypisywania właściwości zamiast notacji kropkowej, np. `Tastes["Sweet"]`, a nie `Tastes.Sweet`. Opcja wyłącza ostrzeżenie, aby umożliwić kontynuowanie pozostałej części procesu.

1. Dodaj `uglify` zadanie przy użyciu poniższego kodu.

    Zadanie minifies *połączonego pliku. js* znajdującego się w katalogu Temp i tworzy plik wynikowy w pliku wwwroot/lib zgodnie ze standardową konwencją nazewnictwa *\<nazwą pliku\>. min. js*.

    ```javascript
    uglify: {
     all: {
       src: ['temp/combined.js'],
       dest: 'wwwroot/lib/combined.min.js'
     }
    },
    ```

1. W obszarze wywołanie do `grunt.loadNpmTasks`, które ładuje `grunt-contrib-clean`, Uwzględnij to samo wywołanie dla jshint, concat i uglify przy użyciu poniższego kodu.

    ```javascript
    grunt.loadNpmTasks('grunt-contrib-jshint');
    grunt.loadNpmTasks('grunt-contrib-concat');
    grunt.loadNpmTasks('grunt-contrib-uglify');
    ```

1. Zapisz *Gruntfile. js*. Plik powinien wyglądać podobnie do poniższego przykładu.

    ![Zakończono przykład pliku grunt](using-grunt/_static/gruntfile-js-complete.png)

1. Należy zauważyć, że lista zadań **Eksploratora modułu uruchamiającego zadania** zawiera `clean`, `concat`, `jshint` i `uglify` zadań. Uruchom każde zadanie w kolejności i obserwuj wyniki w **Eksplorator rozwiązań**. Każde zadanie powinno być uruchamiane bez błędów.

    ![Eksplorator modułu uruchamiającego zadania uruchamia każde zadanie](using-grunt/_static/task-runner-explorer-run-each-task.png)

    Zadanie concat tworzy nowy połączony plik *. js* i umieszcza go w katalogu Temp. Zadanie `jshint` po prostu zostanie uruchomione i nie wygenerowało danych wyjściowych. Zadanie `uglify` tworzy nowy połączony plik *. min. js* i umieszcza go w pliku *wwwroot/lib*. Po zakończeniu rozwiązanie powinno wyglądać podobnie do poniższego zrzutu ekranu:

    ![Eksplorator rozwiązań po wszystkich zadaniach](using-grunt/_static/solution-explorer-after-all-tasks.png)

    > [!NOTE]
    > Aby uzyskać więcej informacji na temat opcji dla każdego pakietu, odwiedź stronę [https://www.npmjs.com/](https://www.npmjs.com/) i wyszukaj nazwę pakietu w polu wyszukiwania na stronie głównej. Na przykład można wyszukać pakiet grunt-contrib-Clean w celu uzyskania linku do dokumentacji, który objaśnia wszystkie jego parametry.

### <a name="all-together-now"></a>Wszystko jest teraz na miejscu

Użyj metody grunt `registerTask()`, aby uruchomić serię zadań w określonej kolejności. Na przykład aby uruchomić przykładowe kroki opisane powyżej w kolejności > concat > jshint-> uglify, Dodaj poniższy kod do modułu. Kod powinien zostać dodany do tego samego poziomu, co wywołania loadNpmTasks (), poza initConfig.

```javascript
grunt.registerTask("all", ['clean', 'concat', 'jshint', 'uglify']);
```

Nowe zadanie zostanie wyświetlone w Eksploratorze modułu uruchamiającego zadania w obszarze zadania aliasu. Możesz kliknąć prawym przyciskiem myszy i uruchomić go tak samo jak w przypadku innych zadań. Zadanie `all` zostanie uruchomione `clean`, `concat`, `jshint` i `uglify`w pożądanej kolejności.

![grunt zadania aliasu](using-grunt/_static/alias-tasks.png)

## <a name="watching-for-changes"></a>Obserwowane zmiany

Zadanie `watch` pozwala na zachowanie oczu na plikach i katalogach. Czujka wyzwala zadania automatycznie w przypadku wykrycia zmian. Dodaj poniższy kod do initConfig, aby obejrzeć zmiany w plikach \*. js w katalogu TypeScript. Jeśli plik JavaScript zostanie zmieniony, `watch` uruchomi `all` zadanie.

```javascript
watch: {
  files: ["TypeScript/*.js"],
  tasks: ["all"]
}
```

Dodaj wywołanie do `loadNpmTasks()`, aby wyświetlić `watch` zadanie w Eksploratorze modułu uruchamiającego zadania.

```javascript
grunt.loadNpmTasks('grunt-contrib-watch');
```

Kliknij prawym przyciskiem myszy zadanie Obejrzyj w Eksploratorze modułu uruchamiającego zadania i wybierz polecenie Uruchom z menu kontekstowego. W oknie wiersza polecenia, w którym jest uruchomione zadanie Obserwuj, zostanie wyświetlony element "oczekiwanie..." Komunikat. Otwórz jeden z plików TypeScript, Dodaj spację, a następnie Zapisz plik. Spowoduje to wyzwolenie zadania czujki i wyzwolenie innych zadań do uruchomienia w pożądanej kolejności. Poniższy zrzut ekranu przedstawia przykład uruchomienia.

![uruchomione zadania wyjściowe](using-grunt/_static/watch-running.png)

## <a name="binding-to-visual-studio-events"></a>Powiązanie ze zdarzeniami programu Visual Studio

Jeśli nie chcesz ręcznie uruchamiać zadań za każdym razem, gdy Pracujesz w programie Visual Studio, powiąż zadania z **przed kompilacją**, **po kompilacji**, **czyszczeniu**i otwartych zdarzeniach **projektu** .

Powiąż `watch` tak, aby były uruchamiane za każdym razem, gdy zostanie otwarty program Visual Studio. W Eksploratorze modułu uruchamiającego zadania kliknij prawym przyciskiem myszy zadanie Obejrzyj i wybierz **powiązania** > **projekcie otwarty** z menu kontekstowego.

![Powiąż zadanie z otwierającym projektu](using-grunt/_static/bindings-project-open.png)

Zwolnij i Załaduj ponownie projekt. Po ponownym załadowaniu projektu zadanie czujki rozpocznie się automatycznie.

## <a name="summary"></a>Podsumowanie

Grunt to zaawansowany moduł uruchamiający zadania, którego można użyć do zautomatyzowania większości zadań kompilacji klienta. Grunt wykorzystuje NPM do dostarczania pakietów i narzędzi integracji z programem Visual Studio. Eksplorator modułu uruchamiającego zadania programu Visual Studio wykrywa zmiany w plikach konfiguracji i udostępnia wygodny interfejs do uruchamiania zadań, wyświetlania uruchomionych zadań i powiązania zadań z zdarzeniami programu Visual Studio.
