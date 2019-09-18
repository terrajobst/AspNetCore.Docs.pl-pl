---
title: 'Samouczek: Wprowadzenie do Razor Pages w ASP.NET Core'
author: rick-anderson
description: W tej serii samouczków pokazano, jak używać Razor Pages w ASP.NET Core. Dowiedz się, jak utworzyć model, wygenerować kod dla stron Razor, użyć Entity Framework Core i SQL Server na potrzeby dostępu do danych, dodać funkcję wyszukiwania, dodać sprawdzanie poprawności danych wejściowych i użyć migracji w celu zaktualizowania modelu.
ms.author: riande
ms.date: 07/25/2019
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 0cc00cb85b6054752417b82c783cfd4c306aeda5
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/18/2019
ms.locfileid: "71082575"
---
# <a name="tutorial-get-started-with-razor-pages-in-aspnet-core"></a>Samouczek: Wprowadzenie do Razor Pages w ASP.NET Core

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

::: moniker range=">= aspnetcore-3.0"
Jest to pierwszy samouczek dotyczący serii, który uczy się podstaw tworzenia aplikacji sieci Web Razor Pages ASP.NET Core.

[!INCLUDE[](~/includes/advancedRP.md)]

Na końcu serii będziesz mieć aplikację, która zarządza bazą danych filmów.  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

W tym samouczku przedstawiono następujące instrukcje:

> [!div class="checklist"]
> * Utwórz aplikację sieci Web Razor Pages.
> * Uruchom aplikację.
> * Przejrzyj pliki projektu.

Na końcu tego samouczka będziesz mieć działającą Razor Pagesową aplikację internetową, która zostanie wdrożona w kolejnych samouczkach.

![Strona główna lub indeks](razor-pages-start/_static/home2.2.png)

## <a name="prerequisites"></a>Wymagania wstępne

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-razor-pages-web-app"></a>Tworzenie aplikacji sieci Web Razor Pages

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* W programie Visual Studio **pliku** menu, wybierz opcję **New** > **projektu**.
* Utwórz nową aplikację sieci Web ASP.NET Core a następnie wybierz pozycję **dalej**.
  ![Nowa aplikacja sieci Web ASP.NET Core](razor-pages-start/_static/np_2.1.png)
* Nazwij projekt **RazorPagesMovie**. Ważne jest, aby nazwa projektu *RazorPagesMovie* , tak aby przestrzenie nazw były zgodne podczas kopiowania i wklejania kodu.
  ![Nowa aplikacja sieci Web ASP.NET Core](razor-pages-start/_static/config.png)

* Wybierz pozycję **ASP.NET Core 3,0** na liście rozwijanej, **aplikacji sieci Web**, a następnie wybierz pozycję **Utwórz**.

![Nowa aplikacja sieci Web ASP.NET Core](razor-pages-start/_static/3/npx.png)

  Tworzony jest następujący projekt początkowy:

  ![Eksplorator rozwiązań](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* Otwórz [zintegrowany terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).

* Przejdź do katalogu (`cd`), który będzie zawierać projekt.

* Uruchom następujące polecenia:

  ```dotnetcli
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * Polecenie tworzy nowy projekt Razor Pages w folderze RazorPagesMovie. `dotnet new`
  * Polecenie otwiera folder RazorPagesMovie w bieżącym wystąpieniu Visual Studio Code. `code`

* Gdy ikona płomienia OmniSharp na pasku stanu zmieni kolor na zielony, w **oknie dialogowym zostanie wyświetlony monit o podanie wymaganych zasobów do skompilowania i debugowania z elementu "RazorPagesMovie". Dodać je?** Wybierz pozycję **tak**.

  Katalog *. programu vscode* , zawierający pliki *Launch. JSON* i *Tasks. JSON* , jest dodawany do katalogu głównego projektu.

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

* Wybierz pozycję **plik** > **nowe rozwiązanie**.

![Nowe rozwiązanie w systemie macOS](../first-mvc-app/start-mvc/_static/new_project_vsmac.png)

* Wybierz pozycję > **aplikacja** > internetowa> aplikacji .NET Core.

  ![okno dialogowe z systemem macOS nowego projektu](razor-pages-start/_static/webapp.png)

* W oknie dialogowym **Konfigurowanie nowego interfejsu API sieci Web ASP.NET Core** Ustaw platformę **docelową** na **.NET Core 3,0**.

  ![wybór macOS .NET Core 3,0](razor-pages-start/_static/targetframework3.png)

* Nazwij projekt **RazorPagesMovie**, a następnie wybierz pozycję **Utwórz**.

  ![nameproj](razor-pages-start/_static/RazorPagesMovie.png)


## <a name="open-the-project"></a>Otwórz projekt

W programie Visual Studio wybierz pozycję **plik > Otwórz**, a następnie wybierz plik *RazorPagesMovie. csproj* .

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a>Uruchamianie aplikacji

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Naciśnij klawisze CTRL + F5, aby uruchomić bez debugera.

  [!INCLUDE[](~/includes/trustCertVS.md)]

  Program Visual Studio jest uruchamiany [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) i uruchomi aplikację. Na pasku adresu są `localhost:port#` wyświetlane inne elementy, `example.com`takie jak. Wynika `localhost` to z tego, że jest standardową nazwą hosta dla komputera lokalnego. Host lokalny obsługuje tylko żądania sieci Web z komputera lokalnego. Gdy program Visual Studio tworzy projekt sieci Web, dla serwera sieci Web jest używany port losowy.
 
# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

  [!INCLUDE[](~/includes/trustCertVSC.md)]

* Naciśnij **klawisze CTRL + F5** , aby uruchomić bez debugera.

  Visual Studio Code uruchamia [Kestrel](xref:fundamentals/servers/kestrel), uruchamia przeglądarkę i nawiguje do `http://localhost:5001`. Na pasku adresu są `localhost:port#` wyświetlane inne elementy, `example.com`takie jak. Wynika `localhost` to z tego, że jest standardową nazwą hosta dla komputera lokalnego. Host lokalny obsługuje tylko żądania sieci Web z komputera lokalnego.

  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

  [!INCLUDE[](~/includes/trustCertMac.md)]

* Naciśnij klawisze **Alt-cmd-Enter** , aby uruchomić bez debugera. Alternatywnie przejdź do paska menu i przejdź do pozycji Uruchom > Uruchom bez debugowania.

  Program Visual Studio uruchamia [Kestrel](xref:fundamentals/servers/kestrel), uruchamia przeglądarkę i przechodzi do `http://localhost:5001`.

<!-- End of VS tabs -->

---

## <a name="examine-the-project-files"></a>Sprawdzanie plików projektu

Poniżej przedstawiono Omówienie folderów i plików projektu głównego, z których będziesz korzystać w kolejnych samouczkach.

### <a name="pages-folder"></a>Folder stron

Zawiera strony Razor i pliki pomocnicze. Każda Strona Razor to para plików:

* Plik *. cshtml* , który zawiera znaczniki HTML z C# kodem przy użyciu składnia Razor.
* Plik *. cshtml.cs* , który zawiera C# kod, który obsługuje zdarzenia strony.

Pliki pomocnicze mają nazwy zaczynające się od znaku podkreślenia. Na przykład plik *_Layout. cshtml* służy do konfigurowania elementów interfejsu użytkownika wspólnych dla wszystkich stron. Ten plik konfiguruje menu nawigacji w górnej części strony i informacje o prawach autorskich w dolnej części strony. Aby uzyskać więcej informacji, zobacz <xref:mvc/views/layout>.

### <a name="wwwroot-folder"></a>folder wwwroot

Zawiera pliki statyczne, takie jak pliki HTML, pliki JavaScript i pliki CSS. Aby uzyskać więcej informacji, zobacz <xref:fundamentals/static-files>.

### <a name="appsettingsjson"></a>appSettings.json

Zawiera dane konfiguracyjne, takie jak parametry połączenia. Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/index>.

### <a name="programcs"></a>Program.cs

Zawiera punkt wejścia dla programu. Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/generic-host>.

### <a name="startupcs"></a>Startup.cs

Zawiera kod, który konfiguruje zachowanie aplikacji. Aby uzyskać więcej informacji, zobacz <xref:fundamentals/startup>.

## <a name="next-steps"></a>Następne kroki

Przejdź do następnego samouczka z serii:

> [!div class="step-by-step"]
> [Dodawanie modelu](xref:tutorials/razor-pages/model)

::: moniker-end

<!--::: moniker range=">= aspnetcore-3.0" -->

::: moniker range="< aspnetcore-3.0"

Jest to pierwszy samouczek dotyczący serii. [Seria zawiera](xref:tutorials/razor-pages/index) podstawowe informacje na temat tworzenia aplikacji sieci web ASP.NET Core Razor Pages.

[!INCLUDE[](~/includes/advancedRP.md)]

Na końcu serii będziesz mieć aplikację, która zarządza bazą danych filmów.  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

W tym samouczku przedstawiono następujące instrukcje:

> [!div class="checklist"]
> * Utwórz aplikację sieci Web Razor Pages.
> * Uruchom aplikację.
> * Przejrzyj pliki projektu.

Na końcu tego samouczka będziesz mieć działającą Razor Pagesową aplikację internetową, która zostanie wdrożona w kolejnych samouczkach.

![Strona główna lub indeks](razor-pages-start/_static/home2.2.png)

## <a name="prerequisites"></a>Wymagania wstępne

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-razor-pages-web-app"></a>Tworzenie aplikacji sieci Web Razor Pages

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* W programie Visual Studio **pliku** menu, wybierz opcję **New** > **projektu**.

* Utwórz nową aplikację sieci Web ASP.NET Core a następnie wybierz pozycję **dalej**.

  ![Nowa aplikacja sieci Web ASP.NET Core](razor-pages-start/_static/np_2.1.png)

* Nazwij projekt **RazorPagesMovie**. Ważne jest, aby nazwa projektu *RazorPagesMovie* , tak aby przestrzenie nazw były zgodne podczas kopiowania i wklejania kodu.

  ![Nowa aplikacja sieci Web ASP.NET Core](razor-pages-start/_static/config.png)

* Wybierz pozycję **ASP.NET Core 2,2** na liście rozwijanej, **aplikacji sieci Web**, a następnie wybierz pozycję **Utwórz**.

![Nowa aplikacja sieci Web ASP.NET Core](razor-pages-start/_static/np_2_2.2.png)

  Tworzony jest następujący projekt początkowy:

  ![Eksplorator rozwiązań](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* Otwórz [zintegrowany terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).

* Przejdź do katalogu (`cd`), który będzie zawierać projekt.

* Uruchom następujące polecenia:

  ```dotnetcli
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * Polecenie tworzy nowy projekt Razor Pages w folderze RazorPagesMovie. `dotnet new`
  * Polecenie otwiera folder RazorPagesMovie w bieżącym wystąpieniu Visual Studio Code. `code`

* Gdy ikona płomienia OmniSharp na pasku stanu zmieni kolor na zielony, w **oknie dialogowym zostanie wyświetlony monit o podanie wymaganych zasobów do skompilowania i debugowania z elementu "RazorPagesMovie". Dodać je?** Wybierz pozycję **tak**.

  Katalog *. programu vscode* , zawierający pliki *Launch. JSON* i *Tasks. JSON* , jest dodawany do katalogu głównego projektu.

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

W terminalu uruchom następujące polecenie:

<!-- TODO: update these instruction once mac support 2.2 projects -->

```dotnetcli
dotnet new webapp -o RazorPagesMovie
```

Poprzednie polecenia używają [interfejs wiersza polecenia platformy .NET Core](/dotnet/core/tools/dotnet) do tworzenia projektu Razor Pages.

## <a name="open-the-project"></a>Otwórz projekt

W programie Visual Studio wybierz pozycję **plik > Otwórz**, a następnie wybierz plik *RazorPagesMovie. csproj* .

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a>Uruchamianie aplikacji

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Naciśnij klawisze CTRL + F5, aby uruchomić bez debugera.

  [!INCLUDE[](~/includes/trustCertVS.md)]

  Program Visual Studio jest uruchamiany [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) i uruchomi aplikację. Na pasku adresu są `localhost:port#` wyświetlane inne elementy, `example.com`takie jak. Wynika `localhost` to z tego, że jest standardową nazwą hosta dla komputera lokalnego. Host lokalny obsługuje tylko żądania sieci Web z komputera lokalnego. Gdy program Visual Studio tworzy projekt sieci Web, dla serwera sieci Web jest używany port losowy.

* Na stronie głównej aplikacji wybierz pozycję **Akceptuj** , aby wyrazić zgodę na śledzenie.

  Ta aplikacja nie śledzi informacji osobistych, ale szablon projektu zawiera funkcję zgody na wypadek, gdyby była niezbędna do przestrzegania Ogólne rozporządzenie o ochronie danych Unii Europejskiej [(Rodo)](xref:security/gdpr).

  ![Strona główna lub indeks](razor-pages-start/_static/homeGDPR2.2.png)

  Na poniższej ilustracji przedstawiono aplikację po udzieleniu zgody na śledzenie:

  ![Strona główna lub indeks](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

  [!INCLUDE[](~/includes/trustCertVSC.md)]

* Naciśnij **klawisze CTRL + F5** , aby uruchomić bez debugera.

  Visual Studio Code uruchamia [Kestrel](xref:fundamentals/servers/kestrel), uruchamia przeglądarkę i nawiguje do `http://localhost:5001`. Na pasku adresu są `localhost:port#` wyświetlane inne elementy, `example.com`takie jak. Wynika `localhost` to z tego, że jest standardową nazwą hosta dla komputera lokalnego. Host lokalny obsługuje tylko żądania sieci Web z komputera lokalnego.

* Na stronie głównej aplikacji wybierz pozycję **Akceptuj** , aby wyrazić zgodę na śledzenie.

  Ta aplikacja nie śledzi informacji osobistych, ale szablon projektu zawiera funkcję zgody na wypadek, gdyby była niezbędna do przestrzegania Ogólne rozporządzenie o ochronie danych Unii Europejskiej [(Rodo)](xref:security/gdpr).

  ![Strona główna lub indeks](razor-pages-start/_static/homeGDPR2.2.png)

  Na poniższej ilustracji przedstawiono aplikację po udzieleniu zgody na śledzenie:

  ![Strona główna lub indeks](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

  [!INCLUDE[](~/includes/trustCertMac.md)]

* Naciśnij **polecenie cmd-opt-F5** , aby uruchomić program bez debugera.

  Program Visual Studio uruchamia [Kestrel](xref:fundamentals/servers/kestrel), uruchamia przeglądarkę i przechodzi do `http://localhost:5001`.

* Na stronie głównej aplikacji wybierz pozycję **Akceptuj** , aby wyrazić zgodę na śledzenie.

  Ta aplikacja nie śledzi informacji osobistych, ale szablon projektu zawiera funkcję zgody na wypadek, gdyby była niezbędna do przestrzegania Ogólne rozporządzenie o ochronie danych Unii Europejskiej [(Rodo)](xref:security/gdpr).

  ![Strona główna lub indeks](razor-pages-start/_static/homeGDPR2.2_safari.png)

  Na poniższej ilustracji przedstawiono aplikację po udzieleniu zgody na śledzenie:

  ![Strona główna lub indeks](razor-pages-start/_static/home2.2_safari.png)

<!-- End of VS tabs -->

---

## <a name="examine-the-project-files"></a>Sprawdzanie plików projektu

Poniżej przedstawiono Omówienie folderów i plików projektu głównego, z których będziesz korzystać w kolejnych samouczkach.

### <a name="pages-folder"></a>Folder stron

Zawiera strony Razor i pliki pomocnicze. Każda Strona Razor to para plików:

* Plik *. cshtml* , który zawiera znaczniki HTML z C# kodem przy użyciu składnia Razor.
* Plik *. cshtml.cs* , który zawiera C# kod, który obsługuje zdarzenia strony.

Pliki pomocnicze mają nazwy zaczynające się od znaku podkreślenia. Na przykład plik *_Layout. cshtml* służy do konfigurowania elementów interfejsu użytkownika wspólnych dla wszystkich stron. Ten plik konfiguruje menu nawigacji w górnej części strony i informacje o prawach autorskich w dolnej części strony. Aby uzyskać więcej informacji, zobacz <xref:mvc/views/layout>.

### <a name="wwwroot-folder"></a>folder wwwroot

Zawiera pliki statyczne, takie jak pliki HTML, pliki JavaScript i pliki CSS. Aby uzyskać więcej informacji, zobacz <xref:fundamentals/static-files>.

### <a name="appsettingsjson"></a>appSettings.json

Zawiera dane konfiguracyjne, takie jak parametry połączenia. Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/index>.

### <a name="programcs"></a>Program.cs

Zawiera punkt wejścia dla programu. Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/generic-host>.

### <a name="startupcs"></a>Startup.cs

Zawiera kod, który konfiguruje zachowanie aplikacji, na przykład czy wymaga zgody na pliki cookie. Aby uzyskać więcej informacji, zobacz <xref:fundamentals/startup>.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Wersja tego samouczka usługi YouTube](https://www.youtube.com/watch?v=F0SP7Ry4flQ&feature=youtu.be)

## <a name="next-steps"></a>Następne kroki

Przejdź do następnego samouczka z serii:

> [!div class="step-by-step"]
> [Dodawanie modelu](xref:tutorials/razor-pages/model)

::: moniker-end
