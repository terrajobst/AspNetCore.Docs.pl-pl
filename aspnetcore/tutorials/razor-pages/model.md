---
title: Dodawanie modelu strony Razor aplikacji w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak można dodać klas związanych z zarządzaniem filmów w bazie danych przy użyciu platformy Entity Framework Core (EF Core).
ms.author: riande
ms.date: 12/05/2019
uid: tutorials/razor-pages/model
ms.openlocfilehash: f6dbac81b4efceb30c379ab06dd715005d879228
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78658936"
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a>Dodawanie modelu strony Razor aplikacji w programie ASP.NET Core

Autor: [Rick Anderson](https://twitter.com/RickAndMSFT)

::: moniker range=">= aspnetcore-3.0"

<!-- In the next update on the CLI version, let the scaffolder do the same work the VS driven scaffolder does. That is, create the DB context, etc -->

W tej sekcji są dodawane klasy do zarządzania filmami w wieloplatformowej [bazie danych SQLite](https://www.sqlite.org/index.html). Aplikacje utworzone na podstawie szablonu ASP.NET Core korzystają z bazy danych programu SQLite. Klasy modelu aplikacji są używane z [Entity Framework Core (Ef Core)](/ef/core) ([dostawca bazy danych programu SQLite EF Core](/ef/core/providers/sqlite)) do pracy z bazą danych. EF Core to struktura obiektów mapowania relacyjnego (ORM), która upraszcza dostęp do danych.

Klasy modelu są znane jako klasy POCO (od "old zwykły CLR obiekty"), ponieważ nie mają żadnych zależności programu EF Core. Mogą określać właściwości danych, które są przechowywane w bazie danych.

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

## <a name="add-a-data-model"></a>Dodawanie modelu danych

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

Kliknij prawym przyciskiem myszy projekt **RazorPagesMovie** > **Dodaj** > **Nowy folder**. Nazwij *modele*folderów.

Kliknij prawym przyciskiem myszy folder *modele* . Wybierz pozycję dodaj **klasę** > . Nazwij **film**klasy.

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* Dodaj folder o nazwie *models*.
* Dodaj klasę do folderu *models* o nazwie *Movie.cs*.

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

# <a name="visual-studio-for-mac"></a>[Visual Studio dla komputerów Mac](#tab/visual-studio-mac)

* W okienko rozwiązania kliknij prawym przyciskiem myszy projekt **RazorPagesMovie** , a następnie wybierz pozycję **Dodaj** > **Nowy folder.** .. Nazwij *modele*folderów.
* Kliknij prawym przyciskiem myszy folder *modele* , a następnie wybierz polecenie **Dodaj** > **nowy plik.** ...
* W oknie dialogowym **nowy plik** :

  * W lewym okienku wybierz pozycję **Ogólne** .
  * Wybierz **pustą klasę** w środkowym okienku.
  * Nazwij **film** klasy i wybierz pozycję **Nowy**.

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

---

Skompiluj projekt, aby sprawdzić, czy nie wystąpiły żadne błędy kompilacji.

## <a name="scaffold-the-movie-model"></a>Tworzenie szkieletu modelu movie

W tej sekcji modelu movie jest szkielet. Oznacza to, że narzędzie do tworzenia szkieletów tworzy strony dla operacji tworzenia, odczytu, aktualizowania lub usuwania (CRUD) do modelu movie.

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

Utwórz folder *stron/filmów* :

* Kliknij prawym przyciskiem myszy folder *strony* > **Dodaj** > **Nowy folder**.
* Nazwij foldery *filmów*

Kliknij prawym przyciskiem myszy folder *strony/filmy* > **Dodaj** > **nowy element szkieletowy**.

![Obraz z poprzednich instrukcji.](model/_static/sca.png)

W oknie dialogowym **Dodawanie szkieletu** wybierz pozycję **Razor Pages przy użyciu Entity Framework (CRUD)** > **Dodaj**.

![Obraz z poprzednich instrukcji.](model/_static/add_scaffold.png)

Wypełnij okno dialogowe **dodawanie Razor Pages przy użyciu Entity Framework (CRUD)** :

* Z listy rozwijanej **Klasa modelu** wybierz pozycję **film (RazorPagesMovie. models)** .
* W wierszu **klasy kontekstu danych** wybierz znak **+** (plus) i Zmień wygenerowaną nazwę z RazorPagesMovie. **Modele**. RazorPagesMovieContext do RazorPagesMovie. **Dane**. RazorPagesMovieContext. [Ta zmiana](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) nie jest wymagana. Tworzy klasę kontekstu bazy danych z poprawną przestrzenią nazw.
* Wybierz pozycję **Dodaj**.

![Obraz z poprzednich instrukcji.](model/_static/3/arp.png)

Plik *appSettings. JSON* jest aktualizowany przy użyciu parametrów połączenia używanych do nawiązywania połączenia z lokalną bazą danych.

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* Otwórz okno polecenia w katalogu projektu (katalog zawierający pliki *program.cs*, *Startup.cs*i *. csproj* ).
* Zainstaluj narzędzia do tworzenia szkieletów:

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* **Dla systemu Windows**: Uruchom następujące polecenie:

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* **W przypadku systemów macOS i Linux**: Uruchom następujące polecenie:

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  dotnet tool install --global dotnet-aspnet-codegenerator
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

[!INCLUDE [use SQL Server in production](~/includes/RP/sqlitedev.md)]

# <a name="visual-studio-for-mac"></a>[Visual Studio dla komputerów Mac](#tab/visual-studio-mac)

Utwórz folder *stron/filmów* :

* Kliknij prawym przyciskiem myszy folder *strony* > **Dodaj** > **Nowy folder**.
* Nazwij foldery *filmów*

Kliknij prawym przyciskiem myszy folder *strony/filmy* > **Dodaj** > **nowe tworzenie szkieletu..** ..

![Obraz z poprzednich instrukcji.](model/_static/scaMac.png)

W oknie dialogowym **Nowa rusztowania** wybierz pozycję **Razor Pages przy użyciu Entity Framework (CRUD)** > **dalej**.

![Obraz z poprzednich instrukcji.](model/_static/add_scaffoldMac.png)

Wypełnij okno dialogowe **dodawanie Razor Pages przy użyciu Entity Framework (CRUD)** :

* W menu rozwijanym **Klasa modelu** wybierz lub wpisz **film (RazorPagesMovie. models)** .
* W wierszu **klasy kontekstu danych** wpisz nazwę nowej klasy, RazorPagesMovie. **Dane**. RazorPagesMovieContext. [Ta zmiana](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) nie jest wymagana. Tworzy klasę kontekstu bazy danych z poprawną przestrzenią nazw.
* Wybierz pozycję **Dodaj**.

![Obraz z poprzednich instrukcji.](model/_static/arpMac.png)

Plik *appSettings. JSON* jest aktualizowany przy użyciu parametrów połączenia używanych do nawiązywania połączenia z lokalną bazą danych.

### <a name="add-ef-tools"></a>Dodawanie narzędzi EF

Uruchom następujące interfejs wiersza polecenia platformy .NET Core polecenie:

```dotnetcli
dotnet tool install --global dotnet-ef
```

Poprzednie polecenie dodaje Entity Framework Core narzędzia dla interfejs wiersza polecenia platformy .NET Core.

---

### <a name="files-created"></a>Utworzone pliki

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

Proces szkieletu tworzy i aktualizuje następujące pliki:

* *Strony/filmy*: Tworzenie, usuwanie, szczegóły, edytowanie i indeksowanie.
* *Data/RazorPagesMovieContext. cs*

### <a name="updated"></a>Aktualny

* *Startup.cs*

Pliki utworzone i zaktualizowane zostały wyjaśnione w kolejnej sekcji.

# <a name="visual-studio-for-mac"></a>[Visual Studio dla komputerów Mac](#tab/visual-studio-mac)

Proces szkieletu tworzy i aktualizuje następujące pliki:

* *Strony/filmy*: Tworzenie, usuwanie, szczegóły, edytowanie i indeksowanie.
* *Data/RazorPagesMovieContext. cs*

### <a name="updated"></a>Aktualny

* *Startup.cs*

Pliki utworzone i zaktualizowane zostały wyjaśnione w kolejnej sekcji.

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Proces tworzenia szkieletu tworzy następujące pliki:

* *Strony/filmy*: Tworzenie, usuwanie, szczegóły, edytowanie i indeksowanie.

Utworzone pliki zostały wyjaśnione w następnej sekcji.

---

<a name="pmc"></a>

## <a name="initial-migration"></a>Początkowej migracji

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

W tej sekcji konsoli Menedżera pakietów (PMC) służy do:

* Dodaj początkowej migracji.
* Zaktualizuj bazy danych przy użyciu początkowej migracji.

W menu **Narzędzia** wybierz kolejno pozycje **menedżer pakietów NuGet** > **konsola Menedżera pakietów**.

  ![Menu konsoli zarządzania Pakietami](../first-mvc-app/adding-model/_static/pmc.png)

W konsoli zarządzania Pakietami wprowadź następujące polecenia:

```powershell
Add-Migration InitialCreate
Update-Database
```

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mac"></a>[Visual Studio dla komputerów Mac](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---

Powyższe polecenia generują następujące ostrzeżenie: "nie określono typu dla kolumny dziesiętnej" price "w typie jednostki" Movie ". Spowoduje to, że wartości powinny być obcinane w trybie dyskretnym, jeśli nie mieszczą się w domyślnej precyzji i skali. Jawnie określ typ kolumny programu SQL Server, który może pomieścić wszystkie wartości przy użyciu "HasColumnType ()".

Możesz zignorować to ostrzeżenie. zostanie on rozwiązany w kolejnym samouczku.

Polecenie migrations generuje kod, aby utworzyć początkowy schemat bazy danych. Schemat jest oparty na modelu określonym w `DbContext`. Argument `InitialCreate` jest używany do nazwy migracji. Można dowolną nazwę, ale zgodnie z Konwencją, wybrane nazwę opisującą migracji.

`update` polecenie uruchamia metodę `Up` w przypadku migracji, które nie zostały zastosowane. W takim przypadku `update` uruchamia metodę `Up` w plikach *migrations/\<sygnatura czasowa > _InitialCreate. cs* , który tworzy bazę danych.

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a>Badanie kontekstu zarejestrowane przy użyciu iniekcji zależności

ASP.NET Core jest skompilowana z [iniekcją zależności](xref:fundamentals/dependency-injection). Usługi (takie jak kontekst bazy danych programu EF Core) zostały zarejestrowane przy użyciu iniekcji zależności podczas uruchamiania aplikacji. Składniki, które wymagają tych usług (np. strony Razor) znajdują się tych usług za pomocą parametry konstruktora. Kod konstruktora, który pobiera wystąpienia kontekstu bazy danych jest przedstawiony w dalszej części tego samouczka.

Narzędzie do tworzenia szkieletów automatycznie tworzone kontekst bazy danych i jest on zarejestrowany za pomocą kontenera iniekcji zależności.

Przeanalizuj metodę `Startup.ConfigureServices`. Wyróżniony wiersz został dodany w procesie tworzenia szkieletu:

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Startup.cs?name=snippet_ConfigureServices&highlight=5-6)]

`RazorPagesMovieContext` koordynuje EF Core funkcji (tworzenie, odczytywanie, aktualizowanie, usuwanie itp.) dla modelu `Movie`. Kontekst danych (`RazorPagesMovieContext`) pochodzi od elementu [Microsoft. EntityFrameworkCore. DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext). Kontekst danych określa, które jednostki są uwzględnione w modelu danych.

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

Poprzedni kod tworzy właściwość [nieogólnymi\<filmu >](/dotnet/api/microsoft.entityframeworkcore.dbset-1) dla zestawu jednostek. W terminologii programu Entity Framework zwykle zestaw jednostek odnosi się do tabeli bazy danych. Jednostki odnosi się do wiersza w tabeli.

Nazwa parametrów połączenia jest przenoszona do kontekstu przez wywołanie metody w obiekcie [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) . W przypadku lokalnego projektowania [system konfiguracji ASP.NET Core](xref:fundamentals/configuration/index) odczytuje parametry połączenia z pliku *appSettings. JSON* .

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Przeanalizuj metodę `Up`.

# <a name="visual-studio-for-mac"></a>[Visual Studio dla komputerów Mac](#tab/visual-studio-mac)

Przeanalizuj metodę `Up`.

---

<a name="test"></a>

### <a name="test-the-app"></a>Testowanie aplikacji

* Uruchom aplikację i Dołącz `/Movies` do adresu URL w przeglądarce (`http://localhost:port/movies`).

Jeśli zostanie wyświetlony błąd:

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

Pominięto [krok migracji](#pmc).

* Przetestuj link **tworzenia** .

  ![Tworzenie strony](model/_static/conan.png)

  > [!NOTE]
  > W polu `Price` nie można wprowadzać przecinków dziesiętnych. Aby zapewnić obsługę [walidacji jQuery](https://jqueryvalidation.org/) dla ustawień regionalnych innych niż angielskie, które używają przecinka (",") dla przecinka dziesiętnego i dla formatów dat innych niż angielski, aplikacja musi być globalna. Aby uzyskać instrukcje dotyczące globalizacji, zobacz [ten problem](https://github.com/dotnet/AspNetCore.Docs/issues/4076#issuecomment-326590420)w usłudze GitHub.

* Przetestuj linki **Edytuj**, **Szczegóły** i **Usuń**.

Następnego samouczka opisano plików utworzonych przez tworzenie szkieletów.

## <a name="additional-resources"></a>Dodatkowe zasoby

> [!div class="step-by-step"]
> [Poprzedni:](xref:tutorials/razor-pages/razor-pages-start) wprowadzenie
> [Dalej: Rusztowania Razor Pages](xref:tutorials/razor-pages/page)

::: moniker-end

<!--  ::: moniker previous version   -->
::: moniker range="< aspnetcore-3.0"

W tej sekcji są dodawane klasy do zarządzania filmami w wieloplatformowej [bazie danych SQLite](https://www.sqlite.org/index.html). Aplikacje utworzone na podstawie szablonu ASP.NET Core korzystają z bazy danych programu SQLite. Klasy modelu aplikacji są używane z [Entity Framework Core (Ef Core)](/ef/core) ([dostawca bazy danych programu SQLite EF Core](/ef/core/providers/sqlite)) do pracy z bazą danych. EF Core to struktura obiektów mapowania relacyjnego (ORM), która upraszcza dostęp do danych.

Klasy modelu są znane jako klasy POCO (od "old zwykły CLR obiekty"), ponieważ nie mają żadnych zależności programu EF Core. Mogą określać właściwości danych, które są przechowywane w bazie danych.

[!INCLUDE[](~/includes/rp/download.md)]

## <a name="add-a-data-model"></a>Dodawanie modelu danych

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

Kliknij prawym przyciskiem myszy projekt **RazorPagesMovie** > **Dodaj** > **Nowy folder**. Nazwij *modele*folderów.

Kliknij prawym przyciskiem myszy folder *modele* . Wybierz pozycję dodaj **klasę** > . Nazwij **film**klasy.

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* Dodaj folder o nazwie *models*.
* Dodaj klasę do folderu *models* o nazwie *Movie.cs*.

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

# <a name="visual-studio-for-mac"></a>[Visual Studio dla komputerów Mac](#tab/visual-studio-mac)

* W Eksplorator rozwiązań kliknij prawym przyciskiem myszy projekt **RazorPagesMovie** , a następnie wybierz pozycję **Dodaj** > **Nowy folder**. Nazwij *modele*folderów.
* Kliknij prawym przyciskiem myszy folder *modele* , a następnie wybierz polecenie **Dodaj** > **nowy plik**.
* W oknie dialogowym **nowy plik** :

  * W lewym okienku wybierz pozycję **Ogólne** .
  * Wybierz **pustą klasę** w środkowym okienku.
  * Nazwij **film** klasy i wybierz pozycję **Nowy**.

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

---

Skompiluj projekt, aby sprawdzić, czy nie wystąpiły żadne błędy kompilacji.

## <a name="scaffold-the-movie-model"></a>Tworzenie szkieletu modelu movie

W tej sekcji modelu movie jest szkielet. Oznacza to, że narzędzie do tworzenia szkieletów tworzy strony dla operacji tworzenia, odczytu, aktualizowania lub usuwania (CRUD) do modelu movie.

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

Utwórz folder *stron/filmów* :

* Kliknij prawym przyciskiem myszy folder *strony* > **Dodaj** > **Nowy folder**.
* Nazwij foldery *filmów*

Kliknij prawym przyciskiem myszy folder *strony/filmy* > **Dodaj** > **nowy element szkieletowy**.

![Obraz z poprzednich instrukcji.](model/_static/sca.png)

W oknie dialogowym **Dodawanie szkieletu** wybierz pozycję **Razor Pages przy użyciu Entity Framework (CRUD)** > **Dodaj**.

![Obraz z poprzednich instrukcji.](model/_static/add_scaffold.png)

Wypełnij okno dialogowe **dodawanie Razor Pages przy użyciu Entity Framework (CRUD)** :
<!-- In the next section, change 
(plus) sign and accept the generated name 
to use Data, it should not use models. That will make the namespace the same for the VS version and the CLI version
-->

* Z listy rozwijanej **Klasa modelu** wybierz pozycję **film (RazorPagesMovie. models)** .
* W wierszu **klasy kontekstu danych** wybierz znak **+** (plus) i zaakceptuj wygenerowaną nazwę **RazorPagesMovie. models. RazorPagesMovieContext**.
* Wybierz pozycję **Dodaj**.

![Obraz z poprzednich instrukcji.](model/_static/arp.png)

Plik *appSettings. JSON* jest aktualizowany przy użyciu parametrów połączenia używanych do nawiązywania połączenia z lokalną bazą danych.

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* Otwórz okno polecenia w katalogu projektu (katalog zawierający pliki *program.cs*, *Startup.cs*i *. csproj* ).

* **Dla systemu Windows**: Uruchom następujące polecenie:

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* **W przypadku systemów macOS i Linux**: Uruchom następujące polecenie:

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

# <a name="visual-studio-for-mac"></a>[Visual Studio dla komputerów Mac](#tab/visual-studio-mac)

Utwórz folder *stron/filmów* :

* Kliknij prawym przyciskiem myszy folder *strony* > **Dodaj** > **Nowy folder**.
* Nazwij foldery *filmów*

Kliknij prawym przyciskiem myszy folder *strony/filmy* > **Dodaj** > **nowy element szkieletowy**.

![Obraz z poprzednich instrukcji.](model/_static/scaMac.png)

W oknie dialogowym **Dodawanie nowej szkieletu** wybierz pozycję **Razor Pages przy użyciu Entity Framework (CRUD)** > **Dodaj**.

![Obraz z poprzednich instrukcji.](model/_static/add_scaffoldMac.png)

Wypełnij okno dialogowe **dodawanie Razor Pages przy użyciu Entity Framework (CRUD)** :

* Z listy rozwijanej **Klasa modelu** wybierz lub wpisz **film**.
* W wierszu **klasy kontekstu danych** wpisz wybierz **RazorPagesMovieContext** to spowoduje utworzenie nowej klasy kontekstu bazy danych z poprawną przestrzenią nazw. W takim przypadku będzie to **RazorPagesMovie. models. RazorPagesMovieContext**.
* Wybierz pozycję **Dodaj**.

![Obraz z poprzednich instrukcji.](model/_static/arpMac.png)

Plik *appSettings. JSON* jest aktualizowany przy użyciu parametrów połączenia używanych do nawiązywania połączenia z lokalną bazą danych.

---

Proces szkieletu tworzy i aktualizuje następujące pliki:

### <a name="files-created"></a>Utworzone pliki

* *Strony/filmy*: Tworzenie, usuwanie, szczegóły, edytowanie i indeksowanie.
* *Data/RazorPagesMovieContext. cs*

### <a name="file-updated"></a>Zaktualizowano plik

* *Startup.cs*

Pliki utworzone i zaktualizowane zostały wyjaśnione w kolejnej sekcji.

<a name="pmc"></a>

## <a name="initial-migration"></a>Początkowej migracji

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

W tej sekcji konsoli Menedżera pakietów (PMC) służy do:

* Dodaj początkowej migracji.
* Zaktualizuj bazy danych przy użyciu początkowej migracji.

W menu **Narzędzia** wybierz kolejno pozycje **menedżer pakietów NuGet** > **konsola Menedżera pakietów**.

  ![Menu konsoli zarządzania Pakietami](../first-mvc-app/adding-model/_static/pmc.png)

W konsoli zarządzania Pakietami wprowadź następujące polecenia:

```powershell
Add-Migration Initial
Update-Database
```

Polecenie `Add-Migration` generuje kod, aby utworzyć początkowy schemat bazy danych. Schemat jest oparty na modelu określonym w `DbContext` (w pliku *RazorPagesMovieContext.cs* ). Argument `InitialCreate` jest używany do nazwy migracji. Można dowolną nazwę, ale przez Konwencję na nazwę opisującą migracji jest używana. Aby uzyskać więcej informacji, zobacz <xref:data/ef-mvc/migrations>.

`Update-Database` polecenie uruchamia metodę `Up` w *sygnaturze czasowej migracji/\<> _InitialCreate. cs* . Metoda `Up` tworzy bazę danych.

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mac"></a>[Visual Studio dla komputerów Mac](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---
> [!NOTE]
> Powyższe polecenia generują następujące ostrzeżenie: "*nie określono typu dla kolumny dziesiętnej" price "w typie jednostki" Movie ". Spowoduje to, że wartości powinny być obcinane w trybie dyskretnym, jeśli nie mieszczą się w domyślnej precyzji i skali. Jawnie określ typ kolumny programu SQL Server, który może pomieścić wszystkie wartości przy użyciu "HasColumnType ()".* Możesz zignorować to ostrzeżenie. zostanie on rozwiązany w kolejnym samouczku.

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a>Badanie kontekstu zarejestrowane przy użyciu iniekcji zależności

ASP.NET Core jest skompilowana z [iniekcją zależności](xref:fundamentals/dependency-injection). Usługi (takie jak kontekst bazy danych programu EF Core) zostały zarejestrowane przy użyciu iniekcji zależności podczas uruchamiania aplikacji. Składniki, które wymagają tych usług (np. strony Razor) znajdują się tych usług za pomocą parametry konstruktora. Kod konstruktora, który pobiera wystąpienia kontekstu bazy danych jest przedstawiony w dalszej części tego samouczka.

Narzędzie do tworzenia szkieletów automatycznie tworzone kontekst bazy danych i jest on zarejestrowany za pomocą kontenera iniekcji zależności.

Przeanalizuj metodę `Startup.ConfigureServices`. Wyróżniony wiersz został dodany w procesie tworzenia szkieletu:

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

`RazorPagesMovieContext` koordynuje EF Core funkcji (tworzenie, odczytywanie, aktualizowanie, usuwanie itp.) dla modelu `Movie`. Kontekst danych (`RazorPagesMovieContext`) pochodzi od elementu [Microsoft. EntityFrameworkCore. DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext). Kontekst danych określa, które jednostki są uwzględnione w modelu danych.

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

Poprzedni kod tworzy właściwość [nieogólnymi\<filmu >](/dotnet/api/microsoft.entityframeworkcore.dbset-1) dla zestawu jednostek. W terminologii programu Entity Framework zwykle zestaw jednostek odnosi się do tabeli bazy danych. Jednostki odnosi się do wiersza w tabeli.

Nazwa parametrów połączenia jest przenoszona do kontekstu przez wywołanie metody w obiekcie [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) . W przypadku lokalnego projektowania [system konfiguracji ASP.NET Core](xref:fundamentals/configuration/index) odczytuje parametry połączenia z pliku *appSettings. JSON* .

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Przeanalizuj metodę `Up`.

# <a name="visual-studio-for-mac"></a>[Visual Studio dla komputerów Mac](#tab/visual-studio-mac)

Przeanalizuj metodę `Up`.

---

<a name="test"></a>

### <a name="test-the-app"></a>Testowanie aplikacji

* Uruchom aplikację i Dołącz `/Movies` do adresu URL w przeglądarce (`http://localhost:port/movies`).

Jeśli zostanie wyświetlony błąd:

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

Pominięto [krok migracji](#pmc).

* Przetestuj link **tworzenia** .

  ![Tworzenie strony](model/_static/conan.png)

  > [!NOTE]
  > W polu `Price` nie można wprowadzać przecinków dziesiętnych. Aby zapewnić obsługę [walidacji jQuery](https://jqueryvalidation.org/) dla ustawień regionalnych innych niż angielskie, które używają przecinka (",") dla przecinka dziesiętnego i dla formatów dat innych niż angielski, aplikacja musi być globalna. Aby uzyskać instrukcje dotyczące globalizacji, zobacz [ten problem](https://github.com/dotnet/AspNetCore.Docs/issues/4076#issuecomment-326590420)w usłudze GitHub.

* Przetestuj linki **Edytuj**, **Szczegóły** i **Usuń**.

Następnego samouczka opisano plików utworzonych przez tworzenie szkieletów.

## <a name="additional-resources"></a>Dodatkowe zasoby

> [!div class="step-by-step"]
> [Poprzedni:](xref:tutorials/razor-pages/razor-pages-start) wprowadzenie
> [Dalej: Rusztowania Razor Pages](xref:tutorials/razor-pages/page)

::: moniker-end
