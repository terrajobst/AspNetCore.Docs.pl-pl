---
title: Rozpoczynanie pracy ze stronami Razor w programie ASP.NET Core
author: rick-anderson
monikerRange: '>= aspnetcore-2.2'
description: W tej serii samouczków pokazano, jak używać stron Razor w programie ASP.NET Core. Dowiedz się, jak utworzyć model, generowanie kodu dla stron Razor, platformy Entity Framework Core i SQL Server na użytek dostęp do danych, dodać funkcje wyszukiwania, dodać sprawdzanie poprawności danych wejściowych i użyć migracje do aktualizacji modelu.
ms.author: riande
ms.date: 12/5/2018
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 1152ebfcee48a46ecd28c941fce32d3fc1e05c41
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52861631"
---
# <a name="tutorial-get-started-with-razor-pages-in-aspnet-core"></a>Samouczek: Rozpoczynanie pracy ze stronami Razor w programie ASP.NET Core

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

W tym samouczku pokazano podstawy tworzenia aplikacji sieci web programu ASP.NET Core Razor strony.

Aplikacja zarządza bazę tytułów filmów. Dowiesz się, jak:

> [!div class="checklist"]
> * Tworzenie aplikacji internetowej stron Razor.
> * Dodaj i tworzenia szkieletu modelu.
> * Praca z bazą danych.
> * Dodaj wyszukiwanie i sprawdzania poprawności.

Na końcu masz aplikację, która może zarządzać i wyświetlania filmu tytułów elementów.

[!INCLUDE[](~/includes/rp/download.md)]

## <a name="prerequisites"></a>Wymagania wstępne

[!INCLUDE[](~/includes/net-core-prereqs-all-2.2.md)]

## <a name="create-a-razor-web-app"></a>Tworzenie aplikacji sieci web Razor

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* W programie Visual Studio **pliku** menu, wybierz opcję **New** > **projektu**.
* Tworzenie nowej aplikacji sieci Web platformy ASP.NET Core. Nadaj projektowi nazwę **RazorPagesMovie**. Ważne jest, aby nadaj projektowi nazwę *RazorPagesMovie* , przestrzenie nazw będą zgodne, jeśli kopiujesz/wklejasz kod.
 ![Nowa aplikacja internetowa ASP.NET Core](razor-pages-start/_static/np_2.1.png)

* Wybierz **platformy ASP.NET Core 2.2** w listy rozwijanej, a następnie wybierz pozycję **aplikacji sieci Web**.

  ![Nowa aplikacja internetowa ASP.NET Core](razor-pages-start/_static/np_2_2.2.png)

  Utworzono następujący projekt startowy:

  ![Eksplorator rozwiązań](razor-pages-start/_static/se2.2.png)

* Naciśnij klawisz **Ctrl-F5** do uruchomienia bez debugera.

  Program Visual Studio uruchamia [usług IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) i uruchomienie aplikacji. Przedstawia pasek adresu `localhost:port#` i nie mielibyśmy mieć czegoś podobnego `example.com`. To dlatego, że `localhost` jest standardowa nazwa hosta komputera lokalnego. Localhost obsługują tylko żądania sieci web z komputera lokalnego. Gdy program Visual Studio tworzy projekt sieci web, losowy port jest używany dla serwera sieci web. Na wcześniejszej ilustracji numer portu to 5001. Po uruchomieniu aplikacji, zobaczysz inny numer portu.

  Uruchamianie aplikacji za pomocą **kombinację klawiszy Ctrl + F5** (bez debugowania w trybie) pozwala wprowadzać zmiany kodu, Zapisz plik, Odśwież przeglądarkę i zobaczyć zmiany kodu. Wielu programistów łatwiej jest w trybie bez debugowania odświeżenie strony i wyświetlić zmiany.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* Otwórz [zintegrowany terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).
* Zmień katalog (`cd`) do folderu, który będzie zawierać projekt.
* Uruchom następujące polecenie:

   ```console
   dotnet new webapp -o RazorPagesMovie
   code -r RazorPagesMovie
   ```

  * Zostanie wyświetlone okno dialogowe z **"RazorPagesMovie" brakuje wymagane zasoby do tworzenia i debugowania. Dodaj je?**  Wybierz **tak**

  * `dotnet new webapp -o RazorPagesMovie`: tworzy nowy projekt strony Razor w *RazorPagesMovie* folderu.
  * `code -r RazorPagesMovie`: Ładuje *RazorPagesMovie.csproj* plik projektu w programie Visual Studio Code.

### <a name="launch-the-app"></a>Uruchom aplikację

* Naciśnij klawisz **Ctrl-F5** do uruchomienia bez debugera.

  Visual Studio Code rozpoczyna się rozpoczyna się [Kestrel](xref:fundamentals/servers/kestrel)otworzy w przeglądarce i przechodzi do `http://localhost:5001`. Przedstawia pasek adresu `localhost:port:5001` i nie mielibyśmy mieć czegoś podobnego `example.com`. To dlatego, że `localhost` jest standardowa nazwa hosta na komputerze lokalnym. Localhost obsługują tylko żądania sieci web z komputera lokalnego.

  Uruchamianie aplikacji za pomocą **kombinację klawiszy Ctrl + F5** (bez debugowania w trybie) pozwala wprowadzać zmiany kodu, Zapisz plik, Odśwież przeglądarkę i zobaczyć zmiany kodu. Wielu programistów łatwiej jest w trybie bez debugowania odświeżenie strony i wyświetlić zmiany.

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

W terminalu uruchom następujące polecenia:

<!-- TODO: update these instruction once mac support 2.2 projects -->

```console
dotnet new webapp -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

Poprzednie polecenia użyj [interfejsu wiersza polecenia platformy .NET Core](/dotnet/core/tools/dotnet) do tworzenia i uruchamiania projektów stron Razor. Otwórz w przeglądarce http://localhost:5000 Aby wyświetlić aplikację.

## <a name="open-the-project"></a>Otwórz projekt

Naciśnij klawisze Ctrl + C, aby zamknąć aplikację.

Z programu Visual Studio, wybierz **Plik > Otwórz**, a następnie wybierz pozycję *RazorPagesMovie.csproj* pliku.

### <a name="launch-the-app"></a>Uruchom aplikację

Wybierz **Uruchom > Uruchom bez debugowania** do uruchomienia aplikacji. Program Visual Studio uruchamia [Kestrel](xref:fundamentals/servers/kestrel)otworzy w przeglądarce i przechodzi do `http://localhost:5001`.

<!-- End of VS tabs -->

---

* Wybierz **Akceptuj** do wyrażenia zgody na śledzenie. Ta aplikacja nie może śledzić informacje osobiste. Kod wygenerowany szablon zawiera zasoby, które ułatwiają korzystanie z [ogólne rozporządzenie o ochronie danych (RODO)](xref:security/gdpr).

  ![Strona główna lub indeks](razor-pages-start/_static/homeGDPR2.2.png)

  Na poniższej ilustracji przedstawiono aplikację po zaakceptowaniu śledzenia:

  ![Strona główna lub indeks](razor-pages-start/_static/home2.2.png)

## <a name="project-files-and-folders"></a>Pliki projektu i foldery

W poniższej tabeli wymieniono pliki i foldery w projekcie. W tym punkcie, w tym samouczku *Startup.cs* plik jest najważniejsze informacje. Nie potrzebujesz zapoznać się z każdym linku podanego poniżej. Linki są dostarczane jako odwołanie, jeśli potrzebujesz więcej informacji na temat pliku lub folderu w projekcie.

| Plik lub folder              | Cel |
| ----------------- | ------------ |
| *wwwroot* | Zawiera pliki statyczne. Zobacz [pliki statyczne](xref:fundamentals/static-files). |
| *Strony* | Folder [stron Razor](xref:razor-pages/index). |
| *appsettings.json* | [Konfiguracja](xref:fundamentals/configuration/index) |
| *Program.cs* | [Hosty](xref:fundamentals/host/index) aplikacji ASP.NET Core.|
| *Startup.cs* | Umożliwia skonfigurowanie usług i potok żądań. Zobacz [uruchamiania](xref:fundamentals/startup).|

### <a name="the-pages-folder"></a>Folder stron

*_Layout.cshtml* plik zawiera wspólne elementy HTML (skrypty i arkusze stylów) i ustawia układ dla aplikacji. Na przykład po kliknięciu **RazorPagesMovie**, **Home**, lub **zachowania**, zobacz te same elementy. Wspólne elementy obejmują menu nawigacji górnej i nagłówek w dolnej części okna. Zobacz [układ](xref:mvc/views/layout) Aby uzyskać więcej informacji.

*_ViewImports.cshtml* plik zawiera dyrektywy Razor, które są importowane do każdej strony Razor. Zobacz [importowania dyrektywy udostępnione](xref:mvc/views/layout#importing-shared-directives) Aby uzyskać więcej informacji.

*_ViewStart.cshtml* ustawia stron Razor `Layout` właściwości, aby korzystała *_Layout.cshtml* pliku. Zobacz [układ](xref:mvc/views/layout) Aby uzyskać więcej informacji.

*_ValidationScriptsPartial.cshtml* plik zawiera odwołanie do [jQuery](https://jquery.com/) skrypty sprawdzania poprawności. Gdy `Create` i `Edit` strony są dodawane w dalszej części tego samouczka *_ValidationScriptsPartial.cshtml* plik będzie używany.

`Index`, `Error`, i `Privacy` strony znajdują się na:

* `Index`: Uruchom aplikację.
* `Error`: Służy do wyświetlania informacji o błędzie.
* `Privacy`: Określ szczegóły dotyczące zasad ochrony prywatności witryny.

W tym samouczku poprzedniej strony nie są używane.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

<a name="f7"></a>
### <a name="use-f7-to-toggle-between-a-razor-page-and-the-pagemodel"></a>F7 umożliwia przełączanie się między strony Razor i PageModel

F7 przełącza między stronami Razor (*\*.cshtml* pliku) i C# pliku (*\*. cshtml.cs*).

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

<!-- TODO review  Need something in these tabs -->

Zgodnie z Konwencją, strona Razor (*\*.cshtml* pliku) oraz skojarzonych z nimi `PageModel` mają taką samą nazwę pliku głównego.

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

Zgodnie z Konwencją, strona Razor (*\*.cshtml* pliku) oraz skojarzonych z nimi `PageModel` mają taką samą nazwę pliku głównego.

---

> [!div class="step-by-step"]
> [Następnie: Dodawanie modelu](xref:tutorials/razor-pages/model)