---
title: Użyj szablonu projektu kątowego z ASP.NET Core
author: SteveSandersonMS
description: Dowiedz się, jak rozpocząć pracę z szablonem projektu aplikacji jednostronicowej (SPA) ASP.NET Core dla elementu kątowego i kątowego interfejsu wiersza polecenia.
monikerRange: '>= aspnetcore-2.1'
ms.author: stevesa
ms.custom: mvc
ms.date: 02/06/2020
uid: spa/angular
ms.openlocfilehash: fee872ff237e14cbe491efed9b320809df4c5654
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78657634"
---
# <a name="use-the-angular-project-template-with-aspnet-core"></a>Użyj szablonu projektu kątowego z ASP.NET Core

Zaktualizowany szablon projektu kątowego zapewnia wygodny punkt początkowy dla ASP.NET Core aplikacji przy użyciu funkcji kątowych i kątowych interfejsu wiersza polecenia, aby zaimplementować rozbudowany interfejs użytkownika po stronie klienta.

Szablon jest równoznaczny z tworzeniem ASP.NET Core projektu do działania jako zaplecza interfejsu API oraz projektu kątowego interfejsu wiersza polecenia do działania jako interfejs użytkownika. Szablon oferuje wygodę hostingu obu typów projektów w jednym projekcie aplikacji. W związku z tym projekt aplikacji można skompilować i opublikować jako pojedynczą jednostkę.

## <a name="create-a-new-app"></a>Tworzenie nowej aplikacji

Jeśli zainstalowano ASP.NET Core 2,1, nie ma potrzeby instalowania szablonu projektu kątowego.

Utwórz nowy projekt z wiersza polecenia przy użyciu polecenia `dotnet new angular` w pustym katalogu. Na przykład następujące polecenia tworzą aplikację w katalogu *My-New-App* i przełączają się do tego katalogu:

```dotnetcli
dotnet new angular -o my-new-app
cd my-new-app
```

Uruchom aplikację z poziomu programu Visual Studio lub interfejs wiersza polecenia platformy .NET Core:

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio/)

Otwórz wygenerowany plik *csproj* i uruchom aplikację w zwykły sposób.

Proces kompilacji przywraca zależności npm w pierwszym przebiegu, co może potrwać kilka minut. Kolejne kompilacje są znacznie szybsze.

# <a name="net-core-cli"></a>[.NET Core CLI](#tab/netcore-cli/)

Upewnij się, że masz zmienną środowiskową o nazwie `ASPNETCORE_Environment` z wartością `Development`. W systemie Windows (w komunikatach innych niż programu PowerShell) Uruchom `SET ASPNETCORE_Environment=Development`. W systemie Linux lub macOS Uruchom `export ASPNETCORE_Environment=Development`.

Uruchom [kompilację dotnet](/dotnet/core/tools/dotnet-build) , aby sprawdzić, czy aplikacja jest poprawnie skompilowana. W pierwszym uruchomieniu proces kompilacji przywraca zależności npm, co może potrwać kilka minut. Kolejne kompilacje są znacznie szybsze.

Uruchom [uruchomienie programu dotnet](/dotnet/core/tools/dotnet-run) , aby uruchomić aplikację. Rejestrowany jest komunikat podobny do następującego:

```console
Now listening on: http://localhost:<port>
```

Przejdź do tego adresu URL w przeglądarce.

> [!WARNING]
> Aplikacja uruchamia wystąpienie kątowego serwera interfejsu wiersza polecenia w tle. Rejestrowany jest komunikat podobny do następującego: Usługa *anie na żywo serwer programistyczny nasłuchuje na hoście lokalnym:&lt;otherport&gt;, Otwórz przeglądarkę, aby http://localhost:&ltotherport&gt;/* . Zignoruj ten komunikat,&mdash;**nie** jest to adres URL połączonej ASP.NET Core i aplikacji interfejsu wiersza polecenia.

---

Szablon projektu tworzy aplikację ASP.NET Core i aplikację kątową. Aplikacja ASP.NET Core jest przeznaczona do użycia na potrzeby dostępu do danych, autoryzacji i innych zagadnień po stronie serwera. Aplikacja kątowa znajdująca się w podkatalogu *ClientApp* jest przeznaczona do użycia dla wszystkich zagadnień związanych z interfejsem użytkownika.

## <a name="add-pages-images-styles-modules-etc"></a>Dodawanie stron, obrazów, stylów, modułów itp.

Katalog *ClientApp* zawiera standardową aplikację interfejsu wiersza polecenia. Aby uzyskać więcej informacji, zobacz oficjalną [dokumentację kątową](https://angular.io) .

Istnieją niewielkie różnice między aplikacją kątową utworzoną przez ten szablon a nią utworzoną przez skośny interfejs wiersza polecenia (za pośrednictwem `ng new`); jednak możliwości aplikacji nie są zmieniane. Aplikacja utworzona przez szablon zawiera układ oparty na [Bootstrap](https://getbootstrap.com/)i podstawowy przykład routingu.

## <a name="run-ng-commands"></a>Uruchamianie poleceń ng

W wierszu polecenia przejdź do podkatalogu *ClientApp* :

```console
cd ClientApp
```

Jeśli masz narzędzie `ng` zainstalowane globalnie, możesz uruchomić dowolne z jego poleceń. Można na przykład uruchomić `ng lint`, `ng test`lub dowolne z innych [poleceń kątowych interfejsu wiersza polecenia](https://angular.io/cli). Nie ma potrzeby uruchamiania `ng serve` mimo że aplikacja ASP.NET Core obsługuje zarówno części aplikacji po stronie serwera, jak i klienta. Wewnętrznie używa `ng serve` w trakcie opracowywania.

Jeśli nie masz zainstalowanego narzędzia `ng`, uruchom polecenie `npm run ng` zamiast tego. Można na przykład uruchomić `npm run ng lint` lub `npm run ng test`.

## <a name="install-npm-packages"></a>Zainstaluj pakiety npm

Aby zainstalować pakiety npm innych firm, należy użyć wiersza polecenia w podkatalogu *ClientApp* . Na przykład:

```console
cd ClientApp
npm install --save <package_name>
```

## <a name="publish-and-deploy"></a>Publikowanie i wdrażanie

W trakcie opracowywania aplikacja jest uruchamiana w trybie zoptymalizowanym pod kątem wygody dla deweloperów. Na przykład pakiety JavaScript zawierają mapy źródłowe (w przypadku debugowania można zobaczyć oryginalny kod TypeScript). Aplikacja będzie oglądać zmiany w plikach TypeScript, HTML i CSS na dysku, a następnie automatycznie ponownie kompilować i ładuje je, gdy zobaczy zmiany tych plików.

W środowisku produkcyjnym można korzystać z wersji aplikacji zoptymalizowanej pod kątem wydajności. To jest skonfigurowane tak, aby były wykonywane automatycznie. Podczas publikowania konfiguracja kompilacji emituje kompilowaną kompilację kodu po stronie klienta (zminimalizowanego). W przeciwieństwie do kompilacji deweloperskiej kompilacja produkcyjna nie wymaga zainstalowania na serwerze programu Node. js (o ile nie włączono renderowania po stronie serwera (SSR)).

Można użyć standardowych [ASP.NET Core metod hostingu i wdrażania](xref:host-and-deploy/index).

## <a name="run-ng-serve-independently"></a>Uruchom "anie" niezależnie

Projekt jest skonfigurowany tak, aby uruchamiał własne wystąpienie kątowego serwera interfejsu wiersza polecenia w tle podczas uruchamiania aplikacji ASP.NET Core w trybie tworzenia. Jest to wygodne, ponieważ nie trzeba ręcznie uruchamiać oddzielnego serwera.

Istnieje zwrot do tej konfiguracji domyślnej. Za każdym razem, gdy C# modyfikujesz kod i aplikacja ASP.NET Core wymaga ponownego uruchomienia, serwer interfejsu wiersza polecenia jest uruchamiany ponownie. Aby rozpocząć tworzenie kopii zapasowej, wymagane jest około 10 sekund. Jeśli wprowadzasz częste C# zmiany kodu i nie chcesz czekać na ponowne uruchomienie interfejsu wiersza polecenia, uruchom skośny serwer interfejsu wiersza polecenia, niezależnie od procesu ASP.NET Core. W tym celu:

1. W wierszu polecenia przejdź do podkatalogu *ClientApp* i uruchom kątowy serwer programistyczny CLI:

    ```console
    cd ClientApp
    npm start
    ```

    > [!IMPORTANT]
    > Użyj `npm start`, aby uruchomić kątowy serwer programistyczny interfejsu wiersza polecenia, a nie `ng serve`, tak aby konfiguracja w pliku *Package. JSON* była respektowana. Aby przekazać dodatkowe parametry do serwera kątowego interfejsu wiersza polecenia, należy dodać je do odpowiedniego wiersza `scripts` w pliku *Package. JSON* .

2. Zmodyfikuj aplikację ASP.NET Core tak, aby korzystała z zewnętrznego wystąpienia interfejsu wiersza polecenia, zamiast uruchamiania jednego z nich. W klasie *uruchomieniowej* Zastąp wywołanie `spa.UseAngularCliServer`, wykonując następujące czynności:

    ```csharp
    spa.UseProxyToSpaDevelopmentServer("http://localhost:4200");
    ```

Po uruchomieniu aplikacji ASP.NET Core nie zostanie uruchomiony kątowy serwer interfejsu wiersza polecenia. Wystąpienie, które zostało uruchomione ręcznie, jest używane w zamian. Pozwala to na szybkie uruchamianie i ponowne uruchamianie. Nie czeka już na kątowy interfejs wiersza polecenia, aby ponownie skompilować aplikację kliencką za każdym razem.

### <a name="pass-data-from-net-code-into-typescript-code"></a>Przekazywanie danych z kodu platformy .NET do kodu TypeScript

Podczas procesu SSR możesz chcieć przekazać dane dla żądania z aplikacji ASP.NET Core do aplikacji kątowej. Na przykład można przekazać informacje o pliku cookie lub coś odczytanego z bazy danych. Aby to zrobić, Edytuj klasę *uruchomieniową* . W polu wywołania zwrotne dla `UseSpaPrerendering`ustaw wartość dla `options.SupplyData` na przykład:

```csharp
options.SupplyData = (context, data) =>
{
    // Creates a new value called isHttpsRequest that's passed to TypeScript code
    data["isHttpsRequest"] = context.Request.IsHttps;
};
```

Wywołanie zwrotne `SupplyData` umożliwia przekazywanie dowolnych danych z możliwością serializacji kodu JSON (na przykład ciągów, wartości logicznych lub cyfr). Kod *Main. Server. TS* odbiera ten `params.data`. Na przykład poprzedni przykładowy kod przekazuje wartość logiczną jako `params.data.isHttpsRequest` do `createServerRenderer` wywołania zwrotnego. Można przekazać ten program do innych części aplikacji w dowolny sposób, który jest obsługiwany przez kątowe. Na przykład, zobacz jak *Main. Server. TS* przekazuje wartość `BASE_URL` do dowolnego składnika, którego Konstruktor został zadeklarowany do odbierania.

### <a name="drawbacks-of-ssr"></a>Wady systemu SSR

Nie wszystkie aplikacje korzystają z programu SSR. Główną korzyścią jest postrzeganie wydajności. Goście korzystający z aplikacji przez wolne połączenie sieciowe lub wolne urządzenia przenośne szybko zobaczą początkowy interfejs użytkownika, nawet jeśli pobieranie lub analizowanie pakietów JavaScript trwa dłużej. Jednak wiele aplikacji jednostronicowych jest używanych głównie w szybkich, wewnętrznych sieciach firmowych na szybkich komputerach, na których aplikacja jest widoczna niemal natychmiast.

W tym samym czasie istnieją znaczne wady umożliwiające włączenie SSR. Zwiększa złożoność procesu tworzenia. Kod musi działać w dwóch różnych środowiskach: po stronie klienta i po stronie serwera (w środowisku Node. js wywoływanym z ASP.NET Core). Oto kilka rzeczy, na których warto mieć świadomość:

* SSR wymaga instalacji środowiska Node. js na serwerach produkcyjnych. W przypadku niektórych scenariuszy wdrażania, takich jak Azure App Services, ale nie dla innych, takich jak Azure Service Fabric.
* Włączenie flagi `BuildServerSideRenderer` Build powoduje, że katalog *node_modules* do opublikowania. Ten folder zawiera 20 000 plików, co wydłuża czas wdrażania.
* Do uruchamiania kodu w środowisku Node. js nie można polegać na istnieniu interfejsów API JavaScript specyficznych dla przeglądarki, takich jak `window` lub `localStorage`. Jeśli Twój kod (lub pewna dostosowana Biblioteka innej firmy) próbuje użyć tych interfejsów API, wystąpi błąd podczas procesu SSR. Na przykład nie należy używać jQuery, ponieważ odwołuje się do interfejsów API specyficznych dla przeglądarki w wielu miejscach. Aby zapobiec błędom, należy koniecznie uniknąć lub uniknąć używania interfejsów API lub bibliotek specyficznych dla przeglądarki. Możesz otoczyć wszystkie wywołania takich interfejsów API w celu upewnienia się, że nie są one wywoływane podczas SSR. Na przykład użyj sprawdzenia, takiego jak następujące w kodzie JavaScript lub TypeScript:

    ```javascript
    if (typeof window !== 'undefined') {
        // Call browser-specific APIs here
    }
    ```

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:security/authentication/identity/spa>
