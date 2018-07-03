---
title: Wprowadzenie do SignalR platformy ASP.NET Core
author: rachelappel
description: W tym samouczku utworzysz aplikację dla platformy ASP.NET Core przy użyciu SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 05/22/2018
uid: tutorials/signalr
ms.openlocfilehash: ca9145d9e16c23e34bbc1d84ff01ce02709187ce
ms.sourcegitcommit: 08f1a9baa97060da5168840b332c9c0805b5f901
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/02/2018
ms.locfileid: "37144875"
---
# <a name="get-started-with-signalr-on-aspnet-core"></a>Wprowadzenie do SignalR platformy ASP.NET Core

Przez [Rachel Appel](https://twitter.com/rachelappel)

W tym samouczku pokazano podstawowe informacje dotyczące tworzenia aplikacji w czasie rzeczywistym przy użyciu biblioteki SignalR dla platformy ASP.NET Core.

   ![Rozwiązanie](signalr/_static/signalr-get-started-finished.png)

W tym samouczku przedstawiono następujące zadania deweloperskie SignalR:

> [!div class="checklist"]
> * Utwórz SignalR w aplikacji internetowej ASP.NET Core.
> * Utwórz Centrum SignalR, aby wypchnąć zawartości do klientów.
> * Modyfikowanie `Startup` klasy i skonfigurowania aplikacji.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

## <a name="prerequisites"></a>Wymagania wstępne

Zainstaluj następujące oprogramowanie:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* [.NET core SDK 2.1 lub nowszej](https://www.microsoft.com/net/download/all)
* [Program Visual Studio 2017](https://www.visualstudio.com/downloads/) wersji 15.7 lub nowszej z **ASP.NET i tworzenie aplikacji internetowych** obciążenia
* [npm](https://www.npmjs.com/get-npm)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* [.NET core SDK 2.1 lub nowszej](https://www.microsoft.com/net/download/all)
* [Visual Studio Code](https://code.visualstudio.com/download)
* [Środowisko C# dla programu Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [npm](https://www.npmjs.com/get-npm)

-----

## <a name="create-an-aspnet-core-project-that-hosts-signalr-client-and-server"></a>Tworzenie projektu platformy ASP.NET Core, który jest hostem SignalR klienta i serwera

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)

1. Użyj **pliku** > **nowy projekt** menu opcji, a następnie wybierz **aplikacji sieci Web programu ASP.NET Core**. Nadaj projektowi nazwę *SignalRChat*.

   ![Okno dialogowe nowego projektu w programie Visual Studio](signalr/_static/signalr-new-project-dialog.png)

2. Wybierz **aplikacji sieci Web** do utworzenia projektu przy użyciu stron Razor. Następnie wybierz pozycję **OK**. Upewnij się, że **platformy ASP.NET Core 2.1** wybrane z selektora framework, chociaż SignalR działa w starszej wersji platformy .NET.

   ![Okno dialogowe nowego projektu w programie Visual Studio](signalr/_static/signalr-new-project-choose-type.png)

Program Visual Studio obejmuje `Microsoft.AspNetCore.SignalR` pakietu zawierającego jego serwera biblioteki jako część jej **aplikacji sieci Web programu ASP.NET Core** szablonu. Jednak z biblioteki klienta JavaScript dla elementu SignalR musi być zainstalowany przy użyciu *npm*.

3. Uruchom następujące polecenia **Konsola Menedżera pakietów** okna z katalogu głównego projektu:

    ```console
    npm init -y
    npm install @aspnet/signalr
    ```

4. Utwórz nowy folder o nazwie "signalr" wewnątrz *lib* folder w projekcie. Kopiuj *signalr.js* plik wchodzącej w skład *node_modules\\ @aspnet\signalr\dist\browser*  do tego folderu.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code/)

1. Z **zintegrowany Terminal**, uruchom następujące polecenie:

    ```console
    dotnet new webapp -o SignalRChat
    ```

    [!INCLUDE[](~/includes/webapp-alias-notice.md)]

2. Zainstaluj język JavaScript klienta biblioteki przy użyciu *npm*.

    ```console
    npm init -y
    npm install @aspnet/signalr
    ```

3. Utwórz nowy folder o nazwie "signalr" wewnątrz *lib* folder w projekcie. Kopiuj *signalr.js* plik wchodzącej w skład *node_modules\\ @aspnet\signalr\dist\browser*  do tego folderu.

---

## <a name="create-the-signalr-hub"></a>Tworzenie Centrum SignalR

Koncentrator, to klasa, która służy jako ogólny potok, który umożliwia klienta i serwera, wywoływanie metod na siebie nawzajem.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)

1. Dodaj klasę do projektu, wybierając **pliku** > **nowy** > **pliku** i wybierając polecenie **klasy Visual C#**. Nazwa klasy `ChatHub` i plik *ChatHub.cs*.

2. Dziedzicz `Microsoft.AspNetCore.SignalR.Hub`. `Hub` Klasa zawiera właściwości i zdarzeń zarządzania połączeniami i grup, a także wysyłania i odbierania danych.

3. Utwórz `SendMessage` metodę, która wysyła wiadomość do wszystkich klientów połączonych rozmowy. Zwróć uwagę, funkcja zwraca [zadań](https://msdn.microsoft.com/library/system.threading.tasks.task(v=vs.110).aspx), ponieważ SignalR jest asynchroniczna. Kod asynchroniczny skaluje się lepiej.

   [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code/)

1. Otwórz *SignalRChat* folderu w programie Visual Studio Code.

2. Dodaj klasę do projektu, wybierając **pliku** > **nowy plik** z menu. Nazwa klasy `ChatHub` i plik *ChatHub.cs*.

3. Dziedzicz `Microsoft.AspNetCore.SignalR.Hub`. `Hub` Klasa zawiera właściwości i zdarzeń związanych z zarządzaniem połączeń i grupy, a także wysyłania i odbierania danych do klientów.

4. Dodaj `SendMessage` metodę do klasy. `SendMessage` Metoda wysyła wiadomość do wszystkich klientów połączonych rozmowy. Zwróć uwagę, funkcja zwraca [zadań](/dotnet/api/system.threading.tasks.task), ponieważ SignalR jest asynchroniczna. Kod asynchroniczny skaluje się lepiej.

   [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

-----

## <a name="configure-the-project-to-use-signalr"></a>Konfigurowanie projektu do korzystania z biblioteki SignalR

Serwer biblioteki SignalR musi być skonfigurowany, tak aby wie, aby przekazywać żądania do SignalR.

1. Aby skonfigurować projekt SignalR, zmodyfikuj projektu `Startup.ConfigureServices` metody.

   `services.AddSignalR` udostępnia usługi SignalR [wstrzykiwanie zależności](xref:fundamentals/dependency-injection) systemu.

1. Konfigurowanie tras do Twojej koncentratorów, za pomocą `UseSignalR` w `Configure` metody. `app.UseSignalR` dodaje SignalR do [oprogramowania pośredniczącego](xref:fundamentals/middleware/index) potoku.

   [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=37,57-60)]

## <a name="create-the-signalr-client-code"></a>Tworzenie kodu klienta SignalR

1. Dodaj plik języka JavaScript o nazwie *chat.js*, *wwwroot\js* folderu. Dodaj następujący kod do niego:

   [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

2. Zastąp zawartość *Pages\Index.cshtml* następującym kodem:

   [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

   Poprzedni kod HTML Wyświetla nazwę i pola wiadomości i przycisk Prześlij. Zwróć uwagę, odwołania do skryptu, u dołu: odwołanie do biblioteki SignalR i *chat.js*.

## <a name="run-the-app"></a>Uruchamianie aplikacji

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. Wybierz **debugowania** > **Uruchom bez debugowania** Uruchom przeglądarkę i załadować witryny sieci Web lokalnie. Skopiuj adres URL z paska adresu.

1. Otwórz inne wystąpienie przeglądarki (w dowolnej przeglądarce) i wklej adres URL w pasku adresu.

1. Wybierz albo przeglądarki, wprowadź nazwę i komunikat, a następnie kliknij przycisk **wysyłania** przycisku. Nazwa i wiadomości są wyświetlane na obu stronach natychmiast.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

1. Naciśnij klawisz **debugowania** (F5), aby skompilować i uruchomić program. Uruchomienie programu powoduje otwarcie okna przeglądarki.

1. Otwarte inne okno przeglądarki i obciążenia witryny sieci Web lokalnie w nim.

1. Wybierz albo przeglądarki, wprowadź nazwę i komunikat, a następnie kliknij przycisk **wysyłania** przycisku. Nazwa i wiadomości są wyświetlane na obu stronach natychmiast.

---

  ![Rozwiązanie](signalr/_static/signalr-get-started-finished.png)

## <a name="related-resources"></a>Powiązane zasoby

[Wprowadzenie do SignalR platformy ASP.NET Core](xref:signalr/introduction)
