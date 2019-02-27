---
title: Szablon projektu platformy React za pomocą platformy ASP.NET Core
author: SteveSandersonMS
description: Dowiedz się, jak rozpocząć pracę przy użyciu szablonu projektu ASP.NET Core jednej strony aplikacji (SPA) dla platformy React i utworzyć react aplikacji.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 02/13/2019
uid: spa/react
ms.openlocfilehash: 3b2b2e67b5d577872bafefef5624a13ca1a22449
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2019
ms.locfileid: "56899180"
---
# <a name="use-the-react-project-template-with-aspnet-core"></a>Szablon projektu platformy React za pomocą platformy ASP.NET Core

Zaktualizowany szablon projektu platformy React udostępnia dogodny punkt początkowy dla platformy ASP.NET Core z aplikacji przy użyciu platformy React i [tworzenie platformy react aplikacji](https://github.com/facebookincubator/create-react-app) konwencje (CRA), aby zaimplementować interfejs rozbudowane, po stronie klienta użytkownika (UI).

Szablon jest odpowiednikiem Tworzenie projektu ASP.NET Core jako zaplecza interfejsu API i standardowy projekt kanadyjskiej administracji podatkowej reagować na działanie jako interfejsu użytkownika, ale zapewniające wygodę hostingu, zarówno w projekcie pojedynczej aplikacji, który może być skompilowane i opublikowane jako pojedyncza jednostka.

## <a name="create-a-new-app"></a>Tworzenie nowej aplikacji

Jeśli masz zainstalowany program ASP.NET Core 2.1, nie ma potrzeby instalowania szablon projektu platformy React.

Utwórz nowy projekt z wiersza polecenia za pomocą polecenia `dotnet new react` w pustym katalogu. Na przykład, następujące polecenia tworzą aplikację w *Moje aplikacyjnego nowe* katalogu i przełącz się do tego katalogu:

```console
dotnet new react -o my-new-app
cd my-new-app
```

Uruchom aplikację z programu Visual Studio lub platformy .NET Core interfejsu wiersza polecenia:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Otwórz wygenerowany *.csproj* pliku i uruchom aplikację w zwykły z tego miejsca.

Proces kompilacji przywraca zależności rozwiązania npm przy pierwszym uruchomieniu może potrwać kilka minut. Kolejne kompilacje są znacznie szybciej.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

Upewnij się, że zmienna środowiskowa o nazwie `ASPNETCORE_Environment` wartością `Development`. Na Windows (w monity-PowerShell), uruchom `SET ASPNETCORE_Environment=Development`. W systemie Linux lub macOS Uruchom `export ASPNETCORE_Environment=Development`.

Uruchom [kompilacji dotnet](/dotnet/core/tools/dotnet-build) Aby sprawdzić, aplikacja poprawnie się kompiluje. Przy pierwszym uruchomieniu procesu kompilacji przywraca zależności rozwiązania npm, co może zająć kilka minut. Kolejne kompilacje są znacznie szybciej.

Uruchom [dotnet, uruchom](/dotnet/core/tools/dotnet-run) Aby uruchomić aplikację.

---

Szablon projektu umożliwia utworzenie aplikacji platformy ASP.NET Core i aplikacji w języku React. Aplikacja platformy ASP.NET Core jest przeznaczona do użytku z dostępem do danych, autoryzację i inne problemy po stronie serwera. Aplikacja platformy React, znajdującej się w *ClientApp* podkatalogu, jest przeznaczona do użycia dla wszystkich obaw interfejsu użytkownika.

## <a name="add-pages-images-styles-modules-etc"></a>Dodawanie stron, obrazów, style, moduły, itp.

*ClientApp* katalog jest standardowa aplikacja kanadyjskiej administracji podatkowej reagować. Można znaleźć w oficjalnej [dokumentacji kanadyjskiej administracji podatkowej](https://github.com/facebookincubator/create-react-app/blob/master/packages/react-scripts/template/README.md) Aby uzyskać więcej informacji.

Istnieją drobne różnice między aplikacją platformy React, utworzony za pomocą tego szablonu i utworzonym przez kanadyjskiej administracji podatkowej. jednak możliwości aplikacji nie uległy zmianie. Aplikacja utworzona przez szablon zawiera [Bootstrap](https://getbootstrap.com/)— na podstawie układu oraz przykład podstawowej routingu.

## <a name="install-npm-packages"></a>Instalowanie pakietów npm

Aby zainstalować pakiety npm innych firm, należy użyć wiersza polecenia w *ClientApp* podkatalogu. Na przykład:

```console
cd ClientApp
npm install --save <package_name>
```

## <a name="publish-and-deploy"></a>Publikowanie i wdrażanie

Na etapie opracowywania aplikacji jest uruchamiany w trybie zoptymalizowane pod kątem dla wygody deweloperów. Na przykład pakiety języka JavaScript obejmują map źródeł (tak, aby podczas debugowania, możesz zobaczyć oryginalnego kodu źródłowego). Aplikacja oczekuje JavaScript, HTML i CSS, zmiany plików na dysku i automatycznie następuje rekompilacja i ponowne załadowanie po wykryciu tych plików, zmiany.

W środowisku produkcyjnym obsługiwać wersję aplikacji, która jest zoptymalizowana pod kątem wydajności. Jest ona konfigurowana automatycznie. Podczas publikowania, konfigurację kompilacji emituje zminimalizowany, kompilacja transpiled kodu po stronie klienta. W przeciwieństwie do tworzenia kompilacji kompilacja w produkcji nie wymaga środowiska Node.js można zainstalować na serwerze.

Można użyć standardowego [metody hosting i wdrażanie platformy ASP.NET Core](xref:host-and-deploy/index).

## <a name="run-the-cra-server-independently"></a>Uruchom serwer kanadyjskiej administracji podatkowej niezależnie

Projekt jest skonfigurowana do uruchamiania wystąpienia serwera wdrożeniowego programu kanadyjskiej administracji podatkowej w tle, gdy aplikacja platformy ASP.NET Core jest uruchamiany w trybie projektowania. Jest to wygodne, ponieważ oznacza to, że nie trzeba ręcznie uruchomić oddzielny serwer.

Brak wadą tego ustawienia domyślnego. Po każdej zmianie kodu C# i ASP.NET Core aplikacja wymaga ponownego uruchomienia, serwer kanadyjskiej administracji podatkowej zostanie ponownie uruchomiony. Kilka sekund, należy uruchomić tworzenie kopii zapasowej. Jeśli wprowadzasz częste C# kodu edycji i nie chcesz czekać, aż serwer kanadyjskiej administracji podatkowej ponownie uruchomić, uruchom serwer kanadyjskiej administracji podatkowej zewnętrznie, niezależnie od procesu, ASP.NET Core. Aby to zrobić:

1. Dodaj *ENV* plik *ClientApp* podkatalogu z następującymi ustawieniami:

    ```
    BROWSER=none
    ```
    
    Uniemożliwi to przeglądarki sieci web otwierania podczas uruchamiania serwera kanadyjskiej administracji podatkowej zewnętrznie.

2. W wierszu polecenia przejdź do *ClientApp* podkatalogu, a następnie uruchom serwera wdrożeniowego programu kanadyjskiej administracji podatkowej:

    ```console
    cd ClientApp
    npm start
    ```

3. Zmodyfikować aplikację platformy ASP.NET Core w taki sposób, aby użyć wystąpienia zewnętrznego serwera kanadyjskiej administracji podatkowej zamiast uruchamiania jednego z własnych. W swojej *uruchamiania* klasy, Zastąp `spa.UseReactDevelopmentServer` wywołania następującym kodem:

    ```csharp
    spa.UseProxyToSpaDevelopmentServer("http://localhost:3000");
    ```

Po uruchomieniu aplikacji platformy ASP.NET Core, nie będzie on uruchomienia serwera kanadyjskiej administracji podatkowej. Wystąpienie, które można uruchomić ręcznie jest używana zamiast tego. To umożliwia uruchamianie i ponowne uruchamianie szybciej. Oczekuje się już na aplikację platformy React, aby odbudować każdorazowo.

> [!IMPORTANT]
> "Renderowania po stronie serwera" nie jest obsługiwaną funkcją tego szablonu. Naszym celem, korzystając z tego szablonu jest spełniają zgodność z "Utwórz react-app". Jako takie scenariusze i funkcje, które nie są uwzględnione w projekcie "Utwórz react-app" (na przykład SSR) nie są obsługiwane i są pozostawiane w charakterze ćwiczenia dla użytkownika.
