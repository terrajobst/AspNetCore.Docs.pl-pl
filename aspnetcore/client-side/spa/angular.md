---
title: Szablon projektu biblioteki Angular za pomocą platformy ASP.NET Core
author: SteveSandersonMS
description: Dowiedz się, jak rozpocząć pracę przy użyciu szablonu projektu ASP.NET Core jednej strony aplikacji (SPA) do usługi Angular i Angular interfejsu wiersza polecenia.
monikerRange: '>= aspnetcore-2.1'
ms.author: stevesa
ms.custom: mvc
ms.date: 03/07/2019
uid: spa/angular
ms.openlocfilehash: 6d0107ef52d63a0f6f5713c518ddc54ac4230d53
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/27/2019
ms.locfileid: "64899998"
---
# <a name="use-the-angular-project-template-with-aspnet-core"></a>Szablon projektu biblioteki Angular za pomocą platformy ASP.NET Core

Szablon projektu biblioteki Angular zaktualizowane udostępnia dogodny punkt początkowy dla platformy ASP.NET Core aplikacji za pomocą usługi Angular i Angular interfejsu wiersza polecenia, aby zaimplementować interfejs rozbudowane, po stronie klienta użytkownika (UI).

Szablon jest odpowiednikiem Tworzenie projektu ASP.NET Core jako zaplecza interfejsu API i projekt usługi Angular interfejsu wiersza polecenia pełnić rolę interfejsu użytkownika. Ten szablon oferuje wygodne hostingu obu typów projektów w projekcie pojedynczej aplikacji. W związku z tym projekt aplikacji można skompilować i opublikowane jako pojedyncza jednostka.

## <a name="create-a-new-app"></a>Tworzenie nowej aplikacji

Jeśli masz zainstalowany program ASP.NET Core 2.1, nie ma potrzeby instalowania szablon projektu biblioteki Angular.

Utwórz nowy projekt z wiersza polecenia za pomocą polecenia `dotnet new angular` w pustym katalogu. Na przykład, następujące polecenia tworzą aplikację w *Moje aplikacyjnego nowe* katalogu i przełącz się do tego katalogu:

```console
dotnet new angular -o my-new-app
cd my-new-app
```

Uruchom aplikację z programu Visual Studio lub platformy .NET Core interfejsu wiersza polecenia:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)

Otwórz wygenerowany *.csproj* pliku i uruchom aplikację w zwykły z tego miejsca.

Proces kompilacji przywraca zależności rozwiązania npm przy pierwszym uruchomieniu może potrwać kilka minut. Kolejne kompilacje są znacznie szybciej.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli/)

Upewnij się, że zmienna środowiskowa o nazwie `ASPNETCORE_Environment` o wartości `Development`. Na Windows (w monity-PowerShell), uruchom `SET ASPNETCORE_Environment=Development`. W systemie Linux lub macOS Uruchom `export ASPNETCORE_Environment=Development`.

Uruchom [kompilacji dotnet](/dotnet/core/tools/dotnet-build) Aby sprawdzić, aplikacja poprawnie się kompiluje. Przy pierwszym uruchomieniu procesu kompilacji przywraca zależności rozwiązania npm, co może zająć kilka minut. Kolejne kompilacje są znacznie szybciej.

Uruchom [dotnet, uruchom](/dotnet/core/tools/dotnet-run) Aby uruchomić aplikację. Zostanie zarejestrowany komunikat podobny do następującego:

```console
Now listening on: http://localhost:<port>
```

Przejdź do tego adresu URL w przeglądarce.

Wystąpienie serwera Angular interfejsu wiersza polecenia w tle uruchamiania aplikacji. Zostanie zarejestrowany komunikat podobny do następującego: *NG Live Development Server nasłuchuje na hoście lokalnym:&lt;otherport&gt;, otwórz przeglądarkę na http://localhost:&lt; otherport&gt;/*. Zignoruj ten komunikat&mdash;ma **nie** adres URL dla połączonych aplikacji platformy ASP.NET Core i wiersza polecenia usługi Angular.

---

Szablon projektu umożliwia utworzenie aplikacji ASP.NET Core oraz aplikacji Angular. Aplikacja platformy ASP.NET Core jest przeznaczona do użytku z dostępem do danych, autoryzację i inne problemy po stronie serwera. Platformy Angular aplikacji znajdujących się w *ClientApp* podkatalogu, jest przeznaczona do użycia dla wszystkich obaw interfejsu użytkownika.

## <a name="add-pages-images-styles-modules-etc"></a>Dodawanie stron, obrazów, style, moduły, itp.

*ClientApp* katalog zawiera standardowe aplikacji Angular interfejsu wiersza polecenia. Można znaleźć w oficjalnej [dokumentacji usługi Angular](https://github.com/angular/angular-cli/wiki) Aby uzyskać więcej informacji.

Istnieją drobne różnice między Angular aplikację utworzoną za pomocą tego szablonu, a ten, który został utworzony przez wierszem polecenia platformy Angular (za pośrednictwem `ng new`); jednak możliwości aplikacji nie ulegną zmianie. Aplikacja utworzona przez szablon zawiera [Bootstrap](https://getbootstrap.com/)— na podstawie układu oraz przykład podstawowej routingu.

## <a name="run-ng-commands"></a>Uruchom polecenia ng

W wierszu polecenia przejdź do *ClientApp* podkatalogu:

```console
cd ClientApp
```

Jeśli masz `ng` narzędzie jest zainstalowane globalnie, możesz uruchomić dowolne z jego poleceń. Na przykład, można uruchomić `ng lint`, `ng test`, ani żadnego innego [Angular polecenia interfejsu wiersza polecenia](https://github.com/angular/angular-cli/wiki#additional-commands). Nie ma potrzeby uruchomić `ng serve` , ponieważ dotyczy zarówno po stronie serwera i klienta części Twojej aplikacji obsługujących aplikacji platformy ASP.NET Core. Używa wewnętrznie, `ng serve` w trakcie opracowywania.

Jeśli nie masz `ng` narzędzie jest zainstalowane, uruchom `npm run ng` zamiast tego. Na przykład, można uruchomić `npm run ng lint` lub `npm run ng test`.

## <a name="install-npm-packages"></a>Instalowanie pakietów npm

Aby zainstalować pakiety npm innych firm, należy użyć wiersza polecenia w *ClientApp* podkatalogu. Na przykład:

```console
cd ClientApp
npm install --save <package_name>
```

## <a name="publish-and-deploy"></a>Publikowanie i wdrażanie

Na etapie opracowywania aplikacji jest uruchamiany w trybie zoptymalizowane pod kątem dla wygody deweloperów. Na przykład pakiety języka JavaScript obejmują map źródeł (tak, aby podczas debugowania, możesz zobaczyć oryginalnego kodu TypeScript). Aplikacja oczekuje na TypeScript, HTML i CSS zmian plików na dysku i automatycznie następuje rekompilacja i ponowne załadowanie po wykryciu tych plików, zmiany.

W środowisku produkcyjnym obsługiwać wersję aplikacji, która jest zoptymalizowana pod kątem wydajności. Jest ona konfigurowana automatycznie. Podczas publikowania, zminimalizowany emituje konfigurację kompilacji, z wyprzedzeniem of-time (AoT) skompilowany kompilację kodu po stronie klienta. W przeciwieństwie do tworzenia kompilacji kompilacja w produkcji nie wymaga środowiska Node.js można zainstalować na serwerze (o ile nie włączono renderowania po stronie serwera (SSR)).

Można użyć standardowego [metody hosting i wdrażanie platformy ASP.NET Core](xref:host-and-deploy/index).

## <a name="run-ng-serve-independently"></a>Uruchom niezależnie "ng służą"

Projekt jest skonfigurowana do uruchamiania własne wystąpienie serwera, usługi Angular interfejsu wiersza polecenia w tle, gdy aplikacja platformy ASP.NET Core jest uruchamiany w trybie projektowania. Jest to wygodne, ponieważ nie trzeba ręcznie uruchomić oddzielny serwer.

Brak wadą tego ustawienia domyślnego. Po każdej zmianie kodu C# i ASP.NET Core aplikacja wymaga ponownego uruchomienia, powoduje ponowne uruchomienie serwera Angular interfejsu wiersza polecenia. Około 10 sekund jest wymagana, aby rozpocząć tworzenie kopii zapasowej. Jeśli wprowadzasz częste C# kodu edycji i nie chcesz czekać na Angular interfejsu wiersza polecenia ponownie uruchomić, uruchom serwer platformy Angular interfejsu wiersza polecenia w zewnętrznie, niezależnie od procesu, ASP.NET Core. Aby to zrobić:

1. W wierszu polecenia przejdź do *ClientApp* podkatalogu, a następnie uruchom serwera wdrożeniowego programu Angular interfejsu wiersza polecenia:

    ```console
    cd ClientApp
    npm start
    ```

    > [!IMPORTANT]
    > Użyj `npm start` można uruchomić serwera wdrożeniowego programu Angular interfejsu wiersza polecenia nie `ng serve`, dzięki czemu konfiguracji w *package.json* jest zachowana. Aby przekazywać dodatkowych parametrów do serwera usługi Angular interfejsu wiersza polecenia, dodaj je do odpowiedniego `scripts` wiersz w Twojej *package.json* pliku.

2. Zmodyfikuj do korzystania z zewnętrznego wystąpienia usługi Angular interfejsu wiersza polecenia zamiast uruchamiania jednego z własnych aplikacji platformy ASP.NET Core. W swojej *uruchamiania* klasy, Zastąp `spa.UseAngularCliServer` wywołania następującym kodem:

    ```csharp
    spa.UseProxyToSpaDevelopmentServer("http://localhost:4200");
    ```

Po uruchomieniu aplikacji platformy ASP.NET Core, nie będzie go uruchomić serwera Angular interfejsu wiersza polecenia. Wystąpienie, które można uruchomić ręcznie jest używana zamiast tego. To umożliwia uruchamianie i ponowne uruchamianie szybciej. Oczekuje się już na Angular interfejsu wiersza polecenia ponownie skompilować aplikację kliencką każdorazowo.

### <a name="pass-data-from-net-code-into-typescript-code"></a>Przekazywanie danych od kodu platformy .NET do kodu TypeScript

Podczas SSR można przekazać dane na żądanie z aplikacji platformy ASP.NET Core w swojej aplikacji Angular. Na przykład można przekazywać informacje dotyczące pliku cookie lub coś odczytu z bazy danych. Aby to zrobić, Edytuj swoje *uruchamiania* klasy. Podczas wywołania zwrotnego dla `UseSpaPrerendering`, ustaw wartość `options.SupplyData` podobny do następującego:

```csharp
options.SupplyData = (context, data) =>
{
    // Creates a new value called isHttpsRequest that's passed to TypeScript code
    data["isHttpsRequest"] = context.Request.IsHttps;
};
```

`SupplyData` Umożliwia wywołanie zwrotne przekazania dowolnych, żądań, możliwy do serializacji JSON danych (na przykład ciągi, wartości logicznych lub liczb). Twoje *main.server.ts* kodu odbiera jako `params.data`. Na przykład w poprzednim przykładzie kodu przekazuje wartość typu boolean jako `params.data.isHttpsRequest` do `createServerRenderer` wywołania zwrotnego. Możesz przekazać ten do innych części aplikacji w jakikolwiek sposób obsługiwane przez usługi Angular. Na przykład, zobacz temat jak *main.server.ts* przekazuje `BASE_URL` wartość każdego składnika, której Konstruktor jest zadeklarowany do jej otrzymania.

### <a name="drawbacks-of-ssr"></a>Wady SSR

Nie wszystkie aplikacje korzystają z SSR. Główną Korzyścią płynącą jest traktowany wydajności. Osoby odwiedzające osiągnięcia swojej aplikacji za pośrednictwem z wolnym połączeniem sieciowym lub na urządzeniach przenośnych powolne początkowej interfejsu użytkownika szybkie wyświetlenie, nawet jeśli zajmuje trochę czasu, aby pobrać lub przeanalizować pakiety języka JavaScript. Jednak wiele aplikacji jednostronicowych są używane głównie za pośrednictwem sieci firmy szybkie, wewnętrzne na komputerach z szybkich, gdzie aplikacja będzie wyświetlana niemal natychmiast.

W tym samym czasie istnieją istotne wady włączania SSR. Dodaje złożoności do procesu opracowywania. Kod, należy uruchomić w dwóch różnych środowiskach: po stronie klienta i po stronie serwera (w środowisku Node.js, wywoływana z platformą ASP.NET Core). Oto kilka rzeczy, aby mieć na uwadze:

* SSR wymaga instalacji środowiska Node.js na serwerach produkcyjnych. Automatycznie jest przypadek, w przypadku niektórych scenariuszy wdrażania, takie jak usługi Azure App Services, ale nie dla inne, takie jak usługi Azure Service Fabric.
* Włączanie `BuildServerSideRenderer` kompilacji flaga powoduje, że Twoje *node_modules* katalogu publikowania. Ten folder zawiera 20 000 + plików, co zwiększa czas wdrażania.
* Aby uruchomić kod w środowisku Node.js, go nie można polegać na istnienie interfejsów API języka JavaScript w przeglądarce specyficzne takich jak `window` lub `localStorage`. Jeśli kod (lub niektórych biblioteki innych firm, którą możesz odwoływać się) próbuje użyć tych interfejsów API, otrzymasz błąd podczas SSR. Na przykład nie używaj jQuery, ponieważ widok odwołuje się interfejsami API właściwymi dla przeglądarki w wielu miejscach. Aby zapobiec wystąpieniu błędów, możesz uniknąć SSR lub uniknąć interfejsów API specyficznych dla przeglądarki lub biblioteki. Wszelkie wywołania do tych interfejsów API można opakować w kontrole, aby upewnić się, że nie są one wywoływane podczas SSR. W kodzie JavaScript i TypeScript, na przykład użyć wyboru podobny do następującego:

    ```javascript
    if (typeof window !== 'undefined') {
        // Call browser-specific APIs here
    }
    ```

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:security/authentication/identity/spa>
