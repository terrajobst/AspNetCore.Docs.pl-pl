---
title: 'Samouczek: Wprowadzenie do SignalR platformy ASP.NET Core'
author: tdykstra
description: W tym samouczku utworzysz aplikację rozmowy, która używa biblioteki SignalR platformy ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 08/20/2018
uid: tutorials/signalr
ms.openlocfilehash: a2573e2817a2d8921954264ca17bc3a7e2a010a8
ms.sourcegitcommit: 847cc1de5526ff42a7303491e6336c2dbdb45de4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "43055835"
---
# <a name="tutorial-get-started-with-signalr-on-aspnet-core"></a>Samouczek: Wprowadzenie do SignalR platformy ASP.NET Core

W tym samouczku pokazano podstawowe informacje dotyczące tworzenia aplikacji w czasie rzeczywistym przy użyciu biblioteki SignalR. Dowiesz się, jak:

> [!div class="checklist"]
> * Tworzenie aplikacji sieci web, która korzysta z biblioteki SignalR platformy ASP.NET Core.
> * Utwórz Centrum SignalR na serwerze.
> * Łączenie z Centrum SignalR poziomu klientów JavaScript.
> * Centrum umożliwia wysyłanie komunikatów za pomocą dowolnego klienta do wszystkich połączonych klientów.

Po zakończeniu będziesz mieć działającą aplikację rozmowy:

![SignalR przykładowej aplikacji](signalr/_static/signalr-get-started-finished.png)

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample)).

## <a name="prerequisites"></a>Wymagania wstępne

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* [Visual Studio 2017 w wersji 15.7.3 lub nowszej](https://www.visualstudio.com/downloads/) z **ASP.NET i tworzenie aplikacji internetowych** obciążenia
* [.NET core SDK 2.1 lub nowszej](https://www.microsoft.com/net/download/all)
* [npm](https://www.npmjs.com/get-npm) (Menedżer pakietów dla środowiska Node.js, używany dla biblioteki klienta SignalR JavaScript).

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* [Visual Studio Code](https://code.visualstudio.com/download)
* [.NET core SDK 2.1 lub nowszej](https://www.microsoft.com/net/download/all)
* [Środowisko C# dla programu Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [npm](https://www.npmjs.com/get-npm) (Menedżer pakietów dla środowiska Node.js, używany dla biblioteki klienta SignalR JavaScript).

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

* [Program Visual Studio dla komputerów Mac w wersji 7.5.4 lub nowszy](https://www.visualstudio.com/downloads/)
* [.NET core SDK 2.1 lub nowszej](https://www.microsoft.com/net/download/all) (dołączone do instalacji programu Visual Studio)
* [npm](https://www.npmjs.com/get-npm) (Menedżer pakietów dla środowiska Node.js, używany dla biblioteki klienta SignalR JavaScript).

---

## <a name="create-the-project"></a>Utwórz projekt

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)

* W menu, wybierz opcję **Plik > Nowy projekt**.

* W **nowy projekt** okno dialogowe, wybierz opcję **zainstalowane > Visual C# > sieci Web > Aplikacja sieci Web programu ASP.NET Core**. Nadaj projektowi nazwę *SignalRChat*.

  ![Okno dialogowe nowego projektu w programie Visual Studio](signalr/_static/signalr-new-project-dialog.png)

* Wybierz **aplikacji sieci Web** do tworzenia projektu, który używa stron Razor.

* Wybierz docelową platformę **platformy .NET Core**, wybierz opcję **platformy ASP.NET Core 2.1**i kliknij przycisk **OK**.

  ![Okno dialogowe nowego projektu w programie Visual Studio](signalr/_static/signalr-new-project-choose-type.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code/)

* Otwórz folder, który można użyć dla nowego projektu.

* W **zintegrowany Terminal**, uruchom następujące polecenie:

   ```console
   dotnet new webapp -o SignalRChat
   ```

   [!INCLUDE[](~/includes/webapp-alias-notice.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

* W menu, wybierz opcję **Plik > nowe rozwiązanie**.

* Wybierz **platformy .NET Core > aplikacji > aplikacji internetowej ASP.NET Core** (nie wybieraj **ASP.NET Core Web App (MVC)**).

* Wybierz **dalej**.

* Nadaj projektowi nazwę *SignalRChat*, a następnie wybierz pozycję **Utwórz**.

---

## <a name="add-the-signalr-client-library"></a>Dodaj bibliotekę klienta SignalR

Serwer biblioteki SignalR znajduje się w [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app). Ale trzeba uzyskać biblioteki klienckiej JavaScript z [npm, Menedżer pakietów Node.js](https://www.npmjs.com/get-npm).

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)

* W **Konsola Menedżera pakietów** (PMC), przejdź do folderu projektu (zawierającego *SignalRChat.csproj* pliku).

  ```console
  cd SignalRChat
  ```

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code/)

2. Przejdź do nowego folderu projektu.

  ```console
  cd SignalRChat
  ``` 

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

* W **terminalu**, przejdź do folderu projektu (zawierającego *SignalRChat.csproj* pliku).

---

* Uruchom inicjator npm, aby utworzyć *package.json* pliku:

  ```console
  npm init -y
  ```

  Polecenie tworzy dane wyjściowe podobne do poniższego przykładu:

  ```console
  Wrote to C:\tmp\SignalRChat\package.json:
  {
    "name": "SignalRChat",
    "version": "1.0.0",
    "description": "",
    "main": "index.js",
    "scripts": {
      "test": "echo \"Error: no test specified\" && exit 1"
    },
    "keywords": [],
    "author": "",
    "license": "ISC"0
  }
  ```

* Instalowanie pakietu biblioteki klienta:

  ```console
  npm install @aspnet/signalr
  ```

  Polecenie tworzy dane wyjściowe podobne do poniższego przykładu:

  ```
  npm notice created a lockfile as package-lock.json. You should commit this file.
  npm WARN signalrchat@1.0.0 No description
  npm WARN signalrchat@1.0.0 No repository field.

  + @aspnet/signalr@1.0.2
  added 1 package in 0.98s
  ```

`npm install` Polecenia pobrane z biblioteki klienta JavaScript w podfolderze *node_modules*. Skopiuj go stamtąd do folderu, w obszarze *wwwroot* odwołujący ze strony sieci web aplikacji rozmów.

* Tworzenie *signalr* folderu w *wwwroot/lib*.

* Kopiuj *signalr.js* plik wchodzącej w skład *node_modules/@aspnet/signalr/dist/browser* do nowego *wwwroot/lib/signalr* folderu.

## <a name="create-the-signalr-hub"></a>Tworzenie Centrum SignalR

A [Centrum](xref:signalr/hubs) jest klasa, która służy jako ogólny potok, który obsługuje komunikację klient serwer.

* W folderze projektu SignalRChat Utwórz *koncentratory* folderu.

* W *Hubs* folderze utwórz *ChatHub.cs* pliku następującym kodem:

  [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

  `ChatHub` Klasa dziedziczy z elementu SignalR [Centrum](/dotnet/api/microsoft.aspnetcore.signalr.hub) klasy. `Hub` Klasa zarządza połączeń, grup i komunikatów.

  `SendMessage` Metoda może być wywoływana przez wszystkie połączone klienta. Wysyła odebranego komunikatu do wszystkich klientów. Kod SignalR jest asynchroniczne w celu zapewnienia maksymalnej skalowalności.

## <a name="configure-the-project-to-use-signalr"></a>Konfigurowanie projektu do korzystania z biblioteki SignalR

Serwer biblioteki SignalR musi być skonfigurowany do przekazywania żądań SignalR z SignalR.

* Dodaj następujący wyróżniony kod do *Startup.cs* pliku.

  [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=7,33,52-55)]

  Te zmiany dodanie SignalR do [wstrzykiwanie zależności](xref:fundamentals/dependency-injection) systemu i [oprogramowania pośredniczącego](xref:fundamentals/middleware/index) potoku.

## <a name="create-the-signalr-client-code"></a>Tworzenie kodu klienta SignalR

* Zastąp zawartość *Pages\Index.cshtml* następującym kodem:

  [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

  Powyższy kod:

  * Tworzy pola tekstowe nazwy i tekst wiadomość i przycisk Prześlij.
  * Tworzy listę z `id="messagesList"` do wyświetlania komunikatów odebranych z Centrum SignalR.
  * Zawiera odwołania do skryptu z SignalR i *chat.js* kod aplikacji, który zostanie utworzony w następnym kroku.

* W *wwwroot/js* folderze utwórz *chat.js* pliku następującym kodem:

  [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

  Powyższy kod:

  * Tworzy i uruchamia połączenie.
  * Dodaje przycisk przesyłania program obsługi, która wysyła komunikaty do Centrum.
  * Dodaje do obiektu połączenia program obsługi, który odbiera komunikaty z Centrum i dodaje je do listy.

## <a name="run-the-app"></a>Uruchamianie aplikacji

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Naciśnij klawisz **kombinację klawiszy CTRL + F5** Aby uruchomić aplikację bez debugowania.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* Naciśnij klawisz **kombinację klawiszy CTRL + F5** Aby uruchomić aplikację bez debugowania.

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

* W menu, wybierz opcję **Uruchom > Uruchom bez debugowania**.

---

* Skopiuj adres URL z paska adresu, a następnie otwórz innego wystąpienia przeglądarki lub karty i wklej adres URL w pasku adresu.

* Wybierz albo przeglądarki, wprowadź nazwę i komunikat, a następnie wybierz **wysyłania** przycisku.

  Nazwa i wiadomości są wyświetlane na obu stronach natychmiast.

  ![SignalR przykładowej aplikacji](signalr/_static/signalr-get-started-finished.png)

> [!TIP]
> Jeśli aplikacja nie działa, Otwórz swoje narzędzia deweloperskie przeglądarki (F12) i przejdź do konsoli. Może pojawić się błędy związane z Twoim kodem HTML i JavaScript. Załóżmy, że możesz umieścić *signalr.js* w innym folderze niż bezpośredniego. W takim przypadku odwołania do tego pliku nie będzie działać, a następnie zostanie wyświetlony błąd 404 w konsoli.
> ![błąd — nie znaleziono signalr.js](signalr/_static/f12-console.png)

## <a name="next-steps"></a>Następne kroki

Klientom na łączenie się z aplikacji SignalR w różnych domenach, należy włączyć udostępnianie zasobów między źródłami (CORS). Aby uzyskać więcej informacji, zobacz [Cross-origin resource sharing](xref:signalr/security?view=aspnetcore-2.1#cross-origin-resource-sharing).

Aby dowiedzieć się więcej na temat biblioteki SignalR, koncentratorów i klientów języka JavaScript, zobacz następujące zasoby:

* [Wprowadzenie do SignalR dla platformy ASP.NET Core](xref:signalr/introduction)
* [Na użytek koncentratory w SignalR platformy ASP.NET Core](xref:signalr/hubs)
* [ASP.NET Core SignalR JavaScript klienta](xref:signalr/javascript-client)
