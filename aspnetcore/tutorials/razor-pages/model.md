---
title: Dodawanie modelu do aplikacji Razor Pages w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak dodać klasy do zarządzania filmami w bazie danych przy użyciu Entity Framework Core (EF Core).
ms.author: riande
ms.date: 11/05/2019
uid: tutorials/razor-pages/model
ms.openlocfilehash: 312b3d4eb13eb04453bf0c3256fc362918157a45
ms.sourcegitcommit: 897d4abff58505dae86b2947c5fe3d1b80d927f3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/06/2019
ms.locfileid: "73634171"
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a>Dodawanie modelu do aplikacji Razor Pages w programie ASP.NET Core

Autor [Rick Anderson](https://twitter.com/RickAndMSFT)

::: moniker range=">= aspnetcore-3.0"

W tej sekcji są dodawane klasy do zarządzania filmami w wieloplatformowej [bazie danych SQLite](https://www.sqlite.org/index.html). Aplikacje utworzone na podstawie szablonu ASP.NET Core korzystają z bazy danych programu SQLite. Klasy modelu aplikacji są używane z [Entity Framework Core (Ef Core)](/ef/core) ([dostawca bazy danych programu SQLite EF Core](/ef/core/providers/sqlite)) do pracy z bazą danych. EF Core to struktura obiektów mapowania relacyjnego (ORM), która upraszcza dostęp do danych.

Klasy modelu są znane jako klasy POCO (z "zwykłych, starych obiektów CLR"), ponieważ nie mają żadnej zależności od EF Core. Definiują właściwości danych przechowywanych w bazie danych programu.

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

## <a name="add-a-data-model"></a>Dodawanie modelu danych

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Kliknij prawym przyciskiem myszy projekt **RazorPagesMovie** > **Dodaj** > **Nowy folder**. Nazwij *modele*folderów.

Kliknij prawym przyciskiem myszy folder *modele* . Wybierz pozycję **Dodaj** **klasy** > . Nazwij **film**klasy.

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* Dodaj folder o nazwie *models*.
* Dodaj klasę do folderu *models* o nazwie *Movie.cs*.

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio dla komputerów Mac](#tab/visual-studio-mac)

* W Eksplorator rozwiązań kliknij prawym przyciskiem myszy projekt **RazorPagesMovie** , a następnie wybierz pozycję **Dodaj** > **Nowy folder**. Nazwij *modele*folderów.
* Kliknij prawym przyciskiem myszy folder *modele* , a następnie wybierz polecenie **Dodaj** > **nowy plik**.
* W oknie dialogowym **nowy plik** :

  * W lewym okienku wybierz pozycję **Ogólne** .
  * Wybierz **pustą klasę** w środkowym okienku.
  * Nazwij **film** klasy i wybierz pozycję **Nowy**.

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

---

Skompiluj projekt, aby sprawdzić, czy nie występują błędy kompilacji.

## <a name="scaffold-the-movie-model"></a>Tworzenie szkieletu modelu filmu

W tej sekcji model filmu jest szkieletem. Oznacza to, że narzędzie tworzenia szkieletów tworzy strony dla operacji Create, Read, Update i Delete (CRUD) dla modelu filmu.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Utwórz folder *stron/filmów* :

* Kliknij prawym przyciskiem myszy folder *strony* > **dodaj** **Nowy folder**>.
* Nazwij foldery *filmów*

Kliknij prawym przyciskiem myszy folder *strony/filmy* > **Dodaj** **nowy element szkieletowy**>.

![Obraz z poprzednich instrukcji.](model/_static/sca.png)

W oknie dialogowym **Dodawanie szkieletu** wybierz pozycję **Razor Pages przy użyciu Entity Framework (CRUD)** > **Dodaj**.

![Obraz z poprzednich instrukcji.](model/_static/add_scaffold.png)

Wypełnij okno dialogowe **dodawanie Razor Pages przy użyciu Entity Framework (CRUD)** :

* Z listy rozwijanej **Klasa modelu** wybierz pozycję **film (RazorPagesMovie. models)** .
* W wierszu **klasy kontekstu danych** wybierz znak **+** (plus) i Zmień wygenerowaną nazwę z RazorPagesMovie. **Modele**. RazorPagesMovieContext do RazorPagesMovie. **Dane**. RazorPagesMovieContext. [Ta zmiana](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) nie jest wymagana. Tworzy klasę kontekstu bazy danych z poprawną przestrzenią nazw.
* Wybierz pozycję **Dodaj**.

![Obraz z poprzednich instrukcji.](model/_static/3/arp.png)

Plik *appSettings. JSON* jest aktualizowany przy użyciu parametrów połączenia używanych do nawiązywania połączenia z lokalną bazą danych.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* Otwórz okno polecenia w katalogu projektu (katalog zawierający pliki *program.cs*, *Startup.cs*i *. csproj* ).
* Zainstaluj narzędzie do tworzenia szkieletu:

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
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio dla komputerów Mac](#tab/visual-studio-mac)

* Otwórz okno polecenia w katalogu projektu (katalog zawierający pliki *program.cs*, *Startup.cs*i *. csproj* ).
* Zainstaluj narzędzie do tworzenia szkieletu:

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* Uruchom następujące polecenie:

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

---

### <a name="files-created"></a>Utworzone pliki

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Proces tworzenia szkieletu tworzy i aktualizuje następujące pliki:

* *Strony/filmy*: Tworzenie, usuwanie, szczegóły, edytowanie i indeksowanie.
* *Data/RazorPagesMovieContext. cs*

### <a name="updated"></a>Aktualny

* *Startup.cs*

Pliki utworzone i zaktualizowane zostały omówione w następnej sekcji.

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code/Visual Studio dla komputerów Mac](#tab/visual-studio-code+visual-studio-mac)

Proces tworzenia szkieletu tworzy następujące pliki:

* *Strony/filmy*: Tworzenie, usuwanie, szczegóły, edytowanie i indeksowanie.

Utworzone pliki zostały wyjaśnione w następnej sekcji.

---

<a name="pmc"></a>

## <a name="initial-migration"></a>Migracja początkowa

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

W tej sekcji konsola Menedżera pakietów (PMC) służy do:

* Dodawanie początkowej migracji.
* Zaktualizuj bazę danych przy użyciu początkowej migracji.

W menu **Narzędzia** wybierz kolejno pozycje **menedżer pakietów NuGet** > **konsola Menedżera pakietów**.

  ![Menu PMC](../first-mvc-app/adding-model/_static/pmc.png)

W obszarze PMC wprowadź następujące polecenia:

```PMC
Add-Migration InitialCreate
Update-Database
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio dla komputerów Mac](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---

Powyższe polecenia generują następujące ostrzeżenie: "nie określono typu dla kolumny dziesiętnej" price "w typie jednostki" Movie ". Spowoduje to, że wartości powinny być obcinane w trybie dyskretnym, jeśli nie mieszczą się w domyślnej precyzji i skali. Jawnie określ typ kolumny programu SQL Server, który może pomieścić wszystkie wartości przy użyciu "HasColumnType ()".

Możesz zignorować to ostrzeżenie. zostanie on rozwiązany w kolejnym samouczku.

Polecenie migrations generuje kod, aby utworzyć początkowy schemat bazy danych. Schemat jest oparty na modelu określonym w `DbContext`. Argument `InitialCreate` jest używany do nazwy migracji. Można użyć dowolnej nazwy, ale według Konwencji została wybrana nazwa opisująca migrację.

Polecenie `update` uruchamia metodę `Up` w migracjach, które nie zostały zastosowane. W takim przypadku `update` uruchamia metodę `Up` w przypadku *migracji/\<sygnatury czasowej > pliku _InitialCreate. cs* , który tworzy bazę danych.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a>Sprawdzanie kontekstu zarejestrowanego przy iniekcji zależności

ASP.NET Core jest skompilowana z [iniekcją zależności](xref:fundamentals/dependency-injection). Usługi (takie jak kontekst EF Core DB) są rejestrowane z iniekcją zależności podczas uruchamiania aplikacji. Składniki wymagające tych usług (takie jak Razor Pages) są udostępniane przez parametry konstruktora. Kod konstruktora, który pobiera wystąpienie kontekstu bazy danych, jest wyświetlany w dalszej części tego samouczka.

Narzędzie do tworzenia szkieletów automatycznie utworzyło kontekst bazy danych i zarejestrował go przy użyciu kontenera iniekcji zależności.

Przeanalizuj metodę `Startup.ConfigureServices`. Podświetlony wiersz został dodany przez program do tworzenia szkieletu:

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Startup.cs?name=snippet_ConfigureServices&highlight=5-6)]

`RazorPagesMovieContext` koordynuje EF Core funkcji (tworzenie, odczytywanie, aktualizowanie, usuwanie itp.) dla modelu `Movie`. Kontekst danych (`RazorPagesMovieContext`) pochodzi od [Microsoft. EntityFrameworkCore. DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext). Kontekst danych określa, które jednostki są uwzględnione w modelu danych.

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

Poprzedni kod tworzy właściwość [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) dla zestawu jednostek. W Entity Framework terminologii zestaw jednostek zwykle odpowiada tabeli bazy danych. Jednostka odnosi się do wiersza w tabeli.

Nazwa parametrów połączenia jest przenoszona do kontekstu przez wywołanie metody w obiekcie [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) . W przypadku lokalnego projektowania [system konfiguracji ASP.NET Core](xref:fundamentals/configuration/index) odczytuje parametry połączenia z pliku *appSettings. JSON* .

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Przeanalizuj metodę `Up`.

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio dla komputerów Mac](#tab/visual-studio-mac)

Przeanalizuj metodę `Up`.

---

<a name="test"></a>

### <a name="test-the-app"></a>Testowanie aplikacji

* Uruchom aplikację i Dołącz `/Movies` do adresu URL w przeglądarce (`http://localhost:port/movies`).

Jeśli wystąpi błąd:

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

Pominięto [krok migracji](#pmc).

* Przetestuj link **tworzenia** .

  ![Utwórz stronę](model/_static/conan.png)

  > [!NOTE]
  > W polu `Price` nie można wprowadzać przecinków dziesiętnych. Aby zapewnić obsługę [walidacji jQuery](https://jqueryvalidation.org/) dla ustawień regionalnych innych niż angielskie, które używają przecinka (",") dla przecinka dziesiętnego i dla formatów dat innych niż angielski, aplikacja musi być globalna. Aby uzyskać instrukcje dotyczące globalizacji, zobacz [ten problem](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420)w usłudze GitHub.

* Przetestuj linki **Edytuj**, **szczegóły**i **Usuń** .

W następnym samouczku objaśniono pliki utworzone przez tworzenie szkieletu.

## <a name="additional-resources"></a>Dodatkowe zasoby

> [!div class="step-by-step"]
> [Poprzedni:](xref:tutorials/razor-pages/razor-pages-start)wprowadzenie 
> [Dalej: Rusztowania Razor Pages](xref:tutorials/razor-pages/page)

::: moniker-end

<!--  ::: moniker previous version   -->
::: moniker range="< aspnetcore-3.0"

W tej sekcji są dodawane klasy do zarządzania filmami w wieloplatformowej [bazie danych SQLite](https://www.sqlite.org/index.html). Aplikacje utworzone na podstawie szablonu ASP.NET Core korzystają z bazy danych programu SQLite. Klasy modelu aplikacji są używane z [Entity Framework Core (Ef Core)](/ef/core) ([dostawca bazy danych programu SQLite EF Core](/ef/core/providers/sqlite)) do pracy z bazą danych. EF Core to struktura obiektów mapowania relacyjnego (ORM), która upraszcza dostęp do danych.

Klasy modelu są znane jako klasy POCO (z "zwykłych, starych obiektów CLR"), ponieważ nie mają żadnej zależności od EF Core. Definiują właściwości danych przechowywanych w bazie danych programu.

[!INCLUDE[](~/includes/rp/download.md)]

## <a name="add-a-data-model"></a>Dodawanie modelu danych

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Kliknij prawym przyciskiem myszy projekt **RazorPagesMovie** > **Dodaj** > **Nowy folder**. Nazwij *modele*folderów.

Kliknij prawym przyciskiem myszy folder *modele* . Wybierz pozycję **Dodaj** **klasy** > . Nazwij **film**klasy.

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* Dodaj folder o nazwie *models*.
* Dodaj klasę do folderu *models* o nazwie *Movie.cs*.

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio dla komputerów Mac](#tab/visual-studio-mac)

* W Eksplorator rozwiązań kliknij prawym przyciskiem myszy projekt **RazorPagesMovie** , a następnie wybierz pozycję **Dodaj** > **Nowy folder**. Nazwij *modele*folderów.
* Kliknij prawym przyciskiem myszy folder *modele* , a następnie wybierz polecenie **Dodaj** > **nowy plik**.
* W oknie dialogowym **nowy plik** :

  * W lewym okienku wybierz pozycję **Ogólne** .
  * Wybierz **pustą klasę** w środkowym okienku.
  * Nazwij **film** klasy i wybierz pozycję **Nowy**.

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

---

Skompiluj projekt, aby sprawdzić, czy nie występują błędy kompilacji.

## <a name="scaffold-the-movie-model"></a>Tworzenie szkieletu modelu filmu

W tej sekcji model filmu jest szkieletem. Oznacza to, że narzędzie tworzenia szkieletów tworzy strony dla operacji Create, Read, Update i Delete (CRUD) dla modelu filmu.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Utwórz folder *stron/filmów* :

* Kliknij prawym przyciskiem myszy folder *strony* > **dodaj** **Nowy folder**>.
* Nazwij foldery *filmów*

Kliknij prawym przyciskiem myszy folder *strony/filmy* > **Dodaj** **nowy element szkieletowy**>.

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

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* Otwórz okno polecenia w katalogu projektu (katalog zawierający pliki *program.cs*, *Startup.cs*i *. csproj* ).
* Zainstaluj narzędzie do tworzenia szkieletu:

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
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio dla komputerów Mac](#tab/visual-studio-mac)

* Otwórz okno polecenia w katalogu projektu (katalog zawierający pliki *program.cs*, *Startup.cs*i *. csproj* ).
* Zainstaluj narzędzie do tworzenia szkieletu:

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* Uruchom następujące polecenie:

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

---

Proces tworzenia szkieletu tworzy i aktualizuje następujące pliki:

### <a name="files-created"></a>Utworzone pliki

* *Strony/filmy*: Tworzenie, usuwanie, szczegóły, edytowanie i indeksowanie.
* *Data/RazorPagesMovieContext. cs*

### <a name="file-updated"></a>Zaktualizowano plik

* *Startup.cs*

Pliki utworzone i zaktualizowane zostały omówione w następnej sekcji.

<a name="pmc"></a>

## <a name="initial-migration"></a>Migracja początkowa

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

W tej sekcji konsola Menedżera pakietów (PMC) służy do:

* Dodawanie początkowej migracji.
* Zaktualizuj bazę danych przy użyciu początkowej migracji.

W menu **Narzędzia** wybierz kolejno pozycje **menedżer pakietów NuGet** > **konsola Menedżera pakietów**.

  ![Menu PMC](../first-mvc-app/adding-model/_static/pmc.png)

W obszarze PMC wprowadź następujące polecenia:

```Powershell
Add-Migration Initial
Update-Database
```

Polecenie `Add-Migration` generuje kod, aby utworzyć początkowy schemat bazy danych. Schemat jest oparty na modelu określonym w `DbContext` (w pliku *RazorPagesMovieContext.cs* ). Do nazwy migracji użyto argumentu `InitialCreate`. Można użyć dowolnej nazwy, ale według Konwencji Nazwa opisująca migrację jest używana. Aby uzyskać więcej informacji, zobacz <xref:data/ef-mvc/migrations>.

`Update-Database` polecenie uruchamia metodę `Up` w *sygnaturze czasowej migracji/\<> pliku _InitialCreate. cs* . Metoda `Up` tworzy bazę danych.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio dla komputerów Mac](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---
> [!NOTE]
> Powyższe polecenia generują następujące ostrzeżenie: "*nie określono typu dla kolumny dziesiętnej" price "w typie jednostki" Movie ". Spowoduje to, że wartości powinny być obcinane w trybie dyskretnym, jeśli nie mieszczą się w domyślnej precyzji i skali. Jawnie określ typ kolumny programu SQL Server, który może pomieścić wszystkie wartości przy użyciu "HasColumnType ()".* Możesz zignorować to ostrzeżenie. zostanie on rozwiązany w kolejnym samouczku.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a>Sprawdzanie kontekstu zarejestrowanego przy iniekcji zależności

ASP.NET Core jest skompilowana z [iniekcją zależności](xref:fundamentals/dependency-injection). Usługi (takie jak kontekst EF Core DB) są rejestrowane z iniekcją zależności podczas uruchamiania aplikacji. Składniki wymagające tych usług (takie jak Razor Pages) są udostępniane przez parametry konstruktora. Kod konstruktora, który pobiera wystąpienie kontekstu bazy danych, jest wyświetlany w dalszej części tego samouczka.

Narzędzie do tworzenia szkieletów automatycznie utworzyło kontekst bazy danych i zarejestrował go przy użyciu kontenera iniekcji zależności.

Przeanalizuj metodę `Startup.ConfigureServices`. Podświetlony wiersz został dodany przez program do tworzenia szkieletu:

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

`RazorPagesMovieContext` koordynuje EF Core funkcji (tworzenie, odczytywanie, aktualizowanie, usuwanie itp.) dla modelu `Movie`. Kontekst danych (`RazorPagesMovieContext`) pochodzi od [Microsoft. EntityFrameworkCore. DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext). Kontekst danych określa, które jednostki są uwzględnione w modelu danych.

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

Poprzedni kod tworzy właściwość [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) dla zestawu jednostek. W Entity Framework terminologii zestaw jednostek zwykle odpowiada tabeli bazy danych. Jednostka odnosi się do wiersza w tabeli.

Nazwa parametrów połączenia jest przenoszona do kontekstu przez wywołanie metody w obiekcie [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) . W przypadku lokalnego projektowania [system konfiguracji ASP.NET Core](xref:fundamentals/configuration/index) odczytuje parametry połączenia z pliku *appSettings. JSON* .

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Przeanalizuj metodę `Up`.

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio dla komputerów Mac](#tab/visual-studio-mac)

Przeanalizuj metodę `Up`.

---

<a name="test"></a>

### <a name="test-the-app"></a>Testowanie aplikacji

* Uruchom aplikację i Dołącz `/Movies` do adresu URL w przeglądarce (`http://localhost:port/movies`).

Jeśli wystąpi błąd:

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

Pominięto [krok migracji](#pmc).

* Przetestuj link **tworzenia** .

  ![Utwórz stronę](model/_static/conan.png)

  > [!NOTE]
  > W polu `Price` nie można wprowadzać przecinków dziesiętnych. Aby zapewnić obsługę [walidacji jQuery](https://jqueryvalidation.org/) dla ustawień regionalnych innych niż angielskie, które używają przecinka (",") dla przecinka dziesiętnego i dla formatów dat innych niż angielski, aplikacja musi być globalna. Aby uzyskać instrukcje dotyczące globalizacji, zobacz [ten problem](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420)w usłudze GitHub.

* Przetestuj linki **Edytuj**, **szczegóły**i **Usuń** .

W następnym samouczku objaśniono pliki utworzone przez tworzenie szkieletu.

## <a name="additional-resources"></a>Dodatkowe zasoby

> [!div class="step-by-step"]
> [Poprzedni:](xref:tutorials/razor-pages/razor-pages-start)wprowadzenie 
> [Dalej: Rusztowania Razor Pages](xref:tutorials/razor-pages/page)

::: moniker-end
