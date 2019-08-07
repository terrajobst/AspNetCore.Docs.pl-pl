---
title: Wprowadzenie do ASP.NET Core MVC
author: rick-anderson
description: Dowiedz się, jak rozpocząć pracę z ASP.NET Core MVC.
ms.author: riande
ms.date: 08/05/2019
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: f6a92423546ebd9d4c8e1a92fb81b6b72f847f61
ms.sourcegitcommit: 2eb605f4f20ac4dd9de6c3b3e3453e108a357a21
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/06/2019
ms.locfileid: "68820099"
---
# <a name="get-started-with-aspnet-core-mvc"></a>Wprowadzenie do ASP.NET Core MVC

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

::: moniker range=">= aspnetcore-3.0"

[!INCLUDE [consider RP](~/includes/razor.md)]

Ten samouczek uczy się podstaw tworzenia aplikacji sieci Web ASP.NET Core MVC.

Aplikacja zarządza bazą danych tytułów filmu. Dowiesz się, jak:

> [!div class="checklist"]
> * Utwórz aplikację internetową.
> * Dodawanie i tworzenie szkieletu modelu.
> * Pracuj z bazą danych.
> * Dodaj wyszukiwanie i sprawdzanie poprawności.

Na końcu masz aplikację, która może zarządzać i wyświetlać dane filmu.

[!INCLUDE[](~/includes/mvc-intro/download.md)]

## <a name="prerequisites"></a>Wymagania wstępne

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-web-app"></a>tworzenie aplikacji internetowej

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* W programie Visual Studio wybierz pozycję **Utwórz nowy projekt**.

* Wybierz **ASP.NET Core aplikacji sieci Web** , a następnie wybierz przycisk **dalej**.

![Nowa aplikacja sieci Web ASP.NET Core](start-mvc/_static/np_2.1.png)

* Nadaj projektowi nazwę **MvcMovie** i wybierz pozycję **Utwórz**. Ważne jest, aby nazwa **MvcMovie** projektu była taka sama, jak w przypadku kopiowania kodu, przestrzeń nazw będzie pasować.

  ![Nowa aplikacja sieci Web ASP.NET Core](start-mvc/_static/config.png)

* Wybierz pozycję **aplikacja sieci Web (Model-View-Controller)** , a następnie wybierz pozycję **Utwórz**.

![Okno dialogowe Nowy projekt, .NET Core w lewym okienku, ASP.NET Core Web ](start-mvc/_static/new_project22-21.png)

Program Visual Studio użył szablonu domyślnego dla projektu MVC, który właśnie został utworzony. Teraz masz działającą aplikację, wprowadzając nazwę projektu i wybierając kilka opcji. Jest to podstawowy projekt startowy.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Samouczek założono familarity z VS Code. Aby uzyskać więcej informacji, zobacz [wprowadzenie do vs Code](https://code.visualstudio.com/docs) i [Visual Studio Code pomocy](#visual-studio-code-help) .

* Otwórz [zintegrowany terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).
* Zmień katalogi (`cd`) na folder, który będzie zawierać projekt.
* Uruchom następujące polecenie:

   ```console
   dotnet new mvc -o MvcMovie
   code -r MvcMovie
   ```

  * Zostanie wyświetlone okno dialogowe z **wymaganymi zasobami do kompilowania i debugowania brakuje w "MvcMovie". Dodać je?**  Wybierz opcję **tak**

  * `dotnet new mvc -o MvcMovie`: tworzy nowy projekt ASP.NET Core MVC w folderze *MvcMovie* .
  * `code -r MvcMovie`: Ładuje plik projektu *MvcMovie. csproj* w Visual Studio Code.

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

* Wybierz pozycję **plik** > **nowe rozwiązanie**.

  ![Nowe rozwiązanie w systemie macOS](./start-mvc/_static/new_project_vsmac.png)

* Wybierz pozycję aplikacja internetowa **aplikacji** > **.NET Core** > **(Model-View-Controller)** > **dalej**.

  ![okno dialogowe z systemem macOS nowego projektu](./start-mvc/_static/new_project_mvc_vsmac.png)

* W oknie dialogowym **Konfigurowanie nowego interfejsu API sieci Web ASP.NET Core** Ustaw platformę docelową programu **.NET Core 3,0**.

<!-- 
  ![macOS .NET Core 2.2 selection](./start-mvc/_static/new_project_22_vsmac.png)
-->

* Nazwij projekt **MvcMovie**, a następnie wybierz pozycję **Utwórz**.

---

### <a name="run-the-app"></a>Uruchamianie aplikacji

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Wybierz **kombinację klawiszy CTRL-F5** , aby uruchomić aplikację w trybie bez debugowania.

[!INCLUDE[](~/includes/trustCertVS.md)]

* Program Visual Studio jest uruchamiany [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) i uruchomi aplikację. Zauważ, że pasek adresu pokazuje `localhost:port#` , a nie `example.com`coś innego. To dlatego `localhost` , że jest standardową nazwą hosta dla komputera lokalnego. Gdy program Visual Studio tworzy projekt sieci Web, dla serwera sieci Web jest używany port losowy.
* Uruchamianie aplikacji za pomocą klawiszy CTRL + F5 (tryb bez debugowania) umożliwia wprowadzanie zmian w kodzie, zapisywanie pliku, odświeżanie przeglądarki i wyświetlanie zmian w kodzie. Wielu deweloperów preferuje Używanie trybu niedebugowania, aby szybko uruchomić aplikację i wyświetlić zmiany.
* Możesz uruchomić aplikację w trybie debugowania lub bez debugowania z elementu menu **Debuguj** :

  ![Menu Debuguj](start-mvc/_static/debug_menu.png)

* Możesz debugować aplikację, wybierając przycisk **IIS Express**

  ![IIS Express](start-mvc/_static/iis_express.png)


  Na poniższej ilustracji przedstawiono aplikację:

  ![Strona główna lub indeks](start-mvc/_static/home2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Naciśnij klawisze CTRL + F5, aby uruchomić bez debugera.

[!INCLUDE[](~/includes/trustCertVSC.md)]

  Visual Studio Code uruchamia [Kestrel](xref:fundamentals/servers/kestrel), uruchamia przeglądarkę i nawiguje do `https://localhost:5001`. Na pasku adresu są `localhost:port:5001` wyświetlane inne elementy, `example.com`takie jak. Wynika `localhost` to z tego, że jest standardową nazwą hosta dla komputera lokalnego. Host lokalny obsługuje tylko żądania sieci Web z komputera lokalnego.

  Uruchamianie aplikacji za pomocą klawiszy CTRL + F5 (tryb bez debugowania) umożliwia wprowadzanie zmian w kodzie, zapisywanie pliku, odświeżanie przeglądarki i wyświetlanie zmian w kodzie. Wielu deweloperów woli używać trybu niedebugowania do odświeżania strony i wyświetlania zmian.

* Wybierz pozycję **Akceptuj** , aby wyrazić zgodę na śledzenie. Ta aplikacja nie śledzi informacji osobistych. Kod wygenerowany przez szablon zawiera zasoby, które pomagają spełnić [ogólne rozporządzenie o ochronie danych (Rodo)](xref:security/gdpr).

  ![Strona główna lub indeks](start-mvc/_static/privacy.png)

  Na poniższej ilustracji przedstawiono aplikację po przyjęciu śledzenia:

  ![Strona główna lub indeks](start-mvc/_static/home2.2.png)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

Wybierz pozycję **Uruchom** > **Uruchom bez debugowania** , aby uruchomić aplikację. Visual Studio dla komputerów Mac uruchamia serwer [Kestrel](xref:fundamentals/servers/index#kestrel) , uruchamia przeglądarkę i przechodzi do `http://localhost:port`, gdzie *port* jest losowo wybranym numerem portu.

[!INCLUDE[](~/includes/trustCertMac.md)]

* Na pasku adresu są `localhost:port#` wyświetlane inne elementy, `example.com`takie jak. To dlatego `localhost` , że jest standardową nazwą hosta dla komputera lokalnego. Gdy program Visual Studio tworzy projekt sieci Web, dla serwera sieci Web jest używany port losowy. Po uruchomieniu aplikacji zobaczysz inny numer portu.
* Aplikację można uruchomić w trybie debugowania lub bez debugowania z menu **Run (uruchamianie** ).

  Na poniższej ilustracji przedstawiono aplikację:

  ![Strona główna lub indeks](./start-mvc/_static/output_macos.png)

---

[!INCLUDE[](~/includes/vs-vsc-vsmac-help.md)]

W następnej części tego samouczka znajdziesz informacje na temat MVC i zacznij pisać kod.

> [!div class="step-by-step"]
> [Next](adding-controller.md)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!INCLUDE [consider RP](~/includes/razor.md)]

Ten samouczek uczy się podstaw tworzenia aplikacji sieci Web ASP.NET Core MVC.

Aplikacja zarządza bazą danych tytułów filmu. Dowiesz się, jak:

> [!div class="checklist"]
> * Utwórz aplikację internetową.
> * Dodawanie i tworzenie szkieletu modelu.
> * Pracuj z bazą danych.
> * Dodaj wyszukiwanie i sprawdzanie poprawności.

Na końcu masz aplikację, która może zarządzać i wyświetlać dane filmu.

[!INCLUDE[](~/includes/mvc-intro/download.md)]

## <a name="prerequisites"></a>Wymagania wstępne

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---
## <a name="create-a-web-app"></a>tworzenie aplikacji internetowej

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* W programie Visual Studio wybierz pozycję **Utwórz nowy projekt**.

* Selecct **ASP.NET Core aplikacji sieci Web** , a następnie wybierz przycisk **dalej**.

![Nowa aplikacja sieci Web ASP.NET Core](start-mvc/_static/np_2.1.png)

* Nadaj projektowi nazwę **MvcMovie** i wybierz pozycję **Utwórz**. Ważne jest, aby nazwa **MvcMovie** projektu była taka sama, jak w przypadku kopiowania kodu, przestrzeń nazw będzie pasować.

  ![Nowa aplikacja sieci Web ASP.NET Core](start-mvc/_static/config.png)


* Wybierz pozycję **aplikacja sieci Web (Model-View-Controller)** , a następnie wybierz pozycję **Utwórz**.

![Okno dialogowe Nowy projekt, .NET Core w lewym okienku, ASP.NET Core Web ](start-mvc/_static/new_project22-21.png)

Program Visual Studio użył szablonu domyślnego dla projektu MVC, który właśnie został utworzony. Teraz masz działającą aplikację, wprowadzając nazwę projektu i wybierając kilka opcji. Jest to podstawowy projekt początkowy i jest dobrym miejscem do rozpoczęcia.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Samouczek założono familarity z VS Code. Aby uzyskać więcej informacji, zobacz [wprowadzenie do vs Code](https://code.visualstudio.com/docs) i [Visual Studio Code pomocy](#visual-studio-code-help) .

* Otwórz [zintegrowany terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).
* Zmień katalogi (`cd`) na folder, który będzie zawierać projekt.
* Uruchom następujące polecenie:

   ```console
   dotnet new mvc -o MvcMovie
   code -r MvcMovie
   ```

  * Zostanie wyświetlone okno dialogowe z **wymaganymi zasobami do kompilowania i debugowania brakuje w "MvcMovie". Dodać je?**  Wybierz opcję **tak**

  * `dotnet new mvc -o MvcMovie`: tworzy nowy projekt ASP.NET Core MVC w folderze *MvcMovie* .
  * `code -r MvcMovie`: Ładuje plik projektu *MvcMovie. csproj* w Visual Studio Code.

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

* Wybierz pozycję **plik** > **nowe rozwiązanie**.

  ![Nowe rozwiązanie w systemie macOS](./start-mvc/_static/new_project_vsmac.png)

* Wybierz pozycję aplikacja internetowa **aplikacji** > **.NET Core** > **(Model-View-Controller)** > **dalej**.

  ![okno dialogowe z systemem macOS nowego projektu](./start-mvc/_static/new_project_mvc_vsmac.png)

* W oknie dialogowym **Konfigurowanie nowego interfejsu API sieci Web ASP.NET Core** zaakceptuj domyślną **platformę** docelową programu **.NET Core 2,2**.

  ![wybór macOS .NET Core 2,2](./start-mvc/_static/new_project_22_vsmac.png)

* Nazwij projekt **MvcMovie**, a następnie wybierz pozycję **Utwórz**.

---

### <a name="run-the-app"></a>Uruchamianie aplikacji

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Wybierz **kombinację klawiszy CTRL-F5** , aby uruchomić aplikację w trybie bez debugowania.

[!INCLUDE[](~/includes/trustCertVS.md)]

* Program Visual Studio jest uruchamiany [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) i uruchomi aplikację. Zauważ, że pasek adresu pokazuje `localhost:port#` , a nie `example.com`coś innego. To dlatego `localhost` , że jest standardową nazwą hosta dla komputera lokalnego. Gdy program Visual Studio tworzy projekt sieci Web, dla serwera sieci Web jest używany port losowy.
* Uruchamianie aplikacji za pomocą klawiszy CTRL + F5 (tryb bez debugowania) umożliwia wprowadzanie zmian w kodzie, zapisywanie pliku, odświeżanie przeglądarki i wyświetlanie zmian w kodzie. Wielu deweloperów preferuje Używanie trybu niedebugowania, aby szybko uruchomić aplikację i wyświetlić zmiany.
* Możesz uruchomić aplikację w trybie debugowania lub bez debugowania z elementu menu **Debuguj** :

  ![Menu Debuguj](start-mvc/_static/debug_menu.png)

* Możesz debugować aplikację, wybierając przycisk **IIS Express**

  ![IIS Express](start-mvc/_static/iis_express.png)

* Wybierz pozycję **Akceptuj** , aby wyrazić zgodę na śledzenie. Ta aplikacja nie śledzi informacji osobistych. Kod wygenerowany przez szablon zawiera zasoby, które pomagają spełnić [ogólne rozporządzenie o ochronie danych (Rodo)](xref:security/gdpr).

  ![Strona główna lub indeks](start-mvc/_static/privacy.png)

  Na poniższej ilustracji przedstawiono aplikację po przyjęciu śledzenia:

  ![Strona główna lub indeks](start-mvc/_static/home2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Naciśnij klawisze CTRL + F5, aby uruchomić bez debugera.

[!INCLUDE[](~/includes/trustCertVSC.md)]

  Visual Studio Code uruchamia [Kestrel](xref:fundamentals/servers/kestrel), uruchamia przeglądarkę i nawiguje do `https://localhost:5001`. Na pasku adresu są `localhost:port:5001` wyświetlane inne elementy, `example.com`takie jak. Wynika `localhost` to z tego, że jest standardową nazwą hosta dla komputera lokalnego. Host lokalny obsługuje tylko żądania sieci Web z komputera lokalnego.

  Uruchamianie aplikacji za pomocą klawiszy CTRL + F5 (tryb bez debugowania) umożliwia wprowadzanie zmian w kodzie, zapisywanie pliku, odświeżanie przeglądarki i wyświetlanie zmian w kodzie. Wielu deweloperów woli używać trybu niedebugowania do odświeżania strony i wyświetlania zmian.

* Wybierz pozycję **Akceptuj** , aby wyrazić zgodę na śledzenie. Ta aplikacja nie śledzi informacji osobistych. Kod wygenerowany przez szablon zawiera zasoby, które pomagają spełnić [ogólne rozporządzenie o ochronie danych (Rodo)](xref:security/gdpr).

  ![Strona główna lub indeks](start-mvc/_static/privacy.png)

  Na poniższej ilustracji przedstawiono aplikację po przyjęciu śledzenia:

  ![Strona główna lub indeks](start-mvc/_static/home2.2.png)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

Wybierz pozycję **Uruchom** > **Uruchom bez debugowania** , aby uruchomić aplikację. Visual Studio dla komputerów Mac uruchamia serwer [Kestrel](xref:fundamentals/servers/index#kestrel) , uruchamia przeglądarkę i przechodzi do `http://localhost:port`, gdzie *port* jest losowo wybranym numerem portu.

[!INCLUDE[](~/includes/trustCertMac.md)]

* Na pasku adresu są `localhost:port#` wyświetlane inne elementy, `example.com`takie jak. To dlatego `localhost` , że jest standardową nazwą hosta dla komputera lokalnego. Gdy program Visual Studio tworzy projekt sieci Web, dla serwera sieci Web jest używany port losowy. Po uruchomieniu aplikacji zobaczysz inny numer portu.
* Aplikację można uruchomić w trybie debugowania lub bez debugowania z menu **Run (uruchamianie** ).

* Wybierz pozycję **Akceptuj** , aby wyrazić zgodę na śledzenie. Ta aplikacja nie śledzi informacji osobistych. Kod wygenerowany przez szablon zawiera zasoby, które pomagają spełnić [ogólne rozporządzenie o ochronie danych (Rodo)](xref:security/gdpr).

  ![Strona główna lub indeks](./start-mvc/_static/output_privacy_macos.png)

  Na poniższej ilustracji przedstawiono aplikację po przyjęciu śledzenia:

  ![Strona główna lub indeks](./start-mvc/_static/output_macos.png)

---

[!INCLUDE[](~/includes/vs-vsc-vsmac-help.md)]

W następnej części tego samouczka znajdziesz informacje na temat MVC i zacznij pisać kod.

> [!div class="step-by-step"]
> [Next](adding-controller.md)

::: moniker-end
