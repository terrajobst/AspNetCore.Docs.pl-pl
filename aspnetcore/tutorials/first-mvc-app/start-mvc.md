---
title: Rozpoczynanie pracy z platformą ASP.NET Core MVC
author: rick-anderson
description: Dowiedz się, jak rozpocząć pracę z platformą ASP.NET Core MVC.
ms.author: riande
ms.date: 04/24/2019
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: dc3499c89860190b76d6be7b8abeeaef827880d6
ms.sourcegitcommit: a1364109d11d414121a6337b611bee61d6e489e9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/04/2019
ms.locfileid: "66491250"
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

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Z programu Visual Studio wybierz opcję **Utwórz nowy projekt**.

* Selecct **aplikacji sieci Web programu ASP.NET Core** , a następnie wybierz **dalej**.

![Nowa aplikacja internetowa ASP.NET Core](start-mvc/_static/np_2.1.png)

* Nadaj projektowi nazwę **MvcMovie** i wybierz **Utwórz**. Ważne jest, aby nadaj projektowi nazwę **MvcMovie** tak podczas kopiowania kodu przestrzeni nazw będą zgodne.

  ![Nowa aplikacja internetowa ASP.NET Core](start-mvc/_static/config.png)


* Wybierz **Web Application(Model-View-Controller)** , a następnie wybierz pozycję **Utwórz**.

![Okno dialogowe nowego projektu, .NET Core w okienku po lewej stronie sieci web platformy ASP.NET Core ](start-mvc/_static/new_project22-21.png)

Program Visual Studio używany domyślny szablon projektu MVC, który został utworzony. Masz działającą aplikację z chwili, wprowadzając nazwę projektu i wybraniu kilku opcji. Jest to projekt startowy podstawowe i jest dobrym miejscem do rozpoczęcia.

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

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

* Wybierz **pliku** > **nowe rozwiązanie**.

  ![Nowe rozwiązanie w systemie macOS](./start-mvc/_static/new_project_vsmac.png)

* Wybierz **platformy .NET Core** > **aplikacji** >  **(Model-View-Controller) aplikacji sieci Web** > **dalej**.

  ![okno dialogowe z systemem macOS nowego projektu](./start-mvc/_static/new_project_mvc_vsmac.png)

* W **Konfigurowanie nowego programu ASP.NET Core internetowy interfejs API** okno dialogowe, zaakceptuj wartość domyślną **platformę docelową** z **platformy .NET Core 2.2**.

  ![System macOS wybór platformy .NET Core 2.2](./start-mvc/_static/new_project_22_vsmac.png)

* Nadaj projektowi nazwę **MvcMovie**, a następnie wybierz pozycję **Utwórz**.

---

### <a name="run-the-app"></a>Uruchamianie aplikacji

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Wybierz **Ctrl-F5** do uruchomienia aplikacji w trybie bez debugowania.

[!INCLUDE[](~/includes/trustCertVS.md)]

* Program Visual Studio uruchamia [usług IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) i uruchomienie aplikacji. Należy zauważyć, że na pasku adresu `localhost:port#` i nie mielibyśmy mieć czegoś podobnego `example.com`. To dlatego, że `localhost` jest standardowa nazwa hosta komputera lokalnego. Gdy program Visual Studio tworzy projekt sieci web, losowy port jest używany dla serwera sieci web.
* Uruchamianie aplikacji za pomocą klawiszy Ctrl + F5 (bez debugowania w trybie) umożliwia wprowadzać zmiany kodu, Zapisz plik, Odśwież przeglądarkę i zobaczyć zmiany kodu. Wielu programistów łatwiej jest w trybie bez debugowania szybko uruchomić aplikację i wyświetlić zmiany.
* Możesz uruchomić aplikację w debugowaniu lub trybie bez debugowania z **debugowania** element menu:

  ![Debugowanie menu](start-mvc/_static/debug_menu.png)

* Można debugować aplikację, wybierając **usług IIS Express** przycisku

  ![IIS Express](start-mvc/_static/iis_express.png)

* Wybierz **Akceptuj** do wyrażenia zgody na śledzenie. Ta aplikacja nie może śledzić informacje osobiste. Kod wygenerowany szablon zawiera zasoby, które ułatwiają korzystanie z [ogólne rozporządzenie o ochronie danych (RODO)](xref:security/gdpr).

  ![Strona główna lub indeks](start-mvc/_static/privacy.png)

  Na poniższej ilustracji przedstawiono aplikację po zaakceptowaniu śledzenia:

  ![Strona główna lub indeks](start-mvc/_static/home2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Naciśnij klawisze Ctrl + F5, aby uruchomić bez debugowania.

[!INCLUDE[](~/includes/trustCertVSC.md)]

  Uruchamia programu Visual Studio Code [Kestrel](xref:fundamentals/servers/kestrel)otworzy w przeglądarce i przechodzi do `https://localhost:5001`. Przedstawia pasek adresu `localhost:port:5001` i nie mielibyśmy mieć czegoś podobnego `example.com`. To dlatego, że `localhost` jest standardowa nazwa hosta na komputerze lokalnym. Localhost obsługują tylko żądania sieci web z komputera lokalnego.

  Uruchamianie aplikacji za pomocą klawiszy Ctrl + F5 (bez debugowania w trybie) umożliwia wprowadzać zmiany kodu, Zapisz plik, Odśwież przeglądarkę i zobaczyć zmiany kodu. Wielu programistów łatwiej jest w trybie bez debugowania odświeżenie strony i wyświetlić zmiany.

* Wybierz **Akceptuj** do wyrażenia zgody na śledzenie. Ta aplikacja nie może śledzić informacje osobiste. Kod wygenerowany szablon zawiera zasoby, które ułatwiają korzystanie z [ogólne rozporządzenie o ochronie danych (RODO)](xref:security/gdpr).

  ![Strona główna lub indeks](start-mvc/_static/privacy.png)

  Na poniższej ilustracji przedstawiono aplikację po zaakceptowaniu śledzenia:

  ![Strona główna lub indeks](start-mvc/_static/home2.2.png)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

Wybierz **Uruchom** > **Rozpocznij bez debugowania** do uruchomienia aplikacji. Program Visual Studio for Mac rozpoczyna [Kestrel](xref:fundamentals/servers/index#kestrel) serwera spowoduje uruchomienie przeglądarki i przechodzi do `http://localhost:port`, gdzie *portu* jest numer portu wybranego losowo.

[!INCLUDE[](~/includes/trustCertMac.md)]

* Przedstawia pasek adresu `localhost:port#` i nie mielibyśmy mieć czegoś podobnego `example.com`. To dlatego, że `localhost` jest standardowa nazwa hosta komputera lokalnego. Gdy program Visual Studio tworzy projekt sieci web, losowy port jest używany dla serwera sieci web. Po uruchomieniu aplikacji, zobaczysz inny numer portu.
* Możesz uruchomić aplikację w debugowaniu lub trybie bez debugowania z **Uruchom** menu.

* Wybierz **Akceptuj** do wyrażenia zgody na śledzenie. Ta aplikacja nie może śledzić informacje osobiste. Kod wygenerowany szablon zawiera zasoby, które ułatwiają korzystanie z [ogólne rozporządzenie o ochronie danych (RODO)](xref:security/gdpr).

  ![Strona główna lub indeks](./start-mvc/_static/output_privacy_macos.png)

  Na poniższej ilustracji przedstawiono aplikację po zaakceptowaniu śledzenia:

  ![Strona główna lub indeks](./start-mvc/_static/output_macos.png)

---

[!INCLUDE[](~/includes/vs-vsc-vsmac-help.md)]

W następnej części tego samouczka Dowiedz się więcej o MVC i rozpocząć pisanie kodu.

> [!div class="step-by-step"]
> [Next](adding-controller.md)
