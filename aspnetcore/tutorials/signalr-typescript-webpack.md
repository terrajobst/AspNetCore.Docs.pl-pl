---
title: Korzystanie z ASP.NET Core SignalR z użyciem języka TypeScript i pakietu WebPack
author: ssougnez
description: W tym samouczku skonfigurujesz pakiet WebPack do tworzenia pakietów i kompilowania ASP.NET Core SignalR aplikacji sieci Web, której klient został zapisany w języku TypeScript.
ms.author: bradyg
ms.custom: mvc
ms.date: 11/21/2019
no-loc:
- SignalR
uid: tutorials/signalr-typescript-webpack
ms.openlocfilehash: a7c99c9e79647995886aec5b3a91584fd2f24451
ms.sourcegitcommit: 3e503ef510008e77be6dd82ee79213c9f7b97607
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/22/2019
ms.locfileid: "74317486"
---
# <a name="use-aspnet-core-opno-locsignalr-with-typescript-and-webpack"></a>Korzystanie z ASP.NET Core SignalR z użyciem języka TypeScript i pakietu WebPack

Autorzy [Sébastien Sougnez](https://twitter.com/ssougnez) i [Scott Addie](https://twitter.com/Scott_Addie)

[Pakiet WebPack](https://webpack.js.org/) umożliwia deweloperom tworzenie i kompilowanie zasobów po stronie klienta aplikacji sieci Web. W tym samouczku pokazano, jak używać pakietu WebPack w ASP.NET Core SignalR aplikacji sieci Web, której klient został zapisany w języku [TypeScript](https://www.typescriptlang.org/).

Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Tworzenie szkieletu ASP.NET Core aplikacji SignalR Starter
> * Konfigurowanie SignalR klienta TypeScript
> * Konfigurowanie potoku kompilacji przy użyciu pakietu WebPack
> * Skonfiguruj serwer SignalR
> * Włącz komunikację między klientem a serwerem

[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/signalr-typescript-webpack/sample) ([jak pobrać](xref:index#how-to-download-a-sample))

::: moniker range=">= aspnetcore-3.0"

## <a name="prerequisites"></a>Wymagania wstępne

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* [Program Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) z **ASP.NET i programowaniem aplikacji sieci Web**
* [Zestaw .NET Core SDK 3,0 lub nowszy](https://www.microsoft.com/net/download/all)
* [Node. js](https://nodejs.org/) z [npm](https://www.npmjs.com/)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* [Visual Studio Code](https://code.visualstudio.com/download)
* [Zestaw .NET Core SDK 3,0 lub nowszy](https://www.microsoft.com/net/download/all)
* [C#dla Visual Studio Code w wersji 1.17.1 lub nowszej](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [Node. js](https://nodejs.org/) z [npm](https://www.npmjs.com/)

---

## <a name="create-the-aspnet-core-web-app"></a>Tworzenie aplikacji sieci Web ASP.NET Core

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Skonfiguruj program Visual Studio, aby szukać npm w zmiennej środowiskowej *Path* . Domyślnie program Visual Studio używa wersji npm znajdującej się w katalogu instalacyjnym. Wykonaj te instrukcje w programie Visual Studio:

1. Przejdź do **opcji narzędzia** > **Opcje** > **projekty i rozwiązania** > **sieci Web zarządzanie pakietami** > **zewnętrznych narzędzi sieci Web**.
1. Wybierz z listy wpis *$ (Path)* . Kliknij strzałkę w górę, aby przenieść wpis do drugiej pozycji na liście.

    ![Konfiguracja programu Visual Studio](signalr-typescript-webpack/_static/signalr-configure-path-visual-studio.png)

Konfiguracja programu Visual Studio została ukończona. Czas na utworzenie projektu.

1. Użyj opcji **plik** > **Nowy** > **projekt** , a następnie wybierz szablon **aplikacja sieci Web ASP.NET Core** .
1. Nazwij projekt *SignalRWebPack*, a następnie wybierz pozycję **Utwórz**.
1. Wybierz pozycję *.NET Core* z listy rozwijanej platforma docelowa, a następnie wybierz pozycję *ASP.NET Core 3,0* z listy rozwijanej selektora struktury. Wybierz **pusty** szablon i wybierz pozycję **Utwórz**.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Uruchom następujące polecenie w **zintegrowanym terminalu**:

```dotnetcli
dotnet new web -o SignalRWebPack
```

Pusta aplikacja sieci Web ASP.NET Core, ukierunkowana na .NET Core, jest tworzona w katalogu *SignalRWebPack* .

---

## <a name="configure-webpack-and-typescript"></a>Konfigurowanie pakietu WebPack i języka TypeScript

Poniższe kroki umożliwiają skonfigurowanie konwersji języka TypeScript na JavaScript i zgrupowanie zasobów po stronie klienta.

1. Wykonaj następujące polecenie w katalogu głównym projektu, aby utworzyć plik *Package. JSON* :

    ```console
    npm init -y
    ```

1. Dodaj wyróżnioną właściwość do pliku *Package. JSON* :

    [!code-json[package.json](signalr-typescript-webpack/sample/3.x/snippets/package1.json?highlight=4)]

    Ustawienie właściwości `private` na `true` uniemożliwia ostrzeżenia instalacji pakietu w następnym kroku.

1. Zainstaluj wymagane pakiety npm. Wykonaj następujące polecenie w katalogu głównym projektu:

    ```console
    npm install -D -E clean-webpack-plugin@1.0.1 css-loader@2.1.0 html-webpack-plugin@4.0.0-beta.5 mini-css-extract-plugin@0.5.0 ts-loader@5.3.3 typescript@3.3.3 webpack@4.29.3 webpack-cli@3.2.3
    ```

    Niektóre szczegóły polecenia do uwagi:

    * Numer wersji jest zgodny ze znakiem `@` dla każdej nazwy pakietu. npm instaluje te określone wersje pakietu.
    * Opcja `-E` wyłącza domyślne zachowanie npm podczas pisania operatorów zakresu [wersji semantycznej](https://semver.org/) w pliku *Package. JSON*. Na przykład `"webpack": "4.29.3"` jest używany zamiast `"webpack": "^4.29.3"`. Ta opcja Zapobiega niezamierzonym uaktualnianiu do nowszych wersji pakietu.

    Aby uzyskać więcej informacji, zobacz oficjalne dokumenty z [npm-Install](https://docs.npmjs.com/cli/install) .

1. Zastąp Właściwość `scripts` pliku *Package. JSON* następującym fragmentem kodu:

    ```json
    "scripts": {
      "build": "webpack --mode=development --watch",
      "release": "webpack --mode=production",
      "publish": "npm run release && dotnet publish -c Release"
    },
    ```

    Niektóre objaśnienia dotyczące skryptów:

    * `build`: pakietuje zasoby po stronie klienta w trybie tworzenia i czujki pod kątem zmian w pliku. Obserwator plików powoduje, że pakiet jest generowany ponownie za każdym razem, gdy plik projektu jest zmieniany. Opcja `mode` wyłącza optymalizacje produkcyjne, takie jak wstrząsanie i minifikacjaowanie drzewa. `build` w trakcie opracowywania.
    * `release`: pakietuje zasoby po stronie klienta w trybie produkcyjnym.
    * `publish`: uruchamia skrypt `release`, aby powiązać zasoby po stronie klienta w trybie produkcyjnym. Wywołuje polecenie [publikowania](/dotnet/core/tools/dotnet-publish) interfejs wiersza polecenia platformy .NET Core, aby opublikować aplikację.

1. Utwórz plik o nazwie *WebPack. config. js*w katalogu głównym projektu o następującej zawartości:

    [!code-javascript[webpack.config.js](signalr-typescript-webpack/sample/3.x/webpack.config.js)]

    Poprzedni plik konfiguruje kompilację pakietu WebPack. Niektóre szczegóły konfiguracji do uwagi:

    * Właściwość `output` przesłania domyślną wartość *rozkłu*. Pakiet jest emitowany w katalogu *wwwroot* .
    * Tablica `resolve.extensions` zawiera *js* do zaimportowania klienta SignalR JavaScript.

1. Utwórz nowy katalog *src* w katalogu głównym projektu. Celem jest przechowywanie zasobów po stronie klienta.

1. Utwórz *src/index.html* z następującą zawartością.

    [!code-html[index.html](signalr-typescript-webpack/sample/3.x/src/index.html)]

    Powyższy kod HTML definiuje standardowe znaczniki strony głównej.

1. Utwórz nowy katalog *src/CSS* . Celem jest przechowywanie plików *CSS* projektu.

1. Utwórz *src/CSS/Main. css* z następującą zawartością:

    [!code-css[main.css](signalr-typescript-webpack/sample/3.x/src/css/main.css)]

    Poprzedni plik *Main. css* jest stylem aplikacji.

1. Utwórz plik *src/tsconfig. JSON* z następującą zawartością:

    [!code-json[tsconfig.json](signalr-typescript-webpack/sample/3.x/src/tsconfig.json)]

    Poprzedni kod konfiguruje kompilator języka TypeScript w celu utworzenia kodu JavaScript zgodnego z [ECMAScript](https://wikipedia.org/wiki/ECMAScript) 5.

1. Utwórz *src/index. TS* z następującą zawartością:

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/3.x/snippets/index1.ts?name=snippet_IndexTsPhase1File)]

    Poprzedni kod TypeScript pobiera odwołania do elementów DOM i dołącza dwa programy obsługi zdarzeń:

    * `keyup`: to zdarzenie jest wyzwalane, gdy użytkownik wpisze coś w polu tekstowym określonym jako `tbMessage`. Funkcja `send` jest wywoływana, gdy użytkownik naciśnie klawisz **Enter** .
    * `click`: to zdarzenie jest wyzwalane, gdy użytkownik kliknie przycisk **Wyślij** . Funkcja `send` jest wywoływana.

## <a name="configure-the-aspnet-core-app"></a>Konfigurowanie aplikacji ASP.NET Core

1. W metodzie `Startup.Configure` Dodaj wywołania do [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) i [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_).

   [!code-csharp[Startup](signalr-typescript-webpack/sample/3.x/Startup.cs?name=snippet_UseStaticDefaultFiles&highlight=2-3)]

   Poprzedni kod pozwala serwerowi zlokalizować i obsłużyć plik *index. html* , niezależnie od tego, czy użytkownik wprowadza pełny adres URL, czy główny adres URL aplikacji sieci Web.

1. Na końcu metody `Startup.Configure` Mapuj trasę */Hub* na centrum `ChatHub`. Zastąp kod, który wyświetla *Hello World!* z następującym wierszem: 

   [!code-csharp[Startup](signalr-typescript-webpack/sample/3.x/Startup.cs?name=snippet_UseSignalR&highlight=3)]

1. W metodzie `Startup.ConfigureServices` Wywołaj metodę [addsignal](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr#Microsoft_Extensions_DependencyInjection_SignalRDependencyInjectionExtensions_AddSignalR_Microsoft_Extensions_DependencyInjection_IServiceCollection_). Dodaje do projektu usługi SignalR.

   [!code-csharp[Startup](signalr-typescript-webpack/sample/3.x/Startup.cs?name=snippet_AddSignalR)]

1. Utwórz nowy katalog o nazwie *Hubs*w katalogu głównym projektu. Celem jest przechowywanie centrum SignalR, które jest tworzone w następnym kroku.

1. Utwórz centra centrów */ChatHub. cs* przy użyciu następującego kodu:

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/3.x/snippets/ChatHub.cs?name=snippet_ChatHubStubClass)]

1. Dodaj następujący kod w górnej części pliku *Startup.cs* , aby rozwiązać `ChatHub` odwołanie:

    [!code-csharp[Startup](signalr-typescript-webpack/sample/3.x/Startup.cs?name=snippet_HubsNamespace)]

## <a name="enable-client-and-server-communication"></a>Włączanie komunikacji klienta i serwera

W aplikacji jest obecnie wyświetlany prosty formularz służący do wysyłania komunikatów. Nic się nie dzieje, gdy użytkownik spróbuje to zrobić. Serwer nasłuchuje określonej trasy, ale nic nie robi z wysłanymi komunikatami.

1. Wykonaj następujące polecenie w katalogu głównym projektu:

    ```console
    npm install @microsoft/signalr
    ```

    Poprzednie polecenie instaluje [SignalR klienta języka TypeScript](https://www.npmjs.com/package/@microsoft/signalr), który umożliwia klientowi wysyłanie komunikatów do serwera programu.

1. Dodaj wyróżniony kod do pliku *src/index. TS* :

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/3.x/snippets/index2.ts?name=snippet_IndexTsPhase2File&highlight=2,9-23)]

    Poprzedni kod obsługuje otrzymywanie komunikatów z serwera. Klasa `HubConnectionBuilder` tworzy nowy Konstruktor służący do konfigurowania połączenia z serwerem. Funkcja `withUrl` konfiguruje adres URL centrum.

    SignalR umożliwia wymianę komunikatów między klientem a serwerem. Każdy komunikat ma określoną nazwę. Można na przykład mieć komunikaty o nazwie `messageReceived`, które wykonują logikę odpowiedzialną za wyświetlanie nowej wiadomości w strefie messages. Nasłuchiwanie określonego komunikatu można wykonać za pomocą funkcji `on`. Można nasłuchiwać dowolnej liczby nazw komunikatów. Możliwe jest również przekazywanie parametrów do wiadomości, takich jak nazwa autora i zawartość otrzymanej wiadomości. Po odebraniu komunikatu przez klienta zostanie utworzony nowy element `div` z nazwą autora i treścią komunikatu w jego atrybucie `innerHTML`. Jest on dodawany do głównego elementu `div` wyświetlającego komunikaty.

1. Teraz, gdy klient może odebrać komunikat, skonfiguruj go do wysyłania wiadomości. Dodaj wyróżniony kod do pliku *src/index. TS* :

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/3.x/src/index.ts?highlight=34-35)]

    Wysyłanie komunikatu przez połączenie z usługą WebSockets wymaga wywołania metody `send`. Pierwszym parametrem metody jest nazwa komunikatu. Dane komunikatu są nieodpowiednie dla innych parametrów. W tym przykładzie komunikat identyfikowany jako `newMessage` jest wysyłany do serwera. Komunikat składa się z nazwy użytkownika i danych wejściowych użytkownika z pola tekstowego. Jeśli wysyłanie działa, wartość pola tekstowego jest wyczyszczona.

1. Dodaj podświetloną metodę do klasy `ChatHub`:

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/3.x/Hubs/ChatHub.cs?highlight=8-11)]

    Poprzedni kod emituje odebrane komunikaty wszystkim połączonym użytkownikom po ich odebraniu przez serwer. Nie jest konieczne posiadanie generycznej metody `on`, aby odbierać wszystkie komunikaty. Metoda o nazwie po wystarczającej nazwie.

    W tym przykładzie klient TypeScript wysyła komunikat zidentyfikowany jako `newMessage`. Metoda C# `NewMessage` oczekuje danych wysyłanych przez klienta. Wykonano wywołanie metody [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) na [klientach. All](/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.all). Odebrane komunikaty są wysyłane do wszystkich klientów podłączonych do centrum.

## <a name="test-the-app"></a>Testowanie aplikacji

Upewnij się, że aplikacja działa z następującymi krokami.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. Uruchom pakiet WebPack w trybie *wydania* . Za pomocą okna **konsoli Menedżera pakietów** wykonaj następujące polecenie w katalogu głównym projektu. Jeśli nie jesteś w katalogu głównym projektu, wprowadź `cd SignalRWebPack` przed wprowadzeniem polecenia.

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. Wybierz pozycję **debuguj** > **Rozpocznij bez debugowania** , aby uruchomić aplikację w przeglądarce bez dołączania debugera. Plik *wwwroot/index.html* jest obsługiwany w `http://localhost:<port_number>`.

   Jeśli zostaną wyświetlone błędy kompilacji, spróbuj zamknąć i ponownie otworzyć rozwiązanie. 

1. Otwórz inne wystąpienie przeglądarki (dowolna przeglądarka). Wklej adres URL na pasku adresu.

1. Wybierz opcję przeglądarka, wpisz coś w polu tekstowym **komunikat** , a następnie kliknij przycisk **Wyślij** . Unikatowa nazwa użytkownika i komunikat są wyświetlane na obu stronach natychmiastowo.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

1. Uruchom pakiet WebPack w trybie *wydania* , wykonując następujące polecenie w katalogu głównym projektu:

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. Skompiluj i uruchom aplikację, wykonując następujące polecenie w katalogu głównym projektu:

    ```dotnetcli
    dotnet run
    ```

    Serwer sieci Web uruchamia aplikację i udostępnia ją na hoście lokalnym.

1. Otwórz przeglądarkę, aby `http://localhost:<port_number>`. Plik *wwwroot/index.html* jest obsługiwany. Skopiuj adres URL z paska adresu.

1. Otwórz inne wystąpienie przeglądarki (dowolna przeglądarka). Wklej adres URL na pasku adresu.

1. Wybierz opcję przeglądarka, wpisz coś w polu tekstowym **komunikat** , a następnie kliknij przycisk **Wyślij** . Unikatowa nazwa użytkownika i komunikat są wyświetlane na obu stronach natychmiastowo.

---

![komunikat wyświetlany w obu oknach przeglądarki](signalr-typescript-webpack/_static/browsers-message-broadcast.png)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* [Program Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) z **ASP.NET i programowaniem aplikacji sieci Web**
* [Zestaw .NET Core SDK 2,2 lub nowszy](https://www.microsoft.com/net/download/all)
* [Node. js](https://nodejs.org/) z [npm](https://www.npmjs.com/)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* [Visual Studio Code](https://code.visualstudio.com/download)
* [Zestaw .NET Core SDK 2,2 lub nowszy](https://www.microsoft.com/net/download/all)
* [C#dla Visual Studio Code w wersji 1.17.1 lub nowszej](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [Node. js](https://nodejs.org/) z [npm](https://www.npmjs.com/)

---

## <a name="create-the-aspnet-core-web-app"></a>Tworzenie aplikacji sieci Web ASP.NET Core

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Skonfiguruj program Visual Studio, aby szukać npm w zmiennej środowiskowej *Path* . Domyślnie program Visual Studio używa wersji npm znajdującej się w katalogu instalacyjnym. Wykonaj te instrukcje w programie Visual Studio:

1. Przejdź do **opcji narzędzia** > **Opcje** > **projekty i rozwiązania** > **sieci Web zarządzanie pakietami** > **zewnętrznych narzędzi sieci Web**.
1. Wybierz z listy wpis *$ (Path)* . Kliknij strzałkę w górę, aby przenieść wpis do drugiej pozycji na liście.

    ![Konfiguracja programu Visual Studio](signalr-typescript-webpack/_static/signalr-configure-path-visual-studio.png)

Konfiguracja programu Visual Studio została ukończona. Czas na utworzenie projektu.

1. Użyj opcji **plik** > **Nowy** > **projekt** , a następnie wybierz szablon **aplikacja sieci Web ASP.NET Core** .
1. Nazwij projekt *SignalRWebPack*, a następnie wybierz pozycję **Utwórz**.
1. Wybierz pozycję *.NET Core* z listy rozwijanej platforma docelowa, a następnie wybierz pozycję *ASP.NET Core 2,2* z listy rozwijanej selektora struktury. Wybierz **pusty** szablon i wybierz pozycję **Utwórz**.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Uruchom następujące polecenie w **zintegrowanym terminalu**:

```dotnetcli
dotnet new web -o SignalRWebPack
```

Pusta aplikacja sieci Web ASP.NET Core, ukierunkowana na .NET Core, jest tworzona w katalogu *SignalRWebPack* .

---

## <a name="configure-webpack-and-typescript"></a>Konfigurowanie pakietu WebPack i języka TypeScript

Poniższe kroki umożliwiają skonfigurowanie konwersji języka TypeScript na JavaScript i zgrupowanie zasobów po stronie klienta.

1. Wykonaj następujące polecenie w katalogu głównym projektu, aby utworzyć plik *Package. JSON* :

    ```console
    npm init -y
    ```

1. Dodaj wyróżnioną właściwość do pliku *Package. JSON* :

    [!code-json[package.json](signalr-typescript-webpack/sample/2.x/snippets/package1.json?highlight=4)]

    Ustawienie właściwości `private` na `true` uniemożliwia ostrzeżenia instalacji pakietu w następnym kroku.

1. Zainstaluj wymagane pakiety npm. Wykonaj następujące polecenie w katalogu głównym projektu:

    ```console
    npm install -D -E clean-webpack-plugin@1.0.1 css-loader@2.1.0 html-webpack-plugin@4.0.0-beta.5 mini-css-extract-plugin@0.5.0 ts-loader@5.3.3 typescript@3.3.3 webpack@4.29.3 webpack-cli@3.2.3
    ```

    Niektóre szczegóły polecenia do uwagi:

    * Numer wersji jest zgodny ze znakiem `@` dla każdej nazwy pakietu. npm instaluje te określone wersje pakietu.
    * Opcja `-E` wyłącza domyślne zachowanie npm podczas pisania operatorów zakresu [wersji semantycznej](https://semver.org/) w pliku *Package. JSON*. Na przykład `"webpack": "4.29.3"` jest używany zamiast `"webpack": "^4.29.3"`. Ta opcja Zapobiega niezamierzonym uaktualnianiu do nowszych wersji pakietu.

    Aby uzyskać więcej informacji, zobacz oficjalne dokumenty z [npm-Install](https://docs.npmjs.com/cli/install) .

1. Zastąp Właściwość `scripts` pliku *Package. JSON* następującym fragmentem kodu:

    ```json
    "scripts": {
      "build": "webpack --mode=development --watch",
      "release": "webpack --mode=production",
      "publish": "npm run release && dotnet publish -c Release"
    },
    ```

    Niektóre objaśnienia dotyczące skryptów:

    * `build`: pakietuje zasoby po stronie klienta w trybie tworzenia i czujki pod kątem zmian w pliku. Obserwator plików powoduje, że pakiet jest generowany ponownie za każdym razem, gdy plik projektu jest zmieniany. Opcja `mode` wyłącza optymalizacje produkcyjne, takie jak wstrząsanie i minifikacjaowanie drzewa. `build` w trakcie opracowywania.
    * `release`: pakietuje zasoby po stronie klienta w trybie produkcyjnym.
    * `publish`: uruchamia skrypt `release`, aby powiązać zasoby po stronie klienta w trybie produkcyjnym. Wywołuje polecenie [publikowania](/dotnet/core/tools/dotnet-publish) interfejs wiersza polecenia platformy .NET Core, aby opublikować aplikację.

1. Utwórz plik o nazwie *WebPack. config. js*w katalogu głównym projektu o następującej zawartości:

    [!code-javascript[webpack.config.js](signalr-typescript-webpack/sample/2.x/webpack.config.js)]

    Poprzedni plik konfiguruje kompilację pakietu WebPack. Niektóre szczegóły konfiguracji do uwagi:

    * Właściwość `output` przesłania domyślną wartość *rozkłu*. Pakiet jest emitowany w katalogu *wwwroot* .
    * Tablica `resolve.extensions` zawiera *js* do zaimportowania klienta SignalR JavaScript.

1. Utwórz nowy katalog *src* w katalogu głównym projektu. Celem jest przechowywanie zasobów po stronie klienta.

1. Utwórz *src/index.html* z następującą zawartością.

    [!code-html[index.html](signalr-typescript-webpack/sample/2.x/src/index.html)]

    Powyższy kod HTML definiuje standardowe znaczniki strony głównej.

1. Utwórz nowy katalog *src/CSS* . Celem jest przechowywanie plików *CSS* projektu.

1. Utwórz *src/CSS/Main. css* z następującą zawartością:

    [!code-css[main.css](signalr-typescript-webpack/sample/2.x/src/css/main.css)]

    Poprzedni plik *Main. css* jest stylem aplikacji.

1. Utwórz plik *src/tsconfig. JSON* z następującą zawartością:

    [!code-json[tsconfig.json](signalr-typescript-webpack/sample/2.x/src/tsconfig.json)]

    Poprzedni kod konfiguruje kompilator języka TypeScript w celu utworzenia kodu JavaScript zgodnego z [ECMAScript](https://wikipedia.org/wiki/ECMAScript) 5.

1. Utwórz *src/index. TS* z następującą zawartością:

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/2.x/snippets/index1.ts?name=snippet_IndexTsPhase1File)]

    Poprzedni kod TypeScript pobiera odwołania do elementów DOM i dołącza dwa programy obsługi zdarzeń:

    * `keyup`: to zdarzenie jest wyzwalane, gdy użytkownik wpisze coś w polu tekstowym określonym jako `tbMessage`. Funkcja `send` jest wywoływana, gdy użytkownik naciśnie klawisz **Enter** .
    * `click`: to zdarzenie jest wyzwalane, gdy użytkownik kliknie przycisk **Wyślij** . Funkcja `send` jest wywoływana.

## <a name="configure-the-aspnet-core-app"></a>Konfigurowanie aplikacji ASP.NET Core

1. Kod podany w metodzie `Startup.Configure` wyświetla *Hello World!* . Zastąp wywołanie metody `app.Run` z wywołaniami do [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) i [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_).

    [!code-csharp[Startup](signalr-typescript-webpack/sample/2.x/Startup.cs?name=snippet_UseStaticDefaultFiles)]

    Poprzedni kod pozwala serwerowi zlokalizować i obsłużyć plik *index. html* , niezależnie od tego, czy użytkownik wprowadza pełny adres URL, czy główny adres URL aplikacji sieci Web.

1. Wywołaj metodę [Addsignaler](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr#Microsoft_Extensions_DependencyInjection_SignalRDependencyInjectionExtensions_AddSignalR_Microsoft_Extensions_DependencyInjection_IServiceCollection_) w metodzie `Startup.ConfigureServices`. Dodaje do projektu usługi SignalR.

    [!code-csharp[Startup](signalr-typescript-webpack/sample/2.x/Startup.cs?name=snippet_AddSignalR)]

1. Zamapuj trasę */Hub* do centrum `ChatHub`. Dodaj następujące wiersze na końcu metody `Startup.Configure`:

    [!code-csharp[Startup](signalr-typescript-webpack/sample/2.x/Startup.cs?name=snippet_UseSignalR)]

1. Utwórz nowy katalog o nazwie *Hubs*w katalogu głównym projektu. Celem jest przechowywanie centrum SignalR, które jest tworzone w następnym kroku.

1. Utwórz centra centrów */ChatHub. cs* przy użyciu następującego kodu:

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/2.x/snippets/ChatHub.cs?name=snippet_ChatHubStubClass)]

1. Dodaj następujący kod w górnej części pliku *Startup.cs* , aby rozwiązać `ChatHub` odwołanie:

    [!code-csharp[Startup](signalr-typescript-webpack/sample/2.x/Startup.cs?name=snippet_HubsNamespace)]

## <a name="enable-client-and-server-communication"></a>Włączanie komunikacji klienta i serwera

W aplikacji jest obecnie wyświetlany prosty formularz służący do wysyłania komunikatów. Nic się nie dzieje, gdy użytkownik spróbuje to zrobić. Serwer nasłuchuje określonej trasy, ale nic nie robi z wysłanymi komunikatami.

1. Wykonaj następujące polecenie w katalogu głównym projektu:

    ```console
    npm install @microsoft/signalr
    ```

    Poprzednie polecenie instaluje [SignalR klienta języka TypeScript](https://www.npmjs.com/package/@microsoft/signalr), który umożliwia klientowi wysyłanie komunikatów do serwera programu.

1. Dodaj wyróżniony kod do pliku *src/index. TS* :

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/2.x/snippets/index2.ts?name=snippet_IndexTsPhase2File&highlight=2,9-23)]

    Poprzedni kod obsługuje otrzymywanie komunikatów z serwera. Klasa `HubConnectionBuilder` tworzy nowy Konstruktor służący do konfigurowania połączenia z serwerem. Funkcja `withUrl` konfiguruje adres URL centrum.

    SignalR umożliwia wymianę komunikatów między klientem a serwerem. Każdy komunikat ma określoną nazwę. Można na przykład mieć komunikaty o nazwie `messageReceived`, które wykonują logikę odpowiedzialną za wyświetlanie nowej wiadomości w strefie messages. Nasłuchiwanie określonego komunikatu można wykonać za pomocą funkcji `on`. Można nasłuchiwać dowolnej liczby nazw komunikatów. Możliwe jest również przekazywanie parametrów do wiadomości, takich jak nazwa autora i zawartość otrzymanej wiadomości. Po odebraniu komunikatu przez klienta zostanie utworzony nowy element `div` z nazwą autora i treścią komunikatu w jego atrybucie `innerHTML`. Jest on dodawany do głównego elementu `div` wyświetlającego komunikaty.

1. Teraz, gdy klient może odebrać komunikat, skonfiguruj go do wysyłania wiadomości. Dodaj wyróżniony kod do pliku *src/index. TS* :

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/2.x/src/index.ts?highlight=34-35)]

    Wysyłanie komunikatu przez połączenie z usługą WebSockets wymaga wywołania metody `send`. Pierwszym parametrem metody jest nazwa komunikatu. Dane komunikatu są nieodpowiednie dla innych parametrów. W tym przykładzie komunikat identyfikowany jako `newMessage` jest wysyłany do serwera. Komunikat składa się z nazwy użytkownika i danych wejściowych użytkownika z pola tekstowego. Jeśli wysyłanie działa, wartość pola tekstowego jest wyczyszczona.

1. Dodaj podświetloną metodę do klasy `ChatHub`:

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/2.x/Hubs/ChatHub.cs?highlight=8-11)]

    Poprzedni kod emituje odebrane komunikaty wszystkim połączonym użytkownikom po ich odebraniu przez serwer. Nie jest konieczne posiadanie generycznej metody `on`, aby odbierać wszystkie komunikaty. Metoda o nazwie po wystarczającej nazwie.

    W tym przykładzie klient TypeScript wysyła komunikat zidentyfikowany jako `newMessage`. Metoda C# `NewMessage` oczekuje danych wysyłanych przez klienta. Wykonano wywołanie metody [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) na [klientach. All](/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.all). Odebrane komunikaty są wysyłane do wszystkich klientów podłączonych do centrum.

## <a name="test-the-app"></a>Testowanie aplikacji

Upewnij się, że aplikacja działa z następującymi krokami.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. Uruchom pakiet WebPack w trybie *wydania* . Za pomocą okna **konsoli Menedżera pakietów** wykonaj następujące polecenie w katalogu głównym projektu. Jeśli nie jesteś w katalogu głównym projektu, wprowadź `cd SignalRWebPack` przed wprowadzeniem polecenia.

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. Wybierz pozycję **debuguj** > **Rozpocznij bez debugowania** , aby uruchomić aplikację w przeglądarce bez dołączania debugera. Plik *wwwroot/index.html* jest obsługiwany w `http://localhost:<port_number>`.

1. Otwórz inne wystąpienie przeglądarki (dowolna przeglądarka). Wklej adres URL na pasku adresu.

1. Wybierz opcję przeglądarka, wpisz coś w polu tekstowym **komunikat** , a następnie kliknij przycisk **Wyślij** . Unikatowa nazwa użytkownika i komunikat są wyświetlane na obu stronach natychmiastowo.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

1. Uruchom pakiet WebPack w trybie *wydania* , wykonując następujące polecenie w katalogu głównym projektu:

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. Skompiluj i uruchom aplikację, wykonując następujące polecenie w katalogu głównym projektu:

    ```dotnetcli
    dotnet run
    ```

    Serwer sieci Web uruchamia aplikację i udostępnia ją na hoście lokalnym.

1. Otwórz przeglądarkę, aby `http://localhost:<port_number>`. Plik *wwwroot/index.html* jest obsługiwany. Skopiuj adres URL z paska adresu.

1. Otwórz inne wystąpienie przeglądarki (dowolna przeglądarka). Wklej adres URL na pasku adresu.

1. Wybierz opcję przeglądarka, wpisz coś w polu tekstowym **komunikat** , a następnie kliknij przycisk **Wyślij** . Unikatowa nazwa użytkownika i komunikat są wyświetlane na obu stronach natychmiastowo.

---

![komunikat wyświetlany w obu oknach przeglądarki](signalr-typescript-webpack/_static/browsers-message-broadcast.png)

::: moniker-end

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:signalr/javascript-client>
* <xref:signalr/hubs>
