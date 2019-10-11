---
title: Wprowadzenie do ASP.NET Core sygnalizującego
author: bradygaster
description: W tym samouczku utworzysz aplikację czatu korzystającą z ASP.NET Core sygnalizującego.
monikerRange: '>= aspnetcore-3.0'
ms.author: bradyg
ms.custom: mvc
ms.date: 10/03/2019
uid: tutorials/signalr
ms.openlocfilehash: 843416cf00c9241f8c05b1aba041ebbe7a05cf80
ms.sourcegitcommit: 73a451e9a58ac7102f90b608d661d8c23dd9bbaf
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/08/2019
ms.locfileid: "72037678"
---
# <a name="tutorial-get-started-with-aspnet-core-signalr"></a>Samouczek: Wprowadzenie do ASP.NET Core sygnalizującego

W tym samouczku przedstawiono podstawy tworzenia aplikacji w czasie rzeczywistym przy użyciu usługi sygnalizującej. Omawiane kwestie:

> [!div class="checklist"]
> * Utwórz projekt sieci web.
> * Dodaj bibliotekę klienta sygnalizującego.
> * Utwórz centrum sygnałów.
> * Skonfiguruj projekt do używania sygnalizującego.
> * Dodaj kod, który wysyła komunikaty z dowolnego klienta do wszystkich połączonych klientów.

Na końcu będziesz mieć działającą aplikację czatu:

![Przykładowa aplikacja sygnalizująca](signalr/_static/3.x/signalr-get-started-finished.png)

## <a name="prerequisites"></a>Wymagania wstępne

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-web-app-project"></a>Tworzenie projektu aplikacji sieci Web

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)

* Z menu wybierz pozycję **plik > nowy projekt**.

* W oknie dialogowym **Tworzenie nowego projektu** wybierz pozycję **ASP.NET Core aplikacja sieci Web**, a następnie wybierz przycisk **dalej**.

* W oknie dialogowym **Konfigurowanie nowego projektu** Nazwij projekt *SignalRChat*, a następnie wybierz pozycję **Utwórz**.

* W oknie dialogowym **Tworzenie nowej ASP.NET Core aplikacji sieci Web** wybierz pozycję **.net Core** i **ASP.NET Core 3,0**. 

* Wybierz pozycję **aplikacja sieci Web** , aby utworzyć projekt, który używa Razor Pages, a następnie wybierz pozycję **Utwórz**.

  ![Okno dialogowe nowego projektu w programie Visual Studio](signalr/_static/3.x/signalr-new-project-dialog.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code/)

* Otwórz [zintegrowany terminal](https://code.visualstudio.com/docs/editor/integrated-terminal) do folderu, w którym zostanie utworzony nowy folder projektu.

* Uruchom następujące polecenia:

   ```dotnetcli
   dotnet new webapp -o SignalRChat
   code -r SignalRChat
   ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

* Z menu wybierz pozycję **plik > nowe rozwiązanie**.

* Wybierz pozycję **.NET Core > App > aplikacji sieci Web** (nie zaznaczaj **aplikacji sieci Web (Model-View-Controller)** ), a następnie wybierz przycisk **dalej**.

* Upewnij się, że **platforma docelowa** jest ustawiona na **platformę .NET Core 3,0**, a następnie wybierz przycisk **dalej**.

* Nazwij projekt *SignalRChat*, a następnie wybierz pozycję **Utwórz**.

---

## <a name="add-the-signalr-client-library"></a>Dodawanie biblioteki klienta sygnalizującego

Biblioteka serwera sygnalizującego jest dołączona do struktury udostępnionej ASP.NET Core 3,0. Biblioteka klienta JavaScript nie jest automatycznie dołączana do projektu. W tym samouczku użyjesz programu Library Manager (LibMan), aby uzyskać bibliotekę kliencką z *unpkg*. unpkg to usługa Content Delivery Network (CDN), która umożliwia dostarczanie elementów znalezionych w programie npm, w Menedżerze pakietów środowiska Node. js.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)

* W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt, a następnie wybierz polecenie **Dodaj** > **biblioteki po stronie klienta**.

* W oknie dialogowym **Dodawanie biblioteki po stronie klienta** dla **dostawcy** wybierz pozycję **unpkg**.

* W obszarze **Biblioteka**wprowadź `@microsoft/signalr@latest`.

* Wybierz pozycję **Wybierz określone pliki**, rozwiń folder *dist/przeglądarka* , a następnie wybierz pozycję *sygnalizujące. js* i *sygnalizujący. min. js*.

* Ustaw **lokalizację docelową** na plik *wwwroot/js/signaler/* , a następnie wybierz pozycję **Zainstaluj**.

  ![Okno dialogowe Dodawanie biblioteki po stronie klienta — wybór biblioteki](signalr/_static/3.x/find-signalr-client-libs-select-files.png)

  LibMan tworzy folder *wwwroot/js/sygnalizujący* i kopiuje do niego wybrane pliki.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code/)

* W zintegrowanym terminalu uruchom następujące polecenie, aby zainstalować LibMan.

  ```dotnetcli
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* Uruchom następujące polecenie, aby uzyskać bibliotekę klienta sygnalizującego za pomocą LibMan. Może być konieczne odczekanie kilku sekund przed wyświetleniem danych wyjściowych.

  ```console
  libman install @microsoft/signalr@latest -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  Parametry określają następujące opcje:
  * Użyj dostawcy unpkg.
  * Kopiuj pliki do lokalizacji *wwwroot/js/sygnalizującer* .
  * Skopiuj tylko określone pliki.

  Dane wyjściowe wyglądają podobnie do poniższego przykładu:

  ```console
  wwwroot/js/signalr/dist/browser/signalr.js written to disk
  wwwroot/js/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@microsoft/signalr@latest" to "wwwroot/js/signalr"
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

* W **terminalu**Uruchom następujące polecenie, aby zainstalować LibMan.

  ```dotnetcli
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* Przejdź do folderu projektu (taki, który zawiera plik *SignalRChat. csproj* ).

* Uruchom następujące polecenie, aby uzyskać bibliotekę klienta sygnalizującego za pomocą LibMan.

  ```console
  libman install @microsoft/signalr@latest -p unpkg -d wwwroot/js/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  Parametry określają następujące opcje:
  * Użyj dostawcy unpkg.
  * Kopiuj pliki do lokalizacji *wwwroot/js/sygnalizującer* .
  * Skopiuj tylko określone pliki.

  Dane wyjściowe wyglądają podobnie do poniższego przykładu:

  ```console
  wwwroot/js/signalr/dist/browser/signalr.js written to disk
  wwwroot/js/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@microsoft/signalr@latest" to "wwwroot/js/signalr"
  ```

---

## <a name="create-a-signalr-hub"></a>Tworzenie centrum sygnałów

*Koncentrator* jest klasą, która służy jako potok wysokiego poziomu, który obsługuje komunikację klient-serwer.

* W folderze projektu SignalRChat Utwórz folder *Hubs* .

* W folderze *Hubs* utwórz plik *ChatHub.cs* o następującym kodzie:

  [!code-csharp[ChatHub](signalr/sample-snapshot/3.x/ChatHub.cs)]

  Klasa `ChatHub` dziedziczy od klasy sygnalizującej `Hub`. Klasa `Hub` zarządza połączeniami, grupami i obsługą wiadomości.

  Metoda `SendMessage` może być wywoływana przez połączonego klienta w celu wysłania komunikatu do wszystkich klientów. Kod klienta JavaScript, który wywołuje metodę, jest wyświetlany w dalszej części samouczka. Kod sygnalizujący jest asynchroniczny, aby zapewnić maksymalną skalowalność.

## <a name="configure-signalr"></a>Konfigurowanie sygnalizującego

Serwer sygnalizujący musi być skonfigurowany tak, aby przekazywać żądania sygnałów do sygnalizującego.

* Dodaj następujący wyróżniony kod do pliku *Startup.cs* .

  [!code-csharp[Startup](signalr/sample-snapshot/3.x/Startup.cs?highlight=11,28,55)]

  Te zmiany umożliwiają dodanie sygnalizacji do ASP.NET Core systemów iniekcji i routingu.

## <a name="add-signalr-client-code"></a>Dodaj kod klienta sygnalizującego

* Zastąp zawartość w *Pages\Index.cshtml* następującym kodem:

  [!code-cshtml[Index](signalr/sample-snapshot/3.x/Index.cshtml)]

  Powyższy kod:

  * Tworzy pola tekstowe dla nazwy i tekstu komunikatu oraz przycisk Prześlij.
  * Tworzy listę z `id="messagesList"` do wyświetlania komunikatów odebranych z centrum sygnałów.
  * Zawiera odwołania do skryptów do programu Sygnalizującer i kod aplikacji *czatu. js* , który tworzysz w następnym kroku.

* W folderze *wwwroot/js* Utwórz plik *czatu. js* o następującym kodzie:

  [!code-javascript[chat](signalr/sample-snapshot/3.x/chat.js)]

  Powyższy kod:

  * Tworzy i uruchamia połączenie.
  * Dodaje do przycisku Prześlij procedurę obsługi, która wysyła komunikaty do centrum.
  * Dodaje do obiektu Connection program obsługi, który odbiera komunikaty z centrum i dodaje je do listy.

## <a name="run-the-app"></a>Uruchamianie aplikacji

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Naciśnij **klawisze CTRL + F5** , aby uruchomić aplikację bez debugowania.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* W zintegrowanym terminalu uruchom następujące polecenie:

  ```dotnetcli
  dotnet watch run -p SignalRChat.csproj
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

* Z menu wybierz polecenie **uruchom > Uruchom bez debugowania**.

---

* Skopiuj adres URL z paska adresu, Otwórz inne wystąpienie przeglądarki lub kartę, a następnie wklej adres URL na pasku adresu.

* Wybierz opcję przeglądarka, wprowadź nazwę i komunikat, a następnie wybierz przycisk **Wyślij wiadomość** .

  Nazwa i komunikat są natychmiast wyświetlane na obu stronach.

  ![Przykładowa aplikacja sygnalizująca](signalr/_static/3.x/signalr-get-started-finished.png)

> [!TIP]
> * Jeśli aplikacja nie działa, Otwórz narzędzia deweloperskie przeglądarki (F12) i przejdź do konsoli programu. Mogą pojawić się błędy związane z kodem HTML i JavaScript. Załóżmy na przykład, że umieścisz polecenie *signaler. js* w innym folderze niż skierowany. W takim przypadku odwołanie do tego pliku nie będzie działało i zobaczysz błąd 404 w konsoli.
>   nie znaleziono @no__t -0signalr. js — błąd @ no__t-1
> * Jeśli zostanie wyświetlony błąd ERR_SPDY_INADEQUATE_TRANSPORT_SECURITY w przeglądarce Chrome, uruchom następujące polecenia, aby zaktualizować certyfikat deweloperski:
>
>   ```dotnetcli
>   dotnet dev-certs https --clean
>   dotnet dev-certs https --trust
>   ```

## <a name="next-steps"></a>Następne kroki

Aby dowiedzieć się więcej na temat sygnalizacji, zobacz Wprowadzenie:

> [!div class="nextstepaction"]
> [Wprowadzenie do ASP.NET Core sygnalizującego](xref:signalr/introduction)
