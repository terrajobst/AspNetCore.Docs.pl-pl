---
title: "Należy użyć szablonu projektu dyrektywy Angular"
author: SteveSandersonMS
description: "Dowiedz się, jak rozpocząć pracę z szablonem projektu platformy ASP.NET Core jednostronicowej aplikacji JEDNOSTRONICOWEJ Podgląd kątową i kątowego interfejsu wiersza polecenia."
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 12/06/2017
ms.devlang: csharp
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: spa/angular
ms.openlocfilehash: de7b32099f225e838ec40ce4f04bf67bffb9c260
ms.sourcegitcommit: 198fb0488e961048bfa376cf58cb853ef1d1cb91
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/14/2017
---
# <a name="use-the-angular-project-template-preview"></a>Należy użyć szablonu projektu kątowego (wersja zapoznawcza)

> [!NOTE]
> Ta dokumentacja nie jest o szablonie wydanych kątowego projektu. **Niniejsza dokumentacja jest dotyczące wersji zapoznawczej kątowego szablonu.** Mamy nadzieję, należy wysłać wersji wydanej w wczesne 2018.

Szablon zaktualizowany projekt kątowego udostępnia dogodny punkt początkowy dla platformy ASP.NET Core aplikacji przy użyciu dyrektywy Angular 5 i kątowego interfejsu wiersza polecenia do zaimplementowania rozbudowanego klienta interfejsu użytkownika (UI).

Szablon jest odpowiednikiem Tworzenie projektu platformy ASP.NET Core do działania jako zaplecza interfejsu API i projektu kątowego interfejsu wiersza polecenia do działania jako interfejsu użytkownika. Szablon oferuje wygodne hosting obu typów projektów w projekcie jednej aplikacji. W rezultacie projektu aplikacji można wbudowane i opublikowane jako pojedyncza jednostka.

## <a name="create-a-new-app"></a>Tworzenie nowej aplikacji

Aby rozpocząć, upewnij się, które zostały [zainstalowane szablonu zaktualizowany projekt kątowego](xref:spa/index#installation). Te instrukcje nie dotyczą poprzedniego szablonu projektu kątowego objęte .NET Core 2.0.x zestawu SDK.

Utwórz nowy projekt z wiersza polecenia przy użyciu polecenia `dotnet new angular` w pustych katalogów. Na przykład poniższe polecenia Utwórz aplikację w *— nowy — aplikacji my* katalogu i przełącznika do tego katalogu:

```console
dotnet new angular -o my-new-app
cd my-new-app
```

Uruchamianie aplikacji z programu Visual Studio lub platformy .NET Core interfejsu wiersza polecenia:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Otwórz wygenerowany *.csproj* pliku i uruchom aplikację w zwykły stamtąd.

Proces kompilacji przywraca npm zależności przy pierwszym uruchomieniu, co może zająć kilka minut. Kolejne kompilacje są znacznie szybsze.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

Upewnij się, masz zmienną środowiskową o nazwie `ASPNETCORE_Environment` o wartości `Development`. W systemie Windows (w monity-PowerShell), uruchom `SET ASPNETCORE_Environment=Development`. W systemie Linux lub macOS Uruchom `export ASPNETCORE_Environment=Development`.

Uruchom `dotnet build` można zweryfikować aplikacji kompilacje poprawnie. Przy pierwszym uruchomieniu procesu kompilacji przywraca npm zależności, co może potrwać kilka minut. Kolejne kompilacje są znacznie szybsze.

Uruchom `dotnet run` do uruchomienia aplikacji. Zostanie zarejestrowany komunikat podobny do następującego:

```console
Now listening on: http://localhost:<port>
```

Przejdź do tego adresu URL w przeglądarce.

Wystąpienie serwera kątowego interfejsu wiersza polecenia w tle uruchamiania aplikacji. Zostanie zarejestrowany komunikat podobny do następującego: *NG Live Development Server nasłuchuje na localhost:&lt;otherport&gt;, otwórz przeglądarkę na http://localhost:&lt;otherport&gt; /* . Zignoruj ten komunikat&mdash;ma **nie** adres URL dla połączonych aplikacji platformy ASP.NET Core i kątowego interfejsu wiersza polecenia.

---

Szablon projektu tworzy aplikację platformy ASP.NET Core i kątowego aplikacji. Aplikacja platformy ASP.NET Core jest przeznaczona do użycia dla dostępu do danych, autoryzację i inne problemy po stronie serwera. Dyrektywy Angular aplikacji, znajdującej się w *ClientApp* podkatalogu, jest przeznaczona do użycia dla wszystkich problemów w zakresie interfejsu użytkownika.

## <a name="add-pages-images-styles-modules-etc"></a>Dodawanie stron, obrazy, style, modułów, itp.

*ClientApp* katalog zawiera standardowe aplikacji kątowego interfejsu wiersza polecenia. Można znaleźć w oficjalnej [kątowego dokumentacji](https://github.com/angular/angular-cli/wiki) Aby uzyskać więcej informacji.

Istnieją niewielkich różnic między kątowego aplikacji utworzonych przez ten szablon i utworzonym przez kątowego interfejsu wiersza polecenia, sam (za pośrednictwem `ng new`), jednak nie uległy zmianie możliwości aplikacji. Utworzona przez szablon aplikacja zawiera [Bootstrap](https://getbootstrap.com/)— na podstawie układu i prosty przykład routingu.

## <a name="run-ng-commands"></a>Uruchom polecenia ng

W wierszu polecenia, przełącz się do *ClientApp* podkatalogu:

```console
cd ClientApp
```

Jeśli masz `ng` narzędzie zainstalowane globalnie, możesz uruchomić dowolne z jego poleceń. Na przykład można uruchomić `ng lint`, `ng test`, ani żadnych innych [kątowego polecenia interfejsu wiersza polecenia](https://github.com/angular/angular-cli/wiki#additional-commands). Nie istnieje potrzeba do uruchamiania `ng serve` , ponieważ aplikacja platformy ASP.NET Core dotyczy obsługująca zarówno po stronie serwera i klienta części aplikacji. Wewnętrznie, używa `ng serve` do rozwoju.

Jeśli nie masz `ng` narzędzie zainstalowane, uruchom `npm run ng` zamiast tego. Na przykład można uruchomić `npm run ng lint` lub `npm run ng test`.

## <a name="install-npm-packages"></a>Instalowanie pakietów npm

Aby zainstalować pakiety innych firm npm, przy użyciu wiersza polecenia w *ClientApp* podkatalogu. Na przykład:

```console
cd ClientApp
npm install --save <package_name>
```

## <a name="publish-and-deploy"></a>Publikowanie i wdrażanie

W rozwoju aplikacji jest uruchamiany w trybie zoptymalizowane pod kątem wygody deweloperów. Na przykład pakiety JavaScript obejmują map źródeł (tak, aby podczas debugowania, możesz zobaczyć oryginalnego kodu TypeScript). Aplikacja oczekuje na maszynie, HTML i CSS zmian plików na dysku i automatycznie rekompiluje i ponowne załadowanie po wykryciu tych plików, zmiany.

W środowisku produkcyjnym obsługiwać wersji aplikacji jest zoptymalizowany pod kątem wydajności. To jest skonfigurowany automatycznie. Podczas publikowania, zminimalizowany emituje konfigurację kompilacji, kompilacja kodu po stronie klienta skompilowane z wyprzedzeniem elementu time (drzewa obiektów aplikacji). W przeciwieństwie do kompilacji programowanie kompilacji produkcyjnym nie wymaga Node.js do zainstalowania na serwerze (Jeśli nie włączono [prerendering po stronie serwera](#server-side-rendering)).

Można użyć standardowego [platformy ASP.NET Core publikowania i metody wdrażania](xref:publishing/index).

## <a name="run-ng-serve-independently"></a>Uruchom niezależnie "ng służą"

Projekt jest skonfigurowany do uruchamiania własnego wystąpienia serwera kątowego interfejsu wiersza polecenia w tle po uruchomieniu aplikacji platformy ASP.NET Core w trybie projektowania. Ta opcja jest przydatna, ponieważ nie trzeba ręcznie uruchomić oddzielny serwer.

Brak wadą tego ustawienia domyślne. Po każdej zmianie kodu C# i ASP.NET podstawowych aplikacja musi ponownie uruchomić, ponowne uruchomienie serwera kątowego interfejsu wiersza polecenia. Około 10 sekund jest wymagany, aby rozpocząć tworzenie kopii zapasowej. Jeśli wprowadzania częste zmiany kodu C# i nie chcesz czekać na kątowego interfejsu wiersza polecenia uruchomić ponownie, uruchom serwer kątowego CLI zewnętrznie, niezależnie od procesu ASP.NET Core. Aby to zrobić:

1. W wierszu polecenia, przełącz się do *ClientApp* podkatalogu, a następnie uruchom serwera wdrożeniowego kątowego interfejsu wiersza polecenia:

    ```console
    cd ClientApp
    npm start
    ```

    > [!IMPORTANT]
    > Użyj `npm start` można uruchomić serwera wdrożeniowego programu kątowego interfejsu wiersza polecenia nie `ng serve`, dzięki czemu konfiguracji w *package.json* jest przestrzegana. Aby przekazywać dodatkowych parametrów do serwera kątowego interfejsu wiersza polecenia, dodaj go do odpowiedniego `scripts` wiersz w Twojej *package.json* pliku.

2. Zmodyfikuj do korzystania z zewnętrznego wystąpienia kątowego CLI zamiast uruchamiania jednego z jego własnej aplikacji platformy ASP.NET Core. W Twojej *uruchamiania* klasy, Zastąp `spa.UseAngularCliServer` wywołania z następujących czynności:

    ```csharp
    spa.UseProxyToSpaDevelopmentServer("http://localhost:4200");
    ```

Po uruchomieniu aplikacji platformy ASP.NET Core, nie będzie go uruchomić serwera kątowego interfejsu wiersza polecenia. Zamiast niego jest używana wystąpienia, które zostanie uruchomione ręcznie. Dzięki temu może uruchomić i ponownie uruchom szybciej. Nie jest już oczekuje na kątowego CLI odbudować zawsze aplikację klienta.

## <a name="server-side-rendering"></a>Renderowanie po stronie serwera

Jako funkcja wydajności można przed renderowaniem kątowego aplikacji na serwerze, a także uruchomiony na kliencie. Oznacza to, że przeglądarka otrzyma kod znaczników HTML reprezentujący początkowej interfejsu użytkownika aplikacji, tak aby wyświetlały nawet przed pobraniem i wykonywanie programu pakiety języka JavaScript. Większość implementacji tego pochodzi z kątowego funkcję [kątowego uniwersalnych](https://universal.angular.io/).

> [!TIP]
> Włączanie renderowania po stronie serwera (SSR) przedstawiono kilka dodatkowych komplikacji zarówno podczas opracowywania i wdrażania. Odczyt [wady SSR](#drawbacks-of-ssr) do ustalenia, czy SSR jest odpowiedni dla wymagań.

Aby włączyć SSR, należy wprowadzić numer ma zostać dodany do projektu.

W *uruchamiania* klasy *po* wiersza, który konfiguruje `spa.Options.SourcePath`, i *przed* wywołanie `UseAngularCliServer` lub `UseProxyToSpaDevelopmentServer`, Dodaj następujący kod:

[!code-csharp[](sample/AngularServerSideRendering/Startup.cs?name=snippet_Call_UseSpa&highlight=5-12)]

W trybie projektowania, ten kod próbuje kompilacji pakietu SSR przez uruchomienie skryptu `build:ssr`, która jest zdefiniowana w *ClientApp\package.json*. Powoduje to skompilowanie kątowego aplikacji o nazwie `ssr`, która nie została jeszcze zdefiniowana. 

Na koniec `apps` tablicy w *ClientApp/.angular-cli.json*, zdefiniuj dodatkowy aplikacji o nazwie `ssr`. Użyj następujących opcji:

[!code-json[](sample/AngularServerSideRendering/ClientApp/.angular-cli.json?range=24-41)]

Tę nową konfigurację aplikacji z włączoną obsługą SSR wymaga dwóch plików dalsze: *tsconfig.server.json* i *main.server.ts*. *Tsconfig.server.json* plik Określa opcje kompilacji języka TypeScript. *Main.server.ts* plik służy jako punkt wejścia kodu podczas SSR.

Dodaj nowy plik o nazwie *tsconfig.server.json* wewnątrz *ClientApp/src* (obok istniejącej *tsconfig.app.json*), zawierających następujące:

[!code-json[](sample/AngularServerSideRendering/ClientApp/src/tsconfig.server.json)]

Ten plik konfiguruje kątową przez kompilator drzewa obiektów aplikacji do wyszukania modułu o nazwie `app.server.module`. Dodaj tę przez utworzenie nowego pliku w *ClientApp/src/app/app.server.module.ts* (obok istniejącej *app.module.ts*), które zawierają: 

[!code-typescript[](sample/AngularServerSideRendering/ClientApp/src/app/app.server.module.ts)]

Ten moduł dziedziczy od strony klienta `app.module` i definiuje, które moduły kątową bardzo są dostępne podczas SSR.

Odwołania, który nowy `ssr` wpis w *.angular cli.json* odwołuje się do pliku punktu wejścia o nazwie *main.server.ts*. Nie dodano jeszcze tego pliku, a nadszedł czas, aby to zrobić. Utwórz nowy plik w *ClientApp/src/main.server.ts* (obok istniejącej *main.ts*), zawierających następujące:

[!code-typescript[](sample/AngularServerSideRendering/ClientApp/src/main.server.ts)]

Kod ten plik znajduje się co platformy ASP.NET Core wykonuje dla każdego żądania po uruchomieniu `UseSpaPrerendering` oprogramowanie pośredniczące, który został dodany do *uruchamiania* klasy. Dotyczy on odbieranie `params` z kodu platformy .NET (na przykład adres URL żądanego) i wykonywania wywołań do dyrektywy Angular SSR interfejsów API można pobrać wynikowy kod HTML. 

Ściśle mówiąc, jest wystarczające, aby umożliwić SSR w trybie projektowania. Należy zmienić jedną końcowego tak, aby aplikacja działa prawidłowo po opublikowaniu. W głównym aplikacji *.csproj* pliku, ustaw `BuildServerSideRenderer` wartości właściwości do `true`:

[!code-xml[](sample/AngularServerSideRendering/AngularServerSideRendering.csproj?name=snippet_EnableBuildServerSideRenderer)]

Spowoduje to skonfigurowanie proces kompilacji, aby uruchomić `build:ssr` podczas publikowania i wdrożyć SSR plików na serwerze. Jeśli nie zostało włączone, SSR kończy się niepowodzeniem w środowisku produkcyjnym.

Gdy aplikacja będzie działać w trybie rozwoju lub produkcji, kod dyrektywy Angular wstępnie renderuje jako HTML na serwerze. Kod po stronie klienta wykonywany jako standardowa.

### <a name="pass-data-from-net-code-into-typescript-code"></a>Przekazywanie danych z kodu platformy .NET do kodu TypeScript

Podczas SSR możesz przekazać dane na żądanie z aplikacji platformy ASP.NET Core w kątowego aplikacji. Na przykład możesz przesłać informacje o pliku cookie lub coś odczytu z bazy danych. Aby to zrobić, Edytuj Twojej *uruchamiania* klasy. Podczas wywołania zwrotnego dla `UseSpaPrerendering`, ustaw wartość `options.SupplyData` podobny do następującego:

```csharp
options.SupplyData = (context, data) =>
{
    // Creates a new value called isHttpsRequest that is passed to TypeScript code
    data["isHttpsRequest"] = context.Request.IsHttps;
};
```

`SupplyData` Przekazania dowolnych umożliwia wywołanie zwrotne, na żądanie, dane do serializacji JSON (na przykład ciągi, wartości logiczne lub cyfry). Twoje *main.server.ts* kod odbiera jako `params.data`. Na przykład w poprzednim przykładzie kodu przekazuje wartość logiczną jako `params.data.isHttpsRequest` do `createServerRenderer` wywołania zwrotnego. To można przekazać do innych części aplikacji w żaden sposób obsługiwane przez kątową. Na przykład, zobacz temat jak *main.server.ts* przekazuje `BASE_URL` wartość każdego składnika, którego konstruktor jest zadeklarowana, aby go otrzymają.

### <a name="drawbacks-of-ssr"></a>Wady SSR

Nie wszystkie aplikacje korzystają z SSR. Podstawowy korzyści jest traktowany wydajności. Osoby odwiedzające osiągnięcia aplikacji za pośrednictwem wolnym połączeniem sieciowym lub na urządzeniach przenośnych powolne początkowej interfejsu użytkownika szybkie wyświetlenie, nawet wtedy, gdy trwa trochę czasu, aby pobrać i przeanalizować pakiety języka JavaScript. Jednak wiele źródła są używane głównie w sieciach szybkie, wewnętrzne firmy na komputerach szybkie, gdzie aplikacja będzie wyświetlana niemal natychmiast.

W tym samym czasie istnieją znaczne wady włączenia SSR. Dodaje złożoność do procesie tworzenia aplikacji. Kod muszą działać w dwóch różnych środowiskach: po stronie klienta i po stronie serwera (w środowisku Node.js wywoływany z platformy ASP.NET Core). Poniżej przedstawiono kilka sposobów pamiętać:

* SSR wymaga instalacji środowiska Node.js na serwerach produkcyjnych. Jest automatycznie dla niektórych scenariuszy wdrażania, takie jak usługi aplikacji Azure, ale nie dla innych osób, takich jak sieć szkieletowa usług Azure.
* Włączanie `BuildServerSideRenderer` flagi przyczyny kompilacji z *node_modules* katalogu publikowania. Ten folder zawiera 20 000 + plików, co zwiększa czas wdrażania.
* Do uruchomienia kodu w środowisku Node.js, go nie może polegać na istnienie specyficzne dla przeglądarki JavaScript API takich jak `window` lub `localStorage`. Jeśli kodu (lub niektóre biblioteki możesz odwoływać się do innych firm) podejmie próbę użycia tych interfejsów API, zostanie wyświetlony błąd podczas SSR. Na przykład nie używaj jQuery, ponieważ odwołuje się do interfejsów API specyficzne dla przeglądarki w wielu miejscach. Aby zapobiec błędom, możesz uniknąć SSR lub uniknąć interfejsów API specyficzne dla przeglądarki lub biblioteki. W kontroli w celu zapewnienia, że nie są one wywoływane podczas SSR może zawijać się żadnych wywołań do tych interfejsów API. W kodzie JavaScript lub maszynie, na przykład użyć wyboru podobny do następującego:

    ```javascript
    if (typeof window !== 'undefined') {
        // Call browser-specific APIs here
    }
    ```
