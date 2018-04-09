---
title: Rozpoczynanie pracy z SignalR platformy ASP.NET Core
author: rachelappel
description: W tym samouczku utworzysz aplikację dla platformy ASP.NET Core za pomocą biblioteki SignalR.
manager: wpickett
ms.author: rachelap
ms.custom: mvc
ms.date: 03/16/2018
ms.prod: aspnet-core
ms.topic: tutorial
ms.technology: aspnet
uid: signalr/get-started
ms.openlocfilehash: cf120d535c85c7871f5b1f27039018ea2405b9cb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
# <a name="get-started-with-signalr-on-aspnet-core"></a>Rozpoczynanie pracy z SignalR platformy ASP.NET Core

Przez [Rachel Appel](https://twitter.com/rachelappel)

[!INCLUDE [Version notice](../includes/signalr-version-notice.md)]

Ten samouczek zawiera podstawowe informacje dotyczące tworzenia aplikacji w czasie rzeczywistym, za pomocą biblioteki SignalR dla platformy ASP.NET Core.

   ![Rozwiązanie](get-started/_static/signalr-get-started-finished.png)

W tym samouczku przedstawiono następujące SignalR zadań związanych z projektowaniem:

> [!div class="checklist"]
> * Utwórz SignalR w aplikacji sieci web platformy ASP.NET Core.
> * Utwórz koncentratora SignalR do dystrybuowania zawartości do klientów.
> * Modyfikowanie `Startup` klasy i skonfigurowania aplikacji.

[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started/sample/) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

# <a name="prerequisites"></a>Wymagania wstępne

Należy zainstalować następujące oprogramowanie:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* [1 — zestaw SDK w wersji Preview .NET core 2.1.0](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview1) lub nowszy
* [Visual Studio 2017](https://www.visualstudio.com/downloads/) wersji 15.6 lub nowszym z **ASP.NET i sieć web development** obciążenia
* [npm](https://www.npmjs.com/get-npm)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* [1 — zestaw SDK w wersji Preview .NET core 2.1.0](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview1) lub nowszy
* [Visual Studio Code](https://code.visualstudio.com/download) 
* [C# dla programu Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [npm](https://www.npmjs.com/get-npm)

-----

## <a name="create-an-aspnet-core-project-that-hosts-signalr-client-and-server"></a>Tworzenie projektu platformy ASP.NET Core obsługującego SignalR klienta i serwera

#### <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)
1. Użyj **pliku** > **nowy projekt** menu opcję i wybierz polecenie **aplikacji sieci Web platformy ASP.NET Core**. Nazwij projekt *SignalRChat*.

   ![Okno dialogowe nowego projektu w programie Visual Studio](get-started/_static/signalr-new-project-dialog.png)

2. Wybierz **aplikacji sieci Web** do tworzenia projektu za pomocą stron Razor. Następnie wybierz **OK**. Upewnij się, że **platformy ASP.NET Core 2.1** wybrano selektora framework, chociaż SignalR działa na starsze wersje programu .NET.

   ![Okno dialogowe nowego projektu w programie Visual Studio](get-started/_static/signalr-new-project-choose-type.png)

3. Kliknij prawym przyciskiem myszy projekt w **Eksploratora rozwiązań** > **Dodaj** > **nowy element** > **plik konfiguracji programu npm** . Nadaj nazwę plikowi *package.json*.

4. Uruchom następujące polecenie **Konsola Menedżera pakietów** okno z katalogu głównego projektu:

    ```console
      npm install @aspnet/signalr
    ```
5. Kopiuj <em>signalr.js</em> plik z <em>node_modules\\ @aspnet\signalr\dist\browser</em>  do <em>wwwroot\lib</em> folder w projekcie.

#### <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code/)
1. Z **zintegrowane Terminal**, uruchom następujące polecenie:

    ```console
      dotnet new razor -o SignalRChat
    ```

2. Zainstaluj JavaScript klienta biblioteki przy użyciu *npm*.

    ```
      npm init -y
      npm install @aspnet/signalr
    ```

* * *
## <a name="create-the-signalr-hub"></a>Tworzenie Centrum SignalR

Koncentrator jest klasa, która służy jako wysokiego poziomu potok, który umożliwia klienta i serwera, wywoływanie metod na siebie.

#### <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)
1. Dodaj do projektu klasę, wybierając **pliku** > **nowy** > **pliku** i wybierając **Visual C# klasy**.

2. Dziedzicz `Microsoft.AspNetCore.SignalR.Hub`. `Hub` Zawiera klasę, właściwości i zdarzeń związanych z zarządzaniem połączeń i grupy, a także wysyłania i odbierania danych.

3. Utwórz `SendMessage` metodę, która wysyła komunikat do wszystkich klientów połączonych rozmów. Zwróć uwagę, zwraca [zadań](https://msdn.microsoft.com/en-us/library/system.threading.tasks.task(v=vs.110).aspx), ponieważ asynchroniczny SignalR. Kod asynchroniczny skaluje się lepiej.

   [!code-csharp[Startup](get-started/sample/Hubs/ChatHub.cs?range=7-14)]

#### <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code/)
1. Otwórz *SignalRChat* folderu w programie Visual Studio Code.

2. Dodaj klasę do projektu, wybierając **pliku** > **nowy plik** z menu.

3. Dziedzicz `Microsoft.AspNetCore.SignalR.Hub`. `Hub` Zawiera klasę, właściwości i zdarzeń związanych z zarządzaniem połączeń i grupy, a także wysyłania i odbierania danych do klientów.

4. Dodaj `SendMessage` metodę do klasy. `SendMessage` Metoda wysyła komunikat do wszystkich klientów połączonych rozmów. Zwróć uwagę, zwraca [zadań](/dotnet/api/system.threading.tasks.task), ponieważ asynchroniczny SignalR. Kod asynchroniczny skaluje się lepiej.

   [!code-csharp[Startup](get-started/sample/Hubs/ChatHub.cs?range=7-14)]

* * *
## <a name="configure-the-project-to-use-signalr"></a>Konfigurowanie projektu do użycia biblioteki SignalR

Serwer SignalR musi być skonfigurowany, tak aby wie, do przekazywania żądań do SignalR.

1. Aby skonfigurować projekt SignalR, zmodyfikuj projektu `Startup.ConfigureServices` metody.

   `services.AddSignalR` dodaje SignalR jako część [oprogramowanie pośredniczące](xref:fundamentals/middleware/index) potoku.

2. Do konfigurowania tras sieci hubs przy użyciu `UseSignalR`.

   [!code-csharp[Startup](get-started/sample/Startup.cs?highlight=22,40-43)]

## <a name="create-the-signalr-client-code"></a>Utwórz kod klienta SignalR

1. Zamień zawartość *Pages\Index.cshtml* następującym kodem:

   [!code-cshtml[Index](get-started/sample/Pages/Index.cshtml)]

   Poprzedni kod HTML Wyświetla nazwę i pola wiadomości i przycisk przesyłania. Zwróć uwagę, odwołań do skryptów u dołu: odwołanie do biblioteki SignalR i *chat.js*.

2. Dodaj plik JavaScript o nazwie *chat.js*, do *wwwroot\js* folderu. Dodaj do niej następujący kod:

   [!code-javascript[Index](get-started/sample/wwwroot/js/chat.js)]

## <a name="run-the-app"></a>Uruchamianie aplikacji

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. Wybierz **debugowania** > **uruchomienie bez debugowania** przeglądarkę i załadować witryny sieci Web lokalnie. Skopiuj adres URL na pasku adresu.

1. Otwórz inne wystąpienie przeglądarki (dowolnej przeglądarki) i wklej adres URL na pasku adresu.

1. Wybierz albo przeglądarkę, wprowadź nazwę i wiadomość, a następnie kliknij przycisk **wysyłania** przycisku. Nazwa i wiadomości są wyświetlane na obu stronach natychmiast.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

1. Naciśnij klawisz **debugowania** (F5), aby skompilować i uruchomić program. Uruchomienie programu powoduje otwarcie okna przeglądarki.

1. Otwiera inne okno przeglądarki i załadować witryny sieci Web lokalnie w nim.

1. Wybierz albo przeglądarkę, wprowadź nazwę i wiadomość, a następnie kliknij przycisk **wysyłania** przycisku. Nazwa i wiadomości są wyświetlane na obu stronach natychmiast.

-----

  ![Rozwiązanie](get-started/_static/signalr-get-started-finished.png)

## <a name="related-resources"></a>Zasoby pokrewne

[Wprowadzenie do platformy ASP.NET Core SignalR](introduction.md)