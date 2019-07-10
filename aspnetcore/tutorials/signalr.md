---
title: Wprowadzenie do biblioteki SignalR platformy ASP.NET Core
author: bradygaster
description: W tym samouczku utworzysz aplikację rozmowy, która korzysta z biblioteki SignalR platformy ASP.NET Core.
ms.author: bradyg
ms.custom: mvc
ms.date: 07/08/2019
uid: tutorials/signalr
ms.openlocfilehash: fd3324dfa0731ae16747178d83bd93ed95dd15ce
ms.sourcegitcommit: bee530454ae2b3c25dc7ffebf93536f479a14460
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/10/2019
ms.locfileid: "67724475"
---
# <a name="tutorial-get-started-with-aspnet-core-signalr"></a>Samouczek: Wprowadzenie do biblioteki SignalR platformy ASP.NET Core

W tym samouczku pokazano podstawowe informacje dotyczące tworzenia aplikacji w czasie rzeczywistym przy użyciu biblioteki SignalR. Dowiesz się, jak:

> [!div class="checklist"]
> * Utwórz projekt sieci web.
> * Dodaj bibliotekę klienta SignalR.
> * Utwórz Centrum SignalR.
> * Skonfiguruj projekt do korzystania z SignalR.
> * Dodaj kod, który wysyła komunikaty z dowolnego klienta do wszystkich połączonych klientów.

Po zakończeniu będziesz mieć działającą aplikację rozmowy:

![SignalR przykładowej aplikacji](signalr/_static/signalr-get-started-finished.png)

## <a name="prerequisites"></a>Wymagania wstępne

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-web-app-project"></a>Tworzenie projektu aplikacji sieci web

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)

* W menu, wybierz opcję **Plik > Nowy projekt**.

* W **Utwórz nowy projekt** okno dialogowe, wybierz opcję **aplikacji sieci Web programu ASP.NET Core**, a następnie wybierz pozycję **dalej**.

* W **konfigurowania nowego projektu** okno dialogowe, nazwę projektu *SignalRChat*, a następnie wybierz pozycję **Utwórz**.

* W **Tworzenie nowej aplikacji sieci Web platformy ASP.NET Core** okno dialogowe, wybierz opcję **platformy .NET Core** i **platformy ASP.NET Core 3.0**. 

* Wybierz **aplikacji sieci Web** utworzyć projekt, który korzysta ze stronami Razor, a następnie wybierz pozycję **Utwórz**.

  ![Okno dialogowe nowego projektu w programie Visual Studio](signalr/_static/signalr-new-project-dialog.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code/)

* Otwórz [zintegrowany terminal](https://code.visualstudio.com/docs/editor/integrated-terminal) do folderu, w którym zostanie utworzony nowy folder projektu.

* Uruchom następujące polecenia:

   ```console
   dotnet new webapp -o SignalRChat
   code -r SignalRChat
   ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

* W menu, wybierz opcję **Plik > nowe rozwiązanie**.

* Wybierz **platformy .NET Core > aplikacji > Aplikacja sieci Web** (nie wybieraj **aplikacji sieci Web (Model-View-Controller)** ), a następnie wybierz pozycję **dalej**.

* Upewnij się, że **platformy docelowej** jest ustawiona na **.NET Core 3.0 to**, a następnie wybierz pozycję **dalej**.

* Nadaj projektowi nazwę *SignalRChat*, a następnie wybierz pozycję **Utwórz**.

---

## <a name="add-the-signalr-client-library"></a>Dodaj bibliotekę klienta SignalR

Serwer biblioteki SignalR znajduje się w udostępnionej platformy ASP.NET Core 3.0. Biblioteki klienta JavaScript automatycznie nie jest zawarty w projekcie. W tym samouczku używasz Library Manager (LibMan) można pobrać z biblioteki klienta z *unpkg*. unpkg jest usługa content delivery network (CDN)), można dostarczać niczego w Menedżer pakietów npm oraz Menedżera pakietów środowiska Node.js.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)

* W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj** > **biblioteki po stronie klienta**.

* W **Dodaj biblioteki po stronie klienta** okno dialogowe, aby uzyskać **dostawcy** wybierz **unpkg**.

* Aby uzyskać **biblioteki**, wprowadź `@aspnet/signalr@next`.
<!-- when 3.0 is released, change @next to @latest -->

* Wybierz **wybierz konkretne pliki**, rozwiń węzeł *dist/przeglądarki* folder, a następnie wybierz *signalr.js* i *signalr.min.js*.

* Ustaw **lokalizacji docelowej** do *wwwroot/lib/signalr/* i wybierz **zainstalować**.

  ![Dodaj okno dialogowe biblioteki po stronie klienta — Wybieranie biblioteki](signalr/_static/libman1.png)

  Tworzy LibMan *wwwroot/lib/signalr* folder i kopiuje wybrane pliki.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code/)

* W zintegrowanym terminalu uruchom następujące polecenie, aby zainstalować LibMan.

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* Uruchom następujące polecenie, aby przy użyciu LibMan uzyskać biblioteki klienta SignalR. Być może musisz odczekaj kilka sekund, zanim dane wyjściowe.

  ```console
  libman install @aspnet/signalr@next -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  Parametry określ następujące opcje:
  * Użyj dostawcy unpkg.
  * Skopiuj pliki do *wwwroot/lib/signalr* docelowego.
  * Skopiuj tylko określonych plików.

  Dane wyjściowe wyglądają jak w poniższym przykładzie:

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@next" to "wwwroot/lib/signalr"
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

* W **terminalu**, uruchom następujące polecenie, aby zainstalować LibMan.

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* Przejdź do folderu projektu (zawierającego *SignalRChat.csproj* pliku).

* Uruchom następujące polecenie, aby przy użyciu LibMan uzyskać biblioteki klienta SignalR.

  ```console
  libman install @aspnet/signalr@next -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  Parametry określ następujące opcje:
  * Użyj dostawcy unpkg.
  * Skopiuj pliki do *wwwroot/lib/signalr* docelowego.
  * Skopiuj tylko określonych plików.

  Dane wyjściowe wyglądają jak w poniższym przykładzie:

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@next" to "wwwroot/lib/signalr"
  ```

---

## <a name="create-a-signalr-hub"></a>Tworzenie Centrum SignalR

A *Centrum* jest klasa, która służy jako ogólny potok, który obsługuje komunikację klient serwer.

* W folderze projektu SignalRChat Utwórz *koncentratory* folderu.

* W *Hubs* folderze utwórz *ChatHub.cs* pliku następującym kodem:

  [!code-csharp[Startup](signalr/sample-snapshot/ChatHub.cs)]

  `ChatHub` Klasa dziedziczy z elementu SignalR `Hub` klasy. `Hub` Klasa zarządza połączeń, grup i komunikatów.

  `SendMessage` Metoda może być wywoływana przez połączone klienta, aby wysłać wiadomość do wszystkich klientów. Kod klienta JavaScript, który wywołuje metodę przedstawiono w dalszej części tego samouczka. Kod SignalR jest asynchroniczne w celu zapewnienia maksymalnej skalowalności.

## <a name="configure-signalr"></a>Konfigurowanie biblioteki SignalR

Serwer biblioteki SignalR musi być skonfigurowany do przekazywania żądań SignalR z SignalR.

* Dodaj następujący wyróżniony kod do *Startup.cs* pliku.

  [!code-csharp[Startup](signalr/sample-snapshot/Startup.cs?highlight=6,30,58)]

  Te zmiany dodanie SignalR do wstrzykiwania zależności platformy ASP.NET Core i routing systemów.

## <a name="add-signalr-client-code"></a>Dodaj kod klienta SignalR

* Zastąp zawartość *Pages\Index.cshtml* następującym kodem:

  [!code-cshtml[Index](signalr/sample-snapshot/Index.cshtml)]

  Powyższy kod:

  * Tworzy pola tekstowe nazwy i tekst wiadomość i przycisk Prześlij.
  * Tworzy listę z `id="messagesList"` do wyświetlania komunikatów odebranych z Centrum SignalR.
  * Zawiera odwołania do skryptu z SignalR i *chat.js* kod aplikacji, który zostanie utworzony w następnym kroku.

* W *wwwroot/js* folderze utwórz *chat.js* pliku następującym kodem:

  [!code-javascript[Index](signalr/sample-snapshot/chat.js)]

  Powyższy kod:

  * Tworzy i uruchamia połączenie.
  * Dodaje przycisk przesyłania program obsługi, która wysyła komunikaty do Centrum.
  * Dodaje do obiektu połączenia program obsługi, który odbiera komunikaty z Centrum i dodaje je do listy.

## <a name="run-the-app"></a>Uruchamianie aplikacji

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Naciśnij klawisz **kombinację klawiszy CTRL + F5** Aby uruchomić aplikację bez debugowania.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* W zintegrowanym terminalu uruchom następujące polecenie:

  ```console
  dotnet run -p SignalRChat.csproj
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

* W menu, wybierz opcję **Uruchom > Uruchom bez debugowania**.

---

* Skopiuj adres URL z paska adresu, a następnie otwórz innego wystąpienia przeglądarki lub karty i wklej adres URL w pasku adresu.

* Wybierz albo przeglądarki, wprowadź nazwę i komunikat, a następnie wybierz **wysyłania komunikatu** przycisku.

  Nazwa i wiadomości są wyświetlane na obu stronach natychmiast.

  ![SignalR przykładowej aplikacji](signalr/_static/signalr-get-started-finished.png)

> [!TIP]
> * Jeśli aplikacja nie działa, Otwórz swoje narzędzia deweloperskie przeglądarki (F12) i przejdź do konsoli. Może pojawić się błędy związane z Twoim kodem HTML i JavaScript. Załóżmy, że możesz umieścić *signalr.js* w innym folderze niż bezpośredniego. W takim przypadku odwołania do tego pliku nie będzie działać, a następnie zostanie wyświetlony błąd 404 w konsoli.
>   ![błąd — nie znaleziono signalr.js](signalr/_static/f12-console.png)
> * Jeśli wystąpi błąd, ERR_SPDY_INADEQUATE_TRANSPORT_SECURITY w przeglądarce Chrome lub NS_ERROR_NET_INADEQUATE_SECURITY w przeglądarce Firefox, uruchom następujące polecenia, aby zaktualizować certyfikat deweloperski:
>   ```
>   dotnet dev-certs https --clean
>   dotnet dev-certs https --trust
>   ```

## <a name="next-steps"></a>Następne kroki

Aby dowiedzieć się więcej na temat biblioteki SignalR, zobacz wprowadzenie:

> [!div class="nextstepaction"]
> [Wprowadzenie do SignalR platformy ASP.NET Core](xref:signalr/introduction)
