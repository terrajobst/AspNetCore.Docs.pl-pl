---
title: Dodawanie do aplikacji stron Razor w ASP.NET Core modelu
author: rick-anderson
description: Dowiedzieć się, jak można dodać klasy do zrządzania filmów w bazie danych przy użyciu Entity Framework Core (EF Core).
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/model
ms.openlocfilehash: ed8faf8b3049adc7bcc7953d63ad805b0a836bd9
ms.sourcegitcommit: 356c8d394aaf384c834e9c90cabab43bfe36e063
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/26/2018
ms.locfileid: "36961178"
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a>Dodawanie do aplikacji stron Razor w ASP.NET Core modelu

::: moniker range=">= aspnetcore-2.1"

[!INCLUDE [model1](~/includes/RP/model1.md)]

## <a name="add-a-data-model"></a>Dodawanie modelu danych

W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy **RazorPagesMovie** projektu > **Dodaj** > **nowy Folder**. Nazwa folderu *modele*.

Kliknij prawym przyciskiem myszy *modele* folderu. Wybierz **dodać** > **klasy**. Nazwa klasy **film** i dodaj następujące właściwości:

Zastąp zawartość `Movie` klasy następującym kodem:

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie21/Models/Movie1.cs?name=snippet)]

## <a name="scaffold-the-movie-model"></a>Tworzenie szkieletu modelu film

W tej sekcji jest szkieletu modelu filmu. Oznacza to, że narzędzie szkieletów zawiera strony dla operacji tworzenia, odczytu, aktualizacji i usuwania (CRUD) dla modelu filmu.

Utwórz *stron/filmów* folderu:

* W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *stron* folder > **Dodaj** > **nowy Folder**.
* Nazwa folderu *filmy*

W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *stron/filmów* folder > **Dodaj** > **nowy element szkieletu**.

![Obraz z poprzednich instrukcji.](model/_static/sca.png)

W **Dodawanie szkieletu** okno dialogowe, wybierz opcję **stron Razor przy użyciu programu Entity Framework (CRUD)** > **dodać**.

![Obraz z poprzednich instrukcji.](model/_static/add_scaffold.png)

Zakończenie **Dodaj Razor strony przy użyciu programu Entity Framework (CRUD)** okna dialogowego:

* W **klasa modelu** listy rozwijanej, wybierz **Movie (RazorPagesMovie.Models)**.
* W **klasa kontekstu danych** wierszu, wybierz opcję **+** (oraz) podpisywania i zaakceptuj nazwę wygenerowanego **RazorPagesMovie.Models.RazorPagesMovieContext**.
* W **klasa kontekstu danych** listy rozwijanej, wybierz **RazorPagesMovie.Models.RazorPagesMovieContext**
* Wybierz **dodać**.

![Obraz z poprzednich instrukcji.](model/_static/arp.png)

Proces szkieletu tworzonych i zmienianych następujące pliki:

### <a name="files-created"></a>Utworzone pliki

* *Strony/filmów* tworzenie, usuwanie i uzyskać szczegółowe informacje, edytowanie indeksu. Te strony są wyszczególnione w następnym samouczku.
* *Data/RazorPagesMovieContext.cs*

### <a name="files-updates"></a>Pliki aktualizacji

* *Startup.cs*: wyszczególnione modyfikacje tego pliku w następnej sekcji.
* *appSettings.JSON*: parametry połączenia używane do nawiązania połączenia z lokalną bazą danych został dodany.

## <a name="examine-the-context-registered-with-dependency-injection"></a>Sprawdź kontekst zarejestrowany iniekcji zależności

Zbudowany platformy ASP.NET Core za pomocą [iniekcji zależności](xref:fundamentals/dependency-injection). Usługi (takie jak kontekst danych podstawowych EF) są zarejestrowane w usłudze iniekcji zależności podczas uruchamiania aplikacji. Składniki, które wymagają tych usług (takich jak strony Razor) są udostępniane tych usług za pomocą parametrów konstruktora. Kod konstruktora, który pobiera wystąpienia kontekstu DB przedstawiono w dalszej części tego samouczka.

Narzędzie do tworzenia szkieletów automatycznie tworzone kontekst bazy danych i zarejestrowana kontenera iniekcji zależności.

Sprawdź `Startup.ConfigureServices` metody. Wybrany element został dodany w procesie tworzenia szkieletu:

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Startup.cs?name=snippet_ConfigureServices&highlight=12-13)]

Klasy głównym, która koordynuje EF podstawową funkcjonalność o dany model danych jest klasa kontekstu bazy danych. Kontekst danych jest określana na podstawie [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext). Kontekst danych określa jednostki, które znajdują się w modelu danych. W tym projekcie klasy o nazwie `RazorPagesMovieContext`.

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Data/RazorPagesMovieContext.cs)]

Poprzedni kod tworzy [DbSet\<filmu >](/dotnet/api/microsoft.entityframeworkcore.dbset-1) właściwości dla zestawu jednostek. W terminologii programu Entity Framework zwykle zestawu jednostek odnosi się do tabeli bazy danych. Jednostka odpowiada wiersza w tabeli.

Nazwa ciągu połączenia jest przekazywany do kontekstu, wywołując metodę na [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) obiektu. Dla wdrożenia lokalnego [systemu konfiguracji platformy ASP.NET Core](xref:fundamentals/configuration/index) odczytuje parametry połączenia z *appsettings.json* pliku.

<a name="pmc"></a>
## <a name="perform-initial-migration"></a>Przeprowadź migrację początkowej

W tej sekcji można użyć do konsoli Menedżera pakietów (PMC):

* Dodaj początkowej migracji.
* Aktualizacji bazy danych z początkowej migracji.

Z **narzędzia** menu, wybierz opcję **Menedżera pakietów NuGet** > **Konsola Menedżera pakietów**.

  ![PMC menu](../first-mvc-app/adding-model/_static/pmc.png)

W kryterium wprowadź następujące polecenia:

```PMC
Add-Migration Initial
Update-Database
```

Alternatywnie można użyć następujących poleceń .NET Core interfejsu wiersza polecenia:

```console
dotnet ef migrations add Initial
dotnet ef database update
```

Ignoruj następujący komunikat ostrzegawczy, możesz rozwiązać ten problem, w następnym samouczku:

`Microsoft.EntityFrameworkCore.Model.Validation[30000]`

      *No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'ForHasColumnType()'.*

`Add-Migration` Polecenie generuje kod w celu utworzenia schematu początkowej bazy danych. Schemat jest oparta na modelu określone w `RazorPagesMovieContext` (w *Data/RazorPagesMovieContext.cs* pliku). `Initial` Argument jest używany do nazywania migracji. Można użyć dowolnej nazwy, ale Konwencja wybierz nazwę, która opisuje migracji. Zobacz [wprowadzenie do migracji](xref:data/ef-mvc/migrations#introduction-to-migrations) Aby uzyskać więcej informacji.

`Update-Database` Polecenia `Up` metody w *migracje / {sygnatury czasowej} _InitialCreate.cs* pliku, który utworzy bazę danych.

Jeśli zostanie wyświetlony błąd:

SqlException: Nie można otworzyć bazy danych "RazorPagesMovieContext GUID" żądanego podczas logowania. Logowanie nie powiodło się.
Użytkownik "Nazwa użytkownika" Logowanie nie powiodło się.

Można pominąć [krok migracji](#pmc).
::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!INCLUDE [model1](~/includes/RP/model1.md)]

## <a name="add-a-data-model"></a>Dodawanie modelu danych

W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy **RazorPagesMovie** projektu > **Dodaj** > **nowy Folder**. Nazwa folderu *modele*.

Kliknij prawym przyciskiem myszy *modele* folderu. Wybierz **dodać** > **klasy**. Nazwa klasy **film** i dodaj następujące właściwości:

[!INCLUDE [model 2](~/includes/RP/model2.md)]

<a name="cs"></a>
### <a name="add-a-database-connection-string"></a>Dodaj parametry połączenia bazy danych

Dodaj parametry połączenia do *appsettings.json* pliku.

[!code-json[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=8-10)]

<a name="reg"></a>
###  <a name="register-the-database-context"></a>Zarejestruj kontekst bazy danych

Zarejestruj kontekst bazy danych z [iniekcji zależności](xref:fundamentals/dependency-injection) kontenera w [ConfigureServices metody klasy uruchamiania](xref:fundamentals/startup#the-startup-class) (*Startup.cs*):

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=3-5,7-9)]

Skompiluj projekt, aby sprawdzić, czy nie zawiera błędów.

<a name="pmc"></a>
## <a name="add-scaffold-tooling-and-perform-initial-migration"></a>Dodawanie szkieletu narzędzi i przeprowadzić migrację początkowej

W tej sekcji można użyć do konsoli Menedżera pakietów (PMC):

* Dodaj pakiet generowania kodu sieci web programu Visual Studio. Ten pakiet jest wymagana do uruchamiania aparatu szkieletów.
* Dodaj początkowej migracji.
* Aktualizacji bazy danych z początkowej migracji.

Z **narzędzia** menu, wybierz opcję **Menedżera pakietów NuGet** > **Konsola Menedżera pakietów**.

  ![PMC menu](../first-mvc-app/adding-model/_static/pmc.png)

W kryterium wprowadź następujące polecenia:

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design -Version 2.0.3
Add-Migration Initial
Update-Database
```

Alternatywnie można użyć następujących poleceń .NET Core interfejsu wiersza polecenia:

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet ef migrations add Initial
dotnet ef database update
```

Ignoruj następujący komunikat:

    `Microsoft.EntityFrameworkCore.Model.Validation[30000]`

      *No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'ForHasColumnType()'*

Napraw który w następnym samouczku.

`Install-Package` Polecenie powoduje zainstalowanie narzędzi wymagane do uruchamiania aparatu szkieletów.

`Add-Migration` Polecenie generuje kod w celu utworzenia schematu początkowej bazy danych. Schemat jest oparta na modelu określone w `DbContext` (w *Models/MovieContext.cs* pliku). `Initial` Argument jest używany do nazywania migracji. Można użyć dowolnej nazwy, ale Konwencja wybierz nazwę, która opisuje migracji. Zobacz [wprowadzenie do migracji](xref:data/ef-mvc/migrations#introduction-to-migrations) Aby uzyskać więcej informacji.

`Update-Database` Polecenia `Up` metody w *migracje / {sygnatury czasowej} _InitialCreate.cs* pliku, który utworzy bazę danych.

[!INCLUDE [model 4windows](~/includes/RP/model4Win.md)]

[!INCLUDE [model 4](~/includes/RP/model4tbl.md)]

::: moniker-end

<a name="test"></a>

### <a name="test-the-app"></a>Testowanie aplikacji

* Uruchom aplikację i Dołącz `/Movies` do adresu URL w przeglądarce (`http://localhost:port/movies`).
* Test **Utwórz** łącza.

  ![Tworzenie strony](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* Test **Edytuj**, **szczegóły**, i **usunąć** łącza.

Jeśli otrzymasz wyjątek SQL, sprawdź zostało uruchomione migracji i aktualizacji bazy danych.

Następny samouczek wyjaśnia plików utworzonych przez funkcję szkieletów.

> [!div class="step-by-step"]
> [Poprzedni: Rozpoczynanie pracy](xref:tutorials/razor-pages/razor-pages-start)
> [dalej: szkieletu stron Razor](xref:tutorials/razor-pages/page)
