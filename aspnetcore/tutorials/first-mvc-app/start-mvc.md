---
title: Rozpoczynanie pracy z platformą ASP.NET Core MVC
author: rick-anderson
description: Dowiedz się, jak rozpocząć pracę z platformą ASP.NET Core MVC.
ms.author: riande
ms.date: 12/12/2018
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: 5403123abed2db6951adc3eac1bfa67b73e48ada
ms.sourcegitcommit: b3894b65e313570e97a2ab78b8addd22f427cac8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/23/2019
ms.locfileid: "56744082"
---
# <a name="get-started-with-aspnet-core-mvc"></a>Rozpoczynanie pracy z platformą ASP.NET Core MVC

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [consider RP](~/includes/razor.md)]

W tym samouczku pokazano podstawy tworzenia aplikacji sieci web platformy ASP.NET Core MVC.

Aplikacja zarządza bazę tytułów filmów. Dowiesz się, jak:

> [!div class="checklist"]
> * Tworzenie aplikacji sieci web.
> * Dodaj i tworzenia szkieletu modelu.
> * Praca z bazą danych.
> * Dodaj wyszukiwanie i sprawdzania poprawności.

Na koniec masz aplikację, która może zarządzać i wyświetlić dane dotyczące filmu.

[!INCLUDE[](~/includes/mvc-intro/download.md)]

[!INCLUDE[](~/includes/net-core-prereqs-all-2.2.md)]

## <a name="create-a-web-app"></a>tworzenie aplikacji internetowej

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

W programie Visual Studio, wybierz **Plik > Nowy > Projekt**.

![Plik > Nowy > Projekt](start-mvc/_static/alt_new_project.png)

Wykonaj **nowy projekt** okno dialogowe:

* W okienku po lewej stronie wybierz **platformy .NET Core**
* W środkowym okienku wybierz **aplikacja sieci Web programu ASP.NET Core (.NET Core)**
* Nazwij projekt "MvcMovie" (warto nazwij projekt "MvcMovie", dzięki czemu podczas kopiowania kodu przestrzeni nazw będą zgodne.)
* Wybierz **OK**

![Okno dialogowe nowego projektu, .net core w okienku po lewej stronie sieci web platformy ASP.NET Core ](start-mvc/_static/new_project2-21.png)

Wykonaj **nowa Core aplikacja internetowa ASP.NET (.NET Core) — MvcMovie** okno dialogowe:

* W polu listy rozwijanej selektora wersji wybierz **platformy ASP.NET Core 2.2**
* Wybierz **(Model-View-Controller) aplikacji sieci Web**
* Wybierz **OK**.

![Okno dialogowe nowego projektu, .net core w okienku po lewej stronie sieci web platformy ASP.NET Core ](start-mvc/_static/new_project22-21.png)

Visual Studio używał domyślnego szablonu projektu MVC, który został utworzony. Masz działającą aplikację z chwili, wprowadzając nazwę projektu i wybraniu kilku opcji. Jest to projekt startowy podstawowe i jest dobrym miejscem do rozpoczęcia.

Wybierz **Ctrl-F5** do uruchomienia aplikacji w trybie bez debugowania.

* Program Visual Studio uruchamia [usług IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) i uruchamia aplikację. Należy zauważyć, że na pasku adresu `localhost:port#` i nie mielibyśmy mieć czegoś podobnego `example.com`. To dlatego, że `localhost` jest standardowa nazwa hosta komputera lokalnego. Gdy program Visual Studio tworzy projekt sieci web, losowy port jest używany dla serwera sieci web. Na powyższej ilustracji numer portu to 5000. Adres URL w pokazuje przeglądarki `localhost:5000`. Po uruchomieniu aplikacji, zobaczysz inny numer portu.
* Uruchamianie aplikacji za pomocą **kombinację klawiszy Ctrl + F5** (bez debugowania w trybie) pozwala wprowadzać zmiany kodu, Zapisz plik, Odśwież przeglądarkę i zobaczyć zmiany kodu. Wielu programistów łatwiej jest w trybie bez debugowania szybko uruchomić aplikację i wyświetlić zmiany.
* Możesz uruchomić aplikację w debugowaniu lub trybie bez debugowania z **debugowania** element menu:

![Debugowanie menu](start-mvc/_static/debug_menu.png)

* Można debugować aplikację, wybierając **usług IIS Express** przycisku

![IIS Express](start-mvc/_static/iis_express.png)

<!-- Code -------------------------->
# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

W samouczku założono familarity z programem VS Code. Zobacz [wprowadzenie do programu VS Code](https://code.visualstudio.com/docs) i [pomocy programu Visual Studio Code](#visual-studio-code-help) Aby uzyskać więcej informacji.

* Otwórz [zintegrowany terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).
* Zmień katalog (`cd`) do folderu, który będzie zawierać projekt.
* Uruchom następujące polecenie:

   ```console
   dotnet new mvc -o MvcMovie
   code -r MvcMovie
   ```

  * Zostanie wyświetlone okno dialogowe z **"MvcMovie" brakuje wymagane zasoby do tworzenia i debugowania. Dodaj je?**  Wybierz **tak**

  * `dotnet new mvc -o MvcMovie`: tworzy nowy projekt ASP.NET Core MVC w *MvcMovie* folderu.
  * `code -r MvcMovie`: Ładunki *MvcMovie.csproj* plik projektu w programie Visual Studio Code.

### <a name="launch-the-app"></a>Uruchom aplikację

* Naciśnij klawisz **Ctrl-F5** do uruchomienia bez debugera.

  Visual Studio Code rozpoczyna się rozpoczyna się [Kestrel](xref:fundamentals/servers/kestrel)otworzy w przeglądarce i przechodzi do `https://localhost:5001`. Przedstawia pasek adresu `localhost:port:5001` i nie mielibyśmy mieć czegoś podobnego `example.com`. To dlatego, że `localhost` jest standardowa nazwa hosta na komputerze lokalnym. Localhost obsługują tylko żądania sieci web z komputera lokalnego.

  Uruchamianie aplikacji za pomocą **kombinację klawiszy Ctrl + F5** (bez debugowania w trybie) pozwala wprowadzać zmiany kodu, Zapisz plik, Odśwież przeglądarkę i zobaczyć zmiany kodu. Wielu programistów łatwiej jest w trybie bez debugowania odświeżenie strony i wyświetlić zmiany.

<!-- Mac -------------------------->
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

* Wybierz **pliku** > **nowe rozwiązanie**.

  ![Nowe rozwiązanie w systemie macOS](~/tutorials/first-web-api-mac/_static/sln.png)

* Wybierz **aplikacji programu .NET Core** > **platformy ASP.NET Core** > **aplikacji internetowej ASP.NET Core (MVC)** > **dalej**.

  ![okno dialogowe z systemem macOS nowego projektu](~/tutorials/first-mvc-app-mac/start-mvc/1.png)

* W **Konfigurowanie nowego programu ASP.NET Core internetowy interfejs API** okno dialogowe, zaakceptuj wartość domyślną **platformę docelową** z **platformy .NET Core 2.2*.

* Nadaj projektowi nazwę **MvcMovie**, a następnie wybierz pozycję **Utwórz**.

### <a name="launch-the-app"></a>Uruchom aplikację

Wybierz **Uruchom** > **Rozpocznij bez debugowania** do uruchomienia aplikacji. Program Visual Studio for Mac rozpoczyna [Kestrel](xref:fundamentals/servers/index#kestrel) serwera spowoduje uruchomienie przeglądarki i przechodzi do `http://localhost:port`, gdzie *portu* jest numer portu wybranego losowo.

* Przedstawia pasek adresu `localhost:port#` i nie mielibyśmy mieć czegoś podobnego `example.com`. To dlatego, że `localhost` jest standardowa nazwa hosta komputera lokalnego. Gdy program Visual Studio tworzy projekt sieci web, losowy port jest używany dla serwera sieci web. Po uruchomieniu aplikacji, zobaczysz inny numer portu.
* Możesz uruchomić aplikację w debugowaniu lub trybie bez debugowania z **Uruchom** menu.

---  
<!-- End of VS tabs -->

* Wybierz **Akceptuj** do wyrażenia zgody na śledzenie. Ta aplikacja nie może śledzić informacje osobiste. Kod wygenerowany szablon zawiera zasoby, które ułatwiają korzystanie z [ogólne rozporządzenie o ochronie danych (RODO)](xref:security/gdpr).

  ![Strona główna lub indeks](start-mvc/_static/privacy.png)

  Na poniższej ilustracji przedstawiono aplikację po zaakceptowaniu śledzenia:

  ![Strona główna lub indeks](start-mvc/_static/home2.2.png)

[!INCLUDE[](~/includes/vs-vsc-vsmac-help.md)]

W następnej części tego samouczka Dowiedz się więcej o MVC i rozpocząć pisanie kodu.

> [!div class="step-by-step"]
> [Next](adding-controller.md)  
