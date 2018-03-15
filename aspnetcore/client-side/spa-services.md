---
title: "Umożliwia tworzenie aplikacji jednostronicowej w platformy ASP.NET Core JavaScriptServices"
author: scottaddie
description: "Poznaj korzyści wynikające ze stosowania JavaScriptServices do utworzenia jednej strony aplikacji JEDNOSTRONICOWEJ obsługiwana przez platformy ASP.NET Core."
manager: wpickett
ms.author: scaddie
ms.custom: H1Hack27Feb2017
ms.date: 08/02/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: client-side/spa-services
ms.openlocfilehash: c962fc160cf39ad1c69f4269616c993fde420035
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/15/2018
---
# <a name="use-javascriptservices-to-create-single-page-applications-in-aspnet-core"></a>Umożliwia tworzenie aplikacji jednostronicowej w platformy ASP.NET Core JavaScriptServices

Przez [Scott Addie](https://github.com/scottaddie) i [Fiyaz Hasan](http://fiyazhasan.me/)

Jednej strony aplikacji (SPA) jest typem popularnych aplikacji sieci web z powodu jego związanego z używaniem zaawansowanego środowiska użytkownika. Integrowanie struktur SPA po stronie klienta lub biblioteki, takich jak [kątową](https://angular.io/) lub [React](https://facebook.github.io/react/), z platform po stronie serwera, takich jak ASP.NET Core mogą być trudne. [JavaScriptServices](https://github.com/aspnet/JavaScriptServices) został opracowany, aby zmniejszyć tarcia w procesie integracji. Umożliwia bezproblemowe operacji między innego klienta i serwera technologii stosów.

[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/spa-services/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

<a name="what-is-js-services"></a>

## <a name="what-is-javascriptservices"></a>Co to jest JavaScriptServices?

JavaScriptServices jest kolekcja technologii po stronie klienta dla platformy ASP.NET Core. Jego celem jest pozwala platformy ASP.NET Core jako deweloperów preferowaną platformę po stronie serwera do tworzenia źródła.

JavaScriptServices składa się z trzech różnych pakietów NuGet:
* [Microsoft.AspNetCore.NodeServices](https://www.nuget.org/packages/Microsoft.AspNetCore.NodeServices/) (NodeServices)
* [Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) (SpaServices)
* [Microsoft.AspNetCore.SpaTemplates](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaTemplates/) (SpaTemplates)

Te pakiety są przydatne w przypadku możesz:
* Uruchom JavaScript na serwerze
* Użyj SPA framework lub biblioteki
* Tworzenie zasobów po stronie klienta z Webpack

Większość fokus w tym artykule jest umieszczona na przy użyciu pakietu SpaServices.

<a name="what-is-spa-services"></a>

## <a name="what-is-spaservices"></a>Co to jest SpaServices?

SpaServices utworzono pozwala platformy ASP.NET Core jako deweloperów preferowaną platformę po stronie serwera do tworzenia źródła. SpaServices nie jest wymagane, aby opracować źródła z platformy ASP.NET Core, a nie blokuje w ramach określonego klienta.

SpaServices zapewnia przydatne infrastruktury, takich jak:
* [Prerendering po stronie serwera](#server-prerendering)
* [Oprogramowanie pośredniczące deweloperów Webpack](#webpack-dev-middleware)
* [Moduł dynamicznej wymiany](#hot-module-replacement)
* [Pomocnicy routingu](#routing-helpers)

Zbiorczo te składniki infrastruktury zwiększenia zarówno przepływu pracy programowania i obsługi środowiska uruchomieniowego. Składniki może przyjąć pojedynczo.

<a name="spa-services-prereqs"></a>

## <a name="prerequisites-for-using-spaservices"></a>Wymagania wstępne dotyczące korzystania z SpaServices

Aby pracować z SpaServices, należy zainstalować następujące elementy:
* [Node.js](https://nodejs.org/) (w wersji 6 lub nowszej) z pakietu npm
    * Aby sprawdzić te składniki są zainstalowane i można go znaleźć, należy uruchomić następujące polecenie z wiersza polecenia:

    ```console
    node -v && npm -v
    ```

Uwaga: Jeśli wdrażasz do witryny sieci web platformy Azure, nie musisz tutaj wykonywać żadnych czynności &mdash; Node.js jest zainstalowany i dostępny w środowisku serwerów.

* [Zestaw SDK programu .NET core](https://www.microsoft.com/net/download/core) 1.0 (lub nowszy)
    * Jeśli w systemie Windows, to można zainstalować, wybierając w Visual Studio 2017 **aplikacji dla wielu platform .NET Core** obciążenia.

* [Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) NuGet package

<a name="server-prerendering"></a>

## <a name="server-side-prerendering"></a>Prerendering po stronie serwera

Uniwersalnej aplikacji (nazywane również isomorphic) to aplikacja JavaScript może działać zarówno na serwerze i kliencie. Kątową platformy React i innych popularnych struktur podanie tego stylu rozwoju aplikacji platformy uniwersalnej. Koncepcja jest najpierw przetworzyć składników framework na serwerze za pośrednictwem środowiska Node.js, a następnie poprzez delegowanie przekazać dalej wykonywania do klienta.

Platformy ASP.NET Core [pomocników tagów](xref:mvc/views/tag-helpers/intro) dostarczonych przez SpaServices upraszczająca implementację elementów prerendering po stronie serwera za pomocą funkcji JavaScript na serwerze.

### <a name="prerequisites"></a>Wymagania wstępne

Zainstaluj następujące czynności:
* [ASPNET prerendering](https://www.npmjs.com/package/aspnet-prerendering) pakietu npm:

    ```console
    npm i -S aspnet-prerendering
    ```

### <a name="configuration"></a>Konfiguracja

Pomocników tagów zostają wykrywalny przez rejestracji przestrzeni nazw w projekcie *_ViewImports.cshtml* pliku:

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/_ViewImports.cshtml?highlight=3)]

Te pomocników tagów optymalizacji abstrakcyjnej mogli dokładnie zapoznać się z komunikują się bezpośrednio z interfejsów API niskiego poziomu przy użyciu składni notacji języka HTML w widoku Razor:

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=5)]

### <a name="the-asp-prerender-module-tag-helper"></a>`asp-prerender-module` Pomocnika tagów

`asp-prerender-module` Wykonuje pomocnika tagów, używany w poprzednim przykładzie kodu *ClientApp/dist/main-server.js* na serwerze za pośrednictwem środowiska Node.js. Dla jasności *main server.js* plik jest artefaktu zadania transpilation TypeScript i JavaScript w [Webpack](http://webpack.github.io/) proces kompilacji. Webpack definiuje alias punktu wejścia `main-server`; i przechodzenie na wykresie zależności dla tego aliasu zaczyna się od *ClientApp/rozruchu server.ts* pliku:

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=53)]

W poniższym przykładzie kątowego *ClientApp/rozruchu server.ts* korzysta z pliku `createServerRenderer` funkcji i `RenderResult` typu `aspnet-prerendering` pakiet npm, aby skonfigurować renderowania serwera za pomocą środowiska Node.js. Kod znaczników HTML, przeznaczonych do renderowania po stronie serwera jest przekazywana do wywołanie funkcji rozpoznawania jest ujęte w języku JavaScript jednoznacznie `Promise` obiektu. `Promise` Znaczenie obiektu to, że asynchronicznie dostarcza kod znaczników HTML strony iniekcji w modelu DOM — symbol zastępczy elementu.

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-34,79-)]

### <a name="the-asp-prerender-data-tag-helper"></a>`asp-prerender-data` Pomocnika tagów

W połączeniu z `asp-prerender-module` pomocnika tagów `asp-prerender-data` pomocnika tagów może służyć do przekazywania informacji kontekstowych z widoku Razor do JavaScript po stronie serwera. Na przykład następujący kod znaczników przekazuje dane użytkownika do `main-server` modułu:

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=9-12)]

Odebranej `UserName` argument jest zserializowanym przy użyciu wbudowanych serializator JSON i są przechowywane w `params.data` obiektu. W poniższym przykładzie kątowego danych jest używany do utworzenia spersonalizowanego pozdrowienia w `h1` elementu:

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,38-52,79-)]

Uwaga: Nazwy właściwości przekazano pomocników tagów są reprezentowane z **PascalCase** notacji. Natomiast który do JavaScript, w których są reprezentowane pod tą samą nazwą właściwości z **(camelcase)**. Domyślna konfiguracja serializacji JSON jest odpowiedzialny za tej różnicy.

Aby rozszerzyć na poprzednim przykładzie kodu, dane mogą być przekazywane z serwera do widoku przez nawilżania `globals` przekazane do właściwości `resolve` funkcji:

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,57-77,79-)]

`postList` Zdefiniowany w tablicy `globals` obiekt jest dołączony do przeglądarki globalne `window` obiektu. Tej zmiennej podnoszenia do globalnego zakresu eliminuje dublowania działań, w szczególności, w odniesieniu do ładowania danych tego samego raz na serwerze i ponownie na kliencie.

![postList — zmienna globalna dołączony do obiektu okna](spa-services/_static/global_variable.png)

<a name="webpack-dev-middleware"></a>

## <a name="webpack-dev-middleware"></a>Oprogramowanie pośredniczące deweloperów Webpack

[Oprogramowanie pośredniczące deweloperów Webpack](https://webpack.github.io/docs/webpack-dev-middleware.html) wprowadza usprawnić programowanie przepływu pracy zgodnie z którymi Webpack kompilacje zasobów na żądanie. Oprogramowanie pośredniczące automatycznie kompiluje i służy zasobów po stronie klienta, gdy strona zostanie ponownie załadowana w przeglądarce. Alternatywne podejście jest ręczne wywoływanie Webpack za pomocą skryptu kompilacji npm projektu po zmianie zależności innych firm lub niestandardowy kod. Npm kompilacji skryptu *package.json* pliku przedstawiono w poniższym przykładzie:

[!code-json[](../client-side/spa-services/sample/SpaServicesSampleApp/package.json?range=5)]

### <a name="prerequisites"></a>Wymagania wstępne

Zainstaluj następujące czynności:
* [ASPNET webpack](https://www.npmjs.com/package/aspnet-webpack) pakietu npm:

    ```console
    npm i -D aspnet-webpack
    ```

### <a name="configuration"></a>Konfiguracja

Oprogramowanie pośredniczące deweloperów Webpack został zarejestrowany z potokiem żądań HTTP za pośrednictwem następujący kod w *Startup.cs* pliku `Configure` metody:

[!code-csharp[](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=webpack-middleware-registration&highlight=4)]

`UseWebpackDevMiddleware` — Metoda rozszerzenia musi zostać wywołana przed [rejestrowanie obsługi plików statycznych](xref:fundamentals/static-files) za pośrednictwem `UseStaticFiles` — metoda rozszerzenia. Ze względów bezpieczeństwa należy zarejestrować oprogramowanie pośredniczące tylko wtedy, gdy aplikacja jest uruchamiana w trybie projektowania.

*Webpack.config.js* pliku `output.publicPath` właściwości informuje oprogramowanie pośredniczące, które należy obserwować `dist` folderu zmiany:

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,13-16)]

<a name="hot-module-replacement"></a>

## <a name="hot-module-replacement"></a>Moduł dynamicznej wymiany

Pomyśl o jego Webpack [dynamicznej wymiany modułu](https://webpack.github.io/docs/hot-module-replacement-with-webpack.html) funkcji (HMR) jako rozwoju [oprogramowanie pośredniczące deweloperów Webpack](#webpack-dev-middleware). HMR wprowadzono takich samych korzyści, ale dodatkowo upraszcza przepływu pracy programowania przez automatyczne aktualizowanie zawartości strony po kompilowanie zmiany. Nie pomyl go z odświeżenia przeglądarki, która będzie zakłócać bieżący stan w pamięci i sesji debugowania aplikacja JEDNOSTRONICOWA. Istnieje aktywne łącze między usługą oprogramowanie pośredniczące deweloperów Webpack i przeglądarki, co oznacza, że zmiany są przenoszone do przeglądarki.

### <a name="prerequisites"></a>Wymagania wstępne

Zainstaluj następujące czynności:
* [webpack-hot — oprogramowanie pośredniczące](https://www.npmjs.com/package/webpack-hot-middleware) pakietu npm:

    ```console
    npm i -D webpack-hot-middleware
    ```

### <a name="configuration"></a>Konfiguracja

Składnik HMR muszą być rejestrowane w MVC Potok żądań HTTP w `Configure` metody:

```csharp
app.UseWebpackDevMiddleware(new WebpackDevMiddlewareOptions {
    HotModuleReplacement = true
});
```

Tak jak została wartość true z [oprogramowanie pośredniczące deweloperów Webpack](#webpack-dev-middleware), `UseWebpackDevMiddleware` — metoda rozszerzenia musi zostać wywołana przed `UseStaticFiles` — metoda rozszerzenia. Ze względów bezpieczeństwa należy zarejestrować oprogramowanie pośredniczące tylko wtedy, gdy aplikacja jest uruchamiana w trybie projektowania.

*Webpack.config.js* muszą być zdefiniowane w pliku `plugins` tablicy nawet wtedy, gdy zostanie pozostawiony pusty:

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,25)]

Po załadowaniu aplikacji w przeglądarce, karta konsoli narzędzi deweloperskich zawiera potwierdzenie HMR aktywacji:

![Gorących komunikat połączonych zastąpienie modułu](spa-services/_static/hmr_connected.png)

<a name="routing-helpers"></a>

## <a name="routing-helpers"></a>Pomocnicy routingu

W większości na podstawie platformy ASP.NET Core źródła należy po stronie klienta routingu oprócz routingu po stronie serwera. Systemy routingu SPA i MVC może pracować wielel osób bez zakłóceń. Brak, jednak jeden krawędzi wielkość stwarza żąda: Identyfikowanie odpowiedzi HTTP 404.

Rozważmy scenariusz, w którym bez rozszerzenia trasa `/some/page` jest używany. Załóżmy, żądanie nie-dopasowania wzorca trasy po stronie serwera, ale jego wzorzec pasuje do trasy po stronie klienta. Teraz Rozważmy przychodzącego żądania dla `/images/user-512.png`, która oczekuje zwykle można znaleźć pliku obrazu na serwerze. Jeśli trasie po stronie serwera lub pliku statycznego że ścieżka do żądanego zasobu nie jest zgodny, jest mało prawdopodobne, czy aplikacja kliencka będzie jego obsługa — zwykle chcesz zwracać kod stanu HTTP 404.

### <a name="prerequisites"></a>Wymagania wstępne

Zainstaluj następujące czynności:
* Po stronie klienta routingu pakiet npm. Na przykład za pomocą kątową:

    ```console
    npm i -S @angular/router
    ```

### <a name="configuration"></a>Konfiguracja

Metody rozszerzenia o nazwie `MapSpaFallbackRoute` jest używany w `Configure` metody:

[!code-csharp[](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=mvc-routing-table&highlight=7-9)]

Porada: Trasy są oceniane w kolejności, w którym jest skonfigurowane. W rezultacie `default` trasy w poprzednim przykładzie kodu jest używana jako pierwsza dopasowywanie do wzorców.

<a name="new-project-creation"></a>

## <a name="creating-a-new-project"></a>Tworzenie nowego projektu

JavaScriptServices zawiera szablony wstępnie skonfigurowanej aplikacji. SpaServices jest używany w tych szablonów w połączeniu z różnych struktury i biblioteki, takich jak kątową, Aurelia odcinania, platformy React i Vue.

Szablony te można zainstalować za pomocą interfejsu wiersza polecenia platformy .NET Core, uruchamiając następujące polecenie:

```console
dotnet new --install Microsoft.AspNetCore.SpaTemplates::*
```

Zostanie wyświetlona lista dostępnych szablonów SPA:

| Szablony                                 | Krótka nazwa | Język | Znaczniki        |
|:------------------------------------------|:-----------|:---------|:------------|
| MVC ASP.NET Core z dyrektywy Angular             | dyrektywy angular    | [C#]     | Web/MVC/SPA |
| MVC ASP.NET Core z Aurelia             | aurelia    | [C#]     | Web/MVC/SPA |
| MVC ASP.NET Core z Knockout.js         | Odcinania   | [C#]     | Web/MVC/SPA |
| MVC ASP.NET Core z React.js            | Reakcji      | [C#]     | Web/MVC/SPA |
| MVC ASP.NET Core z React.js i Redux  | reactredux | [C#]     | Web/MVC/SPA |
| MVC ASP.NET Core z Vue.js              | VUE        | [C#]     | Web/MVC/SPA | 

Aby utworzyć nowy projekt za pomocą jednego z szablonów SPA, dołącz **krótką nazwę** szablonu w [dotnet nowe](/dotnet/core/tools/dotnet-new) polecenia. Poniższe polecenie tworzy aplikację kątowego z platformą ASP.NET MVC Core skonfigurowany po stronie serwera:

```console
dotnet new angular
```

<a name="runtime-config-mode"></a>

### <a name="set-the-runtime-configuration-mode"></a>Ustaw tryb konfiguracji środowiska wykonawczego

Istnieją dwa tryby konfiguracji podstawowego środowiska wykonawczego:
* **Programowanie**:
    * Obejmuje map źródła do jej obsługi ułatwiają debugowania.
    * Nie Optymalizuj kod wydajności po stronie klienta.
* **Produkcji**:
    * Wyklucza mapy źródła.
    * Optymalizuje kod po stronie klienta za pośrednictwem tworzenie pakietów i minimalizowanie.

Zmienna środowiskowa o nazwie korzysta z platformy ASP.NET Core `ASPNETCORE_ENVIRONMENT` do przechowywania trybu konfiguracji. Zobacz  **[konfiguracji środowiska](xref:fundamentals/environments#setting-the-environment)**  Aby uzyskać więcej informacji.

### <a name="running-with-net-core-cli"></a>Uruchomione z platformą .NET Core interfejsu wiersza polecenia

Przywróć wymagane NuGet i pakietów npm, uruchamiając następujące polecenie w katalogu głównym projektu:

```console
dotnet restore && npm i
```

Tworzenie i uruchamianie aplikacji:

```console
dotnet run
```

Na hoście lokalnym zgodnie z uruchomieniem aplikacji [trybu konfiguracji środowiska uruchomieniowego](#runtime-config-mode). Nawigowanie do `http://localhost:5000` w przeglądarce zostanie wyświetlona strona docelowa.

### <a name="running-with-visual-studio-2017"></a>Uruchomiony z programu Visual Studio 2017 r.

Otwórz *.csproj* plik utworzony przez [dotnet nowe](/dotnet/core/tools/dotnet-new) polecenia. Wymagane pakiety NuGet i npm zostaną przywrócone automatycznie po otwarciu projektu. Ten proces przywracania może potrwać kilka minut, a aplikacja jest gotowa do uruchomienia po jego ukończeniu. Kliknij zielony przycisk uruchamiania lub naciśnij klawisz `Ctrl + F5`, i przeglądarka otworzy się Strona początkowa aplikacji. Aplikacja zostanie uruchomiona na hoście lokalnym zgodnie z [trybu konfiguracji środowiska uruchomieniowego](#runtime-config-mode). 

<a name="app-testing"></a>

## <a name="testing-the-app"></a>Testowanie aplikacji

Jest wstępnie skonfigurowana do uruchamiania testów po stronie klienta przy użyciu szablonów SpaServices [Karma](https://karma-runner.github.io/1.0/index.html) i [jaśmin](https://jasmine.github.io/). Jaśmin jest jednostka popularnych testowania framework dla języka JavaScript, Karma jest test runner do tych testów. Karma jest skonfigurowana do pracy z [oprogramowanie pośredniczące deweloperów Webpack](#webpack-dev-middleware) tak, aby projektanta nie jest wymagane, aby zatrzymać i uruchomić test za każdym razem, gdy zostaną wprowadzone zmiany. Czy jest ono kodu uruchamianego przypadek testowy lub samego przypadek testowy, test jest uruchamiany automatycznie.

Korzystanie z aplikacji kątowego jako przykład dwóch przypadków testowych jaśmin są już udostępniane dla `CounterComponent` w *counter.component.spec.ts* pliku:

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/app/components/counter/counter.component.spec.ts?range=15-28)]

Otwórz wiersz polecenia w katalogu głównym projektu i uruchom następujące polecenie:

```console
npm test
```

Skrypt uruchamia uruchamiający Karma, który brzmi ustawień zdefiniowanych w *karma.conf.js* pliku. Wśród innych ustawień *karma.conf.js* identyfikuje pliki testowe, aby być wykonywane przy użyciu jego `files` tablicy:

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/test/karma.conf.js?range=4-5,8-11)]

<a name="app-publishing"></a>

## <a name="publishing-the-application"></a>Publikowanie aplikacji

Łączenie wygenerowanego zasoby po stronie klienta i opublikowanych artefakty platformy ASP.NET Core w gotowe do wdrożenia pakietu może być skomplikowane. Thankfully, SpaServices organizuje procesu całej publikacji o niestandardowych docelowy programu MSBuild o nazwie `RunWebpack`:

[!code-xml[](../client-side/spa-services/sample/SpaServicesSampleApp/SpaServicesSampleApp.csproj?range=31-45)]

Element docelowy programu MSBuild ma następujące obowiązki:
1. Przywracanie pakietów npm
1. Utwórz kompilację produkcji klasy zasobów innych firm, po stronie klienta
1. Utwórz kompilację klasy produkcji niestandardowych zasobów po stronie klienta
1. Skopiuj wygenerowany Webpack zasoby do folderu publikowania

Element docelowy programu MSBuild jest wywoływane, gdy uruchomiona:

```console
dotnet publish -c Release
```

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Dokumentacja dyrektywy angular](https://angular.io/docs)
