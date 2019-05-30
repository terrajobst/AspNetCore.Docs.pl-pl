---
title: Używanie usługi języka JavaScript do tworzenia aplikacji jednostronicowej w programie ASP.NET Core
author: scottaddie
description: Więcej informacji na temat korzyści z używania usługi w języka JavaScript do tworzenia pojedynczej strony aplikacji (SPA) wspartą platformy ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: H1Hack27Feb2017
ms.date: 05/28/2019
uid: client-side/spa-services
ms.openlocfilehash: 2ac7a6c7a5499d0525a61c6401f5996f8543fdba
ms.sourcegitcommit: 4d05e30567279072f1b070618afe58ae1bcefd5a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/29/2019
ms.locfileid: "66376213"
---
# <a name="use-javascript-services-to-create-single-page-applications-in-aspnet-core"></a>Używanie usługi języka JavaScript do tworzenia aplikacji jednostronicowej w programie ASP.NET Core

Przez [Scott Addie](https://github.com/scottaddie) i [Fiyaz Hasan](http://fiyazhasan.me/)

Jednej strony aplikacji (SPA) jest typem popularnych aplikacji sieci web ze względu na jej nieodłączne zaawansowanego środowiska użytkownika. Integrowanie SPA struktur po stronie klienta lub bibliotek, takich jak [Angular](https://angular.io/) lub [React](https://facebook.github.io/react/), przy użyciu struktur po stronie serwera, takich jak ASP.NET Core może być trudne. Usługi języka JavaScript został opracowany, aby zmniejszyć liczbę problemów w procesie integracji. Dzięki temu bezproblemowe wartościach innego klienta i serwera stosów technologicznych.

## <a name="what-is-javascript-services"></a>Co to są usługi języka JavaScript

Usługi języka JavaScript jest kolekcja technologii po stronie klienta dla platformy ASP.NET Core. Jego celem jest pozwala platformy ASP.NET Core jako preferowaną platformę po stronie serwera deweloperów do tworzenia aplikacji jednostronicowych.

Usługi języka JavaScript składa się z trzech odrębnych pakietów NuGet:

* [Microsoft.AspNetCore.NodeServices](https://www.nuget.org/packages/Microsoft.AspNetCore.NodeServices/) (NodeServices)
* [Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) (SpaServices)

Te pakiety są przydatne w następujących scenariuszach:

* Uruchom JavaScript na serwerze
* SPA framework lub biblioteki
* Tworzenie zasobów po stronie klienta za pomocą Webpack

Wiele wysiłków w tym artykule jest umieszczany na temat korzystania z pakietu SpaServices.

## <a name="what-is-spaservices"></a>Co to jest SpaServices

SpaServices utworzono pozwala platformy ASP.NET Core jako preferowaną platformę po stronie serwera deweloperów do tworzenia aplikacji jednostronicowych. SpaServices nie jest wymagane do rozwoju aplikacji jednostronicowych za pomocą programu ASP.NET Core, a jej nie blokuje deweloperów w ramach określonego klienta.

SpaServices zawiera przydatne infrastruktury, takich jak:

* [Prerendering po stronie serwera](#server-side-prerendering)
* [Oprogramowanie pośredniczące Dev Webpack](#webpack-dev-middleware)
* [Zastąpienie gorąca modułu](#hot-module-replacement)
* [Pomocnicy routingu](#routing-helpers)

Zbiorczo te składniki infrastruktury pozwala zwiększyć przepływ pracy tworzenia oprogramowania i środowiska czasu wykonywania. Składniki mogą przyjąć indywidualnie.

## <a name="prerequisites-for-using-spaservices"></a>Wymagania wstępne dotyczące korzystania z SpaServices

Aby pracować z SpaServices, należy zainstalować następujące elementy:

* [Node.js](https://nodejs.org/) (w wersji 6 lub nowszej) za pomocą pakietu npm

  * Aby sprawdzić te składniki są zainstalowane i można go znaleźć, uruchom następujące polecenie w wierszu polecenia:

    ```console
    node -v && npm -v
    ```

  * Jeśli wdrażana w witrynie sieci web platformy Azure, jest wymagana żadna akcja&mdash;Node.js jest zainstalowany i dostępny w środowisku serwerów.

* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]

  * Na Windows przy użyciu programu Visual Studio 2017, zestaw SDK jest zainstalowany, wybierając **programowanie dla wielu platform .NET Core** obciążenia.

* [Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) NuGet package

## <a name="server-side-prerendering"></a>Prerendering po stronie serwera

Uniwersalnej aplikacji (nazywane również isomorphic) jest stanie działać zarówno serwer i klienta aplikacji w języku JavaScript. Platformy Angular, React i innych popularnych platform podanie tego stylu programowania aplikacji platformy uniwersalnej. Chodzi o to, aby najpierw przetworzyć składników framework na serwerze za pomocą środowiska Node.js, a następnie poprzez delegowanie przekazać dalsze wykonywanie do klienta.

Platforma ASP.NET Core [pomocników tagów](xref:mvc/views/tag-helpers/intro) dostarczone przez SpaServices upraszczająca implementację elementów prerendering po stronie serwera za pomocą wywołania funkcji JavaScript na serwerze.

### <a name="server-side-prerendering-prerequisites"></a>Wymagania wstępne prerendering po stronie serwera

Zainstaluj [aspnet prerendering](https://www.npmjs.com/package/aspnet-prerendering) pakietu npm:

```console
npm i -S aspnet-prerendering
```

### <a name="server-side-prerendering-configuration"></a>Prerendering konfiguracji po stronie serwera

Pomocnicy tagów są wykrywalne za pośrednictwem rejestracji przestrzeni nazw w projekcie *_ViewImports.cshtml* pliku:

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/_ViewImports.cshtml?highlight=3)]

Te pomocników tagów natychmiast abstrakcji niewymagającego komunikować się bezpośrednio z interfejsy API niskiego poziomu, wykorzystując składnia przypominająca HTML w widoku Razor:

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=5)]

### <a name="asp-prerender-module-tag-helper"></a>Pomocnik tagu ASP prerender modułowe

`asp-prerender-module` Wykonuje pomocnika tagów, używany w poprzednim przykładzie kodu *ClientApp/dist/main-server.js* na serwerze za pomocą środowiska Node.js. Dla jasności *main server.js* pliku jest pozostałością zadania transpilation TypeScript i JavaScript w [Webpack](http://webpack.github.io/) procesu kompilacji. Webpack definiuje alias punktu wejścia `main-server`; i przechodzenie wykres zależności dla tego aliasu, który rozpoczyna się od *ClientApp/rozruchu server.ts* pliku:

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=53)]

W poniższym przykładzie Angular *ClientApp/rozruchu server.ts* korzysta z pliku `createServerRenderer` funkcji i `RenderResult` typu `aspnet-prerendering` pakietu npm, aby skonfigurować renderowanie na serwerze za pomocą środowiska Node.js. Kod znaczników HTML przeznaczonej do renderowania po stronie serwera jest przekazywany do wywołanie funkcji rozwiązania jest opakowywany w języku JavaScript silnie typizowane `Promise` obiektu. `Promise` Znaczenie obiektu to, że asynchronicznie dostarcza kod znaczników HTML strony do iniekcji w elemencie symbolu zastępczego DOM w LICZBIE.

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-34,79-)]

### <a name="asp-prerender-data-tag-helper"></a>Pomocnik tagu ASP prerender danych

W połączeniu z `asp-prerender-module` Pomocnik tagu `asp-prerender-data` Pomocnik tagu może służyć do przekazywania informacji kontekstowych z widoku Razor w kodzie JavaScript po stronie serwera. Na przykład, następujący kod przekazuje dane użytkownika do `main-server` modułu:

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=9-12)]

Odebrano `UserName` argument jest serializowana, za pomocą wbudowanych serializator JSON i jest przechowywany w `params.data` obiektu. W poniższym przykładzie Angular danych służy do tworzenia spersonalizowanych powitania w ramach `h1` elementu:

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,38-52,79-)]

Nazwy właściwości przekazanej pomocników tagów są reprezentowane z **PascalCase** notacji. Natomiast, w kodzie JavaScript, w których takie same nazwy właściwości są reprezentowane za pomocą **camelCase**. Domyślna konfiguracja serializacji JSON jest odpowiedzialny za tę różnicę.

Aby rozszerzyć na poprzednim przykładzie kodu, dane mogą być przekazywane z serwera do widoku przez nawilżania `globals` właściwości udostępniane `resolve` funkcji:

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,57-77,79-)]

`postList` Zdefiniowane wewnątrz tablicy `globals` obiekt jest dołączony do przeglądarki globalnego `window` obiektu. Tej zmiennej podnoszenia do zakresu globalnego eliminuje dublowania działań, szczególnie w przypadku, ponieważ odnoszą się do ładowania tych samych danych, gdy na serwerze i ponownie na kliencie.

![Zmienna globalna postList dołączony do obiektu okna](spa-services/_static/global_variable.png)

## <a name="webpack-dev-middleware"></a>Oprogramowanie pośredniczące Dev Webpack

[Oprogramowanie pośredniczące Dev Webpack](https://webpack.js.org/guides/development/#using-webpack-dev-middleware) wprowadza usprawnione Tworzenie przepływu pracy, według której Webpack kompilacji zasobów na żądanie. Oprogramowanie pośredniczące kompiluje i automatycznie obsługuje zasoby po stronie klienta po załadowaniu strony w przeglądarce. Alternatywne podejście jest ręcznie wywołać Webpack za pośrednictwem skryptu kompilacji npm projektu podczas zmiany zależności innych firm lub kod niestandardowy. Npm kompilacja skrypt w *package.json* plik jest wyświetlany w następującym przykładzie:

```json
"build": "npm run build:vendor && npm run build:custom",
```

### <a name="webpack-dev-middleware-prerequisites"></a>Wymagania wstępne dotyczące oprogramowania pośredniczącego Dev Webpack

Zainstaluj [aspnet webpack](https://www.npmjs.com/package/aspnet-webpack) pakietu npm:

```console
npm i -D aspnet-webpack
```

### <a name="webpack-dev-middleware-configuration"></a>Oprogramowanie pośredniczące Dev Webpack konfiguracji

Oprogramowanie pośredniczące Dev Webpack zostanie zarejestrowana w Potok żądań HTTP za pośrednictwem następujący kod w *Startup.cs* pliku `Configure` metody:

[!code-csharp[](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=snippet_WebpackMiddlewareRegistration&highlight=4)]

`UseWebpackDevMiddleware` — Metoda rozszerzenia musi zostać wywołana przed [rejestrowanie hostowania plików statycznych](xref:fundamentals/static-files) za pośrednictwem `UseStaticFiles` — metoda rozszerzenia. Ze względów bezpieczeństwa należy zarejestrować oprogramowanie pośredniczące, tylko wtedy, gdy aplikacja jest uruchamiana w trybie projektowania.

*Webpack.config.js* pliku `output.publicPath` właściwość informuje oprogramowanie pośredniczące, aby obejrzeć `dist` folder zmiany:

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,13-16)]

## <a name="hot-module-replacement"></a>Zastąpienie gorąca modułu

Traktować firmy Webpack [gorąca zastąpienie modułu](https://webpack.js.org/concepts/hot-module-replacement/) funkcji (HMR) jako unowocześnienia funkcji [Webpack Dev w oprogramowaniu pośredniczącym](#webpack-dev-middleware). HMR wprowadza te same korzyści, ale dodatkowo upraszcza przepływ pracy tworzenia oprogramowania przez automatyczną aktualizację zawartość strony po kompilacji zmian. Nie pomyl go z odświeżaniem przeglądarki i mogące zakłócać bieżący stan w pamięci i sesji debugowania SPA. Istnieje aktywne łącze między usługą Webpack Dev w oprogramowaniu pośredniczącym i przeglądarki, co oznacza, że zmiany są przekazywane do przeglądarki.

### <a name="hot-module-replacement-prerequisites"></a>Gorąca modułu zastąpienie wymagania wstępne

Zainstaluj [webpack-hot — oprogramowanie pośredniczące](https://www.npmjs.com/package/webpack-hot-middleware) pakietu npm:

```console
npm i -D webpack-hot-middleware
```

### <a name="hot-module-replacement-configuration"></a>Gorąca konfiguracji modułu zastąpienia

Składnik HMR muszą być rejestrowane w Potok żądań HTTP MVC w `Configure` metody:

```csharp
app.UseWebpackDevMiddleware(new WebpackDevMiddlewareOptions {
    HotModuleReplacement = true
});
```

Jak był prawdziwy z [oprogramowania pośredniczącego Dev Webpack](#webpack-dev-middleware), `UseWebpackDevMiddleware` — metoda rozszerzenia musi zostać wywołana przed `UseStaticFiles` — metoda rozszerzenia. Ze względów bezpieczeństwa należy zarejestrować oprogramowanie pośredniczące, tylko wtedy, gdy aplikacja jest uruchamiana w trybie projektowania.

*Webpack.config.js* musi definiować plik `plugins` tablicy, nawet wtedy, gdy zostanie pozostawiony pusty:

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,25)]

Po załadowaniu aplikacji w przeglądarce, karty konsoli narzędzi dla deweloperów zawiera potwierdzenie HMR aktywacji:

![Gorąca komunikat połączonych zastąpienie modułu](spa-services/_static/hmr_connected.png)

## <a name="routing-helpers"></a>Pomocnicy routingu

W większości aplikacji jednostronicowych oparte na programie ASP.NET Core routing po stronie klienta jest często potrzebne oprócz routingu po stronie serwera. Systemy routingu SPA i MVC może działać niezależnie bez zakłóceń. Brak, jednak jednej krawędzi przypadków stanowiące wyzwań: Identyfikowanie odpowiedzi HTTP 404.

Rozważmy scenariusz, w którym będą trasę `/some/page` jest używany. Przyjęto założenie, żądanie nie-dopasowania do wzorca trasy po stronie serwera, ale jego dopasowanie do wzorca, trasę po stronie klienta. Teraz należy wziąć pod uwagę żądanie przychodzące dla `/images/user-512.png`, którego oczekuje ogólnie można znaleźć pliku obrazu na serwerze. Jeśli tej ścieżki żądany zasób nie jest zgodny, trasie po stronie serwera lub pliku statycznego, jest mało prawdopodobne, że aplikacja kliencka będzie go obsłużyć&mdash;pożądana jest zwykle zwraca kod stanu HTTP 404.

### <a name="routing-helpers-prerequisites"></a>Routing pomocników wymagań wstępnych.

Zainstaluj pakiet npm routingu po stronie klienta. Przy użyciu usługi Angular, na przykład:

```console
npm i -S @angular/router
```

### <a name="routing-helpers-configuration"></a>Konfiguracja routingu Pomocnicy

Metody rozszerzenia o nazwie `MapSpaFallbackRoute` jest używany w `Configure` metody:

[!code-csharp[](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=snippet_MvcRoutingTable&highlight=7-9)]

Trasy są obliczane w kolejności, w którym są one konfigurowane. W związku z tym `default` trasy w poprzednim przykładzie kodu jest używana jako pierwsza dopasowywanie do wzorców.

## <a name="create-a-new-project"></a>Tworzenie nowego projektu

Usługi języka JavaScript udostępniają szablony wstępnie skonfigurowanej aplikacji. SpaServices jest używany w tych szablonów w połączeniu z różnych platform i bibliotek, takich jak Angular, React i kontenera Redux.

Szablony te można zainstalować za pomocą interfejsu wiersza polecenia platformy .NET Core, uruchamiając następujące polecenie:

```console
dotnet new --install Microsoft.AspNetCore.SpaTemplates::*
```

Zostanie wyświetlona lista dostępnych szablonów SPA:

| Szablony                                 | Krótka nazwa | Język | Znaczniki        |
| ------------------------------------------| :--------: | :------: | :---------: |
| MVC platformy ASP.NET Core przy użyciu usługi Angular             | Platformy angular    | [C#]     | Web/MVC/SPA |
| MVC platformy ASP.NET Core z użyciem biblioteki React.js            | react      | [C#]     | Web/MVC/SPA |
| MVC platformy ASP.NET Core z użyciem biblioteki React.js i Redux  | reactredux dla platformy | [C#]     | Web/MVC/SPA |

Aby utworzyć nowy projekt za pomocą jednego z szablonów SPA, należy dołączyć **krótką nazwę** szablonu w [dotnet nowe](/dotnet/core/tools/dotnet-new) polecenia. Następujące polecenie tworzy aplikację platformy Angular z platformą ASP.NET Core MVC skonfigurowany po stronie serwera:

```console
dotnet new angular
```

### <a name="set-the-runtime-configuration-mode"></a>Tryb konfiguracji środowiska uruchomieniowego

Istnieją dwa tryby konfiguracji podstawowego środowiska uruchomieniowego:

* **Programowanie**:
  * Obejmuje map źródeł, aby ułatwić debugowanie.
  * Nie Optymalizuj kod po stronie klienta dla wydajności.
* **Produkcji**:
  * Wyklucza mapy źródła.
  * Optymalizuje kod po stronie klienta za pośrednictwem tworzenie pakietów i minimalizowanie.

Platforma ASP.NET Core używa zmiennej środowiskowej o nazwie `ASPNETCORE_ENVIRONMENT` do przechowywania tryb konfiguracji. Aby uzyskać więcej informacji, zobacz [należy ustawić środowisko](xref:fundamentals/environments#set-the-environment).

### <a name="run-with-net-core-cli"></a>Uruchom za pomocą programu .NET Core interfejsu wiersza polecenia

Przywróć pakiety npm i wymagany NuGet, uruchamiając następujące polecenie w katalogu głównym projektu:

```console
dotnet restore && npm i
```

Kompilowanie i uruchamianie aplikacji:

```console
dotnet run
```

Aplikacja uruchamia się na hoście lokalnym, zgodnie z opisem w [tryb konfiguracji środowiska uruchomieniowego](#set-the-runtime-configuration-mode). Przejdź do `http://localhost:5000` w przeglądarce zostanie wyświetlona strona docelowa.

### <a name="run-with-visual-studio-2017"></a>Uruchamianie programu Visual Studio 2017

Otwórz *.csproj* pliku wygenerowanego przez [dotnet nowe](/dotnet/core/tools/dotnet-new) polecenia. Wymagane pakiety npm i NuGet zostaną przywrócone automatycznie po otwarty projekt. Ten proces przywracania może potrwać kilka minut, a aplikacja jest gotowa do uruchomienia po jego ukończeniu. Kliknij zielony przycisk uruchamiania lub naciśnij klawisz `Ctrl + F5`, i przeglądarce otworzy się Strona docelowa aplikacji. Aplikacja zostanie uruchomiona na hoście lokalnym, zgodnie z opisem w [tryb konfiguracji środowiska uruchomieniowego](#set-the-runtime-configuration-mode).

## <a name="test-the-app"></a>Testowanie aplikacji

Jest wstępnie skonfigurowana do uruchamiania testów po stronie klienta za pomocą szablonów SpaServices [Karma](https://karma-runner.github.io/1.0/index.html) i [Jasmine](https://jasmine.github.io/). Jasmine to popularne testowania jednostkowego dla języka JavaScript, Karma jest moduł uruchamiający testy dla tych testów. Karma jest skonfigurowana do pracy z [oprogramowania pośredniczącego Dev Webpack](#webpack-dev-middleware) taki sposób, że deweloper nie jest wymagane do zatrzymywania i uruchamiania testu za każdym razem, gdy zostaną wprowadzone zmiany. Czy jest ono kod działających w odniesieniu do przypadku testowego lub sam przypadek testowy, test jest uruchamiany automatycznie.

Korzystanie z aplikacji Angular jako przykład dwóch Jasmine przypadki testowe są już udostępniane dla `CounterComponent` w *counter.component.spec.ts* pliku:

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/app/components/counter/counter.component.spec.ts?range=15-28)]

Otwórz wiersz polecenia w *ClientApp* katalogu. Uruchom następujące polecenie:

```console
npm test
```

Skrypt uruchamia Karma narzędzie test runner, który odczytuje ustawienia zdefiniowane w *karma.conf.js* pliku. Wśród innych ustawień *karma.conf.js* identyfikuje pliki testowe, które mają być wykonane za pomocą jego `files` tablicy:

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/test/karma.conf.js?range=4-5,8-11)]

## <a name="publish-the-app"></a>Publikowanie aplikacji

Łączenie wygenerowanych elementów zawartości po stronie klienta i opublikowane artefaktów ASP.NET Core w gotowe do wdrożenia pakietu może być kłopotliwe. Szczęście SpaServices organizuje procesu całej publikacji z niestandardowy cel programu MSBuild, o nazwie `RunWebpack`:

[!code-xml[](../client-side/spa-services/sample/SpaServicesSampleApp/SpaServicesSampleApp.csproj?range=31-45)]

Element docelowy programu MSBuild ma następujące obowiązki:

1. Przywróć pakiety npm.
1. Utwórz kompilację klasy produkcyjnej zasoby innych firm, po stronie klienta.
1. Tworzenie klasy produkcyjnej kompilacji niestandardowych zasobów po stronie klienta.
1. Skopiuj wygenerowany Webpack zasobów do folderu publikowania.

Element docelowy programu MSBuild jest wywoływane, gdy uruchomiona:

```console
dotnet publish -c Release
```

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Dokumentacja usługi angular](https://angular.io/docs)
