---
title: 'Samouczek: Wprowadzenie do SignalR platformy ASP.NET Core'
author: tdykstra
description: W tym samouczku utworzysz aplikację rozmowy, która używa biblioteki SignalR platformy ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 08/31/2018
uid: tutorials/signalr
ms.openlocfilehash: 6d96331a4630f766ca11edb056fd3e13b52b6ae4
ms.sourcegitcommit: 4cd8dce371d63a66d780e4af1baab2bcf9d61b24
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/06/2018
ms.locfileid: "43893168"
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

* [Visual Studio 2017 w wersji należy zachować 15,8 lub nowszej](https://www.visualstudio.com/downloads/) z **ASP.NET i tworzenie aplikacji internetowych** obciążenia
* [.NET core SDK 2.1 lub nowszej](https://www.microsoft.com/net/download/all)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* [Visual Studio Code](https://code.visualstudio.com/download)
* [.NET core SDK 2.1 lub nowszej](https://www.microsoft.com/net/download/all)
* [Środowisko C# dla programu Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

* [Program Visual Studio dla komputerów Mac w wersji 7.5.4 lub nowszy](https://www.visualstudio.com/downloads/)
* [.NET core SDK 2.1 lub nowszej](https://www.microsoft.com/net/download/all) (dołączone do instalacji programu Visual Studio)

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

Serwer biblioteki SignalR znajduje się w [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app). Biblioteki klienta JavaScript automatycznie nie jest zawarty w projekcie. W tym samouczku użyjesz [Library Manager (LibMan)](xref:client-side/libman/index) można pobrać z biblioteki klienta z *unpkg*. [unpkg](https://unpkg.com/#/) jest [usługa content delivery network](https://wikipedia.org/wiki/Content_delivery_network) , można dostarczać niczego w [npm, Menedżer pakietów Node.js](https://www.npmjs.com/get-npm).

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)

* W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj** > **biblioteki po stronie klienta**.

* W **Dodaj biblioteki po stronie klienta** okno dialogowe, aby uzyskać **dostawcy** wybierz **unpkg**. 

* Aby uzyskać **biblioteki**, wprowadź _@aspnet/signalr @1_i wybierz najnowszą wersję, która nie jest w wersji zapoznawczej.

  ![Dodaj okno dialogowe biblioteki po stronie klienta — Wybieranie biblioteki](signalr/_static/libman1.png)

* Wybierz **wybierz konkretne pliki**, rozwiń węzeł *dist/przeglądarki* folder, a następnie wybierz *signalr.js* i *signalr.min.js*.

* Ustaw **lokalizacji docelowej** do *wwwroot/lib/signalr/* i wybierz **zainstalować**.

  ![Dodaj okno dialogowe z biblioteki klienta - wybranie plików i miejsce docelowe](signalr/_static/libman2.png)

  [LibMan](xref:client-side/libman/index) tworzy *wwwroot/lib/signalr* folder i kopiuje wybrane pliki.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code/)

* W **zintegrowany Terminal**, uruchom następujące polecenie, aby zainstalować LibMan.

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* Przejdź do folderu projektu (zawierającego *SignalRChat.csproj* pliku).

* Uruchom następujące polecenie, aby przy użyciu LibMan uzyskać biblioteki klienta SignalR. Być może musisz odczekaj kilka sekund, zanim dane wyjściowe.

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot\lib\signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  Parametry określ następujące opcje:
  * Użyj dostawcy unpkg.
  * Skopiuj pliki do *wwwroot/lib/signalr* docelowego.
  * Skopiuj tylko określonych plików.

  Dane wyjściowe wyglądają jak w poniższym przykładzie:

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot\lib\signalr"
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

* W **terminalu**, uruchom następujące polecenie, aby zainstalować LibMan.

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* Przejdź do folderu projektu (zawierającego *SignalRChat.csproj* pliku).

* Uruchom następujące polecenie, aby przy użyciu LibMan uzyskać biblioteki klienta SignalR.

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot\lib\signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  Parametry określ następujące opcje:
  * Użyj dostawcy unpkg.
  * Skopiuj pliki do *wwwroot/lib/signalr* docelowego.
  * Skopiuj tylko określonych plików.

  Dane wyjściowe wyglądają jak w poniższym przykładzie:

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot\lib\signalr"
  ```

---

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
