---
title: SignalR platformy ASP.NET Core za pomocą języka TypeScript i Webpack
author: ssougnez
description: W tym samouczku skonfigurujesz Webpack połączyć w paczkę i tworzenie aplikacji sieci web platformy ASP.NET Core SignalR, którego klient są zapisywane na maszynie.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 06/29/2018
uid: tutorials/signalr-typescript-webpack
ms.openlocfilehash: e4b00fa61ea0becba7d678a0b7c94d2d30f06740
ms.sourcegitcommit: 2941e24d7f3fd3d5e88d27e5f852aaedd564deda
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37126432"
---
# <a name="use-aspnet-core-signalr-with-typescript-and-webpack"></a>SignalR platformy ASP.NET Core za pomocą języka TypeScript i Webpack

Przez [Sébastien Sougnez](https://twitter.com/ssougnez) i [Scott Addie](https://twitter.com/Scott_Addie)

[Webpack](https://webpack.js.org/) umożliwia deweloperom pakietu i utworzyć zasobów aplikacji sieci web po stronie klienta. Ten samouczek pokazuje, przy użyciu Webpack w aplikacji sieci web platformy ASP.NET Core SignalR, którego klient są zapisywane w [TypeScript](https://www.typescriptlang.org/).

Z tego samouczka, dowiesz się, jak:

> [!div class="checklist"]
> * Tworzenie szkieletu początkową aplikację ASP.NET Core SignalR
> * Konfigurowanie klienta SignalR TypeScript
> * Konfigurowanie procesu kompilacji, przy użyciu Webpack
> * Konfigurowanie serwera SignalR
> * Włącz komunikację między klientem a serwerem

[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/signalr-typescript-webpack/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

## <a name="prerequisites"></a>Wymagania wstępne

Należy zainstalować następujące oprogramowanie:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* [Oprogramowanie .NET core SDK 2.1 lub nowszej](https://www.microsoft.com/net/download/all)
* [Node.js](https://nodejs.org/) z [npm](https://www.npmjs.com/)
* [Visual Studio 2017](https://www.visualstudio.com/downloads/) wersji 15.7 lub nowszym z **ASP.NET i sieć web development** obciążenia

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

* [Oprogramowanie .NET core SDK 2.1 lub nowszej](https://www.microsoft.com/net/download/all)
* [Node.js](https://nodejs.org/) z [npm](https://www.npmjs.com/)

---

## <a name="create-the-aspnet-core-web-app"></a>Tworzenie aplikacji sieci web platformy ASP.NET Core

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Konfigurowanie programu Visual Studio, aby wyszukać npm w *ścieżki* zmiennej środowiskowej. Domyślnie program Visual Studio korzysta z wersji programu npm znajdujące się w jego katalogu instalacji. Wykonaj te instrukcje w Visual Studio:

1. Przejdź do **narzędzia** > **opcje** > **projekty i rozwiązania** > **sieci Web pakietu zarządzania**  >  **Narzędzi zewnętrznych sieci Web**.
1. Wybierz *$(PATH)* wpisu z listy. Kliknij strzałkę w górę, aby przenieść wpis drugi pozycji na liście. Jako Zarezerwuj pierwszej pozycji odwołuje się do lokalnego pakietów projektu.

    ![Konfiguracja programu Visual Studio](signalr-typescript-webpack/_static/signalr-configure-path-visual-studio.png)

Visual Studio zostało zakończone. Nadszedł czas, aby utworzyć projekt.

1. Użyj **pliku** > **nowy** > **projektu** menu opcję i wybierz polecenie **aplikacji sieci Web platformy ASP.NET Core** szablon.
1. Nazwij projekt *SignalRWebPack*i kliknij przycisk **OK** przycisku.
1. Wybierz *.NET Core* z listy rozwijanej i wybierz platformę docelową *platformy ASP.NET Core 2.1* z listy rozwijanej selektora framework. Wybierz **pusty** szablonu, a następnie kliknij przycisk **OK** przycisku.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

Uruchom następujące polecenie **zintegrowane Terminal**:

```console
dotnet new web -o SignalRWebPack
```

Pusty aplikacji sieci web platformy ASP.NET Core, przeznaczonych dla platformy .NET Core jest tworzony w *SignalRWebPack* katalogu.

---

## <a name="configure-webpack-and-typescript"></a>Skonfiguruj Webpack i TypeScript

Poniższe kroki konfigurowania konwersji TypeScript do języka JavaScript i paczki zasobów po stronie klienta.

1. Uruchom następujące polecenie w katalogu głównym projektu, aby utworzyć *package.json* pliku:

    ```console
    npm init -y
    ```

1. Dodaj wyróżnione właściwość do *package.json* pliku:

    [!code-json[package.json](signalr-typescript-webpack/sample/snippets/package1.json?highlight=4)]

    Ustawienie `private` właściwości `true` uniemożliwia ostrzeżenia instalacji pakietu w następnym kroku.

1. Instalowanie pakietów npm wymagane. Uruchom następujące polecenie z katalogu głównego projektu:

    ```console
    npm install -D -E clean-webpack-plugin@0.1.19 css-loader@0.28.11 html-webpack-plugin@3.2.0 mini-css-extract-plugin@0.4.0 ts-loader@4.4.1 typescript@2.9.2 webpack@4.12.0 webpack-cli@3.0.6
    ```

    Niektóre szczegóły polecenia należy pamiętać:

    * Liczba wersji następuje `@` podpisywania dla każdej nazwy pakietu. npm instaluje te wersje określonego pakietu.
    * `-E` Opcja powoduje wyłączenie na npm domyślne zachowanie zapisu [wersjonowania semantycznego](https://semver.org/) operatory, które mają należeć do zakresu *package.json*. Na przykład `"webpack": "4.12.0"` jest używany zamiast `"webpack": "^4.12.0"`. Ta opcja uniemożliwia niezamierzone uaktualnienia do nowszych wersji pakietu.

    Można znaleźć w oficjalnej [npm install](https://docs.npmjs.com/cli/install) dokumentów, aby uzyskać więcej szczegółów.

1. Zastąp `scripts` właściwość *package.json* pliku następującym fragmentem kodu:

    ```json
    "scripts": {
      "build": "webpack --mode=development --watch",
      "release": "webpack --mode=production",
      "publish": "npm run release && dotnet publish -c Release"
    },
    ```

    Wyjaśnienie niektórych skryptów:

    * `build`: Zawiera zasoby po stronie klienta w trybie projektowania i oczekuje na zmiany pliku. Obserwator pliku powoduje, że pakiet można ponownie wygenerować zawsze zmiany w pliku projektu. `mode` Opcja wyłącza optymalizacje produkcji, takie jak potrząsając drzewa i minimalizowanie. Należy używać tylko `build` do rozwoju.
    * `release`: Zawiera zasoby po stronie klienta w trybie produkcyjnym.
    * `publish`: Uruchamia `release` skryptu pakietów zasobów po stronie klienta w trybie produkcyjnym. Wywołuje .NET Core CLI [publikowania](/dotnet/core/tools/dotnet-publish) polecenie w celu opublikowania aplikacji.

1. Utwórz plik o nazwie *webpack.config.js*, w katalogu głównym projektu o następującej treści:

    [!code-javascript[webpack.config.js](signalr-typescript-webpack/sample/webpack.config.js)]

    Poprzedniego pliku konfiguruje Webpack kompilacji. Niektóre szczegóły konfiguracji należy zwrócić uwagę:

    * `output` Właściwość zastępuje domyślną wartość *dist*. Zamiast tego emitowanego pakietu w *wwwroot* katalogu.
    * `resolve.extensions` Tablica zawiera *js* do zaimportowania klienta SignalR JavaScript.

1. Utwórz nową *src* katalogu w katalogu głównym projektu. Jej celem jest do przechowywania zasobów projektu po stronie klienta.

1. Utwórz *src/index.html* o następującej zawartości.

    [!code-html[index.html](signalr-typescript-webpack/sample/src/index.html)]

    Poprzedni kod HTML definiuje znaczników umożliwiającego stronie głównej.

1. Utwórz nową *src/css* katalogu. Jej celem jest do przechowywania projektu *.css* plików.

1. Utwórz *src/css/main.css* o następującej treści:

    [!code-css[main.css](signalr-typescript-webpack/sample/src/css/main.css)]

    Poprzedni *main.css* pliku style aplikacji.

1. Utwórz *src/tsconfig.json* o następującej treści:

    [!code-json[tsconfig.json](signalr-typescript-webpack/sample/src/tsconfig.json)]

    Poprzedni kod konfiguruje kompilatora języka TypeScript w celu utworzenia [ECMAScript](https://wikipedia.org/wiki/ECMAScript) zgodnych z 5 JavaScript.

1. Utwórz *src/index.ts* o następującej treści:

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/snippets/index1.ts?name=snippet_IndexTsPhase1File)]

    TypeScript poprzedniego pobiera odwołania do elementów modelu DOM i dołącza dwie procedury obsługi zdarzeń:

    * `keyup`: To zdarzenie generowane, gdy użytkownik wpisze coś w polu tekstowym zidentyfikowane jako `tbMessage`. `send` Funkcja jest wywoływana, gdy użytkownik naciśnie **Enter** klucza.
    * `click`: To zdarzenie generowane, gdy użytkownik kliknie **wysyłania** przycisku. `send` Funkcja jest wywoływana.

## <a name="configure-the-aspnet-core-app"></a>Konfigurowanie aplikacji platformy ASP.NET Core

1. Kod zawarty w `Startup.Configure` metoda Wyświetla *Hello World!*. Zastąp `app.Run` wywołanie metody wywołania [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) i [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_).

    [!code-csharp[Startup](signalr-typescript-webpack/sample/Startup.cs?name=snippet_UseStaticDefaultFiles)]

    Poprzedni kod umożliwia serwerowi zlokalizować i obsługiwać *index.html* pliku, czy użytkownik musi wprowadzić jego pełny adres URL lub adres URL katalogu głównego aplikacji sieci web.

1. Wywołanie [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr#Microsoft_Extensions_DependencyInjection_SignalRDependencyInjectionExtensions_AddSignalR_Microsoft_Extensions_DependencyInjection_IServiceCollection_) w `Startup.ConfigureServices` metody. Dodaje do projektu usługi SignalR.

    [!code-csharp[Startup](signalr-typescript-webpack/sample/Startup.cs?name=snippet_AddSignalR)]

1. Mapa */hub* kierować do `ChatHub` koncentratora. Dodaj następujące wiersze na końcu `Startup.Configure` metody:

    [!code-csharp[Startup](signalr-typescript-webpack/sample/Startup.cs?name=snippet_UseSignalR)]

1. Utwórz nowy katalog o nazwie *koncentratory*, w katalogu głównym projektu. Jego celem jest przechowywanie koncentratora SignalR, który jest tworzony w następnym kroku.

1. Tworzenie Centrum *Hubs/ChatHub.cs* następującym kodem:

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/snippets/ChatHub.cs?name=snippet_ChatHubStubClass)]

1. Dodaj następujący kod w górnej części *Startup.cs* pliku `ChatHub` odwołania:

    [!code-csharp[Startup](signalr-typescript-webpack/sample/Startup.cs?name=snippet_HubsNamespace)]

## <a name="enable-client-and-server-communication"></a>Włącz komunikację klienta i serwera

Aplikacja Wyświetla obecnie prostego formularza do wysyłania wiadomości. Podczas próby zrobić nic się nie dzieje. Serwer nasłuchuje do określonej trasy, ale nie działa z wysyłane wiadomości.

1. Uruchom następujące polecenie w katalogu głównym projektu:

    ```console
    npm install @aspnet/signalr
    ```

    Poprzednie polecenie instaluje [klienta SignalR TypeScript](https://www.npmjs.com/package/@aspnet/signalr), co pozwala klientowi wysłanie wiadomości do serwera.

1. Dodaj wyróżniony kod, aby *src/index.ts* pliku:

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/snippets/index2.ts?name=snippet_IndexTsPhase2File&highlight=2,9-23)]

    Poprzedni kod obsługuje odbieranie komunikatów z serwera. `HubConnectionBuilder` Klasy tworzy nowego Konstruktora do konfigurowania połączenia z serwerem. `withUrl` Funkcja konfiguruje adres URL koncentratora.

    SignalR umożliwia wymianę wiadomości między klientem serwerem. Każdy komunikat ma określoną nazwę. Na przykład masz wiadomości o nazwie `messageReceived` który wykonywania logiki odpowiada za wyświetlanie nowy komunikat w strefie wiadomości. Nasłuchiwanie określonego komunikatu może odbywać się za pośrednictwem `on` funkcji. Można słuchać dowolną liczbą nazw wiadomości. Istnieje również możliwość do przekazania parametrów do wiadomości, takie jak nazwisko autora i zawartość odebranego komunikatu. Gdy klient odbierze komunikat nowy `div` element jest tworzony z nazwisko autora i komunikat zawartości w jego `innerHTML` atrybutu. Jest ona dodawana do głównego `div` element wyświetlanie wiadomości.

1. Teraz, klient może odbierać wiadomości, jest skonfigurowana do wysyłania wiadomości. Dodaj wyróżniony kod, aby *src/index.ts* pliku:

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/src/index.ts?highlight=34-35)]

    Wysyłanie wiadomości przez połączenie Websocket wymaga wywołania `send` metody. Pierwszy parametr metody jest nazwą wiadomości. Dane komunikatów zamieszkuje innych parametrów. W tym przykładzie komunikat zidentyfikowane jako `newMessage` są wysyłane do serwera. Komunikat składa się z nazwy użytkownika i dane wejściowe z pola tekstowego użytkownika. Jeśli działa wysyłania, wartość pola tekstowego jest wyczyszczone.

1. Dodaj metodę wyróżnione `ChatHub` klasy:

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/Hubs/ChatHub.cs?highlight=8-11)]

    Poprzedni kod emituje odebranych komunikatów do wszystkich połączonych użytkowników, gdy serwer odbierze je. Nie trzeba mieć ogólnego `on` metodę, aby odbierać wszystkie wiadomości. Metodę o nazwie po sufiksy z nazwy komunikatu.

    W tym przykładzie klient maszynie wysyła komunikat, który został zidentyfikowany jako `newMessage`. C# `NewMessage` metoda oczekuje danych wysłanych przez klienta. Połączenie jest nawiązywane w przypadku [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) metoda [Clients.All](/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.all). Odebrane komunikaty są wysyłane do wszystkich klientów podłączonych do koncentratora.

## <a name="test-the-app"></a>Testowanie aplikacji

Upewnij się, że aplikacja działa z następujących kroków.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. Uruchom Webpack *wersji* tryb. Przy użyciu **Konsola Menedżera pakietów** okna, uruchom następujące polecenie w katalogu głównym projektu:

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. Wybierz **debugowania** > **uruchomienie bez debugowania** można uruchomić aplikację w przeglądarce bez dołączanie debugera. *Wwwroot/index.html* plik jest obsługiwany w `http://localhost:<port_number>`.

1. Otwórz inne wystąpienie przeglądarki (dowolnej przeglądarki). Wklej adres URL na pasku adresu.

1. Wybierz albo przeglądarkę, wpisz dowolny tekst w **komunikat** pola tekstowego, a następnie kliknij przycisk **wysyłania** przycisku. Unikatowej nazwy użytkownika i wiadomości są wyświetlane na obu stronach natychmiast.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

1. Uruchom Webpack *wersji* tryb, wykonując następujące polecenie w katalogu głównym projektu:

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. Tworzenie i uruchamianie aplikacji, wykonując następujące polecenie w katalogu głównym projektu:

    ```console
    dotnet run
    ```

    Serwer sieci web uruchamia aplikację i udostępnia ją na hoście lokalnym.

1. Otwórz w przeglądarce `http://localhost:<port_number>`. *Wwwroot/index.html* plik jest obsługiwany. Skopiuj adres URL na pasku adresu.

1. Otwórz inne wystąpienie przeglądarki (dowolnej przeglądarki). Wklej adres URL na pasku adresu.

1. Wybierz albo przeglądarkę, wpisz dowolny tekst w **komunikat** pola tekstowego, a następnie kliknij przycisk **wysyłania** przycisku. Unikatowej nazwy użytkownika i wiadomości są wyświetlane na obu stronach natychmiast.

---

![komunikat wyświetlany w oba okna przeglądarki](signalr-typescript-webpack/_static/browsers-message-broadcast.png)

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:signalr/javascript-client>
* <xref:signalr/hubs>