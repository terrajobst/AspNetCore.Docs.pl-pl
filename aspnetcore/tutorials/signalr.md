---
title: Rozpoczynanie pracy z SignalR platformy ASP.NET Core
author: rachelappel
description: W tym samouczku utworzysz aplikację dla platformy ASP.NET Core za pomocą biblioteki SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 05/22/2018
uid: tutorials/signalr
ms.openlocfilehash: e57fa86476dcb57a04211240a7202dcfc2e263ad
ms.sourcegitcommit: 356c8d394aaf384c834e9c90cabab43bfe36e063
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/26/2018
ms.locfileid: "36960609"
---
# <a name="get-started-with-signalr-on-aspnet-core"></a>Rozpoczynanie pracy z SignalR platformy ASP.NET Core

Przez [Rachel Appel](https://twitter.com/rachelappel)

Ten samouczek zawiera podstawowe informacje dotyczące tworzenia aplikacji w czasie rzeczywistym, za pomocą biblioteki SignalR dla platformy ASP.NET Core.

   ![Rozwiązanie](signalr/_static/signalr-get-started-finished.png)

W tym samouczku przedstawiono następujące SignalR zadań związanych z projektowaniem:

> [!div class="checklist"]
> * Utwórz SignalR w aplikacji sieci web platformy ASP.NET Core.
> * Utwórz koncentratora SignalR do dystrybuowania zawartości do klientów.
> * Modyfikowanie `Startup` klasy i skonfigurowania aplikacji.

[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

# <a name="prerequisites"></a>Wymagania wstępne

Należy zainstalować następujące oprogramowanie:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* [Oprogramowanie .NET core SDK 2.1 lub nowszej](https://www.microsoft.com/net/download/all)
* [Visual Studio 2017](https://www.visualstudio.com/downloads/) wersji 15.7 lub nowszym z **ASP.NET i sieć web development** obciążenia
* [npm](https://www.npmjs.com/get-npm)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* [Oprogramowanie .NET core SDK 2.1 lub nowszej](https://www.microsoft.com/net/download/all)
* [Visual Studio Code](https://code.visualstudio.com/download)
* [C# dla programu Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [npm](https://www.npmjs.com/get-npm)

-----

## <a name="create-an-aspnet-core-project-that-hosts-signalr-client-and-server"></a>Tworzenie projektu platformy ASP.NET Core obsługującego SignalR klienta i serwera

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)

1. Użyj **pliku** > **nowy projekt** menu opcję i wybierz polecenie **aplikacji sieci Web platformy ASP.NET Core**. Nazwij projekt *SignalRChat*.

   ![Okno dialogowe nowego projektu w programie Visual Studio](signalr/_static/signalr-new-project-dialog.png)

2. Wybierz **aplikacji sieci Web** do tworzenia projektu za pomocą stron Razor. Następnie wybierz **OK**. Upewnij się, że **platformy ASP.NET Core 2.1** wybrano selektora framework, chociaż SignalR działa na starsze wersje programu .NET.

   ![Okno dialogowe nowego projektu w programie Visual Studio](signalr/_static/signalr-new-project-choose-type.png)

Visual Studio zawiera `Microsoft.AspNetCore.SignalR` pakiet zawierający bibliotek serwer jako część jej **aplikacji sieci Web platformy ASP.NET Core** szablonu. Jednak biblioteka klienta języka JavaScript dla biblioteki SignalR musi być zainstalowany przy użyciu *npm*.

3. Uruchom następujące polecenia **Konsola Menedżera pakietów** okno z katalogu głównego projektu:

    ```console
    npm init -y
    npm install @aspnet/signalr
    ```

4. Utwórz nowy folder o nazwie "signalr" wewnątrz *lib* folder w projekcie. Kopiuj *signalr.js* plik z *node_modules\\ @aspnet\signalr\dist\browser*  do tego folderu.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code/)

1. Z **zintegrowane Terminal**, uruchom następujące polecenie:

    ```console
    dotnet new webapp -o SignalRChat
    ```

    [!INCLUDE[](~/includes/webapp-alias-notice.md)]

2. Zainstaluj JavaScript klienta biblioteki przy użyciu *npm*.

    ```console
    npm init -y
    npm install @aspnet/signalr
    ```

3. Utwórz nowy folder o nazwie "signalr" wewnątrz *lib* folder w projekcie. Kopiuj *signalr.js* plik z *node_modules\\ @aspnet\signalr\dist\browser*  do tego folderu.

---

## <a name="create-the-signalr-hub"></a>Tworzenie Centrum SignalR

Koncentrator jest klasa, która służy jako wysokiego poziomu potok, który umożliwia klienta i serwera, wywoływanie metod na siebie.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)

1. Dodaj do projektu klasę, wybierając **pliku** > **nowy** > **pliku** i wybierając **Visual C# klasy**. Nazwa klasy `ChatHub` i plik *ChatHub.cs*.

2. Dziedzicz `Microsoft.AspNetCore.SignalR.Hub`. `Hub` Zawiera klasę, właściwości i zdarzeń związanych z zarządzaniem połączeń i grupy, a także wysyłania i odbierania danych.

3. Utwórz `SendMessage` metodę, która wysyła komunikat do wszystkich klientów połączonych rozmów. Zwróć uwagę, zwraca [zadań](https://msdn.microsoft.com/library/system.threading.tasks.task(v=vs.110).aspx), ponieważ asynchroniczny SignalR. Kod asynchroniczny skaluje się lepiej.

   [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code/)

1. Otwórz *SignalRChat* folderu w programie Visual Studio Code.

2. Dodaj klasę do projektu, wybierając **pliku** > **nowy plik** z menu. Nazwa klasy `ChatHub` i plik *ChatHub.cs*.

3. Dziedzicz `Microsoft.AspNetCore.SignalR.Hub`. `Hub` Zawiera klasę, właściwości i zdarzeń związanych z zarządzaniem połączeń i grupy, a także wysyłania i odbierania danych do klientów.

4. Dodaj `SendMessage` metodę do klasy. `SendMessage` Metoda wysyła komunikat do wszystkich klientów połączonych rozmów. Zwróć uwagę, zwraca [zadań](/dotnet/api/system.threading.tasks.task), ponieważ asynchroniczny SignalR. Kod asynchroniczny skaluje się lepiej.

   [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

-----

## <a name="configure-the-project-to-use-signalr"></a>Konfigurowanie projektu do użycia biblioteki SignalR

Serwer SignalR musi być skonfigurowany, tak aby wie, do przekazywania żądań do SignalR.

1. Aby skonfigurować projekt SignalR, zmodyfikuj projektu `Startup.ConfigureServices` metody.

   `services.AddSignalR` dodaje SignalR jako część [oprogramowanie pośredniczące](xref:fundamentals/middleware/index) potoku.

2. Do konfigurowania tras sieci hubs przy użyciu `UseSignalR`.

   [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=37,57-60)]

## <a name="create-the-signalr-client-code"></a>Utwórz kod klienta SignalR

1. Dodaj plik JavaScript o nazwie *chat.js*, do *wwwroot\js* folderu. Dodaj do niej następujący kod:

   [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

2. Zamień zawartość *Pages\Index.cshtml* następującym kodem:

   [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

   Poprzedni kod HTML Wyświetla nazwę i pola wiadomości i przycisk przesyłania. Zwróć uwagę, odwołań do skryptów u dołu: odwołanie do biblioteki SignalR i *chat.js*.

## <a name="run-the-app"></a>Uruchamianie aplikacji

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. Wybierz **debugowania** > **uruchomienie bez debugowania** przeglądarkę i załadować witryny sieci Web lokalnie. Skopiuj adres URL na pasku adresu.

1. Otwórz inne wystąpienie przeglądarki (dowolnej przeglądarki) i wklej adres URL na pasku adresu.

1. Wybierz albo przeglądarkę, wprowadź nazwę i wiadomość, a następnie kliknij przycisk **wysyłania** przycisku. Nazwa i wiadomości są wyświetlane na obu stronach natychmiast.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

1. Naciśnij klawisz **debugowania** (F5), aby skompilować i uruchomić program. Uruchomienie programu powoduje otwarcie okna przeglądarki.

1. Otwiera inne okno przeglądarki i załadować witryny sieci Web lokalnie w nim.

1. Wybierz albo przeglądarkę, wprowadź nazwę i wiadomość, a następnie kliknij przycisk **wysyłania** przycisku. Nazwa i wiadomości są wyświetlane na obu stronach natychmiast.

---

  ![Rozwiązanie](signalr/_static/signalr-get-started-finished.png)

## <a name="related-resources"></a>Zasoby pokrewne

[Wprowadzenie do platformy ASP.NET Core SignalR](xref:signalr/introduction)
