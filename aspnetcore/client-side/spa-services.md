---
title: Korzystanie z usług JavaScript do tworzenia aplikacji jednostronicowych w ASP.NET Core
author: scottaddie
description: Dowiedz się więcej na temat korzyści z używania usług JavaScript do tworzenia aplikacji jednostronicowej (SPA) obsługiwanej przez ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: H1Hack27Feb2017
ms.date: 09/06/2019
uid: client-side/spa-services
ms.openlocfilehash: c0c73882afd579510ad9cdf5b485c1d6fbeadd1c
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78663780"
---
# <a name="use-javascript-services-to-create-single-page-applications-in-aspnet-core"></a>Korzystanie z usług JavaScript do tworzenia aplikacji jednostronicowych w ASP.NET Core

Przez [Scott Addie](https://github.com/scottaddie) i [Fiyaz Hasan](https://fiyazhasan.me/)

Jednej strony aplikacji (SPA) jest typem popularnych aplikacji sieci web ze względu na jej nieodłączne zaawansowanego środowiska użytkownika. Integrowanie struktur lub bibliotek SPA po stronie klienta, takich jak [kątowy](https://angular.io/) lub [reaguje](https://facebook.github.io/react/), ze strukturami po stronie serwera, takimi jak ASP.NET Core, może być trudne. Usługi JavaScript opracowano w celu zmniejszenia liczby problemów w procesie integracji. Dzięki temu bezproblemowe wartościach innego klienta i serwera stosów technologicznych.

::: moniker range=">= aspnetcore-3.0"

> [!WARNING]
> Funkcje opisane w tym artykule są przestarzałe w odniesieniu do ASP.NET Core 3,0. W pakiecie NuGet [Microsoft. AspNetCore. SpaServices. Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices.Extensions) jest dostępny prostszy mechanizm integracji systemu Spa Framework. Aby uzyskać więcej informacji, zobacz [[anons] Obsoleting Microsoft. AspNetCore. SpaServices i Microsoft. AspNetCore. NodeServices](https://github.com/dotnet/AspNetCore/issues/12890).

::: moniker-end

## <a name="what-is-javascript-services"></a>Co to jest usługa JavaScript

Usługi JavaScript to zbiór technologii po stronie klienta dla ASP.NET Core. Jego celem jest pozwala platformy ASP.NET Core jako preferowaną platformę po stronie serwera deweloperów do tworzenia aplikacji jednostronicowych.

Usługi JavaScript składają się z dwóch różnych pakietów NuGet:

* [Microsoft. AspNetCore. NodeServices](https://www.nuget.org/packages/Microsoft.AspNetCore.NodeServices/) (NodeServices)
* [Microsoft. AspNetCore. SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) (SpaServices)

Te pakiety są przydatne w następujących scenariuszach:

* Uruchom JavaScript na serwerze
* SPA framework lub biblioteki
* Tworzenie zasobów po stronie klienta za pomocą Webpack

Wiele wysiłków w tym artykule jest umieszczany na temat korzystania z pakietu SpaServices.

## <a name="what-is-spaservices"></a>Co to jest SpaServices

SpaServices utworzono pozwala platformy ASP.NET Core jako preferowaną platformę po stronie serwera deweloperów do tworzenia aplikacji jednostronicowych. SpaServices nie jest wymagana do opracowania aplikacji jednostronicowych z ASP.NET Core i nie blokuje deweloperów w konkretnym środowisku klienta.

SpaServices zawiera przydatne infrastruktury, takich jak:

* [Renderowanie wstępne po stronie serwera](#server-side-prerendering)
* [Oprogramowanie pośredniczące dla deweloperów pakietu WebPack](#webpack-dev-middleware)
* [Zastępowanie modułu gorącego](#hot-module-replacement)
* [Pomocnicy routingu](#routing-helpers)

Zbiorczo te składniki infrastruktury pozwala zwiększyć przepływ pracy tworzenia oprogramowania i środowiska czasu wykonywania. Składniki mogą przyjąć indywidualnie.

## <a name="prerequisites-for-using-spaservices"></a>Wymagania wstępne dotyczące korzystania z SpaServices

Aby pracować z SpaServices, należy zainstalować następujące elementy:

* [Node. js](https://nodejs.org/) (w wersji 6 lub nowszej) z npm

  * Aby sprawdzić te składniki są zainstalowane i można go znaleźć, uruchom następujące polecenie w wierszu polecenia:

    ```console
    node -v && npm -v
    ```

  * W przypadku wdrażania w witrynie sieci Web systemu Azure nie jest wymagane żadne działanie&mdash;Node. js jest zainstalowane i dostępne w środowiskach serwerów.

* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]

  * W systemie Windows przy użyciu programu Visual Studio 2017 zestaw SDK jest instalowany przez wybranie obciążenia **międzyplatformowego dla aplikacji .NET Core** .

* Pakiet NuGet [Microsoft. AspNetCore. SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/)

## <a name="server-side-prerendering"></a>Prerendering po stronie serwera

Uniwersalnej aplikacji (nazywane również isomorphic) jest stanie działać zarówno serwer i klienta aplikacji w języku JavaScript. Platformy Angular, React i innych popularnych platform podanie tego stylu programowania aplikacji platformy uniwersalnej. Chodzi o to, aby najpierw przetworzyć składników framework na serwerze za pomocą środowiska Node.js, a następnie poprzez delegowanie przekazać dalsze wykonywanie do klienta.

ASP.NET Core [pomocników tagów](xref:mvc/views/tag-helpers/intro) zapewnianych przez SpaServices upraszczają implementację wstępnego renderowania po stronie serwera przez wywoływanie funkcji języka JavaScript na serwerze.

### <a name="server-side-prerendering-prerequisites"></a>Wymagania wstępne dotyczące renderowania po stronie serwera

Zainstaluj pakiet npm [renderowania z obsługą ASPNET](https://www.npmjs.com/package/aspnet-prerendering) :

```console
npm i -S aspnet-prerendering
```

### <a name="server-side-prerendering-configuration"></a>Konfiguracja renderowania wstępnego po stronie serwera

Pomocników tagów są odnajdywani za pomocą rejestracji przestrzeni nazw w pliku *_ViewImports. cshtml* projektu:

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/_ViewImports.cshtml?highlight=3)]

Te pomocników tagów natychmiast abstrakcji niewymagającego komunikować się bezpośrednio z interfejsy API niskiego poziomu, wykorzystując składnia przypominająca HTML w widoku Razor:

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=5)]

### <a name="asp-prerender-module-tag-helper"></a>skrypt ASP-PreRender-module tagów modułu

Pomocnik tagu `asp-prerender-module`, użyty w poprzednim przykładzie kodu, wykonuje *ClientApp/dist/Main-Server. js* na serwerze za pośrednictwem środowiska Node. js. Na potrzeby przejrzystości plik *Main-Server. js* jest artefaktem zadania Transpilation języka TypeScript-to-JavaScript w procesie kompilacji [pakietu WebPack](https://webpack.github.io/) . Pakiet WebPack definiuje alias punktu wejścia `main-server`; i przechodzenie wykresu zależności dla tego aliasu zaczyna się od pliku *ClientApp/Boot-Server. TS* :

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=53)]

W poniższym przykładzie kątowym plik *ClientApp/Boot-Server. TS* wykorzystuje funkcję `createServerRenderer` i `RenderResult` typ pakietu `aspnet-prerendering` npm do konfigurowania renderowania serwera za pomocą środowiska Node. js. Znaczniki HTML przeznaczone do renderowania po stronie serwera są przesyłane do wywołania funkcji rozpoznawania, które jest opakowane w obiekt `Promise` JavaScript o jednoznacznie określonym typie. Istotność obiektu `Promise` jest to, że asynchronicznie dostarcza znacznik HTML do strony w celu iniekcji w elemencie symbolu zastępczego modelu DOM.

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-34,79-)]

### <a name="asp-prerender-data-tag-helper"></a>ASP-PreRender — tag danych

Po połączeniu z pomocnikiem tagów `asp-prerender-module`, pomocnik tagów `asp-prerender-data` może służyć do przekazywania informacji kontekstowych z widoku Razor do kodu JavaScript po stronie serwera. Na przykład następujące znaczniki przekazują dane użytkownika do modułu `main-server`:

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=9-12)]

Odebrany argument `UserName` jest serializowany przy użyciu wbudowanego serializatora JSON i jest przechowywany w obiekcie `params.data`. W poniższym przykładzie kątowym dane są używane do konstruowania spersonalizowanego powitania w ramach elementu `h1`:

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,38-52,79-)]

Nazwy właściwości przesyłane przez pomocników tagów są reprezentowane przy użyciu notacji **PascalCase** . Przeciwieństwo do języka JavaScript, gdzie te same nazwy właściwości są reprezentowane przez **CamelCase**. Domyślna konfiguracja serializacji JSON jest odpowiedzialny za tę różnicę.

Aby rozwijać poprzedni przykład kodu, dane mogą być przekazywane z serwera do widoku przez Hydrating właściwości `globals` dostarczonej do funkcji `resolve`:

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,57-77,79-)]

Tablica `postList` zdefiniowana wewnątrz obiektu `globals` jest dołączona do globalnego obiektu `window` przeglądarki. Tej zmiennej podnoszenia do zakresu globalnego eliminuje dublowania działań, szczególnie w przypadku, ponieważ odnoszą się do ładowania tych samych danych, gdy na serwerze i ponownie na kliencie.

![Zmienna globalna postList dołączony do obiektu okna](spa-services/_static/global_variable.png)

## <a name="webpack-dev-middleware"></a>Oprogramowanie pośredniczące Dev Webpack

[Oprogramowanie pośredniczące dla deweloperów pakietu WebPack](https://webpack.js.org/guides/development/#using-webpack-dev-middleware) wprowadza Ulepszony przepływ pracy programistycznej, dzięki czemu pakiet WebPack kompiluje zasoby na żądanie. Oprogramowanie pośredniczące kompiluje i automatycznie obsługuje zasoby po stronie klienta po załadowaniu strony w przeglądarce. Alternatywne podejście jest ręcznie wywołać Webpack za pośrednictwem skryptu kompilacji npm projektu podczas zmiany zależności innych firm lub kod niestandardowy. Skrypt kompilacji npm w pliku *Package. JSON* jest przedstawiony w poniższym przykładzie:

```json
"build": "npm run build:vendor && npm run build:custom",
```

### <a name="webpack-dev-middleware-prerequisites"></a>Wymagania wstępne dotyczące oprogramowania pośredniczącego WebPack dev

Zainstaluj pakiet npm [ASPNET-WebPack](https://www.npmjs.com/package/aspnet-webpack) :

```console
npm i -D aspnet-webpack
```

### <a name="webpack-dev-middleware-configuration"></a>Konfiguracja oprogramowania pośredniczącego WebPack dev

Oprogramowanie pośredniczące programu WebPack dla deweloperów jest rejestrowane w potoku żądania HTTP przez następujący kod w metodzie `Configure` pliku *Startup.cs* :

[!code-csharp[](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=snippet_WebpackMiddlewareRegistration&highlight=4)]

Przed [zarejestrowaniem hostingu pliku statycznego](xref:fundamentals/static-files) za pomocą metody rozszerzenia `UseStaticFiles` należy wywołać metodę rozszerzenia `UseWebpackDevMiddleware`. Ze względów bezpieczeństwa należy zarejestrować oprogramowanie pośredniczące, tylko wtedy, gdy aplikacja jest uruchamiana w trybie projektowania.

Właściwość `output.publicPath` pliku *WebPack. config. js* informuje oprogramowanie pośredniczące o zmianach w folderze `dist`:

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,13-16)]

## <a name="hot-module-replacement"></a>Zastąpienie gorąca modułu

Zastanów się nad funkcją [zastępowania modułu](https://webpack.js.org/concepts/hot-module-replacement/) internetowego (HMR) elementu WebPack jako ewolucją [oprogramowania pośredniczącego dla deweloperów pakietu WebPack](#webpack-dev-middleware). HMR wprowadza te same korzyści, ale dodatkowo upraszcza przepływ pracy tworzenia oprogramowania przez automatyczną aktualizację zawartość strony po kompilacji zmian. Nie pomyl go z odświeżaniem przeglądarki i mogące zakłócać bieżący stan w pamięci i sesji debugowania SPA. Istnieje aktywne łącze między usługą Webpack Dev w oprogramowaniu pośredniczącym i przeglądarki, co oznacza, że zmiany są przekazywane do przeglądarki.

### <a name="hot-module-replacement-prerequisites"></a>Wymagania wstępne dotyczące zastępowania modułu aktywnego

Zainstaluj pakiet [WebPack-gorąca-Middle](https://www.npmjs.com/package/webpack-hot-middleware) npm Pack:

```console
npm i -D webpack-hot-middleware
```

### <a name="hot-module-replacement-configuration"></a>Konfiguracja wymiany modułu aktywnego

Składnik HMR musi być zarejestrowany w potoku żądania HTTP MVC w metodzie `Configure`:

```csharp
app.UseWebpackDevMiddleware(new WebpackDevMiddlewareOptions {
    HotModuleReplacement = true
});
```

Podobnie jak w przypadku [oprogramowania pośredniczącego w pakiecie WebPack](#webpack-dev-middleware), metoda rozszerzenia `UseWebpackDevMiddleware` musi zostać wywołana przed metodą rozszerzenia `UseStaticFiles`. Ze względów bezpieczeństwa należy zarejestrować oprogramowanie pośredniczące, tylko wtedy, gdy aplikacja jest uruchamiana w trybie projektowania.

Plik *WebPack. config. js* musi definiować tablicę `plugins`, nawet jeśli jest pusta:

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,25)]

Po załadowaniu aplikacji w przeglądarce, karty konsoli narzędzi dla deweloperów zawiera potwierdzenie HMR aktywacji:

![Gorąca komunikat połączonych zastąpienie modułu](spa-services/_static/hmr_connected.png)

## <a name="routing-helpers"></a>Pomocnicy routingu

W większości aplikacji jednostronicowych opartych na ASP.NET Core, routing po stronie klienta jest często pożądany oprócz routingu po stronie serwera. Systemy routingu SPA i MVC może działać niezależnie bez zakłóceń. Brak, jednak jednej krawędzi przypadków stanowiące wyzwań: Identyfikowanie odpowiedzi HTTP 404.

Rozważmy scenariusz, w którym jest używana trasa bezrozszerzająca `/some/page`. Przyjęto założenie, żądanie nie-dopasowania do wzorca trasy po stronie serwera, ale jego dopasowanie do wzorca, trasę po stronie klienta. Teraz Rozważmy żądanie przychodzące dla `/images/user-512.png`, które zwykle oczekuje na znalezienie pliku obrazu na serwerze. Jeśli żądana ścieżka zasobu nie jest zgodna z żadną trasą po stronie serwera lub plikiem statycznym, jest mało prawdopodobne, że aplikacja po stronie klienta obsłuży jej&mdash;na ogół zwraca kod stanu HTTP 404.

### <a name="routing-helpers-prerequisites"></a>Wymagania wstępne pomocników routingu

Zainstaluj pakiet npm routingu po stronie klienta. Przy użyciu usługi Angular, na przykład:

```console
npm i -S @angular/router
```

### <a name="routing-helpers-configuration"></a>Konfiguracja pomocników routingu

Metoda rozszerzająca o nazwie `MapSpaFallbackRoute` jest używana w metodzie `Configure`:

[!code-csharp[](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=snippet_MvcRoutingTable&highlight=7-9)]

Trasy są oceniane w kolejności, w jakiej są skonfigurowane. W związku z tym `default` trasa w poprzednim przykładzie kodu jest najpierw używana do dopasowania do wzorca.

## <a name="create-a-new-project"></a>Tworzenie nowego projektu

Usługi JavaScript udostępniają wstępnie skonfigurowane szablony aplikacji. SpaServices jest używany w tych szablonach w połączeniu z różnymi strukturami i bibliotekami, takimi jak kątowy, reagowanie i Redux.

Szablony te można zainstalować za pomocą interfejsu wiersza polecenia platformy .NET Core, uruchamiając następujące polecenie:

```dotnetcli
dotnet new --install Microsoft.AspNetCore.SpaTemplates::*
```

Zostanie wyświetlona lista dostępnych szablonów SPA:

| Szablony                                 | Krótka nazwa | Język | Tagi        |
| ------------------------------------------| :--------: | :------: | :---------: |
| MVC platformy ASP.NET Core przy użyciu usługi Angular             | Platformy angular    | [C#]     | Web/MVC/SPA |
| MVC platformy ASP.NET Core z użyciem biblioteki React.js            | react      | [C#]     | Web/MVC/SPA |
| MVC platformy ASP.NET Core z użyciem biblioteki React.js i Redux  | reactredux dla platformy | [C#]     | Web/MVC/SPA |

Aby utworzyć nowy projekt przy użyciu jednego z szablonów SPA, należy dołączyć **krótką nazwę** szablonu do polecenia [dotnet New](/dotnet/core/tools/dotnet-new) . Następujące polecenie tworzy aplikację platformy Angular z platformą ASP.NET Core MVC skonfigurowany po stronie serwera:

```dotnetcli
dotnet new angular
```

### <a name="set-the-runtime-configuration-mode"></a>Tryb konfiguracji środowiska uruchomieniowego

Istnieją dwa tryby konfiguracji podstawowego środowiska uruchomieniowego:

* **Programowanie**:
  * Obejmuje map źródeł, aby ułatwić debugowanie.
  * Nie Optymalizuj kod po stronie klienta dla wydajności.
* **Produkcja**:
  * Wyklucza mapy źródła.
  * Optymalizuje kod po stronie klienta za pomocą funkcji grupowania i minifikacja.

ASP.NET Core używa zmiennej środowiskowej o nazwie `ASPNETCORE_ENVIRONMENT` do przechowywania trybu konfiguracji. Aby uzyskać więcej informacji, zobacz [Ustawianie środowiska](xref:fundamentals/environments#set-the-environment).

### <a name="run-with-net-core-cli"></a>Uruchom z interfejs wiersza polecenia platformy .NET Core

Przywróć pakiety npm i wymagany NuGet, uruchamiając następujące polecenie w katalogu głównym projektu:

```dotnetcli
dotnet restore && npm i
```

Kompilowanie i uruchamianie aplikacji:

```dotnetcli
dotnet run
```

Aplikacja jest uruchamiana na hoście lokalnym zgodnie z [trybem konfiguracji środowiska uruchomieniowego](#set-the-runtime-configuration-mode). Przechodzenie do `http://localhost:5000` w przeglądarce spowoduje wyświetlenie strony docelowej.

### <a name="run-with-visual-studio-2017"></a>Uruchamianie z programem Visual Studio 2017

Otwórz plik *. csproj* wygenerowany przez polecenie [dotnet New](/dotnet/core/tools/dotnet-new) . Wymagane pakiety npm i NuGet zostaną przywrócone automatycznie po otwarty projekt. Ten proces przywracania może potrwać kilka minut, a aplikacja jest gotowa do uruchomienia po jego ukończeniu. Kliknij przycisk z zielonym uruchomieniem lub naciśnij klawisz `Ctrl + F5`, a w przeglądarce zostanie otwarta strona docelowa aplikacji. Aplikacja jest uruchamiana na hoście lokalnym zgodnie z [trybem konfiguracji środowiska uruchomieniowego](#set-the-runtime-configuration-mode).

## <a name="test-the-app"></a>Testowanie aplikacji

Szablony SpaServices są wstępnie skonfigurowane do uruchamiania testów po stronie klienta za pomocą [Karma](https://karma-runner.github.io/1.0/index.html) i [Jasmine](https://jasmine.github.io/). Jasmine to popularne testowania jednostkowego dla języka JavaScript, Karma jest moduł uruchamiający testy dla tych testów. Karma jest skonfigurowany do współdziałania z [pakietem pośredniczącym tworzenia pakietów WebPack](#webpack-dev-middleware) w taki sposób, aby deweloperzy nie musieli zatrzymywać i uruchamiać testów przy każdym wprowadzeniu zmian. Czy jest ono kod działających w odniesieniu do przypadku testowego lub sam przypadek testowy, test jest uruchamiany automatycznie.

Używając aplikacji kątowej jako przykładu, dwa przypadki testowe Jasmine są już dostarczone dla `CounterComponent` w pliku *Counter. Component. spec. TS* :

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/app/components/counter/counter.component.spec.ts?range=15-28)]

Otwórz wiersz polecenia w katalogu *ClientApp* . Uruchom następujące polecenie:

```console
npm test
```

Skrypt uruchamia moduł Karma Test Runner, który odczytuje ustawienia zdefiniowane w pliku *Karma. conf. js* . Oprócz innych ustawień *Karma. conf. js* identyfikuje pliki testowe do wykonania za pośrednictwem tablicy `files`:

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/test/karma.conf.js?range=4-5,8-11)]

## <a name="publish-the-app"></a>Publikowanie aplikacji

Aby uzyskać więcej informacji na temat publikowania na platformie Azure, zobacz [ten problem](https://github.com/dotnet/AspNetCore.Docs/issues/12474) w usłudze GitHub.

Łączenie wygenerowanych elementów zawartości po stronie klienta i opublikowane artefaktów ASP.NET Core w gotowe do wdrożenia pakietu może być kłopotliwe. Thankfully, SpaServices organizuje cały proces publikacji z niestandardowym elementem docelowym programu MSBuild o nazwie `RunWebpack`:

[!code-xml[](../client-side/spa-services/sample/SpaServicesSampleApp/SpaServicesSampleApp.csproj?range=31-45)]

Element docelowy programu MSBuild ma następujące obowiązki:

1. Przywróć pakiety npm.
1. Utwórz kompilację klasy produkcyjnej innych firm, które są zasobami po stronie klienta.
1. Utwórz kompilację klasy produkcyjnej dla niestandardowych zasobów po stronie klienta.
1. Skopiuj zasoby wygenerowane przez pakiet WebPack do folderu publikowanie.

Element docelowy programu MSBuild jest wywoływane, gdy uruchomiona:

```dotnetcli
dotnet publish -c Release
```

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Dokumenty kątowe](https://angular.io/docs)
