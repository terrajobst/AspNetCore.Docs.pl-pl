---
title: "Należy użyć szablonu projektu w bibliotece React."
author: SteveSandersonMS
description: "Dowiedz się, jak rozpocząć pracę z szablonu projektu platformy ASP.NET Core jednostronicowej aplikacji JEDNOSTRONICOWEJ release candidate dla platformy React i utworzyć platformy react aplikacji."
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 12/06/2017
ms.devlang: csharp
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: spa/react
ms.openlocfilehash: 5978094083a098a771f5dca103434ea8fcce7777
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
# <a name="use-the-react-project-template-release-candidate"></a>Należy użyć szablonu projektu platformy React (w wersji release candidate)

> [!NOTE]
> Ta dokumentacja nie jest o wydanych szablonu projektu platformy React. **Ta dokumentacja dotyczy wersji release candidate szablonu platformy React.** Mamy nadzieję, należy wysłać wersji wydanej w wczesne 2018.

Zaktualizowany szablon projektu platformy React udostępnia dogodny punkt początkowy dla platformy ASP.NET Core aplikacji przy użyciu platformy React i [utworzyć platformy react aplikacji](https://github.com/facebookincubator/create-react-app) konwencje (CRA) do zaimplementowania rozbudowanego klienta interfejsu użytkownika (UI).

Szablon jest odpowiednikiem Tworzenie projektu platformy ASP.NET Core do działania jako zaplecza interfejsu API i standardowe projektu CRA zareagować na działanie jako interfejsu użytkownika, ale z elastycznością obsługi zarówno w projekcie jednej aplikacji, które mogą być wbudowane i opublikowane jako pojedyncza jednostka.

## <a name="create-a-new-app"></a>Tworzenie nowej aplikacji

Aby rozpocząć, upewnij się, które zostały [zainstalowany zaktualizowany szablon projektu platformy React](xref:spa/index#installation). Te instrukcje nie dotyczą poprzedniego szablonu projektu platformy React objęte .NET Core 2.0.x zestawu SDK.

Utwórz nowy projekt z wiersza polecenia przy użyciu polecenia `dotnet new react` w pustych katalogów. Na przykład poniższe polecenia Utwórz aplikację w *— nowy — aplikacji my* katalogu i przełącznika do tego katalogu:

```console
dotnet new react -o my-new-app
cd my-new-app
```

Uruchamianie aplikacji z programu Visual Studio lub platformy .NET Core interfejsu wiersza polecenia:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Otwórz wygenerowany *.csproj* pliku i uruchom aplikację w zwykły stamtąd.

Proces kompilacji przywraca npm zależności przy pierwszym uruchomieniu, co może zająć kilka minut. Kolejne kompilacje są znacznie szybsze.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

Upewnij się, masz zmienną środowiskową o nazwie `ASPNETCORE_Environment` o wartości `Development`. W systemie Windows (w monity-PowerShell), uruchom `SET ASPNETCORE_Environment=Development`. W systemie Linux lub macOS Uruchom `export ASPNETCORE_Environment=Development`.

Uruchom `dotnet build` można zweryfikować aplikacji kompilacje poprawnie. Przy pierwszym uruchomieniu procesu kompilacji przywraca npm zależności, co może potrwać kilka minut. Kolejne kompilacje są znacznie szybsze.

Uruchom `dotnet run` do uruchomienia aplikacji.

---

Szablon projektu tworzy aplikację platformy ASP.NET Core i aplikacji platformy React. Aplikacja platformy ASP.NET Core jest przeznaczona do użycia dla dostępu do danych, autoryzację i inne problemy po stronie serwera. Aplikacja platformy React, znajdującej się w *ClientApp* podkatalogu, jest przeznaczona do użycia dla wszystkich problemów w zakresie interfejsu użytkownika.

## <a name="add-pages-images-styles-modules-etc"></a>Dodawanie stron, obrazy, style, modułów, itp.

*ClientApp* katalog jest standardowych aplikacji CRA reagować. Można znaleźć w oficjalnej [dokumentacji CRA](https://github.com/facebookincubator/create-react-app/blob/master/packages/react-scripts/template/README.md) Aby uzyskać więcej informacji.

Niewielkich różnic między aplikacją platformy React, utworzony przez ten szablon i utworzonym przez CRA jednak możliwości aplikacji nie uległy zmianie. Utworzona przez szablon aplikacja zawiera [Bootstrap](https://getbootstrap.com/)— na podstawie układu i prosty przykład routingu.

## <a name="install-npm-packages"></a>Instalowanie pakietów npm

Aby zainstalować pakiety innych firm npm, przy użyciu wiersza polecenia w *ClientApp* podkatalogu. Na przykład:

```console
cd ClientApp
npm install --save <package_name>
```

## <a name="publish-and-deploy"></a>Publikowanie i wdrażanie

W rozwoju aplikacji jest uruchamiany w trybie zoptymalizowane pod kątem wygody deweloperów. Na przykład pakiety JavaScript obejmują map źródeł (tak, aby podczas debugowania, możesz zobaczyć kodu). Aplikacja oczekuje JavaScript, HTML i CSS pliku zmiany na dysku i automatycznie rekompiluje i ponowne załadowanie po wykryciu tych plików, zmiany.

W środowisku produkcyjnym obsługiwać wersji aplikacji jest zoptymalizowany pod kątem wydajności. To jest skonfigurowany automatycznie. Podczas publikowania, konfiguracja kompilacji emituje zminimalizowany, kompilacji transpiled kodu po stronie klienta. W przeciwieństwie do kompilacji programowanie kompilacji produkcyjnym nie wymaga Node.js do zainstalowania na serwerze.

Można użyć standardowego [metody wdrażania i hostingu ASP.NET Core](xref:host-and-deploy/index).

## <a name="run-the-cra-server-independently"></a>Uruchom serwer CRA niezależnie

Projekt jest skonfigurowany do uruchamiania własnego wystąpienia serwera wdrożeniowego CRA w tle po uruchomieniu aplikacji platformy ASP.NET Core w trybie projektowania. Ta opcja jest przydatna, ponieważ oznacza to, że nie trzeba ręcznie uruchomić oddzielny serwer.

Brak wadą tego ustawienia domyślne. Po każdej zmianie kodu C# i ASP.NET podstawowych aplikacja musi ponownie uruchomić, CRA serwer zostanie ponownie uruchomiony. Kilka sekund, należy rozpocząć tworzenie kopii zapasowej. Jeśli wprowadzania częste zmiany kodu C# i nie chcesz czekać na ponowne uruchomienie serwera CRA, uruchom serwer CRA zewnętrznie, niezależnie od procesu ASP.NET Core. Aby to zrobić:

1. W wierszu polecenia, przełącz się do *ClientApp* podkatalogu, a następnie uruchom serwera wdrożeniowego CRA:

    ```console
    cd ClientApp
    npm start
    ```

2. Zmodyfikuj do korzystania z zewnętrznego wystąpienia serwera CRA zamiast uruchamiania jednego z jego własnej aplikacji platformy ASP.NET Core. W Twojej *uruchamiania* klasy, Zastąp `spa.UseReactDevelopmentServer` wywołania z następujących czynności:

    ```csharp
    spa.UseProxyToSpaDevelopmentServer("http://localhost:3000");
    ```

Po uruchomieniu aplikacji platformy ASP.NET Core, nie będzie on uruchomienia serwera CRA. Zamiast niego jest używana wystąpienia, które zostanie uruchomione ręcznie. Dzięki temu może uruchomić i ponownie uruchom szybciej. Nie jest już oczekuje dla aplikacji platformy React odbudować zawsze.
