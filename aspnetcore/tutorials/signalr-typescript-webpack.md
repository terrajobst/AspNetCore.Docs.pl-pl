---
title: Używanie biblioteki SignalR platformy ASP.NET Core za pomocą TypeScript i Webpack
author: ssougnez
description: W tym samouczku skonfigurujesz Webpack połączyć w paczkę i Utwórz aplikację sieci web biblioteki SignalR platformy ASP.NET Core, którego klient został napisany w TypeScript.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 06/29/2018
uid: tutorials/signalr-typescript-webpack
ms.openlocfilehash: 6d52e915ab20aa69179585eceb497e8abd72c997
ms.sourcegitcommit: 661d30492d5ef7bbca4f7e709f40d8f3309d2dac
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37938187"
---
# <a name="use-aspnet-core-signalr-with-typescript-and-webpack"></a>Używanie biblioteki SignalR platformy ASP.NET Core za pomocą TypeScript i Webpack

Przez [Sébastien Sougnez](https://twitter.com/ssougnez) i [Scott Addie](https://twitter.com/Scott_Addie)

[Webpack](https://webpack.js.org/) umożliwia deweloperom połączyć w paczkę i tworzyć zasoby po stronie klienta aplikacji sieci web. Ten samouczek pokazuje, za pomocą Webpack w aplikacji sieci web biblioteki SignalR platformy ASP.NET Core, którego klient został napisany w [TypeScript](https://www.typescriptlang.org/).

W tym samouczku dowiesz się, jak:

> [!div class="checklist"]
> * Tworzenia szkieletu początkową aplikację biblioteki SignalR platformy ASP.NET Core
> * Konfigurowanie klienta SignalR TypeScript
> * Konfigurowanie potoku kompilacji przy użyciu Webpack
> * Konfigurowanie serwera SignalR
> * Włącz komunikację między klientem i serwerem

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/signalr-typescript-webpack/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

## <a name="prerequisites"></a>Wymagania wstępne

Zainstaluj następujące oprogramowanie:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* [.NET core SDK 2.1 lub nowszej](https://www.microsoft.com/net/download/all)
* [Node.js](https://nodejs.org/) z [npm](https://www.npmjs.com/)
* [Program Visual Studio 2017](https://www.visualstudio.com/downloads/) wersji 15.7.3 lub nowszy z **ASP.NET i tworzenie aplikacji internetowych** obciążenia

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

* [.NET core SDK 2.1 lub nowszej](https://www.microsoft.com/net/download/all)
* [Node.js](https://nodejs.org/) z [npm](https://www.npmjs.com/)

---

## <a name="create-the-aspnet-core-web-app"></a>Tworzenie aplikacji sieci web platformy ASP.NET Core

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Konfigurowanie programu Visual Studio, aby wyszukać pakiety npm w usłudze *ścieżki* zmiennej środowiskowej. Domyślnie program Visual Studio korzysta z wersji npm znajduje się w jego katalogu instalacji. Wykonaj te instrukcje w programie Visual Studio:

1. Przejdź do **narzędzia** > **opcje** > **projekty i rozwiązania** > **zarządzania pakietami sieci Web**  >  **Zewnętrzne narzędzia sieci Web**.
1. Wybierz *$(PATH)* wpis na liście. Kliknij strzałkę w górę, aby przenieść wpis do drugiej pozycji na liście. Jako aside pierwszy wpis odnosi się do projektu pakiety lokalnego.

    ![Konfiguracja programu Visual Studio](signalr-typescript-webpack/_static/signalr-configure-path-visual-studio.png)

Visual Studio zostało zakończone. Nadszedł czas, aby utworzyć projekt.

1. Użyj **pliku** > **New** > **projektu** menu opcji, a następnie wybierz **aplikacji sieci Web programu ASP.NET Core** szablon.
1. Nadaj projektowi nazwę *SignalRWebPack*i kliknij przycisk **OK** przycisku.
1. Wybierz *platformy .NET Core* z listy rozwijanej i wybierz platformę docelową *platformy ASP.NET Core 2.1* z listy rozwijanej selektorów framework. Wybierz **pusty** szablonu, a następnie kliknij przycisk **OK** przycisku.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

Uruchom następujące polecenie **zintegrowany Terminal**:

```console
dotnet new web -o SignalRWebPack
```

Pusta aplikacja internetowa platformy ASP.NET Core, przeznaczonych dla platformy .NET Core jest tworzony w *SignalRWebPack* katalogu.

---

## <a name="configure-webpack-and-typescript"></a>Konfigurowanie Webpack i TypeScript

Poniższe kroki konfigurowania konwersji TypeScript w kodzie JavaScript i paczki zasoby po stronie klienta.

1. Wykonaj następujące polecenie w katalogu głównym projektu, aby utworzyć *package.json* pliku:

    ```console
    npm init -y
    ```

1. Dodaj wyróżnione właściwość do *package.json* pliku:

    [!code-json[package.json](signalr-typescript-webpack/sample/snippets/package1.json?highlight=4)]

    Ustawienie `private` właściwość `true` zapobiega ostrzeżeń instalacji pakietu w następnym kroku.

1. Zainstaluj pakiety npm wymagane. Wykonaj następujące polecenie w katalogu głównym projektu:

    ```console
    npm install -D -E clean-webpack-plugin@0.1.19 css-loader@0.28.11 html-webpack-plugin@3.2.0 mini-css-extract-plugin@0.4.0 ts-loader@4.4.1 typescript@2.9.2 webpack@4.12.0 webpack-cli@3.0.6
    ```

    Niektóre szczegóły polecenia należy pamiętać:

    * Numer wersji jest zgodna `@` podpisywania dla każdej nazwy pakietu. npm instaluje te wersje określonego pakietu.
    * `-E` Opcja wyłącza npm domyślne zachowanie dotyczące pisania [wersji semantycznej](https://semver.org/) zakresu operatorom *package.json*. Na przykład `"webpack": "4.12.0"` jest używana zamiast `"webpack": "^4.12.0"`. Ta opcja uniemożliwia niezamierzone uaktualnienia do nowszej wersji pakietu.

    Można znaleźć w oficjalnej [npm install](https://docs.npmjs.com/cli/install) docs, aby uzyskać więcej szczegółów.

1. Zastąp `scripts` właściwość *package.json* pliku następującym fragmentem kodu:

    ```json
    "scripts": {
      "build": "webpack --mode=development --watch",
      "release": "webpack --mode=production",
      "publish": "npm run release && dotnet publish -c Release"
    },
    ```

    Niektórych wyjaśnienie skrypty:

    * `build`: Zawiera zasoby po stronie klienta w trybie projektowania, a następnie oczekuje na zmiany w plikach. Obserwator plików powoduje, że pakiet ponownie wygenerować każdorazowo zmiany w pliku projektu. `mode` Opcja wyłącza optymalizacje, produkcji, takie jak potrząsając drzewa i minimalizowanie. Używaj tylko `build` w trakcie opracowywania.
    * `release`: Zawiera zasoby po stronie klienta w trybie produkcyjnym.
    * `publish`: Uruchamia `release` skrypt, aby powiązać zasoby po stronie klienta w trybie produkcyjnym. Wywołuje .NET Core interfejsu wiersza polecenia [publikowania](/dotnet/core/tools/dotnet-publish) polecenie, aby opublikować aplikację.

1. Utwórz plik o nazwie *webpack.config.js*, w katalogu głównym projektu o następującej zawartości:

    [!code-javascript[webpack.config.js](signalr-typescript-webpack/sample/webpack.config.js)]

    Poprzedni plik konfiguruje kompilacji Webpack. Niektóre szczegóły konfiguracji należy pamiętać:

    * `output` Właściwość zastępuje wartość domyślną *dist*. Zamiast tego znacznikowej pakietu w *wwwroot* katalogu.
    * `resolve.extensions` Tablica zawiera *js* do zaimportowania klienta SignalR JavaScript.

1. Utwórz nową *src* katalogu w folderze głównym projektu. Jego celem jest do przechowywania zasobów po stronie klienta projektu.

1. Tworzenie *src/index.html* o następującej zawartości.

    [!code-html[index.html](signalr-typescript-webpack/sample/src/index.html)]

    Poprzedni kod HTML definiuje znaczników standardowy na stronie głównej.

1. Utwórz nową *src/css* katalogu. Jej celem jest zapisanie projektu *.css* plików.

1. Tworzenie *src/css/main.css* o następującej zawartości:

    [!code-css[main.css](signalr-typescript-webpack/sample/src/css/main.css)]

    Poprzedni *main.css* pliku style aplikacji.

1. Tworzenie *src/tsconfig.json* o następującej zawartości:

    [!code-json[tsconfig.json](signalr-typescript-webpack/sample/src/tsconfig.json)]

    Powyższy kod konfiguruje kompilatora TypeScript, aby wygenerować [ECMAScript](https://wikipedia.org/wiki/ECMAScript) zgodnego z 5 JavaScript.

1. Tworzenie *src/index.ts* o następującej zawartości:

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/snippets/index1.ts?name=snippet_IndexTsPhase1File)]

    Poprzedni TypeScript pobiera odwołania do elementów modelu DOM i dołącza dwóch programów obsługi zdarzeń:

    * `keyup`: To zdarzenie jest uruchamiana, gdy użytkownik wpisze coś w pole tekstowe zidentyfikowane jako `tbMessage`. `send` Funkcja jest wywoływana, gdy użytkownik naciśnie **Enter** klucza.
    * `click`: To zdarzenie jest uruchamiana, gdy użytkownik kliknie **wysyłania** przycisku. `send` Funkcja jest wywoływana.

## <a name="configure-the-aspnet-core-app"></a>Konfigurowanie aplikacji platformy ASP.NET Core

1. Kod zawarty w `Startup.Configure` metoda Wyświetla *Hello World!*. Zastąp `app.Run` wywołanie metody z wywołaniami [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) i [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_).

    [!code-csharp[Startup](signalr-typescript-webpack/sample/Startup.cs?name=snippet_UseStaticDefaultFiles)]

    Powyższy kod zezwala serwerowi do lokalizowania i obsługiwać *index.html* pliku, czy użytkownik musi wprowadzić jej pełny adres URL lub adres URL katalogu głównego aplikacji sieci web.

1. Wywołaj [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr#Microsoft_Extensions_DependencyInjection_SignalRDependencyInjectionExtensions_AddSignalR_Microsoft_Extensions_DependencyInjection_IServiceCollection_) w `Startup.ConfigureServices` metody. Usługi SignalR dodaje do projektu.

    [!code-csharp[Startup](signalr-typescript-webpack/sample/Startup.cs?name=snippet_AddSignalR)]

1. Mapa */hub* kierować do `ChatHub` koncentratora. Dodaj następujące wiersze na końcu `Startup.Configure` metody:

    [!code-csharp[Startup](signalr-typescript-webpack/sample/Startup.cs?name=snippet_UseSignalR)]

1. Utwórz nowy katalog o nazwie *koncentratory*, w katalogu głównym projektu. Jego celem jest do przechowywania Centrum SignalR, który jest tworzony w następnym kroku.

1. Utwórz koncentrator *Hubs/ChatHub.cs* następującym kodem:

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/snippets/ChatHub.cs?name=snippet_ChatHubStubClass)]

1. Dodaj następujący kod w górnej części *Startup.cs* plik, aby rozwiązać `ChatHub` odwołania:

    [!code-csharp[Startup](signalr-typescript-webpack/sample/Startup.cs?name=snippet_HubsNamespace)]

## <a name="enable-client-and-server-communication"></a>Włącz komunikację z klientem i serwerem

Aplikacja Wyświetla obecnie prosty formularz do wysyłania wiadomości. Podczas próby zrobić nic się nie dzieje. Serwer nasłuchuje do określonej trasy, ale nie robi nic za pomocą wysłanych komunikatów.

1. Wykonaj następujące polecenie w katalogu głównym projektu:

    ```console
    npm install @aspnet/signalr
    ```

    Poprzednie polecenie instaluje [klienta SignalR TypeScript](https://www.npmjs.com/package/@aspnet/signalr), co pozwala klientowi wysłać wiadomości do serwera.

1. Dodaj wyróżniony kod do *src/index.ts* pliku:

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/snippets/index2.ts?name=snippet_IndexTsPhase2File&highlight=2,9-23)]

    Powyższy kod obsługuje odbieranie komunikatów z serwera. `HubConnectionBuilder` Klasy tworzy nowy konstruktor do skonfigurowania połączenia z serwerem. `withUrl` Funkcja konfiguruje adres URL koncentratora.

    SignalR umożliwia wymianę wiadomości między klientem a serwerem. Każdy komunikat ma określoną nazwę. Na przykład masz wiadomości o nazwie `messageReceived` , wykonana logiki odpowiada za wyświetlanie nowy komunikat w strefie wiadomości. Nasłuchiwanie szczegółowy komunikat o błędzie może odbywać się za pośrednictwem `on` funkcji. Można słuchać dowolną liczbę nazw wiadomości. Istnieje również możliwość przekazania parametrów do wiadomości, takie jak imię i nazwisko autora i zawartość odebrany komunikat. Gdy klient odbierze komunikat nową `div` zostanie utworzony element o nazwie autora i komunikat zawartość w jego `innerHTML` atrybutu. Jest ona dodawana do głównej `div` element wyświetlania komunikatów.

1. Teraz, gdy klient może otrzymać komunikat, należy skonfigurować go do wysyłania wiadomości. Dodaj wyróżniony kod do *src/index.ts* pliku:

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/src/index.ts?highlight=34-35)]

    Wysyłanie wiadomości przez połączenie Websocket wymaga wywołania `send` metody. Pierwszy parametr metody jest nazwa komunikatu. Dane wiadomości zamieszkuje innych parametrów. W tym przykładzie wiadomość zidentyfikowane jako `newMessage` są wysyłane do serwera. Komunikat składa się z nazwy użytkownika i dane wejściowe z pola tekstowego użytkownika. Jeśli działa wysyłania, wartość pola tekstowego jest wyczyszczone.

1. Dodaj metodę wyróżnione `ChatHub` klasy:

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/Hubs/ChatHub.cs?highlight=8-11)]

    Powyższy kod emituje Odebrane komunikaty do wszystkich połączonych użytkowników, gdy serwer otrzymuje je. Nie jest konieczne zapewnienie ogólnej `on` metodę, aby otrzymywać wszystkie komunikaty. Metodę o nazwie po sufiksy nazwa komunikatu.

    W tym przykładzie klient TypeScript wysyła komunikat, który został zidentyfikowany jako `newMessage`. C# `NewMessage` metoda oczekuje, że dane wysłane przez klienta. Połączenie jest nawiązywane w przypadku [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) metody [Clients.All](/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.all). Odebrane komunikaty są wysyłane do wszystkich klientów podłączone do koncentratora.

## <a name="test-the-app"></a>Testowanie aplikacji

Upewnij się, że aplikacja działa wykonując następujące kroki.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. Uruchamianie Webpack w *wersji* trybu. Za pomocą **Konsola Menedżera pakietów** okna, wykonaj następujące polecenie w katalogu głównym projektu:

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. Wybierz **debugowania** > **Uruchom bez debugowania** do uruchomienia aplikacji w przeglądarce, bez dołączania debugera. *Wwwroot/index.html* plików jest obsługiwany w `http://localhost:<port_number>`.

1. Otwórz inne wystąpienie przeglądarki (w dowolnej przeglądarce). Wklej adres URL w pasku adresu.

1. Wybierz albo przeglądarki, wpisz coś w **komunikat** pola tekstowego, a następnie kliknij przycisk **wysyłania** przycisku. Unikatowa nazwa użytkownika i wiadomości są wyświetlane na obu stronach natychmiast.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

1. Uruchamianie Webpack w *wersji* trybu, wykonując następujące polecenie w katalogu głównym projektu:

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. Tworzenie i uruchamianie aplikacji, wykonując następujące polecenie w katalogu głównym projektu:

    ```console
    dotnet run
    ```

    Serwer sieci web uruchamia aplikację i udostępnia je na hoście lokalnym.

1. Otwórz w przeglądarce `http://localhost:<port_number>`. *Wwwroot/index.html* plików jest obsługiwany. Skopiuj adres URL z paska adresu.

1. Otwórz inne wystąpienie przeglądarki (w dowolnej przeglądarce). Wklej adres URL w pasku adresu.

1. Wybierz albo przeglądarki, wpisz coś w **komunikat** pola tekstowego, a następnie kliknij przycisk **wysyłania** przycisku. Unikatowa nazwa użytkownika i wiadomości są wyświetlane na obu stronach natychmiast.

---

![komunikat wyświetlany w oba okna przeglądarki](signalr-typescript-webpack/_static/browsers-message-broadcast.png)

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:signalr/javascript-client>
* <xref:signalr/hubs>
